# Dokumentation av CTF-utmaningen "Exfiltratören" publicerad av challenge.fra.se.
## Teknisk information
- **Fil:** Exfiltratoren.pcap  
- **Antal innehållande flaggor:** 7  
- **Analysmiljö:** Kali Linux (Virtuell miljö via VirtualBox)  
- **Huvudsakliga verktyg:** Wireshark, Stegsolve, Zsteg, Exiftool, Zbarimg, tshark, Python 3 (PyCryptodome, base45) samt andra distributionsinkluderade verktyg som strings, base64 etc.

## Sammanfattning
Denna dokumentation redogör för identifiering och extraktion av hittills fyra (4) flaggor ur nätverksdumpen *exfiltratoren.pcap*. Metodiken omfattar bland annat protokollanalys (TCP/UDP), dekryptering av strömkryptering (RC4), dekryptering av krypterade strängar (Base64) samt steganografisk undersökning av bildmaterial. Analysen är pågående och antalet identifierade flaggor kan komma att uppdateras. 

*OBS: Detta är min första CTF-utmaning, inklusive min första erfarenhet med vissa av analysens tillämpade verktyg. För inlärning och stöd har jag utnyttjat vissa av verktygens man-pages samt Google Gemini.*

## Analys
### 🏁 Flagga 1 - flagga{functional_treshold_power} - *FTP-exfiltrering via Dropbox*
1. Upprättar virtuell Kali Linux-miljö med VirtualBox.
2. Öppnar `exfiltratoren.pcap` i Wireshark.
3. Följer TCP-ström 0 (paket 1-28) och ser att användaren använt FTP-protokollet `STOR` för uppladdning av filen `msg.txt` till en Dropbox server. Portberäkningen för FTP-överföringen (`PORT 10,0,0,10,160,249`) ger TCP-port 41209.
4. Filtrerar alla strömmar enligt ovanstående port: `tcp.port == 41209` och isolerar paket 25. Innehållet i `msg.txt` inkluderar en Base64-sträng: `ZmxhZ2dhe2Z1bmN0aW9uYWxfdHJlc2hvbGRfcG93ZXJ9`.
5. Dekrypterar Base64-sträng: `echo ZmxhZ2dhe2Z1bmN0aW9uYWxfdHJlc2hvbGRfcG93ZXJ9 | base64 --decode`, och får: **flagga{functional_treshold_power}**.

### 🏁 Flagga 2 - flagga{qrazy_basy} - *SMTP-bilaga och QR-kod (Base45)*
6. Följer TCP-ström av paket 306 och identifierar överföring av bilagor `presentkort.pdf` och `bg.png` via mail. Bilagorna är Base64-kodade.
7. Inleder med att analysera presentkortet. Klistrar in dess Base64-sträng i en ny textfil: `nano kryptotext` och dekrypterar till en ny fil: `base64 -d kryptotext > presentkort.pdf`. Gör samma för bilden.
8. Den nya filen innehåller en QR-kod, som jag skannar: `zbarimg presentkort.pdf`. 
9. Resultatet ger en krypterad sträng, som antas vara Base45 baserat på teckenuppsättningen (endast versaler kontra Base64).
10. Lägger strängen i en fil: `nano qrstrang`.
11. Dekrypterar med Python-skript: `python3 -c "import base45; print(base45.b45decode(open('qrstrang').read().strip()).decode('utf-8'))"` vilket ger: tEXtcaptionPortkod: 1632. **flagga{qrazy_basy}**.

### 🏁 Flagga 3 - {blueprinting_exfil} - *Steganografisk analys av bildmaterial*
12. Inleder analys av bg.png.
13. Inledande kontroll med `strings` och `zsteg` gav inga indikationer på inbäddad text eller LSB-steganografi. 
14. Kör `exiftool` som gav: `Color Type: Palette`, vilket indikerar att flaggan kan vara skriven tvärs över bilden med en färg som är nästan matematisk identisk med bakgrunden. 
15. Öppnar bg.png i Stegsolve (`java -jar stegsolve.jar`). Bläddrar genom bildens bitlager och identifierar i *Random Colour Map 1* en planritning av företagets fysiska arbetsplats och dess kritiska platser (serverhall, vaktpositioner etc), samt flaggan **flagga{blueprinting_exfil}** som är visuellt renderad i pixelytan. Eftersom flaggan är ritad med pixlar snarare än inbäddad som text, extraherar jag bildlagret och antecknar flaggan för hand.

### Flagga 4 - flagga{play_the_5_tones} - *Krypterad UDP-backdoor och EXIF-injektion*
16. 

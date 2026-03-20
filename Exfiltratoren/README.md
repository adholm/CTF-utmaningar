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
### 🏁 Flagga 1 {functional_treshold_power}
1. Upprättar virtuell Kali Linux-miljö med VirtualBox.
2. Öppnar `exfiltratoren.pcap` i Wireshark.
3. Följer TCP-ström 0 (paket 1-28) och ser att användaren använt FTP-protokollet `STOR` för uppladdning av filen `msg.txt` till en Dropbox server. Portberäkningen för FTP-överföringen (`PORT 10,0,0,10,160,249`) ger TCP-port 41209.
4. Filtrerar alla strömmar enligt ovanstående port: `tcp.port == 41209` och isolerar paket 25. Innehållet i `msg.txt` utgör meddelandet: > De ..r mig p.. sp..ren - jag g..r ..ver till kamouflerad kommunikation. Vi h..rs!" samt Base64-strängen `ZmxhZ2dhe2Z1bmN0aW9uYWxfdHJlc2hvbGRfcG93ZXJ9`.
5. 

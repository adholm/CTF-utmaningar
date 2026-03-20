# Dokumentation av CTF-utmaningen "Exfiltratören" publicerad av challenge.fra.se.
## Teknisk information
Fil: Exfiltratoren.pcap  
Antal innehållande flaggor: 7  
Identifierade flaggor: 4*

OS: Kali Linux (Virtuell miljö via VirtualBox)  
Tillämpade verktyg: Wireshark, Stegsolve, -- och diverse inbyggda kommandon i OS-miljön som base64 -d, strings etc.

## Beskrivning
Denna dokumentation beskriver mitt tillvägagångsätt att identifiera flaggor i nätverksdumpen *exfiltratoren.pcap*. Analysen omfattar bland annat nätverkstrafikanalys, kryptografi och steganografi. Analysen är pågående och antalet identifierade flaggor kan komma att uppdateras.

Det bör nämnas att detta är min första CTF-utmaning, inklusive min första erfarenhet med några av de tillämpade verktygen såsom Wireshark.

## Analys
### Flagga 1 {functional_treshold_power}:
Genom analys av TCP-ström 0 (paket 1-28) upptäcktes att användaren initierat en filuppladdning av filen msg.txt till en Dropbox-server. Portberäkningen för FTP-överföringen (PORT 10,0,0,10,160,249) resulterade i TCP-port 41209.



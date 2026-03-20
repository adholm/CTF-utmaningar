Dokumentation av utmaningen "Exfiltratören" publicerad av challenge.fra.se.

Denna dokumentation beskriver ett möjligt tillvägagångsätt att identifiera 4* flaggor från en nätverksdump (exfiltratoren.pcap). Analysen omfattar bland annat nätverkstrafikanalys, kryptografi och steganografi.

* = Genomförandet är pågående och antalet identifierade flaggor kan komma att uppdateras.

Flagga 1 {functional_treshold_power}:

Genom analys av TCP-ström 0 (paket 1-28) upptäcktes att användaren initierat en filuppladdning av filen msg.txt till en Dropbox-server. Portberäkningen för FTP-överföringen (PORT 10,0,0,10,160,249) resulterade i TCP-port 41209.


test23

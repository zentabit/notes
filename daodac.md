Anteckningar från Dator- och nätverksteknik
31/03-17
------------------------------------------------------------------------------------------------------------------------------------------------
#DEL ETT
Local Area Network
Litet lokalt nätverk som oftast är begränsat till en byggnad. Fysiskt små nätverk. Ethernet.

Metropolitan Area Network
Stadsnät - länkar över större avstånd än LAN, men mindre än WAN

Wide Area Network
Kopplar ihop flera större MANs, LANs, osv. Kan vara hela Sverige, en region eller flera länder. 

Storage Area Network
Lagringsnät, hantering av stora mängder data. I storleken av ett LAN

Wireless Local Area Network
Trådlös motsvarighet av LAN. Använder andra protokoll som är kompatibla med LAN.

Virtual Local Area Network
Virtuell uppdelning av fysiska lokala nätverk. Kan även ske över nätet i VPN.

#DEL TVÅ
OSI
	Lager 1 
	Lager 2 Ethernet
	Lager 3 IP
	Lager 4	TCP/UDP
	Lager 5
	Lager 6
	Lager 7 Applikations prot.

App. prot.: Lager 5,6,7
Tusentals stycken, några vanliga är:
HTTP - 80 - HyperText Transfer Protocol - "Surf", nedladdning av hypertext-dokument. Vi använder dock inte HTTP som det var designat för.
DNS - Domain Name System - gammalt och används inte längre. Namnuppslagning: Namn -> IP-nummer.
DNSSec - uppföljaren till DNS, är dock mycket säkrare. Har stöd från certifikat och stuff.
SSH - 22 - Secure SHell. Säker textbaserad terminal. Fjärrstyrning
RDP - 3389 - Remote Desktop Protocol - Fjärrstyrning av Windows
Telnet - 23 - Fjärrstyrning, textläge, helt okrypterat!!! Överförbar på en sytråd.
SMTP - 587 - Simple Mail Transfer Protocol - Gammalt men fortfarande använt för mejl. 7-bit ASCII.
POP3 - 110 - Post Office Protocol - hämtar epost från servern.
IMAP - 143 - Internet Message Access Protocol - samma funktion som POP3. Kraftfullare än POP.
SIP (+RTP) - Session Initiation Protocol + Real-Time Transport - IP-telefoni.
SNMP - 161 - Simple Network Management Protocol - Övervakning av nätverkshårdvara.
NTP - UDP-123  - Network Time Protocol - Synkronisering av tid.

ARP - Lager 3 - Adress Translation Protocol - översätter IP till MAC.
DHCP - Dynamic Host Configuration Protocol - automatisk utdelning av IP-inställningar. BootP är föregångaren till DHCP.

07/04-17
Vi skall göra något speciellt och intressant.
WinPCAP - spelar in datatrafiken
Wireshark strukturerar upp datan för att man skall göra det lättläsligt.
Genomgång av de olika "huvudena" i ett nätverkspaket:
En Ethernet-ram är det lägsta lagret, ser ut så här:
    Preamble: Signal som synkroniserar klockan
    Start of Frame delimiter: Visar när det "intressanta börjar"
    MAC-Dst: Mottagarens MAC, skickas innan Src för att effektivisera hanteringen av paketet
    MAC-Src: Sändarens MAC
    802.1Q tag: VLAN-taggning -- ?
    Ethertype: Längden på paketet
    Payload: Själva datan i paketet, 42-1500 oktetter
    Frame Check Sequence (32-bit CRC): En slags checksumma som är baserad på resten av paketet, för att kontrollera att hela paketet är korrekt skickat. Om det inte stämmer överens, slängs paketet.
    Interframe gap: Tystnad för att skapa mellanrum mellan paketen, finns inte i WLAN.

Mycket av detta ligger kvar från 70-talet, då man behövde anpassa sig till de dåliga klockorna då.
I payloaden finns ett till huvud, IP-huvudet:
    Version: IP-version
    Header Length: längden på headern, delas upp i "words"
    ToS: Type of Service - åtta bitar som beskriver Differentiated Services Code Point som skall beskriva vilken typ av trafik det är som skickas.
    Total Length: Hur många bytes inklusive payload paketet är. Max 64kB.
    Identification: Slumpmässig siffra
    IP-flags: Olika information, tex fragmentation eller om det kommer mer bitar.
    Fragment Offset: beskriver vart i meddelandet vi är.
    Time To Live: En siffra som beskriver hur många routers paketet får passera.
    Protocol: Beskriver vilket protkoll som används, ex TCP, UDP, ICMP, IGRP.
    Header Checksum: vanlig checksumma
    Src: Källaddress
    Dst: Destination
    IP-option: valbar data
    Payload: payload

Om ett UDP-paket skickas har det också en header:
    Src: port, helst inte några från well-known ports
    Dst: port
    Length,
    Checksum

TCP-paketet ser istället ut så här:
    Src port
    Dst port
    Sequence number: vilken ordning datapaketen kommer i, slumpad
    ACK-number: Samma sekvensnummer som nästa paket mottagaren förväntar sig att få.
    Data offset
    Reserved: 000
    Några flaggor: NS, CWR, ECE, URG, ACK, PSH, RST - reset, SYN, FIN - finish
    Windows Size: Hur många paket sändaren kan skicka innan den får en acknowledge
    Checksum
    Urgent pointer: Var i infon ligger det viktigaste?
    Options

VLAN - Virtual Local Area Network
Man sätter ett VLAN-ID för alla paket, antingen i swithchen eller i datorn, skickas med i Ethernet-ramarna.
Man kan spärra, "trunka" taggar osv.
Nackdel: vi lämnar hela säkerheten åt switcharna

21/04-17
PDF finns på Classroom. Innehåller det mesta av det vi gått igenom.

Dagens lektion: switching vs routing
Två ganska lika processer: information kommer in och en "avgörare" väljer vart trafiken ska gå.
Behöver inte gå utanför datorn, t.ex. i VMs.
Till för att avgöra vart ett datapaket skall ta vägen.

Switching sker på Lager 2, MAC-adresser, säger inget om logisk eller fysisk position.
Routing sker på Lager 3, baserat på IP-adresser.
Switching kräver mindre CPU-kraft.(sker "snabbare")
Båda minskar (ger avbrott) kollisionsdomäner.
Kollisionsdomän: alla enheter som kan råka "krocka" med varandra.

En switch med fem anslutningar har fem kollisionsdomäner, med varsin pryl. Alltså inga kollisioner!
Switchtabell (finns i primärminne)
    Port    MAC
    1       1-2-3
    2       4-5-6
    3       7-8-9
    4       A-B-C
När ett paket skickas, kollar den på "från" och lägger detta i switchtabellen, switchen gissar fel om tabellen inte uppdateras, vilket den gör flera gånger per sekund.
Om inte switchen har "till" i sin tabell, skickar den ut på broadcast, och lär sig när den får svar från rätt dator.
Switchtabellen kan ha flera MAC på samma port, detta kan hända om det finns en hubb/switch under en port.
KOPPLA ALDRIG SWITCHAR I RING.
Stackbara switchar: switchar kan kopplas ihop och låtsas som att de är en switch.

Det finns tre typer av switchar: L2, L3 och Smart. L2 är oftast bara en plåtlåda.
Smart har oftast lite förståelse för IP, så att man kan konfiga den.
L3 är en router-switch, som switchar paket efter att det märkt att det går kommunikation mellan enheter, och routat första paketet.
Switching sker på LAN (lager 2).    Routing sker mellan nätverk.
Man skall i teori inte kunna koppla en kabel mellan switchar i olika nätverk.

En hemmarouter har oftast bara 2 portar. Resten tillhör switchen.
Routingtabell:
    IP-nät          Port    Metric
    1.2.3.0/24      1       1
    10.20.0.0/16    2       1
    100.200.0.0/16  3       3
    100.200.0.0/16  4       2

Routrar kommunicerar mellan varandra med separata protokoll, och lär sina kompisar sina egna routingtabeller.
IP skulle från början vara "självläkande", man skulle kunna använda nätet även om det blev sönderbombat.
Samma nätverk kan finnas på olika portar, paketet tar vägen med minst "metric".
De flesta ISP-er försöker hålla ett paket inom sitt egna nätverk så länge det går för att hålla ned kostnaderna.
De flesta routrar tittar inte på mer än Lager 3, men om routern skall agera brandvägg kan den kolla på portar och annan intressant info.
Default Gateway: IP-nummer till routern. Windows skickar alla paket som inte skall till LAN till dess MAC.
Varje router kostar lite tid, detta kan ställa till det i Ethernet.
Rekommmendation: ha ej fler än 4 switchar "på höjden" för att undvika att nätet blir långsamt.

Visar lite bilder på nätverkshårdvara.

05/05-27
Cisco Packet Tracer

###ARP
Address Resolution Protocol
-Kopplar mellan MAC-adresser & IP-adresser
Det är helt ok att köra LAN med endast Ethernet.
Men eftersom man ville ansluta till Internet, behövde man ha IP också.
####ARP-tabell
IP      MAC
1.2.3.4 3A-4C-11-41-AB-1C
Berättar vilken adress en adress finns på.

###DNS
Högst upp: . (root)
Icke-vinstdrivande; .org
Kommersiell: .com
Regeringar: .gov
Dessa är toppdomäner.
Man kan alltid fråga en .-server om vem som har hand om en toppdomän.
Under dessa finns t.ex.
cisco.com
ford.com
Allt toppdomänen behöver veta är IP-adressen till ciscos domänserver för att nå allt inom cisco.
Man lade sedan till toppdomäner för varje land. Dessa länder fick sedan sköta sina egna domäner.
Under .se kan till exempel finnas: volvo.se, telia.se
Om vi vill exempel till whitehouse.com går detta hela vägen upp till . och sedan ned till dess domänserver.
Rootservrarna är dödligt viktiga för internet. Vill man döda internet så dödar man dessa.
UDP-port 53 sker DNS-förfrågningarna på.



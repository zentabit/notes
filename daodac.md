# Anteckningar från Dator- och nätverksteknik

## 13/1-17

### Lite historia

Först användes modem (vilket står för modulering & demodulering) över telenätet för att koppla upp sig till internet.
Eftersom telenätet är analogt förekom mycket felkorrigering då paket förlorades på vägen. Man använde pariet och checksummor för att åtgärda anslutningarna. Detta gav mycket dåliga hastigheter. Modemet var då kretskopplat med modempoolen, alltså fanns det direkt elektrisk kontakt mellan dessa och de bildade en krets. Modempooler är datorer/servar med massor av serieportar som ansluts till var sitt modem, dessa ingick i något kallat PTSN, Public Switched telehone network.

För att kunna ansluta till internet användes också ADSL-splitters (vilka fortfarande används), dessa möjliggör digital förbindelse över analog förbindelse på frekvenser som är ohörbara för människan. Splittern delar upp frekvenserna vid 64kHz, allt under skickas vidare till telefon och allt över används av ADSL.

Telenätet hanterade samtida uppkopplingar genom att tidsfördela bandbredden. Då fick man full bandbredd i en kort stund, sedan fick någon annan det osv. Modem blev senare ohanterligt då många datorer skulle kopplas samman. Med modem kunde man endast koppla en dator (och det modemet) till internet.

Som en lösning på detta kom PARC/3Com/Robert Metcalfe på "Ethernet" som idag blivit världsstandard på lokala nätverk. Första versionen av Ethernet tillät hastigheter på 2Mbps och kabellängd på 500 m. Numera är det möjligt att ha hastigheter på 40Gbps.

Ethernet använder en teknik som kallas **CSMA/CD** vilket står för _Carrier Sense Multiple Access / Collision Detection_. Detta var nödvändigt på bussnät för att upptäcka samtida sändningar och motverka dessa.

### Nätverkstopologier
+ Bussnät  
Var den första versionen av Ethernet, krävde först ingen kabelkoncentrator då man helt enkelt anslöt alla datorer till en gemensam buss. Skulle denna buss gå sönder skulle hela nätverket gå under.
+ Ringnät  
Närmare bestämt token ring. Varje nod skulle då ha två kablar anslutna till sig för att bilda en ring av noder. Man använder sig av en "token" som ger tillstånd att skicka. Denna token skickas sedan runt i ringen, därav namnet.
+ Stjärnnät  
_**THE FAR SUPERIOR METHOD**_. En central kabelkoncentrator som hanterar trafiken, och man behöver endast dra en sladd till varje nod.

Även om man använde dessa logiska topologier rätt långt in i historien, användes nästan alltid hubbar och MAUs som kabelkoncentratorer för att skapa fysiska stjärnnät ändå. (Vi kan bara komma överens om att stjärnnät är bäst).

Moderna nät använder sig av både logiska och fysiska stjärnor. För att detta skall fungera bra behöver man då använda sig av en annan kabelkoncentrator, _switchen_. _Switchen_ är självlärande och har ett minne, dessutom kan den hantera flera samtida anslutningar. En switch håller koll på vilka MACs som finns på vilka portar och behöver därför inte skicka alla paket på broadcast. (Switchtabell). Endast om _switchen_ inte känner till en MAC skickar den ut på broadcast.

Man bygger ofta nät med en trädstruktur av _switchar_. 

### Standarder
| Standard | Namn |
|:---------|:-----|
| IEEE802.3 | Ethernet |
| IEEE802.4 | Token bus |
| IEEE802.5 | Token Ring |
| IEEE802.11 | WLAN/WiFi |

### WLAN
**Wireless Local Area Network**  
På ett WLAN blandas signalerna som flera noder sänder samtidigt, därför används en teknik som kallas **CSMA/CA** vilket står för _Carrier Sense Multiple Access / Collsion Avoidance_. Man undviker alltså kollisioner helt.  
WLAN använder sig av 2.4 GHz och 5 GHz-banden, varje kanal har en bandbredd på 0.1 GHz.  
**WLAN fungerar som ett hubbat nätverk** (halv duplex). Alla datorer delar på samma bandbredd.

Grundstandarden kallas IEEE802.11. Sedan finns det understandarder, här när några.

| Understandard | Frekvens(er) | Hastighet |
|:--------------|:-------------|:----------|
| 11a | 5GHz | 11Mbps |
| 11b | 2.4 | 11Mbps |
| 11g | 2.4 | 54 Mbps |
| 11n | 2.4 och/eller 5 | 300 Mbps |
| 11ac | 5 | Massa hastigheter |

2.4GHz-bandet är uppdelat i 13 kanaler och man behöver minst 5 kanalers mellanrum för att inte störa de andra nätverken över huvud taget.

Det finns olika typer av kryptering på WLAN då ett öppet är som att säga: "Här lyssna på alla alla mina privata konversationer jag någonsin kommer ha!".

+ WEP - Wired Equivalent Privacy - 40bits kryptering men suger ändå.
+ WPA - använder sig av TKIP/AES.
+ WPA2 - använder sig av AES.

## 31/03-17

### Typer av nätverk
+ Local Area Network  
Litet lokalt nätverk som oftast är begränsat till en byggnad. Fysiskt små nätverk. Ethernet.
+ Metropolitan Area Network
Stadsnät - länkar över större avstånd än LAN, men mindre än WAN
+ Wide Area Network  
Kopplar ihop flera större MANs, LANs, osv. Kan vara hela Sverige, en region eller flera länder. 
+ Storage Area Network  
Lagringsnät, hantering av stora mängder data. I storleken av ett LAN
+ Wireless Local Area Network  
Trådlös motsvarighet av LAN. Använder andra protokoll som är kompatibla med LAN.
+ Virtual Local Area Network  
Virtuell uppdelning av fysiska lokala nätverk. Kan även ske över nätet i VPN.

### OSI-modellen

| Lager | Protokoll |
|:------|-----------|
| Lager 7 | Applikationsprotokoll |
| Lager 6 |  |
| Lager 5 |  |
| Lager 4 | TCP/UDP |
| Lager 3 | IP |
| Lager 2 | Ethternet |
| Lager 1 |  |

Minnesregel: Please Do Not Throw Sausage Pizza Away

#### App. prot.: Lager 5,6,7

Tusentals stycken, några vanliga är:
+ **HTTP** - 80 - HyperText Transfer Protocol - "Surf", nedladdning av hypertext-dokument. Vi använder dock inte HTTP som det var designat för.
+ **DNS** - Domain Name System - gammalt och används inte längre. Namnuppslagning: Namn -> IP-nummer.
+ **DNSSec** - uppföljaren till DNS, är dock mycket säkrare. Har stöd från certifikat och stuff.
+ **SSH** - 22 - Secure SHell. Säker textbaserad terminal. Fjärrstyrning
+ **RDP** - 3389 - Remote Desktop Protocol - Fjärrstyrning av Windows
+ **Telnet** - 23 - Fjärrstyrning, textläge, **helt okrypterat!!!** _Överförbar på en sytråd._
+ **SMTP** - 587 - Simple Mail Transfer Protocol - Gammalt men fortfarande använt för mejl. 7-bit ASCII.
+ **POP3** - 110 - Post Office Protocol - hämtar epost från servern.
+ **IMAP** - 143 - Internet Message Access Protocol - samma funktion som POP3. Kraftfullare än POP.
+ **SIP (+RTP)** - Session Initiation Protocol + Real-Time Transport - IP-telefoni.
+ **SNMP** - 161 - Simple Network Management Protocol - Övervakning av nätverkshårdvara.
+ **NTP** - UDP-123  - Network Time Protocol - Synkronisering av tid.

**ARP** - Lager 3 - Adress Translation Protocol - översätter IP till MAC.  
**DHCP** - Dynamic Host Configuration Protocol - automatisk utdelning av IP-inställningar. BootP är föregångaren till DHCP.  

## 07/04-17

#### Genomgång av de olika "huvudena" i ett nätverkspaket:

+ En Ethernet-ram är det lägsta lagret, ser ut så här:
+ Preamble: Signal som synkroniserar klockan
+ Start of Frame delimiter: Visar när det "intressanta börjar"
+ MAC-Dst: Mottagarens MAC, skickas innan Src för att effektivisera hanteringen av paketet
+ MAC-Src: Sändarens MAC
+ 802.1Q tag: VLAN-taggning -- ?
+ Ethertype: Längden på paketet
+ Payload: Själva datan i paketet, 42-1500 oktetter
+ Frame Check Sequence (32-bit CRC): En slags checksumma som är baserad på resten av paketet, för att kontrollera att hela paketet är korrekt skickat. Om det inte stämmer överens, slängs paketet.
+ Interframe gap: Tystnad för att skapa mellanrum mellan paketen, finns inte i WLAN.

_Mycket av detta ligger kvar från 70-talet, då man behövde anpassa sig till de dåliga klockorna då._
I payloaden finns ett till huvud, IP-huvudet:
+ Version: IP-version
+ Header Length: längden på headern, delas upp i "words"
+ ToS: Type of Service - åtta bitar som beskriver Differentiated Services Code Point som skall beskriva vilken typ av trafik det är som skickas.
+ Total Length: Hur många bytes inklusive payload paketet är. Max 64kB.
+ Identification: Slumpmässig siffra
+ IP-flags: Olika information, tex fragmentation eller om det kommer mer bitar.
+ Fragment Offset: beskriver vart i meddelandet vi är.
+ Time To Live: En siffra som beskriver hur många routers paketet får passera.
+ Protocol: Beskriver vilket protkoll som används, ex TCP, UDP, ICMP, IGRP.
+ Header Checksum: vanlig checksumma
+ Src: Källaddress
+ Dst: Destination
+ IP-option: valbar data
+ Payload: payload

Om ett UDP-paket skickas har det också en header:
+ Src: port, helst inte några från well-known ports
+ Dst: port
+ Length,
+ Checksum

TCP-paketet ser istället ut så här:
+ Src port
+ Dst port
+ Sequence number: vilken ordning datapaketen kommer i, slumpad
+ ACK-number: Samma sekvensnummer som nästa paket mottagaren förväntar sig att få.
+ Data offset
+ Reserved: 000
+ Några flaggor: NS, CWR, ECE, URG, ACK, PSH, RST - reset, SYN, FIN - finish
+ Windows Size: Hur många paket sändaren kan skicka innan den får en acknowledge
+ Checksum
+ Urgent pointer: Var i infon ligger det viktigaste?
+ Options

#### VLAN - Virtual Local Area Network

Man sätter ett VLAN-ID för alla paket, antingen i swithchen eller i datorn, skickas med i Ethernet-ramarna.
Man kan spärra, "trunka" taggar osv.
Nackdel: vi lämnar hela säkerheten åt switcharna

## 21/04-17
PDF finns på Classroom. Innehåller det mesta av det vi gått igenom.

### Dagens lektion: switching vs routing
**Två ganska lika processer: information kommer in och en "avgörare" väljer vart trafiken ska gå.** 
Behöver inte gå utanför datorn, t.ex. i VMs.  
Till för att avgöra vart ett datapaket skall ta vägen.  

+ Switching sker på Lager 2, MAC-adresser, säger inget om logisk eller fysisk position.
+ Routing sker på Lager 3, baserat på IP-adresser.
+ Switching kräver mindre CPU-kraft.(sker "snabbare")
+ Båda minskar (ger avbrott) kollisionsdomäner.  

Kollisionsdomän: alla enheter som kan råka "krocka" med varandra.

En switch med fem anslutningar har fem kollisionsdomäner, med varsin pryl. Alltså inga kollisioner!  
#### Switchtabell (finns i primärminne)
| Port | MAC |
|-----:|-----|
| 1    | 1-2-3 |
| 2    | 4-5-6 |
| 3    | 7-8-9 |
| 4    | A-B-C |

När ett paket skickas, kollar den på "från" och lägger detta i switchtabellen, switchen gissar fel om tabellen inte uppdateras, vilket den gör flera gånger per sekund.  
Om inte switchen har "till" i sin tabell, skickar den ut på broadcast, och lär sig när den får svar från rätt dator.  
Switchtabellen kan ha flera MAC på samma port, detta kan hända om det finns en hubb/switch under en port.  
**_KOPPLA ALDRIG SWITCHAR I RING._**  
Stackbara switchar: switchar kan kopplas ihop och låtsas som att de är en switch.  

Det finns tre typer av switchar: L2, L3 och Smart. L2 är oftast bara en plåtlåda.  
Smart har oftast lite förståelse för IP, så att man kan konfiga den.  
L3 är en router-switch, som switchar paket efter att det märkt att det går kommunikation mellan enheter, och routat första paketet.  
Switching sker på LAN (lager 2).    Routing sker mellan nätverk.  
Man skall i teori inte kunna koppla en kabel mellan switchar i olika nätverk.  

En hemmarouter har oftast bara 2 portar. Resten tillhör switchen.  
#### Routingtabell:
| IP-nät        | Port |   Metric |
|:--------------|-----:|---------:|
| 1.2.3.0/24    | 1    |   1 |
| 10.20.0.0/16  | 2    |   1 |
| 100.200.0.0/16 | 3    |   3 |
| 100.200.0.0/16 | 4    |   2 |

Routrar kommunicerar mellan varandra med separata protokoll, och lär sina kompisar sina egna routingtabeller.  
IP skulle från början vara "självläkande", man skulle kunna använda nätet även om det blev sönderbombat.  
Samma nätverk kan finnas på olika portar, paketet tar vägen med minst "metric".  
De flesta ISP-er försöker hålla ett paket inom sitt egna nätverk så länge det går för att hålla ned kostnaderna.  
De flesta routrar tittar inte på mer än Lager 3, men om routern skall agera brandvägg kan den kolla på portar och annan intressant info.  
Default Gateway: IP-nummer till routern. Windows skickar alla paket som inte skall till LAN till dess MAC.  
Varje router kostar lite tid, detta kan ställa till det i Ethernet.  
Rekommmendation: ha ej fler än 4 switchar "på höjden" för att undvika att nätet blir långsamt.

## 05/05-27

### ARP
Address Resolution Protocol  
**Kopplar mellan MAC-adresser & IP-adresser**  
Det är helt ok att köra LAN med endast Ethernet.  
Men eftersom man ville ansluta till Internet, behövde man ha IP också.  
#### ARP-tabell
| IP     | MAC |
|:-------|:----|
| 1.2.3.4 | 3A-4C-11-41-AB-1C |  
Berättar vilken adress en adress finns på.

### DNS
Högst upp: . (root)  
+ Icke-vinstdrivande; .org
+ Kommersiell: .com
+ Regeringar: .gov  

Dessa är toppdomäner.
Man kan alltid fråga en .-server om vem som har hand om en toppdomän.  
**Under dessa finns t.ex.**
+ cisco.com
+ ford.com  

Allt toppdomänen behöver veta är IP-adressen till ciscos domänserver för att nå allt inom cisco.  
Man lade sedan till toppdomäner för varje land. Dessa länder fick sedan sköta sina egna domäner.  
Under .se kan till exempel finnas: volvo.se, telia.se  
Om vi vill exempel till whitehouse.com går detta hela vägen upp till . och sedan ned till dess domänserver.  
Rootservrarna är dödligt viktiga för internet. Vill man döda internet så dödar man dessa.  
UDP-port 53 sker DNS-förfrågningarna på.  
Alla Linux, windows, mac känner till IP-adresserna till rootservrarna.  
Man kan variera hur man sätter upp DNS, t.ex. kan man ha översättningstabeller, mellan olika domäner.  
**All DNS-trafik är okrypterad.**  
Man kan också injicera DNS-svar före den riktiga servern för att sabba för folk.  
Lösningen för detta kallas DNSSec, som har digitala certifikat och krypterade anslutningar.  
DNS-servrar kan också uppdatera varandra, de kan sprida sin info till andra servrar. Detta görs på TCP 53.  
Ur säkerhetssynpunkt är detta en dum idé då detta kan resultera i att skadliga eller dåliga adresser sprids.  
#### DNS-databas
| Namn      |  Typ   |  Värde |
|-----------|:-------|-------:|
| admin     | A      |  216.194.67.119 |
| localhost | A      | 127.0.0.1 |

#### Typer av records:
+ A: ett "rent" namn, det får bara finnas en av dessa.
+ CNAME: Alias för en annan post, text www leder till A-posten
+ MX: Mail exchange
+ NS: Name Server, poster till DNS-servrarna
+ SOA: Start of Authority, talar om vilken dator som är ansvarig för domänen.  

Många gånger är det många poster som leder till samma server.

### Default Gateway
Den dator du skickar paketet till när paketet du skickar inte har samma IP-nät som din egna dator.  
Om min dator har 1.2.3.95/24 och paketet är adresserat till 1.2.3.4 blandar vi inte in default gateway.  
Annars skickar vi bara till default gateway.  
Om vi har ett LAN som är kopplad till en router så har alla i LANet samma DGW.  

### ISP
  Internet Service Provider  
  Det bolag som ger en anslutning till internet.  
  Om man är ett företag kan man avtala med ännu större ISPs.  
## Några viktiga begrepp

### MAC
+ 48 bits (6 bytes)
+ Skrivs vanligtvis i HEX.
+ AB:CD:EF:00:11:22
+ Varje sektion motsvarar en byte.
+ Varje HEX-tal är 4 bits och paren blir en byte.
+ Tre första byten är tillverkarspecifika, och tre sista är en unik del.
+ 2^48 adresser.
+ Används endast på det lokala nätverket, alltså passerar det aldrig en router.
+ ”Bränns” in i kortet vid tillverkning.
+ MAC-spoofing – att ändra MAC i ont syfte.

### IPv4-adresser
+ 32-bits, 4 bytes
+ Något sådant här: 194.45.31.112
+ Varje sektion motsvarar en byte och är decimaltal mellan 0-255
+ Man delar upp adressen i NetID och HostID, gränsen mellan dessa är flytande och bestäms av en subnätmask eller med hjälp av CIDR-notation (ex /24 blir SNM 255.255.255.0)
+ **CIDR**  - Classless Inter-Domain Routing
+ Med CIDR kan gränsen mellan nätID och nodID gå mitt i ett decimalt tal.
+ Därför är det lättare att omvandla adressen till binärt när man jobbar med det.
+ Formel för antal värdar i ett nätverk: 2^(32-CIDR)-2

### Reserved Address Blocks
Många används för privata nätverk mha NAT.
PAT – Port Address Translation - varje dator har en egen port.

+ 127.0.0.0 Loopback
+ 169.254.0.0, Link-Local, slumpad IP när ingen DHCP-server kan nås.
+ 224.0.0.0/4 IP-multicast, servern behöver endast skicka ett paket till närmaste routern, vilket sedan distribueras vidare.

**Unicast** – alla till alla
**Broadcast** – en till alla

Oftast får man inte välja IP, enda gången detta är möjligt är när man sätter upp ett privat nätverk.
Gråa nät (för privat bruk):

+ 10.0.0.0/8
+ 172.16.0.0/12?
+ 192.168.0.0/16

### Värd
+ Ändpunkt i en kommunikation, med en NIC.
+ Lager 3- och smart-switchar är värdar.
+ Allt som har en IP är en värd. 



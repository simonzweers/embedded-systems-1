# Week 5

- [Week 5](#week-5)
  - [Theorie / Vraagstukken](#theorie--vraagstukken)
    - [Computerarchitectuur basis](#computerarchitectuur-basis)
    - [Geheugen](#geheugen)
    - [RAM en ROM](#ram-en-rom)
    - [CPU](#cpu)
    - [Von Neumann-cyclus](#von-neumann-cyclus)
    - [Instructieset](#instructieset)
    - [Stack pointer](#stack-pointer)
    - [Speciale CPU-registers](#speciale-cpu-registers)
    - [IO](#io)
    - [Exceptions](#exceptions)
    - [Systeembussen](#systeembussen)
    - [PCI-express](#pci-express)
    - [DMA](#dma)
    - [Filesystemen](#filesystemen)
  - [Lightrider](#lightrider)
    - [Hoe werkt het?](#hoe-werkt-het)
    - [Schematisch en fysiek](#schematisch-en-fysiek)

## Theorie / Vraagstukken

### Computerarchitectuur basis

`CPU`: De Central Processing Unit voert alle instructies in een computersysteem uit.
Na een instructie uitgevoerd te hebben slaat deze

`Bus`: Een bus is een connectie waarover data verzonden.

`Databus`: Over deze bus wordt data verzonden.

`Adresbus`: De data op deze bus bepaald op welk geheugenadres de data gelezen of geschreven wordt.

![computer-schematisch](../assets/computersysteem-schematisch.png)

`Controlebus`: Op de controlebus staan veel verschillende soorten signalen.
Een voorbeeld ervan is het signaal dat aangeeft of er gelezen of geschreven wordt (ook wel de read-/writelijn).

`Busbreedte`Een bus heeft een bepaalde breedte.
Dit kan bijvoorbeeld 4, 8, 16, 32 of zelfs 128 bits zijn.
Als bijvoorbeeld de adresbus 16 bits breed is, kan de CPU $2^{16} = 65536$ unieke geheugenadressen aanspreken.

### Geheugen

`Geheugen`: Geheugen is er voor de CPU om instructies, en het resultaat van een instructie in op te slaan.
Onder geheugen versta je in de informatica meestal RAM.
SSD's en spinning disks vallen ook onder geheugen, maar deze zijn niet direct bereikbaar door de CPU (dit valt onder IO).

`Geheugencapaciteit`: Geheugencapaciteit beschrijft hoeveel bits een geheugenmodule kan opslaan.
Het totaal aantal bits wordt bepaald door het aantal bits op een geheugenadres en het aantal geheugenadressen te vermedigvuldigen.  
Het aantal adreslijnen op een geheugenmodule bepaald het aantal geheugenadressen.
Met de formule $2^n$ waarin $n$ het de breedte van de adresbus is.
De adres- en datalijnen van de geheugenmodule en CPU worden op elkaar aangesloten.

![ram-simple](../assets/ram-simple.png)

### RAM en ROM

`ROM`: ROM staat voor Read-only memmory.
Dit is geheugen waar alleen gelezen van wordt.
Er zijn verschillende soorten ROM-geheugen:  
ROM: Hierop wordt 1 keer door de fabrikant op geschreven en kan daarna niet meer veranderd worden.  
PROM: Programmable ROM;
Hierop kan 1 keer door de gebruiken op geschreven worden door de gebruiker.  
EPROM: Erasable Programmable ROM;
De inhoud kan met behulp van ultraviolet licht gewist worden, en daarna opnieuw geprogrammerd worden.  
EEPROM: Electronically Erasable Programmable ROM;
De inhoud kan gewist worden met een elektronisch signaal.
EEPROM wordt ook wel flashROM of flashmemory genoemd.

`RAM`: RAM staat voor Random Access memory.
RAM kan van gelezen worden en naartoe geschreven worden.
Er zijn ook verschillende soorten RAM:  
`SRAM`: Static RAM; SRAM behoud de data op de ram zolang de spanning op de voeding aanwezig blijft.  
`DRAM`: Dynamic RAM; verliest na een tijdje zijn data.
Meestal duurt dat in de praktijk rond de 2 ms.
De "refresh cycle" zorgt ervoor dat de RAM module zijn data niet permanent verliest.
Deze "refresh cycle" wordt uitgevoerd door de MMU.
Een DRAM module heeft vaan een opslagcapaciteit van 4 keer zo groot als de SRAM module.  
De adreslijnen van DRAM moeten gemultiplexed worden.
Eerst wordt de "row" van de module aangegeven.
Er wordt dan een signaal gegeven op de RAS (row address strobe) ingang.
Daarna wordt de "column" aangegeven.
Zo dan de CPU een bepaalde memory cell aanroepen.
Voor de "row" en "column" worden de zelfde adreslijnen gebruikt, vandaar het multiplexen.

![dram](../assets/dram-schematisch.png)

`Simm`: Een simm is een printplaatje dat bestaat uit een aantal parallelgeschakelde DRAM modules.
De eerste simm's hadden 30 pins en zagen er schematisch ongeveer zo uit:

![simm30pins](../assets/simm30pins-schem.png)

Zo ziet een simm er schematisch uit:

![simmschem](../assets/simm30pins-technisch.png)

### CPU

Hier is een schematische weergave van de CPU:

![cpu-global](../assets/cpu-globale-opbouw.png)

`Byte-enable lijnen`: Veel CPU's kunnen op byte-niveau addresseren.
Dit betekend dat ondanks dat een CPU misschien een databus van 64 bits breed heeft, de CPU toch maar 1 of meerdere bytes kan lezen of schrijven.
Bijvoorbeeld voor als je een datatype van 2 bytes groot hebt.

![byte-enable](../assets/cpu-byte-enablelijnen.png)

`General-purpose registers`:
De CPU heeft allemaal general-purpose registers.
Deze registers worden voor allerlei verschillende acties gebruikt, bijvoorbeeld het optellen, bit-shiften etc.

`Program counter`:
Ook wel instruction pointer genoemd.
De program counter bevat het geheugenadres van de instrcutie die uitgevoerd moet worden.
Tijdens het uitvoeren wordt de inhoud van de program counter veranderd naar het adres van de volgende instructie die uitgevoerd moet worden.

`Stack pointer`:
Wijst naar het eerstvolgende vrije adres op de STACK (wordt later teogelicht bij [stack pointer](#stack-pointer)).

`Instructieregister`:
De CPU instructie die op het adres in de program counter staat wordt in het instructieregister geladen, om vervolgens uitgevoerd te worden door de CPU (ALU).

`TU`:
Timing unit.
Genereerd 1 of meerdere kloksignalen voor de interne logica van de CPU.

`ICU`:
Internal control unit.
Bestuurd het geheel van de CPU.
De ICU zorgt ervoor dat de timing signalen uit de TU netjes verdeeld worden, en dat het datatransport tussen registers binnen de CPU.
Het zorgt er dus voor dat CPU instructies uitgevoerd worden.

`ALU`:
Arithmetic Logic Unit.
De ALU is de rekenkracht van de CPU.
CPU instructies worden in de ALU uitgevoerd, en het resultaat daarvan wordt opgeslagen.
De ALU neemt 1 of 2 waardes data als input, en geeft naast een output ook statusinformatie die in het statusregister opgeslagen worden.

`Statusregister`:
De statusinformatie die de ALU na een instrucie oplevert wordt in het statusregister opgeslagen.
Deze informatie bestaat uit enkele bits die een eigen functie hebben.
Dit zijn van bits die bepaalde informatie weergeven:  
zero-bit: is 1 als de instructie 0 als resultaat heeft.  
carry-bit: is 1 als er een carry is bij de hoogste bits (MSB) van de twee getallen:
overflow-bit: is 1 als er bij een 2-complement bewerking overflow is opgetreden.  
De individuele bits worden "status flags" genoemd.

### Von Neumann-cyclus

`Von Neumann-cyclus`:
Verzamelnaam voor het ophalen en uitvoeren van 1 CPU instructie.
De Von Neumann-cyclus bestaat uit 4 stappen:

1. **Fetch**:
De inhoud van de program counter wordt op de adresbus gezet.
Er wordt een leessignaal op de controlebus gegeven.
De instructie op het geheugenadres wordt op de databus gezet.
De instructie wordt op het instructieregister gezet.
De inhoud van de program counter wordt bijgewerkt.

2. **Decode**:
De ICU decodeerd de instructies.
Er wordt gekeken naar de de instructie en de operands, zodat die doorgegeven kunnen worden aan de ALU.

3. **Execute**:
De instructie die door de ICU gedecordeerd is wordt uitgevoerd.

4. **Store**:
Het resultaat van de instructie wordt opgeslagen in een van de CPU registers.

### Instructieset

De instructies die de CPU uitvoert zijn eenen en nullen.
Dit wordt ook wel machinetaal genoemd.
Omdat machinetaal niet goed leesbaar is voor de mens, worden afkortingen toegekent aan elke instructie.
Deze afkortingen worden assemblertaal genoemt.

`Assemblertaal`: Programmeertaal die bestaat uit de afkoritingen van CPU-instructies.

`Little-endian`: De CPU leest eerst de lower byte.

`Big-endian`: De CPU leest eerste de higher byte

`Subroutine`: Een reeks processorinstructies die wordt aangeroepen met een bepaalde instructie.
Deze reeks staat op een andere plek dan de reeks instructies die de CPU aan't uitvoeren was.
Met een return-from-subroutine instructie keert het programma weer terug naar de instructies die de CPU aan het uitvoeren was.

Instructies zijn in een aantal groepen te categoriseren:

- Het laden van registers. Het opslaan van registers in geheugen.
- Rekeninstructies: optellen, aftrekker, verhogen-/verlagen met 1, vermenigvuldigen, delen, shiften.
- Logische operators: AND, OR, EXOR, NOT, en testen van bits.
- Spronginstructies: absolute en relatieve sprongen. Contitional/unconditional.
- Subroutine aanroep/return from subroutine: Het aanroepen van een stukje geheugen met instructies. Om weer terug te komen na de subroutine.
- Speciale instructies: NOP(no-operation), HALT(stop), instructies die de mode van een CPU kunnen veranderen.

### Stack pointer

De stack pointer is een speciaal register in de CPU.
De Stack pointer bevat het geheugenadres van het eerstvolgende vrije geheugenadres van de Stack.

`Stack`:
De stack is een stuk geheugen waarop geheugenadressen worden opgeslagen.
Deze deze geheugenadressen wijzen naar instructies waar de CPU na een subroutine terug naar moet keren.
De geheugenadressen worden een voor een weer geladen in de Program counter zodat de CPU met deze instrucie verder gaat.

`Subroutine`:
Een reeks processorinstructies die wordt aangeroepen met een bepaalde instructie.
Deze reeks staat op een andere plek dan de reeks instructies die de CPU aan't uitvoeren was.
Met een return-from-subroutine instructie keert het programma weer terug naar de instructies die de CPU aan het uitvoeren was.

### Speciale CPU-registers

`Indexregister`:
Slaat het adres van een reeks bijbehorende data (bijvoorbeeld een array) op.
Als je een array hebt, kun je met het indexregister een bepaalde waarde uit de array benaderen.

`Loopcount`:
Functioneert als teller bij het uitvoeren van herhalingen.

`Framepointer`:
Hogere level programmeertalen gebruiken dit register om bij te houden in welke subroutine een proces zit.

### IO

`Memory mapped IO`:
Meest gebruikte vorm van IO.
De CPU kan memory mapped IO op in het RAM zetten.
De CPU kan dus communiceren met peripherals door het RAM te gebruiken.
Memory-mapped IO gebruikt de normale CPU-instructies.  
Een paar voordelen van memory mapped IO zijn:

- Dezelfde instructies kunnen voor RAM en IO gebruikt worden.
- Het is mogelijk voor heel veel CPU's om zo te communiceren met IO.

Een aantal nadelen kunnen zijn:

- IO neemt geheugenadressen in beslag. Vooral nadelig voor CPU's met een kleine adresbus.
- Het selecteren van een IO poort werkt hetzelfde als het selecteren van andere adressen. Dit maakt het selecteren van IO poorten iets ingewikkelder.

`Speciale IO`:
Dit geheugen is met speciale instructies aan te spreken voor IO.
Deze instructies kunnen speciaal geheugen adresseren.
De CPU communiceert met de peripherals door speciale poorten of hardware interfaces.
Wordt vaak gebruikt voor situaties waar snelle communicatie tussen de CPU en de IO nodig is.

![io-spaces](../assets/io-soorten.png)

### Exceptions

Bij een exception zet zet de CPU de waardes van de registers op de stack. Daarna word naar een geheugenplaats gesprongen waar de exception handler staat.

Er zijn verschillende soorten exceptions:

- Hardware interrupts: Signalen van buitenaf, bijvoorbeeld uit IO.
- Interne exceptions die voorkomen bij het afhandelen van een instructie. Een voorbeeld is delen door 0, of een niet-bestaande instructie, wat ook wel een "trap" genoemd word.
- Software interrupt: Zelfde effect als een hardware interrupt. Wordt vanuit de software aangeroepen.

`Hardware interrupt`:
Een hardware interrupt is een signaal dat wordt gezet op de controlebus zodat de CPU het kan lezen.
Meestal is het IO die werk heeft voor de CPU, bijvoorbeeld als er een input klaar staat, bijvoorbeeld een binnengekomen package over internet.
In het geval van internet is de netwerkinterface verantwoordelijk voor het geven van de interrupt.
Zo reageert de CPU meestal op een interrupt:

- Zet de waardes van alle registers, program counter en statusregister op de stack.
- Laad de program counter met het adres van de geheugenplek van de ISR en ga deze uitvoeren. De ISR wordt afgesloten met een RTI(return from interrupt) instructie.

`ISR`:
Interrup Service Routine.
Een stuk code deat uitgevoerd word als de CPU een bepaalde interrupt krijgt.
De ISR is altijd op een vast geheugenadres te vinden, genaamd de "exception vector".

`Exception vector`:
Een vast adres in geheugen waarin de eerste instructie van een exception handler.
Het programma dat begint op de de exception vector wordt uitgevoerd als er een exception optreed in de CPU.

`Non-maskable interrupt`:
De CPU kan sommige interrupts negeren.
Non-maskable interrupts kunnen niet genegeert worden door de CPU.
Deze wordt in noodsituaties, zoals falende hardware of wegvallende spanning gebruikt.

### Systeembussen

Voor dembesturing van een computersysteem kun je de CPU centraal stellen.
Dit werkt goed voor kleine systemen.  
Je kan ook de bus centraal stellen.
Dit heeft het voordeel dat bussen dan gedisigned worden om met meerdere CPU's te werken.
Als de bus centraal staat is het makkelijker om meerdere componenten te gebruiken in het systeem.
Het is dan niet systeemspecifiek.
Een leverancier kan dan hardware designen op basis van de bus, zodat de hardware met veel verschillende CPU's kan samenwerken.

`Initiator`:
De initiator start communicatie via de bus; neemt initiatief.

`Target`:
Communiceert over de bus nadat de initiator met de target communicatie heeft gestart.

`Busmaster`:
Een busmaster kan de bus claimen en datatransfers starten.
Als een busmaster probeert te communiceren met een niet-bestaande busslave onstaat er een buserror.
Een buserror is een signaal op de controlebus dat gegeven wordt na een bepaalde tijd.
Er wordt voor het geven van de buserror een harwaretimer gestart.
De bus kan initiator en target zijn.

`Busslave`:
De busslave kan niet de bus voor zichzelf claimen.
De busmaster initialiseerd datatransfer met de busslave.
De bussslave wacht dus tot de de busmaster communicatie start.
De busslace kan alleen maar target zijn.

`Asynchrone bus`:
Bustransfers zijn niet direct gekoppeld aan het kloksignaal.  
Het VMA (Valid Memory Address) signaal geeft aan of het adres op de bus stabiel is.  
Het DTACK(Datatransfer Acknowledge)-signaal geeft aan of de data beschikbaar is op de databus.  
read/!write signaal geeft net zoals altijd aan of er gelezen of geschreven moet worden.  
Als het DTACK-signaal niet gegeven wordt, zal de read/write cyclus langer duren.
Als dit te lang duurt dan zal er een bus error gegeven worden.
Dit heet een bustimeout.

![async-bus](../assets/asynchrone-bus.png)

`Synchrone bus`:
Alle bustransfers zijn direct gerelateerd aan het kloksignaal.
Bij elke klokcyclus is er datatransfer mogelijk.
Als de slave niet voor het eind van de klokcyclus niet klaar is, geeft het het WAIT signaal.
Als bij de volgende klokpuls het WAIT signaal high is wordt de read/write cyclus met een "wait state" verlengt.

![sync-bus](../assets/synchrone-bus.png)

### PCI-express

De PCIe bus wordt in veel PC's gebruikt.
Door de hoge snelheid van PCIe kunnen er veel apparaten verbonden worden met de CPU, zoals netwerkkaarten GPU, andere IO.
Een PCIe bus is eigenlijk meer een point-to-point verbinding.  
Hieronder is een mogelijke systemarchitectuur op basis van PCIe.

![pcie](../assets/pcie-bus.png)

### DMA

Bij DMA (Direct Memory Access) word de CPU geholpen met IO door een DMA-controller of DMAC.
Een CPU kan een DMAC gebruiken voor het lezen van een diskblok op een harde schijf.
De DMAC krijgt van de CPU te horen waar in RAM de diskdata opgeslagen moet worden en hoeveel bytes de file is.
De DMAC vraagt of de bus beschikbaar is.  
Schrijfcyclus van de DMAC:

1. Er is input van de IO
2. De IO geeft een data request-singaal aan de DMAC
3. De De CPU geeft een bus acknowledge als de bus beschikbaar is
4. De DMAC geeft een data acknowledge aan het IO device.
Het IO device zet de databus.
De DMAC geeft het vrije RAM adres waar de data naartoe geschreven kan worden, en stuurt het proces aan via de controlebus.

![dma-proces](../assets/dmac-proces.png)

### Filesystemen

Meestal bestaat een disk uit blokken en clusters.
Een cluster bestaat dan weer uit meerdere clusters.

`FAT`:
File Allocation Table.
Een FAT is een tabel waarin staat waar files op een disk staan.
Een FAT gebruikt 16 bits getallen, wat betekend dat er maximaal 65536 diskblokken kunnen zijn.
Voor moderne disks is dit niet genoeg, daarom wijst een FAT getal meestal naar een cluster.
Een cluster is dan meestal meerdere blokken groot, maar een cluster kan ook 1 blok groot zijn.
In een directory entry staan dingen zoals de filenaam, extensie en het startblok.
Het FAT gatal bevat het clusternummer van het volgende cluster.
Dit blijft zich opvolgen tot de end-of-file of EOF.
Door de dist op te splitten in partities kun je de clustergrootte verminderen.
Elke partitie is zijn eigen filesysteem, en heeft ook zijn eigen FAT.

![fat](../assets/fat-filesysteem.png)

`Unix i-node`:
Een i-node filesysteem bestaat uit de volgende onderdelen:

1. super block
2. i-node bitmap
3. zone bitmap
4. i-nodes
5. data-zones

![unix-inode](../assets/unix-inode.png)

**Super block**:  
Belangrijke informatie over het filesysteem.
Het is meestal op een vaste plek opgeslagen op de disk.
Het bevat informatie zoals:  
De grootte van het filesysteem, het aantal i-nodes en de plaats van de i-node bitmap, zone bitmap, i-nodes en datagebied.

**I-node bitmap**:
In de inode bitmap staat welke i-nodes vrij of in gebruik zijn.
Voor 1 i-node wordt er 1 bit gebruikt.

**Zone bitmap**:
In de zone bitmap staat welke zones/clusters er vrij of in gebruik zijn.
Voor 1 cluster wordt net zoals bij de i-node bitmap 1 bit gebruikt.

**Inodes**:
In het i-node gedeelte staan welke i-nodes er zijn.
Een i-node bevat i-node zonenummers.  
Dit is de opbouw van een inode:  

1. Metadata zoals filetype, size, owner, group, permissions, links, times.
2. De eerste 7 zonenummers wijzen naar een zone met data erin.  
3. *Indirect zone*: Het volgende nummer wijst naar een zone waar geen data in staat, maar zonenummers van datazones. Er zit 1 laag tussen de datazones en het i-node nummer.
4. *Double indirect zone*: Dit nummer wijst naar een zone, waar de zonenummers van andere zones staan. De zones uit deze zone wijzen naar datazones. De vandaar de naam double indirect zone. Er zitten 2 lagen tussen de datazones en het i-node nummer.
5. *Triple inderect zones*: Dit nummer wijst naar een zone, die naar zones wijst, die naar zones wijzen, die naar datazones wijzen. Er zitten 3 lagen tussen de datazones en het i-node nummer.

**Data-zones**:
In het datagebied staat de inhoud van de files.

## Lightrider

In deze opdracht maak ik de verlichting van KITT uit Knight Rider (uit de jaren 80, dus daar ben ik te jong voor). De bedoeling is dat het knipperen sneller of langzamer kan worden dmv van een potentiometer.

### Hoe werkt het?

[Het programma](./light-rider/src/main.c) werkt door steeds 1 led aan en de rest uit te zetten. Ik heb de `checkDigits()` en de `setLedValues()` functie uit [project led bar tellen](../week2/led-bar-tellen/src/main.c) hergebruikt. De `setLedValues()` functie is iets aangepast om geen binair getal weer te geven maar een enkele led aan te doen. Het getal 1 word gebitshift met de index van de led die aan moet gaan, en zo word bepaald welke van de leds aan moet.

```c
void setLedValues (int numberToConvert, int numOfDigits, int ledPinsArr[]) {
    for (size_t i = 0; i < numOfDigits; i++)
    {
        if (checkDigits(1 << numberToConvert, i)) {
            printf("1");
            gpio_set_level(ledPinsArr[i], HIGH);
        } else {
            printf("0");
            gpio_set_level(ledPinsArr[i], LOW);
        }
    }
}
```

Er is ook een variabele die als teller dient (`ledOnValue`), Deze word met 2 for-loops 9 keer verhoogd en verlaagd. Met de waarde in de teller wordt de `setLedValues()` functie gebruikt om een led uit `ledPinsArray[]` aan te zetten. Na het verhogen van de teller wordt de signaal van de potentiometer uitgelezen naar `adc1Read`, en de loop wordt dan gedelayt op basis van de de waarde daarvan.

### Schematisch en fysiek

![schematisch](../assets/lightrider/lightrider_bb.png)
![fysiek](../assets/lightrider/lightrider_fysiek.jpg)

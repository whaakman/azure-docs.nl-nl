---
title: Opslaginfrastructuur beheren voor Azure Stack | Microsoft Docs
description: Opslaginfrastructuur beheren voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: ''
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 03/11/2019
ms.reviewer: jiahan
ms.openlocfilehash: 4a8287d7ca4da380ad7c2b1e039ab3058ca07a96
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57760270"
---
# <a name="manage-storage-infrastructure-for-azure-stack"></a>Opslaginfrastructuur beheren voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Dit artikel beschrijft de status en de operationele status van Azure Stack-infrastructuur opslagbronnen. Deze bronnen omvatten harde schijven en volumes. De informatie in dit onderwerp kan bijzonder waardevol zijn bij het oplossen van verschillende problemen, zoals een schijf kan niet worden toegevoegd aan een groep.

## <a name="understand-drives-and-volumes"></a>Informatie over schijven en volumes

### <a name="drives"></a>Stations

Azure Stack, mogelijk gemaakt door Windows Server-software gedefinieerd opslagmogelijkheden, met een combinatie van Storage Spaces Direct (S2D) en Windows Server Failover Clustering voor een goed presterende en schaalbare en flexibele storage-service.

Azure Stack geïntegreerde system-partners bieden talloze variaties van de oplossing, met inbegrip van een breed scala aan flexibele opslag. Op dit moment kunt u een combinatie van drie stationstypen selecteren: NVMe (niet-vluchtige geheugen Express), SATA/SAS SSD (Solid-State Drive), HDD (hardeschijfstation).

Storage Spaces Direct is uitgerust met een cache voor maximale prestaties van opslagruimte. In Azure Stack-apparaat met één of meerdere typen stations, opslagruimten Direct automatisch gebruik van alle schijven van de 'snelste' (NVMe &gt; SSD &gt; HDD) voor het opslaan in cache. De resterende stations worden gebruikt voor opslagcapaciteit. De stations kunnen worden gegroepeerd in een 'all-flash' of 'hybride' implementatie:

![Azure Stack-opslaginfrastructuur](media/azure-stack-storage-infrastructure-overview/image1.png)

All-flash implementaties zich op het optimaliseren van prestaties van opslagruimte en maak daarbij dan geen roterende hardeschijfstations (HDD).

![Azure Stack-opslaginfrastructuur](media/azure-stack-storage-infrastructure-overview/image2.png)

Hybride implementaties richt u op de balans tussen prestaties en capaciteit of op de maximale capaciteit en omvatten roterende hardeschijfstations (HDD).

Het gedrag van de cache wordt bepaald automatisch op basis van het type van stations die zijn wordt in de cache opgeslagen. Wanneer u caching voor SSD-schijven (zoals NVMe SSD's opslaan in cache), worden alleen schrijfbewerkingen in cache opgeslagen. Dit vermindert de slijtage van de schijven van de capaciteit, het cumulatieve verkeer voor de schijven van de capaciteit te beperken en uitbreiden van hun levensduur. In de tussentijd omdat leesbewerkingen niet significant van invloed op de levensduur van flash en SSD-schijven bieden universeel lage latentie voor lezen, leesbewerkingen zijn niet in cache opgeslagen. Wanneer de cache voor harde schijven (bijvoorbeeld SSD in cache opslaan voor HDD's), beide leest en schrijft in de cache opgeslagen, zodat flash-achtige latentie (vaak ~ 10 x beter) voor beide.

![Azure Stack-opslaginfrastructuur](media/azure-stack-storage-infrastructure-overview/image3.png)

Voor de configuratie van opslag, kunt u Azure Stack OEM-partners controleren (https://azure.microsoft.com/overview/azure-stack/partners/) voor gedetailleerde specificaties.

> [!Note]  
> Azure Stack-apparaat kan worden geleverd in een hybride implementatie, met zowel harde schijven en SSD (of NVMe)-schijven. Maar de schijven van snellere type moeten worden gebruikt als cachestations en alle andere stations moeten worden gebruikt als de schijven van de capaciteit als een groep. De gegevens van de tenant (blobs, tabellen, wachtrijen en schijven) zouden worden geplaatst op capaciteit stations. Dus richten premium-schijven of premium storage-account type betekent niet dat de objecten selecteren worden gegarandeerd moet worden toegewezen op SSD of NVMe-stations en betere prestaties krijgen.

### <a name="volumes"></a>Volumes

De *opslagservice* partities van de beschikbare opslag in afzonderlijke volumes die zijn toegewezen voor het opslaan van systeem-en tenant. Volumes combineren de stations in de opslaggroep om de fouttolerantie, schaalbaarheid en prestatievoordelen van opslagruimten Direct te introduceren.

![Azure Stack-opslaginfrastructuur](media/azure-stack-storage-infrastructure-overview/image4.png)

Er zijn drie typen van volumes die zijn gemaakt op Azure Stack-opslaggroep:

-   Infrastructuur: host-bestanden die worden gebruikt door Azure Stack-infrastructuur-VM's en core services.

-   Virtuele machine tijdelijk: host de tijdelijke schijven gekoppeld aan tenant-VM's en dat gegevens worden opgeslagen in deze schijven.

-   Store-object: hostgegevens van de tenant onderhoud blobs, tabellen, wachtrijen en VM-schijven.

In een implementatie met meerdere knooppunten ziet u de volumes voor drie infrastructuur, terwijl het aantal VM-Temp volumes en Object Store volumes gelijk aan het aantal knooppunten in de Azure Stack-implementatie is:

-   Op een implementatie met vier knooppunten, moet u er vier gelijk Temp van de VM-volumes en vier gelijk Object Store volumes zijn.

-   Als u een nieuw knooppunt aan het cluster toevoegt, wordt een nieuw volume voor beide typen worden gemaakt.

-   Het aantal volumes blijft de hetzelfde, zelfs als een knooppunt goed werkt of is verwijderd.

-   Als u de Azure Stack Developer Kit gebruikt, is er één volume met meerdere bestandsshares.

Volumes in opslagruimten Direct bieden tolerantie voor bescherming tegen problemen met de hardware, zoals de stations- of serverfouten en inschakelen van continue beschikbaarheid in de gehele serveronderhoud, zoals software-updates. Azure Stack-implementatie maakt gebruik van spiegelen in drie richtingen om ervoor te zorgen tolerantie voor gegevens. Drie kopieën van gegevens van de tenant worden geschreven naar verschillende servers, waar ze worden neergezet in cache:

![Azure Stack-opslaginfrastructuur](media/azure-stack-storage-infrastructure-overview/image5.png)

Spiegelen biedt fouttolerantie door meerdere kopieën van alle gegevens te bewaren. Hoe die gegevens worden striped en geplaatst essentieel is (Zie de blog voor meer informatie), maar het is absoluut waar, om te zeggen dat gegevens die zijn opgeslagen met het gebruik van spiegeling is geschreven, in zijn geheel toe, meerdere keren. Elk exemplaar wordt geschreven naar verschillende fysieke hardware (verschillende stations op verschillende servers) die wordt aangenomen dat onafhankelijk van elkaar uitvallen. Spiegelen in drie richtingen ertegen veilig bestand is ten minste twee hardwareproblemen (schijf of server) op een tijdstip. Bijvoorbeeld, als u nu opnieuw opstarten van één server wanneer er plotseling mislukt een ander station of server, alle gegevens blijven veilige en continu toegankelijk is.

## <a name="volume-states"></a>Volume-statussen

Als u wilt weten welke status volumess worden, gebruik de volgende PowerShell-opdrachten:

\$scaleunit\_name = (Get-AzsScaleUnit)\[0\].name

\$subsystem\_name = (Get-AzsStorageSubSystem -ScaleUnit \$scaleunit\_name)\[0\].name

Get-AzsVolume - ScaleUnit \$scaleunit\_- StorageSubSystem naam \$subsysteem\_naam | Select-Object VolumeLabel, HealthStatus, OperationalStatus, RepairStatus, beschrijving, actie, TotalCapacityGB, RemainingCapacityGB

Hier volgt een voorbeeld van uitvoer met een volume ontkoppeld en een gedegradeerd/onvolledig volume:

| VolumeLabel | HealthStatus | OperationalStatus |
|-------------|--------------|------------------------|
| ObjStore_1 | Onbekend | Losgekoppeld |
| ObjStore_2 | Waarschuwing | {Gedegradeerd, onvolledige} |

De volgende secties worden de status en de operationele statussen.

### <a name="volume-health-state-healthy"></a>Status van het volume: In orde

| Operationele status | Description |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| OK | Het volume is in orde. |
| Suboptimal | Gegevens wordt niet gelijkmatig over stations worden geschreven.<br> <br>**Actie:** Neem contact op met ondersteuning voor het optimaliseren van schijfgebruik in de opslaggroep. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. U moet herstellen vanuit back-up nadat de mislukte verbinding is hersteld. |


### <a name="volume-health-state-warning"></a>Status van het volume: Waarschuwing

Wanneer het volume zich in een waarschuwingsstatus, betekent dit dat een of meer exemplaren van uw gegevens niet beschikbaar zijn, maar Azure Stack nog steeds ten minste één exemplaar van uw gegevens lezen kunt.

| Operationele status | Description |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| In de service | Azure Stack wordt het volume, zoals herstellen na het toevoegen of verwijderen van een station. Wanneer het herstel voltooid is, moet het volume terug naar de status OK.<br> <br>**Actie:** Wachten op Azure Stack voor u klaar bent met het herstellen van het volume, en controleer de status later. |
| Onvolledig | De tolerantie van het volume wordt verlaagd omdat een of meer schijven is mislukt of ontbreken. De ontbrekende schijven bevatten echter up-to-date kopieën van uw gegevens.<br> <br>**Actie:** Opnieuw verbinding maken met een ontbrekende schijven, vervangt u alle mislukte stations en online brengen van alle servers die offline zijn. |
| Verminderd | De tolerantie van het volume wordt verlaagd omdat een of meer schijven is mislukt of er ontbreken en er verouderd kopieën van uw gegevens op deze stations zijn.<br> <br>**Actie:** Opnieuw verbinding maken met een ontbrekende schijven, vervangt u alle mislukte stations en online brengen van alle servers die offline zijn. |

 

### <a name="volume-health-state-unhealthy"></a>Status van het volume: Niet in orde

Als een volume in een slechte status, is enkele of alle van de gegevens op het volume momenteel niet toegankelijk.

| Operationele status | Description |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Er is geen redundantie | Het volume heeft gegevens verloren gegaan omdat er te veel schijven is mislukt.<br> <br>**Actie:** Neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. |


### <a name="volume-health-state-unknown"></a>Status van het volume: Onbekend

Het volume kan ook worden in de onbekende status als de virtuele schijf is losgekoppeld.

| Operationele status | Description |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Losgekoppeld | Er is een fout bij het apparaat van de opslag opgetreden hetgeen kan leiden tot het volume toegankelijk is. Het is mogelijk dat sommige gegevens verloren.<br> <br>**Actie:** <br>1. Controleer de fysieke en de netwerkverbinding van alle opslagapparaten.<br>2. Als alle apparaten goed zijn aangesloten, neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. U moet herstellen vanuit back-up nadat de mislukte verbinding is hersteld. |

## <a name="drive-states"></a>Status van station

Gebruik de volgende PowerShell-opdrachten voor het bewaken van de status van schijven:

 

\$scaleunit\_name = (Get-AzsScaleUnit)\[0\].name

\$subsystem\_name = (Get-AzsStorageSubSystem -ScaleUnit \$scaleunit\_name)\[0\].name

, SerialNumber

 

Get-AzsDrive - ScaleUnit \$scaleunit\_- StorageSubSystem naam \$subsysteem\_naam | Select-Object StorageNode, PhysicalLocation, HealthStatus, OperationalStatus, beschrijving, actie, gebruik, CanPool, CannotPoolReason, serienummer, Model, MediaType, CapacityGB

De volgende secties beschrijven de statussen die een station, kan zich in.

### <a name="drive-health-state-healthy"></a>Status van station: In orde

| Operationele status | Description |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| OK | Het volume is in orde. |
| In de service | Het station met het uitvoeren van bepaalde bewerkingen voor intern gebruik. Wanneer de actie voltooid is, wordt het station moet terug naar de status OK. |

### <a name="drive-health-state-healthy"></a>Status van station: In orde

Een station in de status waarschuwing kan lezen en schrijven van gegevens is, maar er een probleem is.

| Operationele status | Description |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Verbroken | Verbinding is verbroken naar het station.<br> <br>**Actie:** Alle servers weer online brengen. Als dat niet het wordt opgelost, sluit u het station. Als dit plaatsvinden blijft, vervangt u het station om te controleren of de volledige tolerantie. |
| Voorspelde fout | Een fout van het station wordt voorspeld binnenkort plaatsvindt.<br> <br>**Actie:** Vervang het station zo snel mogelijk om te controleren of de volledige tolerantie. |
| I/o-fout | Er is een tijdelijke fout opgetreden bij het openen van het station.<br> <br>**Actie:** Als dit plaatsvinden blijft, vervangt u het station om te controleren of de volledige tolerantie. |
| Tijdelijke fout | Er is een tijdelijke fout opgetreden bij het station. Dit betekent meestal het station is niet meer reageert, maar dit kan ook betekenen dat het Storage Spaces Direct beschermende partitie ongepast is verwijderd uit het station. <br> <br>**Actie:** Als dit plaatsvinden blijft, vervangt u het station om te controleren of de volledige tolerantie. |
| Abnormaal latentie | Het station is het soms niet meer reageert en tekenen van de fout wordt weergegeven.<br> <br>**Actie:** Als dit plaatsvinden blijft, vervangt u het station om te controleren of de volledige tolerantie. |
| Verwijderen uit groep | Azure Stack wordt momenteel het verwijderen van het station van de opslaggroep.<br> <br>**Actie:** Wachten op Azure Stack voor u klaar bent met het verwijderen van het station en daarna de status controleren.<br>Als de status blijft, neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. |
|  |  |
| Onderhoudsmodus starten | Azure Stack wordt het station in de onderhoudsmodus plaatsen. Dit is een tijdelijke situatie: het station moet binnenkort worden in de status In onderhoudsmodus.<br> <br>**Actie:** Wachten op Azure Stack voor het proces te voltooien en daarna de status controleren. |
| In de onderhoudsmodus | Het station is in de onderhoudsmodus bevindt, stoppen, leest en schrijft uit het station. Dit betekent meestal dat beheer van Azure Stack-taken, zoals PNU of FRU het station wordt uitgevoerd. Maar de beheerder kan ook het station plaatsen in de onderhoudsmodus bevindt.<br> <br>**Actie:** Wachten op Azure Stack om de beheertaak te voltooien en daarna de status controleren.<br>Als de status blijft, neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. |
|  |  |
| Onderhoudsmodus stoppen | Azure Stack wordt momenteel de schijf weer online brengen. Dit is een tijdelijke situatie: het station moet binnenkort in een andere status - in het ideale geval in orde.<br> <br>**Actie:** Wachten op Azure Stack voor het proces te voltooien en daarna de status controleren. |

 

### <a name="drive-health-state-unhealthy"></a>Status van station: Niet in orde

Een station in de slechte status kan op dit moment worden geschreven naar of geopend.

| Operationele status | Description |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Splitsen | Het station heeft worden gescheiden van de groep.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. Als u deze schijf, moet u de schijf verwijderen uit het systeem, zorg ervoor dat er geen bruikbare gegevens op de schijf wissen van de schijf en klikt u vervolgens de schijf vervangen. |
| Kan niet worden gebruikt | De fysieke schijf is in quarantaine geplaatst omdat deze niet wordt ondersteund door de leverancier van uw oplossing. Alleen schijven die zijn goedgekeurd voor de oplossing en de juiste schijffirmware worden ondersteund.<br> <br>**Actie:** De schijf vervangen door een schijf met een goedgekeurde fabrikant en modelnummer voor de oplossing. |
| Verlopen metagegevens | De vervangende schijf eerder werd gebruikt en kan gegevens uit een onbekende opslagsysteem bevatten. De schijf in quarantaine is geplaatst.        <br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. Als u deze schijf, moet u de schijf verwijderen uit het systeem, zorg ervoor dat er geen bruikbare gegevens op de schijf wissen van de schijf en klikt u vervolgens de schijf vervangen. |
| Niet-herkende metagegevens | Niet-herkende metagegevens gevonden op de schijf, dit meestal betekent dat het station metagegevens uit een andere groep erop heeft.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. Als u deze schijf, moet u de schijf verwijderen uit het systeem, zorg ervoor dat er geen bruikbare gegevens op de schijf wissen van de schijf en klikt u vervolgens de schijf vervangen. |
| Mislukte media | Het station is mislukt en niet meer door opslagruimten worden gebruikt.<br> <br>**Actie:** Vervang het station zo snel mogelijk om te controleren of de volledige tolerantie. |
| Hardware-uitval van apparaat | Er is een hardwarestoring optreedt op dit station. <br> <br>**Actie:** Vervang het station zo snel mogelijk om te controleren of de volledige tolerantie. |
| Firmware bijwerken | Azure Stack wordt bijgewerkt via de firmware op het station. Dit is een tijdelijke situatie die duurt minder dan een minuut meestal niet langer en gedurende welke tijd die andere stations in de groep al verwerkt leest en schrijft.<br> <br>**Actie:** Wachten op Azure Stack voor u klaar bent met het bijwerken en daarna de status controleren. |
| Starten | Het station wordt voorbereid voor bewerking. Dit moet een tijdelijke status - als u klaar bent, dat het station moet worden overgezet naar een andere operationele status.<br> <br>**Actie:** Wachten op Azure Stack voor de bewerking niet voltooien, en controleer de status later. |
 

## <a name="reasons-a-drive-cant-be-pooled"></a>Redenen waarom die een schijf kan niet worden samengevoegd

Sommige stations alleen nog niet klaar bent om te worden in Azure Stack-opslaggroep. U vindt informatie waarom een station is niet in aanmerking voor groeperen door te kijken de CannotPoolReason-eigenschap van een station. De volgende tabel geeft nog iets meer details over elk van de redenen.

| Reden | Description |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hardware die niet compatibel zijn | Het station is niet in de lijst met goedgekeurde opslagmodellen opgegeven met behulp van de Health-Service.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. |
| De firmware is niet compatibel zijn | De firmware op de fysieke schijf is niet in de lijst met goedgekeurde firmwarerevisies met behulp van de Health-Service.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. |
| Wordt gebruikt door het cluster | Het station wordt momenteel gebruikt door een Failover-Cluster.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. |
| Verwisselbare media | Het station is geclassificeerd als een verwisselbaar station. <br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. |
| Niet in orde | Het station zich niet in orde en moeten mogelijk worden vervangen.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. |
| Er is onvoldoende capaciteit | Er zijn partities de vrije ruimte op de schijf in beslag.<br> <br>**Actie:** De schijf vervangen door een nieuwe schijf. Als u deze schijf, moet u de schijf verwijderen uit het systeem, zorg ervoor dat er geen bruikbare gegevens op de schijf wissen van de schijf en klikt u vervolgens de schijf vervangen. |
| Verificatie wordt uitgevoerd | De Health-Service wordt gecontroleerd om te zien als het station of de firmware op het station is goedgekeurd voor gebruik.<br> <br>**Actie:** Wachten op Azure Stack voor het proces te voltooien en daarna de status controleren. |
| Verificatie mislukt | De Health-Service kan niet worden gecontroleerd als het station of de firmware op het station is goedgekeurd voor gebruik.<br> <br>**Actie:** Neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. |
| Offline | Het station is offline. <br> <br>**Actie:** Neem contact op met ondersteuning. Voordat u dit doet, het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles. |

## <a name="next-step"></a>Volgende stap

[Storage-capaciteit beheren](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-storage-shares) 
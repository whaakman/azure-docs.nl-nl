---
title: Een cluster beschrijven met Cluster Resource Manager | Microsoft Docs
description: Een Service Fabric-cluster door op te geven domeinen met fouten, upgradedomeinen knooppunteigenschappen en knooppuntcapaciteiten voor Cluster Resource Manager wordt beschreven.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 22ccb21a208bbe8e825bff9f7602bfca05990816
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271643"
---
# <a name="describe-a-service-fabric-cluster-by-using-cluster-resource-manager"></a>Een Service Fabric-cluster met behulp van Cluster Resource Manager wordt beschreven
De Cluster Resource Manager-functie van Azure Service Fabric biedt verschillende mechanismen voor het beschrijven van een cluster:

* Foutdomeinen
* Upgrade domains
* Eigenschappen van het knooppunt
* Knooppuntcapaciteiten

Cluster Resource Manager wordt tijdens runtime gebruikgemaakt van deze informatie om hoge beschikbaarheid van de services die in het cluster wordt uitgevoerd. Tijdens het afdwingen van deze belangrijke regels, probeert deze ook te optimaliseren resourceverbruik binnen het cluster.

## <a name="fault-domains"></a>Foutdomeinen
Een foutdomein is een gebied van gecoördineerde fout. Een enkele machine is een domein met fouten. Het kan mislukken voor een eigen diverse redenen zijn waarom uit power supply, mislukte aanmeldingen station, mislukte aanmeldingen ongeldige NIC-firmware. 

Computers die zijn verbonden met dezelfde Ethernet-switch zijn in hetzelfde foutdomein. Dus zijn computers die één bron van kracht of op één locatie delen. 

Omdat het is natuurlijk voor hardwarefouten laten overlappen, worden foutdomeinen inherent hiërarchische. Ze worden weergegeven als de URI's in Service Fabric.

Het is belangrijk dat foutdomeinen correct zijn ingesteld omdat de Service Fabric gebruikt deze informatie om veilig services. Service Fabric wilt niet plaatsen van services, zodat het verlies van een domein met fouten (veroorzaakt door het uitvallen van een onderdeel) zorgt ervoor een service voor het uitvallen dat. 

In de Azure-omgeving gebruikmaakt van Service Fabric de foutgegevens domein geleverd door de omgeving correct configureren van de knooppunten in het cluster uit uw naam. Voor de instanties van de zelfstandige versie van Service Fabric, worden foutdomeinen gedefinieerd op het moment dat het cluster is ingesteld. 

> [!WARNING]
> Het is belangrijk dat de foutgegevens domein opgegeven voor Service Fabric nauwkeurig is. Laten we zeggen dat uw Service Fabric-cluster knooppunten binnen 10 virtuele machines die worden uitgevoerd op 5 fysieke hosts worden uitgevoerd. In dit geval, zelfs als er 10 virtuele machines zijn, er zijn alleen 5 verschillende (hoogste niveau) foutdomeinen. Delen dezelfde fysieke host zorgt ervoor dat virtuele machines voor het delen van de hetzelfde hoofddomein veroorzaakt omdat de virtuele machines gecoördineerde fout optreden als de fysieke host is mislukt.  
>
> Service Fabric wordt verwacht dat het foutdomein van een knooppunt niet te wijzigen. Andere methoden om te waarborgen hoge beschikbaarheid van de virtuele machines, zoals [HA-virtuele machines](https://technet.microsoft.com/library/cc967323.aspx), kan leiden tot conflicten met Service Fabric. Deze mechanismen transparante migratie van virtuele machines van de ene host naar een andere gebruiken. Ze niet opnieuw te configureren of te laten weten de code uitvoeren binnen de virtuele machine. Ze als zodanig *niet ondersteund* als omgevingen voor het uitvoeren van Service Fabric-clusters. 
>
> Service Fabric moet de gebruikte alleen hoge beschikbaarheid-technologie. Mechanismen zoals live VM-migratie en SAN's zijn niet nodig. Als deze methoden worden gebruikt in combinatie met Service Fabric, ze _verminderen_ beschikbaarheid en betrouwbaarheid. De reden is dat ze extra complexiteit introduceren,, gecentraliseerde bronnen van de fout toevoegen en betrouwbaarheid en beschikbaarheid strategieën die conflicteren met die in Service Fabric. 
>
>

In de volgende afbeelding is de kleur we alle entiteiten die bijdragen aan domeinen met fouten en alle andere foutdomeinen die het resultaat te vermelden. In dit voorbeeld hebben we datacenters ("DC"), rekken ('R') en blades ("B"). Als elk blade meer dan één virtuele machine bevat, kan dit worden veroorzaakt door een andere laag in de domeinhiërarchie veroorzaakt.

<center>

![Knooppunten ingedeeld via domeinen met fouten][Image1]
</center>

Tijdens runtime, Service Fabric Cluster Resource Manager rekening gehouden met de domeinen met fouten in het cluster is en indelingen. De stateful replica's of stateless exemplaren van een service worden gedistribueerd, zodat ze in afzonderlijke foutdomeinen. De service te verdelen over foutdomeinen zorgt ervoor dat de beschikbaarheid van de service wordt niet meer veilig als een foutdomein is mislukt op elk niveau van de hiërarchie.

Cluster Resource Manager zorgt niet dat het aantal lagen er in de domeinhiërarchie veroorzaakt zijn. Er wordt geprobeerd om ervoor te zorgen dat het verlies van gegevens van elk één deel van de hiërarchie heeft geen invloed op services die erin worden uitgevoerd. 

Het is raadzaam als hetzelfde aantal knooppunten op elk niveau van de diepte in de domeinhiërarchie veroorzaakt. Als de "structuur" van domeinen met fouten is niet-regelmatige in uw cluster, is het moeilijker voor Cluster Resource Manager om de beste toewijzing van services te achterhalen. Imbalanced fouttolerantie domein indelingen betekenen dat het verlies van sommige domeinen is van invloed op de beschikbaarheid van services die meer dan andere domeinen. Als gevolg hiervan Cluster Resource Manager gegevenskanaal tussen twee doelen: 

* Het bedrijf wil de machines in dat domein 'dikke' door het plaatsen van services op deze gebruiken. 
* Het bedrijf wil plaatsen van services in andere domeinen, zodat het verlies van gegevens van een domein geen problemen veroorzaken. 

Wat imbalanced domeinen eruit? Het volgende diagram toont twee verschillende cluster-indelingen. De knooppunten worden in het eerste voorbeeld, gelijkmatig verdeeld over de domeinen met fouten. In het tweede voorbeeld heeft één foutdomein veel meer knooppunten dan de andere foutdomeinen. 

<center>

![Twee verschillende cluster-indelingen][Image2]
</center>

De keuze van welke fout domein een knooppunt bevat wordt in Azure, voor u beheerd. Maar afhankelijk van het aantal knooppunten die u inricht, u kunt nog steeds uiteindelijk domeinen met fouten die meer knooppunten in deze dan in andere gevallen hebben. 

Stel dat u hebt vijf domeinen met fouten in het cluster, maar zeven knooppunten voor een knooppunttype in te richten (**NodeType**). In dit geval wordt uiteindelijk de eerste twee foutdomeinen meer knooppunten. Als u doorgaat implementeren meer **NodeType** exemplaren met slechts een paar exemplaren, het probleem slechter opgehaald. Daarom wordt u aangeraden dat het aantal knooppunten in elk knooppunttype een veelvoud zijn van het aantal foutdomeinen is.

## <a name="upgrade-domains"></a>Upgrade domains
Upgradedomeinen zijn een andere functie waarmee u kunt Service Fabric Cluster Resource Manager te begrijpen van de indeling van het cluster. Upgradedomeinen definiëren sets met knooppunten die zijn bijgewerkt op hetzelfde moment. Upgradedomeinen helpen met Cluster Resource Manager te begrijpen en bewerkingen, zoals upgrades indelen.

Upgradedomeinen zijn veel zoals domeinen met fouten, maar met enkele belangrijke verschillen. Eerst definieert u gebieden van gecoördineerde hardwarefouten domeinen met fouten. Upgradedomeinen, aan de andere kant zijn gedefinieerd door het beleid. U krijgt om te bepalen hoeveel u kunt in plaats van zodat het dicteren van het nummer van de omgeving. U kunt zoveel upgradedomeinen zoals u dat wel knooppunten doet hebben. Een ander verschil tussen foutdomeinen en upgradedomeinen is dat upgradedomeinen niet hiërarchisch zijn. In feite in plaats daarvan meer, zoals een eenvoudige label. 

Het volgende diagram toont de drie upgradedomeinen striped verdeeld over drie foutdomeinen. U ziet ook een mogelijke plaatsing voor drie verschillende replica's van een stateful service, waar elke in verschillende domeinen en upgradedomeinen eindigt. Deze plaatsing kunt het verlies van een domein met fouten tijdens het in het midden van een service-upgrade en nog steeds een kopie van de code en gegevens.  

<center>

![Plaatsing met fout- en upgradedomeinen][Image3]
</center>

Er zijn voor- en nadelen voor de grote aantallen upgradedomeinen. Meer upgradedomeinen betekent dat elke stap van de upgrade is meer gedetailleerde en is van invloed op een kleiner aantal knooppunten of services. Minder services hebben om te gaan op een tijdstip, maak kennis met minder verloop in het systeem. Dit doorgaans de betrouwbaarheid, omdat minder van de service wordt beïnvloed door een probleem tijdens de upgrade. Meer upgradedomeinen ook betekenen dat u nodig hebt minder beschikbare buffer op andere knooppunten voor het afhandelen van de impact van de upgrade. 

Bijvoorbeeld, als u vijf upgradedomeinen hebt, omgaan de knooppunten in elk met ongeveer 20 procent van uw verkeer. Als u nodig hebt om dat domein upgrade voor een upgrade, moet deze belasting daadwerkelijk meestal om te gaan op een locatie. Omdat u vier resterende upgradedomeinen hebt, moet elke ruimte voor ongeveer 5 procent van het totale verkeer hebben. Meer upgradedomeinen betekenen dat u minder buffer op de knooppunten in het cluster nodig. 

U kunt als er 10 upgradedomeinen in plaats daarvan. Elk upgradedomein wordt in dat geval alleen ongeveer 10 procent van het totale verkeer verwerken. Wanneer een Upgradestappen via het cluster, moet elk domein voldoende ruimte is voor alleen ongeveer 1.1 procent van het totale verkeer. Meer upgradedomeinen kunnen u uw knooppunten uitvoeren op een hoger gebruik, meestal omdat u minder gereserveerde capaciteit nodig hebt. Hetzelfde geldt voor domeinen met fouten.  

Het nadeel van het aantal upgradedomeinen dat is dat upgrades vaak langer duren. Service Fabric wacht een korte periode na een upgradedomein is voltooid en controleert voordat u begint met het volgende object bijwerken. Deze vertragingen inschakelen detectie problemen die door de upgrade voordat de upgrade wordt uitgevoerd. De verhouding is acceptabel omdat deze ongeldige wijzigingen voorkomt dat van invloed zijn op te veel van de service op een tijdstip.

De aanwezigheid van te weinig upgradedomeinen heeft veel negatieve neveneffecten. Hoewel elk upgradedomein is naar beneden en upgrade wordt uitgevoerd, een groot deel van de totale capaciteit is niet beschikbaar. Bijvoorbeeld, als u alleen voor de drie upgradedomeinen hebt, u geleid naar beneden een derde van uw algehele service of clustercapaciteit tegelijk. Veel mogelijkheden van uw service in één keer dat u niet wenselijk is omdat u voldoende capaciteit in de rest van het cluster moet voor het verwerken van de werkbelasting. Onderhouden dat die buffer houdt in dat tijdens normale werking, zijn deze knooppunten dat kleiner zijn dan ze anders zou zijn geladen. Dit verhoogt de kosten van uw service.

Er is geen echte limiet voor het totale aantal fouten of upgrade-domeinen in een omgeving of beperkingen met betrekking tot hoe ze elkaar overlappen. Maar er zijn algemene patronen:

- Foutdomeinen en upgradedomeinen toegewezen 1:1
- Één upgradedomein per knooppunt (fysiek of virtueel instantie van een besturingssysteem)
- Een 'striped' of 'matrix'-model waarbij de foutdomeinen en upgradedomeinen vormen een matrix met de machines die meestal worden uitgevoerd op de diagonaal

<center>

![Lay-outs van fout- en upgradedomeinen][Image4]
</center>

Er is geen aanbevolen antwoord voor waarvoor lay-out om te kiezen. Elk heeft voor- en nadelen. Bijvoorbeeld, is het model 1FD:1UD eenvoudig te installeren. Het model van één upgradedomein per knooppunt model is het meest, zoals welke personen worden gebruikt voor het. Tijdens upgrades, wordt elk knooppunt onafhankelijk bijgewerkt. Dit is vergelijkbaar met hoe kleine sets machines handmatig zijn bijgewerkt in het verleden.

Het meest voorkomende model is de matrix FD en UD, waarbij de foutdomeinen en upgradedomeinen een tabel vormen en knooppunten vanaf langs de diagonaal worden geplaatst. Dit is het model gebruikt standaard in Service Fabric-clusters in Azure. Voor clusters met veel knooppunten kunt van alles wat eindigt ziet eruit als een compacte matrix-patroon.

> [!NOTE]
> Service Fabric-clusters die worden gehost in Azure ondersteuning geen voor het wijzigen van de standaardstrategie. Alleen zelfstandige clusters bieden die aanpassing.
>

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Fout- en beperkingen van domein en het resulterende gedrag
### <a name="default-approach"></a>Standaard-benadering
Cluster Resource Manager houdt standaard services die zijn verdeeld over fout- en upgradedomeinen. Dit is gemodelleerd als een [beperking](service-fabric-cluster-resource-manager-management-integration.md). De beperking voor domeinen met fouten en upgrade-domeinen statussen: "Voor de partitie van een bepaalde service, moet nooit er een verschil meer dan één in het aantal service-objecten (stateless service-exemplaren of stateful service-replica's) tussen twee willekeurige domeinen op hetzelfde niveau van de hiërarchie."

Stel dat deze beperking biedt de zekerheid 'maximale verschil'. De beperking voor fout- en upgradedomeinen wordt voorkomen dat bepaalde verplaatst of regelingen die strijdig zijn met de regel.

Laten we zeggen dat we beschikken over een cluster met zes knooppunten zijn geconfigureerd met vijf foutdomeinen en vijf upgradedomeinen.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

Nu gaan we zeggen dat we een service met maken een **TargetReplicaSetSize** (of, voor een stateless service **InstanceCount**) waarde van vijf. Het land van de replica's op N1 N5. In feite N6 nooit wordt gebruikt, ongeacht hoeveel services zoals dit die u maakt. Maar waarom? Bekijk het verschil tussen de huidige indeling en wat er zou gebeuren als N6 is gekozen.

Hier volgt de indeling die wij en het totale aantal replica's per fout- en domein:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Deze lay-out is verdeeld in termen van knooppunten per foutdomein en upgradedomein. Dit wordt ook gelijkmatig wat betreft het aantal replica's per probleem- en upgrade-domein. Elk domein heeft hetzelfde aantal knooppunten en hetzelfde aantal replica's.

Nu gaan we kijken wat gebeurt er als wij N6 in plaats van N2 gebruikt hadden. Hoe wordt de replica's worden gedistribueerd vervolgens?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

Deze lay-out is in strijd met onze definitie van de garantie 'maximale verschil' voor de beperking van fout met betrekking tot domein. FD0 heeft twee replica's, terwijl FD1 nul is. Het verschil tussen FD0 en FD1 is een totaal van twee, is groter dan het maximale verschil van één. Omdat de beperking is geschonden, is deze overeenkomst niet toegestaan door Cluster Resource Manager. Op dezelfde manier als we N2 en N6 (in plaats van N1 en N2) verzameld, zou krijgen we:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

Deze lay-out is verdeeld in termen van domeinen met fouten. Maar nu wordt deze de upgradedomein-beperking schenden omdat UD0 nul replica's heeft en UD1 twee heeft. Deze lay-out ook is ongeldig en wordt niet door Cluster Resource Manager worden verzameld.

Deze benadering voor de verdeling van stateful replica's of stateless instanties biedt de best mogelijke fouttolerantie. Als één domein uitgeschakeld wordt, is het minimale aantal replica's / exemplaren verloren gaan. 

Deze aanpak kan aan de andere kant worden te strikt en kan het cluster gebruikmaken van alle resources. Voor bepaalde configuraties van clusters op kunnen niet bepaalde knooppunten worden gebruikt. Dit kan leiden tot Service Fabric niet plaats uw services, wat resulteert in waarschuwingsberichten. In het vorige voorbeeld, enkele van de clusterknooppunten kan niet worden gebruikt (N6 in het voorbeeld). Zelfs als u knooppunten hebt toegevoegd aan het cluster (N7-N10), zou replica's / exemplaren alleen op N1 – N5 worden geplaatst vanwege beperkingen met betrekking tot fout- en upgradedomeinen. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |



### <a name="alternative-approach"></a>Alternatieve methode

Cluster Resource Manager biedt ondersteuning voor een andere versie van de beperking voor fout- en upgradedomeinen. Kan de plaatsing terwijl nog steeds een minimaal niveau van de veiligheid te garanderen. De alternatieve beperking kunt als volgt worden vermeld: "Voor de partitie van een bepaalde service, distributie van de replica in meerdere domeinen moet ervoor zorgen dat de partitie niet ten koste gaan een quorumverlies." Stel dat deze beperking biedt de zekerheid 'quorum veilige'. 

> [!NOTE]
> Voor een stateful service, definiëren we *quorumverlies* in een situatie wanneer een meerderheid van de partitiereplica's zijn niet actief op hetzelfde moment. Bijvoorbeeld, als **TargetReplicaSetSize** is vijf, een set met alle drie replica's quorum vertegenwoordigt. Op dezelfde manier als **TargetReplicaSetSize** is zes, vier replica's die nodig zijn voor het quorum. In beide gevallen wordt niet meer dan twee replica's niet beschikbaar mag zijn op hetzelfde moment als de partitie wil gaan normaal functioneren. 
>
> Voor een stateless service is niet goed als *quorumverlies*. Stateless services blijven normaal werken, zelfs als een meerderheid van de instanties omlaag gaan op hetzelfde moment. Dus, ligt de focus op stateful services in de rest van dit artikel.
>

We gaan terug naar het vorige voorbeeld. Met de "quorum veilige"-versie van de beperking zijn alle drie indelingen geldig. Zelfs als FD0 is mislukt in de tweede lay-out of UD1 is mislukt in de derde lay-out, zou de partitie nog quorum hebben. (Een meerderheid van de replica's zou nog steeds worden omhoog.) Met deze versie van de beperking worden N6 bijna altijd gebruikt.

De 'quorum veilige'-aanpak biedt meer flexibiliteit dan met de methode 'maximale verschil'. De reden is dat het gemakkelijker te vinden van de replica-distributies die geldig in de clustertopologie van vrijwel elk zijn is. Echter deze benadering kan niet garanderen de fout met de beste kenmerken van de tolerantie omdat sommige fouten slechter dan andere zijn. 

In het ergste geval, kan een meerderheid van de replica's zijn met het uitvallen van één domein en één extra replica verbroken. Bijvoorbeeld, in plaats van drie fouten moeten gaan verloren quorum met vijf replica's of instanties, kunt u nu verliezen een meerderheid met slechts twee fouten. 

### <a name="adaptive-approach"></a>Adaptieve benadering
Omdat beide benaderingen sterke en zwakke punten hebben, hebben we een adaptieve benadering die deze twee strategieën combineert geïntroduceerd.

> [!NOTE]
> Dit is het standaardgedrag beginnen met Service Fabric versie 6.2. 
> 
> De aanpak van adaptieve maakt standaard gebruik van de logica 'maximale verschil' en schakelt over naar de logica 'quorum veilige' alleen indien nodig. Cluster Resource Manager wordt automatisch zoekt uit welke strategie nodig is door te kijken hoe het cluster en de services zijn geconfigureerd.
> 
> Cluster Resource Manager moet de logica 'quorum op basis van' gebruiken voor een service beide van deze voorwaarden wordt voldaan:
>
> * **TargetReplicaSetSize** voor de service deelbaar is door het aantal domeinen met fouten en het aantal upgradedomeinen is.
> * Het aantal knooppunten is kleiner dan of gelijk aan het aantal foutdomeinen vermenigvuldigd met het aantal upgradedomeinen.
>
> Houd er rekening mee dat Cluster Resource Manager deze benadering voor staatloze en stateful services gebruikt, ook al quorumverlies is niet relevant voor stateless services.

We gaan terug naar het vorige voorbeeld en wordt ervan uitgegaan dat een cluster heeft nu acht knooppunten. Het cluster nog steeds is geconfigureerd met vijf foutdomeinen en vijf upgradedomeinen, en de **TargetReplicaSetSize** waarde van een service die wordt gehost op dat cluster blijft vijf. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

Omdat alle noodzakelijke voorwaarden wordt voldaan, is met Cluster Resource Manager gebruikt de logica 'quorum op basis van' in het distribueren van de service. Hierdoor is het gebruik van N6 N8. Een service die mogelijk distributie in dit geval als volgt uitzien:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

Als de service **TargetReplicaSetSize** waarde is verlaagd tot vier (bijvoorbeeld), met Cluster Resource Manager ziet deze wijziging. Het wordt hervat met behulp van de logica 'maximale verschil' omdat **TargetReplicaSetSize** niet meer wordt dividable door het aantal foutdomeinen en upgradedomeinen. Als gevolg hiervan wordt bepaalde verplaatsingen van het type replica uitgevoerd voor het distribueren van de resterende vier replica's op knooppunten N1 N5. Op die manier kunnen niet door de 'maximale verschil'-versie van de logica fouttolerantie domein en de upgrade is geschonden. 

In de vorige lay-out, als de **TargetReplicaSetSize** waarde is vijf en N1 is verwijderd uit het cluster, wordt het aantal upgradedomeinen gelijk aan vier. Nogmaals, met Cluster Resource Manager opstart met behulp van 'maximale verschil' logische omdat het aantal upgradedomeinen niet gelijkmatig van de service verdelen **TargetReplicaSetSize** meer waarde. Als gevolg hiervan moet replica R1, tijdens het opnieuw, land op N4 zodat de beperking voor het domein met fout- en niet wordt geschonden.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |N/A |N/A |N/A |N/A |N/A |N/A |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

## <a name="configuring-fault-and-upgrade-domains"></a>Fout- en upgradedomeinen configureren
In Azure gehoste Service Fabric-implementaties, worden foutdomeinen en upgradedomeinen automatisch gedefinieerd. Service Fabric opgehaald en worden de omgevingsgegevens van Azure gebruikt.

Als u uw eigen cluster maakt (of wilt uitvoeren van een bepaalde topologie in ontwikkeling), kunt u het foutdomein bieden en domeingegevens zelf bijwerken. In dit voorbeeld definiëren we een cluster met negen-node lokale ontwikkeling die drie datacenters (elk met drie rekken omvat). Dit cluster heeft ook drie upgradedomeinen striped verdeeld over deze drie datacenters. Hier volgt een voorbeeld van de configuratie in ClusterManifest.xml:

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one box/one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

In dit voorbeeld maakt gebruik van ClusterConfig.json voor zelfstandige implementaties:

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> Tijdens het definiëren van clusters via Azure Resource Manager, wordt Azure wijst foutdomeinen en upgradedomeinen. Zodat Hiermee stelt u de definitie van de knooppunttypen en virtuele-machineschaalset in bevat de Azure Resource Manager-sjabloon geen informatie over het foutdomein of upgradedomein.
>

## <a name="node-properties-and-placement-constraints"></a>Eigenschappen van het knooppunt en plaatsingsbeperkingen
Soms (in feite de meeste gevallen) moet u ervoor te zorgen dat bepaalde workloads worden uitgevoerd alleen op bepaalde typen knooppunten in het cluster. Bijvoorbeeld: sommige werkbelastingen mogelijk GPU's of SSD's en anderen kunnen al dan niet. 

Een goed voorbeeld van die gericht is op hardware aan bepaalde workloads is bijna elke n-tier-architectuur. Bepaalde computers fungeren als de front-end- of API-voor-zijde van de toepassing en worden blootgesteld aan de clients of op het internet. Verschillende computers, vaak met verschillende hardwareresources, het werk van de lagen rekencapaciteit of opslag worden verwerkt. Dit zijn meestal _niet_ rechtstreeks worden blootgesteld aan clients of op het internet. 

Service Fabric wordt verwacht dat de bepaalde workloads in sommige gevallen moet mogelijk worden uitgevoerd op specifieke hardwareconfiguraties. Bijvoorbeeld:

* Een bestaande toepassing met meerdere lagen is 'opgeheven en verplaatst' in de omgeving van een Service Fabric.
* Een workload moet worden uitgevoerd op specifieke hardware om redenen van prestaties, schaal en beveiliging isolatie.
* Een workload worden geïsoleerd van andere werkbelastingen voor beleid of resource verbruik redenen.

Ter ondersteuning van dit soort configuraties, bevat Service Fabric-tags die u op de knooppunten toepassen kunt. Deze tags worden genoemd *knooppunteigenschappen*. *Plaatsingsbeperkingen* zijn de instructies die zijn gekoppeld aan afzonderlijke services die u voor een of meer eigenschappen van het knooppunt selecteert. Plaatsingsbeperkingen definiëren waar services moeten worden uitgevoerd. De set met beperkingen worden uitgebreid. Een sleutel/waarde-paar kan worden gebruikt. 

<center>

![Verschillende werkbelastingen voor de indeling van een cluster][Image5]
</center>

### <a name="built-in-node-properties"></a>Eigenschappen van het ingebouwde knooppunt
Service Fabric definieert de eigenschappen van een standaard-knooppunt die automatisch kunnen worden gebruikt, zodat u hoeft ze definiëren. De standaard-eigenschappen gedefinieerd op elk knooppunt zijn **NodeType** en **knooppuntnaam**. 

Bijvoorbeeld, kunt u een beperking voor plaatsing als `"(NodeType == NodeType03)"`. **NodeType** is een veelgebruikte eigenschap. Dit is handig omdat deze 1:1 met een type van een virtuele machine overeenkomt. Elk type machine komt overeen met een soort werkbelasting in een traditionele toepassing met meerdere lagen.

<center>

![Plaatsingsbeperkingen en eigenschappen van het knooppunt][Image6]
</center>

## <a name="placement-constraints-and-node-property-syntax"></a>Plaatsingsbeperkingen en de syntaxis van de eigenschap knooppunt 
De waarde die is opgegeven in de eigenschap van het knooppunt kan bestaan uit een tekenreeks, Boolean, of lang ondertekend. De instructie van de service heet een plaatsing *beperking* omdat beperkt waar de service kan worden uitgevoerd in het cluster. De beperking kan een Booleaanse-instructie die van invloed op de eigenschappen van de knooppunten in het cluster zijn. De geldige selectors in deze Booleaanse instructies zijn:

* Voorwaardelijke gecontroleerd voor het maken van bepaalde instructies:

  | Instructie | Syntaxis |
  | --- |:---:|
  | 'gelijk aan' | "==" |
  | 'niet gelijk aan' | "!=" |
  | "groter is dan" | ">" |
  | 'groter dan of gelijk zijn aan' | ">=" |
  | 'kleiner dan' | "<" |
  | "kleiner dan of gelijk is aan" | "<=" |

* Booleaanse instructies voor groepen en logische bewerkingen:

  | Instructie | Syntaxis |
  | --- |:---:|
  | 'en' | "&&" |
  | "or" | "&#124;&#124;" |
  | "niet" | "!" |
  | "als één instructie een groep" | "()" |

Hier volgen enkele voorbeelden van basisbeperking instructies:

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Alleen knooppunten waar de algehele plaatsing beperking-instructie wordt geëvalueerd als 'True' kunnen de service geplaatst erop hebben. Knooppunten die geen een eigenschap worden gedefinieerd hebben komen niet overeen voor de plaatsing beperkingen die de eigenschap bevat.

Stel dat de eigenschappen van de volgende knooppunten zijn gedefinieerd voor een knooppunttype in ClusterManifest.xml:

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

Het volgende voorbeeld ziet knooppunteigenschappen die zijn gedefinieerd via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor clusters wordt gehost op Azure. 

> [!NOTE]
> Het knooppunttype is meestal parameters in de Azure Resource Manager-sjabloon. Deze zijbalken eruit zouden `"[parameters('vmNodeType1Name')]"` in plaats van NodeType01.
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

Plaatsing van Services kunt u *beperkingen* voor een service als volgt te werk:

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// Add other required ServiceDescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Als alle knooppunten van NodeType01 geldig zijn, kunt u ook dat knooppunttype met de beperking selecteren `"(NodeType == NodeType01)"`.

Plaatsingsbeperkingen van een service kunnen dynamisch worden bijgewerkt tijdens runtime. Als u wilt, kunt u een service in het cluster verplaatsen, toevoegen en verwijderen van de vereisten en enzovoort. Service Fabric zorgt ervoor dat de service actief is en beschikbaar blijft, zelfs wanneer deze typen wijzigingen worden aangebracht.

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

```PowerShell
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Plaatsingsbeperkingen zijn opgegeven voor elke benoemde service-exemplaar. Altijd ter vervanging van updates (overschrijven) wat eerder is opgegeven.

De clusterdefinitie van de definieert de eigenschappen van een knooppunt. Wijzigen van de eigenschappen van een knooppunt, moet een upgrade naar de configuratie van het cluster. Een upgrade van de eigenschappen van een knooppunt vereist elk betrokken knooppunt opnieuw op te starten om te rapporteren van de nieuwe eigenschappen. Rolling upgrades worden beheerd met Service Fabric.

## <a name="describing-and-managing-cluster-resources"></a>Met een beschrijving van en het cluster beheren
Een van de belangrijkste taken van elke orchestrator is voor het beheer van het gebruik van resources in het cluster. Clusterresources beheren, kan een aantal verschillende dingen betekenen. 

Eerst, er ervoor zorgen dat machines niet overbelast zijn. Dit betekent dat ervoor te zorgen dat meer services dan ze kunnen worden verwerkt door machines niet worden uitgevoerd. 

Ten tweede is taakverdeling en optimalisatie, die essentieel zijn voor het efficiënt uitvoeren van services. Rendabele of prestatie-intensieve serviceaanbiedingen toestaan geen enkele knooppunten moet ' hot ', terwijl andere koude. Hot knooppunten leiden tot conflicten tussen resources en slechte prestaties. Koude knooppunten vertegenwoordigen verspilde resources en hogere kosten. 

Service Fabric vertegenwoordigt resources als *metrische gegevens*. Metrische gegevens zijn logische of fysieke resource die u wilt om te beschrijven in Service Fabric. Voorbeelden van metrische gegevens zijn 'WorkQueueDepth' of "MemoryInMb." Zie voor meer informatie over de fysieke resources die Service Fabric kunt bepalen op knooppunten [resourcebeheer](service-fabric-resource-governance.md). Zie voor meer informatie over het configureren van aangepaste metrische gegevens en hoe deze worden gebruikt, [in dit artikel](service-fabric-cluster-resource-manager-metrics.md).

Metrische gegevens verschillen van plaatsingsbeperkingen en eigenschappen van het knooppunt. Eigenschappen van het knooppunt zijn statisch descriptors van de knooppunten zelf. Metrische gegevens beschrijven de resources die knooppunten hebben en dat services gebruiken wanneer ze worden uitgevoerd op een knooppunt. Een eigenschap van het knooppunt is mogelijk **HasSSD** en kan worden ingesteld op waar of ONWAAR. De hoeveelheid ruimte die beschikbaar is op deze SSD en hoeveel is verbruikt door services is een metrische waarde zoals "DriveSpaceInMb." 

Net als voor plaatsingsbeperkingen en eigenschappen van het knooppunt, Service Fabric Cluster Resource Manager niet te begrijpen wat de namen van de gemiddelde metrische gegevens. Metrische namen zijn alleen tekenreeksen. Het is raadzaam om te declareren eenheden als onderdeel van de namen van de metrische gegevens die u maakt wanneer ze zijn mogelijk niet eenduidig.

## <a name="capacity"></a>Capaciteit
Als u alle resource uitgeschakeld *balancing*, Service Fabric Cluster Resource Manager wordt nog steeds Zorg ervoor dat er geen knooppunt over de capaciteit gaat. Beheren van overschrijding van de capaciteit is mogelijk, tenzij het cluster vol is of de werkbelasting groter dan een willekeurig knooppunt is. Capaciteit is een andere *beperking* dat Cluster Resource Manager gebruikt om te weten welk deel van een resource een knooppunt heeft. Resterende capaciteit wordt ook gecontroleerd voor het cluster als geheel. 

Zowel de capaciteit en het verbruik op het serviceniveau van de worden uitgedrukt in termen van metrische gegevens. Bijvoorbeeld, de metriek mogelijk 'ClientConnections' en een knooppunt kan een capaciteit voor "ClientConnections" van 32.768 hebben. Andere knooppunten kunnen hebben andere beperkingen. Een service die wordt uitgevoerd op dat knooppunt zegt dat het al op dit moment gebruikmaakt van de 32,256 van de metrische gegevens "ClientConnections."

Cluster Resource Manager houdt tijdens runtime, resterende capaciteit in het cluster en op knooppunten. Cluster Resource Manager wordt voor het volgen van capaciteit, afgetrokken van elke servicegebruik uit van een knooppunt capaciteit waarop de service wordt uitgevoerd. Met deze gegevens worden met Cluster Resource Manager kunt achterhalen waar te plaatsen of te verplaatsen van replica's zodat knooppunten niet via de capaciteit lopen.

<center>

![Clusterknooppunten en -capaciteit][Image7]
</center>

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

```PowerShell
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

U kunt de capaciteiten die zijn gedefinieerd in het clustermanifest zien. Hier volgt een voorbeeld van ClusterManifest.xml:

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

Hier volgt een voorbeeld van de capaciteit voor implementaties met zelfstandige of Template.json via ClusterConfig.json gedefinieerd voor clusters wordt gehost op Azure: 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

Een service vaak wijzigingen dynamisch worden geladen. Stel dat de belasting van een replica van 'ClientConnections' gewijzigd van 1024 in 2048. Het knooppunt dat deze werd uitgevoerd op vervolgens had een capaciteit van enige 512 resterende voor deze metrische gegevens. Nu die replica of de plaatsing van de instantie is ongeldig, omdat er niet genoeg ruimte op dat knooppunt. Cluster Resource Manager heeft om op te halen van het knooppunt terug hieronder capaciteit. Belasting van het knooppunt dat door een of meer van de replica's of exemplaren van dat knooppunt verplaatsen naar andere knooppunten via capaciteit vermindert. 

Cluster Resource Manager probeert te minimaliseren, de kosten van het verplaatsen van replica's. Vindt u meer informatie over [verplaatsingskosten](service-fabric-cluster-resource-manager-movement-cost.md) en [herverdeling strategieën en regels](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Clustercapaciteit
Hoe houdt de Service Fabric Cluster Resource Manager de algehele cluster op basis van te vol? Met dynamische belasting is er niet veel die mee kunt doen. Services kunnen hun piek load onafhankelijk van acties die met Cluster Resource Manager hebben. Als gevolg hiervan is uw cluster met een heleboel vandaag mogelijk underpowered als er een piek morgen. 

Besturingselementen in Cluster Resource Manager te problemen voorkomen. Het eerste wat dat u kunt doen, is te voorkomen dat het maken van nieuwe werkbelastingen, waardoor het cluster vol raakt.

Stel dat u een stateless service maken en er enkele laden die zijn gekoppeld aan deze. De service wanneer we de metriek 'DiskSpaceInMb'. De service wordt vijf eenheden van 'DiskSpaceInMb' voor elk exemplaar van de service gebruiken. U wilt maken van drie exemplaren van de service. Dit betekent dat u nodig hebt 15 eenheden van "DiskSpaceInMb" aanwezig zijn in het cluster waarmee u kunt zelfs maken deze service-exemplaren.

Cluster Resource Manager berekent voortdurend de capaciteit en het verbruik van alle gegevens, zodat deze de resterende capaciteit in het cluster kunt bepalen. Als er onvoldoende ruimte is, verwerpt Cluster Resource Manager de aanroep voor het maken van een service.

Omdat de vereiste alleen is dat er 15 eenheden beschikbaar zijn, kunt u deze ruimte op veel verschillende manieren kunt toewijzen. Bijvoorbeeld, kunnen er één resterende eenheid van de capaciteit van 15 verschillende knooppunten of drie resterende eenheden van de capaciteit op vijf verschillende knooppunten. Als het Cluster Resource Manager kunt dingen rangschikken, zodat er vijf eenheden beschikbaar in drie knooppunten zijn, wordt de service geplaatst. Het cluster opnieuw rangschikken is het meestal mogelijk, tenzij het cluster bijna vol is of de bestaande services kunnen niet worden gecombineerd om een bepaalde reden.

## <a name="buffered-capacity"></a>Gebufferde capaciteit
Gebufferde capaciteit is een andere functie van Cluster Resource Manager. Hierdoor kan de reservering van een gedeelte van de totale capaciteit van het knooppunt. Deze buffer capaciteit wordt alleen gebruikt om services plaats tijdens upgrades en knooppuntfouten. 

Gebufferde capaciteit is wereldwijd opgegeven per metrische gegevens voor alle knooppunten. De waarde die u hebt gekozen voor de gereserveerde capaciteit is een functie van het aantal fout- en upgradedomeinen die u in het cluster hebt. Meer fout- en upgradedomeinen betekenen dat u een lager getal voor de capaciteit van de buffer kiezen kunt. Als u meer domeinen hebt, kunt u verwachten dat kleinere hoeveelheden van het cluster niet beschikbaar is tijdens upgrades en fouten. Op te geven gebufferde capaciteit zinvol alleen als u de capaciteit van het knooppunt voor metrische gegevens ook hebt opgegeven.

Hier volgt een voorbeeld van het gebufferde capaciteit in ClusterManifest.xml opgeven:

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

Hier volgt een voorbeeld van het opgeven van gebufferde capaciteit via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor clusters wordt gehost op Azure:

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

Het maken van nieuwe services mislukt wanneer het cluster buiten gebufferde capaciteit voor een metrische waarde. Zo wordt voorkomen dat het maken van nieuwe services te behouden van de buffer zorgt ervoor dat knooppunten in op de capaciteit niet leidt tot upgrades en fouten. Gebufferde capaciteit is optioneel, maar we raden deze in een cluster dat een capaciteit voor een metrische waarde definieert.

Cluster Resource Manager wordt aangegeven dat deze informatie laden. Voor elke meetwaarde, wordt deze informatie omvat: 
- De instellingen voor gebufferde capaciteit.
- De totale capaciteit.
- Het huidige verbruik.
- Of alle gegevens wordt beschouwd als met gelijke taakverdeling of niet.
- Statistieken over de standaarddeviatie.
- De knooppunten die het meest zijn en minimaal laden.  
  
De volgende code toont een voorbeeld van deze uitvoer:

```PowerShell
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor informatie over de architectuur en de informatiestroom in Cluster Resource Manager, [overzicht van Cluster Resource Manager-architectuur](service-fabric-cluster-resource-manager-architecture.md).
* Defragmentatie metrische gegevens definiëren is één manier om te laden op knooppunten in plaats van deze uitspreiden consolideren. Zie voor informatie over het configureren van de defragmentatie, [defragmentatie van metrische gegevens en belasting in Service Fabric](service-fabric-cluster-resource-manager-defragmentation-metrics.md).
* Vanaf het begin starten en [een inleiding tot Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md).
* Zie voor meer informatie hoe Cluster Resource Manager beheert en verdeelt de taken in het cluster, [taakverdeling van uw Service Fabric-cluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

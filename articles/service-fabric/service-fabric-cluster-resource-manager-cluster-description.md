---
title: Cluster Resource Manager-Clusterbeschrijving | Microsoft Docs
description: Beschrijving van een Service Fabric-cluster door op te geven domeinen met fouten, Upgrade-domeinen, eigenschappen van het knooppunt en knooppuntcapaciteiten voor het Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7be10f03d65e53b51c3916849dc12feb4de9c919
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737651"
---
# <a name="describing-a-service-fabric-cluster"></a>Met een beschrijving van een service fabric-cluster
De Service Fabric Cluster Resource Manager biedt verschillende mechanismen voor het beschrijven van een cluster. Cluster Resource Manager wordt tijdens runtime gebruikgemaakt van deze informatie om hoge beschikbaarheid van de services die in het cluster wordt uitgevoerd. Tijdens het afdwingen van deze belangrijke regels, probeert deze ook aan het Optimaliseer het gebruik van resources binnen het cluster.

## <a name="key-concepts"></a>Belangrijkste concepten
Cluster Resource Manager biedt ondersteuning voor verschillende functies die een cluster beschrijven:

* Domeinen met fouten
* Upgrade Domains
* Eigenschappen van het knooppunt
* Knooppuntcapaciteiten

## <a name="fault-domains"></a>Foutdomeinen
Een Foutdomein is een gebied van gecoördineerde fout. Een enkele machine is een Foutdomein (omdat deze voor een eigen voor diverse redenen zijn waarom uit power supply, mislukte aanmeldingen station, mislukte aanmeldingen ongeldige NIC firmware mislukken kan). Computers die zijn verbonden met dezelfde Ethernet-switch zich in hetzelfde Foutdomein als machines delen van één bron van kracht of op één locatie. Omdat het natuurlijke voor hardwarefouten laten overlappen, worden de domeinen met fouten zijn inherent hiërarchische en worden weergegeven als de URI's in Service Fabric.

Het is belangrijk dat Foutdomeinen correct zijn ingesteld omdat de Service Fabric gebruikt deze informatie om veilig services. Service Fabric wilt niet plaatsen van services, zodat het verlies van een domein met fouten (veroorzaakt door het uitvallen van een onderdeel) zorgt ervoor een service voor het uitvallen dat. In de Azure Service Fabric-omgeving gegevens gebruikt en Foutdomein geleverd door de omgeving correct configureren van de knooppunten in het cluster uit uw naam. Voor Service Fabric zelfstandig zijn Foutdomeinen gedefinieerd op het moment dat het cluster is ingesteld 

> [!WARNING]
> Het is belangrijk dat de geleverde Service Fabric Foutdomein gegevens correct is. Laten we zeggen dat uw Service Fabric-cluster knooppunten binnen 10 virtuele machines die worden uitgevoerd op vijf fysieke hosts worden uitgevoerd. In dit geval, zelfs als er 10 virtuele machines zijn, er zijn alleen 5 verschillende (hoogste niveau) foutdomeinen. Delen dezelfde fysieke host zorgt ervoor dat de virtuele machines voor het delen van de hetzelfde hoofddomein veroorzaakt omdat de virtuele machines gecoördineerde fout optreden als de fysieke host is mislukt.  
>
> Service Fabric wordt verwacht dat het Foutdomein van een knooppunt niet te wijzigen. Andere methoden, zoals hoge beschikbaarheid van de virtuele machines om te garanderen [HA-virtuele machines](https://technet.microsoft.com/library/cc967323.aspx) kunnen er conflicten ontstaan met Service Fabric, omdat ze transparant migratie van virtuele machines van de ene host naar een andere gebruiken. Deze mechanismen niet opnieuw te configureren of op de hoogte stellen de code uitvoeren binnen de virtuele machine. Als zodanig zijn **niet ondersteund** als omgevingen voor het uitvoeren van Service Fabric-clusters. Service Fabric moet de gebruikte alleen hoge beschikbaarheid-technologie. Mechanismen zoals VM livemigratie, SAN's, of andere gebruikers zijn niet nodig. Als u gebruikt in combinatie met Service Fabric, deze mechanismen _verminderen_ beschikbaarheid en betrouwbaarheid omdat ze extra complexiteit tot, gecentraliseerde bronnen van de fout toevoegen en gebruikmaken van de betrouwbaarheid en beschikbaarheidsstrategieën die in strijd zijn met die in Service Fabric. 
>
>

In de onderstaande afbeelding kleur we alle entiteiten die bijdragen aan domeinen met fouten en alle de verschillende domeinen met fouten die het resultaat te vermelden. In dit voorbeeld hebben we datacenters ("DC"), rekken ('R') en blades ("B"). Mogelijk, als elk blade meer dan één virtuele machine bevat, kan er een andere laag in de hiërarchie van het domein met fouten.

<center>
![Knooppunten ingedeeld via domeinen met fouten][Image1]
</center>

Tijdens runtime, de Service Fabric Cluster Resource Manager rekening gehouden met de domeinen met fouten in het cluster is en indelingen. De stateful replica's of stateless exemplaren voor een bepaalde service worden gedistribueerd, zodat ze zich in afzonderlijke Foutdomeinen. De service te verdelen over foutdomeinen, zorgt ervoor dat de beschikbaarheid van de service niet worden gecompromitteerd als een Foutdomein is mislukt op elk niveau van de hiërarchie.

Het aantal lagen er in de hiërarchie Foutdomein zijn zorgt niet dat er van service Fabric Cluster Resource Manager. Echter, wordt geprobeerd om ervoor te zorgen dat het verlies van gegevens van elk één deel van de hiërarchie van dit geen invloed op services die erin worden uitgevoerd. 

Het is raadzaam als er hetzelfde aantal knooppunten op elk niveau van de diepte in de hiërarchie van het domein met fouten. Als de "structuur" van domeinen met fouten dat niet in balans in het cluster is, moeilijker wordt het wordt voor het Cluster Resource Manager om de beste toewijzing van services te achterhalen. Imbalanced Foutdomeinen indelingen betekenen dat het verlies van sommige domeinen impact de beschikbaarheid van services die meer dan andere domeinen. Als gevolg hiervan wordt de Cluster Resource Manager gegevenskanaal tussen twee doelen: Het bedrijf wil gebruiken de machines in dat domein 'dikke' door het plaatsen van services op deze en het bedrijf wil plaatsen van services in andere domeinen, zodat het verlies van gegevens van een domein geen problemen veroorzaken. 

Wat imbalanced domeinen eruit? In het onderstaande diagram laten we zien twee verschillende cluster-indelingen. De knooppunten worden in het eerste voorbeeld, gelijkmatig verdeeld over de domeinen met fouten. In het tweede voorbeeld heeft één Foutdomein veel meer knooppunten dan de andere Foutdomeinen. 

<center>
![Twee verschillende cluster-indelingen][Image2]
</center>

De keuze van dat domein met fouten een knooppunt bevat wordt in Azure, voor u beheerd. Echter, afhankelijk van het aantal knooppunten die u inricht u kunt nog steeds uiteindelijk domeinen met fouten met meer knooppunten in deze dan andere. Stel dat u hebt vijf domeinen met fouten in het cluster, maar zeven knooppunten in te richten voor een bepaalde NodeType. In dit geval wordt uiteindelijk de eerste twee Foutdomeinen meer knooppunten. Als u nog meer NodeTypes met slechts een paar exemplaren implementeren, wordt het probleem slechter opgehaald. Type is om deze reden het aanbeveling dat het aantal knooppunten in elk knooppunt verdient een veelvoud zijn van het aantal Foutdomeinen.

## <a name="upgrade-domains"></a>Upgrade domains
Upgradedomeinen zijn een andere functie waarmee de Service Fabric Cluster Resource Manager te begrijpen van de indeling van het cluster. Upgradedomeinen definiëren sets met knooppunten die zijn bijgewerkt op hetzelfde moment. Upgradedomeinen helpen met Cluster Resource Manager te begrijpen en bewerkingen, zoals upgrades indelen.

Upgradedomeinen zijn veel zoals domeinen met fouten, maar met enkele belangrijke verschillen. Eerst definieert u gebieden van gecoördineerde hardwarefouten domeinen met fouten. Upgradedomeinen, aan de andere kant zijn gedefinieerd door het beleid. U krijgt om te bepalen hoeveel u kunt in plaats van deze wordt bepaald door de omgeving. U kunt zoveel Upgrade-domeinen zoals u dat wel knooppunten doet hebben. Een ander verschil tussen Foutdomeinen en Upgrade-domeinen is dat Upgrade-domeinen niet hiërarchisch zijn. Ze zijn in plaats daarvan meer, zoals een eenvoudige label. 

Het volgende diagram toont de dat drie Upgrade-domeinen worden striped verdeeld over drie Foutdomeinen. U ziet ook een mogelijke plaatsing voor drie verschillende replica's van een stateful service, waar elke in verschillende domeinen en domeinen bijwerken eindigt. Deze plaatsing kunt het verlies van een domein met fouten tijdens het in het midden van een service-upgrade en nog steeds een kopie van de code en gegevens.  

<center>
![Plaatsing met domeinen en Upgradedomeinen][Image3]
</center>

Er zijn voor- en nadelen voor de grote aantallen Upgrade-domeinen. Meer domeinen upgraden betekent dat elke stap van de upgrade is meer gedetailleerde en daarom is van invloed op een kleiner aantal knooppunten of services. Als gevolg hiervan minder services hebben om te gaan op een tijdstip, maak kennis met minder verloop in het systeem. Dit doorgaans de betrouwbaarheid, omdat minder van de service wordt beïnvloed door een probleem tijdens de upgrade. Meer domeinen upgraden betekent ook dat u nodig hebt minder beschikbare buffer op andere knooppunten voor het afhandelen van de impact van de upgrade. Bijvoorbeeld, als u vijf Upgrade-domeinen hebt, omgaan de knooppunten in elk met ongeveer 20% van uw verkeer. Als u nodig hebt om dat domein een upgrade uitvoert voor een upgrade, moet deze belasting daadwerkelijk meestal om te gaan op een locatie. Omdat u vier resterende Upgrade domeinen hebt, moet elke ruimte voor ongeveer 5% van het totale verkeer hebben. Meer domeinen upgraden betekent dat u minder buffer op de knooppunten in het cluster nodig hebt. Denk bijvoorbeeld als er 10 Upgrade-domeinen in plaats daarvan. In dat geval kan elke UD alleen worden afhandelen van ongeveer 10% van het totale verkeer. Wanneer een Upgradestappen via het cluster, zou alleen elk domein moet voldoende ruimte is voor ongeveer 1.1% van het totale verkeer. Meer Upgrade-domeinen in het algemeen kunt u uw knooppunten worden uitgevoerd op een hoger gebruik, omdat u minder gereserveerde capaciteit nodig hebt. Hetzelfde geldt voor domeinen met fouten.  

Het nadeel dat veel Upgrade-domeinen is dat upgrades vaak langer duren. Service Fabric wacht een korte periode na een Upgrade-domein is voltooid en controleert voordat u begint met het volgende object bijwerken. Deze vertragingen inschakelen detectie problemen die door de upgrade voordat de upgrade wordt uitgevoerd. De verhouding is acceptabel omdat deze ongeldige wijzigingen voorkomt dat van invloed zijn op te veel van de service op een tijdstip.

Te weinig Upgrade-domeinen heeft veel negatieve neveneffecten – terwijl elke afzonderlijke upgraden van domein niet actief is en wordt bijgewerkt een groot deel van de totale capaciteit is niet beschikbaar. Bijvoorbeeld, als u slechts drie Upgrade-domeinen hebt onderneemt u omlaag ongeveer 1/3 van uw algehele service of clustercapaciteit tegelijk. Veel mogelijkheden van uw service in één keer dat u niet wenselijk is omdat u over voldoende capaciteit in de rest van het cluster beschikken voor het afhandelen van de werkbelasting. Onderhoud van deze buffer houdt in dat tijdens normale werking die knooppunten kleiner zijn dan ze anders zou zijn geladen. Dit verhoogt de kosten van uw service.

Er is geen echte limiet voor het totale aantal fouten of Upgrade-domeinen in een omgeving of beperkingen met betrekking tot hoe ze elkaar overlappen. Dat gezegd, er zijn verschillende algemene patronen zijn:

- Foutdomeinen en Upgrade-domeinen toegewezen 1:1
- Een Upgrade Domain per knooppunt (fysiek of virtueel instantie van een besturingssysteem)
- Een 'striped' of 'matrix'-model waarbij de Foutdomeinen en Upgrade-domeinen vormen een matrix met de machines die meestal worden uitgevoerd op de diagonaal

<center>
![Domeinen met fouten en Upgradedomein indelingen][Image4]
</center>

Er is dat geen best beantwoorden waarvoor lay-out om te kiezen, elk heeft enkele voordelen en nadelen. Bijvoorbeeld, is het model 1FD:1UD eenvoudig te installeren. De 1 domein upgraden per knooppunt model is, zoals welke personen worden gebruikt voor de meeste. Elk knooppunt is tijdens upgrades onafhankelijk bijgewerkt. Dit is vergelijkbaar met hoe kleine sets machines handmatig zijn bijgewerkt in het verleden. 

Het meest voorkomende model is de matrix FD en UD, waarbij de Foutdomeinen en Upgradedomeinen een tabel vormen en knooppunten vanaf langs de diagonaal worden geplaatst. Dit is het model gebruikt standaard in Service Fabric-clusters in Azure. Alles wat eindigt als de bovenstaande compacte matrix-patroon voor clusters met veel knooppunten.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Beperkingen voor fouttolerantie en upgraden van domein en het resulterende gedrag
### <a name="default-approach"></a>*Standaard-benadering*
Cluster Resource Manager houdt standaard services die zijn verdeeld over Foutdomeinen en Upgrade-domeinen. Dit is gemodelleerd als een [beperking](service-fabric-cluster-resource-manager-management-integration.md). De domeinen met fouten en het upgraden van domein beperking statussen: 'Voor de partitie van een bepaalde service, moet nooit er een verschil meer dan één in het aantal service-objecten (stateless service-exemplaren of stateful service-replica's) tussen twee willekeurige domeinen op hetzelfde niveau van de hiërarchie'. Stel dat deze beperking biedt de zekerheid 'maximale verschil'. De beperking voor domeinen met fouten en het upgraden van domein wordt voorkomen dat bepaalde verplaatst of regelingen die strijdig zijn met de bovenstaande regel. 

We bekijken een voorbeeld. Stel dat we beschikken over een cluster met zes knooppunten zijn geconfigureerd met vijf Foutdomeinen en vijf Upgrade-domeinen.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

*Configuratie 1*

Nu gaan we zeggen dat we een service met een TargetReplicaSetSize (of voor een stateless service een InstanceCount) van vijf maken. Het land van de replica's op N1 N5. In feite N6 nooit wordt gebruikt, ongeacht hoeveel services zoals dit die u maakt. Maar waarom? Bekijk het verschil tussen de huidige indeling en wat er zou gebeuren als N6 is gekozen.

Hier volgt de indeling die wij en het totale aantal replica's per probleem- en het upgraden van domein:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Indeling 1*


Deze lay-out is verdeeld in termen van knooppunten per domein met fouten en het domein een upgrade uitvoert. Het is ook verdeeld in termen van het aantal replica's per probleem- en Upgrade-domein. Elk domein heeft hetzelfde aantal knooppunten en hetzelfde aantal replica's.

Nu gaan we kijken wat gebeurt er als wij N6 in plaats van N2 gebruikt hadden. Hoe wordt de replica's worden gedistribueerd vervolgens?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

*Indeling 2*


Deze lay-out is in strijd met onze definitie van de garantie 'maximale verschil' voor de beperking van het domein met fouten. FD0 heeft twee replica's, terwijl FD1 nul heeft, waardoor het verschil tussen FD0 en FD1 een totaal van twee, is groter dan het maximale verschil van één. Omdat de beperking is geschonden, is met Cluster Resource Manager kunnen niet deze indeling. Op dezelfde manier als we N2 en N6 (in plaats van N1 en N2) verzameld zou krijgen we:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Indeling 3*


Deze lay-out is verdeeld in termen van domeinen met fouten. Echter, nu deze wordt schenden van de beperking voor het upgraden van domein omdat UD0 nul replica's heeft terwijl UD1 twee heeft. Daarom wordt deze lay-out ook is ongeldig en wordt niet door de Cluster Resource Manager worden verzameld.

Deze benadering voor de verdeling van stateful replica's of stateless instanties biedt de best mogelijke fouttolerantie. In een situatie als één domein uitvalt, is het minimale aantal replica's / exemplaren verloren gegaan. 

Deze aanpak kan aan de andere kant worden te strikt en kan het cluster gebruikmaken van alle resources. Voor bepaalde configuraties van clusters op kunnen niet bepaalde knooppunten worden gebruikt. Dit kan leiden tot Service Fabric niet plaatsen van uw services, wat resulteert in waarschuwingsberichten. In het vorige voorbeeld enkele van de clusterknooppunten kan niet worden gebruikt (N6 in het opgegeven voorbeeld). Zelfs als u knooppunten toevoegen aan het cluster (N7 – N10), zouden alleen replica's / exemplaren op N1 – N5 vanwege beperkingen voor fouttolerantie en upgraden van domein worden geplaatst. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |

*Configuratie 2*


### <a name="alternative-approach"></a>*Alternatieve methode*

Cluster Resource Manager biedt ondersteuning voor een andere versie van de domeinen met fouten en het upgraden van domein beperking waardoor plaatsing terwijl nog steeds een minimaal niveau van de veiligheid te garanderen. De alternatieve domeinen met fouten en het upgraden van domein beperking kunt als volgt worden vermeld: 'Voor de partitie van een bepaalde service, distributie van de replica in meerdere domeinen moet ervoor zorgen dat de partitie niet ten koste gaan een quorumverlies'. Stel dat deze beperking biedt de zekerheid 'quorum veilige'. 

> [!NOTE]
>Voor een stateful service, definiëren we *quorumverlies* in een situatie wanneer een meerderheid van de partitiereplica's zijn niet actief op hetzelfde moment. Bijvoorbeeld, als TargetReplicaSetSize vijf is, vertegenwoordigt een set met alle drie replica's quorum. Op dezelfde manier als TargetReplicaSetSize 6, zijn vier replica's nodig voor het quorum. In beide gevallen niet meer dan twee replica's niet beschikbaar mag zijn op hetzelfde moment als de partitie wil gaan normaal functioneren. Voor een stateless service is niet goed als *quorumverlies* zoals stateless services normaal functioneren blijft, zelfs als een meerderheid van de instanties omlaag gaan op hetzelfde moment. Daarom kan de nadruk op stateful services in de rest van de tekst.
>

We gaan terug naar het vorige voorbeeld. Met de "quorum veilige"-versie van de beperking zijn alle drie opgegeven indelingen geldig. Dit komt doordat, zelfs als er uitval van FD0 in de tweede lay-out- of UD1 in de derde lay-out, de partitie nog steeds quorum (of zou nog steeds een meerderheid van de replica's van worden) hebben. Met deze versie van de beperking kan N6 bijna altijd worden gebruikt.

De 'quorum veilige'-aanpak biedt meer flexibiliteit dan met de methode 'maximale verschil' omdat deze eenvoudiger terug te vinden van de replica-distributies die geldig in de clustertopologie van vrijwel elk zijn. Echter deze benadering kan niet garanderen de fout met de beste kenmerken van de tolerantie omdat sommige fouten slechter dan andere zijn. In het ergste geval kunnen een meerderheid van de replica's verloren gaan met het uitvallen van één domein en één extra replica. Bijvoorbeeld, in plaats van 3 fouten moeten quorum met 5 replica's of instanties verliest, kunt u nu verliezen een meerderheid met slechts twee fouten. 

### <a name="adaptive-approach"></a>*Adaptieve benadering*
Omdat beide van de methoden sterke en zwakke punten hebben, hebben we een adaptieve benadering die deze twee strategieën combineert geïntroduceerd.

> [!NOTE]
>Dit is het standaardgedrag beginnen met Service Fabric versie 6.2. 
>
De aanpak van adaptieve maakt standaard gebruik van de logica 'maximale verschil' en schakelt over naar de logica 'quorum veilige' alleen indien nodig. Cluster Resource Manager wordt automatisch zoekt uit welke strategie nodig is door te kijken hoe het cluster en de services zijn geconfigureerd. Voor een bepaalde service: *Als de TargetReplicaSetSize deelbaar is door het aantal domeinen met fouten en het aantal domeinen upgraden **en** het aantal knooppunten is kleiner dan of gelijk zijn aan de (aantal Foutdomeinen) * (het aantal domeinen upgraden), het Cluster Resource Manager moeten gebruikmaken van de logica 'quorum op basis van' voor die service.* Houd er rekening mee dat met Cluster Resource Manager gebruikt deze benadering voor staatloze en stateful services, ondanks quorumverlies niet meer relevant is voor stateless services.

We gaan terug naar het vorige voorbeeld en wordt ervan uitgegaan dat een cluster heeft nu 8 knooppunten (Er is nog steeds het cluster met vijf Foutdomeinen en vijf domeinen upgraden en de TargetReplicaSetSize van een service die wordt gehost op dat cluster blijft vijf geconfigureerd). 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

*Configuratie 3*

Omdat alle noodzakelijke voorwaarden wordt voldaan, wordt de logica 'quorum op basis van' in het distribueren van de service gebruikmaken van Cluster Resource Manager. Hierdoor is het gebruik van N6 – N8. Een service die mogelijk distributie in dit geval kan er als volgt uitzien:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

*Indeling 4*

Als van uw service TargetReplicaSetSize is beperkt tot vier (bijvoorbeeld), wordt met Cluster Resource Manager u ziet deze wijziging en hervatten met behulp van de logica 'maximale verschil' omdat TargetReplicaSetSize niet meer dividable door het aantal Foutdomeinen en Upgradedomeinen. Als gevolg hiervan wordt bepaalde verplaatsingen van het type replica uitgevoerd om te kunnen distribueren vier replica's op knooppunten N1 N5 resterende zodat de 'maximale verschil'-versie van de domeinlogica Foutdomein en Upgrade niet wordt geschonden. 

Zie de vierde indeling en de TargetReplicaSetSize van vijf. Als N1 wordt verwijderd uit het cluster, wordt het aantal domeinen upgraden gelijk aan vier. Cluster Resource Manager wordt opnieuw gestart met behulp van "maximale verschil" logica van de service TargetReplicaSetSize niet gelijkmatig meer delen door het aantal ud's. Als gevolg hiervan moet replica R1, tijdens het opnieuw, land op N4 zodat niet Foutdomeinen en Upgrade Domain beperking is geschonden.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |N/A |N/A |N/A |N/A |N/A |N/A |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Lay-out 5*

## <a name="configuring-fault-and-upgrade-domains"></a>Fout- en Upgrade-domeinen configureren
Automatisch in voor het definiëren van Foutdomeinen en Upgrade-domeinen wordt gedaan door Azure gehoste Service Fabric-implementaties. Service Fabric opgehaald en worden de omgevingsgegevens van Azure gebruikt.

Als u uw eigen cluster maakt (of wilt uitvoeren van een bepaalde topologie in ontwikkeling), kunt u de informatie over domein met fouten en het upgraden van domein zelf opgeven. In dit voorbeeld definiëren we een cluster met negen knooppunten lokale ontwikkeling die drie "datacenters' (elk met drie rekken omvat). Dit cluster heeft ook drie Upgrade-domeinen striped verdeeld over deze drie datacenters. Een voorbeeld van de configuratie is lager dan: 

ClusterManifest.xml

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
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

via ClusterConfig.json voor zelfstandige implementaties

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
> Bij het definiëren van clusters via Azure Resource Manager zijn-Foutdomeinen en Upgrade-domeinen toegewezen door Azure. De definitie van uw knooppunttypen en Virtual Machine Scale Sets in de Azure Resource Manager-sjabloon bevat daarom geen Foutdomein of het upgraden van domein-informatie.
>

## <a name="node-properties-and-placement-constraints"></a>Eigenschappen van het knooppunt en plaatsingsbeperkingen
Soms (in feite de meeste gevallen) gaat u ervoor wilt zorgen dat bepaalde workloads worden uitgevoerd alleen op bepaalde typen knooppunten in het cluster. Bijvoorbeeld enkele workload mogelijk GPU's of SSD's en andere niet. Een goed voorbeeld van die gericht is op hardware aan bepaalde workloads is er bijna elke n-tier-architectuur. Bepaalde computers fungeren als de front-end of de API voor de kant van de toepassing en worden blootgesteld aan de clients of op het internet. Verschillende computers, vaak met verschillende hardwareresources, het werk van de lagen rekencapaciteit of opslag worden verwerkt. Dit zijn meestal _niet_ rechtstreeks worden blootgesteld aan clients of op het internet. Service Fabric wordt verwacht dat er zijn gevallen waarin bepaalde werkbelastingen moeten worden uitgevoerd op specifieke hardwareconfiguraties. Bijvoorbeeld:

* een bestaande toepassing met meerdere lagen is 'opgeheven en verplaatst' in de omgeving van een Service Fabric
* een workload wil worden uitgevoerd op specifieke hardware voor prestaties, schaalbaarheid of isolatie uit veiligheidsoverwegingen
* Een workload worden geïsoleerd van andere werkbelastingen voor beleid of resource verbruik redenen

Ter ondersteuning van dit soort configuraties, heeft de Service Fabric een eersteklas begrip van de labels die kunnen worden toegepast op de knooppunten. Deze tags worden genoemd **knooppunteigenschappen**. **Plaatsingsbeperkingen** zijn de instructies die zijn gekoppeld aan afzonderlijke services ervan die voor een of meer eigenschappen van het knooppunt selecteren. Plaatsingsbeperkingen definiëren waar services moeten worden uitgevoerd. De set met beperkingen worden uitgebreid - een sleutel/waarde-paar kan worden gebruikt. 

<center>
![Verschillende werkbelastingen lay-out van cluster][Image5]
</center>

### <a name="built-in-node-properties"></a>Ingebouwd in de eigenschappen van het knooppunt
Service Fabric definieert de eigenschappen van een standaard-knooppunt die automatisch kunnen worden gebruikt zonder dat de gebruiker om deze te bepalen. De standaard-eigenschappen gedefinieerd op elk knooppunt zijn de **NodeType** en de **knooppuntnaam**. U kunt dus bijvoorbeeld een beperking voor plaatsing als schrijven `"(NodeType == NodeType03)"`. We hebben in het algemeen gevonden NodeType moet een van de meest vaak gebruikte eigenschappen. Dit is handig omdat deze 1:1 met een type van een virtuele machine overeenkomt. Elk type machine komt overeen met een soort werkbelasting in een traditionele toepassing met meerdere lagen.

<center>
![Plaatsingsbeperkingen en eigenschappen van het knooppunt][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Plaatsing beperking en de syntaxis van de eigenschap knooppunt 
De waarde die is opgegeven in de eigenschap van het knooppunt kan een string, bool, of lang ondertekend. De instructie van de service heet een plaatsing *beperking* omdat beperkt waar de service kan worden uitgevoerd in het cluster. De beperking kan een Booleaanse-instructie die van invloed op de eigenschappen van het andere knooppunt in het cluster zijn. De geldige selectors in deze Booleaanse instructies zijn:

1) voorwaardelijke controles voor het maken van bepaalde instructies

| Verklaring | Syntaxis |
| --- |:---:|
| 'gelijk aan' | "==" |
| 'niet gelijk aan' | "!=" |
| "groter is dan" | ">" |
| 'groter dan of gelijk zijn aan' | ">=" |
| 'kleiner dan' | "<" |
| "kleiner dan of gelijk is aan" | "<=" |

2) Booleaanse instructies voor groepen en logische bewerkingen

| Verklaring | Syntaxis |
| --- |:---:|
| 'en' | "&&" |
| "or" | "&#124;&#124;" |
| "niet" | "!" |
| "als één instructie een groep" | "()" |

Hier volgen enkele voorbeelden van basisbeperking instructies.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Alleen knooppunten waar de algehele plaatsing beperking-instructie wordt geëvalueerd als 'True' kunnen de service geplaatst erop hebben. Knooppunten die nog geen een eigenschap worden gedefinieerd komen niet overeen voor de plaatsing beperkingen met die eigenschap.

Stel dat de eigenschappen van de volgende knooppunten zijn gedefinieerd voor een bepaald knooppunt-type:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

gehost clusters via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure. 

> [!NOTE]
> Het knooppunttype is meestal parameters in de Azure Resource Manager-sjabloon. Het zou er als volgt uitzien '[parameters('vmNodeType1Name')]' in plaats van 'NodeType01'.
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

Plaatsing van Services kunt u *beperkingen* voor een service, zoals als volgt te werk:

C#

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

Als alle knooppunten van NodeType01 geldig zijn, kunt u ook dat knooppunttype met de beperking selecteren "(NodeType == NodeType01) '.

Een van de dingen over plaatsingsbeperkingen van een service is dat ze kunnen dynamisch worden bijgewerkt tijdens runtime. Dus als u wilt, kunt u een service in het cluster verplaatsen, toevoegen en verwijderen van de vereisten, enzovoort. Service Fabric zorgt ervoor te zorgen dat de service actief is en beschikbaar blijft, zelfs wanneer deze typen wijzigingen worden aangebracht.

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

Plaatsingsbeperkingen zijn opgegeven voor elk ander benoemd service-exemplaar. Altijd ter vervanging van updates (overschrijven) wat eerder is opgegeven.

De clusterdefinitie van de definieert de eigenschappen van een knooppunt. Wijzigen van de eigenschappen van een knooppunt is een upgrade van een cluster configuratie vereist. Een upgrade van de eigenschappen van een knooppunt vereist elk betrokken knooppunt opnieuw op te starten om te rapporteren van de nieuwe eigenschappen. Deze rolling upgrades worden beheerd door Service Fabric.

## <a name="describing-and-managing-cluster-resources"></a>Met een beschrijving van en het Cluster beheren
Een van de belangrijkste taken van elke orchestrator is voor het beheer van het gebruik van resources in het cluster. Clusterresources beheren, kan een aantal verschillende dingen betekenen. Eerst, er ervoor zorgen dat machines niet overbelast zijn. Dit betekent dat ervoor te zorgen dat meer services dan ze kunnen worden verwerkt door machines niet worden uitgevoerd. Er is ten tweede, taakverdeling en optimalisatie van wat van essentieel belang voor het efficiënt uitvoeren van services. Kosten effectieve of prestaties-serviceaanbiedingen gevoelige toestaan geen enkele knooppunten moet ' hot ', terwijl andere koude. Hot knooppunten leiden tot conflicten tussen resources en slechte prestaties en koude knooppunten vertegenwoordigen verspilde resources en hogere kosten. 

Service Fabric vertegenwoordigt resources als `Metrics`. Metrische gegevens zijn logische of fysieke resource die u wilt om te beschrijven in Service Fabric. Voorbeelden van metrische gegevens zijn dingen als 'WorkQueueDepth' of 'MemoryInMb'. Zie voor meer informatie over de fysieke resources die Service Fabric kunt bepalen op knooppunten [resourcebeheer](service-fabric-resource-governance.md). Zie voor meer informatie over het configureren van aangepaste metrische gegevens en hoe deze worden gebruikt, [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)

Metrische gegevens zijn anders dan plaatsingen beperkingen en eigenschappen van het knooppunt. Eigenschappen van het knooppunt zijn statisch descriptors van de knooppunten zelf. Metrische gegevens beschrijven de resources die knooppunten hebben en dat services gebruiken wanneer ze worden uitgevoerd op een knooppunt. Een eigenschap van het knooppunt kan "HasSSD" en kan worden ingesteld op waar of ONWAAR. De hoeveelheid ruimte die beschikbaar is op deze SSD en hoeveel is verbruikt door services zou een metrische waarde zoals 'DriveSpaceInMb' zijn. 

Het is belangrijk te weten dat net als voor plaatsingsbeperkingen en eigenschappen van het knooppunt, Service Fabric Cluster Resource Manager niet bekend bent met betekenis van de namen van de metrische gegevens. Metrische namen zijn alleen tekenreeksen. Het is raadzaam om te declareren eenheden als onderdeel van de namen van de metrische gegevens die u maakt wanneer het niet-eenduidige kan zijn.

## <a name="capacity"></a>Capaciteit
Als u alle resource uitgeschakeld *balancing*, van Service Fabric Cluster Resource Manager kan geen knooppunt voor over de capaciteit uiteindelijk nog steeds dat. Beheren van overschrijding van de capaciteit is mogelijk, tenzij het cluster vol is of de werkbelasting groter dan een willekeurig knooppunt is. Capaciteit is een andere *beperking* dat Cluster Resource Manager gebruikt om te weten welk deel van een resource een knooppunt heeft. Resterende capaciteit wordt ook gecontroleerd voor het cluster als geheel. Zowel de capaciteit en het verbruik op het serviceniveau van de worden uitgedrukt in termen van metrische gegevens. Bijvoorbeeld de metrische gegevens mogelijk 'ClientConnections' en een bepaald knooppunt kan een capaciteit voor "ClientConnections" van 32768 hebben. Andere knooppunten kunnen andere limieten deel van de service die wordt uitgevoerd op knooppunt kunnen zeggen dat het is momenteel verbruikt 32256 van de metrische gegevens 'ClientConnections' hebben.

Cluster Resource Manager houdt tijdens runtime, resterende capaciteit in het cluster en op knooppunten. Als u wilt bijhouden capaciteit trekt met Cluster Resource Manager elk servicegebruik van knooppuntcapaciteit, waar de service wordt uitgevoerd. Met deze informatie worden Service Fabric Cluster Resource Manager kunt achterhalen waar te plaatsen of te verplaatsen van replica's zodat knooppunten niet via de capaciteit lopen.

<center>
![Clusterknooppunten en -capaciteit][Image7]
</center>

C#:

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

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

U kunt de capaciteiten die zijn gedefinieerd in het clustermanifest zien:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

gehost clusters via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure. 

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

Veel van de service geladen wijzigingen dynamisch. Stel dat de belasting van een replica van 'ClientConnections' gewijzigd van 1024 op 2048, maar het knooppunt dat deze werd uitgevoerd op vervolgens alleen resterende voor deze metrische gegevens van 512 capaciteit hoeft. Nu die replica of de plaatsing van de instantie is ongeldig, omdat er niet genoeg ruimte op dat knooppunt. Cluster Resource Manager heeft te worden gehouden, trappen het knooppunt terug hieronder capaciteit. Belasting van het knooppunt dat door een of meer van de replica's of exemplaren van dat knooppunt verplaatsen naar andere knooppunten via capaciteit vermindert. Bij het verplaatsen van replica's, wordt met Cluster Resource Manager probeert te minimaliseren, de kosten van deze stromen. Kosten voor gegevensverplaatsing wordt besproken in [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md) en informatie over de Cluster Resource Manager het herverdelen van strategieën en regels beschreven [hier](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Clustercapaciteit
Hoe houdt de Service Fabric Cluster Resource Manager de algehele cluster op basis van te vol? Goed, met de dynamische belasting er veel niet is kunt mee doen. Services kunnen hun piek load onafhankelijk van acties die worden uitgevoerd door de Cluster Resource Manager hebben. Uw cluster met een heleboel vandaag is mogelijk als gevolg hiervan underpowered zodra u beroemde morgen. Die al zei, er zijn sommige besturingselementen die zijn geïntegreerd om problemen te voorkomen. Het eerste wat dat we kunnen doen is het maken van nieuwe werkbelastingen, waardoor het cluster volledig worden voorkomen.

Stel dat u een stateless service maken en er enkele laden die zijn gekoppeld aan deze. Stel dat de service wanneer we over de metriek 'DiskSpaceInMb'. Stel nu dat het gaat vijf eenheden van 'DiskSpaceInMb' voor elk exemplaar van de service gebruiken. U wilt maken van drie exemplaren van de service. Goed gedaan. Dat betekent dat we 15 eenheden van "DiskSpaceInMb" aanwezig zijn in het cluster in zodat we ook moeten worden dus kunnen maken van deze service-exemplaren. Cluster Resource Manager berekent voortdurend de capaciteit en het verbruik van alle gegevens, zodat deze de resterende capaciteit in het cluster kunt bepalen. Als er onvoldoende ruimte is, verwerpt met Cluster Resource Manager de aanroep van de service maken.

Omdat de vereiste alleen dat er 15 eenheden beschikbaar zijn, kan deze ruimte veel verschillende manieren worden toegewezen. Bijvoorbeeld, kan er één resterende eenheid van de capaciteit van 15 verschillende knooppunten of drie resterende eenheden van de capaciteit op vijf verschillende knooppunten. Als de Cluster Resource Manager dingen rangschikken kunt, zodat er vijf eenheden beschikbaar is op drie knooppunten, wordt de service geplaatst. Het cluster opnieuw rangschikken is het meestal mogelijk, tenzij het cluster bijna vol is of de bestaande services kunnen niet worden gecombineerd om een bepaalde reden.

## <a name="buffered-capacity"></a>Gebufferde capaciteit
Gebufferde capaciteit is een andere functie van de Cluster Resource Manager. Hierdoor kan de reservering van een gedeelte van de totale capaciteit van het knooppunt. Deze buffer capaciteit wordt alleen gebruikt om services tijdens upgrades en knooppuntfouten. Gebufferde capaciteit is wereldwijd opgegeven per metrische gegevens voor alle knooppunten. De waarde die u voor de gereserveerde capaciteit kiest is een functie van het aantal fout- en Upgrade-domeinen in het cluster hebt. Meer domeinen en domeinen upgraden betekent dat u een lager getal kunt kiezen voor de capaciteit van de buffer. Als u meer domeinen hebt, kunt u verwachten dat kleinere hoeveelheden van het cluster niet beschikbaar is tijdens upgrades en fouten. Capaciteit buffer wordt opgeslagen op te geven alleen is het handig als u de capaciteit van het knooppunt voor metrische gegevens ook hebt opgegeven.

Hier volgt een voorbeeld van het opgeven van gebufferde capaciteit:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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

Het maken van nieuwe services mislukt wanneer het cluster buiten gebufferde capaciteit voor een metrische waarde. Zo wordt voorkomen dat het maken van nieuwe services te behouden van de buffer zorgt ervoor dat knooppunten in op de capaciteit niet leidt tot upgrades en fouten. Gebufferde capaciteit is optioneel, maar wordt aanbevolen in elke cluster die u een capaciteit voor een metrische waarde definieert.

Cluster Resource Manager wordt aangegeven dat deze informatie laden. Voor elke meetwaarde, wordt deze informatie omvat: 
  - de gebufferde capaciteitsinstellingen
  - de totale capaciteit
  - het huidige verbruik
  - of alle gegevens wordt beschouwd als met gelijke taakverdeling of niet
  - statistieken over de standaarddeviatie
  - de knooppunten waarvoor de meeste en minimale belasting  
  
Hier zien we een voorbeeld van deze uitvoer:

```posh
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
* Bekijk voor meer informatie over de architectuur en de informatiestroom binnen het Cluster Resource Manager [in dit artikel ](service-fabric-cluster-resource-manager-architecture.md)
* Defragmentatie metrische gegevens definiëren is één manier om te laden op knooppunten in plaats van deze uitspreiden consolideren. Raadpleeg voor informatie over het configureren van de defragmentatie, [in dit artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Vanaf het begin starten en [een inleiding tot de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
* Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

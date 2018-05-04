---
title: Beschrijving van de cluster Resource Manager-Cluster | Microsoft Docs
description: Met een beschrijving van een Service Fabric-cluster door te geven voor domeinen met fouten, domeinen upgraden, eigenschappen van het knooppunt en capaciteit van knooppunt voor het Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 07ddf1c2b76230c8d753426d70098603ff14ec4d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="describing-a-service-fabric-cluster"></a>Met een beschrijving van een service fabric-cluster
De Service Fabric Cluster Resource Manager biedt verschillende mechanismen voor het beschrijven van een cluster. De Cluster Resource Manager gebruikt tijdens runtime, deze informatie om te controleren of de hoge beschikbaarheid van de services die in het cluster wordt uitgevoerd. Bij het afdwingen van deze belangrijke regels ook probeert te optimaliseren het brongebruik binnen het cluster.

## <a name="key-concepts"></a>Belangrijkste concepten
De Cluster Resource Manager ondersteunt verschillende functies die worden beschreven van een cluster:

* Domeinen met fouten
* Upgradedomeinen
* Knooppunteigenschappen
* Capaciteit van knooppunt

## <a name="fault-domains"></a>Foutdomeinen
Een domein met fouten is een gebied van gecoördineerde is mislukt. Een enkele computer is een Foutdomein (omdat deze niet kan op een eigen voor verschillende oorzaken hebben, van levering stroomstoringen op fouten van schijf naar een ongeldige NIC firmware). Computers die zijn verbonden met dezelfde Ethernet-switch zijn in hetzelfde Foutdomein, zoals machines delen één bron stroomstoring of op één locatie. Aangezien het natuurlijke voor hardwarefouten overlappen, worden Foutdomeinen inherent hiërarchische zijn en worden weergegeven als URI's in Service Fabric.

Het is belangrijk dat Foutdomeinen correct zijn ingesteld sinds de Service Fabric gebruikt deze gegevens veilig services plaatsen. Service Fabric wilt niet services plaatsen zodat het verlies van een domein met fouten (veroorzaakt door het uitvallen van een onderdeel) zorgt ervoor een service dat omlaag gaan. In de Azure Service Fabric-omgeving de Foutdomein informatie van de omgeving gebruikt correct configureren van de knooppunten in het cluster namens jou. Voor Service Fabric zelfstandig worden domeinen met fouten gedefinieerd op het moment dat het cluster is ingesteld 

> [!WARNING]
> Het is belangrijk dat de geleverde Service Fabric Foutdomein gegevens nauwkeurig is. Stel dat dat uw Service Fabric-cluster knooppunten binnen 10 virtuele machines, op vijf fysieke hosts worden uitgevoerd. In dit geval wordt er 10 virtuele machines, maar er zijn alleen 5 verschillende (hoogste niveau) fault-domeinen. Delen dezelfde fysieke host zorgt ervoor dat de virtuele machines te delen de dezelfde hoofddomein veroorzaakt omdat de virtuele machines gecoördineerde fout optreden als de fysieke host mislukt.  
>
> Service Fabric verwacht het Foutdomein van een knooppunt niet te wijzigen. Andere mechanismen zoals hoge beschikbaarheid van de virtuele machines gezorgd [HA virtuele machines](https://technet.microsoft.com/library/cc967323.aspx) kunnen er conflicten ontstaan met Service Fabric, omdat ze transparant migratie van virtuele machines van de ene host naar een andere gebruiken. Deze mechanismen niet opnieuw te configureren en kennis van de code in de virtuele machine is uitgevoerd. Als zodanig zijn **niet ondersteund** als omgevingen voor het uitvoeren van Service Fabric-clusters. Service Fabric moet de gebruikte beveiligingstechnologieën alleen hoge beschikbaarheid. Mechanismen zoals VM livemigratie, SAN's, of andere gebruikers zijn niet nodig. Als u gebruikt in combinatie met Service Fabric, deze mechanismen _verminderen_ toepassing beschikbaarheid en betrouwbaarheid omdat ze extra complexiteit veroorzaken gecentraliseerde bronnen van de fout toevoegen en gebruiken van betrouwbaarheid en beschikbaarheidsstrategieën die met die in Service Fabric conflicteren. 
>
>

We kleur die de entiteiten die bijdragen aan domeinen met fouten en lijst van alle de verschillende domeinen met fouten die resulteren in de onderstaande afbeelding. In dit voorbeeld hebben we datacenters ('DC'), rekken ('R') en blades ("B"). Stelen, als elke blade meer dan één virtuele machine bevat, kan er een andere laag in de hiërarchie van het domein met fouten.

<center>
![Knooppunten ingedeeld via domeinen met fouten][Image1]
</center>

Tijdens runtime, de Service Fabric Cluster Resource Manager beschouwt de domeinen met fouten in het cluster en indelingen plannen. De stateful replica's of stateless exemplaren voor een bepaalde service worden gedistribueerd, zodat ze in afzonderlijke domeinen met fouten. De service over domeinen met fouten worden verdeeld, zorgt ervoor dat de beschikbaarheid van de service niet is geknoeid als een Foutdomein op elk niveau van de hiërarchie is mislukt.

Van service Fabric-Cluster Resource Manager niet van belang hoeveel lagen die zich in de hiërarchie van het domein met fouten. Echter, wordt geprobeerd om ervoor te zorgen dat het verlies van elk een deel van de hiërarchie heeft geen invloed op services die erin worden uitgevoerd. 

Het is raadzaam als er hetzelfde aantal knooppunten op elk niveau van diepte in de hiërarchie van het domein met fouten. Als de 'boomstructuur' foutdomeinen onbalans in het cluster is, maakt het moeilijker voor het Cluster Resource Manager om de beste toewijzing van services te achterhalen. Imbalanced Foutdomeinen indelingen betekenen dat het verlies van de impact van bepaalde domeinen de beschikbaarheid van services die meer dan andere domeinen. Als gevolg hiervan de Cluster Resource Manager is verwijderd tussen twee doelen: wil de computers in het domein 'dikke' door het plaatsen van services op deze gebruiken en deze wil services in andere domeinen te plaatsen zodat het verlies van een domein geen problemen veroorzaken. 

Wat imbalanced domeinen eruit? In het onderstaande diagram weergeven we twee verschillende cluster indelingen. In het eerste voorbeeld worden de knooppunten gelijkmatig verdeeld over de domeinen met fouten. In het tweede voorbeeld heeft één Foutdomein veel meer knooppunten dan de andere domeinen met fouten. 

<center>
![Twee verschillende cluster indelingen][Image2]
</center>

In Azure, wordt voor u de keuze van dat domein met fouten een knooppunt bevat beheerd. Echter, afhankelijk van het aantal knooppunten die u inricht u kunt nog steeds uiteindelijk domeinen met fouten met meer knooppunten bevat dan andere. Stel dat u hebt vijf domeinen met fouten in het cluster, maar zeven knooppunten inrichten voor een gegeven NodeType. In dit geval wordt eindigen de eerste twee domeinen met fouten met meer knooppunten. Als u nog meer NodeTypes met slechts enkele exemplaren implementeert, wordt het probleem slechter opgehaald. Type is daarom die het is raadzaam dat het aantal knooppunten in elk knooppunt een veelvoud van het aantal Foutdomeinen.

## <a name="upgrade-domains"></a>Upgradedomeinen
Upgradedomeinen zijn een andere functie waarmee de Service Fabric Cluster Resource Manager inzicht in de indeling van het cluster. Upgradedomeinen definiëren sets met knooppunten die zijn bijgewerkt op hetzelfde moment. Upgradedomeinen helpen bij het Cluster Resource Manager begrijpen en te organiseren beheerbewerkingen zoals upgrades.

Upgradedomeinen zijn veel zoals domeinen met fouten, maar met enkele belangrijke verschillen. Eerst definiëren gebieden van gecoördineerde hardwarefouten domeinen met fouten. Upgradedomeinen, aan de andere kant zijn gedefinieerd door het beleid. Krijgt u te bepalen hoeveel u, in plaats van dit wordt bepaald door de omgeving. U kunt zoveel Upgrade-domeinen als knooppunten hebben. Een ander verschil tussen domeinen met fouten en -domeinen upgraden is dat er geen Upgrade domeinen hiërarchische. In plaats daarvan lijken ze meer op een eenvoudige label. 

Het volgende diagram toont de dat drie domeinen upgraden worden striped verdeeld over drie domeinen met fouten. U ziet ook een mogelijke plaatsing voor drie verschillende replica's van een stateful service, waar elke in verschillende domeinen met fouten en domeinen upgraden eindigt. Deze plaatsing kunt het verlies van een domein met fouten tijdens het in het midden van een service-upgrade en nog steeds een kopie van de code en gegevens.  

<center>
![Plaatsing met fouten en Upgradedomeinen][Image3]
</center>

Er zijn voor- en nadelen tot het instellen van grote aantallen domeinen upgraden. Meer domeinen upgraden betekent elke stap van de upgrade is een meer gedetailleerd en daarom is van invloed op een kleiner aantal knooppunten of services. Als gevolg hiervan minder services hebben om te gaan op een tijdstip minder verloop Inleiding in het systeem. Dit heeft vaak de betrouwbaarheid, omdat minder van de service wordt beïnvloed door een probleem tijdens de upgrade is geïntroduceerd. Meer domeinen upgraden betekent ook dat u minder beschikbare buffer op andere knooppunten moet voor het afhandelen van de impact van de upgrade. Bijvoorbeeld, als u vijf upgraden domeinen hebt, afhandelt de knooppunten in elk ongeveer 20% van uw verkeer. Als u buiten werking stellen dat domein bijwerken voor een upgrade moet, moet waarbij deze belasting meestal gaat u een locatie. Aangezien u vier resterende upgraden domeinen hebt, moet elk voldoende ruimte voor ongeveer 5% van al het verkeer. Meer domeinen upgraden betekent dat u minder buffer op de knooppunten in het cluster nodig. Denk bijvoorbeeld als er 10 domeinen upgraden in plaats daarvan. In dat geval zouden elke UD alleen worden verwerkt ongeveer 10% van het totale verkeer. Wanneer een Upgradestappen via het cluster, zou alleen elk domein moet voldoende ruimte voor ongeveer 1.1% van het totale verkeer. Meer upgraden domeinen in het algemeen kunt u uw knooppunten uitgevoerd op een hoger gebruik, omdat u minder gereserveerde capaciteit nodig. Hetzelfde geldt voor domeinen met fouten.  

Het nadeel dat veel domeinen upgraden is dat upgrades vaak langer duren. Service Fabric wacht een korte periode na een Upgrade-domein is voltooid en controleert voordat u begint met de upgrade voor de volgende. Deze vertragingen inschakelen detecteren problemen die zijn geïntroduceerd door de upgrade voordat de upgrade wordt uitgevoerd. Afweging is acceptabel omdat deze onjuiste wijzigingen voorkomt dat van invloed zijn op te veel van de service op een tijdstip.

Te weinig upgraden domeinen heeft veel negatieve neveneffecten – terwijl elke afzonderlijke upgraden domein niet actief is en een groot deel van een upgrade wordt uitgevoerd de algehele capaciteit is niet beschikbaar. Bijvoorbeeld, als u slechts drie domeinen upgraden hebt duurt u omlaag ongeveer 1/3 van uw algehele service of het cluster capaciteit tegelijk. Met zoveel van uw service niet actief in één keer niet gewenst omdat u moet voldoende capaciteit hebben in de rest van het cluster voor het afhandelen van de werkbelasting. Onderhoud van deze buffer houdt in dat tijdens de normale werking die knooppunten kleiner dan ze anders zou zijn geladen. Dit verhoogt de kosten van het uitvoeren van uw service.

Er is geen echte limiet voor het totale aantal fouten of domeinen upgraden in een omgeving, of de beperkingen van hoe ze elkaar overlappen. Die gezegd, er zijn enkele algemene patronen zijn:

- Domeinen met fouten en -domeinen upgraden toegewezen 1:1
- Een Upgrade Domain per knooppunt (fysieke of virtuele OS exemplaar)
- Een 'striped' of 'matrix' model waarbij de domeinen met fouten en -domeinen upgraden vormen een matrix met machines meestal onder de diagonaal

<center>
![Probleem- en Upgradedomein indelingen][Image4]
</center>

Er is dat geen best beantwoordt welke indeling u moet kiezen, elk heeft een aantal voordelen en nadelen. Het model 1FD:1UD is bijvoorbeeld heel eenvoudig te installeren. De 1 domein upgraden per knooppunt model zoals welke mensen worden gebruikt voor de meeste is. Elk knooppunt is tijdens upgrades onafhankelijk bijgewerkt. Dit is vergelijkbaar met hoe kleine sets machines handmatig zijn bijgewerkt in het verleden. 

Het meest voorkomende model is de matrix FD/UD, waarbij de FDs en UDs een tabel vormen en knooppunten worden geplaatst langs de diagonaal wordt gestart. Dit is de standaard gebruikt in Service Fabric-clusters in Azure. Voor clusters met knooppunten die veel belandt alles wat u ziet eruit als het bovenstaande compacte matrix-patroon.

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a>Beperkingen voor fouttolerantie en het upgraden van domein en het resulterende gedrag
### <a name="default-approach"></a>*Standaard-benadering*
Standaard blijft het Cluster Resource Manager services verdeeld zijn over de fout en -domeinen upgraden. Dit is gemodelleerd als een [beperking](service-fabric-cluster-resource-manager-management-integration.md). De fout en het upgraden van domein beperking-status: 'voor de partitie van een bepaalde service, moet nooit er een verschil hoger zijn dan het aantal serviceobjecten (stateless service-exemplaren of stateful service replica's) tussen twee willekeurige domeinen op hetzelfde niveau van de hiërarchie'. Stel dat u dat deze beperking biedt een garantie 'maximale verschil'. De beperking-fout en het upgraden van domein voorkomt u dat bepaalde verplaatst of regelingen die strijdig zijn met de hierboven vermelde regel. 

Bekijk een voorbeeld. Stel dat we een cluster met zes knooppunten zijn geconfigureerd met vijf domeinen met fouten en vijf domeinen upgraden hebben.

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | | |N3 | | |
| **UD3** | | | |N4 | |
| **UD4** | | | | |N5 |

*Configuratie 1*

Nu gaan we spreken maken we een service met een TargetReplicaSetSize (of voor een stateless service een InstanceCount) van vijf. De replica's neerzetten op N1 N5. In feite is nooit N6 gebruikt ongeacht hoeveel services zoals deze die u maakt. Maar waarom? Bekijk het verschil tussen de huidige indeling en wat er gebeurt als N6 is gekozen.

Dit is de indeling die wij en het totale aantal replica's per fouttolerantie en het upgraden van domein:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** | |R2 | | | |1 |
| **UD2** | | |R3 | | |1 |
| **UD3** | | | |R4 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Indeling 1*


Deze indeling wordt verdeeld in termen van knooppunten per domein met fouten en het upgraden van domein. Het is ook taakverdeling in termen van het aantal replica's per probleem- en Upgrade-domein. Elk domein heeft hetzelfde aantal knooppunten en hetzelfde aantal replica's.

Nu gaan we kijken wat gebeurt er als er N6 in plaats van N2 gebruikt waren. Hoe wordt de replica's worden gedistribueerd vervolgens?

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R5 | | | | |1 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |2 |0 |1 |1 |1 |- |

*Indeling 2*


Deze lay-out is in strijd met onze definitie van de garantie 'maximale verschil' voor de beperking van het domein met fouten. FD0 heeft twee replica's, terwijl FD1 nul heeft, waardoor het verschil tussen FD0 en FD1 een totaal van twee, is groter dan het maximale verschil van één. Omdat de beperking wordt geschonden, staat deze regeling niet in de Cluster Resource Manager toe. Op dezelfde manier als we N2 en N6 (in plaats van N1 en N2 gekozen) dat we zouden krijgen:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** | | | | | |0 |
| **UD1** |R5 |R1 | | | |2 |
| **UD2** | | |R2 | | |1 |
| **UD3** | | | |R3 | |1 |
| **UD4** | | | | |R4 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Indeling 3*


Deze indeling wordt verdeeld in termen van domeinen met fouten. Echter, nu deze wordt overtreden van de beperking voor het upgraden van domein omdat UD0 nul replica's heeft terwijl UD1 twee heeft. Daarom wordt deze lay-out ook is ongeldig en won't worden verzameld door de Cluster Resource Manager.

Deze aanpak voor de distributie van stateful replica's of stateless exemplaren biedt de best mogelijke fouttolerantie. In een situatie wanneer één domein uitvalt, gaat het minimale aantal replica's / exemplaren verloren. 

Aan de andere kant deze benadering te strikt worden en het cluster gebruikmaken van alle bronnen niet toestaan. Voor bepaalde configuraties van clusters op kunnen niet bepaalde knooppunten worden gebruikt. Dit kan leiden tot Service Fabric niet plaatsen van uw services, wat resulteert in waarschuwingsberichten. In het vorige voorbeeld enkele van de clusterknooppunten kan niet worden gebruikt (N6 in het opgegeven voorbeeld). Zelfs als u knooppunten toevoegen aan dit cluster (N7 – N10), zou replica's / exemplaren alleen op N1 – N5 vanwege beperkingen voor fouttolerantie en het upgraden van domein worden geplaatst. 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | |N10 |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | |N9 |N5 |

*Configuratie 2*


### <a name="alternative-approach"></a>*Alternatieve methode*

De Cluster Resource Manager biedt ondersteuning voor een andere versie van de fouten en het upgraden van domein beperking waarmee plaatsing terwijl u nog steeds een minimaal niveau van de veiligheid te garanderen. De alternatieve probleem- en Upgrade Domain-beperking kunt als volgt worden opgegeven: 'Voor de partitie van een bepaalde service, replica-verdeling tussen domeinen moet ervoor zorgen dat de partitie niet te een quorumverlies maken'. Stel dat u dat deze beperking biedt een 'quorum veilige' garantie. 

> [!NOTE]
>Voor een stateful service definiëren we *quorumverlies* in een situatie wanneer een meerderheid van de partitie replica's zijn niet beschikbaar op hetzelfde moment. Als TargetReplicaSetSize 5, vertegenwoordigt een set van alle drie replica's quorum. Op dezelfde manier als TargetReplicaSetSize 6, zijn vier replica's nodig voor quorum. In beide gevallen kunnen niet meer dan twee replica's niet beschikbaar op hetzelfde moment als de partitie wil gaan normaal functioneert. Voor een stateless service is niet goed als *quorumverlies* als conitnue naar functionate normaal zelfs als een meerderheid van exemplaren gaat u naar beneden op hetzelfde moment stateless services. Daarom gaan we in op stateful services in de rest van de tekst.
>

Daarvoor gaat u terug naar het vorige voorbeeld. Alle drie opgegeven indelingen kunnen worden gebruikt met de 'quorum veilige'-versie van de beperking. Dit is omdat zelfs als er fout van FD0 in de tweede indeling of UD1 in de indeling van de derde zijn zouden, de partitie nog steeds quorum (een meerderheid van de replica's moeten nog steeds up) hebben. Met deze versie van de beperking kan N6 bijna altijd worden gebruikt.

De methode 'quorum veilige' biedt meer flexibiliteit dan de methode 'maximale verschil' omdat dit eenvoudiger te vinden van de replica-distributies die geldig in de clustertopologie van vrijwel elk zijn. Echter deze aanpak biedt geen garantie de beste fout tolerantie kenmerken omdat een aantal fouten slechter dan andere. In het ergste geval, kan een meerderheid van de replica's met het uitvallen van één domein en één extra replica verloren gaan. Bijvoorbeeld, in plaats van 3 fouten moeten quorum met 5 replica's of exemplaren verliest, kunt u nu verliezen meerderheid met slechts twee fouten. 

### <a name="adaptive-approach"></a>*Adaptieve benadering*
Omdat beide van de methoden sterke en zwakke punten hebben, hebben we een adaptieve benadering die deze twee strategieën combineert geïntroduceerd.

> [!NOTE]
>Dit is het standaardgedrag beginnen met Service Fabric-versie 6.2. 
>
De adaptieve aanpak maakt standaard gebruik van de logica 'maximale verschil' en verandert in de logica 'quorum veilige' alleen indien nodig. De Cluster Resource Manager automatisch weten welke strategie hoeft door te kijken hoe het cluster en de services worden geconfigureerd te achterhalen. Voor een bepaalde service: *als de TargetReplicaSetSize deelbaar is door het aantal Foutdomeinen en het aantal domeinen upgraden **en** het aantal knooppunten is kleiner dan of gelijk zijn aan de (aantal Foutdomeinen) * (de aantal domeinen upgraden), de 'quorum op basis van'-logica voor de service moet gebruikmaken van de Cluster Resource Manager.* Vergeet dat de Cluster Resource Manager gebruikt deze benadering voor zowel stateless als stateful services, ondanks quorumverlies wordt niet relevant voor stateless services.

Daarvoor gaat u terug naar het vorige voorbeeld en wordt ervan uitgegaan dat een cluster heeft nu 8 knooppunten (Er is nog steeds het cluster met vijf domeinen met fouten en vijf domeinen upgraden en de TargetReplicaSetSize van een service die wordt gehost op dat cluster blijft vijf geconfigureerd). 

|  | FD0 | FD1 | FD2 | FD3 | FD4 |
| --- |:---:|:---:|:---:|:---:|:---:|
| **UD0** |N1 | | | | |
| **UD1** |N6 |N2 | | | |
| **UD2** | |N7 |N3 | | |
| **UD3** | | |N8 |N4 | |
| **UD4** | | | | |N5 |

*Configuratie 3*

Omdat alle benodigde voorwaarden is voldaan, wordt de 'quorumconfiguratie op basis van logica in het distribueren van de service gebruikmaken van Cluster Resource Manager. Hiermee wordt informatie over het gebruik van N6 – N8. Een mogelijke Serviceverdeling in dit geval kan er als volgt uitzien:

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |R1 | | | | |1 |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | | | |0 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |2 |1 |1 |0 |1 |- |

*Indeling 4*

Als uw service TargetReplicaSetSize wordt beperkt tot vier (bijvoorbeeld), wordt de Cluster Resource Manager ziet die wijziging en hervatten met behulp van de logica 'maximale verschil' omdat de TargetReplicaSetSize dividable door het aantal FDs en UDs niet meer. Als gevolg hiervan wordt bepaalde verplaatsingen replica uitgevoerd om te distribueren vier replica's op knooppunten N1 N5 resterende zodat de versie 'maximale verschil' van de domein-logica Foutdomein en Upgrade niet is geschonden. 

Zie de vierde indeling en de TargetReplicaSetSize van vijf. Als N1 wordt verwijderd uit het cluster, wordt het aantal domeinen upgraden gelijk aan vier. De Cluster Resource Manager opnieuw wordt gestart met behulp van 'maximale verschil' logica van de service TargetReplicaSetSize niet gelijkmatig meer delen door het aantal UDs. Als gevolg hiervan moet replica R1, wanneer opnieuw opgebouwd land op N4 zodat probleem- en Upgrade Domain-beperking niet is geschonden.

|  | FD0 | FD1 | FD2 | FD3 | FD4 | UDTotal |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| **UD0** |N/A |N/A |N/A |N/A |N/A |N/A |
| **UD1** |R2 | | | | |1 |
| **UD2** | |R3 |R4 | | |2 |
| **UD3** | | | |R1 | |1 |
| **UD4** | | | | |R5 |1 |
| **FDTotal** |1 |1 |1 |1 |1 |- |

*Lay-out 5*

## <a name="configuring-fault-and-upgrade-domains"></a>Configureren van de fout en domeinen upgraden
Domeinen met fouten en -domeinen upgraden definiëren gebeurt automatisch in Azure gehoste Service Fabric-implementaties. Service Fabric opgehaald en worden de gegevens van de omgeving van Azure gebruikt.

Als u uw eigen-cluster maakt (of wilt uitvoeren van een bepaalde topologie in ontwikkeling), kunt u de informatie over domein met fouten en het upgraden van domein zelf opgeven. In dit voorbeeld definiëren we een lokaal ontwikkelcluster van negen knooppunt die drie 'datacenters' (elk met drie rekken omvat). Dit cluster heeft ook drie upgraden domeinen striped verdeeld over de drie datacenters. Een voorbeeld van de configuratie is lager dan: 

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
> Bij het definiëren van clusters via Azure Resource Manager, worden Foutdomeinen en -domeinen upgraden door Azure worden toegewezen. De definitie van uw knooppunttypen en virtuele-Machineschaalsets in uw Azure Resource Manager-sjabloon bevat daarom geen Foutdomein of domeingegevens upgraden.
>

## <a name="node-properties-and-placement-constraints"></a>Eigenschappen van het knooppunt en plaatsingsbeperkingen
Soms (in feite de meeste gevallen) gaat u ervoor wilt zorgen dat bepaalde werkbelastingen alleen op bepaalde soorten knooppunten in het cluster worden uitgevoerd. Voor kan sommige werkbelasting bijvoorbeeld vereisen GPU's of SSD's en andere niet. Een goed voorbeeld van doelen van hardware aan bepaalde werkbelastingen is er bijna alle lagen-architectuur. Bepaalde computers fungeren als de front-end- of -API voor de kant van de toepassing en worden blootgesteld aan de clients of via internet. Andere machines, vaak met verschillende hardware-bronnen, verwerkt de werkzaamheden van de lagen compute of opslag. Dit zijn meestal _niet_ rechtstreeks blootgesteld aan clients of via internet. Service Fabric verwacht dat er gevallen waarin bepaalde werkbelastingen moeten worden uitgevoerd op specifieke hardwareconfiguraties. Bijvoorbeeld:

* een bestaande n-tier-toepassing is 'opgeheven en verplaatst' in een Service Fabric-omgeving
* een werklast wil uitvoeren op specifieke hardware voor prestaties of schaal veiligheidsredenen isolatie
* Een werklast worden geïsoleerd van andere werkbelastingen omwille van beleid of de resource-verbruik

Service Fabric heeft ter ondersteuning van dit soort configuraties, een eerste-klas begrip van de labels die kunnen worden toegepast op de knooppunten. Deze tags worden genoemd **knooppunteigenschappen**. **Plaatsingsbeperkingen** worden de instructies die zijn gekoppeld aan afzonderlijke services die voor een of meer eigenschappen van het knooppunt selecteren. Plaatsingsbeperkingen definiëren waar services moeten worden uitgevoerd. De set van beperkingen is extensible - een sleutel-waardepaar kunt werken. 

<center>
![Cluster-indeling verschillende werkbelastingen][Image5]
</center>

### <a name="built-in-node-properties"></a>Ingebouwd in de eigenschappen van knooppunt
Service Fabric definieert een aantal standaardeigenschappen knooppunt die automatisch kunnen worden gebruikt zonder dat de gebruiker om deze te bepalen. De standaard-eigenschappen gedefinieerd op elk knooppunt zijn de **NodeType** en de **NodeName**. U kunt dus bijvoorbeeld een beperking voor plaatsing als schrijven `"(NodeType == NodeType03)"`. We hebben doorgaans gevonden NodeType moet een van de meest vaak gebruikte eigenschappen. Het is nuttig omdat deze 1:1 met een type van een machine overeenkomt. Elk type van de machine overeenkomt met een type werkbelasting in een traditionele n-tier-toepassing.

<center>
![Plaatsingsbeperkingen en eigenschappen van knooppunt][Image6]
</center>

## <a name="placement-constraint-and-node-property-syntax"></a>Plaatsing van de beperkingen en de syntaxis van de eigenschap knooppunt 
De opgegeven waarde in de eigenschap van het knooppunt kan string, bool, of lang ondertekend. De instructie op de service een plaatsing wordt aangeroepen *beperking* omdat beperkt waar de service kan worden uitgevoerd in het cluster. De beperking kan een Boolean-instructie die is van invloed op de eigenschappen van het andere knooppunt in het cluster zijn. De geldige selectoren in deze instructies Boolean-waarde zijn:

1) voorwaardelijke controles voor het maken van bepaalde instructies

| Verklaring | Syntaxis |
| --- |:---:|
| 'gelijk zijn aan' | "==" |
| 'is niet gelijk aan' | "!=" |
| 'die groter is dan' | ">" |
| 'groter dan of gelijk zijn aan' | ">=" |
| 'kleiner dan' | "<" |
| 'kleiner dan of gelijk aan' | "<=" |

2) Boole-instructies voor groepering en logische bewerkingen

| Verklaring | Syntaxis |
| --- |:---:|
| 'en' | "&&" |
| "of" | "&#124;&#124;" |
| "niet" | "!" |
| 'als één instructie een groep' | "()" |

Hier volgen enkele voorbeelden van basisbeperking instructies.

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

Alleen knooppunten waar de algehele plaatsing beperking-instructie wordt geëvalueerd als 'True' kunnen de service op deze geplaatst hebben. Knooppunten waarvoor geen een eigenschap worden gedefinieerd, komen niet overeen met de plaatsing beperkingen met die eigenschap.

Stel dat de volgende eigenschappen van het knooppunt zijn gedefinieerd voor een bepaald knooppunt-type:

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

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure. 

> [!NOTE]
> Het knooppunttype in uw Azure Resource Manager-sjabloon meestal parameters wordt gebruikt. Deze eruit als '[parameters('vmNodeType1Name')]' in plaats van 'NodeType01'.
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

U kunt de plaatsing van de service maken *beperkingen* voor een service, zoals als volgt:

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

Een van de cool dingen over een service plaatsingsbeperkingen is dat ze kunnen dynamisch worden bijgewerkt tijdens runtime. Dus als u wilt, kunt u een service in het cluster verplaatsen, toevoegen en verwijderen vereisten, enzovoort. Service Fabric zorgt om ervoor te zorgen dat de service up-to-date en beschikbaar blijven zelfs wanneer deze typen wijzigingen worden aangebracht.

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

Plaatsingsbeperkingen zijn opgegeven voor elke andere benoemde service-exemplaar. Altijd ter vervanging van updates (overschrijven) wat eerder is opgegeven.

De definitie van de cluster definieert de eigenschappen op een knooppunt. Kan de eigenschappen van een knooppunt is de upgrade van een cluster configuratie vereist. Eigenschappen van een knooppunt upgraden vereist elk betrokken knooppunt opnieuw op te starten om de eigenschappen van nieuwe melden. Deze rolling upgrades worden beheerd door Service Fabric.

## <a name="describing-and-managing-cluster-resources"></a>Beschrijven en de clusterbronnen beheren
Een van de belangrijkste taken van een orchestrator is voor het beheer van brongebruik in het cluster. Het beheren van clusterbronnen kan leiden tot een aantal verschillende dingen. Eerst, er ervoor zorgen dat machines zijn niet overbelast. Dit betekent om ervoor te zorgen dat machines meer services die deze kunnen verwerken worden niet uitgevoerd. Er is ten tweede taakverdeling en optimalisatie wat van essentieel belang voor het uitvoeren van services efficiënt. Sommige knooppunten moet hot, terwijl andere koude mag geen kosten effectieve of prestaties gevoelige-serviceaanbod. Hot knooppunten leiden tot slechte prestaties en bronconflicten en koude knooppunten vertegenwoordigen verspilde resources en hogere kosten. 

Service Fabric vertegenwoordigt resources als `Metrics`. Metrische gegevens zijn logische of fysieke resource die u wilt beschrijven naar Service Fabric. Voorbeelden van metrische gegevens zijn bijvoorbeeld 'WorkQueueDepth' of 'MemoryInMb'. Zie voor meer informatie over de fysieke resources die Service Fabric kunnen van toepassing op knooppunten [resource governance](service-fabric-resource-governance.md). Zie voor meer informatie over het configureren van aangepaste metrische gegevens en het gebruik [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)

Metrische gegevens zijn anders dan plaatsingen beperkingen en eigenschappen van het knooppunt. Eigenschappen van het knooppunt zijn statisch descriptors van de knooppunten zelf. Metrische gegevens worden de resources die knooppunten hebben en dat services gebruiken wanneer ze worden uitgevoerd op een knooppunt beschreven. De eigenschap van een knooppunt kan 'HasSSD' en kan worden ingesteld op waar of ONWAAR. De hoeveelheid beschikbare ruimte op die SSD en hoeveel is verbruikt door services zou een metriek zoals 'DriveSpaceInMb' zijn. 

Het is belangrijk te weten dat net als voor plaatsingsbeperkingen en eigenschappen van het knooppunt de resourcebeheer voor Service Fabric-Cluster niet begrijpt wat de namen van de metrische gegevens aan de hand. De namen van de metrische gegevens zijn alleen tekenreeksen. Het is raadzaam eenheden als onderdeel van de metrische namen die u maakt wanneer deze is mogelijk niet-eenduidige declareren.

## <a name="capacity"></a>Capaciteit
Als u alle resource uitgeschakeld *balancing*, Service-Fabric-Cluster Resource Manager zou nog ervoor te zorgen dat geen knooppunt uiteindelijk via de capaciteit. Capaciteit overschrijdingen beheren is mogelijk, tenzij het cluster vol is of de werkbelasting groter dan een willekeurig knooppunt is. Capaciteit is een andere *beperking* dat de Cluster Resource Manager gebruikt om te weten welk deel van een bron op een knooppunt heeft. Resterende capaciteit wordt ook bijgehouden voor het cluster als geheel. Zowel de capaciteit en het verbruik op het serviceniveau worden uitgedrukt in termen van metrische gegevens. Dus bijvoorbeeld, de metriek mogelijk 'ClientConnections' en een bepaald knooppunt heeft misschien een capaciteit voor 'ClientConnections' van 32768. Andere knooppunten kunnen andere limieten deel van de service uitgevoerd op knooppunt kunnen zeggen het momenteel verbruikt 32256 van de metrische gegevens 'ClientConnections' hebben.

Tijdens runtime houdt de Cluster Resource Manager resterende capaciteit in het cluster en op knooppunten. De Cluster Resource Manager aftrekken te houden van de capaciteit van elke service informatie over het gebruik van de capaciteit van het knooppunt waar de service wordt uitgevoerd. Met deze gegevens worden de Service Fabric Cluster Resource Manager kunt achterhalen waar te plaatsen of verplaatsen van replica's zodat de knooppunten niet Bespreek de capaciteit.

<center>
![Clusterknooppunten en capaciteit][Image7]
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

Hier ziet u capaciteit die zijn gedefinieerd in het clustermanifest:

ClusterManifest.xml

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure. 

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

Meestal laden een service wijzigingen dynamisch. Stel dat load van 'ClientConnections' een replica gewijzigd van 1024 in 2048, maar het knooppunt dat deze werd uitgevoerd op vervolgens alleen 512 resterende voor deze metriek capaciteit heeft. Nu die replica of de plaatsing van het exemplaar is ongeldig, omdat er niet genoeg ruimte op het knooppunt. De Cluster Resource Manager heeft te starten het knooppunt terug hieronder capaciteit ophalen. Belasting op het knooppunt dat door een of meer van de replica's of exemplaren van dat knooppunt verplaatst naar andere knooppunten via capaciteit vermindert. Bij het verplaatsen van replica's, wordt de Cluster Resource Manager probeert te minimaliseren, de kosten van deze verplaatsingen van het type. Verplaatsingkosten wordt besproken in [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md) en de informatie over het Cluster Resource Manager strategieën herverdeling en regels beschreven [hier](service-fabric-cluster-resource-manager-metrics.md).

## <a name="cluster-capacity"></a>Cluster-capaciteit
Hoe de Service Fabric Cluster Resource Manager het hele cluster vol wordt behouden? Ook met de dynamische belasting bij er veel niet is kunt het doen. Services kunnen hun piek load onafhankelijk van de acties die worden uitgevoerd door de Cluster Resource Manager hebben. Uw cluster met genoeg ruimte vandaag is mogelijk als gevolg hiervan underpowered als u morgen opnieuw stelen. Die reactie van: Er zijn een aantal besturingselementen die standaard zijn uitbreidbaar in om problemen te voorkomen. Het eerste wat dat wij kunnen nu is het maken van nieuwe werkbelastingen die het cluster vol raakt, zou voorkomen.

Stel dat u een stateless service maakt en of hieraan sommige load gekoppeld. Stel dat de service de metriek 'DiskSpaceInMb' hecht. Stel nu dat deze wil verbruiken vijf eenheden van 'DiskSpaceInMb' voor elk exemplaar van de service. U wilt maken van drie exemplaren van de service. Goed gedaan. Dat betekent dat 15 eenheden van 'DiskSpaceInMb' moet aanwezig zijn in het cluster zodat we zelfs moet worden dus kunnen maken van deze service-exemplaren. De Cluster Resource Manager berekent voortdurend de capaciteit en het verbruik van elke metriek zodat deze de resterende capaciteit in het cluster kan bepalen. Als er niet voldoende ruimte, verwerpt de Cluster Resource Manager de aanroep van de service maken.

Omdat de vereiste maar wel dat er 15 eenheden beschikbaar zijn, kan deze ruimte veel verschillende manieren worden toegewezen. Bijvoorbeeld, kan er één resterende eenheid van de capaciteit op 15 verschillende knooppunten of drie resterende eenheden van de capaciteit van vijf verschillende knooppunten. Als de Cluster Resource Manager dingen rangschikken kunt zodat er vijf eenheden beschikbaar is op drie knooppunten, wordt de service. Het cluster opnieuw rangschikken is meestal mogelijk, tenzij het cluster bijna vol is of de bestaande services kunnen niet worden gecombineerd om een bepaalde reden.

## <a name="buffered-capacity"></a>Buffer capaciteit
De capaciteit van de buffer is een andere functie van de Cluster Resource Manager. Hiermee kunt de reservering van een gedeelte van de totale knooppunt. Deze buffer capaciteit wordt alleen gebruikt om services tijdens upgrades en knooppuntfouten. De capaciteit van de buffer is globaal opgegeven per metrische gegevens voor alle knooppunten. De waarde die u voor de gereserveerde capaciteit kiest is een functie van het aantal fout- en Upgrade domeinen hebt u in het cluster. Meer fouttolerantie en -domeinen upgraden betekent dat u een lager getal kunt kiezen voor de capaciteit van de buffer. Als u meer domeinen hebt, kunt u verwachten kleinere hoeveelheden van het cluster niet beschikbaar tijdens upgrades en fouten. Capaciteit gebufferd geven alleen zinvol als u ook de capaciteit van het knooppunt voor een waarde hebt opgegeven.

Hier volgt een voorbeeld van het buffer capaciteit opgeven:

ClusterManifest.xml

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

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

Het maken van nieuwe services mislukt wanneer het cluster niet gebufferd capaciteit voor een metriek. Zo wordt voorkomen dat het maken van nieuwe services te behouden van de buffer zorgt ervoor dat upgrades en fouten knooppunten om te gaan via capaciteit geen veroorzaken. De capaciteit van de buffer is optioneel, maar wordt aanbevolen in een cluster dat een capaciteit voor een metriek definieert.

De Cluster Resource Manager, wordt deze informatie laden. Voor elke metriek omvat deze informatie: 
  - de buffer capaciteitsinstellingen
  - de totale capaciteit
  - het huidige verbruik
  - Hiermee wordt aangegeven of elke metriek wordt beschouwd als met gelijke taakverdeling of niet
  - statistieken over de standaarddeviatie
  - de knooppunten die de meeste hebben en minimaal laden  
  
Hieronder ziet u een voorbeeld van deze uitvoer:

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
* Bekijk voor meer informatie over de architectuur en de informatiestroom binnen het Cluster Resource Manager, [in dit artikel ](service-fabric-cluster-resource-manager-architecture.md)
* Defragmentatie metrische gegevens definiëren, is een manier om te consolideren belasting van de knooppunten in plaats van af te spreiden. Raadpleeg voor meer informatie over het configureren van defragmentatie, [in dit artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* Vanaf het begin starten en [Maak kennis met de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
* Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png

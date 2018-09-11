---
title: Planning van de capaciteit van de Service Fabric-cluster | Microsoft Docs
description: Service Fabric-cluster overwegingen voor capaciteitsplanning. Lagen Nodetypes, bewerkingen, duurzaamheid en betrouwbaarheid
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: chackdan
ms.openlocfilehash: d1d17ff331d3e770b77ce729904e57cf88ebc16c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348565"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-cluster overwegingen voor capaciteitsplanning
Voor een productie-implementatie is plannen van capaciteit een belangrijke stap. Hier volgen enkele van de artikelen waarmee u rekening moet houden als onderdeel van dit proces.

* Het nummer van uw cluster beginnen moet met knooppunttypen
* De eigenschappen van elk knooppunttype (grootte, primair, internetgerichte, aantal VM's, enz.)
* De betrouwbaarheid en duurzaamheid van de clusterkenmerken

> [!NOTE]
> U moet minimaal bekijken alle **niet toegestaan** beleidswaarden bij het plannen van een upgrade uitvoert. Dit is om ervoor te zorgen dat de waarden op de juiste wijze is ingesteld en om branden omlaag van het cluster later vanwege toewijzingstypen systeemconfiguratie-instellingen. 
> 

Laat het ons kort Lees elk van deze items.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Het nummer van uw cluster beginnen moet met knooppunttypen
Eerst moet u achterhalen wat het cluster die u maakt moet worden gebruikt voor er gebeurt.  Welke soorten toepassingen die u van plan bent om te implementeren in dit cluster? Als u niet duidelijk op het doel van het cluster zijn, u waarschijnlijk nog niet klaar om in te voeren van het proces voor de capaciteitsplanning.

Het nummer van uw cluster beginnen moet met knooppunttypen vast.  Elk knooppunttype is toegewezen aan een virtuele-machineschaalset. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. Dus komt de beslissing van het aantal knooppunttypen in feite omlaag naar de volgende overwegingen:

* Uw toepassing beschikt over meerdere services en moet een van deze openbare of internetservices zijn? Typische toepassingen bevatten een gateway in de front-end-service die de invoer van een client ontvangt en een of meer back-end-services die communiceren met de front-end-services. Dus in dit geval krijgt u uiteindelijk met ten minste twee typen.
* Uw services (die gezamenlijk uw toepassing) beschikt over de infrastructuur voor verschillende behoeften, zoals meer RAM-geheugen of hogere CPU-cycli? Bijvoorbeeld, laat het ons wordt ervan uitgegaan dat de toepassing die u wilt implementeren een front-end-service en een back-endservice bevat. De front-end-service kunt uitvoeren op kleinere virtuele machines (VM-grootten, zoals D2) die poorten zijn geopend met het internet.  De back-endservice echter is berekening intensieve en moet worden uitgevoerd op grotere virtuele machines (met VM-grootten, zoals D4, D6, D15) die niet internet gericht.
  
  In dit voorbeeld, maar u kunt alle services op één knooppunttype, plaatst het wordt aangeraden dat u deze in een cluster met twee typen plaatsen.  Hierdoor kan elk knooppunttype om afzonderlijke eigenschappen zoals verbinding met internet of VM-grootte. Het aantal virtuele machines kan worden geschaald, onafhankelijk van elkaar, maar ook.  
* Omdat u de toekomst kan niet voorspellen, gaat u met gegevens die u kent en het aantal knooppunttypen dat uw toepassingen beginnen met moeten kiezen. U kunt altijd toevoegen of verwijderen van knooppunttypen later. Een Service Fabric-cluster moet ten minste één knooppunttype hebben.

## <a name="the-properties-of-each-node-type"></a>De eigenschappen van elk knooppunttype
De **knooppunttype** kunnen worden beschouwd als equivalent aan rollen in Cloud Services. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Elk knooppunttype die is gedefinieerd in een Service Fabric-cluster wordt toegewezen aan een [virtuele-machineschaalset](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Elk knooppunttype is een afzonderlijke scale ingesteld en kan worden opgeschaald of omlaag onafhankelijk van elkaar, hebben verschillende sets open poorten en verschillende capaciteitsstatistieken hebben. Voor meer informatie over de relaties tussen knooppunttypen en VM scale sets, hoe voor RDP in een van de exemplaren, het openen van nieuwe poorten en enzovoort, Zie [knooppunttypen van Service Fabric-cluster](service-fabric-cluster-nodetypes.md).

Een Service Fabric-cluster kan bestaan uit meer dan één knooppunttype. In dat geval kan het cluster bestaat uit één primaire knooppunttype en knooppunttypen van een of meer niet-primaire.

Een type één knooppunt kan niet op betrouwbare wijze te schalen dan 100 knooppunten per virtuele-machineschaalset voor SF-toepassingen. bereiken op betrouwbare wijze groter is dan 100 knooppunten, moet u extra virtuele-machineschaalsets toevoegen.

### <a name="primary-node-type"></a>Primaire knooppunttype

De Service Fabric-systeemservices (bijvoorbeeld het Cluster Manager-service of een afbeelding Store-service) worden op het primaire knooppunttype geplaatst. 

![Schermopname van een cluster met twee knooppunttypen][SystemServices]

* De **minimale grootte van virtuele machines** voor het primaire knooppunt type wordt bepaald door de **duurzaamheidslaag** u kiest. De standaardlaag van de duurzaamheid is Brons. Zie [de duurzaamheid kenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) voor meer informatie.  
* De **minimumaantal VM's** voor het primaire knooppunt type wordt bepaald door de **betrouwbaarheidslaag** u kiest. De standaardlaag van de betrouwbaarheid is lichtgrijs. Zie [de betrouwbaarheidskenmerken van de van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) voor meer informatie.  

Vanuit de Azure Resource Manager-sjabloon, het primaire knooppunttype is geconfigureerd met de `isPrimary` kenmerk onder de [knooppunt typedefinitie](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Niet-primaire knooppunttype

Er is een primaire knooppunttype in een cluster met meerdere knooppunten, en de overige zijn niet-primaire.

* De **minimale grootte van virtuele machines** voor niet-primaire knooppunt typen wordt bepaald door de **duurzaamheidslaag** u kiest. De standaardlaag van de duurzaamheid is Brons. Zie [de duurzaamheid kenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) voor meer informatie.  
* De **minimumaantal VM's** voor niet-primaire knooppunttypen is een. Echter, moet u dit nummer op basis van het aantal replica's van de toepassing /-services die u wilt uitvoeren in dit knooppunttype kiezen. Het aantal virtuele machines in een knooppunttype kan worden verhoogd nadat u het cluster hebt geïmplementeerd.

## <a name="the-durability-characteristics-of-the-cluster"></a>De kenmerken van de duurzaamheid van het cluster
De duurzaamheidslaag wordt gebruikt om aan te geven aan het systeem de rechten van uw virtuele machines met de onderliggende Azure-infrastructuur. In het primaire knooppunttype kan deze bevoegdheid Service Fabric onderbreken van een virtuele machine op infrastructuur-aanvraag (zoals een VM opnieuw wordt opgestart, een virtuele machine terugzetten van een installatiekopie of een VM-migratie) die invloed hebben op de quorum-vereisten voor het systeem en de stateful services. In de knooppunttypen niet-primaire kan deze bevoegdheid Service Fabric onderbreken van een virtuele machine niveau infrastructuur aanvragen (zoals de VM opnieuw wordt opgestart, virtuele machine terugzetten van een installatiekopie en VM-migratie) die invloed hebben op de quorum-vereisten voor uw stateful services.

| Duurzaamheidslaag  | Vereiste minimum aantal VM 's | Ondersteunde VM-SKU 's                                                                  | Updates die u in uw VMSS aanbrengt                               | Updates en onderhoud gestart door Azure                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Goudkleurig             | 5                              | Volledige-node-SKU's die zijn toegewezen aan één klant (bijvoorbeeld L32s, GS5, G5, DS15_v2, D15_v2) | Kan worden uitgesteld totdat goedgekeurd door de Service Fabric-cluster | Gedurende 2 uur per UD waarmee extra tijd voor replica's te herstellen van eerdere fouten kan worden onderbroken |
| Zilver           | 5                              | Virtuele machines van één kern of hoger                                                        | Kan worden uitgesteld totdat goedgekeurd door de Service Fabric-cluster | Voor een aanzienlijke tijd actief kan niet worden uitgesteld                                                    |
| Brons           | 1                              | Alles                                                                                | Niet worden door de Service Fabric-cluster vertraagd           | Voor een aanzienlijke tijd actief kan niet worden uitgesteld                                                    |

> [!WARNING]
> Knooppunttypen met de duurzaamheid Brons verkrijgen _geen bevoegdheden_. Dit betekent dat infrastructuur-taken die invloed hebben op uw staatloze werkbelastingen worden niet worden gestopt of uitgesteld, kan dit gevolgen hebben voor uw workloads. Gebruik alleen Brons voor typen die alleen staatloze werkbelastingen worden uitgevoerd. Voor productieworkloads, Zilver uitgevoerd of hoger wordt aanbevolen. 

> Ongeacht eventuele duurzaamheidsniveau [toewijzing is opgeheven](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/deallocate) bewerking op de VM-Schaalset wordt het cluster vernietigen

**Voordelen van het gebruik van Silver- of Gold duurzaamheid niveaus**
 
- Vermindert het aantal vereiste stappen in een bewerking voor schaal (dat wil zeggen, knooppunt deactiveren en verwijderen-ServiceFabricNodeState wordt automatisch aangeroepen).
- Vermindert het risico van gegevensverlies als gevolg van een klant geïnitieerde ter plekke VM-SKU wijzigingsbewerking of bewerkingen voor Azure-infrastructuur.

**Nadelen van het gebruik van Silver- of Gold duurzaamheid niveaus**
 
- Implementaties naar uw virtuele-machineschaalset is ingesteld en andere gerelateerde Azure-resources kunnen worden uitgesteld, kunnen een time-out of volledig door problemen in uw cluster of op het niveau van de infrastructuur kunnen worden geblokkeerd. 
- Verhoogt het aantal [levenscyclusgebeurtenissen voor replica](service-fabric-reliable-services-lifecycle.md) (bijvoorbeeld primaire worden verwisseld) geautomatiseerde vanwege knooppunt deactivations tijdens de bewerkingen voor Azure-infrastructuur.
- Duurt knooppunten buiten gebruik gesteld voor perioden tijdens het Azure-platform software-updates of het onderhoud van hardware activiteiten plaatsvinden. Mogelijk ziet u knooppunten met de status uitschakelen/uitgeschakeld tijdens deze activiteiten. Dit vermindert de capaciteit van uw cluster tijdelijk, maar u moet niet van invloed op de beschikbaarheid van uw cluster of toepassingen.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Aanbevelingen voor het gebruik van Silver- of Gold duurzaamheid niveaus

Silver- of Gold duurzaamheid gebruiken voor alle typen die als host van stateful services die u verwacht te schalen in (VM-exemplaren beperken) vaak het geval is, en u liever dat implementatiebewerkingen worden vertraagd en -capaciteit worden verlaagd en vervangen door deze schalen in te vereenvoudigen bewerkingen. De scale-out-scenario's (toevoegen van VM-instanties) worden niet in uw eigen keuze aan de duurzaamheidslaag afgespeeld, alleen inschalen doet.

### <a name="changing-durability-levels"></a>Duurzaamheid niveaus wijzigen
- Knooppunttypen met duurzaamheid niveaus van Silver- of Gold geen downgrade worden uitgevoerd naar Brons.
- Upgraden van brons naar Silver- of Gold kan enkele uren duren.
- Bij het wijzigen van het duurzaamheidsniveau van, moet u werken het bij in zowel de Service Fabric configuratie van de extensie in de bron van uw virtuele machine scale set en in de definitie van het type knooppunt in uw Service Fabric-cluster-bron. Deze waarden moeten overeenkomen.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Operationele aanbevelingen voor het knooppunt typt u dat u hebt ingesteld op silver- of gold duurzaamheid niveau.

- Uw cluster en de toepassingen in orde te houden te allen tijde en zorg ervoor dat toepassingen op alle reageren [levenscyclusgebeurtenissen voor replica-Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) op tijdige wijze.
- Veiliger manieren om te maken van een VM-SKU wijzigen (omhoog/omlaag schalen) vast: wijzigen van de VM-SKU van een virtuele-machineschaalset is inherent een onveilige bewerking en dus moeten worden vermeden indien mogelijk. Dit is het proces dat u kunt volgen om veelvoorkomende problemen te voorkomen.
    - **Voor niet-primaire knooppunttypen:** het wordt aanbevolen dat u nieuwe virtuele-machineschaalset maken, wijzigen van de service plaatsing beperking voor het opnemen van de nieuwe virtuele machine scale set/knooppunttype en verlaagt u de oude VM-scale set-instantie aantal op 0, één knooppunt tegelijk (dit is om ervoor te zorgen dat het verwijderen van de knooppunten geen invloed hebben op de betrouwbaarheid van het cluster).
    - **Voor het primaire knooppunttype:** bij de aanbeveling is dat u VM-SKU van het primaire knooppunttype niet wijzigt. Wijzigen van het primaire knooppunttype dat SKU wordt niet ondersteund. Als de reden voor de nieuwe SKU capaciteit, raden wij aan meer exemplaren toevoegen. Als dat niet mogelijk is, een nieuw cluster maken en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster. U hoeft niet te herstellen van een systeemstatus van de service, worden ze opnieuw gemaakt wanneer u uw toepassingen naar het nieuwe cluster implementeert. Als u alleen stateless toepassingen uitgevoerd op het cluster, en u hoeft alleen uw toepassingen naar het nieuwe cluster te implementeren, er is niets om te herstellen. Als u besluit om te gaan van de niet-ondersteunde route en wilt wijzigen van de VM-SKU, instellen vervolgens maken wijzigingen in de virtuele-machineschaalset modeldefinitie in overeenstemming met de nieuwe SKU. Als uw cluster slechts één knooppunttype heeft, zorg ervoor dat uw stateful toepassingen op alle reageren [levenscyclusgebeurtenissen voor replica-Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) in een tijdige wijze en dat uw service-replica opnieuw maken duur is minder dan vijf minuten (voor Silver duurzaamheidsniveau). 
    
- Minimum aantal vijf knooppunten voor een virtuele-machineschaalset waarvoor duurzaamheidsniveau goud onderhouden of Silver ingeschakeld.
- Elke VM-schaalset met duurzaamheidsniveau Silver- of Gold moet worden toegewezen aan een eigen knooppunttype in het Service Fabric-cluster. Toewijzing van meerdere VM-schaalsets met een type één knooppunt wordt voorkomen dat coördinatie tussen de Service Fabric-cluster en de Azure-infrastructuur niet goed werkt.
- Verwijderen van willekeurige VM-exemplaren, altijd gebruik van virtual machine scale set neerschalen functie niet. Het verwijderen van willekeurige VM-exemplaren is een potentieel van evenwicht maken in het VM-exemplaar dat is verdeeld over UD en FD. Deze imbalance kan nadelige invloed heeft op de mogelijkheid systemen goed taakverdeling met de service-exemplaren/Service-replica's.
- Als automatisch schalen gebruik, stelt u de regels zodanig dat inschalen (verwijderen van de VM-exemplaren) slechts één knooppunt tegelijk worden gedaan. Meer dan één exemplaar tegelijk verkleint is niet veilig.
- Als verwijderen of de toewijzing van VM's ongedaan maken op het primaire knooppunttype, moet u het aantal toegewezen VM's hieronder wat de betrouwbaarheidslaag moet nooit verminderen. Deze bewerkingen worden voor onbepaalde tijd geblokkeerd in een schaalset met een duurzaamheidsniveau van Silver- of Gold.

## <a name="the-reliability-characteristics-of-the-cluster"></a>De betrouwbaarheidskenmerken van de van het cluster
De betrouwbaarheidslaag wordt gebruikt om het aantal replica's van de systeemservices die u wilt uitvoeren in dit cluster op het primaire knooppunttype instellen. De meer het aantal replica's, de betrouwbaarder zijn de systeemservices in uw cluster.  

De betrouwbaarheidslaag kan duren voordat de volgende waarden:

* Platina - uitvoeren van de services System met een doel replicaset telling van negen
* Gold - uitvoeren van de services System met een doel replicaset telling van zeven
* Silver - uitvoeren van de services System met een doel replicaset telling van vijf 
* Brons - uitvoeren van de services System met een doel replicaset telling van drie

> [!NOTE]
> De betrouwbaarheidslaag die u kiest, bepaalt het minimum aantal knooppunten dat het primaire knooppunttype moet hebben. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Aanbevelingen voor de betrouwbaarheidslaag

Wanneer u vergroten of verkleinen van de grootte van uw cluster (de som van de VM-exemplaren in alle typen), moet u de betrouwbaarheid van uw cluster op basis van één laag naar een andere bijwerken. Dit activeert het upgraden van clusters die nodig zijn om te wijzigen van de replica van de services system aantal instellen. Wacht tot de upgrade wordt uitgevoerd om te voltooien voordat u andere wijzigingen aanbrengt aan het cluster, zoals het toevoegen van knooppunten.  U kunt de voortgang van de upgrade in Service Fabric Explorer- of door te voeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Hier is de aanbeveling bij het kiezen van de betrouwbaarheidslaag.

| **Grootte van cluster** | **Betrouwbaarheidslaag** |
| --- | --- |
| 1 |Geef de parameter Betrouwbaarheidslaag geen berekend door het systeem |
| 3 |Brons |
| 5 of 6|Zilver |
| 7 of 8 |Goudkleurig |
| 9 en hoger |Platina |

## <a name="primary-node-type---capacity-guidance"></a>Primaire knooppunttype - richtlijnen voor capaciteit

Hier ziet u de aanwijzingen voor het plannen van de capaciteit van het primaire knooppunt-type:

- **Aantal VM-exemplaren voor het uitvoeren van een productie-werkbelasting in Azure:** moet u een minimale primaire knooppunt type grootte van 5 en een laag van Silver van betrouwbaarheid.  
- **Aantal VM-exemplaren testwerkbelastingen uitvoeren in Azure** kunt u een minimale primaire knooppunt type grootte van 1 of 3 opgeven. Het cluster met één knooppunt wordt uitgevoerd met een speciale configuratie en zijn er dus, opwaartse dat cluster wordt niet ondersteund. Het cluster met één knooppunt, heeft geen betrouwbaarheid en daarom in het Resource Manager-sjabloon, moet u verwijderen/niet opgeven dat de configuratie (niet de waarde van de configuratie is niet voldoende). Als u het cluster met één knooppunt ingesteld via de portal hebt ingesteld, wordt klikt u vervolgens de configuratie automatisch afgehandeld. Clusters met één en drie knooppunten worden niet ondersteund voor het uitvoeren van productieworkloads. 
- **VM-SKU:** primaire knooppunttype is waar de systeemservices worden uitgevoerd, zodat de VM-SKU u kiest, moet nemen bij de piek algehele-account laden u van plan bent om in het cluster te plaatsen. Hier volgt een vergelijking te laten zien wat ik hier betekenen - zien van het primaire knooppunttype als uw "longen', is wat uw brein zuurstof biedt, en dus als het brein moeite heeft geen voldoende zuurstof, uw instantie leidt dit tot slechtere. 

Omdat de capaciteitsbehoeften van een cluster wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, niet kan bieden we u met kwalitatieve richtlijnen voor uw specifieke werkbelasting, maar hier wordt de uitgebreide richtlijnen om u te helpen aan de slag

Voor werkbelastingen voor productie: 

- Het is raadzaam te reserveren voor uw clusters primaire NodeType systeemservices en plaatsingsbeperkingen gebruiken om uw toepassing naar secundaire NodeTypes te implementeren.
- De aanbevolen VM-SKU is Standard D3 of standaard D3_V2 of gelijkwaardig met een minimum van 14 GB lokale SSD.
- De minimale ondersteunde gebruik VM-SKU is Standard D1 of standaard D1_V2 of gelijkwaardig met een minimum van 14 GB lokale SSD. 
- De lokale SSD van 14 GB is een minimale vereiste. Onze aanbeveling is een minimum van 50 GB. Voor uw werkbelastingen, met name wanneer Windows-containers, zijn grotere schijven vereist. 
- Gedeeltelijke kern als Standard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standard A1-SKU wordt niet ondersteund voor productieworkloads voor betere prestaties.

> [!WARNING]
> Wijzigen van het primaire knooppunt VM-SKU-grootte op een cluster wordt uitgevoerd, is een bewerking waarbij de vergroten/verkleinen en gedocumenteerd in [virtuele-Machineschaalset scale-out](virtual-machine-scale-set-scale-node-type-scale-out.md) documentatie.

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Niet-primaire knooppunttype - capaciteit richtlijnen voor stateful werkbelastingen

Deze richtlijnen zijn bedoeld voor stateful werkbelastingen met behulp van Service fabric [betrouwbare verzamelingen of reliable Actors](service-fabric-choose-framework.md) die u in de niet-primaire knooppunttype worden uitgevoerd.

**Aantal VM-exemplaren:** voor productieworkloads die stateful zijn, is het raadzaam dat u ze hebt uitgevoerd met een minimum- en doelwaarde aantal replica's van 5. Dit betekent dat in onveranderlijke u uiteindelijk een replica (uit een replicaset) in elk foutdomein en upgradedomein. Het concept van de hele betrouwbaarheid laag voor het primaire knooppunttype is een manier om op te geven van deze instelling voor systeemservices. Zo geldt deze overweging ook is van toepassing op uw stateful services ook.

Voor productieworkloads is minimale aanbevolen niet - primaire type grootte van het knooppunt dus 5, als u in het stateful werkbelastingen worden uitgevoerd.

**VM-SKU:** dit is het knooppunttype waar de toepassingsservices van uw worden uitgevoerd, zodat de VM-SKU u kiest, moet rekening mee dat de piekbelasting die u van plan bent om in elk knooppunt te plaatsen. De capaciteitsbehoeften van de nodetype wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, zodat we kunnen niet u met kwalitatieve richtlijnen voor uw specifieke werkbelasting bieden, maar hier wordt de uitgebreide richtlijnen om u te helpen aan de slag

Voor werkbelastingen voor productie 

- De aanbevolen VM-SKU is Standard D3 of standaard D3_V2 of gelijkwaardig met een minimum van 14 GB lokale SSD.
- De minimale ondersteunde gebruik VM-SKU is Standard D1 of standaard D1_V2 of gelijkwaardig met een minimum van 14 GB lokale SSD. 
- Gedeeltelijke kern als Standard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standard A1-SKU wordt specifiek niet ondersteund voor productieworkloads voor betere prestaties.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Niet-primaire knooppunttype - capaciteit richtlijnen voor staatloze werkbelastingen

Deze richtlijnen van staatloze werkbelastingen die u op de niet-primaire nodetype uitvoert.

**Aantal VM-exemplaren:** voor productieworkloads staatloze items, is de minimale ondersteunde niet - primaire type grootte van het knooppunt 2. Hiermee kunt u twee stateless exemplaren van uw toepassing en zodat uw service voor het overbruggen van het verlies van gegevens van een VM-exemplaar uitvoeren. 

**VM-SKU:** dit is het knooppunttype waar de toepassingsservices van uw worden uitgevoerd, zodat de VM-SKU u kiest, moet rekening mee dat de piekbelasting die u van plan bent om in elk knooppunt te plaatsen. De capaciteitsbehoeften van het knooppunttype, wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, zodat we kunnen niet u met kwalitatieve richtlijnen voor uw specifieke werkbelasting bieden, maar hier wordt de uitgebreide richtlijnen om u te helpen aan de slag

Voor werkbelastingen voor productie 

- De aanbevolen VM-SKU is Standard D3 of standaard D3_V2 of gelijkwaardig. 
- De minimale ondersteunde gebruik VM-SKU is Standard D1 of standaard D1_V2 of gelijkwaardig. 
- Gedeeltelijke kern als Standard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standard A1-SKU wordt niet ondersteund voor productieworkloads voor betere prestaties.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
Zodra u klaar bent met het plannen van capaciteit en een cluster instellen, leest u het volgende:

* [Beveiliging van service Fabric-clusters](service-fabric-cluster-security.md)
* [Service Fabric-cluster schalen](service-fabric-cluster-scaling.md)
* [Planning voor noodherstel gaat](service-fabric-disaster-recovery.md)
* [Relatie van Nodetypes met virtuele-machineschaalset instellen](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

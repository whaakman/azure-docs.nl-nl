---
title: Planning van de capaciteit van de Service Fabric-cluster | Microsoft Docs
description: Service Fabric-cluster overwegingen bij capaciteitsplanning. Nodetypes, bewerkingen, duurzaamheid en betrouwbaarheid lagen
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
ms.date: 01/04/2018
ms.author: chackdan
ms.openlocfilehash: 78cff3ba5bd2f8bc80f302a232e45864159ca88f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641880"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-cluster overwegingen bij capaciteitsplanning
Voor productie-implementatie is capaciteitsplanning een belangrijke stap. Hier zijn enkele van de artikelen waarmee u rekening moet houden als onderdeel van dit proces.

* Het nummer van uw cluster beginnen moet met knooppunttypen
* De eigenschappen van elk knooppunttype (grootte, primair, internetgericht, aantal virtuele machines, enz.)
* De betrouwbaarheid en duurzaamheid van de clusterkenmerken

Laat het ons kort Bekijk elk van deze items.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Het nummer van uw cluster beginnen moet met knooppunttypen
Eerst moet u achterhalen wat het cluster dat u maakt moet worden gebruikt voor er gebeurt.  Welke soorten toepassingen die u van plan bent om te implementeren in dit cluster? Als u niet wissen op het doel van het cluster, u waarschijnlijk nog niet klaar om in te voeren van het proces voor de capaciteitsplanning.

Het nummer van uw cluster beginnen moet met knooppunttypen vast.  Elk knooppunttype is toegewezen aan een virtuele-machineschaalset. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. Dus het besluit van het aantal knooppunttypen in feite geleverd omlaag met de volgende aspecten:

* Uw toepassing beschikt over meerdere services en hoeft deze niet openbaar of verbonden met internet? Standaard-toepassingen bevatten een front-gatewayservice die de invoer van een client ontvangt en een of meer back-end-services die communiceren met de front-end-services. Dus in dit geval u uiteindelijk eindigen met typen voor ten minste twee knooppunten.
* Beschikt over uw services (die gezamenlijk uw toepassing) andere infrastructuur behoeften zoals meer RAM-geheugen of hoger CPU-cycli? Laat het ons aannemen dat de toepassing die u wilt implementeren een front-end-service en een back-endservice bevat. De front-end-service kunt uitvoeren op kleinere virtuele machines (VM-grootten zoals D2) die poorten geopend met het internet.  De back-endservice echter wordt intensief gebruik gemaakt van berekening en moet worden uitgevoerd op grotere VM's (met VM-grootten zoals D4 D6, D15) die geen internet facing.
  
  In dit voorbeeld, maar u kunt alle services op één knooppunttype plaatsen raden wij dat u deze in een cluster met twee typen van de knooppunten plaatsen.  Hiermee kunt elk knooppunttype om afzonderlijke eigenschappen zoals verbinding met internet of VM-grootte. Het aantal virtuele machines kan worden geschaald onafhankelijk, evenals.  
* Omdat u valt niet de toekomst te voorspellen, Ga met feiten die u weet en het aantal knooppunttypen die uw toepassingen worden gestart met moeten kiezen. U kunt altijd toevoegen of knooppunttypen later verwijderen. Een Service Fabric-cluster moet ten minste één knooppunttype hebben.

## <a name="the-properties-of-each-node-type"></a>De eigenschappen van elk knooppunttype
De **knooppunttype** kunnen worden beschouwd als equivalent aan rollen in Cloudservices. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Elk knooppunttype dat is gedefinieerd in een Service Fabric-cluster wordt toegewezen aan een [virtuele-machineschaalset](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Elk knooppunttype is een afzonderlijke scale ingesteld en kan worden uitgebreid of omlaag onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken hebben. Voor meer informatie over de relaties tussen knooppunttypen en virtuele-machineschaalsets hoe voor RDP in één van de exemplaren het openen van nieuwe poorten en enzovoort, Zie [knooppunttypen Service Fabric-cluster](service-fabric-cluster-nodetypes.md).

Een Service Fabric-cluster kan bestaan uit meer dan één knooppunttype. In dat geval het cluster bestaat uit één primaire knooppunttype en knooppunttypen van een of meer niet-primaire.

### <a name="primary-node-type"></a>Primaire knooppunttype

De Service Fabric-systeemservices (bijvoorbeeld de Cluster Manager-service of de Image Store-service) worden geplaatst op het primaire knooppunttype. 

![Schermopname van een cluster met twee knooppunttypen][SystemServices]

* De **minimale grootte van virtuele machines** voor het primaire knooppunt type wordt bepaald door de **duurzaamheid laag** u kiest. De standaard duurzaamheid laag is Brons. Zie [de duurzaamheid kenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) voor meer informatie.  
* De **minimum aantal virtuele machines** voor het primaire knooppunt type wordt bepaald door de **betrouwbaarheidslaag** u kiest. De betrouwbaarheidslaag standaard is Zilver. Zie [de betrouwbaarheidskenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) voor meer informatie.  

In de Azure Resource Manager-sjabloon, het primaire knooppunttype is geconfigureerd met de `isPrimary` kenmerk onder de [knooppunt typedefinitie](https://docs.microsoft.com/en-us/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Niet-primaire knooppunttype

Er is één primaire knooppunttype in een cluster met typen met meerdere knooppunten en de overige zijn niet-primaire.

* De **minimale grootte van virtuele machines** voor niet-primaire knooppunt typen wordt bepaald door de **duurzaamheid laag** u kiest. De standaard duurzaamheid laag is Brons. Zie [de duurzaamheid kenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) voor meer informatie.  
* De **minimum aantal virtuele machines** voor niet-primaire knooppunttypen is een. Echter, moet u dit nummer op basis van het aantal replica's van de toepassing/de services die u wilt uitvoeren in dit knooppunttype. Het aantal virtuele machines in een knooppunttype kan worden verhoogd, nadat u het cluster hebt geïmplementeerd.

## <a name="the-durability-characteristics-of-the-cluster"></a>De duurzaamheid kenmerken van het cluster
De laag duurzaamheid wordt gebruikt om aan te geven aan het systeem de rechten van uw virtuele machines met de onderliggende Azure-infrastructuur. In het primaire knooppunttype kan deze bevoegdheid Service Fabric VM niveau infrastructuur verzoeken (zoals een VM opnieuw wordt opgestart, VM terugzetten van de installatiekopie of VM-migratie) die van invloed zijn de vereisten van het quorum voor systeem en uw stateful services te onderbreken. In de niet-primaire knooppunttypen kan deze bevoegdheid Service Fabric VM niveau infrastructuur de verzoeken (zoals de VM opnieuw wordt opgestart, terugzetten van de installatiekopie VM en VM-migratie) die van invloed zijn de vereisten van de quorumconfiguratie voor uw stateful services te onderbreken.

| Duurzaamheid laag  | Minimum aantal virtuele machines vereist | Ondersteunde VM-SKU 's                                                                  | Updates die u in uw VMSS aanbrengt                               | Updates en onderhoud gestart door Azure                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Goudkleurig             | 5                              | Volledige knooppunt SKU's toegewezen aan één klant (bijvoorbeeld L32s, GS5, G5, DS15_v2, D15_v2) | Kunnen worden uitgesteld totdat goedgekeurd door de Service Fabric-cluster | Kan worden onderbroken gedurende 2 uur per UD waarmee extra tijd voor replica's voor het herstellen van eerdere fouten |
| Zilver           | 5                              | Virtuele machines van één kern of hoger                                                        | Kunnen worden uitgesteld totdat goedgekeurd door de Service Fabric-cluster | Kan niet worden uitgesteld tot een aanzienlijke tijd actief                                                    |
| Brons           | 1                              | Alle                                                                                | Niet worden door het Service Fabric-cluster vertraagd           | Kan niet worden uitgesteld tot een aanzienlijke tijd actief                                                    |

> [!WARNING]
> Knooppunttypen uitgevoerd met Brons duurzaamheid verkrijgen _geen bevoegdheden_. Dit betekent dat infrastructuur taken die invloed zijn op uw staatloze werkbelastingen niet worden gestopt of vertraagd, die mogelijk gevolgen voor uw werkbelastingen. Gebruik alleen Brons voor knooppunttypen die alleen staatloze werkbelastingen worden uitgevoerd. Zilver uitgevoerd of hierboven wordt aanbevolen voor productieworkloads. 
>

**Voordelen van het gebruik van zilver of goud duurzaamheid niveaus**
 
- Vermindert het aantal vereiste stappen in een bewerking voor schaal (dat wil zeggen, deactivering van het knooppunt en verwijder ServiceFabricNodeState heet automatisch).
- Vermindert het risico van gegevensverlies als gevolg van een klant geïnitieerde in-place VM SKU wijzigingsbewerking of bewerkingen van de Azure-infrastructuur.

**Nadelen van het gebruik van zilver of goud duurzaamheid niveaus**
 
- Implementaties voor de schaal van uw virtuele machine ingesteld en andere gerelateerde Azure-resources kunnen worden uitgesteld, kunnen een time-out of volledig door problemen in het cluster of op het niveau van de infrastructuur kunnen worden geblokkeerd. 
- Verhoogt het aantal [replica lifecycle gebeurtenissen](service-fabric-reliable-services-lifecycle.md) (bijvoorbeeld primaire worden verwisseld) automated vanwege knooppunt deactivations tijdens de bewerkingen van de Azure-infrastructuur.
- Neemt knooppunten buiten de service voor perioden tijdens het software-updates voor Azure-platform of onderhoud van hardware activiteiten plaatsvinden. Mogelijk ziet u de knooppunten met de status uitschakelen/uitgeschakeld tijdens deze activiteiten. Dit vermindert de capaciteit van uw cluster tijdelijk, maar u moet geen invloed op de beschikbaarheid van uw cluster of toepassingen.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Aanbevelingen voor het gebruik van zilver of goud duurzaamheid niveaus

Zilver of goud duurzaamheid gebruiken voor alle typen van de knooppunten die als stateful services die u verwacht host te schalen in (VM-exemplaren verminderen) vaak, en u liever implementatiebewerkingen oplopen en capaciteit voor deze schaal in vereenvoudigen worden teruggebracht naar bewerkingen. De scale-out-scenario's (toe te voegen exemplaren van virtuele machines) niet in uw keuze van de laag duurzaamheid worden afgespeeld, heeft alleen schaal in.

### <a name="changing-durability-levels"></a>Duurzaamheid niveaus wijzigen
- Knooppunttypen met duurzaamheid niveaus van zilver of goud kunnen niet worden verlaagd naar Brons.
- Upgraden van brons naar zilver of goud kan enkele uren duren.
- Wanneer u duurzaamheid niveau wijzigt, moet u bij te werken in zowel Service Fabric configuratie voor de uitbreiding in de bron van uw virtuele machine scale set en in de typedefinitie van het knooppunt in de Service Fabric-cluster-bron. Deze waarden moeten overeenkomen.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Operationele aanbevelingen voor het knooppunt typt u dat u hebt ingesteld op zilver of gold duurzaamheid niveau.

- Uw cluster en de toepassingen gezond te houden te allen tijde en zorg ervoor dat toepassingen op alle reageren [replica lifecycle gebeurtenissen Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) op tijdige wijze.
- Vast veiliger manieren een wijziging VM SKU (schaal omhoog/omlaag): het wijzigen van de VM-SKU van een virtuele-machineschaalset is inherent een onveilige bewerking en dus moeten worden vermeden indien mogelijk. Dit is het proces dat u kunt volgen om veelvoorkomende problemen voorkomen.
    - **Voor niet-primaire knooppunttypen:** het is raadzaam dat u een nieuwe virtuele-machineschaalset maken of wijzigen van de service plaatsing beperking zijn van de nieuwe virtuele machine scale set/knooppunttype en verlaagt u de oude virtuele machine scale set-exemplaar aantal op 0, één knooppunt tegelijk (dit is om ervoor te zorgen dat de verwijdering van de knooppunten hebben geen invloed op de betrouwbaarheid van het cluster).
    - **Voor het primaire knooppunttype:** onze aanbeveling is VM SKU van het primaire knooppunttype niet te wijzigen. Het wijzigen van het primaire knooppunttype dat SKU wordt niet ondersteund. Als de reden voor de nieuwe SKU capaciteit is, raden wij meer exemplaren toe te voegen. Als dat niet mogelijk een nieuw cluster maken en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster. U hoeft niet om eventuele systeemstatus van de service te herstellen, ze worden opnieuw gemaakt wanneer u uw toepassingen implementeren op het nieuwe cluster. Als u alleen staatloze toepassingen uitgevoerd op uw cluster, en u hoeft alleen uw toepassingen implementeren op het nieuwe cluster, niets om te herstellen. Als u besluit om te gaan van de niet-ondersteunde route en wilt wijzigen van de VM-SKU, ingesteld vervolgens zorg wijzigingen aan de virtuele-machineschaalset modeldefinitie in overeenstemming met de nieuwe SKU. Als uw cluster slechts één knooppunttype heeft, zorgt u ervoor dat alle stateful toepassingen op alle reageren [replica lifecycle gebeurtenissen Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) in een tijdige wijze en dat de replica van de service opnieuw samenstellen duur is minder dan vijf minuten (voor zilver duurzaamheid niveau). 

    > [!WARNING]
    > Het wijzigen van de grootte van de VM-SKU voor virtuele-machineschaalsets niet actief ten minste zilver duurzaamheid wordt niet aanbevolen. VM-SKU-grootte wijzigen is een gegevens-destructieve in-place infrastructuur-bewerking. Zonder enige mogelijkheid wilt vertragen of bewaken van deze wijziging is het mogelijk dat de bewerking kan leiden gegevensverlies van voor stateful services tot of andere onvoorziene operationele problemen kan, zelfs voor staatloze werkbelastingen veroorzaken. 
    > 
    
- Het minimale aantal vijf knooppunten voor alle virtuele-machineschaalset weergeven die over duurzaamheid niveau Goud onderhouden of zilver ingeschakeld.
- De schaal van elke VM instelt met duurzaamheid niveau Zilver of goud moet toewijzen aan een eigen knooppunttype in het Service Fabric-cluster. Toewijzing van meerdere VM-schaalsets naar een type met één knooppunt wordt voorkomen dat coördinatie tussen het Service Fabric-cluster en de Azure-infrastructuur goed werkt.
- Willekeurige VM-instanties verwijderen, gebruik altijd de virtuele machine scale set scale omlaag functie niet. De verwijdering van VM-instanties van willekeurige heeft een potentieel evenwicht maken in de VM-instantie verdeeld over UD en FD. Deze onbalans kan een nadelige invloed heeft op de mogelijkheid systemen correct verdelen over de service-exemplaren/Service-replica's.
- Als u automatisch schalen, stelt u de regels zodat schaal (Bezig met verwijderen van de VM-instanties) slechts één knooppunt tegelijk worden uitgevoerd. Het verkleinen van meer dan één exemplaar tegelijk is niet veilig.
- Als verwijderen of de toewijzing van virtuele machines op het primaire knooppunttype, moet u het aantal toegewezen virtuele machines onder welke de betrouwbaarheidslaag vereist nooit verminderen. Deze bewerkingen wordt in een schaal van duurzaamheid niveau Zilver of goud instellen voor onbepaalde tijd geblokkeerd.

## <a name="the-reliability-characteristics-of-the-cluster"></a>De betrouwbaarheidskenmerken van het cluster
De betrouwbaarheidslaag wordt gebruikt voor het aantal replica's van de systeemservices die u wilt uitvoeren in dit cluster op het primaire knooppunttype instellen. De meer het aantal replica's, de betrouwbaarder zijn de systeemservices in uw cluster.  

De betrouwbaarheidslaag kan duren voordat de volgende waarden:

* Platina - uitvoeren van de services System met een doel replicaset aantal negen
* Goud - uitvoeren van de services System met een doel replicaset telling van zeven
* Zilver - uitvoeren van de services System met een doel replicaset telling van vijf 
* Brons - uitvoeren van de services System met een doel replicaset telling van drie

> [!NOTE]
> De betrouwbaarheidslaag die u kiest, bepaalt het minimum aantal knooppunten dat het primaire knooppunttype moet hebben. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Aanbevelingen voor de betrouwbaarheidslaag

Als u vergroten of verkleinen van de grootte van uw cluster (de som van de VM-exemplaren in typen van alle knooppunten), moet u de betrouwbaarheid van uw cluster met één laag naar een andere bijwerken. Dit activeert de cluster-upgrades nodig om te wijzigen van de replica van de services system aantal instellen. Wacht totdat de upgrade wordt voltooid voordat u andere wijzigingen aanbrengt in het cluster, zoals het toevoegen van knooppunten.  U kunt de voortgang van de upgrade in Service Fabric Explorer of door te voeren [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Hier volgt de aanbeveling over het kiezen van de betrouwbaarheidslaag.

| **De clustergrootte van het** | **Betrouwbaarheidslaag** |
| --- | --- |
| 1 |Geef de parameter Betrouwbaarheidslaag berekend door het systeem |
| 3 |Brons |
| 5 of 6|Zilver |
| 7 of 8 |Goudkleurig |
| 9 en hoger |Platina |

## <a name="primary-node-type---capacity-guidance"></a>Primaire knooppunttype - capaciteit richtlijnen

Hier vindt u de richtlijnen voor het plannen van de capaciteit van het primaire knooppunt type:

- **Aantal VM-instanties van elke werkbelasting productie uitvoeren in Azure:** moet u een minimale primaire knooppunt puntgrootte van 5 opgeven. 
- **Aantal VM-instanties testwerkbelastingen uitvoeren in Azure** kunt u een primaire knooppunt dat de minimale grootte van 1 of 3. Het cluster met één knooppunt wordt uitgevoerd met een speciale configuratie en kan dus, schaal buiten het cluster wordt niet ondersteund. Het cluster met één knooppunt heeft geen betrouwbaarheid en dus in het Resource Manager-sjabloon u verwijderen/niet opgeven dat de configuratie (de instelling van de configuratiewaarde is niet voldoende). Als u het cluster met één knooppunt instellen via de portal hebt ingesteld, is klikt u vervolgens de configuratie automatisch afgehandeld. Clusters met één en drie knooppunten worden niet ondersteund voor het uitvoeren van de productie-workloads. 
- **VM SKU:** primaire knooppunttype is waar de systeemservices worden uitgevoerd, zodat de VM-SKU u kiest, moet nemen bij de algehele piek-account laden u van plan bent te plaatsen in het cluster. Hier volgt een vergelijking te illustreren heb hier - zien van het primaire knooppunttype als uw 'longen', is wat uw brain zuurstof biedt en dus als de brain geen voldoende zuurstof krijgt, uw instantie te lijden heeft onder. 

Omdat de capaciteitsbehoeften van een cluster wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, kan niet bieden we u met kwalitatieve richtlijnen voor uw specifieke werkbelasting, maar hier vindt u de algemene richtlijnen om u te helpen aan de slag

Voor productieworkloads: 

- De aanbevolen VM-SKU is standaard D3 of standaard D3_V2 of een vergelijkbare met minimaal 14 GB van de lokale SSD.
- Het minimale ondersteunde gebruik VM SKU is standaard D1 of standaard D1_V2 of equivalent met minimaal 14 GB van de lokale SSD. 
- Gedeeltelijke core zoals standaard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standaard A1 SKU wordt niet ondersteund voor productieworkloads voor betere prestaties.

> [!WARNING]
> Op dit moment is wordt het primaire knooppunt VM-SKU-grootte op een actief cluster wijzigen niet ondersteund. Kies het primaire knooppunttype VM SKU dus zorgvuldig, rekening houdend met de capaciteit van de toekomstige behoeften. Op dit moment de enige ondersteunde manier om het primaire knooppunttype op een nieuwe VM-SKU (kleiner of groter) is het maken van een nieuw cluster met de juiste capaciteit uw toepassingen en herstellen van status van de toepassing (indien van toepassing) implementeren vanuit de [ meest recente back-ups service](service-fabric-reliable-services-backup-restore.md) u uit het oude cluster hebt gemaakt. U hoeft niet om eventuele systeemstatus van de service te herstellen, ze worden opnieuw gemaakt wanneer u toepassingen implementeert voor het nieuwe cluster. Als u alleen staatloze toepassingen uitgevoerd op uw cluster, en u hoeft alleen uw toepassingen implementeren op het nieuwe cluster, niets om te herstellen.
> 

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Niet-primaire knooppunttype - capaciteit richtlijnen voor stateful werkbelastingen

In deze richtlijnen voor stateful werkbelastingen met behulp van Service fabric is [betrouwbare verzamelingen of reliable Actors](service-fabric-choose-framework.md) die u in het niet-primaire knooppunttype worden uitgevoerd.

**Aantal VM-instanties:** voor productieworkloads stateful zijn wordt aanbevolen dat u deze met een minimum- en aantal replica's van 5 uitvoeren. Dit betekent dat in de actieve status u uiteindelijk met een replica (van een replicaset) in elk domein met fouten en upgradedomein eindigen. Het concept van de hele betrouwbaarheid laag voor het primaire knooppunttype is een manier om deze instelling opgeven voor systeemservices. Hetzelfde geldt dus voor uw stateful services ook.

Voor productieworkloads dus minimale aanbevolen niet - primaire type grootte van het knooppunt 5, als u in het stateful werkbelastingen worden uitgevoerd.

**VM SKU:** . Dit is het knooppunttype waar uw toepassingsservices worden uitgevoerd, dus de VM-SKU u kiest, moet rekening gehouden met de piekbelasting u van plan bent om in elk knooppunt te plaatsen. De capaciteitsbehoeften van de nodetype wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, zodat we u met kwalitatieve richtlijnen voor uw specifieke werkbelasting, maar hier vindt u de algemene richtlijnen om u te helpen aan de slag niet opgeven

Voor productieworkloads 

- De aanbevolen VM-SKU is standaard D3 of standaard D3_V2 of een vergelijkbare met minimaal 14 GB van de lokale SSD.
- Het minimale ondersteunde gebruik VM SKU is standaard D1 of standaard D1_V2 of equivalent met minimaal 14 GB van de lokale SSD. 
- Gedeeltelijke core zoals standaard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standaard A1 SKU wordt specifiek niet ondersteund voor productieworkloads voor betere prestaties.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Niet-primaire knooppunttype - capaciteit richtlijnen voor staatloze werkbelastingen

In deze richtlijnen staatloze werkbelastingen die u op de niet-primaire nodetype uitvoert.

**Aantal VM-instanties:** voor productieworkloads staatloze zijn, is de minimale ondersteunde niet - primaire type knooppuntgrootte 2. Hiermee kunt u twee staatloze exemplaren van uw toepassing en waardoor uw service te blijven werken het verlies van een VM-instantie worden uitgevoerd. 

**VM SKU:** . Dit is het knooppunttype waar uw toepassingsservices worden uitgevoerd, dus de VM-SKU u kiest, moet rekening gehouden met de piekbelasting u van plan bent om in elk knooppunt te plaatsen. De capaciteitsbehoeften van de nodetype wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, zodat we u met kwalitatieve richtlijnen voor uw specifieke werkbelasting, maar hier vindt u de algemene richtlijnen om u te helpen aan de slag niet opgeven

Voor productieworkloads 

- De aanbevolen VM-SKU is standaard D3 of standaard D3_V2 of gelijkwaardig. 
- Het minimale ondersteunde gebruik VM SKU is standaard D1 of standaard D1_V2 of gelijkwaardig. 
- Gedeeltelijke core zoals standaard A0 VM-SKU's worden niet ondersteund voor productieworkloads.
- Standaard A1 SKU wordt niet ondersteund voor productieworkloads voor betere prestaties.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
Zodra u klaar bent met het plannen van capaciteit en een cluster instellen, leest u het volgende:

* [Beveiliging voor service Fabric-cluster](service-fabric-cluster-security.md)
* [Service Fabric-cluster schalen](service-fabric-cluster-scaling.md)
* [Oplossen van problemen](service-fabric-disaster-recovery.md)
* [Relatie van Nodetypes met virtuele-machineschaalset instellen](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

---
title: Planning van de capaciteit van de Service Fabric-cluster | Microsoft Docs
description: Service Fabric-cluster overwegingen bij capaciteitsplanning. Nodetypes, bewerkingen, duurzaamheid en betrouwbaarheid lagen
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: chackdan
ms.openlocfilehash: 8e2fceaf7e8a0d6c177d3122bd07de5b8c11f295
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-cluster overwegingen bij capaciteitsplanning
Voor productie-implementatie is capaciteitsplanning een belangrijke stap. Hier zijn enkele van de artikelen waarmee u rekening moet houden als onderdeel van dit proces.

* Het nummer van uw cluster beginnen moet met knooppunttypen
* De eigenschappen van elk knooppunttype (grootte, primair, internetgericht, aantal virtuele machines, enz.)
* De betrouwbaarheid en duurzaamheid kenmerken van het cluster

Laat het ons kort Bekijk elk van deze items.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Het nummer van uw cluster beginnen moet met knooppunttypen
Eerst moet u achterhalen wat het cluster dat u maakt moet worden gebruikt voor gaat en welke soorten toepassingen u van plan bent om te implementeren in dit cluster. Als u niet wissen op het doel van het cluster, u waarschijnlijk nog niet klaar om in te voeren van het proces voor de capaciteitsplanning.

Het nummer van uw cluster beginnen moet met knooppunttypen vast.  Elk knooppunttype is toegewezen aan een virtuele-Machineschaalset. Elk knooppunttype kan vervolgens worden uitgebreid of omlaag onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken kan hebben. Dus het besluit van het aantal knooppunttypen in feite geleverd omlaag met de volgende aspecten:

* Uw toepassing beschikt over meerdere services en hoeft deze niet openbaar of verbonden met internet? Standaard-toepassingen bevatten een front-gatewayservice die invoer ontvangt van een client en een of meer back-end-services die communiceren met de front-end-services. Dus in dit geval u uiteindelijk eindigen met typen voor ten minste twee knooppunten.
* Beschikt over uw services (die gezamenlijk uw toepassing) andere infrastructuur behoeften zoals meer RAM-geheugen of hoger CPU-cycli? Laat het ons aannemen dat de toepassing die u wilt implementeren een front-end-service en een back-endservice bevat. De front-end-service kunt uitvoeren op kleinere virtuele machines (VM-grootten zoals D2) die poorten geopend met het internet.  De back-endservice echter wordt intensief gebruik gemaakt van berekening en moet worden uitgevoerd op grotere VM's (met VM-grootten zoals D4 D6, D15) die geen internet facing.
  
  In dit voorbeeld, maar u kunt alle services op één knooppunttype plaatsen raden wij dat u deze in een cluster met twee typen van de knooppunten plaatsen.  Hierdoor kan elk knooppunttype om afzonderlijke eigenschappen zoals verbinding met internet of VM-grootte. Het aantal virtuele machines kan worden geschaald onafhankelijk, evenals.  
* Aangezien u valt niet de toekomst te voorspellen, gaat u met feiten die u precies weet en bepaal van het aantal knooppunttypen die uw toepassingen worden gestart met moeten. U kunt altijd toevoegen of knooppunttypen later verwijderen. Een Service Fabric-cluster moet ten minste één knooppunttype hebben.

## <a name="the-properties-of-each-node-type"></a>De eigenschappen van elk knooppunttype
De **knooppunttype** kunnen worden beschouwd als equivalent aan rollen in Cloudservices. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Elk knooppunttype dat is gedefinieerd in een Service Fabric-cluster is ingesteld als een afzonderlijke virtuele-machineschaalset. Virtuele-machineschaalset is een Azure compute-bron die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Wordt gedefinieerd als afzonderlijke virtuele-machineschaalset, elk knooppunttype kan vervolgens worden uitgebreid of omlaag onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken kan hebben.

Lees [dit document](service-fabric-cluster-nodetypes.md) voor meer informatie over de relatie van Nodetypes met de virtuele-machineschaalset hoe voor RDP in één van de exemplaren nieuwe poorten openen enzovoort.

Het cluster kan meer dan één knooppunttype hebben moet, maar het primaire knooppunttype (de eerste optie die u in de portal definieert) ten minste vijf VM's voor clusters die worden gebruikt voor productieworkloads (of ten minste drie virtuele machines voor testclusters). Als u het cluster met een Resource Manager-sjabloon maakt, zoek naar **primaire** kenmerk onder de typedefinitie van het knooppunt. Het primaire knooppunttype is het knooppunttype waar de Service Fabric-systeemservices worden geplaatst.  

### <a name="primary-node-type"></a>Primaire knooppunttype
Voor een cluster met typen met meerdere knooppunten moet u een van de twee primaire worden kiezen. Hier volgen de kenmerken van een primaire knooppunttype:

* De **minimale grootte van virtuele machines** voor het primaire knooppunt type wordt bepaald door de **duurzaamheid laag** u kiest. De standaardwaarde voor de laag duurzaamheid is Brons. Schuif omlaag voor meer informatie over wat de duurzaamheid laag is en de waarden die kan duren.  
* De **minimum aantal virtuele machines** voor het primaire knooppunt type wordt bepaald door de **betrouwbaarheidslaag** u kiest. De standaardwaarde voor de betrouwbaarheidslaag is Zilver. Schuif omlaag voor meer informatie over wat de betrouwbaarheidslaag is en de waarden die kan duren. 


* De Service Fabric-systeemservices (bijvoorbeeld de Cluster Manager-service of de Image Store-service) worden geplaatst op het primaire knooppunttype en dus de betrouwbaarheid en duurzaamheid van het cluster wordt bepaald door de laag waarde en duurzaamheid betrouwbaarheidslaag waarde als u hebt Selecteer voor het primaire knooppunttype.

![Schermopname van een cluster met twee knooppunttypen ][SystemServices]

### <a name="non-primary-node-type"></a>Niet-primaire knooppunttype
Er is een primaire knooppunttype voor een cluster met typen met meerdere knooppunten en de rest van deze zijn niet-primaire. Hier volgen de kenmerken van een niet-primaire knooppunttype:

* De minimale grootte van virtuele machines voor dit knooppunttype wordt bepaald door de duurzaamheid laag die u kiest. De standaardwaarde voor de laag duurzaamheid is Brons. Schuif omlaag voor meer informatie over wat de duurzaamheid laag is en de waarden die kan duren.  
* Het minimum aantal virtuele machines voor dit knooppunttype zijn. Maar u kunt dit nummer op basis van het aantal replica's van de toepassing/de services die u wilt uitvoeren in dit knooppunttype kiezen moet. Het aantal virtuele machines in een knooppunttype kan worden verhoogd, nadat u het cluster hebt geïmplementeerd.

## <a name="the-durability-characteristics-of-the-cluster"></a>De duurzaamheid kenmerken van het cluster
De laag duurzaamheid wordt gebruikt om aan te geven aan het systeem de rechten van uw virtuele machines met de onderliggende Azure-infrastructuur. In het primaire knooppunttype kan deze bevoegdheid Service Fabric VM niveau infrastructuur verzoeken (zoals een VM opnieuw wordt opgestart, VM terugzetten van de installatiekopie of VM-migratie) die van invloed zijn de vereisten van het quorum voor systeem en uw stateful services te onderbreken. In de niet-primaire knooppunttypen kan deze bevoegdheid Service Fabric VM niveau infrastructuur verzoeken zoals VM opnieuw wordt opgestart, VM terugzetten van de installatiekopie, VM-migratie enz., die van invloed zijn de vereisten van de quorumconfiguratie voor uw stateful services die worden uitgevoerd in het onderbreken.

Deze bevoegdheid wordt uitgedrukt in de volgende waarden:

* Goud - infrastructuur taken kan gedurende een periode van twee uur per UD worden onderbroken. Gold duurzaamheid kan alleen op volledige knooppunt VM-SKU's zoals L32s, GS5, G5, DS15_v2, D15_v2 enzovoort (In het algemeen alle VM-grootten die op http://aka.ms/vmspecs die zijn gemarkeerd als 'Exemplaar is geïsoleerd, zodat de hardware die zijn toegewezen aan één klant' in de opmerking worden ingeschakeld. Volledige knooppunt VM's)
* Zilver - infrastructuur taken gedurende een periode van tien minuten per UD kan worden onderbroken en is beschikbaar op alle standard VM's van één kern en hoger.
* Brons - geen bevoegdheden. Dit is de standaardinstelling. Dit niveau duurzaamheid alleen gebruiken voor knooppunttypen met _alleen_ staatloze werkbelastingen. 

> [!WARNING]
> NodeTypes uitgevoerd met Brons duurzaamheid verkrijgen _geen bevoegdheden_. Dit betekent dat infrastructuur taken die invloed zijn op uw staatloze werkbelastingen niet worden gestopt of vertraagd. Het is mogelijk dat dergelijke taken nog steeds invloed op uw werkbelastingen hebben kunnen, uitvaltijd of andere problemen veroorzaken. Voor een of andere vorm van productie werkbelasting uitgevoerd met ten minste zilver wordt aanbevolen. U moet een minimum aantal 5 knooppunten voor elk knooppunttype met een duurzaamheid van goud of zilver onderhouden. 
> 

U krijgt duurzaamheid voor elk van uw knooppunttypen kiezen. U kunt één knooppunttype om een duurzaamheid mate van goud of zilver en de andere hebt Brons in hetzelfde cluster. **Moet u het minimale aantal 5 knooppunten voor elk knooppunttype met een duurzaamheid van goud of zilver onderhouden**. 

**Voordelen van het gebruik van zilver of goud duurzaamheid niveaus**
 
1. Vermindert het aantal vereiste stappen in een bewerking voor schaal (dat wil zeggen, deactivering van het knooppunt en verwijder ServiceFabricNodeState heet automatisch)
2. Vermindert het risico van gegevensverlies als gevolg van een klant geïnitieerde in-place VM SKU wijzigingsbewerking of bewerkingen van de Azure-infrastructuur.
     
**Nadelen van het gebruik van zilver of goud duurzaamheid niveaus**
 
1. Implementaties voor uw virtuele-Machineschaalset en andere gerelateerde Azure-resources) kunnen worden uitgesteld, kunnen een time-out of volledig door problemen in het cluster of op het niveau van de infrastructuur kunnen worden geblokkeerd. 
2. Verhoogt het aantal [replica lifecycle gebeurtenissen](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (bijvoorbeeld primaire worden verwisseld) automated vanwege knooppunt deactivations tijdens de bewerkingen van de Azure-infrastructuur.

### <a name="recommendations-on-when-to-use-silver-or-gold-durability-levels"></a>Aanbevelingen voor het gebruik van zilver of goud duurzaamheid niveaus

Zilver of goud duurzaamheid gebruiken voor alle typen van de knooppunten die als stateful services die u verwacht host te schalen in (VM-exemplaren verminderen) vaak, en u liever dat implementatiebewerkingen voor het vereenvoudigen van deze schaal in bewerkingen worden uitgesteld. De scale-out-scenario's (toe te voegen exemplaren van virtuele machines) niet in uw keuze van de laag duurzaamheid worden afgespeeld, heeft alleen schaal in.

### <a name="changing-durability-levels"></a>Duurzaamheid niveaus wijzigen
- Knooppunttypen met duurzaamheid niveaus van zilver of goud kunnen niet worden verlaagd naar Brons.
- Upgraden van brons naar zilver of goud kan enkele uren duren.
- Wanneer u duurzaamheid niveau wijzigt, moet u bij te werken in zowel Service Fabric configuratie voor de uitbreiding in de resource VMSS en in de typedefinitie van het knooppunt in de Service Fabric-cluster-bron. Deze waarden moeten overeenkomen.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Operationele aanbevelingen voor het knooppunt typt u dat u hebt ingesteld op zilver of gold duurzaamheid niveau.

1. Uw cluster en de toepassingen gezond te houden te allen tijde en zorg ervoor dat toepassingen op alle reageren [replica lifecycle gebeurtenissen Service](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (zoals het maken van replica is vastgelopen) op tijdige wijze.
2. Vast veiliger manieren een wijziging VM SKU (schaal omhoog/omlaag): het wijzigen van de VM-SKU van een virtuele-Machineschaalset is inherent een onveilige bewerking en dus moeten worden vermeden indien mogelijk. Dit is het proces dat u kunt volgen om veelvoorkomende problemen voorkomen.
    - **Voor niet-primaire nodetypes:** het is raadzaam dat u nieuwe virtuele-Machineschaalset maken of wijzigen van de service plaatsing beperking zijn van de nieuwe virtuele-Machineschaalset Set/knooppunttype en verlaagt u de oude virtuele-Machineschaalset-exemplaar aantal op 0, één knooppunt tegelijk (dit is om ervoor te zorgen dat de verwijdering van de knooppunten hebben geen invloed op de betrouwbaarheid van het cluster).
    - **Voor de primaire nodetype:** onze aanbeveling is VM SKU van het primaire knooppunttype niet te wijzigen. Het wijzigen van het primaire knooppunttype dat SKU wordt niet ondersteund. Als de reden voor de nieuwe SKU capaciteit is, raden wij meer exemplaren toe te voegen. Als dat niet mogelijk een nieuw cluster maken en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster. U hoeft niet om eventuele systeemstatus van de service te herstellen, ze worden opnieuw gemaakt wanneer u uw toepassingen implementeren op het nieuwe cluster. Als u alleen staatloze toepassingen uitgevoerd op uw cluster, en u hoeft alleen uw toepassingen implementeren op het nieuwe cluster, niets om te herstellen. Als u besluit om te gaan van de niet-ondersteunde route en wilt wijzigen van de VM-SKU, brengt u wijzigingen in de virtuele Machine Scale ingesteld modeldefinitie in overeenstemming met de nieuwe SKU. Als uw cluster slechts één nodetype heeft, zorgt u ervoor dat alle stateful toepassingen op alle reageren [replica lifecycle gebeurtenissen Service](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (zoals het maken van replica is vastgelopen) in een tijdige wijze en dat de replica van de service opnieuw samenstellen duur is minder dan vijf minuten (voor zilver duurzaamheid niveau). 


> [!WARNING]
> Wijzigen van de VM-SKU-grootte voor VM-Schaalsets wordt niet actief ten minste zilver duurzaamheid niet aanbevolen. VM-SKU-grootte wijzigen is een gegevens-destructieve in-place infrastructuur-bewerking. Zonder enige mogelijkheid wilt vertragen of bewaken van deze wijziging is het mogelijk dat de bewerking kan leiden gegevensverlies van voor stateful services tot of andere onvoorziene operationele problemen kan, zelfs voor staatloze werkbelastingen veroorzaken. 
> 
    
3. Het minimale aantal vijf knooppunten voor alle virtuele-Machineschaalset die over duurzaamheid niveau Goud onderhouden of zilver ingeschakeld
4. Willekeurige VM-instanties verwijderen, gebruik altijd de virtuele-Machineschaalset scale omlaag functie niet. De verwijdering van VM-instanties van willekeurige heeft een potentieel evenwicht maken in de VM-instantie verdeeld over UD en FD. Deze onbalans kan een nadelige invloed heeft op de mogelijkheid systemen correct verdelen over de service-exemplaren/Service-replica's.
6. Als u automatisch schalen, stelt u de regels zodat schaal (Bezig met verwijderen van de VM-instanties) slechts één knooppunt tegelijk worden uitgevoerd. Het verkleinen van meer dan één exemplaar tegelijk is niet veilig.
7. Als het verkleinen van een primaire knooppunttype, moet u deze nooit omlaag meer dan de betrouwbaarheidslaag kunt schalen.


## <a name="the-reliability-characteristics-of-the-cluster"></a>De betrouwbaarheidskenmerken van het cluster
De betrouwbaarheidslaag wordt gebruikt voor het aantal replica's van de systeemservices die u wilt uitvoeren in dit cluster op het primaire knooppunttype instellen. De meer het aantal replica's, de betrouwbaarder zijn de systeemservices in uw cluster.  

De betrouwbaarheidslaag kan duren voordat de volgende waarden:

* Platina - uitvoeren van de services System met een doel replicaset aantal 9
* Goud - uitvoeren van de services System met een doel replicaset telling van 7
* Zilver - uitvoeren van de services System met een doel replicaset telling van 5 
* Brons - uitvoeren van de services System met een doel replicaset telling van 3

> [!NOTE]
> De betrouwbaarheidslaag die u kiest, bepaalt het minimum aantal knooppunten dat het primaire knooppunttype moet hebben. 
> 
> 


### <a name="recommendations-for-the-reliability-tier"></a>Aanbevelingen voor de betrouwbaarheidslaag.

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

Hier vindt u de richtlijnen voor het plannen van het primaire knooppunt type capaciteit

1. **Aantal VM-instanties van elke werkbelasting productie uitvoeren in Azure:** moet u een minimale primaire knooppunt puntgrootte van 5 opgeven. 
2. **Aantal VM-instanties testwerkbelastingen uitvoeren in Azure** kunt u een primaire knooppunt dat de minimale grootte van 1 of 3. Het cluster met één knooppunt wordt uitgevoerd met een speciale configuratie en kan dus, schaal buiten het cluster wordt niet ondersteund. Het cluster met één knooppunt heeft geen betrouwbaarheid en dus in het Resource Manager-sjabloon u verwijderen/niet opgeven dat de configuratie (de instelling van de configuratiewaarde is niet voldoende). Als u het cluster met één knooppunt instellen via de portal hebt ingesteld, is klikt u vervolgens de configuratie automatisch afgehandeld. clusters met knooppunten 1 en 3 worden niet ondersteund voor het uitvoeren van de productie-workloads. 
3. **VM SKU:** primaire knooppunttype is waar de systeemservices worden uitgevoerd, zodat de VM-SKU u kiest, moet nemen bij de algehele piek-account laden u van plan bent te plaatsen in het cluster. Hier volgt een vergelijking te illustreren heb hier - zien van het primaire knooppunttype als uw 'longen', is wat uw brain zuurstof biedt en dus als de brain geen voldoende zuurstof krijgt, uw instantie te lijden heeft onder. 

Omdat de capaciteitsbehoeften van een cluster wordt bepaald door de werkbelasting die u van plan bent om uit te voeren in het cluster, kan niet bieden we u met kwalitatieve richtlijnen voor uw specifieke werkbelasting, maar hier vindt u de algemene richtlijnen om u te helpen aan de slag

Voor productieworkloads 


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

> [!NOTE]
> Als uw cluster wordt uitgevoerd op een service fabric-versie lager dan 5.6, vanwege een fout in de runtime (dit probleem is opgelost als 5.6), het verkleinen van een niet-primaire knooppunttype tot minder dan 5 resulteert in het cluster health schakelen slechte, totdat u aanroepen [ Verwijder ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState) met de naam van het juiste knooppunt. Lees [Service Fabric-cluster uitvoeren in- of](service-fabric-cluster-scale-up-down.md) voor meer informatie
> 
>

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
* [Oplossen van problemen](service-fabric-disaster-recovery.md)
* [Relatie van Nodetypes met virtuele-machineschaalset instellen](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

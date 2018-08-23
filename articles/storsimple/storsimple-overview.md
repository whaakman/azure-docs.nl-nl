---
title: Overzicht van de oplossing van de StorSimple 8000-serie | Microsoft Docs
description: Beschrijving van StorSimple opslaglagen, het apparaat, virtueel apparaat, services en beheer van opslag, en introduceert de belangrijkste termen in StorSimple gebruikt.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 946b5a568d98367daec0244968b962618f22ae76
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055462"
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000-serie: een oplossing voor hybride cloudopslag
## <a name="overview"></a>Overzicht
Welkom bij Microsoft Azure StorSimple, een geïntegreerde opslagaccountoplossing waarmee tussen on-premises apparaten en Microsoft Azure-cloudopslag opslagtaken. StorSimple is een efficiënte, goedkope en eenvoudig beheersbare storage area network (SAN) oplossing waarmee veel van de problemen en de kosten die zijn gekoppeld aan de enterprise- en gegevensbescherming worden opgelost. Het maakt gebruik van de eigen apparaat van de StorSimple 8000-serie, kan worden geïntegreerd met cloudservices en biedt een set met beheerprogramma's voor een naadloze weergave van de gehele Ondernemingsopslag, waaronder cloudopslag. (Gepubliceerd op de website van Microsoft Azure StorSimple-implementatiegegevens van toepassing op StorSimple 8000-serie alleen apparaten. Als u van een apparaat uit de StorSimple 5000/7000-serie gebruikmaakt, gaat u naar [StorSimple Help](http://onlinehelp.storsimple.com/).)

StorSimple gebruikt [opslaglagen](#automatic-storage-tiering) voor het beheer van opgeslagen gegevens voor verschillende opslagmedia. De huidige werkset is opgeslagen op lokale solid state harde schijven (SSD's), gegevens die minder vaak worden gebruikt, is opgeslagen op hardeschijfstations (HDD's) en archivering van gegevens naar de cloud wordt gepusht. StorSimple gebruikt bovendien deduplicatie en compressie te verminderen de hoeveelheid opslag die de gegevens worden verbruikt. Ga voor meer informatie naar [Deduplicatie en compressie](#deduplication-and-compression). Voor definities van andere belangrijkste termen en concepten die worden gebruikt in de documentatie voor StorSimple 8000-serie, gaat u naar [StorSimple terminologie](#storsimple-terminology) aan het einde van dit artikel.

Naast het beheer van opslag, kunnen u op aanvraag maken met StorSimple functies voor gegevensbescherming en geplande back-ups en sla ze lokaal of in de cloud. Back-ups worden in de vorm van incrementele momentopnamen, wat betekent dat ze worden gemaakt en hersteld snel genomen. Cloudmomentopnamen kunnen zijn in noodgevallen herstelscenario's cruciaal omdat ze vervangen door secundaire opslag-systemen (zoals tapeback-up), en kunnen u gegevens naar uw datacenter of naar andere sites herstellen indien nodig.

![pictogram video](./media/storsimple-overview/video_icon.png) Bekijk de video voor een snelle Inleiding tot Microsoft Azure StorSimple.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?
De volgende tabel worden enkele van de belangrijkste voordelen van Microsoft Azure StorSimple biedt beschreven.

| Functie | Voordeel |
| --- | --- |
| Transparante-integratie |Maakt gebruik van het iSCSI-protocol onzichtbaar opslagruimten gegevens koppelen. Dit zorgt ervoor dat gegevens die zijn opgeslagen in de cloud, in het datacenter of op externe servers wordt weergegeven op één locatie worden opgeslagen. |
| Lagere opslagkosten |Toegewezen voldoende lokaal of opslag in de cloud om te voldoen aan de huidige vraag en opslag in de cloud alleen wanneer dat nodig is een uitbreiding. Dit vermindert verder de opslagvereisten en geld door het elimineren van redundante versies van dezelfde gegevens (gegevensontdubbeling) en met behulp van compressie. |
| Vereenvoudigd opslagbeheer |Biedt system beheerhulpprogramma's voor het configureren en beheren van opgeslagen gegevens on-premises, op een externe server, en in de cloud. Bovendien kunt u back-up beheren en functies herstellen vanuit een module Microsoft Management Console (MMC).|
| Verbeterde noodherstel en naleving |Vereist niet extended-recovery-tijd. In plaats daarvan worden deze gegevens hersteld als dat nodig is. Dit betekent dat normale bewerkingen kunnen doorgaan met minimale onderbreking. Bovendien kunt u beleid om op te geven van de back-upschema's en het bewaren van gegevens configureren. |
| Gegevensmobiliteit |Gegevens geüpload naar Microsoft Azure cloudservices zijn toegankelijk van andere sites voor herstel- en migratiedoeleinden. U kunt bovendien StorSimple gebruiken voor het configureren van StorSimple-Cloudapparaten op virtuele machines (VM's) die worden uitgevoerd in Microsoft Azure. De virtuele machines kunnen vervolgens virtuele apparaten gebruiken voor toegang tot opgeslagen gegevens voor test- of herstel. |
| Bedrijfscontinuïteit |StorSimple kunt 5000-7000 serie gebruikers kunnen hun gegevens migreren naar een StorSimple 8000-apparaat. |
| Beschikbaarheid in de Azure Government-Portal |StorSimple is beschikbaar in de Azure Government-Portal. Zie voor meer informatie, [implementeren van uw on-premises StorSimple-apparaat in Government-Portal](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Gegevensbescherming en -beschikbaarheid |De StorSimple 8000-serie biedt ondersteuning voor Zone Redundant Storage (ZRS), naast lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS). Raadpleeg [in dit artikel over Azure Storage-redundantieopties](https://azure.microsoft.com/documentation/articles/storage-redundancy/) voor ZRS meer informatie. |
| Ondersteuning voor essentiële toepassingen |StorSimple kunt u geschikte volumes als lokaal vastgemaakt identificeren die op zijn beurt zorgt ervoor dat de gegevens die zijn vereist voor essentiële toepassingen niet naar de cloud is gelaagd. Lokaal vastgemaakte volumes vallen niet onder cloud latenties of problemen met de netwerkverbinding. Zie voor meer informatie over het lokaal vastgemaakte volumes [de StorSimple Device Manager-service gebruiken voor het beheren van volumes](storsimple-8000-manage-volumes-u2.md). |
| Lage latentie en hoge prestaties |U kunt die van de hoge prestaties en lage latentie functies van Azure premium storage profiteren-cloudapparaten maken. Zie voor meer informatie over de StorSimple-cloudapparaten premium [implementeren en beheren van een StorSimple-Cloudapparaat in Azure](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple-onderdelen
De Microsoft Azure StorSimple-oplossing omvat de volgende onderdelen:

* **Microsoft Azure StorSimple-apparaat** – een on-premises hybride opslagmatrix met SSD's en HDD, samen met redundante controllers en automatisch failover wordt toegepast. De controllers beheren opslag in lagen, plaatsen die momenteel wordt gebruikt (of ' hot ') gegevens in lokale opslag (in de apparaat- of on-premises servers) tijdens het verplaatsen van gegevens die minder vaak gebruikt in de cloud.
* **StorSimple-Cloudapparaat** – ook wel bekend als de StorSimple Virtual Appliance, dit is een softwareversie van de StorSimple-apparaat dat wordt gerepliceerd van de architectuur en de meeste mogelijkheden van het apparaat fysiek hybride opslag. De StorSimple-Cloudapparaat wordt uitgevoerd op een enkel knooppunt in een virtuele machine van Azure. Premium virtuele apparaten, die van Azure premium-opslag profiteren, zijn beschikbaar in Update 2 en hoger.
* **StorSimple Device Manager-service** ; een uitbreiding van de Azure-portal waarmee u een StorSimple-apparaat of een StorSimple-Cloudapparaat beheren via één webinterface. U kunt de service StorSimple Device Manager maken en beheren van services, weergeven en beheren van apparaten, waarschuwingen weergeven, volumes, beheren en weergeven en beheren van back-upbeleid en de back-catalogus.
* **Windows PowerShell voor StorSimple** – een opdrachtregelinterface die u gebruiken kunt voor het beheren van het StorSimple-apparaat. Windows PowerShell voor StorSimple beschikt over functies die kunnen u uw StorSimple-apparaat registreren, configureren van de netwerkinterface op uw apparaat, bepaalde typen updates installeren, problemen oplossen van uw apparaat door het openen van de ondersteuningssessie en het apparaat wijzigen status. U kunt Windows PowerShell voor StorSimple openen door verbinding te maken met de seriële console of met behulp van Windows PowerShell voor externe toegang.
* **Azure PowerShell StorSimple-cmdlets** : een verzameling van Windows PowerShell-cmdlets waarmee u kunt voor het automatiseren van taken van het serviceniveau en migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [cmdlet-verwijzing](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – een MMC-module die gebruikmaakt van volumegroepen en de Windows Volume Shadow Copy-Service voor het genereren van toepassingsconsistente back-ups. Bovendien kunt u StorSimple Snapshot Manager back-upschema's en kloon maken en herstellen van volumes.
* **StorSimple Adapter voor SharePoint** : een hulpprogramma dat transparant en gegevensbescherming van Microsoft Azure StorSimple biedt een uitbreiding voor SharePoint Server-farms, bij het maken van cloudopslag met StorSimple kan worden weergegeven en beheerd in de SharePoint-Centraal -Beheerportal.

Het volgende diagram toont een globaal overzicht van de Microsoft Azure StorSimple-architectuur en onderdelen.

![StorSimple-architectuur](./media/storsimple-overview/overview-big-picture.png)

De volgende secties beschrijven elk van deze onderdelen in meer detail en wordt uitgelegd hoe de oplossing zorgt ervoor dat gegevens, opslag kan worden toegewezen en vereenvoudigt het uitvoeren van beheer van opslag- en gegevensbeveiliging. De laatste sectie bevat definities voor enkele belangrijke termen en concepten die betrekking hebben op het StorSimple-onderdelen en het beheer ervan.

## <a name="storsimple-device"></a>StorSimple-apparaat
Het Microsoft Azure StorSimple-apparaat is een on-premises hybride opslagmatrix waarmee primaire opslag- en iSCSI-toegang tot gegevens die erop zijn opgeslagen. Deze communicatie met opslag in de cloud beheert, en helpt om te controleren of de beveiliging en geheimhouding van alle gegevens die zijn opgeslagen op de Microsoft Azure StorSimple-oplossing.

Het StorSimple-apparaat bevat SSD's en HDD's van harde schijven, evenals ondersteuning voor clustering en automatische failover. Het bevat een gedeelde-processor, gedeelde opslag en twee gespiegelde domeincontrollers. Elke domeincontroller biedt het volgende:

* Verbinding met een hostcomputer
* Maximaal zes netwerkpoorten verbinding maken met het LAN (local area network)
* Hardwarebewaking
* Niet-vluchtige RAM-geheugen (NVRAM), die informatie behouden, zelfs als de stroom wordt onderbroken
* Clusterbewust bijwerken voor het beheren van software-updates op servers in een failovercluster, zodat de updates minimale hebben of geen invloed op de beschikbaarheid van de service
* Cluster-service, welke functies als een back-end-cluster, hoge beschikbaarheid en het minimaliseren van eventuele nadelige effecten die optreden kunnen als een harde schijf of een SSD uitvalt of offline te halen

Slechts één domeincontroller is actief op elk gewenst moment in-time. Als de actieve controller is mislukt, actief de tweede controller automatisch.

Ga voor meer informatie naar [StorSimple hardware-onderdelen en de status](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
U kunt StorSimple gebruiken om te maken van een cloudapparaat dat de architectuur en capaciteiten van het apparaat fysiek hybride opslag wordt gerepliceerd. De StorSimple Cloud Appliance (ook wel bekend als de StorSimple Virtual Appliance) wordt uitgevoerd op een enkel knooppunt in een virtuele machine van Azure. (Een cloudapparaat kan alleen worden gemaakt op een Azure-machine. U kunt geen maken op een StorSimple-apparaat of een on-premises server.)

Het cloudapparaat heeft de volgende functies:

* Het gedraagt zich als een fysiek apparaat en een iSCSI-interface voor virtuele machines in de cloud kan bieden.
* U kunt een onbeperkt aantal-cloudapparaten maken in de cloud, en deze in of uit als nodig.
* Het kan helpen bij het simuleren van on-premises omgevingen herstel na noodgevallen, ontwikkeling en Testscenario's en kan helpen bij het ophalen van op itemniveau vanuit back-ups.

De StorSimple-Cloudapparaat is beschikbaar in twee modellen: de 8010-apparaat (voorheen bekend als de 1100-model) en de 8020-apparaat. De 8010-apparaat heeft een maximale capaciteit van 30 TB. Het 8020-apparaat, wordt gebruikgemaakt van Azure premium-opslag, heeft een maximale capaciteit van 64 TB. (In de lokale lagen, Azure premium storage slaat gegevens op SSD's terwijl standard-opslag gegevens op HDD's opgeslagen.) Houd er rekening mee dat u moet een Azure premium storage-account naar premium storage gebruiken. Voor meer informatie over premium-opslag, gaat u naar [Premium Storage: hoogwaardige opslag voor Azure Virtual Machine-werkbelasting](../virtual-machines/windows/premium-storage.md).

Voor meer informatie over de StorSimple-Cloudapparaat, gaat u naar [implementeren en beheren van een StorSimple-Cloudapparaat in Azure](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>StorSimple-apparaatbeheerservice
Microsoft Azure StorSimple biedt een web gebaseerde gebruikersinterface (de StorSimple Device Manager-service) waarmee u centraal beheren van datacenter- en cloudopslag. De service StorSimple Device Manager kunt u de volgende taken uitvoeren:

* Configureren van systeeminstellingen voor StorSimple-apparaten.
* Configureren en beheren van instellingen voor StorSimple-apparaten.
* Cloudreferenties en eigenschappen configureren.
* Configureren en beheren van volumes op een server.
* Volumegroepen configureren.
* Back-up en herstellen van gegevens.
* Prestaties controleren.
* Controleer de instellingen en mogelijke problemen te identificeren.

De service StorSimple Device Manager kunt u alle beheerderstaken, behalve de waarvoor system uitvaltijd, zoals de eerste configuratie en installatie van updates.

Ga voor meer informatie naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell voor StorSimple
Windows PowerShell voor StorSimple biedt een opdrachtregelinterface die u gebruiken kunt om te maken en beheren van de Microsoft Azure StorSimple-service en instellen en controleren van de StorSimple-apparaten. Het is een Windows PowerShell gebaseerde opdrachtregelinterface met toegewezen cmdlets voor het beheren van uw StorSimple-apparaat. Windows PowerShell voor StorSimple bevat functies waarmee u kunt:

* Registreer een apparaat.
* Configureer de netwerkinterface op een apparaat.
* Bepaalde typen updates installeren.
* Problemen met het oplossen van uw apparaat door het openen van de ondersteuningssessie.
* Wijzig de status van het apparaat.

U kunt toegang tot Windows PowerShell voor StorSimple vanuit een seriële console (op een hostcomputer rechtstreeks verbonden met het apparaat) of op afstand met behulp van Windows PowerShell voor externe toegang. Houd er rekening mee dat sommige Windows PowerShell voor StorSimple-taken, zoals het eerste apparaatregistratie kan alleen worden uitgevoerd op de seriële console.

Ga voor meer informatie naar [gebruik Windows PowerShell voor StorSimple op uw apparaat beheren](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-cmdlets
De StorSimple van Azure PowerShell-cmdlets zijn een verzameling van Windows PowerShell-cmdlets waarmee u kunt voor het automatiseren van taken van het serviceniveau en migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [cmdlet-verwijzing](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die u gebruiken kunt om te maken consistent is, point-in-time back-ups van lokale gegevens en cloudgegevens. De module wordt uitgevoerd op een host op basis van Windows Server. U kunt StorSimple Snapshot Manager te gebruiken:

* Configureren van back-up en volumes verwijderen.
* Configureer volumegroepen om ervoor te zorgen dat back-upgegevens toepassingsconsistent is.
* Back-upbeleid beheren zodat gegevens worden back-up op een vooraf vastgestelde planning en in een aangewezen locatie opgeslagen (lokaal of in de cloud).
* Volumes en afzonderlijke bestanden herstellen.

Back-ups worden vastgelegd als momentopnamen, die alleen de wijzigingen vastgelegd sinds de laatste momentopname is gemaakt en veel minder opslagruimte dan volledige back-ups vereisen. U kunt maken van back-upschema's of onmiddellijke back-ups nemen indien nodig. Bovendien kunt u StorSimple Snapshot Manager tot stand brengen voor het bewaren van dat besturingselement hoeveel momentopnamen worden opgeslagen. Als u later nodig hebt voor het terugzetten van gegevens uit een back-up, StorSimple Snapshot Manager kunt selecteren u uit de catalogus van lokale of cloudmomentopnamen. 

Als zich een noodgeval voordoet, of als u herstellen van gegevens voor een andere reden wilt, StorSimple Snapshot Manager worden hersteld door het incrementeel nodig is. Gegevens worden hersteld vereist niet dat u het hele systeem afsluiten terwijl u een bestand herstellen, apparatuur vervangen of bewerkingen naar een andere site verplaatsen.

Ga voor meer informatie naar [wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter voor SharePoint
Microsoft Azure StorSimple bevat de StorSimple-Adapter voor SharePoint, een optioneel onderdeel dat transparant StorSimple opslag en functies voor gegevensbescherming op SharePoint-serverfarms breidt. De adapter werkt met een externe Blob Storage planning-provider en de functie voor SQL Server Resourcestructuur, zodat u BLOBs verplaatsen naar een server die back-up gemaakt door de Microsoft Azure StorSimple-systeem. Microsoft Azure StorSimple slaat vervolgens de BLOB-gegevens lokaal of in de cloud, op basis van gebruik.

De StorSimple-Adapter voor SharePoint wordt beheerd vanuit de portal voor Centraal beheer van SharePoint. Als gevolg hiervan blijft het beheer van SharePoint Centraal en alle opslag wordt weergegeven in de SharePoint-farm.

Ga voor meer informatie naar [StorSimple Adapter voor SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Storage management-technologieën
Naast het toegewezen StorSimple-apparaat, virtueel apparaat en andere onderdelen, Microsoft Azure StorSimple gebruikt de volgende softwaretechnologieën voor snelle toegang tot gegevens en aan opslagruimte te verminderen:

* [Automatische opslaglagen](#automatic-storage-tiering) 
* [Thin provisioning](#thin-provisioning) 
* [Deduplicatie en compressie](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatische opslaglagen
Microsoft Azure StorSimple is automatisch zorgt ervoor dat gegevens in logische lagen op basis van het huidige gebruik, leeftijd en relatie met andere gegevens. Gegevens die het meest actief is, worden lokaal opgeslagen terwijl minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. Het volgende diagram illustreert deze benadering van gegevensopslag.

![StorSimple-opslaglagen](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Voor snelle toegang, slaat StorSimple zeer actief data (gegevens) op SSD's in het StorSimple-apparaat. Er worden gegevens opgeslagen die af en toe wordt gebruikt (warme gegevens) op de HDD's in het apparaat of op servers in het datacenter. Wordt verplaatst van inactieve gegevens, back-upgegevens en gegevens bewaard voor archivering of compliancedoeleinden naar de cloud. 

> [!NOTE]
> Bij Update 2 of hoger, kunt u een als lokaal vastgemaakt volume opgeven, in welk geval de gegevens op het lokale apparaat blijft en niet is gelaagde in de cloud. 


StorSimple wordt aangepast en gegevens opnieuw worden ingedeeld en toewijzingen van opslag als de gebruikspatronen wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actieve na verloop van tijd. Omdat het progressief minder actieve, is deze gemigreerd van SSD naar HDD's en vervolgens naar de cloud. Als deze gegevens opnieuw geactiveerd wordt, wordt deze is gemigreerd naar het opslagapparaat.

De opslag cloudlagen proces verloopt als volgt:

1. Een systeembeheerder stelt u een Microsoft Azure cloud storage-account.
2. De beheerder maakt gebruik van de seriële console en de StorSimple Device Manager-service (die wordt uitgevoerd in Azure portal) voor serverconfiguratie apparaat en de bestandsnaam, het maken van volumes en gegevens die beveiligingsbeleid voor apps. Het Internet Small Computer System Interface (iSCSI) on-premises virtuele machines (zoals bestandsservers) gebruiken voor toegang tot het StorSimple-apparaat.
3. StorSimple worden gegevens in eerste instantie wordt opgeslagen op de snelle SSD-laag van het apparaat.
4. Als de SSD-laag capaciteit nadert, StorSimple beleidinstellingen en comprimeert de oudste gegevensblokken, en verplaatst naar de HDD-laag.
5. Als de capaciteit van HDD-laag benaderingen, StorSimple versleutelt de oudste gegevensblokken en stuurt ze veilig naar de Microsoft Azure storage-account via HTTPS.
6. Microsoft Azure maakt meerdere replica's van de gegevens in het datacenter en in een extern datacenter, ervoor te zorgen dat de gegevens kunnen worden hersteld als zich een noodgeval voordoet.
7. Wanneer de bestandsserver vraagt om gegevens die zijn opgeslagen in de cloud, wordt StorSimple naadloos geretourneerd en wordt er een kopie opgeslagen op de SSD-laag van de StorSimple-apparaat.

#### <a name="how-storsimple-manages-cloud-data"></a>Hoe StorSimple data-cloud beheert

StorSimple beleidinstellingen klantgegevens in alle momentopnamen en de primaire gegevens (gegevens die zijn geschreven door hosts). Hoewel Ontdubbeling erg handig voor de efficiëntie van opslag is, wordt de vraag "Wat is in de cloud' ingewikkeld. De primaire gelaagde gegevens en de momentopname van de gegevens overlappen met elkaar. Één segment van de gegevens in de cloud kan worden gebruikt als gelaagde primaire gegevens, en ook naar worden verwezen door meerdere momentopnamen. Elke cloud-momentopname zorgt ervoor dat er een kopie van alle point-in-time-gegevens naar de cloud is vergrendeld totdat het die momentopname is verwijderd.

Gegevens worden alleen verwijderd uit de cloud wanneer er geen verwijzingen naar die gegevens zijn. Bijvoorbeeld, als er een cloudmomentopname van alle gegevens die in het StorSimple-apparaat en verwijder vervolgens de primaire gegevens, zien we dat de _primaire gegevens_ onmiddellijk te verwijderen. De _gegevens in de cloud_ waaronder de gelaagde gegevens en de back-ups, blijft hetzelfde. Dit is omdat er een momentopname nog steeds verwijzen naar de cloudgegevens. Nadat de cloud momentopname is verwijderd (en eventuele andere momentopname die dezelfde gegevens waarnaar wordt verwezen) verbruik val in de cloud. Voordat we cloudgegevens verwijdert, wordt er gecontroleerd of er zijn geen momentopnamen nog steeds verwijzen naar die gegevens. Dit proces wordt genoemd _garbagecollection_ en wordt een achtergrondservice uitgevoerd op het apparaat. Verwijderen van cloudgegevens is niet direct, omdat de garbage collection-Services wordt gecontroleerd voor andere verwijzingen naar die gegevens vóór de verwijdering. De snelheid van de garbagecollection is afhankelijk van het totale aantal momentopnamen en de totale hoeveelheid gegevens. De cloudgegevens wordt doorgaans in minder dan een week opgeschoond.


### <a name="thin-provisioning"></a>Thin provisioning
Thin provisioning is een virtualisatietechnologie waarin beschikbare opslag meer dan fysieke resources wordt weergegeven. In plaats van het reserveren van tevoren voldoende opslagruimte, StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de actuele vereisten toe te wijzen. De elastische aard van opslag in de cloud vereenvoudigt het uitvoeren van deze benadering omdat StorSimple kunt vergroten of verkleinen van opslag in de cloud om te voldoen aan veranderende eisen.

> [!NOTE]
> Lokaal vastgemaakte volumes worden niet dun ingericht. Opslag toegewezen aan een lokaal volume wordt ingericht in zijn geheel toe wanneer het volume wordt gemaakt.


### <a name="deduplication-and-compression"></a>Deduplicatie en compressie
Microsoft Azure StorSimple maakt gebruik van Ontdubbeling en gegevens-compressie om opslagvereisten verder te beperken.

Ontdubbeling vermindert de totale hoeveelheid gegevens die zijn opgeslagen door het elimineren van redundantie in de opgeslagen gegevens-set. Als gegevens veranderen, StorSimple de ongewijzigde gegevens worden genegeerd en wordt alleen de wijzigingen vastgelegd. StorSimple wordt bovendien de hoeveelheid opgeslagen gegevens door te identificeren en verwijderen van onnodige gegevens. 

> [!NOTE]
> Gegevens van lokaal vastgemaakte volumes worden niet ontdubbeld of gecomprimeerd. Back-ups van lokaal vastgemaakte volumes zijn echter ontdubbeld en gecomprimeerd.


## <a name="storsimple-workload-summary"></a>StorSimple workloadoverzicht
Een overzicht van de ondersteunde werkbelastingen van StorSimple is Zie tabel hieronder.

| Scenario | Workload | Ondersteund | Beperkingen | Versie |
| --- | --- | --- | --- | --- |
| Samenwerking |Bestandsdeling |Ja | |Alle versies |
| Samenwerking |Gedistribueerde het delen van bestanden |Ja | |Alle versies |
| Samenwerking |SharePoint |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Archivering |Eenvoudig-bestand archiveren |Ja | |Alle versies |
| Virtualisatie |Virtuele machines |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Database |SQL |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Videobewaking |Videobewaking |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze workload is toegewezen |Update 2 en hoger |
| Backup |Back-up van primaire doel |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze workload is toegewezen |Update 3 en hoger |
| Backup |Back-up van secundaire doel |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze workload is toegewezen |Update 3 en hoger |

*Ja&#42; -oplossing richtlijnen en beperkingen moeten worden toegepast.*

De volgende werkbelastingen worden niet ondersteund door apparaten uit de StorSimple 8000-serie. Als geïmplementeerd op StorSimple, resulteert deze workloads in een niet-ondersteunde configuratie.

* Medische imaging
* Exchange
* VDI
* Oracle
* SAP
* Big Data
* Distributie van inhoud
* Opstarten vanaf SCSI

Hieronder volgt een lijst van de onderdelen van de infrastructuur van StorSimple wordt ondersteund.

| Scenario | Workload | Ondersteund | Beperkingen | Versie |
| --- | --- | --- | --- | --- |
| Algemeen |ExpressRoute |Ja | |Alle versies |
| Algemeen |DataCore FC |Ja* |Ondersteund met DataCore SANsymphony |Alle versies |
| Algemeen |DFSR |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Alle versies |
| Algemeen |Indexeren |Ja* |Voor gelaagde volumes, enige metagegevens indexeren wordt ondersteund (geen gegevens).<br>Voor lokaal vastgemaakte volumes wordt volledige indexering ondersteund. |Alle versies |
| Algemeen |Antivirusprogramma 's |Ja* |Voor gelaagde volumes wordt alleen scannen op open en close ondersteund.<br> Volledige scan wordt ondersteund voor lokaal vastgemaakte volumes. |Alle versies |

*Ja&#42; -oplossing richtlijnen en beperkingen moeten worden toegepast.*

Hieronder volgt een lijst met andere software die worden gebruikt met StorSimple om oplossingen te bouwen.

| Type werkbelasting | Software die worden gebruikt met StorSimple | Ondersteunde versies|Koppeling naar handleiding| 
| --- | --- | --- | --- |
| Back-updoel |Veeam |Veeam v 9 en hoger |[StorSimple als back-updoel met Veaam](storsimple-configure-backup-target-veeam.md)|
| Back-updoel |Veritas Backup Exec |Backup Exec 16 en hoger |[StorSimple als back-updoel met Backup Exec](storsimple-configure-backup-target-using-backup-exec.md)|
| Back-updoel |Veritas NetBackup |NetBackup 7.7.x en hoger  |[StorSimple als back-updoel met NetBackup](storsimple-configure-backuptarget-netbackup.md)|
| Algemene bestanden delen <br></br> Samenwerking |Talon  |[StorSimple met Talon](https://www.talonstorage.com/products/archive/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorSimple-terminologie
Voordat u uw Microsoft Azure StorSimple-oplossing implementeert, wordt u aangeraden de volgende termen en definities te controleren.

### <a name="key-terms-and-definitions"></a>Belangrijkste termen en definities
| Termijn (acroniem of afkorting) | Beschrijving |
| --- | --- |
| Acces controlerecord (ACR) |Een record die zijn gekoppeld aan een volume op uw Microsoft Azure StorSimple-apparaat waarmee wordt bepaald welke hosts verbinding mee kunnen maken. De bepaling is gebaseerd op de iSCSI Qualified Name (IQN) van de hosts (die zijn opgenomen in de ACR) die zijn verbonden met uw StorSimple-apparaat. |
| AES-256 |Een 256-bits Advanced Encryption Standard (AES)-algoritme voor het versleutelen van gegevens worden verplaatst naar en van de cloud. |
| grootte van toewijzingseenheid (AU) |De kleinste hoeveelheid schijfruimte die kan worden toegewezen voor het opslaan van een bestand in uw Windows-bestandssystemen. Als de bestandsgrootte van een niet een veelvoud van de grootte van het cluster is, extra ruimte moet worden gebruikt voor het opslaan van het bestand (tot de volgende meerdere van de clustergrootte) wat resulteert in verloren ruimte en fragmentatie van de harde schijf. <br>De aanbevolen AU's voor Azure StorSimple-volumes is 64 KB, omdat dit goed met de ontdubbelingsalgoritmen die worden samenwerkt. |
| geautomatiseerde opslaglagen |Automatisch minder actieve gegevens verplaatsen van SSD naar HDD's en vervolgens naar een laag in de cloud en vervolgens het inschakelen van beheer van alle opslag van een centrale interface. |
| back-catalogus |Een verzameling van back-ups, meestal die betrekking hebben op het toepassingstype dat is gebruikt. Deze verzameling wordt weergegeven in de blade back-catalogus van de gebruikersinterface van de StorSimple Device Manager-service. |
| back-catalogus-bestand |Een bestand met een lijst van beschikbare momentopnamen die momenteel zijn opgeslagen in de back-up van de StorSimple Snapshot Manager-database. |
| back-upbeleid |Een selectie van volumes, type back-up en een tijdschema waarmee u back-ups maken op een vooraf gedefinieerd schema. |
| binary large object (BLOB's) |Een verzameling van binaire gegevens die zijn opgeslagen als één entiteit in een database management systeem. BLOBs zijn doorgaans afbeeldingen, audio of andere multimedia-objecten, maar soms binaire uitvoerbare code is opgeslagen als een BLOB. |
| Challenge Handshake Authentication Protocol (CHAP) |Een protocol dat wordt gebruikt voor het verifiëren van de peer van een verbinding, op basis van de peer delen van een wachtwoord of geheim. CHAP is één richting of wederzijdse. Met één richting CHAP verifieert het doel een initiator. Wederzijdse CHAP vereist dat het doel de initiator geverifieerd en dat de initiator het doel verifieert. |
| Kloon |Een kopie van een volume. |
| Cloud als een laag (CaaT) |Cloudopslag is geïntegreerd als een laag in de opslagarchitectuur zodat alle opslag moet deel uitmaken van een onderneming opslagnetwerk weergegeven. |
| cloud serviceprovider (CSP) |Een provider van cloudcomputingservices. |
| cloud-momentopname |Een point-in-time-kopie van de volumegegevens die zijn opgeslagen in de cloud. Een cloud-momentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn met name handig voor herstel na noodgevallen. |
| coderingssleutel voor cloudopslag |Een wachtwoord of een sleutel die wordt gebruikt door uw StorSimple-apparaat voor toegang tot de versleutelde gegevens verzonden door uw apparaat naar de cloud. |
| clusterbewust bijwerken |Software-updates op servers in een failover-cluster beheren zodat de updates minimale hebben of geen invloed op de beschikbaarheid van de service. |
| gegevenspad |Een verzameling van functionele eenheden die onderling verbonden gegevensverwerking bewerkingen uitvoeren. |
| deactiveren |Een permanente actie die de verbinding tussen het StorSimple-apparaat en de gekoppelde cloud-service. Cloudmomentopnamen van het apparaat kunnen blijven na dit proces en worden gekloond of gebruikt voor herstel na noodgevallen. |
| het spiegelen van de schijf |Replicatie van de logische schijfvolumes op afzonderlijke vaste schijven in realtime om continue beschikbaarheid te garanderen. |
| het spiegelen van de dynamische schijf |Replicatie van de logische schijfvolumes op de dynamische schijven. |
| dynamische schijven |Een diskette volume-indeling die gebruikmaakt van de Logical Disk Manager (LDM) opslaan en beheren van gegevens over meerdere fysieke schijven. Dynamische schijven kunnen worden uitgebreid om meer ruimte vrij. |
| Behuizing van uitgebreide Bunch van schijven (EBOD) |Een secundaire behuizing van het Microsoft Azure StorSimple-apparaat dat extra harde schijf-schijven voor extra opslag bevat. |
| FAT inrichten |Een traditionele opslag inrichten die opslag ruimte wordt toegewezen op basis van behoeften verwacht (en meestal valt buiten het huidige nodig). Zie ook *thin provisioning*. |
| hardeschijfstation (HDD) |Een station dat gebruikmaakt van draaiende platen voor het opslaan van gegevens. |
| hybride cloudopslag |Een opslagarchitectuur die gebruikmaakt van lokale en externe bronnen, waaronder cloudopslag. |
| Internet Small Computer System Interface (iSCSI) |Een opslag op basis van IP-Protocol Internet netwerken standaard voor het koppelen van de gegevens opslagapparatuur of faciliteiten. |
| iSCSI-initiator |Een softwareonderdeel waarmee een hostcomputer waarop Windows verbinding maken met een externe opslag op basis van een iSCSI-netwerk wordt uitgevoerd. |
| iSCSI Qualified Name (IQN) |Een unieke naam ter identificatie van een iSCSI-doel of de initiator. |
| iSCSI-doel |Een softwareonderdeel dat gecentraliseerde iSCSI-schijfsubsystemen in storage area network biedt. |
| Live archiveren |Een benadering van gegevensopslag waarin archivering van gegevens toegankelijk voortdurend is (dit wordt niet opgeslagen op een externe locatie op tape, bijvoorbeeld). Microsoft Azure StorSimple maakt gebruik van live archiveren. |
| lokaal vastgemaakt volume |een volume dat zich bevindt op het apparaat en is nooit doorverbonden naar de cloud. |
| lokale momentopname |Een point-in-time-kopie van de volumegegevens die zijn opgeslagen op het Microsoft Azure StorSimple-apparaat. |
| Microsoft Azure StorSimple |Een krachtige oplossing die bestaan uit een opslagapparaat datacenter en de software waarmee IT-organisaties gebruikmaken van opslag in de cloud alsof het datacenter-opslag. StorSimple vereenvoudigt de beveiliging van gegevens en gegevensbeheer terwijl de kosten te verlagen. De oplossing worden geconsolideerd primaire opslag, archivering, back-up, en herstel na noodgevallen (DR) Dankzij de naadloze integratie met de cloud. StorSimple-apparaten inschakelen door een combinatie van SAN-opslag- en cloud gegevensbeheer op een enterprise-klasse-platform, de snelheid en eenvoud en betrouwbaarheid voor alle behoeften met betrekking tot opslag. |
| Voeding en koeling Module (PCM) |Hardware-onderdelen van uw StorSimple-apparaat die bestaan uit de voedingen en de ventilator, dus de naam van de stroom en koeling module. De primaire behuizing van het apparaat heeft twee 764W PCMs terwijl de behuizing EBOD twee 580 w bij PCMs heeft. |
| Primaire behuizing |Hoofdbehuizing van uw StorSimple-apparaat dat de toepassing platform domeincontrollers bevat. |
| Beoogde hersteltijd (RTO) |De maximale hoeveelheid tijd die moet worden opgebruikt voordat u een bedrijfsproces of het systeem is volledig is hersteld na een noodgeval. |
| serieel gekoppelde SCSI (SAS) |Een type hardeschijfstation (HDD). |
| versleutelingssleutel voor servicegegevens |Een sleutel beschikbaar gesteld aan een nieuwe StorSimple-apparaat dat wordt geregistreerd bij de StorSimple Device Manager-service. De configuratiegegevens worden overgebracht tussen de StorSimple Device Manager-service en het apparaat is versleuteld met een openbare sleutel en klikt u vervolgens alleen op het apparaat met een persoonlijke sleutel kan worden ontsleuteld. Versleutelingssleutel voor servicegegevens kan de service om op te halen van deze persoonlijke sleutel voor ontsleuteling. |
| serviceregistratiesleutel |Een sleutel die helpt het StorSimple-apparaat registreren bij de StorSimple Device Manager-service zodat deze wordt weergegeven in de Azure-portal voor verdere beheeracties. |
| Small Computer System Interface (SCSI) |Een verzameling standaarden voor fysieke computers verbinding en het doorgeven van gegevens. |
| SSD (Solid-state drive) |Een schijf die geen bewegende onderdelen bevat; bijvoorbeeld: een flash-station. |
| opslagaccount |Een set referenties voor toegang is gekoppeld aan uw storage-account voor een bepaalde cloudserviceprovider. |
| StorSimple Adapter voor SharePoint |Een Microsoft Azure StorSimple-component die transparant wordt uitgebreid StorSimple en gegevensbescherming naar SharePoint-serverfarms. |
| StorSimple-apparaatbeheerservice |Een uitbreiding van de Azure-portal waarmee u voor het beheren van uw Azure StorSimple on-premises en virtuele apparaten. |
| StorSimple Snapshot Manager |Een Microsoft Management Console (MMC)-module voor het beheren van back-up en herstellen van bewerkingen in Microsoft Azure StorSimple. |
| back-up maken |Een functie waarmee de gebruiker een interactieve back-up van een volume. Het is een alternatieve manier om een handmatige back-up maken van een volume in plaats van een geautomatiseerde back-up maken via een gedefinieerd beleid. |
| thin provisioning |Een methode voor het optimaliseren van de efficiëntie waarmee de beschikbare opslagruimte wordt gebruikt in opslagsystemen. De opslag wordt in dunne inrichting wordt toegewezen door meerdere gebruikers op basis van de minimale schijfruimte vereist voor elke gebruiker op een bepaald moment. Zie ook *fat inrichting*. |
| opslaglagen |Rangschikken van gegevens in logische groepen op basis van het huidige gebruik, leeftijd en relatie met andere gegevens. StorSimple is automatisch zorgt ervoor dat gegevens in lagen. |
| volume |Logische opslagplaatsen die zijn gepresenteerd in de vorm van schijven. StorSimple-volumes komen overeen met de volumes die zijn gekoppeld met de host, met inbegrip van die door het gebruik van iSCSI- en een StorSimple-apparaat wordt gedetecteerd. |
| volumecontainer |Een groepering van volumes en de instellingen die op hen van toepassing. Alle volumes in uw StorSimple-apparaat zijn gegroepeerd in volumecontainers. Containerinstellingen volume zijn opslagaccounts, encryption-instellingen voor gegevens die worden verzonden naar de cloud gekoppelde versleutelingssleutels, en bandbreedte voor bewerkingen met betrekking tot de cloud. |
| volumegroep |StorSimple Snapshot Manager, een volumegroep is een verzameling van volumes die zijn geconfigureerd voor het vereenvoudigen van een back-up. |
| Volume Shadow Copy Service (VSS) |Een service van de Windows Server-besturingssysteem die zorgt voor een toepassing consistente door te communiceren met de VSS-bewuste toepassingen voor de coördinatie van het maken van incrementele momentopnamen. VSS zorgt ervoor dat de toepassingen tijdelijk niet actief zijn wanneer momentopnamen worden gemaakt. |
| Windows PowerShell voor StorSimple |Een Windows PowerShell gebaseerde opdrachtregelinterface gebruikt om te werken en uw StorSimple-apparaat te beheren. Tijdens het onderhoud van enkele van de functionaliteit van basis van Windows PowerShell, heeft deze interface extra toegewezen cmdlets die zijn afgestemd op het beheren van een StorSimple-apparaat. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple security](storsimple-8000-security.md).


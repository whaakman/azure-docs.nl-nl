---
title: Overzicht van de oplossing StorSimple 8000 series | Microsoft Docs
description: Beschrijft StorSimple lagen, het apparaat, virtueel apparaat, services en opslagbeheer en introduceert belangrijke termen die in StorSimple gebruikt.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 10f4b6ec29c2d9539fa7fe61c96581755e630822
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000-serie: een hybride cloud-opslagoplossing
## <a name="overview"></a>Overzicht
Welkom bij Microsoft Azure StorSimple, een geïntegreerde opslagoplossing die opslagtaken tussen on-premises apparaten en Microsoft Azure cloud-opslag beheert. StorSimple is een efficiënte, rendabele en eenvoudig beheerbare storage area network (SAN) oplossing die veel van de problemen en de kosten in verband met enterprise-opslag en gegevensbeveiliging. Deze wordt het eigen StorSimple 8000 series apparaat gebruikt, kan worden geïntegreerd met cloudservices en biedt een set van beheerhulpprogramma's voor een naadloze weergave van alle Ondernemingsopslag, met inbegrip van cloud-opslag. (Gepubliceerd op de website van Microsoft Azure StorSimple-implementatiegegevens van toepassing op StorSimple 8000 series alleen apparaten. Als u van een apparaat StorSimple 5000/7000-serie gebruikmaakt, gaat u naar [StorSimple Help](http://onlinehelp.storsimple.com/).)

Maakt gebruik van StorSimple [opslaglagen](#automatic-storage-tiering) voor het beheren van opgeslagen gegevens op verschillende opslagmedia. De huidige werkset is lokaal opgeslagen op Solid-State stations (SSD's), gegevens die minder vaak wordt gebruikt, worden opgeslagen op de harde schijven (HDD's) en archivering van gegevens naar de cloud wordt gepusht. Bovendien StorSimple maakt gebruik van gegevensontdubbeling en compressie te verminderen de hoeveelheid opslagruimte die de gegevens worden verbruikt. Ga voor meer informatie naar [Gegevensontdubbeling en compressie](#deduplication-and-compression). Voor definities van andere belangrijkste termen en concepten die worden gebruikt in de documentatie van de StorSimple 8000 series, gaat u naar [StorSimple terminologie](#storsimple-terminology) aan het einde van dit artikel.

Naast het beheer van opslag, kunnen u op aanvraag maken met functies voor gegevensbeveiliging StorSimple en geplande back-ups en deze vervolgens opslaan lokaal of in de cloud. Back-ups zijn gemaakt in de vorm van incrementele momentopnamen, wat betekent dat ze worden gemaakt en snel worden hersteld. Cloudmomentopnamen is zeer belangrijk in herstel na noodgevallen, omdat ze secundaire opslagsystemen (zoals tapeback-up vervangen) en kunnen u gegevens op uw datacenter of op alternatieve locaties herstellen indien nodig.

![video-pictogram](./media/storsimple-overview/video_icon.png) Bekijk de video voor een korte inleiding in Microsoft Azure StorSimple.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?
De volgende tabel worden enkele van de belangrijkste voordelen van Microsoft Azure StorSimple beschreven.

| Functie | Voordeel |
| --- | --- |
| Transparante-integratie |Maakt gebruik van het iSCSI-protocol onzichtbaar opslagruimten gegevens te koppelen. Dit zorgt ervoor dat gegevens die zijn opgeslagen in de cloud, in het datacenter of op externe servers wordt weergegeven op één locatie worden opgeslagen. |
| Lagere opslagkosten |Toegewezen voldoende lokaal of cloud-opslag om te voldoen aan de huidige behoeften en breidt cloud-opslag alleen indien nodig. Dit vermindert verder de opslagvereisten en onkosten doordat redundante versies van dezelfde gegevens (Ontdubbeling) en door het gebruik van compressie. |
| Vereenvoudigd opslagbeheer |Biedt beheerprogramma's om te configureren en beheren van gegevens die zijn opgeslagen op locatie, op een externe server en in de cloud. Bovendien kunt u back-up beheren en functies herstellen vanuit een module Microsoft Management Console (MMC).|
| Verbeterde noodherstel en naleving |Vereist geen uitgebreide hersteltijd. In plaats daarvan worden hersteld gegevens nodig is. Dit betekent dat met het normale bewerkingen kunnen doorgaan met minimale onderbrekingen. Bovendien kunt u beleid om back-upschema en bewaren van gegevens te geven. |
| Gegevensmobiliteit |Gegevens geüpload naar Microsoft Azure cloudservices toegankelijk zijn vanaf andere sites voor herstel- en migratiedoeleinden. Bovendien kunt u StorSimple StorSimple Cloud toestellen configureren op virtuele machines (VM's) in Microsoft Azure wordt uitgevoerd. De virtuele machines kunnen vervolgens virtuele apparaten gebruiken voor toegang tot opgeslagen gegevens voor test- of herstel. |
| Bedrijfscontinuïteit |Kan StorSimple 5000 7000-serie gebruikers hun om gegevens te migreren naar een StorSimple 8000 series apparaat. |
| Beschikbaarheid in de Portal voor Azure Government |StorSimple is beschikbaar in de overheid van de Azure Portal. Zie voor meer informatie [implementeren van uw on-premises StorSimple-apparaat in de Portal Government](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Gegevensbescherming en beschikbaarheid |De StorSimple 8000-serie ondersteunt Zone redundante opslag (ZRS), naast de lokaal redundante opslag (LRS) en geografisch redundante opslag (GRS). Raadpleeg [in dit artikel over Azure Storage redundantieopties](https://azure.microsoft.com/documentation/articles/storage-redundancy/) voor ZRS meer informatie. |
| Ondersteuning voor kritieke toepassingen |StorSimple kunt u identificeren relevante volumes als lokaal vastgemaakt die op zijn beurt zorgt ervoor dat gegevens die nodig zijn voor kritieke toepassingen niet naar de cloud is gelaagd. Lokaal vastgemaakte volumes vallen niet onder cloud latenties of problemen met de netwerkverbinding. Zie voor meer informatie over lokaal vastgemaakte volumes [de service Manager voor StorSimple-apparaat gebruiken voor het beheren van volumes](storsimple-8000-manage-volumes-u2.md). |
| Lage latentie en hoge prestaties |U kunt de cloud-toepassingen die van de hoge prestaties, lage latentie functies van Azure premium-opslag gebruikmaken maken. Zie voor meer informatie over StorSimple premium cloud toestellen [implementeren en beheren van een StorSimple-Cloud-toestel in Azure](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple-onderdelen
De Microsoft Azure StorSimple-oplossing omvat de volgende onderdelen:

* **Microsoft Azure StorSimple-apparaat** – een lokale hybride-opslagmatrix die SSD en HDD's, samen met automatische failover-mogelijkheden en redundante controllers bevat. De controllers beheren opslag in lagen, het plaatsen van momenteel gebruikte (of hot) gegevens op de lokale opslag (in de apparaat- of on-premises servers) tijdens het verplaatsen van minder vaak gebruikte gegevens naar de cloud.
* **StorSimple Cloud toestel** – ook wel bekend als het virtuele StorSimple-apparaat, dit is een softwareversie van het StorSimple-apparaat die de architectuur repliceert en de meeste functies van het fysieke hybride opslagapparaat. Het toestel StorSimple-Cloud op één knooppunt in Azure een virtuele machine wordt uitgevoerd. Premium virtuele apparaten die van Azure premium-opslag gebruikmaken, zijn beschikbaar in Update 2 en hoger.
* **StorSimple-apparaat Manager-service** – een uitbreiding van de Azure portal waarmee u een StorSimple-apparaat of StorSimple Cloud toestel beheren via een enkel webinterface. U kunt de service Manager voor StorSimple-apparaat maken en beheren van services, weergeven en beheren van apparaten, waarschuwingen weergeven, volumes, beheren en weergeven en beheren van back-upbeleid en de back-upcatalogus gebruiken.
* **Windows PowerShell voor StorSimple** – een opdrachtregelinterface die u gebruiken kunt voor het beheren van het StorSimple-apparaat. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u uw StorSimple-apparaat registreren, configureren van de netwerkinterface op het apparaat, bepaalde typen updates installeren, het apparaat oplossen door het openen van de ondersteuningssessie en de apparaatstatus te wijzigen. U kunt Windows PowerShell voor StorSimple openen door verbinding te maken met de seriële console of via Windows PowerShell op afstand.
* **Azure PowerShell StorSimple-cmdlets** : een verzameling van Windows PowerShell-cmdlets waarmee u kunt het automatiseren van taken van het serviceniveau en migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [cmdlet-verwijzing](/powershell/module/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – een MMC-module die gebruikmaakt van volume groepen en de Windows Volume Shadow Copy-Service voor het genereren van toepassingsconsistente back-ups. Bovendien kunt u StorSimple Snapshot Manager te maken van back-upschema en klonen of volumes te herstellen.
* **StorSimple-Adapter voor SharePoint** – een hulpprogramma dat transparant Microsoft Azure StorSimple-opslag en gegevensbescherming naar SharePoint-Server uitbreiden farms tijdens het maken van StorSimple opslag worden bekeken en beheerd via de portal SharePoint Centraal beheer.

Het volgende diagram toont een globaal overzicht van de Microsoft Azure StorSimple-architectuur en onderdelen.

![StorSimple-architectuur](./media/storsimple-overview/overview-big-picture.png)

De volgende secties beschrijven elk van deze onderdelen in meer detail en wordt uitgelegd hoe de oplossing rangschikt gegevens, wijst opslag en vereenvoudigt het beheer van opslag en gegevensbeveiliging. De laatste sectie bevat definities voor sommige van de belangrijke termen en concepten die betrekking hebben op het StorSimple-onderdelen en het beheer ervan.

## <a name="storsimple-device"></a>StorSimple-apparaat
Het Microsoft Azure StorSimple-apparaat is een lokale hybride-opslagmatrix die voorziet in primaire opslag- en iSCSI-toegang tot gegevens die erop zijn opgeslagen. Deze communicatie met de cloudopslag beheert en kunt u de beveiliging en de vertrouwelijkheid van alle gegevens die zijn opgeslagen op de Microsoft Azure StorSimple-oplossing.

Het StorSimple-apparaat bevat SSD en HDD's van harde schijven, evenals ondersteuning voor clustering en automatische failover. Het bevat een gedeelde processor, gedeelde opslag en twee gespiegelde controllers. Elke domeincontroller biedt het volgende:

* Verbinding met een hostcomputer
* Maximaal zes netwerkpoorten verbinding maken met het local area network (LAN)
* Hardwarebewaking
* Niet-vluchtige RAM-geheugen (NVRAM) waarin informatie behouden, zelfs als de stroom wordt onderbroken
* Clusterbewust updaten voor het beheren van software-updates op servers in een failovercluster, zodat de updates minimale hebben of geen effect op de beschikbaarheid van de service
* Cluster-service, welke functies, zoals een back-end-cluster, bieden hoge beschikbaarheid en voor het minimaliseren van een nadelige effecten die optreden kunnen als een harde schijf of de SSD uitvalt of offline gehaald

Slechts één domeincontroller is actief op elk gewenst moment in de tijd. Als de actieve controller mislukt, actief de tweede controller automatisch.

Ga voor meer informatie naar [StorSimple hardwareonderdelen en status](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
U kunt StorSimple gebruiken voor het maken van een cloud-apparaat waarmee de architectuur en mogelijkheden van het opslagapparaat fysieke hybride worden gerepliceerd. Het StorSimple-Cloud-apparaat (ook wel bekend als de StorSimple virtueel apparaat) wordt uitgevoerd op één knooppunt in Azure een virtuele machine. (Een cloud-apparaat kan alleen worden gemaakt op een virtuele machine van Azure. U kunt geen maken op een StorSimple-apparaat of een on-premises server.)

Het toestel cloud heeft de volgende functies:

* Het gedraagt zich als een fysieke apparaat en een iSCSI-interface voor virtuele machines in de cloud te kan bieden.
* U kunt een onbeperkt aantal cloud-apparaten in de cloud maken en deze in en uit te schakelen indien nodig.
* Het kan helpen bij het simuleren van on-premises omgevingen herstel na noodgevallen, ontwikkeling en Testscenario's en kan helpen bij het ophalen van back-ups op itemniveau.

Het StorSimple Cloud toestel is beschikbaar in twee modellen: apparaat aan de 8010 (voorheen bekend als het model 1100) en het 8020-apparaat. De 8010-apparaat heeft een maximale capaciteit van 30 TB. Het 8020-apparaat, die gebruikmaakt van Azure premium-opslag, heeft een maximale capaciteit van 64 TB. (In de lokale lagen Azure premium-opslag gegevens opgeslagen op SSD's dat standaardopslag gegevens op HDD's slaat.) Houd er rekening mee dat er moet een Azure premium storage-account te gebruiken van premium-opslag. Voor meer informatie over de premium-opslag, gaat u naar [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../virtual-machines/windows/premium-storage.md).

Voor meer informatie over het StorSimple-Cloud-apparaat, gaat u naar [implementeren en beheren van een StorSimple-Cloud-toestel in Azure](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>StorSimple-apparaat Manager-service
Microsoft Azure StorSimple biedt een webgebaseerde gebruikersinterface (de StorSimple-apparaat Manager-service) waarmee u kunt centraal datacenter beheren en cloudopslag. De service Manager voor StorSimple-apparaat kunt u de volgende taken uitvoeren:

* Systeeminstellingen voor StorSimple-apparaten configureren.
* Configureren en beheren van de beveiligingsinstellingen voor StorSimple-apparaten.
* Cloud-referenties en eigenschappen configureren.
* Configureren en beheren van volumes op een server.
* Volume groepen configureren.
* Back-up en herstellen van gegevens.
* Prestaties controleren.
* Systeeminstellingen bekijken en mogelijke problemen te identificeren.

De service Manager voor StorSimple-apparaat kunt u alle beheerderstaken, behalve de waarvoor system uitvaltijd, zoals de eerste configuratie en installatie van updates.

Ga voor meer informatie naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell voor StorSimple
Windows PowerShell voor StorSimple biedt een opdrachtregelinterface die u kunt maken en beheren van de Microsoft Azure StorSimple-service en instellen en controleren van de StorSimple-apparaten. Het is een Windows PowerShell gebaseerde opdrachtregelinterface die specifieke cmdlets voor het beheren van uw StorSimple-apparaat bevat. Windows PowerShell voor StorSimple bevat functies waarmee u kunt:

* Een apparaat registreren.
* Configureer de netwerkinterface op een apparaat.
* Bepaalde typen updates installeren.
* Los problemen met uw apparaat met het openen van de ondersteuningssessie.
* Wijzig de apparaatstatus.

U kunt toegang tot Windows PowerShell voor StorSimple van een seriële console (op een hostcomputer rechtstreeks verbonden met het apparaat) of op afstand met behulp van Windows PowerShell op afstand. Houd er rekening mee dat sommige Windows PowerShell voor StorSimple taken, zoals het eerste apparaatregistratie, kan alleen worden uitgevoerd op de seriële console.

Ga voor meer informatie naar [gebruik Windows PowerShell voor StorSimple voor het beheren van uw apparaat](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-cmdlets
De StorSimple van Azure PowerShell-cmdlets zijn een verzameling van Windows PowerShell-cmdlets waarmee u kunt het automatiseren van taken van het serviceniveau en migratie vanaf de opdrachtregel. Voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple, gaat u naar de [cmdlet-verwijzing](/powershell/module/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module maken consistent is, kunt u back-ups van de punt in tijd van lokale en cloudgegevens. De module wordt uitgevoerd op een host op basis van Windows Server. U kunt StorSimple Snapshot Manager te gebruiken:

* Configureer back-up en volumes verwijderen.
* Configureer volume groepen om ervoor te zorgen dat de back-upgegevens toepassingsconsistente is.
* Back-upbeleid beheren, zodat gegevens een back-up op een vooraf vastgestelde planning en in een bepaalde locatie opgeslagen (lokaal of in de cloud).
* Volumes en afzonderlijke bestanden terugzetten.

Back-ups worden vastgelegd als momentopnamen die alleen de wijzigingen vastleggen sinds de laatste momentopname is gemaakt en veel minder opslagruimte dan volledige back-ups vereisen. U kunt maken van back-upschema's of directe back-ups uitvoeren indien nodig. Bovendien kunt u StorSimple Snapshot Manager tot stand brengen bewaarbeleid instellen die bepalen hoeveel momentopnamen worden opgeslagen. Als u later nodig hebt om gegevens te herstellen vanaf een back-up, StorSimple Snapshot Manager kunt u vanuit de catalogus van de lokale of cloudmomentopnamen. 

Als er een ramp optreedt of als u nodig hebt om gegevens te herstellen voor een andere reden, StorSimple Snapshot Manager herstellen incrementeel nodig is. Het herstel van gegevens is niet vereist dat u het volledige systeem afsluiten terwijl u een bestand te herstellen, apparatuur vervangen of taken worden verplaatst naar een andere site.

Ga voor meer informatie naar [wat is er StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter voor SharePoint
Microsoft Azure StorSimple omvat de StorSimple-Adapter voor SharePoint, een optioneel onderdeel dat transparant StorSimple opslag- en beveiligingsfuncties voor SharePoint server-farms breidt. De adapter werkt met een externe Blob Storage planning provider en de functie SQL Server Resourcestructuur zodat u BLOBs verplaatsen naar een server die back-up gemaakt door de Microsoft Azure StorSimple-systeem. Microsoft Azure StorSimple slaat vervolgens de BLOB-gegevens lokaal of in de cloud, op basis van gebruik.

De StorSimple-Adapter voor SharePoint wordt beheerd vanuit de portal SharePoint Centraal beheer. Als gevolg daarvan kan SharePoint management blijft gecentraliseerd en alle opslag lijkt te zijn in de SharePoint-farm.

Ga voor meer informatie naar [StorSimple-Adapter voor SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Beheer van opslagtechnologieën
Naast de toegewezen StorSimple-apparaat, een virtueel apparaat en andere onderdelen, Microsoft Azure StorSimple gebruikt de volgende softwaretechnologieën voor snelle toegang tot gegevens en aan opslagruimte te verminderen:

* [Automatische opslaglagen](#automatic-storage-tiering) 
* [Thin provisioning](#thin-provisioning) 
* [Gegevensontdubbeling en compressie](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatische opslaglagen
Microsoft Azure StorSimple rangschikt automatisch gegevens in logische categorieën op basis van het huidige gebruik, leeftijd en relatie met andere gegevens. Gegevens die het meest actief is is lokaal opgeslagen, terwijl minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. Het volgende diagram ziet u deze benadering van gegevensopslag.

![Opslaglagen StorSimple](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Voor snelle toegang, slaat StorSimple zeer actieve gegevens (hot gegevens) op SSD's in het StorSimple-apparaat. Gegevens die wordt gebruikt van tijd tot tijd worden opgeslagen (oefenen gegevens) op HDD's in het apparaat of op de servers in het datacenter. Inactieve gegevens, back-upgegevens worden verplaatst en blijven gegevens bewaard archivering of naleving doeleinden naar de cloud. 

> [!NOTE]
> In de Update 2 of hoger, kunt u een als lokaal vastgemaakt volume opgeven, in welk geval de gegevens op het lokale apparaat blijft en geen is gelaagd naar de cloud. 


StorSimple aanpast en gegevens opnieuw worden ingedeeld en toewijzingen van de opslag als gebruikspatronen wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actief gedurende een bepaalde periode. Als deze actief geleidelijk minder, is deze gemigreerd van SSD's naar HDD's en vervolgens naar de cloud. Als die dezelfde gegevens opnieuw geactiveerd wordt, wordt deze is gemigreerd naar het opslagapparaat.

De opslagruimte lagen proces verloopt als volgt:

1. Een systeembeheerder stelt u een opslagaccount van de Microsoft Azure-cloud.
2. De beheerder gebruikt de seriële console en de Apparaatbeheer StorSimple-service (actief in Azure portal) voor het configureren van de server apparaat en de bestandsnaam, volumes en gegevens protection-beleid maken. De Internet Small Computer System Interface (iSCSI) op lokale computers (zoals bestandsservers) gebruiken voor toegang tot het StorSimple-apparaat.
3. In eerste instantie opslaat StorSimple-gegevens in de snelle SSD-laag van het apparaat.
4. Als de SSD-laag capaciteit nadert, StorSimple deduplicates de oudste gegevensblokken worden gecomprimeerd en naar de HDD-laag verplaatst.
5. Als de capaciteit van HDD-laag benaderingen, StorSimple versleutelt de oudste gegevensblokken en zendt deze veilig naar de Microsoft Azure storage-account via HTTPS.
6. Microsoft Azure maakt meerdere replica's van de gegevens in het datacenter en op een externe datacenter, waarbij u ervoor zorgt dat de gegevens kunnen worden hersteld als een noodsituatie voordoet.
7. De bestandsserver aanvraagt gegevens die zijn opgeslagen in de cloud, StorSimple naadloos retourneert als een kopie opgeslagen op de SSD-laag van het StorSimple-apparaat.

#### <a name="how-storsimple-manages-cloud-data"></a>Hoe StorSimple cloudgegevens beheert

StorSimple deduplicates klantgegevens over alle momentopnamen en de primaire gegevens (gegevens geschreven door hosts). Ontdubbeling is ideaal voor de opslagruimte, maakt de kwestie van het 'Wat is in de cloud' ingewikkeld. De gelaagde primaire gegevens en de momentopname overlappen met elkaar. Een hoeveelheid gegevens in de cloud in één kan worden gebruikt als gelaagde primaire gegevens, en ook naar worden verwezen door meerdere momentopnamen. Elke cloudmomentopname zorgt ervoor dat er een kopie van alle gegevens van de punt in tijd in de cloud is vergrendeld totdat het die momentopname wordt verwijderd.

Gegevens worden alleen verwijderd uit de cloud wanneer er geen verwijzingen naar die gegevens. Bijvoorbeeld, als er een cloudmomentopname van alle gegevens die in het StorSimple-apparaat en verwijder vervolgens de primaire gegevens, zou zien we de _primaire gegevens_ onmiddellijk te verwijderen. De _cloudgegevens_ waaronder de gelaagde gegevens en de back-ups, blijft hetzelfde. Dit is omdat er een momentopname nog verwijzen naar de cloudgegevens. Nadat de cloud momentopname is verwijderd (en eventuele andere momentopname die verwijst naar dezelfde gegevens), cloud verbruik verwijdert. Voordat we cloudgegevens verwijdert, wordt er gecontroleerd of er zijn geen momentopnamen nog steeds verwijzen naar die gegevens. Dit proces wordt genoemd _garbagecollection_ en een achtergrondservice wordt uitgevoerd op het apparaat. Verwijderen van cloudgegevens is niet direct omdat de verzameling afval service op andere verwijzingen naar die gegevens vóór de verwijdering controleert. De snelheid van de garbage collector is afhankelijk van het totale aantal momentopnamen en de totale gegevens. De cloudgegevens wordt normaal gesproken in minder dan een week opgeschoond.


### <a name="thin-provisioning"></a>Thin provisioning
Thin provisioning is een virtualisatietechnologie voor het waarin beschikbare opslag zijn dan de fysieke resources wordt weergegeven. In plaats van voldoende opslagruimte van tevoren reserveren, StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. De elastische aard van cloud-opslag vergemakkelijkt deze benadering omdat StorSimple kunt vergroten of verkleinen van cloud-opslag om te voldoen aan veranderende verzoeken.

> [!NOTE]
> Lokaal vastgemaakte volumes zijn niet dun ingericht. Wanneer het volume wordt gemaakt, wordt de opslag die is toegewezen aan een lokale alleen-lezen volume ingericht in zijn geheel.


### <a name="deduplication-and-compression"></a>Gegevensontdubbeling en compressie
Microsoft Azure StorSimple gebruikt Ontdubbeling en gegevens compressie om opslagvereisten te verminderen.

Ontdubbeling reduceert de totale hoeveelheid gegevens die zijn opgeslagen doordat redundantie in de opgeslagen gegevensset. Als gegevens worden gewijzigd, StorSimple ongewijzigde gegevens worden genegeerd en wordt alleen de wijzigingen vastgelegd. StorSimple vermindert bovendien de hoeveelheid opgeslagen gegevens door het identificeren en onnodige gegevens verwijderen. 

> [!NOTE]
> Gegevens op lokaal vastgemaakte volumes is niet ontdubbeld of gecomprimeerd. Back-ups van lokaal vastgemaakte volumes zijn echter ontdubbeld en gecomprimeerd.


## <a name="storsimple-workload-summary"></a>StorSimple workloadoverzicht
Hieronder wordt een samenvatting van de ondersteunde StorSimple werkbelastingen in een tabel.

| Scenario | Workload | Ondersteund | Beperkingen | Versie |
| --- | --- | --- | --- | --- |
| Samenwerking |Delen van bestanden |Ja | |Alle versies |
| Samenwerking |Gedistribueerde bestanden delen |Ja | |Alle versies |
| Samenwerking |SharePoint |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Archivering |Eenvoudige bestand archiveren |Ja | |Alle versies |
| Virtualisatie |Virtuele machines |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Database |SQL |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Update 2 en hoger |
| Videobewaking |Videobewaking |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze werkbelasting is toegewezen |Update 2 en hoger |
| Back-up |Back-up van primaire doel |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze werkbelasting is toegewezen |Update 3 en hoger |
| Back-up |Back-up van secundaire doel |Ja* |Ondersteund bij het StorSimple-apparaat alleen aan deze werkbelasting is toegewezen |Update 3 en hoger |

*Ja &#42; -Oplossing richtlijnen en beperkingen worden toegepast.*

De volgende werkbelastingen worden niet ondersteund door apparaten StorSimple 8000-serie. Als op de StorSimple wordt geïmplementeerd, wordt deze werkbelastingen leidt tot een niet-ondersteunde configuratie.

* Medische imaging
* Exchange
* VDI
* Oracle
* SAP
* Big data
* Distributie van inhoud
* Opstarten vanaf een SCSI

Hier volgt een lijst met onderdelen van de infrastructuur StorSimple ondersteund.

| Scenario | Workload | Ondersteund | Beperkingen | Versie |
| --- | --- | --- | --- | --- |
| Algemeen |ExpressRoute |Ja | |Alle versies |
| Algemeen |DataCore FC |Ja* |Ondersteund met DataCore SANsymphony |Alle versies |
| Algemeen |DFSR |Ja* |Alleen ondersteund met lokaal vastgemaakte volumes |Alle versies |
| Algemeen |Indexeren |Ja* |Voor gelaagde volumes alleen metagegevens indexeren wordt ondersteund (geen gegevens).<br>Voor lokaal vastgemaakte volumes wordt volledige indexering ondersteund. |Alle versies |
| Algemeen |Antivirusprogramma 's |Ja* |Voor gelaagde volumes wordt alleen scan Open en close ondersteund.<br> Voor lokaal vastgemaakte volumes, wordt volledige scan ondersteund. |Alle versies |

*Ja &#42; -Oplossing richtlijnen en beperkingen worden toegepast.*

Hier volgt een lijst met andere software die worden gebruikt met StorSimple om oplossingen te maken.

| Type werkbelasting | Software die worden gebruikt met StorSimple | Ondersteunde versies|Handleiding koppelen| 
| --- | --- | --- | --- |
| Back-updoel |Veeam |Veeam v 9 en hoger |[StorSimple als een back-updoel met Veaam](storsimple-configure-backup-target-veeam.md)|
| Back-updoel |Veritas Backup Exec |Back-up uitvoeren / 16 en hoger |[StorSimple als een back-updoel met Backup Exec](storsimple-configure-backup-target-using-backup-exec.md)|
| Back-updoel |VERITAS NetBackup |NetBackup 7.7.x en hoger  |[StorSimple als een back-updoel met NetBackup](storsimple-configure-backuptarget-netbackup.md)|
| Globale bestanden delen <br></br> Samenwerking |Talon  |[StorSimple met Talon](https://www.talonstorage.com/products/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorSimple-terminologie
Voordat u uw Microsoft Azure StorSimple-oplossing implementeert, wordt u aangeraden de volgende termen en definities te controleren.

### <a name="key-terms-and-definitions"></a>Belangrijkste termen en definities
| Term (acroniem of afkorting) | Beschrijving |
| --- | --- |
| record voor toegangscontrole (ACR) |Een record die is gekoppeld aan een volume op uw Microsoft Azure StorSimple-apparaat waarmee wordt bepaald welke hosts verbinding mee kunnen maken. De bepaling is gebaseerd op de iSCSI Qualified Name (IQN) van de hosts (opgenomen in de ACR) die zijn verbonden met uw StorSimple-apparaat. |
| AES-256 |Een algoritme van de Advanced Encryption Standard (AES) 256-bits voor het versleutelen van gegevens worden verplaatst naar en van de cloud. |
| grootte van toewijzingseenheid (AUS) |De kleinste hoeveelheid schijfruimte die kan worden toegewezen aan een bestand in uw Windows-bestandssystemen. Als de bestandsgrootte van een niet een veelvoud van de clustergrootte is, extra ruimte moet worden gebruikt voor het opslaan van het bestand (tot het volgende veelvoud van de clustergrootte) waardoor verloren ruimte en fragmentatie van de harde schijf. <br>De aanbevolen AUS voor Azure StorSimple-volumes is 64 KB omdat deze goed samen met de ontdubbelingsalgoritmen die worden werkt. |
| geautomatiseerde opslaglagen |Automatisch minder actieve gegevens verplaatsen van SSD's naar HDD's en vervolgens naar een laag in de cloud en vervolgens het inschakelen van beheer van alle opslag van een centrale interface. |
| Back-upcatalogus |Een verzameling van back-ups, meestal gekoppeld door het toepassingstype dat is gebruikt. Deze verzameling wordt weergegeven in de blade back-up-catalogus van de service Manager voor StorSimple-apparaat gebruikersinterface. |
| back-catalogusbestand |Een bestand met een lijst met beschikbare momentopnamen die momenteel zijn opgeslagen in de back-up van StorSimple Snapshot Manager-database. |
| back-upbeleid |Een selectie van volumes, type back-up en een tijdschema waarmee u back-ups maken op een vooraf gedefinieerd schema. |
| binaire grote objecten (BLOB's) |Een verzameling van binaire gegevens die zijn opgeslagen als één entiteit in een databasebeheersysteem. BLOBs zijn meestal afbeeldingen, audio of andere multimedia objecten, hoewel soms binaire uitvoerbare code wordt opgeslagen als een BLOB. |
| Challenge Handshake Authentication Protocol (CHAP) |Een protocol dat wordt gebruikt voor het verifiëren van de peer van een verbinding, op basis van de peer-delen van een wachtwoord of geheim. CHAP kan eenzijdige of wederzijdse zijn. Met één richting CHAP verifieert het doel een initiator. Wederzijdse CHAP vereist dat het doel de initiator geverifieerd en dat de initiator het doel moeten worden geverifieerd. |
| kloon |Een kopie van een volume. |
| Cloud als een laag (CaaT) |Cloudopslag als een laag binnen de opslagarchitectuur worden geïntegreerd, zodat alle opslag moet deel uitmaken van een onderneming opslagnetwerk weergegeven. |
| cloud serviceprovider (CSP) |Een provider van services voor cloudcomputing. |
| cloudmomentopname |Een kopie van de punt in tijd van de volumegegevens die zijn opgeslagen in de cloud. Een cloudmomentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn bijzonder nuttig voor herstel na noodgevallen. |
| coderingssleutel voor cloudopslag |Een wachtwoord of een sleutel die wordt gebruikt door uw StorSimple-apparaat voor toegang tot de versleutelde gegevens verzonden door uw apparaat naar de cloud. |
| clusterbewust bijwerken |Het beheren van software-updates op servers in een failovercluster, zodat de updates minimale hebben of geen effect op de beschikbaarheid van de service. |
| gegevenspad |Een verzameling functionele eenheden die onderling verbonden gegevensverwerking bewerkingen uitvoeren. |
| deactiveren |Een permanente actie die de verbinding tussen het StorSimple-apparaat en de gekoppelde cloud-service wordt verbroken. Cloudmomentopnamen van het apparaat kunnen blijven na dit proces en worden gekloond of gebruikt voor herstel na noodgevallen. |
| het spiegelen van de schijf |Replicatie van de logische schijfvolumes op afzonderlijke vaste schijven in realtime bijgewerkt om voortdurende beschikbaarheid te garanderen. |
| dynamische schijf spiegelen |Replicatie van de logische schijfvolumes op dynamische schijven. |
| dynamische schijven |Een volume-schijfindeling die gebruikmaakt van de Logical Disk Manager (LDM) opslaan en beheren van gegevens over meerdere fysieke schijven. Dynamische schijven kunnen worden vergroot meer ruimte vrij te geven. |
| Uitgebreide Bunch van schijven (EBOD) behuizing |Een secundaire behuizing van uw Microsoft Azure StorSimple-apparaat met extra harde schijf schijven voor extra opslagruimte. |
| FAT-inrichting |Een conventionele opslaginrichting in welke opslag wordt ruimte toegewezen op basis van behoeften verwacht (en meestal valt buiten het huidige behoefte). Zie ook *thin provisioning,*. |
| harde schijf (HDD) |Een station dat gebruikmaakt van draaiende platen voor het opslaan van gegevens. |
| hybride-cloudopslag |Een opslagarchitectuur die gebruikmaakt van lokale en externe bronnen, met inbegrip van cloud-opslag. |
| Internet Small Computer System Interface (iSCSI) |Een opslag op basis van Internet Protocol-IP-netwerken standaard voor het koppelen van de gegevens opslagapparatuur of faciliteiten. |
| iSCSI-initiator |Een softwareonderdeel waarmee een hostcomputer met Windows verbinding maken met een externe opslag op basis van iSCSI-netwerk. |
| iSCSI Qualified Name (IQN) |Een unieke naam die een iSCSI-doel of de initiator aangeeft. |
| iSCSI-doel |Een softwareonderdeel dat gecentraliseerd iSCSI schijfsubsystemen in een storage area network biedt. |
| Live archiveren |Een benadering van gegevensopslag waarmee archivering van gegevens toegankelijk voortdurend is (deze wordt niet opgeslagen op een externe locatie op tape, bijvoorbeeld). Microsoft Azure StorSimple maakt gebruik van live archiveren. |
| lokaal vastgemaakt volume |een volume dat zich bevindt op het apparaat en nooit naar de cloud is gelaagd. |
| lokale momentopname |Een kopie van de punt in tijd van de volumegegevens die zijn opgeslagen op de Microsoft Azure StorSimple-apparaat. |
| Microsoft Azure StorSimple |Een krachtige oplossing die bestaan uit een opslagapparaat datacenter- en software waarmee IT-organisaties gebruikmaken van cloud-opslag alsof het datacenter-opslag. StorSimple vereenvoudigt de bescherming van gegevens en het beheer van gegevens tijdens de kosten te verlagen. De oplossing consolideert primaire opslag, archiveren, back-up, en herstel na noodgevallen (DR) via naadloze integratie met de cloud. StorSimple-apparaten inschakelen door een combinatie van SAN-opslag- en cloud gegevensbeheer op een platform bedrijfsniveau snelheid, eenvoud en betrouwbaarheid voor alle opslag-gerelateerde behoeften. |
| Stroom en koeling van Module (PCM) |Hardware-onderdelen van uw StorSimple-apparaat dat bestaat uit de voedingen en de ventilator, daarom de naam van de stroom en koeling van de module. De primaire behuizing van het apparaat heeft twee 764W PCMs terwijl de behuizing EBOD twee 580 w bij PCMs heeft. |
| Primaire behuizing |Belangrijkste behuizing van uw StorSimple-apparaat dat de toepassing platform-controllers bevat. |
| Beoogde hersteltijd (RTO) |De maximale hoeveelheid tijd die voordat een bedrijfsproces of het systeem opgebruikt moet is volledig hersteld na een noodgeval. |
| seriële gekoppelde SCSI (SAS) |Een type harde schijf (HDD). |
| gegevensversleutelingssleutel van service |Een sleutel beschikbaar gesteld aan een nieuwe StorSimple-apparaat dat wordt geregistreerd bij de service Manager voor StorSimple-apparaat. De configuratiegegevens overgedragen tussen de service Manager voor StorSimple-apparaat en het apparaat is versleuteld met een openbare sleutel en vervolgens alleen op het apparaat met een persoonlijke sleutel kan worden ontsleuteld. Gegevensversleutelingssleutel van service kan de service voor deze persoonlijke sleutel voor ontsleuteling. |
| serviceregistratiesleutel |Een sleutel die helpt het StorSimple-apparaat registreren bij de Apparaatbeheer StorSimple-service zodat deze wordt weergegeven in de Azure portal voor verdere acties. |
| Small Computer System Interface (SCSI) |Een verzameling standaarden voor fysiek verbinden van computers en het doorgeven van gegevens. |
| Solid state schijf (SSD) |Een schijf die geen bewegende onderdelen bevat; bijvoorbeeld: een flash-station. |
| Storage-account |Een set referenties voor toegang is gekoppeld aan uw storage-account voor een bepaalde cloud serviceprovider. |
| StorSimple Adapter voor SharePoint |Een Microsoft Azure StorSimple-component die transparant StorSimple opslag en gegevensbeveiliging voor SharePoint server-farms breidt. |
| StorSimple-apparaat Manager-service |Een uitbreiding van de Azure portal die u kunt uw Azure StorSimple on-premises en virtuele apparaten beheren. |
| StorSimple Snapshot Manager |Een Microsoft Management Console (MMC)-module voor het beheren van back-up en herstellen van bewerkingen in Microsoft Azure StorSimple. |
| back-up maken |Een functie waarmee de gebruiker een interactieve back-up maken van een volume. Het is een andere manier een handmatige back-up van een volume in plaats van een automatische back-up via een gedefinieerd beleid duurt te nemen. |
| Thin provisioning |Een methode voor het optimaliseren van de efficiëntie waarmee de beschikbare opslagruimte wordt gebruikt in opslagsystemen. De opslag is in dunne inrichting wordt verdeeld over meerdere gebruikers op basis van de minimaal vereiste door elke gebruiker op elk moment schijfruimte. Zie ook *fat inrichting*. |
| lagen |Gegevens in logische groepen op basis van het huidige gebruik, leeftijd en relatie met andere gegevens rangschikken. StorSimple rangschikt automatisch gegevens in categorieën. |
| Volume |Logische opslagplaatsen aangeboden in de vorm van stations. StorSimple-volumes overeenkomen met de volumes die zijn gekoppeld met de host, met inbegrip van die door het gebruik van iSCSI- en een StorSimple-apparaat wordt gedetecteerd. |
| volumecontainer |Een groepering van volumes en de instellingen die op hen van toepassing. Alle volumes in uw StorSimple-apparaat zijn gegroepeerd in volumecontainers. Instellingen voor volume-container omvatten storage-accounts, versleutelingsinstellingen voor gegevens die worden verzonden naar de cloud met bijbehorende coderingssleutels en bandbreedte voor bewerkingen met betrekking tot de cloud. |
| volume-groep |In StorSimple Snapshot Manager een volume-groep is een verzameling van volumes die zijn geconfigureerd voor het faciliteren van een back-up. |
| Volume Shadow Copy Service (VSS) |Een service van de Windows Server-besturingssysteem die zorgt voor een toepassing consistente door te communiceren met VSS-compatibele toepassingen voor het coördineren van het maken van incrementele momentopnamen. VSS zorgt ervoor dat de toepassingen tijdelijk niet actief zijn wanneer momentopnamen worden gemaakt. |
| Windows PowerShell voor StorSimple |Een Windows PowerShell gebaseerde opdrachtregelinterface gebruikt om te werken en uw StorSimple-apparaat te beheren. Deze interface heeft terwijl sommige van de basismogelijkheden van Windows PowerShell andere speciale cmdlets die zijn gericht op het beheren van een StorSimple-apparaat. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [StorSimple security](storsimple-8000-security.md).


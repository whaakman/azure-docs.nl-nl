---
title: Aanbevolen procedures voor StorSimple Virtual Array | Microsoft Docs
description: Beschrijft de aanbevolen procedures voor het implementeren en beheren van de StorSimple Virtual Array.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2018
ms.author: alkohli
ms.openlocfilehash: b8e9f12a549f71971c2da3b9865f6a74dad58f61
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300911"
---
# <a name="storsimple-virtual-array-best-practices"></a>Aanbevolen procedures voor StorSimple Virtual Array
## <a name="overview"></a>Overzicht
Microsoft Azure StorSimple Virtual Array is een geïntegreerde opslagaccountoplossing waarmee opslagtaken tussen een on-premises virtuele apparaat uitgevoerd in een hypervisor en Microsoft Azure-cloudopslag. StorSimple Virtual Array is een efficiënte en voordelige alternatief voor de fysieke 8000-serie-matrix. De virtuele matrix kan worden uitgevoerd op uw bestaande infrastructuur van de hypervisor, biedt ondersteuning voor de iSCSI- en de SMB-protocollen en is zeer geschikt voor scenario's voor externe kantoren/filialen office. Voor meer informatie over de StorSimple-oplossingen, gaat u naar [overzicht van Microsoft Azure StorSimple](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

In dit artikel bevat informatie over de best practices geïmplementeerd tijdens de eerste configuratie, implementatie en beheer van de StorSimple Virtual Array. Deze aanbevolen procedures bieden gevalideerde richtlijnen voor het instellen en beheren van uw virtuele matrix. In dit artikel is gericht op de IT-beheerders die implementeren en beheren van de virtuele matrices in hun datacenters.

U wordt aangeraden een periodieke controle van de aanbevolen procedures om te zorgen voor dat uw apparaat is nog steeds in acht genomen wanneer wijzigingen worden aangebracht in de installatie- of operationele stroom. Moet u eventuele problemen optreden tijdens het implementeren van deze aanbevolen procedures op uw virtuele array [Neem contact op met Microsoft Support](storsimple-virtual-array-log-support-ticket.md) voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie
Deze aanbevolen procedures hebben betrekking op de richtlijnen die moeten worden gevolgd tijdens de initiële installatie en implementatie van de virtuele matrices. Deze aanbevolen procedures bevatten die zijn gerelateerd aan het inrichten van de virtuele machine, instellingen voor Groepsbeleid, grootte, instellen van het netwerk, storage-accounts configureren en het maken van bestandsshares en volumes voor de virtuele matrix. 

### <a name="provisioning"></a>Inrichten
StorSimple Virtual Array is een virtuele machine (VM) die is ingericht op de hypervisor (Hyper-V of VMware) van de hostserver. Zorg ervoor dat de host kan besteden aan het voldoende bronnen zijn bij het inrichten van de virtuele machine. Ga voor meer informatie naar [minimale resourcevereisten](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) voor het inrichten van een matrix.

Implementeer de volgende aanbevolen procedures bij het inrichten van de virtuele matrix:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Type virtuele machine** |**Generatie 2** VM voor gebruik met Windows Server 2012 of hoger en een *.vhdx* installatiekopie. <br></br> **Generatie 1** VM voor gebruik met een Windows Server 2008 of hoger en een *.vhd* installatiekopie. |Gebruik virtuele machine versie 8 bij het gebruik van *.vmdk* installatiekopie. |
| **Geheugentype** |Configureren als **statisch geheugen**. <br></br> Gebruik niet de **dynamisch geheugen** optie. | |
| **Schijf-gegevenstype** |Inrichten als **dynamisch uitbreidbare**.<br></br> **Vaste grootte** lang duurt. <br></br> Gebruik niet de **differentiërende** optie. |Gebruik de **dunne inrichting** optie. |
| **Wijziging van de gegevens-schijf** |Breiden of te verkleinen is niet toegestaan. Een poging om u te doen, leidt tot het verlies van gegevens van alle lokale gegevens op het apparaat. |Breiden of te verkleinen is niet toegestaan. Een poging om u te doen, leidt tot het verlies van gegevens van alle lokale gegevens op het apparaat. |

### <a name="sizing"></a>Grootte aanpassen
Wanneer de grootte van uw StorSimple Virtual Array, houd rekening met de volgende factoren:

* Lokale reservering voor volumes of shares. Ongeveer 12% van de ruimte wordt gereserveerd op de lokale laag voor elke betreffende gelaagde volume of share. 10% van de ruimte is ongeveer ook gereserveerd voor een lokaal vastgemaakt volume voor file system.
* Snapshot-overhead. Ongeveer is 15% ruimte op de lokale laag gereserveerd voor momentopnamen.
* U hoeft voor herstelbewerkingen. Als herstel als een nieuwe bewerking uitvoert, moet sizing account voor de ruimte die nodig zijn om te herstellen. Terugzetten is uitgevoerd op een share of het volume van dezelfde grootte.
* Sommige buffer moet worden toegewezen voor eventuele onverwachte groei.

Op basis van de voorgaande factoren, worden de vereisten voor volumegrootte vertegenwoordigd door de volgende vergelijking:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

De volgende voorbeelden laten zien hoe u kunt het formaat van een virtuele matrix op basis van uw vereisten.

#### <a name="example-1"></a>Voorbeeld 1:
Op uw virtuele array die u wilt mogelijk

* richt een 2 TB gelaagd volume of share.
* richt een gelaagd volume 1 TB of de share.
* richt een 300 GB van lokaal vastgemaakte volume of share.

Voor de voorgaande volumes of shares, de vrije ruimte nodig op de lokale laag om ons hierover te berekenen.

Eerst, voor elke gelaagde volume/delen, lokale reservering zou gelijk zijn aan 12% van de grootte van de volume/bestandsshare. Lokale reservering is voor het lokaal vastgemaakte volume/delen, 10% van de grootte van de lokaal vastgemaakte volume/bestandsshare (naast de ingerichte grootte). In dit voorbeeld moet u

* Lokale reservering 240 GB (voor een share/2 TB gelaagde volume)
* Lokale reservering 120 GB (voor een share/1 TB gelaagde volume)
* 330-GB voor lokaal vastgemaakte volume of share (10% van de lokale reservering toevoegen aan de grootte 300 GB ingericht)

De totale ruimte vereist op de lokale laag tot nu toe is: 240 GB + 120 GB + 330 GB = 690 GB.

Ten tweede moet ten minste net zoveel ruimte op de lokale laag als de grootste één reservering. Deze extra hoeveelheid die wordt gebruikt als u wilt herstellen vanuit een cloudmomentopname. In dit voorbeeld is de grootste lokale reservering 330 GB (met inbegrip van de reservering voor file system), zodat u kunt het beste toevoegen dat naar de 690 GB: 690 GB + 330 GB = 1020 GB.
Als we herstelt de volgende aanvullende uitgevoerd, kunnen we altijd de ruimte van de vorige herstelbewerking vrij.

Derde, moeten we 15% van uw totale lokale ruimte tot nu toe voor het opslaan van lokale momentopnamen, zodat alleen 85% van het beschikbaar is. In dit voorbeeld, dat zou zijn 1020 GB = 0.85&ast;ingerichte gegevensschijf TB. Dus de ingerichte gegevensschijf zou zijn (1020&ast;(1/0.85)) 1200 GB = 1,20 TB = ~ 1,25 TB (afronden naar de dichtstbijzijnde kwartiel)

Waarbij u rekening houdt onverwachte groei en de nieuwe herstelt, moet u een lokale schijf van rond inrichten 1,25-1,5 TB.

> [!NOTE]
> We raden u ook aan de lokale schijf is dun ingericht. Deze aanbeveling is omdat de restore-ruimte is alleen nodig als u wilt herstellen van gegevens die ouder zijn dan vijf dagen. Herstel op itemniveau kunt u gegevens herstellen voor de afgelopen vijf dagen zonder de extra ruimte om te herstellen.


#### <a name="example-2"></a>Voorbeeld 2:
Op uw virtuele array die u wilt mogelijk

* inrichten van een gelaagd volume 2 TB
* inrichten van een lokaal vastgemaakt volume van 300 GB

Op basis van 12% van de lokale ruimte gereserveerd gelaagde volumes/shares en 10% voor lokaal vastgemaakte volumes/shares, moeten we

* Lokale reservering 240 GB (gelaagd volume of andere share voor 2 TB)
* 330-GB voor lokaal vastgemaakte volume of share (10% van de lokale reservering toevoegen aan de ruimte 300 GB ingericht)

De totale ruimte vereist op de lokale laag is: 240 GB + 330 GB = 570 GB

De minimale lokale ruimte die nodig zijn voor de terugzetbewerking is 330 GB.

15% van de totale schijf wordt gebruikt voor het opslaan van momentopnamen, zodat alleen 0.85 beschikbaar is. De schijfgrootte is dus (900&ast;(1/0.85)) 1,06 TB = ~ 1,25 TB (afronden naar de dichtstbijzijnde kwartiel)

Waarbij u rekening houdt een eventuele onverwachte groei, kunt u een lokale schijf 1,25-1,5 TB inrichten.

### <a name="group-policy"></a>Groepsbeleid
Groepsbeleid is een infrastructuur waarmee u specifieke configuraties voor gebruikers en computers te implementeren. Instellingen voor Groepsbeleid zijn opgenomen in groepsbeleidsobjecten (GPO's), die zijn gekoppeld aan de volgende Active Directory Domain Services (AD DS)-containers: sites, domeinen of organisatie-eenheden (OU's). 

Als uw virtuele array domein is, kunnen groepsbeleidsobjecten worden toegepast op deze. Deze groepsbeleidsobjecten kunnen toepassingen, zoals de antivirussoftware dit negatieve voor de werking van de StorSimple Virtual Array gevolgen kan installeren.

Daarom raden wij aan dat u:

* Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory.
* Zorg ervoor dat er geen groepsbeleidsobjecten (GPO's) worden toegepast op uw virtuele matrix. U kunt overnemen om ervoor te zorgen dat de virtuele matrix (onderliggend knooppunt) niet automatisch geen GPO's van de bovenliggende overgenomen is blokkeren. Ga voor meer informatie naar [overname blokkeren](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Netwerken
De netwerkconfiguratie voor uw virtuele array wordt gedaan door de lokale webgebruikersinterface. Een virtuele netwerkinterface wordt ingeschakeld via de hypervisor waarin de virtuele matrix is ingericht. Gebruik de [netwerkinstellingen](storsimple-virtual-array-deploy3-fs-setup.md) pagina voor het configureren van het virtuele netwerkinterface IP-adres, het subnet en de gateway.  U kunt ook de primaire en secundaire DNS-server, de instellingen voor tijd en de optionele proxy-instellingen configureren voor uw apparaat. De meeste van de configuratie van het netwerk is een eenmalige installatie. Controleer de [StorSimple netwerkvereisten](storsimple-ova-system-requirements.md#networking-requirements) vóór de implementatie van de virtuele matrix.

Bij het implementeren van uw virtuele array wordt u aangeraden dat u deze aanbevolen procedures volgt:

* Zorg ervoor dat het netwerk waarin de virtuele matrix altijd is geïmplementeerd de capaciteit te reserveren voor 5 Mbps bandbreedte met Internet (of meer).
  
  * Internet bandbreedte nodig zijn, is afhankelijk van de kenmerken van de werkbelasting en de wijzigingssnelheid van gegevens.
  * De gegevenswijziging die kan worden verwerkt is rechtstreeks in verhouding met uw internetbandbreedte. Als u bijvoorbeeld bij het nemen van een back-up aankan een 5 Mbps bandbreedte een gegevenswijziging van ongeveer 18 GB in 8 uur. Met vier keer meer bandbreedte (Mbps 20), kunt u vier keer meer gegevens wijzigen (72 GB) verwerken.
* Zorg ervoor dat de verbinding met Internet is altijd beschikbaar. Er kunnen sporadisch of onbetrouwbare Internet-verbindingen met de apparaten kunnen leiden tot verlies van toegang tot gegevens in de cloud en kunnen leiden tot een niet-ondersteunde configuratie.
* Als u van plan bent uw apparaat als een iSCSI-server implementeren:
  
  * Het is raadzaam dat u CredSSP de **IP-adres automatisch ophalen** optie (DHCP).
  * Statische IP-adressen configureren. U moet een primaire en secundaire DNS-server configureren.
  * Als u meerdere netwerkinterfaces definieert op uw virtuele matrix, alleen de eerste netwerkinterface (deze interface is standaard **Ethernet**) de cloud kunt bereiken. Voor het beheren van het type verkeer, kunt u meerdere virtuele netwerkinterfaces maken op uw virtuele array (geconfigureerd als een iSCSI-server) en deze interfaces verbinden met verschillende subnetten.
* Alleen de cloud-bandbreedte beperken (gebruikt door de virtuele matrix), beperkingen op de router of de firewall configureren. Als u de beperkingen in de hypervisor definieert, wordt het beperken van alle protocollen, waaronder iSCSI- en SMB in plaats van alleen de bandbreedte van de cloud.
* Zorg ervoor dat tijdsynchronisatie voor hypervisors is ingeschakeld. Als u Hyper-V gebruikt, selecteert u uw virtuele array in de Hyper-V-beheer, gaat u naar **instellingen &gt; Integration Services**, en zorg ervoor dat de **tijdsynchronisatie** is ingeschakeld.

### <a name="storage-accounts"></a>Opslagaccounts
StorSimple Virtual Array kan worden gekoppeld aan één opslagaccount. Dit opslagaccount kan een automatisch gegenereerde storage-account, een account in hetzelfde abonnement als de service, of een opslagaccount met betrekking tot een ander abonnement. Zie voor meer informatie over het [opslagaccounts voor uw virtuele array beheren](storsimple-virtual-array-manage-storage-accounts.md).

Gebruik de volgende aanbevelingen voor storage-accounts die zijn gekoppeld aan uw virtuele matrix.

* Bij het koppelen van meerdere virtuele matrices met een enkel opslagaccount, factor in de maximale capaciteit (64 TB) voor een virtuele matrix en de maximale grootte (500 TB) voor een opslagaccount. Dit beperkt het aantal op volledige grootte virtuele matrices die gekoppeld aan dit account naar ongeveer 7 worden kan.
* Bij het maken van een nieuw opslagaccount
  
  * Het is raadzaam dat u dit in de regio die het dichtst bij de externe kantoren/filialen office waar uw StorSimple Virtual Array wordt geïmplementeerd maken op de latentie te minimaliseren.
  * Houd er rekening mee dat u kunt een storage-account niet tussen verschillende regio's verplaatsen. U kunt ook een service niet verplaatsen tussen abonnementen.
  * Gebruik een opslagaccount waarmee de redundantie tussen de datacentra. Geografisch redundante opslag (GRS), Zone Redundant Storage (ZRS) en lokaal redundante opslag (LRS) worden alle ondersteund voor gebruik met uw virtuele matrix. Voor meer informatie over de verschillende typen opslagaccounts, gaat u naar [Azure storage-replicatie](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Shares en volumes
U kunt voor uw StorSimple Virtual Array shares inrichten wanneer deze is geconfigureerd als een bestandsserver en de volumes uit wanneer geconfigureerd als een iSCSI-server. De aanbevolen procedures voor het maken van bestandsshares en volumes zijn gerelateerd aan de grootte en het type dat is geconfigureerd.

#### <a name="volumeshare-size"></a>Grootte van de volume/bestandsshare
U kunt op uw virtuele array shares inrichten wanneer deze is geconfigureerd als een bestandsserver en de volumes uit wanneer geconfigureerd als een iSCSI-server. De aanbevolen procedures voor het maken van bestandsshares en volumes hebben betrekking op de grootte en het type dat is geconfigureerd. 

Houd rekening met de volgende aanbevolen procedures bij het inrichten van shares of volumes op uw virtuele apparaat.

* De bestandsgrootte ten opzichte van de ingerichte grootte van een gelaagde share kunnen de cloudlagen prestaties beïnvloeden. Werken met grote bestanden kan leiden tot een trage laag uitschalen. Als u werkt met grote bestanden, wordt u aangeraden het grootste bestand kleiner is dan 3% van de grootte van de bestandsshare is.
* Maximaal 16 volumes/shares kan worden gemaakt voor de virtuele matrix. Voor de maximale grootte van de lokaal vastgemaakte als gelaagde volumes/shares, altijd verwijzen naar de [StorSimple Virtual Array beperkt](storsimple-ova-limits.md).
* Bij het maken van een volume, rekening houden in de verwachte gegevensverbruik, evenals de toekomstige groei. Het volume kan niet later worden uitgebreid.
* Als het volume eenmaal is gemaakt, kunt u de grootte van het volume op StorSimple niet verkleinen.
* Bij het schrijven naar een gelaagd volume op StorSimple, wanneer de gegevens op het volume een bepaalde drempelwaarde (ten opzichte van de lokale ruimte die is gereserveerd voor het volume) bereikt, wordt de i/o beperkt. U verdergaat met het schrijven naar dit volume vertraagt de i/o aanzienlijk. Hoewel u kunt schrijven naar een gelaagd volume buiten de ingerichte capaciteit (we niet actief stopt de gebruiker schrijven buiten de ingerichte capaciteit), ziet u een waarschuwing naar het effect dat u hebt oversubscribed. Als u de waarschuwing ziet, is het van essentieel belang dat u corrigerende maatregelen, zoals verwijderen gegevens op het volume treffen (volume-uitbreiding is momenteel niet ondersteund).
* Voor disaster recovery van use cases, zoals het aantal toegestane shares/volumes 16 is en het maximum aantal shares/volumes die parallel kunnen worden verwerkt ook 16 is, heeft het aantal shares/volumes geen invloed op uw RPO en RTO's ingezet.

#### <a name="volumeshare-type"></a>Volume/sharetype
StorSimple ondersteunt twee volume of andere share typen op basis van het gebruik: lokaal vastgemaakt en lagen. Lokaal vastgemaakte volumes/shares zijn compact ingericht dat de gelaagde volumes/shares zijn thin provisioning. Een lokaal vastgemaakt volume of een andere share kan niet worden geconverteerd naar gelaagde of *omgekeerd* eenmaal is gemaakt.

U wordt aangeraden dat u de volgende aanbevolen procedures implementeren bij het configureren van StorSimple-volumes/shares:

* Geef het volume dat op basis van de werkbelastingen die u implementeren wilt voordat u een volume maken. Gebruik lokaal vastgemaakte volumes voor workloads waarvoor lokale garanties van gegevens (zelfs tijdens een storing in de cloud) en die de cloud met lage latentie vereisen. Als u een volume op uw virtuele array maakt, u kunt het volumetype niet wijzigen van lokaal vastgemaakt aan gelaagde of *omgekeerd*. Als u bijvoorbeeld lokaal vastgemaakte volumes maken bij het implementeren van de SQL-werkbelastingen of werkbelastingen hosten van virtuele machines (VM's); Gebruik gelaagde volumes voor file share werkbelastingen.


#### <a name="volume-format"></a>Volume-indeling
Nadat u StorSimple-volumes op de iSCSI-server, die u wilt initialiseren, koppelen en formatteren van de volumes. Met deze bewerking wordt uitgevoerd op de host verbonden is met uw StorSimple-apparaat. Bij koppelen en formatteren van volumes op de host StorSimple is het raadzaam om volgende aanbevolen procedures te volgen.

* Voer een snelle formattering uit op alle StorSimple-volumes.
* Bij het opmaken van een StorSimple-volume, gebruikt u een grootte van toewijzingseenheid (AU's) van 64 KB (de standaardwaarde is 4 KB). De 64 KB AU's dat is gebaseerd op tests uitgevoerd intern voor StorSimple werkvolumesystemen en andere werkbelastingen.
* Wanneer u de StorSimple Virtual Array geconfigureerd als een iSCSI-server, gebruik geen spanned volumes of dynamische schijven als deze volumes of schijven worden niet ondersteund door StorSimple.

#### <a name="share-access"></a>Bestandsshare-toegang
Bij het maken van shares op uw virtuele matrix-bestandsserver, volgt u deze richtlijnen:

* Bij het maken van een share, moet u een gebruikersgroep toewijzen als de beheerder van een share in plaats van één gebruiker.
* Nadat de share is gemaakt door de shares via Windows Verkenner te bewerken, kunt u de NTFS-machtigingen beheren.

#### <a name="volume-access"></a>Volumetoegang
Bij het configureren van de iSCSI-volumes op uw StorSimple Virtual Array, is het belangrijk om toegang te bepalen waar nodig. Om te bepalen welke hostservers toegang hebben tot de volumes, maken en koppelen van access control records (ACR's) met StorSimple-volumes.

Gebruik de volgende aanbevolen procedures bij het configureren van ACR's voor StorSimple-volumes:

* Altijd ten minste één ACR koppelen met een volume.

* Bij het toewijzen van meer dan één ACR naar een volume, zorg ervoor dat het volume niet beschikbaar is in een manier waarbij gelijktijdig door meer dan één niet-geclusterde host kunnen worden geopend. Als u meerdere ACR's naar een volume, verschijnt een waarschuwingsbericht ter controle van uw configuratie.

### <a name="data-security-and-encryption"></a>Gegevensbeveiliging en -versleuteling
Uw StorSimple Virtual Array is beveiliging en versleuteling gegevensfuncties die ervoor zorgen de vertrouwelijkheid en integriteit van uw gegevens dat. Wanneer u deze functies gebruikt, wordt het aanbevolen dat u deze aanbevolen procedures volgt: 

* Definieer een coderingssleutel voor cloudopslag voor het genereren van AES-256-codering voordat de gegevens van uw virtuele array wordt verzonden naar de cloud. Deze sleutel is niet vereist als uw gegevens begint met is versleuteld. De sleutel kan worden gegenereerd en veilig met een sleutelbeheersysteem zoals bewaard [Azure sleutelkluis](../key-vault/key-vault-whatis.md).
* Bij het configureren van de storage-account via de StorSimple Manager-service, zorg ervoor dat u de SSL-modus voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud inschakelen.
* Sleutels opnieuw genereren de voor uw storage-accounts (door het openen van de Azure Storage-service) periodiek naar rekening gehouden met eventuele wijzigingen voor toegang tot op basis van de gewijzigde lijst met beheerders.
* Gegevens op uw virtuele array wordt gecomprimeerd en ontdubbeld voordat deze wordt verzonden naar Azure. U kunt beter geen de functieservice Gegevensontdubbeling op uw Windows Server-host.

## <a name="operational-best-practices"></a>Aanbevolen operationele procedures
De aanbevolen procedures zijn richtlijnen die moeten worden gevolgd tijdens het dagelijkse beheer of de werking van de virtuele matrix. Deze procedures betrekking hebben op specifieke beheertaken zoals het maken van back-ups herstellen vanuit een back-upset, uitvoeren van een failover, deactiveren en verwijderen van de matrix, gebruik van het bestandssysteem en de status controleren en het uitvoeren van virus scant op uw virtuele matrix.

### <a name="backups"></a>Back-ups
De gegevens op uw virtuele array de back-up naar de cloud op twee manieren, een standaard automatische dagelijkse back-up van het hele apparaat starten om 22:30 of via een handmatige back-up op aanvraag. Standaard maakt het apparaat automatisch dagelijkse cloudmomentopnamen van alle gegevens die zich hierop bevindt. Ga voor meer informatie naar [maakt u een back-up van uw StorSimple Virtual Array](storsimple-virtual-array-backup.md).

De frequentie en de retentie die zijn gekoppeld aan de standaard back-ups kunnen niet worden gewijzigd, maar u kunt configureren dat de tijd waarop de dagelijkse back-ups dagelijks zijn gestart. Bij het configureren van de begintijd voor de automatische back-ups, raden wij aan dat:

* Uw back-ups plannen voor daluren. Begintijd back-up moet niet overeenkomen met talrijke i/o-host.
* Start een handmatige back-up op aanvraag wanneer u wilt uitvoeren van een failover van het apparaat of vóór het onderhoudsvenster om de gegevens op uw virtuele array te beveiligen.

### <a name="restore"></a>Herstellen
U kunt herstellen vanuit een back-upset op twee manieren: terugzetten naar een ander volume of share of een herstel op itemniveau (alleen beschikbaar op een virtuele matrix die is geconfigureerd als een bestandsserver) uit te voeren. Herstel op itemniveau kunt u een gedetailleerde herstel van bestanden en mappen van een cloudback-up van alle shares op het StorSimple-apparaat. Ga voor meer informatie naar [herstellen vanaf een back-up](storsimple-virtual-array-clone.md).

Bij het uitvoeren van een herstelpunt, houd rekening met de volgende richtlijnen:

* Uw StorSimple Virtual Array biedt geen ondersteuning voor in-place herstellen. Dit echter gemakkelijk kan worden bereikt door een proces in twee stappen: Maak ruimte op de virtuele matrix en vervolgens herstelt naar een ander volume of andere share.
* Bij het herstellen van een lokaal volume, houd er rekening mee worden het terugzetten van een langdurige bewerking. Hoewel het volume kan snel online is, blijven de gegevens op de achtergrond worden gemigreerd.
* Het volumetype blijft hetzelfde tijdens het herstelproces. Een gelaagd volume wordt hersteld naar een ander gelaagde volume en een lokaal vastgemaakt volume naar een ander lokaal vastgemaakt volume.
* Wanneer u een volume of een share herstellen vanuit een back-up wilt instellen, als de hersteltaak is mislukt, een doelvolume of share nog steeds kan worden gemaakt in de portal. Het is belangrijk dat u deze niet-gebruikte doelvolume verwijderen of de share in de portal voor het minimaliseren van toekomstige problemen die ontstaan van dit element.

### <a name="failover-and-disaster-recovery"></a>Failover en herstel na noodgevallen
Een failover van het apparaat kunt u uw gegevens migreert vanuit een *bron* apparaat in het datacenter naar de andere *doel* apparaat zich in hetzelfde of een andere geografische locatie. De failover van het apparaat is voor het hele apparaat. Tijdens de failover verandert de cloudgegevens voor het bronapparaat eigendom met die van het doelapparaat.

Voor uw StorSimple Virtual Array, kunt u zich alleen failover naar een andere virtuele matrix beheerd door de dezelfde StorSimple Manager-service. Een failover naar een apparaat 8000-serie of een matrix beheerd door een andere StorSimple Manager-service (dan de versie van het bronapparaat) is niet toegestaan. Voor meer informatie over de failover-overwegingen, gaat u naar [vereisten voor de failover van het apparaat](storsimple-virtual-array-failover-dr.md).

Wanneer u een failover uitvoert via voor uw virtuele array, houd rekening met het volgende:

* Voor een geplande failover is een aanbevolen procedure te nemen van alle volumes/shares offline voordat de failover wordt gestart. De besturingssysteem-specifieke instructies voor de offline volumes/shares op de host eerst duren en vervolgens die offline zetten op het virtuele apparaat.
* Voor een bestand managementserver na noodgeval (DR), wordt u aangeraden dat u het doelapparaat aan hetzelfde domein als de bron, toevoegen zodat de sharemachtigingen automatisch opgelost worden. Alleen de failover naar een doelapparaat in hetzelfde domein wordt ondersteund in deze release.
* Nadat het herstel na Noodgevallen met succes is voltooid, wordt het bronapparaat automatisch verwijderd. Hoewel het apparaat niet meer beschikbaar is, wordt de virtuele machine die u hebt ingericht op het hostsysteem nog steeds resources verbruikt. Het is raadzaam dat u deze virtuele machine uit uw hostsysteem verwijderen om te voorkomen dat eventuele kosten oplopen.
* Houd er rekening mee dat, zelfs als de failover mislukt is, **de gegevens in de cloud altijd veilig is**. Houd rekening met de volgende drie scenario's waarin de failover is niet voltooid:
  
  * Er is een fout opgetreden in de eerste fasen van de failover, zoals wanneer de voorcontroles voor het herstel na Noodgevallen worden uitgevoerd. In dit geval kan het doelapparaat nog steeds worden gebruikt. U kunt de failover op hetzelfde apparaat opnieuw proberen.
  * Er is een fout opgetreden tijdens het werkelijke failover. In dit geval is het doelapparaat onbruikbaar gemarkeerd. U moet inrichten en een ander doel virtuele matrix configureren en gebruiken die voor failover.
  * De failover is voltooid waarna het bronapparaat is verwijderd, maar het doelapparaat heeft problemen en u geen toegang tot gegevens. De gegevens in de cloud nog steeds veilig is en eenvoudig kan worden opgehaald door het maken van een andere virtuele matrix en vervolgens wordt gebruikt als een apparaat voor het herstel na Noodgevallen.

### <a name="deactivate"></a>Deactiveren
Wanneer u een StorSimple Virtual Array deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deactivering is een **permanente** bewerking kan niet ongedaan worden gemaakt. Een gedeactiveerd apparaat kan niet opnieuw worden geregistreerd bij de StorSimple Manager-service. Ga voor meer informatie naar [deactiveren en verwijderen van uw StorSimple Virtual Array](storsimple-virtual-array-deactivate-and-delete-device.md).

Houd er rekening mee met de volgende aanbevolen procedures bij het deactiveren van uw virtuele matrix:

* Maakt u een cloudmomentopname van de gegevens voordat u een virtueel apparaat deactiveren. Wanneer u een virtuele matrix deactiveert, zijn alle gegevens van het lokale apparaat gaat verloren. Maken van een cloudmomentopname kunt u gegevens herstellen op een later tijdstip.
* Voordat u een StorSimple Virtual Array deactiveert, beëindigt of verwijdert u clients en hosts die afhankelijk van dat apparaat zijn maken.
* Een gedeactiveerd apparaat verwijderen als u niet langer gebruikt zodat deze kosten niet doorlopen.

### <a name="monitoring"></a>Bewaking
Om ervoor te zorgen dat uw StorSimple Virtual Array in een doorlopende status in orde is, moet u de matrix controleren en ervoor te zorgen dat u gegevens uit het systeem met inbegrip van waarschuwingen ontvangen. Implementeer de volgende aanbevolen procedures voor het controleren van de algemene status van de virtuele matrix:

* Bewaking voor het volgen van het schijfgebruik van de gegevensschijf van uw virtuele matrix, evenals de schijf met het besturingssysteem te configureren. Als Hyper-V wordt uitgevoerd, kunt u een combinatie van System Center Virtual Machine Manager (SCVMM) en System Center Operations Manager gebruiken voor het bewaken van uw virtualisatiehosts.
* E-mailmeldingen configureren op uw virtuele array voor het verzenden van waarschuwingen op het gebruiksniveau van bepaalde.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Zoeken op index en virus scan toepassingen
Een StorSimple Virtual Array kan automatisch gegevens uit de lokale laag in de Microsoft Azure-cloud onderverdelen in lagen. Wanneer een toepassing zoals zoeken in een index of een virusscan wordt gebruikt voor het scannen van de gegevens die zijn opgeslagen op StorSimple, moet u om te zorgen dat de cloudgegevens niet ophalen geopend en terug naar de lokale laag worden opgehaald.

U wordt aangeraden dat u de volgende aanbevolen procedures implementeren bij het configureren van de index zoeken of virus scan op uw virtuele matrix:

* Alle bewerkingen die worden automatisch geconfigureerde volledige scan uitschakelen.
* Voor gelaagde volumes, de index zoeken of virus scan toepassing configureren voor een incrementele scannen. Dit zou scannen alleen de nieuwe gegevens waarschijnlijk die zich bevinden op de lokale laag. De gegevens die naar de cloud is gelaagd is niet geopend tijdens een incrementele bewerking.
* Zorg ervoor dat de juiste zoekfilters en instellingen worden geconfigureerd zodat alleen de beoogde typen bestanden gescand ophalen. Bijvoorbeeld, afbeeldingsbestanden (JPEG, GIF- en TIFF-bestand) en technische tekeningen moet niet worden gecontroleerd tijdens de incrementele of volledige indexen.

Als Windows process indexeren, volgt u deze richtlijnen:

* Gebruik de Windows-indexeerfunctie niet voor gelaagde volumes, zoals het grote hoeveelheden gegevens (TB) vanuit de cloud roept als de index moet vaak opnieuw worden opgebouwd. Opnieuw opbouwen van de index ophalen van alle bestandstypen wilt hun inhoud indexeren.
* Gebruik de Windows-proces voor lokaal vastgemaakte volumes indexeren als dit alleen toegang gegevens op de lokale lagen om de index tot zou (de cloudgegevens wordt niet toegankelijk) te maken.

### <a name="byte-range-locking"></a>Vergrendelen van de byte-bereik
Toepassingen kunnen vergrendelen van een opgegeven bereik van bytes binnen de bestanden. Als byte bereik vergrendeling is ingeschakeld op de toepassingen die zijn schrijven naar uw StorSimple, werkt klikt u vervolgens meerdere lagen niet op uw virtuele array. Voor de opslaglagen om te werken, moeten alle gebieden van de bestanden die benaderd worden ontgrendeld. Vergrendelen van de byte-bereik wordt met gelaagde volumes op uw virtuele array niet ondersteund.

Aanbevolen maatregelen te verlichten byte bereik vergrendelen zijn onder andere:

* Bytebereik vergrendelen in uw toepassingslogica uitschakelen.
* Gebruik lokaal vastgemaakt volumes (in plaats van lagen) voor de gegevens die zijn gekoppeld aan deze toepassing. Lokaal vastgemaakte volumes niet laag naar de cloud.
* Wanneer met behulp van de volumes lokaal vastgemaakt met byte-bereik vergrendeling is ingeschakeld, kan het volume online komen voordat de herstelbewerking voltooid is. In dergelijke gevallen moet u wachten voor het terugzetten van worden voltooid.

## <a name="multiple-arrays"></a>Meerdere matrices
Meerdere virtuele matrices kunnen moet worden geïmplementeerd voor een account voor een groeiende werkset van gegevens die kunnen worden gelekt naar de cloud dus die betrekking hebben op de prestaties van het apparaat. In dergelijke gevallen is het is raadzaam op schaal apparaten terwijl de werkset groeit. Hiervoor moet een of meer apparaten moeten worden toegevoegd in de on-premises Datacenter. Bij het toevoegen van de apparaten, kunt u het volgende doen:

* De huidige set gegevens gesplitst.
* Nieuwe werkbelastingen implementeert op nieuwe apparaten.
* Als meerdere virtuele matrices implementeert, raden wij aan dat uit de taakverdeling perspectief van de matrix verdelen over verschillende hypervisorhosts.
* Meerdere virtuele matrices (indien geconfigureerd als een bestandsserver of een iSCSI-server) kunnen worden geïmplementeerd in een Namespace Distributed File System. Ga voor gedetailleerde stappen naar [Distributed File System Namespace oplossing met Hybrid Cloud Storage Deployment Guide](https://www.microsoft.com/download/details.aspx?id=45507). Distributed File System Replication wordt momenteel niet aanbevolen voor gebruik met de virtuele matrix. 

## <a name="see-also"></a>Zie ook
Meer informatie over het [beheren van uw StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md) via de StorSimple Manager-service.


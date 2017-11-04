---
title: Aanbevolen procedures voor het virtuele StorSimple-matrix | Microsoft Docs
description: Beschrijft aanbevolen procedures voor het implementeren en beheren van de virtuele StorSimple-matrix.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2017
ms.author: alkohli
ms.openlocfilehash: 264764c5e9c32574d97beb2cc3c1bb1cfb555568
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-best-practices"></a>Aanbevolen procedures voor virtuele StorSimple-matrix
## <a name="overview"></a>Overzicht
Microsoft Azure StorSimple virtuele matrix is een geïntegreerde opslagoplossing die beheert opslagtaken tussen een on-premises virtuele apparaat uitgevoerd in een hypervisor en Microsoft Azure cloud-opslag. Virtuele StorSimple-matrix is een efficiënte en voordelige alternatief voor de fysieke matrix 8000-serie. De virtuele matrix kan worden uitgevoerd op uw bestaande infrastructuur van de hypervisor ondersteunt zowel de iSCSI- en de SMB-protocollen en geschikt is voor externe office/filiaalscenario's. Ga voor meer informatie over de StorSimple-oplossingen naar [overzicht van Microsoft Azure StorSimple](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

In dit artikel bevat informatie over de aanbevolen procedures geïmplementeerd tijdens de eerste installatie, implementatie en beheer van de virtuele StorSimple-matrix. Deze aanbevolen procedures bieden gevalideerde richtlijnen voor de installatie en beheer van uw virtuele matrix. In dit artikel is gericht op de IT-beheerders die implementeren en beheren van de virtuele matrices in hun datacenters.

Het wordt aangeraden de periodieke evaluatie van de aanbevolen procedures om te zorgen voor dat uw apparaat is nog steeds compatibel wanneer wijzigingen zijn aangebracht aan de installatie- of operationele stroom. Moet er problemen optreden tijdens het implementeren van deze aanbevolen procedures op uw virtuele matrix [contact op met Microsoft Support](storsimple-virtual-array-log-support-ticket.md) voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie
Deze aanbevolen procedures betrekking hebben op de richtlijnen die moet worden gevolgd tijdens de eerste configuratie en implementatie van de virtuele-matrices. Deze aanbevolen procedures bevatten die gerelateerd zijn aan de inrichting van de virtuele machine, instellingen voor Groepsbeleid sizing, instellen van de netwerken, opslagaccounts configureren en maken van bestandsshares en volumes voor de virtuele-matrix. 

### <a name="provisioning"></a>Inrichten
Virtuele StorSimple-matrix is een virtuele machine (VM) die geconfigureerd zijn op de hypervisor (Hyper-V- of VMware) van de hostserver. Bij het inrichten van de virtuele machine, zorg ervoor dat de host te reserveren voor voldoende bronnen. Ga voor meer informatie naar [minimale resourcevereisten](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) voor het inrichten van een matrix.

Implementeer de volgende aanbevolen procedures bij het inrichten van de virtuele matrix:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Type van de virtuele machine** |**Generatie 2** VM voor gebruik met Windows Server 2012 of hoger en een *.vhdx* installatiekopie. <br></br> **Generatie 1** VM voor gebruik met een Windows Server 2008 of hoger en een *.vhd* installatiekopie. |Gebruik van virtuele machine versie 8-11 wanneer u *.vmdk* installatiekopie. |
| **Geheugentype** |Configureren als **statisch geheugen**. <br></br> Gebruik niet de **dynamisch geheugen** optie. | |
| **Gegevenstype van de schijf** |Inrichten als **dynamisch uitbreidbare**.<br></br> **Een vaste grootte** lang duurt. <br></br> Gebruik niet de **differentiërende** optie. |Gebruik de **thin inrichten** optie. |
| **Wijziging van de schijf gegevens** |Uitbreiden of verkleinen is niet toegestaan. Een poging om dit te doen, resulteert het verlies van alle lokale gegevens op het apparaat. |Uitbreiden of verkleinen is niet toegestaan. Een poging om dit te doen, resulteert het verlies van alle lokale gegevens op het apparaat. |

### <a name="sizing"></a>Schaling
Wanneer het formaat van uw virtuele StorSimple-matrix, houd rekening met de volgende factoren:

* Lokale reservering voor de volumes of shares. Ongeveer 12% van de ruimte is gereserveerd op de lokale laag voor elke ingerichte gelaagd volume of share. 10% van de ruimte is ongeveer ook gereserveerd voor een lokaal vastgemaakt volume voor bestandssysteem.
* De overhead in momentopname. Ongeveer is 15% ruimte op de lokale laag gereserveerd voor momentopnamen.
* Nodig is voor terugzetbewerkingen. Als u herstellen als een nieuwe bewerking uitvoert, worden sizing moet rekening houden met de benodigde ruimte voor herstel. Terugzetten wordt uitgevoerd op een share of het volume van dezelfde grootte hebben.
* Buffer moet worden toegewezen voor eventuele onverwachte groei.

Op basis van de voorgaande factoren, worden de vereisten sizing vertegenwoordigd door de volgende vergelijking:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

De volgende voorbeelden laten zien hoe u kunt het formaat van een virtuele-matrix op basis van uw vereisten.

#### <a name="example-1"></a>Voorbeeld 1:
Op uw virtuele matrix die u wilt mogelijk

* inrichten van een 2 TB gelaagde volume of share.
* inrichten van een 1 TB gelaagde volume of share.
* inrichten van een lokaal vastgemaakt volume van 300 GB of delen.

De voorgaande volumes of shares, de benodigde schijfruimte op de lokale laag laat het ons te berekenen.

Eerste moet voor elke gelaagde volume/share, zou lokale reservering worden gelijk is aan % 12 van de grootte van het volume/delen. Lokale reservering is voor het lokaal vastgemaakt volume/delen, 10% van de lokaal vastgemaakt volume/delen grootte (in aanvulling op de grootte van de ingerichte). In dit voorbeeld moet u

* Lokale reservering 240 GB (gelaagd volume/delen voor een 2 TB)
* Lokale reservering 120 GB (gelaagd volume/delen voor een 1 TB)
* 330 GB voor lokaal vastgemaakt volume of share (10% van de lokale reservering toevoegen aan de grootte van 300 GB ingericht)

De totale ruimte vereist op de lokale laag tot nu toe is: 240 GB + 120 GB + 330 GB = 690 GB.

Ten tweede moet ten minste net zoveel ruimte op de lokale laag als de grootste één reservering. Deze extra hoeveelheid wordt gebruikt als u wilt herstellen vanuit een cloudmomentopname. In dit voorbeeld is de grootste lokale reservering 330 GB (met inbegrip van de reservering voor file system), zodat u toevoegen wilt die aan de 690 GB: 690 GB + 330 GB = 1020 GB.
Als we herstelt de volgende aanvullende hebt uitgevoerd, kunnen we altijd de ruimte van de vorige herstelbewerking vrijmaken.

Tot slot moeten we 15% van uw totale lokale ruimte tot nu toe voor het opslaan van lokale momentopnamen, zodat alleen 85% ervan beschikbaar is. In dit voorbeeld dat is rond 1020 GB = 0.85&ast;ingerichte gegevens schijf TB. Ja, de ingerichte gegevensschijf zou zijn (1020&ast;(1/0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (afronden naar de dichtstbijzijnde kwartiel)

Waarbij u rekening houdt onverwachte groei en nieuwe herstelt, moet u een lokale schijf van rond inrichten 1,25-1,5 TB.

> [!NOTE]
> We raden u ook aan de lokale schijf is dun ingericht. Deze aanbeveling is omdat de restore-ruimte is alleen nodig als u wilt herstellen van gegevens die ouder zijn dan vijf dagen. Herstel op itemniveau kunt u voor het terugzetten van gegevens voor de afgelopen vijf dagen zonder de extra ruimte om te herstellen.


#### <a name="example-2"></a>Voorbeeld 2:
Op uw virtuele matrix die u wilt mogelijk

* inrichten van een 2 TB gelaagd volume
* 300 GB lokaal vastgemaakte volumes creëren

Op basis van 12% van de lokale ruimte reservering voor gelaagde volumes/shares en 10% voor lokaal vastgemaakte volumes/shares, moeten we

* Lokale reservering 240 GB (gelaagd volume/delen voor 2 TB)
* 330 GB voor lokaal vastgemaakt volume of share (10% van de lokale reservering toevoegen aan de ruimte 300 GB ingericht)

Totale ruimte vereist op de lokale laag is: 240 GB + 330 GB = 570 GB

De minimale lokale ruimte die nodig zijn voor herstel is 330 GB.

15% van de totale schijfruimte wordt gebruikt voor het opslaan van momentopnamen, zodat alleen 0.85 beschikbaar is. Ja, de schijfgrootte van de is (900&ast;(1/0.85)) = 1,06 TB ~ 1,25 TB (afronden naar de dichtstbijzijnde kwartiel)

U kunt een lokale schijf 1,25-1,5 TB waarbij in een onverwachte groei, inrichten.

### <a name="group-policy"></a>Groepsbeleid
Groepsbeleid is een infrastructuur waarmee u een specifieke configuraties voor gebruikers en computers te implementeren. Instellingen voor Groepsbeleid zijn opgenomen in groepsbeleidsobjecten (GPO's) die zijn gekoppeld aan de volgende Active Directory Domain Services (AD DS)-containers: sites, domeinen of organisatie-eenheden (OE's). 

Als uw virtuele matrix lid van een domein is, kunnen groepsbeleidsobjecten worden toegepast. Deze groepsbeleidsobjecten kunnen toepassingen zoals antivirussoftware die heeft een nadelige invloed op de werking van de virtuele StorSimple-matrix hebben kunnen installeren.

Daarom aangeraden dat u:

* Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory.
* Zorg ervoor dat er geen groepsbeleidsobjecten (GPO's) worden toegepast op uw virtuele matrix. U kunt overname om ervoor te zorgen dat de virtuele-matrix (onderliggend knooppunt) niet automatisch geen GPO's van de bovenliggende overgenomen is blokkeren. Ga voor meer informatie naar [overname blokkeren](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Netwerken
De netwerkconfiguratie voor uw virtuele matrix wordt gedaan door de lokale webgebruikersinterface. De virtuele netwerkinterface is ingeschakeld via de hypervisor waarin de virtuele matrix is ingericht. Gebruik de [netwerkinstellingen](storsimple-virtual-array-deploy3-fs-setup.md) pagina voor het configureren van de virtuele netwerk interface IP-adres, subnetmasker en gateway.  U kunt ook de primaire en secundaire DNS-server, tijdinstellingen en optionele proxy-instellingen configureren voor uw apparaat. De meeste van de netwerkconfiguratie is een eenmalige configuratie. Controleer de [StorSimple netwerkvereisten](storsimple-ova-system-requirements.md#networking-requirements) vóór implementatie van de virtuele-matrix.

Bij het implementeren van uw virtuele matrix, raden we aan dat u deze aanbevolen procedures volgt:

* Zorg ervoor dat het netwerk waarin de virtuele matrix altijd wordt geïmplementeerd, de capaciteit heeft te reserveren voor 5 Mbps internetbandbreedte (of meer).
  
  * Internet bandbreedte nodig varieert, afhankelijk van de kenmerken van uw werkbelasting en de frequentie van gewijzigde gegevens.
  * De gegevenswijziging die kan worden verwerkt is rechtstreeks evenredig aan uw internetbandbreedte. Als u bijvoorbeeld bij het nemen van een back-up een 5 Mbps bandbreedte geschikt voor een gegevenswijziging van ongeveer 18 GB in 8 uur. Met vier keer meer bandbreedte (20 Mbps), kunt u vier keer meer gegevens wijzigen (72 GB) te verwerken.
* Zorg ervoor dat de verbinding met het Internet altijd beschikbaar is. Sporadisch of onbetrouwbare Internet-verbindingen naar de apparaten kunnen leiden tot verlies van toegang tot gegevens in de cloud en kunnen leiden tot een niet-ondersteunde configuratie.
* Als u van plan bent om het apparaat een iSCSI-server implementeren:
  
  * Het is raadzaam dat u uitschakelen de **IP-adres automatisch ophalen** optie (DHCP).
  * Configureer statische IP-adressen. U moet een primaire en secundaire DNS-server configureren.
  * Als het definiëren van meerdere netwerkinterfaces op de virtuele-matrix, worden alleen de eerste netwerkinterface (deze interface is standaard **Ethernet**) toegang heeft tot de cloud. U kunt meerdere virtuele netwerkinterfaces te maken op uw virtuele matrix (geconfigureerd als een server met iSCSI-) en die interfaces verbinden met verschillende subnetten voor het beheren van het type verkeer.
* Alleen de cloud-bandbreedte beperken (gebruikt door de virtuele matrix), beperkingen op de router of de firewall configureren. Als u in de hypervisor beperking definieert, wordt deze vertraging alle protocollen zoals iSCSI- en SMB in plaats van alleen de bandbreedte van de cloud.
* Zorg ervoor dat tijdsynchronisatie voor hypervisors is ingeschakeld. Als uw virtuele matrix met Hyper-V in de Hyper-V-beheer is geselecteerd, gaat u naar **instellingen &gt; integratieservices**, en zorg ervoor dat de **tijdsynchronisatie** is ingeschakeld.

### <a name="storage-accounts"></a>Opslagaccounts
Virtuele StorSimple-matrix kan worden gekoppeld aan een enkele storage-account. Dit opslagaccount kan een automatisch gegenereerde storage-account, een account in hetzelfde abonnement als de service of een opslagaccount die is gerelateerd aan een ander abonnement. Zie voor meer informatie hoe [storage-accounts voor uw virtuele matrix beheren](storsimple-virtual-array-manage-storage-accounts.md).

Gebruik de volgende aanbevelingen voor storage-accounts die zijn gekoppeld aan uw virtuele matrix.

* Bij het koppelen van meerdere virtuele matrices met een enkele opslagaccount rekening te houden in de maximale capaciteit (64 TB) voor een virtuele-matrix en de maximale grootte (500 TB) voor een opslagaccount. Dit beperkt het aantal grote virtuele matrices die gekoppeld aan dit opslagaccount naar ongeveer 7 worden kunnen.
* Bij het maken van een nieuw opslagaccount
  
  * U wordt aangeraden dat u dit hebt gemaakt in de regio die het dichtst bij de externe office/filiaal waar uw virtuele StorSimple-matrix minimaliseren latenties wordt geïmplementeerd.
  * Vergeet dat u een opslagaccount niet tussen verschillende regio's verplaatsen. U kunt ook een service voor abonnementen niet verplaatsen.
  * Gebruik een opslagaccount dat redundantie tussen de datacentra implementeert. Geo-Redundant Storage (GRS), Zone-redundante opslag (ZRS) en lokaal redundante opslag (LRS) worden alle ondersteund voor gebruik met uw virtuele matrix. Ga voor meer informatie over de verschillende typen opslagaccounts naar [Azure storage-replicatie](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Shares en -volumes
Voor uw virtuele StorSimple-matrix, kunt u shares inrichten wanneer deze is geconfigureerd als een bestandsserver en volumes wanneer geconfigureerd als een iSCSI-server. Aanbevolen procedures voor het maken van bestandsshares en volumes zijn gerelateerd aan de grootte en het type dat is geconfigureerd.

#### <a name="volumeshare-size"></a>Grootte van de volume/bestandsshare
U kunt op uw virtuele matrix shares inrichten wanneer deze is geconfigureerd als een bestandsserver en volumes wanneer geconfigureerd als een iSCSI-server. Aanbevolen procedures voor het maken van bestandsshares en volumes gerelateerd zijn aan de grootte en het type dat is geconfigureerd. 

Houd er rekening mee houden de volgende aanbevolen procedures bij het inrichten van shares of volumes op uw virtuele apparaat.

* De bestandsgroottes ten opzichte van de ingerichte grootte van een gelaagde share kunnen invloed hebben op de prestaties van de lagen. Werken met grote bestanden kan leiden tot een trage laag. Wanneer u werkt met grote bestanden, raden we aan dat het grootste bestand kleiner dan 3% van de grootte van de bestandsshare is.
* Maximaal 16 volumes/shares kan worden gemaakt op de virtuele-matrix. Voor de maximale grootte van de lokaal vastgemaakte en gelaagde volumes/shares, altijd verwijzen naar de [virtuele StorSimple-matrix beperkt](storsimple-ova-limits.md).
* Bij het maken van een volume, rekening te houden in de verwachte gegevens verbruik evenals toekomstige groei. Het volume kan niet hoger worden uitgebreid.
* Als het volume is gemaakt, kunt u de grootte van het volume op StorSimple niet verkleinen.
* Bij het schrijven naar een gelaagd volume op StorSimple, wanneer de volumegegevens een bepaalde drempelwaarde (ten opzichte van de lokale ruimte die is gereserveerd voor het volume) bereikt, wordt de i/o beperkt. U wilt doorgaan met het schrijven naar dit volume vertraagt de i/o aanzienlijk. Hoewel u kunt schrijven naar een gelaagd volume buiten de ingerichte capaciteit (we niet actief stopt de gebruiker uit schrijven buiten de ingerichte capaciteit), ziet u een waarschuwing dat u hebt oversubscribed. Zodra u de waarschuwing ziet, is het van cruciaal belang dat u corrigerende maatregelen zoals verwijderen de volumegegevens nemen (volume uitbreiding is momenteel niet ondersteund).
* Voor gevallen herstel na noodgevallen, zoals het aantal toegestane shares/16 en het maximum aantal shares/volumes die parallel kunnen worden verwerkt tevens 16, heeft het aantal shares/volumes geen invloed op uw RPO en RTO's.

#### <a name="volumeshare-type"></a>Volume/sharetype
StorSimple ondersteunt twee volume/delen typen op basis van het gebruik: lokaal vastgemaakt en lagen. Lokaal vastgemaakte volumes/shares compact ingericht terwijl de gelaagde volumes/shares zijn dun ingericht. U kunt een lokaal vastgemaakt volume/share niet converteren naar gelaagde of *omgekeerd* eenmaal is gemaakt.

U wordt aangeraden dat u de volgende best practices implementeren bij het configureren van StorSimple-volumes/shares:

* Geef het volume dat op basis van de werkbelastingen die u implementeren wilt voordat u een volume maken. Gebruik lokaal vastgemaakte volumes voor workloads waarvoor lokale garanties van gegevens (zelfs tijdens een onderbreking van de cloud) en waarvoor cloud lage latenties. Wanneer u een volume op uw virtuele matrix maakt, kunt u het volumetype niet wijzigen van lokaal vastgemaakt aan gelaagde of *omgekeerd*. Als u bijvoorbeeld lokaal vastgemaakte volumes maken bij het implementeren van de SQL-werkbelastingen of werkbelastingen hosten van virtuele machines (VM's); gelaagde volumes gebruiken voor bestandsshare werkbelastingen.
* Controleer de optie voor minder frequent gebruikte gearchiveerde gegevens omvangrijke bestandsgrootten betreft. Een grotere chunkgrootte voor Ontdubbeling van 512 kB wordt gebruikt wanneer deze optie is ingeschakeld voor snellere de gegevensoverdracht naar de cloud.

#### <a name="volume-format"></a>Volume-indeling
Nadat u StorSimple-volumes op uw iSCSI-server maakt, moet u initialiseren, te koppelen en de volumes formatteren. Deze bewerking wordt uitgevoerd op de host verbonden met uw StorSimple-apparaat. Bij het koppelen en formatteren van volumes op de host StorSimple is het raadzaam om volgende aanbevolen procedures te volgen.

* Voer een snelle formattering op alle StorSimple-volumes.
* Bij het opmaken van een StorSimple-volume een clustergrootte (AUS) van 64 KB gebruiken (de standaardwaarde is 4 KB). De 64 KB AUS is gebaseerd op tests uitgevoerd intern voor algemene StorSimple werkbelastingen en andere werkbelastingen.
* Wanneer u de virtuele StorSimple-matrix geconfigureerd als een server met iSCSI-, gebruik geen spanned volumes of dynamische schijven als deze volumes of schijven worden niet ondersteund door StorSimple.

#### <a name="share-access"></a>Bestandsshare-toegang
Bij het maken van shares op de bestandsserver virtuele matrix, volg deze richtlijnen:

* Bij het maken van een share toewijzen een gebruikersgroep als de beheerder van een share in plaats van een enkele gebruiker.
* Nadat de share is gemaakt door de shares in Windows Verkenner te bewerken, kunt u de NTFS-machtigingen beheren.

#### <a name="volume-access"></a>Volumetoegang
Bij het configureren van de iSCSI-volumes op uw virtuele StorSimple-matrix, is het belangrijk om toegang te beheren telkens wanneer nodig. Om te bepalen welke hostservers toegang tot volumes, maken en access control-records (ACRs) koppelen aan StorSimple-volumes.

Gebruik de volgende aanbevolen procedures bij het configureren van ACRs voor StorSimple-volumes:

* Altijd ten minste één ACR met een volume koppelen.

* Bij het toewijzen van meer dan één ACR op een volume, zorg ervoor dat het volume niet beschikbaar is in een manier waarbij gelijktijdig door meer dan één niet-geclusterde host kan worden geopend. Als u meerdere ACRs hebt toegewezen aan een volume, weergegeven een waarschuwingsbericht voor u deze kunt bekijken van uw configuratie.

### <a name="data-security-and-encryption"></a>Beveiliging van gegevens en versleuteling
Uw virtuele StorSimple-matrix heeft gegevens beveiligings- en functies die de vertrouwelijkheid en integriteit van uw gegevens te garanderen. Wanneer deze functies gebruikt, is het raadzaam dat u deze aanbevolen procedures volgt: 

* Definieer een coderingssleutel voor cloudopslag voor het genereren van AES-256-versleuteling voordat de gegevens van uw virtuele matrix wordt verzonden naar de cloud. Deze sleutel is niet vereist als uw gegevens eerst worden versleuteld. De sleutel worden gegenereerd en veilig met een sleutelbeheersysteem zoals blijft [Azure sleutelkluis](../key-vault/key-vault-whatis.md).
* Zorg dat u de SSL-modus voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud inschakelen bij het configureren van de storage-account via de StorSimple Manager-service.
* Opnieuw genereren van de sleutels voor uw storage-accounts (door het openen van de Azure Storage-service) periodiek account op basis van wijzigingen voor toegang tot op basis van de gewijzigde lijst met beheerders.
* Gegevens op uw virtuele matrix wordt gecomprimeerd en ontdubbeld voordat deze wordt verzonden naar Azure. Gebruik de functie gegevensontdubbeling op uw Windows Server-host aanbevolen niet.

## <a name="operational-best-practices"></a>Aanbevolen operationele procedures
De aanbevolen procedures zijn richtlijnen die moeten worden gevolgd tijdens het dagelijkse beheer of de bewerking van de virtuele matrix. Deze procedures hebben betrekking op specifieke beheertaken, zoals het maken van back-ups terugzetten vanuit een back-upset uitvoeren van een failover, deactiveren en verwijderen van de matrix,, systeemgebruik en status controleren en het uitvoeren van virus scant op uw virtuele matrix.

### <a name="backups"></a>Back-ups
De gegevens op uw virtuele matrix back-up naar de cloud op twee manieren, een standaard geautomatiseerde dagelijkse back-up van het gehele apparaat om 22.30 of via een handmatige back-up op aanvraag wordt gestart. Standaard maakt het apparaat automatisch dagelijks cloudmomentopnamen van alle gegevens die zich hierop bevindt. Ga voor meer informatie naar [back-up van uw virtuele StorSimple-matrix](storsimple-virtual-array-backup.md).

De frequentie en bewaren die zijn gekoppeld aan de standaard back-ups kunnen niet worden gewijzigd, maar u kunt de tijd waarop de dagelijkse back-ups dagelijks zijn geïnitieerd configureren. Bij het configureren van de begintijd voor de automatische back-ups wordt aangeraden dat:

* Uw back-ups plannen voor daluren. Begintijd van back-up moet niet overeenkomen met talrijke host IO.
* Start een handmatige back-up op aanvraag in bij het plannen van een failover-apparaat of het onderhoudsvenster vóór de gegevens op uw virtuele matrix beveiligen uit te voeren.

### <a name="restore"></a>Herstellen
U kunt herstellen met een back-upset op twee manieren: herstellen naar een ander volume of share of een herstel op itemniveau (alleen beschikbaar op een virtuele matrix die is geconfigureerd als een bestandsserver) uitvoeren. Herstel op itemniveau kunt u een gedetailleerde herstel van bestanden en mappen doen vanuit een cloud back-up van alle shares op het StorSimple-apparaat. Ga voor meer informatie naar [terugzetten vanaf een back-up](storsimple-virtual-array-clone.md).

Houd rekening met de volgende richtlijnen bij het uitvoeren van een terugzetbewerking:

* Uw virtuele StorSimple-matrix biedt geen ondersteuning voor in-place herstellen. Dit echter gemakkelijk kan worden bereikt door een proces in twee stappen: Maak ruimte op de virtuele matrix en herstel vervolgens op een ander volume/delen.
* Bij het herstellen van een lokaal volume, houd er rekening mee worden het terugzetten van een langdurige bewerking. Hoewel het volume snel online komen mogelijk, blijft de gegevens op de achtergrond worden gehydrateerd.
* Het volumetype blijft hetzelfde tijdens het herstelproces. Een gelaagd volume is teruggezet naar een ander gelaagde volume en een lokaal vastgemaakt volume naar een ander lokaal vastgemaakt volume.
* Als u een volume of een share terugzetten vanaf een back-up wilt instellen, als de hersteltaak is mislukt, een doelvolume of share kan nog steeds in de portal worden gemaakt. Het is belangrijk dat u dit ongebruikte doelvolume verwijderen of in de portal delen om toekomstige problemen die voortkomen uit dit element.

### <a name="failover-and-disaster-recovery"></a>Failover en herstel na noodgevallen
De failover van een apparaat, kunt u uw gegevens migreert vanuit een *bron* apparaat in het datacenter naar de andere *doel* apparaat zich in dezelfde of een andere geografische locatie. De failover van het apparaat is voor het hele apparaat. Tijdens de failover verandert de cloudgegevens voor de Bronapparaat eigendom in die van het doelapparaat.

Voor uw virtuele StorSimple-matrix, kunt u zich alleen failover naar een andere virtuele matrix die worden beheerd door de dezelfde StorSimple Manager-service. Een failover naar een apparaat 8000 series of een matrix die worden beheerd door een andere StorSimple Manager-service (dan de versie voor de bron-device) is niet toegestaan. Voor meer informatie over overwegingen met betrekking tot de failover, gaat u naar [vereisten voor de failover apparaat](storsimple-virtual-array-failover-dr.md).

Houd rekening met het volgende bij het uitvoeren van een mislukken via voor uw virtuele matrix is:

* Voor een geplande failover is een aanbevolen procedure voor het uitvoeren van alle volumes/shares offline voordat u de failover start. Volg de instructies van het besturingssysteem-specifieke te nemen voor het eerst de offline volumes/shares op de host en vervolgens die offline halen op het virtuele apparaat.
* Voor een bestand server-noodherstel (DR), wordt u aangeraden het doelapparaat te koppelen aan hetzelfde domein als de bron zo in dat de sharemachtigingen automatisch opgelost. Alleen de failover naar een doelapparaat in hetzelfde domein wordt ondersteund in deze release.
* Zodra de DR met succes is voltooid, wordt het apparaat van de gegevensbron automatisch verwijderd. Hoewel het apparaat niet langer beschikbaar is, is de virtuele machine die u hebt ingericht op het hostsysteem nog steeds resources verbruikt. Het is raadzaam dat u deze virtuele machine uit uw hostsysteem om te voorkomen dat eventuele kosten doorberekend verwijderen.
* Houd er rekening mee dat zelfs als de failover mislukt is, **de gegevens in de cloud altijd veilig is**. Houd rekening met de volgende drie scenario's waarin de failover is niet voltooid:
  
  * Er is een fout opgetreden in de eerste stadia van de failover zoals wanneer de eerste DR-controles worden uitgevoerd. In dit geval is het doelapparaat nog steeds bruikbaar. U kunt opnieuw proberen de failover op hetzelfde apparaat.
  * Er is een fout opgetreden tijdens het werkelijke failover. In dit geval wordt het doelapparaat is gemarkeerd als onbruikbaar. U moet inrichten en een andere virtuele doelmatrix configureren en gebruiken die voor failover.
  * De failover is voltooid waarna het bronapparaat is verwijderd, maar het doelapparaat problemen heeft en u geen toegang tot alle gegevens. De gegevens in de cloud nog steeds veilig is en eenvoudig kan worden opgehaald door een andere virtuele matrix maken en vervolgens wordt gebruikt als een apparaat voor het herstel na Noodgevallen.

### <a name="deactivate"></a>deactiveren
Wanneer u een virtueel StorSimple-matrix deactiveert, verbreekt u de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deactivering is een **permanente** bewerking en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd met de StorSimple Manager-service opnieuw. Ga voor meer informatie naar [deactiveren en verwijderen van uw virtuele StorSimple-matrix](storsimple-virtual-array-deactivate-and-delete-device.md).

Houd rekening met de volgende aanbevolen procedures bij het deactiveren van uw virtuele matrix:

* Een cloud momentopname van de gegevens voordat u een virtueel apparaat deactiveren. Wanneer u een virtuele-matrix deactiveert, zijn alle gegevens van het lokale apparaat gaat verloren. Maken van een cloudmomentopname kunt u gegevens in een later stadium te herstellen.
* Zorg voordat u een virtueel StorSimple-matrix deactiveert, dat u stoppen of verwijderen van clients en hosts die afhankelijk van dat apparaat zijn.
* Een gedeactiveerde apparaat verwijderen als u niet langer gebruikt zodat deze geen kosten doorlopen.

### <a name="monitoring"></a>Bewaking
Om ervoor te zorgen dat uw virtuele StorSimple-matrix in een continue status in orde is, moet u de matrix controleren en ervoor te zorgen dat u informatie van het systeem met inbegrip van waarschuwingen ontvangen. Implementeer de volgende aanbevolen procedures voor het controleren van de algemene status van de virtuele matrix:

* Configureer controle met het bijhouden van het schijfgebruik van de gegevensschijf virtuele matrix, evenals de besturingssysteemschijf. Als Hyper-V wordt uitgevoerd, kunt u een combinatie van System Center Virtual Machine Manager (SCVMM) en System Center Operations Manager (SCOM) gebruiken voor het bewaken van uw virtualisatiehosts.
* E-mailmeldingen configureren op uw virtuele matrix om waarschuwingen te verzenden op het niveau van bepaalde informatie over het gebruik.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Zoeken op index en virus scan toepassingen
Een virtueel StorSimple-matrix kunt automatisch trapsgewijs gegevens van de lokale laag naar de Microsoft Azure-cloud. Wanneer een toepassing zoals een indexzoekopdracht of een virusscan wordt gebruikt om het scannen van de gegevens die zijn opgeslagen op de StorSimple, moet u ervoor te zorgen dat de cloudgegevens niet ophalen geopend en terug naar de lokale laag worden opgehaald.

U wordt aangeraden dat u de volgende best practices implementeren bij het configureren van de zoekopdracht of virus indexscan op uw virtuele matrix:

* Schakel volledige scan automatisch geconfigureerde bewerkingen.
* Voor gelaagde volumes, de index zoekopdracht of virus scan toepassing configureren een incrementele scan uit te voeren. Dit zou scannen alleen de nieuwe gegevens waarschijnlijk die zich op de lokale laag. De gegevens die naar de cloud is gelaagd is niet toegankelijk tijdens een incrementele bewerking.
* Zorg ervoor dat de juiste zoekresultaten filters en instellingen worden geconfigureerd zodat alleen de beoogde typen bestanden gescand ophalen. Bijvoorbeeld: afbeeldingsbestanden (JPEG, GIF- en TIFF-) en engineering tekeningen moet niet worden gescand tijdens de incrementele of volledige index opnieuw maken.

Als Windows indexeren proces wordt gebruikt, volg deze richtlijnen:

* Gebruik de Windows-indexeerfunctie niet voor gelaagde volumes, zoals het grote hoeveelheden gegevens (TBs) teruggehaald uit de cloud de index moet regelmatig opnieuw worden opgebouwd. Bouw de index opnieuw zou alle bestandstypen voor het indexeren van hun inhoud ophalen.
* Gebruik het Windows-proces voor lokaal vastgemaakte volumes indexeren, omdat dit zou alleen toegang heeft tot gegevens op de lokale lagen voor het bouwen van de index (de cloudgegevens kan niet worden bereikt).

### <a name="byte-range-locking"></a>Vergrendelen van een byte-bereik
Toepassingen kunnen een opgegeven aantal bytes vergrendeld binnen de bestanden. Als byte bereik vergrendeling is ingeschakeld op de toepassingen die zijn schrijven naar uw StorSimple, werkt klikt u vervolgens lagen niet op uw virtuele matrix. Voor de lagen om te werken, moeten alle gebieden van de bestanden die benaderd worden ontgrendeld. Byte bereik vergrendelen wordt niet ondersteund met gelaagde volumes op uw virtuele matrix.

Aanbevolen maatregelen te verlichten byte bereik vergrendelen omvatten:

* Bereik aan bytes is vergrendeld in uw toepassingslogica uitschakelen.
* Gebruik lokaal vastgemaakte volumes (in plaats van gelaagd) voor de gegevens die zijn gekoppeld aan deze toepassing. Lokaal vastgemaakte volumes niet in de cloud gegevenslaag vormt.
* Wanneer met behulp van de volumes lokaal vastgemaakt met byte-bereik vergrendeling is ingeschakeld, kan het volume online worden voordat de herstelbewerking voltooid is. In dergelijke gevallen moet u wachten voor het terugzetten van zijn voltooid.

## <a name="multiple-arrays"></a>Meerdere matrices
Matrices met meerdere virtuele moet mogelijk worden geïmplementeerd voor een account voor een groeiende werkset van gegevens die naar de cloud zo beïnvloeden de prestaties van het apparaat kan worden gelekt. In dergelijke gevallen is het is raadzaam op schaal apparaten wanneer de werkset groeit. Hiervoor moet een of meer apparaten moeten worden toegevoegd in de on-premises Datacenter. Wanneer u de apparaten toevoegt, kunt u het volgende doen:

* De huidige set gegevens splitsen.
* Nieuwe werkbelastingen implementeert op nieuwe apparaten.
* Als u meerdere virtuele matrices implementeert, wordt aangeraden van taakverdeling perspectief wordt de matrix verdelen over andere hypervisorhosts.
* Meerdere virtuele matrices (indien geconfigureerd als een bestandsserver of een iSCSI-server) kunnen worden geïmplementeerd in een Namespace Distributed File System. Ga voor gedetailleerde stappen naar [Distributed File System Namespace oplossing met hybride Cloud-opslag Deployment Guide](https://www.microsoft.com/download/details.aspx?id=45507). Distributed File System Replication wordt momenteel niet aanbevolen voor gebruik met de virtuele-matrix. 

## <a name="see-also"></a>Zie ook
Meer informatie over hoe [beheren van uw virtuele StorSimple-matrix](storsimple-virtual-array-manager-service-administration.md) via de StorSimple Manager-service.


---
title: Wat is StorSimple Snapshot Manager? | Microsoft Docs
description: Beschrijft de StorSimple Snapshot Manager, de architectuur en functies.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38c197c7bc57110b29b1d8cb789d5b7310823da2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>Een inleiding tot StorSimple Snapshot Manager

## <a name="overview"></a>Overzicht
StorSimple Snapshot Manager is een module Microsoft Management Console (MMC) die vereenvoudigt de bescherming van gegevens en back-beheer in een Microsoft Azure StorSimple-omgeving. Met StorSimple Snapshot Manager beheren Microsoft Azure StorSimple-gegevens in het datacenter en in de cloud als een oplossing voor één geïntegreerde opslag, dus back-processen te vereenvoudigen en de kosten te verlagen.

Dit overzicht StorSimple Snapshot Manager introduceert, worden de functies beschreven en wordt uitgelegd de rol in Microsoft Azure StorSimple. 

Zie voor een overzicht van het gehele Microsoft Azure StorSimple-systeem, inclusief het StorSimple-apparaat, StorSimple Manager-service, StorSimple Snapshot Manager en StorSimple-Adapter voor SharePoint, [StorSimple 8000-serie: een hybride cloud-opslagoplossing](storsimple-overview.md). 

> [!NOTE]
> * U kunt StorSimple Snapshot Manager niet gebruiken voor het beheren van virtuele matrices van Microsoft Azure StorSimple (ook wel bekend als StorSimple on-premises virtuele apparaten).
> * Als u van plan bent te StorSimple Update 2 op uw StorSimple-apparaat installeert, moet u de meest recente versie van StorSimple Snapshot Manager downloaden en installeren **voordat u het StorSimple Update 2 installeert**. De meest recente versie van StorSimple Snapshot Manager is achterwaarts compatibel en werkt met alle uitgebrachte versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u bij te werken (u hoeft niet te verwijderen van de vorige versie voordat u de nieuwe versie installeert).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager doel en architectuur
StorSimple Snapshot Manager biedt een centrale beheerconsole maken consistent is, kunt u back-ups van de punt in tijd van lokale en cloudgegevens. U kunt bijvoorbeeld de console om te gebruiken:

* Configureer back-up en volumes verwijderen.
* Configureer volume groepen om ervoor te zorgen dat de back-upgegevens toepassingsconsistente is.
* Back-upbeleid beheren zodat de back-up op een vooraf vastgestelde planning.
* Maken van lokale en cloud worden opgeslagen die kunnen worden opgeslagen in de cloud en gebruikt voor herstel na noodgevallen.

De StorSimple Snapshot Manager haalt de lijst met toepassingen die zijn geregistreerd bij de VSS-provider op de host. Vervolgens voor het maken van toepassingsconsistente back-ups, controleert de volumes die door een toepassing gebruikt en wordt voorgesteld volume groepen te configureren. StorSimple Snapshot Manager maakt gebruik van deze groepen volume back-ups die toepassingsconsistente zijn genereren. (Consistentie van de toepassing bestaat als alle bestanden bijbehorende en de databases zijn gesynchroniseerd en vertegenwoordigen de werkelijke status van de toepassing op een bepaald punt in tijd). 

StorSimple Snapshot Manager back-ups nemen de vorm van incrementele momentopnamen die alleen de wijzigingen vast te sinds de laatste back-up leggen. Als gevolg hiervan worden back-ups vereisen minder opslagruimte en gemaakt en snel worden hersteld. StorSimple Snapshot Manager maakt gebruik van Windows Volume Shadow Copy Service (VSS) om ervoor te zorgen dat momentopnamen van toepassingsconsistente gegevens vastleggen. (Voor meer informatie gaat u naar de integratie met Windows Volume Shadow Copy Service-sectie.) Met StorSimple Snapshot Manager kunt u back-upschema's maken of directe back-ups uitvoeren indien nodig. Als u wilt gegevens terugzetten vanaf een back-up, StorSimple Snapshot Manager kunt selecteren u uit een catalogus van de lokale of cloudmomentopnamen. Azure StorSimple herstelt alleen de gegevens die nodig is deze nodig is, waardoor vertragingen in de beschikbaarheid van gegevens tijdens de herstelbewerkingen.)

![StorSimple Snapshot Manager-architectuur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager-architectuur** 

## <a name="support-for-multiple-volume-types"></a>Ondersteuning voor meerdere volumetypen
U kunt het StorSimple Snapshot Manager configureren en back-up van de volgende soorten volumes: 

* **Basisvolumes** – een standaardvolume wordt één partitie op een standaardschijf zijn. 
* **Eenvoudige volumes** – een eenvoudig volume is een dynamisch volume met de schijfruimte van één dynamische schijf. Een eenvoudig volume bestaat uit één regio op een schijf of meerdere regio's die op dezelfde schijf zijn gekoppeld. (U kunt eenvoudige volumes maken alleen op dynamische schijven.) Eenvoudige volumes zijn niet fouttolerant.
* **Dynamische volumes** – een dynamisch volume is een volume dat is gemaakt op een dynamische schijf. Dynamische schijven gebruiken een database voor het bijhouden van gegevens over de volumes die zijn opgenomen op de dynamische schijven op een computer. 
* **Dynamische volumes met mirroring** – dynamische volumes met mirroring zijn gebouwd op de RAID 1-architectuur. Met RAID 1 identieke gegevens op twee of meer schijf, het opstellen van een gespiegelde set geschreven. Een leesaanvraag kan vervolgens worden verwerkt door een schijf met de aangevraagde gegevens.
* **Gedeelde clustervolumes** – met gedeelde clustervolumes (CSV's), meerdere knooppunten in een failover-cluster kan tegelijkertijd lezen of schrijven naar dezelfde schijf. Failover van één knooppunt naar een ander knooppunt, kan optreden snel, zonder een wijziging in eigendom van het station of koppelen, ontkoppelen en verwijderen van een volume. 

> [!IMPORTANT]
> Niet door elkaar op CSV's en niet-CSV in de dezelfde momentopname. De combinatie van CSV's en niet-CSV in een momentopname wordt niet ondersteund. 
> 
> 

StorSimple Snapshot Manager kunt u herstellen hele volume groepen of afzonderlijke volumes klonen en afzonderlijke bestanden te herstellen.

* [Volumes en volume-groepen](#volumes-and-volume-groups) 
* [Back-uptypen en back-upbeleid](#backup-types-and-backup-policies) 

Zie voor meer informatie over StorSimple Snapshot Manager-functies en het gebruik ervan [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Volumes en volume-groepen
Met StorSimple Snapshot Manager volumes maken en deze vervolgens in volume groepen te configureren. 

Volume groepen StorSimple Snapshot Manager gebruikt voor het maken van back-ups die toepassingsconsistente zijn. Consistentie van de toepassing bestaat wanneer alle bestanden bijbehorende en databases zijn gesynchroniseerd en de waarde true status van een toepassing op een bepaald punt in tijd weer. Volume groepen (die ook wel bekend als zijn *consistentie groepen*) de basis vormen van een back-up of herstellen van de taak.

Volume-groepen zijn niet hetzelfde zijn als volumecontainers. Een volumecontainer bevat een of meer volumes die delen van een cloud-opslagaccount en andere kenmerken, zoals versleuteling en bandbreedte. Een container voor één volume mag maximaal 256 thin provisioning StorSimple-volumes. Voor meer informatie over volumecontainers, gaat u naar [beheren van uw volumecontainers](storsimple-manage-volume-containers.md). Volume-groepen zijn verzamelingen van volumes die u configureert voor back-upbewerkingen vergemakkelijken. Als u twee volumes die behoren tot verschillende volumecontainers, plaats deze in een groep één volume en maak vervolgens een back-upbeleid voor de groep volume selecteert, elk volume wordt een back-up in de container relevante volume, met het account de juiste opslag.

> [!NOTE]
> Alle volumes in een groep volume moeten afkomstig zijn van een serviceprovider één cloud.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integratie met Windows Volume Shadow Copy-Service
StorSimple Snapshot Manager maakt gebruik van Windows Volume Shadow Copy Service (VSS) om vast te leggen van toepassingsconsistente gegevens. VSS zorgt voor een toepassing consistente door te communiceren met VSS-compatibele toepassingen voor het coördineren van het maken van incrementele momentopnamen. VSS zorgt ervoor dat de toepassingen tijdelijk niet actief of quiescent, wanneer momentopnamen worden gemaakt. 

De implementatie van StorSimple Snapshot Manager van VSS werkt met SQL Server- en algemene NTFS-volumes. Het proces is als volgt: 

1. Een aanvrager die normaal gesproken een gegevensbeheer en de oplossing voor gegevensbeveiliging (zoals StorSimple Snapshot Manager) of een back-uptoepassing, roept VSS en wordt u gevraagd deze voor het verzamelen van informatie van de schrijver-software in de doeltoepassing.
2. VSS contact op met de schrijver-onderdeel voor het ophalen van een beschrijving van de gegevens. De writer retourneert de beschrijving van de gegevens naar een back-up. 
3. VSS geeft de schrijver voor het voorbereiden van de toepassing voor back-up. De writer bereidt de gegevens voor back-up door te voeren van geopende transacties voor het bijwerken van transactielogboeken, enzovoort, en wordt vervolgens VSS.
4. VSS Hiermee geeft u de writer tijdelijk stoppen van de toepassing gegevensarchieven en zorg ervoor dat er geen gegevens naar het volume wordt geschreven terwijl de schaduwkopie is gemaakt. Deze stap zorgt ervoor dat de consistentie van de gegevens, en wordt niet meer dan 60 seconden.
5. VSS Hiermee geeft u de provider voor het maken van de schaduwkopie. Providers, wat kunnen software of hardware-gebaseerde, beheren de volumes die momenteel worden uitgevoerd en maken van schaduwkopieën van deze op aanvraag. De provider maakt de schaduwkopie en VSS waarschuwt wanneer dit is voltooid.
6. VSS contact op met de schrijver van de toepassing die i/o kunt hervatten melden en om te bevestigen dat i/o is onderbroken tijdens de aanmaak van schaduwkopieën. 
7. Als de kopie voltooid is, retourneert VSS locatie van het exemplaar naar de aanvrager. 
8. Als de gegevens zijn geschreven terwijl de schaduwkopie is gemaakt, wordt de back-up inconsistent worden. VSS wordt verwijderd van de schaduwkopie en ontvangt een melding van de aanvrager. De aanvrager kan het back-upproces automatisch herhalen of contact met de netwerkbeheerder om het op een later tijdstip opnieuw uit te.

Zie de volgende afbeelding.

![VSS-proces](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Volume Shadow Copy-Service verwerken** 

## <a name="backup-types-and-backup-policies"></a>Back-uptypen en back-upbeleid
Met StorSimple Snapshot Manager kunt u back-up van gegevens en sla deze lokaal en in de cloud. U kunt StorSimple Snapshot Manager onmiddellijk een back-up van gegevens of kunt u een back-upbeleid maken om een planning voor het automatisch maken van back-ups te maken. Back-upbeleid kunnen u opgeven hoeveel momentopnamen behouden blijft. 

### <a name="backup-types"></a>Back-uptypen
StorSimple Snapshot Manager kunt u de volgende typen van back-ups maken:

* **Lokale momentopnamen** : lokale momentopnamen zijn punt in tijd kopieën van volumegegevens die zijn opgeslagen op het StorSimple-apparaat. Normaal gesproken worden deze type back-up gemaakt en snel worden hersteld. Net als een lokale back-up, kunt u een lokale momentopname gebruiken.
* **Momentopnamen cloud** – cloudmomentopnamen zijn punt in tijd kopieën van volumegegevens die zijn opgeslagen in de cloud. Een cloudmomentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn bijzonder nuttig voor herstel na noodgevallen.

### <a name="on-demand-and-scheduled-backups"></a>Op aanvraag en geplande back-ups
Met StorSimple Snapshot Manager kunt u een eenmalige back-up worden gemaakt onmiddellijk starten of kunt u een back-upbeleid terugkerende back-ups plannen.

Een back-upbeleid is een set van geautomatiseerde regels die u regelmatig back-ups kunt gebruiken. Een back-upbeleid kunt u de frequentie en de parameters op voor het maken van momentopnamen van een specifieke volume groep definiëren. U kunt beleidsregels gebruiken om op te geven het begin- en verloopdatum datums, tijden, frequenties en vereisten voor de bewaarperiode, van lokale en cloud worden opgeslagen. Een beleid wordt toegepast onmiddellijk nadat u deze variabele definiëren. 

StorSimple Snapshot Manager kunt u configureren of back-upbeleid indien nodig opnieuw te configureren. 

U configureert de volgende informatie voor elke back-upbeleid die u maakt:

* **Naam** – de unieke naam van de geselecteerde back-upbeleid.
* **Type** : het type back-upbeleid; lokale momentopname of cloudmomentopname.
* **Volume groep** – de volume-groep waaraan de geselecteerde back-upbeleid is toegewezen.
* **Bewaartermijn** : het aantal back-ups wilt bewaren. Als u de **alle** vak alle back-ups worden bewaard totdat het maximum aantal back-ups per volume is bereikt, waarna het beleid mislukken en er een foutbericht weergegeven. U kunt ook opgeven dat een aantal back-ups behouden (tussen 1 en 64).
* **Datum** – de datum waarop de back-upbeleid is gemaakt.

Voor informatie over het configureren van de back-upbeleid, gaat u naar [gebruik StorSimple Snapshot Manager maken en beheren van back-upbeleid](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Back-uptaak bewaking en beheer
U kunt StorSimple Snapshot Manager gebruiken om te controleren en beheren van toekomstige geplande en voltooide back-uptaken. Daarnaast biedt StorSimple Snapshot Manager een catalogus van maximaal 64 voltooide back-ups. U kunt de catalogus gebruiken om te zoeken en herstellen van volumes of afzonderlijke bestanden. 

Ga voor informatie over het controleren van de back-uptaken naar [gebruik StorSimple Snapshot Manager weergeven en beheren van back-uptaken](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple Snapshot Manager gebruiken voor het beheer van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Download [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).


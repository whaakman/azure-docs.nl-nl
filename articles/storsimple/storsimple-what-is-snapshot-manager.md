---
title: Wat is StorSimple Snapshot Manager? | Microsoft Docs
description: Beschrijving van de StorSimple Snapshot Manager, de architectuur en de functies ervan.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3f7436bb63f52c9c2b697c8e7031922ce89d786b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60789595"
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>Een inleiding tot StorSimple Snapshot Manager

## <a name="overview"></a>Overzicht
StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module waarmee de beveiliging van gegevens en back-upbeheer in een omgeving met Microsoft Azure StorSimple vereenvoudigd. Met StorSimple Snapshot Manager kunt u beheren Microsoft Azure StorSimple-gegevens in het datacenter en in de cloud als een enkel geïntegreerde opslagoplossing, waardoor de back-processen te vereenvoudigen en kosten te verlagen.

In dit overzicht introduceert StorSimple Snapshot Manager, worden de functies beschreven en wordt uitgelegd van de rol in Microsoft Azure StorSimple. 

Zie voor een overzicht van het gehele Microsoft Azure StorSimple-systeem, met inbegrip van het StorSimple-apparaat, de StorSimple Manager-service, de StorSimple Snapshot Manager en de StorSimple-Adapter voor SharePoint, [StorSimple 8000-serie: een hybride cloud opslagoplossing](storsimple-overview.md). 

> [!NOTE]
> * U kunt geen StorSimple Snapshot Manager gebruiken voor het beheren van Microsoft Azure StorSimple-matrices (ook wel bekend als StorSimple on-premises virtuele apparaten).
> * Als u van plan bent voor het StorSimple Update 2 installeren op uw StorSimple-apparaat, moet u de meest recente versie van StorSimple Snapshot Manager download en installeer deze **voordat u het StorSimple Update 2 installeren**. De meest recente versie van StorSimple Snapshot Manager achterwaarts compatibel is en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple Snapshot Manager gebruikt, moet u bij te werken (u hoeft niet de vorige versie verwijderen voordat u de nieuwe versie installeert).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager-doel en architectuur
StorSimple Snapshot Manager biedt een centraal beheer-console die u gebruiken kunt om te maken consistent is, point-in-time back-ups van lokale gegevens en cloudgegevens. U kunt bijvoorbeeld de-console te gebruiken:

* Configureren van back-up en volumes verwijderen.
* Configureer volumegroepen om ervoor te zorgen dat back-upgegevens toepassingsconsistent is.
* Back-upbeleid beheren zodat de back-up op een vooraf vastgestelde planning.
* Maken van lokale en cloud-momentopnamen, dat kunnen worden opgeslagen in de cloud en kan worden gebruikt voor herstel na noodgevallen.

StorSimple Snapshot Manager haalt de lijst met toepassingen die zijn geregistreerd bij de VSS-provider op de host. Vervolgens voor het maken van toepassingsconsistente back-ups, controleert de volumes die worden gebruikt door een toepassing en stelt volumegroepen te configureren. StorSimple Snapshot Manager maakt gebruik van deze volumegroepen voor het genereren van back-ups die toepassingsconsistente zijn. (Consistentie van toepassingen bestaat als alle bestanden bijbehorende en -databases zijn gesynchroniseerd en de waarde true status van de toepassing op een bepaald punt in tijd weer.) 

StorSimple Snapshot Manager back-ups nemen de vorm van incrementele momentopnamen, die alleen de wijzigingen worden vastgelegd sinds de laatste back-up. Als gevolg hiervan kunnen back-ups vereisen minder opslag en worden gemaakt en hersteld snel. StorSimple Snapshot Manager maakt gebruik van de Windows Volume Shadow Copy Service (VSS) om ervoor te zorgen dat momentopnamen van toepassingsconsistente gegevens vastleggen. (Voor meer informatie gaat u naar de integratie met Windows Volume Shadow Copy-Service-sectie.) Met StorSimple Snapshot Manager kunt u back-upschema's maken of onmiddellijke back-ups nemen indien nodig. Als u wilt gegevens terugzetten vanaf een back-up, StorSimple Snapshot Manager kunt selecteren u uit een catalogus van lokale of cloudmomentopnamen. Azure StorSimple worden hersteld door alleen de gegevens die nodig zijn als nodig is, waardoor vertragingen in de beschikbaarheid van gegevens tijdens herstelbewerkingen.)

![StorSimple Snapshot Manager-architectuur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager-architectuur** 

## <a name="support-for-multiple-volume-types"></a>Ondersteuning voor meerdere typen
U kunt StorSimple Snapshot Manager gebruiken om te configureren en back-up van de volgende typen volumes: 

* **Eenvoudige volumes** – een standaardvolume wordt één partitie op een standaardschijf. 
* **Eenvoudige volumes** – een eenvoudig volume is een dynamisch volume met de schijfruimte van één dynamische schijf. Een eenvoudig volume bestaat uit een enkele regio op een schijf of meerdere regio's die zijn gekoppeld op dezelfde schijf. (U kunt eenvoudige volumes maken op de dynamische schijven.) Eenvoudige volumes zijn niet fouttolerant.
* **Dynamische volumes** – een dynamisch volume is een volume op een dynamische schijf gemaakt. Dynamische schijven gebruiken een database voor het bijhouden van gegevens over de volumes die zijn opgenomen op de dynamische schijven in een computer. 
* **Dynamische volumes met mirroring** : dynamische volumes met mirroring zijn gebouwd op de RAID 1-architectuur. Identieke gegevens worden geschreven met RAID-1, op twee of meer schijf, een gespiegelde set produceren. Een leesaanvraag kan vervolgens worden verwerkt door een schijf met de aangevraagde gegevens.
* **Gedeelde clustervolumes** – met gedeelde clustervolumes (CSV's), meerdere knooppunten in een failover-cluster kan tegelijkertijd lezen of schrijven op dezelfde schijf. Failover van één knooppunt naar een ander knooppunt vindt snel, zonder een wijziging in eigendom van het station of koppelen, ontkoppelen en verwijderen van een volume. 

> [!IMPORTANT]
> Geen verschillende CSV's en niet-CSV-'s in dezelfde momentopname overigens. Met een combinatie van CSV's en niet-CSV-'s in een momentopname wordt niet ondersteund. 
> 
> 

StorSimple Snapshot Manager kunt u groepen van gehele volume herstellen of afzonderlijke volumes klonen en herstellen van afzonderlijke bestanden.

* [Volumes en volumegroepen](#volumes-and-volume-groups) 
* [Back-uptypen en back-upbeleid](#backup-types-and-backup-policies) 

Zie voor meer informatie over StorSimple Snapshot Manager-functies en het gebruik ervan [StorSimple Snapshot Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Volumes en volumegroepen
Met StorSimple Snapshot Manager volumes maken en deze vervolgens configureren in volumegroepen. 

StorSimple Snapshot Manager maakt gebruik van volumegroepen te maken van back-ups die toepassingsconsistente zijn. Consistentie van toepassingen bestaat als alle bestanden bijbehorende en -databases zijn gesynchroniseerd en de waarde true status van een toepassing op een bepaald punt in tijd weer. Volumegroepen (die ook wel bekend als zijn *consistentiegroepen*) vormen de basis van een back-up of herstellen van de taak.

Volumegroepen zijn niet gelijk zijn aan volumecontainers. Een volumecontainer bevat een of meer volumes die een cloudopslagaccount en andere kenmerken, zoals versleuteling en bandbreedte delen. Een enkel volumecontainer mag maximaal 256 dun ingerichte StorSimple-volumes. Voor meer informatie over volumecontainers, gaat u naar [uw volumecontainers beheren](storsimple-manage-volume-containers.md). Volumegroepen zijn verzamelingen van volumes die u configureert voor back-upbewerkingen vergemakkelijken. Als u twee volumes die deel uitmaken van verschillende volumecontainers, plaats deze in een groep één volume en maak vervolgens een back-upbeleid voor de volumegroep van dat selecteert, elk volume back-ups in de juiste volumecontainer, met behulp van het gewenste opslagaccount.

> [!NOTE]
> Alle volumes in de volumegroep van een moeten afkomstig zijn van een enkele cloudserviceprovider.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integratie met Windows Volume Shadow Copy-Service
StorSimple Snapshot Manager maakt gebruik van de Windows Volume Shadow Copy Service (VSS) om vast te leggen toepassingsconsistente gegevens. VSS vereenvoudigt het uitvoeren van consistentie van toepassingen door te communiceren met de VSS-bewuste toepassingen voor de coördinatie van het maken van incrementele momentopnamen. VSS zorgt ervoor dat de toepassingen tijdelijk niet-actieve of quiescent, wanneer momentopnamen worden gemaakt. 

De StorSimple Snapshot Manager-implementatie van VSS werkt met SQL Server en algemene NTFS-volumes. Het proces is als volgt: 

1. Een aanvrager die normaal gesproken een gegevensbeheer en oplossing voor gegevensbeveiliging (zoals StorSimple Snapshot Manager) of een back-uptoepassing, VSS aanroept en vraag deze om informatie te verzamelen van de schrijver-software in de doeltoepassing.
2. VSS neemt contact op met het onderdeel writer om op te halen van een beschrijving van de gegevens. De writer retourneert de beschrijving van de gegevens naar de back-up worden gemaakt. 
3. VSS geeft de schrijver voor de toepassing voorbereiden voor back-up. De schrijver bereidt de gegevens voor back-up door het uitvoeren van open transacties bijwerken transactielogboeken, enzovoort, en wordt vervolgens VSS.
4. VSS Hiermee geeft u de writer tijdelijk stoppen van de toepassing gegevensarchieven en zorg ervoor dat er geen gegevens worden geschreven naar het volume terwijl de schaduwkopie is gemaakt. Deze stap zorgt ervoor dat de consistentie van gegevens en het duurt niet meer dan 60 seconden.
5. VSS Hiermee geeft u de provider te maken van de schaduwkopie. Providers, dit kunnen software of hardware-gebaseerde, beheren de volumes die momenteel wordt uitgevoerd en maken van schaduwkopieën van deze on-demand. De provider maakt de schaduwkopie en VSS ontvangt een melding wanneer deze is voltooid.
6. VSS neemt contact op met de schrijver van de toepassing die i/o kunt hervatten informeert en ook om te bevestigen dat i/o is onderbroken tijdens het maken van schaduwkopie is. 
7. Als de kopie voltooid is, retourneert VSS locatie van de kopie naar de aanvrager. 
8. Als de gegevens zijn geschreven terwijl de schaduwkopie is gemaakt, wordt de back-up niet consistent zijn. VSS Hiermee verwijdert u de schaduwkopie en ontvangt een melding naar de aanvrager verstuurt. De aanvrager kan het back-upproces automatisch herhalen of contact op met de beheerder om deze op een later tijdstip opnieuw uit te.

Zie de volgende afbeelding.

![VSS-proces](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Volume Shadow Copy-Service verwerken** 

## <a name="backup-types-and-backup-policies"></a>Back-uptypen en back-upbeleid
Met StorSimple Snapshot Manager kunt u back-up van gegevens en lokaal en in de cloud opslaan. U kunt StorSimple Snapshot Manager gebruiken voor back-up van gegevens direct of kunt u een back-upbeleid om een planning voor het automatisch maken van back-ups te maken. Back-upbeleid kunnen u opgeven hoeveel momentopnamen worden bewaard. 

### <a name="backup-types"></a>Back-uptypen
StorSimple Snapshot Manager kunt u de volgende typen back-ups maken:

* **Lokale momentopnamen** : lokale momentopnamen zijn point-in-time-kopieën van volumegegevens die zijn opgeslagen op het StorSimple-apparaat. Normaal gesproken worden dit type back-up gemaakt en hersteld snel. U kunt een momentopname van een lokale kunt gebruiken, net als een lokale back-up.
* **Cloud-momentopnamen** – Cloud-momentopnamen zijn point-in-time-kopieën van volumegegevens die zijn opgeslagen in de cloud. Een cloud-momentopname is gelijk aan een momentopname op een andere, externe opslagsysteem gerepliceerd. Cloudmomentopnamen zijn met name handig voor herstel na noodgevallen.

### <a name="on-demand-and-scheduled-backups"></a>On-demand en geplande back-ups
Met StorSimple Snapshot Manager kunt u een eenmalige back-up moet onmiddellijk worden gemaakt, starten of u kunt een back-upbeleid gebruiken om te plannen, periodieke back-upbewerkingen.

Een back-upbeleid is een reeks geautomatiseerde regels die u gebruiken kunt voor het plannen van regelmatige back-ups. Een back-upbeleid kunt u de frequentie en de parameters voor het maken van momentopnamen van een bepaald volumegroep definiëren. U kunt beleidsregels gebruiken om op te geven van de begin- en verloopdatum datums, tijden, frequenties en vereisten voor de bewaarperiode, voor zowel lokale en cloud-momentopnamen. Een beleid wordt toegepast, onmiddellijk nadat u deze hebt gedefinieerd. 

StorSimple Snapshot Manager kunt u configureren of back-upbeleid indien nodig opnieuw te configureren. 

U configureert voor elke back-upbeleid dat u maakt de volgende informatie:

* **Naam** : de unieke naam van de geselecteerde back-upbeleid.
* **Type** : het type back-upbeleid; lokale momentopname of cloud-momentopname.
* **Volumegroep** – de volumegroep waaraan de geselecteerde back-upbeleid is toegewezen.
* **Retentie** : het aantal back-ups te behouden. Als u het selectievakje de **alle** vak alle back-upexemplaren worden bewaard totdat het maximum aantal back-ups per volume is bereikt, op het moment waarop het beleid mislukken en er een foutbericht weergegeven. U kunt ook een aantal back-ups behouden (tussen 1 en 64) opgeven.
* **Datum** : de datum waarop de back-upbeleid is gemaakt.

Voor informatie over het configureren van back-upbeleid, gaat u naar [gebruikt StorSimple Snapshot Manager maken en beheren van back-upbeleid](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Back-uptaak controle en beheer
U kunt StorSimple Snapshot Manager gebruiken om te controleren en beheren van toekomstige, geplande en voltooide back-uptaken. Daarnaast biedt StorSimple Snapshot Manager een catalogus van maximaal 64 voltooide back-ups. U kunt de catalogus gebruiken om te zoeken en herstellen van volumes of afzonderlijke bestanden. 

Ga voor informatie over het controleren van de back-uptaken naar [gebruikt StorSimple Snapshot Manager weergeven en beheren van back-uptaken](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple Snapshot Manager gebruiken voor het beheren van uw StorSimple-oplossing](storsimple-snapshot-manager-admin.md).
* Download [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).


---
title: Beheren van uw StorSimple-volumes | Microsoft Docs
description: Legt uit hoe toevoegen, wijzigen, bewaken en StorSimple-volumes verwijderen, en hoe offline te nemen ze indien nodig.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 31ed9dad8ba56a3746873b7b35e678e97743fbfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>De StorSimple Manager-service gebruiken voor het beheren van volumes
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe u de StorSimple Manager-service maken en beheren van volumes op de StorSimple-apparaat en het virtuele StorSimple-apparaat.

De StorSimple Manager-service is een uitbreiding van de klassieke Azure portal waarmee u uw StorSimple-oplossing beheren via een enkel webinterface. Naast het beheren van volumes, kunt u de StorSimple Manager-service te maken en beheren van StorSimple-services, weergeven en beheren van apparaten, waarschuwingen, weergeven en weergeven en beheren van back-upbeleid en de back-upcatalogus.

> [!NOTE]
> Azure StorSimple kunt alleen thin ingerichte volumes maken. U kunt geen maken volledig of gedeeltelijk ingerichte volumes op een Azure StorSimple-systeem.
> 
> Thin provisioning is een virtualisatietechnologie voor het waarin beschikbare opslag zijn dan de fysieke resources wordt weergegeven. In plaats van voldoende opslagruimte van tevoren reserveren, Azure StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. De elastische aard van cloud-opslag vergemakkelijkt deze benadering omdat Azure StorSimple kunt vergroten of verkleinen van cloud-opslag om te voldoen aan veranderende verzoeken.
> 
> 

## <a name="the-volumes-page"></a>De pagina Volumes
De **Volumes** pagina kunt u voor het beheren van de opslagvolumes die zijn ingericht op de Microsoft Azure StorSimple-apparaat voor uw initiators (servers). De lijst van volumes wordt weergegeven op uw StorSimple-apparaat.

 ![Pagina volumes](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Een volume bestaat uit een reeks van kenmerken:

* **Naam** – een beschrijvende naam die moet uniek en kunt u het volume te identificeren. Deze naam wordt ook gebruikt in de bewaking van rapporten, wanneer u op een bepaald volume filtert.
* **Status** – online of offline kan zijn. Als een volume als offline, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume.
* **Capaciteit** – Hiermee geeft u op hoe lang het volume is, zoals gezien door de initiator (server). Capaciteit geeft de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server). Volumes zijn dun ingericht en gegevens is ontdubbeld. Dit betekent dat uw apparaat niet het toewijzen van fysieke opslagcapaciteit intern of in de cloud volgens de geconfigureerde volumecapaciteit. De volumecapaciteit is toegewezen en op verzoek gebruikt.
* **Type** – het volumetype mag gelaagde of archivering (een subtype van gelaagd)
* **Toegang** – Hiermee geeft u de initiators (servers) die toegang tot dit volume hebben. Initiators die geen deel uitmaken van record voor toegangscontrole (ACR) die is gekoppeld aan het volume zichtbaar niet voor het volume.
* **Bewaking** – Hiermee wordt aangegeven of een volume wordt bewaakt. Een volume hebt bewaking standaard ingeschakeld wanneer deze wordt gemaakt. Wordt echter bewaking wordt uitgeschakeld voor de kloon van een volume. Volg de instructies in de Monitor een volume zodat bewaking voor een volume.

De meest algemene taken die zijn gekoppeld aan een volume zijn:

* Een volume toevoegen
* Een volume wijzigen
* Een volume verwijderen
* Een volume offline zetten
* Een volume bewaken

## <a name="add-a-volume"></a>Een volume toevoegen
U [gemaakt van een volume](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) tijdens de implementatie van uw StorSimple-oplossing. Toevoegen van een volume is een vergelijkbare procedure.

### <a name="to-add-a-volume"></a>Een volume toevoegen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer een volumecontainer en klik op de pijl in de bijbehorende rij voor toegang tot de volumes die zijn gekoppeld aan de container.
3. Klik op **toevoegen** aan de onderkant van de pagina. De wizard volume wordt gestart.
   
     ![Wizard volume basisinstellingen toevoegen](./media/storsimple-manage-volumes/AddVolume1.png)
4. Voer in de wizard onder **Basisinstellingen** de volgende stappen uit:
   
   1. Geef een **naam** op voor het volume.
   2. Geef de **ingerichte capaciteit** voor het volume in GB of TB op. De capaciteit moet liggen tussen 1 GB en 64 TB voor een fysiek apparaat. De maximale capaciteit die kan worden ingericht voor een volume op een virtueel StorSimple-apparaat is 30 TB.
   3. Selecteer de **gebruikstype** voor het volume. Als u het gelaagde volume voor archivering van gegevens gebruikt, selecteer de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje verandert de chunkgrootte Ontdubbeling voor het volume tot 512 KB. Als u deze optie niet selecteert, wordt het overeenkomstige gelaagde volume een chunkgrootte van 64 KB gebruikt. Een grotere chunkgrootte voor Ontdubbeling kunt het apparaat sneller de overdracht van grote archivering van gegevens naar de cloud. (Gelaagde volumes werden voorheen primaire volumes genoemd.)
   4. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)naar de **extra instellingen** pagina.
      
        ![Extra instellingen van de wizard Volume toevoegen](./media/storsimple-manage-volumes/AddVolume2.png)
5. Onder **extra instellingen**, Voeg een nieuwe record voor toegangscontrole (ACR):
   
   1. Selecteer een record voor toegangscontrole (ACR) in de vervolgkeuzelijst. U kunt ook een nieuwe ACR toevoegen. ACRs bepalen welke hosts kunnen toegang krijgen tot de volumes die overeenkomt met de host IQN met die vermeld in de record.
   2. Het is verstandig standaard een back-up te maken door het selectievakje **Een standaardback-up voor dit volume in schakelen** in te schakelen.
   3. Klik op het vinkje ![Vinkje](./media/storsimple-manage-volumes/HCS_CheckIcon.png) het volume te maken met de opgegeven instellingen.

Het nieuwe volume is nu klaar voor gebruik.

## <a name="modify-a-volume"></a>Een volume wijzigen
Een volume wijzigen wanneer u moet deze uitbreiden of wijzigen van de hosts die toegang hebben tot het volume.

> [!IMPORTANT]
> * Als u de grootte van het volume op het apparaat wijzigt, moet de grootte van het volume worden gewijzigd op de host en.
> * De hier beschreven stappen voor de host-zijde zijn voor Windows Server 2012 (2012R2). Procedures voor het Linux- of andere hostbesturingssystemen zullen afwijken. Raadpleeg de documentatie van uw host-besturingssysteem bij het wijzigen van het volume op een host waarop een ander besturingssysteem.
> 
> 

### <a name="to-modify-a-volume"></a>Een volume wijzigen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainer** tabblad. Deze pagina geeft een lijst in tabelvorm de volumecontainers die gekoppeld aan het apparaat zijn.
2. Selecteer een volumecontainer en klik om de lijst van alle volumes in de container weer te geven.
3. Op de **Volumes** pagina, selecteert u een volume en klik op **wijzigen**.
4. In de wizard Modify volume onder **basisinstellingen**, kunt u het volgende doen:
   
   * Bewerk de **naam** en **Type** als u een gelaagd volume op een volume archivering wijzigen wilt door de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje om te wijzigen de chunkgrootte Ontdubbeling voor het volume tot 512 KB.
   * Vergroot de **ingerichte capaciteit**. De **ingerichte capaciteit** kan alleen worden verhoogd. Nadat deze is gemaakt, kunt u een volume niet verkleinen.
     
     > [!NOTE]
     > U kunt de volumecontainer niet wijzigen nadat deze is toegewezen aan een volume.
     > 
     > 
5. Onder **extra instellingen**, kunt u het volgende doen:
   
   * Wijzig de ACRs voorwaarde dat het volume offline is. Als het volume online is, moet u eerst offline halen. Raadpleeg de stappen in [offline zetten van een volume](#take-a-volume-offline) voorafgaand aan de ACR wijzigen.
   * De lijst met ACRs wijzigen nadat het volume offline is.
     
     > [!NOTE]
     > U kunt niet wijzigen de **een standaardback-up voor dit volume inschakelen** optie voor het volume.
     > 
     > 
6. Sla de wijzigingen door te klikken op het vinkje ![vinkje](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Een bijwerken volume bericht weergegeven met de klassieke Azure portal. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.
7. Als u een volume uitbreidt, voert u de volgende stappen uit op uw Windows-hostcomputer:
   
   1. Ga naar **Computerbeheer** ->**Schijfbeheer**.
   2. Met de rechtermuisknop op **Schijfbeheer** en selecteer **schijven**.
   3. Selecteer het volume dat u hebt bijgewerkt, klik met de rechtermuisknop en selecteer in de lijst met schijven **Volume uitbreiden**. De wizard Volume uitbreiden wordt gestart. Klik op **Volgende**.
   4. Voltooi de wizard en accepteer de standaardwaarden. Nadat de wizard voltooid is, moet de grootte van de hogere het volume worden weergegeven.

![Video beschikbaar](./media/storsimple-manage-volumes/Video_icon.png) **Video beschikbaar**

Als u wilt een video over het uitbreiden van een volume, klikt u op [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Een volume offline zetten
Mogelijk moet u een volume offline zetten wanneer u van plan bent om te wijzigen of verwijderen. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet het volume offline nemen op de host, evenals op het apparaat. De volgende stappen voor het offline zetten van een volume uitvoeren.

### <a name="to-take-a-volume-offline"></a>Offline te nemen een volume
1. Zorg ervoor dat het betreffende volume niet gebruikt wordt voordat deze offline wordt gezet.
2. Het volume offline op de host eerst duren. Hierdoor wordt een potentieel risico van gegevensbeschadiging op het volume. Raadpleeg de instructies voor het besturingssysteem van de host voor specifieke stappen.
3. Nadat de host offline is, voert u het volume op het apparaat offline door de volgende stappen uit te voeren:
   
   1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad. De **Volumecontainers** tabblad lijst in tabelvorm de volumecontainers die gekoppeld aan het apparaat zijn.
   2. Selecteer een volumecontainer en klik om de lijst van alle volumes in de container weer te geven.
   3. Selecteer een volume en klik op **offline zetten**.
   4. Klik op **Ja** als u om bevestiging wordt gevraagd. Het volume worden nu offline.
      
      Nadat een volume offline is, de **Online brengen** optie beschikbaar.

> [!NOTE]
> De **Offline nemen** opdracht verzendt een aanvraag naar het apparaat offline te nemen het volume. Als het volume wordt nog steeds worden gebruikt door hosts, resulteert dit in verbroken verbindingen maar offline halen van het volume niet mislukken.
> 
> 

## <a name="delete-a-volume"></a>Een volume verwijderen
> [!IMPORTANT]
> U kunt een volume alleen verwijderen als deze offline is.
> 
> 

De volgende stappen voor het verwijderen van een volume.

### <a name="to-delete-a-volume"></a>Een volume verwijderen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer de volumecontainer met het volume dat u wilt verwijderen. Klik op de volumecontainer voor toegang tot de **Volumes** pagina.
3. Alle volumes die zijn gekoppeld aan deze container worden weergegeven in tabelvorm. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst de stappen in [offline zetten van een volume](#take-a-volume-offline).
4. Nadat het volume offline is, klikt u op **verwijderen** aan de onderkant van de pagina.
5. Klik op **Ja** als u om bevestiging wordt gevraagd. Het volume wordt nu verwijderd en de **Volumes** ziet u de bijgewerkte lijst van volumes in de container.

## <a name="monitor-a-volume"></a>Een volume bewaken
Bewaking van volume, kunt u I/O-gerelateerde statistische gegevens verzamelen voor een volume. Bewaking is standaard ingeschakeld voor de eerste 32 volumes die u maakt. Bewaking van extra volumes is standaard uitgeschakeld. Bewaking van de gekloonde volumes wordt ook standaard uitgeschakeld.

De volgende stappen uitvoeren om te of schakel bewaking voor een volume.

### <a name="to-enable-or-disable-volume-monitoring"></a>-Of uitschakelen volume bewaking
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer de volumecontainer waarin het volume zich bevindt en klik vervolgens op de volumecontainer voor toegang tot de **Volumes** pagina.
3. Alle volumes die zijn gekoppeld aan deze container worden vermeld in de weergegeven in de tabel. Klik op en selecteer het volume of een volume klonen.
4. Klik onder aan de pagina op **wijzigen**.
5. In de wizard Volume wijzigen onder **basisinstellingen**, selecteer **inschakelen** of **uitschakelen** van de **bewaking** vervolgkeuzelijst.
   
    ![Een volume basisinstellingen wijzigen](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [klonen van een StorSimple-volume](storsimple-clone-volume.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).


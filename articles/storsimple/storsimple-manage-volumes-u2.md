---
title: Beheren van uw StorSimple-volumes (U2) | Microsoft Docs
description: Legt uit hoe toevoegen, wijzigen, bewaken en StorSimple-volumes verwijderen, en hoe offline te nemen ze indien nodig.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: a61c57cd74a0df8363648dd8df40e433b0e6489d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>De StorSimple Manager-service gebruiken voor het beheren van volumes (Update 2)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt uitgelegd hoe u de StorSimple Manager-service maken en beheren van volumes op de StorSimple-apparaat en het virtuele StorSimple-apparaat met Update 2 is geïnstalleerd.

De StorSimple Manager-service is een uitbreiding in de klassieke Azure portal waarmee u uw StorSimple-oplossing beheren via een enkel webinterface. Naast het beheren van volumes, kunt u de StorSimple Manager-service te maken en beheren van StorSimple-services, weergeven en beheren van apparaten, waarschuwingen, weergeven en weergeven en beheren van back-upbeleid en de back-upcatalogus.

## <a name="volume-types"></a>Volumetypen
StorSimple-volumes kunnen zijn:

* **Lokaal vastgemaakte volumes**: gegevens in deze volumes blijft op de lokale StorSimple-apparaat te allen tijde.
* **Gelaagde volumes**: gegevens in deze volumes kunnen worden gelekt naar de cloud.

Een archivering volume is een gelaagd volume. De grotere chunkgrootte Ontdubbeling is gebruikt voor archivering volumes kunt het apparaat groter gegevenssegmenten overbrengen naar de cloud. 

Indien nodig, kunt u het volume van een lokale bron naar lagen of van lagen naar lokale. Ga voor meer informatie naar [wijzigen van het volumetype](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokaal vastgemaakte volumes
Lokaal vastgemaakte volumes zijn volledig ingerichte volumes die geen gegevens van de laag naar de cloud te doen, garanties waardoor lokale voor primaire gegevens, onafhankelijk van de verbinding van de cloud. Gegevens op lokaal vastgemaakte volumes is niet ontdubbeld en gecomprimeerd. momentopnamen van lokaal vastgemaakte volumes zijn echter ontdubbeld. 

Lokaal vastgemaakte volumes zijn volledig is ingericht; Daarom moet u voldoende ruimte hebben op uw apparaat wanneer u deze maakt. U kunt lokaal vastgemaakte volumes tot een maximale grootte van 8 TB op het StorSimple 8100-apparaat en 20 TB op het apparaat 8600 inrichten. StorSimple behoudt zich alle lokale ruimte op het apparaat voor momentopnamen, metagegevens en verwerken van gegevens. U kunt de grootte van een lokaal vastgemaakt volume om de maximale ruimte beschikbaar te verhogen, maar u kunt de grootte van een volume eenmaal is gemaakt niet verlagen.

Wanneer u een lokaal vastgemaakt volume maakt, wordt de beschikbare ruimte voor het maken van gelaagde volumes verminderd. Het omgekeerde geldt ook: als er bestaande gelaagde volumes, de ruimte die beschikbaar zijn voor het maken van lokaal vastgemaakte volumes wordt niet lager zijn dan de bovenstaande maximale grenzen. Raadpleeg voor meer informatie over lokale volumes, de [Veelgestelde vragen op lokaal vastgemaakte volumes](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Gelaagde volumes
Gelaagde volumes zijn dun ingerichte volumes waarin vaak gebruikte gegevens blijft lokale op het apparaat en minder vaak gebruikte gegevens automatisch naar de cloud is gelaagd. Thin provisioning is een virtualisatietechnologie voor het waarin beschikbare opslag zijn dan de fysieke resources wordt weergegeven. In plaats van voldoende opslagruimte van tevoren reserveren, StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. De elastische aard van cloud-opslag vergemakkelijkt deze benadering omdat StorSimple kunt vergroten of verkleinen van cloud-opslag om te voldoen aan veranderende verzoeken.

Als u het gelaagde volume voor archivering van gegevens gebruikt, selecteer de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje verandert de chunkgrootte Ontdubbeling voor het volume tot 512 KB. Als u deze optie niet selecteert, wordt het overeenkomstige gelaagde volume een chunkgrootte van 64 KB gebruikt. Een grotere chunkgrootte voor de ontdubbeling zorgt ervoor dat het apparaat sneller grotere archiveringsgegevens naar de cloud kan overzetten.

> [!NOTE]
> Archivering volumes die zijn gemaakt met een vooraf Update 2-versie van StorSimple worden geïmporteerd omdat er gelaagde met het selectievakje archivering ingeschakeld.
> 
> 

### <a name="provisioned-capacity"></a>Ingerichte capaciteit
Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk type apparaat en het volume. (Houd er rekening mee dat lokaal vastgemaakte volumes niet beschikbaar op een virtueel apparaat zijn.)

|  | De grootte van maximaal gelaagde volume | Maximale lokaal vastgemaakt volumegrootte |
| --- | --- | --- |
| **Fysieke apparaten** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuele apparaten** | | |
| 8010 |30 TB |N.v.t. |
| 8020 |64 TB |N.v.t. |

## <a name="the-volumes-page"></a>De pagina Volumes
De **Volumes** pagina kunt u voor het beheren van de opslagvolumes die zijn ingericht op de Microsoft Azure StorSimple-apparaat voor uw initiators (servers). De lijst van volumes wordt weergegeven op uw StorSimple-apparaat.

 ![Pagina volumes](./media/storsimple-manage-volumes-u2/VolumePage.png)

Een volume bestaat uit een reeks van kenmerken:

* **Volumenaam** – een beschrijvende naam die moet uniek en kunt u het volume te identificeren. Deze naam wordt ook gebruikt in de bewaking van rapporten, wanneer u op een bepaald volume filtert.
* **Status** – online of offline kan zijn. Als een volume offline is, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume.
* **Capaciteit** – Hiermee geeft u de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server). Lokaal vastgemaakte volumes volledig is ingericht en bevinden zich op het StorSimple-apparaat. Gelaagde volumes zijn dun ingericht en de gegevens is ontdubbeld. Met thin provisioning volumes toewijzen niet uw apparaat vooraf capaciteit van de fysieke opslag intern of in de cloud volgens de geconfigureerde volumecapaciteit. De volumecapaciteit is toegewezen en op verzoek gebruikt.
* **Type** – Hiermee wordt aangegeven of het volume **tiers verdeelde** (de standaardinstelling) of **lokaal vastgemaakt**.
* **Back-** – Hiermee wordt aangegeven of een back-standaardbeleid voor het volume bestaat.
* **Toegang** – Hiermee geeft u de initiators (servers) die toegang tot dit volume hebben. Initiators die geen deel uitmaken van record voor toegangscontrole (ACR) die is gekoppeld aan het volume zichtbaar niet voor het volume.
* **Bewaking** – Hiermee wordt aangegeven of een volume wordt bewaakt. Een volume hebt bewaking standaard ingeschakeld wanneer deze wordt gemaakt. Wordt echter bewaking wordt uitgeschakeld voor de kloon van een volume. Volg de instructies in zodat bewaking voor een volume [bewaken van een volume](#monitor-a-volume). 

Volg de instructies in deze zelfstudie aan de volgende taken uitvoeren:

* Een volume toevoegen 
* Een volume wijzigen 
* Het volumetype wijzigen
* Een volume verwijderen 
* Een volume offline zetten 
* Een volume bewaken 

## <a name="add-a-volume"></a>Een volume toevoegen
U [gemaakt van een volume](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) tijdens de implementatie van uw StorSimple-oplossing. Toevoegen van een volume is een vergelijkbare procedure.

#### <a name="to-add-a-volume"></a>Een volume toevoegen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer een volumecontainer in de lijst en dubbelklik erop om toegang te krijgen tot de volumes die zijn gekoppeld aan de container.
3. Klik op **toevoegen** aan de onderkant van de pagina. De wizard volume wordt gestart.
   
     ![Wizard volume basisinstellingen toevoegen](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. Voer in de wizard onder **Basisinstellingen** de volgende stappen uit:
   
   1. Geef een **naam** op voor het volume.
   2. Selecteer een **gebruikstype** uit de vervolgkeuzelijst. Selecteer voor werkbelastingen waarvoor gegevens worden lokaal beschikbaar op het apparaat te allen tijde, **lokaal vastgemaakt**. Selecteer voor alle andere typen gegevens, **tiers verdeelde**. (**Tiers verdeelde** is de standaardinstelling.)
   3. Als u hebt geselecteerd **tiers verdeelde** in stap 2, kunt u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje voor het configureren van een volume archivering.
   4. Voer de **ingerichte capaciteit** voor het volume in GB of TB. Zie [ingerichte capaciteit](#provisioned-capacity) voor de maximale grootte voor elk type apparaat en het volume. Bekijk de **beschikbare capaciteit** om te bepalen hoeveel opslagruimte op het apparaat daadwerkelijk beschikbaar is.
5. Klik op het pijlpictogram![Pictogram pijl](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Als u een lokaal vastgemaakt volume configureert, ziet u het volgende bericht.
   
    ![Tekstbericht Volume wijzigen](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)opnieuw om door te gaan naar de **extra instellingen** pagina.
   
    ![Extra instellingen van de wizard Volume toevoegen](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. Onder **extra instellingen**, Voeg een nieuwe record voor toegangscontrole (ACR):
   
   1. Selecteer een record voor toegangscontrole (ACR) in de vervolgkeuzelijst. U kunt ook een nieuwe ACR toevoegen. ACRs bepalen welke hosts kunnen toegang krijgen tot de volumes die overeenkomt met de host IQN met die vermeld in de record. Als u een ACR niet opgeeft, ziet u het volgende bericht.
      
        ![ACR opgeven](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Het is raadzaam dat u selecteert de **een standaardback-up voor dit volume inschakelen** selectievakje.
   3. Klik op het vinkje ![Vinkje](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) het volume te maken met de opgegeven instellingen.

Het nieuwe volume is nu klaar voor gebruik.

> [!NOTE]
> Als u een lokaal vastgemaakt volume maakt en vervolgens een andere lokaal vastgemaakt volume maakt onmiddellijk na het maken van het volume taken worden opeenvolgend uitgevoerd. De eerste taak voor het maken van volume moet zijn voltooid voordat de volgende taak voor het maken van volume kan beginnen.
> 
> 

## <a name="modify-a-volume"></a>Een volume wijzigen
Een volume wijzigen wanneer u moet deze uitbreiden of wijzigen van de hosts die toegang hebben tot het volume.

> [!IMPORTANT]
> * Als u de grootte van het volume op het apparaat wijzigt, moet de grootte van het volume worden gewijzigd op de host en. 
> * De hier beschreven stappen voor de host-zijde zijn voor Windows Server 2012 (2012R2). Procedures voor het Linux- of andere hostbesturingssystemen zullen afwijken. Raadpleeg de documentatie van uw host-besturingssysteem bij het wijzigen van het volume op een host waarop een ander besturingssysteem. 
> 
> 

#### <a name="to-modify-a-volume"></a>Een volume wijzigen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer een volumecontainer in de lijst en dubbelklik erop om de volumes die zijn gekoppeld aan de container weer te geven.
3. Selecteer een volume en klik onder aan de pagina op **wijzigen**. De wizard Modify volume wordt gestart.
4. In de wizard Modify volume onder **basisinstellingen**, kunt u het volgende doen:
   
   * Bewerk de **naam**.
   * Converteert de **gebruikstype** van lokaal vastgemaakt lagen naar of van lagen naar lokaal vastgemaakt (Zie [wijzigen van het volumetype](#change-the-volume-type) voor meer informatie).
   * Vergroot de **ingerichte capaciteit**. De **ingerichte capaciteit** kan alleen worden verhoogd. Nadat deze is gemaakt, kunt u een volume niet verkleinen.
5. Onder **extra instellingen**, kunt u de ACR voorwaarde dat het volume offline is. Als het volume online is, moet u eerst offline halen. Raadpleeg de stappen in [offline zetten van een volume](#take-a-volume-offline) voorafgaand aan de ACR wijzigen.
   
   > [!NOTE]
   > U kunt niet wijzigen de **een standaardback-up inschakelen** optie voor het volume.
   > 
   > 
6. Sla de wijzigingen door te klikken op het vinkje ![vinkje](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Een bijwerken volume bericht weergegeven met de klassieke Azure portal. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.
7. Als u een volume uitbreidt, voert u de volgende stappen uit op uw Windows-hostcomputer:
   
   1. Ga naar **Computerbeheer** ->**Schijfbeheer**.
   2. Met de rechtermuisknop op **Schijfbeheer** en selecteer **schijven**.
   3. Selecteer het volume dat u hebt bijgewerkt, klik met de rechtermuisknop en selecteer in de lijst met schijven **Volume uitbreiden**. De wizard Volume uitbreiden wordt gestart. Klik op **Volgende**.
   4. Voltooi de wizard en accepteer de standaardwaarden. Nadat de wizard voltooid is, moet de grootte van de hogere het volume worden weergegeven.
      
      > [!NOTE]
      > Als u een lokaal vastgemaakt volume uitvouwen en vouwt u vastgemaakt een ander lokaal volume onmiddellijk daarna de taken van de uitbreiding volume worden opeenvolgend uitgevoerd. De eerste taak van de volume-uitbreiding moet zijn voltooid voordat de volgende taak van de volume-uitbreiding kan beginnen.
      > 
      > 

![Video beschikbaar](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video beschikbaar**

Als u wilt een video over het uitbreiden van een volume, klikt u op [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Het volumetype wijzigen
U kunt het volumetype van lagen naar een lokaal vastgemaakt of van lokaal vastgemaakt aan gelaagde. Deze conversie mag echter geen een frequente exemplaar. Enkele redenen voor het converteren van een volume van zijn lagen naar een lokaal vastgemaakt:

* Lokale garanties met betrekking tot de beschikbaarheid van gegevens en prestaties
* Afschaffing van de cloud latenties en problemen met de netwerkverbinding van de cloud.

Dit zijn meestal kleine bestaande volumes die u wilt regelmatig openen. Een lokaal vastgemaakt volume volledig is ingericht wanneer deze wordt gemaakt. Als u een gelaagd volume naar een lokaal vastgemaakt volume converteert, StorSimple wordt gecontroleerd of er voldoende ruimte op het apparaat voordat er begonnen wordt met de conversie. Als er onvoldoende ruimte, ontvangt u een fout opgetreden en de bewerking wordt geannuleerd. 

> [!NOTE]
> Voordat u begint met de conversie van een gelaagde naar een lokaal vastgemaakt, ervoor zorgen dat u rekening houden met de benodigde schijfruimte van andere werkbelastingen. 
> 
> 

Mogelijk wilt u een lokaal vastgemaakt volume wijzigen in een gelaagd volume, als u extra ruimte voor het inrichten van andere volumes nodig. Wanneer u het lokaal vastgemaakt volume de lagen, de beschikbare capaciteit op het apparaat verhoogd door de grootte van de capaciteit voor uitgebrachte converteren. Als u problemen met de netwerkverbinding te voorkomen dat de conversie van een volume van de lokale type naar het type gelaagde, vertonen de lokaal volume eigenschappen van een gelaagd volume totdat de conversie is voltooid. Dit komt omdat sommige gegevens naar de cloud terechtgekomen mogelijk. Deze gemorst gegevens blijven nemen lokale ruimte op het apparaat dat niet kan worden vrijgemaakt totdat de bewerking wordt opnieuw gestart en voltooid.

> [!NOTE]
> Converteren van een volume kan enige tijd duren en u een conversie niet annuleren nadat deze is gestart. Het volume online blijft tijdens de conversie en u kunt back-ups uitvoeren, maar u niet kunt uitbreiden of het volume te herstellen tijdens de conversie.  
> 
> 

Conversie van een gelaagde naar een lokaal vastgemaakt volume, kan een nadelige invloed heeft op de prestaties van een apparaat. Bovendien de volgende factoren het mogelijk meer tijd nodig voor het voltooien van de conversie:

* Er is onvoldoende bandbreedte.
* Er is geen huidige back-up.

Om de effecten die deze factoren kunnen hebben:

* Bekijk uw beleid voor bandbreedteregeling en zorg ervoor dat er geen speciale 40 Mbps bandbreedte beschikbaar is.
* De conversie daluren.
* Een cloud momentopname voordat u begint met de conversie.

Als u meerdere volumes (ondersteunen verschillende werkbelastingen) converteert, moet u de volumeconversie prioriteren zodat volumes met hogere prioriteit eerst worden geconverteerd. Bijvoorbeeld, moet u volumes die virtuele machines (VM's) hosten of volumes met SQL-werkbelastingen converteren voordat u de volumes met file share werkbelastingen converteert.

#### <a name="to-change-the-volume-type"></a>Het volumetype wijzigen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer een volumecontainer in de lijst en dubbelklik erop om de volumes die zijn gekoppeld aan de container weer te geven.
3. Selecteer een volume en klik onder aan de pagina op **wijzigen**. De wizard Modify volume wordt gestart.
4. Op de **basisinstellingen** pagina, wijzigt u het gebruikstype informatie over het door het selecteren van het nieuwe type van de **gebruikstype** vervolgkeuzelijst.
   
   * Als u het type wijzigt **lokaal vastgemaakt**, StorSimple wordt gecontroleerd om te zien of er voldoende capaciteit.
   * Als u het type wijzigt **tiers verdeelde** en dit volume wordt gebruikt voor archivering van gegevens, selecteer de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje.
     
       ![Archief selectievakje](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) naar de **extra instellingen** pagina. Als u een lokaal vastgemaakt volume configureert, wordt het volgende bericht weergegeven.
   
    ![Tekstbericht Volume wijzigen](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) opnieuw om door te gaan.
7. Klik op het vinkje ![Vinkje](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) om te starten tijdens de conversie. Een bijwerken volume bericht weergegeven met de Azure-portal. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.

## <a name="take-a-volume-offline"></a>Een volume offline zetten
Mogelijk moet u een volume offline zetten wanneer u van plan bent om te wijzigen of verwijderen. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet het volume offline nemen op de host, evenals op het apparaat. 

#### <a name="to-take-a-volume-offline"></a>Offline te nemen een volume
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

#### <a name="to-delete-a-volume"></a>Een volume verwijderen
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer de volumecontainer met het volume dat u wilt verwijderen. Klik op de volumecontainer voor toegang tot de **Volumes** pagina.
3. Alle volumes die zijn gekoppeld aan deze container worden weergegeven in tabelvorm. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst de stappen in [offline zetten van een volume](#take-a-volume-offline).
4. Nadat het volume offline is, klikt u op **verwijderen** aan de onderkant van de pagina.
5. Klik op **Ja** als u om bevestiging wordt gevraagd. Het volume wordt nu verwijderd en de **Volumes** ziet u de bijgewerkte lijst van volumes in de container.
   
   > [!NOTE]
   > Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor de nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-Service updates regelmatig van de lokale ruimte beschikbaar. We raden dat u wacht enkele minuten duren voordat u probeert te maken van het nieuwe volume.<br> Ook als u een lokaal vastgemaakt volume verwijdert en verwijdert u vervolgens een ander lokaal vastgemaakt volume onmiddellijk daarna de volume-verwijdering taken worden opeenvolgend uitgevoerd. De eerste taak volume moet zijn voltooid voordat de volgende volume verwijderen van een taak kan beginnen.
   > 
   > 

## <a name="monitor-a-volume"></a>Een volume bewaken
Bewaking van volume, kunt u I/O-gerelateerde statistische gegevens verzamelen voor een volume. Bewaking is standaard ingeschakeld voor de eerste 32 volumes die u maakt. Bewaking van extra volumes is standaard uitgeschakeld. Bewaking van de gekloonde volumes wordt ook standaard uitgeschakeld.

De volgende stappen uitvoeren om te of schakel bewaking voor een volume.

#### <a name="to-enable-or-disable-volume-monitoring"></a>-Of uitschakelen volume bewaking
1. Op de **apparaten** pagina, selecteert u het apparaat, dubbelklikt u op en klik vervolgens op de **Volumecontainers** tabblad.
2. Selecteer de volumecontainer waarin het volume zich bevindt en klik vervolgens op de volumecontainer voor toegang tot de **Volumes** pagina.
3. Alle volumes die zijn gekoppeld aan deze container worden vermeld in de weergegeven in de tabel. Klik op en selecteer het volume of een volume klonen.
4. Klik onder aan de pagina op **wijzigen**.
5. In de wizard Volume wijzigen onder **basisinstellingen**, selecteer **inschakelen** of **uitschakelen** van de **bewaking** vervolgkeuzelijst.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [klonen van een StorSimple-volume](storsimple-clone-volume.md).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).


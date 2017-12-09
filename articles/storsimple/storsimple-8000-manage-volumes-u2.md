---
title: Beheren van StorSimple-volumes (Update 3) | Microsoft Docs
description: Legt uit hoe toevoegen, wijzigen, bewaken en StorSimple-volumes verwijderen, en hoe offline te nemen ze indien nodig.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: alkohli
ms.openlocfilehash: c9c575f42e6c8730b9404c62fb60e710d9d3bc80
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-volumes-update-3-or-later"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van volumes (Update 3 of hoger)

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u met de service Manager voor StorSimple-apparaat maken en beheren van volumes op de apparaten van het StorSimple 8000 serie met Update 3 en hoger.

De service Manager voor StorSimple-apparaat is een uitbreiding in de Azure portal waarmee u uw StorSimple-oplossing beheren via een enkel webinterface. De Azure portal gebruiken voor het beheren van volumes op al uw apparaten. U kunt ook maken en beheren van StorSimple-services, apparaten, back-upbeleid en back-upcatalogus beheren en waarschuwingen weergeven.

## <a name="volume-types"></a>Volumetypen

StorSimple-volumes kunnen zijn:

* **Lokaal vastgemaakte volumes**: gegevens in deze volumes blijft op de lokale StorSimple-apparaat te allen tijde.
* **Gelaagde volumes**: gegevens in deze volumes kunnen worden gelekt naar de cloud.

Een archivering volume is een gelaagd volume. De grotere chunkgrootte Ontdubbeling is gebruikt voor archivering volumes kunt het apparaat groter gegevenssegmenten overbrengen naar de cloud.

Indien nodig, kunt u het volume van een lokale bron naar lagen of van lagen naar lokale. Ga voor meer informatie naar [wijzigen van het volumetype](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokaal vastgemaakte volumes

Lokaal vastgemaakte volumes zijn volledig ingerichte volumes die geen gegevens van de laag naar de cloud te doen, garanties waardoor lokale voor primaire gegevens, onafhankelijk van de verbinding van de cloud. Gegevens op lokaal vastgemaakte volumes is niet ontdubbeld en gecomprimeerd. momentopnamen van lokaal vastgemaakte volumes zijn echter ontdubbeld. 

Lokaal vastgemaakte volumes zijn volledig is ingericht; Daarom moet u voldoende ruimte hebben op uw apparaat wanneer u deze maakt. U kunt lokaal vastgemaakte volumes tot een maximale grootte van 8 TB op het StorSimple 8100-apparaat en 20 TB op het apparaat 8600 inrichten. StorSimple behoudt zich alle lokale ruimte op het apparaat voor momentopnamen, metagegevens en verwerken van gegevens. U kunt de grootte van een lokaal vastgemaakt volume om de maximale ruimte beschikbaar te verhogen, maar u kunt de grootte van een volume eenmaal is gemaakt niet verlagen.

Wanneer u een lokaal vastgemaakt volume maakt, wordt de beschikbare ruimte voor het maken van gelaagde volumes verminderd. Het omgekeerde geldt ook: als er bestaande gelaagde volumes, de ruimte die beschikbaar zijn voor het maken van lokaal vastgemaakte volumes wordt niet lager zijn dan de bovenstaande maximale grenzen. Raadpleeg voor meer informatie over lokale volumes, de [Veelgestelde vragen op lokaal vastgemaakte volumes](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Gelaagde volumes

Gelaagde volumes zijn dun ingerichte volumes waarin vaak gebruikte gegevens blijft lokale op het apparaat en minder vaak gebruikte gegevens automatisch naar de cloud is gelaagd. Thin provisioning is een virtualisatietechnologie voor het waarin beschikbare opslag zijn dan de fysieke resources wordt weergegeven. In plaats van voldoende opslagruimte van tevoren reserveren, StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de huidige vereisten toe te wijzen. De elastische aard van cloud-opslag vergemakkelijkt deze benadering omdat StorSimple kunt vergroten of verkleinen van cloud-opslag om te voldoen aan veranderende verzoeken.

Als u het gelaagde volume voor archivering van gegevens gebruikt, selecteert u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje in om te wijzigen van de chunkgrootte Ontdubbeling voor het volume tot 512 KB. Als u deze optie niet selecteert, wordt het overeenkomstige gelaagde volume een chunkgrootte van 64 KB gebruikt. Een grotere chunkgrootte voor de ontdubbeling zorgt ervoor dat het apparaat sneller grotere archiveringsgegevens naar de cloud kan overzetten.


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

## <a name="the-volumes-blade"></a>De blade volumes

De **Volumes** blade kunt u de opslagvolumes die zijn ingericht op de Microsoft Azure StorSimple-apparaat voor uw initiators (servers) beheren. De lijst van volumes wordt weergegeven op de StorSimple-apparaten dat is verbonden met uw service.

 ![Pagina volumes](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Een volume bestaat uit een reeks van kenmerken:

* **Volumenaam** – een beschrijvende naam die moet uniek en kunt u het volume te identificeren. Deze naam wordt ook gebruikt in de bewaking van rapporten, wanneer u op een bepaald volume filtert. Zodra het volume is gemaakt, kan niet worden gewijzigd.
* **Status** – online of offline kan zijn. Als een volume offline is, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume.
* **Capaciteit** – Hiermee geeft u de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server). Lokaal vastgemaakte volumes volledig is ingericht en bevinden zich op het StorSimple-apparaat. Gelaagde volumes zijn dun ingericht en de gegevens is ontdubbeld. Met thin provisioning volumes toewijzen niet uw apparaat vooraf capaciteit van de fysieke opslag intern of in de cloud volgens de geconfigureerde volumecapaciteit. De volumecapaciteit is toegewezen en op verzoek gebruikt.
* **Type** – Hiermee wordt aangegeven of het volume **tiers verdeelde** (de standaardinstelling) of **lokaal vastgemaakt**.

Volg de instructies in deze zelfstudie aan de volgende taken uitvoeren:

* Een volume toevoegen 
* Een volume wijzigen 
* Het volumetype wijzigen
* Een volume verwijderen 
* Een volume offline zetten 
* Een volume bewaken 

## <a name="add-a-volume"></a>Een volume toevoegen

U [gemaakt van een volume](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) tijdens de implementatie van uw StorSimple 8000 serie-apparaat. Toevoegen van een volume is een vergelijkbare procedure.

#### <a name="to-add-a-volume"></a>Een volume toevoegen

1. Selecteer en klik op uw apparaat in de lijst in tabelvorm met apparaten op de blade **Apparaten**. Klik op **Volume toevoegen**.

    ![Een nieuw volume toevoegen](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. Doe het volgende op de blade **Volume**:
   
    1. In het veld **Apparaat selecteren** wordt automatisch uw huidige apparaat ingevuld.

    2. Selecteer in de vervolgkeuzelijst de volumecontainer waaraan u een volume wilt toevoegen.

    3.  een **naam** voor het volume. Zodra het volume is gemaakt, kunt u het volume niet hernoemen.

    4. Selecteer in de vervolgkeuzelijst het **Type** voor het volume. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties vereist zijn, selecteert u een **lokaal vastgemaakt** volume. Voor alle overige gegevens selecteert u een **gelaagd** volume. Als u dit volume gebruikt voor de archivering van gegevens, schakelt u **Dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** in.
      
       Een gelaagd volume is dun ingericht (thin provisioning) en kan zeer snel worden gemaakt. Als u voor een gelaagd volume dat is bedoeld voor gegevens archivering de optie **Dit volume gebruiken voor minder vaak gebruikte gegevensarchivering** selecteert, verandert de chunkgrootte voor de ontdubbeling voor het volume in 512 KB. Als dit veld niet is ingeschakeld, gebruikt het betreffende gelaagde volume een chunkgrootte van 64 KB. Een grotere chunkgrootte voor de ontdubbeling zorgt ervoor dat het apparaat sneller grotere archiveringsgegevens naar de cloud kan overzetten.
       
       Een lokaal vastgemaakt volume is compact ingericht en zorgt ervoor dat de primaire gegevens op het volume lokaal op het apparaat blijven en niet naar de cloud worden gelekt.  Als u een lokaal vastgemaakt volume maakt, controleert het apparaat de beschikbare ruimte op de lokale lagen om ervoor te zorgen dat het volume het juiste formaat heeft. Het maken van een lokaal vastgemaakt volume heeft als risico dat bestaande gegevens van het apparaat naar de cloud worden gelekt. Ook duurt het maken van het volume mogelijk lang. De totale tijd is afhankelijk van de grootte van het betreffende volume, de beschikbare netwerkbandbreedte en de gegevens op uw apparaat.

    5. Geef de **ingerichte capaciteit** voor het volume op. Noteer de beschikbare capaciteit op basis van het volumetype dat is geselecteerd. De grootte van het opgegeven volume mag niet groter zijn dan de beschikbare ruimte.
      
       U kunt lokaal vastgemaakte volumes inrichten tot maximaal 8,5 TB, of gelaagde volumes op het 8100-apparaat tot 200 TB. Op een groter 8600-apparaat kunt u lokaal vastgemaakte volumes inrichten tot 22,5 TB en gelaagde volumes tot maximaal 500 TB. Aangezien lokale ruimte op het apparaat is vereist om de werkset van gelaagde volumes te hosten, heeft het maken van lokaal vastgemaakte volumes invloed op de beschikbare schijfruimte voor het inrichten van gelaagde volumes. Als u dus een lokaal vastgemaakt volume maakt, wordt de beschikbare schijfruimte voor het maken van gelaagde volumes verminderd. En andersom: als een gelaagd volume wordt gemaakt, wordt de beschikbare ruimte voor het maken van lokaal vastgemaakte volumes verminderd.
      
       Als u een lokaal vastgemaakt volume van 8,5 TB (maximaal toegestane grootte) op uw 8100-apparaat inricht, hebt u de lokale ruimte die beschikbaar is op het apparaat volledig gebruikt. Vanaf dat moment kunt u geen gelaagd volume maken, omdat er geen lokale ruimte op het apparaat is voor het hosten van de werkset van het gelaagde volume. Bestaande gelaagde volumes zijn ook van invloed op de beschikbare ruimte. Als u bijvoorbeeld een 8100-apparaat hebt met reeds gelaagde volumes van circa 106 TB, is er nog maar 4 TB ruimte beschikbaar voor lokaal vastgemaakte volumes.

    6. Klik in het veld **Verbonden hosts** op de pijl. In de **verbonden hosts** blade, kiest u een bestaande ACR of Voeg een nieuwe ACR. Als u een nieuwe ACR kiest, geeft vervolgens een **naam** voor uw ACR bieden de **iSCSI Qualified Name** (IQN) van uw Windows-host. Als u het IQN niet hebt, gaat u naar [Het IQN van een Windows Server-host ophalen](#get-the-iqn-of-a-windows-server-host). Klik op **Create**. Er wordt een volume gemaakt met de opgegeven instellingen.

        ![Klik op Maken](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Het nieuwe volume is nu klaar voor gebruik.

> [!NOTE]
> Als u een lokaal vastgemaakt volume maakt en vervolgens een andere lokaal vastgemaakt volume maakt onmiddellijk na het maken van het volume taken worden opeenvolgend uitgevoerd. De eerste taak voor het maken van volume moet zijn voltooid voordat de volgende taak voor het maken van volume kan beginnen.

## <a name="modify-a-volume"></a>Een volume wijzigen

Een volume wijzigen wanneer u moet deze uitbreiden of wijzigen van de hosts die toegang hebben tot het volume.

> [!IMPORTANT]
> * Als u de grootte van het volume op het apparaat wijzigt, moet de grootte van het volume worden gewijzigd op de host en.
> * De hier beschreven stappen voor de host-zijde zijn voor Windows Server 2012 (2012R2). Procedures voor het Linux- of andere hostbesturingssystemen zullen afwijken. Raadpleeg de documentatie van uw host-besturingssysteem bij het wijzigen van het volume op een host waarop een ander besturingssysteem.

#### <a name="to-modify-a-volume"></a>Een volume wijzigen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer het apparaat waarvoor het volume dat u van plan bent om te wijzigen van de tabellaire vermelding van de apparaten. Klik op **instellingen > Volumes**.

    ![Ga naar de blade Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Selecteer het volume uit de lijst in tabelvorm van volumes en met de rechtermuisknop om het aanroepen van het contextmenu. Selecteer **offline zetten** offline te nemen het volume u wijzigt.

    ![Selecteer en volume offline zetten](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. In de **offline zetten** blade Bekijk het effect van het volume offline halen en het bijbehorende selectievakje selecteert. Zorg ervoor dat de bijbehorende volume op de host eerst offline is. Raadpleeg voor informatie over hoe u een volume offline nemen op uw host-server die is verbonden met StorSimple, specifieke besturingssysteem-instructies. Klik op **offline zetten**.

    ![Gevolgen van het volume offline halen controleren](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Nadat het volume is offline (zoals weergegeven door de status van het volume), selecteert u het volume en met de rechtermuisknop om het aanroepen van het contextmenu. Selecteer **volume wijzigen**.

    ![Selecteer wijzigen volume](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. In de **volume wijzigen** blade kunt u de volgende wijzigingen aanbrengen:
   
   1. Het volume **naam** kan niet worden bewerkt.
   2. Converteert de **Type** van lokaal vastgemaakt lagen naar of van lagen naar lokaal vastgemaakt (Zie [wijzigen van het volumetype](#change-the-volume-type) voor meer informatie).
   3. Vergroot de **ingerichte capaciteit**. De **ingerichte capaciteit** kan alleen worden verhoogd. Nadat deze is gemaakt, kunt u een volume niet verkleinen.
   4. Onder **verbonden hosts**, kunt u de ACR wijzigen. Als u wilt een ACR wijzigen, moet het volume offline.

       ![Gevolgen van het volume offline halen controleren](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Klik op **opslaan** uw wijzigingen op te slaan. Klik op **Ja** als u om bevestiging wordt gevraagd. Een bijwerken volume bericht weergegeven met de Azure-portal. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.

    ![Gevolgen van het volume offline halen controleren](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Als u een volume uitbreidt, voert u de volgende stappen uit op uw Windows-hostcomputer:
   
   1. Ga naar **Computerbeheer** ->**Schijfbeheer**.
   2. Met de rechtermuisknop op **Schijfbeheer** en selecteer **schijven**.
   3. Selecteer het volume dat u hebt bijgewerkt, klik met de rechtermuisknop en selecteer in de lijst met schijven **Volume uitbreiden**. De wizard Volume uitbreiden wordt gestart. Klik op **Volgende**.
   4. Voltooi de wizard en accepteer de standaardwaarden. Nadat de wizard voltooid is, moet de grootte van de hogere het volume worden weergegeven.
      
      > [!NOTE]
      > Als u een lokaal vastgemaakt volume uitvouwen en vouwt u vastgemaakt een ander lokaal volume onmiddellijk daarna de taken van de uitbreiding volume worden opeenvolgend uitgevoerd. De eerste taak van de volume-uitbreiding moet zijn voltooid voordat de volgende taak van de volume-uitbreiding kan beginnen.
      

## <a name="change-the-volume-type"></a>Het volumetype wijzigen

U kunt het volumetype van lagen naar een lokaal vastgemaakt of van lokaal vastgemaakt aan gelaagde. Deze conversie mag echter geen een frequente exemplaar.

### <a name="tiered-to-local-volume-conversion-considerations"></a>Gelaagde naar lokaal volume conversie overwegingen

Enkele redenen voor het converteren van een volume van zijn lagen naar een lokaal vastgemaakt:

* Lokale garanties met betrekking tot de beschikbaarheid van gegevens en prestaties
* Afschaffing van de cloud latenties en problemen met de netwerkverbinding van de cloud.

Dit zijn meestal kleine bestaande volumes die u wilt regelmatig openen. Een lokaal vastgemaakt volume volledig is ingericht wanneer deze wordt gemaakt. 

Als u een gelaagd volume naar een lokaal vastgemaakt volume converteert, StorSimple wordt gecontroleerd of er voldoende ruimte op het apparaat voordat er begonnen wordt met de conversie. Als er onvoldoende ruimte, ontvangt u een fout opgetreden en de bewerking wordt geannuleerd. 

> [!NOTE]
> Voordat u begint met de conversie van een gelaagde naar een lokaal vastgemaakt, ervoor zorgen dat u rekening houden met de benodigde schijfruimte van andere werkbelastingen. 

Conversie van een gelaagde naar een lokaal vastgemaakt volume, kan een nadelige invloed heeft op de prestaties van een apparaat. Bovendien de volgende factoren het mogelijk meer tijd nodig voor het voltooien van de conversie:

* Er is onvoldoende bandbreedte.
* Er is geen huidige back-up.

Om de effecten die deze factoren kunnen hebben:

* Bekijk uw beleid voor bandbreedteregeling en zorg ervoor dat er geen speciale 40 Mbps bandbreedte beschikbaar is.
* De conversie daluren.
* Een cloud momentopname voordat u begint met de conversie.

Als u meerdere volumes (ondersteunen verschillende werkbelastingen) converteert, moet u de volumeconversie prioriteren zodat volumes met hogere prioriteit eerst worden geconverteerd. Bijvoorbeeld, moet u volumes die virtuele machines (VM's) hosten of volumes met SQL-werkbelastingen converteren voordat u de volumes met file share werkbelastingen converteert.

### <a name="local-to-tiered-volume-conversion-considerations"></a>Lokaal naar aandachtspunten bij de conversie van gelaagde volume

U wilt een lokaal vastgemaakt volume wijzigen in een gelaagd volume als u extra ruimte voor het inrichten van andere volumes nodig. Wanneer u het lokaal vastgemaakt volume de lagen, de beschikbare capaciteit op het apparaat verhoogd door de grootte van de capaciteit voor uitgebrachte converteren. Als u problemen met de netwerkverbinding te voorkomen dat de conversie van een volume van de lokale type naar het type gelaagde, vertonen de lokaal volume eigenschappen van een gelaagd volume totdat de conversie voltooid is. Dit komt omdat sommige gegevens naar de cloud terechtgekomen mogelijk. Deze gemorst gegevens blijft nemen lokale ruimte op het apparaat dat niet kan worden vrijgemaakt totdat de bewerking wordt opnieuw gestart en voltooid.

> [!NOTE]
> Converteren van een volume kan enige tijd duren en u een conversie niet annuleren nadat deze is gestart. Het volume online blijft tijdens de conversie en u kunt back-ups uitvoeren, maar u niet kunt uitbreiden of het volume te herstellen tijdens de conversie.


#### <a name="to-change-the-volume-type"></a>Het volumetype wijzigen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer het apparaat waarvoor het volume dat u van plan bent om te wijzigen van de tabellaire vermelding van de apparaten. Klik op **instellingen > Volumes**.

    ![Ga naar de blade Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Selecteer het volume uit de lijst in tabelvorm van volumes en met de rechtermuisknop om het aanroepen van het contextmenu. Selecteer **wijzigen**.

    ![Selecteer wijzigen in contextmenu](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Op de **volume wijzigen** blade het volumetype wijzigen door het selecteren van het nieuwe type van de **Type** vervolgkeuzelijst.
   
   * Als u het type wijzigt **lokaal vastgemaakt**, StorSimple wordt gecontroleerd om te zien of er voldoende capaciteit.
   * Als u het type wijzigt **tiers verdeelde** en dit volume wordt gebruikt voor archivering van gegevens, selecteer de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje.
   * Als u een lokaal vastgemaakt volume configureert zoals lagen of _omgekeerd_, het volgende bericht wordt weergegeven.
   
    ![Wijziging volume tekstbericht](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Klik op **Opslaan** om de wijzigingen op te slaan. Wanneer u om bevestiging gevraagd, klikt u op **Ja** het conversieproces te starten. 

    ![Opslaan en bevestigen](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. De Azure-portal geeft een melding voor het maken van de taak die het volume wordt bijgewerkt. Klik op de melding voor het bewaken van de status van de conversietaak volume.

    ![Taak voor volumeconversie](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Een volume offline zetten

Mogelijk moet u een volume offline zetten wanneer u van plan bent om te wijzigen of verwijderen van het volume. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet rekening houden met het volume offline op de host en het apparaat.

#### <a name="to-take-a-volume-offline"></a>Offline te nemen een volume

1. Zorg ervoor dat het betreffende volume niet gebruikt wordt voordat deze offline wordt gezet.
2. Het volume offline op de host eerst duren. Hierdoor wordt een potentieel risico van gegevensbeschadiging op het volume. Raadpleeg de instructies voor het besturingssysteem van de host voor specifieke stappen.
3. Nadat de host offline is, voert u het volume op het apparaat offline door de volgende stappen uit te voeren:
   
    1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer het apparaat waarvoor het volume dat u van plan bent om te wijzigen van de tabellaire vermelding van de apparaten. Klik op **instellingen > Volumes**.

        ![Ga naar de blade Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Selecteer het volume uit de lijst in tabelvorm van volumes en met de rechtermuisknop om het aanroepen van het contextmenu. Selecteer **offline zetten** offline te nemen het volume u wijzigt.

        ![Selecteer en volume offline zetten](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. In de **offline zetten** blade Bekijk het effect van het volume offline halen en het bijbehorende selectievakje selecteert. Klik op **offline zetten**. 

    ![Gevolgen van het volume offline halen controleren](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      U wordt gewaarschuwd wanneer het volume offline is. Status van het volume is ook bijgewerkt op Offline.
      
4. Nadat een volume offline is, als u het volume en klik met de rechtermuisknop, selecteer **Online brengen** optie is beschikbaar in het contextmenu.

> [!NOTE]
> De **Offline nemen** opdracht verzendt een aanvraag naar het apparaat offline te nemen het volume. Als het volume wordt nog steeds worden gebruikt door hosts, resulteert dit in verbroken verbindingen maar offline halen van het volume niet mislukken.

## <a name="delete-a-volume"></a>Een volume verwijderen

> [!IMPORTANT]
> U kunt een volume alleen verwijderen als deze offline is.

De volgende stappen voor het verwijderen van een volume.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer het apparaat waarvoor het volume dat u van plan bent om te wijzigen van de tabellaire vermelding van de apparaten. Klik op **instellingen > Volumes**.

    ![Ga naar de blade Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst. Volg de stappen in [offline zetten van een volume](#take-a-volume-offline).
4. Nadat het volume offline is, selecteert u het volume, met de rechtermuisknop om het contextmenu aanroepen en selecteer vervolgens **verwijderen**.

    ![Selecteer verwijderen in contextmenu](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. In de **verwijderen** blade bekijken en schakel het selectievakje tegen de gevolgen van het verwijderen van een volume. Wanneer u een volume verwijdert, zijn alle gegevens die zich op het volume wordt verbroken. 

    ![Opslaan en wijzigingen bevestigen](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Als het volume is verwijderd, wordt de lijst in tabelvorm van volumes bijgewerkt om aan te geven van de verwijdering.

    ![Bijgewerkte Volumelijst](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor de nieuwe volumes mogelijk niet meteen bijgewerkt. De Manager voor StorSimple-apparaat Service updates periodiek de lokale ruimte beschikbaar. We raden dat u wacht enkele minuten duren voordat u probeert te maken van het nieuwe volume.
   >
   > Ook als u een lokaal vastgemaakt volume verwijdert en verwijdert u vervolgens een ander lokaal vastgemaakt volume onmiddellijk daarna de volume-verwijdering taken worden opeenvolgend uitgevoerd. De eerste taak volume moet zijn voltooid voordat de volgende volume verwijderen van een taak kan beginnen.

## <a name="monitor-a-volume"></a>Een volume bewaken

Bewaking van volume, kunt u I/O-gerelateerde statistische gegevens verzamelen voor een volume. Bewaking is standaard ingeschakeld voor de eerste 32 volumes die u maakt. Bewaking van extra volumes is standaard uitgeschakeld. 

> [!NOTE]
> Bewaking van de gekloonde volumes is standaard uitgeschakeld.


De volgende stappen uitvoeren om te of schakel bewaking voor een volume.

#### <a name="to-enable-or-disable-volume-monitoring"></a>-Of uitschakelen volume bewaking

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer het apparaat waarvoor het volume dat u van plan bent om te wijzigen van de tabellaire vermelding van de apparaten. Klik op **instellingen > Volumes**.
2. Selecteer het volume uit de lijst in tabelvorm van volumes en met de rechtermuisknop om het aanroepen van het contextmenu. Selecteer **wijzigen**.
3. In de **volume wijzigen** blade voor **bewaking** Selecteer **inschakelen** of **uitschakelen** naar of schakel bewaking.

    ![Schakel bewaking](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Klik op **opslaan** wanneer u om bevestiging gevraagd, klikt u op **Ja**. De Azure-portal geeft een melding voor het bijwerken van het volume en klik vervolgens op een bericht nadat het volume is bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [klonen van een StorSimple-volume](storsimple-8000-clone-volume-u2.md).
* Meer informatie over hoe [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).


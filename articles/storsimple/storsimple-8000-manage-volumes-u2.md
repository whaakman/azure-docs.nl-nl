---
title: Beheren van StorSimple-volumes (Aktualizace 3) | Microsoft Docs
description: Wordt uitgelegd hoe u kunt toevoegen, wijzigen, controleren en verwijderen van de StorSimple-volumes en hoe u offline te nemen ze indien nodig.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: alkohli
ms.openlocfilehash: b748e203e3bf769eef8ce728bbb9471b8d13fb9a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822303"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-volumes-update-3-or-later"></a>De StorSimple Device Manager-service gebruiken voor het beheren van volumes (Update 3 of hoger)

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u de service StorSimple Device Manager maken en beheren van volumes op de apparaten van de StorSimple 8000-serie met Update 3 en hoger gebruiken.

De StorSimple Device Manager-service is een uitbreiding in de Azure-portal waarmee u uw StorSimple-oplossing beheren via één webinterface. Gebruik de Azure-portal voor het beheren van volumes op al uw apparaten. U kunt ook maken en beheren van StorSimple-services, apparaten, back-upbeleid en back-upcatalogus beheren en waarschuwingen weergeven.

## <a name="volume-types"></a>Volumetypen

StorSimple-volumes kunnen zijn:

* **Lokaal vastgemaakt volumes**: Gegevens in deze volumes blijft op het lokale StorSimple-apparaat te allen tijde.
* **Gelaagde volumes**: Gegevens in deze volumes kunnen worden gelekt naar de cloud.

Een archivering volume is een gelaagd volume. De grotere chunkgrootte voor Ontdubbeling wordt gebruikt voor archivering volumes kunt het apparaat om over te dragen van grotere segmenten van gegevens naar de cloud.

Indien nodig, kunt u het volume van lokale lagen of uit naar de lokale lagen. Ga voor meer informatie naar [wijzigen van het volumetype](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokaal vastgemaakte volumes

Lokaal vastgemaakte volumes worden volledig ingerichte volumes die geen gegevens van de laag naar de cloud doen, garanties waardoor er lokale voor primaire gegevens, onafhankelijk van cloud-connectiviteit. Gegevens op de lokaal vastgemaakte volumes niet is ontdubbeld en gecomprimeerd. echter, momentopnamen van lokaal vastgemaakte volumes worden ontdubbeld. 

Lokaal vastgemaakte volumes worden volledig is ingericht; u moet daarom voldoende ruimte op uw apparaat hebt, wanneer u deze maakt. U kunt lokaal vastgemaakte volumes een maximale grootte van 8 TB op het StorSimple 8100-apparaat en 20 TB op 8600-apparaat inrichten. StorSimple behoudt zich het resterende lokale ruimte op het apparaat voor momentopnamen, metagegevens en gegevens verwerken. U kunt de grootte van een lokaal vastgemaakt volume om de maximaal beschikbare ruimte te verhogen, maar u kunt de grootte van een volume eenmaal is gemaakt niet verlagen.

Wanneer u een lokaal vastgemaakt volume maakt, wordt de beschikbare ruimte voor het maken van gelaagde volumes verminderd. Het omgekeerde geldt ook: hebt u een bestaande gelaagde volumes, de ruimte die beschikbaar zijn voor het maken van lokaal volumes vastgemaakte worden niet lager zijn dan de bovenstaande maximumlimieten. Raadpleeg voor meer informatie over lokale volumes, de [Veelgestelde vragen over de lokaal vastgemaakte volumes](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Gelaagde volumes

Gelaagde volumes zijn dun ingerichte volumes waarop de veelgebruikte gegevens lokaal op het apparaat blijft en gelaagde minder vaak gebruikte gegevens automatisch naar de cloud. Thin provisioning is een virtualisatietechnologie waarin beschikbare opslag meer dan fysieke resources wordt weergegeven. In plaats van het reserveren van tevoren voldoende opslagruimte, StorSimple maakt gebruik van thin provisioning, net voldoende ruimte om te voldoen aan de actuele vereisten toe te wijzen. De elastische aard van opslag in de cloud vereenvoudigt het uitvoeren van deze benadering omdat StorSimple kunt vergroten of verkleinen van opslag in de cloud om te voldoen aan veranderende eisen.

Als u het gelaagde volume voor archiveringsgegevens die u gebruikt, selecteert u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje in om te wijzigen van de chunkgrootte voor de Ontdubbeling voor het volume tot 512 KB. Als u deze optie niet selecteert, wordt het overeenkomstige gelaagde volume een chunkgrootte van 64 KB gebruikt. Een grotere chunkgrootte voor de ontdubbeling zorgt ervoor dat het apparaat sneller grotere archiveringsgegevens naar de cloud kan overzetten.


### <a name="provisioned-capacity"></a>Ingerichte capaciteit

Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk type apparaat en het volume. (Houd er rekening mee dat lokaal vastgemaakte volumes niet beschikbaar op een virtueel apparaat zijn.)

|  | De grootte van maximaal gelaagde volume | Maximale lokaal vastgemaakt volumegrootte |
| --- | --- | --- |
| **Fysieke apparaten** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuele apparaten** | | |
| 8010 |30 TB |N/A |
| 8020 |64 TB |N/A |

## <a name="the-volumes-blade"></a>De blade volumes

De **Volumes** blade kunt u voor het beheren van de opslagvolumes die zijn ingericht op het Microsoft Azure StorSimple-apparaat voor de initiators (servers). De lijst van volumes wordt weergegeven op het StorSimple-apparaten die zijn verbonden met uw service.

 ![Pagina volumes](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Een volume bestaat uit een reeks kenmerken:

* **Volumenaam** : een beschrijvende naam moet uniek zijn en helpt bij het identificeren van het volume. Deze naam wordt ook gebruikt in rapporten controleren wanneer u op een bepaald volume filtert. Als het volume is gemaakt, kan niet worden gewijzigd.
* **Status** – online of offline kan zijn. Als een volume offline is, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume gebruiken.
* **Capaciteit** – Hiermee geeft u de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server). Lokaal vastgemaakt volumes zijn volledig is ingericht en bevinden zich op het StorSimple-apparaat. Gelaagde volumes zijn thin provisioning en de gegevens is ontdubbeld. Met dun ingerichte volumes toewijzen niet uw apparaat vooraf fysieke opslagcapaciteit intern of in de cloud op basis van geconfigureerde volumecapaciteit. De capaciteit van het volume is toegewezen en die op aanvraag worden gebruikt.
* **Type** – geeft aan of het volume **gelaagd** (de standaardinstelling) of **lokaal vastgemaakt**.

Volg de instructies in deze zelfstudie de volgende taken uitvoeren:

* Een volume toevoegen 
* Een volume wijzigen 
* Het volumetype wijzigen
* Een volume verwijderen 
* Een volume offline halen 
* Een volume bewaken 

## <a name="add-a-volume"></a>Een volume toevoegen

U [gemaakt van een volume](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) tijdens de implementatie van de StorSimple 8000-apparaat. Toevoegen van een volume is een vergelijkbare procedure.

#### <a name="to-add-a-volume"></a>Een volume toevoegen

1. Selecteer en klik op uw apparaat in de lijst in tabelvorm met apparaten op de blade **Apparaten**. Klik op **Volume toevoegen**.

    ![Een nieuw volume toevoegen](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. Doe het volgende op de blade **Volume**:
   
    1. In het veld **Apparaat selecteren** wordt automatisch uw huidige apparaat ingevuld.

    2. Selecteer in de vervolgkeuzelijst de volumecontainer waaraan u een volume wilt toevoegen.

    3.  een **naam** voor het volume. Als het volume is gemaakt, kunt u het volume niet hernoemen.

    4. Selecteer in de vervolgkeuzelijst het **Type** voor het volume. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties vereist zijn, selecteert u een **lokaal vastgemaakt** volume. Voor alle overige gegevens selecteert u een **gelaagd** volume. Als u dit volume gebruikt voor de archivering van gegevens, schakelt u **Dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** in.
      
       Een gelaagd volume is dun ingericht (thin provisioning) en kan zeer snel worden gemaakt. Als u voor een gelaagd volume dat is bedoeld voor gegevens archivering de optie **Dit volume gebruiken voor minder vaak gebruikte gegevensarchivering** selecteert, verandert de chunkgrootte voor de ontdubbeling voor het volume in 512 KB. Als dit veld niet is ingeschakeld, gebruikt het betreffende gelaagde volume een chunkgrootte van 64 KB. Een grotere chunkgrootte voor de ontdubbeling zorgt ervoor dat het apparaat sneller grotere archiveringsgegevens naar de cloud kan overzetten.
       
       Een lokaal vastgemaakt volume is compact ingericht en zorgt ervoor dat de primaire gegevens op het volume lokaal op het apparaat blijven en niet naar de cloud worden gelekt.  Als u een lokaal vastgemaakt volume maakt, controleert het apparaat de beschikbare ruimte op de lokale lagen om ervoor te zorgen dat het volume het juiste formaat heeft. Het maken van een lokaal vastgemaakt volume heeft als risico dat bestaande gegevens van het apparaat naar de cloud worden gelekt. Ook duurt het maken van het volume mogelijk lang. De totale tijd is afhankelijk van de grootte van het betreffende volume, de beschikbare netwerkbandbreedte en de gegevens op uw apparaat.

    5. Geef de **ingerichte capaciteit** voor het volume op. Noteer de beschikbare capaciteit op basis van het volumetype dat is geselecteerd. De grootte van het opgegeven volume mag niet groter zijn dan de beschikbare ruimte.
      
       U kunt lokaal vastgemaakte volumes inrichten tot maximaal 8,5 TB, of gelaagde volumes op het 8100-apparaat tot 200 TB. Op een groter 8600-apparaat kunt u lokaal vastgemaakte volumes inrichten tot 22,5 TB en gelaagde volumes tot maximaal 500 TB. Aangezien lokale ruimte op het apparaat is vereist om de werkset van gelaagde volumes te hosten, heeft het maken van lokaal vastgemaakte volumes invloed op de beschikbare schijfruimte voor het inrichten van gelaagde volumes. Als u dus een lokaal vastgemaakt volume maakt, wordt de beschikbare schijfruimte voor het maken van gelaagde volumes verminderd. En andersom: als een gelaagd volume wordt gemaakt, wordt de beschikbare ruimte voor het maken van lokaal vastgemaakte volumes verminderd.
      
       Als u een lokaal vastgemaakt volume van 8,5 TB (maximaal toegestane grootte) op uw 8100-apparaat inricht, hebt u de lokale ruimte die beschikbaar is op het apparaat volledig gebruikt. Vanaf dat moment kunt u geen gelaagd volume maken, omdat er geen lokale ruimte op het apparaat is voor het hosten van de werkset van het gelaagde volume. Bestaande gelaagde volumes zijn ook van invloed op de beschikbare ruimte. Als u bijvoorbeeld een 8100-apparaat hebt met reeds gelaagde volumes van circa 106 TB, is er nog maar 4 TB ruimte beschikbaar voor lokaal vastgemaakte volumes.

    6. Klik in het veld **Verbonden hosts** op de pijl. In de **verbonden hosts** blade, kiest u een bestaande ACR of Voeg een nieuwe ACR toe. Als u een nieuwe ACR, geef vervolgens een **naam** voor uw ACR, geeft u de **iSCSI Qualified Name** (IQN) van uw Windows-host. Als u het IQN niet hebt, gaat u naar het IQN ophalen van een Windows Server-host. Klik op **Create**. Er wordt een volume gemaakt met de opgegeven instellingen.

        ![Klik op Maken](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Het nieuwe volume is nu klaar voor gebruik.

> [!NOTE]
> Als u een lokaal vastgemaakt volume maakt en vervolgens een andere lokaal vastgemaakt volume maakt onmiddellijk na het maken van de volume taken sequentieel worden uitgevoerd. De eerste taak voor het maken van volume moet worden voltooid voordat u kunt beginnen met de volgende taak voor het maken van volume.

## <a name="modify-a-volume"></a>Een volume wijzigen

Een volume niet wijzigen als u wilt uitvouwen of wijzigen van de hosts die toegang hebben tot het volume.

> [!IMPORTANT]
> * Als u de grootte van het volume op het apparaat wijzigt, moet de grootte van het volume op de host en worden gewijzigd.
> * De hier beschreven stappen voor de host aan clientzijde zijn voor Windows Server 2012 (2012R2). Procedures voor het Linux- of andere hostbesturingssystemen zijn verschillend. Raadpleeg de documentatie van uw host-besturingssysteem bij het wijzigen van het volume op een host met een ander besturingssysteem.

#### <a name="to-modify-a-volume"></a>Een volume wijzigen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer in de lijst in tabelvorm van de apparaten, het apparaat waarvoor het volume dat u van plan bent om te wijzigen. Klik op **instellingen > Volumes**.

    ![Ga naar de blade van de Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. In de lijst in tabelvorm van volumes, selecteert u het volume en met de rechtermuisknop op voor het aanroepen van het contextmenu. Selecteer **offline zetten** offline te nemen het volume wordt het bestand aangepast.

    ![Selecteer en volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. In de **offline zetten** blade, de impact van het volume offline te bekijken en het bijbehorende selectievakje. Zorg ervoor dat de bijbehorende volume op de host eerst offline is. Raadpleeg voor meer informatie over hoe u een volume offline nemen op uw host-server die is verbonden met StorSimple specifieke besturingssysteem-instructies. Klik op **offline zetten**.

    ![Bekijk de gevolgen van het volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Nadat het volume is offline (zoals aangegeven in de status van het volume), selecteert u het volume en met de rechtermuisknop op voor het aanroepen van het contextmenu. Selecteer **wijzigen van een volume**.

    ![Selecteer wijzigen volume](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. In de **wijzigen van een volume** blade kunt u de volgende wijzigingen aanbrengen:
   
   1. Het volume **naam** kan niet worden bewerkt.
   2. Converteer de **Type** van lokaal vastgemaakt doorverbonden naar of van gelaagde in lokaal vastgemaakt (Zie [wijzigen van het volumetype](#change-the-volume-type) voor meer informatie).
   3. Verhoog de **ingerichte capaciteit**. De **ingerichte capaciteit** kan alleen worden verhoogd. Nadat deze is gemaakt, kunt u een volume niet verkleinen.
   4. Onder **verbonden hosts**, kunt u de ACR wijzigen. Als u wilt een ACR wijzigt, moet het volume offline.

       ![Bekijk de gevolgen van het volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Klik op **opslaan** uw wijzigingen op te slaan. Klik op **Ja** als u om bevestiging wordt gevraagd. Een bijwerken volume bericht weergegeven met de Azure-portal. Er verschijnt een bericht weergegeven wanneer het volume is bijgewerkt.

    ![Bekijk de gevolgen van het volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Als u een volume uitbreidt, voert u de volgende stappen uit op uw Windows-hostcomputer:
   
   1. Ga naar **Computerbeheer** ->**Schijfbeheer**.
   2. Met de rechtermuisknop op **Schijfbeheer** en selecteer **schijven**.
   3. Selecteer in de lijst met schijven, het volume dat u hebt bijgewerkt, klik met de rechtermuisknop en selecteer vervolgens **Volume uitbreiden**. De wizard Volume uitbreiden wordt gestart. Klik op **volgende**.
   4. Voltooi de wizard, accepteer de standaardwaarden. Nadat de wizard voltooid is, moet het volume de toegenomen grootte weergeven.
      
      > [!NOTE]
      > Als u een lokaal vastgemaakt volume uitbreiden en vouw vervolgens vastgemaakt een ander lokaal volume direct daarna de taken van de uitbreiding volume worden opeenvolgend uitgevoerd. De eerste taak van de volume-uitbreiding moet worden voltooid voordat de volgende volume uitbreiding van taak kan beginnen.
      

## <a name="change-the-volume-type"></a>Het volumetype wijzigen

U kunt wijzigen het volumetype van gelaagde in een lokaal vastgemaakt of van lokaal vastgemaakt aan gelaagde. Deze conversie een hoeveelheid mag echter geen een frequente exemplaar.

### <a name="tiered-to-local-volume-conversion-considerations"></a>Gelaagde in aandachtspunten bij de conversie van het lokale volume

Enkele redenen voor het converteren van een volume van zijn gelaagde in een lokaal vastgemaakt:

* Lokale garanties met betrekking tot beschikbaarheid van gegevens en prestaties
* Afschaffing van de cloud latenties en problemen met de netwerkverbinding van de cloud.

Dit zijn doorgaans kleine bestaande volumes die u wilt vaak openen. Een lokaal vastgemaakt volume is volledig is ingericht als deze wordt gemaakt. 

Als u een gelaagd volume naar een lokaal vastgemaakt volume converteert, controleert StorSimple of dat er voldoende ruimte is op uw apparaat voordat er begonnen wordt met de conversie. Als er onvoldoende ruimte beschikbaar is, ontvangt u een fout en de bewerking wordt geannuleerd. 

> [!NOTE]
> Voordat u de conversie van een gelaagde begint in een lokaal vastgemaakt, zorg er dan voor dat u rekening houden met de vereisten voor de andere werkbelastingen. 

Conversie van een gelaagde naar een lokaal vastgemaakt volume, kan een nadelige invloed heeft op de prestaties van het apparaat. De volgende factoren verhogen bovendien de tijd die nodig is voor de conversie is voltooid:

* Er is onvoldoende bandbreedte.
* Er is geen huidige back-up.

Om te beperken de effecten die deze factoren kunnen hebben:

* Controleer uw beleid voor bandbreedteregeling en zorg ervoor dat een toegewezen 40 Mbps bandbreedte beschikbaar is.
* De conversie voor daluren.
* Een cloudmomentopname voordat u begint met de conversie.

Als u meerdere volumes (ondersteunen verschillende werkbelastingen) converteert, moet u de volumeconversie prioriteit zodat volumes met hogere prioriteit eerst worden geconverteerd. Bijvoorbeeld, moet u volumes die virtuele machines (VM's) hosten of volumes met SQL-workloads converteren voordat u de volumes met file share werklasten converteren.

### <a name="local-to-tiered-volume-conversion-considerations"></a>Lokale en gelaagde volume conversie overwegingen

U wilt wijzigen op een lokaal vastgemaakt volume een gelaagd volume als u extra ruimte voor het inrichten van andere volumes nodig hebt. Wanneer u het lokaal vastgemaakte volume lagen, de beschikbare capaciteit voor de verhoging van het apparaat door de grootte van de uitgebrachte capaciteit converteren. Als u problemen met de netwerkverbinding te voorkomen dat de conversie van een volume van het lokale type naar de gelaagde type, wordt het lokale volume eigenschappen van een gelaagd volume vertonen totdat de conversie voltooid is. Dit is omdat sommige gegevens naar de cloud terechtgekomen mogelijk. Deze gemorst gegevens blijft het lokale ruimte op het apparaat dat niet kan worden vrijgemaakt totdat de bewerking wordt opnieuw opgestart en uitgevoerd in beslag nemen.

> [!NOTE]
> Converteren van een volume kan enige tijd duren en u kunt een conversie niet annuleren nadat deze is gestart. Het volume online blijft tijdens de conversie, en u kunt back-ups uitvoeren, maar u niet kunt uitbreiden of het volume herstellen tijdens de conversie.


#### <a name="to-change-the-volume-type"></a>Het volumetype wijzigen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer in de lijst in tabelvorm van de apparaten, het apparaat waarvoor het volume dat u van plan bent om te wijzigen. Klik op **instellingen > Volumes**.

    ![Ga naar de blade van de Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. In de lijst in tabelvorm van volumes, selecteert u het volume en met de rechtermuisknop op voor het aanroepen van het contextmenu. Selecteer **wijzigen**.

    ![Selecteer wijzigen in contextmenu](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Op de **wijzigen van een volume** blade het volumetype wijzigen door het selecteren van het nieuwe type van de **Type** vervolgkeuzelijst.
   
   * Als u het type wijzigen **lokaal vastgemaakt**, StorSimple wordt gecontroleerd om te zien of er voldoende capaciteit.
   * Als u het type wijzigen **gelaagd** en dit volume wordt gebruikt voor archivering van gegevens, selecteer de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje.
   * Als u een lokaal vastgemaakt volume configureert als gelaagde of _omgekeerd_, het volgende bericht wordt weergegeven.
   
    ![Wijziging volume type bericht](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Klik op **Opslaan** om de wijzigingen op te slaan. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** het conversieproces te starten. 

    ![Opslaan en bevestigen](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. De Azure-portal geeft een melding voor het maken van de taak die het volume wordt bijgewerkt. Klik op de melding voor het bewaken van de status van de taak volume.

    ![Taak voor de volumeconversie](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Een volume offline halen

Mogelijk moet u een volume offline halen als u van plan bent om te wijzigen of verwijderen van het volume. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet het volume offline op de host en het apparaat op te nemen.

#### <a name="to-take-a-volume-offline"></a>Op een volume offline halen

1. Zorg ervoor dat het betreffende volume zich niet in gebruik voordat deze offline te zetten.
2. Het volume offline op de host eerst duren. Hierdoor is er mogelijk kosten in rekening van beschadiging van gegevens op het volume. Raadpleeg de instructies voor het hostbesturingssysteem voor specifieke stappen.
3. Plaats het volume op het apparaat offline nadat de host offline is, door de volgende stappen uit:
   
    1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer in de lijst in tabelvorm van de apparaten, het apparaat waarvoor het volume dat u van plan bent om te wijzigen. Klik op **instellingen > Volumes**.

        ![Ga naar de blade van de Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. In de lijst in tabelvorm van volumes, selecteert u het volume en met de rechtermuisknop op voor het aanroepen van het contextmenu. Selecteer **offline zetten** offline te nemen het volume wordt het bestand aangepast.

        ![Selecteer en volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. In de **offline zetten** blade, de impact van het volume offline te bekijken en het bijbehorende selectievakje. Klik op **offline zetten**. 

    ![Bekijk de gevolgen van het volume offline halen](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      U krijgt een melding wanneer het volume offline is. Status van het volume kan ook worden bijgewerkt naar Offline.
      
4. Nadat een volume offline is, als u het volume en klik met de rechtermuisknop selecteert **Online brengen** optie is beschikbaar in het contextmenu.

> [!NOTE]
> De **Offline nemen** opdracht een aanvraag verzendt naar het apparaat offline te nemen het volume. Als het volume nog altijd van hosts gebruikmaken, dit resulteert in verbroken verbindingen, maar niet het volume offline te mislukken.

## <a name="delete-a-volume"></a>Een volume verwijderen

> [!IMPORTANT]
> U kunt een volume alleen verwijderen als deze offline is.

De volgende stappen als u wilt verwijderen van een volume.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer in de lijst in tabelvorm van de apparaten, het apparaat waarvoor het volume dat u van plan bent om te wijzigen. Klik op **instellingen > Volumes**.

    ![Ga naar de blade van de Volumes](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst. Volg de stappen in [een volume offline halen](#take-a-volume-offline).
4. Nadat het volume offline is, selecteert u het volume, met de rechtermuisknop op voor het aanroepen van het contextmenu en selecteer vervolgens **verwijderen**.

    ![Selecteer verwijderen in contextmenu](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. In de **verwijderen** blade, controleren en schakel het selectievakje tegen de gevolgen van het verwijderen van een volume. Wanneer u een volume verwijdert, zijn alle gegevens die zich bevinden op het volume gaat verloren. 

    ![Opslaan en wijzigingen bevestigen](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Nadat het volume is verwijderd, wordt de lijst in tabelvorm van volumes bijgewerkt om aan te geven van de verwijdering.

    ![Volumelijst met bijgewerkte](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Device Manager-Service-updates worden periodiek de lokale ruimte die beschikbaar. We raden dat u wachten tot een paar minuten voordat u probeert te maken van het nieuwe volume.
   >
   > Bovendien als u een lokaal vastgemaakt volume verwijdert en verwijder vervolgens nog een lokaal vastgemaakt volume direct daarna de volume-verwijdering taken sequentieel worden uitgevoerd. De eerste taak volume moet worden voltooid voordat u kunt beginnen met de volgende taak volume.

## <a name="monitor-a-volume"></a>Een volume bewaken

Controle van de volumes, kunt u voor het verzamelen van I/O-statistieken voor een volume. Bewaking is standaard ingeschakeld voor de eerste 32 volumes die u maakt. Bewaking van extra volumes is standaard uitgeschakeld. 

> [!NOTE]
> Bewaking van de gekloonde volumes is standaard uitgeschakeld.


De volgende stappen uitvoeren om in- of uitschakelen bewaking voor een volume.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Controle van de volumes uit te schakelen

1. Ga naar de StorSimple-apparaatbeheerfunctie en klik op **Apparaten**. Selecteer in de lijst in tabelvorm van de apparaten, het apparaat waarvoor het volume dat u van plan bent om te wijzigen. Klik op **instellingen > Volumes**.
2. In de lijst in tabelvorm van volumes, selecteert u het volume en met de rechtermuisknop op voor het aanroepen van het contextmenu. Selecteer **wijzigen**.
3. In de **wijzigen van een volume** blade voor **bewaking** Selecteer **inschakelen** of **uitschakelen** bewaking uit te schakelen.

    ![Bewaking uitschakelen](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Klik op **opslaan** wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**. De Azure-portal geeft een melding voor het bijwerken van het volume en vervolgens een bericht wanneer het volume is bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [klonen van een StorSimple-volume](storsimple-8000-clone-volume-u2.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).


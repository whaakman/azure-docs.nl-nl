---
title: Beheren van volumes op de virtuele StorSimple-matrix | Microsoft Docs
description: Worden Apparaatbeheer StorSimple beschreven en wordt uitgelegd hoe u deze voor het beheren van volumes op uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: a507bf1866952cb79fa6334fed80c88cd207cd0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-device-manager-service-to-manage-volumes-on-the-storsimple-virtual-array"></a>De service Manager gebruiken StorSimple-apparaat voor het beheren van volumes op de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe de service Manager van StorSimple-apparaat gebruiken voor het maken en beheren van volumes op uw virtuele StorSimple-matrix.

De service Manager voor StorSimple-apparaat is een uitbreiding in de Azure portal waarmee u uw StorSimple-oplossing beheren via een enkel webinterface. Naast het beheren van bestandsshares en volumes, kunt u de service Manager voor StorSimple-apparaat weergeven en beheren van apparaten, waarschuwingen, weergeven en weergeven en beheren van back-upbeleid en de back-catalogus.

## <a name="volume-types"></a>Volumetypen

StorSimple-volumes kunnen zijn:

* **Lokaal vastgemaakt**: gegevens op deze volumes blijft op de matrix te allen tijde en komt niet in de cloud worden gelekt.
* **Gelaagde**: gegevens in deze volumes kunnen worden gelekt naar de cloud. Wanneer u een gelaagd volume maakt, wordt ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld: als u een volume 1 TB ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB in de cloud moet worden gebruikt wanneer de gegevenslagen. Dit wordt op zijn beurt betekent dat als u buiten de lokale ruimte op het apparaat uitvoeren, u kunt geen een gelaagd volume inrichten (omdat de 10% vereist op de lokale laag niet meer beschikbaar).

### <a name="provisioned-capacity"></a>Ingerichte capaciteit
Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk volumetype.

| **Limiet-ID**                                       | **Limiet**     |
|------------------------------------------------------------|---------------|
| Minimale grootte van een gelaagd volume                            | 500 GB        |
| Maximale grootte van een gelaagd volume                            | 5 TB          |
| Minimale grootte van een lokaal vastgemaakt volume                    | 50 GB         |
| Maximale grootte van een lokaal vastgemaakt volume                    | 500 GB        |

## <a name="the-volumes-blade"></a>De blade Volumes
De **Volumes** menu op de blade voor een overzicht van het StorSimple-service geeft een lijst met opslagvolumes op een gegeven StorSimple-matrix en kunt u ze kunt beheren.

![Blade volumes](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Een volume bestaat uit een reeks van kenmerken:

* **Volumenaam** – een beschrijvende naam die moet uniek en kunt u het volume te identificeren.
* **Status** – online of offline kan zijn. Als een volume offline is, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume.
* **Type** – Hiermee wordt aangegeven of het volume **tiers verdeelde** (de standaardinstelling) of **lokaal vastgemaakt**.
* **Capaciteit** – geeft de hoeveelheid gegevens die worden gebruikt in vergelijking met de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server).
* **Back-** – voor het geval van de virtuele StorSimple-matrix, worden alle volumes automatisch ingeschakeld voor back-up.
* **Verbonden hosts** – Hiermee geeft u de initiators (servers) die toegang tot dit volume hebben.

![Details van de volumes](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Volg de instructies in deze zelfstudie aan de volgende taken uitvoeren:

* Een volume toevoegen
* Een volume wijzigen
* Een volume offline zetten
* Een volume verwijderen

## <a name="add-a-volume"></a>Een volume toevoegen

1. Klik op de StorSimple-service samenvatting blade **+ volume toevoegen** uit de opdrachtbalk. Hiermee opent u de **volume toevoegen** blade.
   
    ![Volume toevoegen](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. In de **volume toevoegen** blade het volgende doen:
   
   * In de **volumenaam** en voer een unieke naam voor het volume. De naam moet een tekenreeks met 3 tot en met 127 tekens.
   * In de **Type** dropdown lijst, opgeven of maken van een **tiers verdeelde** of **lokaal vastgemaakt** volume. Selecteer voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, **lokaal vastgemaakt volume**. Voor alle overige gegevens selecteert **tiers verdeelde** volume.
   * In de **capaciteit** veld, geeft u de grootte van het volume. Een gelaagd volume moet liggen tussen 500 GB en 5 TB en een lokaal vastgemaakt volume moet tussen 50 en 500 GB.
   * * Klik op **verbonden hosts**, selecteert u een record voor toegangscontrole (ACR) overeenkomt met de iSCSI-initiator die u wilt verbinding maken met dit volume en klik vervolgens op **Selecteer**.
3. Een nieuwe verbonden als host wilt toevoegen, klikt u op **nieuwe toevoegen**, voer een naam voor de host en de bijbehorende iSCSI Qualified Name (IQN) en klik vervolgens op **toevoegen**.
   
    ![Volume toevoegen](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Wanneer u klaar bent met het configureren van het volume, klikt u op **maken**. Een volume wordt gemaakt met de opgegeven instellingen en u ziet een melding van de geslaagde aanmaak van hetzelfde. Standaard wordt de back-up worden ingeschakeld voor het volume.
5. Om te bevestigen dat het volume is gemaakt, gaat u naar de **Volumes** blade. Hier ziet u het volume dat wordt vermeld.
   
    ![Volume maken geslaagd](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Een volume wijzigen

Een volume niet wijzigen als u wilt wijzigen van de hosts die toegang hebben tot het volume. De andere kenmerken van een volume kunnen niet worden gewijzigd wanneer het volume is gemaakt.

#### <a name="to-modify-a-volume"></a>Een volume wijzigen

1. Van de **Volumes** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop het volume dat u wenst te maken van zich bevindt.
2. **Selecteer** het volume en klik op **verbonden hosts** de momenteel verbonden host weergeven en wijzigen naar een andere server.
   
    ![Volume bewerken](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Sla de wijzigingen door te klikken op de **opslaan** opdrachtbalk. De opgegeven instellingen worden toegepast en u ziet een melding.

## <a name="take-a-volume-offline"></a>Een volume offline zetten

Mogelijk moet u een volume offline zetten wanneer u van plan bent om te wijzigen of verwijderen. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet het volume offline nemen op de host, evenals op het apparaat.

#### <a name="to-take-a-volume-offline"></a>Offline te nemen een volume

1. Zorg ervoor dat het betreffende volume niet gebruikt wordt voordat deze offline wordt gezet.
2. Het volume offline op de host eerst duren. Hierdoor wordt een potentieel risico van gegevensbeschadiging op het volume. Raadpleeg de instructies voor het besturingssysteem van de host voor specifieke stappen.
3. Nadat het volume op de host offline is, voert u het volume op de matrix offline door de volgende stappen uit te voeren:
   
   * Van de **Volumes** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop het volume dat u wenst dat u offline te halen zich bevindt.
   * **Selecteer** het volume en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **offline zetten**.
     
        ![Offline volume](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Lees de informatie in de **offline zetten** blade en de bevestiging van de bewerking te bevestigen. Klik op **offline zetten** offline te nemen het volume. U ziet een melding van de bewerking wordt uitgevoerd.
   * Om te bevestigen dat het volume is offline is gehaald, gaat u naar de **Volumes** blade. U ziet de status van het volume als offline.
     
       ![Offline volume bevestigen](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Een volume verwijderen

> [!IMPORTANT]
> U kunt een volume alleen verwijderen als deze offline is.
> 
> 

De volgende stappen voor het verwijderen van een volume.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Van de **Volumes** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop het volume dat u wenst te verwijderen dat zich bevindt.
2. **Selecteer** het volume en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **verwijderen**.
   
    ![Volume verwijderen](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst de stappen in [offline zetten van een volume](#take-a-volume-offline).
4. Als u wordt gevraagd om bevestiging in de **verwijderen** blade, accepteer de bevestiging en klik op **verwijderen**. Het volume wordt nu verwijderd en de **Volumes** blade ziet de bijgewerkte lijst van volumes in de virtuele-matrix.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [klonen van een StorSimple-volume](storsimple-virtual-array-clone.md).


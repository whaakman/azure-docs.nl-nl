---
title: Beheren van volumes op de StorSimple Virtual Array | Microsoft Docs
description: Beschrijving van de StorSimple Device Manager en wordt uitgelegd hoe u deze gebruiken voor het beheren van volumes op uw StorSimple Virtual Array.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: a233a9deb58a7c1abc87a622a4f1f2581ee2e477
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125794"
---
# <a name="use-storsimple-device-manager-service-to-manage-volumes-on-the-storsimple-virtual-array"></a>Gebruik StorSimple Device Manager-service voor het beheren van volumes op de StorSimple Virtual Array

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u de service StorSimple Device Manager maken en beheren van volumes op uw StorSimple Virtual Array gebruiken.

De StorSimple Device Manager-service is een uitbreiding in de Azure-portal waarmee u uw StorSimple-oplossing beheren via één webinterface. Naast het beheren van bestandsshares en volumes, kunt u de service StorSimple Device Manager weergeven en beheren van apparaten, waarschuwingen, weergeven en weergeven en beheren van back-upbeleid en de back-catalogus.

## <a name="volume-types"></a>Volumetypen

StorSimple-volumes kunnen zijn:

* **Lokaal vastgemaakt**: Gegevens in deze volumes blijft op de matrix te allen tijde en wordt niet naar de cloud worden gelekt.
* **Gelaagde**: Gegevens in deze volumes kunnen worden gelekt naar de cloud. Wanneer u een gelaagd volume maakt, wordt ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld, als u een volume van 1 TB hebt ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou in de cloud worden gebruikt wanneer de gegevenslagen. Dit wordt op zijn beurt betekent dat als u van de lokale ruimte op het apparaat uitvoert, u kunt geen een gelaagd volume inrichten (omdat de 10% vereist op de lokale laag niet meer beschikbaar).

### <a name="provisioned-capacity"></a>Ingerichte capaciteit
Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk volume.

| **Limiet-ID**                                       | **Limiet**     |
|------------------------------------------------------------|---------------|
| Minimale grootte van een gelaagd volume                            | 500 GB        |
| Maximale grootte van een gelaagd volume                            | 5 TB          |
| Minimale grootte van een lokaal vastgemaakt volume                    | 50 GB         |
| Maximale grootte van een lokaal vastgemaakt volume                    | 500 GB        |

## <a name="the-volumes-blade"></a>De blade Volumes
De **Volumes** menu op de overzichtsblade van uw StorSimple-service geeft de lijst van opslagvolumes op een opgegeven StorSimple-matrix en kunt u ze kunt beheren.

![Blade volumes](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Een volume bestaat uit een reeks kenmerken:

* **Volumenaam** : een beschrijvende naam moet uniek zijn en helpt bij het identificeren van het volume.
* **Status** – online of offline kan zijn. Als een volume offline is, is het niet zichtbaar voor de initiators (servers) die toegang hebben tot het volume gebruiken.
* **Type** – geeft aan of het volume **gelaagd** (de standaardinstelling) of **lokaal vastgemaakt**.
* **Capaciteit** – Hiermee geeft u de hoeveelheid gegevens die worden gebruikt in vergelijking met de totale hoeveelheid gegevens die kunnen worden opgeslagen door de initiator (server).
* **Back-up** : In het geval van de StorSimple Virtual Array automatisch alle volumes zijn ingeschakeld voor back-up.
* **Verbonden hosts** – Hiermee geeft u de initiators (servers) die toegang hebben tot dit volume.

![Details van volumes](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Volg de instructies in deze zelfstudie de volgende taken uitvoeren:

* Een volume toevoegen
* Een volume wijzigen
* Een volume offline halen
* Een volume verwijderen

## <a name="add-a-volume"></a>Een volume toevoegen

1. Uit de StorSimple overzichtsblade service, klikt u op **volume toevoegen** vanuit de opdrachtbalk. Hiermee opent u de **volume toevoegen** blade.
   
    ![Volume toevoegen](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. In de **volume toevoegen** blade, doet u het volgende:
   
   * In de **volumenaam** veld, voer een unieke naam voor het volume. De naam moet een tekenreeks met 3 tot en met 127 tekens.
   * In de **Type** vervolgkeuzelijst lijst, Geef op of maak een **gelaagd** of **lokaal vastgemaakt** volume. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, selecteert u **lokaal vastgemaakt volume**. Voor alle andere gegevens, selecteert u **gelaagd** volume.
   * In de **capaciteit** veld, geeft u de grootte van het volume. Een gelaagd volume moet liggen tussen 500 GB en 5 TB en een lokaal vastgemaakt volume moet tussen de 50 GB en 500 GB.
   * * Klik op **verbonden hosts**, selecteert u een Acces controlerecord (ACR) overeenkomt met de iSCSI-initiator die u wilt verbinding maken met dit volume en klik vervolgens op **Selecteer**.
3. Als u wilt een nieuw verbonden host toevoegt, klikt u op **nieuwe toevoegen**, voer een naam voor de host en de iSCSI Qualified Name (IQN) en klik vervolgens op **toevoegen**.
   
    ![Volume toevoegen](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Wanneer u klaar bent met het configureren van het volume, klikt u op **maken**. Een volume wordt gemaakt met de opgegeven instellingen en u ziet een melding op de is gemaakt van dezelfde. Standaard wordt de back-up worden ingeschakeld voor het volume.
5. Om te bevestigen of het volume is gemaakt, gaat u naar de **Volumes** blade. Hier ziet u het volume dat wordt vermeld.
   
    ![Volume maken geslaagd](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Een volume wijzigen

Een volume niet wijzigen als u wilt wijzigen van de hosts die toegang hebben tot het volume. De overige kenmerken van een volume kunnen niet worden gewijzigd nadat het volume is gemaakt.

#### <a name="to-modify-a-volume"></a>Een volume wijzigen

1. Uit de **Volumes** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop het volume dat u wenst dat u wilt wijzigen zich bevindt.
2. **Selecteer** het volume en klik op **verbonden hosts** de momenteel verbonden host bekijken en aanpassen aan een andere server.
   
    ![Volume bewerken](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Sla de wijzigingen door te klikken op de **opslaan** opdrachtbalk. De opgegeven instellingen worden toegepast en u ziet een melding.

## <a name="take-a-volume-offline"></a>Een volume offline halen

Mogelijk moet u een volume offline halen als u van plan bent om te wijzigen of te verwijderen. Als een volume offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet het volume offline nemen op de host, maar ook op het apparaat.

#### <a name="to-take-a-volume-offline"></a>Op een volume offline halen

1. Zorg ervoor dat het betreffende volume zich niet in gebruik voordat deze offline te zetten.
2. Het volume offline op de host eerst duren. Hierdoor is er mogelijk kosten in rekening van beschadiging van gegevens op het volume. Raadpleeg de instructies voor het hostbesturingssysteem voor specifieke stappen.
3. Plaats het volume op de matrix offline nadat het volume op de host offline is, door de volgende stappen uit:
   
   * Uit de **Volumes** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop het volume dat u wenst dat u offline zetten zich bevindt.
   * **Selecteer** het volume en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **offline zetten**.
     
        ![Offline volume](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Lees de informatie in de **offline zetten** blade en controleer of de acceptatie van de bewerking. Klik op **offline zetten** offline te nemen het volume. U ziet een melding van de bewerking wordt uitgevoerd.
   * Om te bevestigen dat het volume is offline is gehaald, gaat u naar de **Volumes** blade. U ziet de status van het volume als offline.
     
       ![Bevestiging van de offline volume](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Een volume verwijderen

> [!IMPORTANT]
> U kunt een volume alleen verwijderen als deze offline is.
> 
> 

De volgende stappen als u wilt verwijderen van een volume.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Uit de **Volumes** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop het volume dat u wilt verwijderen zich bevindt.
2. **Selecteer** het volume en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **verwijderen**.
   
    ![Volume verwijderen](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Controleer de status van het volume dat u wilt verwijderen. Als het volume dat u wilt verwijderen niet offline is, het offline halen eerst de stappen in [een volume offline halen](#take-a-volume-offline).
4. Wanneer u wordt gevraagd om bevestiging in de **verwijderen** blade, accepteer de bevestiging en klik op **verwijderen**. Het volume worden nu verwijderd en de **Volumes** blade ziet u de bijgewerkte lijst van volumes in de virtuele matrix.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [klonen van een StorSimple-volume](storsimple-virtual-array-clone.md).


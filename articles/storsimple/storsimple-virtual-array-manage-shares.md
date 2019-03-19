---
title: StorSimple Virtual Array shares beheren | Microsoft Docs
description: Beschrijving van de StorSimple Device Manager en wordt uitgelegd hoe u deze gebruiken voor het beheren van shares op uw StorSimple Virtual Array.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 82a6cdb6c9a39a0d196049a7ba662681ea06b36a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58092426"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a>De StorSimple Device Manager-service gebruiken voor het beheren van shares op de StorSimple Virtual Array

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt uitgelegd hoe u de service StorSimple Device Manager maken en beheren van shares op uw StorSimple Virtual Array gebruiken.

De StorSimple Device Manager-service is een uitbreiding in de Azure-portal waarmee u uw StorSimple-oplossing beheren via één webinterface. Naast het beheren van bestandsshares en volumes, kunt u de service StorSimple Device Manager kunt gebruiken om te bekijken en beheren van apparaten, waarschuwingen bekijken, back-upbeleid beheren en de back-upcatalogus beheren.

## <a name="share-types"></a>Typen delen

StorSimple-bestandsshares kunnen worden:

* **Lokaal vastgemaakt**: Gegevens in deze shares blijft op de matrix te allen tijde en wordt niet naar de cloud worden gelekt.
* **Gelaagde**: Gegevens in deze shares kunnen worden gelekt naar de cloud. Wanneer u een gelaagde share maakt, wordt ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld, als u een share 1 TB hebt ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou in de cloud worden gebruikt wanneer de gegevenslagen. Dit wordt op zijn beurt betekent dat als u van de lokale ruimte op het apparaat uitvoert, u kunt geen een gelaagde share inrichten (omdat de 10% vereist op de lokale laag niet meer beschikbaar).

### <a name="provisioned-capacity"></a>Ingerichte capaciteit

Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk type delen.

| **Limiet-ID** | **Limiet** |
| --- | --- |
| Minimale grootte van een gelaagde share |500 GB |
| Maximale grootte van een gelaagde share |20 TB |
| Minimale grootte van een lokaal vastgemaakt share |50 GB |
| Maximale grootte van een lokaal vastgemaakt share |2 TB |

## <a name="the-shares-blade"></a>De blade Shares

De **Shares** menu op de overzichtsblade van uw StorSimple-service geeft de lijst van storage-shares op een opgegeven StorSimple-matrix en kunt u ze kunt beheren.

![Shares-blade](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Een share bestaat uit een reeks kenmerken:

* **Sharenaam** : een beschrijvende naam moet uniek zijn en helpt bij het identificeren van de share.
* **Status** – online of offline kan zijn. Als een share offline is, kunnen gebruikers van de share zich niet openen.
* **Type** – geeft aan of de share **gelaagd** (de standaardinstelling) of **lokaal vastgemaakt**.
* **Capaciteit** – Hiermee geeft u de hoeveelheid gegevens die worden gebruikt in vergelijking met de totale hoeveelheid gegevens die kunnen worden opgeslagen op de share.
* **Beschrijving** : een optionele instelling waarmee de share wordt beschreven.
* **Machtigingen** -de NTFS-machtigingen voor de share die kunnen worden beheerd via Windows Explorer.
* **Back-up** : In het geval van de StorSimple Virtual Array automatisch alle shares zijn ingeschakeld voor back-up.

![Details van shares](./media/storsimple-virtual-array-manage-shares/share-details.png)

Volg de instructies in deze zelfstudie de volgende taken uitvoeren:

* Een share toevoegen
* Wijzigen van een share
* Een share offline halen
* Een share verwijderen

## <a name="add-a-share"></a>Een share toevoegen

1. Uit de StorSimple overzichtsblade service, klikt u op **+ bestandsshare toevoegen** vanuit de opdrachtbalk. Hiermee opent u de **toevoegen share** blade.

    ![Share toevoegen](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. In de **toevoegen share** blade, doet u het volgende:
   
   1. In de **sharenaam** veld, voer een unieke naam voor de share. De naam moet een tekenreeks met 3 tot en met 127 tekens.

   2. Een optionele **beschrijving** voor de share. De beschrijving kunt identificeren de eigenaren van de share.

   3. In de **Type** vervolgkeuzelijst lijst, Geef op of maak een **gelaagd** of **lokaal vastgemaakt** delen. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, selecteert u **lokaal vastgemaakt share**. Voor alle andere gegevens, selecteert u **gelaagd** delen.

   4. In de **capaciteit** veld, geeft u de grootte van de share. Een gelaagde share moet tussen 500 GB en 20 TB en een lokaal vastgemaakt share moet tussen de 50 GB en 2 TB.

   5. In de **standaard volledige machtigingen ingesteld op** veld, de machtigingen toewijzen aan de gebruiker of de groep die toegang heeft tot deze share. Geef de naam van de gebruiker of de gebruikersgroep in _john@contoso.com_ indeling. U wordt aangeraden dat u een gebruikersgroep (in plaats van één gebruiker) gebruiken om toe te staan van beheerdersbevoegdheden voor toegang tot deze shares. Als u de bevoegdheden hier hebt toegewezen, kunt u Verkenner gebruiken om de bevoegdheden te wijzigen.
3. Wanneer u klaar bent met het configureren van uw bestandsshare, klikt u op **maken**. Een share wordt gemaakt met de opgegeven instellingen en u ziet een melding. Standaard wordt de back-up worden ingeschakeld voor de share.
4. Om te bevestigen of de share is gemaakt, gaat u naar de **Shares** blade. Hier ziet u de share die worden vermeld.
   
    ![Bestandsshare maken geslaagd](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Wijzigen van een share

Wijzigen van een share wanneer u nodig hebt om te wijzigen van de beschrijving van de share. Er zijn geen andere eigenschappen van de share kunnen worden gewijzigd nadat de share is gemaakt.

#### <a name="to-modify-a-share"></a>Wijzigen van een share

1. Uit de **Shares** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop de share die u wenst dat u wilt wijzigen zich bevindt.
2. **Selecteer** de share bij de huidige beschrijving bekijken en wijzigen.
3. Sla de wijzigingen door te klikken op de **opslaan** opdrachtbalk. De opgegeven instellingen worden toegepast en u ziet een melding.
   
    ![ Share bewerken](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Een share offline halen

Mogelijk moet u een share offline halen als u van plan bent om te wijzigen of te verwijderen. Als een share offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet de share offline nemen op de host, maar ook op het apparaat.

#### <a name="to-take-a-share-offline"></a>Op een share offline halen

1. Zorg ervoor dat de share in kwestie zich niet in gebruik voordat deze offline te zetten.
2. De share op de matrix offline nemen door de volgende stappen uit:
   
    1. Uit de **Shares** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop de share die u wenst dat u offline zetten zich bevindt.

    2. **Selecteer** de share en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **offline zetten**.
     
        ![Offline delen](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Lees de informatie in de **offline zetten** blade en controleer of de acceptatie van de bewerking. Klik op **offline zetten** offline te nemen de share. U ziet een melding van de bewerking wordt uitgevoerd.

    4. Om te bevestigen dat de share is offline is gehaald, gaat u naar de **Shares** blade. U ziet de status van de share als offline.

## <a name="delete-a-share"></a>Een share verwijderen

> [!IMPORTANT]
> U kunt een bestandsshare alleen verwijderen als deze offline is.


De volgende stappen als u wilt verwijderen van een share.

#### <a name="to-delete-a-share"></a>Een share verwijderen

1. Uit de **Shares** instellen op de overzichtsblade van de StorSimple-service, selecteert u de virtuele matrix waarop de share die u wilt verwijderen zich bevindt.
2. **Selecteer** de share en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **verwijderen**.
   
    ![Share verwijderen](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Controleer de status van de share die u wilt verwijderen. Als de share die u wilt verwijderen niet offline is, het offline halen eerst. Volg de stappen in [een share offline halen](#take-a-share-offline).
4. Wanneer u wordt gevraagd om bevestiging in de **verwijderen** blade, accepteer de bevestiging en klik op **verwijderen**. De share wordt nu verwijderd en de **Shares** blade ziet u de bijgewerkte lijst met shares binnen de virtuele matrix.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [klonen van een StorSimple-share](storsimple-virtual-array-clone.md).


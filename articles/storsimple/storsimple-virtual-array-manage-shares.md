---
title: Virtuele StorSimple-matrix shares kunt beheren | Microsoft Docs
description: Worden Apparaatbeheer StorSimple beschreven en wordt uitgelegd hoe u deze voor het beheren van shares op uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: e5c62689de36baa175001f5f4f70d87568876ef0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van shares op de virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u met de service Manager voor StorSimple-apparaat maken en beheren van shares op uw virtuele StorSimple-matrix.

De service Manager voor StorSimple-apparaat is een uitbreiding in de Azure portal waarmee u uw StorSimple-oplossing beheren via een enkel webinterface. Naast het beheren van bestandsshares en volumes, kunt u de service Manager voor StorSimple-apparaat om te bekijken en beheren van apparaten, waarschuwingen weergeven back-upbeleid beheren en de back-catalogus beheren.

## <a name="share-types"></a>Typen delen

StorSimple-shares zijn:

* **Lokaal vastgemaakt**: gegevens in deze shares blijft op de matrix te allen tijde en komt niet in de cloud worden gelekt.
* **Gelaagde**: gegevens in deze shares kunnen worden gelekt naar de cloud. Bij het maken van een gelaagde share ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld: als u een share 1 TB ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB in de cloud moet worden gebruikt wanneer de gegevenslagen. Dit wordt op zijn beurt betekent dat als u buiten de lokale ruimte op het apparaat uitvoeren, u kunt geen een gelaagde share inrichten (omdat de 10% vereist op de lokale laag niet meer beschikbaar).

### <a name="provisioned-capacity"></a>Ingerichte capaciteit

Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk sharetype.

| **Limiet-ID** | **Limiet** |
| --- | --- |
| Minimale grootte van een gelaagde share |500 GB |
| Maximale grootte van een gelaagde share |20 TB |
| Minimale grootte van een lokaal vastgemaakt share |50 GB |
| Maximale grootte van een lokaal vastgemaakt share |2 TB |

## <a name="the-shares-blade"></a>De blade Shares

De **Shares** menu op de blade voor een overzicht van het StorSimple-service geeft een lijst met storage-shares op een gegeven StorSimple-matrix en kunt u ze kunt beheren.

![Blade shares](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Een share bestaat uit een reeks van kenmerken:

* **Sharenaam** – een beschrijvende naam die moet uniek en kunt u de share te identificeren.
* **Status** – online of offline kan zijn. Als een share offline is, wordt gebruikers van de share niet mogelijk om deze te openen.
* **Type** – Hiermee wordt aangegeven of de share **tiers verdeelde** (de standaardinstelling) of **lokaal vastgemaakt**.
* **Capaciteit** – geeft de hoeveelheid gegevens die worden gebruikt in vergelijking met de totale hoeveelheid gegevens die kunnen worden opgeslagen op de share.
* **Beschrijving** : een optionele instelling waarmee de share te beschrijven.
* **Machtigingen** -het NTFS-machtigingen voor de share die kunnen worden beheerd via de Windows Verkenner.
* **Back-** – voor het geval van de virtuele StorSimple-matrix, alle shares automatisch ingeschakeld voor back-up.

![Shares details](./media/storsimple-virtual-array-manage-shares/share-details.png)

Volg de instructies in deze zelfstudie aan de volgende taken uitvoeren:

* Share toevoegen
* Een share wijzigen
* Een share offline zetten
* Een share verwijderen

## <a name="add-a-share"></a>Share toevoegen

1. Klik op de StorSimple-service samenvatting blade **+ bestandsshare toevoegen** uit de opdrachtbalk. Hiermee opent u de **toevoegen share** blade.

    ![Share toevoegen](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. In de **toevoegen share** blade het volgende doen:
   
    1. In de **sharenaam** en voer een unieke naam voor de share. De naam moet een tekenreeks met 3 tot en met 127 tekens.

    2. Een optionele **beschrijving** voor de share. De beschrijving kunt identificeren de eigenaren van de bestandsshare.

    3. In de **Type** dropdown lijst, opgeven of maken van een **tiers verdeelde** of **lokaal vastgemaakt** delen. Selecteer voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, **lokaal vastgemaakt share**. Voor alle overige gegevens selecteert **tiers verdeelde** delen.

    4. In de **capaciteit** veld, geeft u de grootte van de share. Een gelaagde share moet tussen 500 GB en 20 TB en een lokaal vastgemaakt share moet tussen 50 GB en 2 TB.

    5. In de **standaard volledige machtigingen ingesteld op** veld, machtigingen toewijzen aan de gebruiker of de groep die toegang heeft tot deze share. Geef de naam van de gebruiker of de gebruikersgroep in  _john@contoso.com_  indeling. Het is raadzaam dat u een gebruikersgroep (in plaats van een enkele gebruiker) om toe te staan beheerdersbevoegdheden voor toegang tot deze shares. Nadat u hier de machtigingen hebt toegewezen, kunt u Windows Verkenner vervolgens gebruiken om deze machtigingen te wijzigen.
3. Wanneer u klaar bent met het configureren van de share, klikt u op **maken**. Een share met de opgegeven instellingen wordt gemaakt en u ziet een melding. Standaard wordt de back-up worden ingeschakeld voor de share.
4. Om te bevestigen dat de share is gemaakt, gaat u naar de **Shares** blade. Hier ziet u de share die wordt vermeld.
   
    ![Share maken geslaagd](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Een share wijzigen

Een share wijzigen als u wilt wijzigen, de beschrijving van de share. Er zijn geen andere eigenschappen van de share kunnen worden gewijzigd zodra de share is gemaakt.

#### <a name="to-modify-a-share"></a>Wijzigen van een share

1. Van de **Shares** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop de share die u wenst te maken van zich bevindt.
2. **Selecteer** de share bij de huidige beschrijving bekijken en wijzigen.
3. Sla de wijzigingen door te klikken op de **opslaan** opdrachtbalk. De opgegeven instellingen worden toegepast en u ziet een melding.
   
    ![ Share bewerken](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Een share offline zetten

Mogelijk moet u een share offline zetten wanneer u van plan bent om te wijzigen of verwijderen. Als een share offline is, is het niet beschikbaar voor lees-/ schrijftoegang. U moet de share offline nemen op de host, evenals op het apparaat.

#### <a name="to-take-a-share-offline"></a>Offline te nemen een share

1. Zorg ervoor dat de betreffende share niet gebruikt wordt voordat deze offline wordt gezet.
2. De share op de matrix offline nemen door de volgende stappen uit te voeren:
   
    1. Van de **Shares** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop de share die u wenst dat u offline te halen zich bevindt.

    2. **Selecteer** de share en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **offline zetten**.
     
        ![Offline share](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Lees de informatie in de **offline zetten** blade en de bevestiging van de bewerking te bevestigen. Klik op **offline zetten** offline te nemen de share. U ziet een melding van de bewerking wordt uitgevoerd.

    4. Om te bevestigen dat de share is offline is gehaald, gaat u naar de **Shares** blade. U ziet de status van de share als offline.

## <a name="delete-a-share"></a>Een share verwijderen

> [!IMPORTANT]
> U kunt een share alleen verwijderen als deze offline is.


De volgende stappen voor het verwijderen van een share.

#### <a name="to-delete-a-share"></a>Verwijderen van een share

1. Van de **Shares** instellen op de blade van StorSimple-service-samenvatting, de virtuele matrix selecteren waarop de share die u wilt verwijderen zich bevindt.
2. **Selecteer** de share en klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **verwijderen**.
   
    ![Share verwijderen](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Controleer de status van de share die u wilt verwijderen. Als de share die u wilt verwijderen niet offline is, het offline halen eerst. Volg de stappen in [offline zetten van een share](#take-a-share-offline).
4. Als u wordt gevraagd om bevestiging in de **verwijderen** blade, accepteer de bevestiging en klik op **verwijderen**. De share wordt nu verwijderd en de **Shares** blade ziet u de bijgewerkte lijst met shares binnen de virtuele-matrix.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [klonen van een StorSimple-share](storsimple-virtual-array-clone.md).


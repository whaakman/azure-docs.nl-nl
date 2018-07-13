---
title: StorSimple Device Manager-service implementeren | Microsoft Docs
description: Uitleg over het maken en verwijderen van de StorSimple Device Manager-service in Azure portal en wordt beschreven hoe u voor het beheren van de serviceregistratiesleutel.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 1881a0625b107ae1a90e5b772f5296a4d728973d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38688150"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a>De StorSimple Device Manager-service voor StorSimple Virtual Array implementeren
## <a name="overview"></a>Overzicht

De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van de apparaten uit de Microsoft Azure portal wordt uitgevoerd in een browser. Hiermee kunt u voor het bewaken van alle apparaten die zijn verbonden met de StorSimple Device Manager-service vanaf een centrale locatie, waardoor de administratieve overhead worden geminimaliseerd.

De algemene taken met betrekking tot een StorSimple Device Manager-service zijn:

* Een service maken
* Een service verwijderen
* De serviceregistratiesleutel ophalen
* De serviceregistratiesleutel opnieuw genereren

Deze zelfstudie wordt beschreven hoe u elk van de voorgaande taken uitvoert. De informatie in dit artikel is alleen van toepassing op virtuele StorSimple-matrices. Voor meer informatie over de StorSimple 8000-serie, gaat u naar [implementeren van een StorSimple Manager-service](storsimple-manage-service.md).

## <a name="create-a-service"></a>Een service maken

Voor het maken van een service, moet u beschikken over:

* Een abonnement met een Enterprise Agreement
* Een actieve Microsoft Azure storage-account
* De informatie die wordt gebruikt voor toegangsbeheer

U kunt ook kiezen voor het genereren van een storage-account wanneer u de service maakt.

Een enkele service kunt meerdere apparaten beheren. Een apparaat kan niet echter meerdere services omvatten. Een grote onderneming kan meerdere exemplaren van de service te werken met verschillende abonnementen, organisaties, of zelfs implementatie locaties hebben.

> [!NOTE]
> U moet afzonderlijke exemplaren van StorSimple Device Manager-service voor het beheer van apparaten uit de StorSimple 8000-serie en virtuele StorSimple-matrices.


Voer de volgende stappen uit voor het maken van een service.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Een service verwijderen

Voordat u een service verwijdert, controleert u dat er zijn geen verbonden apparaten worden gebruikt. Als de service gebruikt wordt, schakelt u de verbonden apparaten. De bewerking uitschakelen wordt de verbinding tussen het apparaat en de service-Server, maar behouden de apparaatgegevens in de cloud.

> [!IMPORTANT]
> Nadat een service is verwijderd, de bewerking kan niet ongedaan worden gemaakt. Een apparaat dat is met de service moet worden de fabrieksinstellingen herstellen voordat deze kan worden gebruikt met een andere service. In dit scenario wordt gaan de lokale gegevens op het apparaat, evenals de configuratie, verloren.
 

Voer de volgende stappen uit als een service wilt verwijderen.

#### <a name="to-delete-a-service"></a>Verwijderen van een service

1. Ga naar **alle resources**. Zoeken naar uw StorSimple Device Manager-service. Selecteer de service die u wilt verwijderen.
   
    ![Selecteer service verwijderen](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Ga naar uw servicedashboard om te controleren of er zijn geen apparaten die zijn verbonden met de service. Als er geen apparaten geregistreerd bij deze service, ziet u ook een banner weergegeven aan het effect. Klik op **Verwijderen**.
   
    ![Service verwijderen](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** in het bevestigingsbericht. 
   
    ![Service verwijderen bevestigen](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Het duurt een paar minuten voor de service moet worden verwijderd. Nadat de service is verwijderd, krijgt u bericht.
   
    ![Geslaagde verwijdering](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

De lijst met services wordt vernieuwd.

 ![Bijgewerkte lijst met services](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen
Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Voor het registreren van uw eerste StorSimple-apparaat, moet u de serviceregistratiesleutel. Om extra apparaten registreren met een bestaande StorSimple-service, moet u de registratiesleutel zowel de versleutelingssleutel voor servicegegevens (die wordt gegenereerd op het eerste apparaat tijdens de registratie). Zie voor meer informatie over de versleutelingssleutel voor servicegegevens [StorSimple security](storsimple-security.md). U kunt de registratiesleutel krijgen door de toegang tot de **sleutels** blade voor uw service.

De volgende stappen uitvoeren om de serviceregistratiesleutel ophalen.

#### <a name="to-get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen
1. In de **StorSimple Device Manager** blade, Ga naar **Management &gt;**  **sleutels**.
   
   ![De blade Sleutels](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. In de **sleutels** blade een serviceregistratiesleutel wordt weergegeven. Kopieer de registratiesleutel die met behulp van het pictogram voor kopiëren. 

Houd de serviceregistratiesleutel op een veilige locatie. U moet deze sleutel, evenals de versleutelingssleutel voor servicegegevens, extra apparaten registreren bij deze service. Nadat u de serviceregistratiesleutel, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface te configureren.

## <a name="regenerate-the-service-registration-key"></a>De serviceregistratiesleutel opnieuw genereren
U moet een serviceregistratiesleutel opnieuw genereren als u bent verplicht om uit te voeren rouleren van de sleutel of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereert, wordt de nieuwe sleutel alleen gebruikt voor latere apparaten registreren. De apparaten die al zijn geregistreerd, worden niet beïnvloed door dit proces.

De volgende stappen uitvoeren om een serviceregistratiesleutel opnieuw genereren.

#### <a name="to-regenerate-the-service-registration-key"></a>De serviceregistratiesleutel opnieuw genereren
1. In de **StorSimple Device Manager** blade, Ga naar **Management &gt;**  **sleutels**.
   
   ![De blade Sleutels](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. In de **sleutels** blade, klikt u op **opnieuw genereren**.
   
   ![Klik op sleutel opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. In de **serviceregistratiesleutel opnieuw genereren** blade, Controleer de actie vereist als de sleutels zijn opnieuw gegenereerd. De volgende apparaten die zijn geregistreerd bij deze service gebruikt de nieuwe registratiesleutel. Klik op **opnieuw genereren** om te bevestigen. U krijgt bericht wanneer de registratie voltooid is.
   
   ![Controleer of de sleutel opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Een nieuwe serviceregistratiesleutel wordt weergegeven.
   
    ![Controleer of de sleutel opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Kopieer deze sleutel en bewaar het voor het registreren van nieuwe apparaten met deze service.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [aan de slag](storsimple-virtual-array-deploy1-portal-prep.md) met een StorSimple Virtual Array.
* Meer informatie over het [beheren van uw StorSimple-apparaat](storsimple-ova-web-ui-admin.md).


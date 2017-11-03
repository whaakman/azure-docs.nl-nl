---
title: Service Manager voor StorSimple-apparaat implementeren | Microsoft Docs
description: Wordt uitgelegd hoe u maken en verwijderen van de service Manager voor StorSimple-apparaat in de Azure portal en wordt beschreven hoe u de serviceregistratiesleutel beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 1881a0625b107ae1a90e5b772f5296a4d728973d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a>Implementeer de service Manager voor StorSimple-apparaat voor virtuele StorSimple-matrix
## <a name="overview"></a>Overzicht

De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van de apparaten uit de Microsoft Azure-portal uitgevoerd in een browser. Hiermee kunt u voor het bewaken van de apparaten die zijn verbonden met de service Manager voor StorSimple-apparaat vanaf een centrale locatie, waardoor administratieve last voor het minimaliseren.

De algemene taken met betrekking tot een service Manager voor StorSimple-apparaat zijn:

* Een service maken
* Een service verwijderen
* De serviceregistratiesleutel ophalen
* Opnieuw genereren van de serviceregistratiesleutel

Deze zelfstudie wordt beschreven hoe elk van de voorgaande taken uitvoeren. De informatie in dit artikel is alleen van toepassing op virtuele StorSimple-matrices. Voor meer informatie over StorSimple 8000-serie gaat u naar [een StorSimple Manager-service implementeren](storsimple-manage-service.md).

## <a name="create-a-service"></a>Een service maken

Voor het maken van een service, moet u beschikken over:

* Een abonnement met een Enterprise Agreement
* Een actieve Microsoft Azure storage-account
* De informatie die wordt gebruikt voor toegangsbeheer

U kunt ook kiezen voor het genereren van een opslagaccount bij het maken van de service.

Een enkele service kunt meerdere apparaten beheren. Een apparaat niet kan echter meerdere services omvatten. Een grote onderneming kan meerdere exemplaren van de service te werken met verschillende abonnementen behoren, organisaties of zelfs implementatie locaties hebben.

> [!NOTE]
> U moet afzonderlijke exemplaren van StorSimple-apparaat Manager-service voor het beheren van apparaten voor StorSimple 8000 serie en virtuele StorSimple-matrices.


Voer de volgende stappen uit voor het maken van een service.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Een service verwijderen

Zorg voordat u een service hebt verwijderd, dat geen verbonden apparaten wordt gebruikt. Als de service gebruikt wordt, schakelt u verbonden apparaten. De bewerking deactiveren server de verbinding tussen het apparaat en de service, maar behouden van de apparaatgegevens in de cloud.

> [!IMPORTANT]
> Een service wordt verwijderd, de bewerking kan niet ongedaan worden gemaakt. Elk apparaat dat is met de service moet worden de fabrieksinstellingen voordat deze kan worden gebruikt met een andere service. In dit scenario wordt de lokale gegevens op het apparaat, evenals de configuratie niet verloren.
 

Voer de volgende stappen uit om een service te verwijderen.

#### <a name="to-delete-a-service"></a>Een service verwijderen

1. Ga naar **alle resources**. Zoek uw StorSimple-apparaat Manager-service. Selecteer de service die u wilt verwijderen.
   
    ![Selecteer de service verwijderen](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Ga naar uw servicedashboard om te controleren of er zijn geen apparaten die zijn verbonden met de service. Als er geen apparaten die zijn geregistreerd met deze service zijn, ziet u ook een bannerbericht naar het effect. Klik op **Verwijderen**.
   
    ![Verwijderen van de service](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Wanneer u om bevestiging gevraagd, klikt u op **Ja** in het bevestigingsbericht weergegeven. 
   
    ![Service verwijderen bevestigen](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Duurt enkele minuten duren voordat de service moet worden verwijderd. Nadat de service is verwijderd, wordt u gewaarschuwd.
   
    ![Geslaagde service verwijderen](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

De lijst met services wordt vernieuwd.

 ![Bijgewerkte lijst met services](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen
Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Voor het registreren van uw eerste StorSimple-apparaat, moet u de serviceregistratiesleutel. Om extra apparaten registreren met een bestaande StorSimple-service, moet u zowel de registratiesleutel als de gegevensversleutelingssleutel van service (die is gegenereerd op de eerste apparaat tijdens de registratie). Zie voor meer informatie over de gegevensversleutelingssleutel van service [StorSimple security](storsimple-security.md). U kunt de registratiesleutel ophalen door het openen van de **sleutels** blade voor uw service.

Voer de volgende stappen uit om de serviceregistratiesleutel ophalen.

#### <a name="to-get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen
1. In de **StorSimple Apparaatbeheer** blade, gaat u naar **Management &gt;**  **sleutels**.
   
   ![De blade Sleutels](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. In de **sleutels** blade een serviceregistratiesleutel wordt weergegeven. Kopieer de registratiesleutel met behulp van het pictogram kopiëren. 

Houd de serviceregistratiesleutel op een veilige locatie. U moet deze sleutel, evenals de gegevensversleutelingssleutel service, extra apparaten registreren bij deze service. Nadat de serviceregistratiesleutel, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface configureren.

## <a name="regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
U moet een serviceregistratiesleutel genereren als u nodig zijn voor sleutel rotatie uitgevoerd of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel alleen gebruikt voor latere apparaten registreren. De apparaten die al zijn geregistreerd, worden hierdoor niet beïnvloed door dit proces.

De volgende stappen voor het genereren van een serviceregistratiesleutel uitvoeren.

#### <a name="to-regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
1. In de **StorSimple Apparaatbeheer** blade, gaat u naar **Management &gt;**  **sleutels**.
   
   ![De blade Sleutels](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. In de **sleutels** blade, klikt u op **genereren**.
   
   ![Klik op opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. In de **opnieuw genereren serviceregistratiesleutel** blade, Controleer de actie vereist als de sleutels opnieuw worden gegenereerd. De volgende apparaten die zijn geregistreerd bij deze service gebruikt de nieuwe registratiesleutel. Klik op **genereren** om te bevestigen. U ontvangt een melding nadat de registratie voltooid is.
   
   ![Bevestig sleutel opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Een nieuwe registratiecode van de service wordt weergegeven.
   
    ![Bevestig sleutel opnieuw genereren](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Kopieer deze sleutel en sla het voor het registreren van nieuwe apparaten aan deze service.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [aan de slag](storsimple-virtual-array-deploy1-portal-prep.md) met een virtueel StorSimple-matrix.
* Meer informatie over hoe [beheren van uw StorSimple-apparaat](storsimple-ova-web-ui-admin.md).


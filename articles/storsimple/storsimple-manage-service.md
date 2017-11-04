---
title: De StorSimple Manager-service implementeren | Microsoft Docs
description: Wordt uitgelegd hoe u maken en verwijderen van de StorSimple Manager-service in de klassieke Azure portal en wordt beschreven hoe u de serviceregistratiesleutel beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba3637a3a8b15b45c16bf5a00c1f4225bcfc5af8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-manager-service-in-the-azure-classic-portal"></a>Implementeer de StorSimple Manager-service in de klassieke Azure portal

## <a name="overview"></a>Overzicht
De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van de apparaten via de klassieke portal van Microsoft Azure wordt uitgevoerd in een browser. Hiermee kunt u voor het bewaken van de apparaten die zijn verbonden met de StorSimple Manager-service vanaf een centrale locatie, waardoor administratieve last voor het minimaliseren.

De StorSimple Manager-startpagina geeft een lijst van alle StorSimple Manager-services die u gebruiken kunt om uw StorSimple-opslagapparaten te beheren. De volgende informatie wordt weergegeven voor elke StorSimple Manager-service op de pagina StorSimple Manager:

* **Naam** : de naam die is toegewezen aan uw StorSimple Manager-service wanneer deze is gemaakt. **Naam van de service kan niet worden gewijzigd nadat de service is gemaakt. Dit geldt ook voor andere entiteiten, zoals apparaten, volumes, volumecontainers en back-upbeleid dat in de klassieke Azure portal kunnen niet worden gewijzigd.**
* **Status** – de status van de service, die kan worden **Active**, **maken**, of **Online**.
* **Locatie** – de geografische locatie op waarin het StorSimple-apparaat wordt geïmplementeerd.
* **Abonnement** – het Facturering abonnement dat is gekoppeld aan uw service.

De algemene taken die kunnen worden uitgevoerd via de pagina StorSimple Manager zijn:

* Een service maken
* Een service verwijderen
* De serviceregistratiesleutel ophalen
* Opnieuw genereren van de serviceregistratiesleutel

Deze zelfstudie wordt beschreven hoe elk van deze taken uitvoeren.

## <a name="create-a-service"></a>Een service maken
Gebruik de **snelle invoer** optie voor het maken van een StorSimple Manager-service als u wilt uw StorSimple-apparaat implementeren. Voor het maken van een service, moet u beschikken over:

* Een abonnement met een Enterprise Agreement
* Een actieve Microsoft Azure storage-account
* De informatie die wordt gebruikt voor toegangsbeheer

U kunt ook een standaardopslagaccount genereren bij het maken van de service.

Een enkele service kunt meerdere apparaten beheren. Een apparaat niet kan echter meerdere services omvatten. Een grote onderneming kan meerdere exemplaren van de service te werken met verschillende abonnementen behoren, organisaties of zelfs implementatie locaties hebben. Houd er rekening mee dat u verschillende exemplaren van de StorSimple Manager-service voor het beheren van apparaten voor StorSimple 8000 serie en virtuele StorSimple-matrices nodig.

> [!IMPORTANT] 
> Als u een niet-gebruikte service gemaakt (geen apparaat bewerkingen zijn uitgevoerd op deze resource) voordat augustus 2016 hebt, kan deze kan niet worden beheerd via Azure portal of de klassieke Azure-portal. U wordt aangeraden dat u een nieuwe service in de Azure portal maken.

Voer de volgende stappen uit voor het maken van een service.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Een service verwijderen
Zorg voordat u een service hebt verwijderd, dat geen verbonden apparaten wordt gebruikt. Als de service gebruikt wordt, schakelt u verbonden apparaten. De bewerking deactiveren server de verbinding tussen het apparaat en de service, maar behouden van de apparaatgegevens in de cloud.

> [!IMPORTANT] 
> Een service wordt verwijderd, de bewerking kan niet ongedaan worden gemaakt. Elk apparaat dat is met de service moet worden de fabrieksinstellingen voordat deze kan worden gebruikt met een andere service. In dit scenario wordt de lokale gegevens op het apparaat, evenals de configuratie niet verloren.

Voer de volgende stappen uit om een service te verwijderen.

### <a name="to-delete-a-service"></a>Een service verwijderen
1. Op de **StorSimple Manager-service** pagina, selecteert u de service die u wilt verwijderen.
2. Klik op **verwijderen** aan de onderkant van de pagina.
3. Klik op **Ja** in het bevestigingsbericht weergegeven. Duurt enkele minuten duren voordat de service moet worden verwijderd.

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen
Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Voor het registreren van uw eerste StorSimple-apparaat, moet u de serviceregistratiesleutel. Om extra apparaten registreren met een bestaande StorSimple-service, moet u zowel de registratiesleutel als de gegevensversleutelingssleutel van service (die is gegenereerd op de eerste apparaat tijdens de registratie). Zie voor meer informatie over de gegevensversleutelingssleutel van service [StorSimple security](storsimple-security.md). U kunt de registratiesleutel ophalen door het openen van **registratiesleutel** op de **Services** pagina.

Voer de volgende stappen uit om de serviceregistratiesleutel ophalen.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Houd de serviceregistratiesleutel op een veilige locatie. U moet deze sleutel, evenals de gegevensversleutelingssleutel service, extra apparaten registreren bij deze service. Nadat de serviceregistratiesleutel, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface configureren.

Zie voor meer informatie over het gebruik van deze registratiecode [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
U moet een serviceregistratiesleutel genereren als u nodig zijn voor sleutel rotatie uitgevoerd of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel alleen gebruikt voor latere apparaten registreren. De apparaten die al zijn geregistreerd, worden hierdoor niet beïnvloed door dit proces.

De volgende stappen voor het genereren van een serviceregistratiesleutel uitvoeren.

### <a name="to-regenerate-the-service-registration-key"></a>Opnieuw genereren van de serviceregistratiesleutel
1. Op de **StorSimple Manager-service** pagina, klikt u op **registratiesleutel**.
2. In de **Serviceregistratiesleutel** in het dialoogvenster, klikt u op **genereren**.
3. Hier ziet u een bevestigingsbericht. Klik op **OK** om door te gaan met het opnieuw genereren.
4. Een nieuwe registratiecode van de service wordt weergegeven.
5. Kopieer deze sleutel en sla het voor het registreren van nieuwe apparaten aan deze service.
6. Klik op het vinkje ![Vinkje](./media/storsimple-manage-service/HCS_CheckIcon.png) Dit dialoogvenster sluiten.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [StorSimple-implementatieproces](storsimple-deployment-walkthrough-u2.md).
* Meer informatie over [het beheren van uw StorSimple-opslagaccount](storsimple-manage-storage-accounts.md).
* Meer informatie over het [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

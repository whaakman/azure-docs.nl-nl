---
title: Een Azure IoT Central-toepassing maken | Microsoft Docs
description: Maak een nieuwe Azure IoT Central-toepassing. Maak een proefversie of betalen per gebruik-toepassing met behulp van een toepassingssjabloon.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 07b062bd39177f283bd38ed6b687b590c5c4fb46
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066019"
---
# <a name="create-an-azure-iot-central-application"></a>Een Azure IoT Central-toepassing maken

Als _maker_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Microsoft Azure IoT Central-toepassing te definiëren. In deze snelstart ziet u hoe u een Azure IoT Central-toepassing maakt die een voorbeeld_apparaatsjabloon_ en gesimuleerde _apparaten_ bevat.

## <a name="create-an-application"></a>Een app maken

Navigeer naar de pagina [​​Toepassingsbeheer ](https://aka.ms/iotcentral) van Azure IoT Central. U moet zich aanmelden met een persoonlijk account of werk- of schoolaccount van Microsoft.

Als u wilt beginnen met het maken van een nieuwe Azure IoT Central-toepassing, selecteert u **Nieuwe toepassing**. Hiermee gaat u naar de pagina **Toepassing maken**.

![Pagina Toepassing maken van Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

Een nieuwe Azure IoT Central-toepassing maken:

1. Kies een betalingsplan:
   - **Proefversies** van toepassingen zijn zeven dagen gratis en verlopen daarna. Voordat ze verlopen, kunnen ze op elk gewenst moment worden geconverteerd naar betalen per gebruik. Als u maakt een **proefversie** toepassing, moet u uw contactgegevens invoeren en kies of u wilt ontvangen informatie en tips van Microsoft.
   - **Betalen per gebruik**-toepassingen worden per apparaat in rekening gebracht, waarbij de eerste vijf apparaten gratis zijn. Als u maakt een **betalen per gebruik** toepassing, moet u selecteren om uw *Directory*, *Azure-abonnement*, en *regio*:
      - *Directory* is de Azure Active Directory (AD) om uw toepassing te maken. Deze bevat gebruikers-id's, referenties en andere organisatiegegevens. Als u een Azure AD hebt, is een gemaakt voor u bij het maken van een Azure-abonnement.
      - Als u een *Azure-abonnement* hebt, kunt u instanties van Azure-services maken. IoT Central zorgt ervoor dat resources in uw abonnement worden ingericht. Als u geen Azure-abonnement hebt, kunt u er een maken via de [Azure-aanmeldingspagina](https://aka.ms/createazuresubscription). Nadat u het Azure-abonnement hebt gemaakt, gaat u terug naar de pagina **Toepassing maken**. Uw nieuwe abonnement wordt weergegeven in de vervolgkeuzelijst.**Azure-abonnement**.
      - *Regio* is de fysieke locatie waar u de toepassing wilt maken. Normaal gesproken kiest u de regio die zich fysiek het dichtst bij uw apparaten bevindt om optimale prestaties te behalen. Op de pagina [Beschikbare producten per regio](https://azure.microsoft.com/regions/services/) kunt u zien in welke regio's Azure IoT Central beschikbaar is. Als u een regio hebt gekozen, kunt u uw toepassing naar een andere regio niet later verplaatsen.

      Lees meer over prijzen op de [prijzenpagina van IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

1. Kies een beschrijvende toepassingsnaam, zoals **Contoso IoT**. Azure IoT Central genereert een uniek URL-voorvoegsel voor u. U kunt dit URL-voorvoegsel wijzigen in iets dat gemakkelijker te onthouden is.

1. Kies een toepassingssjabloon. Een toepassingssjabloon kan vooraf gedefinieerde items bevatten, zoals apparaatsjablonen en dashboards om u op weg te helpen.

    | Toepassingsjabloon | Description |
    | -------------------- | ----------- |
    | Contoso-voorbeeld       | Hiermee maakt u een toepassing die onder andere een vooraf gemaakte apparaatjabloon voor een gekoelde verkoopautomaat bevat. Gebruik deze sjabloon als u wilt beginnen met het verkennen van Azure IoT Central. |
    | Devkits-voorbeeld       | Hiermee maakt u een toepassing met apparaatsjablonen die u in staat stelt verbinding te maken met een MXChip- of Raspberry Pi-apparaat. Gebruik deze sjabloon als u een apparaatontwikkelaar bent die met een van deze apparaten experimenteert. |
    | Aangepaste toepassing   | Hiermee maakt u een lege toepassing die u kunt vullen met uw eigen apparaatsjablonen en apparaten. |

1. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een IoT Central-toepassing gemaakt. Dit is de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Volg een rondleiding door IoT Central](overview-iot-central-tour.md)

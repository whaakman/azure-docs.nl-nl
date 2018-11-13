---
title: Een Azure IoT Central-toepassing maken | Microsoft Docs
description: Maak een nieuwe Azure IoT Central-toepassing. Maak een proefversie of betalen per gebruik-toepassing met behulp van een toepassingssjabloon.
author: viv-liu
ms.author: viviali
ms.date: 10/31/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: f0e5b9d6e873cad1a997bda2ee286c92ad3818d3
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959457"
---
# <a name="create-an-azure-iot-central-application"></a>Een Azure IoT Central-toepassing maken

Als _maker_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Microsoft Azure IoT Central-toepassing te definiëren. In deze snelstart ziet u hoe u een Azure IoT Central-toepassing maakt die een voorbeeld_apparaatsjabloon_ en gesimuleerde _apparaten_ bevat.

Navigeer naar de pagina [​​Toepassingsbeheer ](https://aka.ms/iotcentral) van Azure IoT Central. U moet zich aanmelden met een persoonlijk account of werk- of schoolaccount van Microsoft.

Als u wilt beginnen met het maken van een nieuwe Azure IoT Central-toepassing, selecteert u **Nieuwe toepassing**. Hiermee gaat u naar de pagina **Toepassing maken**.

![Pagina Toepassing maken van Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

Een nieuwe Azure IoT Central-toepassing maken:

1. Kies een betalingsplan:
    - **Proefversies** van toepassingen zijn zeven dagen gratis en verlopen daarna. Voordat ze verlopen, kunnen ze op elk gewenst moment worden geconverteerd naar betalen per gebruik.
    - **Betalen per gebruik**-toepassingen worden per apparaat in rekening gebracht, waarbij de eerste vijf apparaten gratis zijn.

    Lees meer over prijzen op de [prijzenpagina van IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

1. Kies een beschrijvende toepassingsnaam, zoals **Contoso IoT**. Azure IoT Central genereert een uniek URL-voorvoegsel voor u. U kunt dit URL-voorvoegsel wijzigen in iets dat gemakkelijker te onthouden is.

1. Kies een toepassingssjabloon. Een toepassingssjabloon kan vooraf gedefinieerde items bevatten, zoals apparaatsjablonen en dashboards om u op weg te helpen.
    | Toepassingsjabloon | Beschrijving |
    | -------------------- | ----------- |
    | Contoso-voorbeeld       | Hiermee maakt u een toepassing die onder andere een vooraf gemaakte apparaatjabloon voor een gekoelde verkoopautomaat bevat. Gebruik deze sjabloon als u wilt beginnen met het verkennen van Azure IoT Central. |
    | Devkits-voorbeeld       | Hiermee maakt u een toepassing met apparaatsjablonen die u in staat stelt verbinding te maken met een MXChip- of Raspberry Pi-apparaat. Gebruik deze sjabloon als u een apparaatontwikkelaar bent die met een van deze apparaten experimenteert. |
    | Aangepaste toepassing   | Hiermee maakt u een lege toepassing die u kunt vullen met uw eigen apparaatsjablonen en apparaten. |

1. Als u een **betalen per gebruik**-toepassing maakt, moet u uw *directory*, uw *Azure-abonnement* en *regio* selecteren. 
    - De *directory* is de Azure Active Directory waarmee uw toepassing wordt gemaakt. Deze bevat gebruikers-id's, referenties en andere organisatiegegevens. Als u geen AAD hebt, wordt er een voor u gemaakt op het moment dat u een Azure-abonnement maakt.

    - Als u een *Azure-abonnement* hebt, kunt u instanties van Azure-services maken. IoT Central zorgt ervoor dat resources in uw abonnement worden ingericht. Als u geen Azure-abonnement hebt, kunt u er een maken via de [Azure-aanmeldingspagina](https://aka.ms/createazuresubscription). Nadat u het Azure-abonnement hebt gemaakt, gaat u terug naar de pagina **Toepassing maken**. Uw nieuwe abonnement wordt weergegeven in de vervolgkeuzelijst.**Azure-abonnement**.

    - *Regio* is de fysieke locatie waar u de toepassing wilt maken. Normaal gesproken kiest u de regio die zich fysiek het dichtst bij uw apparaten bevindt om optimale prestaties te behalen. Op de pagina [Beschikbare producten per regio](https://azure.microsoft.com/regions/services/) kunt u zien in welke regio's Azure IoT Central beschikbaar is.

    > [!Note]
    > Als u eenmaal een regio hebt gekozen, kunt u de toepassing later niet naar een andere regio verplaatsen.

1. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een IoT Central-toepassing gemaakt. Dit is de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Volg een rondleiding door IoT Central](#overview-iot-central-tour)

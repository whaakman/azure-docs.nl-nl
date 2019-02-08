---
title: Speech Services gratis uitproberen
titleSuffix: Azure Cognitive Services
description: Aan de slag met de Speech-service is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: a378948d1bd9a48e85122a18ce34260056c9a1f1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860997"
---
# <a name="try-the-speech-service-for-free"></a>Speech Service gratis uitproberen

Aan de slag met de Speech-service is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.

Als u meer tijd nodig hebt, zich registreren voor een Microsoft Azure-account, wordt geleverd met $200 aan tegoed voor services die u op een betaald abonnement van de service spraak tot 30 dagen toepassen kunt.

Ten slotte biedt de Speech-service een gratis, klein volume laag die geschikt is voor het ontwikkelen van toepassingen. Zelfs nadat uw servicetegoed is verlopen, kunt u dit gratis abonnement behouden.

## <a name="free-trial"></a>Gratis proefversie

De gratis proefversie van 30 dagen hebt u toegang tot de standaardprijscategorie voor gedurende een beperkte periode.

Om u te registreren voor een gratis proefversie van 30 dagen:

1. Ga naar [probeer de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

1. Selecteer de **Speech-API's** tabblad.

   ![Tabblad voor spraak-Services](media/index/try-speech-api-free-trial1.png)

1. Onder **spraakservices**, selecteer de **API-sleutel ophalen** knop.

   ![API-sleutel](media/index/try-speech-api-free-trial2.png)

1. Ga akkoord met de voorwaarden en selecteer uw land/regio uit de vervolgkeuzelijst.

   ![Ga akkoord met voorwaarden](media/index/try-speech-api-free-trial3.png)

1. Meld u aan met uw Microsoft, Facebook, LinkedIn of GitHub-account.

    U kunt zich aanmelden voor een gratis Microsoft-account op de [portal voor Microsoft-account](https://account.microsoft.com/account). Als u wilt beginnen, klikt u op **aanmelden bij Microsoft** en wanneer u wordt gevraagd aan te melden, klikt u vervolgens **maken.** Volg de stappen voor het maken en te controleren of uw nieuwe Microsoft-account.

Nadat u zich bij probeer Cognitive Services aanmelden, begint de gratis proefversie. De weergegeven webpagina worden alle van de Azure Cognitive Services-services waarvoor u momenteel proefabonnementen hebt weergegeven. Twee abonnementssleutels worden vermeld naast **spraakservices**. U kunt beide sleutels gebruiken in uw toepassingen.

> [!NOTE]
> Alle gratis proefabonnementen komen in de regio VS-West. Wanneer u aanvragen, zorg ervoor dat u de `westus` eindpunt.

## <a name="new-azure-account"></a>Nieuwe Azure-account

Nieuwe Azure-accounts, ontvangen een tegoed van $200-services die beschikbaar is gedurende maximaal 30 dagen. U kunt dit tegoed kunt gebruiken om het verder verkennen de Speech-service of om te beginnen met de ontwikkeling van toepassingen.

Als u zich voor een nieuwe Azure-account, gaat u naar de [Azure registratiepagina](https://azure.microsoft.com/free/ai/), klikt u op **gratis starten** en maak een nieuwe Azure-account met behulp van uw Microsoft-account.

U kunt zich aanmelden voor een gratis Microsoft-account op de [portal voor Microsoft-account](https://account.microsoft.com/account). Als u wilt beginnen, klikt u op **aanmelden bij Microsoft** en wanneer u wordt gevraagd aan te melden, klikt u vervolgens **maken.** Volg de stappen voor het maken en te controleren of uw nieuwe Microsoft-account.

Volg de stappen in de volgende sectie voor een abonnement op de spraak-service starten na het maken van uw Azure-account.

## <a name="create-a-speech-resource-in-azure"></a>Een spraak-resource maken in Azure

Een bron van de spraak-service (gratis of betaalde laag) toevoegen aan uw Azure-account:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met uw Microsoft-account.

1. Selecteer **een resource maken** op linksboven in de portal.

    ![Een resource maken](media/index/try-speech-api-create-speech1.png)

1. In de **nieuw** venster, zoekt u **spraak**.

1. Selecteer in de lijst met zoekresultaten **spraak**.

    ![Selecteer spraak](media/index/try-speech-api-create-speech2.png)

1. Onder **spraak**, selecteer de **maken** knop.

    ![Selecteer de knop maken](media/index/try-speech-api-create-speech3.png)

1. Onder **maken**, invoeren:

    * Een naam voor de nieuwe resource. De naam van de kunt u onderscheid maken tussen meerdere abonnementen voor dezelfde service.
    * Kies het Azure-abonnement waaraan de nieuwe resource is gekoppeld om te bepalen hoe de kosten worden in rekening gebracht.
    * Kies de regio waarin de resource zal worden gebruikt. De spraak-service is momenteel beschikbaar in regio's Oost-Azië, Noord-Europa en VS-West.
    * Op een gratis of betaalde prijscategorie kiezen. Klik op **volledige prijsgegevens weergeven** voor volledige informatie over prijzen en gebruik quota's voor elke laag.
    * Maak een nieuwe resourcegroep voor dit abonnement spraak of het abonnement toewijzen aan een bestaande resourcegroep. Resourcegroepen houdt u uw verschillende Azure-abonnementen ingedeeld.
    * Voor een gemakkelijke manier toegang tot uw abonnement in de toekomst, selecteert u de **vastmaken aan dashboard** selectievakje.
    * Selecteer **maken.**

    ![Selecteer de knop maken](media/index/try-speech-api-create-speech4.png)

    Het duurt even om te maken en implementeren van uw nieuwe spraak-resource. Selecteer **snelstartgids** voor informatie over de nieuwe resource.

    ![Snelstartgids-Configuratiescherm](media/index/try-speech-api-create-speech5.png)

1. Onder **snelstartgids**, klikt u op de **sleutels** koppeling onder stap 1 om de abonnementssleutels van uw weer te geven. Elk abonnement heeft twee sleutels; u kunt beide sleutels gebruiken in uw toepassing. Selecteer de knop naast elke sleutel te kopiëren naar het Klembord plakken in uw code.

> [!NOTE]
> U kunt een onbeperkt aantal standard-laag-abonnementen maken in een of meerdere regio's. U kunt echter slechts één gratis-laag-abonnement maken. Modelimplementaties op de gratis laag die nog niet-gebruikte 7 dagen is uit bedrijf is genomen automatisch.

## <a name="switch-to-a-new-subscription"></a>Overschakelen naar een nieuw abonnement

Als u wilt overschakelen van één abonnement naar een andere, vervangen bijvoorbeeld wanneer de gratis proefversie verloopt of bij het publiceren van uw toepassing, door de regio en abonnement sleutel in uw code de regio en abonnement sleutel van de nieuwe Azure-resource.

> [!NOTE]
> Sleutels voor een gratis proefversie worden gemaakt in de VS-West (`westus`) regio. Een abonnement dat is gemaakt via het Azure-dashboard mogelijk in een andere regio, indien gewenst.

* Als uw toepassing gebruikmaakt van een [spraak SDK](speech-sdk.md), geef u de regiocode zoals `westus`, bij het maken van een spraak-configuratie.
* Als uw toepassing een van de spraakservice gebruikt [REST-API's](rest-apis.md), de regio is onderdeel van het eindpunt-URI die u gebruikt bij het maken van aanvragen.

Sleutels die zijn gemaakt voor een regio zijn alleen geldig in deze regio. Er wordt geprobeerd om ze te gebruiken met andere regio's leidt tot verificatiefouten.

## <a name="next-steps"></a>Volgende stappen

Voer een van onze quickstarts van 10 minuten of Bekijk onze SDK-voorbeelden:

> [!div class="nextstepaction"]
> [Snelstart: Water in C# ](quickstart-csharp-dotnet-windows.md) 
>  [spraak SDK-voorbeelden](speech-sdk.md#get-the-samples)

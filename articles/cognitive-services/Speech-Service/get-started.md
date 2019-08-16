---
title: Probeer de speech-service gratis uit
titleSuffix: Azure Cognitive Services
description: Het is eenvoudig en betaalbaar om aan de slag te gaan met de speech-service. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 41d12013ec7eaa4e2aae59e1b366cc511a41f749
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69535104"
---
# <a name="try-speech-services-for-free"></a>Speech Services gratis uitproberen

Aan de slag met de spraak Services is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.

Als u meer tijd nodig hebt, meldt u zich aan voor een Microsoft Azure-account. het wordt geleverd met $200 in de service credit die u Maxi maal 30 dagen kunt gebruiken in een betaald abonnement voor spraak Services.

Ten slotte biedt de spraak Services een gratis laag die geschikt is voor het ontwikkelen van toepassingen. Zelfs nadat uw servicetegoed is verlopen, kunt u dit gratis abonnement behouden.

## <a name="free-trial"></a>Gratis proefversie

De gratis proefversie van 30 dagen hebt u toegang tot de standaardprijscategorie voor gedurende een beperkte periode.

Om u te registreren voor een gratis proefversie van 30 dagen:

1. Ga naar [probeer de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

1. Selecteer de **Speech-API's** tabblad.

   ![Tabblad voor spraak-Services](media/index/try-speech-api-free-trial1.png)

1. Onder **spraak Services**selecteert u **API-sleutel ophalen**.

   ![API-sleutel](media/index/try-speech-api-free-trial2.png)

1. Ga akkoord met de voorwaarden en selecteer uw land/regio uit de vervolgkeuzelijst.

   ![Ga akkoord met voorwaarden](media/index/try-speech-api-free-trial3.png)

1. Meld u aan met uw Microsoft, Facebook, LinkedIn of GitHub-account.

    U kunt zich aanmelden voor een gratis Microsoft-account op de [portal voor Microsoft-account](https://account.microsoft.com/account). Als u aan de slag wilt gaan, selecteert u **Aanmelden met Microsoft** en selecteert u vervolgens **een account maken** wanneer u wordt gevraagd om u aan te melden. Volg de stappen voor het maken en te controleren of uw nieuwe Microsoft-account.

Nadat u zich bij probeer Cognitive Services aanmelden, begint de gratis proefversie. De weergegeven webpagina worden alle van de Azure Cognitive Services-services waarvoor u momenteel proefabonnementen hebt weergegeven. Naast **spraak Services**worden twee abonnements sleutels weer gegeven. U kunt beide sleutels gebruiken in uw toepassingen.

> [!NOTE]
> Alle gratis proefabonnementen komen in de regio VS-West. Wanneer u aanvragen, zorg ervoor dat u de `westus` eindpunt.

## <a name="new-azure-account"></a>Nieuwe Azure-account

Nieuwe Azure-accounts, ontvangen een tegoed van $200-services die beschikbaar is gedurende maximaal 30 dagen. U kunt dit tegoed gebruiken om meer te weten te komen over de spraak Services of om te beginnen met het ontwikkelen van toepassingen.

Als u zich wilt aanmelden voor een nieuw Azure-account, gaat u naar de [pagina aanmelden bij Azure](https://azure.microsoft.com/free/ai/), selecteert u **gratis starten** en maakt u een nieuw Azure-account met behulp van uw Microsoft-account.

U kunt zich aanmelden voor een gratis Microsoft-account op de [portal voor Microsoft-account](https://account.microsoft.com/account). Als u aan de slag wilt gaan, selecteert u **Aanmelden met Microsoft** en selecteert u vervolgens **een account maken** wanneer u wordt gevraagd om u aan te melden. Volg de stappen voor het maken en te controleren of uw nieuwe Microsoft-account.

Nadat u uw Azure-account hebt gemaakt, volgt u de stappen in de volgende sectie om een abonnement op de spraak services te starten.

## <a name="create-a-speech-resource-in-azure"></a>Een spraak-resource maken in Azure

Een speech Services-resource (gratis of betaalde laag) toevoegen aan uw Azure-account:

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
   * Kies de [regio](regions.md) waar de resource wordt gebruikt.
   * Op een gratis of betaalde prijscategorie kiezen. Selecteer **volledige prijs informatie weer geven** voor volledige informatie over prijzen en gebruiks quota's voor elke laag.
   * Maak een nieuwe resourcegroep voor dit abonnement spraak of het abonnement toewijzen aan een bestaande resourcegroep. Resourcegroepen houdt u uw verschillende Azure-abonnementen ingedeeld.
   * Voor een gemakkelijke manier toegang tot uw abonnement in de toekomst, selecteert u de **vastmaken aan dashboard** selectievakje.
   * Selecteer **maken.**

     ![Selecteer de knop maken](media/index/try-speech-api-create-speech4.png)

     Het duurt even om te maken en implementeren van uw nieuwe spraak-resource. Selecteer **snelstartgids** voor informatie over de nieuwe resource.

     ![Snelstartgids-Configuratiescherm](media/index/try-speech-api-create-speech5.png)

1. Onder **Quick**start selecteert u de koppeling **sleutels** onder stap 1 om uw abonnements sleutels weer te geven. Elk abonnement heeft twee sleutels; u kunt beide sleutels gebruiken in uw toepassing. Selecteer de knop naast elke sleutel te kopiëren naar het Klembord plakken in uw code.

> [!NOTE]
> U kunt een onbeperkt aantal standard-laag-abonnementen maken in een of meerdere regio's. U kunt echter slechts één gratis-laag-abonnement maken. Modelimplementaties op de gratis laag die nog niet-gebruikte 7 dagen is uit bedrijf is genomen automatisch.

## <a name="switch-to-a-new-subscription"></a>Overschakelen naar een nieuw abonnement

Als u wilt overschakelen van één abonnement naar een andere, vervangen bijvoorbeeld wanneer de gratis proefversie verloopt of bij het publiceren van uw toepassing, door de regio en abonnement sleutel in uw code de regio en abonnement sleutel van de nieuwe Azure-resource.

> [!NOTE]
> Sleutels voor een gratis proefversie worden gemaakt in de VS-West (`westus`) regio. Een abonnement dat is gemaakt via het Azure-dashboard mogelijk in een andere regio, indien gewenst.

* Als uw toepassing gebruikmaakt van een [spraak SDK](speech-sdk.md), geef u de regiocode zoals `westus`, bij het maken van een spraak-configuratie.
* Als uw toepassing gebruikmaakt van een van de rest- [api's](rest-apis.md)van spraak Services, maakt de regio deel uit van de EINDPUNT-URI die u gebruikt bij het maken van aanvragen.

Sleutels die zijn gemaakt voor een regio zijn alleen geldig in deze regio. Er wordt geprobeerd om ze te gebruiken met andere regio's leidt tot verificatiefouten.

## <a name="next-steps"></a>Volgende stappen

Voer een van onze quickstarts van 10 minuten of Bekijk onze SDK-voorbeelden:

> [!div class="nextstepaction"]
> [Snelstart: Spraak herkennen in C# ](quickstart-csharp-dotnet-windows.md) 
>  [Speech SDK](speech-sdk.md#get-the-samples) -voor beelden

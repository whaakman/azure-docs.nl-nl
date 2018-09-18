---
title: Speech Service gratis uitproberen
description: Ontdek hoe kunt u de Speech-service gratis proberen.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 5ae57764e2950d027e832eccec5393d4aec4486a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981829"
---
# <a name="try-the-speech-service-for-free"></a>Speech Service gratis uitproberen

Aan de slag met de Speech-service is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.

Als u meer tijd nodig hebt, zich registreren voor een Microsoft Azure-account, wordt geleverd met $200 aan tegoed voor services die u op een betaald abonnement van de service spraak tot 30 dagen toepassen kunt.

Ten slotte biedt de Speech-service een gratis, klein volume laag die geschikt is voor het ontwikkelen van toepassingen. Zelfs nadat uw servicetegoed is verlopen, kunt u dit gratis abonnement behouden.

## <a name="free-trial"></a>Gratis proefversie

De gratis proefversie van 30 dagen hebt u toegang tot de S0 standard prijscategorie gedurende een beperkte periode. 

Om u te registreren voor een gratis proefversie van 30 dagen:

1. Ga naar [probeer de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

1. Selecteer de **Speech-API's** tabblad.

   ![Tabblad voor spraak-Services](media/index/try-speech-api-free-trial1.png)
   
1. Onder **spraakservices <sup>PREVIEW</sup>**, selecteer de **API-sleutel ophalen** knop.

   ![API-sleutel](media/index/try-speech-api-free-trial2.png)

1. Ga akkoord met de voorwaarden en selecteer uw land uit de vervolgkeuzelijst.

   ![Ga akkoord met voorwaarden](media/index/try-speech-api-free-trial3.png)

1. Meld u aan met uw Microsoft, Facebook, LinkedIn of GitHub-account. Of, kunt u zich registreren voor een gratis Microsoft-account:

    * Ga naar de [portal voor Microsoft-account](https://account.microsoft.com/account).
    * Selecteer **aanmelden bij Microsoft**.

    ![Aanmelden](media/index/try-speech-api-free-trial4.png)

    * Wanneer u wordt gevraagd aan te melden, selecteert u **maakt u er een**.

    ![Nieuw account maken](media/index/try-speech-api-free-trial5.png)

    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toewijst en volg de instructies om uw nieuwe Microsoft-account te verifiëren.

Nadat u zich hebt aangemeld, begint de gratis proefversie. De weergegeven webpagina worden alle van de Azure Cognitive Services-services waarvoor u momenteel proefabonnementen hebt weergegeven. Twee abonnementssleutels worden vermeld naast **spraakservices**. U kunt beide sleutels gebruiken in uw toepassingen.

> [!NOTE]
> Alle gratis proefabonnementen komen in de regio VS-West. Wanneer u aanvragen, moet u het eindpunt dat overeenkomt met gebruiken voor uw regio.

## <a name="new-azure-account"></a>Nieuwe Azure-account

Nieuwe Azure-accounts, ontvangen een tegoed van $200-services die beschikbaar is gedurende maximaal 30 dagen. U kunt dit tegoed kunt gebruiken om het verder verkennen de Speech-service of om te beginnen met de ontwikkeling van toepassingen.

Om u te registreren voor een nieuwe Azure-account:

1. Ga naar de [Azure registratiepagina](https://azure.microsoft.com/free/ai/). 

1. Selecteer **begin gratis**.

    ![Start gratis](media/index/try-speech-api-new-azure1.png)

1. Meld u aan met uw Microsoft-account. Als u geen een:

    * Ga naar de [portal voor Microsoft-account](https://account.microsoft.com/account).
    * Selecteer **aanmelden bij Microsoft**.
    * Wanneer u wordt gevraagd aan te melden, selecteert u **maken.**
    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toewijst en volg de instructies om uw nieuwe Microsoft-account te verifiëren.

1. Voer de rest van de gegevens die zich aanmelden voor een account heeft aangevraagd. Geef uw land/regio en uw naam en een telefoonnummer en e-mailadres opgeven.

    ![Gegevens invoeren](media/index/try-speech-api-new-azure2.png)

    Uw identiteit verifiëren via de telefoon en door te geven van een creditcardnummer. (Uw creditcard worden niet in rekening gebracht.) kip, de gebruiker van Azure-overeenkomst te accepteren. 

    ![Overeenkomst te accepteren](media/index/try-speech-api-new-azure3.png)

Uw gratis Azure-account wordt gemaakt. Volg de stappen in de volgende sectie om te beginnen een abonnement op de spraak-service.

## <a name="create-a-speech-resource-in-azure"></a>Een spraak-resource maken in Azure

Een bron van de spraak-service toevoegen aan uw Azure-account:

1. Aanmelden bij de [Azure-portal](https://ms.portal.azure.com/) met behulp van uw Microsoft-account.

1. Selecteer **een resource maken** op linksboven in de portal.

    ![Een resource maken](media/index/try-speech-api-create-speech1.png)

1. In de **nieuw** venster, zoekt u **spraak**.

1. Selecteer in de lijst met zoekresultaten **spraak (preview)**.

    ![Selecteer spraak (preview)](media/index/try-speech-api-create-speech2.png)

1. Onder **spraak (preview)**, selecteer de **maken** knop.

    ![Selecteer de knop maken](media/index/try-speech-api-create-speech3.png)

1. Onder **maken**, invoeren:

    * Een naam voor de nieuwe resource. De naam van de kunt u onderscheid maken tussen meerdere abonnementen voor dezelfde service.
    * Kies het Azure-abonnement waaraan de nieuwe resource is gekoppeld om te bepalen hoe de kosten worden in rekening gebracht.
    * Kies de regio waarin de resource zal worden gebruikt. De spraak-service is momenteel beschikbaar in regio's Oost-Azië, Noord-Europa en VS-West.
    * Kies de prijscategorie, een van beide F0 (maximaal gratis abonnement) of S0 (standard-abonnement). Selecteer **volledige prijsgegevens weergeven** voor volledige informatie over prijzen en gebruik quota's voor elke laag.
    * Maak een nieuwe resourcegroep voor dit abonnement spraak of het abonnement toewijzen aan een bestaande resourcegroep. Resourcegroepen houdt u uw verschillende Azure-abonnementen ingedeeld.
    * Voor een gemakkelijke manier toegang tot uw abonnement in de toekomst, selecteert u de **vastmaken aan dashboard** selectievakje.
    * Selecteer **maken.**

    ![Selecteer de knop maken](media/index/try-speech-api-create-speech4.png)

    Het duurt even om te maken en implementeren van uw nieuwe spraak-resource. Selecteer **snelstartgids** voor informatie over de nieuwe resource.

    ![Snelstartgids-Configuratiescherm](media/index/try-speech-api-create-speech5.png)

1. Onder **snelstartgids**, selecteer de **sleutels** koppeling onder stap 1 om de abonnementssleutels van uw weer te geven. Elk abonnement heeft twee sleutels; u kunt beide sleutels gebruiken in uw toepassing. Selecteer de knop naast elke sleutel te kopiëren naar het Klembord plakken in uw code.

> [!NOTE]
> U kunt een onbeperkt aantal standard-laag-abonnementen maken in een of meerdere regio's. U kunt echter slechts één gratis-laag-abonnement maken. Modelimplementaties op de gratis laag die nog niet-gebruikte 7 dagen is uit bedrijf is genomen automatisch.

## <a name="next-steps"></a>Volgende stappen

Voer een van onze quickstarts van 10 minuten of Bekijk onze SDK-voorbeelden:

> [!div class="nextstepaction"]
> [Snelstartgids: Herkennen gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
> [spraak SDK-voorbeelden](speech-sdk.md#get-the-samples)

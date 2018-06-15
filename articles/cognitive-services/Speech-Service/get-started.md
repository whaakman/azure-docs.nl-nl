---
title: De service spraak gratis uitproberen | Microsoft Docs
description: Ontdek hoe u de service spraak gratis kunt uitproberen.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 3feef04694336d9173b419285a96fcaef543e18f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35349988"
---
# <a name="try-the-speech-service-for-free"></a>De service spraak gratis uitproberen

Aan de slag met de service spraak is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.

Als u meer tijd nodig hebt, zich aanmelden voor een Microsoft Azure-account, wordt geleverd met $200 servicevergoeding die u naar een betaald abonnement van de service spraak voor 30 dagen toepassen kunt.

Ten slotte biedt de spraak-service een gratis, kleinschalige laag geschikt is voor het ontwikkelen van toepassingen. Zelfs nadat uw tegoed service is verlopen, kunt u dit gratis abonnement behouden.

## <a name="free-trial"></a>Gratis proefversie

De gratis proefversie van 30 dagen hebt u toegang tot de standard-S0 prijscategorie voor een beperkte periode. Volg deze stappen om te registreren voor een gratis proefversie van 30 dagen.

1. Ga naar de [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/) pagina.

1. Overschakelen naar het tabblad spraak en klik op de **ophalen API-sleutel** knop naast 'spraakservices'.

   ![Tabblad voor spraak-Services](media/index/try-speech-api-free-trial1.png)<br>
   ![API-sleutel](media/index/try-speech-api-free-trial2.png)

3. Akkoord met de voorwaarden en selecteer uw land in de vervolgkeuzelijst.

   ![Voorwaarden accepteren](media/index/try-speech-api-free-trial3.png)

4. Meld u aan met uw Microsoft, Facebook, LinkedIn of GitHub-account. Of u kunt zich aanmelden voor een gratis Microsoft-account:

    * Ga naar de [Microsoft accountportal](https://account.microsoft.com/account).
    * Klik op **aanmelden met Microsoft**.

    ![Aanmelden](media/index/try-speech-api-free-trial4.png)

    * Wanneer u wordt gevraagd aan te melden, klikt u op 'Maken een'.

    ![Nieuw account maken](media/index/try-speech-api-free-trial5.png)

    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toe te wijzen en volg de instructies om te controleren of je nieuwe Microsoft-account.

Nadat u zich aanmeldt, wordt uw gratis proefabonnement begint. De weergegeven pagina geeft een lijst van alle cognitieve Services die u momenteel proefabonnementen hebt. Twee sleutels van het abonnement worden weergegeven naast 'spraakservices'. U kunt de sleutel in uw toepassingen.

> [!NOTE]
> Gratis proefabonnementen zijn in de regio VS-West. Zorg ervoor dat het eindpunt dat overeenkomt met uw regio bij aanvragen te gebruiken.

## <a name="new-azure-account"></a>Nieuwe Azure-account

Nieuwe Azure-accounts ontvangen een servicevergoeding $200, die maximaal 30 dagen duurt. Deze tegoed kan worden gebruikt om verder te verkennen van de service spraak of toepassingsontwikkeling begint.

Volg deze stappen om te registreren voor een nieuwe Azure-account.

1. Ga naar de [Azure aanmeldingspagina](https://azure.microsoft.com/free/ai/). 

1. Klik op **Start gratis**.

    ![Start gratis](media/index/try-speech-api-new-azure1.png)

3. Aanmelden met je Microsoft-account. Als u niet hebt:

    * Ga naar de [Microsoft accountportal](https://account.microsoft.com/account).
    * Klik op **aanmelden met Microsoft**.
    * Wanneer u wordt gevraagd aan te melden, klikt u op 'Maken een'.
    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toe te wijzen en volg de instructies om te controleren of je nieuwe Microsoft-account.

1. Voer de rest van de aangevraagde aanmelden voor een account informatie. Geef uw land en uw naam en geef een telefoonnummer en e-mailadres.

    ![Geef informatie op](media/index/try-speech-api-new-azure2.png)

    Uw identiteit verifiëren via de telefoon en door te geven van een creditcardnummer en de Azure gebruikersovereenkomst accepteren. (Je creditcard niet worden gefactureerd.)

    ![Overeenkomst te accepteren](media/index/try-speech-api-new-azure3.png)

Uw gratis Azure-account wordt gemaakt. Volg de stappen in de volgende sectie voor het starten van een abonnement op de service spraak.

## <a name="create-a-speech-resource-in-azure"></a>Een spraak-resource maken in Azure

Volg deze stappen om een resource van de service spraak toevoegen aan uw Azure-account.

1. Meld u aan bij de [Azure-portal](https://ms.portal.azure.com/) met uw Microsoft-account.

1. Klik op **maken van een resource** (de groene **+** pictogram) op linksboven in de portal.

    ![Bron maken](media/index/try-speech-api-create-speech1.png)

1. Zoek in het nieuwe venster spraak.

    ![Klik op spraak](media/index/try-speech-api-create-speech2.png)

1. Klik in de zoekresultaten op 'Spraak (preview).'

1. Klik op de **maken** knop onderaan in het deelvenster van de service spraak.

    ![Klik op maken](media/index/try-speech-api-create-speech3.png)

1. Voer in het deelvenster maken:

    * Een naam voor de nieuwe resource. De naam kunt u meerdere abonnementen op dezelfde service onderscheiden.
    * Kies het Azure-abonnement waaraan de nieuwe resource is gekoppeld om te bepalen hoe de kosten worden gefactureerd.
    * Kies de regio waar de resource worden gebruikt. De spraak-service is momenteel beschikbaar in Azië Oost, Noord-Europa en VS-West regio's.
    * Kies de prijscategorie beide F0 (maximaal gratis abonnement) of S0 (standard-abonnement). Klik op **volledige prijsinformatie bekijken** voor volledige informatie over prijzen en -gebruik quota's voor elke laag.
    * Maak een nieuwe resourcegroep voor dit abonnement spraak of toewijzen aan een bestaande. Resourcegroepen houdt u uw verschillende Azure-abonnementen ingedeeld.
    * Markeren voor gemakkelijke toegang tot uw abonnement in de toekomst de **vastmaken aan dashboard** selectievakje.
    * Klik op **maken.**

    ![Klik op maken in het Configuratiescherm](media/index/try-speech-api-create-speech4.png)

    Het duurt even maken en implementeren van uw nieuwe spraak-resource. Het paneel Quick Start weergegeven met informatie over uw nieuwe resource.

    ![Quick Start-Configuratiescherm](media/index/try-speech-api-create-speech5.png)

1. Klik op de **sleutels** koppeling onder stap 1 in het deelvenster Quick Start als de sleutels van uw abonnement wilt weergeven. Elk abonnement heeft twee sleutels; u mag gebruiken in uw toepassing. Klik op de knop naast elke sleutel om deze te kopiëren naar het Klembord plakken in uw code.

> [!NOTE]
> U kunt een willekeurig aantal standard-laag abonnementen maken in een of meerdere regio's. U kunt echter slechts één gratis laag abonnement maken.

## <a name="next-steps"></a>Volgende stappen

Download een SDK en sommige voorbeeldcode om de service spraak gebruik te maken.

> [!div class="nextstepaction"]
> [Spraak SDK 's](speech-sdk.md)

> [!div class="nextstepaction"]
> [Voorbeeldcode](samples.md)

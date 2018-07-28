---
title: Speech Service gratis uitproberen
description: Ontdek hoe kunt u de Speech-service gratis proberen.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: ff9a258d42f25ded82545909cdeade119548148e
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325204"
---
# <a name="try-the-speech-service-for-free"></a>Speech Service gratis uitproberen

Aan de slag met de Speech-service is eenvoudig en betaalbaar. Een gratis proefversie van 30 dagen kunt u ontdekken wat de service kan doen en bepalen of deze geschikt is voor de behoeften van uw toepassing.

Als u meer tijd nodig hebt, zich registreren voor een Microsoft Azure-account, wordt geleverd met $200 aan tegoed voor services die u op een betaald abonnement van de service spraak tot 30 dagen toepassen kunt.

Ten slotte biedt de Speech-service een gratis, klein volume laag die geschikt is voor het ontwikkelen van toepassingen. Zelfs nadat uw servicetegoed is verlopen, kunt u dit gratis abonnement behouden.

## <a name="free-trial"></a>Gratis proefversie

De gratis proefversie van 30 dagen hebt u toegang tot de S0 standard prijscategorie gedurende een beperkte periode. Volg deze stappen om u te registreren voor een gratis proefversie van 30 dagen.

1. Ga naar de [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/) pagina.

1. Ga naar het tabblad spraak en klikt u op de **ophalen API-sleutel** knop naast 'Speech services'.

   ![Tabblad voor spraak-Services](media/index/try-speech-api-free-trial1.png)<br>
   ![API-sleutel](media/index/try-speech-api-free-trial2.png)

3. Ga akkoord met de voorwaarden en selecteer uw land uit de vervolgkeuzelijst.

   ![Ga akkoord met voorwaarden](media/index/try-speech-api-free-trial3.png)

4. Meld u aan met uw Microsoft, Facebook, LinkedIn of GitHub-account. Of u kunt zich aanmelden voor een gratis Microsoft-account:

    * Ga naar de [portal voor Microsoft-account](https://account.microsoft.com/account).
    * Klik op **aanmelden bij Microsoft**.

    ![Aanmelden](media/index/try-speech-api-free-trial4.png)

    * Wanneer u wordt gevraagd aan te melden, klikt u op 'Maken een'.

    ![Nieuw account maken](media/index/try-speech-api-free-trial5.png)

    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toewijst en volg de instructies om uw nieuwe Microsoft-account te verifiëren.

Nadat u zich hebt aangemeld, begint de gratis proefversie. De weergegeven pagina geeft een lijst van alle Cognitive Services waarvoor u momenteel proefabonnementen hebt. Twee abonnementssleutels worden vermeld naast 'Speech services'. U kunt beide sleutels in uw toepassingen.

> [!NOTE]
> Alle gratis proefabonnementen komen in de regio VS-West. Zorg ervoor dat het eindpunt dat overeenkomt met uw regio bij aanvragen gebruiken.

## <a name="new-azure-account"></a>Nieuwe Azure-account

Nieuwe Azure-accounts, ontvangen een tegoed van $200-services die maximaal 30 dagen duurt. Dit tegoed kan worden gebruikt om verder te verkennen van de spraakservice of begin van toepassingsontwikkeling.

Volg deze stappen om u te registreren voor een nieuwe Azure-account.

1. Ga naar de [Azure aanmeldingspagina](https://azure.microsoft.com/free/ai/). 

1. Klik op **begin gratis**.

    ![Start gratis](media/index/try-speech-api-new-azure1.png)

3. Meld u aan met uw Microsoft-account. Als u geen een:

    * Ga naar de [portal voor Microsoft-account](https://account.microsoft.com/account).
    * Klik op **aanmelden bij Microsoft**.
    * Wanneer u wordt gevraagd aan te melden, klikt u op 'Maken een'.
    * In de stappen die volgen, Voer uw e-mailadres of telefoonnummer, een wachtwoord toewijst en volg de instructies om uw nieuwe Microsoft-account te verifiëren.

1. Voer de rest van de informatie die is aangevraagd om u te registreren voor een account. Geef uw land/regio en uw naam en een telefoonnummer en e-mailadres opgeven.

    ![Gegevens invoeren](media/index/try-speech-api-new-azure2.png)

    Uw identiteit verifiëren via de telefoon en door te geven van een creditcardnummer, en vervolgens de gebruiker van Azure-overeenkomst te accepteren. (Uw creditcard worden niet in rekening gebracht.)

    ![Overeenkomst te accepteren](media/index/try-speech-api-new-azure3.png)

Uw gratis Azure-account wordt gemaakt. Volg de stappen in de volgende sectie om te beginnen een abonnement op de spraak-service.

## <a name="create-a-speech-resource-in-azure"></a>Een spraak-resource maken in Azure

Als u wilt een spraak-service-resource toevoegen aan uw Azure-account, de volgende stappen uit.

1. Meld u aan bij de [Azure-portal](https://ms.portal.azure.com/) met uw Microsoft-account.

1. Klik op **een resource maken** (de groene **+** pictogram) op linksboven in de portal.

    ![Bron maken](media/index/try-speech-api-create-speech1.png)

1. Zoek in het nieuwe venster spraak.

    ![Klik op spraak](media/index/try-speech-api-create-speech2.png)

1. Klik in de lijst met zoekresultaten op 'Spraak (preview).'

1. Klik op de **maken** knop aan de onderkant van het paneel Speech-service.

    ![Klik op maken](media/index/try-speech-api-create-speech3.png)

1. Voer in het deelvenster maken:

    * Een naam voor de nieuwe resource. De naam van de kunt u onderscheid maken tussen meerdere abonnementen voor dezelfde service.
    * Kies het Azure-abonnement waaraan de nieuwe resource is gekoppeld om te bepalen hoe de kosten worden in rekening gebracht.
    * Kies de regio waarin de resource zal worden gebruikt. De spraak-service is momenteel beschikbaar in regio's Oost-Azië, Noord-Europa en VS-West.
    * Kies de prijscategorie, een van beide F0 (maximaal gratis abonnement) of S0 (standard-abonnement). Klik op **volledige prijsgegevens weergeven** voor meer informatie over prijzen en gebruik quota's voor elke laag.
    * Maak een nieuwe resourcegroep voor dit abonnement spraak of wijs deze toe aan een bestaande resourcegroep. Resourcegroepen houdt u uw verschillende Azure-abonnementen ingedeeld.
    * Voor een gemakkelijke manier toegang tot uw abonnement in de toekomst, markeert de **vastmaken aan dashboard** selectievakje.
    * Klik op **maken.**

    ![Klik op maken in het Configuratiescherm](media/index/try-speech-api-create-speech4.png)

    Het duurt even om te maken en implementeren van uw nieuwe spraak-resource. De Quick Start-deelvenster wordt weergegeven met informatie over de nieuwe resource.

    ![Snelstartgids-Configuratiescherm](media/index/try-speech-api-create-speech5.png)

1. Klik op de **sleutels** koppeling onder stap 1 in het deelvenster van de Snelstartgids om de abonnementssleutels van uw weer te geven. Elk abonnement heeft twee sleutels; u kunt gebruiken in uw toepassing. Klik op de knop naast elke sleutel te kopiëren naar het Klembord plakken in uw code.

> [!NOTE]
> U kunt een willekeurig aantal standard-laag-abonnementen maken in een of meerdere regio's. U kunt echter slechts één gratis-laag-abonnement maken.

## <a name="next-steps"></a>Volgende stappen

Voer een van onze quickstarts van 10 minuten of Bekijk onze SDK-voorbeelden.

> [!div class="nextstepaction"]
> [Snelstartgids: herkennen gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
> [spraak SDK-voorbeelden](speech-sdk.md#get-the-samples)

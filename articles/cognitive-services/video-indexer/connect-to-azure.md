---
title: Maak een indexeerfunctie Video-account met Azure bent verbonden | Microsoft Docs
description: In dit artikel laat zien hoe u een Video indexeerfunctie account verbonden met Azure maakt.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: ac9093d41a2e70905ea82c6d11f020696488ff27
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345902"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Maak een indexeerfunctie Video-account met Azure bent verbonden

Wanneer u een gratis proefaccount van Video indexeerfunctie, bent u beperkt door de quota en het nummer van video's die u kunt de index. In dit artikel laat zien hoe u een Video indexeerfunctie account die gekoppeld aan een Azure-abonnement die hoeft u deze limieten en die gebruikmaakt van de prijzen voor betalen naar gebruik maakt.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. 

    Als u een Azure-abonnement nog geen hebt, zich aanmelden voor [gratis proefversie van Azure](https://azure.microsoft.com/free/).

* Een Azure Active Directory (AD)-domein. 

    Als u geen Azure AD-domein, moet u dit domein maken met uw Azure-abonnement.

* Een gebruiker en het lid in uw Azure AD-domein. U gebruikt dit lid bij het verbinden van uw account Video indexeerfunctie met Azure.

    Deze gebruiker aan deze criteria voldoen:

    * Een Azure AD-gebruiker met een werk- of schoolaccount, niet een persoonlijke account, zoals outlook.com, live.com of hotmail.com worden.
        
        ![alle AAD-gebruikers](./media/create-account/all-aad-users.png)

    *  Een lid in uw Azure-abonnement met een rol van eigenaar of bijdrager en-beheerder voor gebruikerstoegang zijn.

        ![Toegangsbeheer](./media/create-account/access-control-iam.png)

## <a name="connect-to-azure"></a>Verbinding maken met Azure

1. Meld u aan met die gebruiker en klik op de **verbinding maken met Azure** knop:

    ![Verbinding maken met Azure](./media/create-account/connect-to-azure.png)

2. Wanneer de lijst met abonnementen wordt weergegeven, selecteert u het abonnement dat u wilt gebruiken. 

    ![Video indexeerfunctie verbinding te maken met Azure](./media/create-account/connect-vi-to-azure-subscription.png)

3. Een Azure-regio selecteert in de ondersteunde locaties: VS-West 2, Noord-Europa of Oost-Azië.
4. Onder **Azure Media Services-account**, kies een van de volgende opties:

    * U maakt een nieuw Media Services-account selecteren **nieuwe resourcegroep maken**. Geef een naam voor de resourcegroep.

        Azure maakt uw nieuwe account in uw abonnement, met inbegrip van een nieuw Azure Storage-account. Uw nieuwe Media Services-account heeft een standaard initiële configuratie met een Streaming-eindpunt en 10 S3 gereserveerde eenheden.
    * Als u een bestaande Media Services-account, schakelt **bestaande resource gebruiken**. Selecteer uw account uit de lijst van accounts.

        Media Services-account moet dezelfde regio bevinden als uw Video indexeerfunctie-account hebben. Om te beperken indexering duur en lage doorvoer, het type en aantal gereserveerde eenheden aanpassen **10 S3 gereserveerde eenheden** in uw Media Services-account.
    * Handmatig configureren van uw verbinding, klikt u op de **overschakelen naar handmatige configuratie** koppelen en leveren de nodige informatie:

    ![Video indexeerfunctie verbinding te maken met Azure](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. Als u bent klaar, kiest u **Connect**. Deze bewerking kan enkele minuten duren. 

    Nadat u met Azure verbonden bent, wordt uw nieuwe account voor de indexeerfunctie Video wordt weergegeven in de lijst met accounts:

    ![nieuwe account](./media/create-account/new-account.png)

6. Blader naar uw nieuwe account: 

    ![Video indexeerfunctie-account](./media/create-account/vi-account.png)

## <a name="considerations"></a>Overwegingen

De volgende gerelateerde Azure Media Services-overwegingen van toepassing:

* Als u met een nieuw Media Services-account verbonden, ziet u een nieuwe resourcegroep Media Services-account en een opslagaccount in uw Azure-abonnement.
* Als u met een nieuw Media Services-account verbonden, Video-indexeerfunctie het medium wordt ingesteld **gereserveerde eenheden** op 10 S3 eenheden:

    ![Gereserveerde Media Services-eenheden](./media/create-account/ams-reserved-units.png)

* Als u aan een bestaand Media Services-account verbonden, Video-indexeerfunctie verandert niets aan de bestaande mediaset **gereserveerde eenheden** configuratie.

    Mogelijk moet u het type en aantal media aanpassen **gereserveerde eenheden**, volgens uw geplande laden. Houd er rekening mee dat als de belasting hoge wordt en er geen voldoende eenheden of de snelheid, video's verwerken leiden mislukte time-out tot kunnen.

* Als u met een nieuw Media Services-account verbonden, Video-indexeerfunctie wordt automatisch gestart. een **Streaming-eindpunt** erin:

    ![Media Services-streaming-eindpunt](./media/create-account/ams-streaming-endpoint.png)

* Als u aan een bestaand Media Services-account verbonden, wordt de indexeerfunctie Video de streaming-eindpunten-configuratie niet gewijzigd. Als er geen uitgevoerd **Streaming-eindpunt**, u zich niet video van deze Media Services-account of in de indexeerfunctie Video.

## <a name="use-video-indexer-apis-v2"></a>Gebruik Video indexeerfunctie v2 API's

U programmatisch kunt werken met uw proefaccount en/of aan uw Video indexeerfunctie gebruikersaccounts die zijn verbonden met azure door de instructies in: [gebruik API's](video-indexer-use-apis.md).

U moet dezelfde Azure AD-gebruiker die u hebt gebruikt bij het verbinden met Azure.

## <a name="next-steps"></a>Volgende stappen

[Bekijk de details van de JSON-uitvoer](video-indexer-output-json-v2.md).


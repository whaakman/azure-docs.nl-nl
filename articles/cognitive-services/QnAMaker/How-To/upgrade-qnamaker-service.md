---
title: Upgrade uw service QnA Maker - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het bijwerken van uw QnA Maker-service
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e8d3713d6729c4e30da9a64a382e9d5a647dfefd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345295"
---
# <a name="upgrade-your-qna-maker-service"></a>Upgrade uw QnA Maker-service
U kunt afzonderlijke onderdelen van de stack QnA Maker upgrade na het aanvankelijk maken. Zie de details van de afhankelijke onderdelen en SKU selectie [hier](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Upgrade QnA Maker Management SKU
De QnA Maker management SKU upgraden:
1. Ga naar uw QnA Maker-resource in de Azure-portal en selecteer **prijscategorie**.

    ![QnA Maker-resource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Kies de juiste SKU en druk op **Selecteer**.

    ![QnA Maker prijzen](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Upgrade van App service
U kunt [opschalen](https://docs.microsoft.com/azure/app-service/web-sites-scale) of terugschroeven App service.

1. Ga naar de resource voor de App-service in de Azure portal en selecteer **opschalen** of **omlaag schalen** opties zoals vereist.

    ![Schaal van QnA Maker-app service](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Upgrade van Azure Search-service
Het is momenteel niet mogelijk om uit te voeren van een in-place upgrade van de Azure SKU zoeken. U kunt echter Maak een nieuwe Azure search-resource met de gewenste SKU, de gegevens herstellen naar de nieuwe resource en vervolgens te koppelen aan de stack QnA Maker.

1. Maak een nieuwe Azure search-resource in de Azure portal en kies de gewenste SKU.

    ![QnA Maker Azure search-resource](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. De indexen van de oorspronkelijke bron van de Azure search terugzetten naar de nieuwe. Zie de voorbeeldcode van de back-up terugzetten [hier](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Zodra de gegevens is hersteld, gaat u naar uw nieuwe Azure search-resource, worden Selecteer **sleutels**, en noteer de **naam** en de **administratorsleutel**.

    ![QnA Maker Azure search-sleutels](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Als u wilt de nieuwe Azure search-resource een koppeling naar de stack QnA Maker, gaat u naar de QnA Maker-App service.

    ![QnA Maker appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Selecteer **toepassingsinstellingen** en vervang de **AzureSearchName** en **AzureSearchAdminKey** velden uit stap 3.

    ![QnA Maker appservice-instelling](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. De App-service opnieuw starten.

    ![QnA Maker appservice opnieuw opstarten](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [QnA Maker API gebruiken](../Quickstarts/csharp.md)

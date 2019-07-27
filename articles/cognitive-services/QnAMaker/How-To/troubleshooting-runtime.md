---
title: Probleemoplossing - QnAMaker
titleSuffix: Azure Cognitive Services
description: QnAMaker bestaat uit onderdelen die worden gehost in Azure-account van de gebruiker. Foutopsporing kan vereisen dat gebruikers wijzigingen aanbrengen in de QnAMaker-Azure-resources of QnAMaker-ondersteuningsteam bieden met aanvullende informatie over de installatie.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 94a3dfd9a3560321d7e2753ccd385fb1a5323107
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559924"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Tips voor probleemoplossing voor de ondersteuning van de QnA Maker-service en runtime
QnAMaker bestaat uit onderdelen die worden gehost in Azure-account van de gebruiker. Foutopsporing kan vereisen dat gebruikers wijzigingen aanbrengen in de QnAMaker-Azure-resources of QnAMaker-ondersteuningsteam bieden met aanvullende informatie over de installatie.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Over het verkrijgen van de meest recente updates voor QnAMaker-runtime
QnAMaker runtime maakt deel uit van de Azure App Service geïmplementeerd wanneer u [maken van een service QnAMaker](./set-up-qnamaker-service-azure.md) in Azure portal. Updates worden periodiek gesteld in de runtime. QnA Maker App Service zich in de modus automatisch bijwerken bevindt, wordt de implementatie van de site-uitbrei ding van apr 2019 (versie 5 +) post. Dit is al zo ontworpen dat er tijdens upgrades geen uitval tijd in rekening wordt gehouden. U kunt uw huidige versie controleren op https://www.qnamaker.ai/UserSettings. Als uw versie ouder is dan versie 5. x, moet u de App Service opnieuw opstarten om de meest recente updates toe te passen op de installatie van uw QnAMaker.

1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker-Azure-resourcegroep](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op de Service-App en opent u de sectie overzicht

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Start de App-service. Het zou binnen een paar seconden voltooid. Houd er rekening mee dat uw downstream-toepassingen/bots voor deze QnAMaker-service bouwen is niet beschikbaar is voor eindgebruikers tijdens deze periode opnieuw opstarten.

    ![QnAMaker appservice-opnieuw opstarten](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Over het verkrijgen van de hostnaam van de service QnAMaker
Hostnaam van de QnAMaker-service is handig voor de foutopsporing als u contact op met ondersteuning voor QnAMaker of UserVoice. De hostnaam is een URL met deze indeling: https:// *{hostnaam}* . azurewebsites.net.
    
1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker-Azure-resourcegroep in Azure portal](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op de App Service

     ![Selecteer QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. De URL van de hostnaam is beschikbaar in de sectie overzicht

    ![QnAMaker-hostnaam](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Knowledge Base-vragen verbeteren met actief leren](./improve-knowledge-base.md)

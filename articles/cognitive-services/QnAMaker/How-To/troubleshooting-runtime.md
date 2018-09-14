---
title: Probleemoplossing - QnAMaker
titlesuffix: Azure Cognitive Services
description: QnAMaker bestaat uit onderdelen die worden gehost in Azure-account van de gebruiker. Foutopsporing kan vereisen dat gebruikers wijzigingen aanbrengen in de QnAMaker-Azure-resources of QnAMaker-ondersteuningsteam bieden met aanvullende informatie over de installatie.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 35ae2f5362b8432b94a05f3d86252d35f6f025ad
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543874"
---
# <a name="qnamaker-troubleshooting"></a>Het oplossen van QnAMaker
QnAMaker bestaat uit onderdelen die worden gehost in Azure-account van de gebruiker. Foutopsporing kan vereisen dat gebruikers wijzigingen aanbrengen in de QnAMaker-Azure-resources of QnAMaker-ondersteuningsteam bieden met aanvullende informatie over de installatie.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Over het verkrijgen van de meest recente updates voor QnAMaker-runtime
QnAMaker runtime maakt deel uit van de Azure App Service geïmplementeerd wanneer u [maken van een service QnAMaker](./set-up-qnamaker-service-azure.md) in Azure portal. Updates worden periodiek gesteld in de runtime. Als u wilt toepassen op de meest recente updates worden toegepast op de QnAMaker-configuratie, moet u de App-Service opnieuw.
1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker-Azure-resourcegroep](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op de Service-App en opent u de sectie overzicht

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Start de App-service. Het zou binnen een paar seconden voltooid. Houd er rekening mee dat uw downstream-toepassingen/bots voor deze QnAMaker-service bouwen is niet beschikbaar is voor eindgebruikers tijdens deze periode opnieuw opstarten.

    ![QnAMaker appservice-opnieuw opstarten](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Over het verkrijgen van de hostnaam van de service QnAMaker
Hostnaam van de QnAMaker-service is handig voor de foutopsporing als u contact op met ondersteuning voor QnAMaker of UserVoice. De hostnaam is een URL met deze indeling: https://*{hostnaam}*. azurewebsites.net.
    
1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker-Azure-resourcegroep](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op de App Service

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. De URL van de hostnaam is beschikbaar in de sectie overzicht

    ![QnAMaker-hostnaam](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [QnAMaker-API gebruiken](./upgrade-qnamaker-service.md)

---
title: Het oplossen van QnAMaker | Microsoft Docs
titleSuffix: Azure
description: Het oplossen van uw QnAMaker runtime
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 06/04/2018
ms.author: saneppal
ms.openlocfilehash: 23847c81a39ea392b6e64575406c9dc338b852de
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "35345950"
---
# <a name="qnamaker-troubleshooting"></a>QnAMaker probleemoplossing
QnAMaker omvat van onderdelen die worden gehost in Azure-account van de gebruiker. Foutopsporing, kan gebruikers hun QnAMaker Azure-resources te manipuleren, of geef ondersteuningsteam QnAMaker met aanvullende informatie over het instellen van vereisen.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Het ophalen van de meest recente updates voor QnAMaker runtime
QnAMaker runtime maakt deel uit van de Azure App Service geïmplementeerd wanneer u [maken van een service QnAMaker](./set-up-qnamaker-service-azure.md) in Azure-portal. Zijn er updates periodiek naar de runtime. Als u wilt toepassen op de meest recente updates worden toegepast op uw instellingen QnAMaker, moet u de App-Service opnieuw.
1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker Azure-resourcegroep](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op de App Service en open de sectie overzicht

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. De App-service opnieuw starten. Het moet binnen een paar seconden worden voltooid. Let op: uw downstream-toepassingen/bots voor deze QnAMaker-service bouwen is niet beschikbaar is voor eindgebruikers tijdens deze periode opnieuw opstarten.

    ![QnAMaker appservice opnieuw opstarten](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Het ophalen van de hostnaam van de service QnAMaker
QnAMaker service hostnaam is handig voor foutopsporing als u contact op met ondersteuning voor QnAMaker of UserVoice. De hostnaam is een URL van dit formulier: https://*{hostnaam}*. azurewebsites.net.
    
1. Ga naar uw QnAMaker-service (resourcegroep) in de [Azure-portal](https://portal.azure.com)

    ![QnAMaker Azure-resourcegroep](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Klik op App Service

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. De URL van de hostnaam is beschikbaar in de sectie overzicht

    ![QnAMaker hostnaam](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [QnAMaker API gebruiken](./upgrade-qnamaker-service.md)

---
title: Een Cordova-app maken in Azure App Service Mobile Apps| Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met back-ends voor mobiele apps van Azure voor Apache Cordova-ontwikkeling.
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
tags: 
keywords: cordova,javascript,mobiel,client
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 07/07/2017
ms.author: glenga
ms.openlocfilehash: b620465cdc3cfa04933dc6e70163fc32aa9a839b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-apache-cordova-app"></a>Een Apache Cordova-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Apache Cordova-app met een back-end voor mobiele apps van Azure.  U maakt zowel een nieuwe back-end voor een mobiele app als een eenvoudige Apache Cordova-app voor *takenlijsten* die app-gegevens opslaat in Azure.

Het volgen van deze zelfstudie is een vereiste voor alle andere Apache Cordova-zelfstudies over het gebruik van de functie Mobile Apps in Azure App Service.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Een pc met [Visual Studio Community 2017] of hoger.
* [Visual Studio Tools for Apache Cordova].
* Een [actief Azure-account](https://azure.microsoft.com/pricing/free-trial/).

U kunt Visual Studio ook omzeilen en de Apache Cordova-opdrachtregel rechtstreeks gebruiken.  Het gebruiken van de opdrachtregel is handig bij het voltooien van de zelfstudie op een Mac-computer.  Het compileren van Apache Cordova-clienttoepassingen via de opdrachtregel wordt niet behandeld in deze zelfstudie.

## <a name="create-an-azure-mobile-app-backend"></a>Een back-end voor mobiele apps van Azure maken
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Bekijk een video van vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Het serverproject configureren
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>De Apache Cordova-app downloaden en uitvoeren
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Volgende stappen
Nu u deze zelfstudie hebt voltooid, gaat u verder met een van de volgende zelfstudies:

* [Voeg offlinegegevens toe](app-service-mobile-cordova-get-started-offline-data.md) aan uw Apache Cordova-app.
* [Verificatie toevoegen](app-service-mobile-cordova-get-started-users.md) aan uw Apache Cordova-app.
* [Pushmeldingen toevoegen](app-service-mobile-cordova-get-started-push.md) aan uw Apache Cordova-app.

Meer informatie over belangrijke concepten met Azure App Service.

* [Offlinegegevens]
* [Verificatie]
* [Pushmeldingen]

Informatie over het gebruik van de SDK's.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Offlinegegevens]: app-service-mobile-offline-data-sync.md
[Verificatie]: app-service-mobile-auth.md
[Pushmeldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

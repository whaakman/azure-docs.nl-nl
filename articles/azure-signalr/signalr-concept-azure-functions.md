---
title: Ontwikkel Real-time apps met Azure Functions en Azure SignalR Service
description: Een overzicht van het gebruik van Azure SignalR Service in serverloze toepassingen.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: c88e2b012086f61aad3143c8fe6b0bd25ec333e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275093"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Ontwikkel real-time Apps met Azure Functions en Azure SignalR Service

Omdat de Azure SignalR-service en Azure Functions beide volledig beheerde, zeer schaalbare services zijn waarmee u zich kunt richten op het ontwikkelen van toepassingen in plaats van op het beheren van de infrastructuur, worden de beide services vaak samen gebruikt om realtime communicatie in een [serverloze](https://azure.microsoft.com/solutions/serverless/) omgeving te bieden.

> [!NOTE]
> Leer hoe u SignalR en Azure Functions samen gebruiken in de interactieve zelfstudie [inschakelen van automatische updates in een webtoepassing met Azure Functions en SignalR-Service](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr).

## <a name="integrate-real-time-communications-with-azure-services"></a>Realtimecommunicatie integreren met Azure-services

Met Azure Functions kunt u code in [verschillende talen](../azure-functions/supported-languages.md), waaronder JavaScript, C# en Java, schrijven die wordt geactiveerd als er een gebeurtenis in de cloud plaatsvindt. Voorbeelden van deze gebeurtenissen zijn:

* HTTP- en webhook-aanvragen
* Periodieke timers
* Gebeurtenissen van Azure-services, zoals:
    - Event Grid
    - Event Hubs
    - Service Bus
    - Cosmos DB-wijzigingenfeed
    - Storage - blobs en wachtrijen
    - Logic Apps-connectoren, zoals Salesforce en SQL Server

Als u Azure Functions gebruikt om deze evenementen met Azure SignalR-service te integreren, hebt u de mogelijkheid om duizenden clients te informeren als zich gebeurtenissen voordoen.

Sommige veelvoorkomende scenario’s voor serverloze berichten in realtime die u met Azure Functions en SignalR-service kunt implementeren, zijn:

* IoT-apparaattelemetrie visualiseren op een realtimedashboard of -kaart
* Gegevens bijwerken in een toepassing als documenten in Cosmos DB worden bijgewerkt
* Meldingen in apps verzenden als er nieuwe bestellingen in Salesforce worden gemaakt

## <a name="signalr-service-bindings-for-azure-functions"></a>SignalR-servicebindingen voor Azure Functions

Met de SignalR-servicebindingen voor Azure Functions kan een Azure Function-app worden toegestaan om berichten te publiceren voor clients die met de SignalR-service zijn verbonden. Clients kunnen verbinding met de service maken via een SDK voor SignalR-clients die beschikbaar is in .NET, JavaScript en Jave; er worden binnenkort meer talen toegevoegd.

### <a name="an-example-scenario"></a>Een voorbeeldscenario

Een voorbeeld van hoe u de SignalR-servicebindingen kunt gebruiken, maakt gebruik van Azure Functions om te integreren met Azure Cosmos DB en SignalR-service om realtimeberichten te verzenden als er gebeurtenissen voorkomen in een Cosmos DB-wijzigingenfeed.

![Cosmos DB, Azure Functions, SignalR-service](media/signalr-concept-azure-functions/signalr-cosmosdb-functions.png)

1. Er wordt een wijziging doorgevoerd in een Cosmos DB-verzameling
2. De wijzigingsgebeurtenis wordt doorgegeven aan de Cosmos DB-wijzigingenfeed
3. Er wordt een Azure Functions door de wijzigingenfeed geactiveerd met behulp van de Cosmos DB-trigger
4. De uitvoerbinding van de SignalIR-service publiceert een bericht naar SignalR-service
5. SignalR-service publiceert het bericht aan alle verbonden clients

### <a name="authentication-and-users"></a>Verificatie en gebruikers

Met SignalR-service kunt u berichten uitzenden naar alle clients of alleen naar een subset van clients, zoals clients die tot één gebruiker behoren. De SignalR-servicebindingen voor Azure Functions kunnen worden gecombineerd met App Service-verificatie om gebruikers te verifiëren met behulp van providers, zoals Azure Active Directory, Facebook en Twitter. U kunt vervolgens rechtstreeks berichten naar deze geverifieerde gebruikers verzenden.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een overzicht gekregen van hoe u Azure Functions kunt gebruiken met SignalIR-service om een breed scala aan scenario’s voor serverloze realtimeberichten mogelijk te maken.

Ga naar de volgende bronnen voor meer informatie over het gebruik van Azure Functions en SignalR Service samen:

* [Azure Functions-ontwikkeling en -configuratie met SignalR-Service](signalr-concept-serverless-development-config.md)
* [Automatische updates in een webtoepassing met Azure Functions en SignalR-Service inschakelen](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr)

Volg een van deze snelstartgidsen voor meer informatie.

* [Snelstart voor serverloze Azure SignalR-service - C#](signalr-quickstart-azure-functions-csharp.md)
* [Snelstart voor serverloze Azure SignalR-service - JavaScript](signalr-quickstart-azure-functions-javascript.md)
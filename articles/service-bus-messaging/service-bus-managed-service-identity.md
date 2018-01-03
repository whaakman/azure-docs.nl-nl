---
title: Service-identiteit beheerd met Azure Service Bus-preview | Microsoft Docs
description: Gebruik van beheerde Service-identiteiten met Azure Servicebus
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 6965e80cf10b732d4d0a8fb78447f188c133979d
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="managed-service-identity-preview"></a>Beheerde Service-identiteit (preview)

Een beheerde Service identiteit (MSI) is een cross-Azure-functie waarmee u een beveiligde identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. U kunt vervolgens die identiteit koppelen aan de access control-functies die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing moet verlenen.

Met MSI beheert het Azure-platform deze runtime-identiteit. U hoeft niet op te slaan en toegangstoetsen in uw toepassingscode of de configuratie voor de identiteit zelf of voor de bronnen die u wilt beveiligen. Een Service Bus-client-app uitgevoerd binnen een Azure App Service-toepassing of in een virtuele machine met ingeschakelde MSI-ondersteuning hoeft niet te handelen SAS-regels en sleutels of andere toegangstokens. De client-app moet alleen een adres van het eindpunt van de Service Bus-berichtenservice-naamruimte. Wanneer de app verbinding maakt, wordt de MSI-context in Service Bus gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel. 

Zodra deze gekoppeld aan een beheerde service-identiteit is, kan alle gemachtigde bewerkingen uitvoeren in een Service Bus-clienttoepassing. Autorisatie wordt verleend door een MSI-bestand koppelen aan Service Bus-functies. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus-rollen en machtigingen

Voor de eerste openbare preview-versie, kunt u een beheerde service-identiteit alleen toevoegen aan de rollen 'Eigenaar' of 'Inzender' van een Service Bus-naamruimte die de volledige controle van de identiteit op alle entiteiten in de naamruimte toekent. Beheer van bewerkingen die de naamruimte-topologie wijzigen in eerste instantie zijn ondersteund echter alleen via Azure Resource Manager en niet via de systeemeigen Service Bus REST-interface. Deze ondersteuning betekent ook dat u de .NET Framework-client kan geen [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object binnen een beheerde service-identiteit.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Servicebus gebruiken met een beheerde Service-identiteit

De volgende sectie wordt beschreven hoe u maken en implementeren van een voorbeeldtoepassing die compatibel is met een beheerde service-identiteit, hoe die identiteit toegang verlenen tot een Service Bus-berichtenservice-naamruimte en de interactie van de toepassing met de Service Bus entiteiten met die identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost in [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u dit doet in Azure bent, voert u de [in deze handleiding instructies](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van een MVC-toepassing, zoals wordt weergegeven in de zelfstudie maakt, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-service-identity"></a>De identiteit van de beheerde service instellen

Als u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in de Azure portal (wordt ook weergegeven in de procedures voor) en vervolgens gaat u naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![](./media/service-bus-managed-service-identity/msi1.png)

Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory hebt gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Een nieuwe Service Bus-berichtenservice-naamruimte maken

Vervolgens [maken van een Service Bus-berichtenservice naamruimte](service-bus-create-namespace-portal.md) in een van de Azure-regio's die preview ondersteuning bieden voor RBAC: **VS-Oost**, **ons Oost 2**, of **West-Europa** . 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** om toe te voegen aan de identiteit van de beheerde service de **eigenaar** rol. Om dit te doen, zoekt u de naam van de webtoepassing in de **machtigingen toevoegen** Configuratiescherm **Selecteer** veld en klik op de vermelding. Klik vervolgens op **Opslaan**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
De webtoepassing beheerde service-identiteit heeft nu toegang tot de Service Bus-naamruimte en naar de wachtrij die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Nu wijzigen de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt ook de code van de web application [deze GitHub-opslagplaats](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ManagedServiceIdentity). 

De pagina Default.aspx is uw startpagina. De code kan worden gevonden in het bestand Default.aspx.cs. Het resultaat is een minimale webtoepassing met enkele invoervelden en met **verzenden** en **ontvangen** knoppen die verbinding met Service Bus maken te verzenden of ontvangen van berichten.

Opmerking hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt een tokenprovider voor de identiteit van de beheerde service met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` aanroepen. Er zijn als zodanig geen geheimen te behouden en te gebruiken. De stroom van de context van de identiteit beheerde service naar Service Bus en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider is een eenvoudiger dan het gebruik van SAS-model.

Zodra u deze wijzigingen hebt aangebracht, publiceren en voer de toepassing. Er is een eenvoudige manier om op te halen van de juiste gegevens te publiceren om te downloaden en vervolgens importeren een publicatieprofiel in Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt, en klik vervolgens op een **verzenden** of **ontvangen**. 
 
Houd er rekening mee dat de identiteit van de beheerde service alleen werkt in de Azure-omgeving, en alleen in de App Service-implementatie waarin u het hebt geconfigureerd. Houd er ook rekening mee dat beheerde service-identiteiten niet met App Service-implementatiesites op dit moment werken.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
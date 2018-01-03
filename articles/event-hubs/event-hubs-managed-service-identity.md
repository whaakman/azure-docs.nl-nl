---
title: Service-identiteit beheerd met Azure Event Hubs preview | Microsoft Docs
description: Beheerde Service-id's gebruiken met Azure Event Hubs
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2017
ms.author: sethm
ms.openlocfilehash: dd50e4f6ebc5fdf5496a5127fde20bd052087b59
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="managed-service-identity-preview"></a>Beheerde Service-identiteit (preview)

Een beheerde Service identiteit (MSI) is een cross-Azure-functie waarmee u een beveiligde identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. U kunt vervolgens die identiteit koppelen aan de access control-functies die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing moet verlenen. 

Met MSI beheert het Azure-platform deze runtime-identiteit. U hoeft niet op te slaan en toegangstoetsen in uw toepassingscode of de configuratie voor de identiteit zelf of voor de bronnen die u wilt beveiligen. Een Event Hubs-client-app uitgevoerd binnen een Azure App Service-toepassing of in een virtuele machine met ingeschakelde MSI-ondersteuning hoeft niet te handelen SAS-regels en sleutels of andere toegangstokens. De client-app moet alleen een adres van het eindpunt van de Event Hubs-naamruimte. Wanneer de app verbinding maakt, wordt de MSI-context in Event Hubs gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel.

Zodra deze gekoppeld aan een beheerde service-identiteit is, kan alle gemachtigde bewerkingen uitvoeren in een Event Hubs-client. Autorisatie wordt verleend door het koppelen van een MSI-bestand met de rol van Event Hubs. 

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen

Voor de eerste openbare preview-versie, kunt u een beheerde service-identiteit alleen toevoegen aan de rollen 'Eigenaar' of 'Inzender' van een naamruimte Event Hubs, die de volledige controle van de identiteit op alle entiteiten in de naamruimte toekent. Beheer van bewerkingen die de naamruimte-topologie wijzigen in eerste instantie zijn ondersteund echter alleen via Azure Resource Manager en niet via de systeemeigen Event Hubs REST-interface. Deze ondersteuning betekent ook dat u de .NET Framework-client kan geen [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object binnen een beheerde service-identiteit. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Event Hubs met een beheerde Service-identiteit gebruiken

De volgende sectie wordt beschreven hoe u maken en implementeren van een voorbeeldtoepassing die compatibel is met een beheerde service-identiteit, hoe die identiteit toegang verlenen tot een Event Hubs-naamruimte en de interactie van de toepassing met event hubs met behulp van die de identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost in [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u dit doet in Azure bent, voert u de [in deze handleiding instructies](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van een MVC-toepassing, zoals wordt weergegeven in de zelfstudie maakt, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-service-identity"></a>De identiteit van de beheerde service instellen

Als u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in de Azure portal (wordt ook weergegeven in de procedures voor) en vervolgens gaat u naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory hebt gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-event-hubs-namespace"></a>Een nieuwe Event Hubs-naamruimte maken

Vervolgens [maken van een naamruimte Event Hubs](event-hubs-create.md) in een van de Azure-regio's die ondersteuning bieden voor preview voor MSI: **VS-Oost**, **ons Oost 2**, of **West-Europa**. 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** om toe te voegen aan de identiteit van de beheerde service de **eigenaar** rol. Om dit te doen, zoekt u de naam van de webtoepassing in de **machtigingen toevoegen** Configuratiescherm **Selecteer** veld en klik op de vermelding. Klik vervolgens op **Opslaan**.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
De webtoepassing beheerde service-identiteit heeft nu toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Nu wijzigen de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt ook de code van de web application [deze GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Nadat u uw app te starten, wijst u EventHubsMSIDemo.aspx van uw browser. U kunt ook deze instellen als uw startpagina. De code kan worden gevonden in het bestand EventHubsMSIDemo.aspx.cs. Het resultaat is een minimale webtoepassing met enkele invoervelden en met **verzenden** en **ontvangen** knoppen die verbinding met Event Hubs maken te verzenden of ontvangen van berichten. 

Opmerking hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt een tokenprovider voor de identiteit van de beheerde service met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` aanroepen. Er zijn als zodanig geen geheimen te behouden en te gebruiken. De stroom van de context van de identiteit beheerde service met Event Hubs en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider is een eenvoudiger dan het gebruik van SAS-model.

Zodra u deze wijzigingen hebt aangebracht, publiceren en voer de toepassing. Er is een eenvoudige manier om op te halen van de juiste gegevens te publiceren om te downloaden en vervolgens importeren een publicatieprofiel in Visual Studio:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt, en klik vervolgens op een **verzenden** of **ontvangen**. 
 
Houd er rekening mee dat de identiteit van de beheerde service alleen werkt in de Azure-omgeving, en alleen in de App Service-implementatie waarin u het hebt geconfigureerd. Houd er ook rekening mee dat beheerde service-identiteiten niet met App Service-implementatiesites op dit moment werken.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Event Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
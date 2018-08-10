---
title: Beheerde Service-identiteit met Azure Event Hubs-preview | Microsoft Docs
description: Beheerde Service-identiteiten gebruiken met Azure Eventhubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: shvija
ms.openlocfilehash: f87a04b3b78bab124ca1b75006ed2c93a74f9198
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005830"
---
# <a name="managed-service-identity-preview"></a>Beheerde service-identiteit (preview)

Een Managed Service Identity (MSI) is een cross-Azure-functie waarmee u te maken van een veilige identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. Daarna kunt u die identiteit koppelen met access-control-rollen die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing nodig heeft. 

Met MSI-bestand beheert het Azure-platform deze runtime-identiteit. U hoeft niet voor het opslaan en toegang tot sleutels in uw toepassingscode of de configuratie voor de identiteit zelf, of voor de resources die u nodig hebt om toegang te beveiligen. Een Event Hubs-client-app die wordt uitgevoerd binnen een Azure App Service-toepassing of in een virtuele machine met ingeschakelde MSI-ondersteuning hoeft niet voor het afhandelen van SAS-regels en sleutels of andere toegangstokens. De client-app hoeft alleen het adres van het eindpunt van de Event Hubs-naamruimte. Wanneer de app verbinding maakt, wordt de MSI-context in Event Hubs gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel.

Zodra deze gekoppeld aan een beheerde service-identiteit is, kan alle geautoriseerde bewerkingen uitvoeren in een Event Hubs-client. Autorisatie is verleend door een MSI-bestand met behulp van Event Hubs koppelen. 

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen

Voor de eerste openbare preview-versie kunt u alleen een beheerde service-identiteit toevoegen aan de rollen 'Eigenaar' of 'Medewerker' van een Event Hubs-naamruimte, waarmee de identiteit volledig beheer voor alle entiteiten in de naamruimte worden verleend. Beheer van bewerkingen die wijzigen van de naamruimte-topologie in eerste instantie zijn ondersteund echter alleen via Azure Resource Manager en niet via de systeemeigen Event Hubs REST-beheerinterface. Deze ondersteuning betekent ook dat u de .NET Framework-client niet gebruiken [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object in een beheerde service-identiteit. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Eventhubs gebruiken met een beheerde Service-identiteit

De volgende sectie wordt beschreven hoe u maken en implementeren van een voorbeeldtoepassing die wordt uitgevoerd onder een beheerde service-identiteit, hoe u die identiteit toegang verlenen tot een Event Hubs-naamruimte en hoe de toepassing communiceert met eventhubs met behulp van die de identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u dit doet in Azure bent, voert u de [in deze gebruiksaanwijzing](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van het maken van een MVC-toepassing zoals wordt weergegeven in de zelfstudie, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-service-identity"></a>Instellen van de beheerde service-identiteit

Zodra u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in Azure portal (wordt ook weergegeven in de instructies) en navigeer naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-event-hubs-namespace"></a>Een nieuwe Event Hubs-naamruimte maken

Volgende [maken van een Event Hubs-naamruimte](event-hubs-create.md) in een van de Azure-regio's met Preview-versie-ondersteuning voor MSI: **VS Oost**, **VS Oost 2**, of **West-Europa**. 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** om toe te voegen van de beheerde service-identiteit voor de **eigenaar** rol. Om dit te doen, zoekt u de naam van de web-App in de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
De beheerde service-identiteit voor de webtoepassing nu heeft toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Wijzig nu de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt ook de web-toepassingscode van [deze GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

>[!NOTE] 
> Wanneer de MSI-functie in preview is, zorg ervoor dat u de [preview-versie van de Service Bus-bibliotheek](https://www.nuget.org/packages/WindowsAzure.ServiceBus/4.2.2-preview) voor toegang tot de nieuwe API's. 

Als u de app gestart, wijst u EventHubsMSIDemo.aspx van uw browser. U kunt ook deze instellen als uw startpagina. De code kan worden gevonden in het bestand EventHubsMSIDemo.aspx.cs. Het resultaat is een minimale webtoepassing met een paar velden, en met **verzenden** en **ontvangen** knoppen die verbinding maken met Event Hubs verzenden of ontvangen van gebeurtenissen. 

Houd er rekening mee hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt u een token provider voor de identiteit van de beheerde service met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` aanroepen. Er zijn daarom geen geheimen behouden en te gebruiken. De stroom van de context van de beheerde service-identiteit met Event Hubs en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider, dit is een eenvoudiger dan het gebruik van SAS-model.

Zodra u deze wijzigingen hebt aangebracht, publiceren en de toepassing wordt uitgevoerd. Een eenvoudige manier om op te halen van de juiste publiceren van gegevens is om te downloaden en vervolgens importeren een publicatieprofiel in Visual Studio:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt, en klik vervolgens op een **verzenden** of **ontvangen**. 
 
Houd er rekening mee dat de beheerde service-identiteit alleen werkt in de Azure-omgeving, en alleen in de App Service-implementatie waarin u deze hebt geconfigureerd. Houd er ook rekening mee dat beheerde service-identiteiten niet met implementatiesites voor App Service op dit moment werken.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Details van prijzen van Eventhubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
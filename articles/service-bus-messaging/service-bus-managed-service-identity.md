---
title: Beheerde Service-identiteit met Azure Service Bus-preview | Microsoft Docs
description: Beheerde Service-identiteiten gebruiken met Azure Servicebus
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/01/2018
ms.author: spelluru
ms.openlocfilehash: c8722aeb9e957eb77dfc3dd975587717f91d5d1f
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395624"
---
# <a name="managed-service-identity-preview"></a>Beheerde service-identiteit (preview)

Een Managed Service Identity (MSI) is een cross-Azure-functie waarmee u te maken van een veilige identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. Daarna kunt u die identiteit koppelen met access-control-rollen die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing nodig heeft.

Met MSI-bestand beheert het Azure-platform deze runtime-identiteit. U hoeft niet voor het opslaan en toegang tot sleutels in uw toepassingscode of de configuratie voor de identiteit zelf, of voor de resources die u nodig hebt om toegang te beveiligen. Een Service Bus-client-app die wordt uitgevoerd binnen een Azure App Service-toepassing of in een virtuele machine met ingeschakelde MSI-ondersteuning hoeft niet voor het afhandelen van SAS-regels en sleutels of andere toegangstokens. De client-app hoeft alleen het adres van het eindpunt van de Service Bus Messaging-naamruimte. Wanneer de app verbinding maakt, wordt de MSI-context in Service Bus gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel. 

Zodra deze gekoppeld aan een beheerde service-identiteit is, kan alle geautoriseerde bewerkingen uitvoeren in een Service Bus-clienttoepassing. Autorisatie is verleend door te koppelen van een MSI-bestand met behulp van Service Bus. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus-rollen en machtigingen

Voor de eerste openbare preview-versie kunt u alleen een beheerde service-identiteit toevoegen aan de rollen 'Eigenaar' of 'Medewerker' van een Service Bus-naamruimte, waarmee de identiteit volledig beheer voor alle entiteiten in de naamruimte worden verleend. Beheer van bewerkingen die wijzigen van de naamruimte-topologie in eerste instantie zijn ondersteund echter alleen via Azure Resource Manager en niet via de systeemeigen Service Bus REST-beheerinterface. Deze ondersteuning betekent ook dat u de .NET Framework-client niet gebruiken [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object in een beheerde service-identiteit.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Servicebus gebruiken met een beheerde Service-identiteit

De volgende sectie wordt beschreven hoe u maken en implementeren van een voorbeeldtoepassing die wordt uitgevoerd onder een beheerde service-identiteit, hoe u die identiteit toegang verlenen tot een Service Bus Messaging-naamruimte en hoe de toepassing communiceert met Service Bus entiteiten met die identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u dit doet in Azure bent, voert u de [in deze gebruiksaanwijzing](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van het maken van een MVC-toepassing zoals wordt weergegeven in de zelfstudie, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-service-identity"></a>Instellen van de beheerde service-identiteit

Zodra u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in Azure portal (wordt ook weergegeven in de instructies) en navigeer naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![](./media/service-bus-managed-service-identity/msi1.png)

Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Een nieuwe Service Bus Messaging-naamruimte maken

Volgende [maken van een Service Bus Messaging-naamruimte](service-bus-create-namespace-portal.md) in een van de Azure-regio's die ondersteuning bieden voor Preview-versie voor RBAC: **VS Oost**, **VS Oost 2**, of **West-Europa** . 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** om toe te voegen van de beheerde service-identiteit voor de **eigenaar** rol. Om dit te doen, zoekt u de naam van de web-App in de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Beheerde service-identiteit voor de web-App heeft nu toegang tot de Service Bus-naamruimte en naar de wachtrij die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Wijzig nu de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt de code van de web-endtoepassing van [deze GitHub-opslagplaats](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

De pagina Default.aspx is de landingspagina. De code kan worden gevonden in het bestand Default.aspx.cs. Het resultaat is een minimale webtoepassing met een paar velden, en met **verzenden** en **ontvangen** knoppen die verbinding met Service Bus maken te verzenden of ontvangen van berichten.

Houd er rekening mee hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt u een token provider voor de identiteit van de beheerde service met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` aanroepen. Er zijn daarom geen geheimen behouden en te gebruiken. De stroom van de context van de beheerde service-identiteit voor Service Bus en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider, dit is een eenvoudiger dan het gebruik van SAS-model.

Zodra u deze wijzigingen hebt aangebracht, publiceren en de toepassing wordt uitgevoerd. Een eenvoudige manier om op te halen van de juiste publiceren van gegevens is om te downloaden en vervolgens importeren een publicatieprofiel in Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt, en klik vervolgens op een **verzenden** of **ontvangen**.


> [!NOTE]
> - De beheerde service-identiteit werkt alleen in de Azure-omgeving van App-services, Azure-VM's, en schaal worden ingesteld. Voor .NET-toepassingen, de Microsoft.Azure.Services.AppAuthentication library, die wordt gebruikt door de Service Bus NuGet-pakket, biedt een abstractie via dit protocol en biedt ondersteuning voor een lokale ontwikkeling-ervaring. Deze bibliotheek kunt u uw code lokaal op uw ontwikkelcomputer, met behulp van uw gebruikersaccount vanuit Visual Studio, Azure CLI 2.0 of Active Directory Integrated Authentication testen. Zie voor meer informatie over opties voor lokale ontwikkeling met deze bibliotheek [Service-naar-serviceverificatie naar Azure Key Vault met behulp van .NET](../key-vault/service-to-service-authentication.md).  
> 
> - Beheerde service-identiteiten werken op dit moment niet met implementatiesites voor App Service.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
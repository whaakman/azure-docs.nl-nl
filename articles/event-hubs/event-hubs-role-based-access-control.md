---
title: Voorbeeld van Azure Event Hubs Role-Based toegangsbeheer (RBAC) | Microsoft Docs
description: Azure Event Hubs Role-Based Access Control
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: shvija
ms.openlocfilehash: ef74600fdf5051394f8b7bfbdd71e144b3f26d8a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005735"
---
# <a name="active-directory-role-based-access-control-preview"></a>Actieve Directory Role-Based Access Control (preview)

Microsoft Azure biedt geïntegreerde toegangsbeheer voor bronnen en toepassingen op basis van Azure Active Directory (Azure AD). Met Azure AD, kunt u gebruikersaccounts en toepassingen kunt beheren specifiek voor uw Azure-toepassingen of kunt u uw bestaande Active Directory-infrastructuur met Azure AD voor het hele bedrijf single-sign-on die ook Azure-resources omvat federeren en toepassingen wordt gehost op Azure. Vervolgens kunt u deze Azure AD-identiteiten voor gebruikers en toepassingen toewijzen aan globale en servicespecifieke rollen om toegang verlenen tot Azure-resources.

Voor Azure Event Hubs, het beheer van naamruimten en alle gerelateerde resources via Azure portal en de Azure Resourcemanagement-API is al beveiligd met behulp van de *op rollen gebaseerd toegangsbeheer* (RBAC)-model. RBAC voor bewerkingen van de runtime is een functie nu in openbare preview. 

Een toepassing die gebruikmaakt van Azure AD RBAC hoeft niet voor het afhandelen van SAS-regels en sleutels of een andere specifieke naar Event Hubs toegangstokens. De client-app communiceert met Azure AD om geen verificatiecontext stand te brengen en een toegangstoken opgehaald voor Event Hubs. Met de domeingebruikersaccounts waarvoor interactieve aanmelding, de toepassing verwerkt nooit geen referenties rechtstreeks.

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen

Voor de eerste openbare preview, kunt u Azure AD-accounts en service-principals alleen toevoegen aan de rollen 'Eigenaar' of 'Medewerker' van een Event Hubs-naamruimte. Met deze bewerking verleent de identiteit volledige controle over alle entiteiten in de naamruimte. -Beheerbewerkingen waarbij wijzigen van de topologie van de naamruimte worden in eerste instantie alleen ondersteund via Azure resource Manager en niet via de systeemeigen Event Hubs REST-beheerinterface. Deze ondersteuning betekent dat ook de .NET Framework client [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object kan niet worden gebruikt met een Azure AD-account.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Event Hubs gebruiken met een gebruikersaccount voor Azure AD-domein

De volgende sectie wordt beschreven hoe u maken en uitvoeren van een voorbeeldtoepassing die om een interactieve Azure vraagt AD-gebruiker aanmelden, het verlenen van toegang aan de gebruikersaccount Event Hubs en die identiteit gebruiken voor toegang tot de Event Hubs. 

Deze inleiding beschrijft een eenvoudige consoletoepassing de [code waarvoor is op Github](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>Een Active Directory-gebruikersaccount maken

Deze eerste stap is optioneel. Elk Azure-abonnement wordt automatisch gekoppeld aan een Azure Active Directory-tenant en hebt u toegang tot een Azure-abonnement, wordt uw account is al geregistreerd. Dit betekent dat u zojuist hebt uw account kunt gebruiken. 

Als u nog steeds wilt maken van een speciaal account voor dit scenario [als volgt te werk](../automation/automation-create-aduser-account.md). U moet gemachtigd voor het maken van accounts in de Azure Active Directory-tenant, wat mogelijk niet het geval is bij grotere enterprise-scenario's.

### <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Volgende [maken van een Event Hubs-naamruimte](event-hubs-create.md) in een van de Azure-regio's die Event Hubs preview ondersteuning bieden voor RBAC: **VS Oost**, **VS Oost 2**, of **West-Europa **. 

Nadat de naamruimte is gemaakt, gaat u naar de **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** de Azure AD-gebruikersaccount toevoegen aan de rol van eigenaar. Als u uw eigen gebruikersaccount gebruikt en u de naamruimte hebt gemaakt, bent u al in de rol van eigenaar. Zoek de naam van de web-App in een ander account toevoegen aan de rol, de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**.
 
![](./media/event-hubs-role-based-access-control/rbac1.PNG)

Het gebruikersaccount heeft nu toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt.
 
### <a name="register-the-application"></a>De toepassing registreren

Voordat u de voorbeeldtoepassing uitvoeren kunt, deze te registreren in Azure AD en de toestemmingsprompt waarmee de aanvraag voor toegang tot Event Hubs op de andere gebruikers goedkeuren. 

Omdat de voorbeeldtoepassing een toepassing is, moet u een native toepassing registreren en toevoegen van de API-machtigingen voor **Microsoft.EventHub** aan de set 'vereist machtigingen'. Systeemeigen toepassingen moeten ook een **omleidings-URI** in Azure AD die als een-id. fungeert de URI niet hoeft te worden van een netwerk-doel. Gebruik `http://eventhubs.microsoft.com` voor dit voorbeeld omdat het voorbeeld code al gebruikmaakt van deze URI.

De registratie van gedetailleerde stapsgewijze [in deze zelfstudie](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Volg de stappen voor het registreren van een **systeemeigen** app, en volg de instructies update toe te voegen de **Microsoft.EventHub** API aan de vereiste machtigingen. Als u de stappen hebt uitgevoerd, noteer de **TenantId** en de **ApplicationId**, moet u deze waarden in de toepassing uit te voeren.

### <a name="run-the-app"></a>De app uitvoeren

Voordat u het voorbeeld uitvoeren kunt, bewerkt u het bestand App.config en, afhankelijk van uw scenario, stel de volgende waarden:

- `tenantId`: Ingesteld op **TenantId** waarde.
- `clientId`: Ingesteld op **ApplicationId** waarde. 
- `clientSecret`: Als u wilt zich aanmelden met het clientgeheim, kunt u deze in Azure AD maken. Een web-app of API ook gebruiken in plaats van een systeemeigen app. Voeg ook de app onder **Access Control (IAM)** in de naamruimte die u eerder hebt gemaakt.
- `eventHubNamespaceFQDN`: Ingesteld op de volledig gekwalificeerde DNS-naam van uw zojuist gemaakte Event Hubs-naamruimte. bijvoorbeeld, `example.servicebus.windows.net`.
- `eventHubName`: Ingesteld op de naam van de event hub die u hebt gemaakt.
- De omleidings-URI die u in uw app in de vorige stappen hebt opgegeven.
 
Wanneer u de consoletoepassing uitvoert, wordt u gevraagd om te selecteren van een scenario; Klik op **interactieve aanmelding voor gebruiker** door te typen van het nummer en druk op ENTER. De toepassing verschijnt een venster aanmelden, wordt u gevraagd om uw toestemming voor toegang tot de Event Hubs en gebruikt vervolgens de service uit te voeren via het scenario verzenden/ontvangen met behulp van de identiteit van de aanmelding.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Details van prijzen van Eventhubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
---
title: Voorbeeld van Role-Based Access Control - Azure Event Hubs | Microsoft Docs
description: In dit artikel bevat informatie over op rollen gebaseerd toegangsbeheer voor Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/21/2019
ms.author: shvija
ms.openlocfilehash: 117b7d4adb508628ee768bb9531d0bbc52f61121
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816055"
---
# <a name="active-directory-role-based-access-control-preview"></a>Actieve Directory Role-Based Access Control (preview)

Microsoft Azure biedt geïntegreerde toegangsbeheer voor bronnen en toepassingen op basis van Azure Active Directory (Azure AD). Met Azure AD, kunt u gebruikersaccounts en toepassingen kunt beheren specifiek voor uw Azure-toepassingen of kunt u uw bestaande Active Directory-infrastructuur met Azure AD voor het hele bedrijf single-sign-on die ook Azure-resources omvat federeren en toepassingen wordt gehost op Azure. Vervolgens kunt u deze Azure AD-identiteiten voor gebruikers en toepassingen toewijzen aan globale en servicespecifieke rollen om toegang verlenen tot Azure-resources.

Voor Azure Event Hubs, het beheer van naamruimten en alle gerelateerde resources via Azure portal en de Azure Resourcemanagement-API is al beveiligd met behulp van de *op rollen gebaseerd toegangsbeheer* (RBAC)-model. RBAC voor bewerkingen van de runtime is een functie nu in openbare preview. 

Een toepassing die gebruikmaakt van Azure AD RBAC hoeft niet voor het afhandelen van SAS-regels en sleutels of een andere specifieke naar Event Hubs toegangstokens. De client-app communiceert met Azure AD om geen verificatiecontext stand te brengen en een toegangstoken opgehaald voor Event Hubs. Met de domeingebruikersaccounts waarvoor interactieve aanmelding, de toepassing verwerkt nooit geen referenties rechtstreeks.

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen
Azure biedt de volgende ingebouwde RBAC-rollen voor het verlenen van toegang tot een Event Hubs naam ruimte:

De rol [Azure Event hubs data Owner (preview)](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner-preview) maakt gegevens toegang tot een event hubs naam ruimte en de bijbehorende entiteiten (wacht rijen, onderwerpen, abonnementen en filters) mogelijk

>[!IMPORTANT]
> We hebben eerder ondersteuning geboden voor het toevoegen van beheerde identiteit aan de rol **Owner** of **Inzender** . De bevoegdheden voor gegevens toegang voor de rol **eigenaar** en **Inzender** worden echter niet meer nageleefd. Als u de rol **eigenaar** of **Inzender** gebruikt, schakelt u over naar het gebruik van de rol **Azure Event hubs data Owner (preview)** .


## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Event Hubs gebruiken met een gebruikersaccount voor Azure AD-domein

De volgende sectie wordt beschreven hoe u maken en uitvoeren van een voorbeeldtoepassing die om een interactieve Azure vraagt AD-gebruiker aanmelden, het verlenen van toegang aan de gebruikersaccount Event Hubs en die identiteit gebruiken voor toegang tot de Event Hubs. 

In deze inleiding wordt een eenvoudige console toepassing beschreven, de [code waarvoor op github](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/AzureEventHubsSDK)

### <a name="create-an-active-directory-user-account"></a>Een Active Directory-gebruikersaccount maken

Deze eerste stap is optioneel. Elk Azure-abonnement wordt automatisch gekoppeld aan een Azure Active Directory-tenant en hebt u toegang tot een Azure-abonnement, wordt uw account is al geregistreerd. Dit betekent dat u zojuist hebt uw account kunt gebruiken. 

Als u nog steeds wilt maken van een speciaal account voor dit scenario [als volgt te werk](../automation/automation-create-aduser-account.md). U moet gemachtigd voor het maken van accounts in de Azure Active Directory-tenant, wat mogelijk niet het geval is bij grotere enterprise-scenario's.

### <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Maak vervolgens [een event hubs naam ruimte](event-hubs-create.md). 

Nadat de naamruimte is gemaakt, gaat u naar de **Access Control (IAM)** pagina in de portal en klik vervolgens op **roltoewijzing toevoegen** de Azure AD-gebruikersaccount toevoegen aan de rol van eigenaar. Als u uw eigen gebruikersaccount gebruikt en u de naamruimte hebt gemaakt, bent u al in de rol van eigenaar. Zoek de naam van de web-App in een ander account toevoegen aan de rol, de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**. Het gebruikersaccount heeft nu toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt.
 
### <a name="register-the-application"></a>De toepassing registreren

Voordat u de voorbeeldtoepassing uitvoeren kunt, deze te registreren in Azure AD en de toestemmingsprompt waarmee de aanvraag voor toegang tot Event Hubs op de andere gebruikers goedkeuren. 

Omdat de voorbeeldtoepassing een toepassing is, moet u een native toepassing registreren en toevoegen van de API-machtigingen voor **Microsoft.EventHub** aan de set 'vereist machtigingen'. Systeemeigen toepassingen moeten ook een **omleidings-URI** in Azure AD die als een-id. fungeert de URI niet hoeft te worden van een netwerk-doel. Gebruik `https://eventhubs.microsoft.com` voor dit voorbeeld omdat het voorbeeld code al gebruikmaakt van deze URI.

De registratie van gedetailleerde stapsgewijze [in deze zelfstudie](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Volg de stappen voor het registreren van een **systeemeigen** app, en volg de instructies update toe te voegen de **Microsoft.EventHub** API aan de vereiste machtigingen. Als u de stappen hebt uitgevoerd, noteer de **TenantId** en de **ApplicationId**, moet u deze waarden in de toepassing uit te voeren.

### <a name="run-the-app"></a>De app uitvoeren

Voordat u het voorbeeld uitvoeren kunt, bewerkt u het bestand App.config en, afhankelijk van uw scenario, stel de volgende waarden:

- `tenantId`: Stel in op de waarde **TenantId** .
- `clientId`: Stel in op **ApplicationId** -waarde. 
- `clientSecret`: Als u zich wilt aanmelden met behulp van het client geheim, maakt u het in azure AD. Een web-app of API ook gebruiken in plaats van een systeemeigen app. Voeg ook de app onder **Access Control (IAM)** in de naamruimte die u eerder hebt gemaakt.
- `eventHubNamespaceFQDN`: Stel in op de volledig gekwalificeerde DNS-naam van de zojuist gemaakte Event Hubs naam ruimte; bijvoorbeeld `example.servicebus.windows.net`.
- `eventHubName`: Stel in op de naam van de Event Hub die u hebt gemaakt.
- De omleidings-URI die u in uw app in de vorige stappen hebt opgegeven.
 
Wanneer u de consoletoepassing uitvoert, wordt u gevraagd om te selecteren van een scenario; Klik op **interactieve aanmelding voor gebruiker** door te typen van het nummer en druk op ENTER. De toepassing verschijnt een venster aanmelden, wordt u gevraagd om uw toestemming voor toegang tot de Event Hubs en gebruikt vervolgens de service uit te voeren via het scenario verzenden/ontvangen met behulp van de identiteit van de aanmelding.

De app wordt `ServiceAudience.EventHubsAudience` gebruikt als de token doelgroep. Wanneer u andere talen of Sdk's gebruikt, waarbij de doel groep niet beschikbaar is als een constante, is `https://eventhubs.azure.net/`de juiste waarde die moet worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Details van prijzen van Eventhubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

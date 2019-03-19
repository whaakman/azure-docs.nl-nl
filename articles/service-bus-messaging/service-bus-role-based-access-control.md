---
title: Voorbeeld van Azure Service Bus Role-Based toegangsbeheer (RBAC) | Microsoft Docs
description: Access control van Azure Service Bus op basis van rollen
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: aschhab
ms.openlocfilehash: 7ef152b130e77e833e19c51ff97d0cea577216c5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57844998"
---
# <a name="active-directory-role-based-access-control-preview"></a>Actieve Directory Role-Based Access Control (preview)

Microsoft Azure biedt geïntegreerde toegangsbeheer voor bronnen en toepassingen op basis van Azure Active Directory (Azure AD). Met Azure AD, kunt u gebruikersaccounts beheren en toepassingen specifiek voor uw Azure-toepassingen, of u kunt uw bestaande Active Directory-infrastructuur met Azure AD voor het hele bedrijf single-sign-on die ook Azure-resources omvat federeren en toepassingen wordt gehost op Azure. Vervolgens kunt u deze Azure AD-identiteiten voor gebruikers en toepassingen toewijzen aan globale en servicespecifieke rollen om toegang verlenen tot Azure-resources.

Voor Azure Service Bus, het beheer van naamruimten en alle gerelateerde resources via Azure portal en de Azure Resourcemanagement-API is al beveiligd met behulp van de *op rollen gebaseerd toegangsbeheer* (RBAC)-model. RBAC voor bewerkingen van de runtime is een functie nu in openbare preview.

Een toepassing die gebruikmaakt van Azure AD RBAC hoeft niet voor het afhandelen van SAS-regels en sleutels of een andere specifiek voor Service Bus toegangstokens. De client-app communiceert met Azure AD om geen verificatiecontext stand te brengen en een toegangstoken opgehaald voor Service Bus. Met de domeingebruikersaccounts waarvoor interactieve aanmelding, de toepassing verwerkt nooit geen referenties rechtstreeks.

## <a name="service-bus-roles-and-permissions"></a>Service Bus-rollen en machtigingen

Voor de eerste openbare preview, kunt u Azure AD-accounts en service-principals alleen toevoegen aan de rollen 'Eigenaar' of 'Medewerker' van een Service Bus Messaging-naamruimte. Met deze bewerking verleent de identiteit volledige controle over alle entiteiten in de naamruimte. -Beheerbewerkingen waarbij wijzigen van de topologie van de naamruimte worden in eerste instantie alleen ondersteund via Azure resource Manager en niet via de systeemeigen Service Bus REST-beheerinterface. Deze ondersteuning betekent dat ook de .NET Framework client [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object kan niet worden gebruikt met een Azure AD-account.

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Service Bus gebruiken met een gebruikersaccount voor Azure AD-domein

De volgende sectie wordt beschreven hoe u maken en uitvoeren van een voorbeeldtoepassing die om een interactieve Azure vraagt AD-gebruiker om aan te melden, hoe u Service Bus toegang verlenen tot dat gebruikersaccount en die identiteit gebruiken voor toegang tot de Event Hubs.

Deze inleiding beschrijft een eenvoudige consoletoepassing de [code waarvoor is op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Een Active Directory-gebruikersaccount maken

Deze eerste stap is optioneel. Elk Azure-abonnement wordt automatisch gekoppeld aan een Azure Active Directory-tenant en hebt u toegang tot een Azure-abonnement, wordt uw account is al geregistreerd. Dit betekent dat u zojuist hebt uw account kunt gebruiken.

Als u nog steeds wilt maken van een speciaal account voor dit scenario [als volgt te werk](../automation/automation-create-aduser-account.md). U moet gemachtigd voor het maken van accounts in de Azure Active Directory-tenant, wat mogelijk niet het geval is bij grotere enterprise-scenario's.

### <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken

Volgende [maken van een Service Bus Messaging-naamruimte](service-bus-create-namespace-portal.md) in een van de Azure-regio's die ondersteuning bieden voor Preview-versie voor RBAC: **VS Oost**, **VS Oost 2**, of **West-Europa**.

Nadat de naamruimte is gemaakt, gaat u naar de **Access Control (IAM)** pagina in de portal en klik vervolgens op **roltoewijzing toevoegen** de Azure AD-gebruikersaccount toevoegen aan de rol van eigenaar. Als u uw eigen gebruikersaccount gebruikt en u de naamruimte hebt gemaakt, bent u al in de rol van eigenaar. Zoek de naam van de web-App in een ander account toevoegen aan de rol, de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**.

Het gebruikersaccount heeft nu toegang tot de Service Bus-naamruimte en naar de wachtrij die u eerder hebt gemaakt.

### <a name="register-the-application"></a>De toepassing registreren

Voordat u de voorbeeldtoepassing uitvoeren kunt, deze te registreren in Azure AD en de toestemmingsprompt waarmee de aanvraag voor toegang tot Azure Service Bus namens goedkeuren.

Omdat de voorbeeldtoepassing een toepassing is, moet u een native toepassing registreren en toevoegen van de API-machtigingen voor **Microsoft.ServiceBus** aan de set 'vereist machtigingen'. Systeemeigen toepassingen moeten ook een **omleidings-URI** in Azure AD die als een-id. fungeert de URI niet hoeft te worden van een netwerk-doel. Gebruik `https://servicebus.microsoft.com` voor dit voorbeeld omdat het voorbeeld code al gebruikmaakt van deze URI.

De registratie van gedetailleerde stapsgewijze [in deze zelfstudie](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Volg de stappen voor het registreren van een **systeemeigen** app, en volg de instructies update toe te voegen de **Microsoft.ServiceBus** API aan de vereiste machtigingen. Als u de stappen hebt uitgevoerd, noteer de **TenantId** en de **ApplicationId**, moet u deze waarden in de toepassing uit te voeren.

### <a name="run-the-app"></a>De app uitvoeren

Voordat u het voorbeeld uitvoeren kunt, bewerkt u het bestand App.config en, afhankelijk van uw scenario, stel de volgende waarden:

- `tenantId`: Ingesteld op **TenantId** waarde.
- `clientId`: Ingesteld op **ApplicationId** waarde.
- `clientSecret`: Als u wilt om aan te melden met behulp van het clientgeheim, kunt u deze in Azure AD maken. Een web-app of API ook gebruiken in plaats van een systeemeigen app. Voeg ook de app onder **Access Control (IAM)** in de naamruimte die u eerder hebt gemaakt.
- `serviceBusNamespaceFQDN`: Ingesteld op de volledige DNS-naam van uw zojuist gemaakte Service Bus-naamruimte. bijvoorbeeld, `example.servicebus.windows.net`.
- `queueName`: Stel op de naam van de wachtrij die u hebt gemaakt.
- De omleidings-URI die u in uw app in de vorige stappen hebt opgegeven.

Wanneer u de consoletoepassing uitvoert, wordt u gevraagd om te selecteren van een scenario; Klik op **interactieve aanmelding voor gebruiker** door te typen van het nummer en druk op ENTER. De toepassing een aanmelding weergegeven, wordt u gevraagd om uw toestemming voor toegang tot Service Bus en gebruikt vervolgens de service uit te voeren via het scenario verzenden/ontvangen met behulp van de Aanmeldingsidentiteit van de.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

---
title: Preview van Azure Event Hubs Role-Based toegangsbeheer (RBAC) | Microsoft Docs
description: Azure Event Hubs rollen gebaseerd toegangsbeheer
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
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 0d3a779eb2cccf242bcd42d82c1a90048b3512ab
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="active-directory-role-based-access-control-preview"></a>Actieve Directory Role-Based Access Control (preview)

Microsoft Azure biedt een geïntegreerde toegangsbeheer voor bronnen en toepassingen op basis van Azure Active Directory (Azure AD). Met Azure AD, kunt u gebruikersaccounts beheren en toepassingen specifiek voor uw Azure op basis van toepassingen, of u kunt uw bestaande Active Directory-infrastructuur met Azure AD voor het hele bedrijf eenmalige aanmelding die ook Azure-resources omvat federeren en Azure gehoste toepassingen. Vervolgens kunt u die Azure AD-gebruiker en toepassing identiteiten aan globale en servicespecifieke rollen toewijzen om het verlenen van toegang tot Azure-resources.

Voor Azure Event Hubs, het beheer van naamruimten en alle gerelateerde resources via de Azure-portal en de API voor het beheer van de Azure-resource is al beveiligd met behulp van de *toegangsbeheer op basis van rollen* (RBAC)-model. RBAC voor bewerkingen van de runtime is nu een functie openbare preview. 

Een toepassing die gebruikmaakt van Azure AD RBAC hoeft niet voor het afhandelen van SAS-regels en -sleutels of een andere specifieke naar Event Hubs toegangstokens. De client-app werkt met Azure AD tot stand brengen van verificatiecontext en verkrijgt van een toegangstoken voor Event Hubs. Met domeingebruikersaccounts waarvoor interactieve aanmelding, de toepassing verwerkt nooit geen referenties rechtstreeks.

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen

Voor de eerste openbare preview, kunt u Azure AD-accounts en service-principals alleen toevoegen aan de rollen 'Eigenaar' of 'Inzender' van een Event Hubs-naamruimte. Deze bewerking verleent de identiteit volledige controle over alle entiteiten in de naamruimte. Beheerbewerkingen die wijzigen van de naamruimte-topologie in eerste instantie worden alleen ondersteund via Azure resourcemanagement en niet via de systeemeigen Event Hubs REST-interface. Deze ondersteuning betekent dat ook de .NET Framework client [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) object kan niet worden gebruikt met een Azure AD-account.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Event Hubs met een Azure AD-domeingebruikersaccount gebruiken

De volgende sectie wordt beschreven hoe u maken en uitvoeren van een voorbeeldtoepassing die om een interactieve Azure vraagt AD-gebruiker zich aanmeldt, het verlenen van toegang aan de gebruikersaccount Event Hubs en die identiteit gebruiken voor toegang tot de Event Hubs. 

Deze inleiding een eenvoudige consoletoepassing beschrijft de [waarvan code is op Github](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>Een Active Directory-gebruikersaccount maken

Deze eerste stap is optioneel. Elke Azure-abonnement is automatisch gekoppeld aan een Azure Active Directory-tenant en hebt u toegang tot een Azure-abonnement, wordt uw gebruikersaccount is al geregistreerd. Dit betekent dat u zojuist hebt uw account kunt gebruiken. 

Als u nog steeds wilt maken van een specifiek account voor dit scenario [als volgt te werk](../automation/automation-create-aduser-account.md). U moet gemachtigd zijn voor het maken van accounts in de Azure Active Directory-tenant, wat mogelijk niet het geval is bij grotere bedrijfsscenario's.

### <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Vervolgens [maken van een naamruimte Event Hubs](event-hubs-create.md) in een van de Azure-regio's die Event Hubs preview ondersteuning bieden voor RBAC: **VS-Oost**, **ons Oost 2**, of **West-Europa** . 

Zodra de naamruimte is gemaakt, gaat u naar de **Access Control (IAM)** pagina in de portal en klik vervolgens op **toevoegen** de Azure AD-gebruikersaccount toevoegen aan de rol van eigenaar. Als u uw eigen gebruikersaccount gebruiken en u de naamruimte hebt gemaakt, bent u al in de rol van eigenaar. Als u wilt een ander account toevoegen aan de rol, zoekt u de naam van de webtoepassing in de **machtigingen toevoegen** Configuratiescherm **Selecteer** veld en klik op de vermelding. Klik vervolgens op **Opslaan**.
 
![](./media/event-hubs-role-based-access-control/rbac1.PNG)

Het gebruikersaccount heeft nu toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt.
 
### <a name="register-the-application"></a>De toepassing registreren

Voordat u de voorbeeldtoepassing uitvoert kunt, registreert u dit in Azure AD en goedkeuren van de instemmingsprompt waarmee de toepassing toegang krijgen tot Event Hubs voor zijn rekening. 

Omdat de voorbeeldtoepassing een consoletoepassing is, moet u een systeemeigen toepassing registreren en machtigingen voor API toevoegen **Microsoft.EventHub** aan de set 'vereist de machtigingen'. Systeemeigen toepassingen moeten ook een **omleidings-URI** in Azure AD die als een id fungeert; de URI niet hoeft te worden van een doel van het netwerk. Gebruik `http://eventhubs.microsoft.com` voor dit voorbeeld, omdat het voorbeeld code al wordt gebruikt die URI.

De registratie van gedetailleerde stappen worden beschreven [in deze zelfstudie](../active-directory/develop/active-directory-integrating-applications.md). Volg de stappen voor het registreren van een **systeemeigen** app, en volg de instructies update toevoegen de **Microsoft.EventHub** API's voor de vereiste machtigingen. Als u de stappen hebt uitgevoerd, noteer de **TenantId** en de **ApplicationId**, zoals moet u deze waarden in de toepassing uit te voeren.

### <a name="run-the-app"></a>De app uitvoeren

Voordat u het voorbeeld uitvoeren kunt, bewerk het bestand App.config en, afhankelijk van uw scenario, stel de volgende waarden:

- `tenantId`: Ingesteld op **TenantId** waarde.
- `clientId`: Ingesteld op **ApplicationId** waarde. 
- `clientSecret`: Als u wilt zich aanmelden met het clientgeheim, in Azure AD maken. Een web-app of API ook gebruiken in plaats van een systeemeigen app. De app onder ook toe te voegen **Access Control (IAM)** in de naamruimte die u eerder hebt gemaakt.
- `eventHubNamespaceFQDN`: Ingesteld op de volledige DNS-naam van de naamruimte van uw nieuwe Event Hubs. bijvoorbeeld: `example.servicebus.windows.net`.
- `eventHubName`: Ingesteld op de naam van de event hub die u hebt gemaakt.
- De omleidings-URI die u in uw app in de vorige stappen hebt opgegeven.
 
Wanneer u de consoletoepassing uitvoert, wordt u gevraagd om een scenario; te selecteren Klik op **interactieve gebruikersaanmelding** door het nummer te typen en op ENTER te drukken. De toepassing een aanmelding-upvenster wordt weergegeven, wordt u gevraagd om uw toestemming voor toegang tot de Event Hubs en gebruikt vervolgens de service worden uitgevoerd via het scenario verzenden/ontvangen met behulp van de identiteit van de aanmelding.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Event Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
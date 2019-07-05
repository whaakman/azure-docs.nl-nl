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
ms.openlocfilehash: 6f5390162ce56a0e77ef41740d7e88f3546c5530
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444733"
---
# <a name="active-directory-role-based-access-control-preview"></a>Actieve Directory Role-Based Access Control (preview)

Microsoft Azure biedt geïntegreerde toegangsbeheer voor bronnen en toepassingen op basis van Azure Active Directory (Azure AD). Met Azure AD, kunt u gebruikersaccounts beheren en toepassingen specifiek voor uw Azure-toepassingen, of u kunt uw bestaande Active Directory-infrastructuur met Azure AD voor het hele bedrijf single-sign-on die ook Azure-resources omvat federeren en toepassingen wordt gehost op Azure. Vervolgens kunt u deze Azure AD-identiteiten voor gebruikers en toepassingen toewijzen aan globale en servicespecifieke rollen om toegang verlenen tot Azure-resources.

Voor Azure Service Bus, het beheer van naamruimten en alle gerelateerde resources via Azure portal en de Azure Resourcemanagement-API is al beveiligd met behulp van de *op rollen gebaseerd toegangsbeheer* (RBAC)-model. RBAC voor bewerkingen van de runtime is een functie nu in openbare preview.

Een toepassing die gebruikmaakt van Azure AD RBAC hoeft niet voor het afhandelen van SAS-regels en sleutels of een andere specifiek voor Service Bus toegangstokens. De client-app communiceert met Azure AD om geen verificatiecontext stand te brengen en een toegangstoken opgehaald voor Service Bus. Met de domeingebruikersaccounts waarvoor interactieve aanmelding, de toepassing verwerkt nooit geen referenties rechtstreeks.

## <a name="service-bus-roles-and-permissions"></a>Service Bus-rollen en machtigingen

Azure biedt de volgende ingebouwde RBAC-rollen voor het verlenen van toegang tot een Service Bus-naamruimte:

* [Azure Service Bus de eigenaar van gegevens (Preview)](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner-preview): Hiermee kunt toegang tot gegevens in Service Bus-naamruimte en de entiteiten (wachtrijen, onderwerpen, abonnementen en Filters)

>[!IMPORTANT]
> We eerder toe te voegen beheerde identiteit ondersteund de **'Eigenaar'** of **"Bijdrager"** rol.
>
> Echter bevoegdheden voor toegang tot de gegevens **'Eigenaar'** en **"Bijdrager"** rol wordt niet meer worden herkend. Als u de **'Eigenaar'** of **"Bijdrager"** rol en vervolgens die moet worden aangepast aan het gebruik van de **'Azure Service Bus Gegevenseigenaar (Preview)'** rol.

## <a name="use-service-bus-with-an-azure-ad-domain-user-account"></a>Service Bus gebruiken met een gebruikersaccount voor Azure AD-domein

De volgende sectie wordt beschreven hoe u maken en uitvoeren van een voorbeeldtoepassing die om een interactieve Azure vraagt AD-gebruiker zich aanmeldt, hoe u Service Bus toegang verlenen tot dat gebruikersaccount en die identiteit gebruiken voor toegang tot de Event Hubs.

Deze inleiding beschrijft een eenvoudige consoletoepassing de [code waarvoor is op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl).

### <a name="create-an-active-directory-user-account"></a>Een Active Directory-gebruikersaccount maken

Deze eerste stap is optioneel. Elk Azure-abonnement wordt automatisch gekoppeld aan een Azure Active Directory-tenant en hebt u toegang tot een Azure-abonnement, wordt uw account is al geregistreerd. Dit betekent dat u zojuist hebt uw account kunt gebruiken.

Als u nog steeds wilt maken van een speciaal account voor dit scenario [als volgt te werk](../automation/automation-create-aduser-account.md). U moet gemachtigd voor het maken van accounts in de Azure Active Directory-tenant, wat mogelijk niet het geval is bij grotere enterprise-scenario's.

### <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken

Volgende [maken van een Service Bus Messaging-naamruimte](service-bus-create-namespace-portal.md).

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
- `clientSecret`: Als u wilt zich aanmelden met het clientgeheim, kunt u deze in Azure AD maken. Een web-app of API ook gebruiken in plaats van een systeemeigen app. Voeg ook de app onder **Access Control (IAM)** in de naamruimte die u eerder hebt gemaakt.
- `serviceBusNamespaceFQDN`: Ingesteld op de volledige DNS-naam van uw zojuist gemaakte Service Bus-naamruimte. bijvoorbeeld, `example.servicebus.windows.net`.
- `queueName`: Stel op de naam van de wachtrij die u hebt gemaakt.
- De omleidings-URI die u in uw app in de vorige stappen hebt opgegeven.

Wanneer u de consoletoepassing uitvoert, wordt u gevraagd om te selecteren van een scenario; Klik op **interactieve aanmelding voor gebruiker** door te typen van het nummer en druk op ENTER. De toepassing een aanmelding weergegeven, wordt u gevraagd om uw toestemming voor toegang tot Service Bus en gebruikt vervolgens de service uit te voeren via het scenario verzenden/ontvangen met behulp van de Aanmeldingsidentiteit van de.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over de Service Bus-berichtenservice

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)

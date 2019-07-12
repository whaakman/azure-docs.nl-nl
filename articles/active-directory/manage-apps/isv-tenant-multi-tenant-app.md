---
title: Een Azure-tenant voor een multitenant-toepassing maken
description: Richtlijnen voor onafhankelijke softwareleveranciers over de integratie met Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69cc625500af60a753ad8e7db0363954088f3307
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659578"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Een Azure-tenant voor een multitenant-toepassing maken  

U moet een Azure Active Directory-tenant voor het registreren van de toepassing en inschakelen van de Federatie van uw klant-id's maken voor toegang tot uw toepassing met meerdere tenants. Zie [kiezen van het juiste federation-protocol voor uw toepassing met meerdere tenants](isv-choose-multi-tenant-federation.md). Deze tenant kunt u bij het testen van uw toepassing en de Federatie in een omgeving die vergelijkbaar is met uw Azure AD-omgeving van klanten.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Kosten voor het hosten van een toepassing met meerdere tenants

Azure Active Directory is beschikbaar in drie SKU's, gratis, Basic en Premium. [Zie de gedetailleerde vergelijking](https://azure.microsoft.com/pricing/details/active-directory/).

U kunt maken van uw Azure-abonnement en Azure active directory gratis en basic-functies gebruiken.

## <a name="create-your-tenant"></a>Uw tenant maken

1. Maak uw Tenant. Zie [een ontwikkelaarsomgeving instellen](../develop/quickstart-create-new-tenant.md).

2. Inschakelen en eenmalige aanmelding toegang tot uw toepassing testen

   a. **Voor toepassingen met OIDC of Oath**, [uw toepassing registreren](../develop/quickstart-register-app.md) als een toepassing met meerdere tenants. Selecteer de Accounts in een organisatie-map en persoonlijke Microsoft-accounts option in accounttypen ondersteund

   b. **Voor SAML - en WS Fed-gebaseerde toepassingen**, u [configureren SAML gebaseerde eenmalige aanmelding](configure-single-sign-on-non-gallery-applications.md) toepassingen die gebruikmaken van een algemene SAML-sjabloon in Azure AD.

U kunt ook [converteren van een toepassing één tenant naar meerdere tenants](../develop/howto-convert-app-to-be-multi-tenant.md) indien nodig.

## <a name="next-steps"></a>Volgende stappen

[Eenmalige aanmelding in uw toepassing integreren](isv-sso-content.md)

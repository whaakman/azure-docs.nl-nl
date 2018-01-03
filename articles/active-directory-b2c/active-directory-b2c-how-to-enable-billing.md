---
title: Het koppelen van een Azure-abonnement aan Azure AD B2C | Microsoft Docs
description: Stapsgewijze handleiding voor het inschakelen van facturering voor Azure AD B2C-tenant in een Azure-abonnement.
services: active-directory-b2c
documentationcenter: dev-center-name
author: parakhj
manager: mtillman
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2017
ms.author: parja
ms.openlocfilehash: 063c00fe47be25b9359e80d71abfaf453c7a7074
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-ad-b2c-tenant"></a>Een Azure-abonnement koppelen aan een Azure AD B2C-tenant

> [!IMPORTANT]
> De meest recente informatie over het gebruik van facturering en prijzen voor Azure AD B2C op de volgende pagina is: [prijzen van Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

Gebruikskosten voor Azure AD B2C wordt gefactureerd voor een Azure-abonnement. Wanneer een Azure AD B2C-tenant is gemaakt, moet de tenantbeheerder expliciet de Azure AD B2C-tenant te koppelen aan een Azure-abonnement. Dit artikel ziet u hoe.

> [!NOTE]
> Een abonnement dat is gekoppeld aan een Azure AD B2C-tenant kan alleen worden gebruikt voor de facturering van Azure AD B2C-gebruik. Het abonnement kan niet worden gebruikt voor het toevoegen van andere Azure-services of Office 365-licenties *binnen de Azure AD B2C-tenant*.

 De abonnementskoppeling wordt bereikt door het maken van een Azure AD B2C 'resource' binnen de doel-Azure-abonnement. Veel Azure AD B2C 'resources' kunnen worden gemaakt binnen een één Azure-abonnement, samen met andere Azure-resources (voor opslag van bijvoorbeeld virtuele machines, gegevens, LogicApps). U kunt alle resources binnen het abonnement zien door te gaan met de Azure AD-tenant waaraan het abonnement is gekoppeld.

Er is een geldige Azure-abonnement nodig om door te gaan.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

U moet eerst [een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md) dat u wilt koppelen van een abonnement op. Deze stap overslaan als u een Azure AD B2C-tenant al hebt gemaakt.

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Open Azure-portal op de Azure AD-tenant waarin uw Azure-abonnement

Navigeer naar de Azure AD-tenant waarin uw Azure-abonnement. Open de [Azure-portal](https://portal.azure.com), en schakel over naar de Azure AD-tenant met het Azure-abonnement u wilt gebruiken.

![Overschakelen naar uw Azure AD-tenant](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a>Azure AD B2C zoeken in Azure Marketplace

Klik op de knop **Nieuw**. Voer in het veld **Marketplace doorzoeken** in: `B2C`.

![Knop gemarkeerd en de Azure AD B2C-tekst toevoegen in de zoekopdracht de marketplace-veld](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

Selecteer in de lijst met resultaten **Azure AD B2C**.

![Azure AD B2C wordt geselecteerd in de lijst met resultaten](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

Meer informatie over Azure AD B2C worden weergegeven. Klik op de knop **Maken** om de nieuwe Azure Active Directory B2C-tenant te configureren.

Selecteer in het scherm voor het maken van resource **koppeling een bestaande Azure AD B2C-Tenant met mijn Azure-abonnement**.

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a>Een Azure AD B2C-resource in het Azure-abonnement maken

Selecteer een Azure AD B2C-tenant in de vervolgkeuzelijst in het dialoogvenster voor het maken van resource. U ziet alle van de tenants dat u de globale beheerder van en die nog niet zijn gekoppeld aan een abonnement.

De naam van de Azure AD B2C-resource wordt vooraf geselecteerd zodat deze overeenkomen met de domeinnaam van de Azure AD B2C-tenant.

Voor het abonnement, selecteert u een actief Azure-abonnement dat u de beheerder van.

Selecteer een resourcegroep en locatie van resourcegroep. De selectie hier heeft geen invloed op de locatie van Azure AD B2C-tenant, prestaties of facturering status.

![B2C-Resource maken](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Uw Azure AD B2C-tenant resources beheren

Als een Azure AD B2C-resource is gemaakt in het Azure-abonnement, ziet u een nieuwe resource van het type 'B2C-tenant' samen met andere Azure-resources toegevoegd.

U kunt deze bron te gebruiken:

- Ga naar het abonnement op de factureringsgegevens te bekijken.
- Ga naar uw Azure AD B2C-tenant
- Een ondersteuningsaanvraag indienen
- Uw Azure AD B2C-tenant resource verplaatsen naar een andere Azure-abonnement of een andere resourcegroep.

![B2C broninstellingen](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bekende problemen

### <a name="csp-subscriptions"></a>CSP-abonnementen

Op dit moment wordt een Azure AD B2C-tenant **kan niet** koppeling naar de CSP-abonnementen.

### <a name="self-imposed-restrictions"></a>Zelf-opgelegde beperkingen

Een gebruiker kan een regionale beperking voor het maken van Azure-resource hebt vastgesteld. Deze beperking kan voorkomen dat het maken van Azure AD B2C-bron. Om te beperken, verwijzen wij u versoepelen deze beperking.

## <a name="next-steps"></a>Volgende stappen

Zodra deze stappen voltooid voor elk van uw Azure AD B2C-tenants zijn, wordt uw Azure-abonnement in rekening gebracht volgens de details van uw Azure directe of Enterprise Agreement.

U kunt het gebruik en factureringsgegevens bekijken binnen uw geselecteerde Azure-abonnement. U kunt ook bekijken gedetailleerde dag gebruiksrapporten met behulp van de [gebruiksrapportage API](active-directory-b2c-reference-usage-reporting-api.md).

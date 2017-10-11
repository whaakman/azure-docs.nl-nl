---
title: Het koppelen van een Azure-abonnement aan Azure AD B2C | Microsoft Docs
description: Stapsgewijze handleiding voor het inschakelen van facturering voor Azure AD B2C-tenant in een Azure-abonnement.
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Een Azure-abonnement koppelen aan een Azure B2C-tenant betalen voor gebruikskosten

Doorlopende gebruikskosten voor Azure Active Directory B2C (of Azure AD B2C) worden gefactureerd op een Azure-abonnement. Dit is nodig voor de tenantbeheerder expliciet de Azure AD B2C-tenant te koppelen aan een Azure-abonnement na het maken van de B2C-tenant zelf.  Deze koppeling wordt bereikt door het maken van een Azure AD 'B2C-Tenant' resource in de doel-Azure-abonnement. Veel B2C tenants kunnen worden gekoppeld aan een enkele Azure-abonnement samen met andere Azure-resources (bijvoorbeeld virtuele machines, gegevensopslag, LogicApps)


> [!IMPORTANT]
> De meest recente informatie over het gebruik van facturering en prijzen voor B2C op de volgende pagina is: [prijzen van Azure AD B2C](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Stap 1: een Azure AD B2C-Tenant maken
Maken van de B2C-tenant moet eerst worden voltooid. Deze stap overslaan als u uw doel B2C-Tenant al hebt gemaakt. [Aan de slag met Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Stap 2: Open Azure-portal op de Azure AD-Tenant waarin uw Azure-abonnement
Navigeer naar [Azure Portal](https://portal.azure.com). Overschakelen naar de Azure AD-Tenant met het Azure-abonnement dat u wilt gebruiken. Deze Azure AD-tenant wijkt af van de B2C-tenant. Klik in de Azure-portal op de accountnaam in de rechterbovenhoek van het dashboard te selecteren van de Azure AD-Tenant. Er is een Azure-abonnement nodig om door te gaan. [Een Azure-abonnement ophalen](https://account.windowsazure.com/signup?showCatalog=True)

![Overschakelen naar uw Azure AD-Tenant](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Stap 3: een B2C-Tenant-resource maken in Azure Marketplace
Open Marketplace door te klikken op het pictogram Marketplace of selecteren van de groene '+' in de linkerbovenhoek van het dashboard.  Zoek en selecteer Azure Active Directory B2C. Selecteer maken.

![Selecteer Marketplace](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![AD B2C zoeken](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

De Azure AD B2C-Resource maken dialoogvenster omvat de volgende parameters:

1. Azure AD B2C-Tenant – Selecteer een Azure AD B2C-Tenant in de vervolgkeuzelijst.  Alleen in aanmerking komende Azure AD B2C tenants weergeven.  In aanmerking komende B2C tenants aan deze voorwaarden voldoet: U bent van de globale beheerder van de B2C-tenant en de B2C-tenant is momenteel niet gekoppeld aan een Azure-abonnement

2. Resourcenaam van Azure AD B2C - is voorgeselecteerd, zodat deze overeenkomen met de domeinnaam van de B2C-Tenant

3. Abonnement - een actief Azure-abonnement waarin u een beheerder of medebeheerder zijn.  Meerdere Tenants van Azure AD B2C kunnen worden toegevoegd aan één Azure-abonnement

4. Locatie van de resourcegroep en resourcegroep - deze artefacten kunt u meerdere Azure-resources te organiseren.  Deze optie heeft geen invloed op uw B2C-tenant locatie, prestaties of facturering status

5. Vastmaken aan dashboard voor eenvoudige toegang tot je factureringsgegevens voor B2C-tenant en de instellingen van de tenant B2C ![B2C-Resource maken](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Stap 4: uw B2C-Tenant resources (optioneel) beheren
Zodra de implementatie is voltooid, wordt een nieuwe 'B2C-Tenant'-resource is gemaakt in de doelresourcegroep en gerelateerde Azure-abonnement.  U ziet een nieuwe resource van het type 'B2C-Tenant' toegevoegd naast uw andere Azure-resources.

![B2C-Resource maken](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Door te klikken op de resource B2C-tenant, zich u kunt
- Klik op de naam van het abonnement factureringsgegevens te bekijken. Zie facturering en informatie over het gebruik.
- Klik op Azure AD B2C-instellingen om een nieuw browsertabblad geopend rechtstreeks in uw B2C-tenant blade instellingen
- Een ondersteuningsaanvraag indienen
- Uw B2C-tenant resource verplaatsen naar een andere Azure-abonnement of naar een andere resourcegroep.  Deze keuze wijzigingen in welke Azure-abonnement ontvangt gebruikskosten.

![B2C broninstellingen](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bekende problemen
- Verwijderen van een B2C-Tenant. Als een B2C-Tenant is gemaakt, verwijderd en opnieuw gemaakt met dezelfde domeinnaam moet ook verwijderen en opnieuw maken van de resource 'Linking' met dezelfde domeinnaam.  Deze resource 'Linking' onder 'Alle resources' vindt u in de tenant voor abonnement via de Azure-portal.
- Zelf-opgelegde beperkingen op regionale Resourcelocatie.  In zeldzame gevallen, kan een gebruiker een regionale beperking voor het maken van Azure resource hebt vastgesteld.  Deze beperking kan voorkomen dat het maken van de koppeling tussen een Azure-abonnement en een B2C-Tenant. Om te beperken, verwijzen wij u versoepelen deze beperking.

## <a name="next-steps"></a>Volgende stappen
Zodra deze stappen voltooid voor elk van uw B2C-tenants zijn, wordt uw Azure-abonnement in rekening gebracht volgens de details van uw Azure directe of Enterprise Agreement.
- Bekijk informatie over het gebruik en facturering binnen uw geselecteerde Azure-abonnement
- Bekijk gedetailleerde dag gebruiksrapporten met behulp van de [gebruik rapportage-API](active-directory-b2c-reference-usage-reporting-api.md)

---
title: Een Azure Active Directory B2C-tenant maken | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een Azure Active Directory B2C-tenant kunt maken
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: parja
ms.openlocfilehash: afca6cf8f19c9b96de292881582e27133e35f096
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a>Een Azure Active Directory B2C-tenant maken in de Azure portal

Deze snelstartgids kunt u een Microsoft Azure Active Directory (Azure AD) B2C-tenant maken in een paar minuten. Wanneer u klaar bent, hebt u een B2C-tenant (directory) te gebruiken voor het registreren van de B2C-toepassingen.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

B2C-functies kunnen niet worden ingeschakeld in uw bestaande tenants. U moet een Azure AD B2C-tenant maken.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Gefeliciteerd, u hebt een Azure Active Directory B2C-tenant gemaakt. Bent u een globale beheerder van de tenant. U kunt naar believen hoofdbeheerders toevoegen. Als u wilt overschakelen naar de nieuwe tenant, klikt u op de *beheren van uw nieuwe koppeling van de tenant*.

![Uw nieuwe koppeling van de tenant beheren](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Als u van plan bent te gebruiken van een B2C-tenant voor een productie-app, lees het artikel op [productie-scale versus B2C preview tenants](active-directory-b2c-reference-tenant-type.md). Er zijn bekende problemen wanneer u een bestaande B2C-tenant verwijderen en opnieuw met dezelfde domeinnaam maken. U moet een B2C-tenant maken met een andere domeinnaam.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Uw tenant te koppelen aan uw abonnement

U moet uw Azure AD B2C-tenant koppelen aan uw Azure-abonnement alle B2C-functies inschakelen en betaalt voor gebruikskosten. Voor meer informatie lezen [in dit artikel](active-directory-b2c-how-to-enable-billing.md). Als u niet uw Azure AD B2C-tenant koppeling naar uw Azure-abonnement, bepaalde functionaliteit is geblokkeerd en ziet u een waarschuwing ('geen abonnement is gekoppeld aan deze B2C-tenant of de abonnement-behoeften uw aandacht.') in de B2C-instellingen. Het is belangrijk dat u deze stap uitvoeren voordat u uw apps naar de productie verzendt.

## <a name="easy-access-to-settings"></a>Eenvoudig toegang krijgen tot instellingen

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

U kunt de blade ook openen door te voeren `Azure AD B2C` in **zoeken bronnen** aan de bovenkant van de portal. Selecteer in de lijst met resultaten **Azure AD B2C** voor toegang tot de blade B2C-instellingen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw B2C-toepassing registreren in een B2C-tenant](active-directory-b2c-app-registration.md)
---
title: Een Azure Active Directory B2C-tenant maken | Microsoft Docs
description: Een onderwerp over het maken van een Azure Active Directory B2C-tenant.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d1031074885b0ca55d0b1647575354fcd0f49889
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712263"
---
# <a name="create-an-azure-active-directory-b2c-tenant"></a>Een Azure Active Directory B2C-tenant maken

Deze snelstartgids kunt u een Microsoft Azure Active Directory (Azure AD) B2C-tenant maken in een paar minuten. Wanneer u klaar bent, hebt u een B2C-tenant (directory) te gebruiken voor het registreren van de B2C-toepassingen.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

B2C-functies kunnen niet worden ingeschakeld in uw bestaande tenants. U moet een Azure AD B2C-tenant maken.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Gefeliciteerd, u hebt een Azure Active Directory B2C-tenant gemaakt. Bent u een globale beheerder van de tenant. U kunt naar believen hoofdbeheerders toevoegen. Als u wilt overschakelen naar de nieuwe tenant, klikt u op *beheren van uw nieuwe tenant*.

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
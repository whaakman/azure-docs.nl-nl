---
title: 'Azure Active Directory B2C: overschakelen naar een B2C-tenant | Microsoft Docs'
description: Overschakelen naar de context van uw Active Directory B2C-tenant
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 40d8d57d974a949fbdc0a06eeceb2d06bfbaa09f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Overschakelen naar uw Azure AD B2C-tenant

U moet zich in de context van uw Azure AD B2C-tenant bevinden om Azure AD B2C te configureren.

## <a name="log-into-azure-ad-b2c-tenant"></a>Aanmelden bij de Azure AD B2C-tenant

U moet bij Azure Portal zijn aangemeld als een globale beheerder van de Azure AD B2C-tenant om naar uw Azure AD B2C-tenant te gaan.

1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
1. Schakel tenants over door te klikken op uw e-mailadres of afbeelding in de rechterbovenhoek.
1. In de `Directory`-lijst die wordt weergegeven selecteert u de Azure AD B2C-tenant die u wilt beheren.

Azure Portal wordt vernieuwd.  U bent nu aangemeld bij Azure Portal in de context van uw Azure AD B2C-tenant.

## <a name="navigate-to-the-b2c-features-blade"></a>Ga naar de blade B2C-functies

1. Klik op **Bladeren** in de navigatiebalk aan de linkerkant.
1. Klik op **> Meer services** en zoek vervolgens naar `Azure AD B2C` in het linker navigatievenster.  (Als u het aan het Startboard links wilt vastmaken, klikt u op de ster aan de linkerkant van Azure AD B2C)
1. Klik op **Azure AD B2C** voor toegang tot de blade B2C-functies.
   
    ![Schermafbeelding van bladeren naar de blade B2C-functies](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> U moet een globale beheerder van de B2C-tenant zijn om de blade B2C-functies te kunnen openen. Een globale beheerder van andere tenant of een gebruiker van een tenant heeft hiertoe geen toegang.  U kunt overschakelen naar uw B2C-tenant met behulp van de tenantwisselaar in de rechterbovenhoek van Azure Portal.

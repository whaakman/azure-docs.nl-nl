---
title: Zelfstudie - een Azure Active Directory B2C-tenant maken | Microsoft Docs
description: Informatie over het voorbereiden voor het registreren van uw toepassingen door het maken van een Azure Active Directory B2C-tenant met behulp van de Azure-portal.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/19/2018
ms.author: davidmu
ms.openlocfilehash: 04f3dbbe461bfe0f07b6930a92bdd8a721e55098
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296097"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Zelfstudie: Een Azure Active Directory B2C-tenant maken

Voordat u uw toepassingen kunnen communiceren met Azure Active Directory (Azure AD) B2C, moeten ze worden geregistreerd in een tenant die u beheert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant te koppelen aan uw abonnement

U leert hoe een toepassing registreren in de volgende zelfstudie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

1. Kies **maken van een resource** in de linkerbovenhoek van de Azure portal.
2. Zoek in het zoekvak boven de lijst van resources in Azure Marketplace, en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
3. Kies **maken van een nieuwe Azure AD B2C-Tenant**, voer een naam van de organisatie en een initiële domeinnaam die wordt gebruikt in de naam van de tenant, selecteert u het land en klik vervolgens op **maken**. Zorg dat van het land van de tenant omdat deze later niet meer wijzigen.

    ![Een tenant maken](./media/tutorial-create-tenant/create-tenant.png)

    In dit voorbeeld is de naam van de tenant contoso0522Tenant.onmicrosoft.com

Als u wilt beginnen met het beheren van uw nieuwe tenant, klikt u op het woord **hier** aanduiding **Klik hier, voor het beheren van uw nieuwe map**. U ziet een **oplossen** bericht moet u uw abonnement koppelen aan de nieuwe tenant. 

## <a name="link-your-tenant-to-your-subscription"></a>Uw tenant te koppelen aan uw abonnement

U moet uw Azure AD B2C-tenant koppelen aan uw Azure-abonnement inschakelen van alle functionaliteit en betaalt voor gebruikskosten. Als u niet uw tenant aan uw abonnement koppelt, werkt uw toepassingen niet correct.

1. Zorg ervoor dat u de map waarin het abonnement dat u koppelen aan de nieuwe tenant wilt door het overschakelen van de map in de rechterbovenhoek van de Azure portal.

    ![Schakelen tussen mappen](./media/tutorial-create-tenant/switch-directories.png)

    En vervolgens de map waarin u uw abonnement te selecteren.

    ![Map selecteren](./media/tutorial-create-tenant/select-directory.png)

2. Kies **maken van een resource** in de linkerbovenhoek rechterbovenhoek van de Azure portal.
3. Zoek in het zoekvak boven de lijst van resources in Azure Marketplace, en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
4. Kies **koppeling een bestaande Azure AD B2C-Tenant met mijn Azure-abonnement**, selecteert u de tenant die u hebt gemaakt, selecteer uw abonnement, voer *myContosoTenantRG* accepteren om de naam van de resourcegroep, de locatie en klik vervolgens op **maken**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant te koppelen aan uw abonnement

> [!div class="nextstepaction"]
> [Een webtoepassing om te verifiëren met accounts inschakelen](active-directory-b2c-tutorials-web-app.md)
---
title: 'Zelfstudie: een Azure Active Directory B2C-tenant maken | Microsoft Docs'
description: Informatie over het voorbereiden voor het registreren van uw toepassingen door het maken van een Azure Active Directory B2C-tenant met behulp van de Azure portal.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fe86541ed64d780c094eeb761053c76028b38862
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063605"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Zelfstudie: Een Azure Active Directory B2C-tenant maken

Voordat u uw toepassingen kunnen communiceren met Azure Active Directory (Azure AD) B2C, moeten deze worden geregistreerd in een tenant die u beheert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

Leert u hoe u een toepassing registreren in de volgende zelfstudie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

1. Kies **een resource maken** in de linkerbovenhoek van Azure portal.
2. Zoek in het zoekvak boven de lijst van Azure Marketplace-resources, en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
3. Kies **maken van een nieuwe Azure AD B2C-Tenant**, voer een naam van de organisatie en de initiële domeinnaam die wordt gebruikt in de naam van de tenant, selecteert u het land en klik vervolgens op **maken**. Zorg dat van het land van de tenant omdat deze later niet meer wijzigen.

    ![Een tenant maken](./media/tutorial-create-tenant/create-tenant.png)

    In dit voorbeeld is de naam van de tenant contoso0522Tenant.onmicrosoft.com

Als u wilt beginnen met het beheren van uw nieuwe tenant, klikt u op het woord **hier** aanduiding **Klik hier voor het beheren van uw nieuwe adreslijst**. U ziet een **oplossen** bericht weergegeven dat u wilt uw abonnement koppelen aan de nieuwe tenant. 

## <a name="link-your-tenant-to-your-subscription"></a>Uw tenant koppelt aan uw abonnement

U moet uw Azure AD B2C-tenant koppelen aan uw Azure-abonnement om te beschikken over alle functionaliteit en te betalen voor gebruikskosten. Als u niet uw tenant aan uw abonnement koppelt, werkt uw toepassingen niet correct.

Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen. 

![Overschakelen naar de Azure AD B2C-tenant](./media/tutorial-create-tenant/switch-directories.png)

1. Kies **een resource maken** in de linkerbovenhoek van de linkerbovenhoek van de Azure-portal.
2. Zoek in het zoekvak boven de lijst van Azure Marketplace-resources, en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
3. Kies **koppeling een bestaande Azure AD B2C-Tenant aan mijn Azure-abonnement**, selecteert u de tenant die u hebt gemaakt, selecteert u uw abonnement, voer *myContosoTenantRG* accepteren om de naam van de resourcegroep, de locatie en klik vervolgens op **maken**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

> [!div class="nextstepaction"]
> [Een webtoepassing te verifiëren met accounts inschakelen](active-directory-b2c-tutorials-web-app.md)
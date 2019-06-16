---
title: 'Zelfstudie: een Azure Active Directory B2C-tenant maken'
description: Informatie over het voorbereiden voor het registreren van uw toepassingen door het maken van een Azure Active Directory B2C-tenant met behulp van de Azure portal.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 711b9152f9f3fa1b3573e39d1950f18b628c268a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056314"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Zelfstudie: Een Azure Active Directory B2C-tenant maken

Voordat u uw toepassingen kunnen communiceren met Azure Active Directory (Azure AD) B2C, moeten deze worden geregistreerd in een tenant die u beheert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

Leert u hoe u een toepassing registreren in de volgende zelfstudie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map waarin u uw abonnement. Klik op de **map- en abonnementsfilter** in het bovenste menu Selecteer vervolgens de map waarin u uw abonnement. Deze map wijkt af van het account waarmee uw Azure AD B2C-tenant bevat.

    ![Schakel over naar de abonnementsmap](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Kies **een resource maken** in de linkerbovenhoek van Azure portal.
4. Zoek en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
5. Kies **maken van een nieuwe Azure AD B2C-Tenant** en voer een naam van de organisatie en de initiële domeinnaam. Selecteer het land/de regio (deze kan niet later worden gewijzigd), en klik vervolgens op **maken**.

    De initiële domeinnaam wordt gebruikt als onderdeel van de naam van uw tenant. In dit voorbeeld wordt de tenantnaam van de is *contoso0926Tenant.onmicrosoft.com*:

    ![Een tenant maken](./media/tutorial-create-tenant/create-tenant.PNG)

6. Op de **nieuwe B2C-Tenant maken of koppelen met bestaande Tenant** pagina, kies **koppeling een bestaande Azure AD B2C-Tenant aan mijn Azure-abonnement**.

    Selecteer de tenant die u hebt gemaakt en selecteer uw abonnement.

    Selecteer voor de resourcegroep **nieuw**. Voer een naam voor de resourcegroep die wordt bevatten van de tenant, selecteert u de locatie en klik vervolgens op **maken**.
1. Als u wilt gaan met deze nieuwe tenant, zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en het kiezen van de map waarvan deze deel uitmaakt.

    Als u niet eerst uw nieuwe Azure B2C-tenant in de lijst ziet, het browservenster vernieuwen, en selecteer vervolgens de **map- en abonnementsfilter** opnieuw in het bovenste menu.

    ![Overschakelen naar de directory-tenant](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

Hierna leert u hoe u een web-App registreren in uw nieuwe tenant.

> [!div class="nextstepaction"]
> [Registreren van uw toepassingen >](tutorial-register-applications.md)

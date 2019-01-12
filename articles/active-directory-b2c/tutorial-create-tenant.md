---
title: 'Zelfstudie: een Azure Active Directory B2C-tenant maken | Microsoft Docs'
description: Informatie over het voorbereiden voor het registreren van uw toepassingen door het maken van een Azure Active Directory B2C-tenant met behulp van de Azure portal.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 982932f538f13ac3cb8f842a32f9439f717d971f
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247772"
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
2. Zorg ervoor dat u van de map waarin u uw abonnement gebruikmaakt door te klikken op de **map- en abonnementsfilter** in het bovenste menu en het kiezen van de map waarvan deze deel uitmaakt. Deze map is anders dan die het bevat uw Azure AD B2C-tenant.

    ![Schakel over naar de abonnementsmap](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Kies **een resource maken** in de linkerbovenhoek van Azure portal.
4. Zoek en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
5. Kies **maken van een nieuwe Azure AD B2C-Tenant**, voer een naam van de organisatie en de initiële domeinnaam die wordt gebruikt in de naam van de tenant, selecteert u het land (deze kan niet later worden gewijzigd) en klik vervolgens op **maken**.

    ![Een tenant maken](./media/tutorial-create-tenant/create-tenant.png)

    In dit voorbeeld is de naam van de tenant contoso0926Tenant.onmicrosoft.com

6. Op de **nieuwe B2C-Tenant maken of een koppeling naar een bestaande Tenant** pagina, kies **koppeling een bestaande Azure AD B2C-Tenant aan mijn Azure-abonnement**, selecteert u de tenant die u hebt gemaakt, wordt uw abonnement te selecteren en vervolgens Klik op **nieuw**.
7. Voer een naam voor de resourcegroep die wordt bevatten van de tenant, selecteert u de locatie en klik vervolgens op **maken**.
8. Als u wilt gaan met deze nieuwe tenant, zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en het kiezen van de map waarvan deze deel uitmaakt.

    ![Overschakelen naar de directory-tenant](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

> [!div class="nextstepaction"]
> [Uw toepassingen registreren](tutorial-register-applications.md)
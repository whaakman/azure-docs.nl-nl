---
title: Een galerie-app toevoegen-Azure Active Directory | Microsoft Docs
description: Meer informatie over het toevoegen van een app uit de Azure AD-galerie aan uw Azure-bedrijfs toepassingen.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 06/18/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1bdf275bbafa9c46cfc4577ac2843da0be74c7ef
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477272"
---
# <a name="add-a-gallery-app-to-your-azure-ad-organization"></a>Een galerie-app toevoegen aan uw Azure AD-organisatie

Azure Active Directory (Azure AD) bevat een galerie met duizenden vooraf geïntegreerde toepassingen die zijn ingeschakeld met eenmalige aanmelding in de onderneming. In dit artikel worden de algemene stappen beschreven voor het toevoegen van een app uit de galerie aan uw Azure AD-organisatie.

> [!IMPORTANT]
> Controleer eerst op uw app in de [lijst met zelf studies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/). U vindt waarschijnlijk stapsgewijze richt lijnen voor het toevoegen en configureren van de galerie-app die u wilt toevoegen.

## <a name="add-a-gallery-application"></a>Een galerie toepassing toevoegen

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

1. In de [Azure Portal](https://portal.azure.com), in het navigatie venster aan de linkerkant, selecteert u **Azure Active Directory**.

1. Selecteer in  het deel venster Azure Active Directory **bedrijfs toepassingen**.

    ![Bedrijfstoepassingen openen](media/add-application-portal/open-enterprise-apps.png)

1. Selecteer **Nieuwe toepassing**.

    ![Nieuwe toepassing](media/add-application-portal/new-application.png)

1. Voer onder **toevoegen uit de galerie**in het zoekvak de naam in van de toepassing die u wilt toevoegen. 

    ![Zoeken op naam of categorie](media/add-application-portal/categories.png)

1. Selecteer de toepassing in de resultaten.

1. Beschrijving In het formulier toepassingsspecifiek kunt u de naam van de toepassing bewerken zodat deze overeenkomt met de behoeften van uw organisatie.

1. Selecteer **Toevoegen**. De **overzichts** pagina van de toepassing wordt geopend.

## <a name="configure-user-sign-in-properties"></a>Eigenschappen voor gebruikersaanmelding configureren

1. Selecteer **Eigenschappen** om het deel venster Eigenschappen te openen om het te bewerken.

    ![Eigenschappen venster bewerken](media/add-application-portal/edit-properties.png)

1. Stel de volgende opties in om te bepalen hoe gebruikers die zijn toegewezen aan of niet zijn toegewezen aan de toepassing, zich kunnen aanmelden bij de toepassing en of een gebruiker de toepassing kan zien in het toegangs venster.

    - Met **Ingeschakeld voor gebruikers voor aanmelden** wordt bepaald of gebruikers die zijn toegewezen aan de toepassing, zich kunnen aanmelden.
    - **Gebruikers toewijzing vereist** bepaalt of gebruikers die niet zijn toegewezen aan de toepassing zich kunnen aanmelden.
    - Met **Zichtbaar voor gebruiker** wordt bepaald of gebruikers die zijn toegewezen aan een app, deze kunnen zien in het toegangsvenster en het startprogramma voor O365.

      Gedrag voor **toegewezen** gebruikers:

       | Instellingen voor toepassingseigenschappen | | | Toegewezen gebruikerservaring | |
       |---|---|---|---|---|
       | Ingeschakeld voor gebruikers voor aanmelden? | Gebruikerstoewijzing vereist? | Zichtbaar voor gebruiker? | Kunnen toegewezen gebruikers zich aanmelden? | Kunnen toegewezen gebruikers de toepassing zien?* |
       | ja | ja | ja | ja | ja  |
       | ja | ja | nee  | ja | nee   |
       | ja | nee  | ja | ja | ja  |
       | ja | nee  | nee  | ja | nee   |
       | nee  | ja | ja | nee  | nee   |
       | nee  | ja | nee  | nee  | nee   |
       | nee  | nee  | ja | nee  | nee   |
       | nee  | nee  | nee  | nee  | nee   |

      Gedrag voor **niet-toegewezen** gebruikers:

       | Instellingen voor toepassingseigenschappen | | | Niet-toegewezen gebruikerservaring | |
       |---|---|---|---|---|
       | Ingeschakeld voor gebruikers om zich aan te melden? | Gebruikerstoewijzing vereist? | Zichtbaar voor gebruiker? | Kunnen niet-toegewezen gebruikers zich aanmelden? | Kunnen niet-toegewezen gebruikers de toepassing zien?* |
       | ja | ja | ja | nee  | nee   |
       | ja | ja | nee  | nee  | nee   |
       | ja | nee  | ja | ja | nee   |
       | ja | nee  | nee  | ja | nee   |
       | nee  | ja | ja | nee  | nee   |
       | nee  | ja | nee  | nee  | nee   |
       | nee  | nee  | ja | nee  | nee   |
       | nee  | nee  | nee  | nee  | nee   |

     *Kan de gebruiker de toepassing zien in het toegangsvenster en het startprogramma voor Office 365-apps?

1. Als u een aangepast logo wilt gebruiken, maakt u een logo van 215 x 215 pixels en slaat u het op in PNG-indeling. Blader vervolgens naar uw logo en upload het.

    ![Het logo wijzigen](media/add-application-portal/change-logo.png)

1. Wanneer u klaar bent, selecteert u **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Nu u de toepassing aan uw Azure AD-organisatie hebt toegevoegd, [kiest](what-is-single-sign-on.md#choosing-a-single-sign-on-method) u de methode voor eenmalige aanmelding die u wilt gebruiken en raadpleegt u het juiste artikel hieronder:

- [Eenmalige aanmelding op basis van SAML configureren](configure-single-sign-on-non-gallery-applications.md)
- [Eenmalige aanmelding met een wacht woord configureren](configure-password-single-sign-on-non-gallery-applications.md)
- [Gekoppelde aanmelding configureren](configure-linked-sign-on.md)


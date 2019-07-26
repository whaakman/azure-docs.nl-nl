---
title: Een app toevoegen aan uw Azure Active Directory-tenant | Microsoft Docs
description: In deze quickstart wordt de Azure-portal gebruikt om een galerietoepassing toe te voegen aan uw Azure AD-tenant (Azure Active Directory).
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 04/09/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 466660a1e064ef41eb330b36107dbdcb1d097498
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477307"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Quickstart: een toepassing toevoegen aan uw Azure Active Directory-tenant

Azure AD (Azure Active Directory) heeft een galerie met duizenden vooraf geïntegreerde toepassingen. Sommige toepassingen die worden gebruikt in uw organisatie, bevinden zich waarschijnlijk in de galerie. In deze quickstart wordt de Azure-portal gebruikt om een galerietoepassing toe te voegen aan uw Azure AD-tenant (Azure Active Directory).

Nadat een toepassing is toegevoegd aan de Azure AD-tenant, kunt u:

- Gebruikers toegang tot de toepassing beheren met beleid voor voorwaardelijke toegang.
- Gebruikers configureren voor eenmalige aanmelding bij de toepassing via hun Azure AD-accounts.

## <a name="before-you-begin"></a>Voordat u begint

Als u een toepassing wilt toevoegen aan uw tenant, hebt u het volgende nodig:

- Een Azure AD-abonnement
- Een abonnement met eenmalige aanmelding voor de toepassing

Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

Als u de stappen in deze zelfstudie wilt testen, raden we u aan om een niet-productieomgeving te gebruiken. Als u niet beschikt over een niet-productieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Een toepassing toevoegen aan uw Azure AD-tenant

Ga als volgt te werk om een galerietoepassing toe te voegen aan de Azure AD-tenant:

1. In de [Azure Portal](https://portal.azure.com), in het navigatie venster aan de linkerkant, selecteert u **Azure Active Directory**.
1. Selecteer in  het deel venster Azure Active Directory **bedrijfs toepassingen**.
1. Het deel venster **alle toepassingen** wordt geopend, waarin een wille keurig voor beeld van de toepassingen in uw Azure AD-Tenant wordt weer gegeven. Selecteer **nieuwe toepassing** boven aan het deel venster **alle toepassingen** om een galerie-app toe te voegen aan uw Tenant.

    ![Selecteer nieuwe toepassing om een galerie-app toe te voegen aan uw Tenant](media/add-application-portal/new-application.png)

1. In het deel venster **Categorieën** ziet u pictogrammen onder het gebied **Aanbevolen toepassingen** die een wille keurig voor beeld zijn van Galerie toepassingen. Als u meer toepassingen wilt zien, kunt u **meer weer geven** selecteren, maar dit wordt niet aangeraden om op deze manier te zoeken omdat er duizenden toepassingen in de galerie zijn.

    ![Zoeken naar een app op naam of categorie](media/add-application-portal/categories.png)

1. Als u wilt zoeken naar een toepassing, voert u onder **toevoegen in de galerie**de naam in van de toepassing die u wilt toevoegen. Selecteer de toepassing uit de resultaten en selecteer **toevoegen**. In het volgende voorbeeld ziet u het formulier **App toevoegen** dat wordt weergegeven nadat u hebt gezocht naar github.com.

    ![Laat zien hoe u een toepassing kunt toevoegen vanuit de galerie](media/add-application-portal/add-an-application.png)

1. In het toepassingsformulier kunt u de eigenschapsinformatie wijzigen. U kunt bijvoorbeeld de naam van de toepassing bewerken zodat deze past bij de behoeften in uw organisatie. In dit voorbeeld wordt de naam **GitHub-test** gebruikt.
1. Wanneer u klaar bent met het aanbrengen van wijzigingen in de eigenschappen, selecteert u **toevoegen**.
1. Er wordt nu een pagina Aan de slag weergegeven met de opties voor het configureren van de toepassing voor uw organisatie.

U bent klaar met het toevoegen van uw toepassing. Neem gerust even pauze. In de volgende secties ziet u hoe u het logo kunt wijzigen en andere eigenschappen voor de toepassing kunt bewerken.

## <a name="find-your-azure-ad-tenant-application"></a>Uw Azure AD-tenanttoepassing zoeken

Laten we aannemen dat u even weg bent gegaan en dat u nu verdergaat met het configureren van de toepassing. Het eerste wat u moet doen, is uw toepassing te vinden.

1. In de **[Azure Portal](https://portal.azure.com)** , in het navigatie venster aan de linkerkant, selecteert u **Azure Active Directory**.
1. Selecteer in  het deel venster Azure Active Directory **bedrijfs toepassingen**.
1. Selecteer **alle toepassingen**in het vervolg keuzemenu **toepassings type** en selecteer vervolgens **Toep assen**. Zie [Tenanttoepassingen weergeven](view-applications-portal.md) voor meer informatie over de weergaveopties.
1. U ziet nu een lijst met alle toepassingen in de Azure AD-tenant. De lijst is een willekeurig voorbeeld. Als u meer toepassingen wilt zien, selecteert u meer dan een of meer keren **weer geven** .
1. Als u snel een toepassing in uw Tenant wilt vinden, voert u de naam van de toepassing in het zoekvak in en selecteert u **Toep assen**. In dit voor beeld wordt gezocht naar de GitHub-test toepassing die u eerder hebt toegevoegd.

    ![Laat zien hoe u een toepassing kunt vinden met het zoekvak](media/add-application-portal/find-application.png)

## <a name="configure-user-sign-in-properties"></a>Eigenschappen voor gebruikersaanmelding configureren

Nu u de toepassing hebt gevonden, kunt u deze openen en toepassings eigenschappen configureren.

De eigenschappen van de toepassing bewerken:

1. Selecteer de toepassing om deze te openen.
1. Selecteer **Eigenschappen** om het deel venster Eigenschappen te openen om het te bewerken.

    ![Het eigenschappen scherm en bewerk bare app-eigenschappen weer geven](media/add-application-portal/edit-properties.png)

1. Neem even de tijd om de opties voor aanmelden goed te bestuderen. De opties bepalen hoe gebruikers die zijn toegewezen aan of niet toegewezen aan de toepassing, zich kunnen aanmelden bij de toepassing. En de opties bepalen ook of een gebruiker de toepassing kan zien in het toegangs venster.

    - Met **Ingeschakeld voor gebruikers voor aanmelden** wordt bepaald of gebruikers die zijn toegewezen aan de toepassing, zich kunnen aanmelden.
    - **Gebruikers toewijzing vereist** bepaalt of gebruikers die niet zijn toegewezen aan de toepassing zich kunnen aanmelden.
    - Met **Zichtbaar voor gebruiker** wordt bepaald of gebruikers die zijn toegewezen aan een app, deze kunnen zien in het toegangsvenster en het startprogramma voor O365.

1. Gebruik de volgende tabellen om u te helpen bij het kiezen van de beste opties voor uw behoeften.

   - Gedrag voor **toegewezen** gebruikers:

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

   - Gedrag voor **niet-toegewezen** gebruikers:

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

## <a name="use-a-custom-logo"></a>Een aangepast logo gebruiken

Ga als volgt te werk om een aangepast logo te gebruiken:

1. Maak een logo van 215 x 215 pixels en sla dit op in de PNG-indeling.
1. Omdat u uw toepassing al hebt gevonden, selecteert u de toepassing.
1. Selecteer **Eigenschappen**in het linkerdeel venster.
1. Upload het logo.
1. Wanneer u klaar bent, selecteert u **Opslaan**.

    ![Laat zien hoe u het logo kunt wijzigen op de eigenschappen pagina van de app](media/add-application-portal/change-logo.png)

## <a name="next-steps"></a>Volgende stappen

Nu u de toepassing aan uw Azure AD-organisatie hebt toegevoegd, [kiest](what-is-single-sign-on.md#choosing-a-single-sign-on-method) u de methode voor eenmalige aanmelding die u wilt gebruiken en raadpleegt u het juiste artikel hieronder:

- [Eenmalige aanmelding op basis van SAML configureren](configure-single-sign-on-non-gallery-applications.md)
- [Eenmalige aanmelding met een wacht woord configureren](configure-password-single-sign-on-non-gallery-applications.md)
- [Gekoppelde aanmelding configureren](configure-linked-sign-on.md)

---
title: Een app toevoegen aan uw Azure Active Directory-tenant | Microsoft Docs
description: In deze quickstart wordt de Azure-portal gebruikt om een galerietoepassing toe te voegen aan uw Azure AD-tenant (Azure Active Directory).
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/24/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: af30568d3ab35b35d2a0e0bef00921debb1ea451
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917801"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Quickstart: een toepassing toevoegen aan uw Azure Active Directory-tenant

Azure AD (Azure Active Directory) heeft een galerie met duizenden vooraf geïntegreerde toepassingen. Sommige toepassingen die worden gebruikt in uw organisatie, bevinden zich waarschijnlijk in de galerie. In deze quickstart wordt de Azure-portal gebruikt om een galerietoepassing toe te voegen aan uw Azure AD-tenant (Azure Active Directory).

Nadat een toepassing is toegevoegd aan de Azure AD-tenant, kunt u:

- Gebruikerstoegang tot de toepassing beheren met een voorwaardelijk toepassingsbeleid.
- Gebruikers configureren voor eenmalige aanmelding bij de toepassing via hun Azure AD-accounts.

## <a name="before-you-begin"></a>Voordat u begint

Als u een toepassing wilt toevoegen aan uw tenant, hebt u het volgende nodig:

- Een Azure AD-abonnement
- Een abonnement met eenmalige aanmelding voor de toepassing

Meld u bij de [Azure-portal](https://portal.azure.com) aan als globale beheerder van uw Azure AD-tenant, als beheerder van cloudtoepassingen of als toepassingsbeheerder.

Als u de stappen in deze zelfstudie wilt testen, raden we u aan om een niet-productieomgeving te gebruiken. Als u niet beschikt over een niet-productieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Een toepassing toevoegen aan uw Azure AD-tenant

Ga als volgt te werk om een galerietoepassing toe te voegen aan de Azure AD-tenant:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

2. In de **Azure Active Directory** venster **bedrijfstoepassingen**.

    ![Bedrijfstoepassingen openen](media/add-application-portal/open-enterprise-apps.png)

3. De **alle toepassingen** deelvenster geopend om weer te geven van een steekproef van de toepassingen in uw Azure AD-tenant. Selecteer **nieuwe toepassing** aan de bovenkant van de **alle toepassingen** deelvenster.

    ![Nieuwe toepassing](media/add-application-portal/new-application.png)

4. In de **categorieën** deelvenster ziet u pictogrammen onder de **aanbevolen toepassingen** gebied die zijn van een steekproef van galerietoepassingen.  Als u wilt meer toepassingen zien, kunt u selecteren **meer weergeven**. Maar u kunt beter geen zoeken op deze manier omdat er duizenden toepassingen in de galerie zijn.

    ![Zoeken op naam of categorie](media/add-application-portal/categories.png)

5. Om te zoeken naar een toepassing, onder **toevoegen vanuit de galerie**, voer de naam van de toepassing die u wilt toevoegen. Selecteer de toepassing uit de resultaten en selecteer **toevoegen**. In het volgende voorbeeld ziet u het formulier **App toevoegen** dat wordt weergegeven nadat u hebt gezocht naar github.com.

    ![Een toepassing toevoegen](media/add-application-portal/add-an-application.png)

6. In het toepassingsformulier kunt u de eigenschapsinformatie wijzigen. U kunt bijvoorbeeld de naam van de toepassing bewerken zodat deze past bij de behoeften in uw organisatie. In dit voorbeeld wordt de naam **GitHub-test** gebruikt.

7. Wanneer u klaar bent met het aanbrengen van wijzigingen in de eigenschappen, selecteert u **toevoegen**.

8. Er wordt nu een pagina Aan de slag weergegeven met de opties voor het configureren van de toepassing voor uw organisatie.

U klaar bent met het toevoegen van uw toepassing. Neem gerust even pauze. In de volgende secties ziet u hoe u het logo kunt wijzigen en andere eigenschappen voor de toepassing kunt bewerken.

## <a name="find-your-azure-ad-tenant-application"></a>Uw Azure AD-tenanttoepassing zoeken

Laten we aannemen dat u even weg bent gegaan en dat u nu verdergaat met het configureren van de toepassing. Het eerste wat te doen is uw toepassing vinden.

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatievenster aan de linkerkant **Azure Active Directory**.

2. In de **Azure Active Directory** venster **bedrijfstoepassingen**.

3. Uit de **toepassingstype** vervolgkeuzelijst in het menu **alle toepassingen**, en selecteer vervolgens **toepassen**. Zie [Tenanttoepassingen weergeven](view-applications-portal.md) voor meer informatie over de weergaveopties.

4. U ziet nu een lijst met alle toepassingen in de Azure AD-tenant. De lijst is een willekeurig voorbeeld. Meer toepassingen Selecteer **meer weergeven** een of meer keer.

5. Als u wilt snel een toepassing in uw tenant kunt vinden, van de toepassing invoeren in het zoekvak en selecteer **toepassen**. In dit voorbeeld wordt gezocht naar de GitHub-testtoepassing eerder toegevoegd.

    ![Zoeken naar een toepassing](media/add-application-portal/find-application.png)


## <a name="configure-user-sign-in-properties"></a>Eigenschappen voor gebruikersaanmelding configureren

Nu dat u de toepassing hebt gevonden, kunt u deze kunt openen en configureren van eigenschappen voor de toepassing.

De toepassingseigenschappen bewerken:

1. Selecteer de toepassing om deze te openen.
2. Selecteer **eigenschappen** te openen voor het bewerken van het deelvenster met eigenschappen.

    ![Het deelvenster met eigenschappen bewerken](media/add-application-portal/edit-properties.png)

3. Neem even de tijd om de opties voor aanmelden goed te bestuderen. De opties te bepalen hoe gebruikers die zijn toegewezen of niet-toegewezen aan de toepassing kunnen zich aanmelden bij de toepassing. En de opties bepalen ook als een gebruiker de toepassing in het toegangsvenster kunt zien.

    - Met **Ingeschakeld voor gebruikers voor aanmelden** wordt bepaald of gebruikers die zijn toegewezen aan de toepassing, zich kunnen aanmelden.
    - **Gebruikerstoewijzing vereist** bepaalt of gebruikers die niet zijn toegewezen aan de toepassing kunnen aanmelden.
    - Met **Zichtbaar voor gebruiker** wordt bepaald of gebruikers die zijn toegewezen aan een app, deze kunnen zien in het toegangsvenster en het startprogramma voor O365.

4. Gebruik de volgende tabellen kunt u het beste opties kiezen voor uw behoeften.

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
       | Ingeschakeld voor gebruikers zich aanmelden? | Gebruikerstoewijzing vereist? | Zichtbaar voor gebruiker? | Kunnen niet-toegewezen gebruikers zich aanmelden? | Kunnen niet-toegewezen gebruikers de toepassing zien?* |
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
2. Omdat u uw toepassing al hebt gevonden, selecteert u de toepassing.
2. Selecteer in het linkerdeelvenster **eigenschappen**.
4. Upload het logo.
5. Wanneer u klaar bent, selecteert u **opslaan**.

    ![Het logo wijzigen](media/add-application-portal/change-logo.png)


## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u een galerietoepassing kunt toevoegen aan uw Azure AD-tenant. U hebt geleerd hoe u de eigenschappen van een toepassing kunt bewerken.

Nu bent u klaar om de toepassing te configureren voor eenmalige aanmelding.

> [!div class="nextstepaction"]
> [Eenmalige aanmelding configureren](configure-single-sign-on-portal.md)



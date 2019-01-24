---
title: 'Zelfstudie: Azure Active Directory-integratie met G Suite | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.openlocfilehash: f37873fdbc26e1c82b6c1d9e6be8132641d94773
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809170"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Zelfstudie: Azure Active Directory-integratie met G Suite

In deze zelfstudie leert u hoe u G Suite integreert met Azure AD (Azure Active Directory).
De integratie van G Suite met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot G Suite.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij G Suite (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie wilt configureren met G Suite, hebt u de volgende items nodig:

- Een Azure AD-abonnement
- Een abonnement waarvoor G Suite-eenmalige aanmelding is ingeschakeld
- Een Google Apps-abonnement of Google Cloud Platform-abonnement.

> [!NOTE]
> Als u de stappen in deze zelfstudie wilt testen, is het raadzaam om niet de productieomgeving te gebruiken. Dit document is gemaakt met behulp van de nieuwe ervaring voor eenmalige aanmelding. Als u nog steeds gebruikmaakt van de oude versie, ziet de installatie er anders uit. U kunt de nieuwe ervaring inschakelen in de instellingen voor eenmalige aanmelding van de G Suite-toepassing. Ga naar **Azure AD, bedrijfstoepassingen**, selecteer **G Suite**, selecteer **Eenmalige aanmelding** en klik vervolgens op **Nieuwe ervaring uitproberen**.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **V: Biedt deze integratie ondersteuning voor Google Cloud Platform-integratie voor eenmalige aanmelding met Azure AD?**

    A: Ja. Google Cloud Platform en Google Apps delen hetzelfde verificatieplatform. Voor de GCP-integratie moet u dus de eenmalige aanmelding met Google Apps configureren.

2. **V: Zijn Chromebooks en andere Chrome-apparaten compatibel met eenmalige aanmelding in Azure AD?**
  
    A: Ja, gebruikers kunnen zich met behulp van hun Azure AD-referenties aanmelden bij hun Chromebook-apparaten. Bekijk dit [G Suite-ondersteuningsartikel](https://support.google.com/chrome/a/answer/6060880) voor informatie over waarom gebruikers mogelijk tweemaal om hun referenties wordt gevraagd.

3. **V: Kunnen gebruikers, als ik eenmalige aanmelding inschakel, hun Azure AD-referenties gebruiken om zich aan te melden bij Google-producten, zoals Google Classroom, GMail, Google Drive, YouTube, enzovoort?**

    A: Ja, afhankelijk van [de G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) die u inschakelt of uitschakelt voor uw organisatie.

4. **V: Kan ik eenmalige aanmelding voor alleen een subset G Suite-gebruikers inschakelen?**

    A: Nee, als u eenmalige aanmelding inschakelt, moeten onmiddellijk alle G Suite-gebruikers zich verifiëren met hun Azure AD-referenties. Omdat G Suite geen ondersteuning biedt voor meerdere id-providers, kan de id-provider voor uw G Suite-omgeving Azure AD of Google zijn, maar niet beide tegelijkertijd.

5. **V: Wordt een gebruiker die is aangemeld via Windows, automatisch gevraagd om zich te verifiëren bij G Suite, zonder dat om een wachtwoord wordt gevraagd?**

    A: Er zijn twee opties waarbij dit scenario zich voordoet. Ten eerste kan een gebruiker zich via [Deelnemen aan Azure Active Directory](../device-management-introduction.md) aanmelden bij een Windows 10-apparaat. Gebruikers kunnen zich ook aanmelden bij Windows-apparaten die zijn toegevoegd aan een domein in on-premises Active Directory waarvoor eenmalige aanmelding bij Azure AD is ingeschakeld via een [AD FS-implementatie (Active Directory Federation Services)](../hybrid/plan-connect-user-signin.md). Voor beide opties moet u de stappen in de volgende zelfstudie uitvoeren om eenmalige aanmelding tussen Azure AD en G Suite in te schakelen.

6. **V: Wat moet ik doen als ik een foutbericht over ongeldige e-mail krijg?**

    A: Voor deze installatie is het e-mailkenmerk vereist voor de gebruikers om zich te kunnen aanmelden. Dit kenmerk kan niet handmatig worden ingesteld.

    Het e-mailkenmerk wordt automatisch ingevuld voor elke gebruiker met een geldige Exchange-licentie. Als e-mail niet is ingeschakeld voor een gebruiker, wordt dit foutbericht ontvangen omdat de toepassing dit kenmerk moet ophalen om toegang te kunnen verlenen.

    U kunt met een beheerdersaccount naar portal.office.com gaan, in het beheercentrum klikken op Facturering > Abonnementen, uw Office 365-abonnement selecteren en vervolgens klikken op Toewijzen aan gebruikers. Selecteer de gebruikers van wie u het abonnement wilt controleren, en klik in het rechterdeelvenster op Licenties bewerken.

    Nadat de O365-licentie is toegewezen, kan het enkele minuten duren voordat deze is toegepast. Hierna wordt het kenmerk user.mail automatisch ingevuld en is het probleem zeer waarschijnlijk opgelost.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* G Suite biedt ondersteuning voor met **SP** geïnitieerde eenmalige aanmelding
* G Suite biedt ondersteuning voor **[automatisch inrichten van gebruikers](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)**

## <a name="adding-g-suite-from-the-gallery"></a>G Suite toevoegen uit de galerie

Als u de integratie van G Suite in Azure AD wilt configureren, moet u G Suite vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om G Suite toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **G Suite** in het zoekvak, selecteer **G Suite** in het paneel voor resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![G Suite in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u Azure AD-eenmalige aanmelding met G Suite op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tot stand is gebracht tussen een Azure AD-gebruiker en de gerelateerde gebruiker in G Suite.

Als u Azure AD-eenmalige aanmelding met G Suite wilt configureren en testen, voltooit u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[G Suite-eenmalige aanmelding configureren](#configure-g-suite-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wil configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[G Suite-testgebruiker maken](#create-g-suite-test-user)**: als u een tegenhanger van Britta Simon wilt in G Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Als u Azure AD-eenmalige aanmelding met G Suite wilt configureren en testen, voert u de volgende stappen uit:

1. Selecteer in de [Azure-portal](https://portal.azure.com/), op de integratiepagina van de **G Suite**-toepassing, de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over G Suite-domein en URL’s voor eenmalige aanmelding](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `https://google.com` |
    | `https://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met het [Ondersteuningsteam voor G Suite](https://www.google.com/contact/) om deze waarden op te halen.

5. In de G Suite-toepassing worden de SAML-beweringen in een specifieke indeling verwacht. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-tokenkenmerken. In de volgende schermopname ziet u een voorbeeld hiervan. De standaardwaarde van **Unieke gebruikers-id** is **user.userprincipalname**, maar in G Suite wordt verwacht dat aan deze waarde het e-mailadres van de gebruiker is toegewezen. Hiervoor kunt u het kenmerk **user.mail** in de lijst gebruiken of de juiste kenmerkwaarde op basis van uw organisatieconfiguratie.

    ![image](common/edit-attribute.png)

6. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Naam | Bronkenmerk |
    | ---------------| --------------- |
    | Unieke gebruikers-id | User.mail |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. In het gedeelte **G Suite instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-g-suite-single-sign-on"></a>G Suite-eenmalige aanmelding configureren

1. Open een nieuw tabblad in uw browser en meld u met uw beheerdersaccount aan bij de [G Suite-beheerconsole](https://admin.google.com/).

2. Klik op **Beveiliging**. Als u de koppeling niet ziet, is deze mogelijk verborgen in het menu **Meer besturingselementen** onder aan het scherm.

    ![Klik op Beveiliging.][10]

3. Klik op de pagina **Beveiliging** op **Eenmalige aanmelding (SSO) instellen.**

    ![Klik op Eenmalige aanmelding.][11]

4. Voer de volgende configuratiewijzigingen uit:

    ![Eenmalige aanmelding configureren][12]

    a. Selecteer **Installatie van eenmalige aanmelding met id-provider van derden**.

    b. Plak in het veld **URL van aanmeldingspagina** in G Suite de waarde van  **Aanmeldings-URL**  die u hebt gekopieerd in de Azure-portal.

    c. Plak in het veld **URL van afmeldingspagina** in G Suite de waarde van  **Afmeldings-URL**  die u hebt gekopieerd in de Azure-portal.

    d. Plak in het veld **URL voor wachtwoord wijzigen** in G Suite de waarde van  **URL voor wachtwoord wijzigen**  die u hebt gekopieerd in de Azure-portal.

    e. Upload in G Suite voor **Verificatiecertificaat** het certificaat dat u hebt gedownload in de Azure-portal.

    f. Selecteer **Een domein-specifieke verlener gebruiken**.

    g. Klik op **Wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van Azure-eenmalige aanmelding door haar toegang te verlenen tot G Suite.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **G Suite**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **G Suite** in de lijst met toepassingen.

    ![De koppeling G Suite in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-g-suite-test-user"></a>G Suite-testgebruiker maken

Het doel van dit gedeelte is om een gebruiker met de naam Britta Simon te maken in G Suite-software. G Suite biedt ondersteuning voor automatisch inrichten, wat standaard is ingeschakeld. U hoeft geen handelingen uit te voeren in dit gedeelte. Als een gebruiker nog niet bestaat in G Suite-software, wordt een nieuwe gebruiker gemaakt als u toegang probeert te krijgen tot G Suite-software.

> [!NOTE]
> Zorg ervoor dat uw gebruiker al bestaat in G Suite, als inrichten in Azure AD niet is ingeschakeld vóór het testen van eenmalige aanmelding.

> [!NOTE]
> Neem contact op met het [ondersteuningsteam van Google](https://www.google.com/contact/) als u handmatig een gebruiker moet maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel G Suite in het toegangsvenster klikt, wordt u automatisch aangemeld bij het G Suite-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Inrichten van gebruikers configureren](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-provisioning-tutorial)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png
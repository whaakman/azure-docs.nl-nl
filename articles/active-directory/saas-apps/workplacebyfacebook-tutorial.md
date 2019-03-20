---
title: 'Zelfstudie: Azure Active Directory-integratie met Workplace by Facebook | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Workplace by Facebook configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27173c8beeecf2be43e80f59df8907952734c06b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57900876"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Zelfstudie: Azure Active Directory-integratie met Workplace by Facebook

In deze zelfstudie leert u hoe u Workplace by Facebook integreert met Azure Active Directory (Azure AD).
De integratie van Workplace by Facebook met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Workplace by Facebook.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Workplace by Facebook (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Workplace by Facebook:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op Workplace by Facebook met eenmalige aanmelding (SSO) ingeschakeld

> [!NOTE]
> Als u de stappen in deze zelfstudie wilt testen, is het raadzaam om niet de  productieomgeving te gebruiken.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

> [!NOTE]
> Facebook heeft twee producten, Workplace Standard (gratis) en Workplace Premium (betaald). Elke tenant van Workplace Premium kan gratis en zonder licenties SCIM- en SSO-integratie configureren. SSO en SCIM zijn niet beschikbaar in exemplaren van Workplace Standard.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Workplace by Facebook ondersteunt door **SP** geïnitieerde SSO
* Workplace by Facebook ondersteunt **just-in-time inrichten**
* Workplace by Facebook ondersteunt **[automatisch inrichten van gebruikers](workplacebyfacebook-provisioning-tutorial.md)**

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Workplace by Facebook toevoegen vanuit de galerie

Om de integratie van Workplace by Facebook in Azure AD te configureren, moet u Workplace by Facebook vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om Workplace by Facebook vanuit de galerie toe te voegen:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Workplace by Facebook** in het zoekvak, selecteer **Workplace by Facebook** in de lijst met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Workplace by Facebook in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u eenmalige aanmelding van Azure AD met Workplace by Facebook configureren en testen op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Workplace by Facebook tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met Workplace by Facebook, moet u de volgende procedures uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Workplace by Facebook configureren](#configure-workplace-by-facebook-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Workplace by Facebook maken](#create-workplace-by-facebook-test-user)**: een tegenhanger voor Britta Simon maken in Workplace by Facebook die wordt gekoppeld aan de Azure AD-voorstelling van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met Workplace by Facebook:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina met de integratie van de toepassing **Workplace by Facebook** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij Workplace by Facebook](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<instancename>.facebook.com`

    b. In het tekstvak **Id (entiteits-id)** typt u een URL met het volgende patroon: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Dit zijn niet de echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Zie de pagina Verificatie van het dashboard Workplace Company voor de juiste waarden voor uw Workplace-community.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In het gedeelte **Set up Workplace by Facebook** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-workplace-by-facebook-single-sign-on"></a>Eenmalige aanmelding voor Workplace by Facebook configureren

1. Meld u in een ander browservenster als beheerder aan bij de bedrijfssite van Workplace by Facebook.
  
    > [!NOTE]
    > Als onderdeel van het SAML-verificatieproces kan Workplace queryreeksen van maximaal 2,5 kB gebruiken om parameters door te geven aan Azure AD.

2. Ga in het **deelvenster Beheer** naar het tabblad **Beveiliging**.

    ![Deelvenster Beheer](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure01.png)

3. Selecteer op het tabblad **Verificatie** de optie **Eenmalige aanmelding (SSO)** en voer de volgende stappen uit:

    ![Tabblad Verificatie](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure02.png)

    a. Plak in het tekstvak **SAM-URL** de waarde van **Aanmeldings-URL**, die u hebt gekopieerd uit de Azure-portal.

    b. Plak in het tekstvak **URI SAML-verlener** de waarde van **Azure AD-id**, die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het tekstvak **Omleiden SAML-afmelding** (optioneel) de waarde van **Afmeldings-URL**, die u hebt gekopieerd uit de Azure-portal.

    d. Open in Kladblok het **met Base 64 gecodeerde certificaat** dat u hebt gedownload uit de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze vervolgens in het tekstvak **SAML-certificaat**.

    e. Kopiëren de **doelgroep URL** voor uw exemplaar en plak deze in **id (entiteits-ID)** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    f. Kopiëren de **ontvanger URL** voor uw exemplaar en plak deze in **aanmeldings-URL** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    g. Blader naar de onderkant van het gedeelte en klik op de knop **SSO testen**. Er verschijnt een pop-upvenster met de aanmeldingspagina van Azure AD. Voer uw referenties op de gebruikelijke manier in om te verifiëren.

    **Problemen oplossen:** Controleer of het e-mailadres dat wordt geretourneerd door Azure AD hetzelfde is als het Workplace-account waarbij u bent aangemeld.

    h. Zodra de test is voltooid, bladert u naar de onderkant van de pagina en klikt u op de knop **Opslaan**.

    i. Alle gebruikers van Workplace zien nu voortaan de Azure AD-aanmeldingspagina voor verificatie.

4. **Omleiden SAML-afmelding (optioneel)** -

    U kunt eventueel een URL voor SAML afmelding configureren, die kan worden gebruikt om te verwijzen naar de afmeldingspagina van Azure AD. Als deze instelling is ingeschakeld en geconfigureerd, wordt de gebruiker niet meer omgeleid naar de afmeldingspagina van Workplace. In plaats daarvan wordt de gebruiker omgeleid naar de URL die is opgegeven in het tekstvak Omleiden SAML-afmelding.

### <a name="configuring-reauthentication-frequency"></a>Frequentie voor hernieuwde verificatie configureren

U kunt instellen dat Workplace elke dag, drie dagen, week, twee weken, maanden of nooit vraagt om een SAML-controle.

> [!NOTE]
> De minimumwaarde voor de SAML-controle voor mobiele toepassingen is ingesteld op één week.

U kunt ook afdwingen dat SAML opnieuw wordt ingesteld voor alle gebruikers. Dit kan met de knop: Nu SAML-verificatie vereisen voor alle gebruikers.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Workplace by Facebook.

1. Selecteer **Bedrijfstoepassingen** in de Azure-portal, selecteer **Alle toepassingen** en selecteer vervolgens **Workplace by Facebook**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Workplace by Facebook** in de lijst met toepassingen.

    ![De koppeling Workplace by Facebook in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-workplace-by-facebook-test-user"></a>Testgebruiker voor Workplace by Facebook maken

In dit gedeelte wordt er een gebruiker met de naam Britta Simon gemaakt in Workplace by Facebook. Workplace by Facebook ondersteunt just-in-time inrichten. Deze functie is standaard ingeschakeld.

U hoeft geen handelingen uit te voeren in dit gedeelte. Als er geen gebruiker bestaat in Workplace by Facebook, wordt er een nieuwe gemaakt wanneer u probeert toegang te krijgen tot Workplace by Facebook.

>[!Note]
>Als u handmatig een gebruiker wilt maken, neemt u contact op met [het ondersteuningsteam van Workplace by Facebook](https://workplace.fb.com/faq/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Workplace by Facebook klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van in het toegangsvenster waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Inrichten van gebruikers configureren](workplacebyfacebook-provisioning-tutorial.md)

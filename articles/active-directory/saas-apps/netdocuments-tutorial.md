---
title: 'Zelfstudie: Azure Active Directory-integratie met NetDocuments | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en NetDocuments.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 50850fc9372976382724d6bbc98be9a8d10a0655
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850176"
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Zelfstudie: Azure Active Directory-integratie met NetDocuments

In deze zelfstudie leert u hoe u NetDocuments integreren met Azure Active Directory (Azure AD).
NetDocuments integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot NetDocuments heeft.
* U kunt uw gebruikers worden automatisch aangemeld NetDocuments (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met NetDocuments, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* NetDocuments eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor NetDocuments **SP** gestart door SSO

## <a name="adding-netdocuments-from-the-gallery"></a>NetDocuments uit de galerie toe te voegen

Voor het configureren van de integratie van NetDocuments in Azure AD, moet u NetDocuments uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen NetDocuments uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **NetDocuments**, selecteer **NetDocuments** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![NetDocuments in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met NetDocuments op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in NetDocuments tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met NetDocuments, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding NetDocuments](#configure-netdocuments-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker NetDocuments](#create-netdocuments-test-user)**  : als u wilt een equivalent van Britta Simon in NetDocuments die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met NetDocuments, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **NetDocuments** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![NetDocuments domein en URL's, eenmalige aanmelding informatie](common/sp-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmelding-URL en antwoord-URL. Neem contact op met [NetDocuments Client ondersteuningsteam](https://support.netdocuments.com/hc/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **NetDocuments instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-netdocuments-single-sign-on"></a>NetDocuments voor eenmalige aanmelding configureren

1. Meld u in een ander browservenster in uw bedrijf NetDocuments site als beheerder.

2. Ga naar **Admin**.

3. Klik op **toevoegen en verwijderen van gebruikers en groepen**.
   
    ![Opslagplaats](./media/netdocuments-tutorial/ic795047.png "opslagplaats")

4. Klik op **geavanceerde verificatieopties configureren**.
    
    ![Configureer geavanceerde verificatieopties](./media/netdocuments-tutorial/ic795048.png "geavanceerde verificatieopties configureren")

5. Op de **federatieve identiteit** dialoogvenster, voer de volgende stappen uit:
   
    ![Federatieve Identitty](./media/netdocuments-tutorial/ic795049.png "federatieve Identitty")
   
    a. Als **federatieve identiteit servertype**, selecteer **Active Directory Federation Services**.
   
    b. Klik op **bestand kiezen**, het uploaden van het gedownloade metagegevensbestand dat u hebt gedownload vanuit Azure portal.
   
    c. Klik op **OK**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan NetDocuments.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **NetDocuments**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **NetDocuments**.

    ![De koppeling NetDocuments in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-netdocuments-test-user"></a>NetDocuments testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich aanmelden bij NetDocuments, moeten ze worden ingericht voor NetDocuments.  
In het geval van NetDocuments is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Sing aan bij uw **NetDocuments** bedrijf site als administrator.

2. Klik in het menu bovenaan op **Admin**.
   
    ![Admin](./media/netdocuments-tutorial/ic795051.png "Admin")

3. Klik op **toevoegen en verwijderen van gebruikers en groepen**.
   
    ![Opslagplaats](./media/netdocuments-tutorial/ic795047.png "opslagplaats")

4. In de **e-mailadres** tekstvak typt u het e-mailadres van een geldige Azure Active Directory-account dat u wilt inrichten, en klik vervolgens op **gebruiker toevoegen**.
   
    ![E-mailadres](./media/netdocuments-tutorial/ic795053.png "e-mailadres")
   
    >[!NOTE]
    >De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze geactiveerd wordt. U kunt elke andere NetDocuments gebruiker account hulpmiddelen voor het maken of API's geleverd door NetDocuments voor het inrichten van Azure Active Directory-gebruikersaccounts.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel NetDocuments in het toegangsvenster, moet u worden automatisch aangemeld bij de NetDocuments waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


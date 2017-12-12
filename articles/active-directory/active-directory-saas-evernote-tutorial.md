---
title: 'Zelfstudie: Azure Active Directory-integratie met Evernote | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Evernote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: c0dda5288d7985669afef5464bf2f9a34f9e8e8c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Zelfstudie: Azure Active Directory-integratie met Evernote

In deze zelfstudie leert u hoe Evernote integreren met Azure Active Directory (Azure AD).

Evernote integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Evernote heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Evernote (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Evernote, moet u de volgende items:

- Een Azure AD-abonnement
- Een Evernote eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Evernote uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-evernote-from-the-gallery"></a>Evernote uit de galerie toevoegen
Voor het configureren van de integratie van Evernote in Azure AD, moet u Evernote uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Evernote uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Evernote**, selecteer **Evernote** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Evernote in de lijst met resultaten](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Evernote op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Evernote is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Evernote tot stand worden gebracht.

Wijs in Evernote, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Evernote, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Evernote](#create-an-evernote-test-user)**  - Evernote die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Evernote configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Evernote, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Evernote** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. Op de **Evernote domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de IDP geïnitieerd modus:

    ![URL's en Evernote domein eenmalige aanmelding informatie](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    In de **id** textbox, typ de URL:`https://www.evernote.com/saml2`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Evernote domein eenmalige aanmelding informatie](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    In de **aanmelden URL** textbox, typ de URL:`https://www.evernote.com/Login.action`   

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. Op de **Evernote configuratie** sectie, klikt u op **configureren Evernote** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Evernote configuratie](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. In een ander browservenster, meld u bij uw bedrijf Evernote site als beheerder.

9. Ga naar **Admin-Console**

    ![-Beheerconsole](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. Van de **Admin-Console**, gaat u naar **'Security'** en selecteer **' Single Sign-On'**

    ![SSO-instelling](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. Configureer de volgende waarden:

    ![Certificaat-instelling](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **Eenmalige aanmelding inschakelen:** eenmalige aanmelding is standaard ingeschakeld (Klik op **uitschakelen Single Sign-on** verwijderen van de vereiste SSO)

    b. Plakken **SAML eenmalige aanmelding Service-URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **SAML HTTP-verzoek-URL** textbox.

    c. Het gedownloade certificaat openen vanuit Azure AD in Kladblok en kopieer de inhoud, met inbegrip van 'BEGIN CERTIFICATE' en 'END CERTIFICATE' en plak deze in de **X.509-certificaat** textbox. 

    d.Click **wijzigingen opslaan**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-evernote-test-user"></a>Een testgebruiker Evernote maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Evernote, moeten ze worden ingericht in Evernote.  
In het geval van Evernote is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Evernote site als beheerder.

2. Klik op de **Admin-Console**.

    ![-Beheerconsole](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. Van de **Admin-Console**, gaat u naar **toevoegen van gebruikers**.

    ![Voeg testgebruiker](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. **Teamleden toevoegen** in de **e** textbox, typ het e-mailadres van gebruikersaccount en klik op **uit te nodigen.**

    ![Voeg testgebruiker](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. Nadat de uitnodiging is verzonden, ontvangt de houder van Azure Active Directory-account een e-mail met de uitnodiging accepteren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Evernote.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Evernote, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Evernote**.

    ![De koppeling Evernote in de lijst met toepassingen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Evernote in het deelvenster toegang, u moet ophalen aangemeld bij uw toepassing Evernote. U zult logboekregistratie in als een organisatie account maar moet zich aanmelden met uw persoonlijke account. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png


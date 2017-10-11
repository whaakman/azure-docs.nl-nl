---
title: 'Zelfstudie: Azure Active Directory-integratie met Velpic SAML | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5325f3cca00167e6b7b687509ce43435447ad2f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Zelfstudie: Azure Active Directory-integratie met Velpic SAML

In deze zelfstudie leert u hoe Velpic SAML integreren met Azure Active Directory (Azure AD).

Velpic SAML integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Velpic SAML heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Velpic SAML (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Velpic SAML, moet u de volgende items:

- Een Azure AD-abonnement
- Een Velpic SAML-eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Velpic SAML uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-velpic-saml-from-the-gallery"></a>Velpic SAML uit de galerie toevoegen
Voor het configureren van de integratie van Velpic SAML in Azure AD, moet u Velpic SAML uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Velpic SAML uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Velpic SAML**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. Selecteer in het deelvenster resultaten **Velpic SAML**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Velpic SAML op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Velpic SAML is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante Velpic SAML-gebruiker worden gemaakt.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Velpic SAML.

Om te configureren en testen van Azure AD eenmalige aanmelding met Velpic SAML, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Velpic SAML](#creating-a-velpic-saml-test-user)**  - Velpic SAML die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding configureren in uw toepassing Velpic SAML.

**Voor het configureren van Azure AD eenmalige aanmelding met Velpic SAML, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **Velpic SAML** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Voer de gegevens in de **Velpic SAML-domein en de URL's** sectie -

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. In de **aanmeldings-URL** textbox, typ de waarde als:`https://<sub-domain>.velpicsaml.net`

    b. In de **id** textbox, plak de **'Eenmalige op URL'** waarde`https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Houd er rekening mee dat de aanmelding URL worden geleverd door het team Velpic SAML en id-waarde beschikbaar zijn bij het configureren van de SSO-invoegtoepassing Velpic SAML-zijde. U wilt dat de waarde van de Velpic SAML toepassingspagina kopiëren en plak dit hier.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Klik in de configuratiesectie Velpic SAML Velpic SAML configureren om configureren aanmelding venster te openen. Kopieer de SAML-entiteit-ID uit de sectie eenvoudig kunt raadplegen.

7. In een ander browservenster, meld u bij uw bedrijf Velpic SAML site als beheerder.

8. Klik op **beheren** tabblad en Ga naar **integratie** sectie waarin u klikt u wilt op **Plugins** knop nieuwe invoegtoepassing maken voor aanmelden.

    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Klik op de **'Add invoegtoepassing'** knop.
    
    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Klik op de **SAML** -tegel in de pagina invoegtoepassing toevoegen.
    
    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Voer de naam van de nieuwe SAML-invoegtoepassing en klik op de **'Add-** knop.

    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Voer de gegevens als volgt:

    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. In de **naam** textbox, typ de naam van SAML-invoegtoepassing.

    b. In de **URL-verlener** textbox plakken de **SAML entiteit-ID** u gekopieerd uit de **eenmalige aanmelding configureren** venster van de Azure portal.

    c. In de **Provider metagegevens Config** het Metadata XML-bestand dat u hebt gedownload van Azure-portal uploaden.

    d. U kunt er ook voor kiezen om in te schakelen SAML in tijd inrichten doordat de **'Automatische Maak nieuwe gebruikers'** selectievakje. Als een gebruiker niet in Velpic bestaat en deze markering niet is ingeschakeld, mislukt de aanmelding van Azure. Als de vlag is ingeschakeld. de gebruiker automatisch worden ingericht in Velpic op het moment van de aanmelding. 

    e. Kopieer de **eenmalige op URL** uit de tekst vak en plak deze in de Azure portal.
    
    f. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Een testgebruiker Velpic SAML maken

Deze stap is meestal niet vereist als de toepassing alleen bij tijd gebruikersaanvragen ondersteunt. Als de automatische gebruikersinrichting niet is ingeschakeld kan vervolgens maken van een handmatige gebruikersaccount worden uitgevoerd zoals hieronder wordt beschreven.

Meld u aan bij uw site van het bedrijf Velpic SAML als beheerder en voert u de volgende stappen:
    
1. Klik op het tabblad beheren en gaat u naar de sectie gebruikers en klik vervolgens op de knop Nieuw gebruikers toe te voegen.

    ![gebruiker toevoegen](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. Op de **'Nieuwe gebruiker maken'** dialoogvenster pagina, de volgende stappen uitvoeren.

    ![Gebruiker](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. In de **voornaam** textbox typenaam de eerste van Britta Simon.

    b. In de **achternaam** textbox, typt u de achternaam van Britta Simon.

    c. In de **gebruikersnaam** textbox, typ de gebruikersnaam van Britta Simon.

    d. In de **e** textbox, typ de e-mailadres van Britta Simon account.

    e. De rest van de informatie is optioneel en kan ingevuld als die nodig zijn.
    
    f. Klik op **OPSLAAN**.  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Velpic SAML.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Velpic SAML, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Velpic SAML**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

1. Als u op de tegel Velpic SAML in het deelvenster toegang, krijgt u de aanmeldingspagina van Velpic SAML-toepassing. U ziet de **'Aanmelden met Azure AD'** knop op de aanmeldingspagina.

    ![Invoegtoepassing](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Klik op de **'Aanmelden met Azure AD'** knop aanmelden bij Velpic met behulp van uw Azure AD-account.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png


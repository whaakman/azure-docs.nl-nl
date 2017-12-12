---
title: 'Zelfstudie: Azure Active Directory-integratie met Trakstar | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Trakstar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 411cb8c3-95c6-4138-acf2-ffc7f663e89a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 72ba6b9f02bc36b46628394afdb29acc6a2fa332
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trakstar"></a>Zelfstudie: Azure Active Directory-integratie met Trakstar

In deze zelfstudie leert u hoe Trakstar integreren met Azure Active Directory (Azure AD).

Trakstar integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Trakstar heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Trakstar (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Trakstar, moet u de volgende items:

- Een Azure AD-abonnement
- Een Trakstar eenmalige aanmelding ingeschakeld abonnement
    - Eenmalige aanmelding is een betaald functie in Trakstar. Inschakelen voor uw organisatie bereiken [Trakstar Client ondersteuningsteam](mailto:support@trakstar.com).

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Trakstar uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-trakstar-from-the-gallery"></a>Trakstar uit de galerie toevoegen
Voor het configureren van de integratie van Trakstar in Azure AD, moet u Trakstar uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Trakstar uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Trakstar**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_search.png)

5. Selecteer in het deelvenster resultaten **Trakstar**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Trakstar op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Trakstar is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Trakstar tot stand worden gebracht.

Wijs in Trakstar, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Trakstar, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Trakstar](#creating-a-trakstar-test-user)**  - Trakstar die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Trakstar configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Trakstar, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Trakstar** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_samlbase.png)

3. Op de **Trakstar domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_url.png)

    a. In de **aanmeldings-URL** textbox-, Kopieer de waarde gevonden in de **ACS (consument) URL** binnen Trakstar (instellingen > verificatie en eenmalige aanmelding) in de indeling:`https://app.trakstar.com/auth/saml/callback?namespace=<YOUR_NAMESPACE>`

    b. In de **id** textbox laat de standaardwaarde:`https://app.trakstar.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Meld u bij Trakstar als een beheerder om deze waarden.
    > Als u het tabblad 'Verificatie en SSO' in de instellingen niet ziet, hebt u mogelijk geen de functie
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_general_400.png)

6. Op de **Trakstar configuratie** sectie, klikt u op **configureren Trakstar** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_configure.png) 

7. Eenmalige aanmelding configureren op **Trakstar** zijde, moet u zich aanmelden als beheerder en voer de **certificaat (Base64)**, **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On-Service URL**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-trakstar-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-trakstar-test-user"></a>Een testgebruiker Trakstar maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Trakstar genoemd.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Trakstar.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Trakstar, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Trakstar**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.  
Als u op de tegel Trakstar in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Trakstar. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_203.png


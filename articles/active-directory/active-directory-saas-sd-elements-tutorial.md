---
title: 'Zelfstudie: Azure Active Directory-integratie met SD-elementen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SD-elementen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 796f4d1b679c06be8677cd07f52ce305a7bc1ef8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Zelfstudie: Azure Active Directory-integratie met SD-elementen

In deze zelfstudie leert u hoe SD-elementen integreren met Azure Active Directory (Azure AD).

SD-elementen integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SD-elementen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SD-elementen (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SD-elementen, moet u de volgende items:

- Een Azure AD-abonnement
- Een SD-elementen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Het toevoegen van SD-elementen in de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sd-elements-from-the-gallery"></a>Het toevoegen van SD-elementen in de galerie
Voor het configureren van de integratie van SD-elementen in Azure AD, moet u SD-elementen in de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SD-elementen in de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SD-elementen**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. Selecteer in het deelvenster resultaten **SD-elementen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met SD-elementen die op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SD-elementen in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SD-elementen tot stand worden gebracht.

In de SD-elementen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SD-elementen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SD-elementen testgebruiker](#creating-a-sd-elements-test-user)**  - SD-elementen die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing SD-elementen.

**Voor het configureren van Azure AD eenmalige aanmelding met SD-elementen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SD-elementen** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. Op de **SD-elementen domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [SD-elementen ondersteuningsteam](mailto:support@sdelements.com) ophalen van deze waarden.

4. De SAML-asserties verwacht toepassing SD-elementen in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **'Gebruikerskenmerk'** tabblad van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren: 

    | Kenmerknaam | Waarde kenmerk |
    | --- | --- |
    | e-mailen |User.mail |
    | Voornaam |User.givenName |
    | Achternaam |User.surname |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.
 
6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. Op de **SD-elementen configuratie** sectie, klikt u op **SD-elementen configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. Als u eenmalige aanmelding is ingeschakeld, neem contact op met uw [SD-elementen ondersteuningsteam](mailto:support@sdelements.com) en geeft u het gedownloade certificaatbestand. 

10. In een ander browservenster aanmelding in uw tenant, SD-elementen als beheerder.

11. Klik in het menu bovenaan op **System**, en vervolgens **Single Sign-on**. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. Op de **instellingen voor eenmalige aanmelding** dialoogvenster de volgende stappen uitvoeren:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Als **SSO Type**, selecteer **SAML**.
   
    b. In de **identiteit Provider entiteit-ID** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 
   
    c. In de **Provider Single Sign-On Identiteitsservice** textbox, plakt u de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal. 
   
    d. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sd-elements-test-user"></a>Maken van een testgebruiker SD-elementen

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in SD-elementen. In het geval van SD-elementen is elementen van de SD-gebruikers te maken een handmatige taak.

**Als u wilt Britta Simon in SD-elementen maken, moet u de volgende stappen uitvoeren:**

1. In een browservenster geopend, aanmelding bij uw bedrijf SD-elementen site als beheerder.

2. Klik in het menu bovenaan op **Gebruikersbeheer**, en vervolgens **gebruikers**.
   
    ![Maken van een testgebruiker SD-elementen](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. Klik op **nieuwe gebruiker toevoegen**.
   
    ![Maken van een testgebruiker SD-elementen](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. Op de **nieuwe gebruiker toevoegen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Maken van een testgebruiker SD-elementen](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. In de **e** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .
   
    b. In de **voornaam** textbox, voer de voornaam van de gebruiker zoals **Britta**.
   
    c. In de **achternaam** textbox, geef de achternaam van de gebruiker zoals **Simon**.
   
    d. Als **rol**, selecteer **gebruiker**. 
   
    e. Klik op **gebruiker maken**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SD-elementen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar SD-elementen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SD-elementen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
  
Als u op de tegel SD-elementen in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SD-elementen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png


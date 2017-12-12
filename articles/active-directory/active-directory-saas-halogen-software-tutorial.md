---
title: 'Zelfstudie: Azure Active Directory-integratie met halogeen Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en halogeen Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af9c9d4b9bd562169bfe6c9158254beec5c3e1e7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Zelfstudie: Azure Active Directory-integratie met halogeen Software

In deze zelfstudie leert u hoe halogeen Software integreren met Azure Active Directory (Azure AD).

Halogeen Software integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot halogeen Software heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij halogeen Software (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met halogeen Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een halogeen Software eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Halogeen Software uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-halogen-software-from-the-gallery"></a>Halogeen Software uit de galerie toevoegen

Voor het configureren van de integratie van halogeen Software in Azure AD, moet u halogeen Software uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen halogeen Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **halogeen Software**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. Selecteer in het deelvenster resultaten **halogeen Software**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met halogeen Software op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in halogeen Software is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in halogeen Software worden gemaakt.

Wijs in halogeen Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met halogeen Software, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker halogeen Software](#creating-a-halogen-software-test-user)**  - halogeen-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing halogeen Software configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met halogeen Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **halogeen Software** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. Op de **halogeen Software domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://global.hgncloud.com/<companyname>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [halogeen Software Client ondersteuningsteam](https://support.halogensoftware.com/) ophalen van deze waarden. 
 


4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelding bij uw **halogeen Software** toepassing als beheerder.

7. Klik op de **opties** tabblad. 
   
    ![Wat is Azure AD Connect?][12]

8. Klik in het navigatiedeelvenster links op **SAML-configuratie**. 
   
    ![Wat is Azure AD Connect?][13]

9. Op de **SAML-configuratie** pagina, voert u de volgende stappen uit: 

    ![Wat is Azure AD Connect?][14]

     a. Als **unieke id**, selecteer **NameID**.

     b. Als **unieke id gerelateerd aan**, selecteer **gebruikersnaam**.
  
     c. Als u wilt uw van het gedownloade metagegevensbestand uploadt, klikt u op **Bladeren** om het bestand te selecteren en vervolgens **-bestand uploaden**.
 
     d. Om de configuratie te testen, klikt u op **uitvoeren testen**. 
    
    >[!NOTE]
    >U moet wachten op het bericht '*de SAML-test is voltooid. Sluit dit venster*'. Sluit het browservenster is geopend. De **SAML inschakelen** selectievakje is alleen ingeschakeld als de test is voltooid. 
     
     e. Selecteer **SAML inschakelen**.
    
     f. Klik op **wijzigingen opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox typenaam als **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-halogen-software-test-user"></a>Een testgebruiker halogeen Software maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in halogeen Software.

**Voer de volgende stappen uit voor het maken van een gebruiker Britta Simon aangeroepen in halogeen Software:**

1. Meld u aan bij uw **halogeen Software** toepassing als beheerder.

2. Klik op de **gebruiker Center** tabblad en klik vervolgens op **gebruiker maken**.
   
    ![Wat is Azure AD Connect?][300]  

3. Op de **nieuwe gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Wat is Azure AD Connect?][301]

    a. In de **voornaam** textbox type voornaam van de gebruiker zoals **Britta**.
    
    b. In de **achternaam** textbox type achternaam van de gebruiker zoals **Simon**. 

    c. In de **gebruikersnaam** textbox type **Britta Simon**, de gebruikersnaam, zoals in de Azure-portal.

    d. In de **wachtwoord** textbox, typ een wachtwoord voor Britta.
    
    e. Klik op **Opslaan**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent tot halogeen Software gebruiken.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen halogeen Software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **halogeen Software**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel halogeen Software in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing halogeen Software.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png

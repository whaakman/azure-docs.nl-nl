---
title: 'Zelfstudie: Azure Active Directory-integratie met Veracode | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Veracode.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d49349c5ae08e67d91e30967f3644623211823ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Zelfstudie: Azure Active Directory-integratie met Veracode

In deze zelfstudie leert u hoe Veracode integreren met Azure Active Directory (Azure AD).

Veracode integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Veracode heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Veracode (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Veracode, moet u de volgende items:

- Een Azure AD-abonnement
- Een Veracode eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Veracode uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-veracode-from-the-gallery"></a>Veracode uit de galerie toevoegen
Voor het configureren van de integratie van Veracode in Azure AD, moet u Veracode uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Veracode uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Veracode**, selecteer **Veracode** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Veracode in de lijst met resultaten](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Veracode op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Veracode is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Veracode tot stand worden gebracht.

Wijs in Veracode, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Veracode, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Veracode](#create-a-veracode-test-user)**  - Veracode die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Veracode configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Veracode, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Veracode** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. Op de **Veracode domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren Veracode aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

    Uw toepassing Veracode verwacht de SAML-asserties in een specifieke indeling waarvoor u het toevoegen van aangepast kenmerktoewijzingen aan uw **saml-token kenmerken** configuratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Kenmerken](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "kenmerken")

6. Als u wilt de vereiste kenmerktoewijzingen toevoegen, moet u de volgende stappen uitvoeren:

    | Naam van kenmerk | De waarde van kenmerk |
    |--- |--- |
    | Voornaam |User.givenName |
    | Achternaam |User.surname |
    | E-mail |User.mail |
    
    a. Voor elke gegevensrij in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    
    ![Kenmerken](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "kenmerken")
    
    ![Kenmerken](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "kenmerken")
    
    b. In de **kenmerknaam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. In de **kenmerkwaarde** textbox, selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. Op de **Veracode configuratie** sectie, klikt u op **configureren Veracode** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** van de **Naslaggids punt.**

    ![Veracode configuratie](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. In een ander browservenster, meld u bij uw bedrijf Veracode site als beheerder.

10. Klik in het menu bovenaan op **instellingen**, en klik vervolgens op **Admin**.
   
    ![Beheer](./media/active-directory-saas-veracode-tutorial/ic802911.png "beheer")

11. Klik op de **SAML** tabblad.

12. In de **organisatie SAML-instellingen** sectie, voert u de volgende stappen uit:
   
    ![Beheer](./media/active-directory-saas-veracode-tutorial/ic802912.png "beheer")
   
    a.  In **verlener** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
    
    b. Als u wilt uploaden uw gedownloade certificaat vanuit Azure-portal, klikt u op **bestand kiezen**.
   
    c. Selecteer **Self-registratie inschakelen**.

13. In de **Self-instellingen voor statusregistratie** sectie, het uitvoeren van de volgende stappen uit en klik vervolgens op **opslaan**:
   
    ![Beheer](./media/active-directory-saas-veracode-tutorial/ic802913.png "beheer")
   
    a. Als **nieuwe gebruikersactivering**, selecteer **Nee activering vereist**.
   
    b. Als **gebruiker Gegevensupdates**, selecteer **voorkeur Veracode gebruikersgegevens**.
   
    c. Voor **Details van SAML-kenmerk**, selecteert u het volgende:
      * **Gebruikersrollen**
      * **Beleid beheerder**
      * **Revisor**
      * **Beveiliging Lead**
      * **Executive**
      * **Indienings**
      * **Maker**
      * **Alle scannen typen**
      * **Lidmaatschap van een team**
      * **Standaardteam**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-veracode-test-user"></a>Een testgebruiker Veracode maken
Om in te schakelen gebruikers van Azure AD aan te melden bij Veracode, moeten ze worden ingericht in Veracode. In het geval van Veracode is inrichting een geautomatiseerde taak. Er is geen actie-item voor u. Gebruikers worden automatisch gemaakt indien nodig tijdens de eerste eenmalige aanmelding poging.

> [!NOTE]
> U kunt andere Veracode gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Veracode voor het inrichten van Azure AD-gebruikersaccounts.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Veracode.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Veracode, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Veracode**.

    ![De koppeling Veracode in de lijst met toepassingen](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Veracode in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Veracode.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png


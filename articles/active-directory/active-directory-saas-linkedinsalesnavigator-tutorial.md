---
title: 'Zelfstudie: Azure Active Directory-integratie met LinkedInSalesNavigator | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LinkedInSalesNavigator.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: ef26a16e79d9c9b0654634960b57dc59827b2c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Zelfstudie: Azure Active Directory-integratie met LinkedIn verkoop Navigator

In deze zelfstudie leert u hoe LinkedIn verkoop Navigator integreren met Azure Active Directory (Azure AD).

LinkedIn verkoop Navigator integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LinkedIn verkoop Navigator heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LinkedIn verkoop Navigator (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u meer gedetailleerde informatie over de integratie van de SaaS-app met Azure AD, bladert u [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LinkedIn verkoop Navigator, moet u de volgende items:

- Een Azure AD-abonnement
- Een LinkedIn verkoop Navigator eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Vermijd het gebruik van uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LinkedIn verkoop Navigator uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>LinkedIn verkoop Navigator uit de galerie toevoegen
Voor het configureren van de integratie van LinkedIn verkoop Navigator in Azure AD, moet u LinkedIn verkoop Navigator uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LinkedIn verkoop Navigator uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **LinkedIn verkoop Navigator**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. Selecteer in het deelvenster resultaten **LinkedIn verkoop Navigator**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met LinkedIn verkoop Navigator op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in LinkedIn verkoop Navigator is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in LinkedIn verkoop Navigator tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in LinkedIn verkoop Navigator.

Om te configureren en testen van Azure AD eenmalige aanmelding met LinkedIn verkoop Navigator, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker LinkedIn verkoop Navigator](#creating-a-linkedin-sales-navigator-test-user)**  - LinkedIn verkoop Navigator die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing LinkedIn verkoop Navigator.

**Voor het configureren van Azure AD eenmalige aanmelding met LinkedIn verkoop Navigator, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LinkedIn verkoop Navigator** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster in **modus** Selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. In een ander browservenster aanmelding bij uw **LinkedIn verkoop Navigator** website als beheerder.

4. In **Accountcentrum**, klikt u op **globale instellingen** onder **instellingen**. Schakel ook **verkoop Navigator** uit de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Klik op **of Klik hier om te laden en afzonderlijke velden van het formulier kopiëren** en kopieer **entiteit-Id** en **Assertion Consumer Access (ACS) Url**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. Op de Azure-portal onder **LinkedIn verkoop Navigator domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. In de **id** textbox, voer de **entiteit-ID** gekopieerd uit LinkedIn-Portal 

    b. In de **antwoord-URL** textbox, voer de **Assertion Consumer Access (ACS) Url** gekopieerd uit LinkedIn-Portal

7. Controleer **weergeven geavanceerde instellingen voor URL**, als u wilt configureren van de toepassing in **SP** modus gestart.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

8. Uw **LinkedIn verkoop Navigator** toepassing de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken verwacht. De volgende Schermafbeelding toont een voorbeeld. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar LinkedIn verkoop Navigator verwacht met e-mailadres van de gebruiker worden toegewezen. U kunt **user.mail** kenmerk uit de lijst of gebruik de juiste kenmerkwaarde op basis van de organisatieconfiguratie van uw. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. In **gebruikerskenmerken** sectie, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** en kenmerken instellen. De gebruiker moet vier claims met de naam toe te voegen **e**, **afdeling**, **firstname**, en **lastname** en de waarde moet worden toegewezen met **user.mail**, **user.department**, **user.givenname**, en **user.surname** respectievelijk

    | Naam van kenmerk | De waarde van kenmerk |
    | --- | --- |    
    | E-mail| User.mail |
    | Afdeling| User.Department |
    | Voornaam| User.givenName |
    | Achternaam| User.surname |
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klik op **kenmerk toevoegen** om het dialoogvenster kenmerk te openen.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

10. Voer de volgende stappen uit op de **naam** kenmerk -

    a. Klik op het kenmerk openen de **kenmerk bewerken** venster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    b. Verwijder de URL-waarde van de **naamruimte**.
    
    c. Klik op **Ok** om op te slaan van de instelling.

11. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. Ga naar **LinkedIn beheerdersinstellingen** sectie. Klik op **uploaden XML-bestand** voor het uploaden van de Metadata-XML-bestand dat u hebt gedownload vanuit de Azure-portal.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. Klik op **op** eenmalige aanmelding inschakelen. Status SSO wordt gewijzigd van **niet verbonden** naar **verbonden**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Een testgebruiker LinkedIn verkoop Navigator maken

Toepassing van de Navigator gekoppelde verkoop ondersteunt Just in Time (Just in time) gebruikers inrichten en na verificatie gebruikers automatisch in de toepassing gemaakt worden. Activeren **automatisch toewijzen van licenties** een licentie toewijzen aan de gebruiker.
   
   ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan LinkedIn verkoop Navigator.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen LinkedIn verkoop Navigator, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **LinkedIn verkoop Navigator**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel LinkedIn verkoop Navigator in het deelvenster toegang, moet u naar organisatie pagina waar u de details van uw persoonlijke LinkedIn moet worden omgeleid. Uw persoonlijke account aan uw LinkedIn business-account worden gekoppeld. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png


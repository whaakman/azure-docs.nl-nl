---
title: 'Zelfstudie: Azure Active Directory-integratie met herkennen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en herkennen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 97d85183d0307c41a3b879d440d87a6fb0c53190
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Zelfstudie: Azure Active Directory-integratie met herkennen

In deze zelfstudie leert u hoe herkennen integreren met Azure Active Directory (Azure AD).

Herkennen integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot herkennen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij herkennen (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met herkennen, moet u de volgende items:

- Een Azure AD-abonnement
- Een herkennen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Herkennen in de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-recognize-from-the-gallery"></a>Herkennen in de galerie toevoegen
Voor het configureren van de integratie van herkennen in Azure AD, moet u herkennen in de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om te herkennen in de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **herkennen**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. Selecteer in het deelvenster resultaten **herkennen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met herkennen op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in herkennen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in herkennen tot stand worden gebracht.

Wijs in herkennen, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met herkennen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker herkennen](#creating-a-recognize-test-user)**  - herkennen die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing herkennen.

**Voor het configureren van Azure AD eenmalige aanmelding met herkennen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **herkennen** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. Op de **domein herkennen en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://recognizeapp.com/<your-domain>/saml/sso`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Client herkennen support team](mailto:support@recognizeapp.com) ophalen aanmeldings-URL en u de id-waarde kunt ophalen door het openen van metagegevens van de URL van de Provider van de sectie instellingen voor eenmalige aanmelding die verderop in de zelfstudie wordt beschreven. . 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. Op de **herkent Configuration** sectie, klikt u op **configureren herkennen** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. In een ander browservenster aanmelding bij uw tenant herkennen als beheerder.

8. Klik in de rechterbovenhoek op **Menu**. Ga naar **Admin bedrijf**.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. Klik in het navigatiedeelvenster links op **instellingen**.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. Voer de volgende stappen uit op **SSO instellingen** sectie.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    a. Als **eenmalige aanmelding inschakelen**, selecteer **ON**.

    b. In de **IDP entiteit-ID** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
    
    c. In de **doel-url voor eenmalige aanmelding** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
    
    d. In de **Slo-doel-url** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal. 
    
    e. Open uw gedownloade **certificaat (Base64)** bestand in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **certificaat** textbox.
    
    f. Klik op de **instellingen opslaan** knop. 

11. Naast de **SSO instellingen** sectie, Kopieer de URL onder **Service Provider metagegevens-url**.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. Open de **metagegevens-URL koppeling** onder een lege browser voor het downloaden van het metagegevensdocument. Vervolgens de value(entityID) EntityDescriptor uit het bestand kopiëren en plakken in **id** textbox in **sectie domein herkennen en URL's** op Azure-portal.
    
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-recognize-test-user"></a>Maken van een testgebruiker herkennen

Om in te schakelen gebruikers aan te melden bij het herkennen van Azure AD, moeten ze worden ingericht in herkennen. In het geval van herkennen is inrichting een handmatige taak.

Deze app biedt geen ondersteuning voor het inrichten van SCIM, maar heeft een andere gebruiker synchronisatie die gebruikers richt. 

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf herkennen site als beheerder.

2. Klik in de rechterbovenhoek op **Menu**. Ga naar **Admin bedrijf**.

3. Klik in het navigatiedeelvenster links op **instellingen**.

4. Voer de volgende stappen uit op **gebruiker Sync** sectie.
   
   ![Nieuwe gebruiker](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "nieuwe gebruiker")
   
   a. Als **synchroniseren ingeschakeld**, selecteer **ON**.
   
   b. Als **Kies sync provider**, selecteer **Microsoft / Office 365**.
   
   c. Klik op **Voer gebruiker synchronisatie**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan herkennen.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen herkennen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **herkennen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel herkennen in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing herkennen. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png


---
title: 'Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7542360f9c75786bf400553090ba0a891d9c2fcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud

In deze zelfstudie leert u hoe Lifesize Cloud integreren met Azure Active Directory (Azure AD).

Lifesize Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Lifesize Cloud heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Lifesize Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Lifesize Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Lifesize Cloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Lifesize Cloud uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Lifesize Cloud met Azure AD, moet u Lifesize Cloud uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Lifesize Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Lifesize Cloud**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. Selecteer in het deelvenster resultaten **Lifesize Cloud**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Lifesize Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de Lifesize Cloud is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Lifesize Cloud tot stand worden gebracht.

In de Lifesize Cloud, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Lifesize Cloud, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  - Lifesize Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Lifesize Cloud.

**Om eenmalige aanmelding Azure AD met Lifesize Cloud configureert, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Lifesize Cloud** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. Op de **Lifesize Cloud-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://login.lifesizecloud.com/ls/?acs`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://login.lifesizecloud.com/<companyname>`

     
4. Controleer **weergeven geavanceerde instellingen voor URL**, voer de volgende stap:    
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    In de **Relay-status** textbox, typ een URL met het volgende patroon volgen:`https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Houd er rekening mee dat deze niet de werkelijke waarden zijn. u hebt deze waarden bijwerken met de werkelijke aanmeldings-URL, Relay-status en -id. Neem contact op met [Lifesize Cloud Client ondersteuningsteam](https://www.lifesize.com/support) ophalen aanmeldings-URL, en id-waarden en u kunt benutten Relay status SSO-configuratie die verderop in de zelfstudie wordt uitgelegd.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. Op de **Lifesize Cloudconfiguratie** sectie, klikt u op **Lifesize Cloud configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. Ophalen van eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, aanmelding bij de toepassing Lifesize Cloud met beheerdersbevoegdheden.

8. Klik op uw naam in de rechterbovenhoek en klik vervolgens op de **geavanceerde instellingen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. In de geavanceerde instellingen en klik nu op de **SSO configuratie** koppeling. De configuratiepagina van eenmalige aanmelding voor uw exemplaar, wordt deze geopend.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Configureer nu de volgende waarden in de configuratie van de SSO-gebruikersinterface.    
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. In **identiteit Provider verlener** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.

    b.  In **aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    c. De base-64 gecodeerde certificaat openen in Kladblok van Azure portal hebt gedownload, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **X.509-certificaat** textbox.
  
    d. Toewijzingen voor het tekstvak Voornaam Voer de waarde als in het SAML-kenmerk **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. In de toewijzing van SAML-kenmerk voor de **achternaam** tekstvak voert u de waarde als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. In de toewijzing van SAML-kenmerk voor de **e** tekstvak voert u de waarde als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. Controleer de configuratie kunt u op de **Test** knop.
   
    >[!NOTE]
    >Voor het testen van geslaagde moet u de configuratiewizard kunt voltooien in Azure AD en ook toegang te bieden aan gebruikers of groepen die de test kunnen uitvoeren.

12. De eenmalige aanmelding inschakelen door te controleren op de **eenmalige aanmelding inschakelen** knop.

13. Klik nu op de **Update** knop zodat alle instellingen worden opgeslagen. Hierdoor wordt de waarde RelayState gegenereerd. Kopieer de waarde RelayState, die wordt gegenereerd in het tekstvak, plak deze in de **Relay status** textbox onder **Lifesize Cloud-domein en de URL's** sectie. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Een testgebruiker Lifesize Cloud maken

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in de Lifesize Cloud. Lifesize cloud biedt ondersteuning voor automatisch gebruikers inrichten. Na de geslaagde verificatie op Azure AD worden de gebruiker automatisch ingericht in de toepassing. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Lifesize Cloud.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Lifesize Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Lifesize Cloud**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Lifesize Cloud in het deelvenster toegang, krijgt u de aanmeldingspagina van Lifesize Cloud-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png


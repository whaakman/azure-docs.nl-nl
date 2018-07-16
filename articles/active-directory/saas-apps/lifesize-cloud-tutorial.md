---
title: 'Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b699714a2ab90fd0ad1c2f290681ccdae7aeb1ba
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052190"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud

In deze zelfstudie leert u hoe u Lifesize Cloud integreren met Azure Active Directory (Azure AD).

Lifesize Cloud integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Lifesize Cloud heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Lifesize Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Lifesize Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Lifesize Cloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Lifesize Cloud uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud uit de galerie toe te voegen
Voor het configureren van de integratie van Lifesize Cloud in Azure AD, moet u Lifesize Cloud uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Lifesize Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Lifesize Cloud**.

    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. Selecteer in het deelvenster resultaten **Lifesize Cloud**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met Lifesize Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Lifesize Cloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Lifesize Cloud tot stand worden gebracht.

In de Lifesize Cloud, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Lifesize Cloud, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  : als u wilt een equivalent van Britta Simon in Lifesize Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Lifesize Cloud.

**Voor het configureren van Azure AD eenmalige aanmelding met Lifesize Cloud, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Lifesize Cloud** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. Op de **Lifesize Cloud domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://login.lifesizecloud.com/ls/?acs`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://login.lifesizecloud.com/<companyname>`

     
4. Controleer **geavanceerde URL-instellingen weergeven**, de volgende stap uitvoeren:    
   
    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    In de **Relay-status** tekstvak, een URL met behulp van het volgende patroon: `https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Houd er rekening mee dat deze niet de werkelijke waarden zijn. u hebt deze waarden bijwerken met de werkelijke aanmeldings-URL, Relaystatus en -id. Neem contact op met [Lifesize Cloud Client ondersteuningsteam](https://www.lifesize.com/support) om op te halen van de aanmeldings-URL en id-waarden en u kunnen benutten Relaystatus SSO-configuratie die later in de zelfstudie wordt uitgelegd.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_general_400.png)

6. Op de **Lifesize Cloudconfiguratie** sectie, klikt u op **Lifesize Cloud configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. Om op te halen SSO is geconfigureerd voor uw toepassing, meld u aan bij de Lifesize cloudtoepassing met beheerdersbevoegdheden.

8. Klik op uw naam in de rechterbovenhoek en klik vervolgens op de **geavanceerde instellingen**.
   
    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. In de geavanceerde instellingen en klik nu op de **SSO-configuratie** koppeling. De pagina configuratie van eenmalige aanmelding voor uw exemplaar wordt geopend.
   
    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Configureer nu de volgende waarden in de gebruikersinterface voor SSO-configuratie.    
   
    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. In **Identity Provider Issuer** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    b.  In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. Uw base-64 gecodeerde certificaat openen in Kladblok gedownload vanuit Azure portal, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak.
  
    d. Voer de waarde als in de SAML-kenmerktoewijzingen voor het tekstvak Voornaam **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. In de toewijzing van het SAML-kenmerk voor de **achternaam** in het tekstvak de waarde als invoeren **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. In de toewijzing van het SAML-kenmerk voor de **e** in het tekstvak de waarde als invoeren **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. Om te controleren of de configuratie die u kunt klikken op de **Test** knop.
   
    >[!NOTE]
    >Voor het testen van geslaagde moet u de configuratiewizard kunt voltooien in Azure AD en ook welke toegang aan gebruikers of groepen die u kunnen de test uitvoeren.

12. De SSO inschakelen door te controleren op de **SSO inschakelen** knop.

13. Klik nu op de **Update** knop zodat alle instellingen worden opgeslagen. Hierdoor wordt de RelayState-waarde gegenereerd. Kopieer de waarde RelayState, die wordt gegenereerd in het tekstvak, plak deze in de **Relaystatus** tekstvak onder **Lifesize Cloud domein en URL's** sectie. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Het maken van een testgebruiker Lifesize Cloud

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Lifesize Cloud. Lifesize cloud biedt ondersteuning voor automatisch inrichten van gebruikers. Na een succesvolle verificatie bij Azure AD worden de gebruiker automatisch ingericht in de toepassing. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Lifesize Cloud.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Lifesize Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Lifesize Cloud**.

    ![Eenmalige aanmelding configureren](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Lifesize Cloud in het toegangsvenster, krijgt u de aanmeldingspagina van Lifesize cloudtoepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/lifesize-cloud-tutorial/tutorial_general_203.png


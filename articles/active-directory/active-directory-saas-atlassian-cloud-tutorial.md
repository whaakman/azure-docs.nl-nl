---
title: 'Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: 98623b6734726d43136703cbfee0b7ed0d82f815
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud

In deze zelfstudie leert u hoe Atlassian Cloud integreren met Azure Active Directory (Azure AD).

Atlassian Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Atlassian Cloud heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Atlassian Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Atlassian Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- SAML eenmalige aanmelding inschakelen voor Atlassian Cloud producten hebt u nodig om in te stellen van Identity Manager. Meer informatie over [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Atlassian Cloud uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Atlassian Cloud met Azure AD, moet u Atlassian Cloud uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Atlassian Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Atlassian Cloud**, selecteer **Atlassian Cloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Atlassian Cloud in de lijst met resultaten](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Atlassian Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de Atlassian Cloud is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Atlassian Cloud tot stand worden gebracht.

In de Atlassian Cloud, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Atlassian Cloud, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Atlassian Cloud](#create-an-atlassian-cloud-test-user)**  - Atlassian Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Atlassian Cloud.

**Om eenmalige aanmelding Azure AD met Atlassian Cloud configureert, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Atlassian Cloud** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Op de **Atlassian Cloud-domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Atlassian Cloud domein eenmalige aanmelding informatie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. In de **id** textbox, typ de URL:`https://auth.atlassian.com/saml/<unique ID>`
    
    b. In de **antwoord-URL** textbox, typ de URL:`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. In de **Relay status** textbox, typ een URL met het volgende patroon volgen:`https://<instancename>.atlassian.net`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Atlassian Cloud domein eenmalige aanmelding informatie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    In de **aanmeldings-URL textbox**, typt u een URL met het volgende patroon volgen:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. U krijgt deze waarden van de Cloudconfiguratie SAML Atlassian scherm die is beschreven in latere stappen van de zelfstudie.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Uw toepassing Atlassian Cloud verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw Token kenmerken van SAML-configuratie. Standaard wordt de gebruikers-id gekoppeld aan user.userprincipalname. Wijzig dit om toe te wijzen met **user.mail**. U kunt ook een andere geschikte waarde per de instellingen van uw organisatie, maar in de meeste gevallen e-mail moet werken.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Op de **Atlassian Cloudconfiguratie** sectie, klikt u op **Atlassian Cloud configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Atlassian Cloudconfiguratie](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

9. Ophalen van eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, meld u aan bij de Atlassian-Portal met behulp van de administrator-rechten.

10. Navigeer naar **Atlassian Sitebeheer** > **organisaties & beveiliging**. Als u dat nog niet gedaan hebt, maken en de naam van uw organisatie. Klik in het linkernavigatievenster **domeinen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

11. Selecteer de manier waarop dat u wilt controleren of uw domein - **DNS** of **HTTPS**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

12. Selecteer voor de DNS-verificatie **DNS** tabblad op de **domeinen** pagina en voer dit stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Klik op **kopie** om de waarde voor de TXT-record te kopiëren.

    b. Van uw DNS te zoeken naar de instellingenpagina voor het toevoegen van een nieuwe record.

    c. Selecteer de optie voor het toevoegen van een nieuwe record en plak de waarde die u hebt gekopieerd uit de **domeinen** pagina naar de **waarde** veld. Uw DNS kan ook verwijzen naar dit als **antwoord** of **beschrijving**.

    d. Uw DNS-record kan ook de volgende velden bevatten:
    
    * **Recordtype**: Voer **TXT**
    * **Host-naam-Alias**: laat de standaardwaarde (@ of leeg)
    * **Tijd tot live (TTL)**: Voer **86400**
    
    e.  Sla de record.

13. Ga terug naar de **pagina domeinen** Organisatiebeheer en klik op de **domein controleren** knop. Voer uw domeinnaam in het pop-upvenster en klik op de **domein controleren** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Het kan om de TXT-recordwijzigingen pas van kracht tot 72 uur duren voordat, weet u niet meteen of de verificatie van uw domein geslaagd is. Controleer uw **domeinen** pagina kort nadat u deze stappen voor de verificatiestatus van de uitvoert. U ziet na scherm met de bijgewerkte status als **GEVERIFIEERDE**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

14. Voor HTTPS-verificatie, selecteer de **HTTPS** tabblad op de **domeinen** pagina en voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Klik op **gedownloade bestand** voor het downloaden van het HTML-bestand.

    b.  Het HTML-bestand uploaden naar de hoofdmap van uw domein.

15. Ga terug naar de **domeinen** pagina in Organisatiebeheer en klik op de **domein controleren** knop. Voer uw **domeinnaam** in het pop-upvenster en klik op de **domein controleren** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

16. Als het verificatieproces in de hoofdmap geüploade bestand vinden kunt, de status van het domein werkt bij **geverifieerde**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Raadpleeg voor meer informatie over domeinverificatie [Atlassian van domein verificatie documentatie](https://confluence.atlassian.com/cloud/domain-verification-873871234.html)

17. Klik in de linkernavigatiebalk op **eenmalige aanmelding SAML**. Als u nog niet gedaan hebt, zich abonneren op Atlassian van Identity Manager.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

18. In **toevoegen de SAML-configuratie** dialoogvenster, het toevoegen van de instellingen van de identiteit als volgt:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. In de **identiteitsprovider entiteit-ID** tekst vak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.

    b. In de **identiteitsprovider URL SSO** tekst vak, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    c. Het gedownloade certificaat openen vanuit Azure-portal in een Kladblok, Kopieer de waarden zonder de regels van het certificaat Begin en einde certificaat en plak deze in de **openbare X509 certificaat** vak.
    
    d. Klik op **configuratie op te slaan**.
     
19. Werk de instellingen van Azure AD om ervoor te zorgen dat u setup de juiste URL's hebben.
  
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieer de **SP Identity-ID** van het SAML scherm en plak de waarde in de **id** vak in de Azure-portal onder Atlassian Cloud **domein en de URL's** sectie.
    
    b. Kopieer de **SP Assertion Consumer Service-URL** van het SAML scherm en plak de waarde in de **antwoord-URL** vak in de Azure-portal onder Atlassian Cloud **domein en de URL's** sectie.
    
    c. Meld u op de URL is de tenant-URL van uw Atlassian Cloud. 

    > [!NOTE]
    > Bestaande klanten moeten klikken op **Ja, werk de configuratie van** na het bijwerken van de **SP identiteit ID** en **SP Assertion Consumer Service-URL** waarden in Azure Portal. Nieuwe klanten hoeft niet in deze stap uitvoeren. 
    
20. Klik in de Azure-portal op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Een testgebruiker Atlassian Cloud maken

Om Azure AD-gebruikers zich aanmelden bij Atlassian Cloud, moeten ze worden ingericht in Atlassian Cloud. In geval van een Atlassian Cloud is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Klik in de beheersectie van de Site op het **gebruikers** knop

    ![Atlassian Cloud-gebruiker maken](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klik op de **uitnodiging gebruiker** om te maken van een gebruiker in de Atlassian Cloud.

    ![Atlassian Cloud-gebruiker maken](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Voer de gebruiker **e-mailadres** en de toegang tot de toepassing toe te wijzen. 

    ![Atlassian Cloud-gebruiker maken](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klik op **gebruikers uitnodigen** klikt, wordt de uitnodiging e-mailbericht naar de gebruiker wordt verzonden en na de uitnodiging accepteren de gebruiker zich actief in het systeem. 

>[!NOTE] 
>U kunt ook de gebruikers bulksgewijs maken door te klikken op de **bulksgewijs maken** knop in de sectie gebruikers.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Atlassian Cloud.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Atlassian Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Atlassian Cloud**.

    ![De koppeling Atlassian Cloud in de lijst met toepassingen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Atlassian Cloud in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Atlassian Cloud.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png


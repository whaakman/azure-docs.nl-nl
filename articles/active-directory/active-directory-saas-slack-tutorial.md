---
title: 'Zelfstudie: Azure Active Directory-integratie met Slack | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de toegestane vertraging.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 67d95b48520cd990428447dc775d9af22528c0f0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Zelfstudie: Azure Active Directory-integratie met Slack

In deze zelfstudie leert u hoe Slack integreren met Azure Active Directory (Azure AD).

Vertraging integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de toegestane vertraging heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de vertraging (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Slack, moet u de volgende items:

- Een Azure AD-abonnement
- Een toegestane eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vertraging in de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-slack-from-the-gallery"></a>Vertraging in de galerie toevoegen
Voor het configureren van de integratie van Slack met Azure AD, moet u de vertraging in de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Slack uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Slack**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. Selecteer in het deelvenster resultaten **Slack**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Slack op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Slack is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Slack tot stand worden gebracht.

Wijs in Slack, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Slack, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een toegestane testgebruiker](#creating-a-slack-test-user)**  - vertraging die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing toegestane configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Slack, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Slack** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. Op de **Slack-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.slack.com`

    b. In de **id** textbox, typ de URL:`https://slack.com`

    > [!NOTE] 
    > De waarde is geen echte. U moet de waarde wordt bijgewerkt met de werkelijke aanmelding op de URL. Neem contact op met [toegestane ondersteuningsteam](https://slack.com/help/contact) de waarde op te halen
     
4. De SAML-asserties verwacht toegestane toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster Selecteer **user.mail** als **gebruikers-id** en voor elke rij in de onderstaande tabel wordt weergegeven, voert u de volgende stappen uit:
    
    | Kenmerknaam | Waarde kenmerk |
    | --- | --- |
    | Voornaam | User.givenName |
    | Achternaam | User.surname |
    | User.Email | User.mail |  
    | User.Username | User.userPrincipalName |

    a. Klik op **kenmerk** openen **kenmerk bewerken** dialoogvenster vak en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    a. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    b. Van de **waarde** , selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    c. Klik op **OK**

6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. Op de **Slack configuratie** sectie, klikt u op **Slack configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  In een ander browservenster, meld u aan bij uw site toegestane bedrijf als beheerder.

10.  Navigeer naar **Microsoft Azure AD** Ga vervolgens naar **instellingen Team**.

     ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  In de **instellingen Team** sectie, klikt u op de **verificatie** tabblad en klik vervolgens op **Wijzigingsinstellingen**.

     ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. Op de **SAML-verificatie-instellingen** dialoogvenster de volgende stappen uitvoeren:

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  In de **SAML 2.0-eindpunt (HTTP)** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    b.  In de **identiteit Provider verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    c.  Open uw gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **openbaar certificaat** textbox.

    d. Configureer de bovenstaande drie instellingen voor uw team toegestane. Voor meer informatie over de instellingen, gaat u naar de **vertraging van SSO configuratiehandleiding** hier. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Klik op **configuratie op te slaan**.
     
    <!-- Deselect **Allow users to change their email address**.

    e.  Select **Allow users to choose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-slack-test-user"></a>Een toegestane testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Slack genoemd. Vertraging ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot de toegestane vertraging als deze nog niet bestaat.

> [!NOTE]
> Als u een gebruiker handmatig maken wilt, moet u contact op met [toegestane ondersteuningsteam](https://slack.com/help/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang met Slack.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon met Slack, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Slack**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de toegestane tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw toegestane toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png


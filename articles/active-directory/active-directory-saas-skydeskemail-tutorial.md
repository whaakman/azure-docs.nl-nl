---
title: 'Zelfstudie: Azure Active Directory-integratie met e-mailadres SkyDesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SkyDesk e-mailbericht.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: c495bd85fa2c55eec93f2e1100505b64389207f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Zelfstudie: Azure Active Directory-integratie met SkyDesk e-mailadres

In deze zelfstudie leert u hoe SkyDesk e integreren met Azure Active Directory (Azure AD).

E-mailadres SkyDesk integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SkyDesk e heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SkyDesk e (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SkyDesk e-mailadres, moet u de volgende items:

- Een Azure AD-abonnement
- Een e-mailadres SkyDesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u hier een proefversie van één maand krijgen [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SkyDesk e uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-skydesk-email-from-the-gallery"></a>SkyDesk e uit de galerie toevoegen
Voor het configureren van de integratie van SkyDesk E-mail in Azure AD, moet u SkyDesk E-mail uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SkyDesk e uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SkyDesk e**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. Selecteer in het deelvenster resultaten **SkyDesk e**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met SkyDesk e-mailadres op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SkyDesk e-mailbericht is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SkyDesk e-mailbericht worden gemaakt.

Wijs in SkyDesk e-mailbericht, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SkyDesk e](#creating-a-skydesk-email-test-user)**  - SkyDesk e-mailadres dat is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw SkyDesk e-mailtoepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SkyDesk e-mailadres, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SkyDesk e** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. Op de **SkyDesk e-maildomein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > De waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.sg/support/) de waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. Op de **SkyDesk e-mailconfiguratie** sectie, klikt u op **configureren SkyDesk e** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. Inschakelen van eenmalige aanmelding in **SkyDesk e**, voer de volgende stappen uit:

    a. Aanmelding bij uw SkyDesk e-mailaccount als administrator.

    b. Klik in het menu bovenaan op **Setup**, en selecteer **Org**. 
    
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Klik op **domeinen** in het linkerpaneel.
    
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klik op **domein toevoegen**.
    
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Voer uw domeinnaam en controleer vervolgens of het domein.
    
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klik op **SAML-verificatie** in het linkerpaneel.
    
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. Op de **SAML-verificatie** dialoogvenster pagina, voert u de volgende stappen uit:
   
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Als u wilt gebruiken op basis van SAML-verificatie, moet u ofwel hebben **gecontroleerde domein** of **portal URL** setup. U kunt de portal instellen URL met de unieke naam.
    > 
    > 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. In de **aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
   
    b. In de **afmelding** tekstvak URL gebruikersportal, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    c. **De URL van het wachtwoord wijzigen** is optioneel dus laat dit veld leeg.

    d. Klik op **sleutelbestand ophalen** selecteert u de gedownloade certificaat in Azure-portal en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Als **algoritme**, selecteer **RSA**.

    f. Klik op **Ok** de wijzigingen wilt opslaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-skydesk-email-test-user"></a>Maken van een testgebruiker SkyDesk e

In deze sectie kunt u een gebruiker met de naam van Britta Simon in SkyDesk E-mail maken.

1. Klik op **gebruikerstoegang** vanaf de linkerkant van het deelvenster in SkyDesk e-mailbericht en voer uw gebruikersnaam. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Als u nodig hebt om bulksgewijs gebruikers te maken, moet u contact opnemen met de [SkyDesk e-mailclient ondersteuningsteam](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot E-mail SkyDesk.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen SkyDesk E-mail, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SkyDesk e**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel SkyDesk e-mailadres in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw SkyDesk e-mailtoepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png


---
title: 'Zelfstudie: Azure Active Directory-integratie met Soonr werkplek | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Soonr werkplek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 55e99b2dd566dbf5a3bcb86974210c0399584e72
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Zelfstudie: Azure Active Directory-integratie met Soonr werkplek

In deze zelfstudie leert u hoe werkplek Soonr integreren met Azure Active Directory (Azure AD).

Soonr werkplek integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Soonr werkplek heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Soonr werkplek (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Soonr werkplek, moet u de volgende items:

- Een Azure AD-abonnement
- Een Soonr werkplek eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Soonr werkplek toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-soonr-workplace-from-the-gallery"></a>Soonr werkplek toevoegen uit de galerie
Voor het configureren van de integratie van Soonr werkplek in Azure AD, moet u Soonr werkplek uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Soonr werkplek uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Soonr werkplek**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_search.png)

5. Selecteer in het deelvenster resultaten **Soonr werkplek**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD bij Soonr werkplek op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Soonr werkplek is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker op de werkplek Soonr tot stand worden gebracht.

Wijs in Soonr werkplek, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding bij Soonr werkplek, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Soonr werkplek](#creating-a-soonr-workplace-test-user)**  - Soonr werkplek die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Soonr werkplek.

**Voor het configureren van Azure AD eenmalige aanmelding bij Soonr werkplek, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Soonr werkplek** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_samlbase.png)

3. Op de **Soonr werkplek domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.soonr.com/singlesignon/saml/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.soonr.com/singlesignon/saml/SSO`

4. Op de **Soonr werkplek domein en de URL's** sectie als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_url1.png)

    a. Klik op de **weergeven geavanceerde instellingen voor URL**.

    b. In de **aanmelding op URL** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.soonr.com/singlesignon/saml/SSO`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id en meld u aan bij de URL en antwoord. Neem contact op met [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/) ophalen van deze waarden.
 
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_general_400.png)

7. Op de **Soonr werkplek configuratie** sectie, klikt u op **Soonr werkplek configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_configure.png) 

8. Eenmalige aanmelding configureren op **Soonr werkplek** zijde, moet u de gedownloade verzenden **Metadata XML**, **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** naar [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

    >[!Note]
    >Als u hulp bij het configureren van Autotask werkplek nodig hebt, raadpleegt u [deze pagina](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) voor hulp bij uw werkplekaccount.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-soonr-workplace-test-user"></a>Maken van een testgebruiker Soonr werkplek

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Soonr werkplek. Werken met [Soonr werkplek ondersteuningsteam](https://awp.autotask.net/help/) een gebruiker te maken van het platform. U kunt de ondersteuningsticket met Soonr van verhogen <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">hier</a>.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan de werkplek Soonr.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Soonr werkplek, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Soonr werkplek**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.  

Als u op de tegel Soonr werkplek in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Soonr werkplek.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png


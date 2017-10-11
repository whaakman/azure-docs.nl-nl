---
title: 'Zelfstudie: Azure Active Directory-integratie met Syncplicity | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Syncplicity.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1321fa71bcd625d6ea754432bfb402d3919e38f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Zelfstudie: Azure Active Directory-integratie met Syncplicity

In deze zelfstudie leert u hoe Syncplicity integreren met Azure Active Directory (Azure AD).

Syncplicity integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Syncplicity heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Syncplicity (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Syncplicity, moet u de volgende items:

- Een Azure AD-abonnement
- Een Syncplicity eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Syncplicity uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity uit de galerie toevoegen
Voor het configureren van de integratie van Syncplicity in Azure AD, moet u Syncplicity uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Syncplicity uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Syncplicity**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. Selecteer in het deelvenster resultaten **Syncplicity**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Syncplicity op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Syncplicity is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Syncplicity tot stand worden gebracht.

Wijs in Syncplicity, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Syncplicity, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Syncplicity](#creating-a-syncplicity-test-user)**  - Syncplicity die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Syncplicity configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Syncplicity, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Syncplicity** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. Op de **Syncplicity domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.syncplicity.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Syncplicity Client ondersteuningsteam](https://www.syncplicity.com/contact-us) ophalen van deze waarden. 
 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. Op de **Syncplicity configuratie** sectie, klikt u op **configureren Syncplicity** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Aanmelden bij uw **Syncplicity** tenant.

8. Klik in het menu bovenaan op **admin**, selecteer **instellingen**, en klik vervolgens op **aangepaste domeinen en eenmalige aanmelding**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. Op de **eenmalige aanmelding (SSO)** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding \(eenmalige aanmelding\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. In de **aangepaste domeinen** textbox, typ de naam van uw domein.
  
    b. Selecteer **ingeschakeld** als **Single Sign-On Status**.

    c. In de **entiteit-Id** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.

    d. In de **aanmelden pagina-URL** textbox, plak de **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    e. In de **pagina-URL voor afmelden** textbox, plak de **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.

    f. In **Provider identiteitscertificaat**, klikt u op **bestand kiezen**, en vervolgens het certificaat dat u hebt gedownload vanuit de Azure-portal uploaden. 

    g. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-syncplicity-test-user"></a>Een testgebruiker Syncplicity maken
AAD-gebruikers moeten kunnen aanmelden, als ze worden ingericht op Syncplicity toepassing. Deze sectie beschrijft het maken van gebruikersaccounts van AAD in Syncplicity.

**Voor het inrichten van een gebruikersaccount aan Syncplicity, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Syncplicity** tenant (bijvoorbeeld: `https://company.Syncplicity.com`).

2. Klik op **admin** en selecteer **gebruikersaccounts**.

3. Klik op **toevoegen van een gebruiker**.
   
    ![Gebruikers beheren](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "gebruikers beheren")

4. Type de **e adressen** van een AAD-account dat u inrichten wilt, selecteer **gebruiker** als **rol**, en klik vervolgens op **volgende**.
   
    ![Accountgegevens](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "accountgegevens")
   
    >[!NOTE]
    >De accounthouder AAD Hiermee haalt u een e-mailbericht een koppeling om te bevestigen en activeren van het account waaronder. 
    > 

5. Selecteer een groep in uw bedrijf dat de nieuwe gebruiker moet lid van en klik vervolgens op **volgende**.
   
    ![Groepslidmaatschap](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "groepslidmaatschap")
   
    >[!NOTE]
    >Als er geen groepen die zijn vermeld zijn, klikt u op **volgende**. 
    > 

6. Selecteer de mappen die u wilt plaatsen, onder het beheer van Syncplicity op de computer van de gebruiker en klik vervolgens op **volgende**.
   
    ![Syncplicity mappen](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappen")

>[!NOTE]
>U kunt andere Syncplicity gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Syncplicity aan inrichten AAD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Syncplicity.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Syncplicity, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Syncplicity**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel Syncplicity in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Syncplicity.
## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png


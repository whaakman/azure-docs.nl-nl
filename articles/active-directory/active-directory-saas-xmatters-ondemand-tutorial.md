---
title: 'Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en xMatters OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ac4d4b5d810aca41620a5af2d29b86f07c1c031f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand

In deze zelfstudie leert u hoe xMatters OnDemand integreren met Azure Active Directory (Azure AD).

XMatters OnDemand integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot xMatters OnDemand heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij xMatters OnDemand (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met xMatters OnDemand, moet u de volgende items:

- Een Azure AD-abonnement
- Een xMatters eenmalige aanmelding OnDemand abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. XMatters OnDemand uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>XMatters OnDemand uit de galerie toevoegen
Voor het configureren van de integratie van xMatters OnDemand in Azure AD, moet u xMatters OnDemand uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen xMatters OnDemand uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **xMatters OnDemand**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. Selecteer in het deelvenster resultaten **xMatters OnDemand**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met xMatters die OnDemand op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in xMatters OnDemand aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in xMatters OnDemand moet tot stand worden gebracht.

Wijs in xMatters OnDemand, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met xMatters OnDemand, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gebruiker xMatters OnDemand test](#creating-a-xmatters-ondemand-test-user)**  - hebben een equivalent van Britta Simon in xMatters OnDemand die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw xMatters OnDemand-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met xMatters OnDemand, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **xMatters OnDemand** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. Op de **xMatters OnDemand domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. In de **id** textbox, typ een URL met het volgende patroon volgen:   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [xMatters OnDemand ondersteuningsteam](https://www.xmatters.com/company/contact-us/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand lokaal als **c:\\XMatters OnDemand.cer**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > U moet voor het doorsturen van het certificaat naar de [ondersteuningsteam voor OnDemand xMatters](https://www.xmatters.com/company/contact-us/). Het certificaat moet worden geüpload door het ondersteuningsteam xMatters voordat u de configuratie voor eenmalige aanmelding kunt voltooien. 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. Op de **xMatters OnDemand configuratie** sectie, klikt u op **xMatters OnDemand configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf XMatters OnDemand site als beheerder.

8. Klik in de werkbalk bovenaan op **Admin**, en klik vervolgens op **bedrijfsgegevens** in de navigatiebalk aan de linkerkant.
   
    ![Beheerder](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

9. Op de **SAML-configuratie** pagina, voert u de volgende stappen uit:
   
    ![De SAML-configuratie](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-configuratie")
   
    a. Selecteer **SAML inschakelen**.
   
    b. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure-portal in de **identiteit Provider-ID** textbox.
   
    c. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **eenmalige aanmelding op URL** textbox.
   
    d. Plakken **Sign-Out URL**, die u hebt gekopieerd vanuit de Azure-portal in de **-URL met eenmalige afmelding** textbox.
   
    e. Klik op de pagina Bedrijfsgegevens aan de bovenkant op **wijzigingen opslaan**.
    
    ![Details voor de bedrijfsportal](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "details voor de bedrijfsportal")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-xmatters-ondemand-test-user"></a>Maken van een gebruiker xMatters OnDemand testen

Om Azure AD-gebruikers zich aanmelden bij XMatters OnDemand inschakelt, moeten ze worden ingericht in XMatters OnDemand. In het geval van XMatters OnDemand is inrichting een handmatige taak.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **XMatters OnDemand** tenant.

2.  Klik op **gebruikers** tabblad en klik vervolgens op **gebruiker toevoegen**.
  
    ![Gebruikers](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "gebruikers")

3. In de **toevoegen van een gebruiker** sectie, voert u de volgende stappen uit:
   
    ![Een gebruiker toevoegen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "een gebruiker toevoegen")

    a. Selecteer **Active**.

    b. In de **gebruikers-ID** textbox type de gebruikers-id van gebruiker, zoals Brittasimon@contoso.com.
   
    c. In de **voornaam** textbox type voornaam van de gebruiker zoals Britta.

    d. In de **achternaam** textbox achternaam van de gebruiker zoals Simon type.
    
    e. In de **Site** textbox, voer de geldige site van een geldig Azure AD-account die u inrichten wilt.
    
    f. Klik op **Opslaan**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot xMatters OnDemand.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen xMatters OnDemand, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **xMatters OnDemand**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de xMatters OnDemand-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw xMatters OnDemand-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png


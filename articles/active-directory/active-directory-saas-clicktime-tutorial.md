---
title: 'Zelfstudie: Azure Active Directory-integratie met ClickTime | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: 0e0123a40d52dfd7a2e29c29cb2239e979089ca9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Zelfstudie: Azure Active Directory-integratie met ClickTime

In deze zelfstudie leert u hoe ClickTime integreren met Azure Active Directory (Azure AD).

ClickTime integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ClickTime heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ClickTime (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ClickTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een ClickTime eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ClickTime uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-clicktime-from-the-gallery"></a>ClickTime uit de galerie toevoegen
Voor het configureren van de integratie van ClickTime in Azure AD, moet u ClickTime uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ClickTime uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **ClickTime**, selecteer **ClickTime** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ClickTime in de lijst met resultaten](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met ClickTime op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ClickTime is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ClickTime tot stand worden gebracht.

Wijs in ClickTime, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ClickTime, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker ClickTime](#create-a-clicktime-test-user)**  - ClickTime die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ClickTime configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ClickTime, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ClickTime** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. Op de **ClickTime domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en ClickTime domein eenmalige aanmelding informatie](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    a. In de **id** textbox, typ een URL als:`https://app.clicktime.com/sp/`
    
    b. In de **antwoord-URL** textbox, typ een URL met de volgende patronen: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. Op de **ClickTime configuratie** sectie, klikt u op **configureren ClickTime** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![ClickTime configuratie](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. In een ander browservenster, meld u bij uw bedrijf ClickTime site als beheerder.

8. Klik in de werkbalk bovenaan op **voorkeuren**, en klik vervolgens op **beveiligingsinstellingen**.

9. In de **voorkeuren voor eenmalige aanmelding** configuratie sectie, voert u de volgende stappen uit:
   
    ![Beveiligingsinstellingen](./media/active-directory-saas-clicktime-tutorial/tic777280.png "beveiligingsinstellingen")
   
    a.  Selecteer **toestaan** aanmelden met eenmalige aanmelding (SSO) met **Azure AD**.
   
    b. In de **eindpunt van de Provider identiteit** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
    c.  Open de **base-64 gecodeerde certificaat** gedownload van Azure-portal op **Kladblok**, de inhoud kopieert en plakt u deze in de **X.509-certificaat** textbox.
   
    d.  Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-clicktime-test-user"></a>Een testgebruiker ClickTime maken

Om in te schakelen gebruikers van Azure AD aan te melden bij ClickTime, moeten ze worden ingericht in ClickTime.  
In het geval van ClickTime is inrichting een handmatige taak.

> [!NOTE]
> U kunt andere ClickTime gebruiker account hulpmiddelen voor het maken of API's die is geleverd door ClickTime voor het inrichten van Azure AD-gebruikersaccounts.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**
1. Meld u aan bij uw **ClickTime** tenant.
2. Klik in de werkbalk bovenaan op **bedrijf**, en klik vervolgens op **mensen**.
   
    ![Mensen](./media/active-directory-saas-clicktime-tutorial/tic777282.png "personen")
3. Klik op **persoon toevoegen**.
   
    ![Persoon toevoegen](./media/active-directory-saas-clicktime-tutorial/tic777283.png "persoon toevoegen")
4. Voer de volgende stappen uit in de sectie nieuwe persoon:
   
    ![Mensen](./media/active-directory-saas-clicktime-tutorial/tic777284.png "personen")
   
    a.  In de **volledige naam** textbox type volledige naam van gebruiker zoals **Britta Simon**. 
  
    b.  In de **e-mailadres** textbox, typ het e-mailadres van gebruiker, zoals  **brittasimon@contoso.com** .
       
    > [!NOTE]
    > Als u wilt, kunt u aanvullende eigenschappen van het nieuwe persoon object instellen.
   
    c.  Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ClickTime.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen ClickTime, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ClickTime**.

    ![ClickTimne koppeling in de lijst met toepassingen](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ClickTime in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing ClickTime.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png


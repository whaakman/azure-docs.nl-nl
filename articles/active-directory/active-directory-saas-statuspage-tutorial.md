---
title: 'Zelfstudie: Azure Active Directory-integratie met StatusPage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en StatusPage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: fa16cdec7b89404c140435fe57d5aa4b08cfa985
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Zelfstudie: Azure Active Directory-integratie met StatusPage

In deze zelfstudie leert u hoe StatusPage integreren met Azure Active Directory (Azure AD).

StatusPage integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot StatusPage heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij StatusPage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met StatusPage, moet u de volgende items:

- Een Azure AD-abonnement
- Een StatusPage eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. StatusPage uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-statuspage-from-the-gallery"></a>StatusPage uit de galerie toevoegen
Voor het configureren van de integratie van StatusPage in Azure AD, moet u StatusPage uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen StatusPage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **StatusPage**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. Selecteer in het deelvenster resultaten **StatusPage**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met StatusPage op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in StatusPage is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in StatusPage tot stand worden gebracht.

Wijs in StatusPage, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker StatusPage](#creating-a-statuspage-test-user)**  - StatusPage die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing StatusPage configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **StatusPage** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. Op de **StatusPage domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > Neem contact op met het ondersteuningsteam StatusPage op [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)om aan te vragen van de metagegevens die nodig zijn voor het configureren van eenmalige aanmelding. 
    >
    >a. Kopieer de waarde van de verlener van de metagegevens en plakt u deze in de **id** textbox.
    >
    >b. Kopieer de URL van het antwoord van de metagegevens en plakt u deze in de **antwoord-URL** textbox.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. Op de **StatusPage configuratie** sectie, klikt u op **configureren StatusPage** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. In een ander browservenster zich aanmelden bij uw bedrijf StatusPage site als een beheerder.

8. Klik in de hoofdwerkbalk **Account beheren**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. Klik op de **Single Sign-on** tabblad. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. Voer de volgende stappen uit op de installatiepagina van eenmalige aanmelding:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    a. In de **doel-URL voor eenmalige aanmelding** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    b. Open uw gedownloade certificaat in Kladblok, Kopieer de inhoud en plakt u deze in de **certificaat** textbox. 

    c. Klik op **configuratie opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-statuspage-test-user"></a>Een testgebruiker StatusPage maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in StatusPage genoemd.

StatusPage ondersteuning biedt voor just-in-time-inrichting. U hebt al ingeschakeld in [eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on).

**Als u wilt een gebruiker Britta Simon aangeroepen in StatusPage maakt, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf StatusPage site als beheerder.

2. Klik in het menu bovenaan op **Account beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. Klik op de **teamleden** tabblad. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. Klik op **toevoegen TEAMLID**. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. Typ de **e-mailadres**, **voornaam**, en **achternaam** van een geldige gebruiker die u inrichten in de bijbehorende tekstvakken wilt. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. Als **rol**, kies **Client Administrator**.

7. Klik op **ACCOUNT maken**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan StatusPage.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen StatusPage, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **StatusPage**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel StatusPage in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing StatusPage.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png


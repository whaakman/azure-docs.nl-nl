---
title: 'Zelfstudie: Azure Active Directory-integratie met OpenAthens | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en OpenAthens.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: af26e007c953c4157f5ee7a4251a52e9c45a6eac
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Zelfstudie: Azure Active Directory-integratie met OpenAthens

In deze zelfstudie leert u hoe OpenAthens integreren met Azure Active Directory (Azure AD).

OpenAthens integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot OpenAthens heeft.
- U kunt uw gebruikers automatisch aan te melden op OpenAthens (eenmalige aanmelding) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met OpenAthens, moet u de volgende items:

- Een Azure AD-abonnement
- Een OpenAthens eenmalige aanmelding ingeschakeld abonnement

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. OpenAthens uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-openathens-from-the-gallery"></a>OpenAthens uit de galerie toevoegen
Voor het configureren van de integratie van OpenAthens in Azure AD, moet u OpenAthens uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**OpenAthens uit de galerie toevoegen**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Blader naar **bedrijfstoepassingen**, en ga vervolgens naar **alle toepassingen**.

    ![Het deelvenster Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **OpenAthens**, selecteer **OpenAthens** vanuit het deelvenster resultaten en selecteer vervolgens de **toevoegen** knop.

    ![OpenAthens in de lijst met resultaten](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met OpenAthens op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in OpenAthens is bij de gebruiker in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in OpenAthens vast te stellen.

Wijs in OpenAthens, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met OpenAthens, moet u de volgende bouwstenen voltooien:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on), zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user), voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een testgebruiker OpenAthens](#create-a-openathens-test-user), met een exemplaar van Britta Simon OpenAthens die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user), Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on), om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing OpenAthens configureren.

**Eenmalige aanmelding Azure AD configureren met OpenAthens**

1. In de Azure-portal op de **OpenAthens** toepassing Integratiepagina **eenmalige aanmelding**.

    ![De koppeling voor eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** dialoogvenster, **op basis van SAML aanmelding** als de **modus**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_samlbase.png)

3. In de **OpenAthens domein en de URL's** sectie, voert u de waarde `https://login.openathens.net/saml/2/metadata-sp` in de **id** in het tekstvak.

    ![OpenAthens domein en één URL's aanmelding informatie](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_url.png)

4. In de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![Het certificaat voor ondertekening AMSL downloadkoppeling](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_certificate.png) 

5. Selecteer de knop **Opslaan**.

    ![De eenmalige aanmelding knop Opslaan](./media/active-directory-saas-openathens-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij uw bedrijf OpenAthens site als beheerder.

7. Selecteer **verbindingen** in de lijst onder de **Management** tabblad. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application1.png)

8. Selecteer **SAML 1.1/2.0**, en selecteer vervolgens de **configureren** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application2.png)
    
9. Selecteer om de configuratie toe de **Bladeren** klikken om het uploaden van de metagegevens van XML-bestand dat u hebt gedownload van de Azure-portal en selecteer vervolgens **toevoegen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application3.png)

10. Voer de volgende stappen uit onder de **Details** tabblad.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application4.png)

    a. In **weergave naamtoewijzing**, selecteer **kenmerk Use**.

    b. In de **weergave naamkenmerk** tekst en voer de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. In **unieke Gebruikerskoppeling**, selecteer **kenmerk Use**.

    d. In de **unieke gebruikerskenmerk** tekst en voer de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. In **Status**, schakel alle drie selectievakjes.

    f. In **maken van lokale accounts**, selecteer **automatisch**.

    g. Selecteer **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. Zie voor meer informatie over de functie embedded-documentatie, de [documentatie van Azure AD ingesloten](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal genaamd "Britta Simon."

   ![Een Azure AD-testgebruiker maken][100]

**Een testgebruiker maken in Azure AD**

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/active-directory-saas-openathens-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-openathens-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-openathens-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-openathens-tutorial/create_aaduser_04.png)

    a. In de **naam** in het tekstvak **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, typt u het e-mailadres voor Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** in het tekstvak.

    d. Selecteer **Maken**.
  
### <a name="create-an-openathens-test-user"></a>Een testgebruiker OpenAthens maken

OpenAthens ondersteunt just-in-time-inrichting en gebruikers worden automatisch gemaakt wanneer u bent geverifieerd. U hoeft niet elke actie in deze sectie uitvoeren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan OpenAthens.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon toewijzen aan OpenAthens**

1. In de Azure portal, open de toepassingen weergeven, blader naar de directoryweergave en Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. In de **toepassingen** selecteert **OpenAthens**.

    ![De koppeling OpenAthens in de lijst met toepassingen](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** selecteert **Britta Simon**.

6. Selecteer de **Selecteer** knop in de **gebruikers en groepen** lijst.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** deelvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u selecteert de **OpenAthens** tegel in het deelvenster toegang u moet automatisch aangemeld bij uw toepassing OpenAthens.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* Zie voor een lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory, [SaaS app integratie-zelfstudies voor gebruik met Azure AD](active-directory-saas-tutorial-list.md).
* Zie voor meer informatie over toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

<!--Image references-->

[1]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_203.png


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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: ddd669bb8ff8e520c5538457789d4f660a82255d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051421"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Zelfstudie: Azure Active Directory-integratie met OpenAthens

In deze zelfstudie leert u hoe u OpenAthens integreren met Azure Active Directory (Azure AD).

OpenAthens integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot OpenAthens heeft.
- U kunt uw gebruikers automatisch aan te melden op OpenAthens (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met OpenAthens, moet u de volgende items:

- Een Azure AD-abonnement
- Een OpenAthens eenmalige aanmelding ingeschakeld abonnement

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. OpenAthens uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-openathens-from-the-gallery"></a>OpenAthens uit de galerie toe te voegen
Voor het configureren van de integratie van OpenAthens in Azure AD, moet u OpenAthens uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**OpenAthens uit de galerie toevoegen**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Blader naar **bedrijfstoepassingen**, en ga vervolgens naar **alle toepassingen**.

    ![Het deelvenster met Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **OpenAthens**, selecteer **OpenAthens** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** knop.

    ![OpenAthens in de lijst met resultaten](./media/openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met OpenAthens op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in OpenAthens is voor de gebruiker in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in OpenAthens vast te stellen.

In OpenAthens, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met OpenAthens, moet u de volgende bouwstenen voltooien:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on), zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user), voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker OpenAthens](#create-a-openathens-test-user), hebben een equivalent van Britta Simon in OpenAthens die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user), Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#test-single-sign-on), om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing OpenAthens.

**Het configureren van Azure AD eenmalige aanmelding met OpenAthens**

1. In de Azure-portal op de **OpenAthens** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![De koppeling voor eenmalige aanmelding configureren][4]

2. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster, selecteer **SAML gebaseerde aanmelding** als de **modus**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/openathens-tutorial/tutorial_openathens_samlbase.png)

3. In de **OpenAthens domein en URL's** sectie, voert u de waarde `https://login.openathens.net/saml/2/metadata-sp` in de **id** in het tekstvak.

    ![OpenAthens domein en URL's, eenmalige aanmelding informatie](./media/openathens-tutorial/tutorial_openathens_url.png)

4. In de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![Het certificaat voor ondertekening AMSL downloadkoppeling](./media/openathens-tutorial/tutorial_openathens_certificate.png) 

5. Selecteer de knop **Opslaan**.

    ![De eenmalige aanmelding knop Opslaan](./media/openathens-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelden bij uw bedrijf OpenAthens site als beheerder.

7. Selecteer **verbindingen** in de lijst onder de **Management** tabblad. 

    ![Eenmalige aanmelding configureren](./media/openathens-tutorial/tutorial_openathens_application1.png)

8. Selecteer **SAML 1.1/2.0**, en selecteer vervolgens de **configureren** knop.

    ![Eenmalige aanmelding configureren](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
9. Als u wilt toevoegen de configuratie, selecteer de **Bladeren** klikken om het uploaden van het metagegevens-XML-bestand dat u hebt gedownload vanuit Azure portal en selecteer vervolgens **toevoegen**.

    ![Eenmalige aanmelding configureren](./media/openathens-tutorial/tutorial_openathens_application3.png)

10. Voer de volgende stappen uit onder de **Details** tabblad.

    ![Eenmalige aanmelding configureren](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. In **weergave Gebruikersnaamtoewijzing**, selecteer **gebruikskenmerk**.

    b. In de **weergave naamkenmerk** tekst voert u de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. In **unieke Gebruikerstoewijzing**, selecteer **gebruikskenmerk**.

    d. In de **unieke gebruikerskenmerk** tekst voert u de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. In **Status**, schakel alle drie selectievakjes.

    f. In **maken van lokale accounts**, selecteer **automatisch**.

    g. Selecteer **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app. Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. Zie voor meer informatie over de documentatie voor embedded-functie, de [documentatie over Azure AD embedded](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het een testgebruiker maken in Azure portal met de naam "Britta Simon."

   ![Maak een testgebruiker Azure AD][100]

**Een testgebruiker maken in Azure AD**

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/openathens-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/openathens-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/openathens-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/openathens-tutorial/create_aaduser_04.png)

    a. In de **naam** in het tekstvak **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak typt u het e-mailadres voor Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** in het tekstvak.

    d. Selecteer **Maken**.
  
### <a name="create-an-openathens-test-user"></a>Maak een testgebruiker OpenAthens

OpenAthens ondersteunt just-in-time inrichting, en gebruikers automatisch worden gemaakt na een geslaagde authenticatie. U hoeft niet te elke actie in deze sectie uitvoeren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot OpenAthens.

![De de gebruikersrol toewijzen][200] 

**Britta Simon toewijzen aan OpenAthens**

1. In de Azure-portal, open de toepassingen zien, bladert u naar de directoryweergave en Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. In de **toepassingen** in de lijst met **OpenAthens**.

    ![De koppeling OpenAthens in de lijst met toepassingen](./media/openathens-tutorial/tutorial_openathens_app.png)  

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in de lijst met **Britta Simon**.

6. Selecteer de **Selecteer** knop in de **gebruikers en groepen** lijst.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** deelvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u selecteert de **OpenAthens** tegel in het toegangsvenster, u moet automatisch aangemeld bij uw toepassing OpenAthens.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* Zie voor een lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory, [zelfstudies over integratie van SaaS-app voor gebruik met Azure AD](tutorial-list.md).
* Zie voor meer informatie over toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

<!--Image references-->

[1]: ./media/openathens-tutorial/tutorial_general_01.png
[2]: ./media/openathens-tutorial/tutorial_general_02.png
[3]: ./media/openathens-tutorial/tutorial_general_03.png
[4]: ./media/openathens-tutorial/tutorial_general_04.png

[100]: ./media/openathens-tutorial/tutorial_general_100.png

[200]: ./media/openathens-tutorial/tutorial_general_200.png
[201]: ./media/openathens-tutorial/tutorial_general_201.png
[202]: ./media/openathens-tutorial/tutorial_general_202.png
[203]: ./media/openathens-tutorial/tutorial_general_203.png


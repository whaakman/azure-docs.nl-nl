---
title: 'Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en Citrix ShareFile.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 9919be128ae651b589a37f957cc59ce6d171143f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431729"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile

In deze zelfstudie leert u hoe u Citrix ShareFile integreren met Azure Active Directory (Azure AD).

Citrix ShareFile integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Citrix ShareFile heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Citrix ShareFile (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Citrix ShareFile, moet u de volgende items:

- Een Azure AD-abonnement
- Een Citrix ShareFile eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Citrix ShareFile uit de galerie toevoegen
1. Configureren en Azure AD eenmalige aanmelding testen

## <a name="add-citrix-sharefile-from-the-gallery"></a>Citrix ShareFile uit de galerie toevoegen
Voor het configureren van de integratie van Citrix ShareFile in Azure AD, moet u Citrix ShareFile uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Citrix ShareFile uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Citrix ShareFile**, selecteer **Citrix ShareFile** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Citrix ShareFile in de lijst met resultaten](./media/sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Citrix ShareFile op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Citrix ShareFile is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Citrix ShareFile tot stand worden gebracht.

In Citrix ShareFile, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Citrix ShareFile, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  : als u wilt een equivalent van Britta Simon in Citrix ShareFile die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Citrix ShareFile.

**Voor het configureren van Azure AD eenmalige aanmelding met Citrix ShareFile, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Citrix ShareFile** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/sharefile-tutorial/tutorial_sharefile_samlbase.png)

1. Op de **Citrix ShareFile domein en URL's** sectie, voert u de volgende stappen uit:

    ![Citrix ShareFile domein en URL's, eenmalige aanmelding informatie](./media/sharefile-tutorial/tutorial_sharefile_url.png)
    
    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenant-name>.sharefile.com/saml/login`

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|
    | |

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [Citrix ShareFile Client ondersteuningsteam](https://www.citrix.co.in/products/sharefile/support.html) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/sharefile-tutorial/tutorial_sharefile_certificate.png)

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/sharefile-tutorial/tutorial_general_400.png)

1. Op de **Citrix ShareFile configuratie** sectie, klikt u op **configureren Citrix ShareFile** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Citrix ShareFile configuratie](./media/sharefile-tutorial/tutorial_sharefile_configure.png)

1. In een ander browservenster, meld u aan bij uw **Citrix ShareFile** bedrijf site als beheerder.

1. Klik in de werkbalk bovenaan op **Admin**.

1. Selecteer in het navigatiedeelvenster links **configureren Single Sign-On**.
   
    ![Beheer van account](./media/sharefile-tutorial/ic773627.png "beheer van Account")

1. Op de **Single Sign-On / configuratie van SAML 2.0** dialoogvenster pagina onder **basisinstellingen**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/sharefile-tutorial/ic773628.png "eenmalige aanmelding")
   
    a. Klik op **SAML inschakelen**.
    
    b. In **uw id-provider Issuer / entiteits-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **wijziging** naast de **X.509-certificaat** veld en upload het certificaat dat u hebt gedownload van de Azure-portal.
    
    d. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
    
    e. In **afmeldings-URL van** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

1. Klik op **opslaan** op de Citrix ShareFile management portal.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/sharefile-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/sharefile-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/sharefile-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/sharefile-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-citrix-sharefile-test-user"></a>Maak een testgebruiker Citrix ShareFile

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Citrix ShareFile, moeten ze worden ingericht voor Citrix ShareFile. In het geval van Citrix ShareFile is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Citrix ShareFile** tenant.

1. Klik op **gebruikers beheren \> startpagina van gebruikers beheren \> + maken van de werknemer**.
   
   ![Maken van de werknemer](./media/sharefile-tutorial/IC781050.png "werknemer maken")

1. Op de **basisinformatie** sectie, voert u onderstaande stappen te volgen:
   
   ![Algemene informatie](./media/sharefile-tutorial/IC799951.png "basisinformatie")
   
   a. In de **e-mailadres** tekstvak typt u het e-mailadres van Britta Simon als **brittasimon@contoso.com**.
   
   b. In de **voornaam** tekstvak, type **voornaam** van gebruiker als **Julia**.
   
   c. In de **achternaam** tekstvak, type **achternaam** van gebruiker als **Simon**.

1. Klik op **gebruiker toevoegen**.
  
   >[!NOTE]
   >De houder van Azure AD-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt. U kunt een andere hulpprogramma's voor Citrix ShareFile gebruiker-account maken of API's ontwerpgereedschappen van Citrix ShareFile gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Citrix ShareFile.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Citrix ShareFile, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Citrix ShareFile**.

    ![De Citrix ShareFile-koppeling in de lijst met toepassingen](./media/sharefile-tutorial/tutorial_sharefile_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Citrix ShareFile in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Citrix ShareFile.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/sharefile-tutorial/tutorial_general_01.png
[2]: ./media/sharefile-tutorial/tutorial_general_02.png
[3]: ./media/sharefile-tutorial/tutorial_general_03.png
[4]: ./media/sharefile-tutorial/tutorial_general_04.png

[100]: ./media/sharefile-tutorial/tutorial_general_100.png

[200]: ./media/sharefile-tutorial/tutorial_general_200.png
[201]: ./media/sharefile-tutorial/tutorial_general_201.png
[202]: ./media/sharefile-tutorial/tutorial_general_202.png
[203]: ./media/sharefile-tutorial/tutorial_general_203.png

---
title: 'Zelfstudie: Azure Active Directory-integratie met Yodeck | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Yodeck.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b2c8dccb-eeb0-4f4d-a24d-8320631ce819
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: b017efd2c170f543041dcb35a3a3d040389d1dac
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436791"
---
# <a name="tutorial-azure-active-directory-integration-with-yodeck"></a>Zelfstudie: Azure Active Directory-integratie met Yodeck

In deze zelfstudie leert u hoe u Yodeck integreren met Azure Active Directory (Azure AD).

Yodeck integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Yodeck heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Yodeck (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Yodeck, moet u de volgende items:

- Een Azure AD-abonnement
- Een Yodeck eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Yodeck uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-yodeck-from-the-gallery"></a>Yodeck uit de galerie toe te voegen
Voor het configureren van de integratie van Yodeck in Azure AD, moet u Yodeck uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Yodeck uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Yodeck**, selecteer **Yodeck** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Yodeck in de lijst met resultaten](./media/yodeck-tutorial/tutorial_yodeck_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Yodeck op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Yodeck is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Yodeck tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Yodeck, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Yodeck](#create-a-yodeck-test-user)**  : als u wilt een equivalent van Britta Simon in Yodeck die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Yodeck.

**Voor het configureren van Azure AD eenmalige aanmelding met Yodeck, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Yodeck** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/yodeck-tutorial/tutorial_yodeck_samlbase.png)

1. Op de **Yodeck domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Yodeck domein en URL's, eenmalige aanmelding informatie](./media/yodeck-tutorial/tutorial_yodeck_url.png)

    In de **id (entiteits-ID)** tekstvak typt u de URL: `https://app.yodeck.com/api/v1/account/metadata/`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Yodeck domein en URL's, eenmalige aanmelding informatie](./media/yodeck-tutorial/tutorial_yodeck_url1.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://app.yodeck.com/login`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/yodeck-tutorial/tutorial_yodeck_certificate.png)

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/yodeck-tutorial/tutorial_general_400.png)
    
1. In een ander browservenster aanmelden bij uw bedrijf Yodeck site als beheerder.

1. Klik op **gebruikersinstellingen** vorm de rechterbovenhoek van de pagina en selecteer de optie **Accountinstellingen**.

    ![Yodeck configuratie](./media/yodeck-tutorial/configure1.png)

1. Selecteer **SAML** en voer de volgende stappen uit:

    ![Yodeck configuratie](./media/yodeck-tutorial/configure2.png)

    a. Selecteer **importeren vanaf URL**.

    b. In de **URL** tekstvak, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd uit de Azure-portal en op **importeren**.
    
    c. Na het importeren van **App-Url voor federatieve metagegevens**, de resterende velden automatisch ingevuld.

    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/yodeck-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/yodeck-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/yodeck-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/yodeck-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-yodeck-test-user"></a>Maak een testgebruiker Yodeck

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Yodeck, moeten ze worden ingericht voor Yodeck.
In het geval van Yodeck is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Yodeck site aan als beheerder.

1. Klik op **gebruikersinstellingen** vorm de rechterbovenhoek van de pagina en selecteer de optie **gebruikers**.

    ![Werknemer toevoegen](./media/yodeck-tutorial/user1.png)

1. Klik op **+ gebruiker** openen de **Gebruikersdetails** tabblad.

    ![Werknemer toevoegen](./media/yodeck-tutorial/user2.png)

1. Op de **Gebruikersdetails** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/yodeck-tutorial/user3.png)

    a. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **Julia**.

    b. In de **achternaam** tekstvak, type de achternaam van de gebruiker, zoals **Simon**.

    c. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    d. Selecteer het geschikte **accountmachtigingen** optie aan de hand van uw organisatie nodig.
    
    e. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Yodeck.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Yodeck toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **Yodeck**.

    ![De koppeling Yodeck in de lijst met toepassingen](./media/yodeck-tutorial/tutorial_yodeck_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Yodeck in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Yodeck.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/yodeck-tutorial/tutorial_general_01.png
[2]: ./media/yodeck-tutorial/tutorial_general_02.png
[3]: ./media/yodeck-tutorial/tutorial_general_03.png
[4]: ./media/yodeck-tutorial/tutorial_general_04.png

[100]: ./media/yodeck-tutorial/tutorial_general_100.png

[200]: ./media/yodeck-tutorial/tutorial_general_200.png
[201]: ./media/yodeck-tutorial/tutorial_general_201.png
[202]: ./media/yodeck-tutorial/tutorial_general_202.png
[203]: ./media/yodeck-tutorial/tutorial_general_203.png


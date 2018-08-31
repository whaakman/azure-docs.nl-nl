---
title: 'Zelfstudie: Azure Active Directory-integratie met ArcGIS Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ArcGIS Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: ea2b32b43fedacba7b8a60db29762c32fda65aa5
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306339"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS Enterprise

In deze zelfstudie leert u hoe u ArcGIS-onderneming integreren met Azure Active Directory (Azure AD).

ArcGIS-onderneming integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ArcGIS Enterprise heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ArcGIS Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ArcGIS Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een ArcGIS ondernemingsbrede eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ArcGIS Enterprise uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-arcgis-enterprise-from-the-gallery"></a>ArcGIS Enterprise uit de galerie toe te voegen

Voor het configureren van de integratie van ArcGIS Enterprise in Azure AD, moet u ArcGIS Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ArcGIS Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **ArcGIS Enterprise**, selecteer **ArcGIS Enterprise** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ArcGIS-onderneming in de lijst met resultaten](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met ArcGIS Enterprise op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de onderneming ArcGIS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de onderneming ArcGIS tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met ArcGIS Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker ArcGIS Enterprise](#create-an-arcgis-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in ArcGIS-onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ArcGIS Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met ArcGIS Enterprise, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ArcGIS Enterprise** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_samlbase.png)

3. Op de **ArcGIS Enterprise domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![ArcGIS Enterprise domein en URL's, eenmalige aanmelding informatie](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `<EXTERNAL_DNS_NAME>.portal`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![ArcGIS Enterprise domein en URL's, eenmalige aanmelding informatie](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [ArcGIS Enterprise Client-ondersteuningsteam](mailto:support@esri.com) om deze waarden te verkrijgen. Krijgt u de id-waarde van **id-Provider instellen** sectie, die verderop in deze zelfstudie wordt uitgelegd.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_certificate.png)

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/arcgisenterprise-tutorial/tutorial_general_400.png)

7. In een ander browservenster aanmelden bij uw bedrijf ArcGIS Enterprise site als beheerder.

8. Selecteer **organisatie > Instellingen bewerken**.

    ![ArcGIS-Ondernemingsconfiguratie](./media/arcgisenterprise-tutorial/configure1.png)

9. Selecteer **Security** tabblad.

    ![ArcGIS-Ondernemingsconfiguratie](./media/arcgisenterprise-tutorial/configure2.png)

10. Schuif omlaag naar de **Enterprise aanmeldingen via SAML** sectie en selecteer **ENTERPRISE-aanmelding instellen**.

    ![ArcGIS-Ondernemingsconfiguratie](./media/arcgisenterprise-tutorial/configure3.png)

11. Op de **id-Provider instellen** sectie, voert u de volgende stappen uit:

    ![ArcGIS-Ondernemingsconfiguratie](./media/arcgisenterprise-tutorial/configure4.png)

    a. Geef een naam, zoals **Azure Active Directory-Test** in de **naam** tekstvak.

    b. In de **URL** tekstvak, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **geavanceerde instellingen weergeven** en kopieer de **entiteit-ID** waarde en plak deze in de **id** -tekstvak in de **ArcGIS Enterprise domein en URL's** sectie in Azure portal.
    
    ![ArcGIS-Ondernemingsconfiguratie](./media/arcgisenterprise-tutorial/configure5.png)

    d. Klik op **id-PROVIDER voor UPDATE**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/arcgisenterprise-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/arcgisenterprise-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/arcgisenterprise-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/arcgisenterprise-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-an-arcgis-enterprise-test-user"></a>Maak een testgebruiker ArcGIS Enterprise

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ArcGIS Enterprise. ArcGIS Enterprise biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot ArcGIS Enterprise als deze nog niet bestaat.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ArcGIS Enterprise-ondersteuningsteam](mailto:support@esri.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ArcGIS onderneming.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon voor ArcGIS-onderneming, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **ArcGIS Enterprise**.

    ![De ArcGIS Enterprise-koppeling in de lijst met toepassingen](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ArcGIS Enterprise in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ArcGIS Enterprise.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/arcgisenterprise-tutorial/tutorial_general_01.png
[2]: ./media/arcgisenterprise-tutorial/tutorial_general_02.png
[3]: ./media/arcgisenterprise-tutorial/tutorial_general_03.png
[4]: ./media/arcgisenterprise-tutorial/tutorial_general_04.png

[100]: ./media/arcgisenterprise-tutorial/tutorial_general_100.png

[200]: ./media/arcgisenterprise-tutorial/tutorial_general_200.png
[201]: ./media/arcgisenterprise-tutorial/tutorial_general_201.png
[202]: ./media/arcgisenterprise-tutorial/tutorial_general_202.png
[203]: ./media/arcgisenterprise-tutorial/tutorial_general_203.png
---
title: 'Zelfstudie: Azure Active Directory-integratie met Hightail | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Hightail.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: jeedes
ms.openlocfilehash: 1151044d5c1002c808ae1214086aff5fad84a55e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431331"
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Zelfstudie: Azure Active Directory-integratie met Hightail

In deze zelfstudie leert u hoe u Hightail integreren met Azure Active Directory (Azure AD).

Hightail integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Hightail heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Hightail (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Hightail, moet u de volgende items:

- Een Azure AD-abonnement
- Een Hightail eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Hightail uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hightail-from-the-gallery"></a>Hightail uit de galerie toe te voegen
Voor het configureren van de integratie van Hightail in Azure AD, moet u Hightail uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Hightail uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Hightail**.

    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/tutorial_hightail_search.png)

1. Selecteer in het deelvenster resultaten **Hightail**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Hightail op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Hightail is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Hightail tot stand worden gebracht.

In Hightail, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Hightail, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Hightail](#creating-a-hightail-test-user)**  : als u wilt een equivalent van Britta Simon in Hightail die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Hightail.

**Voor het configureren van Azure AD eenmalige aanmelding met Hightail, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Hightail** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_samlbase.png)

1. Op de **Hightail domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_url.png)

    In de **antwoord-URL** tekstvak typt u de URL als: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE]
    > De antwoord-URL-waarde is geen echte waarde. U kunt de antwoord-URL-waarde wordt bijgewerkt met de werkelijke antwoord-URL, die later in de zelfstudie wordt uitgelegd.

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_url1.png)

    In de **aanmelding URL** tekstvak typt u de URL als: `https://www.hightail.com/loginSSO`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_certificate.png) 

1. Hightail toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **'Kenmerk'** tabblad van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze. 

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_attribute.png) 

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |
    | FirstName | User.givenName |
    | LastName | User.surname |
    | Email | User.mail |    
    | UserIdentity | User.mail |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_officespace_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.

    e. Klik op **OK**.

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_general_400.png)

1. Op de **Hightail configuratie** sectie, klikt u op **configureren Hightail** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_configure.png)

    >[!NOTE]
    >Voordat u configureert de eenmalige aanmelding bij Hightail app, neemt u de accpetatielijst op in uw e-maildomein met Hightail team, zodat alle gebruikers die dit domein functionaliteit voor eenmalige aanmelding kunnen gebruiken.

1. Open in een ander browservenster de **Hightail** -beheerportal.

1. Klik op **Gebruikerspictogram** in de rechterbovenhoek van de pagina. 

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/configure1.png)

1. Klik op **weergave-beheerconsole** tabblad.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/configure2.png)

1. Klik in het menu bovenaan op de **SAML** tabblad en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/configure3.png)

    a. In de **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** gekopieerd vanuit Azure portal.

    b. Uw base-64 gecodeerde certificaat openen in Kladblok gedownload vanuit Azure portal, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **SAML-certificaat** tekstvak.

    c. Klik op **kopie** naar de URL voor SAML-consument voor uw exemplaar Kopieer en plak deze in **antwoord-URL** -tekstvak in **Hightail domein en URL's** sectie in Azure portal.

    d. Klik op **configuraties opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/hightail-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-hightail-test-user"></a>Het maken van een testgebruiker Hightail

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Hightail. 

Er is geen actie-item voor u in deze sectie. Hightail ondersteunt het inrichten van just-in-time-gebruikers op basis van de aangepaste claims. Als u de aangepaste claims hebt geconfigureerd zoals wordt weergegeven in de sectie **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , een gebruiker automatisch wordt gemaakt in de toepassing deze nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [Hightail ondersteuningsteam](mailto:support@hightail.com). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Hightail.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Hightail toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Hightail**.

    ![Eenmalige aanmelding configureren](./media/hightail-tutorial/tutorial_hightail_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel Hightail in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Hightail.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/hightail-tutorial/tutorial_general_01.png
[2]: ./media/hightail-tutorial/tutorial_general_02.png
[3]: ./media/hightail-tutorial/tutorial_general_03.png
[4]: ./media/hightail-tutorial/tutorial_general_04.png

[100]: ./media/hightail-tutorial/tutorial_general_100.png

[200]: ./media/hightail-tutorial/tutorial_general_200.png
[201]: ./media/hightail-tutorial/tutorial_general_201.png
[202]: ./media/hightail-tutorial/tutorial_general_202.png
[203]: ./media/hightail-tutorial/tutorial_general_203.png


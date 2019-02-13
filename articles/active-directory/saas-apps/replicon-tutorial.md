---
title: 'Zelfstudie: Azure Active Directory-integratie met Replicon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Replicon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46466ac85a8be1dec273440751489833f12c00db
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166808"
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Zelfstudie: Azure Active Directory-integratie met Replicon

In deze zelfstudie leert u hoe u Replicon integreren met Azure Active Directory (Azure AD).

Replicon integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Replicon heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Replicon (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Replicon, moet u de volgende items:

- Een Azure AD-abonnement
- Een Replicon eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Replicon uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-replicon-from-the-gallery"></a>Replicon uit de galerie toe te voegen
Voor het configureren van de integratie van Replicon in Azure AD, moet u Replicon uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Replicon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Replicon**, selecteer **Replicon** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Replicon in de lijst met resultaten](./media/replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Replicon op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Replicon is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Replicon tot stand worden gebracht.

In Replicon, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Replicon, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Replicon](#create-a-replicon-test-user)**  : als u wilt een equivalent van Britta Simon in Replicon die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Replicon.

**Voor het configureren van Azure AD eenmalige aanmelding met Replicon, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Replicon** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/replicon-tutorial/tutorial_replicon_samlbase.png)

3. Op de **Replicon domein en URL's** sectie, voert u de volgende stappen uit:

    ![Replicon domein en URL's, eenmalige aanmelding informatie](./media/replicon-tutorial/tutorial_replicon_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://global.replicon.com/<companyname>`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [Replicon Client ondersteuningsteam](https://www.replicon.com/customerzone/contact-support) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/replicon-tutorial/tutorial_replicon_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/replicon-tutorial/tutorial_general_400.png)

6. Meld u in een ander browservenster in uw bedrijf Replicon site als beheerder.

7. Voor het configureren van SAML 2.0, moet u de volgende stappen uitvoeren:

    ![SAML-verificatie inschakelen](./media/replicon-tutorial/ic777805.png "inschakelen SAML-verificatie")

    a. Om weer te geven de **EnableSAML Authentication2** dialoogvenster, voegt u het volgende op de URL, nadat de sleutel van uw bedrijf: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Hieronder ziet u het schema van de volledige URL: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Klik op de **+** om uit te breiden de **v20Configuration** sectie.

   c. Klik op de **+** om uit te breiden de **metaDataConfiguration** sectie.

   d. Klik op **bestand kiezen**, voor het selecteren van uw id-provider metagegevens XML-bestand en klik op **indienen**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/replicon-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/replicon-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/replicon-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/replicon-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-replicon-test-user"></a>Maak een testgebruiker Replicon

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Replicon.

**Als u de gebruiker handmatig moet maken, voert u de volgende stappen uit:**

1. Meld u in een browservenster in uw bedrijf Replicon site als beheerder.

2. Ga naar **beheer \> gebruikers**.

    ![Gebruikers](./media/replicon-tutorial/ic777806.png "Gebruikers")

3. Klik op **+ toevoegen van gebruiker**.

    ![Gebruiker toevoegen](./media/replicon-tutorial/ic777807.png "Gebruiker toevoegen")

4. In de **gebruikersprofiel** sectie, voert u de volgende stappen uit:

    ![Gebruikersprofiel](./media/replicon-tutorial/ic777808.png "gebruikersprofiel")

    a. In de **aanmeldingsnaam** tekstvak, typ de Azure AD e-mailadres van de Azure AD-gebruiker die u inrichten wilt, zoals **BrittaSimon@contoso.com**.

    b. Als **verificatietype**, selecteer **SSO**.

    c. In de **afdeling** tekstvak, typ de afdeling van de gebruiker.

    d. Als **werknemerstype**, selecteer **beheerder**.

    e. Klik op **gebruikersprofiel opslaan**.

>[!NOTE]
>U kunt alle andere Replicon gebruiker-account maken van hulpprogramma's of API's geleverd door Replicon voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Replicon.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Replicon toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Replicon**.

    ![De koppeling Replicon in de lijst met toepassingen](./media/replicon-tutorial/tutorial_replicon_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Replicon in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Replicon.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
[4]: ./media/replicon-tutorial/tutorial_general_04.png

[100]: ./media/replicon-tutorial/tutorial_general_100.png

[200]: ./media/replicon-tutorial/tutorial_general_200.png
[201]: ./media/replicon-tutorial/tutorial_general_201.png
[202]: ./media/replicon-tutorial/tutorial_general_202.png
[203]: ./media/replicon-tutorial/tutorial_general_203.png

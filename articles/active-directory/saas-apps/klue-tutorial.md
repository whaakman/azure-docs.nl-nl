---
title: 'Zelfstudie: Azure Active Directory-integratie met Klue | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Klue.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 08341008-980b-4111-adb2-97bbabbf1e47
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.openlocfilehash: 4afe11d6d241e86b57ebb40d54e4c2dceb63a46c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123053"
---
# <a name="tutorial-azure-active-directory-integration-with-klue"></a>Zelfstudie: Azure Active Directory-integratie met Klue

In deze zelfstudie leert u hoe u Klue integreren met Azure Active Directory (Azure AD).

Klue integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Klue heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Klue (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Klue, moet u de volgende items:

- Een Azure AD-abonnement
- Een Klue eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Klue uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-klue-from-the-gallery"></a>Klue uit de galerie toe te voegen

Voor het configureren van de integratie van Klue in Azure AD, moet u Klue uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Klue uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Klue**.

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/tutorial_klue_search.png)

5. Selecteer in het deelvenster resultaten **Klue**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/tutorial_klue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Klue op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Klue is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Klue tot stand worden gebracht.

In Klue, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Klue, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Klue](#creating-a-klue-test-user)**  : als u wilt een equivalent van Britta Simon in Klue die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Klue.

**Voor het configureren van Azure AD eenmalige aanmelding met Klue, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Klue** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_samlbase.png)

3. Op de **Klue domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `urn:klue:<Customer ID>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.klue.com/account/auth/saml/<Customer UUID>/callback`

4. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.klue.com/account/auth/saml/<Customer UUID>/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL, id en aanmeldings-URL. Neem contact op met [Klue Client ondersteuningsteam](mailto:support@klue.com) om deze waarden te verkrijgen.

5. De toepassing Klue wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk      | Waarde kenmerk      |
    | ------------------- | -------------------- |
    | first_name          | User.givenName |
    | last_name           | User.surname |
    | e-mailen               | User.userPrincipalName|

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.

    > [!NOTE]
    > Laat de **Namespace** waarde leeg.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_certificate.png) 

8. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_general_400.png)

9. Op de **Klue configuratie** sectie, klikt u op **configureren Klue** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_configure.png) 

10. Het configureren van eenmalige aanmelding op **Klue** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64), Single Sign-On Service URL voor SAML- en SAML entiteit-ID** naar [Klue ondersteuningsteam](mailto:support@klue.com).

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/create_aaduser_03.png)

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/klue-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-klue-test-user"></a>Het maken van een testgebruiker Klue

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Klue. Klue biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Klue als deze nog niet bestaat.

> [!Note]
> Als u wilt maken van een gebruiker handmatig, neem contact op met [Klue ondersteuningsteam](mailto:support@klue.com).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Klue.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Klue toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Klue**.

    ![Eenmalige aanmelding configureren](./media/klue-tutorial/tutorial_klue_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Klue in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Klue.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/klue-tutorial/tutorial_general_01.png
[2]: ./media/klue-tutorial/tutorial_general_02.png
[3]: ./media/klue-tutorial/tutorial_general_03.png
[4]: ./media/klue-tutorial/tutorial_general_04.png

[100]: ./media/klue-tutorial/tutorial_general_100.png

[200]: ./media/klue-tutorial/tutorial_general_200.png
[201]: ./media/klue-tutorial/tutorial_general_201.png
[202]: ./media/klue-tutorial/tutorial_general_202.png
[203]: ./media/klue-tutorial/tutorial_general_203.png
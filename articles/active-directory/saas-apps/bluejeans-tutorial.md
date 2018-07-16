---
title: 'Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: d485c16719c07062249e8d40f0feca9685851834
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043462"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Zelfstudie: Azure Active Directory-integratie met BlueJeans

In deze zelfstudie leert u hoe u BlueJeans integreren met Azure Active Directory (Azure AD).

BlueJeans integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BlueJeans heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij BlueJeans (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BlueJeans, moet u de volgende items:

- Een Azure AD-abonnement
- Een BlueJeans eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BlueJeans uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans uit de galerie toe te voegen
Voor het configureren van de integratie van BlueJeans in Azure AD, moet u BlueJeans uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BlueJeans uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **BlueJeans**.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Selecteer in het deelvenster resultaten **BlueJeans**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met BlueJeans op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BlueJeans is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BlueJeans tot stand worden gebracht.

In BlueJeans, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker BlueJeans](#creating-a-bluejeans-test-user)**  : als u wilt een equivalent van Britta Simon in BlueJeans die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BlueJeans.

**Voor het configureren van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BlueJeans** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Op de **BlueJeans domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.BlueJeans.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [BlueJeans Client ondersteuningsteam](https://support.bluejeans.com/contact) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_general_400.png)

6. Op de **BlueJeans configuratie** sectie, klikt u op **configureren BlueJeans** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, de URL van wijzigen wachtwoord en de Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. In een ander browservenster aanmelden bij uw **BlueJeans** bedrijf site als beheerder.

8. Ga naar **ADMIN \> groepsinstellingen \> Security**.

   ![Beheerder](./media/bluejeans-tutorial/IC785868.png "Admin")

9. In de **Security** sectie, voert u de volgende stappen uit:

   ![SAML-eenmalige aanmelding](./media/bluejeans-tutorial/IC785869.png "SAML-eenmalige aanmelding")

   a. Selecteer **SAML-eenmalige aanmelding**.

   b. Selecteer **automatische inrichting inschakelen**.

10. Verdergaan met de volgende stappen uit:

    ![Pad van het certificaat](./media/bluejeans-tutorial/IC785870.png "pad van het certificaat")

    a. Klik op **bestand kiezen**, en upload vervolgens het gedownloade certificaat.

    b. Plakken **Single Sign-On Service URL voor SAML** in de **aanmeldings-URL** tekstvak.

    c. Plakken **URL van wijzigen wachtwoord** in de **URL voor wachtwoord wijzigen** tekstvak.

    d. Plakken **afmelding URL** in de **afmeldings-URL van** tekstvak.

11. Verdergaan met de volgende stappen uit:

    ![Wijzigingen opslaan](./media/bluejeans-tutorial/IC785874.png "wijzigingen opslaan")

    a. In de **gebruikers-id** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. In de **e** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Klik op **wijzigingen opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_03.png)

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-bluejeans-test-user"></a>Het maken van een testgebruiker BlueJeans

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in BlueJeans. BlueJeans ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](bluejeans-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, voert u de volgende stappen uit:**

1. Meld u aan bij uw **BlueJeans** bedrijf site als beheerder.

2. Ga naar **ADMIN \> gebruikers beheren \> gebruiker toevoegen**.

   ![Beheerder](./media/bluejeans-tutorial/IC785877.png "Admin")

   >[!IMPORTANT]
   >De **gebruiker toevoegen** tabblad is alleen beschikbaar als in de **tabblad Beveiliging**, **automatische inrichting inschakelen** is uitgeschakeld. 

3. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Gebruiker toevoegen](./media/bluejeans-tutorial/IC785886.png "gebruiker toevoegen")

    a. Typ een **BlueJeans gebruikersnaam**, een **e-mailadres**, een **BlueJeans vergadering ID**, een **Moderator wachtwoordcode**, een **volledige naam** , wordt de **bedrijf** van een geldige AAD-account dat u inrichten in de bijbehorende tekstvakken wilt.

    b. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere BlueJeans gebruiker-account maken van hulpprogramma's of API's geleverd door BlueJeans aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BlueJeans.

![Gebruiker toewijzen][200]

**Als u wilt Britta Simon aan BlueJeans toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **BlueJeans**.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel BlueJeans in het toegangsvenster, krijgt u de aanmeldingspagina van BlueJeans toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png

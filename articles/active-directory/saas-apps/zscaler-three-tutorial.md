---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler drie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler drie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.openlocfilehash: 0ef8fc2ea8b006d49dd54d638183a58bf78a5797
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312723"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Zelfstudie: Azure Active Directory-integratie met drie Zscaler

In deze zelfstudie leert u hoe u drie Zscaler integreren met Azure Active Directory (Azure AD).

Drie Zscaler integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot drie Zscaler heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij drie Zscaler (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler drie, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler drie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Uit de galerie toe te voegen drie Zscaler
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-three-from-the-gallery"></a>Uit de galerie toe te voegen drie Zscaler

Voor het configureren van de integratie van drie Zscaler in Azure AD, moet u drie Zscaler toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Zscaler drie uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens de **alle toepassingen** optie.

    ![Toepassingen][2]

3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Selecteer in het deelvenster resultaten **Zscaler drie**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler drie op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in drie Zscaler is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in drie Zscaler tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Proxy-instellingen configureren](#configuring-proxy-settings)**  : als u wilt de proxy-instellingen in Internet Explorer configureren
3. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Het maken van een Zscaler drie testgebruiker](#creating-a-zscaler-three-test-user)**  : als u wilt een equivalent van Britta Simon in Zscaler drie die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler drie.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler drie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler drie** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Als u wijzigen wilt in **SAML** modus van een andere modus, klikt u op **modus wijzigen in één aanmelding** boven op het scherm.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    Voer in het tekstvak de antwoord-URL de URL: `https://login.zscalerthree.net/sfc_sso`

    > [!NOTE]
    > U moet deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Zscaler drie Client ondersteuningsteam](https://www.zscaler.com/company/contact) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op op **downloaden** downloaden **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. Op de **instellen Zscaler drie** sectie, Kopieer de **aanmeldings-URL**.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. In een ander browservenster aanmelden bij uw Zscaler drie bedrijf site als beheerder.

10. Klik in het menu aan de bovenkant op **beheer**.

    ![Beheer](./media/zscaler-three-tutorial/ic800206.png "beheer")

9. Onder **beheren beheerders en rollen**, klikt u op **gebruikers beheren & verificatie**.

    ![Gebruikers & verificatie beheren](./media/zscaler-three-tutorial/ic800207.png "gebruikers & verificatie beheren")

10. In de **verificatie-opties kiezen voor uw organisatie** sectie, voert u de volgende stappen uit:

    ![Verificatie](./media/zscaler-three-tutorial/ic800208.png "verificatie")

    a. Selecteer **verifiëren met behulp van SAML Single Sign-On**.

    b. Klik op **configureren van eenmalige SAML-aanmelding Parameters**.

11. Op de **configureren SAML Single Sign-On Parameters** dialoogvenster pagina, voer de volgende stappen uit en klik vervolgens op **gedaan**

    ![Single Sign-On](./media/zscaler-three-tutorial/ic800209.png "Single Sign-On")

    a. Plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit de Azure portal in de **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** tekstvak.

    b. In de **aanmeldingsnaam die het kenmerk** tekstvak, type **NameID**.

    c. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.

    d. Selecteer **SAML automatische inrichting inschakelen**.

12. Op de **gebruikersverificatie configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Beheer](./media/zscaler-three-tutorial/ic800210.png "beheer")

    a. Klik op **Opslaan**.

    b. Klik op **nu activeren**.

## <a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen in Internet Explorer configureren

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** uit de **extra** menu voor open de **Internetopties** dialoogvenster.

     ![Internetopties](./media/zscaler-three-tutorial/ic769492.png "Internetopties")

3. Klik op de **verbindingen** tabblad.
  
     ![Verbindingen](./media/zscaler-three-tutorial/ic769493.png "verbindingen")

4. Klik op **LAN-instellingen** openen de **LAN-instellingen** dialoogvenster.

5. In het gedeelte van de server Proxy de volgende stappen uitvoeren:

    ![Proxyserver](./media/zscaler-three-tutorial/ic769494.png "proxyserver")

    a. Selecteer **een proxyserver gebruiken voor uw LAN**.

    b. Typ in het tekstvak adres **gateway.zscalerthree.net**.

    c. Typ in het tekstvak poort **80**.

    d. Selecteer **proxyserver niet gebruiken voor lokale adressen**.

    e. Klik op **OK** sluiten de **Local Area Network (LAN)-instellingen** dialoogvenster.

6. Klik op **OK** sluiten de **Internetopties** dialoogvenster.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Klik op **Create**.

### <a name="creating-a-zscaler-three-test-user"></a>Het maken van een gebruiker met Zscaler drie testen

Als u wilt dat gebruikers zich aanmelden bij Zscaler drie Azure AD, moeten ze worden ingericht op drie Zscaler. In het geval van drie Zscaler is inrichten een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zscaler drie** tenant.

2. Klik op **beheer**.

    ![Beheer](./media/zscaler-three-tutorial/ic781035.png "beheer")

3. Klik op **Gebruikersbeheer**.

     ![Voeg](./media/zscaler-three-tutorial/ic781036.png "toevoegen")

4. In de **gebruikers** tabblad **toevoegen**.

    ![Voeg](./media/zscaler-three-tutorial/ic781037.png "toevoegen")

5. In de sectie toevoegen van gebruiker door de volgende stappen uitvoeren:

    ![Gebruiker toevoegen](./media/zscaler-three-tutorial/ic781038.png "gebruiker toevoegen")

    a. Type de **UserID**, **weergavenaam gebruiker**, **wachtwoord**, **wachtwoord bevestigen**, en selecteer vervolgens **groepen**en de **afdeling** van een geldige Azure AD-account die u inrichten wilt.

    b. Klik op **Opslaan**.

> [!NOTE]
> U kunt een andere Zscaler drie gebruiker-account maken-hulpprogramma's of API's die worden geleverd door Zscaler drie gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot drie Zscaler.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Zscaler drie**.

    ![Eenmalige aanmelding configureren](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **gebruiker toevoegen** knop en selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster klikt u op de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de drie Zscaler tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zscaler drie toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png
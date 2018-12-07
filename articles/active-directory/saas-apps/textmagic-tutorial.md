---
title: 'Zelfstudie: Azure Active Directory-integratie met TextMagic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TextMagic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: jeedes
ms.openlocfilehash: 5ab193d908063230946ebb2bb6320ab50bf14971
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014885"
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Zelfstudie: Azure Active Directory-integratie met TextMagic

In deze zelfstudie leert u hoe u TextMagic integreren met Azure Active Directory (Azure AD).

TextMagic integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TextMagic heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TextMagic (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TextMagic, moet u de volgende items:

- Een Azure AD-abonnement
- Een TextMagic eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TextMagic uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-textmagic-from-the-gallery"></a>TextMagic uit de galerie toe te voegen

Voor het configureren van de integratie van TextMagic in Azure AD, moet u TextMagic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TextMagic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **TextMagic**, selecteer **TextMagic** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TextMagic in de lijst met resultaten](./media/textmagic-tutorial/tutorial_textmagic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met TextMagic op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TextMagic is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TextMagic tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met TextMagic, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker TextMagic](#creating-a-textmagic-test-user)**  : als u wilt een equivalent van Britta Simon in TextMagic die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TextMagic.

**Voor het configureren van Azure AD eenmalige aanmelding met TextMagic, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TextMagic** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![TextMagic domein en URL's, eenmalige aanmelding informatie](./media/textmagic-tutorial/tutorial_textmagic_url.png)

    In de **id** tekstvak, een URL typen: `https://my.textmagic.com/saml/metadata`

5. TextMagic toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken en Claims** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](./media/textmagic-tutorial/i4-attribute.png)

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Name  | Kenmerk van de gegevensbron  | Naamruimte |
    | --------------- | --------------- | --------------- |
    | Bedrijf | User.CompanyName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Voornaam               | User.givenName |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName            | User.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | telefoon               | User.telephoneNumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    
    a. Klik op **bewerkingspictogram** bewerken de **naam id-waarde** van **user.userprinicipalname** naar **user.mail**.

    ![TextMagic kenmerk](./media/textmagic-tutorial/tutorial_textmagic_email.png)

    b. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./common/new_save_attribute.png)

    ![image](./common/new_attribute_details.png)

    c. In de **naam**tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    d. Voer de **Namespace** waarde.

    e. Selecteer de bron als **kenmerk**.

    f. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    g. Klik op **OK**.

    h. Klik op **Opslaan**. 

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/textmagic-tutorial/tutorial_textmagic_certificate.png) 

8. Op de **TextMagic instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![TextMagic configuratie](common/configuresection.png)

9. In een ander browservenster aanmelden bij uw bedrijf TextMagic site als beheerder.

10. Selecteer **Accountinstellingen** onder de gebruikersnaam.

    ![TextMagic configuratie](./media/textmagic-tutorial/config1.png)

11. Klik op het tabblad **eenmalige aanmelding (SSO)** en vult u de volgende velden:  
    
    ![TextMagic configuratie](./media/textmagic-tutorial/config2.png)

    a. In **id-provider entiteit-ID:** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    b. In **id-provider voor eenmalige aanmelding-URL:** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    c. In **id-provider SLO-URL:** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    d. Open uw **base-64 gecodeerde certificaat** in Kladblok gedownload vanuit Azure portal, kopieert u de inhoud ervan naar het Klembord en plakt u deze naar de **openbare x509 certificaat:** tekstvak.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="creating-a-textmagic-test-user"></a>Het maken van een testgebruiker TextMagic

Toepassing ondersteunt **Just-in-time gebruikersinrichting** en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt. U moet de gegevens eenmaal op de eerste aanmelding bij het activeren van de onderliggende account in het systeem in te vullen.
Er is geen actie-item voor u in deze sectie.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TextMagic.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **TextMagic**.

    ![Eenmalige aanmelding configureren](./media/textmagic-tutorial/tutorial_textmagic_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TextMagic in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TextMagic.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png

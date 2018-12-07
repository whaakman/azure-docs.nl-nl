---
title: 'Zelfstudie: Azure Active Directory-integratie met Procore SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Procore eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 12e7a37c1d2811b44dbf943b460b7d620770d45e
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53017522"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Zelfstudie: Azure Active Directory-integratie met Procore eenmalige aanmelding

In deze zelfstudie leert u hoe u Procore SSO integreren met Azure Active Directory (Azure AD).

Procore SSO integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Procore eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Procore SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Procore eenmalige aanmelding, moet u de volgende items:

- Een Azure AD-abonnement
- Een Procore SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Procore SSO uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-procore-sso-from-the-gallery"></a>Procore SSO uit de galerie toe te voegen

Voor het configureren van de integratie van Procore SSO in Azure AD, moet u Procore SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Procore SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Procore SSO**, selecteer **Procore SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Procore eenmalige aanmelding in de lijst met resultaten](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Procore eenmalige aanmelding op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Procore SSO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Procore eenmalige aanmelding tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Procore eenmalige aanmelding testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Procore SSO](#creating-a-procore-sso-test-user)**  : als u wilt een equivalent van Britta Simon in Procore eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Procore eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met Procore eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Procore SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![Procore SSO-domein en URL's, eenmalige aanmelding informatie](./media/procoresso-tutorial/tutorial_procoresso_url.png)

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

6. Op de **Procore SSO instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![Procore SSO-configuratie](common/configuresection.png)

7. Het configureren van eenmalige aanmelding op **Procore SSO** side, meld u aan bij uw site procore bedrijf als beheerder.

8. In de vervolgkeuzelijst werkset omlaag, klik op **Admin** de SSO-instellingen voor pagina te openen.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/procore_tool_admin.png)

9. Plak de waarden in de vakken zoals beschreven onder-

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. In de **eenmalige aanmelding op URL-verlener** tekst vak, plak de waarde van **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.

    b. In de **SAML Meld u op de doel-URL** vak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. Open nu het **federatieve metagegevens-XML** hierboven gedownload vanuit Azure portal en kopieer het certificaat in de tag met de naam **X509Certificate**. Plak de gekopieerde waarde in de **Single Sign On x509 certificaat** vak.

10. Klik op **wijzigingen opslaan**.

11. Na deze instellingen zijn, moet u voor het verzenden van de **domeinnaam** (bijvoorbeeld **contoso.com**) via waarmee u zich bij Procore naar de [Procore ondersteuningsteam](https://support.procore.com/) en deze zullen activeren van federatieve SSO voor dat domein.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->

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

### <a name="creating-a-procore-sso-test-user"></a>Het maken van een testgebruiker Procore eenmalige aanmelding

Volg de onderstaande stappen voor het maken van een Procore testgebruiker Procore SSOc aan.

1. Meld u aan met uw site procore bedrijf als beheerder.  

2. In de vervolgkeuzelijst werkset omlaag, klik op **Directory** om het bedrijf directory-pagina te openen.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Klik op **toevoegen van een persoon** optie voor het openen van het formulier en voer de volgende mogelijkheden: uitvoeren

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_add.png)

    a. In de **voornaam** tekstvak de voornaam van de gebruiker van het type, zoals **Julia**.

    b. In de **achternaam** tekstvak, de achternaam van de gebruiker van het type, zoals **Simon**.

    c. In de **e-mailadres** tekstvak van de gebruiker van het type e-mailadres zoals **BrittaSimon@contoso.com**.

    d. Selecteer **machtigingssjabloon** als **machtigingssjabloon Later toepassen**.

    e. Klik op **Create**.

4. Controleer en de details voor de toegevoegde contactpersoon bijwerken.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_check.png)

5. Klik op **opslaan en verzenden Invitiation** (als een uitnodiging via e-mail vereist is) of **opslaan** (opslaan rechtstreeks) om de Gebruikersregistratie te voltooien.
    
    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_save.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Procore eenmalige aanmelding.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Procore SSO**.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Procore eenmalige aanmelding in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Procore eenmalige aanmelding.
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

---
title: 'Zelfstudie: Azure Active Directory-integratie met G Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2018
ms.author: jeedes
ms.openlocfilehash: b8f6e69169cd146ec9dd20d8dad43b74ddb59228
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726879"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Zelfstudie: Azure Active Directory-integratie met G Suite

In deze zelfstudie leert u over het integreren van G Suite met Azure Active Directory (Azure AD).

G Suite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot G Suite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij G Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met G Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een G Suite eenmalige aanmelding ingeschakeld abonnement
- Een abonnement op Google Apps of Google Cloud Platform-abonnement.

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving. Dit document is gemaakt met behulp van de nieuwe gebruiker-op-ervaring. Als u nog steeds de oude heeft, wordt de installatie diferent bekijken. U kunt de nieuwe ervaring in de instellingen voor eenmalige aanmelding van G Suite-toepassing inschakelen. Ga naar **Azure AD, bedrijfstoepassingen**, selecteer **G Suite**, selecteer **Single Sign-on** en klik vervolgens op **proberen onze nieuwe ervaring voor**.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **V: bevat ondersteuning voor deze integratie Google Cloud Platform SSO-integratie met Azure AD?**

    A: Ja. Google Cloud Platform en Google Apps delen de dezelfde verificatie-platform. Hiervoor de GCP-integratie moet u dus de eenmalige aanmelding met Google Apps configureren.

2. **V: Chromebooks en andere apparaten Chrome compatibel zijn met Azure AD eenmalige aanmelding?**
  
    Antwoord: Ja, gebruikers kunnen zich aanmelden bij hun Chromebook-apparaten met behulp van hun Azure AD-referenties. Raadpleeg deze [G Suite-ondersteuningsartikel](https://support.google.com/chrome/a/answer/6060880) voor meer informatie over waarom gebruikers mogelijk wordt gevraagd om referenties twee keer.

3. **V: als ik eenmalige aanmelding inschakelen, gebruikers zich met hun Azure AD-referenties aanmelden bij een Google-product, zoals Google Classroom, GMail, Google Drive, YouTube, enzovoort?**

    A: Ja, afhankelijk van [welke G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) u wilt in- of uitschakelen voor uw organisatie.

4. **V: kan ik eenmalige aanmelding voor slechts een subset van mijn gebruikers G Suite inschakelen?**

    A: Nee, onmiddellijk op eenmalige aanmelding in te schakelen, is uw G Suite-gebruikers te verifiëren met hun Azure AD-referenties vereist. Omdat G Suite biedt geen ondersteuning voor meerdere id-providers, de id-provider voor uw G Suite-omgeving kan Azure AD zijn of Google, maar niet beide op hetzelfde moment.

5. **Vraag: als een gebruiker is aangemeld via Windows, worden dat ze automatisch worden geverifieerd met G Suite zonder wordt gevraagd om een wachtwoord?**

    Antwoord: Er zijn twee opties voor het inschakelen van dit scenario. Eerste gebruikers zich kunnen aanmelden bij Windows 10-apparaten via [Azure Active Directory Join](../device-management-introduction.md). U kunt ook gebruikers zich kunnen aanmelden bij Windows-apparaten die zijn toegevoegd aan een on-premises Active Directory die is ingeschakeld voor eenmalige aanmelding met Azure AD via-een [Active Directory Federation Services (AD FS)](../hybrid/plan-connect-user-signin.md) implementatie. Beide opties moeten u de stappen in de volgende zelfstudie voor eenmalige aanmelding tussen Azure AD inschakelen uitvoert en G Suite.

6. **Fout: Ongeldig e-mailadres**

    Het e-kenmerk is voor deze installatie vereist voor de gebruikers kunnen aanmelden. Dit kenmerk kan niet handmatig worden ingesteld.

    Het kenmerk e-mailbericht wordt automatisch ingevuld voor elke gebruiker met een geldige licentie voor Exchange. Als de gebruiker is geen e-mail kunnen ontvangen, wordt deze fout ontvangen als de toepassing nodig heeft om op te halen van dit kenmerk om toegang te geven.

    U kunt naar portal.office.com met een beheerdersaccount, gaan en vervolgens klikt u in het beheercentrum, facturering, abonnementen, selecteert u uw Office 365-abonnement en vervolgens klikt u op aan gebruikers toewijzen, selecteer de gebruikers die u wilt controleren van hun abonnement en klik in het rechter deelvenster op licenties voor bewerken.

    Zodra de O365-licentie is toegewezen, is het duurt enkele minuten worden toegepast. Hierna het kenmerk user.mail wordt automatisch ingevuld worden en het probleem moet worden omgezet.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. G Suite uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-g-suite-from-the-gallery"></a>G Suite uit de galerie toe te voegen

Voor het configureren van de integratie van G Suite in Azure AD, moet u G Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen G Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **G Suite**, selecteer **G Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![G Suite in de lijst met resultaten](./media/google-apps-tutorial/tutorial_gsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met G Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in G Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in G Suite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met G Suite, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker G Suite](#creating-a-g-suite-test-user)**  : als u wilt een equivalent van Britta Simon in G Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw G Suite-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met G Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **G Suite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![G Suite-domein en URL's, eenmalige aanmelding informatie](./media/google-apps-tutorial/tutorial_gsuite_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [G Suite-Client-ondersteuningsteam](https://www.google.com/contact/) om deze waarden te verkrijgen.

5. G Suite-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    a. Klik op **bewerken** te openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Uit de **bronkenmerk** aanbieden, selelct waarde van het kenmerk.

    c. Klik op **Opslaan**.

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/google-apps-tutorial/tutorial_gsuite_certificate.png) 

6. Op de **instellen van G Suite** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![Configuratie van de G-Suite](common/configuresection.png)

9. Open een nieuw tabblad in uw browser en meld u aan bij de [G Suite-beheerconsole](http://admin.google.com/) met uw administrator-account.

10. Klik op **Security**. Als u de koppeling niet ziet, kan deze worden verborgen in de **meer besturingselementen** menu aan de onderkant van het scherm.

    ![Klik op 'Security' (Beveiliging).][10]

11. Op de **Security** pagina, klikt u op **instellen van eenmalige aanmelding (SSO).**

    ![Klik op eenmalige aanmelding.][11]

12. Voer de volgende configuratiewijzigingen:

    ![Eenmalige aanmelding configureren][12]

    a. Selecteer **Setup eenmalige aanmelding met externe id-provider**.

    b. In de **aanmelden pagina-URL** veld in G Suite, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL voor afmelden pagina** veld in G Suite, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    d. In de **de URL van het wachtwoord wijzigen** veld in G Suite, plak de waarde van **de URL van het wachtwoord wijzigen** die u hebt gekopieerd vanuit Azure portal.

    e. In G Suite, voor de **verificatiecertificaat**, upload het certificaat dat u hebt gedownload vanuit Azure portal.

    f. Selecteer **gebruik van een bepaalde uitgever domein**.

    g. Klik op **wijzigingen opslaan**.

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

### <a name="creating-a-g-suite-test-user"></a>Het maken van een testgebruiker G Suite

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in G Suite-Software. G Suite biedt ondersteuning voor automatische inrichting, dit is standaard ingeschakeld. Er is geen actie voor u in deze sectie. Als een gebruiker nog niet in G Suite-Software bestaat, wordt een nieuwe resourcegroep wordt gemaakt wanneer u probeert te krijgen tot G Suite-Software.

> [!NOTE]
> Zorg ervoor dat de gebruiker al in G Suite bestaat als inrichten in Azure AD niet is ingeschakeld voordat u test eenmalige aanmelding.

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Google-ondersteuningsteam](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan G Suite.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **G Suite**.

    ![Eenmalige aanmelding configureren](./media/google-apps-tutorial/tutorial_gsuite_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel G Suite in het toegangsvenster, u moet u automatisch aangemeld bij uw G Suite-toepassing.
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
<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png
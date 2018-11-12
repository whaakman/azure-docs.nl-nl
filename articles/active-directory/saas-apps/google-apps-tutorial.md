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
ms.date: 11/06/2018
ms.author: jeedes
ms.openlocfilehash: 4ed571d34e5df67f556f39b898e7ae5efc06a3e1
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288931"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Zelfstudie: Azure Active Directory-integratie met G Suite

In deze zelfstudie leert u over het integreren van G Suite met Azure Active Directory (Azure AD).

G Suite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot G Suite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij G Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met G Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een G Suite eenmalige aanmelding ingeschakeld abonnement
- Een abonnement op Google Apps of Google Cloud Platform-abonnement.

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

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

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. G Suite uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-g-suite-from-the-gallery"></a>G Suite uit de galerie toe te voegen

Voor het configureren van de integratie van G Suite in Azure AD, moet u G Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen G Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/google-apps-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/google-apps-tutorial/a_select_app.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/google-apps-tutorial/a_new_app.png)

4. Typ in het zoekvak **G Suite**, selecteer **G Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![image](./media/google-apps-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met G Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in G Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in G Suite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met G Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker G Suite](#create-a-g-suite-test-user)**  : als u wilt een equivalent van Britta Simon in G Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw G Suite-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met G Suite, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **G Suite** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/google-apps-tutorial/b1_b2_select_sso.png)

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/google-apps-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/google-apps-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/google-apps-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    ![image](./media/google-apps-tutorial/b1-domains_and_urls.png)

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [G Suite-Client-ondersteuningsteam](https://www.google.com/contact/) om deze waarden te verkrijgen.

6. G Suite-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

7. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    a. Klik op **bewerken** te openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Uit de **bronkenmerk** aanbieden, selelct waarde van het kenmerk.

    c. Klik op **Opslaan**.

8. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van het juiste certificaat volgens uw vereiste en sla deze op uw computer.

    ![image](./media/google-apps-tutorial/certificatebase64.png)

9. Op de **instellen van G Suite** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/google-apps-tutorial/d1_saml.png) 

10. Open een nieuw tabblad in uw browser en meld u aan bij de [G Suite-beheerconsole](http://admin.google.com/) met uw administrator-account.

11. Klik op **Security**. Als u de koppeling niet ziet, kan deze worden verborgen in de **meer besturingselementen** menu aan de onderkant van het scherm.

    ![Klik op 'Security' (Beveiliging).][10]

12. Op de **Security** pagina, klikt u op **instellen van eenmalige aanmelding (SSO).**

    ![Klik op eenmalige aanmelding.][11]

13. Voer de volgende configuratiewijzigingen:

    ![Eenmalige aanmelding configureren][12]

    a. Selecteer **Setup eenmalige aanmelding met externe id-provider**.

    b. In de **aanmelden pagina-URL** veld in G Suite, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL voor afmelden pagina** veld in G Suite, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    d. In de **de URL van het wachtwoord wijzigen** veld in G Suite, plak de waarde van **de URL van het wachtwoord wijzigen** die u hebt gekopieerd vanuit Azure portal.

    e. In G Suite, voor de **verificatiecertificaat**, upload het certificaat dat u hebt gedownload vanuit Azure portal.

    f. Selecteer **gebruik van een bepaalde uitgever domein**.

    g. Klik op **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/google-apps-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/google-apps-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/google-apps-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-g-suite-test-user"></a>Maak een testgebruiker G Suite

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in G Suite-Software. G Suite biedt ondersteuning voor automatische inrichting, dit is standaard ingeschakeld. Er is geen actie voor u in deze sectie. Als een gebruiker nog niet in G Suite-Software bestaat, wordt een nieuwe resourcegroep wordt gemaakt wanneer u probeert te krijgen tot G Suite-Software.

> [!NOTE]
> Zorg ervoor dat de gebruiker al in G Suite bestaat als inrichten in Azure AD niet is ingeschakeld voordat u test eenmalige aanmelding.

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Google-ondersteuningsteam](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan G Suite.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/google-apps-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **G Suite**.

    ![image](./media/google-apps-tutorial/d_all_proapplications.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/google-apps-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/google-apps-tutorial/d_assign_user.png)

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel G Suite in het toegangsvenster, u moet u automatisch aangemeld bij uw G Suite-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png
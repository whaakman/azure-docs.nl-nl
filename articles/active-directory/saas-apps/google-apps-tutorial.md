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
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: 9281c5fe04e3f29d1e3425edb4841dfaca1cd9e2
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294829"
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
1.  **V: bevat ondersteuning voor deze integratie Google Cloud Platform SSO-integratie met Azure AD?**
    
    A: Ja. Google Cloud Platform en Google Apps delen de dezelfde verificatie-platform. Hiervoor de GCP-integratie moet u dus de eenmalige aanmelding met Google Apps configureren.


1. **V: Chromebooks en andere apparaten Chrome compatibel zijn met Azure AD eenmalige aanmelding?**
   
    Antwoord: Ja, gebruikers kunnen zich aanmelden bij hun Chromebook-apparaten met behulp van hun Azure AD-referenties. Raadpleeg deze [G Suite-ondersteuningsartikel](https://support.google.com/chrome/a/answer/6060880) voor meer informatie over waarom gebruikers mogelijk wordt gevraagd om referenties twee keer.

1. **V: als ik eenmalige aanmelding inschakelen, gebruikers zich met hun Azure AD-referenties aanmelden bij een Google-product, zoals Google Classroom, GMail, Google Drive, YouTube, enzovoort?**
   
    A: Ja, afhankelijk van [welke G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) u wilt in- of uitschakelen voor uw organisatie.

1. **V: kan ik eenmalige aanmelding voor slechts een subset van mijn gebruikers G Suite inschakelen?**
   
    A: Nee, onmiddellijk op eenmalige aanmelding in te schakelen, is uw G Suite-gebruikers te verifiëren met hun Azure AD-referenties vereist. Omdat G Suite biedt geen ondersteuning voor meerdere id-providers, de id-provider voor uw G Suite-omgeving kan Azure AD zijn of Google, maar niet beide op hetzelfde moment.

1. **Vraag: als een gebruiker is aangemeld via Windows, worden dat ze automatisch worden geverifieerd met G Suite zonder wordt gevraagd om een wachtwoord?**
   
    Antwoord: Er zijn twee opties voor het inschakelen van dit scenario. Eerste gebruikers zich kunnen aanmelden bij Windows 10-apparaten via [Azure Active Directory Join](../device-management-introduction.md). U kunt ook gebruikers zich kunnen aanmelden bij Windows-apparaten die zijn toegevoegd aan een on-premises Active Directory die is ingeschakeld voor eenmalige aanmelding met Azure AD via-een [Active Directory Federation Services (AD FS)](../hybrid/plan-connect-user-signin.md) implementatie. Beide opties moeten u de stappen in de volgende zelfstudie voor eenmalige aanmelding tussen Azure AD inschakelen uitvoert en G Suite.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. G Suite uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-g-suite-from-the-gallery"></a>G Suite uit de galerie toe te voegen
Voor het configureren van de integratie van G Suite in Azure AD, moet u G Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen G Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **G Suite**, selecteer **G Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![G Suite in de lijst met resultaten](./media/google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met G Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in G Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in G Suite tot stand worden gebracht.

In G Suite, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met G Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker G Suite](#create-a-g-suite-test-user)**  : als u wilt een equivalent van Britta Simon in G Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw G Suite-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met G Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **G Suite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/google-apps-tutorial/tutorial_googleapps_samlbase.png)

1. Op de **G Suite-domein en URL's** sectie, als u configureren wilt voor de **Gmail** de volgende stappen uitvoeren:

    ![G Suite-domein en URL's, eenmalige aanmelding informatie](./media/google-apps-tutorial/tutorial_googleapps_urlgmail.png)

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

1. Op de **G Suite-domein en URL's** sectie, als u configureren wilt voor de **Google Cloud Platform** de volgende stappen uitvoeren:

    ![G Suite-domein en URL's, eenmalige aanmelding informatie](./media/google-apps-tutorial/tutorial_googleapps_url1.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [G Suite-Client-ondersteuningsteam](https://www.google.com/contact/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/google-apps-tutorial/tutorial_googleapps_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/google-apps-tutorial/tutorial_general_400.png)

1. Op de **configuratie van de G Suite** sectie, klikt u op **G Suite configureren** openen **aanmelding configureren** venster. Kopiëren de **URL van de afmelding, Single Sign-On Service URL voor SAML- en wijzigen wachtwoord URL** uit de **Naslaggids sectie.**

    ![Configuratie van de G-Suite](./media/google-apps-tutorial/tutorial_googleapps_configure.png) 

1. Open een nieuw tabblad in uw browser en meld u aan bij de [G Suite-beheerconsole](http://admin.google.com/) met uw administrator-account.

1. Klik op **Security**. Als u de koppeling niet ziet, kan deze worden verborgen in de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Klik op 'Security' (Beveiliging).][10]

1. Op de **Security** pagina, klikt u op **instellen van eenmalige aanmelding (SSO).**
   
    ![Klik op eenmalige aanmelding.][11]

1. Voer de volgende configuratiewijzigingen:
   
    ![Eenmalige aanmelding configureren][12]
   
    a. Selecteer **Setup eenmalige aanmelding met externe id-provider**.

    b. In de **aanmelden pagina-URL** veld in G Suite, plak de waarde van **Single Sign-On Service URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL voor afmelden pagina** veld in G Suite, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal. 

    d. In de **de URL van het wachtwoord wijzigen** veld in G Suite, plak de waarde van **de URL van het wachtwoord wijzigen** die u hebt gekopieerd vanuit Azure portal. 

    e. In G Suite, voor de **verificatiecertificaat**, upload het certificaat dat u hebt gedownload vanuit Azure portal.

    f. Selecteer **gebruik van een bepaalde uitgever domein**.

    g. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/google-apps-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/google-apps-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/google-apps-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/google-apps-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-g-suite-test-user"></a>Maak een testgebruiker G Suite

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in G Suite-Software. G Suite biedt ondersteuning voor automatische inrichting, dit is standaard ingeschakeld. Er is geen actie voor u in deze sectie. Als een gebruiker nog niet in G Suite-Software bestaat, wordt een nieuwe resourcegroep wordt gemaakt wanneer u probeert te krijgen tot G Suite-Software.

>[!NOTE] 
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Google-ondersteuningsteam](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan G Suite.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met G Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **G Suite**.

    ![De G Suite-koppeling in de lijst met toepassingen](./media/google-apps-tutorial/tutorial_googleapps_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel G Suite in het toegangsvenster, u moet u automatisch aangemeld bij uw G Suite-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/googleapps-tutorial/tutorial_general_01.png
[2]: ./media/googleapps-tutorial/tutorial_general_02.png
[3]: ./media/googleapps-tutorial/tutorial_general_03.png
[4]: ./media/googleapps-tutorial/tutorial_general_04.png

[100]: ./media/googleapps-tutorial/tutorial_general_100.png

[200]: ./media/googleapps-tutorial/tutorial_general_200.png
[201]: ./media/googleapps-tutorial/tutorial_general_201.png
[202]: ./media/googleapps-tutorial/tutorial_general_202.png
[203]: ./media/googleapps-tutorial/tutorial_general_203.png
[10]: ./media/googleapps-tutorial/gapps-security.png
[11]: ./media/googleapps-tutorial/security-gapps.png
[12]: ./media/googleapps-tutorial/gapps-sso-config.png


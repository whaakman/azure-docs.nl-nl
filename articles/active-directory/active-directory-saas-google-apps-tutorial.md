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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: f785a74d5cab7c2fbfb0e2e1036dfddbb0f4838b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34345813"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Zelfstudie: Azure Active Directory-integratie met G-Suite

In deze zelfstudie leert u hoe G Suite integreren met Azure Active Directory (Azure AD).

G Suite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot G Suite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij G Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Suite G, moet u de volgende items:

- Een Azure AD-abonnement
- Een G Suite eenmalige aanmelding ingeschakeld abonnement
- Een abonnement van Google Apps of Google Cloud Platform-abonnement.

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
1.  **V: biedt deze ondersteuning voor integratie Google Cloud Platform SSO-integratie met Azure AD?**
    
    A: Ja. Google Cloud Platform en Google Apps delen hetzelfde platform voor verificatie. Hiertoe de GCP-integratie moet u dus de eenmalige aanmelding configureren met Google Apps.


2. **V: Chromebooks en andere apparaten Chrome compatibel zijn met eenmalige aanmelding Azure AD?**
   
    A: Ja, kunnen gebruikers zich aanmelden bij hun Chromebook apparaten met hun Azure AD-referenties. Zie dit [G Suite support-artikel](https://support.google.com/chrome/a/answer/6060880) voor informatie over gebruikers mogelijk om wordt gevraagd referenties twee keer.

3. **V: als ik eenmalige aanmelding inschakelen, wordt gebruikers mogelijk hun Azure AD-referenties gebruiken om u te melden bij een Google-product, zoals Google leslokaal, GMail, Google Drive, YouTube, enzovoort?**
   
    A: Ja, afhankelijk van [welke Suite G](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) u wilt in- of uitschakelen voor uw organisatie.

4. **V: kan ik eenmalige aanmelding voor alleen een subset van mijn gebruikers G Suite inschakelen?**
   
    A: niet onmiddellijk bij eenmalige aanmelding inschakelen, is uw G Suite-gebruikers te verifiëren met hun Azure AD-referenties vereist. Omdat G Suite biedt geen ondersteuning voor meerdere id-providers, de id-provider voor uw omgeving G Suite kan Azure AD zijn of Google-- maar niet beide op hetzelfde moment.

5. **V: als een gebruiker is aangemeld via Windows, worden dat ze automatisch een verificatie G Suite zonder gevraagd om een wachtwoord?**
   
    A: Er zijn twee opties voor het inschakelen van dit scenario. Eerst gebruikers kunnen aanmelden bij Windows 10-apparaten via [Azure Active Directory Join](active-directory-azureadjoin-overview.md). U kunt ook gebruikers kunnen aanmelden bij Windows-apparaten die zijn lid van een domein in een lokale Active Directory die is ingeschakeld voor eenmalige aanmelding bij Azure AD via een [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) implementatie. In beide gevallen moet u de stappen uitvoeren in de volgende zelfstudie voor eenmalige aanmelding tussen Azure AD inschakelen en G Suite.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. G Suite uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-g-suite-from-the-gallery"></a>G Suite uit de galerie toevoegen
Voor het configureren van de integratie van G Suite in Azure AD, moet u G Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen G Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **G Suite**, selecteer **G Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![G Suite in de lijst met resultaten](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met G Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in G Suite is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Suite G tot stand worden gebracht.

In de Suite G, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Suite G, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker G Suite](#create-a-g-suite-test-user)**  - G-pakket dat is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing G Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met Suite G, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **G Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_samlbase.png)

3. Op de **G Suite domein en de URL's** sectie als u configureren wilt voor de **Gmail** de volgende stappen uitvoeren:

    ![G Suite domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_urlgmail.png)

    a. In de **aanmeldings-URL** textbox, typ een URL-URL met het volgende patroon volgen: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [G Suite Client ondersteuningsteam](https://www.google.com/contact/) ophalen van deze waarden.

4. Op de **G Suite domein en de URL's** sectie als u configureren wilt voor de **Google Cloud Platform** de volgende stappen uitvoeren:

    ![G Suite domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_url1.png)

    a. In de **aanmeldings-URL** textbox, typ een URL-URL met het volgende patroon volgen: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [G Suite Client ondersteuningsteam](https://www.google.com/contact/) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-googleapps-tutorial/tutorial_general_400.png)

7. Op de **configuratie van de Suite G** sectie, klikt u op **G Suite configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML Single Sign-On Service-URL en wijzigen wachtwoord URL** van de **Naslaggids punt.**

    ![Configuratie van de Suite G](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_configure.png) 

8. Een nieuw tabblad openen in uw browser en meld u aan bij de [G Suite-beheerconsole](http://admin.google.com/) met uw administrator-account.

9. Klik op **beveiliging**. Als u de koppeling niet ziet, kan het verborgen onder de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Klik op 'Security' (Beveiliging).][10]

10. Op de **beveiliging** pagina, klikt u op **instellen van eenmalige aanmelding (SSO).**
   
    ![Klik op eenmalige aanmelding.][11]

11. Voer de volgende configuratiewijzigingen:
   
    ![Eenmalige aanmelding configureren][12]
   
    a. Selecteer **Setup-SSO met derden identiteitsprovider**.

    b. In de **aanmelden pagina-URL** veld in de Suite G, plak de waarde van **Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    c. In de **afmelden pagina-URL** veld in de Suite G, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal. 

    d. In de **URL voor wachtwoord wijzigen** veld in de Suite G, plak de waarde van **wijzigen wachtwoord URL** die u hebt gekopieerd vanuit Azure-portal. 

    e. In de Suite G, voor de **verificatiecertificaat**, upload het certificaat dat u hebt gedownload vanuit Azure-portal.

    f. Selecteer **gebruik van de uitgever van een domein specifieke**.

    g. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-googleapps-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-googleapps-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-googleapps-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-googleapps-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-g-suite-test-user"></a>Maak een testgebruiker G-Suite

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in G Suite-Software. G Suite ondersteunt automatische inrichting, dit is standaard ingeschakeld. Er is geen actie voor u in deze sectie. Als een gebruiker in G Suite Software nog niet bestaat, wordt een nieuwe gemaakt wanneer u probeert te krijgen tot G Suite-Software.

>[!NOTE] 
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Google ondersteuningsteam](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon toegang verlenen aan G Suite gebruikt Azure eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon G Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **G Suite**.

    ![De G-Suite-koppeling in de lijst met toepassingen](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel G Suite in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing G Suite.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-googleapps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-googleapps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-googleapps-tutorial/gapps-sso-config.png


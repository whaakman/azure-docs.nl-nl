---
title: 'Zelfstudie: Azure Active Directory-integratie met Google Apps in Azure | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Google Apps.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Zelfstudie: Azure Active Directory-integratie met Google Apps

In deze zelfstudie leert u hoe Google Apps integreren met Azure Active Directory (Azure AD).

Google Apps integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Google Apps heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Google Apps (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Google Apps, moet u de volgende items:

- Een Azure AD-abonnement
- Een Google Apps eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Zelfstudievideo
Het inschakelen van eenmalige aanmelding met Google Apps 2 minuten:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
1. **V: Chromebooks en andere apparaten Chrome compatibel zijn met eenmalige aanmelding Azure AD?**
   
    A: Ja, kunnen gebruikers zich aanmelden bij hun Chromebook apparaten met hun Azure AD-referenties. Zie dit [Google Apps ondersteunen artikel](https://support.google.com/chrome/a/answer/6060880) voor informatie over gebruikers mogelijk om wordt gevraagd referenties twee keer.

2. **V: als ik eenmalige aanmelding inschakelen, wordt gebruikers mogelijk hun Azure AD-referenties gebruiken om u te melden bij een Google-product, zoals Google leslokaal, GMail, Google Drive, YouTube, enzovoort?**
   
    A: Ja, afhankelijk van [welke Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) u wilt in- of uitschakelen voor uw organisatie.

3. **V: kan ik eenmalige aanmelding voor alleen een subset van mijn gebruikers Google Apps inschakelen?**
   
    A: niet onmiddellijk bij eenmalige aanmelding inschakelen, moet uw Google Apps-gebruikers te verifiëren met hun Azure AD-referenties. Omdat Google Apps biedt geen ondersteuning voor meerdere id-providers, de id-provider voor uw Google Apps-omgeving kan Azure AD zijn of Google-- maar niet beide op hetzelfde moment.

4. **V: als een gebruiker is aangemeld via Windows, worden dat ze automatisch een verificatie met Google Apps zonder gevraagd om een wachtwoord?**
   
    A: Er zijn twee opties voor het inschakelen van dit scenario. Eerst gebruikers kunnen aanmelden bij Windows 10-apparaten via [Azure Active Directory Join](active-directory-azureadjoin-overview.md). U kunt ook gebruikers kunnen aanmelden bij Windows-apparaten die zijn lid van een domein in een lokale Active Directory die is ingeschakeld voor eenmalige aanmelding bij Azure AD via een [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) implementatie. In beide gevallen moet u de stappen uitvoeren in de volgende zelfstudie voor eenmalige aanmelding tussen Azure AD inschakelen en Google Apps.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Google Apps uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-google-apps-from-the-gallery"></a>Google Apps uit de galerie toevoegen
Voor het configureren van de integratie van Google Apps in Azure AD, moet u Google Apps uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Google Apps uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Google Apps**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. Selecteer in het deelvenster resultaten **Google Apps**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Google Apps op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Google Apps in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Google Apps tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Google Apps.

Om te configureren en testen van Azure AD eenmalige aanmelding met Google Apps, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Google Apps](#creating-a-google-apps-test-user)**  - Google Apps die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Google Apps configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Google Apps, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Google Apps** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Op de **Google Apps Domain en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Deze waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met de [Google ondersteuningsteam](https://www.google.com/contact/).
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaat op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. Op de **Google Apps-configuratie** sectie, klikt u op **Google Apps configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML Single Sign-On Service-URL en wijzigen wachtwoord URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. Een nieuw tabblad openen in uw browser en meld u aan bij de [Google Apps-beheerconsole](http://admin.google.com/) met uw administrator-account.

8. Klik op **beveiliging**. Als u de koppeling niet ziet, kan het verborgen onder de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Klik op 'Security' (Beveiliging).][10]

9. Op de **beveiliging** pagina, klikt u op **instellen van eenmalige aanmelding (SSO).**
   
    ![Klik op eenmalige aanmelding.][11]

10. Voer de volgende configuratiewijzigingen:
   
    ![Eenmalige aanmelding configureren][12]
   
    a. Selecteer **Setup-SSO met derden identiteitsprovider**.

    b. In de **aanmelden pagina-URL** veld in Google Apps, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    c. In de **afmelden pagina-URL** veld in Google Apps, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal. 

    d. In de **URL voor wachtwoord wijzigen** veld in Google Apps, plak de waarde van **wijzigen wachtwoord URL**, die u hebt gekopieerd vanuit Azure-portal. 

    e. In Google Apps voor de **verificatiecertificaat**, upload het certificaat dat u hebt gedownload vanuit Azure-portal.

    f. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-google-apps-test-user"></a>Een testgebruiker Google Apps maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Google Apps Software. Google Apps ondersteunt automatische inrichting, dit is standaard ingeschakeld. Er is geen actie voor u in deze sectie. Als een gebruiker in Google Apps Software nog niet bestaat, wordt een nieuwe gemaakt wanneer u probeert te krijgen tot Software voor Google Apps.

>[!NOTE] 
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Google ondersteuningsteam](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Google Apps.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Google Apps, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Google Apps**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie voor het testen van uw instellingen voor eenmalige aanmelding, opent u het toegangsvenster op [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), meldt u zich bij de testaccount en klikt u op **Google Apps** tegel in het deelvenster toegang.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png
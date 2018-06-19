---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe ervaring Manager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe ervaring Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4904d9b0fc0746bc915477bb0ac83a0083ca198f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35914952"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Zelfstudie: Azure Active Directory-integratie met Adobe ervaring Manager

In deze zelfstudie leert u hoe Adobe ervaring Manager integreren met Azure Active Directory (Azure AD).

Adobe ervaring Manager integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Adobe ervaring Manager.
- U kunt uw gebruikers automatisch ophalen aangemeld Adobe ervaring Manager met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met Adobe ervaring als Manager wilt configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een Adobe ervaring Manager eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u een proefabonnement Azure AD-omgeving geen [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe ervaring Manager uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Adobe ervaring Manager uit de galerie toevoegen
Voor het configureren van de integratie van Adobe ervaring Manager in Azure AD, moet u Adobe ervaring Manager uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Adobe ervaring Manager uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Adobe ervaring Manager**. Selecteer **Adobe ervaring Manager** vanuit het deelvenster resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Adobe ervaring Manager in de lijst met resultaten](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u gegevens kunt configureren en testen eenmalige aanmelding Azure AD met Adobe ervaring Manager op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de equivalente gebruiker in Adobe ervaring Manager is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker maken in Adobe ervaring Manager.

De waarde in Adobe ervaring Manager geven **gebruikersnaam** dezelfde waarde voor de **gebruikersnaam** in Azure AD. U kunt de koppeling tussen de twee gebruikers nu hebt vastgesteld. 

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Adobe ervaring Manager, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een testgebruiker Adobe ervaring Manager](#create-an-adobe-experience-manager-test-user) hebben een equivalent van Britta Simon in Adobe ervaring Manager die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Adobe ervaring Manager.

**Azure AD eenmalige aanmelding met Adobe ervaring als Manager wilt configureren, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Adobe ervaring Manager** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. In de **Adobe ervaring Manager-domein en URL's** sectie, voert de volgende stappen uit als u wilt configureren in de app **IdP** modus:

    ![Adobe ervaring Manager-domein en URL's van eenmalige aanmelding informatie](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. In de **id** typt u een unieke waarde die u definieert op uw AEM-server. 

    b. In de **antwoord-URL** vak een URL met het volgende patroon volgen: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke identificatie en antwoord-URL. Als u deze waarden, neem contact op met de [Adobe ervaring Manager ondersteuningsteam](https://helpx.adobe.com/support/experience-manager.html).
 
4. Controleer **weergeven geavanceerde instellingen voor URL**. Vervolgens voert de volgende stappen uit als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Adobe ervaring Manager-domein en URL's van eenmalige aanmelding informatie](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    In de **aanmelding op URL** vak uw Adobe ervaring Manager server-URL. 

5. In de **SAML-certificaat voor ondertekening van** sectie **certificaat (Base64)**. Sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Selecteer de configuratie van eenmalige aanmelding om venster te openen in de configuratiesectie van Adobe ervaring Manager, **Adobe ervaring Manager configureren**. Kopiëren de **SAML-Service-URL aanmelding**, **SAML entiteit-ID**, en **Sign-Out ID** uit de sectie eenvoudig kunt raadplegen.

    ![Configuratie sectie koppeling](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Open in een ander browservenster de **Adobe ervaring Manager** -beheerportal.

9. Selecteer **instellingen** > **beveiliging** > **gebruikers**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Selecteer **beheerder** of een andere relevante gebruiker.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Selecteer **Accountinstellingen** > **TrustStore beheren**.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Onder **certificaat uit het CER-bestand toevoegen**, klikt u op **certificaatbestand selecteren**. Blader naar en selecteer het certificaatbestand dat u al hebt gedownload van de Azure-portal.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Het certificaat is toegevoegd aan de TrustStore. Noteer de alias van het certificaat.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Op de **gebruikers** pagina **verificatieservice**.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Selecteer **Accountinstellingen** > **maken/beheren KeyStore**. KeyStore maken door het opgeven van een wachtwoord.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Ga terug naar het scherm van de beheerder. Selecteer vervolgens **instellingen** > **Operations** > **webconsole**.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Hiermee opent u de configuratiepagina.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Zoeken naar **Adobe Granite SAML 2.0-verificatie Handler**. Selecteer vervolgens de **toevoegen** pictogram.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. De volgende acties uitvoeren op deze pagina.

    ![Knop Single Sign-On opslaan configureren](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. In de **pad** Voer **/**.

    b. In de **IDP URL** en voer de **SAML-Service-URL aanmelding** waarde die u hebt gekopieerd uit de Azure-portal.

    c. In de **IDP certificaat Alias** en voer de **certificaat Alias** waarde die u hebt toegevoegd in TrustStore.

    d. In de **beveiligings-ID opgegeven entiteit** Geef de unieke **SAML entiteit-ID** waarde die u hebt geconfigureerd in de Azure-portal.

    e. In de **Assertion Consumer Service-URL** en voer de **antwoord-URL** waarde die u hebt geconfigureerd in de Azure-portal.

    f. In de **wachtwoord of sleutel Store** en voer de **wachtwoord** die u instelt in de sleutelopslag.

    g. In de **kenmerk-ID van gebruiker** en voer de **naam-ID** of een andere gebruikersnaam die relevant is in uw geval.

    h. Selecteer **automatisch CRX gebruikers**.

    i. In de **afmelding URL** Geef de unieke **Sign-Out URL** waarde die u hebt verkregen via de Azure-portal.

    j. Selecteer **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad. Vervolgens toegang krijgen tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster aan de bovenkant van de **alle gebruikers** dialoogvenster, **toevoegen**.

    ![De knop toevoegen](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Een testgebruiker Adobe ervaring Manager maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in Adobe ervaring Manager. Als u hebt geselecteerd de **automatisch CRX gebruikers** optie gebruikers worden automatisch gemaakt wanneer u bent geverifieerd. 

Als u wilt dat gebruikers dat handmatig wilt maken, samen met de [Adobe ervaring Manager ondersteuningsteam](https://helpx.adobe.com/support/experience-manager.html) om toe te voegen de gebruikers van het platform Adobe ervaring Manager. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot Adobe ervaring Manager.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon Adobe ervaring Manager, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Ga vervolgens naar de directoryweergave, selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Adobe ervaring Manager**.

    ![De koppeling Adobe ervaring Manager in de lijst met toepassingen](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel Adobe ervaring Manager in het deelvenster toegang selecteert, moet u ophalen automatisch aangemeld bij uw toepassing Adobe ervaring Manager.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/adobeexperiencemanager-tutorial/tutorial_general_203.png


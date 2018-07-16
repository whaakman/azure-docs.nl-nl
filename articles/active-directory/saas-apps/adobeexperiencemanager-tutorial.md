---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe Experience Manager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe Experience Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 56b392e57809cea0ae93800df39bb9dacd164ce2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054179"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Zelfstudie: Azure Active Directory-integratie met Adobe Experience Manager

In deze zelfstudie leert u hoe u Adobe Experience Manager integreert met Azure Active Directory (Azure AD).

Adobe Experience Manager integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD wie toegang tot Adobe Experience Manager heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld voor Adobe Experience Manager met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe Experience Manager, moet u de volgende items:

- Een Azure AD-abonnement
- Een Adobe Experience Manager eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u een proefversie Azure AD-omgeving, geen [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe Experience Manager uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Adobe Experience Manager uit de galerie toevoegen
Voor het configureren van de integratie van Adobe Experience Manager in Azure AD, moet u Adobe Experience Manager uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Adobe Experience Manager uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Adobe Experience Manager**. Selecteer **Adobe Experience Manager** vanuit het deelvenster met resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Adobe Experience Manager in de lijst met resultaten](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Adobe Experience Manager op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in Adobe Experience Manager is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in Adobe Experience Manager.

De waarde in Adobe Experience Manager geven **gebruikersnaam** dezelfde waarde voor de **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt. 

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Adobe Experience Manager, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Adobe Experience Manager](#create-an-adobe-experience-manager-test-user) hebben een equivalent van Britta Simon in Adobe Experience Manager die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Adobe Experience Manager.

**Voor het configureren van Azure AD eenmalige aanmelding met Adobe Experience Manager, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Adobe Experience Manager** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster de **modus** vervolgkeuzelijst in het menu **SAML gebaseerde aanmelding**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. In de **Adobe Experience Manager-domein en URL's** sectie, voert de volgende stappen uit als u wilt configureren in de app **IdP** modus:

    ![Adobe Experience Manager-domein en URL's, eenmalige aanmelding informatie](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. In de **id** typt u een unieke waarde die u definieert op uw AEM-server. 

    b. In de **antwoord-URL** vak, typ een URL met het volgende patroon: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id en antwoord-URL. Als u deze waarden, neem contact op met de [Adobe Experience Manager ondersteuningsteam](https://helpx.adobe.com/support/experience-manager.html).
 
4. Controleer **geavanceerde URL-instellingen weergeven**. Vervolgens voert de volgende stappen uit als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Adobe Experience Manager-domein en URL's, eenmalige aanmelding informatie](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    In de **aanmelding URL** vak, typ de URL van uw Adobe Experience Manager-server. 

5. In de **SAML-handtekeningcertificaat** sectie, selecteer **certificaat (Base64)**. Sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Als het venster van de aanmeldings-configuratie in de sectie configuratie van Adobe Experience Manager, schakelt u **configureren Adobe Experience Manager**. Kopiëren de **SAML-aanmelding de URL van de Service**, **SAML entiteit-ID**, en **afmelding ID** uit de sectie ter referentie.

    ![Sectie configuratiekoppeling](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Open in een ander browservenster de **Adobe Experience Manager** -beheerportal.

9. Selecteer **instellingen** > **Security** > **gebruikers**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Selecteer **beheerder** of een andere relevante gebruiker.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Selecteer **Accountinstellingen** > **TrustStore beheren**.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Onder **certificaat in CER-bestand toevoegen**, klikt u op **certificaatbestand selecteren**. Blader naar en selecteer het certificaatbestand dat u hebt al gedownload vanuit Azure portal.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. Het certificaat wordt toegevoegd aan de TrustStore. Houd er rekening mee de alias van het certificaat.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Op de **gebruikers** weergeeft, schakelt **verificatieservice**.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Selecteer **Accountinstellingen** > **maken/beheren KeyStore**. KeyStore maken door het opgeven van een wachtwoord.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Ga terug naar het scherm van de beheerder. Selecteer vervolgens **instellingen** > **Operations** > **webconsole**.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Hiermee opent u de configuratiepagina.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Zoek **Adobe Granieten SAML 2.0-verificatie Handler**. Selecteer vervolgens de **toevoegen** pictogram.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. De volgende acties uitvoeren op deze pagina.

    ![Configureren van eenmalige aanmelding opslaan](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. In de **pad** Voer **/**.

    b. In de **IDP URL** voert u de **SAML-aanmelding de URL van de Service** waarde die u hebt gekopieerd uit de Azure-portal.

    c. In de **IDP certificaat Alias** voert u de **certificaat Alias** waarde die u hebt toegevoegd in TrustStore.

    d. In de **beveiligings-ID opgegeven entiteit** voert u de unieke **SAML entiteit-ID** waarde die u hebt geconfigureerd in Azure portal.

    e. In de **URL van de Bevestigingsconsumerservice** voert u de **antwoord-URL** waarde die u hebt geconfigureerd in Azure portal.

    f. In de **wachtwoord van de sleutel Store** voert u de **wachtwoord** die u in KeyStore ingesteld.

    g. In de **kenmerk-ID van gebruiker** voert u de **naam-ID** of een andere gebruikers-ID die relevant is in uw situatie.

    h. Selecteer **Autocreate CRX gebruikers**.

    i. In de **afmeldings-URL van** voert u de unieke **afmelding URL** waarde die u hebt verkregen via de Azure-portal.

    j. Selecteer **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad. Vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie op [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, aan de bovenkant van de **alle gebruikers** in het dialoogvenster, selecteer **toevoegen**.

    ![De knop toevoegen](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Maak een testgebruiker Adobe Experience Manager

In deze sectie maakt u een gebruiker met de naam Britta Simon in Adobe Experience Manager. Als u hebt geselecteerd de **Autocreate CRX gebruikers** optie gebruikers worden automatisch gemaakt na een geslaagde authenticatie. 

Als u wilt dat gebruikers handmatig maken, samen met de [Adobe Experience Manager ondersteuningsteam](https://helpx.adobe.com/support/experience-manager.html) om toe te voegen de gebruikers in het platform, Adobe Experience Manager. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken ze door toegang te verlenen aan Adobe Experience Manager.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Adobe Experience Manager, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Ga vervolgens naar de directoryweergave, selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Adobe Experience Manager**.

    ![De koppeling Adobe Experience Manager in de lijst met toepassingen](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel Adobe Experience Manager in het toegangsvenster selecteert, moet u u automatisch aangemeld bij uw Adobe Experience Manager-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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


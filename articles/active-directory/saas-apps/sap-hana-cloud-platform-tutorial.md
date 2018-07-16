---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Cloud Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 176ed1718818b24866620d86d7eff4d9af1d378f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050871"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform

In deze zelfstudie leert u hoe u SAP Cloud Platform integreren met Azure Active Directory (Azure AD).

SAP Cloud Platform te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SAP Cloud Platform heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP Cloud Platform (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud Platform, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP Cloud Platform eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de SAP Cloud Platform zich één Meld u aan bij de toepassing met behulp van de [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>U moet uw eigen toepassing implementeren of zich abonneert op een toepassing op uw account SAP Cloud Platform voor het testen van eenmalige aanmelding op. In deze zelfstudie wordt een toepassing geïmplementeerd in het account.
> 

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Cloud Platform uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>SAP Cloud Platform uit de galerie toe te voegen
Voor het configureren van de integratie van SAP Cloud Platform in Azure AD, moet u SAP Cloud Platform uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de SAP Cloud Platform uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SAP Cloud Platform**, selecteer **SAP Cloud Platform** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP Cloud Platform in de lijst met resultaten](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAP Cloud Platform op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de SAP Cloud Platform is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAP Cloud Platform tot stand worden gebracht.

In SAP Cloud Platform, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud Platform, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SAP Cloud Platform](#create-a-sap-cloud-platform-test-user)**  : als u wilt een equivalent van Britta Simon hebben in SAP Cloud Platform dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SAP Cloud Platform.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud Platform, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Cloud Platform** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

3. Op de **SAP Cloud Platform-domein en URL's** sectie, voert u de volgende stappen uit:

    ![SAP Cloud Platform-domein en URL's, eenmalige aanmelding informatie](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

    a. In de **aanmelding URL** tekstvak, type de URL die wordt gebruikt door uw gebruikers zich aanmelden bij uw **SAP Cloud Platform** toepassing. Dit is de account-specifieke URL van een beveiligde bron in uw toepassing SAP Cloud Platform. De URL is gebaseerd op het volgende patroon: `https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Dit is de URL in uw toepassing met SAP Cloud Platform waarvoor de gebruiker om te verifiëren.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. In de **id** tekstvak levert u uw SAP Cloud Platform typt u een URL met behulp van een van de volgende patronen: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [SAP Cloud Platform-Client-ondersteuningsteam](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) om aanmeldings-URL en -id te verkrijgen. Antwoord-URL die u kunt ophalen uit de beheersectie vertrouwen die later in de zelfstudie wordt uitgelegd.
    > 
     
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij de SAP Cloud Platform Cockpit op `https://account.<landscape host>.ondemand.com/cockpit`(bijvoorbeeld: https://account.hanatrial.ondemand.com/cockpit).

7. Klik op de **vertrouwen** tabblad.
   
    ![Vertrouwen](./media/sap-hana-cloud-platform-tutorial/ic790800.png "vertrouwen")

8. In de sectie beheer vertrouwen onder **lokale serviceprovider**, voer de volgende stappen uit:

    ![Beheer vertrouwen](./media/sap-hana-cloud-platform-tutorial/ic793931.png "beheer vertrouwen")
   
    a. Klik op **Bewerken**.

    b. Als **configuratietype**, selecteer **aangepaste**.

    c. Als **lokale providernaam**, laat de standaardwaarde staan. Deze waarde kopiëren en plak deze in de **id** veld in de Azure AD-configuratie voor SAP Cloud Platform.

    d. Voor het genereren van een **handtekeningsleutel** en een **certificaat voor ondertekening van** sleutelpaar, klikt u op **sleutelpaar genereren**.

    e. Als **Principal doorgifte**, selecteer **uitgeschakelde**.

    f. Als **Force verificatie**, selecteer **uitgeschakelde**.

    g. Klik op **Opslaan**.

9. Nadat ze zijn opgeslagen de **lokale serviceprovider** instellingen, het volgende om op te halen van de antwoord-URL uitvoeren:
   
    ![Metagegevens ophalen](./media/sap-hana-cloud-platform-tutorial/ic793930.png "metagegevens ophalen")

    a. Het metagegevensbestand van de SAP Cloud Platform downloaden door te klikken op **Ophaalactiviteit voor metagegevens**.

    b. Open het gedownloade SAP Cloud Platform metagegevens-XML-bestand en zoek vervolgens de **ns3:AssertionConsumerService** tag.
 
    c. Kopieer de waarde van de **locatie** kenmerk en plak deze in de **antwoord-URL** veld in de Azure AD-configuratie voor SAP Cloud Platform.

10. Klik op de **vertrouwde id-Provider** tabblad en klik vervolgens op **vertrouwde id-Provider toevoegen**.
   
    ![Beheer vertrouwen](./media/sap-hana-cloud-platform-tutorial/ic790802.png "beheer vertrouwen")
   
    >[!NOTE]
    >Voor het beheren van de lijst met vertrouwde id-providers, moet u het type aangepaste configuratie hebt gekozen in de sectie lokale Service-Provider. Voor het configuratietype standaard hebt u een niet-bewerkbare en impliciete vertrouwensrelatie met de SAP-id-Service. Voor geen hebt u geen vertrouwensrelatie instellingen.
    > 
    > 

11. Klik op de **algemene** tabblad en klik vervolgens op **Bladeren** de van het gedownloade metagegevensbestand te uploaden.
    
    ![Beheer vertrouwen](./media/sap-hana-cloud-platform-tutorial/ic793932.png "beheer vertrouwen")
    
    >[!NOTE]
    >Na het uploaden van het bestand met metagegevens, de waarden voor **-URL met eenmalige aanmelding**, **URL voor eenmalige afmelding**, en **certificaat voor ondertekening van** automatisch worden ingevuld.
    > 
     
12. Klik op het tabblad **Kenmerken**.

13. Op de **kenmerken** tabblad, voert u de volgende stap:
    
    ![Kenmerken](./media/sap-hana-cloud-platform-tutorial/ic790804.png "kenmerken") 

    a. Klik op **Add Assertion-Based kenmerk**, en voeg de volgende kenmerken op basis van een bevestiging:
       
    | Bewering kenmerk | Principal-kenmerk |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |Voornaam |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Achternaam |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-mailen |
   
     >[!NOTE]
     >De configuratie van de kenmerken, is afhankelijk van hoe de toepassing(en) op SCP worden ontwikkeld, dat wil zeggen, welke kenmerken die ze verwachten dat ze in het SAML-antwoord en onder welke naam u (Principal-kenmerk) toegang tot dit kenmerk in de code.
     > 
    
    b. De **kenmerk standaard** is alleen ter illustratie wordt in de schermafbeelding. Het is niet vereist om te maken van het scenario werkt.  
 
    c. De namen en waarden voor **Principal kenmerk** weergegeven in de schermafbeelding afhankelijk van hoe de toepassing wordt ontwikkeld. Het is mogelijk dat uw toepassing is vereist voor andere toewijzingen.

### <a name="assertion-based-groups"></a>Groepen op basis van een verklaring

Als optionele stap, kunt u groepen op basis van een bewering configureren voor uw identiteitsprovider Azure Active Directory.

Met behulp van groepen op de SAP Cloud Platform, kunt u een of meer gebruikers dynamisch toewijzen aan een of meer rollen in uw SAP Cloud Platform-toepassingen, bepaald door de waarden van kenmerken in het SAML 2.0-verklaring. 

Bijvoorbeeld, als de verklaring bevat het kenmerk '*contract = tijdelijke*', kunt u alle betrokken gebruikers worden toegevoegd aan de groep'*tijdelijke*'. De groep '*tijdelijke*' kan een of meer rollen uit een of meer toepassingen die zijn geïmplementeerd in uw account SAP Cloud Platform bevatten.
 
Groepen op basis van een verklaring gebruiken wanneer u veel gebruikers gelijktijdig toewijzen aan een of meer rollen van toepassingen in uw account SAP Cloud Platform. Als u wilt dat slechts een enkele of een klein aantal gebruikers toewijzen aan specifieke rollen, het beste toegewezen rechtstreeks in de '**autorisaties**"tabblad van de SAP Cloud Platform-cockpit.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>Maak een testgebruiker SAP Cloud Platform

Als u wilt dat Azure AD-gebruikers zich aanmelden bij de SAP Cloud Platform, moet u de rollen in de SAP Cloud Platform aan hen toewijzen.

**Als u wilt een rol toewijzen aan een gebruiker, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **SAP Cloud Platform** cockpit.

2. Het volgende doen:
   
    ![Autorisaties](./media/sap-hana-cloud-platform-tutorial/ic790805.png "autorisaties")
   
    a. Klik op **autorisatie**.

    b. Klik op de **gebruikers** tabblad.

    c. In de **gebruiker** tekstvak, typ de e-mailadres van de gebruiker.

    d. Klik op **toewijzen** de gebruiker toewijzen aan een rol.

    e. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot de SAP Cloud Platform.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon SAP Cloud platform, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SAP Cloud Platform**.

    ![De SAP Cloud Platform-koppeling in de lijst met toepassingen](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de SAP Cloud Platform-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SAP Cloud Platform.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_203.png


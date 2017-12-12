---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP-Cloudplatform | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Cloud-Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 309415a68308943f638195303ceb236569519472
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud-Platform

In deze zelfstudie leert u hoe SAP Cloud Platform integreren met Azure Active Directory (Azure AD).

SAP-Cloudplatform integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP Cloud-Platform heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP Cloud Platform (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud-Platform, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP-Cloudplatform eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan de Cloud-Platform SAP kan worden één Meld u aan bij de toepassing met behulp van de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>U moet uw eigen toepassing implementeren of zich abonneert op een toepassing op uw Cloud-Platform SAP-account voor het testen van eenmalige aanmelding op. In deze zelfstudie is een toepassing wordt geïmplementeerd in het account.
> 

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP-Cloudplatform uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>SAP-Cloudplatform uit de galerie toevoegen
Voor het configureren van de integratie van SAP Cloud-Platform in Azure AD, moet u SAP-Cloudplatform uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP-Cloudplatform uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP Cloud Platform**, selecteer **SAP-Cloudplatform** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP Cloud-Platform in de lijst met resultaten](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SAP Cloud Platform op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SAP Cloud Platform is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Cloud-Platform SAP tot stand worden gebracht.

In de Cloud-Platform SAP, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud-Platform, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SAP-Cloudplatform](#create-a-sap-cloud-platform-test-user)**  - SAP Cloud-Platform die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing SAP Cloud-Platform.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud-Platform, moet u de volgende stappen uitvoeren:**

1. In de Azure portal op de **SAP-Cloudplatform** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

3. Op de **SAP Cloud Platform domein en de URL's** sectie, voert u de volgende stappen uit:

    ![SAP Cloud Platform domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

    a. In de **aanmelding op URL** textbox, typ de URL gebruikt door uw gebruikers zich aanmeldt bij uw **SAP-Cloudplatform** toepassing. Dit is de account-specifieke URL van een beveiligde bron in uw toepassing SAP Cloud-Platform. De URL is gebaseerd op het volgende patroon volgen:`https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Dit is de URL in uw Cloud-Platform SAP-toepassing waarvoor de gebruiker te verifiëren.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. In de **id** textbox u uw SAP-Cloudplatform typt u een URL met een van de volgende patronen biedt: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:

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
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [SAP Cloud Platform Client ondersteuningsteam](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) aanmeldings-URL en -id ophalen. Antwoord-URL die u kunt ophalen van sectie in management vertrouwensrelatie die verderop in de zelfstudie wordt beschreven.
    > 
     
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

6. In een ander browservenster geopend, moet u zich aanmelden bij de SAP Cloud Platform Cockpit op `https://account.<landscape host>.ondemand.com/cockpit`(bijvoorbeeld: https://account.hanatrial.ondemand.com/cockpit).

7. Klik op de **vertrouwen** tabblad.
   
    ![Vertrouwen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790800.png "vertrouwen")

8. In de sectie beheer vertrouwen onder **lokale serviceprovider**, voer de volgende stappen uit:

    ![Beheer vertrouwen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793931.png "beheer vertrouwen")
   
    a. Klik op **Bewerken**.

    b. Als **configuratietype**, selecteer **aangepaste**.

    c. Als **lokale providernaam**, accepteer de standaardwaarde. Deze waarde Kopieer en plak deze in de **id** veld in de Azure AD-configuratie voor SAP Cloud-Platform.

    d. Voor het genereren van een **handtekeningsleutel** en een **certificaat voor ondertekening van** sleutelpaar, klikt u op **sleutelpaar genereren**.

    e. Als **Principal doorgeven**, selecteer **uitgeschakelde**.

    f. Als **Force verificatie**, selecteer **uitgeschakelde**.

    g. Klik op **Opslaan**.

9. Nadat ze zijn opgeslagen de **lokale serviceprovider** instellingen, voer de volgende stappen om op te halen van de URL van antwoord:
   
    ![Ophalen van metagegevens](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793930.png "metagegevens ophalen")

    a. Het metagegevensbestand SAP Cloud Platform downloaden door te klikken op **metagegevens ophalen**.

    b. Open het gedownloade SAP Cloud Platform metagegevens-XML-bestand en zoek vervolgens de **ns3:AssertionConsumerService** label.
 
    c. Kopieer de waarde van de **locatie** kenmerk en plak deze in de **antwoord-URL** veld in de Azure AD-configuratie voor SAP Cloud-Platform.

10. Klik op de **identiteitsprovider vertrouwde** tabblad en klik vervolgens op **vertrouwde identiteitsprovider toevoegen**.
   
    ![Beheer vertrouwen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790802.png "beheer vertrouwen")
   
    >[!NOTE]
    >Voor het beheren van de lijst met vertrouwde id-providers, moet u het type aangepaste configuratie hebt gekozen in de sectie lokale-Provider. U hebt een niet-bewerkbare en impliciete vertrouwensrelatie met de SAP-id-Service voor het type standaard configuratie. Voor geen hebt u geen vertrouwensrelatie instellingen.
    > 
    > 

11. Klik op de **algemene** tabblad en klik vervolgens op **Bladeren** het gedownloade metagegevensbestand te uploaden.
    
    ![Beheer vertrouwen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793932.png "beheer vertrouwen")
    
    >[!NOTE]
    >Na het uploaden van het metagegevensbestand, de waarden voor **-URL met eenmalige aanmelding**, **-URL met eenmalige afmelding**, en **certificaat voor ondertekening van** worden automatisch ingevuld.
    > 
     
12. Klik op het tabblad **Kenmerken**.

13. Op de **kenmerken** tabblad, voert u de volgende stap:
    
    ![Kenmerken](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790804.png "kenmerken") 

    a. Klik op **Add Assertion-Based kenmerk**, en voeg vervolgens de volgende kenmerken op basis van een bevestiging:
       
    | Verklaring kenmerk | Principal-kenmerk |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |Voornaam |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Achternaam |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-mailen |
   
     >[!NOTE]
     >De configuratie van de kenmerken is afhankelijk van hoe de toepassing(en) op SCP worden ontwikkeld, dat wil zeggen, welke kenmerken ze in de SAML-reactie verwachten en onder welke naam (Principal kenmerk) toegang tot dit kenmerk in de code.
     > 
    
    b. De **kenmerk Default** in de schermafbeelding is alleen bedoeld ter illustratie. Het is niet vereist voor het maken van het scenario werken.  
 
    c. De namen en waarden voor **Principal kenmerk** weergegeven in de schermafbeelding afhankelijk van hoe de toepassing is ontwikkeld. Het is mogelijk dat uw toepassing andere toewijzingen vereist.

###<a name="assertion-based-groups"></a>Verklaring gebaseerde groepen

U kunt groepen op basis van een bevestiging voor uw Azure Active Directory id-Provider configureren als een optionele stap.

Met behulp van groepen op SAP Cloud-Platform, kunt u een of meer gebruikers dynamisch toewijzen aan een of meer rollen in uw Cloud-Platform SAP-toepassingen, bepaald door de waarden van kenmerken in het SAML 2.0-verklaring. 

Bijvoorbeeld, als de bevestiging bevat het kenmerk '*contract = tijdelijke*', kunt u alle betrokken gebruikers worden toegevoegd aan de groep'*tijdelijke*'. De groep '*tijdelijke*' kan een of meer rollen van een of meer toepassingen die zijn geïmplementeerd in uw account SAP-Cloudplatform bevatten.
 
Groepen op basis van een bewering gebruiken wanneer u veel gebruikers tegelijk toewijzen aan een of meer rollen van toepassingen in uw Cloud-Platform SAP-account wilt maken. Als u toewijzen slechts een enkele of een klein aantal gebruikers aan specifieke rollen wilt, raden wij aan eraan toe te wijzen rechtstreeks in de '**autorisaties**' tabblad van de cockpit SAP Cloud-Platform.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>Maak een testgebruiker SAP Cloud-Platform

Om Azure AD-gebruikers zich aanmelden bij SAP Cloud-Platform inschakelen, moet u rollen in de Cloud-Platform voor SAP aan ze toewijzen.

**Als u wilt een rol toewijzen aan een gebruiker, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **SAP-Cloudplatform** cockpit.

2. Het volgende doen:
   
    ![Autorisaties](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790805.png "autorisaties")
   
    a. Klik op **autorisatie**.

    b. Klik op de **gebruikers** tabblad.

    c. In de **gebruiker** textbox, typ de e-mailadres van de gebruiker.

    d. Klik op **toewijzen** toewijzen aan de gebruiker aan een rol.

    e. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP Cloud-Platform.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAP Cloud-Platform, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP-Cloudplatform**.

    ![De koppeling SAP Cloud-Platform in de lijst met toepassingen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel SAP Cloud-Platform in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SAP Cloud-Platform.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_203.png


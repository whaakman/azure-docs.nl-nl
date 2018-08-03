---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 797be143284566efcefce5ed6c7ded822d5aa97f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438894"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe u Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD).

Amazon Web Services (AWS) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD met toegang op Amazon Web Services (AWS).
- U kunt uw gebruikers automatisch ophalen aangemeld op Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Amazon Web Services (AWS) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amazon Web Services (AWS) uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) uit de galerie toe te voegen
Voor het configureren van de integratie van Amazon Web Services (AWS) in Azure AD, moet u Amazon Web Services (AWS) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amazon Web Services (AWS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Amazon Web Services (AWS)**, selecteer **Amazon Web Services (AWS)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Amazon Web Services (AWS) in de lijst met resultaten](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Amazon Web Services (AWS) op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

In Amazon Web Services (AWS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**  - hebben een equivalent van Britta Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Amazon Web Services (AWS)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

1. Op de **Amazon Web Services (AWS)-domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![Amazon Web Services (AWS)-domein en URL's, eenmalige aanmelding informatie](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

1. De toepassing Amazon Web Services (AWS) wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Single Sign-On attb configureren](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png) 

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >U moet het inrichten van gebruikers in Azure AD om op te halen van alle functies van AWS-Console configureren. Raadpleeg de onderstaande stappen inrichten.

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On addattb configureren](./media/amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** tekstvak typt u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/amazon-web-service-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden voor uw bedrijf Amazon Web Services (AWS) site als administrator.

1. Klik op **AWS Home**.
   
    ![Startpagina van eenmalige aanmelding configureren][11]

1. Klik op **Identity and Access Management**. 
   
    ![Identiteit voor eenmalige aanmelding configureren][12]

1. Klik op **id-Providers**, en klik vervolgens op **Provider maken**. 
   
    ![Provider voor eenmalige aanmelding configureren][13]

1. Op de **Provider configureren** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Dialoogvenster voor eenmalige aanmelding configureren][14]
 
    a. Als **providertype**, selecteer **SAML**.

    b. In de **providernaam** tekstvak typt u de naam van een provider (bijvoorbeeld: *WAAD*).

    c. Het uploaden van uw gedownloade **metagegevensbestand** van Azure-portal klikt u op **bestand kiezen**.

    d. Klik op **volgende stap**.

1. Op de **providergegevens controleren** dialoogvenster pagina, klikt u op **maken**. 
    
    ![Configureren van eenmalige aanmelding controleren][15]

1. Klik op **rollen**, en klik vervolgens op **rol maken**. 
    
    ![Rollen voor eenmalige aanmelding configureren][16]

1. Op de **rol maken** pagina, voert u de volgende stappen uit:  
    
    ![Single Sign-On-vertrouwensrelatie configureren][19] 

    a. Selecteer **SAML 2.0 federation** onder **selecteert u het type van vertrouwde entiteit**.

    b. Onder **kiest u een sectie voor SAML 2.0-Provider**, selecteer de **SAML-provider** u eerder hebt gemaakt (bijvoorbeeld: *WAAD*)

    c. Selecteer **toestaan programmatische en toegang tot de AWS Management Console**.
  
    d. Klik op **volgende: machtigingen**.

1. Op de **beleidsregels voor beveiligingsmachtigingen koppelen** dialoogvenster, u hoeft niet te koppelen van elk beleid. Klik op **volgende: Controleer**.  
    
    ![Single Sign-On-beleid configureren][33]

1. Op de **revisie** dialoogvenster, voer de volgende stappen uit:   
    
    ![Configureren van eenmalige aanmelding controleren][34] 

    a. In de **rolnaam** tekstvak, voer de naam van uw rol.

    b. In de **beschrijving van de rol** tekstvak, de omschrijving.

    c. Klik op **rol maken**.

    d. Maak zo veel functies indien nodig en toe te wijzen aan de id-Provider.

1. AWS-referenties voor service-account gebruiken voor het ophalen van de functies van AWS-account in Azure AD-gebruiker inrichten. Open hiervoor de AWS-console start.

1. Klik op **Services** -> **identiteits- en naleving** -> **IAM**.

    ![het ophalen van de functies van AWS-account](./media/amazon-web-service-tutorial/fetchingrole1.png)

1. Selecteer de **beleid** tabblad in de IAM-sectie.

    ![het ophalen van de functies van AWS-account](./media/amazon-web-service-tutorial/fetchingrole2.png)

1. Een nieuw beleid maken door te klikken op **beleid maken** voor het ophalen van de functies van AWS-account in Azure AD-gebruiker inrichten.

    ![Nieuw beleid maken](./media/amazon-web-service-tutorial/fetchingrole3.png)

1. Maak uw eigen beleid om op te halen van alle functies van AWS-accounts door de volgende stappen uit:

    ![Nieuw beleid maken](./media/amazon-web-service-tutorial/policy1.png)

    a. In de **'Beleid maken'** sectie klikt u op **'JSON'** tabblad.

    b. In het beleidsdocument toevoegen de volgende JSON.
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam:ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Klik op **Review Policy knop** voor het valideren van het beleid.

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy5.png)

1. Definieer de **nieuw beleid** door de volgende stappen uit:

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy2.png)

    a. Geef de **beleidsnaam** als **AzureAD_SSOUserRole_Policy**.

    b. U kunt opgeven **beschrijving** aan het beleid als **dit beleid kunnen ophalen van de functies van AWS-accounts**.
    
    c. Klik op **'-beleid maken'** knop.

1.  Maak een nieuw gebruikersaccount in de AWS IAM-Service door de volgende stappen uit:

    a. Klik op **gebruikers** navigatie in de AWS IAM-console.

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy3.png)
    
    b. Klik op **gebruiker toevoegen** knop een nieuwe gebruiker te maken.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/policy4.png)

    c. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:
    
    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser1.png)
    
    * Voer de naam van de gebruiker als **AzureADRoleManager**.
    
    * Selecteer in het type toegang tot de **toegang op programmeerniveau** optie. Op deze manier de gebruiker kan de API's aanroepen en ophalen van de functies van AWS-account.
    
    * Klik op de **volgende machtigingen** knop in de rechterbenedenhoek.

1. Maak nu een nieuw beleid voor deze gebruiker door de volgende stappen uit:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser2.png)
    
    a. Klik op de **bestaande beleidsregels rechtstreeks koppelen** knop.

    b. Zoeken naar de zojuist gemaakte beleid in de sectie filteren **AzureAD_SSOUserRole_Policy**.
    
    c. Selecteer de **beleid** en klik vervolgens op de **volgende: Controleer** knop.

1.  Het beleid voor de gekoppelde gebruiker bekijken door te voeren stappen te volgen:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser3.png)
    
    a. Controleer de gebruikersnaam, het toegangstype en het beleid dat is toegewezen aan de gebruiker.
    
    b. Klik op de **gebruiker maken** knop in de rechterbenedenhoek om de gebruiker te maken.

1. Download de referenties van de gebruiker van een gebruiker door te voeren stappen te volgen:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser4.png)
    
    a. Kopiëren van de gebruiker **toegang ID sleutel** en **geheime toegangssleutel**.
    
    b. Deze referenties invoeren in Azure AD-gebruiker sectie inrichting om op te halen van de functies van AWS-console.
    
    c. Klik op **sluiten** knop aan de onderkant.

1. Navigeer naar **Gebruikersinrichting** sectie van Amazon Web Services-app in Azure AD-beheerportal.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning.png)

1. Voer de **toegangssleutel** en **geheim** in de **Clientgeheim** en **geheim Token** respectievelijk veld.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning1.png)
    
    a. Voer de toegangssleutel van de AWS-gebruiker in de **clientsecret** veld.
    
    b. Voer het geheim van de gebruiker (AWS) in de **geheim Token** veld.
    
    c. Klik op de **verbinding testen** knop en u moet testen is deze verbinding.

    d. Sla de instelling door te klikken op de **opslaan** bovenaan op de knop.
 
1. Nu Zorg ervoor dat u de Status van inrichting inschakelen **op** in de sectie instellingen door het maken van de switch op en vervolgens te klikken op de **opslaan** bovenaan op de knop.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning2.png)

> [!Note]
> Als u integreren meerdere AWS-accounts naar een Azure-account voor eenmalige aanmelding wilt, raadpleegt u [dit](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artikel. 


### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/amazon-web-service-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/amazon-web-service-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/amazon-web-service-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/amazon-web-service-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Maak een testgebruiker Amazon Web Services (AWS)

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in Amazon Web Services (AWS). Amazon Web Services (AWS) hoeven niet een gebruiker moet worden gemaakt in hun systeem voor eenmalige aanmelding, dus u hoeft hier actie uit te voeren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Amazon Web Services (AWS).

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon op Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Amazon Web Services (AWS)**.

    ![De koppeling Amazon Web Services (AWS) in de lijst met toepassingen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Amazon Web Services (AWS) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Amazon Web Services (AWS). Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

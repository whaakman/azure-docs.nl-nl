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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.openlocfilehash: 8e91fbf0befaef9088e9afaa6e69c0cb29ad4858
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363756"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe u Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD).

Amazon Web Services (AWS) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD met toegang op Amazon Web Services (AWS).
- U kunt uw gebruikers automatisch ophalen aangemeld op Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

U kunt meerdere id's voor meerdere exemplaren configureren zoals hieronder. 

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Met deze waarden, Azure AD wordt verwijdert u de waarde van **#** en verzenden van de juiste waarde `https://signin.aws.amazon.com/saml` als de URL van de doelgroep in het SAML-Token.

**We raden je aan het gebruik van deze benadering voor de volgende redenen:**

a. Elke toepassing biedt u de unieke X509 certificaat en dus elk exemplaar een ander certificaat-vervaldatum kunnen hebben en u kunt dit op AWS-account afzonderlijk beheren. Algemene is rollover van certificaten eenvoudig in dit geval.

b. U inrichten van gebruikers kunt inschakelen met AWS-app in Azure AD en vervolgens onze service alle rollen worden opgehaald uit dat account (AWS). U hebt geen handmatig toevoegen of bijwerken van de AWS-rollen op de app.

c. U kunt de app eigenaar afzonderlijk voor de app die de app rechtstreeks in Azure AD kan beheren.

> [!Note]
> Zorg ervoor dat u alleen de App-galerie gebruiken

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Amazon Web Services (AWS) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

> [!Note]
> Als u integreren meerdere AWS-accounts naar een Azure-account voor eenmalige aanmelding wilt, raadpleegt u [dit](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artikel.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amazon Web Services (AWS) uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) uit de galerie toe te voegen
Voor het configureren van de integratie van Amazon Web Services (AWS) in Azure AD, moet u Amazon Web Services (AWS) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amazon Web Services (AWS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/amazon-web-service-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/amazon-web-service-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/amazon-web-service-tutorial/a_new_app.png)

4. Typ in het zoekvak **Amazon Web Services (AWS)**, selecteer **Amazon Web Services (AWS)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Amazon Web Services (AWS) op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**  - hebben een equivalent van Britta Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **Amazon Web Services (AWS)** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/amazon-web-service-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/amazon-web-service-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/amazon-web-service-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, de gebruiker beschikt niet over een stap uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Als u meer dan één exemplaar configureert, geef de id-waarde. Geef de id-waarde in de volgende indeling in tweede instantie en hoger. Gebruik een **#** Meld u aan een unieke waarde van de SPN op te geven. 

    `https://signin.aws.amazon.com/saml#2`

    ![Amazon Web Services (AWS)-domein en URL's, eenmalige aanmelding informatie](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_identifier.png)

6. Amazon Web Services (AWS)-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken en Claims** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](./media/amazon-web-service-tutorial/i4-attribute.png)

7. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam  | Kenmerk van de gegevensbron  | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "Geef een waarde tussen de 900 seconden (15 minuten) op 43200 seconden (12 uur)" |  https://aws.amazon.com/SAML/Attributes |

    a. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/amazon-web-service-tutorial/i2-attribute.png)

    ![image](./media/amazon-web-service-tutorial/i3-attribute.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Voer de **Namespace** waarde.

    d. Selecteer de bron als **kenmerk**.

    e. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    f. Klik op **Opslaan**.

8. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  en sla deze op uw computer.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

9. In een ander browservenster aanmelden voor uw bedrijf Amazon Web Services (AWS) site als administrator.

10. Klik op **AWS Home**.

    ![Startpagina van eenmalige aanmelding configureren][11]

11. Klik op **Identity and Access Management**.

    ![Identiteit voor eenmalige aanmelding configureren][12]

12. Klik op **id-Providers**, en klik vervolgens op **Provider maken**.

    ![Provider voor eenmalige aanmelding configureren][13]

13. Op de **Provider configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Dialoogvenster voor eenmalige aanmelding configureren][14]

    a. Als **providertype**, selecteer **SAML**.

    b. In de **providernaam** tekstvak typt u de naam van een provider (bijvoorbeeld: *WAAD*).

    c. Het uploaden van uw gedownloade **metagegevensbestand** van Azure-portal klikt u op **bestand kiezen**.

    d. Klik op **volgende stap**.

14. Op de **providergegevens controleren** dialoogvenster pagina, klikt u op **maken**.

    ![Configureren van eenmalige aanmelding controleren][15]

15. Klik op **rollen**, en klik vervolgens op **rol maken**.

    ![Rollen voor eenmalige aanmelding configureren][16]

16. Op de **rol maken** pagina, voert u de volgende stappen uit:  

    ![Single Sign-On-vertrouwensrelatie configureren][19]

    a. Selecteer **SAML 2.0 federation** onder **selecteert u het type van vertrouwde entiteit**.

    b. Onder **kiest u een sectie voor SAML 2.0-Provider**, selecteer de **SAML-provider** u eerder hebt gemaakt (bijvoorbeeld: *WAAD*)

    c. Selecteer **toestaan programmatische en toegang tot de AWS Management Console**.
  
    d. Klik op **volgende: machtigingen**.

17. Op de **beleidsregels voor beveiligingsmachtigingen koppelen** dialoogvenster, u hoeft niet te koppelen van elk beleid. Klik op **volgende: Controleer**.  

    ![Single Sign-On-beleid configureren][33]

18. Op de **revisie** dialoogvenster, voer de volgende stappen uit:

    ![Configureren van eenmalige aanmelding controleren][34]

    a. In de **rolnaam** tekstvak, voer de naam van uw rol.

    b. In de **beschrijving van de rol** tekstvak, de omschrijving.

    c. Klik op **rol maken**.

    d. Maak zo veel functies indien nodig en toe te wijzen aan de id-Provider.

19. AWS-referenties voor service-account gebruiken voor het ophalen van de functies van AWS-account in Azure AD-gebruiker inrichten. Open hiervoor de AWS-console start.

20. Klik op **Services** -> **identiteits- en naleving** -> **IAM**.

    ![het ophalen van de functies van AWS-account](./media/amazon-web-service-tutorial/fetchingrole1.png)

21. Selecteer de **beleid** tabblad in de IAM-sectie.

    ![het ophalen van de functies van AWS-account](./media/amazon-web-service-tutorial/fetchingrole2.png)

22. Een nieuw beleid maken door te klikken op **beleid maken** voor het ophalen van de functies van AWS-account in Azure AD-gebruiker inrichten.

    ![Nieuw beleid maken](./media/amazon-web-service-tutorial/fetchingrole3.png)

23. Maak uw eigen beleid om op te halen van alle functies van AWS-accounts door de volgende stappen uit:

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

24. Definieer de **nieuw beleid** door de volgende stappen uit:

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy2.png)

    a. Geef de **beleidsnaam** als **AzureAD_SSOUserRole_Policy**.

    b. U kunt opgeven **beschrijving** aan het beleid als **dit beleid kunnen ophalen van de functies van AWS-accounts**.

    c. Klik op **'-beleid maken'** knop.

25. Maak een nieuw gebruikersaccount in de AWS IAM-Service door de volgende stappen uit:

    a. Klik op **gebruikers** navigatie in de AWS IAM-console.

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy3.png)

    b. Klik op **gebruiker toevoegen** knop een nieuwe gebruiker te maken.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/policy4.png)

    c. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser1.png)

    * Voer de naam van de gebruiker als **AzureADRoleManager**.

    * Selecteer in het type toegang tot de **toegang op programmeerniveau** optie. Op deze manier de gebruiker kan de API's aanroepen en ophalen van de functies van AWS-account.

    * Klik op de **volgende machtigingen** knop in de rechterbenedenhoek.

26. Maak nu een nieuw beleid voor deze gebruiker door de volgende stappen uit:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser2.png)

    a. Klik op de **bestaande beleidsregels rechtstreeks koppelen** knop.

    b. Zoeken naar de zojuist gemaakte beleid in de sectie filteren **AzureAD_SSOUserRole_Policy**.

    c. Selecteer de **beleid** en klik vervolgens op de **volgende: Controleer** knop.

27. Het beleid voor de gekoppelde gebruiker bekijken door te voeren stappen te volgen:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser3.png)

    a. Controleer de gebruikersnaam, het toegangstype en het beleid dat is toegewezen aan de gebruiker.

    b. Klik op de **gebruiker maken** knop in de rechterbenedenhoek om de gebruiker te maken.

28. Download de referenties van de gebruiker van een gebruiker door te voeren stappen te volgen:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kopiëren van de gebruiker **toegang ID sleutel** en **geheime toegangssleutel**.

    b. Deze referenties invoeren in Azure AD-gebruiker sectie inrichting om op te halen van de functies van AWS-console.

    c. Klik op **sluiten** knop aan de onderkant.

29. Navigeer naar **Gebruikersinrichting** sectie van Amazon Web Services-app in Azure AD-beheerportal.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning.png)

30. Voer de **toegangssleutel** en **geheim** in de **Clientgeheim** en **geheim Token** respectievelijk veld.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Voer de toegangssleutel van de AWS-gebruiker in de **clientsecret** veld.

    b. Voer het geheim van de gebruiker (AWS) in de **geheim Token** veld.

    c. Klik op de **verbinding testen** knop en u moet testen is deze verbinding.

    d. Sla de instelling door te klikken op de **opslaan** bovenaan op de knop.

31. Nu Zorg ervoor dat u de Status van inrichting inschakelen **op** in de sectie instellingen door het maken van de switch op en vervolgens te klikken op de **opslaan** bovenaan op de knop.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/amazon-web-service-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/amazon-web-service-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/amazon-web-service-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Maak een testgebruiker Amazon Web Services (AWS)

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in Amazon Web Services (AWS). Amazon Web Services (AWS) hoeven niet een gebruiker moet worden gemaakt in hun systeem voor eenmalige aanmelding, dus u hoeft hier actie uit te voeren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Amazon Web Services (AWS).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/amazon-web-service-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Amazon Web Services (AWS)**.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/amazon-web-service-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/amazon-web-service-tutorial/d_assign_user.png)

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_users.png)

6. In de **rol selecteren** dialoogvenster kiest u de desbetreffende gebruikersrol in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_roles.png)

    >[!NOTE]
    >Nadat de gebruikers inrichten met de toepassing is ingeschakeld, moet u wachten gedurende 30 minuten om op te halen alle rollen en vervolgens u moet de pagina vernieuwd en vervolgens tijdens het toewijzen van de toepassing aan gebruikers en groepen die u ziet de rollen voor het gebruik van Amazon Web Services (AWS) r.

7. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_assign.png)
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Amazon Web Services (AWS) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Amazon Web Services (AWS).
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

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

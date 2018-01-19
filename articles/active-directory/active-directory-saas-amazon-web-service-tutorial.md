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
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 8d77215fd2923e22a9cc87e469cb135d035d22d9
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD).

Amazon Web Services (AWS) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die naar Amazon Web Services (AWS) toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde naar Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Amazon Web Services (AWS) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amazon Web Services (AWS) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) uit de galerie toevoegen
Voor het configureren van de integratie van Amazon Web Services (AWS) in Azure AD, moet u Amazon Web Services (AWS) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amazon Web Services (AWS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Amazon Web Services (AWS)**, selecteer **Amazon Web Services (AWS)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Amazon Web Services (AWS) in de lijst met resultaten](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Amazon Web Services (AWS) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

In Amazon Web Services (AWS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**  - hebben een equivalent van Britta Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Amazon Web Services (AWS)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Op de **Amazon Web Services (AWS) domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure.

    ![Amazon Web Services (AWS)-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. De SAML-asserties verwacht de Amazon Web Services (AWS) softwaretoepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Single Sign-On attb configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)   

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >U moet de gebruiker wordt ingericht in Azure AD ophalen van alle functies van de AWS-Console configureren. Raadpleeg de onderstaande stappen inrichting.

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On addattb configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** textbox, typt u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. In een ander browservenster aanmelding bij uw bedrijf Amazon Web Services (AWS) site als administrator.

9. Klik op **Console-startpagina**.
   
    ![Startpagina voor eenmalige aanmelding configureren][11]

10. Klik op **Identity and Access Management**. 
   
    ![Configureer de identiteit voor één aanmelding][12]

11. Klik op **identiteitsproviders**, en klik vervolgens op **Provider maken**. 
   
    ![Provider voor eenmalige aanmelding configureren][13]

12. Op de **Provider configureren** dialoogvenster pagina, voert u de volgende stappen uit: 
   
    ![Dialoogvenster voor eenmalige aanmelding configureren][14]
 
    a. Als **providertype**, selecteer **SAML**.

    b. In de **providernaam** textbox, typ een providernaam (bijvoorbeeld: *WAAD*).

    c. Voor het uploaden van uw gedownloade **metagegevensbestand** vanuit Azure-portal klikt u op **bestand kiezen**.

    d. Klik op **volgende stap**.

13. Op de **providerinformatie controleren** dialoogvenster pagina, klikt u op **maken**. 
    
    ![Eenmalige aanmelding configureren controleren][15]

14. Klik op **rollen**, en klik vervolgens op **nieuwe rol maken**. 
    
    ![Rollen voor eenmalige aanmelding configureren][16]

15. Op de **rolnaam ingesteld** dialoogvenster de volgende stappen uitvoeren: 
    
    ![De naam van eenmalige aanmelding configureren][17] 

    a. In de **rolnaam** textbox, typ een rolnaam (bijvoorbeeld: *testgebruiker*). 

    b. Klik op **volgende stap**.

16. Op de **Roltype Selecteer** dialoogvenster de volgende stappen uitvoeren: 
    
    ![Roltype voor eenmalige aanmelding configureren][18] 

    a. Selecteer **rol voor toegang van de Provider identiteit**. 

    b. In de **Grant Web eenmalige aanmelding (WebSSO) toegang tot de SAML-providers** sectie, klikt u op **Selecteer**.

17. Op de **vertrouwensrelatie tot stand brengen** dialoogvenster de volgende stappen uitvoeren:  
    
    ![Een vertrouwensrelatie voor eenmalige aanmelding configureren][19] 

    a. Selecteer de SAML-provider die u eerder hebt gemaakt als SAML-provider (bijvoorbeeld: *WAAD*) 
  
    b. Klik op **volgende stap**.

18. Op de **controleren rol vertrouwen** dialoogvenster, klikt u op **volgende stap**. 
    
    ![Een vertrouwensrelatie voor één rol voor eenmalige aanmelding configureren][32]

19. Op de **beleid koppelen** dialoogvenster, klikt u op **volgende stap**.  
    
    ![Beleid voor één aanmelding configureren][33]

20. Op de **revisie** dialoogvenster de volgende stappen uitvoeren:   
    
    ![Controleren voor eenmalige aanmelding configureren][34] 

    a. Klik op **rol maken**.

    b. Zo veel functies naar behoefte maken en toe te wijzen aan de id-Provider.

21. AWS-referenties voor service-account gebruiken voor het ophalen van de rollen van AWS-account in Azure AD-gebruiker inrichten. Open hiervoor de thuis AWS-console.

22. Klik op **Services** -> **beveiliging, identiteit en compatibiliteit** -> **IAM**.

    ![de rollen ophalen van AWS-account](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Selecteer de **beleid** tabblad in de IAM-sectie.

    ![de rollen ophalen van AWS-account](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Een nieuw beleid maken door te klikken op **beleid maken**.

    ![Nieuw beleid maken](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Maak uw eigen beleid voor het ophalen van alle functies van de AWS-accounts door de volgende stappen uit te voeren:

    ![Nieuw beleid maken](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. In de **'Beleid maken'** sectie klikt u op **'JSON'** tabblad.

    b. In het beleidsdocument, voegt u de onderstaande JSON.
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Klik op **revisie beleid knop** voor het valideren van het beleid.

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Definieer de **nieuw beleid** door de volgende stappen uit te voeren:

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Geef de **beleidsnaam** als **AzureAD_SSOUserRole_Policy**.

    b. U kunt opgeven **beschrijving** aan het beleid zo **dit beleid kunnen ophalen van de rollen van de AWS accounts**.
    
    c. Klik op **'Beleid maken'** knop.
        
27. Maak een nieuw gebruikersaccount in de AWS IAM-Service door de volgende stappen uit te voeren:

    a. Klik op **gebruikers** navigatie in de IAM-AWS-console.

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b. Klik op **gebruiker toevoegen** knop een nieuwe gebruiker te maken.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:
    
    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Geef de gebruikersnaam op als **AzureADRoleManager**.
    
    * Selecteer in het type toegang tot de **toegang op programmeerniveau** optie. Op deze manier de gebruiker kan aanroepen van de API's en ophalen van de rollen van de AWS-account.
    
    * Klik op de **volgende machtigingen** knop in de rechterbenedenhoek.

28. Maak nu een nieuw beleid voor deze gebruiker door de volgende stappen uit te voeren:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Klik op de **bestaande beleidsregels rechtstreeks koppelen** knop.

    b. Zoeken naar het nieuwe beleid in de sectie filteren **AzureAD_SSOUserRole_Policy**.
    
    c. Selecteer de **beleid** en klik vervolgens op de **volgende: Bekijk** knop.

29. Het beleid voor de gekoppelde gebruiker bekijken door het uitvoeren van de volgende stappen uit:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Controleer de gebruikersnaam, het toegangstype en het beleid dat is toegewezen aan de gebruiker.
    
    b. Klik op de **gebruiker maken** knop in de rechterbenedenhoek om de gebruiker te maken.

30. Download de referenties van de gebruiker van een gebruiker door het uitvoeren van de volgende stappen uit:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kopiëren van de gebruiker **toegang ID sleutel** en **geheime toegangssleutel**.
    
    b. Deze referenties invoeren in Azure AD-gebruiker inrichting sectie ophalen van de rollen van de AWS-console.
    
    c. Klik op **sluiten** knop onderaan.

31. Navigeer naar **Gebruikersinrichting** sectie van Amazon Web Services-app in Azure AD-beheerportal.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Voer de **toegangssleutel** en **geheim** in de **Clientgeheim** en **geheim Token** respectievelijk veld.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Voer de AWS-toegangssleutel gebruiker in de **clientsecret** veld.
    
    b. Geef de AWS gebruiker geheim in de **geheim Token** veld.
    
    c. Klik op de **verbinding testen** knop en u moet deze verbinding met succes te testen.

    d. Opslaan van de instelling door te klikken op de **opslaan** bovenaan op de knop.
 
33. Controleren of de Status van de inrichting in te schakelen **op** in de sectie instellingen door het maken van de switch op en vervolgens te klikken op de **opslaan** bovenaan op de knop.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Een testgebruiker Amazon Web Services (AWS) maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Amazon Web Services (AWS) genoemd. Een gebruiker moet worden gemaakt in het systeem voor eenmalige aanmelding, dus u hoeft geen actie hier nodig niet voor Amazon Web Services (AWS).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent aan Amazon Web Services (AWS) gebruiken.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon naar Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Amazon Web Services (AWS)**.

    ![De koppeling Amazon Web Services (AWS) in de lijst met toepassingen](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Amazon Web Services (AWS) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Amazon Web Services (AWS).
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png


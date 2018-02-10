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
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD).

Amazon Web Services (AWS) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die naar Amazon Web Services (AWS) toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld op de Amazon Web Services (AWS) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Amazon Web Services (AWS) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Amazon Web Services (AWS) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) uit de galerie toevoegen
Voor het configureren van de integratie van Amazon Web Services (AWS) in Azure AD, moet u Amazon Web Services (AWS) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Amazon Web Services (AWS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, selecteer in het navigatiedeelvenster links de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Amazon Web Services (AWS)**. Selecteer **Amazon Web Services (AWS)** van het resultatenpaneel en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Amazon Web Services (AWS) in de lijst met resultaten](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u gegevens kunt configureren en testen eenmalige aanmelding Azure AD met Amazon Web Services (AWS) op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de equivalente gebruiker in Amazon Web Services (AWS) is een gebruiker in Azure AD. Met andere woorden, hoeft u te maken van een koppeling tussen een Azure AD-gebruiker en een gebruiker in Amazon Web Services (AWS).

Geef de waarde om vast te stellen de koppeling in Amazon Web Services (AWS), **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. 

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Amazon Web Services (AWS), voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een testgebruiker met Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user) hebben een equivalent van Britta Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Amazon Web Services (AWS)** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. In de **Amazon Web Services (AWS) domein en de URL's** sectie, de gebruiker heeft geen om maatregelen te treffen omdat de app al vooraf is geïntegreerd met Azure.

    ![Amazon Web Services (AWS)-domein en één URL's aanmelding informatie](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. De SAML-asserties verwacht de softwaretoepassing Amazon Web Services (AWS) in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de integratie van toepassingen. De volgende Schermafbeelding toont een voorbeeld:

    ![Kenmerk voor eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. In de **gebruikerskenmerken** sectie het **eenmalige aanmelding** vak, het SAML-token kenmerk configureren zoals wordt weergegeven in de vorige afbeelding en vervolgens de volgende stappen uitvoeren:
    
    | Naam van kenmerk  | De waarde van kenmerk | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Configureer het inrichten van de gebruiker in Azure AD ophalen van alle functies van de console Amazon Web Services (AWS). Raadpleeg de volgende stappen voor het inrichten.

    a. Openen van de **kenmerk toevoegen** dialoogvenster, **toevoegen kenmerk**.

    ![Kenmerk voor eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Kenmerk voor eenmalige aanmelding configureren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. In de **naam** typt u de naam van het kenmerk dat wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk dat wordt weergegeven voor die rij.

    d. In de **Namespace** typt u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    d. Selecteer **Ok**.

6. In de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**. Sla het metagegevensbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. In een ander browservenster aanmelden bij uw bedrijf Amazon Web Services (AWS) site als beheerder.

9. Selecteer **Console-startpagina**.
   
    ![Startpagina voor eenmalige aanmelding configureren][11]

10. Selecteer **Identity and Access Management**. 
   
    ![Configureer de identiteit voor één aanmelding][12]

11. Selecteer **identiteitsproviders**. Selecteer vervolgens **Provider maken**. 
   
    ![Provider voor eenmalige aanmelding configureren][13]

12. In de **Provider configureren** dialoogvenster vak, voert de volgende stappen uit: 
   
    ![Single sign-n dialoogvenster configureren][14]
 
    a. Voor **providertype**, selecteer **SAML**.

    b. In de **providernaam** typt u de providernaam van een (bijvoorbeeld: *WAAD*).

    c. Voor het uploaden van uw gedownloade **metagegevensbestand** Selecteer in de Azure-portal **bestand kiezen**.

    d. Selecteer **volgende stap**.

13. In de **providerinformatie controleren** dialoogvenster, **maken**. 
    
    ![Verificatie voor eenmalige aanmelding configureren][15]

14. Selecteer **rollen**. Selecteer vervolgens **nieuwe rol maken**. 
    
    ![Rollen voor eenmalige aanmelding configureren][16]

15. In de **rolnaam ingesteld** dialoogvenster vak, voert de volgende stappen uit: 
    
    ![De naam van eenmalige aanmelding configureren][17] 

    a. In de **rolnaam** typt u de rolnaam van een (bijvoorbeeld *testgebruiker*). 

    b. Selecteer **volgende stap**.

16. In de **Roltype Selecteer** dialoogvenster vak, voert de volgende stappen uit: 
    
    ![Roltype voor eenmalige aanmelding configureren][18] 

    a. Selecteer **rol voor toegang van de Provider identiteit**. 

    b. In de **Grant Web eenmalige aanmelding (WebSSO) toegang tot de SAML-providers** sectie, klikt u op **Selecteer**.

17. In de **vertrouwensrelatie tot stand brengen** dialoogvenster vak, voert de volgende stappen uit:  
    
    ![Een vertrouwensrelatie voor eenmalige aanmelding configureren][19] 

    a. Selecteer de SAML-provider die u eerder hebt gemaakt (bijvoorbeeld: *WAAD*). 
  
    b. Selecteer **volgende stap**.

18. In de **controleren rol vertrouwen** dialoogvenster, **volgende stap**. 
    
    ![Een vertrouwensrelatie voor één rol voor eenmalige aanmelding configureren][32]

19. In de **beleid koppelen** dialoogvenster, **volgende stap**.  
    
    ![Beleid voor één aanmelding configureren][33]

20. In de **revisie** dialoogvenster vak, voert de volgende stappen uit:   
    
    ![Controleren voor eenmalige aanmelding configureren][34] 

    a. Selecteer **rol maken**.

    b. Maak zo veel functies indien nodig en vervolgens toe te wijzen aan de id-Provider.

21. Serviceaccountreferenties Amazon Web Services (AWS) gebruiken voor het ophalen van de rollen van de Amazon Web Services (AWS)-account in Azure AD-gebruikers inrichten. Open de thuis Amazon Web Services (AWS)-console voor het starten van deze taak.

22. Selecteer **Services** > **beveiliging, identiteit en compatibiliteit** > **IAM**.

    ![De rollen ophalen van Amazon Web Services (AWS)-account](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Selecteer in de sectie IAM de **beleid** tabblad.

    ![De rollen ophalen van Amazon Web Services (AWS)-account](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Voor het maken van een nieuw beleid selecteert **beleid maken**.

    ![Een nieuw beleid maken](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Voor het maken van uw eigen beleid voor het ophalen van alle functies van de Amazon Web Services (AWS) accounts, moet u de volgende stappen uitvoeren:

    ![Een nieuw beleid maken](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. In de **beleid maken** sectie, selecteer de **JSON** tabblad.

    b. Voeg de volgende JSON in het beleidsdocument:
    
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

    c. Voor het valideren van het beleid, selecteer de **revisie beleid knop**.

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Definieer de **nieuw beleid** door de volgende stappen:

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Geef de **beleidsnaam** als **AzureAD_SSOUserRole_Policy**.

    b. U kunt de volgende bieden **beschrijving** voor het beleid: **dit beleid kunt u voor het ophalen van de rollen van AWS-accounts**.
    
    c. Selecteer de **beleid maken** knop.
        
27. Voor het maken van een nieuw gebruikersaccount in de Amazon Web Services (AWS) IAM-service, moet u de volgende stappen uitvoeren:

    a. Selecteer **gebruikers** in de Amazon Web Services (AWS) IAM-console.

    ![Het nieuwe beleid definiëren](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b.To Maak een nieuwe gebruiker, selecteer de **gebruiker toevoegen** knop.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. In de **gebruiker toevoegen** sectie, voert de volgende stappen uit:
    
    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Voer **AzureADRoleManager** in het vak gebruikersnaam.
    
    * Voor type toegang, selecteert u de **toegang op programmeerniveau** optie. Op deze manier de gebruiker kan aanroepen van de API's en ophalen van de rollen van het account Amazon Web Services (AWS).
    
    * Selecteer de **volgende machtigingen** knop in de rechterbenedenhoek.

28. Maak een nieuw beleid voor deze gebruiker door de volgende stappen:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Selecteer de **bestaande beleidsregels rechtstreeks koppelen** knop.

    b. Zoeken naar het nieuwe beleid in de sectie filteren **AzureAD_SSOUserRole_Policy**.
    
    c. Selecteer de **beleid**. Selecteer vervolgens de **volgende: Bekijk** knop.

29. Het beleid voor de gekoppelde gebruiker bekijken door de volgende stappen:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Controleer de gebruikersnaam, het toegangstype en het beleid die zijn toegewezen aan de gebruiker.
    
    b. Voor het maken van de gebruiker selecteert de **gebruiker maken** knop in de rechterbenedenhoek om de gebruiker te maken.

30. Download de referenties van een gebruiker door de volgende stappen:

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kopiëren van de gebruiker **toegang ID sleutel** en **geheime toegangssleutel**.
    
    b. Deze referenties invoeren in de Azure AD-gebruiker inrichting sectie ophalen van de rollen van de console Amazon Web Services (AWS).
    
    c. Selecteer de **sluiten** knop in de rechterbenedenhoek.

31. Navigeer naar de **Gebruikersinrichting** sectie van de app Amazon Web Services (AWS) in de Azure AD-beheerportal.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Voer de **toegangssleutel** en **geheim** in de **Clientgeheim** en **geheim Token** respectievelijk veld.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Voer de toegangssleutel van Amazon Web Services (AWS) gebruiker in de **clientsecret** veld.
    
    b. Voer het geheim van de gebruiker Amazon Web Services (AWS) in de **geheim Token** veld.
    
    c. Selecteer de **testverbinding** knop. U moet deze verbinding met succes te testen.

    d. Opslaan van de instelling door de **opslaan** bovenaan op de knop.
 
33. Zorg ervoor dat u de Status van de inrichting **op** in **instellingen**. U dit doen door het selecteren van **op**, en vervolgens te klikken op de **opslaan** bovenaan op de knop.

    ![Gebruiker toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad. Vervolgens toegang krijgen tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Een testgebruiker Amazon Web Services (AWS) maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Amazon Web Services (AWS) genoemd. Amazon Web Services (AWS) niet nodig voor een gebruiker moet worden gemaakt in het systeem voor één aanmelding, dus u hoeft hier actie uit te voeren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door ze naar Amazon Web Services (AWS) toegang te verlenen.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon naar Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Vervolgens gaat u naar de directoryweergave en selecteer **bedrijfstoepassingen**. Selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Amazon Web Services (Amazon Web Services (AWS)**.

    ![De koppeling Amazon Web Services (AWS) in de lijst met toepassingen](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop. 

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel Amazon Web Services (AWS) in het deelvenster toegang selecteert, u moet ophalen automatisch aangemeld bij uw toepassing Amazon Web Services (AWS). Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

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


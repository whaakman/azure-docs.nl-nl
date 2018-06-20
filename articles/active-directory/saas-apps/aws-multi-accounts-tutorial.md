---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) voor het verbinding maken met meerdere accounts | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure AD en meerdere accounts van Amazon Web Services (AWS).
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
ms.date: 03/28/2018
ms.author: jeedes
ms.openlocfilehash: 2678cf043bb4b2569555309e873ae9ce0ab64eab
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36217794"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Zelfstudie: Azure Active Directory-integratie met meerdere accounts met Amazon Web Services (AWS)

In deze zelfstudie leert u het integreren van Azure Active Directory (Azure AD) met meerdere accounts van Amazon Web Services (AWS).

Amazon Web Services (AWS) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die naar Amazon Web Services (AWS) toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde naar Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Amazon Web Services (AWS), moet u de volgende items:

- Een Azure AD basic of premium-abonnement
- Amazon Web Services (AWS) meerdere eenmalige aanmelding ingeschakeld accounts

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

    ![Amazon Web Services (AWS) in de lijst met resultaten](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

5. Wanneer de toepassing is toegevoegd, gaat u naar **eigenschappen** pagina en kopieer de **Object-ID**.

    ![Amazon Web Services (AWS) in de lijst met resultaten](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Amazon Web Services (AWS) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

In Amazon Web Services (AWS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Amazon Web Services (AWS)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Op de **Amazon Web Services (AWS) domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure.

    ![Amazon Web Services (AWS)-domein en de URL's van eenmalige aanmelding informatie](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. De SAML-asserties verwacht de Amazon Web Services (AWS) softwaretoepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Single Sign-On kenmerk configureren](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_attribute.png)    

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://aws.amazon.com/SAML/Attributes |
    | Rol            | User.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >U moet de gebruiker wordt ingericht in Azure AD ophalen van alle functies van de AWS-Console configureren. Raadpleeg de onderstaande stappen inrichting.

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/aws-multi-accounts-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On kenmerk configureren](./media/aws-multi-accounts-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** textbox, typt u de naamruimtewaarde die wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/aws-multi-accounts-tutorial/tutorial_general_400.png)

8. In een ander browservenster aanmelding bij uw bedrijf Amazon Web Services (AWS) site als Administrator.

9. Klik op **AWS Home**.
   
    ![Startpagina voor eenmalige aanmelding configureren][11]

10. Klik op **IAM** (Identity and Access Management). 
   
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

14. Klik op **rollen**, en klik vervolgens op **rol maken**. 
    
    ![Rollen voor eenmalige aanmelding configureren][16]

15. Op de **rol maken** pagina, voert u de volgende stappen uit:  
    
    ![Een vertrouwensrelatie voor eenmalige aanmelding configureren][19] 

    a. Selecteer **SAML 2.0 federation** onder **Selecteer type vertrouwde entiteit**.

    b. Onder **kiest u een sectie SAML 2.0 Provider**, selecteer de **SAML provider** u eerder hebt gemaakt (bijvoorbeeld: *WAAD*)

    c. Selecteer **toestaan programmatische en toegang tot de AWS Management Console**.
  
    d. Klik op **volgende: machtigingen**.

16. Op de **koppelen machtigingsbeleid** dialoogvenster, klikt u op **volgende: Bekijk**.  
    
    ![Beleid voor één aanmelding configureren][33]

17. Op de **revisie** dialoogvenster de volgende stappen uitvoeren:   
    
    ![Controleren voor eenmalige aanmelding configureren][34] 

    a. In de **rolnaam** textbox, voer de naam van uw rol.

    b. In de **beschrijving van de functie** tekstvak de omschrijving.

    a. Klik op **rol maken**.

    b. Zo veel functies naar behoefte maken en toe te wijzen aan de id-Provider.

18. Meld u af van de huidige AWS-account en meld u aan met een andere account waar u configureren voor eenmalige wilt op Azure AD.

19. Voer stap-9 voor stap-17 meerdere rollen die u setup voor dit account wilt maken. Als u meer dan twee accounts hebt, Voer u dezelfde stappen voor alle accounts rollen voor hen maken.

20. Zodra alle rollen in de accounts worden gemaakt, ze weergegeven in de **rollen** lijst voor deze accounts.

    ![Installatie van functies](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

21. Er moet voor het vastleggen van de rol informatie en vertrouwde entiteiten voor alle rollen over alle accounts, die we moeten handmatig worden toegewezen met Azure AD-toepassing. 

22. Klik op de functies kopiëren **rol informatie** en **vertrouwde entiteiten** waarden. In dat geval moet u deze waarden in voor alle functies die u wilt maken in Azure AD.

    ![Installatie van functies](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)
 
23. De bovenstaande stap voor alle functies in de accounts uitvoeren en deze opslaan in de indeling **rol informatie, vertrouwde entiteiten** in Kladblok. 

24. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

    a. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

    b. U moet voldoende rechten hebt voor het maken van de rollen. Klik op **wijzigingsmachtiging** de vereiste machtigingen op te halen. 

    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Selecteer volgende machtigingen in de lijst (als u nog geen deze hebt) en klik op 'Wijzigingsmachtigingen' 

    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Dit kunt u zich opnieuw aanmelden en accepteert de toestemming wordt gevraagd. Na de toestemming accepteren, bent u aangemeld bij de grafiek Explorer opnieuw.

    e. Wijzigen van de vervolgkeuzelijst versie naar **beta**. Als u wilt ophalen alle Service-Principals van uw tenant, gebruik de volgende query:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Als u meerdere mappen gebruikt, kunt u volgen met uw primaire domein in het patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)
    
    f. Ophalen uit de lijst met de Service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. U kunt na de query met behulp van de **Object-id** die u hebt gekopieerd uit de pagina Azure AD-eigenschappen ophalen voor de betreffende Service-Principal.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. De eigenschap appRoles extraheren uit het service-principal-object. 

    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Nu moet u nieuwe functies voor uw toepassing te genereren. 

    i. Is een voorbeeld van appRoles object hieronder JSON. Maak een vergelijkbaar object op om de functies die u voor uw toepassing wilt toevoegen. 

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > U kunt alleen nieuwe rollen na toevoegen de **msiam_access** voor de patch-bewerking. U kunt ook de rollen die u wilt dat uw organisatie behoefte toevoegen. Azure AD ontvangt de **waarde** van deze rollen als de claimwaarde SAML-reactie.
    
    j. Ga terug naar uw grafiek Explorer en wijzig de methode van **ophalen** naar **PATCH**. Patch voor het Service-Principal-object hebt rollen gewenst door het bijwerken van appRoles eigenschap lijkt op de hierboven weergegeven in het voorbeeld. Klik op **Query uitvoeren** de patch-bewerking uit te voeren. Een bericht wordt bevestigd dat het maken van de functie voor uw toepassing Amazon Web Services.

    ![Dialoogvenster van grafiek explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

25. Nadat de Service-Principal is een patch uitgevoerd met meer functies, kunt u gebruikers/groepen toewijzen aan de betreffende rollen. Dit kan worden gedaan door te gaan naar de portal en navigeren naar de Amazon Web Services-toepassing. Klik op de **gebruikers en groepen** tabblad bovenaan. 

26. We raden u aan te maken van nieuwe groepen voor elke rol AWS zodat u die bepaalde rol in de groep kunt toewijzen. Houd er rekening mee dat dit een-op-een-toewijzing voor een groep aan een rol is. Vervolgens kunt u de leden die deel uitmaken van die groep toevoegen.

27. Nadat de groepen hebt gemaakt, selecteert u de groep en toewijzen aan de toepassing. 

    ![Eenmalige aanmelding configureren toevoegen](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

28. Als u wilt de rol toewijst aan de groep, selecteert u de functie en klik op **toewijzen** knop in onder aan de pagina.

    ![Eenmalige aanmelding configureren toevoegen](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Houd er rekening mee dat u wilt vernieuwen van uw sessie in Azure portal om te zien van nieuwe rollen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Amazon Web Services (AWS) in het deelvenster toegang, krijgt u de toepassingspagina Amazon Web Services (AWS) met de optie om de rol te selecteren.

![Eenmalige aanmelding configureren toevoegen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

U kunt ook het SAML-antwoord om te zien van de rollen die worden doorgegeven als claims controleren.

![Eenmalige aanmelding configureren toevoegen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/aws-multi-accounts-tutorial/tutorial_general_01.png
[2]: ./media/aws-multi-accounts-tutorial/tutorial_general_02.png
[3]: ./media/aws-multi-accounts-tutorial/tutorial_general_03.png
[4]: ./media/aws-multi-accounts-tutorial/tutorial_general_04.png

[100]: ./media/aws-multi-accounts-tutorial/tutorial_general_100.png

[200]: ./media/aws-multi-accounts-tutorial/tutorial_general_200.png
[201]: ./media/aws-multi-accounts-tutorial/tutorial_general_201.png
[202]: ./media/aws-multi-accounts-tutorial/tutorial_general_202.png
[203]: ./media/aws-multi-accounts-tutorial/tutorial_general_203.png
[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_on.png


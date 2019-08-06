---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6640708905abc266b07b7b66f5da09aeb890f01
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68823862"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Zelfstudie: Amazon Web Services (AWS) integreren met Azure Active Directory

In deze zelf studie leert u hoe u Amazon Web Services (AWS) integreert met Azure Active Directory (Azure AD). Wanneer u Amazon Web Services (AWS) integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Amazon Web Services (AWS).
* Stel uw gebruikers in staat om automatisch te worden aangemeld bij Amazon Web Services (AWS) met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

U kunt meerdere id's voor meerdere instanties configureren, zoals hieronder.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Met deze waarden verwijdert Azure AD de waarde van **#** en stuurt de juiste waarde `https://signin.aws.amazon.com/saml` als de doelgroep-URL in het SAML-token.

**We raden u om de volgende redenen aan deze methode te gebruiken:**

a. Elke toepassing krijgt u een uniek x509-certificaat. Elk exemplaar van het exemplaar van de AWS-app kan vervolgens een andere verloop datum voor het certificaat hebben dat kan worden beheerd op basis van een afzonderlijk AWS-account. De algehele rollover van het certificaat is in dit geval gemakkelijker.

b. U kunt het inrichten van gebruikers inschakelen met de AWS-app in Azure AD, en onze service haalt dan alle rollen van dat AWS-account op. U hoeft de AWS-rollen in de app niet handmatig toe te voegen of bij te werken.

c. U kunt individueel de app-eigenaar toewijzen voor de app die de app rechtstreeks in Azure AD kan beheren.

> [!Note]
> Zorg ervoor dat u alleen Gallery App gebruikt

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Amazon Web Services (AWS) eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving. Amazon Web Services (AWS) ondersteunt door **SP en IDP** geïnitieerde SSO.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) toevoegen uit de galerie

Om de integratie van Amazon Web Services (AWS) met Azure AD te configureren, moet u Amazon Web Services (AWS) vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. In de sectie **toevoegen vanuit de galerie** typt u **Amazon Web Services (AWS)** in het zoekvak.
1. Selecteer **Amazon Web Services (AWS)** in het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Azure AD SSO met Amazon Web Services (AWS) configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Amazon Web Services (AWS).

Als u Azure AD SSO wilt configureren en testen met Amazon Web Services (AWS), voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** zodat uw gebruikers deze functie kunnen gebruiken.
2. **[Amazon Web Services configureren (AWS)](#configure-amazon-web-services-aws)** om de SSO-instellingen aan de kant van de toepassing te configureren.
3. **[Maak een Azure AD-test gebruiker om de](#create-an-azure-ad-test-user)** eenmalige aanmelding van Azure ad te testen met B. Simon.
4. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe om B. Simon in te scha kelen voor het gebruik van eenmalige aanmelding voor Azure AD.
5. **[Maak Amazon Web Services (AWS) test gebruiker](#create-amazon-web-services-aws-test-user)** een equivalent van B. Simon in Amazon Web Services (AWS) dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Test SSO](#test-sso)** om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in de [Azure Portal](https://portal.azure.com/)op de pagina **Amazon Web Services (AWS)** Application Integration de sectie **Manage** en selecteer eenmalige **aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **basis configuratie van SAML** is de toepassing vooraf geconfigureerd en de benodigde url's zijn al vooraf ingevuld met Azure. De gebruiker moet de configuratie opslaan door te klikken op de knop **Opslaan** .

5. Geef de id-waarde op als u meer dan één instantie configureert. Geef de id-waarde voor de tweede en volgende instanties op in de volgende notatie. Gebruik een **#** -teken om een unieke SPN-waarde op te geven.

    `https://signin.aws.amazon.com/saml#2`

6. Amazon Web Services-toepassing (AWS) verwacht de SAML-beweringen in een specifieke indeling. hiervoor moet u aangepaste kenmerk toewijzingen toevoegen aan de configuratie van uw SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

7. In aanvulling op de bovenstaande, Amazon Web Services (AWS)-toepassing verwacht nog enkele kenmerken te worden door gegeven in het SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name  | Bronkenmerk  | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Role            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "Geef een waarde op tussen 900 seconden (15 minuten) en 43200 seconden (12 uur)" |  https://aws.amazon.com/SAML/Attributes |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Typ de naamruimtewaarde voor die rij in het tekstvak **Naamruimte**.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , de **federatieve meta gegevens-XML** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

   ![De downloadkoppeling certificaat](common/metadataxml.png)

1. Kopieer op de sectie **Amazon Web Services instellen (AWS)** de gewenste URL ('s) op basis van uw vereiste.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Amazon Web Services configureren (AWS)

1. Meld u in een ander browservenster aan als beheerder bij uw Amazon Web Services (AWS) bedrijfssite.

2. Klik op **AWS Home**.

    ![Startpagina voor configureren van eenmalige aanmelding][11]

3. Klik op **Identity and Access Management**.

    ![Identiteit voor eenmalige aanmelding configureren][12]

4. Klik op **Identity Providers** en vervolgens op **Create Provider**.

    ![Provider voor eenmalige aanmelding configureren][13]

5. Voer op de dialoogpagina **Configure Provider** de volgende stappen uit:

    ![Dialoogvenster Configure Single Sign-On][14]

    a. Selecteer **SAML** als **Provider Type**.

    b. Typ in het tekstvak **Provider Name** de naam van een provider (bijvoorbeeld *WAAD*).

    c. Klik op **Choose File** om het uit de Azure-portal gedownloade **metagegevensbestand** te uploaden.

    d. Klik op **Next Step**.

6. Klik op de dialoogpagina **Verify Provider Information** op **Create**.

    ![Verificatie voor het configureren van eenmalige aanmelding][15]

7. Klik op **Roles** en vervolgens op **Create role**.

    ![Rollen voor eenmalige aanmelding configureren][16]

8. Voer op de pagina **Create role** de volgende stappen uit:  

    ![Vertrouwensrelatie voor eenmalige aanmelding configureren][19]

    a. Selecteer **SAML 2.0 federation** onder **Select type of trusted entity**.

    b. Selecteer onder **Choose a SAML 2.0 Provider section** de **SAML-provider** die u eerder hebt gemaakt (bijvoorbeeld *WAAD*)

    c. Selecteer **Allow programmatic and AWS Management Console access**.
  
    d. Klik op **Next: Permissions**.

9. Voeg in het dialoogvenster **Attach Permissions Policies** het juiste beleid voor uw organisatie toe. Klik op **Next: Review**.  

    ![Beleid voor eenmalige aanmelding configureren][33]

10. Voer in het dialoogvenster **Review** de volgende stappen uit:

    ![Configuratie van eenmalige aanmelding controleren][34]

    a. Voer in het tekstvak **Role name** de naam van uw rol in.

    b. Voer in het tekstvak **Role description** de omschrijving van de rol in.

    c. Klik op **Create Role**.

    d. Maak zoveel rollen als nodig is en wijs ze toe aan de id-provider.

11. Gebruik de aanmeldingsgegevens van uw AWS-serviceaccount voor het ophalen van de rollen van het AWS-account voor het inrichten van gebruikers in Azure AD. Open hiervoor de startpagina van de AWS-console.

12. Klik op **Services** -> **Security, Identity& Compliance** -> **IAM**.

    ![de rollen van het AWS-account ophalen](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Selecteer het tabblad **Policies** in de sectie IAM.

    ![de rollen van het AWS-account ophalen](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Maak een nieuw beleid door op **Create policy** te klikken voor het ophalen van de rollen van het AWS-account voor het inrichten van gebruikers in Azure AD.

    ![Nieuw beleid maken](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Maak uw eigen beleid om alle rollen van AWS-accounts op te halen door de volgende stappen uit te voeren:

    ![Nieuw beleid maken](./media/amazon-web-service-tutorial/policy1.png)

    a. Klik in de sectie **Create policy** op het tabblad **JSON**.

    b. Voeg de volgende JSON toe in het beleidsdocument.

    ```json
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

    c. Klik op de knop **Review policy** om het beleid te valideren.

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy5.png)

16. Definieer het **nieuwe beleid** door de volgende stappen uit te voeren:

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy2.png)

    a. Geef bij **Policy Name** de naam **AzureAD_SSOUserRole_Policy** op.

    b. U kunt in het veld **Description** bijvoorbeeld de beschrijving **Met dit beleid kunnen de rollen van AWS-accounts worden opgehaald** opgeven.

    c. Klik op de knop **Create Policy**.

17. Maak een nieuw gebruikersaccount in de AWS IAM-service door de volgende stappen uit te voeren:

    a. Klik op **Users** in de navigatie van de AWS IAM-console.

    ![Het nieuwe beleid definiëren](./media/amazon-web-service-tutorial/policy3.png)

    b. Klik op de knop **Add user** om een nieuwe gebruiker te maken.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/policy4.png)

    c. Voer in de sectie **Add user** de volgende stappen uit:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser1.png)

    * Voer de naam van de gebruiker in als **AzureADRoleManager**.

    * Selecteer bij Access type de optie **Programmatic access**. Op deze manier kan de gebruiker de API's aanroepen en de rollen van het AWS-account ophalen.

    * Klik in de rechterbenedenhoek op de knop **Next Permissions**.

18. Maak nu een nieuw beleid voor deze gebruiker door de volgende stappen uit te voeren:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser2.png)

    a. Klik op de knop **Attach existing policies directly**.

    b. Zoek het zojuist gemaakte beleid op in de filtersectie **AzureAD_SSOUserRole_Policy**.

    c. Selecteer de **beleid** en klik vervolgens op de knop **Next: Review**.

19. Bekijk het beleid voor de gekoppelde gebruiker door de volgende stappen uit te voeren:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser3.png)

    a. Controleer de gebruikersnaam, het toegangstype en het beleid dat is toegewezen aan de gebruiker.

    b. Klik op de knop **Create user** in de rechterbenedenhoek om de gebruiker te maken.

20. Download de referenties van een gebruiker door de volgende stappen uit te voeren:

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kopieer de **Access key ID** en **Secret access key** van de gebruiker.

    b. Voer deze referenties in in de sectie voor het inrichten van gebruikers van Azure AD om de rollen op te halen uit de AWS-console.

    c. Klik op de knop **Close** onderaan.

21. Navigeer naar de sectie **Gebruikers inrichten** van de Amazon Web Services-app in Azure AD Management Portal.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning.png)

22. Voer de **Toegangssleutel** en het **Geheim** in het veld **Clientgeheim** respectievelijk **Token voor geheim** in.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Voer de gebruikerstoegangssleutel van AWS in het veld **Clientgeheim** in.

    b. Voer het gebruikersgeheim van AWS in het veld **Token voor geheim** in.

    c. Klik op de knop **Verbinding testen** knop om de verbinding te testen.

    d. Sla de instelling op door bovenaan op de knop **Opslaan** te klikken.

23. Zorg er nu voor dat de Inrichtingsstatus op **Aan** staat in de sectie Instellingen door de schakelaar op Aan te zetten en bovenaan op de knop **Opslaan** te klikken.

    ![Gebruiker toevoegen](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. Voer in het veld **gebruikers naam** het username@companydomain.extensionin. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Amazon Web Services (AWS).

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Amazon Web Services (AWS)** in de lijst toepassingen.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-amazon-web-services-aws-test-user"></a>Testgebruiker voor Amazon Web Services (AWS) maken

Het doel van deze sectie is het maken van een gebruiker met de naam B. Simon in Amazon Web Services (AWS). Amazon Web Services (AWS) vereist niet dat een gebruiker is aangemaakt in hun systeem voor eenmalige aanmelding, dus u hoeft hier niets te doen.

### <a name="test-sso"></a>SSO testen

Wanneer u de tegel Amazon Web Services (AWS) in het toegangs venster selecteert, moet u automatisch worden aangemeld bij de Amazon Web Services (AWS) waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="known-issues"></a>Bekende problemen

 * In de sectie **Inrichten** onder de subsectie **Toewijzingen**, wordt het bericht 'Laden...' weergeven, maar nooit de kenmerktoewijzingen. De enige inrichtingswerkstroom die tegenwoordig wordt ondersteund is het importeren van de rollen van AWS naar Azure Active Directory voor selectie tijdens de toewijzing van de gebruiker of groep. De kenmerktoewijzingen hiervoor zijn vooraf bepaald en kunnen niet worden geconfigureerd.
 
 * De sectie **Inrichten** ondersteunt alleen de invoer van één set referenties per AWS-tenant tegelijk. Alle geïmporteerde rollen worden geschreven naar de eigenschap appRoles van het Azure Active Directory [servicePrincipal-object](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) voor de AWS-tenant. Meerdere AWS-tenants (vertegenwoordigd door servicePrincipals) kunnen worden toegevoegd aan Azure Active Directory vanuit de galerie voor het inrichten, maar er is een bekend probleem waarbij u niet meer in staat bent om automatisch alle geïmporteerde rollen van de meerdere AWS servicePrincipals die voor inrichten worden gebruikt, te schrijven naar de enkele servicePrincipal die wordt gebruikt voor eenmalige aanmelding. Als tijdelijke oplossing kan de [Microsoft Graph API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) worden gebruikt om alle appRoles op te halen die zijn geïmporteerd in elke AWS-servicePrincipal waar de inrichting is geconfigureerd. Deze roltekenreeksen kunnen vervolgens worden toegevoegd aan de AWS-servicePrincipal waarop eenmalige aanmelding is geconfigureerd.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

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

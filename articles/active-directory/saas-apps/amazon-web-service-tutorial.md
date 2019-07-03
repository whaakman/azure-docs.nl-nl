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
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f897653442a3e1b2d6098b3be60c85e75ca54f9a
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551482"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Zelfstudie: Amazon webservices (AWS) integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Amazon Web Services (AWS) integreren met Azure Active Directory (Azure AD). Als u Amazon Web Services (AWS) met Azure AD integreren, kunt u het volgende doen:

* Beheren in Azure AD met toegang op Amazon Web Services (AWS).
* Kunnen uw gebruikers worden automatisch aangemeld op Amazon Web Services (AWS) met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

U kunt meerdere id's voor meerdere instanties configureren, zoals hieronder.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Met deze waarden verwijdert Azure AD de waarde van **#** en stuurt de juiste waarde `https://signin.aws.amazon.com/saml` als de doelgroep-URL in het SAML-token.

**We raden u om de volgende redenen aan deze methode te gebruiken:**

a. Elke toepassing krijgt u een unieke X509 certificaat. Elk exemplaar van AWS-App-exemplaar kan vervolgens een ander certificaat-vervaldatum die kan worden beheerd op een afzonderlijk voor AWS-account hebben. Algemene wordt rollover van certificaten eenvoudiger zijn in dit geval.

b. U kunt het inrichten van gebruikers inschakelen met de AWS-app in Azure AD, en onze service haalt dan alle rollen van dat AWS-account op. U hoeft de AWS-rollen in de app niet handmatig toe te voegen of bij te werken.

c. U kunt individueel de app-eigenaar toewijzen voor de app die de app rechtstreeks in Azure AD kan beheren.

> [!Note]
> Zorg ervoor dat u alleen Gallery App gebruikt

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Amazon Web Services (AWS) eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Amazon Web Services (AWS) **SP en IDP** gestart door SSO.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) toevoegen uit de galerie

Om de integratie van Amazon Web Services (AWS) met Azure AD te configureren, moet u Amazon Web Services (AWS) vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Amazon Web Services (AWS)** in het zoekvak in.
1. Selecteer **Amazon Web Services (AWS)** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Amazon Web Services (AWS) met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een relatie tot stand brengt een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amazon Web Services (AWS).

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Amazon Web Services (AWS), voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Amazon Web Services (AWS)](#configure-amazon-web-services-aws)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van Amazon Web Services (AWS) testgebruiker](#create-amazon-web-services-aws-test-user)**  hebben een equivalent van B.Simon in Amazon Web Services (AWS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Amazon Web Services (AWS)** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, de toepassing is vooraf geconfigureerd en de vereiste URL's al vooraf zijn ingevuld met Azure. De gebruiker moet de configuratie op te slaan door te klikken op de **opslaan** knop.

5. Geef de id-waarde op als u meer dan één instantie configureert. Geef de id-waarde voor de tweede en volgende instanties op in de volgende notatie. Gebruik een **#** -teken om een unieke SPN-waarde op te geven.

    `https://signin.aws.amazon.com/saml#2`

6. Amazon Web Services (AWS)-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

7. In aanvulling op de bovenstaande, Amazon Web Services (AWS)-toepassing wordt verwacht dat enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

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

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de **van Amazon Web Services (AWS) ingesteld** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Configureren van Amazon webservices (AWS)

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

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Amazon Web Services (AWS).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Amazon Web Services (AWS)** .
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-amazon-web-services-aws-test-user"></a>Testgebruiker voor Amazon Web Services (AWS) maken

Het doel van deze sectie is het maken van een gebruiker met de naam B.Simon in Amazon Web Services (AWS). Amazon Web Services (AWS) vereist niet dat een gebruiker is aangemaakt in hun systeem voor eenmalige aanmelding, dus u hoeft hier niets te doen.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Amazon Web Services (AWS) in het toegangsvenster selecteert, moet u worden automatisch aangemeld op de Amazon Web Services (AWS) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

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

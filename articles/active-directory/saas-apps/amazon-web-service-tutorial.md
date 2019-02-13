---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: jeedes
ms.openlocfilehash: def9d44c31ed50a859bf42aa148fb7e6a36764fd
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55751086"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS)

In deze zelfstudie leert u hoe u Amazon Web Services (AWS) kunt integreren met Azure Active Directory (Azure AD).
De integratie van Amazon Web Services (AWS) met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie toegang heeft tot Amazon Web Services (AWS).
* U kunt uw gebruikers zich automatisch laten aanmelden bij Amazon Web Services (AWS) (eenmalige aanmelding) met hun Azure AD-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

U kunt meerdere id's voor meerdere instanties configureren, zoals hieronder.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Met deze waarden verwijdert Azure AD de waarde van **#** en stuurt de juiste waarde `https://signin.aws.amazon.com/saml` als de doelgroep-URL in het SAML-token.

**We raden u om de volgende redenen aan deze methode te gebruiken:**

a. Elke toepassing geeft u het unieke X509-certificaat en dus kan elke instantie een andere vervaldatum hebben voor het certificaat en u kunt dit beheren op basis van het individuele AWS-account. In dit geval is algemene certificaatrollover gemakkelijk.

b. U kunt het inrichten van gebruikers inschakelen met de AWS-app in Azure AD, en onze service haalt dan alle rollen van dat AWS-account op. U hoeft de AWS-rollen in de app niet handmatig toe te voegen of bij te werken.

c. U kunt individueel de app-eigenaar toewijzen voor de app die de app rechtstreeks in Azure AD kan beheren.

> [!Note]
> Zorg ervoor dat u alleen Gallery App gebruikt

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Amazon Web Services (AWS):

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een Amazon Web Services (AWS) abonnement met eenmalige aanmelding ingeschakeld

> [!NOTE]
> Als u de stappen in deze zelfstudie wilt testen, is het raadzaam om niet de productieomgeving te gebruiken.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

> [!Note]
> Als u meerdere AWS-accounts wilt integreren met één Azure-account voor eenmalige aanmelding, raadpleegt u [dit](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artikel.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Amazon Web Services (AWS) ondersteunt door **SP en IDP** gestarte SSO

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) toevoegen uit de galerie

Om de integratie van Amazon Web Services (AWS) met Azure AD te configureren, moet u Amazon Web Services (AWS) vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Amazon Web Services (AWS) toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Amazon Web Services (AWS)** in het zoekvak, selecteer **Amazon Web Services (AWS)** in het resultatenvenster, en klik op **Toevoegen**  om de toepassing toe te voegen.

     ![Amazon Web Services (AWS) in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Amazon Web Services (AWS) op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Amazon Web Services (AWS) tot stand is gebracht.

U moet de volgende bouwstenen voltooien om Azure AD Single Sign-On met Amazon Web Services (AWS) te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Amazon Web Services (AWS) configureren](#configure-amazon-web-services-aws-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Amazon Web Services (AWS) maken](#create-amazon-web-services-aws-test-user)**: als u een tegenhanger van Britta Simon in Amazon Web Services (AWS) wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding met Azure AD te configureren voor Amazon Web Services (AWS):

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de overzichtspagina van de integratie voor **Amazon Web Services (AWS)** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **SAML-basisconfiguratie** hoeft de gebruiker geen enkele stap uit te voeren omdat de app al vooraf is geïntegreerd met Azure.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Geef de id-waarde op als u meer dan één instantie configureert. Geef de id-waarde voor de tweede en volgende instanties op in de volgende notatie. Gebruik een **#**-teken om een unieke SPN-waarde op te geven.

    `https://signin.aws.amazon.com/saml#2`

    ![Domein- en URL-informatie voor eenmalige aanmelding bij Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_identifier.png)

6. Amazon Web Services (AWS) verwacht de SAML-asserties in een specifieke notatie. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Naam  | Bronkenmerk  | Naamruimte |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
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

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. In het gedeelte **Amazon Web Services (AWS) instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>Eenmalige aanmelding bij Amazon Web Services (AWS) configureren

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

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Amazon Web Services (AWS).

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **Amazon Web Services (AWS)**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecter **Amazon Web Services (AWS)** in de lijst met toepassingen.

    ![De koppeling Amazon Web Services (AWS) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-amazon-web-services-aws-test-user"></a>Testgebruiker voor Amazon Web Services (AWS) maken

Het doel van deze sectie is het maken van een gebruiker met de naam Britta Simon in Amazon Web Services (AWS). Amazon Web Services (AWS) vereist niet dat een gebruiker is aangemaakt in hun systeem voor eenmalige aanmelding, dus u hoeft hier niets te doen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel Amazon Web Services (AWS) klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van Amazon Web Services (AWS) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="known-issues"></a>Bekende problemen

 * In de sectie **Inrichten** onder de subsectie **Toewijzingen**, wordt het bericht 'Laden...' weergeven, maar nooit de kenmerktoewijzingen. De enige inrichtingswerkstroom die tegenwoordig wordt ondersteund is het importeren van de rollen van AWS naar Azure Active Directory voor selectie tijdens de toewijzing van de gebruiker of groep. De kenmerktoewijzingen hiervoor zijn vooraf bepaald en kunnen niet worden geconfigureerd.
 
 * De sectie **Inrichten** ondersteunt alleen de invoer van één set referenties per AWS-tenant tegelijk. Alle geïmporteerde rollen worden geschreven naar de eigenschap appRoles van het Azure Active Directory [servicePrincipal-object](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/serviceprincipal) voor de AWS-tenant. Meerdere AWS-tenants (vertegenwoordigd door servicePrincipals) kunnen worden toegevoegd aan Azure Active Directory vanuit de galerie voor het inrichten, maar er is een bekend probleem waarbij u niet meer in staat bent om automatisch alle geïmporteerde rollen van de meerdere AWS servicePrincipals die voor inrichten worden gebruikt, te schrijven naar de enkele servicePrincipal die wordt gebruikt voor eenmalige aanmelding. Als tijdelijke oplossing kan de [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/serviceprincipal) worden gebruikt om alle appRoles op te halen die zijn geïmporteerd in elke AWS-servicePrincipal waar de inrichting is geconfigureerd. Deze roltekenreeksen kunnen vervolgens worden toegevoegd aan de AWS-servicePrincipal waarop eenmalige aanmelding is geconfigureerd.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

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

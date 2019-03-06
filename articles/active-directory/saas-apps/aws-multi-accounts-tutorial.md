---
title: 'Zelfstudie: Azure Active Directory-integratie met Amazon Web Services (AWS) naar meerdere accounts verbinden | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure AD en meerdere accounts van Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 322615203d188581dd04aadeff2a08307b733d06
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57450563"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Zelfstudie: Azure Active Directory-integratie met meerdere accounts van Amazon Web Services (AWS)

In deze zelfstudie leert u over het integreren van Azure Active Directory (Azure AD) met meerdere accounts van Amazon Web Services (AWS).

De integratie van Amazon Web Services (AWS) met Azure AD heeft de volgende voordelen:

- U kunt in Azure AD beheren wie toegang heeft tot Amazon Web Services (AWS).
- U kunt uw gebruikers automatisch ophalen aangemeld op Amazon Web Services (AWS) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

![Amazon Web Services (AWS) in de resultatenlijst](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

>[!NOTE]
>Houd er rekening mee een AWS-app verbinden met al uw AWS-accounts is niet onze aanbevolen aanpak. In plaats daarvan raden we u om te gebruiken [dit](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) benadering voor het configureren van meerdere exemplaren van AWS-account naar meerdere exemplaren van AWS-apps in Azure AD.

**Houd er rekening mee dat we niet raden aan deze aanpak gebruiken om volgende redenen:**

* U moet de aanpak van Graph Explorer gebruiken voor het vullen van de rollen aan de app. U kunt beter geen de aanpak van het manifestbestand.

* We hebben gezien klanten melden dat na het toevoegen van ~ 1200 app-rollen voor een AWS-app, een bewerking op de app gestart de fouten met betrekking tot de grootte. Er is een vaste limiet van de grootte van het toepassingsobject.

* U moet de rol handmatig bijwerken als de rollen die zijn toegevoegd in een van de accounts die helaas is een benadering vervangen en niet toevoegen. Als uw accounts er die vervolgens dit groeien zijn wordt ook n x n-relatie met accounts en rollen.

* Alle AWS-accounts gebruikt hetzelfde federatieve metagegevens-XML-bestand en op het moment van rollover van certificaten die u hebt om deze enorme oefening voor het bijwerken van het certificaat op alle AWS-accounts op hetzelfde moment te stimuleren

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Amazon Web Services (AWS):

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een Amazon Web Services (AWS) abonnement met eenmalige aanmelding ingeschakeld

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Amazon Web Services (AWS) ondersteunt door **SP en IDP** gestarte SSO

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Amazon Web Services (AWS) toevoegen uit de galerie

Om de integratie van Amazon Web Services (AWS) met Azure AD te configureren, moet u Amazon Web Services (AWS) vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Amazon Web Services (AWS) toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Amazon Web Services (AWS)** in het zoekvak, selecteer **Amazon Web Services (AWS)** in het resultatenvenster, en klik op **Toevoegen**  om de toepassing toe te voegen.

     ![Amazon Web Services (AWS) in de resultatenlijst](common/search-new-app.png)

5. Nadat de toepassing wordt toegevoegd, gaat u naar **eigenschappen** pagina en kopieer de **Object-ID**.

    ![Amazon Web Services (AWS) in de resultatenlijst](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Amazon Web Services (AWS) op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Amazon Web Services (AWS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amazon Web Services (AWS) tot stand worden gebracht.

In Amazon Web Services (AWS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

U moet de volgende bouwstenen voltooien om Azure AD Single Sign-On met Amazon Web Services (AWS) te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Amazon Web Services (AWS) configureren](#configure-amazon-web-services-aws-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Amazon Web Services (AWS).

**Voor het configureren van Azure AD eenmalige aanmelding met Amazon Web Services (AWS), moet u de volgende stappen uitvoeren:**

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de overzichtspagina van de integratie voor **Amazon Web Services (AWS)** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **SAML-basisconfiguratie** hoeft de gebruiker geen enkele stap uit te voeren omdat de app al vooraf is geïntegreerd met Azure.

    ![image](common/preintegrated.png)

5. Amazon Web Services (AWS) verwacht de SAML-asserties in een specifieke notatie. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](common/edit-attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Name  | Bronkenmerk  | Naamruimte |
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

7. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  en sla deze op uw computer.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

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

11. Meld u af van de huidige AWS-account en meld u aan met een ander account waar u configureren van eenmalige aanmelding wilt op Azure AD.

12. Voer stap-2 tot en met stap-10 om meerdere rollen die u instellen voor dit account wilt te maken. Als u meer dan twee accounts hebt, Voer u dezelfde stappen voor alle accounts om rollen te maken voor hen.

13. Zodra alle rollen in de accounts worden gemaakt, worden ze weergegeven in de **rollen** lijst voor deze accounts.

    ![Rollen instellen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

14. We moeten om vast te leggen van de rol van informatie en vertrouwde entiteiten voor alle rollen voor alle accounts, die we nodig hebben om toe te wijzen handmatig met Azure AD-toepassing.

15. Klik op de rollen te kopiëren **rol informatie** en **vertrouwde entiteiten** waarden. U hebt deze waarden nodig voor alle functies die u wilt maken in Azure AD.

    ![Rollen instellen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)

16. De bovenstaande stap voor alle rollen in alle accounts uitvoeren en sla ze allemaal in de indeling **rol informatie, vertrouwde entiteiten** in Kladblok.

17. Open [Azure AD Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) in een nieuw venster.

    a. Meld u aan de site van de Graph Explorer met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

    b. U moet voldoende machtigingen voor het maken van de rollen hebben. Klik op **wijzigingsmachtigingen** de vereiste machtigingen op te halen.

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Selecteer de volgende machtigingen in de lijst (als u dit nog niet hebt) en klikt u op 'Machtigingen wijzigen' 

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Hiermee vraagt u zich opnieuw aanmelden en de toestemming accepteren. Na de toestemming accepteren, bent u aangemeld bij de Graph Explorer het opnieuw.

    e. Wijzig de vervolgkeuzelijst versie om de **Bèta**. Om op te halen van alle Service-Principals in uw tenant, gebruikt u de volgende query:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Als u meerdere mappen, en vervolgens kunt u de volgende patroon met het primaire domein in het `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Ophalen uit de lijst met Service-Principals die zijn opgehaald, de versie die u wilt wijzigen. U kunt ook de Ctrl + F om te zoeken naar de toepassing van de vermelde ServicePrincipals gebruiken. U kunt de volgende query met behulp van de **Object-id** die u hebt gekopieerd vanaf de pagina Azure AD-eigenschappen om op te halen voor de desbetreffende Service-Principal.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. De eigenschap appRoles onttrekken aan het service-principal-object.

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Nu moet u voor het genereren van nieuwe functies voor uw toepassing. 

    i. Is een voorbeeld van appRoles object hieronder JSON. Een soortgelijk object om toe te voegen van de rollen die u voor uw toepassing wilt maken.

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
    > U kunt alleen nieuwe rollen na toevoegen de **msiam_access** voor de patch-bewerking. U kunt ook rollen die u wilt dat uw organisatie moet toevoegen. Azure AD stuurt de **waarde** van deze rollen als de waarde van de claim in SAML-antwoord.

    j. Ga terug naar uw Graph Explorer en de methode wijzigen **ophalen** naar **PATCH**. Patch uitvoeren voor het Service-Principal-object hebt rollen gewenst door bij te werken appRoles eigenschap die vergelijkbaar is met de hierboven weergegeven in het voorbeeld. Klik op **Query uitvoeren** de patchbewerking uit te voeren. Een bericht wordt bevestigd dat het maken van de rol voor uw toepassing Amazon Web Services.

    ![Dialoogvenster voor Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

18. Nadat de Service-Principal is gevuld met meer functies, kunt u gebruikers/groepen toewijzen aan de respectieve rollen. Dit kan worden gedaan door te gaan naar de portal en te navigeren naar de Amazon Web Services-toepassing. Klik op de **gebruikers en groepen** tabblad in de rechterbovenhoek.

19. We raden u aan te maken van nieuwe groepen voor elke rol (AWS) zodat u die specifieke rol in de groep kunt toewijzen. Houd er rekening mee dat dit een-op-een-toewijzing voor een groep aan één rol is. Vervolgens kunt u de leden die deel uitmaken van die groep toevoegen.

20. Nadat de groepen zijn gemaakt, selecteert u de groep en toewijzen aan de toepassing.

    ![Configureren van eenmalige aanmelding toevoegen](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > Geneste groepen worden niet ondersteund bij het toewijzen van groepen.

21. Als u wilt de rol toewijzen aan de groep, selecteert u de rol en klik op **toewijzen** knop in de onderaan de pagina.

    ![Configureren van eenmalige aanmelding toevoegen](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Houd er rekening mee dat u wilt vernieuwen van uw sessie in Azure portal om te zien van de nieuwe rollen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u klikt op de tegel Amazon Web Services (AWS) in het toegangsvenster, krijgt u moet de toepassingspagina Amazon Web Services (AWS) met de optie om de rol te selecteren.

![Configureren van eenmalige aanmelding toevoegen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

U kunt ook controleren of het SAML-antwoord om te zien van de rollen die worden doorgegeven als claims.

![Configureren van eenmalige aanmelding toevoegen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

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
[41]: ./media/aws-multi-accounts-tutorial/

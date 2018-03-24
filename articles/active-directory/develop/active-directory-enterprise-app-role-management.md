---
title: Configureren rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory | Microsoft Docs
description: Informatie over het configureren van de rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 88a9f5988d1fe3f4de4fe10da23a5f713e3f3370
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="configuring-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory configureren

Deze functie kunt gebruikers om aan te passen van het claimtype voor de claim 'functies' in het antwoord token ontvangen bij het verlenen van een app met Azure AD.

## <a name="prerequisites"></a>Vereisten
- Een Azure AD-abonnement met de installatie van de directory
- Een eenmalige aanmelding ingeschakeld abonnement
- U moet eenmalige aanmelding met uw toepassing configureren

## <a name="when-to-use-this-feature"></a>Wanneer deze functie wilt gebruiken

Als uw toepassing aangepaste rollen in SAML-reactie die moet worden doorgegeven verwacht, moet u deze functie te gebruiken. Deze functie kunt u rollen die u wilt terug van Azure AD worden doorgegeven aan uw toepassing te maken.

## <a name="steps-to-use-this-feature"></a>Stappen om deze functie te gebruiken

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak de naam van uw toepassing, selecteer uw toepassing uit resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Toepassing in de lijst met resultaten](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Wanneer de toepassing is toegevoegd, gaat u naar **eigenschappen** pagina en kopieer de **Object-ID**

    ![Eigenschappenpagina](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

    a. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

    b. Wijzigen van de versie van de **beta** en ophalen van de lijst met de service-Principals van uw tenant met behulp van volgende query:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    c. Ophalen uit de lijst met de service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    d. De eigenschap appRoles extraheren uit het service-principal-object.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    e. U moet nieuwe functies voor uw toepassing nu te genereren. U kunt downloaden via Azure AD-functie Generator van [hier](https://app.box.com/s/jw6m9p9ehmf4ut5jx8xhcw87cu09ml3y).

    f. Open de Azure AD-Generator en volgende stappen - uitvoeren

    ![Azure AD-Generator](./media/active-directory-enterprise-app-role-management/azure_ad_role_generator.png)
    
    Voer **rolnaam**, **beschrijving van de functie**, en **waarde voor de rol**. Klik op **toevoegen** de rol toe te voegen
    
    Nadat u de vereiste functies hebt toegevoegd, klikt u op **genereren**
    
    Kopieer de inhoud door te klikken op **inhoud kopiëren**

    > [!NOTE] 
    > Zorg ervoor dat u hebt **msiam_access** gebruikersrol en de id komt overeen met de gegenereerde rol.

    g. Ga terug naar uw grafiek Explorer. De methode van wijzigen **ophalen** naar **PATCH**. Patch voor het service-principal-object om te hebben appRoles gewenst door appRoles eigenschap met de gekopieerde waarden bij te werken. Klik op **Query uitvoert**.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-patch.png)

    > [!NOTE]
    > Hieronder volgt een voorbeeld van appRoles-object. 
    > 
    >   ```
    > {
    > "appRoles": [
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "msiam_access",
    >   "displayName": "msiam_access",
    >   "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
    >   "isEnabled": true,
    >   "origin": "Application",
    >   "value": null
    > },
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "teacher",
    >   "displayName": "teacher",
    >   "id": "6478ffd2-5dbd-4584-b2ce-137390b09b60",
    >   "isEnabled": ,
    >   "origin": "ServicePrincipal",
    >   "value": "teacher"
    > }
    > ] 
    > }
    >
    >   ```

7. Nadat de service-principal is een patch uitgevoerd met meer functies, kunnen we gebruikers toewijzen aan de betreffende rollen. Dit kan worden gedaan door te gaan naar de Portal en navigeren naar de betreffende app. Vervolgens te klikken op de **gebruikers en groepen** tabblad bovenaan. Dit proces wordt een lijst alle gebruikers of groepen.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/userrole.png)

    a. Als u wilt een rol toewijzen aan een gebruiker, alleen de bepaalde gebruikersgroep selecteren en klik op **toewijzen** knop in het onderste gedeelte van de pagina.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/userandgroups.png)

    b. Te klikken op die brengt een pop-upvenster een rol selecteren uit verschillende rollen die zijn gedefinieerd voor de betreffende service-principal.

    c. Kies de vereiste functie en klik op verzenden.

8. Na het toewijzen van rollen aan de gebruikers, moeten we werken **kenmerken** tabel voor het definiëren van aangepaste toewijzing van **rol** claim.

9. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | -------------- | ----------------|    
    | Naam van rol      | user.assignedrole |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Kenmerk voor eenmalige aanmelding configureren](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

10. Voor het testen van uw toepassing in de IDP eenmalige aanmelding op de machine gestart, logboek in het deelvenster toegang (https://myapps.microsoft.com) en klik op de tegel voor uw toepassing. U ziet in het SAML-token toegewezen rollen voor de gebruiker met de naam van de claim die u hebt gegeven.

## <a name="update-existing-role"></a>De rol van de bestaande bijwerken

1. Voor het bijwerken van een bestaande rol uitvoeren na de stap-

    a. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

    b. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.
    
    c. Wijzigen van de versie van de **beta** en ophalen van de lijst met de service-Principals van uw tenant met behulp van volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
        
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Ophalen uit de lijst met de service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. De eigenschap appRoles extraheren uit het service-principal-object.
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)
    
    f. Volg onderstaande stappen voor het bijwerken van de bestaande rol:

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * De methode van wijzigen **ophalen** naar **PATCH**.

    * Kopieer de bestaande rollen van de toepassing en plak ze in de **aanvraagtekst**.
    
    * Werk de waarde van de rol door te vervangen de **beschrijving van de functie**, **waarde voor de rol**, en **rol displayname** volgens de organisatievereisten van uw.
    
    * Nadat u de vereiste functies hebt bijgewerkt, klikt u op **Query uitvoeren**.
        
## <a name="delete-existing-role"></a>Bestaande rol verwijderen

1. Voor het verwijderen van een bestaande rol uitvoeren na de stap-

    a. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

    b. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

    c. Wijzigen van de versie van de **beta** en ophalen van de lijst met de service-Principals van uw tenant met behulp van volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Ophalen uit de lijst met de service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.
    
    e. De eigenschap appRoles extraheren uit het service-principal-object.
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    f. Volg onderstaande stappen voor het verwijderen van de bestaande rol:

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-patchdelete.png)

    De methode van wijzigen **ophalen** naar **PATCH**.

    De bestaande rollen van de toepassing Kopieer en plak deze in de **aanvraagtekst**.
    
    Stel de **IsEnabled** van waarde naar **false** voor de rol die u wilt verwijderen

    Klik op **Query uitvoert**.
    
    > [!NOTE] 
    > Zorg ervoor dat u hebt **msiam_access** gebruikersrol en de id komt overeen met de gegenereerde rol.
    
    g. Hierna moet u de bovenstaande procedure, blijven de methode als **PATCH** en plak de functie remianing inhoud in de **aanvraagtekst** en klik op **Query uitvoeren**.
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-patchfinal.png)

    h. Na het uitvoeren van de query wordt de rol worden verwijderd.
    
    > [!NOTE]
    > De rol moet eerst worden uitgeschakeld voordat het kan worden verwijderd. 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg [documentatie App ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-tutorial-list) voor extra stappen uitvoeren.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png

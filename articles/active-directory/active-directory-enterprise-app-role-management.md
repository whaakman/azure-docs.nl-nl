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
ms.openlocfilehash: 94b451f66d286426f6dd2cc556e8c6785c3f743e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
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

5. Wanneer de toepassing is toegevoegd, gaat u naar **eigenschappen** pagina en kopieer de **Object-ID**.

    ![Eigenschappenpagina](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.PNG)

6. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

    a. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

    b. U moet voldoende rechten hebt voor het maken van de rollen. Klik op **wijzigingsmachtiging** de vereiste machtigingen op te halen. 

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Selecteer volgende machtigingen in de lijst (als u nog geen deze hebt) en klik op 'Wijzigingsmachtigingen' 

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Dit kunt u zich opnieuw aanmelden en accepteert de toestemming wordt gevraagd. Na de toestemming accepteren, bent u aangemeld bij het systeem opnieuw.

    e. Wijzigen van de versie van de **beta** en ophalen van de lijst met de Service-Principals van uw tenant met behulp van volgende query:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Ophalen uit de lijst met de service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. De eigenschap appRoles extraheren uit het service-principal-object. 

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

    > [!Note]
    > Als u de aangepaste (niet-galerie)-app gebruikt, ziet u de twee standaardrollen - gebruiker en msiam_access. In geval van een galerij-app is msiam_access de enige standaardrol. U hoeft niet te wijzigen in de standaardrollen.

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
            "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Administrators Only",
            "displayName": "Admin",
            "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "Administrator"
        }
    ]
    }
    ```
    > [!Note]
    > U kunt alleen nieuwe rollen na toevoegen de **msiam_access** voor de patch-bewerking. U kunt ook de rollen die u wilt dat uw organisatie behoefte toevoegen. Azure AD ontvangt de **waarde** van deze rollen als de claimwaarde SAML-reactie.
    
    j. Ga terug naar uw grafiek Explorer en wijzig de methode van **ophalen** naar **PATCH**. Patch voor het service-principal-object om rollen hebben gewenst door het bijwerken van appRoles eigenschap lijkt op de hierboven weergegeven in het voorbeeld. Klik op **Query uitvoeren** de patch-bewerking uit te voeren. Een bericht wordt bevestigd dat het maken van de rol.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Nadat de service-principal is een patch uitgevoerd met meer functies, kunt u gebruikers toewijzen aan de betreffende rollen. Dit kan worden gedaan door te gaan naar de portal en navigeren naar de betreffende app. Klik op de **gebruikers en groepen** tabblad bovenaan. Hiermee worden alle gebruikers en groepen die al zijn toegewezen aan de toepassing. U kunt nieuwe gebruikers toevoegen van nieuwe functie en kunt ook bestaande gebruiker selecteren en op **bewerken** kunt u de serverfunctie.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

     Als u wilt de rol toewijst aan een gebruiker, selecteer de nieuwe rol en klik op **toewijzen** knop in onder aan de pagina.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Houd er rekening mee dat u wilt vernieuwen van uw sessie in Azure portal om te zien van nieuwe rollen.

8. Na het toewijzen van rollen aan de gebruikers, moeten we werken **kenmerken** tabel voor het definiëren van aangepaste toewijzing van **rol** claim.

9. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | -------------- | ----------------|    
    | Naam van rol      | User.assignedrole |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Kenmerk voor eenmalige aanmelding configureren](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. In de **naam** textbox, typt u de kenmerknaam indien nodig. In dit voorbeeld hebben we gebruikt **rolnaam** als de naam van claim.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

10. Voor het testen van uw toepassing in de IDP eenmalige aanmelding op de machine gestart, logboek in het deelvenster toegang (https://myapps.microsoft.com) en klik op de tegel voor uw toepassing. In het SAML-token ziet u de toegewezen rollen voor de gebruiker met de naam van de claim die u hebt gegeven.

## <a name="update-existing-role"></a>De rol van de bestaande bijwerken

Voor het bijwerken van een bestaande rol uitvoeren na de stap-

1. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer).

2. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.
    
3. Wijzigen van de versie van de **beta** en ophalen van de lijst met de service-Principals van uw tenant met behulp van volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Ophalen uit de lijst met de Service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. De eigenschap appRoles extraheren uit het service-principal-object.
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Volg onderstaande stappen voor het bijwerken van de bestaande rol:

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * De methode van wijzigen **ophalen** naar **PATCH**.

    * De bestaande rollen Kopieer en plak ze in de **aanvraagtekst**.

    * Werk de waarde van de rol door het bijwerken van de **beschrijving van de functie**, **waarde voor de rol** of **rol displayname** indien nodig.

    * Nadat u de vereiste functies hebt bijgewerkt, klikt u op **Query uitvoeren**.
        
## <a name="delete-existing-role"></a>Bestaande rol verwijderen

Een bestaande als rol wilt verwijderen, voert u de volgende stappen:

1. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

2. Meld u aan de grafiek Explorer-site met de referenties van de globale beheerder/Co-beheerder voor uw tenant.

3. Wijzigen van de versie van de **beta** en ophalen van de lijst met de service-Principals van uw tenant met behulp van volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Als u meerdere mappen gebruikt, moet vervolgens u dit patroon `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Ophalen uit de lijst met de service-Principals die zijn opgehaald, die u wilt wijzigen. U kunt de Ctrl + F ook gebruiken om te zoeken van de toepassing van de vermelde ServicePrincipals. Zoeken naar de **Object-id**, die u hebt gekopieerd uit de pagina eigenschappen en gebruikt u de volgende query om op te halen voor de betreffende Service-Principal.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. De eigenschap appRoles extraheren uit het service-principal-object.
    
    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Volg onderstaande stappen voor het verwijderen van de bestaande rol:

    ![Dialoogvenster van grafiek explorer](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    * De methode van wijzigen **ophalen** naar **PATCH**.

    * De bestaande rollen van de toepassing Kopieer en plak deze in de **aanvraagtekst**.
        
    * Stel de **IsEnabled** van waarde naar **false** voor de rol die u wilt verwijderen

    * Klik op **Query uitvoert**.
    
    > [!NOTE] 
    > Zorg ervoor dat u hebt **msiam_access** gebruikersrol en de id komt overeen met de gegenereerde rol.
    
7. Zodra de functie is uitgeschakeld, dat blok rol verwijderen uit de sectie appRoles, blijven de methode als **PATCH** en klik op **Query uitvoeren**.
    
8. Na het uitvoeren van de query wordt de rol worden verwijderd.
    
    > [!NOTE]
    > De rol moet eerst worden uitgeschakeld voordat het kan worden verwijderd. 

## <a name="next-steps"></a>Volgende stappen

Raadpleeg [documentatie App ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) voor extra stappen uitvoeren.

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png

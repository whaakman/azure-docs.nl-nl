---
title: De rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory configureren | Microsoft Docs
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
ms.date: 05/30/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: c9a1d605f6cf2ef9dae3a5549e3848931d508394
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082740"
---
# <a name="configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>De rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory configureren

U kunt met behulp van Azure Active Directory (Azure AD), het claimtype aanpassen voor de rol claim in het antwoord-token dat wordt weergegeven nadat u een app autoriseren.

## <a name="prerequisites"></a>Vereisten
- Een Azure AD-abonnement met de installatie van de directory.
- Een abonnement dat is eenmalige aanmelding (SSO) ingeschakeld. Met uw toepassing, moet u eenmalige aanmelding configureren.

## <a name="when-to-use-this-feature"></a>Wanneer deze functie wilt gebruiken

Als uw toepassing aangepaste rollen in een SAML-reactie die moet worden doorgegeven verwacht, moet u deze functie te gebruiken. U kunt zoveel rollen als nodig is voor terug van Azure AD worden doorgegeven aan uw toepassing maken.

## <a name="create-roles-for-an-application"></a>Functies voor een toepassing maken

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![Azure Active Directory-pictogram][1]

2. Selecteer **bedrijfstoepassingen**. Selecteer vervolgens **alle toepassingen**.

    ![Deelvenster voor Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    !['Nieuwe application' knop][3]

4. In het zoekvak, typ de naam van uw toepassing en selecteer vervolgens uw toepassing in het deelvenster resultaten. Selecteer de **toevoegen** om toe te voegen van de toepassing.

    ![Toepassing in de lijst met resultaten](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Nadat de toepassing is toegevoegd, gaat u naar de **eigenschappen** pagina en kopieer de object-ID.

    ![Eigenschappenpagina](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Open de [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) in een ander venster en voer de volgende stappen uit:

    a. Meld u aan de grafiek Explorer-site met behulp van de globale beheerder of coadmin referenties voor uw tenant.

    b. U moet voldoende machtigingen voor het maken van de rollen. Selecteer **wijzigingsmachtiging** de machtigingen op te halen. 

      ![De knop 'wijzigingsmachtiging'](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Selecteer de volgende machtigingen in de lijst (als u nog geen deze hebt) en selecteer **wijzigingsmachtigingen**. 

      ![Lijst met machtigingen en de knop 'Wijzigingsmachtigingen'](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. De toestemming accepteren. U bent aangemeld bij het systeem opnieuw.

    e. Wijzigen van de versie van de **beta**, en het ophalen van de lijst met de service-principals van uw tenant met behulp van de volgende query:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
      Als u meerdere mappen, moet u dit patroon volgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
      ![Grafiek Explorer in het dialoogvenster met de query voor het ophalen van de service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Ophalen uit de lijst met opgehaalde service-principals, die u wilt wijzigen. U kunt Ctrl + F ook gebruiken om te zoeken van de toepassing van alle vermelde service-principals. Zoeken naar de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te krijgen tot de service-principal:
    
      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Pak de **appRoles** eigenschap van het service-principal-object. 

      ![Details van de eigenschap appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Als u de aangepaste app (niet de Azure Marketplace-app), ziet u twee standaardrollen: gebruikers en msiam_access. Voor de Marketplace-app is msiam_access de enige standaardrol. U hoeft niet te wijzigen in de standaardrollen.

    h. Nieuwe functies voor uw toepassing worden gegenereerd. 

      De volgende JSON is een voorbeeld van de **appRoles** object. Maak een vergelijkbaar object op om de functies die u voor uw toepassing wilt toevoegen. 

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
      > U kunt alleen nieuwe rollen toevoegen na msiam_access voor de patch-bewerking. U kunt ook zo veel functies toevoegen als de behoeften van uw organisatie. Azure AD wordt de waarde van deze rollen worden verzonden als de waarde van de claim in de SAML-reactie. Voor het genereren van de GUID waarden voor de ID van nieuwe functies gebruik van de web-hulpmiddelen zoals [dit](https://www.guidgenerator.com/)
    
    i. Ga terug naar grafiek Explorer en wijzig de methode van **ophalen** naar **PATCH**. Patch voor het service-principal-object om de gewenste rollen door het bijwerken van de **appRoles** eigenschap zoals weergegeven in het voorgaande voorbeeld. Selecteer **Query uitvoeren** de patch-bewerking uit te voeren. Een bericht wordt bevestigd dat het maken van de rol.

      ![Patch-bewerking met succes bericht](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Nadat de service-principal is een patch uitgevoerd met meer functies, kunt u gebruikers toewijzen aan de betreffende rollen. U kunt de gebruikers toewijzen door te gaan naar de portal en bladeren naar de toepassing. Selecteer de **gebruikers en groepen** tabblad. Dit tabblad bevat alle gebruikers en groepen die al zijn toegewezen aan de app. U kunt nieuwe gebruikers toevoegen op de nieuwe rollen. U kunt ook een bestaande gebruiker en selecteren **bewerken** kunt u de serverfunctie.

    ![Tabblad 'Gebruikers en groepen'](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Als u wilt de rol toewijst aan een gebruiker, selecteert u de nieuwe rol en selecteert u de **toewijzen** knop aan de onderkant van de pagina.

    ![Deelvenster 'Toewijzing bewerken' en 'Rol selecteren' deelvenster](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > U moet uw sessie in de Azure portal om te zien van nieuwe functies te vernieuwen.

8. Update de **kenmerken** tabel voor het definiëren van een aangepaste toewijzing van de claim rol.

9. In de **gebruikerskenmerken** sectie van de **eenmalige aanmelding** dialoogvenster vak, het kenmerk van SAML-token te configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren.
    
    | Naam van kenmerk | De waarde van kenmerk |
    | -------------- | ----------------|    
    | Rolnaam      | User.assignedrole |

    a. Selecteer **toevoegen kenmerk** openen de **kenmerk toevoegen** deelvenster.

      ![Knop 'Kenmerk toevoegen'](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      ![Deelvenster 'Kenmerk toevoegen'](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. In de **naam** typt u de kenmerknaam indien nodig. In dit voorbeeld wordt **rolnaam** als de claimnaam van de.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** vak leeg.
    
    e. Selecteer **Ok**.

10. Testen van uw toepassing in een eenmalige aanmelding die geïnitieerd door een id-provider, moet u zich aanmelden bij de [Toegangspaneel](https://myapps.microsoft.com) en selecteer de tegel voor uw toepassing. In het SAML-token ziet u de toegewezen rollen voor de gebruiker met de naam van de claim die u hebt opgegeven.

## <a name="update-an-existing-role"></a>De rol van een bestaande bijwerken

Voor het bijwerken van een bestaande rol, moet u de volgende stappen uitvoeren:

1. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer).

2. Meld u aan de grafiek Explorer-site met behulp van de globale beheerder of coadmin referenties voor uw tenant.
    
3. Wijzigen van de versie van de **beta**, en het ophalen van de lijst met de service-principals van uw tenant met behulp van de volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Als u meerdere mappen, moet u dit patroon volgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Grafiek Explorer in het dialoogvenster met de query voor het ophalen van de service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Ophalen uit de lijst met opgehaalde service-principals, die u wilt wijzigen. U kunt Ctrl + F ook gebruiken om te zoeken van de toepassing van alle vermelde service-principals. Zoeken naar de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te krijgen tot de service-principal:
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Pak de **appRoles** eigenschap van het service-principal-object.
    
    ![Details van de eigenschap appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Gebruik de volgende stappen voor het bijwerken van de bestaande rol.

    ![Aanvraagtekst voor 'PATCH', 'beschrijving' en 'weergavenaam' gemarkeerd](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    a. De methode van wijzigen **ophalen** naar **PATCH**.

    b. De bestaande rollen Kopieer en plak deze onder **aanvraagtekst**.

    c. Werk de waarde van een rol door het bijwerken van de beschrijving van de functie, waarde voor de rol of functie weergavenaam indien nodig.

    d. Nadat u de vereiste functies bijwerken, selecteert u **Query uitvoeren**.
        
## <a name="delete-an-existing-role"></a>Een bestaande rol verwijderen

Als u wilt verwijderen van een bestaande rol, moet u de volgende stappen uitvoeren:

1. Open [Explorer van Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) in een ander venster.

2. Meld u aan de grafiek Explorer-site met behulp van de globale beheerder of coadmin referenties voor uw tenant.

3. Wijzigen van de versie van de **beta**, en het ophalen van de lijst met de service-principals van uw tenant met behulp van de volgende query:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Als u meerdere mappen, moet u dit patroon volgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Grafiek Explorer in het dialoogvenster met de query voor het ophalen van de lijst van de service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Ophalen uit de lijst met opgehaalde service-principals, die u wilt wijzigen. U kunt Ctrl + F ook gebruiken om te zoeken van de toepassing van alle vermelde service-principals. Zoeken naar de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te krijgen tot de service-principal:
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Pak de **appRoles** eigenschap van het service-principal-object.
    
    ![Details van de eigenschap appRoles van het service-principal-object](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Gebruik de volgende stappen voor het verwijderen van de bestaande rol.

    ![Aanvraagtekst voor PATCH-,' met IsEnabled ingesteld op false](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. De methode van wijzigen **ophalen** naar **PATCH**.

    b. De bestaande rollen van de toepassing Kopieer en plak deze onder **aanvraagtekst**.
        
    c. Stel de **IsEnabled** van waarde naar **false** voor de rol die u wilt verwijderen.

    d. Selecteer **Query uitvoert**.
    
    > [!NOTE] 
    > Zorg ervoor dat u de rol msiam_access hebt en de ID met de gegenereerde rol overeen komt.
    
7. Nadat u de functie is uitgeschakeld, verwijdert u die rol blokkeren vanuit de **appRoles** sectie. De methode als houden **PATCH**, en selecteer **Query uitvoeren**.
    
8. Nadat u de query uitvoert, wordt de rol verwijderd.
    
    > [!NOTE]
    > De functie moet worden uitgeschakeld voordat het kan worden verwijderd. 

## <a name="next-steps"></a>Volgende stappen

Zie voor aanvullende stappen de [documentatie app](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
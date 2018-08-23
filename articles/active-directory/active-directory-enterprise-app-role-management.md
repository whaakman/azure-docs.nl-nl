---
title: Configureren van de rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory | Microsoft Docs
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
ms.date: 08/10/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: cb4c9f91c7a116e6171a8e94030b6bb40fdb38ea
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42058067"
---
# <a name="configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>De rol claim uitgegeven in het SAML-token voor bedrijfstoepassingen in Azure Active Directory configureren

U kunt met behulp van Azure Active Directory (Azure AD), het claimtype aanpassen voor de rol claim in het antwoordtoken dat wordt weergegeven nadat u een app toestaan.

## <a name="prerequisites"></a>Vereisten
- Een Azure AD-abonnement met de installatie van de directory.
- Een abonnement met eenmalige aanmelding (SSO) ingeschakeld. Met uw toepassing, moet u eenmalige aanmelding configureren.

## <a name="when-to-use-this-feature"></a>Wanneer deze functie wilt gebruiken

Als uw toepassing wordt verwacht dat de aangepaste rollen in een SAML-antwoord wordt doorgegeven, moet u deze functie wilt gebruiken. U kunt zo veel functies als u nodig hebt van Azure AD worden doorgegeven aan uw toepassing kunt maken.

## <a name="create-roles-for-an-application"></a>Rollen voor een toepassing maken

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![Azure Active Directory-pictogram][1]

2. Selecteer **bedrijfstoepassingen**. Selecteer vervolgens **alle toepassingen**.

    ![Deelvenster voor Enterprise applications][2]

3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    !['Nieuwe application' knop][3]

4. In het zoekvak, typ de naam van uw toepassing en selecteer vervolgens uw toepassing vanuit het deelvenster resultaten. Selecteer de **toevoegen** om toe te voegen van de toepassing.

    ![Toepassing in de lijst met resultaten](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Nadat de toepassing is toegevoegd, gaat u naar de **eigenschappen** pagina en kopieer de object-ID.

    ![Eigenschappenpagina](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Open [Azure AD Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) in een ander venster en voer de volgende stappen uit:

    a. Meld u aan de site Graph Explorer met behulp van de globale beheerder of coadmin referenties voor uw tenant.

    b. U moet voldoende machtigingen om de rollen te maken. Selecteer **wijzigingsmachtigingen** de machtigingen op te halen.

      ![De knop "machtigingen wijzigen"](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Selecteer de volgende machtigingen in de lijst (als u dit nog niet hebt) en selecteer **wijzigingsmachtigingen**.

      ![Lijst met machtigingen en de knop 'Machtigingen wijzigen'](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    > [!Note]
    > Functie van cloud App-beheerder en beheerder van de App werkt niet in dit scenario als we de globale beheerder machtigingen nodig hebt voor de map lezen en schrijven.

    d. De toestemming accepteren. U bent aangemeld bij het systeem opnieuw.

    e. Wijzig de versie aan **Bèta**, en de lijst met service-principals in uw tenant ophalen met behulp van de volgende query uit:

     `https://graph.microsoft.com/beta/servicePrincipals`

      Als u meerdere mappen, gaat u als volgt dit patroon: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

      ![Graph Explorer in het dialoogvenster met de query voor het ophalen van service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

      > [!Note]
      > We zijn al bezig met bijwerken van de API's, zodat klanten een onderbreking in de service zien mogelijk.

    f. In de lijst van de opgehaalde service-principals, krijgen die u wilt wijzigen. U kunt ook Ctrl + F om te zoeken naar de toepassing uit alle vermelde service-principals. Zoek de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te gaan naar de service-principal:

      `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

      ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Pak de **appRoles** eigenschap van het service-principal-object.

      ![Details van de eigenschap appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

      > [!Note]
      > Als u de aangepaste app (niet de Azure Marketplace-app), ziet u twee standaardrollen: msiam_access en gebruiker. Voor de Marketplace-app is msiam_access de enige rol die standaard. U hoeft niet te wijzigingen aanbrengen in de standaardrollen.

    h. Nieuwe functies voor uw toepassing worden gegenereerd.

      De volgende JSON wordt een voorbeeld van de **appRoles** object. Een soortgelijk object om toe te voegen de functies die u voor uw toepassing wilt maken.

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
      > U kunt alleen nieuwe rollen toevoegen na msiam_access voor de patch-bewerking. U kunt ook zo veel functies toevoegen naarmate de behoeften van uw organisatie. Azure AD wordt de waarde van deze rollen worden verzonden als de waarde van de claim in het SAML-antwoord. Voor het genereren van de GUID voor de ID van nieuwe functies wordt gebruikgemaakt van de web-hulpprogramma's zoals [dit](https://www.guidgenerator.com/)

    i. Ga terug naar Graph Explorer en de methode wijzigen **ophalen** naar **PATCH**. Patch uitvoeren voor het service-principal-object om de gewenste rollen door bij te werken de **appRoles** eigenschap zoals weergegeven in het voorgaande voorbeeld. Selecteer **Query uitvoeren** de patchbewerking uit te voeren. Een bericht wordt bevestigd dat het maken van de rol.

      ![Patch-bewerking met het bericht geslaagd](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Nadat de service-principal is gevuld met meer functies, kunt u gebruikers kunt toewijzen aan de betreffende rollen. U kunt de gebruikers toewijzen door naar de portal te gaan en te bladeren naar de toepassing. Selecteer de **gebruikers en groepen** tabblad. Op dit tabblad geeft een lijst van alle gebruikers en groepen die al zijn toegewezen aan de app. U kunt nieuwe gebruikers toevoegen voor de nieuwe rollen. U kunt ook een bestaande gebruiker selecteren en selecteer **bewerken** te wijzigen van de rol.

    ![Tabblad 'Gebruikers en groepen'](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Als u wilt de rol toewijzen aan een gebruiker, selecteert u de nieuwe rol en selecteert de **toewijzen** knop aan de onderkant van de pagina.

    ![Deelvenster 'Toewijzing bewerken' en 'Rol selecteren' deelvenster](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > U moet uw sessie in de Azure-portal om te zien van de nieuwe rollen vernieuwen.

8. Update de **kenmerken** tabel voor het definiëren van een aangepaste toewijzing van de rol-claim.

9. In de **gebruikerskenmerken** sectie van de **eenmalige aanmelding** dialoogvenster vak, configureren van het kenmerk van SAML-token, zoals weergegeven in de afbeelding en voer de volgende stappen uit.

    | De naam van kenmerk | De waarde van kenmerk |
    | -------------- | ----------------|
    | Rolnaam  | User.assignedroles |

    a. Selecteer **kenmerk toevoegen** openen de **kenmerk toevoegen** deelvenster.

      ![Knop 'Kenmerk toevoegen'](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

      ![Deelvenster 'Kenmerk toevoegen'](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. In de **naam** typt u de kenmerknaam indien nodig. In dit voorbeeld wordt **rolnaam** als naam van de claim.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** vak leeg.

    e. Selecteer **OK**.

10. Als u wilt uw toepassing testen in een eenmalige aanmelding die geïnitieerd door een id-provider, moet u zich aanmelden bij de [Toegangsvenster](https://myapps.microsoft.com) en selecteer de tegel voor uw toepassing. In het SAML-token ziet u de toegewezen rollen voor de gebruiker met de naam van de claim die u hebt opgegeven.

## <a name="update-an-existing-role"></a>Een bestaande rol bijwerken

Voor het bijwerken van een bestaande rol, moet u de volgende stappen uitvoeren:

1. Open [Azure AD Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

2. Meld u aan de site Graph Explorer met behulp van de globale beheerder of coadmin referenties voor uw tenant.

3. Wijzig de versie aan **Bèta**, en de lijst met service-principals in uw tenant ophalen met behulp van de volgende query uit:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Als u meerdere mappen, gaat u als volgt dit patroon: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph Explorer in het dialoogvenster met de query voor het ophalen van service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. In de lijst van de opgehaalde service-principals, krijgen die u wilt wijzigen. U kunt ook Ctrl + F om te zoeken naar de toepassing uit alle vermelde service-principals. Zoek de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te gaan naar de service-principal:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Pak de **appRoles** eigenschap van het service-principal-object.

    ![Details van de eigenschap appRoles](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

6. Gebruik de volgende stappen voor het bijwerken van de bestaande rol.

    ![Aanvraagtekst voor 'PATCH', 'description' en 'weergavenaam' is gemarkeerd](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)

    a. Wijzigen van de methode **ophalen** naar **PATCH**.

    b. De bestaande rollen Kopieer en plak deze onder **aanvraagtekst**.

    c. Werk de waarde van een rol door bij te werken van de beschrijving van de rol, de waarde voor de rol of de weergavenaam van de rol, indien nodig.

    d. Nadat u de vereiste functies bijwerken, selecteert u **Query uitvoeren**.

## <a name="delete-an-existing-role"></a>Een bestaande rol verwijderen

Als u wilt verwijderen van een bestaande rol, moet u de volgende stappen uitvoeren:

1. Open [Azure AD Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) in een nieuw venster.

2. Meld u aan de site Graph Explorer met behulp van de globale beheerder of coadmin referenties voor uw tenant.

3. Wijzig de versie aan **Bèta**, en de lijst met service-principals in uw tenant ophalen met behulp van de volgende query uit:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Als u meerdere mappen, gaat u als volgt dit patroon: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph Explorer in het dialoogvenster met de query voor het ophalen van de lijst met service-principals](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

4. In de lijst van de opgehaalde service-principals, krijgen die u wilt wijzigen. U kunt ook Ctrl + F om te zoeken naar de toepassing uit alle vermelde service-principals. Zoek de object-ID die u hebt gekopieerd uit de **eigenschappen** pagina en gebruik de volgende query om te gaan naar de service-principal:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Query voor het ophalen van de service-principal die u nodig hebt om te wijzigen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

5. Pak de **appRoles** eigenschap van het service-principal-object.

    ![Details van de eigenschap appRoles van het service-principal-object](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Als u wilt verwijderen van de bestaande rol, gebruik de volgende stappen.

    ![Hoofdtekst van de aanvraag voor 'PATCH,' met IsEnabled ingesteld op false](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    a. Wijzigen van de methode **ophalen** naar **PATCH**.

    b. De bestaande rollen van de toepassing Kopieer en plak deze onder **aanvraagtekst**.

    c. Stel de **IsEnabled** waarde die moet worden **false** voor de rol die u wilt verwijderen.

    d. Selecteer **Query uitvoeren**.

    > [!NOTE]
    > Zorg ervoor dat u de msiam_access-rol hebt en de ID met de rol gegenereerd overeen komt.

7. Nadat de functie is uitgeschakeld, verwijdert u die rol blokkeren vanuit de **appRoles** sectie. Houd de methode **PATCH**, en selecteer **Query uitvoeren**.

8. Nadat u de query uitvoert, wordt de rol verwijderd.

    > [!NOTE]
    > De rol moet worden uitgeschakeld voordat het kan worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

Zie voor aanvullende stappen de [appdocumentatie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
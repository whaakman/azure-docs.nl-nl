---
title: Aanpassen van de gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het aanpassen van een gebruikersinterface met behulp van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 3c3b7d73de9a0e7e72351687af16b98b11f9581b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188488"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>De gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C aanpassen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nadat u dit artikel hebt voltooid, hebt u een aangepast beleid voor registreren en aanmelden met uw merk en de weergave. Met Azure Active Directory B2C (Azure AD B2C), krijgt u bijna volledig beheer van de HTML en CSS-inhoud die wordt weergegeven aan gebruikers. Wanneer u een aangepast beleid gebruiken, configureert u de UI-aanpassing in XML-bestand in plaats van het gebruik van besturingselementen in de Azure-portal. 

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). U hebt een aangepast beleid werkt voor registratie en aanmelding met lokale accounts.

## <a name="page-ui-customization"></a>De gebruikersinterface van de pagina aanpassen

U kunt het uiterlijk van een aangepast beleid aanpassen met behulp van de pagina-functie voor het aanpassen van de gebruikersinterface. U kunt er ook voor zorgen dat er visuele en merkconsistentie is tussen uw toepassing en Azure AD B2C.

Dit is hoe het werkt: Azure AD B2C wordt uitgevoerd de code in de browser van uw klant en maakt gebruik van een moderne manier met de naam [Cross-Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/). U kunt eerst een URL opgeven in het aangepaste beleid met aangepaste HTML-inhoud. Azure AD B2C combineert elementen van de gebruikersinterface met de HTML-inhoud die vanaf de URL wordt geladen en geeft vervolgens de pagina weer aan de klant.

## <a name="create-your-html5-content"></a>Uw HTML5 inhoud maken

HTML-inhoud met de naam van uw product maken in de titel.

1. Kopieer de volgende HTML-codefragment. Het is goed ingedeelde HTML5 met een leeg element met de naam *\<div-element-id = 'api'\>\</div\>* zich binnen de *\<hoofdtekst\>* tags. Dit element geeft aan waar Azure AD B2C-inhoud moet worden ingevoegd.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

2. Plak de gekopieerde codefragment in een teksteditor en sla het bestand op als *aanpassen ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob storage-account maken

>[!NOTE]
> In dit artikel gebruiken we Azure Blob-opslag om onze inhoud te hosten. U kunt kiezen voor het hosten van uw inhoud op een webserver, maar u moet [CORS inschakelen op uw webserver](https://enable-cors.org/server.html).

Voor het hosten van deze HTML-inhoud in Blob-opslag, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Op de **Hub** in het menu **nieuw** > **opslag** > **opslagaccount**.
3. Voer een unieke **naam** voor uw opslagaccount.
4. **Implementatiemodel** kan blijven **Resource Manager**.
5. Wijziging **soort Account** naar **Blob storage**.
6. **Prestaties** kan blijven **Standard**.
7. **Replicatie** kan blijven **RA-GRS**.
8. **Toegangslaag** kan blijven **warm**.
9. **Storage-serviceversleuteling** kan blijven **uitgeschakelde**.
10. Selecteer een **abonnement** voor uw opslagaccount.
11. Maak een **resourcegroep** of Selecteer een bestaande resourcegroep.
12. Selecteer de **geografische locatie** voor uw opslagaccount.
13. Klik op **Maken** om het opslagaccount te maken.  
    Nadat de implementatie is voltooid, de **opslagaccount** blade wordt automatisch geopend.

## <a name="create-a-container"></a>Een container maken

Voor het maken van een openbare container in Blob-opslag, het volgende doen:

1. Klik op de **overzicht** tabblad.
2. Klik op **Container**.
3. Voor **naam**, type **$root**.
4. Stel **toegangstype** naar **Blob**.
5. Klik op **$root** openen van de nieuwe container.
6. Klik op **Uploaden**.
7. Klik op het mappictogram naast **selecteert u een bestand**.
8. Ga naar **aanpassen ui.html**, die u eerder hebt gemaakt in de [Page UI-aanpassing](#the-page-ui-customization-feature) sectie.
9. Klik op **Uploaden**.
10. Selecteer de aanpassen ui.html-blob die u hebt geüpload.
11. Naast **URL**, klikt u op **kopie**.
12. Plak de gekopieerde URL in een browser en Ga naar de site. Als de site niet toegankelijk is, zorg ervoor dat het type container toegang is ingesteld op **blob**.

## <a name="configure-cors"></a>CORS configureren

Blob-opslag configureren voor het delen van Cross-Origin-Resource door het volgende te doen:

1. Selecteer in het menu **CORS**.
2. Voor **oorsprongen toegestaan**, voer `your-tenant-name.b2clogin.com`. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant. Bijvoorbeeld `fabrikam.b2clogin.com`. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw.
3. Voor **toegestaan methoden**, selecteert u beide `GET` en `OPTIONS`.
4. Voor **toegestaan Headers**, geeft u een sterretje (*).
5. Voor **blootgesteld Headers**, geeft u een sterretje (*).
6. Voor **maximumleeftijd**, Voer 200.
7. Klik op **Opslaan**.

## <a name="test-cors"></a>Test CORS

Valideren dat u klaar bent, door het volgende te doen:

1. Ga naar de [www.test-cors.org](https://www.test-cors.org/) website, en plak de URL in de **externe URL** vak.
2. Klik op **aanvraag verzenden**.  
    Als u een foutbericht ontvangt, controleert u of uw [CORS-instellingen](#configure-cors) juist zijn. Mogelijk moet u ook uw browsercache wissen of een InPrivate-browsersessie openen door op Ctrl + Shift + P te drukken.

## <a name="modify-the-extensions-file"></a>Het extensiebestand wijzigen

Als u wilt configureren voor UI-aanpassing, kopieert u de **ContentDefinition** en de onderliggende elementen uit het basis-bestand naar het extensiebestand.

1. Open het bestand basis van uw beleid. Bijvoorbeeld, *TrustFrameworkBase.xml*.
2. Zoek en kopieer de gehele inhoud van de **ContentDefinitions** element.
3. Open het extensiebestand. Bijvoorbeeld, *TrustFrameworkExtensions.xml*. Zoek de **BuildingBlocks** element. Als het element niet bestaat, deze toevoegen.
4. Plak de volledige inhoud van de **ContentDefinitions** element dat u hebt gekopieerd als onderliggende site van de **BuildingBlocks** element. 
5. Zoek de **ContentDefinition** element bevat `Id="api.signuporsignin"` in het XML-bestand dat u hebt gekopieerd.
6. Wijzig de waarde van **LoadUri** naar de URL van het HTML-bestand dat u hebt geüpload naar de opslag. Bijvoorbeeld `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.
    
    Het aangepaste beleid ziet er als volgt uit:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

7. Sla het extensiebestand.

## <a name="upload-your-updated-custom-policy"></a>Uploaden van uw bijgewerkte aangepast beleid

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **Identity-Ervaringsframework**.
2. Klik op **alle beleidsregels**.
3. Klik op **beleid uploaden**.
4. Upload het extensiebestand die u eerder hebt gewijzigd.

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van **nu uitvoeren**

1. Op de **Azure AD B2C** blade, Ga naar **alle beleidsregels**.
2. Selecteer het aangepaste beleid dat u geüpload en klikt u op de **nu uitvoeren** knop.
3. U zou het mogelijk om u te registreren met behulp van een e-mailadres.

## <a name="reference"></a>Referentie

U kunt voorbeeldsjablonen voor aanmeldbeleid hier vinden:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

De map sample_templates/wingtip bevat de volgende HTML-bestanden:

| HTML5-sjabloon | Beschrijving |
|----------------|-------------|
| *phonefactor.html* | Dit bestand gebruiken als een sjabloon voor een multi-factor authentication-pagina. |
| *resetpassword.html* | Dit bestand gebruiken als een sjabloon voor een pagina voor vergeten wachtwoorden. |
| *selfasserted.html* | Dit bestand gebruiken als een sjabloon voor een pagina voor het registreren van sociale account, een pagina voor het registreren van een lokaal account of een lokaal account aanmeldingspagina opgeven. |
| *unified.html* | Dit bestand gebruiken als een sjabloon voor een uniforme pagina voor registreren of aanmelden. |
| *updateprofile.html* | Dit bestand gebruiken als een sjabloon voor een update profielpagina. |

In de [, wijzigt u de beleidssectie van uw aangepaste voor registreren of aanmelden](#modify-your-sign-up-or-sign-in-custom-policy), u hebt geconfigureerd dat de definitie van de inhoud voor `api.idpselections`. De volledige set met inhoud roldefinitie-id's die worden herkend door de Azure AD B2C identiteitservaring-framework en de bijbehorende beschrijvingen zijn in de volgende tabel:

| De definitie van de inhoud-ID | Beschrijving | 
|-----------------------|-------------|
| *api.error* | **Foutpagina**. Deze pagina wordt weergegeven wanneer er een uitzondering of een fout is opgetreden. |
| *api.idpselections* | **Pagina voor het id-provider selecteren**. Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens het aanmelden kiezen kan. Deze opties zijn enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| *api.idpselections.signup* | **Selectie van de id-provider voor aanmelding bij**. Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens de registratie kiezen kan. Deze opties zijn enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| *api.localaccountpasswordreset* | **Pagina voor vergeten wachtwoorden**. Deze pagina bevat een formulier die de gebruiker moet worden voltooid voor het starten van een wachtwoord opnieuw instellen.  |
| *api.localaccountsignin* | **Lokaal account aanmelden pagina**. Deze pagina bevat een aanmeldingsformulier voor aanmelden met een lokaal account dat is gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Een tekstvak voor invoer en wachtwoord vermelding in bevatten het formulier. |
| *api.localaccountsignup* | **Pagina voor het registreren van lokale account**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld om zich te registreren voor een lokaal account dat is gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Het formulier kan verschillende invoer besturingselementen, zoals een tekstvak voor invoer, een wachtwoord invoeren in een keuzerondje, enkelvoudige selectie vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten. |
| *api.phonefactor* | **Multi-factor authentication-pagina**. Op deze pagina, kunnen gebruikers hun telefoonnummers (via SMS of spraak) controleren tijdens het registreren of aanmelden. |
| *api.selfasserted* | **Aanmeldpagina voor sociaal account**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld die gebruikers moeten worden voltooid wanneer ze zich aanmelden met behulp van een bestaand account van een sociale id-provider zoals Facebook of Google +. Deze pagina is vergelijkbaar met de voorgaande sociaal accountpagina voor het registreren, met uitzondering van de velden van de vermelding wachtwoord. |
| *api.selfasserted.profileupdate* | **Update-profielpagina**. Deze pagina bevat een formulier waarmee gebruikers kunnen hun profiel bijwerken. Deze pagina is vergelijkbaar met de sociaal account registratiepagina, met uitzondering van de velden van de vermelding wachtwoord. |
| *api.signuporsignin* | **Uniforme pagina voor registreren of aanmelden**. Deze pagina wordt gebruikt voor zowel de Meld u aan en aanmelden van gebruikers, die zakelijke id-providers, sociale id-providers zoals Facebook of Google + of lokale accounts kunnen gebruiken.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de UI-elementen die kunnen worden aangepast, [Snelzoekgids voor UI-aanpassing voor ingebouwde beleidsregels voor](active-directory-b2c-reference-ui-customization.md).

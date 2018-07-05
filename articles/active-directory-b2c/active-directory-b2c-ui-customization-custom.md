---
title: Een gebruikersinterface aanpassen met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het aanpassen van een gebruikersinterface (UI) tijdens het gebruik van aangepaste beleidsregels in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9908a7cf96c56e414e0a8d7faea0352b60214ea4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446160"
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: UI-aanpassing in een aangepast beleid configureren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nadat u dit artikel hebt voltooid, hebt u een aangepast beleid voor registreren en aanmelden met uw merk en de weergave. Met Azure Active Directory B2C (Azure AD B2C), krijgt u bijna volledig beheer van de HTML en CSS-inhoud die wordt weergegeven aan gebruikers. Wanneer u een aangepast beleid gebruiken, configureert u de UI-aanpassing in XML-bestand in plaats van het gebruik van besturingselementen in de Azure-portal. 

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). U hebt een aangepast beleid werkt voor registratie en aanmelding met lokale accounts.

## <a name="page-ui-customization"></a>De gebruikersinterface van de pagina aanpassen

U kunt het uiterlijk van een aangepast beleid aanpassen met behulp van de pagina-functie voor het aanpassen van de gebruikersinterface. U kunt ook onderhouden merk en visuele consistentie tussen uw toepassing en Azure AD B2C.

Hoe het werkt als volgt: Azure AD B2C wordt uitgevoerd de code in de browser van uw klant en maakt gebruik van een moderne manier met de naam [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/). U kunt eerst een URL opgeven in het aangepaste beleid met aangepaste HTML-inhoud. Azure AD B2C worden samengevoegd UI-elementen met de HTML-inhoud die wordt geladen vanaf uw URL en klikt u vervolgens de pagina wordt weergegeven aan de klant.

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

   >[!NOTE]
   >Uit veiligheidsoverwegingen is het gebruik van JavaScript momenteel geblokkeerd voor aanpassing.

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

>[!NOTE]
>Wilt u voor het uitproberen van de functie van de gebruikersinterface aanpassen met behulp van onze voorbeeld-HTML en CSS-inhoud? Zijn er beschikbaar gesteld [een eenvoudige hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) die wordt geüpload en onze voorbeelden voor inhoud op uw Blob storage-account configureert. Als u het hulpprogramma gebruikt, gaat u verder met [wijzigen van het aangepaste beleid registreren of aanmelden](#modify-your-sign-up-or-sign-in-custom-policy).

1. Op de **opslag** blade onder **instellingen**Open **CORS**.
2. Klik op **Add**.
3. Voor **oorsprongen toegestaan**, typt u een sterretje (\*).
4. In de **toegestane termen** vervolgkeuzelijst, selecteer **ophalen** en **opties**.
5. Voor **toegestaan headers**, typt u een sterretje (\*).
6. Voor **blootgesteld headers**, typt u een sterretje (\*).
7. Voor **maximale leeftijd (seconden)**, type **200**.
8. Klik op **Add**.

## <a name="test-cors"></a>CORS testen

Valideren dat u klaar bent, door het volgende te doen:

1. Ga naar de [www.test-cors.org](http://www.test-cors.org/) website, en plak de URL in de **externe URL** vak.
2. Klik op **aanvraag verzenden**.  
    Als u een foutbericht ontvangt, controleert u of uw [CORS-instellingen](#configure-cors) juist zijn. Mogelijk moet u ook uw browsercache wissen of een InPrivate-browsersessie openen door op Ctrl + Shift + P te drukken.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Wijzigen van het aangepaste beleid registreren of aanmelden

Onder de hoogste *\<TrustFrameworkPolicy\>* code, vindt u *\<BuildingBlocks\>* tag. Binnen de *\<BuildingBlocks\>* tags toevoegen een *\<ContentDefinitions\>* tag door te kopiëren van het volgende voorbeeld. Vervang *your_storage_account* met de naam van uw opslagaccount.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0</DataUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Uploaden van uw bijgewerkte aangepast beleid

1. In de [Azure-portal](https://portal.azure.com), [overschakelen naar de context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens de **Azure AD B2C** blade.
2. Klik op **alle beleidsregels**.
3. Klik op **beleid uploaden**.
4. Uploaden `SignUpOrSignin.xml` met de *\<ContentDefinitions\>* tag die u eerder hebt toegevoegd.

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
| *phonefactor.HTML* | Dit bestand gebruiken als een sjabloon voor een multi-factor authentication-pagina. |
| *resetpassword.html* | Dit bestand gebruiken als een sjabloon voor een pagina voor vergeten wachtwoorden. |
| *selfasserted.html* | Dit bestand gebruiken als een sjabloon voor een pagina voor het registreren van sociale account, een pagina voor het registreren van een lokaal account of een lokaal account aanmeldingspagina opgeven. |
| *Unified.HTML* | Dit bestand gebruiken als een sjabloon voor een uniforme pagina voor registreren of aanmelden. |
| *updateprofile.HTML* | Dit bestand gebruiken als een sjabloon voor een update profielpagina. |

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

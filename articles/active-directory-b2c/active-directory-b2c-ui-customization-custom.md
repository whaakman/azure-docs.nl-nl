---
title: Een gebruikersinterface aanpassen met behulp van aangepaste beleidsregels - Azure AD B2C | Microsoft Docs
description: Meer informatie over het aanpassen van een gebruikersinterface (UI) tijdens het gebruik van aangepast beleid in Azure AD B2C.
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: d5a3c0a323b31696d39e3d2b36317dec3a2337d7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: UI-aanpassing in een aangepast beleid configureren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nadat u dit artikel hebt voltooid, hebt u een aangepast beleid voor aanmelden en aanmelden met uw merk en identiteit. Met Azure Active Directory B2C (Azure AD B2C) dat u bijna volledig beheer van de HTML- en CSS-inhoud die wordt weergegeven aan gebruikers. Wanneer u een aangepast beleid gebruikt, configureert u de UI-aanpassing in XML in plaats van besturingselementen in de Azure portal. 

## <a name="prerequisites"></a>Vereisten

Voordat u begint, voltooien [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). U hebt een werkende aangepast beleid voor aanmelden en aanmelden met lokale accounts.

## <a name="page-ui-customization"></a>Page UI-aanpassing

U kunt het uiterlijk van het aangepaste beleid aanpassen met behulp van de pagina UI aanpassing-functie. U kunt ook behouden merk en visuele consistentie tussen uw toepassing en de Azure AD B2C.

Dit is hoe het werkt: Azure AD B2C wordt code wordt uitgevoerd in de browser van uw klant en maakt gebruik van een benadering van moderne aangeroepen [Cross-Origin-Resource delen (CORS)](http://www.w3.org/TR/cors/). Eerst, geeft u een URL in het aangepaste beleid met aangepaste HTML-inhoud. Azure AD B2C samenvoegingen UI-elementen met de HTML-inhoud die is geladen vanaf uw URL en wordt de pagina weergegeven in de klant.

## <a name="create-your-html5-content"></a>Uw HTML5 inhoud maken

HTML-inhoud met de naam van uw product maken in de titel.

1. Kopieer het volgende fragment in de HTML-code. Indeling geldig is HTML5 met een leeg element aangeroepen  *\<div-id = 'api'\>\</div\>*  zich binnen de  *\<hoofdtekst\>*  labels. Dit element geeft aan waar Azure AD B2C-inhoud moet worden ingevoegd.

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
   >Uit veiligheidsoverwegingen is het gebruik van JavaScript momenteel geblokkeerd om aan te passen.

2. Plak de gekopieerde codefragment in een teksteditor en sla het bestand als *aanpassen ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob storage-account maken

>[!NOTE]
> In dit artikel gebruiken we Azure Blob-opslag om onze inhoud te hosten. U kunt kiezen voor het hosten van uw inhoud op een webserver, maar u moet [CORS zijn ingeschakeld op uw webserver](https://enable-cors.org/server.html).

Voor het hosten van deze HTML-inhoud in de Blob-opslag, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Op de **Hub** selecteert u **nieuw** > **opslag** > **opslagaccount**.
3. Voer een unieke **naam** voor uw opslagaccount.
4. **Implementatiemodel** kan blijven **Resource Manager**.
5. Wijziging **soort Account** naar **Blob storage**.
6. **Prestaties** kan blijven **standaard**.
7. **Replicatie** kan blijven **RA-GRS**.
8. **Toegangslaag** kan blijven **Hot**.
9. **Versleuteling van de opslagruimte** kan blijven **uitgeschakelde**.
10. Selecteer een **abonnement** voor uw opslagaccount.
11. Maak een **resourcegroep** of een bestaande set selecteren.
12. Selecteer de **geografische locatie** voor uw opslagaccount.
13. Klik op **Maken** om het opslagaccount te maken.  
    Nadat de implementatie is voltooid, de **opslagaccount** blade wordt automatisch geopend.

## <a name="create-a-container"></a>Een container maken

U maakt een openbare container in Blob-opslag door het volgende doen:

1. Klik op de **overzicht** tabblad.
2. Klik op **Container**.
3. Voor **naam**, type **$root**.
4. Stel **toegangstype** naar **Blob**.
5. Klik op **$root** openen van de nieuwe container.
6. Klik op **Uploaden**.
7. Klik op het pictogram naast **selecteert u een bestand**.
8. Ga naar **aanpassen ui.html**, die u eerder hebt gemaakt in de [Page UI-aanpassing](#the-page-ui-customization-feature) sectie.
9. Klik op **Uploaden**.
10. Selecteer de aanpassen ui.html blob die u hebt geüpload.
11. Naast **URL**, klikt u op **kopie**.
12. In een browser, plak de gekopieerde URL en Ga naar de site. Als de site niet toegankelijk is, zorg ervoor dat het type van de container toegang is ingesteld op **blob**.

## <a name="configure-cors"></a>CORS configureren

Blob-opslag configureren voor het delen van Cross-Origin-Resource als volgt:

>[!NOTE]
>Wilt u de UI-functie voor aanpassing door het gebruik van onze voorbeeld HTML en CSS-inhoud uitproberen? We bieden [een eenvoudige helper hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) die uploadt en configureert u onze voorbeelden op uw Blob storage-account. Als u het hulpprogramma gebruikt, gaat u verder met [wijzigen van het aangepaste beleid registreren of aanmelden](#modify-your-sign-up-or-sign-in-custom-policy).

1. Op de **opslag** blade onder **instellingen**Open **CORS**.
2. Klik op **Add**.
3. Voor **toegestane oorsprongen**, typt u een sterretje (\*).
4. In de **toegestane termen** vervolgkeuzelijst, selecteer **ophalen** en **opties**.
5. Voor **toegestaan headers**, typt u een sterretje (\*).
6. Voor **blootgesteld headers**, typt u een sterretje (\*).
7. Voor **maximale leeftijd (seconden)**, type **200**.
8. Klik op **Add**.

## <a name="test-cors"></a>Test CORS

Valideren dat u klaar bent als volgt:

1. Ga naar de [test cors.org](http://test-cors.org/) website, en plak de URL in de **externe URL** vak.
2. Klik op **aanvraag verzenden**.  
    Als u een foutbericht ontvangt, controleert u of uw [CORS-instellingen voor](#configure-cors) juist zijn. Mogelijk moet u ook uw browsercache wissen of een browsersessie in particuliere openen door op Ctrl + Shift + P te drukken.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Wijzigen van het aangepaste beleid registreren of aanmelden

Onder de hoogste  *\<TrustFrameworkPolicy\>*  labels, zult u  *\<BuildingBlocks\>*  label. Binnen de  *\<BuildingBlocks\>*  tags, Voeg een  *\<ContentDefinitions\>*  code door te kopiëren van het volgende voorbeeld. Vervang *your_storage_account* met de naam van uw opslagaccount.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Upload uw bijgewerkte aangepast beleid

1. In de [Azure-portal](https://portal.azure.com), [switch in de context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens de **Azure AD B2C** blade.
2. Klik op **alle beleidsregels**.
3. Klik op **uploaden beleid**.
4. Uploaden `SignUpOrSignin.xml` met de  *\<ContentDefinitions\>*  code die u eerder hebt toegevoegd.

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van **nu uitvoeren**

1. Op de **Azure AD B2C** blade, gaat u naar **alle beleidsregels**.
2. Selecteer het aangepaste beleid die u hebt geüpload en klik op de **nu uitvoeren** knop.
3. U moet kunnen aanmelden met een e-mailadres.

## <a name="reference"></a>Naslaginformatie

Voor de UI-aanpassing Hier vindt u voorbeeldsjablonen:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

De map sample_templates/wingtip bevat de volgende HTML-bestanden:

| HTML5-sjabloon | Beschrijving |
|----------------|-------------|
| *phonefactor.HTML* | Dit bestand als sjabloon gebruiken voor een multi-factor authentication-pagina. |
| *ResetPassword.HTML* | Dit bestand te gebruiken als een sjabloon voor een wachtwoordpagina vergeten. |
| *selfasserted.HTML* | Dit bestand als een sjabloon voor een aanmeldingspagina sociale account, de aanmeldingspagina van een lokaal account of een aanmeldingspagina lokale account gebruiken. |
| *Unified.HTML* | Dit bestand als sjabloon gebruiken voor een uniforme registreren of aanmelden pagina. |
| *updateprofile.HTML* | Dit bestand als een sjabloon voor een update-profielpagina gebruiken. |

In de [, wijzigt u de sectie registreren of aanmelden aangepast beleid](#modify-your-sign-up-or-sign-in-custom-policy), geconfigureerd van de inhoud definitie voor `api.idpselections`. De volledige set van inhoud definitie-id's die worden herkend door het Azure AD B2C identiteit ervaring framework en de bijbehorende beschrijvingen zijn in de volgende tabel:

| De definitie van de inhoud-ID | Beschrijving | 
|-----------------------|-------------|
| *API.Error* | **Foutpagina**. Deze pagina wordt weergegeven wanneer een uitzondering of een fout is opgetreden. |
| *API.idpselections* | **Id-provider selectiepagina**. Deze pagina bevat een lijst met de id-providers die de gebruiker uit tijdens het aanmelden kiezen kan. Deze opties zijn enterprise identiteitsproviders, sociale id-providers zoals Facebook en Google + of lokale accounts. |
| *API.idpselections.Signup* | **Selectie van de id-provider voor registratie**. Deze pagina bevat een lijst met de id-providers die de gebruiker uit tijdens de registratie kiezen kan. Deze opties zijn enterprise identiteitsproviders, sociale id-providers zoals Facebook en Google + of lokale accounts. |
| *API.localaccountpasswordreset* | **Wachtwoordpagina vergeten**. Deze pagina bevat een formulier dat de gebruiker moeten worden voltooid voor het initiëren van een wachtwoord opnieuw instellen.  |
| *API.localaccountsignin* | **Aanmeldingspagina voor lokaal account**. Deze pagina bevat een formulier-in voor het aanmelden met een lokaal account dat is gebaseerd op een e-mailadres of een gebruikersnaam. Het formulier kan een tekstinvoervak en wachtwoordvak vermelding bevatten. |
| *API.localaccountsignup* | **Lokaal account aanmeldingspagina**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld voor het aanmelden voor een lokaal account dat is gebaseerd op een e-mailadres of een gebruikersnaam. Het formulier kan verschillende invoer besturingselementen bevatten, zoals een tekstinvoervak vak een wachtwoord, een keuzerondje, één vervolgkeuzelijsten en meervoudige selectie selectievakjes. |
| *API.phonefactor* | **Multi-factor authentication-pagina**. Op deze pagina, kunnen gebruikers hun telefoonnummers (met behulp van tekst of stem) controleren tijdens het registreren of aanmelden. |
| *API.selfasserted* | **De aanmeldpagina sociale account**. Deze pagina bevat een aanmeldingsformulier hebt ingevuld die gebruikers moeten worden voltooid wanneer ze zich registreren met behulp van een bestaand account van een identiteitsprovider van sociale zoals Facebook of Google +. Deze pagina is vergelijkbaar met de voorgaande sociale account aanmeldpagina, met uitzondering van de invoervelden wachtwoord. |
| *API.selfasserted.profileupdate* | **Update profielpagina**. Deze pagina bevat een formulier dat gebruikers gebruiken kunnen om hun profiel bijwerken. Deze pagina is vergelijkbaar met de sociale account aanmeldpagina, met uitzondering van de invoervelden wachtwoord. |
| *API.signuporsignin* | **Unified registreren of aanmelden pagina**. Deze pagina verwerkt de registreren en aanmelden van gebruikers, die enterprise identiteitsproviders, sociale id-providers zoals Facebook of Google + of lokale accounts kunnen gebruiken.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de UI-elementen die kunnen worden aangepast, [Naslaggids voor aanpassing van de gebruikersinterface voor ingebouwde beleid](active-directory-b2c-reference-ui-customization.md).

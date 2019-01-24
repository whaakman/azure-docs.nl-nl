---
title: Over aanpassingen van de gebruikersinterface in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het aanpassen van de gebruikersinterface voor uw toepassingen die gebruikmaken van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 44d1e5996b1f9b8ae65df9ad16c4bb543fdab1a0
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54844759"
---
# <a name="about-user-interface-customization-in-azure-active-directory-b2c"></a>Over aanpassingen van de gebruikersinterface in Azure Active Directory B2C

De mogelijkheid om te voorzien en de gebruikersinterface (UI) die Azure Active Directory (Azure AD) B2C voor uw toepassingen fungeert aanpassen is belangrijk voor een naadloze ervaring bieden aan uw klant. Deze ervaringen zijn gebruikersregistratie, aanmelding, profiel bewerken, en het wachtwoord opnieuw instellen. In dit artikel biedt informatie waarmee u de gebruikersinterface van uw toepassingen aanpassen.

Afhankelijk van uw behoeften als het gaat om deze ervaringen, aanpassen u de gebruikersinterface van uw toepassing op verschillende manieren. Bijvoorbeeld:

- Als u [gebruikersstromen](active-directory-b2c-reference-policies.md) om u te registreren of aanmelden, wachtwoord opnieuw instellen of bewerken van profielen ervaringen in uw toepassing opgeven, gebruikt u de [Azure portal om aan te passen van de gebruikersinterface](tutorial-customize-ui.md).
- Als u alleen aanmelding bieden en verificatie e-mails, u dezelfde stappen aanpassen die worden gebruikt voor het bijbehorende wachtwoord opnieuw instellen van pagina een [Azure AD-aanmeldingspagina](../active-directory/fundamentals/customize-branding.md).
- Als klanten hun profiel te bewerken voordat ze zich aanmelden, worden ze omgeleid naar een pagina die u met behulp van de stappen die worden gebruikt voor het aanpassen van de Azure AD-aanmeldingspagina aanpassen.
- Als u [aangepast beleid](active-directory-b2c-overview-custom.md) opnieuw registreren of aanmelden,-wachtwoord op te geven, of profiel bewerken in uw toepassing, u [beleidsbestanden om aan te passen van de gebruikersinterface](active-directory-b2c-ui-customization-custom.md).
- Als u nodig hebt voor dynamische inhoud op basis van de beslissing van de klant, u [aangepaste beleidsregels die u kunnen wijzigen pagina-inhoud](active-directory-b2c-ui-customization-custom-dynamic.md) , afhankelijk van een parameter die wordt verzonden in een queryreeks. Bijvoorbeeld, wordt de afbeelding op de Azure AD B2C-pagina voor registreren of aanmelden gewijzigd, op basis van een parameter die u vanuit uw web- of mobiele toepassing doorgeven.

Azure AD B2C wordt uitgevoerd de code in de browser van uw klant en maakt gebruik van een moderne manier met de naam [Cross-Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/). Inhoud is geladen op het moment van uitvoering, vanuit een URL die u in een beleid of Groepsbeleid opgeeft. U opgeven verschillende URL's voor verschillende pagina's. Na het laden van inhoud van de URL van uw, moeten ze worden samengevoegd met een HTML-fragment ingevoegd vanuit Azure AD B2C en vervolgens naar de klant worden weergegeven.

Voordat u begint, controleert u de volgende richtlijnen:

- Azure AD B2C samengevoegd HTML-inhoud met uw pagina's. Niet kopiëren en probeert te wijzigen van de Standaardinhoud die Azure AD B2C biedt. Het is raadzaam uw HTML-inhoud helemaal bouwen en gebruiken van de Standaardinhoud als verwijzing.
- Uit veiligheidsoverwegingen mag u geen JavaScript opnemen in uw inhoud.
- Ondersteunde browserversies zijn: 
    - Internet Explorer 11, 10 en Microsoft Edge
    - Beperkte ondersteuning voor Internet Explorer 9 en 8
    - Google Chrome 42.0 en hoger
    - Mozilla Firefox 38.0 en hoger
- Zorg ervoor dat u vormlabels niet in uw HTML-code opnemen omdat deze een conflict met de POST-bewerkingen die worden gegenereerd door de geïnjecteerde HTML uit Azure AD B2C veroorzaakt.

## <a name="where-do-i-store-ui-content"></a>Waar kan ik de gebruikersinterface inhoud opslaan?

U kunt uw gebruikersinterface inhoud overal, zoals op hosten [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md), webservers, CDN, AWS S3, of delen-bestandssystemen. Het belangrijkste is dat u host de inhoud van een openbaar beschikbare HTTPS-eindpunt met CORS ingeschakeld. Wanneer u deze in uw inhoud opgeeft, moet u een absolute URL gebruiken.

## <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?

U als volgt voor het aanpassen van de gebruikersinterface:

- Opgemaakte HTML-inhoud met een lege maken `<div id="api"></div>` element zich ergens in de `<body>`. Dit element merken waar de inhoud van de Azure AD B2C wordt ingevoegd. Het volgende voorbeeld ziet u een minimale pagina:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- Host uw inhoud op een HTTPS-eindpunt (met CORS toegestaan). Beide ophalen en opties aanvraagmethoden moeten zijn ingeschakeld bij het configureren van CORS.
- CSS gebruiken voor de stijl van de UI-elementen die Azure AD B2C wordt ingevoegd in de pagina. Het volgende voorbeeld ziet u een eenvoudige CSS-bestand bevat ook de instellingen voor de aanmelding geïnjecteerde HTML-elementen:

    ```css 
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Maken of bewerken van een beleid voor het gebruik van de inhoud die u hebt gemaakt.

De volgende tabel bevat de HTML-fragmenten die Azure AD B2C worden samengevoegd in de `<div id="api"></div>` element zich in uw inhoud.

| Ingevoegde pagina | Beschrijving van HTML |
| ------------- | ------------------- |
| Id-provider selecteren | Bevat een lijst van de knoppen voor id-providers die de klant uit tijdens het registreren of aanmelden kiezen kunt. Deze knoppen bevatten sociale id-providers, zoals Facebook, Google of lokale accounts (op basis van e-mailadres of gebruikersnaam naam). |
| Lokaal account aanmelden | Bevat een formulier voor het lokale account Meld u aan op basis van een e-mailadres of de naam van een gebruiker. Het formulier kan verschillende besturingselementen voor tekstinvoer zoals tekstinvoervak, wachtwoordinvoervak, keuzerondje, enkelvoudige selectie vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten. |
| Meld u aan sociaalnetwerkaccount | Kan worden weergegeven bij het aanmelden met een bestaand account van een sociale id-provider zoals Facebook of Google. Deze wordt gebruikt wanneer aanvullende informatie moet worden verzameld van de klant die gebruikmaken van een aanmeldingsformulier hebt ingevuld. |
| Unified registreren of aanmelden | Zowel registreren en aanmelden van gebruikers met sociale id-providers, zoals Facebook, Google of lokale accounts kunnen worden verwerkt. |
| Multi-Factor Authentication | Klanten kunnen hun telefoonnummers (via SMS of spraak) tijdens het registreren of aanmelden controleren. |
| Fout | Biedt informatie over de fout aan de klant. |


## <a name="how-do-i-localize-content"></a>Hoe ik inhoud lokaliseren?

Lokalisatie van uw HTML-inhoud door in te schakelen [aanpassing van taal](active-directory-b2c-reference-language-customization.md) in uw Azure AD B2C-tenant. Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales` aan uw eindpunt. Uw server voor webinhoud kunt u deze parameter gebruiken voor taalspecifieke HTML-pagina's.

Inhoud kan worden opgehaald uit verschillende plaatsen op basis van de landinstelling die wordt gebruikt. In uw eindpunt CORS is ingeschakeld, moet u een mapstructuur tot inhoud voor specifieke talen host instellen. U moet de juiste is aanroepen als u het jokerteken {cultuur: RFC5646}. Bijvoorbeeld, uw aangepaste pagina URI als volgt uitzien `https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html`. U kunt de pagina in het Frans laden door het binnenhalen van inhoud van `https://contoso.blob.core.windows.net/fr/myHTML/unified.html`

## <a name="examples"></a>Voorbeelden

Voor voorbeelden van aanpassingen, downloaden en bekijk deze [sjabloon voorbeeldbestanden](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).

## <a name="next-steps"></a>Volgende stappen

- Als u gebruikersstromen gebruikt, kunt u beginnen met het aanpassen van uw gebruikersinterface met de zelfstudie voor: [De gebruikersinterface van uw toepassingen in Azure Active Directory B2C aanpassen](tutorial-customize-ui.md).
- Als u aangepaste beleidsregels, kunt u beginnen met het aanpassen van de gebruikersinterface met het artikel: [De gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C aanpassen](active-directory-b2c-ui-customization-custom.md).


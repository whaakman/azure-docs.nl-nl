---
title: Het invullen van specifieke velden voor een aangepaste toepassing | Microsoft Docs
description: Instructies over het invullen van specifieke velden wanneer u een aangepaste ontwikkelde toepassing met Azure AD registreert
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bccaa28d34ebff47c7de73a4d9b3d8296ae9fef
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476127"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Het invullen van specifieke velden voor een aangepaste toepassing

Dit artikel vindt u een korte beschrijving van alle beschikbare velden in de vorm van de toepassing registreren in de [Azure-portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Een nieuwe toepassing registreren

-   Als u wilt een nieuwe toepassing registreren, gaat u naar de [Azure-portal](https://portal.azure.com).

-   Klik in het linkernavigatiedeelvenster op **Azure Active Directory.**

-   Kies **App-registraties** en klikt u op **toevoegen**.

-   Deze openen om het formulier van de registratie van toepassing.

## <a name="fields-in-the-application-registration-form"></a>Velden in het registratieformulier toepassing


| Veld            | Description                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Name             | De naam van de toepassing. Er moet ten minste vier tekens.                |
| Ondersteunde accounttypen| Selecteer welke accounts u wilt uw toepassing om te ondersteunen: accounts in deze organisatie-map alleen, accounts in een organisatie-map of accounts in een organisatie-map en persoonlijke Microsoft-accounts.  |
| Omleidings-URI (optioneel) | Selecteer het type app dat u maakt, **Web** of **openbare client (mobiele en desktop)** , en voer vervolgens de omleidings-URI (of de antwoord-URL) voor uw toepassing. Geef voor webtoepassingen de basis-URL van de app op. http://localhost:31544 kan bijvoorbeeld de URL zijn van een web-app die op uw lokale machine wordt uitgevoerd. Gebruikers moeten deze URL gebruiken om zich bij een webclienttoepassing aan te melden. Geef voor openbare clienttoepassingen de URI op die in Azure Active Directory wordt gebruikt om tokenantwoorden te retourneren. Voer een specifieke waarde aan uw toepassing, zoals myapp://auth. Raadpleeg onze [snelstarts](https://docs.microsoft.com/azure/active-directory/develop/#quickstarts) om specifieke voorbeelden te zien van webtoepassingen of systeemeigen toepassingen.|

Nadat u de bovenstaande velden hebt ingevuld, de toepassing is geregistreerd in Azure portal en wordt u omgeleid naar de overzichtspagina van de toepassing. De instellingen voor pagina's in het linkerdeelvenster onder **beheren** meer velden bevatten voor u het aanpassen van uw toepassing. De onderstaande tabellen beschrijven alle velden. U ziet alleen een subset van deze velden, afhankelijk van of u een webtoepassing of een openbare client-toepassing gemaakt.

### <a name="overview"></a>Overzicht
| Veld           | Description        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Toepassings-id  | Als u een toepassing registreert, wijst Azure AD uw toepassing een toepassings-ID. De aanvraag-ID kan worden gebruikt voor het aanduiden van uw toepassing in verificatieaanvragen naar Azure AD, evenals voor toegang tot resources, zoals de Graph API.                                                          |
| App-ID-URI      | Dit moet een unieke URI, meestal van het formulier **https://&lt;tenant\_naam&gt;/&lt;toepassing\_naam&gt;.** Dit wordt gebruikt tijdens de stroom voor het verlenen van machtiging als een unieke id om op te geven van de resource die het token moet worden uitgegeven voor. Ook wordt de claim 'aud' in het toegangstoken verleende. |

### <a name="branding"></a>Huisstijl

| Veld           | Description        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nieuw logo uploaden | U kunt dit gebruiken voor het uploaden van een logo voor uw toepassing. Het logo moet bmp-, JPG of PNG-indeling en de bestandsgrootte moet minder dan 100 KB. Voor de afmetingen van de installatiekopie moet 215 x 215 pixels, met een centrale afmetingen van 94 x 94 pixels.|
| URL van startpagina   | Dit is de aanmeldings-URL die is opgegeven tijdens de toepassingsregistratie.|

### <a name="authentication"></a>Verificatie

| Veld           | Description        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afmeldings-URL      | Dit is de URL voor eenmalige afmelding afmelding. Azure AD verzendt een afmeldingsaanvraag naar deze URL als de gebruiker hun sessie wist met Azure AD met behulp van andere geregistreerde toepassing.|
| Ondersteunde accounttypen  | Deze schakeloptie geeft aan of de toepassing kan worden gebruikt door meerdere tenants. Dit betekent doorgaans dat externe organisaties uw toepassing door deze in hun tenant te registreren en toegang verlenen tot gegevens van hun organisatie kunnen gebruiken.|
| Omleidings-URL 's      | De omleidings- of antwoord, URL's zijn de eindpunten waarop Azure AD tokens retourneert die die uw toepassing worden aangevraagd. Dit is waar de gebruiker wordt verzonden na een geslaagde autorisatie voor systeemeigen toepassingen. Azure AD wordt gecontroleerd of de omleidings-URI die uw toepassing wordt verstrekt in de OAuth 2.0-aanvraag overeenkomt met een van de geregistreerde waarden in de portal.|

### <a name="certificates-and-secrets"></a>Certificaten en geheimen

| Veld           | Description        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Client-geheimen            | U kunt clientinstellingen geheimen en sleutels, om programmatisch toegang tot web-API's die zijn beveiligd door Azure AD zonder tussenkomst van de gebruiker te maken. Uit de **nieuwe clientgeheim** pagina, voer een beschrijving van de sleutel en de vervaldatum en opslaan als de sleutel wilt genereren. Zorg ervoor dat u Sla deze ergens veilig, omdat het niet mogelijk toegang tot het later opnieuw.             |

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](../manage-apps/what-is-application-management.md)

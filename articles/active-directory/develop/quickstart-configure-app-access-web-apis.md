---
title: Een toepassing configureren voor toegang tot web-API's (preview) | Azure
description: Informatie over het configureren van een toepassing die is geregistreerd bij het Microsoft Identity Platform zodat deze omleidings-URI('s), referenties of machtigingen bevat voor toegang tot web-API's.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08ad61070fd3bfb8472cffb51fd0b1bb4bcf169b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169374"
---
# <a name="quickstart-configure-a-client-application-to-access-web-apis-preview"></a>Quickstart: Een clienttoepassing configureren voor toegang tot web-API's (preview)

Voordat een web-/vertrouwelijke clienttoepassing kan deelnemen aan een werkstroom voor autorisatieverlening die verificatie (en het verkrijgen van een toegangstoken) vereist, moeten beveiligde referenties worden vastgesteld. De standaardmethode voor verificatie die wordt ondersteund door Azure Portal is de client-ID + geheime sleutel.

Voordat een client toegang krijgt tot een web-API die beschikbaar is gemaakt door een resourcetoepassing (zoals de Microsoft Graph API), zorgt het toestemmingsframework er ook voor dat de client de vereiste machtiging verkrijgt, op basis van de machtigingen die zijn aangevraagd. Standaard kunnen alle toepassingen machtigingen kiezen uit de Microsoft Graph API. De [Graph API-machtiging 'Aanmelden en gebruikersprofiel lezen'](https://developer.microsoft.com/graph/docs/concepts/permissions_reference#user-permissions) is standaard geselecteerd. U kunt kiezen uit twee [typen machtigingen](developer-glossary.md#permissions) voor elke gewenste web-API:

* **Toepassingsmachtigingen**: de clienttoepassing heeft rechtstreeks als zichzelf (geen gebruikerscontext) toegang nodig tot de web-API. Dit type machtiging vereist toegang van de beheerder en is ook niet beschikbaar voor openbare clienttoepassingen (desktop en mobiel).
* **Gedelegeerde machtigingen**: de clienttoepassing heeft toegang tot de web-API als de aangemelde gebruiker nodig, maar met toegang die wordt beperkt door de geselecteerde machtiging. Dit type machtiging kan worden verleend door een gebruiker tenzij de machtiging toestemming van de beheerder vereist.

  > [!NOTE]
  > Wanneer een gedelegeerde machtiging aan een toepassing wordt toegevoegd, wordt niet automatisch toestemming verleend aan de gebruikers binnen de tenant. Gebruikers moeten nog steeds handmatig toestemming geven voor de toegevoegde gedelegeerde machtigingen tijdens runtime, tenzij de beheerder toestemming namens alle gebruikers verleent.

In deze snelstart leert u hoe u uw toepassing kunt configureren voor het volgende:

* [Omleidings-URI's toevoegen aan uw toepassing](#add-redirect-uris-to-your-application)
* [Referenties toevoegen aan uw webtoepassing](#add-credentials-to-your-web-application)
* [Machtigingen toevoegen voor toegang tot web-API's](#add-permissions-to-access-web-apis)

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u, voordat u aan de slag gaat, aan deze vereisten voldoet:

* Lees de informatie over de ondersteunde [machtigingen en toestemming](v2-permissions-and-consent.md). Een goed begrip hiervan is belangrijk bij het bouwen van toepassingen die moeten worden gebruikt door andere gebruikers of met andere toepassingen.
* U moet een tenant hebben waarvoor toepassingen zijn geregistreerd.
  * Als u uw apps niet hebt geregistreerd, [kunt u hier lezen hoe u toepassingen registreert bij het Microsoft Identity Platform](quickstart-register-app.md).
* U hebben aangemeld voor de preview-versie voor app-registraties in de Azure-portal. De stappen in deze snelstart zijn compatibel met de nieuwe gebruikersinterface en werken alleen als u zich hebt aangemeld voor de preview-versie.

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Aanmelden bij de Azure-portal en de app selecteren

Voordat u de app kunt configureren, volgt u deze stappen:

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
1. Selecteer in het linkernavigatiedeelvenster de **Azure Active Directory**-service en selecteer vervolgens **App-registraties (preview)**.
1. Zoek en selecteer de toepassing die u wilt configureren. Wanneer u de app hebt geselecteerd, ziet u het **Overzicht** of de hoofdregistratiepagina van de toepassing.
1. Volg de stappen om uw toepassing te configureren voor toegang tot web-API's: 
    * [Omleidings-URI's toevoegen aan uw toepassing](#add-redirect-uris-to-your-application)
    * [Referenties toevoegen aan uw webtoepassing](#add-credentials-to-your-web-application)
    * [Machtigingen toevoegen voor toegang tot web-API's](#add-permissions-to-access-web-apis)

## <a name="add-redirect-uris-to-your-application"></a>Omleidings-URI('s) toevoegen aan uw toepassing

[![Aangepaste omleidings-URI's toevoegen voor web- en openbare client-apps](./media/quickstart-update-azure-ad-app-preview/authentication-redirect-uris-expanded.png)](./media/quickstart-update-azure-ad-app-preview/authentication-redirect-uris-expanded.png#lightbox)

Ga als volgt te werk om een omleidings-URI toe te voegen aan uw toepassing:

1. Selecteer op de pagina **Overzicht** van de app de sectie **Verificatie**.

1. Als u een aangepaste omleidings-URI's voor web- en openbare client-apps wilt toevoegen, voert u deze stappen uit:

    1. Ga naar de sectie **Omleidings-URI**.
    1. Selecteer het type toepassing dat u bouwt, **Web** of **Openbare client (mobiel en desktop)**.
    1. Voer de omleidings-URI voor uw toepassing in.
        * Geef voor webtoepassingen de basis-URL van uw toepassing op. http://localhost:31544 kan bijvoorbeeld de URL zijn van een webtoepassing die op uw lokale machine wordt uitgevoerd. Gebruikers moeten deze URL gebruiken om zich bij een webclienttoepassing aan te melden.
        * Geef voor openbare toepassingen de URI op die door Azure Active Directory wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld https://MyFirstApp.

1. Volg deze stappen om een keuze te maken uit voorgestelde omleidings-URI's voor openbare clients (mobiel, desktop):

    1. Ga naar de sectie **Voorgestelde omleidings-URI's voor openbare clients (mobiel, desktop)**.
    1. Selecteer de juiste omleidings-URI's voor uw toepassing met behulp van de selectievakjes.

## <a name="add-credentials-to-your-web-application"></a>Referenties toevoegen aan uw webtoepassing

[![Certificaten en clientgeheimen toevoegen](./media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)](./media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png#lightbox)

Ga als volgt te werk om een referentie aan uw webtoepassing toe te voegen:

1. Selecteer op de pagina **Overzicht** van de app de sectie **Certificaten en geheimen**.

1. Volg deze stappen om een certificaat toe te voegen:

    1. Selecteer **Certificaat uploaden**.
    1. Selecteer het bestand dat u wilt uploaden. Dit moet een van de volgende bestandstypen zijn: .cer, .pem, .crt.
    1. Selecteer **Toevoegen**.

1. Als u een clientgeheim wilt toevoegen, voert u deze stappen uit:

    1. Selecteer **Nieuw clientgeheim**.
    1. Voeg een beschrijving voor uw clientgeheim toe.
    1. Selecteer een duur.
    1. Selecteer **Toevoegen**.

> [!NOTE]
> Nadat u de configuratiewijzigingen hebt opgeslagen, bevat de meest rechtse kolom de waarde van het clientgeheim. **Vergeet niet de waarde te kopiëren** voor gebruik in de code van uw clienttoepassing. Deze is namelijk niet meer toegankelijk wanneer u deze pagina verlaat.

## <a name="add-permissions-to-access-web-apis"></a>Machtigingen toevoegen voor toegang tot web-API's

[![API-machtigingen toevoegen](./media/quickstart-update-azure-ad-app-preview/api-permissions-expanded.png)](./media/quickstart-update-azure-ad-app-preview/api-permissions-expanded.png#lightbox)

Ga als volgt te werk om machtiging(en) voor toegang tot resource-API's van uw client toe te voegen:

1. Selecteer op de pagina **Overzicht** van de app de optie **API-machtigingen**.
1. Selecteer de knop **Een machtiging toevoegen**.
1. Standaard kunt u in de weergave een keuze maken uit **Microsoft-API's**. Selecteer de sectie met API's waarin u geïnteresseerd bent:
    * **Microsoft-API's**: hier kunt u machtigingen selecteren voor Microsoft-API's, zoals Microsoft Graph.
    * **API's die in mijn organisatie worden gebruikt**: hier kunt u machtigingen selecteren voor de API's die beschikbaar zijn gemaakt door uw organisatie of API's waarmee uw organisatie is geïntegreerd.
    * **Mijn API's**: hier kunt u machtigingen selecteren voor API's die u beschikbaar hebt gemaakt.
1. Wanneer u de API's hebt geselecteerd, ziet u de pagina **API-machtigingen aanvragen**. Als de API zowel gedelegeerde als toepassingsmachtigingen bevat, selecteert u het type machtiging dat uw toepassing nodig heeft.
1. Selecteer **Machtigingen toevoegen** als u klaar bent. U keert terug naar de pagina **API-machtigingen**, waar de machtigingen zijn opgeslagen en toegevoegd aan de tabel.

## <a name="next-steps"></a>Volgende stappen

Lees meer in deze andere gerelateerde snelstarts voor app-beheer:

* [Een toepassing registreren met het Microsoft Identity Platform](quickstart-register-app.md)
* [Een toepassing configureren voor het beschikbaar maken van web-API's](quickstart-configure-app-expose-web-apis.md)
* [De accounts wijzigen die worden ondersteund door een toepassing](quickstart-modify-supported-accounts.md)
* [Een geregistreerde toepassing verwijderen uit het Microsoft Identity Platform](quickstart-remove-app.md)

Zie [Toepassingsobjecten en service-principal-objecten](app-objects-and-service-principals.md) voor meer informatie over de twee Azure Active Directory-objecten die een geregistreerde toepassing vertegenwoordigen en de relatie ertussen.

Zie [Huisstijlrichtlijnen voor apps](howto-add-branding-in-azure-ad-apps.md) voor meer informatie over de huisstijlrichtlijnen die u moet gebruiken bij het ontwikkelen van toepassingen met Azure Active Directory.

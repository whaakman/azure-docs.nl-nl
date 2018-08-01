---
title: Het invullen van specifieke velden voor een aangepaste toepassing | Microsoft Docs
description: Instructies over het invullen van specifieke velden wanneer u een aangepaste ontwikkelde toepassing met Azure AD registreert
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: ecdd76a8a6974a819cd0bedffd095299435dcc88
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365310"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Het invullen van specifieke velden voor een aangepaste toepassing

Dit artikel vindt u een korte beschrijving van alle beschikbare velden in de vorm van de toepassing registreren in de [Azure-portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Een nieuwe toepassing registreren

-   Als u wilt een nieuwe toepassing registreren, gaat u naar de [Azure-portal](https://portal.azure.com).

-   Klik in het linkernavigatiedeelvenster op **Azure Active Directory.**

-   Kies **App-registraties** en klikt u op **toevoegen**.

-   Deze openen om het formulier van de registratie van toepassing.

## <a name="fields-in-the-application-registration-form"></a>Velden in het registratieformulier toepassing


| Veld            | Beschrijving                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Naam             | De naam van de toepassing. Er moet ten minste vier tekens.                |
| Toepassingstype | **Web-app/Web API**: een toepassing die een webtoepassing, een web-API of beide 
| |**Systeemeigen**: een toepassing die kan worden geïnstalleerd op het apparaat of de computer van een gebruiker           |
| Aanmeldings-URL      | De URL waar gebruikers zich aanmelden kunnen om uw toepassing te gebruiken                                  |

Nadat u de bovenstaande velden hebt ingevuld, de toepassing is geregistreerd in Azure portal en wordt u omgeleid naar de toepassingspagina. De **instellingen** knop in het deelvenster application wordt de pagina instellingen, die meer velden voor u het aanpassen van uw toepassing heeft geopend. De volgende tabel beschrijft de velden op de pagina instellingen. Houd er rekening mee dat u alleen een subset van deze velden, afhankelijk van of u een webtoepassing of een systeemeigen toepassing gemaakt zou zien.

| Veld           | Beschrijving                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Toepassings-id  | Als u een toepassing registreert, wijst Azure AD uw toepassing een toepassings-ID. De aanvraag-ID kan worden gebruikt voor het aanduiden van uw toepassing in verificatieaanvragen naar Azure AD, evenals voor toegang tot resources, zoals de Graph API.                                                          |
| URI voor de app-id      | Dit moet een unieke URI, meestal van het formulier **https://&lt;tenant\_naam&gt;/&lt;toepassing\_naam&gt;.** Dit wordt gebruikt tijdens de stroom voor het verlenen van machtiging als een unieke id om op te geven van de resource waarvoor het token moet worden uitgegeven. Ook wordt de claim 'aud' in het toegangstoken verleende. |
| Nieuw logo uploaden | U kunt dit gebruiken voor het uploaden van een logo voor uw toepassing. Het logo moet bmp-, JPG of PNG-indeling en de bestandsgrootte moet minder dan 100KB. Voor de afmetingen van de installatiekopie moet 215 x 215 pixels, met een centrale afmetingen van 94 x 94 pixels.                                                       |
| URL van startpagina   | Dit is de aanmeldings-URL die is opgegeven tijdens de toepassingsregistratie.                                                                                                                                                                                                                                              |
| URL voor afmelden      | Dit de URL voor eenmalige afmelding afmelding. Azure AD verzendt een afmeldingsaanvraag naar deze URL als de gebruiker hun sessie wist met Azure AD met behulp van andere geregistreerde toepassing.                                                                                                                                       |
| Met meerdere tenants  | Deze schakeloptie geeft aan of de toepassing kan worden gebruikt door meerdere tenants. Dit betekent doorgaans dat externe organisaties uw toepassing door deze in hun tenant te registreren en toegang verlenen tot gegevens van hun organisatie kunnen gebruiken.                                                                   |
| Antwoord-URL's      | Het antwoord-URL's zijn de eindpunten waarop Azure AD tokens retourneert die die uw toepassing worden aangevraagd.                                                                                                                                                                                                          |
| Omleidings-URI's   | Dit is waar de gebruiker wordt verzonden na een geslaagde autorisatie voor systeemeigen toepassingen. Azure AD-Controleer of de omleidings-URI van uw toepassing wordt verstrekt in de OAuth 2.0-aanvraag komt overeen met een van de geregistreerde waarden in de portal.                                                            |
| Sleutels            | U kunt sleutels om programmatisch toegang tot web-API's die zijn beveiligd door Azure AD zonder tussenkomst van de gebruiker te maken. Uit de \* \*sleutels\* \* pagina, voer een beschrijving van de sleutel en de vervaldatum en opslaan als de sleutel wilt genereren. Zorg ervoor dat u Sla deze ergens veilig, omdat het niet mogelijk toegang tot het later opnieuw.             |

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](manage-apps/what-is-application-management.md)

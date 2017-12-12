---
title: Het specifieke velden voor een toepassing ontwikkelde aangepaste invullen | Microsoft Docs
description: Instructies voor het specifieke velden invullen bij het registreren van een aangepaste toepassing ontwikkeld met Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cb85971d6d3d4dade69b9dc04e070c67341533a5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Het specifieke velden voor een toepassing ontwikkelde aangepaste invullen

In dit artikel kunt u een korte beschrijving van de beschikbare velden in het registratieformulier toepassing in de [Azure-portal](https://portal.azure.com).

## <a name="register-a-new-application"></a>Een nieuwe toepassing registreren

-   Als u wilt een nieuwe toepassing registreren, gaat u naar de [Azure-portal](https://portal.azure.com).

-   Klik in het navigatiedeelvenster links op **Azure Active Directory.**

-   Kies **App registraties** en klik op **toevoegen**.

-   Deze openen van het formulier van de registratie van toepassing.

## <a name="fields-in-the-application-registration-form"></a>Velden in de vorm van de registratie van toepassing


| Veld            | Beschrijving                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Naam             | De naam van de toepassing. Er moet ten minste vier tekens.                |
| Toepassingstype | **Web-app of Web API**: een toepassing die staat voor een webtoepassing, een web-API of beide 
| |**Systeemeigen**: een toepassing die op het apparaat of de computer van een gebruiker kan worden geïnstalleerd           |
| Aanmeldings-URL      | De URL waar gebruikers zich aanmelden kunnen bij uw toepassing gebruiken                                  |

Zodra u de bovenstaande velden hebt ingevuld, de toepassing worden geregistreerd in de Azure portal en u omgeleid naar de toepassingspagina. De **instellingen** knop in het Toepassingsdeelvenster opent u de pagina instellingen, waarvoor meer velden die u kunt uw toepassing aanpassen. De volgende tabel bevat alle velden in de pagina instellingen. Houd er rekening mee dat u alleen een subset van deze velden ziet, afhankelijk van of u een webtoepassing of een systeemeigen toepassing gemaakt.

| Veld           | Beschrijving                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Toepassings-id  | Wanneer u een toepassing registreert, wijst Azure AD uw toepassing een toepassing. De toepassing-ID kan worden gebruikt als unieke identificatie van uw toepassing in verificatieaanvragen naar Azure AD, evenals voor toegang tot resources, zoals de Graph API.                                                          |
| App-ID-URI      | Dit moet een unieke URI, meestal van het formulier **https://&lt;tenant\_naam&gt;/&lt;toepassing\_naam&gt;.** Dit wordt gebruikt tijdens de machtiging grant-stroom als een unieke id op te geven van de resource waarvoor het token moet worden uitgegeven. Dit wordt ook de claim 'aud' in het uitgegeven toegangstoken. |
| Nieuw logo uploaden | U kunt deze gebruiken voor het uploaden van een logo voor uw toepassing. Het logo moet bmp-, JPG of PNG-indeling en de bestandsgrootte moet groter zijn dan 100KB. Voor de afmetingen van de installatiekopie moet 215 x 215 pixels, met de afmetingen van de centrale afbeelding van 94 x 94 pixels.                                                       |
| URL van startpagina   | Dit is de aanmeldings-URL opgegeven tijdens de toepassingsregistratie.                                                                                                                                                                                                                                              |
| URL voor afmelden      | Deze de URL van één afmelden afmelden. Azure AD nu een afmeldingsaanvraag verzonden naar deze URL als de gebruiker de sessie met Azure AD wist met behulp van andere geregistreerde toepassing.                                                                                                                                       |
| Met meerdere tenants  | Deze schakeloptie geeft aan of de toepassing kan worden gebruikt door meerdere tenants. Dit betekent doorgaans dat uw toepassing in externe organisaties kunnen worden gebruikt door het registreren van deze in de tenant en het verlenen van toegang tot gegevens van hun organisatie.                                                                   |
| Antwoord-URL's      | Het antwoord-URL's zijn de eindpunten waarop Azure AD tokens retourneert die door uw toepassing worden aangevraagd.                                                                                                                                                                                                          |
| Omleidings-URI's   | Voor systeemeigen toepassingen is dit waar de gebruiker worden verzonden naar de volgende geslaagde autorisatie. Azure AD-Controleer of de omleidings-URI van uw toepassing wordt verstrekt in de OAuth 2.0-aanvraag komt overeen met een van de geregistreerde waarden in de portal.                                                            |
| Sleutels            | U kunt sleutels om programmatisch toegang tot web-API's die zijn beveiligd door Azure AD zonder tussenkomst van de gebruiker te maken. Van de \* \*sleutels\* \* pagina, Geef een beschrijving van de sleutel en de vervaldatum en opslaan om de sleutel te genereren. Zorg ervoor dat het op te slaan ergens veilig, omdat het niet mogelijk later gebruiken.             |

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)

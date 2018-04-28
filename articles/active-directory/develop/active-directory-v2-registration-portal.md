---
title: App-registratie Portal Help-onderwerpen | Microsoft Docs
description: Een beschrijving van verschillende functies in de Microsoft app-registratieportal.
services: active-directory
documentationcenter: ''
author: lnalepa
manager: mtillman
editor: ''
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: dc915facfcd62aaa05c403895631614e1a3381b9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="app-registration-reference"></a>Verwijzing van App-registratie
Dit document bevat context en beschrijvingen van de verschillende functies die zijn gevonden in de Microsoft App-Registratieportal [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/).

## <a name="my-applications"></a>Mijn toepassingen
Deze lijst bevat alle van de toepassingen die zijn geregistreerd voor gebruik met de Azure AD v2.0-eindpunt. Deze toepassingen hebben de mogelijkheid aan te melden gebruikers met persoonlijke Microsoft-accounts en werk/schoolaccounts van Azure Active Directory. Zie voor meer informatie over het Azure AD v2.0-eindpunt, de [v2.0 overzicht](active-directory-appmodel-v2-overview.md). Deze toepassingen kunnen ook worden gebruikt om te integreren met het eindpunt van de verificatie in de Microsoft-account, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK-Apps
Deze lijst bevat alle van de toepassingen die zijn geregistreerd voor gebruik uitsluitend met Microsoft-account. Ze zijn niet ingeschakeld voor gebruik met Azure Active Directory. Dit is waar het vinden van alle toepassingen die eerder zijn geregistreerd bij de portal voor ontwikkelaars van MSA op `https://account.live.com/developers/applications`. Alle functies die u eerder hebt uitgevoerd op `https://account.live.com/developers/applications` kunnen nu worden uitgevoerd in deze nieuwe portal `https://apps.dev.microsoft.com`. Als u meer vragen over uw toepassingen van Microsoft-account hebt, contact met ons opnemen.

## <a name="application-secrets"></a>Toepassingsgeheimen
Toepassing geheimen zijn referenties waarmee uw toepassing uit te voeren betrouwbare [clientverificatie](http://tools.ietf.org/html/rfc6749#section-2.3) met Azure AD. In het OAuth- & OpenID Connect, een toepassingsgeheim wordt vaak aangeduid als een `client_secret`. In het v2.0-protocol, alle toepassingen die u een beveiligingstoken voor een adresseerbare weblocatie ontvangt (met behulp van een `https` schema) moet een toepassingsgeheim gebruiken om zichzelf te identificeren bij Azure AD bij inwisseling die beveiligingstoken. Bovendien wordt een native client die tokens op een apparaat ontvangt worden verboden clientverificatie uitvoeren via een toepassingsgeheim. Dit raadt de opslag van geheimen in onbeveiligde omgevingen af.

Elke app kan twee geldige aanvraag-geheimen op elk moment bevatten. Door twee geheimen, hebt u de mogelijkheid om uit te voeren periodieke sleutelrollover over de hele omgeving van uw toepassing. Zodra u het geheel van uw toepassing naar een nieuwe geheim hebt gemigreerd, kunt u deze kunt verwijderen van het oude geheim en inrichten van een nieuwe.

Op dit moment zijn in de app-portal voor wachtwoordregistratie slechts twee soorten toepassing geheimen toegestaan. Kiezen **nieuw wachtwoord genereren** genereert en slaat u een gedeeld geheim in de respectieve gegevensopslag, kunt u in uw toepassing. Kiezen **sleutelpaar genereren** maakt een nieuw openbaar/persoonlijk sleutelpaar sleutel die kan worden gedownload en kan worden gebruikt voor clientverificatie naar Azure AD. Kiezen **openbare sleutel uploaden** kunt u uw eigen openbare/persoonlijke sleutelpaar gebruiken.
U moet een certificaat met een openbare sleutel te uploaden.

## <a name="profile"></a>Profiel
De profielsectie van de portal van de registratie van de app kan worden gebruikt voor het aanpassen van de aanmeldingspagina voor uw toepassing. Op dit moment kunt u de aanmeldingspagina van toepassing logo, alter voorwaarden van de service-URL en de URL van privacyverklaring. Het logo moet een transparante afbeelding van 48 x 48 of 50 x 50 pixels in een GIF-, PNG- of JPEG-bestand van 15 kB of kleiner zijn. Probeer de waarden wijzigen en het bekijken van de resulterende aanmeldingspagina!

## <a name="live-sdk-support"></a>Ondersteuning voor Live SDK
Als u 'Live SDK Support' inschakelt, geen toepassing geheimen die u maakt in de Azure AD worden ingericht en Microsoft-Account gegevensarchieven. Dit kan de toepassing rechtstreeks te integreren met de service Microsoft-Account (login.live.com). Als u wenst voor het bouwen van een app met behulp van Microsoft-Account rechtstreeks (in plaats van via het Azure AD v2.0-eindpunt), moet u ervoor zorgen dat ondersteuning voor Live SDK is ingeschakeld.

Ondersteuning voor Live SDK uitschakelen zorgt ervoor dat de toepassingsgeheim alleen wordt geschreven naar de Azure AD-gegevens opslaat. De Azure AD-gegevens store bedrijfsniveau voorschriften die van toe te staan om te voldoen aan bepaalde standaarden, zoals FISMA naleving opgenomen. Als u ondersteuning voor Live SDK inschakelt, kan uw toepassing compatibiliteit met enkele van deze standaarden niet bereiken.

Als u alleen ooit het Azure AD v2.0-eindpunt te gebruiken, kunt u veilig ondersteuning voor Live SDK uitschakelen.


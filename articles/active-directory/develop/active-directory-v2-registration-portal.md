---
title: App-registratie Portal Help-onderwerpen | Microsoft Docs
description: Een beschrijving van verschillende functies in de Microsoft app-registratieportal.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 5484f939cfc3e1187f282251fdc181a13af275b4
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080647"
---
# <a name="app-registration-reference"></a>Naslaginformatie over app-registratie
Dit document bevat context en beschrijvingen van de verschillende functies die zijn gevonden in de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/).

## <a name="my-applications-or-converged-applications"></a>Mijn toepassingen of geconvergeerde toepassingen
Deze lijst bevat al uw toepassingen die zijn geregistreerd voor gebruik met de Azure AD v2.0-eindpunt. Deze toepassingen hebben de mogelijkheid om aan te melden bij de gebruikers met persoonlijke Microsoft-accounts en werk-of schoolaccounts van Azure Active Directory. Zie voor meer informatie over de Azure AD v2.0-eindpunt, de [v2.0 overzicht](active-directory-appmodel-v2-overview.md). Deze toepassingen kunnen ook worden gebruikt om te integreren met de Microsoft-account verificatie-eindpunt `https://login.live.com`.

## <a name="azure-ad-only-applications"></a>Alleen toepassingen met Azure AD
Deze lijst bevat al uw toepassingen die zijn geregistreerd voor gebruik met de Azure AD v1.0-eindpunt. Deze toepassingen hebben alleen de mogelijkheid om aan te melden bij de gebruikers met een werk-of schoolaccounts van Azure Active Directory. Deze lijst bevat de toepassingen die zijn geregistreerd met behulp van de **App-registraties** -ervaring in de [Azure Portal](https://portal.azure.com).

## <a name="live-sdk-applications"></a>Live SDK-toepassingen
Deze lijst bevat al uw toepassingen die zijn geregistreerd voor gebruik uitsluitend met Microsoft-account. Ze zijn niet ingeschakeld voor gebruik met Azure Active Directory. Dit is waar het vinden van alle toepassingen die eerder zijn geregistreerd met de MSA-ontwikkelaarsportal op `https://account.live.com/developers/applications`. Alle functies die u eerder hebt uitgevoerd op `https://account.live.com/developers/applications` kan nu worden uitgevoerd in deze nieuwe portal `https://apps.dev.microsoft.com`.

## <a name="application-secrets"></a>Toepassingsgeheimen
Toepassingsgeheimen zijn referenties waarmee uw toepassing uit te voeren betrouwbare [clientverificatie](https://tools.ietf.org/html/rfc6749#section-2.3) met Azure AD. In OAuth en OpenID Connect, een toepassingsgeheim wordt vaak aangeduid als een `client_secret`. In het v2.0-protocol, een toepassing die u een beveiligingstoken op een website opgevraagd locatie ontvangt (met behulp van een `https` schema) moet een toepassingsgeheim gebruiken om zichzelf te identificeren met Azure AD bij het inwisselen van die beveiligingstoken. Bovendien een systeemeigen client die tokens op een apparaat ontvangt zal worden is niet toegestaan vanuit een toepassingsgeheim met clientverificatie uitvoeren. Dit ontmoedigt de opslag van geheimen in omgevingen met niet-beveiligd.

Elke app kan twee geldige toepassingsgeheimen op een bepaald moment bevatten. Dankzij de twee geheimen, hebt u de mogelijkheid om uit te voeren van periodieke sleutelrollover binnen de gehele omgeving van uw toepassing. Wanneer u het geheel van uw toepassing in een nieuw geheim hebt gemigreerd, kunt u de oude geheim verwijderen en inrichten van een nieuwe.

Op dit moment zijn slechts twee typen van toepassingsgeheimen toegestaan in de portal voor app-registratie. Kiezen **nieuw wachtwoord genereren** genereert en slaat u een gedeeld geheim in het betreffende gegevensarchief, die u in uw toepassing gebruiken kunt. Kiezen **nieuw sleutelpaar genereren** maakt u een nieuw openbaar/persoonlijk-sleutelpaar dat kan worden gedownload en kan worden gebruikt voor clientverificatie voor Azure AD. Kiezen **openbare sleutel uploaden** kunt u uw eigen openbaar/persoonlijk sleutelpaar gebruiken.
U moet een certificaat met een openbare sleutel uploaden.

## <a name="profile"></a>Profiel
De profielsectie van de portal van de registratie van de app kan worden gebruikt voor het aanpassen van de aanmeldingspagina voor uw toepassing. Op dit moment kunt u de aanmeldingspagina van toepassing logo, alter voorwaarden van de service-URL en de URL voor de privacyverklaring. Het logo moet een transparante afbeelding van 48 x 48 of 50 x 50 pixels in een GIF-, PNG- of JPEG-bestand van 15 kB of kleiner zijn. Probeer te wijzigen van de waarden en het bekijken van de resulterende aanmeldingspagina!

## <a name="live-sdk-support"></a>Live SDK-ondersteuning
Wanneer u 'Live SDK Support' inschakelt, een toepassingsgeheimen die u maakt in zowel de Azure AD worden ingericht en Microsoft-Account data-archieven. Hiermee wordt uw toepassing integreren rechtstreeks met de service Microsoft-Account (login.live.com). Als u wilt een app bouwen met Microsoft-Account rechtstreeks (in plaats van met behulp van de Azure AD v2.0-eindpunt), moet u ervoor zorgen dat Live SDK-ondersteuning is ingeschakeld.

Uitschakelen van de Live SDK-ondersteuning, zorgt u ervoor dat het toepassingsgeheim alleen is geschreven in de Azure AD-gegevens opslaan. De Azure AD-gegevens store zakelijke voorschriften die toe te staan om te voldoen aan bepaalde standaarden, zoals FISMA naleving tevens. Als u de Live SDK-ondersteuning inschakelt, kan uw toepassing naleving met enkele van deze standaarden niet bereiken.

Als u alleen ooit het Azure AD v2.0-eindpunt gebruiken wilt, kunt u veilig Live SDK-ondersteuning uitschakelen.


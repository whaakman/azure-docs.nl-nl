---
title: App-registratie Portal Help-onderwerpen | Microsoft Docs
description: Een beschrijving van verschillende functies in de Microsoft app-registratieportal.
services: active-directory
documentationcenter: 
author: lnalepa
manager: mtillman
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 3340df3fcc2456a355e523bfcf09978a16966036
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="app-registration-reference"></a>Verwijzing van App-registratie
Dit document bevat context en beschrijvingen van de verschillende functies die zijn gevonden in de Microsoft App-Registratieportal [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Mijn toepassingen
Deze lijst bevat alle van de toepassingen die zijn geregistreerd voor gebruik met de Azure AD v2.0-eindpunt.  Deze toepassingen hebben de mogelijkheid aan te melden gebruikers met beide persoonlijke accounts van Microsoft-account en werk/schoolaccounts van Azure Active Directory.  Zie voor meer informatie over het Azure AD v2.0-eindpunt, onze [v2.0 overzicht](active-directory-appmodel-v2-overview.md).  Deze toepassingen kunnen ook worden gebruikt om te integreren met het eindpunt van de verificatie in de Microsoft-account, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK-Apps
Deze lijst bevat alle van de toepassingen die zijn geregistreerd voor gebruik uitsluitend met Microsoft-account.  Ze zijn niet ingeschakeld voor gebruik met Azure Active Directory beeïndigen.  Dit is waar vindt u alle toepassingen die eerder was is geregistreerd in de ontwikkelaarsportal MSA op `https://account.live.com/developers/applications`.  Alle functies die u eerder hebt uitgevoerd op `https://account.live.com/developers/applications` kunnen nu worden uitgevoerd in deze nieuwe portal `https://apps.dev.microsoft.com`.  Als u meer vragen over uw toepassingen van Microsoft-account hebt, neem dan contact met ons.

## <a name="application-secrets"></a>Toepassingsgeheimen
Toepassing geheimen zijn referenties waarmee uw toepassing uit te voeren betrouwbare [clientverificatie](http://tools.ietf.org/html/rfc6749#section-2.3) met Azure AD.  In het OAuth- & OpenID Connect, een toepassing geheimen wordt vaak aangeduid als een `client_secret`.  In het v2.0-protocol, alle toepassingen die u een beveiligingstoken voor een adresseerbare weblocatie ontvangt (met behulp van een `https` schema) moet een toepassingsgeheim gebruiken om zichzelf te identificeren bij Azure AD bij inwisseling die beveiligingstoken.  Bovendien een native client die krijgen tokens op een apparaat wordt worden niet toegestaan vanaf een toepassingsgeheim met voor het uitvoeren van clientverificatie, om te voorkomen dat de opslag van geheimen in onbeveiligde omgevingen.

Elke app kan twee geldige aanvraag-geheimen op elk gewenst moment in de tijd bevatten.  Door twee geheimen, hebt u de ablilty periodieke sleutelrollover uitvoert voor de gehele omgeving van uw toepassing.  Zodra u het geheel van uw toepassing naar een nieuwe geheim hebt gemigreerd, kunt u deze kunt verwijderen van het oude geheim en inrichten van een nieuwe.

Slechts twee soorten geheimen van toepassing zijn op dit moment toegestaan in de app-portal voor wachtwoordregistratie.  Kiezen **nieuw wachtwoord genereren** gaat genereren en opslaan van een gedeeld geheim in de respectieve gegevensopslag, kunt u in uw toepassing.  Kiezen **sleutelpaar genereren** maakt een nieuw openbaar/persoonlijk sleutelpaar sleutel die kan worden gedownload en kan worden gebruikt voor clientverificatie naar Azure AD.

## <a name="profile"></a>Profiel
De profielsectie van de portal van de registratie van de app kan worden gebruikt voor het aanpassen van de aanmeldingspagina voor uw toepassing.  Op dit moment kunt u de aanmeldingspagina van pagina toepassing logo, voorwaarden van de service-URL en privacyverklaring wijzigen.  Het logo moet een transparante afbeelding van 48 x 48 of 50 x 50 pixels in een GIF-, PNG- of JPEG-bestand van 15 kB of kleiner zijn.  Probeer de waarden wijzigen en het bekijken van de resulterende aanmelding op de pagina!

## <a name="live-sdk-support"></a>Ondersteuning voor Live SDK
Als u 'Live SDK Support' inschakelt, geen toepassing geheimen die u maakt in de Azure AD worden ingericht en Microsoft-Account gegevensarchieven.  Hierdoor kan uw toepassing rechtstreeks te integreren met de service Microsoft-Account (login.live.com).  Als u wenst voor het bouwen van een app met behulp van Microsoft-Account rechtstreeks (in plaats van via het Azure AD v2.0-eindpunt), moet u ervoor zorgen dat de ondersteuning van Live SDK is ingeschakeld.

Ondersteuning voor Live SDK uitschakelen zorgt ervoor dat de toepassingsgeheim alleen wordt geschreven naar de Azure AD-gegevens opslaat.  De Azure AD-gegevens store bedrijfsniveau voorschriften die van toe te staan om te voldoen aan bepaalde standaarden, zoals FISMA naleving opgenomen.  Als u ondersteuning voor Live SDK inschakelt, kan uw toepassing compatibiliteit met enkele van deze standaarden niet bereiken.

Als u alleen ooit het Azure AD v2.0-eindpunt te gebruiken, kunt u veilig ondersteuning voor Live SDK uitschakelen.


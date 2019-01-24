---
title: Gebruiker stroom versies in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over de versies van gebruikersstromen beschikbaar in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: eed5b27a2bdcc30aaf6befc19c5974936cabf849
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54854155"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Gebruiker stroom versies in Azure Active Directory B2C

>[!IMPORTANT]
> Gebruiker stromen die worden vermeld in het artikel wordt beschouwd als openbare preview-versie, tenzij geïdentificeerd als **aanbevolen**. U moet de aanbevolen gebruikersstromen alleen gebruiken voor uw productietoepassingen.

Gebruikersstromen in Azure Active Directory (Azure AD) B2C kunnen u voor het instellen van algemene [beleid](active-directory-b2c-reference-policies.md) die identiteit klantervaringen volledig te beschrijven. Deze ervaringen zijn gebruikersregistratie, aanmelding, wachtwoord opnieuw instellen of bewerken van profielen. In Azure AD B2C, kunt u selecteren uit een verzameling van aanbevolen gebruikersstromen en gebruikersstromen Preview-versie. 

Nieuwe gebruikersstromen worden toegevoegd als nieuwe versies. Gebruikersstromen komen stabiel, wordt ze worden aanbevolen voor gebruik. Gebruikersstromen zijn gemarkeerd als **aanbevolen** als ze grondig hebt getest. Gebruikersstromen wordt beschouwd als Preview-versie totdat gemarkeerd als aanbevolen. Een aanbevolen beleid voor een productietoepassing gebruiken, maar kiezen uit andere versies voor het testen van nieuwe functionaliteit wanneer deze beschikbaar. Oudere versies van aanbevolen gebruikersstromen kunt u al dan niet mogen gebruiken.

## <a name="v1"></a>V1

| Gebruikersstroom | Aanbevolen | Description |
| --------- | ----------- | ----------- |
| Wachtwoord opnieuw instellen | Ja | Kan een gebruiker een nieuw wachtwoord kiezen nadat u hebt gecontroleerd van hun e-mailadres. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Profiel bewerken | Ja | Kan een gebruiker configureren hun gebruikerskenmerken. Met dit beleid kunt configureren u: <ul><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li></ul> |
| resource-eigenaar | Nee | Kan een gebruiker met een lokaal account aanmelden rechtstreeks in systeemeigen toepassingen (geen browser vereist). Met dit beleid kunt configureren u: <ul><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li></ul> |
| Aanmelden | Nee | Kan een gebruiker zich aanmeldt bij hun account. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li><li>Aanmelding blokkeren</li><li>Opnieuw instellen van wachtwoorden forceren</li><li>Aangemeld blijven In (KMSI)</ul><br>U kunt de gebruikersinterface met deze gebruikersstroom niet aanpassen. |
| Aanmelden | Nee | Kan een gebruiker om een account te maken. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registreren en aanmelden | Ja | Kan een gebruiker voor het maken van een account of aanmelden bij hun account. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul>|

## <a name="v2"></a>V2

| Gebruikersstroom | Aanbevolen | Description |
| --------- | ----------- | ----------- |
| Wachtwoord opnieuw instellen v2 | Nee | Kan een gebruiker een nieuw wachtwoord kiezen nadat u hebt gecontroleerd van hun e-mailadres. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>[Leeftijdsbeperking](basic-age-gating.md)</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Aanmelden v2 | Nee | Kan een gebruiker zich aanmeldt bij hun account. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li><li>[Leeftijdsbeperking](basic-age-gating.md)</li><li>Aanpassing van de aanmeldingspagina opgeven</li></ul> |
| Registreren v2 | Nee | Kan een gebruiker om een account te maken. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Levensduur van token](active-directory-b2c-reference-tokens.md)</li><li>Instellingen voor tokencompatibiliteit</li><li>Sessiegedrag</li><li>[Leeftijdsbeperking](basic-age-gating.md)</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registreren en aanmelden in v2 | Nee | Kan een gebruiker voor het maken van een account of meld u aan hun account. Met dit beleid kunt configureren u: <ul><li>[Multi-factor authentication](active-directory-b2c-reference-mfa.md)</li><li>[Leeftijdsbeperking](basic-age-gating.md)</li><li>[Vereisten voor wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity.md)</li></ul> |

---
title: Schakel leeftijdsbeperking in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het identificeren van minderjarigen met behulp van uw toepassing.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ddc4b7b899385eb7388198973460386a4a0af10c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845769"
---
# <a name="enable-age-gating-in-azure-active-directory-b2c"></a>Leeftijdsbeperking in Azure Active Directory B2C inschakelen

>[!IMPORTANT]
>Deze functie is beschikbaar voor openbare preview. Gebruik de functie niet voor productie-Apps. 
>

Leeftijdsbeperking in Azure Active Directory (Azure AD) B2C, kunt u identificeren minderjarigen die uw toepassing gebruiken. U kunt de secundaire zich in de toepassing blokkeren. Gebruikers kunnen ook gaat u terug naar de toepassing en hun leeftijdsgroep en hun status ouderlijke toestemming te identificeren. Azure AD B2C kunt minderjarigen zonder toestemming van ouders blokkeren. Azure AD B2C kan ook worden ingesteld zodat de toepassing om te bepalen wat er moet gebeuren met minderjarigen.

Nadat u hebt ingeschakeld leeftijdsbeperking uw [gebruikersstroom](active-directory-b2c-reference-policies.md), gebruikers wordt gevraagd wanneer ze zijn voortgekomen en welk land ze bevinden zich in. Als een gebruiker zich die de informatie nog niet eerder zijn ingevoerd, moet ze voert u deze de volgende keer dat ze zich aanmelden. De regels worden toegepast wanneer een gebruiker zich aanmeldt.

Azure AD B2C gebruikt de informatie die de gebruiker moet invoeren om te bepalen of ze een minderjarige. De **ageGroup** veld wordt vervolgens bijgewerkt in hun account. De waarde kan zijn `null`, `Undefined`, `Minor`, `Adult`, en `NotAdult`.  De **ageGroup** en **consentProvidedForMinor** velden worden vervolgens gebruikt voor het berekenen van de waarde van **legalAgeGroupClassification**.

Leeftijdsbeperking bestaat uit twee waarden van de leeftijd: de leeftijd die iemand anders niet meer beschouwd als een minderjarige en de leeftijd waarmee een kleine ouderlijke toestemming moet hebben. De volgende tabel bevat de leeftijd regels die worden gebruikt voor het definiëren van een klein en de toestemming van een secundaire vereisen.

| Land/regio | Naam van land/regio | Secundaire toestemming leeftijd | Secundaire leeftijd |
| ------- | ------------ | ----------------- | --------- |
| Standaard | Geen | Geen | 18 |
| AE | Verenigde Arabische Emiraten | Geen | 21 |
| AT | Oostenrijk | 14 | 18 |
| BE | België | 14 | 18 |
| BG | Bulgarije | 16 | 18 |
| BH | Bahrein | Geen | 21 |
| CM | Kameroen | Geen | 21 |
| CY | Cyprus | 16 | 18 |
| CZ | Tsjechië | 16 | 18 |
| DE | Duitsland | 16 | 18 |
| DK | Denemarken | 16 | 18 |
| EE | Estland | 16 | 18 |
| EG | Egypte | Geen | 21 |
| ES | Spanje | 13 | 18 |
| FR | Frankrijk | 16 | 18 |
| GB | Verenigd Koninkrijk | 13 | 18 |
| GR | Griekenland | 16 | 18 |
| HR | Kroatië | 16 | 18 |
| HU | Hongarije | 16 | 18 |
| IE | Ierland | 13 | 18 |
| IT | Italië | 16 | 18 |
| KR | Zuid-Korea | 14 | 18 |
| LT | Litouwen | 16 | 18 |
| LU | Luxemburg | 16 | 18 |
| LV | Letland | 16 | 18 |
| MT | Malta | 16 | 18 |
| N.v.t. | Namibië | Geen | 21 |
| NL | Nederland | 16 | 18 |
| PL | Polen | 13 | 18 |
| PT | Portugal | 16 | 18 |
| RO | Roemenië | 16 | 18 |
| SE | Zweden | 13 | 18 |
| SG | Singapore | Geen | 21 |
| SI | Slovenië | 16 | 18 |
| SK | Slowakije | 16 | 18 |
| TD | Tsjaad | Geen | 21 |
| TH | Thailand | Geen | 20 |
| TW | Taiwan | Geen | 20 | 
| VS | Verenigde Staten | 13 | 18 |

## <a name="age-gating-options"></a>Opties voor leeftijdsbeperking
 
### <a name="allowing-minors-without-parental-consent"></a>Minderjarigen zonder toestemming van ouders toestaan

Voor gebruikersstromen waarmee u registratie, aanmelding, of beide, u kunt kiezen om minderjarigen zonder toestemming in uw toepassing. Minderjarigen zonder ouderlijke toestemming mogen aanmelden of registreren als normale en Azure AD B2C problemen met een ID-token met de **legalAgeGroupClassification** claim. Deze claim definieert de ervaring die gebruikers hebben, zoals ouderlijke toestemming verzamelen en het bijwerken van de **consentProvidedForMinor** veld.

### <a name="blocking-minors-without-parental-consent"></a>Minderjarigen zonder toestemming van ouders blokkeren

Voor de gebruikersstromen waarmee registratie, aanmelding bij of beide, kunt u minderjarigen zonder toestemming van de toepassing blokkeren. De volgende opties zijn beschikbaar voor het verwerken van geblokkeerde gebruikers in Azure AD B2C:

- Verzenden van een JSON terug naar de toepassing: deze optie stuurt een antwoord terug naar de toepassing die een minderjarige is geblokkeerd.
- Een foutpagina - kunt weergeven die de gebruiker een pagina die wordt gemeld dat ze geen toegang de toepassing tot wordt weergegeven.

## <a name="set-up-your-tenant-for-age-gating"></a>Instellen van uw tenant voor leeftijdsbeperking

Voor het gebruik van leeftijdsbeperking in de gebruikersstroom van een, moet u uw tenant om aanvullende eigenschappen configureren.

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu. Selecteer de map waarin uw tenant. 
2. Selecteer **alle services** Zoek in de linkerbovenhoek van Azure portal en selecteer **Azure AD B2C**.
3. Selecteer **eigenschappen** voor uw tenant in het menu aan de linkerkant.
2. Onder de **leeftijdsbeperking** sectie, klikt u op **configureren**.
3. Wacht totdat de bewerking is voltooid en uw tenant worden ingesteld voor leeftijdsbeperking.

## <a name="enable-age-gating-in-your-user-flow"></a>Leeftijdsbeperking in uw beleid inschakelen

Nadat uw tenant ingesteld voor leeftijdsbeperking gebruikt is, kunt u vervolgens deze functie in gebruiken [gebruikersstromen](user-flow-versions.md) waarin deze ingeschakeld. U schakelt leeftijdsbeperking met de volgende stappen uit:

1. Maak de gebruikersstroom van een met leeftijdsbeperking ingeschakeld.
2. Nadat u de gebruikersstroom maakt, selecteert u **eigenschappen** in het menu.
3. In de **leeftijdsbeperking** sectie, selecteer **ingeschakeld**.
4. U kunt vervolgens bepalen hoe u gebruikers wilt beheren die als minderjarigen identificeren. Voor **registreren of aanmelden**, selecteert u `Allow minors to access your application` of `Block minors from accessing your application`. Als minderjarigen blokkeren is ingeschakeld, selecteert u `Send a JSON bcak to the application` of `Show an error message`. 





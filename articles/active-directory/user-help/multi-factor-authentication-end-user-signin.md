---
title: Azure MFA aanmelding met verificatie in twee stappen | Microsoft Docs
description: Deze pagina vindt u richtlijnen op waar u om te zien van de verschillende aanmelden methoden beschikbaar met Azure MFA.
keywords: verificatie van de gebruiker, aanmelden, aanmelding met telefoon, aanmelding met telefoon (werk)
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
ms.date: 04/02/2017
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 21228801f866d7221270034e862fe8145ccb839e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815549"
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>De ervaring van aanmelding met Azure multi-factor Authentication
> [!NOTE]
> Het doel van dit artikel is om te zien hoe een typische ervaring aanmelden. Zie voor hulp bij het aanmelden of het oplossen van problemen, [problemen hebt met Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Wat is uw ervaring aanmelden?
Uw aanmeldingsprocedure is afhankelijk van wat u wilt gebruiken als de tweede factor: een telefonische oproep, een authentication-app of teksten. Kies de optie die het beste beschrijft wat u doet:

| Hoe meld u aan? |
| --- |
| [Met een telefonische oproep naar mijn telefoon mobile- of office](#signing-in-with-a-phone-call) |
| [Met een tekst naar mijn mobiele telefoon](#signing-in-with-a-text-message)
| [Dankzij de meldingen van de Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Met verificatiecodes uit de Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Met een alternatieve methode, omdat ik mijn voorkeursmethode nu niet kan gebruiken](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Aanmelden met een telefonische oproep
De volgende informatie beschrijft de verificatie in twee stappen verificatie-ervaring met een aanroep naar uw telefoon mobile- of office.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.  
2. Microsoft roept u.  
3. Beantwoord de telefoon en druk op de toets #.  

## <a name="signing-in-with-a-text-message"></a>Aanmelden met een SMS-bericht
De volgende informatie beschrijft de ervaring van de verificatie in twee stappen met een SMS-bericht naar uw mobiele telefoon:

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft stuurt u een SMS-bericht met een aantal code.
3. Voer de code in het vak op de pagina aanmelden.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Aanmelden met de Microsoft Authenticator-app
De volgende informatie beschrijft de ervaring van het gebruik van de Microsoft Authenticator-app voor verificatie in twee stappen verificaties. Er zijn twee verschillende manieren om de app te gebruiken. U kunt pushmeldingen ontvangen op uw apparaat kunt, of u de app om een verificatiecode.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Aanmelden met een melding van de Microsoft Authenticator-app
1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft stuurt een melding naar de Microsoft Authenticator-app op uw apparaat.

  ![Microsoft stuurt melding](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Open de melding op uw telefoon en selecteer de **controleren** sleutel. Als uw bedrijf een PINCODE is vereist, moet u deze hier opgeven.
4. U moet nu worden aangemeld.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Zich aanmelden met een verificatiecode met de Microsoft Authenticator-app

Als u de Microsoft Authenticator-app gebruikt om op te halen verificatiecodes, klikt u vervolgens ziet bij het openen van de app u een nummer onder de accountnaam van uw. Deze waarde verandert elke 30 seconden, zodat u niet twee keer hetzelfde getal gebruikt. Wanneer u wordt gevraagd om een verificatiecode, open de app en gebruik getal op dat moment wordt weergegeven.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft vraagt u om een verificatiecode.

  ![Verificatiecode invoeren](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Open de Microsoft Authenticator-app op uw telefoon en voer de code in het vak waar u zich aanmeldt.

## <a name="signing-in-with-an-alternate-method"></a>Aanmelden met een alternatieve methode
Soms hebt u niet de telefoon of het apparaat die u hebt ingesteld als uw contactmethode voorkeursoptie voor verificatie. Deze situatie is de reden waarom is het raadzaam dat u om back-upmethoden voor uw account instelt. De volgende sectie wordt beschreven hoe u zich aanmelden met een alternatieve methode wanneer de primaire methode mogelijk niet beschikbaar.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Selecteer **gebruiken een andere verificatieoptie**. Ziet u verschillende verificatie-opties op basis van hoeveel u ingesteld hebt.
3. Kies een alternatieve methode en meld u aan.

  ![Alternatieve methode gebruiken](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Volgende stappen

Als u problemen met aanmelden met verificatie in twee stappen hebt, u meer informatie op [problemen hebt met Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

Meer informatie over het [beheren van de verificatie-instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md).

Meer informatie over het [aan de slag met de Microsoft Authenticator-app](microsoft-authenticator-app-how-to.md) zodat u meldingen kunt aan te melden bij, in plaats van teksten en telefoongesprekken.

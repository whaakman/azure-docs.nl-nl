---
title: Azure MFA aanmelden met verificatie in twee stappen | Microsoft Docs
description: Deze pagina vindt u u richtlijnen voor het bekijken van de verschillende aanmelden methoden beschikbaar met Azure MFA.
keywords: verificatie van gebruikers, -aanmeldingservaring aanpast, aanmelden met een mobiele telefoon, aanmelden met een telefoon (werk)
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: c47356b7b84e38a1db9259304c2a975958b1977c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>De ervaring aanmelden met Azure multi-factor Authentication
> [!NOTE]
> Het doel van dit artikel is een typische aanmeldingservaring doorlopen. Zie voor meer informatie met het aanmelden of problemen [problemen hebt met Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Wat is uw ervaring aanmelden?
Uw aanmeldingservaring is afhankelijk van wat u wilt gebruiken als uw tweede factor: een telefonische oproep of een app authentication teksten. Kies de optie die het beste beschrijft wat u doet:

| Hoe meld u aan? |
| --- |
| [Met een telefonische oproep naar mijn telefoon mobile- of office](#signing-in-with-a-phone-call) |
| [Met een tekst naar mijn mobiele telefoon](#signing-in-with-a-text-message)
| [Met meldingen van de Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Met verificatiecodes vanuit de Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Met een alternatieve methode omdat ik mijn voorkeursmethode nu kunt gebruiken](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Aanmelden met een telefonische oproep
De volgende informatie beschrijft de ervaring van de verificatie in twee stappen met een aanroep naar uw telefoon mobile- of kantoornetwerk.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.  
2. Microsoft roept u.  
3. Beantwoord de telefoon en druk op de #-toets.  

## <a name="signing-in-with-a-text-message"></a>Aanmelden met een SMS-bericht
De volgende informatie beschrijft de ervaring van de verificatie in twee stappen met een SMS-bericht naar uw mobiele telefoon:

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft stuurt u een SMS-bericht met een aantal code.
3. Voer de code in de daarvoor bestemde vak op de aanmeldingspagina.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Aanmelden met de Microsoft Authenticator-app
De volgende informatie beschrijft de ervaring van het gebruik van de Microsoft Authenticator-app voor verificatie in twee stappen. Er zijn twee verschillende manieren om de app te gebruiken. U pushmeldingen kan ontvangen op uw apparaat kunt, of u de app als u een verificatiecode.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Aanmelden met een melding van de Microsoft Authenticator-app
1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft stuurt een melding met de Microsoft Authenticator-app op uw apparaat.

  ![Microsoft verzendt melding](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Open de melding op uw telefoon en selecteer de **controleren** sleutel. Als uw bedrijf een PINCODE vereist is, moet u deze hier opgeven.
4. U moet nu worden aangemeld.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Aanmelden met een verificatiecode met de Microsoft Authenticator-app

Als u de Microsoft Authenticator-app om op te halen verificatiecodes gebruikt, vervolgens ziet wanneer u de app opent u een getal onder de accountnaam van uw. Deze waarde verandert elke 30 seconden, zodat u niet hetzelfde nummer twee keer gebruikt. Wanneer u wordt gevraagd of u voor een verificatiecode, open de app en gebruiken met het opgegeven getal momenteel wordt weergegeven.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Microsoft vraagt u om een verificatiecode.

  ![Verificatiecode invoeren](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Open de Microsoft Authenticator-app op uw telefoon en voer de code in het vak waar u zich aanmeldt.

## <a name="signing-in-with-an-alternate-method"></a>Aanmelden met een alternatieve methode
Soms hebt u niet de telefoon of het apparaat die u hebt ingesteld als uw contactmethode voorkeursoptie voor verificatie. Deze situatie is de reden waarom het is raadzaam dat u back-upmethoden voor uw account ingesteld. De volgende sectie leest u hoe zich kunnen aanmelden met een alternatieve methode als uw primaire contactmethode mogelijk niet beschikbaar.

1. Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
2. Selecteer **gebruik een andere verificatieoptie**. Er zijn andere verificatie-opties op basis van hoeveel ingesteld hebt.
3. Selecteer een alternatieve methode en meld u aan.

  ![Alternatieve methode gebruiken](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Volgende stappen

Als u problemen met aanmelden met verificatie in twee stappen hebt, informatie op te krijgen [problemen hebt met Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

Meer informatie over hoe [de instellingen van de verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md).

Ontdek hoe [aan de slag met de app Microsoft Authenticator](microsoft-authenticator-app-how-to.md) zodat u meldingen kunt aan te melden, in plaats van teksten en telefoongesprekken.

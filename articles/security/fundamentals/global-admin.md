---
title: MFA inschakelen voor alle Azure-beheerders
description: Richt lijnen voor het inschakelen van globale beheerder
ms.service: security
ms.subservice: security-fundamentals
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 702feded450aed7368836c7eff799e969b9f396b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727303"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Multi-factor Authentication (MFA) afdwingen voor abonnements beheerders

Wanneer u uw beheerders maakt, inclusief uw globale beheerders account, is het essentieel dat u zeer krachtige verificatie methoden gebruikt.

U kunt dagelijks beheer uitvoeren door specifieke beheerders rollen, zoals Exchange-beheerder of wachtwoord beheerder, toe te wijzen aan gebruikers accounts van IT-personeel als dat nodig is.
Daarnaast voegt [Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) voor uw beheerders een tweede beveiligingslaag toe aan gebruikers aanmeldingen en trans acties. Azure MFA helpt u ook om de kans te verkleinen dat een aangetast referentie toegang heeft tot de gegevens van de organisatie.

Bijvoorbeeld: U dwingt Azure MFA af voor uw gebruikers en configureert deze om een telefoon gesprek of SMS-bericht als verificatie te gebruiken. Als de referenties van de gebruiker zijn aangetast, kan de aanvaller geen toegang krijgen tot een resource omdat deze geen toegang heeft tot de telefoon van de gebruiker. Organisaties die geen extra lagen van identiteits beveiliging toevoegen, zijn gevoeliger voor aanvallen met een referentie diefstal, wat kan leiden tot inbreuk op de gegevens.

Een alternatief voor organisaties die het volledige verificatie beheer on-premises willen houden, is het gebruik van [Azure multi-factor Authentication-Server](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), ook wel ' MFA op locatie ' genoemd. Met deze methode kunt u de multi-factor Authentication nog steeds afdwingen, terwijl de MFA-server on-premises wordt bewaard.

Als u wilt controleren wie in uw organisatie beheerders bevoegdheden heeft, kunt u deze controleren met behulp van de volgende Microsoft Azure AD v2 Power shell-opdracht:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>MFA inschakelen

Bekijk hoe [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) werkt voordat u verdergaat.

Als uw gebruikers licenties hebben die Azure Multi-Factor Authentication bevatten, hoeft u niets te doen om Azure MFA in te schakelen. U kunt starten met het vereisen van verificatie in twee stappen voor afzonderlijke gebruikers. De licenties die Azure MFA inschakelen zijn:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Verificatie in twee stappen inschakelen voor gebruikers

Gebruik een van de procedures in de [procedure voor verificatie in twee stappen vereisen](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor een gebruiker of groep om te beginnen met Azure MFA. U kunt ervoor kiezen om verificatie in twee stappen af te dwingen voor alle aanmeldingen, of u kunt beleids regels voor voorwaardelijke toegang maken om verificatie in twee stappen alleen in te stellen wanneer het voor u belang rijk is.


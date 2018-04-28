---
title: MFA inschakelen voor alle Azure-administrators
description: Richtlijnen voor het inschakelen van globale beheerder
ms.service: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: a7a08c54fe0c59e1e100e1c46e7a640da0692077
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Afdwingen van multi-factor authentication (MFA) voor abonnementbeheerders

Bij het maken van uw beheerders, met inbegrip van uw globale beheerdersaccount is het essentieel dat u zeer sterk verificatiemethoden.

U kunt dagelijkse beheer uitvoeren door specifieke beheerdersrollen toewijzen, zoals Exchange-beheerder of wachtwoordbeheerder — aan gebruikersaccounts van IT-personeel, indien nodig.
Bovendien inschakelen [Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) voor uw beheerders een tweede beveiligingslaag aan gebruikersaanmeldingen en transacties toegevoegd. Azure MFA ook helpt IT verminderen de kans dat een verdachte referenties toegang tot gegevens van de organisatie hebben.

Bijvoorbeeld: u Azure MFA afdwingen voor uw gebruikers en configureer het om een telefoongesprek of tekstbericht gebruiken als verificatie. Als de referenties van de gebruiker verdacht zijn, kunnen de kwaadwillende persoon toegang tot alle bronnen omdat hij geen toegang tot het telefoonnummer van gebruiker niet mogelijk. Organisaties die Voeg geen extra beveiligingslagen identiteit zijn vatbaarder zijn voor de aanval diefstal van referenties, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die u behouden van de gehele verificatie besturingselement wilt on-premises is [Azure multi-factor Authentication-Server](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), ook wel 'MFA on-premises' genoemd. Met deze methode kunt zich u nog steeds multi-factor authentication, terwijl de MFA-server on-premises afdwingen.

Om te controleren die in uw organisatie beheerdersrechten die u controleren kunt met behulp van de volgende V2 voor Microsoft Azure AD PowerShell-opdracht:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>MFA inschakelen

Bekijk hoe [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) werkt voordat u doorgaat.

Als uw gebruikers licenties hebben die Azure Multi-Factor Authentication bevatten, hoeft u niets te doen om Azure MFA in te schakelen. U kunt starten met het vereisen van verificatie in twee stappen voor afzonderlijke gebruikers. De licenties die Azure MFA inschakelen zijn:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Verificatie in twee stappen inschakelen voor gebruikers

Gebruik een van de procedures die worden vermeld in [vereisen verificatie in twee stappen](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor een gebruiker of groep aan de slag met Azure MFA. U kunt ervoor kiezen om verificatie in twee stappen af te dwingen voor alle aanmeldingen of u kunt een beleid voor voorwaardelijke toegang maken om verificatie in twee stappen alleen te vereisen wanneer dit voor u belangrijk is.


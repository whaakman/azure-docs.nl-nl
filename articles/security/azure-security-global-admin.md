---
title: MFA inschakelen voor alle Azure-beheerders
description: Richtlijnen voor het inschakelen van globale beheerder
ms.service: security
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 2dcd7f42d86000dd3b642c10f6d3db0b0d0fcb03
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116334"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Afdwingen van multi-factor authentication (MFA) voor alle abonnementsbeheerders

Bij het maken van uw beheerders, met inbegrip van uw globale beheerdersaccount, is het essentieel dat u sterk verificatiemethoden gebruiken.

U kunt dagelijkse beheer uitvoeren door specifieke beheerdersrollen toewijzen, zoals Exchange-beheerder of wachtwoordbeheerder: aan gebruikersaccounts van de IT-personeel indien nodig.
Bovendien inschakelen [Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) voor uw beheerders voegt u een tweede beveiligingslaag toe aan gebruikersaanmeldingen en transacties. Azure MFA helpt ook bij IT de kans dat een verdachte referenties toegang tot gegevens van de organisatie hebben.

Bijvoorbeeld: u Azure MFA afdwingen voor uw gebruikers en configureer deze voor het gebruik van een telefoongesprek of SMS-bericht als verificatie. Als de referenties van de gebruiker zijn aangetast, kunnen de aanvaller heeft toegang tot alle bronnen omdat hij geen toegang tot het telefoonnummer van gebruiker niet mogelijk. Organisaties die geen extra lagen van identiteitsbeveiliging toevoegen zijn vatbaar voor referentie diefstal aanval, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die de volledige verificatieset besturingselement willen on-premises houden is het gebruik van [Azure multi-factor Authentication-Server](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), ook wel 'MFA on-premises' genoemd. Met deze methode, zich kunt u nog steeds multi-factor authentication, terwijl de MFA server on-premises afdwingen.

Om te controleren wie in uw organisatie heeft de beheerdersbevoegdheden die u controleren kunt met behulp van de Microsoft Azure AD V2 PowerShell-opdracht:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>MFA inschakelen

Lees hoe [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) werkt voordat u doorgaat.

Als uw gebruikers licenties hebben die Azure Multi-Factor Authentication bevatten, hoeft u niets te doen om Azure MFA in te schakelen. U kunt starten met het vereisen van verificatie in twee stappen voor afzonderlijke gebruikers. De licenties die Azure MFA inschakelen zijn:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Verificatie in twee stappen inschakelen voor gebruikers

Gebruik een van de procedures in [hoe u verificatie in twee stappen vereist](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) voor een gebruiker of groep aan de slag met Azure MFA. U kunt ervoor kiezen om verificatie in twee stappen af te dwingen voor alle aanmeldingen of u kunt een beleid voor voorwaardelijke toegang maken om verificatie in twee stappen alleen te vereisen wanneer dit voor u belangrijk is.


---
title: Privileged Identity Management voor Azure-Resources - MFA | Microsoft Docs
description: Dit document beschrijft het inschakelen van multi-factor authentication voor PIM-resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8d1c05e7f61ed76c47613bfab7bb8afd9b66cbe7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---mfa"></a>Privileged Identity Management - resourcerollen - MFA

PIM voor Azure-resource-functies kunt resource-beheerders en beheerders van de identiteit ter bescherming van essentiële Azure-infrastructuur met tijdsgebonden lidmaatschap en just-in-time-toegang. Daarnaast biedt PIM optionele afdwinging van Azure multi-factor Authentication (MFA) voor twee verschillende scenario's.

## <a name="require-mfa-to-activate"></a>Vereis MFA voor activeren

Resource-beheerders kunnen vereisen in aanmerking komende leden van een rol slagen Azure MFA voordat ze kunnen worden geactiveerd. Dit proces zorgt ervoor dat de aanvragende gebruiker-activering is wie hij of zij beweren te zijn met redelijke zekerheid. Afdwingen van deze optie worden beveiligd op kritieke bronnen in situaties wanneer het gebruikersaccount waarmee is geknoeid. 

Selecteer een resource in de lijst met beheerde bronnen voor het afdwingen van deze vereiste. Van de [Overzichtsdashboard](pim-resource-roles-overview-dashboards.md), selecteer een rol in de lijst met rollen in de rechterbenedenhoek van het scherm.

Daarnaast krijgt u functie-instellingen van beide de 'functies' of 'rolinstellingen' tabbladen in het navigatiemenu links.

>[!Note]
>Als de opties in het navigatiemenu links zijn niet beschikbaar en u een banner ziet aan de bovenkant van de pagina 'You have in aanmerking komende rollen die kunnen worden geactiveerd' weergegeven dat u geen actieve beheerder bent en moet [activeren](pim-resource-roles-activate-your-roles.md) voordat u doorgaat.

![](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Als lidmaatschap van een rol weergeven, selecteert u 'Instellingen van de rol' van de balk aan de bovenkant van het scherm wordt de 'rol instelling detail' openen.

Klik op de **bewerken** bovenaan op de knop om de functie-instellingen te wijzigen.

In de sectie onder **activeren**, klikt u op het selectievakje **multi-factor Authentication vereisen voor het activeren van**, klik op opslaan.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-mfa-on-assignment"></a>MFA vereisen voor toewijzing

In sommige gevallen kan kan de resourcebeheerder van een wilt toewijzen van een lid aan een rol gedurende een korte periode (één dag voor voorbeeld) en hoeven niet de toegewezen leden activering kunnen aanvragen. In dit scenario kan geen PIM MFA afdwingen als het lid hun roltoewijzing gebruikt, omdat ze al actief is in de rol vanaf het moment dat ze worden toegewezen.

Om te controleren of de resourcebeheerder die voldoen aan de toewijzing is wie hij of zij beweren te zijn, kunt u MFA afdwingen voor toewijzing.

Schakel het selectievakje ' Verplicht stellen van multi-factor Authentication voor toewijzing ' uit hetzelfde rol instelling details scherm.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Volgende stappen

[Goedkeuring vereist voor het activeren](pim-resource-roles-approval-workflow.md)

[Het controlelogboek gebruiken](pim-resource-roles-use-the-audit-log.md)




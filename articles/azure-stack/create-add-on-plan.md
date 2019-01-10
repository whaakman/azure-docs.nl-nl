---
title: In dit artikel leert u hoe u Azure Stack-aanbiedingen en plannen bijwerken | Microsoft Docs
description: In dit artikel wordt beschreven hoe u weergeven en wijzigen van bestaande Azure Stack-aanbiedingen en plannen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 0a7cf008dd2d2656153d7f1d70f71b9832be5d81
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157799"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack aanvullende plannen

Als u Azure Stack-operators, u aanvullende plannen om te wijzigen maken een [basisplan](azure-stack-create-plan.md) indien u wenst te bieden extra services of uit te breiden *computer*, *opslag*, of *netwerk* quota na de initiële basisplan-aanbieding. Aanvullende plannen wijzigen van het basisplan en zijn optionele uitbreidingen die gebruikers kiezen kunnen voor een abonnement op.

Er zijn tijden wanneer alles combineren in één abonnement optimaal is. In andere gevallen kunt u hebt een base plannen en vervolgens de aanvullende services bieden met behulp van aanvullende plannen. U wilt bijvoorbeeld kan aanbieden van IaaS-services als onderdeel van een basis-abonnement, met alle PaaS-services behandeld als aanvullende plannen.

Er is een andere reden voor het gebruik van aanvullende plannen om gebruikers Houd ook rekening met het gebruik van hun bronnen te helpen. Om dit te doen, kunt u beginnen met een basis-abonnement met relatief klein aantal quota (afhankelijk van de services die nodig zijn). Klik, als gebruikers capaciteit bereiken, ze zou worden gewaarschuwd dat ze de toewijzing van resources op basis van hun toegewezen planning hebt verbruikt. Van daaruit kunnen de gebruikers een aanvullende plan waarmee de extra resources selecteren.

> [!NOTE]
> Wanneer u niet wilt gebruiken een aanvullende plan uit te breiden van een quotum, kunt u ook kiezen voor [bewerken van de oorspronkelijke configuratie van de quota voor](azure-stack-quota-types.md#edit-a-quota).

Als een aanvullende plan een gebruiker aan een bestaand abonnement voor de aanbieding toevoegt, kunnen de extra resources duren tot een uur worden weergegeven.

## <a name="create-an-add-on-plan"></a>Een invoegtoepassing maken

Aanvullende plannen worden gemaakt door het wijzigen van een bestaande aanbieding:

1. Meld u aan de portal van Azure Stack-beheerder als de beheerder van een cloud.
2. Volg dezelfde stappen gebruikt voor het [maken van een nieuwe basisplan](azure-stack-create-plan.md) te maken van een nieuw abonnement met services die zijn niet eerder aangeboden. In dit voorbeeld Key Vault (**Microsoft.KeyVault**) services worden opgenomen in het nieuwe abonnement.
3. Klik in de beheerdersportal **biedt** en selecteer vervolgens de aanbieding moet worden bijgewerkt met een aanvullende-plan.

   ![Aanvullende plan maken](media/create-add-on-plan/1.PNG)

4. Ga naar de onderkant van de eigenschappen van de aanbieding en selecteer **aanvullende plannen**. Klik op **Toevoegen**.

    ![Aanvullende plan maken](media/create-add-on-plan/2.PNG)

5. Selecteer het abonnement toe te voegen. In dit voorbeeld wordt het abonnement heet **Key vault-abonnement**. Selecteer het abonnement en klik op **Selecteer** om toe te voegen van het abonnement op de aanbieding. U ontvangt een melding dat het plan is toegevoegd aan de aanbieding.

    ![Aanvullende plan maken](media/create-add-on-plan/3.PNG)

6. Bekijk de lijst met aanvullende plannen die zijn opgenomen in de aanbieding om te verifiëren dat het nieuwe abonnement van de invoegtoepassing wordt vermeld.

    ![Aanvullende plan maken](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Volgende stappen

* [Een aanbieding maken](azure-stack-create-offer.md)
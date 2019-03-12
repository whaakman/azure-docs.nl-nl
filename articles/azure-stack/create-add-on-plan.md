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
ms.topic: conceptual
ms.custom: mvc
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 00bb17eadfee32e9b0d006ac76bb8e1cd614f13e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780385"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack aanvullende plannen

Als u Azure Stack-operators, u aanvullende plannen om te wijzigen maken een [basisplan](azure-stack-create-plan.md) indien u wenst te bieden extra services of uit te breiden *computer*, *opslag*, of *netwerk* quota na de initiële basisplan-aanbieding. Aanvullende plannen wijzigen van het basisplan en zijn optionele uitbreidingen die gebruikers kiezen kunnen voor een abonnement op.

Er zijn tijden wanneer alles combineren in één abonnement optimaal is. In andere gevallen kunt u hebt een base plannen en vervolgens de aanvullende services bieden met behulp van aanvullende plannen. U wilt bijvoorbeeld kan aanbieden van IaaS-services als onderdeel van een basis-abonnement, met alle PaaS-services behandeld als aanvullende plannen.

Een andere reden voor het gebruik van aanvullende plannen is voor de monitor Resourcegebruik. Om dit te doen, kunt u beginnen met een basis-abonnement met relatief klein aantal quota (afhankelijk van de services die nodig zijn). Klik, als gebruikers capaciteit bereiken, ze zou worden gewaarschuwd dat ze de toewijzing van resources op basis van hun toegewezen planning hebt verbruikt. Van daaruit kunnen de gebruikers een aanvullende plan waarmee de extra resources selecteren.

> [!NOTE]
> Wanneer u niet wilt gebruiken een aanvullende plan uit te breiden van een quotum, kunt u ook kiezen voor [bewerken van de oorspronkelijke configuratie van de quota voor](azure-stack-quota-types.md#edit-a-quota).

Wanneer u een aanvullende plan aan een bestaand abonnement voor de aanbieding toevoegt, de extra resources kunnen een uur duren om te worden weergegeven.

Aanvullende plannen worden gemaakt door het wijzigen van een bestaande aanbieding.

## <a name="create-an-add-on-plan-1902-and-later"></a>Een invoegtoepassing maken (1902 en hoger)

1. Meld u aan de portal van Azure Stack-beheerder als de beheerder van een cloud.
2. Volg dezelfde stappen gebruikt voor het [maken van een nieuwe basisplan](azure-stack-create-plan.md) te maken van een nieuw abonnement met services die zijn niet eerder aangeboden.
3. Klik in de beheerdersportal **biedt** en selecteer vervolgens de aanbieding moet worden bijgewerkt met een aanvullende-plan.

   ![Aanvullende plan maken](media/create-add-on-plan/add-on1.png)

4. Ga naar de onderkant van de eigenschappen van de aanbieding en selecteer **aanvullende plannen**. Klik op **Add**.

    ![Aanvullende plan maken](media/create-add-on-plan/add-on2.png)

5. Selecteer het abonnement toe te voegen. In dit voorbeeld wordt het abonnement heet **20 storageaccounts**. Selecteer het abonnement en klik op **Selecteer** om toe te voegen van het abonnement op de aanbieding. U ontvangt een melding dat het plan is toegevoegd aan de aanbieding.

    ![Aanvullende plan maken](media/create-add-on-plan/add-on3.png)

6. Bekijk de lijst met aanvullende plannen die zijn opgenomen in de aanbieding om te verifiëren dat het nieuwe abonnement van de invoegtoepassing wordt vermeld.

    [![Plan maken-invoegtoepassing](media/create-add-on-plan/add-on4.png "plan van de invoegtoepassing maken")](media/create-add-on-plan/add-on4lg.png#lightbox)

## <a name="create-an-add-on-plan-1901-and-earlier"></a>Een invoegtoepassing maken (1901 en eerder)

1. Meld u aan de portal van Azure Stack-beheerder als de beheerder van een cloud.
2. Volg dezelfde stappen gebruikt voor het [maken van een nieuwe basisplan](azure-stack-create-plan.md) te maken van een nieuw abonnement met services die zijn niet eerder aangeboden. In dit voorbeeld Key Vault (**Microsoft.KeyVault**) services worden opgenomen in het nieuwe abonnement.
3. Klik in de beheerdersportal **biedt** en selecteer vervolgens de aanbieding moet worden bijgewerkt met een aanvullende-plan.

   ![Aanvullende plan maken](media/create-add-on-plan/1.PNG)

4. Ga naar de onderkant van de eigenschappen van de aanbieding en selecteer **aanvullende plannen**. Klik op **Add**.

    ![Aanvullende plan maken](media/create-add-on-plan/2.PNG)

5. Selecteer het abonnement toe te voegen. In dit voorbeeld wordt het abonnement heet **Key vault-abonnement**. Selecteer het abonnement en klik op **Selecteer** om toe te voegen van het abonnement op de aanbieding. U ontvangt een melding dat het plan is toegevoegd aan de aanbieding.

    ![Aanvullende plan maken](media/create-add-on-plan/3.PNG)

6. Bekijk de lijst met aanvullende plannen die zijn opgenomen in de aanbieding om te verifiëren dat het nieuwe abonnement van de invoegtoepassing wordt vermeld.

    ![Aanvullende plan maken](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Volgende stappen

* [Een aanbieding maken](azure-stack-create-offer.md)
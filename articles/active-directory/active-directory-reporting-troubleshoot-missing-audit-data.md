---
title: 'Probleemoplossing: Ontbrekende gegevens in activiteitenlogboeken van Azure Active Directory | Microsoft Docs'
description: Bevat een lijst met de diverse beschikbare rapporten voor Azure Active Directory
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 55587fb4ff6d8a2130eb838782a65788fb2dd2b3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>Ik kan een aantal acties die ik heb uitgevoerd, niet terugvinden in de activiteitenlogboeken van Azure Active Directory


## <a name="symptoms"></a>Symptomen

Ik heb enkele acties uitgevoerd in Azure Portal en had verwacht de auditlogboeken voor deze acties te zien op de blade `Activity logs > Audit Logs`, maar ik kan ze niet vinden.

 ![Rapportage](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Oorzaak

Acties worden niet direct weergegeven in het auditlogboek voor activiteiten. Vanaf het moment dat de bewerking is uitgevoerd, kan het tussen 15 minuten tot een uur duren voordat de auditlogboeken beschikbaar zijn in de portal.

## <a name="resolution"></a>Oplossing

Wacht 15 minuten tot een uur en kijk of de acties nu wel worden vermeld in het logboek. Als u ze nog steeds niet ziet, kunt u een ondersteuningsaanvraag indien en dan zullen we ernaar kijken.


## <a name="next-steps"></a>Volgende stappen
Zie [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md) (Veelgestelde vragen over Azure Active Directory-rapportage).


---
title: 'Probleemoplossing: Ontbrekende gegevens in activiteitenlogboeken van Azure Active Directory - preview | Microsoft Docs'
description: Bevat een lijst met de diverse beschikbare rapporten voor Azure Active Directory-previews
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/09/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: c372fe5f3a419a6a27ef00d755d5d46325b956c6
ms.lasthandoff: 04/06/2017


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



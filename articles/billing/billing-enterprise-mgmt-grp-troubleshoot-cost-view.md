---
title: Problemen met Azure enterprise kosten weergaven oplossen
description: Informatie over het oplossen van problemen die u mogelijk met organisatie-kosten weergaven in de Azure-portal.
author: bandersmsft
manager: amberb
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 83f7f424b265582a3830c02973cbbb9962ddfbfb
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491266"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Weergave voor bedrijfskosten oplossen

Er zijn verschillende instellingen die kunnen ertoe leiden dat gebruikers binnen de inschrijving niet ziet kosten in enterprise-inschrijvingen.  Deze instellingen worden beheerd door de beheerder voor inschrijving. Of, als de inschrijving wordt niet rechtstreeks via Microsoft gekocht, de instellingen worden beheerd door de partner.  In dit artikel helpt u begrijpen wat de instellingen zijn en hoe ze gevolgen hebben voor de inschrijving. Deze instellingen zijn onafhankelijk van de functies van Azure Role-based access control (RBAC).

## <a name="enable-access-to-costs"></a>Toegang tot kosten inschakelen

Weet u ziet een bericht niet gemachtigd, of *"kosten weergaven zijn uitgeschakeld in uw inschrijving."* Wanneer u op zoek naar kostengegevens?
![Schermopname die laat 'onbevoegd' in het veld van de huidige kosten voor het abonnement zien.](media/billing-enterprise-mgmt-groups/unauthorized.png)

Het is mogelijk om een van de volgende redenen:

1. U kunt Azure hebt gekocht via een enterprise-partner en de partner niet vrijgeven prijzen nog. Neem contact op met uw partner voor het bijwerken van de prijzen instellen binnen de [Enterprise portal](https://ea.azure.com).
2. Als u een EA Direct-klant bent, zijn er verschillende mogelijkheden:
    * U bent eigenaar van een Account en de beheerder voor uw inschrijving uitgeschakeld de **kosten weergeven die door de AO** instelling.  
    * U bent een beheerder van een afdeling en de beheerder voor uw inschrijving uitgeschakeld de **DA weergave kosten** instelling.
    * Neem contact op met de beheerder van uw registratie om toegang te krijgen. De registratie-beheerder kunt bijwerken de instellingen in de [Enterprise portal](https://ea.azure.com/manage/enrollment).

      ![Schermafbeelding van de Enterprise Portal-instellingen voor kosten weergeven.](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Asset is niet beschikbaar

Als er een foutbericht dat aangeeft **deze asset is niet beschikbaar** proberen te krijgen tot een abonnement of beheergroep, hoeft u niet de juiste rol om dit item weer te geven.  

![Schermopname die laat 'asset is niet beschikbaar' wordt weergegeven zien.](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Vraag uw Azure-abonnement of management groep beheerder om toegang te krijgen. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen
- Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

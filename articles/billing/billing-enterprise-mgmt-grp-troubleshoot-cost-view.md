---
title: Problemen met Azure enterprise kosten weergaven oplossen | Microsoft Docs
description: Informatie over het oplossen van problemen die u mogelijk met organisatie-kosten weergaven in de Azure-portal.
author: rthorn17
manager: adpick
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2017
ms.author: cwatson
ms.custom: seodec18
ms.openlocfilehash: 434f717d9590eff39df161a74931477bdf71bb67
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309563"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Weergave voor bedrijfskosten oplossen

Er zijn verschillende instellingen die kunnen ertoe leiden dat gebruikers binnen de inschrijving niet ziet kosten in enterprise-inschrijvingen.  Deze instellingen worden beheerd door de beheerder voor inschrijving. Of, als de inschrijving wordt niet rechtstreeks via Microsoft gekocht, de instellingen worden beheerd door de partner.  In dit artikel helpt u begrijpen wat de instellingen zijn en hoe ze gevolgen hebben voor de inschrijving. Deze instellingen zijn onafhankelijk van de functies van Azure Role-based access control (RBAC).

## <a name="enabling-access-to-costs"></a>Voor toegang tot kosten

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

Als u een foutmelding krijgt 'deze asset is niet beschikbaar' wanneer probeert te krijgen tot een abonnement of management groep, dan hebt u niet de juiste rol hebben om dit item weer te geven.  

![Schermopname die laat 'asset is niet beschikbaar' wordt weergegeven zien.](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Vraag uw Azure-abonnement of management groep beheerder om toegang te krijgen. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

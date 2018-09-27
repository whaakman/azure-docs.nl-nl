---
title: Problemen met Azure enterprise kosten weergaven oplossen | Microsoft Docs
description: Informatie over het oplossen van problemen die u mogelijk met organisatie-kosten weergaven in de Azure-portal.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2017
ms.author: cwatson
ms.openlocfilehash: 853307f24574e6cea5841bcace815fea2bbcf2c8
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47390661"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Weergave voor bedrijfskosten oplossen 

Er zijn meerdere instellingen die kunnen ertoe leiden dat gebruikers binnen de inschrijving niet mogelijk om kosten weer te geven in de enterprise-inschrijvingen.  Deze instellingen worden beheerd door de beheerder voor inschrijving, of door de partner als de inschrijving is niet rechtstreeks met Microsoft hebt aangeschaft.  In dit artikel helpt u begrijpen wat de instellingen zijn en hoe ze gevolgen hebben voor de inschrijving. Deze instellingen zijn onafhankelijk van de [Azure RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). 


## <a name="enabling-access-to-costs"></a>Voor toegang tot kosten

Weet u ziet een bericht niet gemachtigd, of *"kosten weergaven zijn uitgeschakeld in uw inschrijving."* Wanneer u op zoek naar kostengegevens? ![niet-geautoriseerde](media/billing-enterprise-mgmt-groups/unauthorized.png)

Dit kan worden veroorzaakt door een van de volgende redenen:

1. U kunt Azure hebt aangeschaft via een enterprise-partner en de partner die nog niet zijn uitgebracht nog prijzen. Als u wilt vrijgeven prijzen, neem contact op met uw partner voor werk de instelling in de [Enterprise portal](https://ea.azure.com).
2. U kunt ook als u een EA Direct-klant bent, zijn er een aantal mogelijkheden:
    * U bent eigenaar van een Account en de Inschrijvingsbeheerder heeft de 'door de AO kosten weergeven' instelling uitgeschakeld.  
    * U bent een beheerder van een afdeling en de Inschrijvingsbeheerder heeft het "DA kosten weergeven' instellen uitgeschakeld.
    * Neem contact op met de beheerder van uw registratie om toegang te krijgen. Kan de beheerder van de inschrijving gaat u naar de [Enterprise portal](https://ea.azure.com/manage/enrollment) en werk de instelling zoals hier wordt weergegeven:

![Enterprise Portal-instellingen](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Asset is niet beschikbaar? 
Als u een foutbericht ontvangt "deze asset is niet beschikbaar" wanneer het openen van een abonnement of management groep, dan hebt u hoeft niet de juiste rol om dit item weer te geven.  

![asset-not-found](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Neem contact op met het beheren van het abonnement of beheergroep groepen die toegang moet krijgen.  
* Raadpleeg voor abonnementen, [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) document voor meer informatie over waarop de rol is vereist.

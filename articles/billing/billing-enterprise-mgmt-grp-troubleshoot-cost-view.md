---
title: Enterprise kosten weergaven - Azure oplossen | Microsoft Docs
description: Informatie over het oplossen van problemen die u met organisatie-kosten weergaven in de Azure-portal wellicht.
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: acdcf5b772cbaca1827663b49d5cc8cafa238108
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-enterprise-cost-views"></a>Enterprise kosten weergaven oplossen 

Er zijn meerdere instellingen waardoor gebruikers binnen de inschrijving niet om te kunnen kosten weergeven in de enterprise-inschrijvingen.  Deze instellingen worden beheerd door de beheerder van de inschrijving of door de partner als de registratie is niet rechtstreeks met Microsoft hebt aangeschaft.  In dit artikel helpt u begrijpen wat de instellingen zijn en hoe ze invloed hebben op de inschrijving. Deze instellingen zijn onafhankelijk van de [Azure RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). 

> [!Note]
> Deze functie is momenteel in een private preview. [Hier aanmelden](https://forms.office.com/Pages/DesignPage.aspx#FormId=v4j5cvGGr0GRqy180BHbR0YtfU6ham9OsGsPPYdu2xdUNk1BQUwzTkUyOVc5NUpCTFcwR0pIOVFETS4u) uw inschrijving voor het koppelen van de Preview-versie hebben.     

## <a name="enabling-access-to-costs"></a>Toegang tot kosten inschakelen

U ziet een bericht niet geautoriseerd, zijn of *"kosten weergaven zijn uitgeschakeld in uw registratie."* bij het zoeken naar kostengegevens? ![onbevoegde](media/billing-enterprise-mgmt-groups/unauthorized.png)

Dit wordt mogelijk veroorzaakt door een van de volgende redenen:

1. U kunt Azure hebt aangeschaft via een enterprise-partner en de partner nog niet vrijgegeven nog prijzen. Als u wilt vrijgeven prijzen, neem contact op met uw partner voor werk de instelling in de [Enterprise portal](https://ea.azure.com).
2. U kunt ook als u een directe EA-klant bent, zijn er enkele mogelijkheden:
    * U bent de eigenaar van een Account en de 'AO kosten weergeven' instellen door de beheerder van de inschrijving is uitgeschakeld.  
    * U afdeling beheerder bent en uw registratie-beheerder heeft de 'DA kosten weergeven' instelling uitgeschakeld.
    * Neem contact op met uw beheerder inschrijving om toegang te krijgen. De beheerder van de inschrijving vindt de [Enterprise portal](https://ea.azure.com/manage/enrollment) en werk de instelling zoals hier:

![Enterprise Portal-instellingen](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Asset is niet beschikbaar? 
Als u een foutbericht ontvangt 'dit activum is niet beschikbaar' wanneer een abonnement of management groep, dan hebt u toegang wilt tot hoeft niet de juiste rol voor dit item weergeven.  

![Asset niet is gevonden](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Neem contact op met het beheren van het abonnement of management groepen om toegang te krijgen.  
* Voor abonnementen, verwijzen naar [gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) document voor help waarop de rol is vereist.
* Voor-beheergroepen RBAC toegang is niet beschikbaar en is binnenkort beschikbaar. Neem contact op met uw enterprise portal beheren voor toegang is toegewezen.   

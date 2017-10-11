---
title: De status van Azure CDN resources bewaken | Microsoft Docs
description: Informatie over het controleren van de status van uw Azure CDN-resources met behulp van de status van de Azure-resources.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 37fe208f5087f318e665e76825127854b4a11c98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a>De status van Azure CDN resources bewaken
  
Azure CDN resourcestatus is een subset van [Azure resourcestatus](../resource-health/resource-health-overview.md).  Azure-resource health kunt u de status van resources CDN controleren en ontvangen van bruikbare richtlijnen voor het oplossen van problemen.

>[!IMPORTANT] 
>Azure CDN-resourcestatus accounts alleen momenteel de status van globale levering van CDN en API-mogelijkheden.  Azure CDN-resourcestatus controleert niet of afzonderlijke CDN-eindpunten.
>
>De signalen die Azure CDN resourcestatus feed mogelijk maximaal 15 minuten vertraagd.

## <a name="how-to-find-azure-cdn-resource-health"></a>Het zoeken van Azure CDN-resourcestatus

1. In de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel.

2. Klik op de **instellingen** knop.

    ![Knop Instellingen](./media/cdn-resource-health/cdn-profile-settings.png)

3. Onder *ondersteuning + probleemoplossing*, klikt u op **resourcestatus**.

    ![CDN-resourcestatus](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>U vindt ook CDN bronnen in de *resourcestatus* -tegel in de *Help + ondersteuning* blade.  U snel toegang krijgen tot *Help + ondersteuning* door te klikken op de cirkel **?** in de rechterbovenhoek van de portal.
>
> ![Help en ondersteuning](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN-specifieke berichten

Hieronder vindt u statussen die betrekking hebben op Azure CDN resourcestatus.

|Bericht | Aanbevolen actie |
|---|---|
|U kunt hebt gestopt, verwijderd of onjuist geconfigureerd een of meer van uw CDN-eindpunten | U kunt hebt gestopt, verwijderd of onjuist geconfigureerd een of meer van uw CDN-eindpunten.|
|Beschikbaar is, de CDN-management-service is momenteel niet beschikbaar | Controleer hier terug voor statusupdates; Neem contact op met de ondersteuning als het probleem zich blijft nadat de verwachte oplossingstijd voordoen.|
|Helaas is dat uw CDN-eindpunten wordt mogelijk beïnvloed door een voortdurende problemen met enkele van onze CDN-providers | Controleer hier terug voor statusupdates; Gebruik het hulpprogramma problemen oplossen voor informatie over het testen van uw oorsprong en CDN-eindpunt; Neem contact op met de ondersteuning als het probleem zich blijft nadat de verwachte oplossingstijd voordoen. |
|Helaas CDN-eindpunt configuratiewijzigingen ondervinden vertragingen bij het doorgeven | Controleer hier terug voor statusupdates; Als u uw wijzigingen in de configuratie niet volledig binnen de verwachte tijd worden overgebracht, moet u contact op met ondersteuning.|
|We vinden het jammer, dat er zijn momenteel problemen bij het laden van de aanvullende portal | Controleer hier terug voor statusupdates; Neem contact op met de ondersteuning als het probleem zich blijft nadat de verwachte oplossingstijd voordoen.|
Helaas, dat we hebben momenteel problemen met enkele van onze CDN-providers | Controleer hier terug voor statusupdates; Neem contact op met de ondersteuning als het probleem zich blijft nadat de verwachte oplossingstijd voordoen. |

## <a name="next-steps"></a>Volgende stappen

- [Een overzicht van Azure resourcestatus lezen](../resource-health/resource-health-overview.md)
- [Problemen oplossen met CDN compressie](./cdn-troubleshoot-compression.md)
- [Problemen oplossen met 404-fouten](./cdn-troubleshoot-endpoint.md)
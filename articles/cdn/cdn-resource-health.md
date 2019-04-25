---
title: Controleer de status van resources voor Azure CDN | Microsoft Docs
description: Leer hoe u de status van uw Azure CDN-resources met behulp van Azure Resource Health worden gecontroleerd.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: ad4bf7ae97a08f89b9d82e1d4e025a5bd5d47fc1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324667"
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a>Controleer de status van Azure CDN-resources
  
Azure CDN-Resource health is een subset van [Azure resource health](../resource-health/resource-health-overview.md).  Azure resource health kunt u de status van CDN-resources bewaken en ontvang praktische richtlijnen voor het oplossen van problemen.

>[!IMPORTANT] 
>Azure CDN-resourcestatus accounts alleen momenteel de status van wereldwijde CDN-levering en API-functies.  Azure CDN-resourcestatus controleert niet of afzonderlijke CDN-eindpunten.
>
>De signalen die feed resourcestatus Azure CDN kunnen tot 15 minuten vertraagd zijn.

## <a name="how-to-find-azure-cdn-resource-health"></a>Over het vinden van Azure CDN-resourcestatus

1. In de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel.

2. Klik op de **instellingen** knop.

    ![Knop Instellingen](./media/cdn-resource-health/cdn-profile-settings.png)

3. Onder *ondersteuning en probleemoplossing*, klikt u op **resourcestatus**.

    ![CDN-resourcestatus](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>U vindt hier ook CDN-resources die worden vermeld de *resourcestatus* tegel de *Help en ondersteuning* blade.  U kunt snel toegang verkrijgen tot *Help en ondersteuning* door te klikken op de cirkel **?** in de rechterbovenhoek van de portal.
>
> ![Help en ondersteuning](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN-specifieke berichten

Status met betrekking tot Azure CDN resource health vindt u hieronder.

|Bericht | Aanbevolen actie |
|---|---|
|Een of meer CDN-eindpunten zijn mogelijk gestopt, verwijderd of verkeerd geconfigureerd | Een of meer CDN-eindpunten zijn mogelijk gestopt, verwijderd of verkeerd geconfigureerd.|
|De CDN-beheerservice is momenteel niet beschikbaar | Kom hier terug voor statusupdates; Als het probleem zich blijft na de tijd dat het probleem zou moeten voordoen contact opnemen met ondersteuning.|
|Uw CDN-eindpunten worden mogelijk getroffen door lopende problemen met een aantal van onze CDN-providers | Kom hier terug voor statusupdates; Gebruik het hulpprogramma problemen oplossen voor informatie over het testen van uw oorsprong en CDN-eindpunt; Als het probleem zich blijft na de tijd dat het probleem zou moeten voordoen contact opnemen met ondersteuning. |
|De wijzigingen in de configuratie van uw CDN-eindpunten ondervinden helaas vertragingen bij de doorgifte | Kom hier terug voor statusupdates; Als uw configuratiewijzigingen niet volledig worden doorgegeven in de verwachte tijd, contact op met ondersteuning.|
|Er zijn momenteel problemen met het laden van de aanvullende portal | Kom hier terug voor statusupdates; Als het probleem zich blijft na de tijd dat het probleem zou moeten voordoen contact opnemen met ondersteuning.|
Er zijn momenteel problemen met een aantal van onze CDN-providers | Kom hier terug voor statusupdates; Als het probleem zich blijft na de tijd dat het probleem zou moeten voordoen contact opnemen met ondersteuning. |

## <a name="next-steps"></a>Volgende stappen

- [Lees een overzicht van Azure resource health](../resource-health/resource-health-overview.md)
- [Problemen oplossen met CDN-compressie](./cdn-troubleshoot-compression.md)
- [Oplossen van problemen met 404-fouten](./cdn-troubleshoot-endpoint.md)
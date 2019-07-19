---
title: Bot-beveiliging configureren voor Web Application Firewall met Azure front deur (preview)
description: Meer informatie Web Application Firewall (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 0f36ea33badad52c55cd11491874db955df5408b
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846343"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Bot-beveiliging configureren voor Web Application Firewall (preview-versie)
Dit artikel laat u zien hoe u de bot-beveiligings regel in azure Web Application Firewall (WAF) kunt configureren voor de voor deur met behulp van Azure CLI, Azure PowerShell of Azure Resource Manager sjabloon.

Een set Managed bot Protection kan worden ingeschakeld voor uw WAF om aangepaste acties uit te voeren op aanvragen van bekende schadelijke IP-adressen. De IP-adressen worden vanuit de micro soft Threat Intelligence-feed gebrond. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) voorziet in micro soft Threat Intelligence en wordt gebruikt door meerdere services, waaronder Azure Security Center.

> [!IMPORTANT]
> De set bot Protection-regelset bevindt zich momenteel in een open bare preview en wordt weer gegeven met een preview-service level agreement. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben.  Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Maak een basis WAF-beleid voor de voor deur door de instructies te volgen die worden beschreven in [een WAF-beleid maken voor Azure front deur met behulp van de Azure Portal](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Regel set voor bot-beveiliging inschakelen

1. Klik op de pagina basis beleid die u in de voor gaande sectie hebt gemaakt onder **instellingen**op **regels**.
2. Schakel op de pagina Details, onder de sectie **regels beheren** , in de vervolg keuzelijst het selectie vakje in voor de regel **BotProtection-Preview-0,1**en Selecteer hierboven **Opslaan** hierboven.
    
   ![Bot-beveiligings regel](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het bewaken van [WAF](waf-front-door-monitor.md).

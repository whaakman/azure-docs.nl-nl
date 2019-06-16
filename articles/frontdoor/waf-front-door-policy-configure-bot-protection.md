---
title: Bot-beveiliging voor de web application firewall configureren met Azure voordeur (Preview)
description: Meer informatie over de web application firewall (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: af92f3b9049862fc19c69965f979b7dfe8c62526
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481622"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Bot beveiligingsconfiguratie voor de web application firewall (Preview)
Dit artikel laat u de regel protection bot in Azure web application firewall (WAF) voor voordeur configureren met behulp van Azure CLI, Azure PowerShell of Azure Resource Manager-sjabloon.

Een beheerde regelset van de Bot-beveiliging kan worden ingeschakeld voor uw WAF aan aangepaste acties uitvoeren op aanvragen van bekende schadelijke IP-adressen. De IP-adressen zijn afkomstig van de Microsoft-Bedreigingsinformatie-feed. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) wordt gebruikt door bedreigingsinformatie van Microsoft en wordt gebruikt door meerdere services, met inbegrip van Azure Security Center.

> [!IMPORTANT]
> Regelset voor bot-beveiliging is momenteel in openbare preview en is voorzien van een Preview-versie service level agreement. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben.  Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

Een eenvoudige WAF-beleid voor voordeur maken door de instructies die worden beschreven [een WAF-beleid voor de voordeur voor Azure maken met behulp van de Azure-portal](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Regelset voor bot-beveiliging inschakelen

1. In de basic beleidspagina die u in de vorige sectie hebt gemaakt, onder **instellingen**, klikt u op **regels**.
2. Pagina met details, onder de **regels beheren** sectie van de vervolgkeuzelijst, schakel het selectievakje voor de regel **BotProtection-preview-0.1**, en selecteer vervolgens **opslaan**hierboven.
    
   ![Regel voor bot](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [WAF bewaken](waf-front-door-monitor.md).

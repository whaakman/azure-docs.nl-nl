---
title: Een web application firewall-beleid voor de voordeur voor Azure maken met behulp van Azure portal
titlesuffix: Azure web application firewall
description: Informatie over het maken van een web application firewall (WAF)-beleid met behulp van de Azure-portal.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/8/2019
ms.author: kumud;tyao
ms.openlocfilehash: 26db3a67c3efbd0ba2a5c58facd0c07175f7ed12
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564136"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Een WAF-beleid voor de voordeur voor Azure maken met behulp van Azure portal

In dit artikel wordt beschreven hoe u een eenvoudige Azure-web application firewall (WAF)-beleid maken en dit toepassen op een front-host aan de voordeur voor Azure.

## <a name="prerequisites"></a>Vereisten

Een profiel voordeur maken door de instructies die worden beschreven in [Quick Start: Maak een profiel voordeur](quickstart-create-front-door.md). 

## <a name="create-a-waf-policy"></a>Een WAF-beleid maken

Maak eerst een eenvoudige WAF-beleid met beheerde standaard regel ingesteld (DRS) met behulp van de portal. 

1. Selecteer op de bovenste linkerkant van het scherm, **een resource maken**> zoeken naar **WAF**> Selecteer **Web application firewall (Preview)** > Selecteer  **Maak**.
2. In de **basisbeginselen** tabblad van de **een WAF-beleid maken** pagina, invoeren of Selecteer de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **revisie + maken**:

    | Instelling                 | Value                                              |
    | ---                     | ---                                                |
    | Abonnement            |Selecteer de naam van uw voordeur-abonnement.|
    | Resourcegroep          |Selecteer de voordeur Resourcegroepnaam.|
    | Beleidsnaam             |Voer een unieke naam voor uw WAF-beleid.|

   ![Een WAF-beleid maken](./media/waf-front-door-create-portal/basic.png)

3. In de **koppeling** tabblad van de **een WAF-beleid maken** weergeeft, schakelt **toevoegen frontend host**, voer de volgende instellingen en selecteer vervolgens **toevoegen**:

    | Instelling                 | Value                                              |
    | ---                     | ---                                                |
    | Voordeur              | Selecteer de naam van uw voordeur-profiel.|
    | Frontend-host           | Selecteer de naam van de host van de voordeur **toevoegen**.|
    
    > [!NOTE]
    > Als de front-end-host gekoppeld aan een WAF-beleid is, wordt deze weergegeven als lichter gekleurd weergegeven. U moet eerst de frontend-host van het bijbehorende beleid verwijderen en vervolgens opnieuw de frontend-host naar een nieuwe WAF-beleid te koppelen.
1. Selecteer **revisie + maken**en selecteer vervolgens **maken**.

## <a name="configure-waf-rules-optional"></a>WAF-regels (optioneel) configureren

### <a name="change-mode"></a>Modus wijzigen

Wanneer u een WAF-beleid maakt, door de standaard-WAF beleid bevindt zich in **detectie** modus. In **detectie** WAF-modus geen verzoeken blokkeert, in plaats daarvan de aanvragen die overeenkomt met de WAF-regels zijn vastgelegd bij WAF-Logboeken.
Als u wilt WAF in actie zien, kunt u de instellingen voor de modus van **detectie** naar **preventie**. In **preventie** modus, vraagt dat overeenkomst-regels die zijn gedefinieerd in standaard regel ingesteld (DRS) zijn geblokkeerd en vastgelegd bij WAF-Logboeken.

 ![Wijziging WAF-beleidsmodus](./media/waf-front-door-create-portal/policy.png)

### <a name="default-rule-set-drs"></a>Standaard-regelset (DRS)

Standaard regelset Azure beheerde is standaard ingeschakeld. Als u wilt een afzonderlijke regel in een regelgroep uitschakelt, vouw de regels in die regelgroep, selecteer de **selectievakje** in het zicht van het regelnummer van de en selecteer **uitschakelen** op het tabblad hierboven. Wijzigen van typen acties voor afzonderlijke regels in de regel ingesteld, schakel het selectievakje in het zicht van het regelnummer van de en selecteer vervolgens de **actie wijzigen** hierboven een tab.

 ![WAF-regelset wijzigen](./media/waf-front-door-create-portal/managed.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure web application firewall](waf-overview.md).
- Meer informatie over [Azure voordeur](front-door-overview.md).





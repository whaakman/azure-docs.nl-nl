---
title: Maak een Web Application Firewall beleid voor Azure front-deur door gebruik te maken van de Azure Portal
titlesuffix: Azure web application firewall
description: Meer informatie over het maken van een Web Application Firewall-beleid (WAF) met behulp van de Azure Portal.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: abaef0fb521d848134885a06591b0656c60c67e6
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846378"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Maak een WAF-beleid voor de front-deur van Azure met behulp van de Azure Portal

In dit artikel wordt beschreven hoe u een Basic Azure Web Application Firewall-beleid (WAF) maakt en dit toepast op een front-end-host in azure front-deur.

## <a name="prerequisites"></a>Vereisten

Maak een voor deur profiel door de instructies te volgen die [worden beschreven in Quick Start: Een front deur profiel](quickstart-create-front-door.md)maken. 

## <a name="create-a-waf-policy"></a>Een WAF-beleid maken

Maak eerst een basis-WAF-beleid met beheerde standaard regelset (DRS) met behulp van de portal. 

1. Selecteer in de linkerbovenhoek van het scherm de optie **een resource maken**> zoek naar **WAF**> Selecteer **Web application firewall (preview)** > Selecteer **Create**.
2. Voer op het tabblad **basis beginselen** van de pagina **een WAF-beleid maken** de volgende informatie in of Selecteer deze, accepteer de standaard waarden voor de overige instellingen en selecteer vervolgens **controleren + maken**:

    | Instelling                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription            |Selecteer de naam van uw front-deur abonnement.|
    | Resource group          |Selecteer de naam van de resource groep voor de voor deur.|
    | Beleids naam             |Voer een unieke naam in voor uw WAF-beleid.|

   ![Een WAF-beleid maken](./media/waf-front-door-create-portal/basic.png)

3. Op het tabblad **koppeling** van de pagina **een WAF-beleid maken** selecteert u **frontend-host toevoegen**, voert u de volgende instellingen in en selecteert u **toevoegen**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Voor deur              | Selecteer de naam van uw front deur-profiel.|
    | Frontend-host           | Selecteer de naam van de voor deur van de host en selecteer vervolgens **toevoegen**.|
    
    > [!NOTE]
    > Als de frontend-host aan een WAF-beleid is gekoppeld, wordt deze als grijs weer gegeven. U moet eerst de frontend-host uit het bijbehorende beleid verwijderen en vervolgens de frontend-host opnieuw koppelen aan een nieuw WAF-beleid.
1. Selecteer **controleren + maken**en selecteer vervolgens **maken**.

## <a name="configure-waf-rules-optional"></a>WAF-regels configureren (optioneel)

### <a name="change-mode"></a>Modus wijzigen

Wanneer u een WAF-beleid maakt, bevindt het standaard WAF-beleid zich in de **detectie** modus. In de **detectie** modus worden in plaats daarvan geen aanvragen door WAF geblokkeerd. aanvragen die overeenkomen met de WAF-regels worden in WAF-logboeken vastgelegd.
Als u WAF in actie wilt zien, kunt u de modus instellingen wijzigen van **detectie** in **preventie**. In de **preventie** modus worden aanvragen die overeenkomen met de regels die zijn gedefinieerd in de standaardregelset (drs) geblokkeerd en geregistreerd bij WAF-Logboeken.

 ![WAF-beleids modus wijzigen](./media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Aangepaste regels

U kunt een aangepaste regel maken door **aangepaste regel toevoegen** te selecteren onder de sectie **aangepaste regels** . Hiermee opent u de pagina aangepaste regel configuratie. Hieronder ziet u een voor beeld van het configureren van een aangepaste regel voor het blok keren van een aanvraag als de query reeks **blockme**bevat.

![WAF-beleids modus wijzigen](./media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Standaardregelset (DRS)

De standaard regelset die door Azure wordt beheerd, is standaard ingeschakeld. Als u een afzonderlijke regel binnen een regel groep wilt uitschakelen, vouwt u de regels binnen die regel groep uit, selecteert u het **selectie vakje** vóór het regel nummer en selecteert u **uitschakelen** op het bovenstaande tabblad. Als u de typen acties voor afzonderlijke regels in de regelset wilt wijzigen, schakelt u het selectie vakje vóór het regel nummer in en selecteert u vervolgens het tabblad **wijzigings actie** hierboven.

 ![WAF Rule set wijzigen](./media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Web Application firewall](waf-overview.md).
- Meer informatie over de [voor deur van Azure](front-door-overview.md).

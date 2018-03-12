---
title: Verbinding maken met Microsoft Azure-toepassingsgateway met Azure Security Center | Microsoft Docs
description: Informatie over het integreren van Application Gateway en Azure Security Center om te verbeteren de algehele beveiliging van uw resources.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/07/2018
ms.author: terrylan
ms.openlocfilehash: 7c15e5a86df7ff2a374aa9b62d2775b1eb035fc6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Verbinding maken met Microsoft Azure-toepassingsgateway met Azure Security Center
Dit document helpt u de integratie met Application Gateway web application firewall (WAF) en Security Center configureren.

## <a name="why-connect-application-gateway"></a>Waarom Application Gateway verbinding?
De WAF in Application Gateway beveiligt webtoepassingen van algemene web gebaseerde aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks. Security Center kan worden geïntegreerd met Application Gateway om te voorkomen en bedreigingen voor de niet-beveiligde webtoepassingen in uw omgeving detecteren.

## <a name="how-do-i-configure-this-integration"></a>Hoe kan ik deze integratie configureren?
Security Center detecteert eerder geïmplementeerde WAF instanties in het abonnement. Verbinding met het maken van deze oplossingen met Security Center om toe te staan van de integratie van waarschuwingen.

> [!NOTE]
> De toepassing Gateway WAF kan ook worden ingericht vanuit Security Center van **aanbevelingen** zoals beschreven in [web application firewall toevoegen](security-center-add-web-application-firewall.md).
>
>

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).

2. Op de **Microsoft Azure-menu**, selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

3. Onder **overzicht**, selecteer **beveiligingsoplossingen**.

  ![Security Center-overzicht](./media/security-center-connect-application-gateway/overview.png)

4. Onder **gedetecteerd oplossingen** Microsoft SaaS gebaseerde Web Application Firewall Zoek en selecteer **CONNECT**.

  ![Gedetecteerde oplossingen](./media/security-center-connect-application-gateway/connect.png)

5. **WAF-oplossing verbinden** wordt geopend.  Selecteer **Connect** de WAF en Security Center integreren.

  ![Verbinding maken met WAF-oplossing](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de toepassing Gateway WAF in Security Center integreren. Zie de volgende artikelen voor meer informatie over Security Center:

* [Integratie van beveiligingsoplossingen in Security Center](security-center-partner-integration.md)
* [Maken van verbinding Microsoft Advanced Threat Analytics met Security Center](security-center-ata-integration.md)
* [Verbinding maken met Azure Active Directory Identity Protection Security Center](security-center-aadip-integration.md)
* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md).
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md).
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md).
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/).

---
title: Microsoft Azure Application Gateway met Azure Security Center verbinden | Microsoft Docs
description: Informatie over het integreren van Application Gateway en Azure Security Center ter verbetering van de algehele beveiliging van uw resources.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: 5638b71147592ae71c741ca86da68ddfec668af5
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299063"
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Microsoft Azure Application Gateway met Azure Security Center verbinden
Dit document helpt u bij het configureren van de integratie met Application Gateway web application firewall (WAF) en Security Center.

## <a name="why-connect-application-gateway"></a>Waarom verbinding maken met Application Gateway?
De WAF in Application Gateway beschermt webtoepassingen tegen veelvoorkomende webgebaseerde aanvallen, zoals SQL-injectie, aanvallen via cross-site scripting, websites en sessiehijacks. Security Center kan worden geïntegreerd met Application Gateway om te voorkomen en detecteren van bedreigingen voor niet-beveiligde webtoepassingen in uw omgeving.

## <a name="how-do-i-configure-this-integration"></a>Hoe configureer ik deze integratie?
Security Center detecteert eerder geïmplementeerde WAF-instanties in het abonnement. Deze oplossingen verbinden met Security Center om toe te staan van de integratie van waarschuwingen.

> [!NOTE]
> De Application Gateway WAF kan ook worden ingericht vanuit Security Center **aanbevelingen** zoals beschreven in [toevoegen van een web application firewall](security-center-add-web-application-firewall.md).
>
>

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).

2. Ga naar het menu van **Microsoft Azure** en selecteer **Security Center**.

3. Onder **RESOURCE beveiliging hygiëne**, selecteer **beveiligingsoplossingen**.

  ![Overzicht van Security Center](./media/security-center-connect-application-gateway/overview.png)

4. Onder **gedetecteerde oplossingen** Microsoft SaaS gebaseerde Web Application Firewall Zoek en selecteer **CONNECT**.

  ![Gedetecteerde oplossingen](./media/security-center-connect-application-gateway/connect.png)

5. **Verbinding maken met WAF-oplossing** wordt geopend.  Selecteer **Connect** de WAF en Security Center integreren.

  ![Verbinding maken met WAF-oplossing](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u de Application Gateway WAF in Security Center integreert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Beveiligingsoplossingen integreren in Security Center](security-center-partner-integration.md)
* [Maken van verbinding Microsoft Advanced Threat Analytics naar Security Center](security-center-ata-integration.md)
* [Azure Active Directory Identity Protection koppelen aan Security Center](security-center-aadip-integration.md)
* [Beveiligingsstatus controleren in Security Center](security-center-monitoring.md).
* [Partneroplossingen controleren met Security Center](security-center-partner-solutions.md).
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md).
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/).

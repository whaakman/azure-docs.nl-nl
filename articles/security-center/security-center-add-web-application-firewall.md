---
title: Een Web Application Firewall toevoegen in Azure Security Center | Microsoft Docs
description: Dit document bevat informatie over het implementeren van de Azure Security Center aanbevelingen om **een Web Application firewall toe** te voegen en de **beveiliging van toepassingen te volt ooien**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: 63852ccab842f11f30bcbe695206fedf72931911
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60706173"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Een Web Application Firewall toevoegen in Azure Security Center
Azure Security Center kan worden aangeraden om een Web Application Firewall (WAF) van een micro soft-partner toe te voegen om uw webtoepassingen te beveiligen. In dit document vindt u een voor beeld van het Toep assen van deze aanbeveling.

Er wordt een WAF-aanbeveling weer gegeven voor een openbaar gericht IP-adres (IP op exemplaar niveau of IP-verkeer met gelijke taak verdeling) met een netwerk beveiligings groep die is gekoppeld aan open inkomende webpoorten (80.443).

Security Center raadt u aan om een WAF in te richten, zodat u zich kunt beschermen tegen aanvallen die gericht zijn op uw webtoepassingen op virtuele machines en op [](https://azure.microsoft.com/pricing/details/app-service/windows/) externe app service omgevingen (ASE) die zijn geïmplementeerd onder het geïsoleerde service-abonnement. Het Isolated-abonnement host uw apps in een privé en speciale Azure-omgeving en is ideaal voor apps die beveiligde verbindingen in uw lokale netwerk of extra prestaties en schaalgrootte vereisen. Naast uw app in een geïsoleerde omgeving moet uw app beschikken over een extern IP-adres load balancer. Zie de [app service Environment-documentatie](../app-service/environment/intro.md)voor meer informatie over ASE.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze hand leiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Onder **aanbevelingen**selecteert **u beveiligde webtoepassing met Web Application firewall**.
   ![Veilige webtoepassing][1]
2. Onder **Beveilig uw**webtoepassingen met Web Application firewall, selecteert u een web-app. **Een Web Application firewall** wordt geopend.
   ![Een firewall voor webtoepassingen toevoegen][2]
3. U kunt ervoor kiezen om een bestaande Web Application Firewall te gebruiken als deze beschikbaar zijn of u kunt een nieuwe maken. In dit voor beeld zijn er geen bestaande Waf's beschikbaar, dus we maken een WAF.
4. Als u een WAF wilt maken, selecteert u een oplossing in de lijst met geïntegreerde partners. In dit voor beeld selecteren we **Barracuda Web Application firewall**.
5. **Barracuda Web Application firewall** wordt geopend met informatie over de partner oplossing. Selecteer **Maken**.

   ![Blade firewall gegevens][3]

6. Er wordt een **nieuwe firewall** voor webtoepassingen geopend, waar u de **VM-configuratie** tappen kunt uitvoeren en **WAF-informatie**kan opgeven. Selecteer de **VM-configuratie**.
7. Onder **VM-configuratie**voert u de vereiste informatie in om de virtuele machine waarop de WAF wordt uitgevoerd, in te stellen.

   ![VM-configuratie][4]
   
8. Ga terug naar **nieuwe Web Application firewall** en selecteer **WAF-informatie**. Onder **WAF Information**configureert u de WAF zelf. Met stap 7 kunt u de virtuele machine configureren waarop de WAF wordt uitgevoerd en met stap 8 kunt u de WAF zelf inrichten.

## <a name="finalize-application-protection"></a>Toepassingsbeveiliging voltooien
1. Ga terug naar **aanbevelingen**. Er is een nieuwe vermelding gegenereerd nadat u de WAF hebt gemaakt, met de naam **toepassings beveiliging volt ooien**. Met deze vermelding weet u dat u het proces van de daad werkelijke bedrading van de WAF binnen de Azure-Virtual Network moet volt ooien zodat de toepassing kan worden beveiligd.

   ![Toepassingsbeveiliging voltooien][5]

2. Selecteer **toepassings beveiliging volt ooien**. Er wordt een nieuwe blade geopend. U kunt zien dat er een webtoepassing is waarvoor het verkeer moet worden omgeleid.
3. Selecteer de webtoepassing. Er wordt een Blade geopend met stappen voor het volt ooien van de Web Application Firewall Setup. Voer de stappen uit en selecteer vervolgens **verkeer beperken**. Security Center dan de bedrading voor u.

   ![Verkeer beperken][6]

> [!NOTE]
> U kunt meerdere webtoepassingen in Security Center beveiligen door deze toepassingen toe te voegen aan uw bestaande WAF-implementaties.
>
>

De logboeken van deze WAF zijn nu volledig geïntegreerd. Security Center kunt de logboeken automatisch verzamelen en analyseren zodat deze belang rijke beveiligings waarschuwingen voor u kunnen ondergaan.

## <a name="next-steps"></a>Volgende stappen
In dit document wordt uitgelegd hoe u de Security Center-aanbeveling implementeert een webtoepassing toevoegen. Ga voor meer informatie over het configureren van een Web Application Firewall naar het volgende:

* [Een Web Application Firewall (WAF) voor de App Service-omgeving configureren](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Beveiligings status controleren in azure Security Center](security-center-monitoring.md) --meer informatie over het controleren van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Aanbevelingen voor beveiliging in azure Security Center](security-center-recommendations.md) : Ontdek hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligings blog](https://blogs.msdn.com/b/azuresecurity/) : vind blog berichten over beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png

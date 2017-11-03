---
title: Web application firewall toevoegen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de Azure Security Center aanbevelingen implementeren ** toevoegen van een web application firewall ** en ** voltooien toepassing beveiliging **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: e858db97c3e7a832ad01e16a60d486a758109d7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Web application firewall toevoegen in Azure Security Center
Azure Security Center kan aanbevelen web application firewall (WAF) toe te voegen uit een Microsoft-partner voor het beveiligen van uw webtoepassingen. Dit document vindt u via een voorbeeld van het toepassen van deze aanbeveling.

Een WAF aanbeveling wordt voor een openbare internetgerichte IP (exemplaar niveau IP- of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met poorten voor inkomend web openen (80,443) weergegeven.

Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op App Service-omgeving inrichten. Een as-omgeving (App Service omgeving) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service-plan optie van Azure App Service die een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van Azure App Service-apps. Zie voor meer informatie over het as-omgeving, de [documentatie van App Service-omgeving](../app-service/environment/intro.md).

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is niet een stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **-webtoepassing met web application firewall Secure**.
   ![Web Application beveiligen][1]
2. In de **beveiligen van uw webtoepassingen met web application firewall** blade, selecteert u een webtoepassing. De **Web Application Firewall toevoegen** blade wordt geopend.
   ![Een firewall voor webtoepassingen toevoegen][2]
3. U kunt kiezen om een bestaande web application firewall als beschikbaar of u kunt een nieuwe maken. In dit voorbeeld zijn er geen bestaande WAFs beschikbaar, dus we een WAF maken.
4. Selecteer voor het maken van een WAF, een oplossing uit de lijst met geïntegreerde partners. In dit voorbeeld selecteren we **Barracuda Web Application Firewall**.
5. De **Barracuda Web Application Firewall** blade wordt geopend zodat u informatie over de partneroplossing. Selecteer **maken** in de blade informatie.

   ![Firewall-informatie-blade][3]

6. De **nieuwe Web Application Firewall** blade wordt geopend, waar u kunt uitvoeren **VM-configuratie** stappen en geef **WAF informatie**. Selecteer **VM-configuratie**.
7. In de **VM-configuratie** blade, voert u gegevens die zijn vereist voor de virtuele machine die wordt uitgevoerd de WAF draaien.
   ![VM-configuratie][4]
8. Ga terug naar de **nieuwe Web Application Firewall** blade en selecteer **WAF informatie**. In de **WAF informatie** blade u de WAF zelf configureren. Stap 7 kunt u voor het configureren van de virtuele machine waarop de WAF wordt uitgevoerd en stap 8 kunt u voor het inrichten van de WAF zelf.

## <a name="finalize-application-protection"></a>Toepassingsbeveiliging voltooien
1. Ga terug naar de **aanbevelingen** blade. Een nieuwe vermelding is gegenereerd nadat u de WAF, aangeroepen gemaakt **Toepassingsbeveiliging voltooien**. Deze vermelding laat u weten dat u voltooien van het proces moet van het daadwerkelijk bekabeling van de WAF binnen het virtuele netwerk van Azure, zodat deze de toepassing kunt beveiligen.

   ![Toepassingsbeveiliging voltooien][5]

2. Selecteer **Toepassingsbeveiliging voltooien**. Er wordt een nieuwe blade geopend. U ziet dat er een webtoepassing die u moet beschikken over de verkeer gerouteerd.
3. Selecteer de webtoepassing. Er wordt een blade geopend waarmee u stappen voor het voltooien van de web application firewall-instellingen. Voer de stappen uit en selecteer vervolgens **verkeer te beperken**. Security Center doet vervolgens de bedrading-up voor u.

   ![Verkeer te beperken][6]

> [!NOTE]
> U kunt meerdere webtoepassingen in Security Center beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties.
>
>

De logboeken van die WAF zijn nu volledig geïntegreerd. Security Center kunt starten automatisch verzamelen en analyseren in de logboeken zodat dit kan belangrijk beveiligingswaarschuwingen surface voor u.

## <a name="next-steps"></a>Volgende stappen
Dit document hebt u geleerd hoe u implementeert de aanbeveling Security Center "Een webtoepassing toevoegen". Zie de volgende onderwerpen voor meer informatie over het configureren van een web application firewall:

* [Web Application Firewall (WAF) configureren voor App Service-omgeving](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png

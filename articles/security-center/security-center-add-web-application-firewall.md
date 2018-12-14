---
title: Een web application firewall toevoegen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe u de aanbevelingen voor Azure Security Center implementeren **toevoegen van een web application firewall** en **Toepassingsbeveiliging**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: 5e6d7dffb831e4e6541b5ef91cfc4ae4bbe88167
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343136"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Een web application firewall toevoegen in Azure Security Center
Azure Security Center kan raden u aan een web application firewall (WAF) via een Microsoft-partner voor het beveiligen van uw webtoepassingen. In dit document wordt u via een voorbeeld van hoe u deze aanbeveling toepast.

Een WAF-aanbeveling wordt voor een openbaar IP-adres (Instance Level IP of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met open binnenkomende webpoorten (80,443) weergegeven.

Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op externe App Service-omgevingen (ASE inricht) geïmplementeerd onder de [geïsoleerd](https://azure.microsoft.com/pricing/details/app-service/windows/) service-plan. Het Isolated-abonnement host uw apps in een privé en speciale Azure-omgeving en is ideaal voor apps die beveiligde verbindingen in uw lokale netwerk of extra prestaties en schaalgrootte vereisen. Naast uw app wordt in een geïsoleerde omgeving, moet uw app in een externe load balancer IP-adres hebben. Zie voor meer informatie over het as-omgeving, de [documentatie over App Service-omgeving](../app-service/environment/intro.md).

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. Onder **aanbevelingen**, selecteer **beveiligde web-App met behulp van de web application firewall**.
   ![Web-toepassing beveiligen][1]
2. Onder **beveiligen van uw webtoepassingen die met web application firewall**, selecteert u een web-App. **Een Web Application Firewall toevoegen** wordt geopend.
   ![Een firewall voor webtoepassingen toevoegen][2]
3. U kunt een bestaande webtoepassingsfirewall gebruiken, indien beschikbaar, of kunt u een nieuwe groep maken. In dit voorbeeld zijn er geen bestaande WAF's beschikbaar, zodat we een WAF maken.
4. Voor het maken van een WAF selecteert u een oplossing uit de lijst met geïntegreerde partners. In dit voorbeeld selecteren we **Barracuda Web Application Firewall**.
5. **Barracuda Web Application Firewall** wordt geopend zodat u meer informatie over de partneroplossing. Selecteer **Maken**.

   ![Blade firewall-informatie][3]

6. **Nieuwe Web Application Firewall** wordt geopend, waar u kunt uitvoeren **VM-configuratie** stappen en geef **WAF-informatie**. Selecteer **VM-configuratie**.
7. Onder **VM-configuratie**, u vereiste informatie op voor de virtuele machine die wordt uitgevoerd de WAF instellen.

   ![VM-configuratie][4]
   
8. Ga terug naar **nieuwe Web Application Firewall** en selecteer **WAF-informatie**. Onder **WAF-informatie**, configureren van de WAF zelf. Stap 7 kunt u het configureren van de virtuele machine waarop de WAF wordt uitgevoerd en stap 8 kunt u voor het inrichten van de WAF zelf.

## <a name="finalize-application-protection"></a>Toepassingsbeveiliging voltooien
1. Ga terug naar **aanbevelingen**. Een nieuwe vermelding is gegenereerd nadat u hebt gemaakt met de WAF, met de naam **Toepassingsbeveiliging**. Deze vermelding laat u weten dat u het voltooien wilt van de WAF binnen het Virtueelnetwerk van Azure daadwerkelijk voorbereiden zodat deze de toepassing kunt beveiligen.

   ![Toepassingsbeveiliging voltooien][5]

2. Selecteer **Toepassingsbeveiliging**. Er wordt een nieuwe blade geopend. U ziet dat er een web-App die u moet beschikken over haar verkeer omgeleid.
3. Selecteer de web-App. Er wordt een blade geopend waarmee u stappen voor het voltooien van de installatie van web application firewall. Voer de stappen uit en selecteer vervolgens **verkeer beperken tot**. De bekabeling van wordt Security Center voor u.

   ![Verkeer beperken][6]

> [!NOTE]
> U kunt meerdere webtoepassingen in Security Center beveiligen door toe te voegen deze toepassingen aan uw bestaande WAF-implementaties.
>
>

De logboeken van die WAF zijn nu volledig geïntegreerd. Security Center kunt starten automatisch verzamelen en analyseren van de logboeken zodat deze kan Maak belangrijke beveiligingswaarschuwingen duidelijk aan u.

## <a name="next-steps"></a>Volgende stappen
Dit document hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center 'Een webtoepassing toevoegen'. Zie de volgende onderwerpen voor meer informatie over het configureren van een web application firewall:

* [Een Web Application Firewall (WAF) voor de App Service-omgeving configureren](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png

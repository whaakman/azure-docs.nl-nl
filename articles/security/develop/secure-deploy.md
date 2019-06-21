---
title: Implementeren van veilige toepassingen op Microsoft Azure
description: In dit artikel wordt beschreven aanbevolen procedures overwegen tijdens de fasen van de release en het antwoord van uw web application-project.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: e8249113ee65c28414c79f00c53d11596673434b
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144449"
---
# <a name="deploy-secure-applications-on-azure"></a>Beveiligde toepassingen in Azure implementeren
In dit artikel geven we beveiligingsactiviteiten en besturingselementen om te overwegen bij het implementeren van toepassingen voor de cloud. Beveiligingsvragen en -concepten te houden tijdens de fasen van de release en het antwoord van de Microsoft [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) vallen. Het doel is voor het definiëren van activiteiten en Azure-services die u gebruiken kunt om een beter beveiligde toepassing te implementeren.

De volgende fasen van de SDL worden in dit artikel besproken:

- Release
- Antwoord

## <a name="release"></a>Release
De focus van de release-fase is een project voor openbare release readying.
Dit omvat het plannen van de manieren om efficiënt uitvoeren van onderhoud taken na de introductie en los beveiligingsproblemen die later kunnen optreden.

### <a name="check-your-applications-performance-before-you-launch"></a>Prestaties van uw toepassing controleren voordat u starten

Controleer de prestaties van uw toepassing voordat u het starten of updates naar productie implementeren. Uitvoeren van cloud-gebaseerde [belastingstests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) met behulp van Visual Studio prestatieproblemen vinden in uw toepassing, implementatie-kwaliteit te verbeteren, zorg ervoor dat uw toepassing is altijd omhoog of beschikbaar is en dat uw toepassing kan verkeer verwerken voor uw implementatie.

### <a name="install-a-web-application-firewall"></a>Installeren van een web application firewall

Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Algemene tussen deze aanvallen zijn SQL-injectieaanvallen en aanvallen via cross-site scripting. Zo wordt voorkomen dat deze aanvallen in toepassingscode kan lastig zijn. Er kan tevens veel onderhoud, patching en controle op veel lagen van de Toepassingstopologie is vereist. Een gecentraliseerde WAF helpt maakt het beveiligingsbeheer eenvoudiger. Een WAF-oplossing kan ook reageren op een beveiligingsrisico patches voor een bekend beveiligingsprobleem op een centrale locatie ten opzichte van elke afzonderlijke webtoepassing te beveiligen.

De [Azure Application Gateway WAF](https://docs.microsoft.com/azure/application-gateway/waf-overview) gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. De WAF is gebaseerd op regels uit de [OWASP core rule set](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 of 2.2.9.

### <a name="create-an-incident-response-plan"></a>Maak een plan voor respons op incidenten

Voorbereiden van een plan voor respons op incidenten is essentieel om u te helpen bij het aanpakken van nieuwe bedreigingen die na verloop van tijd worden mogelijk. Voorbereiden van een plan voor respons op incidenten bevat contactpersonen in noodgevallen geschikte beveiligingscontroles identificeren en het tot stand brengen van beveiliging onderhoudsplannen voor code die overgenomen van andere groepen in de organisatie en een licentie van derden code.

### <a name="conduct-a-final-security-review"></a>Een definitieve beveiligingsbeoordeling uitvoeren

Opzettelijk controleren van alle activiteiten die zijn uitgevoerd, zorgt ervoor dat de gereedheid voor de release van de software of de toepassing. De laatste beveiligingsbeoordeling (FSR) omvat gewoonlijk het onderzoeken van bedreigingsmodellen, hulpprogramma's voor uitvoer en prestaties ten opzichte van de kwaliteit gates en fout-balken die zijn gedefinieerd in de fase van de vereisten.

### <a name="certify-release-and-archive"></a>Certificeren release en archiveren

Certificering software voordat een release ervoor zorgt dat de beveiliging en privacy-vereisten wordt voldaan. Archiveren van alle relevante gegevens is het essentieel is voor het uitvoeren van onderhoud taken na de introductie. Archiveren ook helpt verlagen de kosten voor lange termijn met aanhoudende software engineering.

## <a name="response"></a>Antwoord
De reactie na de Introductie fase draait om het ontwikkelingsteam kan en om te reageren op de juiste wijze op rapporten van opkomende softwarebedreigingen en zwakke plekken.

### <a name="execute-the-incident-response-plan"></a>Uitvoeren van het plan voor respons op incidenten

De mogelijkheid voor het implementeren van de reactieplan ingesteld in de release-fase is essentieel voor het klanten beveiligen tegen software beveiligings- of beveiligingsproblemen die indelingsversies beschikbaar komen.

### <a name="monitor-application-performance"></a>Prestaties van toepassingen controleren

Voortdurende controle van uw toepassing nadat deze mogelijk geïmplementeerd, kunt u prestatieproblemen, evenals beveiligingsproblemen detecteren.
Azure-services die u met het bewaken van toepassing helpen zijn:

  - Azure Application Insights
  - Azure Security Center

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Application Insights detecteert automatisch prestatieafwijkingen. Het bevat krachtige analysehulpmiddelen om u te helpen u problemen identificeren en te begrijpen wat gebruikers daadwerkelijk doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

#### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het voorkomen, detecteren, en reageer op bedreigingen met verbeterde zichtbaarheid (en controle over) de beveiliging van uw Azure-resources, met inbegrip van webtoepassingen. Azure Security Center helpt bedreigingen detecteren die anders onopgemerkt. Het werkt met verschillende beveiligingsoplossingen.

De gratis laag van Security Center biedt beperkte beveiliging voor uw Azure-resources alleen. De [Security Center Standard-laag](https://docs.microsoft.com/azure/security-center/security-center-onboarding) breidt deze mogelijkheden voor on-premises bronnen en andere clouds.
Standard van Security Center helpt u om:

  - Opsporen en oplossen van beveiligingsproblemen.
  - Toegangs- en toepassingsbesturingselementen om schadelijke activiteiten blokkeren van toepassing.
  - Bedreigingen detecteren met behulp van analyses en gegevens.
  - Snel reageren bij aanvallen.

## <a name="next-steps"></a>Volgende stappen
In de volgende artikelen, is het raadzaam beveiligingsmaatregelen en activiteiten waarmee u kunnen ontwerpen en ontwikkelen van veilige toepassingen.

- [Beveiligde toepassingen ontwerpen](secure-design.md)
- [Beveiligde toepassingen ontwikkelen](secure-develop.md)

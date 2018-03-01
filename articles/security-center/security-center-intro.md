---
title: Wat is Azure Security Center?| Microsoft Docs
description: Meer informatie over Azure Security Center, de belangrijkste mogelijkheden van Azure Security Center en hoe Azure Security Center werkt.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor verschillende hybride cloudworkloads. Met Security Center kunt u beveiligingsbeleid toepassen voor verschillende workloads, blootstelling aan bedreigingen beperken en aanvallen detecteren en afwenden.

Waarom Security Center gebruiken?

- **Gecentraliseerd beheer van beleid**: zorg voor naleving van bedrijfsspecifieke en wettelijke beveiligingsvereisten door beveiligingsbeleid voor al uw hybride cloudworkloads centraal te beheren.
- **Continue beveiligingsevaluatie**: bewaak de beveiliging van computers, netwerken, opslag- en gegevensservices en toepassingen door mogelijke beveiligingsproblemen te detecteren.
- **Uitvoerbare aanbevelingen**: los beveiligingsproblemen op voordat ze kunnen worden misbruikt door aanvallers met op prioriteit gebaseerde en uitvoerbare beveiligingsaanbevelingen.
- **Geavanceerde cloudbeveiliging**: verminder bedreigingen met just-in-time-toegang tot beheerpoorten en opname in whitelist om controle te houden over de toepassingen die worden uitgevoerd op uw virtuele machines.
- **Op prioriteit gebaseerde waarschuwingen en incidenten**: richt u eerst op de meest kritieke bedreigingen met op prioriteit gebaseerde beveiligingswaarschuwingen en -incidenten.
- **Geïntegreerde beveiligingsoplossingen**: verzamel, zoek en analyseer beveiligingsgegevens uit verschillende bronnen, inclusief verbonden oplossingen van partners.

**Security Center - overzicht** biedt snel inzicht in de beveiligingsstatus van uw Azure- en niet-Azure-workloads, zodat u de beveiliging van uw workloads kunt bepalen en beoordelen en risico's kunt herkennen en verminderen. Het ingebouwde dashboard geeft onmiddellijk inzicht in beveiligingswaarschuwingen en zwakke plekken die aandacht vereisen.

![Overzicht][1]

## <a name="centralized-policy-management"></a>Gecentraliseerd beleidsbeheer
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Security Center definieert u beleid en past u dit aan uw type workload of de gevoeligheid van uw gegevens aan.

Beleidsregels van Security Center bevatten de volgende onderdelen:

- **Gegevensverzameling**: bepaalt instellingen voor configuratie van agent en beveiliging van [gegevensverzameling](security-center-enable-data-collection.md).
- **Beveiligingsbeleid**: bepaalt welke besturingselementen Security Center bewaakt en aanbeveelt door het [beveiligingsbeleid](security-center-policies.md) te bewerken.
- **E-mailmeldingen**: bepaalt instellingen voor contactpersonen en [e-mailmeldingen](security-center-provide-security-contact-details.md) voor beveiliging.
- **Prijscategorie**: definieert de [prijsselectie](security-center-pricing.md) Gratis of Standard. De laag die u kiest, bepaalt welke Security Center-functies beschikbaar zijn voor resources binnen het bereik.

![Beveiligingsbeleid][2]

Zie [Overzicht van beveiligingsbeleid](security-center-policies-overview.md) voor meer informatie.

## <a name="continuous-security-assessment"></a>Continue beveiligingsevaluatie
Security Center analyseert de beveiligingsstatus van uw rekenresources, virtuele netwerken, opslag- en gegevensservices en toepassingen. Continue evaluatie helpt u bij het detecteren van mogelijke beveiligingsproblemen, zoals systemen met ontbrekende beveiligingsupdates of netwerkpoorten die risico lopen. Selecteer een tegel in de sectie Preventie voor meer informatie, waaronder een lijst met resources en alle zwakke plekken die zijn geïdentificeerd.

![Bewaking van beveiligingsstatus][3]

Zie [Bewaking van beveiligingsstatus](security-center-monitoring.md) voor meer informatie.

## <a name="actionable-recommendations"></a>Uitvoerbare aanbevelingen
Security Center analyseert de beveiligingsstatus van uw Azure- en niet-Azure-resources om mogelijke beveiligingsproblemen op te sporen. Een lijst met op prioriteit gebaseerde beveiligingsaanbevelingen helpt u bij het afhandelen van beveiligingsproblemen.

![Aanbevelingen][4]

Zie [Beveiligingsaanbevelingen beheren](security-center-recommendations.md) voor meer informatie.

## <a name="just-in-time-vm-access"></a>Just-In-Time-VM-toegang
Verminder het oppervlak voor netwerkaanvallen met just-in-time, gecontroleerde toegang tot beheerpoorten op Azure VM's in om het oppervlak dat wordt blootgesteld aan beveiligingsaanvallen en andere netwerkaanvallen, drastisch te reduceren.

![Just-In-Time-VM-toegang][5]

Geef regels op voor hoe gebruikers verbinding met virtuele machines kunnen maken. Indien nodig, kan toegang worden aangevraagd vanuit Security Center of via PowerShell. Als de aanvraag aan de regels voldoet, wordt automatisch toegang verleend voor de gevraagde tijd.

Zie [VM-toegang beheren met behulp van just-in-time](security-center-just-in-time.md) voor meer informatie.

## <a name="adaptive-application-controls"></a>Adaptieve toepassingsbesturingselementen
Blokkeer malware en andere ongewenste toepassingen door aanbevelingen voor opname in de whitelist toe te passen die zijn afgestemd op uw specifieke Azure-workloads en gebruikmaken van machine learning.

![Adaptieve toepassingsbesturingselementen][6]

Bekijk en klik op toepassingen om de aanbevolen regels voor de opname in de whitelist die door Security Center zijn gegenereerd toe te passen of de regels die al zijn geconfigureerd te bewerken.

Zie [Adaptieve toepassingsbesturingselementen](security-center-adaptive-application.md) voor meer informatie.

## <a name="prioritized-alerts-and-incidents"></a>Waarschuwingen en incidenten op basis van prioriteit
Security Center maakt gebruik van geavanceerde analyses en informatie over wereldwijde dreigingen om binnenkomende aanvallen en schendingen van de beveiliging te detecteren. Waarschuwingen krijgen een prioriteit en worden gegroepeerd in incidenten, zodat u zich eerst op de meest kritieke bedreigingen kunt richten. U kunt ook uw eigen aangepaste beveiligingswaarschuwingen maken.

![Waarschuwingen en incidenten op basis van prioriteit][7]

U kunt snel de omvang en de impact van een aanval beoordelen via een visuele, interactieve onderzoeksmethode en vooraf gedefinieerde of ad hoc-query's gebruiken voor diepergaand onderzoek van de beveiligingsgegevens.

Zie [Beveiligingswaarschuwingen beheren en erop reageren](security-center-managing-and-responding-alerts.md).

## <a name="integrate-your-security-solutions"></a>Uw beveiligingsoplossingen integreren
U kunt beveiligingsgegevens verzamelen, zoeken en analyseren uit diverse bronnen, met inbegrip van verbonden partneroplossingen zoals netwerkfirewalls en andere Microsoft-services in Security Center.

![Beveiligingsoplossingen integreren][8]

Zie [Beveiligingsoplossingen integreren](security-center-partner-integration.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- U moet over een Microsoft Azure-abonnement beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- De prijscategorie Gratis van Security Center wordt met uw Azure-abonnement ingeschakeld. Als u wilt profiteren van geavanceerd beveiligingsbeheer en mogelijkheden voor het detecteren van bedreigingen, moet u een upgrade uitvoeren naar de prijscategorie Standard. De prijscategorie Standard is gedurende de eerste 60 dagen gratis. Zie de [pagina met prijzen van Security Center](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.
- Als u nu Security Center Standard wilt inschakelen, raadpleegt u de [Quickstart: Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Snelstartgids: uw Azure-abonnement registreren voor Security Center Standard) voor meer informatie.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png

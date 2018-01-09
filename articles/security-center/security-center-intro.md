---
title: Wat is Azure Security Center? | Microsoft Docs
description: Meer informatie over Azure Security Center, de belangrijkste mogelijkheden van Azure Security Center en hoe Azure Security Center werkt.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 50a54b8d2a73807aa9a0217f7ccf971b8c516494
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Security Center biedt geïntegreerde beveiligingsbewaking en geavanceerde threat protection over hybride cloudwerkbelastingen. Met Security Center, kunt u toepassen van beveiligingsbeleid voor uw werkbelastingen, uw blootstelling aan bedreigingen, beperken en detecteren en reageren op aanvallen.

Waarom Security Center gebruiken?

- **Gecentraliseerd beheer van** : Zorg ervoor dat de naleving van het bedrijf of regelgeving beveiligingsvereisten door het centraal beheren van beveiligingsbeleid voor alle werkbelastingen van uw hybride cloud.
- **Continue beveiligingsevaluatie** – bewaken van de beveiliging van de machines, netwerken, opslag- en -services en toepassingen voor het detecteren van mogelijke beveiligingsproblemen.
- **Uitvoerbare aanbevelingen** – beveiligingsproblemen oplossen voordat ze kunnen worden misbruikt door aanvallers met aanbevelingen voor beveiliging van prioriteit en actie worden uitgevoerd.
- **Geavanceerde beveiliging cloud** – bedreigingen met alleen bij het time-toegang voor de poorten voor beheertaken en whitelisting beperken tot besturingselement toepassingen die worden uitgevoerd op uw virtuele machines.
- **Prioriteit van waarschuwingen en incidenten** -Focus op de meest kritieke bedreigingen eerst met prioriteit beveiligingswaarschuwingen en incidenten.
- **Geïntegreerde beveiligingsoplossingen** - verzamelen, zoeken en van beveiligingsgegevens uit diverse bronnen, met inbegrip van verbonden partneroplossingen analyseren.

De **Security Center - overzicht** biedt snel inzicht in de beveiligingsstatus van uw Azure en niet-Azure-werkbelastingen, zodat u om te detecteren en beoordelen van de beveiliging van uw werkbelastingen kunt herkennen en verhelpen van risico's. De ingebouwde dashboard geeft onmiddellijk inzicht in beveiligingswaarschuwingen voor en zwakke plekken die aandacht vereisen.

![Overzicht][1]

## <a name="centralized-policy-management"></a>Gecentraliseerd beheer
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Security Center kunt u beleid definiëren en aanpassen aan uw type werkbelasting of de vertrouwelijkheid van uw gegevens.

Beleidsregels van Security Center bevatten de volgende onderdelen:

- **Gegevensverzameling**: bepaalt agent inrichting en beveiliging [gegevensverzameling](security-center-enable-data-collection.md) instellingen.
- **Beveiligingsbeleid**: bepalen welke besturingselementen Security Center monitors en raadt aan om door te bewerken de [beveiligingsbeleid](security-center-policies.md).
- **E-mailmeldingen**: beveiliging contactpersonen bepaalt en [e-mailmelding](security-center-provide-security-contact-details.md) instellingen.
- **Prijscategorie**: gratis definieert of standaard [prijzen selectie](security-center-pricing.md). De laag die u kiest, bepaalt welke Security Center-functies zijn beschikbaar voor resources binnen het bereik.

![Beveiligingsbeleid][2]

Zie [beleid beveiligingsoverzicht](security-center-policies-overview.md) voor meer informatie.

## <a name="continuous-security-assessment"></a>Continue beveiligingsevaluatie
Security Center analyseert de beveiligingsstatus van uw rekenresources, virtuele netwerken, opslag en services van gegevens en toepassingen. Continue assessment helpt u bij het detecteren van mogelijke beveiligingsproblemen, zoals systemen met ontbrekende beveiligingsupdates of blootgestelde netwerkpoorten. Selecteer een tegel in de sectie voorkomen voor meer informatie, waaronder een lijst met bronnen en alle zwakke plekken die zijn geïdentificeerd.

![Beveiligingsstatus bewaken][3]

Zie [Security health monitoring](security-center-monitoring.md) voor meer informatie.

## <a name="actionable-recommendations"></a>Uitvoerbare aanbevelingen
Security Center analyseert de beveiligingsstatus van uw Azure en niet-Azure-resources voor het identificeren van mogelijke beveiligingsproblemen. Een lijst met aanbevelingen voor beveiliging prioriteit leidt u door het proces van patches.

![Aanbevelingen][4]

Zie [aanbevelingen voor beveiliging beheren](security-center-recommendations.md) voor meer informatie.

## <a name="just-in-time-vm-access"></a>Just-In-Time-VM-toegang
Verminder de kwetsbaarheid van netwerk met alleen in de tijd, beheerde toegang tot de poorten voor beheertaken op Azure Virtual machines blootstelling aan brute force en andere netwerkaanvallen drastisch wordt gereduceerd.

![Just-In-Time-VM-toegang][5]

Regels opgeven voor hoe gebruikers verbinding met virtuele machines maken kunnen. Wanneer deze nodig is, kan toegang worden aangevraagd vanuit Security Center of via PowerShell. Als de aanvraag aan de regels voldoet, wordt automatisch toegang verleend voor de gevraagde tijd.

Zie [beheren via in de tijd toegang tot een virtuele machine](security-center-just-in-time.md) voor meer informatie.

## <a name="adaptive-application-controls"></a>Adaptieve toepassingsbesturingselementen
Malware en andere ongewenste toepassingen blokkeren door het toepassen van aanbevelingen whitelisting aangepast aan uw specifieke Azure werkbelastingen en aangedreven door machine learning.

![Adaptieve toepassingsbesturingselementen][6]

Bekijk en klik op om toe te passen de aanbevolen toepassing whitelisting regels die zijn gegenereerd door Security Center of bewerk de regels die al zijn geconfigureerd.

Zie [adaptieve toepassing besturingselementen](security-center-adaptive-application.md) voor meer informatie.

## <a name="prioritized-alerts-and-incidents"></a>Waarschuwingen van prioriteit en incidenten
Security Center maakt gebruik van geavanceerde analyses en wereldwijde dreigingen binnenkomende aanvallen detecteren en na schenden activiteit. Waarschuwingen worden geplaatst en gegroepeerd in incidenten, helpen u zich op de meest kritieke bedreigingen eerst richten. U kunt ook uw eigen aangepaste beveiligingswaarschuwingen maken.

![Waarschuwingen van prioriteit en incidenten][7]

U kunt snel de omvang en de impact van een aanval-ervaring bieden een visuele, interactieve onderzoek beoordelen en gebruiken van vooraf gedefinieerde of ad-hoc-query's voor dieper exploratie van beveiligingsgegevens.

Zie [beheren en erop reageren beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) voor meer informatie.

## <a name="integrate-your-security-solutions"></a>Integratie van uw beveiligingsoplossingen
U kunt verzamelen, zoeken en van beveiligingsgegevens uit diverse bronnen, met inbegrip van verbonden partneroplossingen zoals netwerkfirewalls en andere Microsoft-services in Security Center analyseren.

![Beveiligingsoplossingen integreren][8]

Zie [integreren beveiligingsoplossingen](security-center-partner-integration.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Om te beginnen met Security Center, moet u een abonnement op Microsoft Azure. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Security Center de gratis prijscategorie is met uw Azure-abonnement ingeschakeld. Als u wilt profiteren van Geavanceerd beveiligingsbeheer en threat detectiemogelijkheden, moet u upgraden naar de Standard-prijscategorie. De Standard-laag is gratis gedurende de eerste 60 dagen. Zie de [Security Center pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.
- Als u klaar bent om in te schakelen Security Center Standard nu de [Snelstartgids: vrijgeven uw Azure-abonnement Security Center Standard](security-center-get-started.md) leidt u door de stappen.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png

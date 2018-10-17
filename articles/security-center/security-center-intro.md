---
title: Wat is Azure Security Center?| Microsoft Docs
description: Meer informatie over Azure Security Center, de belangrijkste mogelijkheden van Azure Security Center en hoe Azure Security Center werkt.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: bd0e517845b9cfcbe6090dff8d656edcca782c83
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126289"
---
# <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor verschillende hybride cloudworkloads. Met Security Center kunt u beveiligingsbeleid toepassen voor verschillende workloads, blootstelling aan bedreigingen beperken en aanvallen detecteren en afwenden.

Waarom Security Center gebruiken?

- **Gecentraliseerd beheer van beleid**: zorg voor naleving van bedrijfsspecifieke en wettelijke beveiligingsvereisten door beveiligingsbeleid voor al uw hybride cloudworkloads centraal te beheren.
- **Continue beveiligingsevaluatie** – Bewaak de beveiligingsstatus van machines, netwerken, opslag- en gegevensservices en toepassingen voor het detecteren van mogelijke beveiligingsproblemen.
- **Uitvoerbare aanbevelingen**: los beveiligingsproblemen op voordat ze kunnen worden misbruikt door aanvallers met op prioriteit gebaseerde en uitvoerbare beveiligingsaanbevelingen.
- **Op prioriteit gebaseerde waarschuwingen en incidenten**: richt u eerst op de meest kritieke bedreigingen met op prioriteit gebaseerde beveiligingswaarschuwingen en -incidenten.
- **Geavanceerde cloudbeveiliging** - Verminder bedreigingen met just-in-time-toegang tot beheerpoorten en adaptief toepassingsbeheer die wordt uitgevoerd op uw virtuele machines.
- **Geïntegreerde beveiligingsoplossingen**: verzamel, zoek en analyseer beveiligingsgegevens uit verschillende bronnen, inclusief verbonden oplossingen van partners.

**Security Center - overzicht** biedt snel inzicht in de beveiligingsstatus van uw Azure- en niet-Azure-workloads, zodat u de beveiliging van uw workloads kunt bepalen en beoordelen en risico's kunt herkennen en verminderen. Het ingebouwde dashboard geeft onmiddellijk inzicht in beveiligingswaarschuwingen en zwakke plekken die aandacht vereisen.

![Overzicht][1]

## <a name="centralized-policy-management"></a>Gecentraliseerd beleidsbeheer
De **Beleid en naleving** sectie (zie hierboven) bevat samengevatte informatie over uw abonnementsdekking, beleidsnaleving en beveiliging.

### <a name="subscription-coverage"></a>Abonnementsdekking
In deze sectie vindt u het totale aantal abonnementen waar u toegang tot hebt (lezen en schrijven) en het niveau van de dekking van Security Center (standard of gratis):

- **Gedekt (standaard)** – Gedekte abonnementen worden uitgevoerd onder het maximumniveau van beveiliging dat wordt aangeboden door Security Center
- **Gedekt (gratis)** – Gedekte abonnementen worden uitgevoerd onder het beperkte, gratis-niveau van beveiliging dat wordt aangeboden door Security Center
- **Niet gedekt** – Abonnementen met deze status worden niet bewaakt door Security Center

Bij het selecteren van het diagram opent het **Dekking**-venster. Als u een van de tabbladen (**Niet gedekt**, **Basisdekking**, of **Standaarddekking**) kiest wordt een lijst weergegeven met de abonnementen in elke status. Als u een abonnement selecteert in een van de tabbladen, krijgt u aanvullende informatie over het abonnement. Met deze informatie kunt u de eigenaar van een abonnement identificeren en contact opnemen om Security Center in te kunnen schakelen of om het abonnement uit te breiden.

![Dekking Security Center][9]

### <a name="policy-compliance"></a>Beleidsnaleving
Beleidsnaleving wordt bepaald door de nalevingsfactoren van alle beleidsregels die zijn toegewezen. De algemene nalevingsscore voor een beheergroep, abonnement of werkruimte is het gewogen gemiddelde van de toewijzingen. Bij het gewogen gemiddelde wordt rekening gehouden met het aantal beleidsregels in één toewijzing en het aantal resources waarop de toewijzing van toepassing is.

Als uw abonnement bijvoorbeeld twee virtuele machines heeft en een initiatief waaraan vijf beleidsregels zijn toegewezen, dan hebt u tien beoordelingen in uw abonnement. Als een van de virtuele machines niet aan twee van de beleidsregels voldoet, is de algemene nalevingsscore van uw abonnementstoewijzing 80%.

Deze sectie vindt u de verhouding van uw algemene naleving en uw abonnement die minst overeenkomen. Als u **Toon naleving van het beleid van uw omgeving** selecteert, wordt het **Beleidsbeheer**-venster geopend. In **Beleidsbeheer** wordt de hiërarchische structuur van de beheergroepen, abonnementen en werkruimten weergegeven. U kunt hier uw beveiligingsbeleid beheren door een abonnement of beheergroep te kiezen.

![Beleidsbeheer][10]

Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Security Center definieert u beleid en past u dit aan uw type workload of de gevoeligheid van uw gegevens aan, waarbij u bepaalt welk beheer en welke aanbevelingen worden uitgevoerd door Security Center. U kunt het beveiligingsbeleid in Security Center bewerken door te klikken op een beheergroep of een abonnement. U kunt ook een Azure-beleid gebruiken voor het maken van nieuwe definities, het definiëren van extra beleidsregels en het toewijzen van beleidsregels binnen beheergroepen.

Kies **Instellingen bewerken >** om de volgende instellingen voor Security Center te bewerken voor het abonnement, de beheergroep, de resourcegroep of het werkruimteniveau:

- **Gegevensverzameling**: bepaalt instellingen voor configuratie van agent en beveiliging van [gegevensverzameling](security-center-enable-data-collection.md).
- **E-mailmeldingen**: bepaalt instellingen voor contactpersonen en [e-mailmeldingen](security-center-provide-security-contact-details.md) voor beveiliging.
- **Prijscategorie**: definieert de [prijsselectie](security-center-pricing.md) Gratis of Standard. De laag die u kiest, bepaalt welke Security Center-functies beschikbaar zijn voor resources binnen het bereik.
- **Bewerken veiligheidsconfiguraties**: Hier kunt u de besturingssysteemconfiguraties weergeven en aanpassen die door Security Center worden beoordeeld om potentiële beveiligingsproblemen te identificeren.

Zie [Integratie van Security Center-beveiligingsbeleid met Azure Policy](security-center-azure-policy.md) voor meer informatie.

### <a name="manage-and-govern-your-security-posture"></a>Het beveiligingspostuur beheren en bepalen
De rechterkant van het dashboard onder **Beleid en naleving** geeft inzicht waarmee u onmiddellijk de algehele beveiliging kan verbeteren. Een aantal voorbeelden:

- Definieer Security Center-beleidsregels en wijs deze toe om naleving van de beveiligingsstandaarden te evalueren en te volgen
- U kunt Security Center-beveiligingsmeldingen beschikbaar maken voor een SIEM-connector
- Naleving van beleid na verloop van tijd

## <a name="continuous-security-assessment"></a>Continue beveiligingsevaluatie
De sectie Resource beveiligingshygiëne onder **Security Center - Overzicht** biedt een snelle weergave van uw resources beveiligingshygiëne, de weergave van het aantal geïdentificeerde problemen en de beveiligingsstatus voor elk resourcetype. Continue evaluatie helpt u bij het detecteren van mogelijke beveiligingsproblemen, zoals systemen met ontbrekende beveiligingsupdates of netwerkpoorten die risico lopen.

### <a name="secure-score"></a>Beveiligingsscore
De beveiligingsscore van Azure Security Center geeft u aanbevelingen voor beveiliging en bepaalt de volgorde van deze voor u, zodat u weet welke aanbevelingen het eerst moeten worden uitgevoerd. Hiermee ziet u direct de belangrijkste beveiligingsproblemen zodat u weet wat prioriteit heeft. De beveiligingsscore is een hulpprogramma waarmee u de beveiliging kunt aanpassen voor een veilige werkbelasting. Ga voor meer informatie naar [Beveiligingsscore in het Azure Security Center](security-center-secure-score.md).

### <a name="health-monitoring"></a>Statuscontrole
Het selecteren van een resourcetype onder **Statuscontrole resource** geeft een lijst van resources en alle zwakke plekken die zijn geïdentificeerd. Resourcetypen zijn rekenprocessen en toepassingen, netwerken, gegevens en opslag, identiteit en toegang.

We kozen **Rekenprocessen en toepassingen**. Onder **Rekenprocessen** vindt u vier tabbladen:

- **Overzicht**: bewaking en aanbevelingen door Security Center.
- **VM’s en computers**: lijst met alle virtuele machines, computers en de bijbehorende actuele beveiligingsstatussen.
- **Cloud Services**: lijst van al uw web- en werkrollen die worden bewaakt door Security Center.
- **App-services (Preview)**: lijst met uw webtoepassingen en app-service-omgevingen en de huidige beveiligingsstatus van elk.

![Bewaking van beveiligingsstatus][3]

Ga voor meer informatie naar [Bewaking van beveiligingsstatus](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Uitvoerbare aanbevelingen
Security Center analyseert de beveiligingsstatus van uw Azure- en niet-Azure-resources om mogelijke beveiligingsproblemen op te sporen. Als u **Aanbevelingen** kiest onder **Bronnen** krijgt u een lijst met op prioriteit gebaseerde beveiligingsaanbevelingen als hulp u bij het afhandelen van beveiligingsproblemen.

![Aanbevelingen][4]

Ga voor meer informatie naar [Beveiligingsaanbevelingen beheren](security-center-recommendations.md).

### <a name="most-prevalent-recommendations"></a>Meest voorkomende aanbevelingen
De rechterkant van het dashboard onder **Bronnen** geeft lijst met de meest belangrijkste aanbevelingen voor het grootste aantal bronnen. De meest belangrijke aanbevelingen geven aan waarop u uw aandacht moet richten. De rechter pijl biedt de aanbeveling met hoogste impact.

![Meest voorkomende aanbevelingen][11]

Dit is de aanbeveling met de grootste impact in uw omgeving. Het oplossen van deze aanbeveling zal de naleving het meest verbeteren. In dit voorbeeld is de aanbeveling 'schijfversleuteling toepassen'. Het kiezen van **Naleving verbeteren** biedt een beschrijving van de aanbeveling en een lijst met betrokken bronnen.

![Schijfversleuteling toepassen][12]

## <a name="threat-protection"></a>Bescherming tegen bedreigingen
Dit gedeelte biedt meer inzicht in de beveiligingswaarschuwingen die zijn gedetecteerd voor uw bronnen en het beveiligingsniveau voor deze waarschuwingen.

### <a name="prioritized-alerts-and-incidents"></a>Waarschuwingen en incidenten op basis van prioriteit
Security Center maakt gebruik van geavanceerde analyses en informatie over wereldwijde dreigingen om binnenkomende aanvallen en schendingen van de beveiliging te detecteren. Waarschuwingen krijgen een prioriteit en worden gegroepeerd in incidenten, zodat u zich eerst op de meest kritieke bedreigingen kunt richten. U kunt ook uw eigen aangepaste beveiligingswaarschuwingen maken.

Als u **Beveiligingswaarschuwingen op basis van ernst** of **Beveiligingswaarschuwingen op basis van tijd** kiest, krijgt u gedetailleerde informatie over de waarschuwingen.

![Waarschuwingen en incidenten op basis van prioriteit][7]

U kunt snel de omvang en de impact van een aanval beoordelen via een visuele, interactieve onderzoeksmethode en vooraf gedefinieerde of ad hoc-query's gebruiken voor diepergaand onderzoek van de beveiligingsgegevens.

Ga voor meer informatie naar [Beveiligingswaarschuwingen beheren en erop reageren](security-center-managing-and-responding-alerts.md).

De rechterkant van het dashboard biedt informatie over welke waarschuwingen u als eerste moet verhelpen en geeft informatie wat uw algemene beveiligingsproblemen zijn:

- **Meest aangevallen bronnen**: Specifieke bronnen die het hoogst aantal meldingen hebben
- **Belangrijkste waarschuwingen**: Waarschuwingen die betrekking hebben op een groot aantal bronnen

## <a name="just-in-time-vm-access"></a>Just-In-Time-VM-toegang
Verminder het oppervlak voor netwerkaanvallen met just-in-time, gecontroleerde toegang tot beheerpoorten op Azure VM's in om het oppervlak dat wordt blootgesteld aan beveiligingsaanvallen en andere netwerkaanvallen, drastisch te reduceren.

![Just-In-Time-VM-toegang][5]

Geef regels op voor hoe gebruikers verbinding met virtuele machines kunnen maken. Indien nodig, kan toegang worden aangevraagd vanuit Security Center of via PowerShell. Als de aanvraag aan de regels voldoet, wordt automatisch toegang verleend voor de gevraagde tijd.

Zie [Manage virtual machine access using just in time](security-center-just-in-time.md) (VM-toegang beheren met behulp van JIT) voor meer informatie.

## <a name="adaptive-application-controls"></a>Adaptieve toepassingsbesturingselementen
Blokkeer malware en andere ongewenste toepassingen door aanbevelingen voor opname in de whitelist toe te passen die zijn afgestemd op uw specifieke Azure-workloads en gebruikmaken van machine learning.

![Adaptieve toepassingsbesturingselementen][6]

Bekijk en klik op toepassingen om de aanbevolen regels voor de opname in de whitelist die door Security Center zijn gegenereerd toe te passen of de regels die al zijn geconfigureerd te bewerken.

Ga voor meer informatie naar [Adaptieve besturingselementen voor toepassingen](security-center-adaptive-application.md).

## <a name="integrate-your-security-solutions"></a>Uw beveiligingsoplossingen integreren
U kunt beveiligingsgegevens verzamelen, zoeken en analyseren uit diverse bronnen, met inbegrip van verbonden partneroplossingen zoals netwerkfirewalls en andere Microsoft-services in Security Center.

![Beveiligingsoplossingen integreren][8]

Ga voor meer informatie naar [Integreren van beveiligingsoplossingen](security-center-partner-integration.md).

## <a name="next-steps"></a>Volgende stappen

- U moet over een Microsoft Azure-abonnement beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- De prijscategorie Gratis van Security Center wordt met uw Azure-abonnement ingeschakeld. Als u wilt profiteren van geavanceerd beveiligingsbeheer en mogelijkheden voor het detecteren van bedreigingen, moet u een upgrade uitvoeren naar de prijscategorie Standard. De prijscategorie Standard is gedurende de eerste 60 dagen gratis. Zie de [pagina met prijzen van Security Center](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.
- Als u nu Security Center Standard wilt inschakelen, raadpleegt u de [Quickstart: Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Snelstartgids: uw Azure-abonnement registreren voor Security Center Standard) voor meer informatie.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
[9]: ./media/security-center-intro/coverage.png
[10]: ./media/security-center-intro/policy-management.png
[11]: ./media/security-center-intro/highest-impact.png
[12]: ./media/security-center-intro/apply-disk-encryption.png

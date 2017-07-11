---
title: Plannings- en bedieningsgids voor het Beveiligingscentrum | Microsoft Docs
description: Dit document helpt u met uw planning voordat u overstapt op het Azure Beveiligingscentrum en bevat aandachtspunten voor het dagelijks gebruik.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: c502e4363dbaa37455d1aad90d1e9fa855fd09b0
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017


---
<a id="azure-security-center-planning-and-operations-guide" class="xliff"></a>

# Plannings- en bedieningsgids voor Azure Security Center
Deze gids is bedoeld voor IT-specialisten, IT-architecten, gegevensbeveiligingsanalisten en cloudbeheerders die willen gaan werken met Azure Security Center.

>[!NOTE] 
>Vanaf begin juni 2017 zal Security Center de Microsoft Monitoring Agent gebruiken voor het verzamelen en opslaan van gegevens. Zie [Migratie van Azure Security Center-platform](security-center-platform-migration.md) voor meer informatie. De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>

<a id="planning-guide" class="xliff"></a>

## Planningsgids
Deze gids bevat een reeks stappen en taken die u kunt volgen om uw gebruik van Azure Security Center te optimaliseren op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie. Als u optimaal wilt profiteren van Security Center, is het belangrijk te begrijpen hoe verschillende personen of teams binnen uw organisatie de service gaan gebruiken om te voldoen aan alle vereisten voor veilige ontwikkeling en gebruik, bewaking, bestuur en reactie op incidenten. De belangrijkste gebieden waarmee u rekening moet houden bij het plannen van het gebruik van Security Center zijn:

* Beveiligingsrollen en toegangsbeheer
* Beveiligingsbeleid en aanbevelingen
* Gegevensverzameling en -opslag
* Continue beveiligingsbewaking
* Reageren op incidenten

In de volgende sectie leert u hoe u voor elk van deze gebieden een planning maakt en hoe u deze aanbevelingen toepast op grond van uw vereisten.

> [!NOTE]
> Raadpleeg de [Azure Security Center frequently asked questions (Veelgestelde vragen over het Azure Beveiligingscentrum)](security-center-faq.md) voor een lijst met veelgestelde vragen die ook nuttig kunnen zijn tijdens de ontwerp- en planningsfase.
> 

<a id="security-roles-and-access-controls" class="xliff"></a>

## Beveiligingsrollen en toegangsbeheer
Afhankelijk van de grootte en de structuur van uw organisatie kunnen meerdere individuen en teams het Beveiligingscentrum gebruiken om verschillende beveiligingstaken uit te voeren. In het volgende diagram vindt u een voorbeeld met fictieve personen en hun respectieve rollen en beveiligingsverantwoordelijkheden:

![Rollen](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Met Security Center kunnen deze personen voldoen aan deze verschillende verantwoordelijkheden. Bijvoorbeeld:

**Jeff (eigenaar van workloads in de cloud)**

* Een cloud-workload en de bijbehorende resources beheren
* Verantwoordelijk voor het implementeren en onderhouden van beveiliging volgens het beveiligingsbeleid van het bedrijf

**Ellen (CISO/CIO)**

* Verantwoordelijk voor alle beveiligingsaspecten van het bedrijf
* Wil de beveiliging van het bedrijf in alle cloud-workloads begrijpen
* Moet worden geïnformeerd over belangrijke aanvallen en risico 's

**David (IT-beveiliging)**

* Stelt beveiligingsbeleid in om er zeker van te zijn dat de juiste beveiliging aanwezig is
* Bewaakt naleving van beleid
* Genereert rapporten voor leidinggevenden of auditors

**Judy (beveiligingsbewerkingen)**

* Bewaakt en reageert 24/7 op beveiligingswaarschuwingen
* Escaleert naar eigenaar cloud-workload of IT-beveiligingsanalist

**Sam (beveiligingsanalist)**

* Onderzoekt aanvallen
* Werkt samen met de eigenaar van de workload in de cloud om herstelstappen toe te passen 

Security Center gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md), dat [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot resources waartoe ze toegang hebben. Dit betekent dat aan de gebruiker de rol van de eigenaar, bijdrager of lezer is toegewezen voor het abonnement of de resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- **Beveiligingslezer**: een gebruiker die deel uitmaakt van deze rol heeft weergaverechten voor Security Center, en kan dus onder andere aanbevelingen, waarschuwingen, beleid en statusinformatie bekijken, maar kan geen wijzigingen aanbrengen.
- **Beveiligingsbeheerder**: dezelfde machtigingen als de rol Beveiligingslezer, maar kan ook het beveiligingsbeleid bijwerken, en aanbevelingen en waarschuwingen verwijderen.

De hierboven beschreven Security Center-rollen hebben geen toegang tot andere servicegebieden van Azure, zoals Storage, Web & Mobile of Internet of Things.  

> [!NOTE]
> Een gebruiker moet ten minste een abonnementseigenaar, resourcegroepeigenaar of medewerker zijn om Security Center in Azure te kunnen zien. 
> 
> 

Wanneer gebruik wordt gemaakt van de personen in het vorige diagram, zou de volgende RBAC nodig zijn:

**Jeff (eigenaar van workloads in de cloud)**

* Resourcegroepeigenaar/medewerker

**David (IT-beveiliging)**

* Abonnementseigenaar/medewerker of beveiligingsbeheerder

**Judy (beveiligingsbewerkingen)**

* Abonnementslezer of beveiligingslezer voor het bekijken van waarschuwingen
* Abonnementseigenaar/medewerker of beveiligingsbeheerder voor het verwijderen van waarschuwingen

**Sam (beveiligingsanalist)**

* Abonnementslezer voor het bekijken van waarschuwingen
* Abonnementseigenaar/medewerker voor het verwijderen van waarschuwingen
* Toegang tot de werkruimte is mogelijk vereist

Andere belangrijke informatie om rekening mee te houden:

* Alleen abonnementseigenaren/medewerkers en beveiligingsbeheerders kunnen een beveiligingsbeleid bewerken
* Alleen abonnements- en resourcegroepeigenaren en bijdragers kunnen beveiligingsaanbevelingen voor een resource toepassen

Bij het plannen van het toegangsbeheer met het RBAC voor Security Center moet u zeker weten wie in uw organisatie het Security Center gaat gebruiken. Ook moet u weten welke typen taken zij gaan uitvoeren en RBAC vervolgens dienovereenkomstig configureren.

> [!NOTE]
> We raden u aan de rol toe te wijzen die gebruikers minimaal nodig hebben om hun taken uit te voeren. Wijs bijvoorbeeld gebruikers die alleen informatie hoeven te bekijken over de beveiligingstoestand van resources, maar geen maatregelen hoeven te nemen zoals het toepassen van aanbevelingen of het bewerken van beleid, de rol Lezer toe.
> 
> 

<a id="security-policies-and-recommendations" class="xliff"></a>

## Beveiligingsbeleid en aanbevelingen
Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement. In Security Center definieert u de beleidsregels op grond van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens.

Beleidsregels die zijn ingeschakeld op abonnementsniveau, worden automatisch doorgegeven naar alle resourcegroepen binnen het abonnement, zoals weergegeven in het volgende diagram:

![Beveiligingsbeleid](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Als u moet controleren welke beleidsregels zijn gewijzigd, kunt u dat doen met de [Azure controlelogboeken](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Beleidswijzigingen worden altijd geregistreerd in Azure controlelogboeken.
> 
> 

<a id="security-recommendations" class="xliff"></a>

### Aanbevelingen voor beveiliging
Voordat u beleidsregels voor veiligheid configureert, moet u elk van de [aanbevelingen voor beveiliging](security-center-recommendations.md) controleren en bepalen of deze beleidsregels geschikt zijn voor uw verschillende abonnementen en resourcegroepen. Het is ook belangrijk om te begrijpen welke actie moet worden ondernomen om aan de slag te gaan met [beveiligingsaanbevelingen](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) en wie in uw organisatie verantwoordelijk is voor het controleren op nieuwe aanbevelingen en het nemen van de benodigde stappen.

Security Center raadt u aan om voor de beveiliging contactgegevens voor uw Azure-abonnement te verstrekken. Deze informatie wordt gebruikt door Microsoft om contact met u op te nemen als door Microsoft Security Response Center (MSRC) wordt gedetecteerd dat uw klantgegevens zijn geopend door een illegale of niet-geautoriseerde derde. Lees [Contactgegevens voor beveiliging verstrekken in Azure Security Center](security-center-provide-security-contact-details.md) voor meer informatie over het inschakelen van deze mogelijkheid.

<a id="data-collection-and-storage" class="xliff"></a>

## Gegevensverzameling en -opslag
Azure Security Center maakt gebruik van de Microsoft Monitoring Agent. Dit is dezelfde agent die ook wordt gebruikt door de Operations Management Suite en de Log Analytics-service om beveiligingsgegevens van uw virtuele machines te verzamelen. Gegevens die worden verzameld van deze agent worden opgeslagen in uw Log Analytics-werkruimte(n).

<a id="agent" class="xliff"></a>

### Agent

Nadat gegevensverzameling is ingeschakeld in het beveiligingsbeleid, wordt de Microsoft Monitoring Agent (voor [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) of [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) geïnstalleerd op alle ondersteunde virtuele machines van Azure en op nieuwe virtuele machines die worden gemaakt.  Als de Microsoft Monitoring Agent al op de virtuele machine is geïnstalleerd, maakt Azure Security Center gebruik van de huidige geïnstalleerde agent. Het proces van de agent is ontworpen om niet-invasief te zijn en minimale invloed te hebben op de prestaties van de virtuele machine.

De Microsoft Monitoring Agent voor Windows vereist TCP-poort 443. Zie de [Handleiding voor het oplossen van problemen met Azure Security Center](security-center-troubleshooting-guide.md) voor meer informatie.

Als u gegevensverzameling op een bepaald moment wilt uitschakelen, kunt u dat doen in het beveiligingsbeleid. Omdat de Microsoft Monitoring Agent kan worden gebruikt door andere Azure-services voor beheer en controle, wordt de agent niet automatisch verwijderd wanneer u het verzamelen van gegevens uitschakelt in Security Center. U kunt de agent indien nodig echter handmatig verwijderen.

> [!NOTE]
> Lees de [veelgestelde vragen over Azure Security Center](security-center-faq.md) (Engelstalig) als u een lijst met ondersteunde virtuele machines nodig hebt.
> 

<a id="workspace" class="xliff"></a>

### Werkruimte

Gegevens die (namens Azure Security Center) via de Microsoft Monitoring Agent worden verzameld, worden opgeslagen in een bestaande Log Analytics-werkruimte die is gekoppeld aan uw Azure-abonnement, of in een nieuwe werkruimte, rekening houdend met de geolocatie van de virtuele machine. 

In Azure Portal kunt u bladeren om een overzicht te zien van uw Log Analytics-werkruimten, inclusief alle werkruimten die door Azure Security Center zijn gemaakt. Er wordt een gerelateerde resourcegroep gemaakt voor nieuwe werkruimten. Beide volgen deze naamconventie: 

* Werkruimte: *DefaultWorkspace-[abonnements-id]-[geolocatie]*
* Resourcegroep: *DefaultResouceGroup-[geolocatie]*

Voor werkruimten die zijn gemaakt door Azure Security Center worden gegevens 30 dagen bewaard. Voor bestaande werkruimten is de bewaarperiode gebaseerd op de prijscategorie van de werkruimte.

> [!NOTE]
> Microsoft doet er alles aan om de privacy van gegevens te beschermen en deze gegevens te beveiligen. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service. Lees [Gegevensbeveiliging van Azure Security Center](security-center-data-security.md) voor meer informatie over de verwerking van gegevens en privacy.
> 

<a id="ongoing-security-monitoring" class="xliff"></a>

## Continue beveiligingsbewaking
Na de eerste configuratie en toepassing van aanbevelingen voor Security Center, komen in de volgende stap de operationele processen voor Security Center ter sprake.

Als u vanuit Azure Portal naar Security Center wilt gaan, klikt u op **Bladeren** en typt u **Beveiligingscentrum** in het veld **Filter**. De weergaven die de gebruiker krijgt, zijn op basis van deze toegepaste filters. In het volgende voorbeeld ziet u een omgeving waarin heel wat problemen moeten worden opgelost:

![dashboard](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Security Center heeft geen invloed op uw normale operationele procedures. Het bewaakt uw implementaties passief en doet aanbevelingen op grond van het beveiligingsbeleid dat u hebt ingeschakeld.

Wanneer u Security Center voor het eerst inschakelt voor uw huidige Azure-omgeving, moet u alle aanbevelingen doornemen. Dit kunt u doen op de tegel **Aanbevelingen** of per resource (**Compute**, **Netwerken**, **Opslag en gegevens** en **Toepassingen**).

Zodra u alle aanbevelingen hebt toegepast, wordt de sectie **Preventie** groen voor alle toegepaste resources. Continue bewaking wordt op dit punt eenvoudiger omdat u alleen acties onderneemt op basis van wijzigingen in de resourcebeveiligingsstatus en aanbevelingstegels.

De sectie **Detectie** is meer reactief en betreft waarschuwingen over problemen die nu plaatsvinden of hebben plaatsgevonden in het verleden en zijn gedetecteerd bij controles door Security Center of door systemen van derden. Met de tegel Beveiligingswaarschuwingen wordt een staafdiagram weergegeven dat het aantal dagelijks geconstateerde waarschuwingen voor gedetecteerde bedreigingen en de verdeling ervan over verschillende ernstigheidscategorieën (laag, gemiddeld, hoog) aangeeft. Lees voor meer informatie over beveiligingswaarschuwingen [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> U kunt uw gegevens uit Security Center ook visualiseren met Microsoft Power BI. Lees [Managing and responding to security alerts in Azure Security Center](security-center-powerbi.md).
> 
> 

<a id="monitoring-for-new-or-changed-resources" class="xliff"></a>

### Bewaking voor nieuwe of gewijzigde resources
De meeste Azure-omgevingen zijn dynamisch, met nieuwe resources die regelmatig omhoog en omlaag worden verplaatst, met configuraties of wijzigingen enz. Security Center zorgt ervoor dat de beveiligingsstatus van deze nieuwe resources voor u goed zichtbaar is.

Wanneer u nieuwe resources (virtuele machines, SQL Databases) toevoegt aan uw Azure-omgeving, detecteert Security Center deze resources automatisch en begint het de beveiliging ervan te bewaken. Dit omvat ook PaaS-webrollen en -werkrollen. Als gegevensverzameling wordt ingeschakeld in het [beveiligingsbeleid](security-center-policies.md), worden er automatisch extra bewakingsmogelijkheden ingeschakeld voor uw virtuele machines.

![Belangrijke gebieden](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Voor virtuele machines klikt u op **Compute**, in het gedeelte **Preventie**. Problemen bij het inschakelen van gegevensverzameling of verwante aanbevelingen worden weergegeven op het tabblad **Overzicht**, in het gedeelte **Aanbevelingen ten aanzien van controle**.
2. Geef de **aanbevelingen** weer om te zien welke beveiligingsrisico's mogelijk zijn geïdentificeerd voor de nieuwe resource.
3. Het is heel gebruikelijk dat wanneer nieuwe virtuele machines worden toegevoegd aan uw omgeving, eerst alleen het besturingssysteem wordt geïnstalleerd. De resource-eigenaar heeft wellicht enige tijd nodig om andere apps te implementeren die door deze VM’s worden gebruikt.  In het ideale geval moet u de uiteindelijke doelstelling van deze workload kennen. Wordt het een toepassingsserver? Op grond van wat deze nieuwe workload gaat worden, kunt u het juiste **beveiligingsbeleid** inschakelen. Dit is de derde stap in deze werkstroom.
4. Naarmate nieuwe resources worden toegevoegd aan uw Azure-omgeving, kunnen er nieuwe waarschuwingen worden weergegeven in de tegel **Beveiligingswaarschuwingen**. Controleer altijd of er nieuwe waarschuwingen in deze tegel worden weergegeven en neem maatregelen op grond van de aanbevelingen van Security Center.

U zult ook regelmatig de status van bestaande resources willen controleren om configuratiewijzigingen te identificeren die hebben geleid tot beveiligingsrisico's, afwijking van aanbevolen basislijnen en beveiligingswaarschuwingen. Begin bij het dashboard van Security Center. Van daaruit hebt u drie belangrijke gebieden die u consequent moet controleren.

![Bewerkingen](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Het venster **Preventie** biedt snel toegang tot uw belangrijkste resources. Gebruik deze optie voor het bewaken van Compute, Netwerken, Opslag en gegevens en Toepassingen.
2. In het venster **Aanbevelingen** kunt u de aanbevelingen van Security Center controleren. Tijdens de continue bewaking merkt u misschien dat u niet dagelijks aanbevelingen krijgt. Dit is normaal omdat u alle aanbevelingen hebt gevolgd bij de eerste configuratie van Security Center. Daarom bevat deze sectie niet elke dag nieuwe informatie en hoeft u deze alleen te raadplegen wanneer dat nodig is.
3. Het venster **Detectie** kan ofwel heel vaak, of zeer incidenteel veranderen. Controleer altijd uw beveiligingswaarschuwingen en neem maatregelen op grond van de aanbevelingen van Security Center.

<a id="incident-response" class="xliff"></a>

## Reageren op incidenten
Security Center detecteert en waarschuwt voor bedreigingen wanneer deze optreden. Organisaties moeten controleren op nieuwe beveiligingswaarschuwingen en zo nodig maatregelen nemen om het probleem verder te onderzoeken of de aanval af te weren. Lees [Detectiemogelijkheden van Azure Security](security-center-detection-capabilities.md) voor meer informatie over de werking van detectie van bedreigingen van Azure Security Center.

Hoewel dit artikel niet de bedoeling heeft om u te helpen uw eigen plan voor het reageren op incidenten te maken, maken we gebruik van Microsoft Azure Security Response in de cloud-levenscyclus als de basis voor fasen voor het reageren op incidenten. In het volgende diagram ziet u de fasen:

![Verdachte activiteit](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> U kunt de [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) van het National Institute of Standards and Technology (NIST) gebruiken als richtlijn om uw eigen plan te ontwikkelen.
> 

U kunt waarschuwingen van Security Center gebruiken tijdens de volgende fasen:

* **Detecteren**: een verdachte activiteit in een of meer resources identificeren. 
* **Beoordelen**: de eerste beoordeling uitvoeren voor meer informatie over de verdachte activiteiten.
* **Diagnose uitvoeren**: de herstelstappen gebruiken voor het uitvoeren van de technische procedure om het probleem op te lossen.

Elke beveiligingswaarschuwing bevat informatie die kan worden gebruikt om beter te begrijpen wat de aard van de aanval is en om mogelijke oplossingen voor te stellen. Sommige waarschuwingen bevatten ook koppelingen naar meer informatie of naar andere informatiebronnen binnen Azure. U kunt de weergegeven informatie gebruiken voor verder onderzoek en om te beginnen met risicobeperking. Bovendien kunt u zoeken in beveiligingsgegevens die zijn opgeslagen in uw werkruimte.

Het volgende voorbeeld betreft een verdachte RDP-activiteit die op dat moment plaatsvindt:

![Verdachte activiteit](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Zoals u ziet, bevat deze blade informatie over het tijdstip waarop de aanval plaatsvond, de hostnaam van de bron, de VM die het doelwit was en aanbevolen maatregelen. Onder bepaalde omstandigheden kunnen de brongegevens van de aanval leeg zijn. Lees [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) voor meer informatie over dit type gedrag.

In de video [Reageren op incidenten met Azure Security Center & Microsoft Operations Management Suite](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) ziet u enkele demonstraties waarmee u kunt begrijpen hoe Security Center kan worden gebruikt in elk van deze fasen.

> [!NOTE]
> Lees [Azure Security Center gebruiken voor reacties op incidenten](security-center-incident-response.md) voor meer informatie over hoe u Security Center kunt gebruiken wanneer u reageert op incidenten. 
> 
> 

<a id="see-also" class="xliff"></a>

## Zie ook
In dit document hebt u kunnen lezen hoe u een planning kunt maken voor het overstappen op Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.



---
title: FedRAMP Azure blauwdruk Automation - controle en verantwoordelijkheid
description: Webtoepassingen voor FedRAMP - controle en verantwoordelijkheid
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: c5858e07-ca74-4526-b31f-e6b4e55bb594
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 83ef9cbb7652bf128d7758237a8e6fbeed6c6565
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="audit-and-accountability-au"></a>Controle en verantwoordelijkheid (AU)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-au-1"></a>NIST 800 53 besturingselement Australië-1

#### <a name="audit-and-accountability-policy-and-procedures"></a>Controle en verantwoordelijkheid beleid en Procedures

**Australië-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een controle en verantwoordelijkheid beleid die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management-streven samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van de audit en de accountability beleid en de bijbehorende controlegegevens en accountability controles; en controleert en updates van het huidige beleid voor controle en verantwoordelijkheid [toewijzing: organisatie gedefinieerde frequentie]; en -procedures voor controle en verantwoordelijkheid [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De controle en verantwoordelijkheid beleid op bedrijfsniveau en -procedures van de klant mogelijk voldoende zijn voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-2a"></a>NIST 800 53 besturingselement Australië-2.

#### <a name="audit-events"></a>Controlegebeurtenissen

**Australië-2.** de organisatie vaststelt dat het informatiesysteem is geschikt voor controle van de volgende gebeurtenissen: [toewijzing: controleerbare gebeurtenissen organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Audit mogelijkheid voor deze Azure blauwdruk wordt verstrekt door Azure bewaken en de Log Analytics-service in OMS. Azure biedt een gedetailleerde controlelogboeken over activiteit die is gekoppeld aan het geïmplementeerde resources. Deze en logboeken voor OS-niveau worden verzameld door Log Analytics en opgeslagen in de OMS-opslagplaats. Log Analytics correleert controlegegevens op de resources die zijn geïmplementeerd met deze oplossing en kan worden uitgebreid naar de webtoepassing die door de klant is geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-2b"></a>NIST 800 53 besturingselement Australië-2.b

#### <a name="audit-events"></a>Controlegebeurtenissen

**Australië-2.b** de organisatie coördineert de auditfunctie beveiliging met andere organisatie entiteiten audit-gerelateerde informatie voor het verbeteren van wederzijdse ondersteuning en om u te helpen de selectie van controleerbare gebeurtenissen vereisen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een tot stand gebrachte op bedrijfsniveau-proces waarmee wordt bepaald controleerbare gebeurtenissen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-2c"></a>NIST 800 53 besturingselement Australië-2.c

#### <a name="audit-events"></a>Controlegebeurtenissen

**Australië-2.c** de organisatie biedt een logica voor waarom de controleerbare gebeurtenissen worden geacht voldoende voor de ondersteuning van na-de-fact onderzoeken beveiligingsincidenten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Gebeurtenissen die worden gecontroleerd door deze blauwdruk Azure bevatten informatie die voldoende zijn om te bepalen wanneer gebeurtenissen plaatsvinden, de bron van de gebeurtenis, het resultaat van de gebeurtenis en andere gedetailleerde informatie die ondersteuning biedt voor onderzoek beveiligingsincidenten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-2d"></a>NIST 800 53 besturingselement Australië-2.d

#### <a name="audit-events"></a>Controlegebeurtenissen

**Australië-2.d** de organisatie vaststelt dat de volgende gebeurtenissen moeten worden gecontroleerd vanuit het informatiesysteem: [toewijzing: gebeurtenissen (de subset van de controleerbare gebeurtenissen die zijn gedefinieerd in Australië 2 a.) samen met de frequentie van de organisatie gedefinieerd gecontroleerd (of situation requiring) controle-instellingen voor elke gebeurtenis geïdentificeerde].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Gebeurtenissen die worden gecontroleerd door deze blauwdruk Azure bevatten die worden gecontroleerd door Azure activiteitenlogboeken voor geïmplementeerde resources, OS-niveau Logboeken, Active Directory-logboeken en logboeken van de SQL Server. Klanten kunnen aanvullende gebeurtenissen moeten worden gecontroleerd om te voldoen aan de behoeften van de missie selecteren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-2-3"></a>NIST 800 53 besturingselement Australië-2 (3)

#### <a name="audit-events--reviews-and-updates"></a>Controlegebeurtenissen | Revisies en Updates

**Australië-2 (3)** de organisatie controleert en updates van de gecontroleerde gebeurtenissen [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant mogelijk zijn afhankelijk van een periodieke controle van tot stand gebrachte op bedrijfsniveau en proces voor de gedefinieerde set controlegebeurtenissen niet bijwerken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-3"></a>NIST 800 53 besturingselement Australië-3

#### <a name="content-of-audit-records"></a>Inhoud van AuditRecords

**Australië-3** wordt de informatie gegenereerd controlerecords dat informatie bevat die welk type gebeurtenis is opgetreden bepaalt, bij de gebeurtenis heeft plaatsgevonden, waarop de gebeurtenis heeft plaatsgevonden, de bron van de gebeurtenis, de uitkomst van de gebeurtenis en de identiteit van een personen of onderwerpen die zijn gekoppeld aan de gebeurtenis.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk afhankelijk van de ingebouwde audit-mogelijkheden van Azure, Windows Server en SQL Server. Deze controle oplossingen vastleggen controlerecords met voldoende details om te voldoen aan de vereisten van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-3-1"></a>NIST 800 53 besturingselement Australië-3 (1)

#### <a name="content-of-audit-records--additional-audit-information"></a>Inhoud van AuditRecords | Als u meer controle-informatie

**Australië-3 (1)** controlerecords met de volgende aanvullende informatie wordt de informatie gegenereerd: [toewijzing: organisatie gedefinieerd aanvullende, meer gedetailleerde informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Een gedetailleerde schema met velden voor meer dan 20 soorten controle-informatie wordt gebruikt door Azure Activity Log-gebeurtenissen. Naast het activiteitenlogboek implementeert u deze blauwdruk Azure de Log Analytics-oplossing in OMS die ondersteuning biedt voor een grote verscheidenheid aan gegevensbronnen, met inbegrip van Logboeken van Windows, Linux-Logboeken, Azure Diagnostics-logboeken en logboeken van de klant.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-3-2"></a>NIST 800 53 besturingselement Australië-3 (2)

#### <a name="content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Inhoud van AuditRecords | Gecentraliseerd beheer van geplande controle Record inhoud

**Australië-3 (2)** het informatiesysteem biedt gecentraliseerd beheer en configuratie van de inhoud kan worden vastgelegd in controlerecords gegenereerd door [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Alle virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure zijn toegevoegd aan het geïmplementeerde Active Directory-domein. Alle domein-virtuele machines implementeren van een Groepsbeleid die kan worden geconfigureerd voor de systeemconfiguratie van de OS-niveau audit centraal te beheren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-4"></a>NIST 800 53 besturingselement Australië-4

#### <a name="audit-storage-capacity"></a>Opslagcapaciteit controleren

**Australië-4** de organisatie wijst audit record opslagcapaciteit in overeenstemming met [toewijzing: de record opslagvereisten audit organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk wijst voldoende opslagcapaciteit voor het bewaren van AuditRecords gedurende een periode van één jaar. Alle controlerecords worden verzameld door logboekanalyse die is geconfigureerd voor het bewaren van één jaar. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-5a"></a>NIST 800 53 besturingselement Australië-5.

#### <a name="response-to-audit-processing-failures"></a>Antwoord om te controleren met verwerkingsfouten

**Australië-5.** de signalen informatie [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] in het geval van een audit verwerkingsfout.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Status van de service voor Azure Monitor en Log Analytics is beschikbaar op de van de Azure-statuswebsite en de blade van de health service in de Azure portal. Waarschuwingen kunnen worden geconfigureerd via logboekanalyse worden gemeld van andere typen van audit verwerkingsfouten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-5b"></a>NIST 800 53 besturingselement Australië-5.b

#### <a name="response-to-audit-processing-failures"></a>Antwoord om te controleren met verwerkingsfouten

**Australië-5.b** het informatiesysteem de volgende aanvullende acties uitgevoerd: [toewijzing: organisatie gedefinieerd acties moeten worden uitgevoerd (bijvoorbeeld informatiesysteem afsluiten, overschrijven oudste controlerecords stoppen met het genereren van AuditRecords)].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Alle controlerecords gegenereerd door de bronnen die zijn geïmplementeerd door deze blauwdruk Azure worden verzameld door Log Analytics en bewaard gedurende een periode van één jaar. De opslagtoewijzing voor deze record audit-opslag wordt dynamisch toegewezen gezorgd voldoende capaciteit is beschikbaar. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-5-1"></a>NIST 800 53 besturingselement Australië-5 (1)

#### <a name="response-to-audit-processing-failures--audit-storage-capacity"></a>Antwoord om te controleren verwerkingsfouten | Opslagcapaciteit controleren

**Australië-5 (1)** het informatiesysteem biedt een waarschuwing [toewijzing: organisatie gedefinieerd personeel, rollen en/of locaties] in [toewijzing: organisatie gedefinieerde periode] wanneer toegewezen audit record opslagvolume [bereikt Toewijzing: organisatie gedefinieerde percentage] van opslagplaats maximale audit record opslagcapaciteit.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Alle controlerecords gegenereerd door de bronnen die zijn geïmplementeerd door deze blauwdruk Azure worden verzameld door Log Analytics en bewaard gedurende een periode van één jaar. De opslagtoewijzing voor deze record audit-opslag wordt dynamisch toegewezen gezorgd voldoende capaciteit is beschikbaar. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-5-2"></a>NIST 800 53 besturingselement Australië-5 (2)

#### <a name="response-to-audit-processing-failures--real-time-alerts"></a>Antwoord om te controleren verwerkingsfouten | Realtime-waarschuwingen

**Australië-5 (2)** het informatiesysteem biedt een waarschuwing in [toewijzing: realtime periode organisatie gedefinieerd] naar [toewijzing: organisatie gedefinieerd personeel, rollen en/of locaties] wanneer de volgende controlegebeurtenissen fout optreden: [toewijzing: organisatie gedefinieerde fout controlegebeurtenissen vereisen van realtime-waarschuwingen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Status van de service voor Azure is beschikbaar op de blade van de health service in de Azure portal. Waarschuwingen kunnen worden geconfigureerd via logboekanalyse worden gemeld van andere typen van audit verwerkingsfouten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-6a"></a>NIST 800 53 besturingselement Australië-6.a

#### <a name="audit-review-analysis-and-reporting"></a>Audit controle, analyse en rapportage

**Australië-6.a** de organisatie controleert en analyseert gegevens system controlerecords [toewijzing: organisatie gedefinieerde frequentie] nakijken op aanwijzingen van [toewijzing: organisatie gedefinieerd ongeschikte of ongebruikelijke activiteiten].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beoordelen en analyseren van controlerecords van klant geïmplementeerde resources (om op te nemen toepassingen, besturingssystemen, databases en software). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-6b"></a>NIST 800 53 besturingselement Australië-6.b

#### <a name="audit-review-analysis-and-reporting"></a>Audit controle, analyse en rapportage

**Australië-6.b** de organisatie bevindingen rapporten [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor rapportage over bevindingen van onjuiste of ongebruikelijke activiteiten (gedefinieerd in Australië 06.a) van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-1"></a>NIST 800 53 besturingselement Australië-6 (1)

#### <a name="audit-review-analysis-and-reporting--process-integration"></a>Controleer, analyse en rapportage van de audit | Procesintegratie

**Australië-6 (1)** geautomatiseerde mechanismen voor het integreren van audit controle, analyse en rapportage van processen voor ondersteuning van organisatie-processen voor onderzoek en reactie op verdachte activiteiten maakt gebruik van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een gecentraliseerde audit op bedrijfsniveau controle, analyse en rapportagemogelijkheid. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-3"></a>NIST 800 53 besturingselement Australië-6 (3)

#### <a name="audit-review-analysis-and-reporting--correlate-audit-repositories"></a>Controleer, analyse en rapportage van de audit | Audit-opslagplaatsen correleren

**Australië-6 (3)** analyseert de organisatie en standaarddocumentnaam controleren records over verschillende opslagplaatsen situationeel bewustzijn binnen de gehele organisatie te krijgen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-oplossing in OMS te centraliseren controlegegevens op de geïmplementeerde resources binnen de gehele organisatie situationeel bewustzijn ondersteunen. Klanten kunnen worden gekozen om logboekanalyse verder te integreren met andere systemen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-4"></a>NIST 800 53 besturingselement Australië-6 (4)

#### <a name="audit-review-analysis-and-reporting--central-review-and-analysis"></a>Controleer, analyse en rapportage van de audit | Centrale controle en analyse

**Australië-6 (4)** het informatiesysteem biedt de mogelijkheid centraal controleren en analyseren van AuditRecords uit meerdere onderdelen in het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-oplossing in OMS te centraliseren controlegegevens over geïmplementeerde resources, ondersteunende gecentraliseerde controle, analyse en rapportage. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-5"></a>NIST 800 53 besturingselement Australië-6 (5)

#### <a name="audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>Controleer, analyse en rapportage van de audit | Integratie / scannen en bewakingsmogelijkheden

**Australië-6 (5)** de organisatie analyse van AuditRecords integreert met analyse van [selectie (een of meer): beveiligingslek scannen informatie; prestatiegegevens; informatie system controlegegevens; [Toewijzing: organisatie gedefinieerd/gegevens verzameld uit andere bronnen]] voor het verder verbeteren de mogelijkheid om u te identificeren ongeschikte of ongebruikelijke activiteiten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS-beveiligings- en Audit-oplossing. Deze oplossing biedt een uitgebreid overzicht van beveiliging. De beveiligings- en Audit dashboard biedt op hoog niveau inzicht in de beveiligingsstatus van geïmplementeerde resources met gegevens die beschikbaar zijn voor geïmplementeerde OMS-oplossingen integreren logboekgegevens en beveiligingslek gegevens uit de basislijn en patch-evaluatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-6"></a>NIST 800 53 besturingselement Australië-6 (6)

#### <a name="audit-review-analysis-and-reporting--correlation-with-physical-monitoring"></a>Controleer, analyse en rapportage van de audit | Correlatie met fysieke bewaking

**Australië-6 (6)** de organisatie informatie van AuditRecords met informatie verkregen van de bewaking van de fysieke toegang tot het verder verbeteren van de mogelijkheid om u te identificeren verdachte, ongeschikte, ongebruikelijke of kwaadwillige activiteit correleert.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | SIM-team van Microsoft Azure maakt gebruik van de bijbehorende fysieke bewakingsgegevens en verbindt deze met controlerecords om te bepalen of er was een inbreuk op de gekoppelde logische of verdacht gedrag wanneer fysieke incidenten worden gedetecteerd. |


 ### <a name="nist-800-53-control-au-6-7"></a>NIST 800 53 besturingselement Australië-6 (7)

#### <a name="audit-review-analysis-and-reporting--permitted-actions"></a>Controleer, analyse en rapportage van de audit | Toegestane acties

**Australië-6 (7)** de organisatie Hiermee geeft u de toegestane acties voor elke [selectie (een of meer): informatie systeemproces; rol; gebruiker] die is gekoppeld aan de evaluatie, analyse en rapportage van controle-informatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows virtuele machines zijn geïmplementeerd door deze blauwdruk Azure implementeren de acties die een gebruiker uitvoeren met de controlegegevens ten opzichte van kunt machtigingen voor OS-niveau. In Azure, gebruikers of groepen van gebruikers te beperken welke acties beschikbaar met betrekking tot alle resources kunnen worden toegewezen aan functies (bijvoorbeeld eigenaar, bijdrager, lezer of een aangepaste beveiligingsrol) of oplossingen, inclusief Log Analytics geïmplementeerd.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-6-10"></a>NIST 800 53 besturingselement Australië-6 (10)

#### <a name="audit-review-analysis-and-reporting--audit-level-adjustment"></a>Controleer, analyse en rapportage van de audit | Aanpassing van de audit

**Australië-6 (10)** de organisatie past u het niveau van de audit controle, analyse en rapportage in het informatiesysteem wanneer er een wijziging in de risico's op basis van wet afdwinging informatie, intelligence-informatie of andere geloofwaardige informatiebronnen informatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het aanpassen van het niveau van de audit controle, analyse en rapportage voor bronnen klant geïmplementeerd (om op te nemen toepassingen, besturingssystemen, databases en software) als er een wijziging in de risico's op basis van informatie door de wet afdwinging, intelligence of andere geloofwaardige bronnen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-7a"></a>NIST 800 53 besturingselement Australië-7.

#### <a name="audit-reduction-and-report-generation"></a>Audit-reductie en rapporten te genereren

**Australië-7.** het informatiesysteem biedt een vermindering van de audit en de mogelijkheid van een rapport genereren die ondersteuning biedt voor op aanvraag van de audit controleren, analyse en rapportage-vereisten en na-de-fact onderzoeken beveiligingsincidenten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-oplossing in OMS. Log Analytics biedt bewakingsservices voor OMS door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-7b"></a>NIST 800 53 besturingselement Australië-7.b

#### <a name="audit-reduction-and-report-generation"></a>Audit-reductie en rapporten te genereren

**Australië-7.b** het informatiesysteem biedt een audit-reductie en een rapport genereren mogelijkheid die geen invloed heeft op de oorspronkelijke inhoud of de tijd ordening van AuditRecords.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-oplossing in OMS. Log Analytics biedt bewakingsservices voor OMS door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. De inhoud en de tijd ordening van AuditRecords niet worden gewijzigd wanneer verzameld door logboekanalyse. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-7-1"></a>NIST 800 53 besturingselement Australië-7 (1)

#### <a name="audit-reduction-and-report-generation--automatic-processing"></a>Audit-reductie en het genereren van rapporten | Automatische verwerking

**Australië-7 (1)** het informatiesysteem biedt de mogelijkheid voor het verwerken van AuditRecords gebeurtenissen op basis van [toewijzing: organisatie gedefinieerd audit velden binnen controlerecords].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-oplossing in OMS. Log Analytics biedt bewakingsservices voor OMS door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. Log Analytics bevat een krachtige querytaal om gegevens te extraheren die zijn opgeslagen in de opslagplaats. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-8a"></a>NIST 800 53 besturingselement Australië-8.

#### <a name="time-stamps"></a>Tijdstempels

**Australië-8.** informatie wordt gebruikt interne systeemklokken tijdstempels voor controlerecords genereren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Interne systeemklokken resources die zijn geïmplementeerd door deze blauwdruk Azure gebruiken voor het genereren van tijdstempels voor AuditRecords. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-8b"></a>NIST 800 53 besturingselement Australië-8.b

#### <a name="time-stamps"></a>Tijdstempels

**Australië-8.b** het informatiesysteem registreert tijdstempels voor controlerecords die kunnen worden toegewezen aan Coordinated Universal Time (UTC) of Greenwich Mean Time (GMT) en voldoet aan [toewijzing: organisatie gedefinieerd granulatie van tijdmeting].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Interne systeemklokken resources die zijn geïmplementeerd door deze blauwdruk Azure gebruiken voor het genereren van tijdstempels voor AuditRecords. Tijdstempels worden geregistreerd in UTC. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-8-1a"></a>(((1) een van NIST 800 53 besturingselement Australië-8

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Tijdstempels | Synchronisatie met tijdsbron

**Australië-8 (1) een** het informatiesysteem vergelijkt de systeemklokken interne informatie [toewijzing: organisatie gedefinieerde frequentie] met [toewijzing: organisatie gedefinieerd tijdsbron].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Interne systeemklokken resources die zijn geïmplementeerd door deze blauwdruk Azure gebruiken voor het genereren van tijdstempels voor AuditRecords. Interne systeemklokken zijn geconfigureerd om te synchroniseren met een bindende tijdsbron. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-8-1b"></a>(((1) .b van NIST 800 53 besturingselement Australië-8

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Tijdstempels | Synchronisatie met tijdsbron

**Australië-8 (1) .b** het informatiesysteem synchroniseert de interne systeemklokken naar tijdsbron als het tijdsverschil groter dan is [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Interne systeemklokken resources die zijn geïmplementeerd door deze blauwdruk Azure gebruiken voor het genereren van tijdstempels voor AuditRecords. Interne systeemklokken zijn geconfigureerd om te synchroniseren met een bindende tijdsbron. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-9"></a>NIST 800 53 besturingselement Australië-9

#### <a name="protection-of-audit-information"></a>Beveiliging van controle-informatie

**Australië-9** het informatiesysteem audit informatie en audit extra beveiligt tegen onbevoegde toegang, aanpassen en verwijderen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Logische toegang besturingselementen worden gebruikt controle-informatie en hulpmiddelen binnen deze blauwdruk Azure te beschermen tegen onbevoegde toegang, aanpassen en verwijderen. Azure Active Directory wordt afgedwongen goedgekeurde logische toegang met behulp van op rollen gebaseerde groepslidmaatschappen. De mogelijkheid controle-informatie weergeven en gebruiken van controleprogramma's worden beperkt tot gebruikers die deze machtigingen zijn vereist. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-9-2"></a>NIST 800 53 besturingselement Australië-9 (2)

#### <a name="protection-of-audit-information--audit-backup-on-separate-physical-systems--components"></a>Beveiliging van controle-informatie | Back-up op afzonderlijke fysieke systemen audit / onderdelen

**Australië-9 (2)** het systeem gegevens een back-up controlerecords [toewijzing: organisatie gedefinieerde frequentie] op een ander fysiek systeem of systeemonderdeel dan of het onderdeel wordt gecontroleerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-service in OMS. Geïmplementeerde virtuele machines en Azure storage-accounts voor diagnostische gegevens zijn verbonden bronnen met logboekanalyse en behouden afzonderlijk van de oorsprong. Gegevens worden verzameld door OMS in bijna realtime. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-9-3"></a>NIST 800 53 besturingselement Australië-9 (3)

#### <a name="protection-of-audit-information--cryptographic-protection"></a>Beveiliging van controle-informatie | Cryptografische bescherming

**Australië-9 (3)** cryptografische mechanismen ter bescherming van de integriteit van gegevens en audit audit-hulpprogramma's door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-service in OMS. Log Analytics zorgt ervoor dat binnenkomende gegevens van een vertrouwde bron door het valideren van certificaten en de integriteit van gegevens met Azure-verificatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-9-4"></a>NIST 800 53 besturingselement Australië-9 (4)

#### <a name="protection-of-audit-information--access-by-subset-of-privileged-users"></a>Beveiliging van controle-informatie | Toegang door een Subset van bevoegde gebruikers

**Australië-9 (4)** de organisatie gemachtigd voor toegang tot beheer van de functionaliteit voor alleen [toewijzing: organisatie gedefinieerde subset van bevoegde gebruikers].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Logische toegang besturingselementen worden gebruikt controle-informatie en hulpmiddelen binnen deze blauwdruk Azure te beschermen tegen onbevoegde toegang, aanpassen en verwijderen. Azure Active Directory wordt afgedwongen goedgekeurde logische toegang met behulp van op rollen gebaseerde groepslidmaatschappen. De mogelijkheid controle-informatie weergeven en gebruiken van controleprogramma's worden beperkt tot gebruikers die deze machtigingen zijn vereist.
 |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-10"></a>NIST 800 53 besturingselement Australië-10

#### <a name="non-repudiation"></a>Niet-afwijzing

**Australië-10** het informatiesysteem wordt beschermd tegen een persoon (of het proces namens een persoon) onterecht weigeren hebben uitgevoerd [toewijzing: organisatie gedefinieerd acties moeten worden gedekt door onweerlegbaarheid].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Audit mogelijkheid voor deze Azure blauwdruk wordt verstrekt door Azure bewaken en de Log Analytics-service in OMS. Azure biedt een gedetailleerde controlelogboeken over activiteit die is gekoppeld aan het geïmplementeerde resources. Deze en logboeken voor OS-niveau worden verzameld door Log Analytics en opgeslagen in de OMS-opslagplaats. Deze logboeken bevat gedetailleerde records van systeemgebeurtenissen informatie en kunnen helpen beschermen tegen niet-afwijzing. Verder kan is om gegevens te registreren beperkt met toegangsbeheer op basis van rollen om te voorkomen dat unauthored wijziging en verwijdering van logboekgegevens. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-11"></a>NIST 800 53 besturingselement Australië-11

#### <a name="audit-record-retention"></a>Audit Record bewaren

**Australië-11** de organisatie behoudt controlerecords voor [toewijzing: organisatie gedefinieerde tijdsperiode die consistent zijn met het bewaarbeleid records] ter ondersteuning van na-de-fact onderzoeken beveiligingsincidenten en om te voldoen aan wettelijke en vereisten voor de bewaarperiode organisatiegegevens.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-service in OMS. Log Analytics biedt bewakingsservices voor OMS door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. Zodra verzameld, blijven de gegevens bewaard één jaar per Log Analytics-configuratie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-12a"></a>NIST 800 53 besturingselement Australië-12.a

#### <a name="audit-generation"></a>Audit generatie

**Australië-12.a** het informatiesysteem biedt de mogelijkheid van de audit record generatie voor de controleerbare gebeurtenissen die zijn gedefinieerd in Australië-2 een. op [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Gebeurtenissen die worden gecontroleerd door deze blauwdruk Azure bevatten die worden gecontroleerd door Azure activiteitenlogboeken voor geïmplementeerde resources, OS-niveau Logboeken, Active Directory-logboeken en logboeken van de SQL Server. Klanten kunnen aanvullende gebeurtenissen moeten worden gecontroleerd om te voldoen aan de behoeften van de missie selecteren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-12b"></a>NIST 800 53 besturingselement Australië-12.b

#### <a name="audit-generation"></a>Audit generatie

**Australië-12.b** staat het informatiesysteem [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] selecteren welke controleerbare gebeurtenissen moeten worden gecontroleerd door bepaalde onderdelen van het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Toegang tot de functies van de audit is beperkt met behulp van op rollen gebaseerde toegangsbeheer in Azure en op de virtuele machine OS-niveau. De configuratie van gebeurtenissen die zijn geselecteerd om te worden gecontroleerd door de bronnen die zijn geïmplementeerd door deze blauwdruk Azure kan worden geconfigureerd door gebruikers met de juiste autorisatie op basis van rollen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-au-12c"></a>NIST 800 53 besturingselement Australië-12.c

#### <a name="audit-generation"></a>Audit generatie

**Australië-12.c** controlerecords voor de gebeurtenissen die zijn gedefinieerd in Australië 2.d wordt de informatie gegenereerd. met de inhoud die is gedefinieerd in Australië-3.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Gebeurtenissen die worden gecontroleerd door deze blauwdruk Azure bevatten die worden gecontroleerd door Azure activiteitenlogboeken voor geïmplementeerde resources, OS-niveau Logboeken, Active Directory-logboeken en logboeken van de SQL Server. Klanten kunnen aanvullende gebeurtenissen moeten worden gecontroleerd om te voldoen aan de behoeften van de missie selecteren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-12-1"></a>NIST 800 53 besturingselement Australië-12 (1)

#### <a name="audit-generation--system-wide--time-correlated-audit-trail"></a>Generatie van de audit | Systeeminstellingen / tijd gecorreleerde audittrail

**Australië-12 (1)** het informatiesysteem compileert controlerecords van [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] in een hele systeem (logische of fysieke) audittrail die tijd-gecorreleerd met binnen [toewijzing: organisatie gedefinieerd niveau van tolerantie voor de relatie tussen de tijdstempels van afzonderlijke records in de audittrail].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-service in OMS. Log Analytics biedt bewakingsservices voor OMS door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. Audit record tijdstempels zijn niet gewijzigd, daarom is de audittrail tijd gecorreleerde. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-au-12-3"></a>NIST 800 53 besturingselement Australië-12 (3)

#### <a name="audit-generation--changes-by-authorized-individuals"></a>Generatie van de audit | Wijzigingen van bevoegde personen

**Australië-12 (3)** het informatiesysteem biedt de mogelijkheid voor [toewijzing: organisatie gedefinieerd personen of rollen] te wijzigen van de controles worden uitgevoerd op [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] op basis van [ Toewijzing: selecteerbare gebeurteniscriteria organisatie gedefinieerd] in [toewijzing: organisatie gedefinieerde drempelwaarden].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Toegang tot de functies van de audit is beperkt met behulp van op rollen gebaseerde toegangsbeheer in Azure en op de virtuele machine OS-niveau. De configuratie van gebeurtenissen die zijn geselecteerd om te worden gecontroleerd door de bronnen die zijn geïmplementeerd door deze blauwdruk Azure kan worden geconfigureerd door gebruikers met de juiste autorisatie op basis van rollen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |

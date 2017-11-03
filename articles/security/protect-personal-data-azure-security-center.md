---
title: Persoonlijke gegevens beschermen met Azure Security Center | Microsoft Docs
description: beveiligen van persoonlijke gegevens met Azure security center
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3a941389713a4d3dbffbbfe8a717409927d85c6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Persoonlijke gegevens beschermen tegen aanvallen en inbreuken op: Azure Security Center

In dit artikel helpt u begrijpen hoe Azure Security Center persoonlijke gegevens beveiligen tegen aanvallen en inbreuken op.

## <a name="scenario"></a>Scenario 

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse en Baltische veiligheid, evenals Florida. Om te helpen bij deze inspanningen, heeft die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk

Het bedrijf gebruikmaakt van Microsoft Azure voor het opslaan van bedrijfsgegevens in de cloud. Dit omvat persoonlijk identificeerbare informatie zoals namen, adressen, telefoonnummers en creditcardgegevens. Het bevat ook informatie Human Resources, zoals:

- Adressen
- telefoonnummers
- BTW-id 's
- medische informatie

De regel cruise onderhoudt ook een grote database van derden en loyaliteit voor leden. Zakelijke werknemers toegang tot het netwerk van het bedrijf externe kantoren en reizen agents over de hele wereld hebben toegang tot een aantal bedrijfsresources.
Persoonlijke gegevens verzonden via het netwerk tussen deze locaties en het Microsoft-datacentrum.

## <a name="problem-statement"></a>Probleemformulering

Het bedrijf maakt zich zorgen over de bedreiging van aanvallen op Azure-resources. Ze willen blootstelling van werknemers en klanten persoonlijke gegevens te voorkomen dat onbevoegden. Ze willen richtlijnen op zowel voorkomen en antwoord/herstel, evenals een effectieve manier voor het bewaken van de actieve beveiliging van hun cloudbronnen.
Ze moeten een sterke line-of beveiliging tegen kwaadwillende personen de hedendaagse geavanceerde en geordend.

## <a name="company-goal"></a>Bedrijf-doel

Een van de doelstellingen van het bedrijf is om te garanderen dat de privacy van klanten en werknemers persoonlijke gegevens te beveiligen tegen bedreigingen. Een van hun doelstellingen is onmiddellijk reageren op tekenen van inbreuk op de impact helpen. Is er een manier om te beoordelen van de huidige status van de beveiliging, kwetsbaar configuraties te identificeren en ze herstellen.

## <a name="solutions"></a>Oplossingen

Microsoft Azure Security Center (ASC) biedt een geïntegreerde beveiliging en beleidsbeheer beheeroplossing. Het biedt eenvoudig te gebruiken en effectieve bedreiging mogelijkheden voor preventie, detectie en reactie.

### <a name="prevention"></a>Preventie

ASC kunt u voorkomen dat schendingen doordat u beveiligingsbeleid instellen en implementeren van aanbevelingen voor beveiliging just-in-time-toegang bieden.

Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement. Alleen bij het time-toegang kan worden gebruikt vergrendelen binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen. Aanbevelingen voor beveiliging worden gemaakt door ASC na het analyseren van de beveiligingsstatus van uw Azure-resources.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Hoe Stel beleidsregels voor veiligheid in ASC?

U kunt voor elk abonnement beveiligingsbeleid configureren. Het beveiligingsbeleid kan alleen worden gewijzigd door een eigenaar of bijdrager van het abonnement. In de Azure portal, het volgende doen:

1. Selecteer **beleid** in het dashboard ASC.

2. Selecteer het abonnement die u wilt inschakelen, het beleid.

3. Kies **preventiebeleid** configureren van beleid per abonnement. **Verzamel gegevens van virtuele machines** moet worden ingesteld op **op.**

4. In de **preventiebeleid** opties, dient u **op** om in te schakelen van de aanbevelingen voor beveiliging die relevant voor het abonnement zijn.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Zie voor meer gedetailleerde instructies en een uitleg van elk van de beleid-aanbevelingen die kunnen worden ingeschakeld, [beveiligingsbeleid instellen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Hoe configureer ik Just in Time toegang (Just in time)?

Wanneer JIT is ingeschakeld, wordt in Security Center binnenkomend verkeer naar uw Azure VM's vergrendelt door het maken van een NSG-regel. Selecteert u de poorten op de virtuele machine waarmee binnenkomend verkeer wordt worden vergrendeld. Voor het gebruik van JIT-toegang tot het volgende doen:

1. Selecteer de **Just in time VM toegang tegel** op de blade ASC.

2. Selecteer de **aanbevolen** tabblad.

3. Onder **VMs**, selecteert u de virtuele machines die u wilt inschakelen. Hiermee wordt het selectievakje naast een virtuele machine geplaatst. 
4. Selecteer **inschakelen JIT** op virtuele machines.
5. Selecteer **Opslaan**.

Vervolgens ziet u de standaardpoorten die ASC aanbeveelt wordt ingeschakeld voor JIT. U kunt ook toevoegen en configureren van een nieuwe poort waarop u wilt de zojuist inschakelen in tijdoplossing. De **Just in time VM toegang** tegel in het Beveiligingscentrum toont het aantal virtuele machines die zijn geconfigureerd voor JIT-toegang. Ook ziet u het aantal goedgekeurde toegangsaanvragen in de afgelopen week.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Voor instructies over hoe u dit doet, en aanvullende informatie over Just in Time-toegang, Zie [beheren via in de tijd toegang tot een virtuele machine.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Hoe ik ASC beveiligingsaanbevelingen implementeren?

Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen. 
1. Selecteer de **aanbevelingen** tegel op het dashboard ASC.

2. Bekijk de aanbevelingen die worden weergegeven in een tabel waarin elke regel een aanbeveling vertegenwoordigt.

3. Filter aanbevelingen, selecteer **Filter** en selecteer de ernst en status waarden die u wilt zien.

4. Als u wilt verwijderen van een aanbeveling die niet van toepassing, kunt u met de rechtermuisknop op en selecteer **sluiten.**

5. Denk na over welke aanbeveling eerst moet worden toegepast.

6. De aanbevelingen in volgorde van prioriteit toepassen.

Zie voor een lijst van mogelijke aanbevelingen en de zelfstudies over het toepassen van elk [aanbevelingen voor beveiliging in Azure Security Center beheren.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Detectie van en reactie

Detectie- en Ga samen als moet worden gereageerd zo snel mogelijk na een bedreiging wordt gedetecteerd.
Detectie van dreigingen ASC werkt door het automatisch verzamelen van gegevens van de beveiliging van uw Azure-resources, het netwerk en verbonden partneroplossingen. ASC kunt snel de detectiealgoritmen bijwerken als aanvallers release nieuwe en steeds meer geavanceerde aanvallen. Zie voor meer informatie gedetailleerde over de werking van de detectie van dreigingen van ASC [Azure Security Center detectiemogelijkheden](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a>Hoe ik beheren en reageren op beveiligingswaarschuwingen?

Een lijst met beveiligingswaarschuwingen wordt weergegeven in Security Center samen met de informatie die u nodig hebt om snel onderzoek te het probleem. Security Center bevat ook aanbevelingen voor het herstellen van een aanval. Voor het beheren van uw beveiligingswaarschuwingen, het volgende doen:

1. Selecteer de **beveiligingswaarschuwingen** -tegel in het dashboard ASC. Hiermee worden de details voor elke waarschuwing weergegeven.

2. U kunt waarschuwingen op basis van datum, status en ernst filteren selecteren **Filter** en selecteer vervolgens de waarden die u wilt zien.

3. Om te reageren op een waarschuwing, selecteert u deze en lees de informatie in en selecteer de resource die is aangevallen.

4. In de **beschrijving** veld, ziet u informatie, zoals aanbevolen herstel.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Zie voor meer informatie over het reageren op beveiligingswaarschuwingen, [beheren en erop reageren beveiligingswaarschuwingen in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

Voor verdere hulp bij het onderzoeken van beveiligingswaarschuwingen het bedrijf ASC waarschuwingen kunt integreren met een eigen SIEM-oplossing met behulp van [Azure Log integratie](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Hoe beheer ik beveiligingsincidenten?

In ASC is een beveiligingsincident voordoet een aggregatie van alle waarschuwingen voor een resource uitgelijnd met de kill-keten patronen. Een incident zal de lijst van gerelateerde waarschuwingen tonen, zodat u meer info krijgt over elke gebeurtenis. Incidenten worden weergegeven in de tegel beveiligingswaarschuwingen en de blade.

Om te bekijken en beheren van beveiligingsincidenten, het volgende doen:

1. Selecteer de **beveiligingswaarschuwingen** tegel. Als een beveiligingsincident voordoet wordt gedetecteerd, wordt weergegeven onder de grafiek security-waarschuwingen. Er is een pictogram dat verschilt van andere waarschuwingen.

2. Selecteer het incident voor meer informatie over deze beveiligingsincident voordoet. Aanvullende informatie bevatten een volledige beschrijving, de ernst, de huidige status, de aangevallen resource, de herstelstappen uit voor het incident en de waarschuwingen die zijn opgenomen in dit incident.

U kunt filteren om te zien **alleen incidenten**, **alleen waarschuwingen**, of **beide**.

#### <a name="how-do-i-access-the-threat-intelligence-report"></a>Hoe krijg ik toegang tot het Threat Intelligence-rapport?

ASC analyseert gegevens uit meerdere bronnen bedreigingen identificeren. Security Center bevat om te helpen respons op incidenten teams onderzoeken en bedreigingen te herstellen, een bedreiging intelligence-rapport dat informatie bevat over de bedreiging die is gedetecteerd.

Security Center heeft drie soorten threat rapporten die per aanval kunnen variëren.
De beschikbare rapporten zijn:

- Activiteitengroep rapport: biedt verdiepen in aanvallers, hun doelstellingen en -tactieken.

- Campagne rapport: Er is gericht op gegevens van specifieke aanval campagnes.

- Het overzichtsrapport voor Threat: bevat informatie over alle items in de vorige twee rapporten.

Dit type gegevens zeer nuttig is tijdens de respons op incidenten, waarbij er lopende onderzoek om te begrijpen van de bron van de aanval, de aanvaller beweegredenen en wat te doen om dit probleem vooruitgang beperken.

1. Voor toegang tot de threat intelligence-rapport moet het volgende doen:

2. Selecteer de **beveiligingswaarschuwingen** tegel op het dashboard ASC.

3. Selecteer de beveiligingswaarschuwing waarvoor u wilt meer informatie vinden.

4. In de **rapporten** veld, klikt u op de koppeling naar het threat intelligence-rapport.

5. Hiermee opent u het PDF-bestand, die u kunt downloaden.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

Zie voor meer informatie over het rapport ASC threat intelligence [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Evaluatie

Om te helpen met testen, beoordelen en evalueren van uw beveiligingspostuur, biedt ASC voor het evalueren van de geïntegreerde beveiligingsproblemen met Qualys cloud agents, als onderdeel van het onderdeel van de aanbevelingen voor virtuele machine.

De agent Qualys rapporten beveiligingslek gegevens voor het beheerplatform Qualys die verzendt vervolgens beveiligingslek en health monitoring die gegevens terug naar ASC. De aanbeveling om een vulnerability assessment oplossing toevoegen wordt weergegeven in de **aanbevelingen** blade op het dashboard ASC.

Nadat de oplossing voor de evaluatie van beveiligingsproblemen op de doel-VM is geïnstalleerd, wordt de VM met Security Center gescand op beveiligingsproblemen in het systeem en in toepassingen. Gedetecteerde problemen worden weergegeven bij de optie **Aanbevelingen voor virtuele machines**.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Hoe ik een vulnerability assessment oplossing implementeren? 

Als een virtuele Machine geen een geïntegreerde vulnerability assessment oplossing is al geïmplementeerd Security Center raadt aan om te worden geïnstalleerd.

1. In het dashboard ASC op de **aanbevelingen** blade Selecteer **een vulnerability assessment oplossing toevoegen.**

2. Selecteer de virtuele machines waarop u wilt installeren van de vulnerability assessment-oplossing.

3. Klik op **installeren op virtuele machines [aantal].**

4. Selecteer een partneroplossing in Azure Marketplace of onder **bestaande oplossing gebruiken** Selecteer **Qualys.**

5. U kunt de instellingen voor automatisch bijwerken inschakelen of uitschakelen de **partneroplossingen** blade.

Zie voor meer informatie wilt over het implementeren van een oplossing voor vulnerability assessment [controle op beveiligingslekken in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Volgende stappen

- [Snelstartgids voor Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Inleiding tot Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Waarschuwingen van Azure Security Center integreren met Azure-logboekanalyse-integratie](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Versterking Azure Security Center met geïntegreerde Vulnerability Assessment](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)

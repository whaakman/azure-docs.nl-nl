---
title: Actieregels voor Azure Monitor-waarschuwingen
description: Inzicht krijgen in wat actieregels zijn, en over het configureren en deze beheren.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 212e6b042caec5f24a620dc491dc674417816df7
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310368"
---
# <a name="action-rules-preview"></a>Actieregels (preview)

Actieregels kunnen u voor het definiëren van acties (of de onderdrukking van acties) op een Resource Manager-bereik (abonnement, resourcegroep of Resource). Ze hebben verschillende filters waarmee u kunt verfijnen op de specifieke subset van de waarschuwing exemplaren die u wilt ondernemen. 

## <a name="why-and-when-should-you-use-action-rules"></a>Waarom en wanneer moet u actieregels gebruiken?

### <a name="suppression-of-alerts"></a>Onderdrukking van waarschuwingen

Er zijn vaak veel scenario's waarin het nuttig om de meldingen die worden gegenereerd door waarschuwingen te onderdrukken tijdens de kantooruren van onderdrukking tijdens gepland onderhoud variëren kunnen te onderdrukken zou zijn. Het team dat verantwoordelijk is voor 'ContosoVM' wil bijvoorbeeld meldingen van waarschuwingen onderdrukken voor de toekomstige weekend omdat 'ContosoVM' een gepland onderhoud ondergaat. Terwijl u ze kunnen uitschakelen van elke waarschuwing regel ingesteld op 'ContosoVM' handmatig (en schakelt u het plaatsen van het onderhoud), maar is niet een eenvoudige ervaring. Actieregels kunnen u definiëren waarschuwingsonderdrukking op schaal met de mogelijkheid om de periode van onderdrukking flexibel te configureren. Ga terug naar het vorige voorbeeld, kunt het team nu een actieregel definiëren op ContosoVM' die voor het weekend, alle waarschuwingsmeldingen worden onderdrukt.


### <a name="actions-at-scale"></a>Acties op schaal

Hoewel waarschuwingsregels u voor het definiëren van de actiegroep die wordt geactiveerd kunt wanneer de waarschuwing wordt gegenereerd, meestal klanten vaak hebben een algemene actiegroep via hun bereik van bewerkingen. Een team dat verantwoordelijk is voor de resourcegroep 'ContosoRG' definieert bijvoorbeeld waarschijnlijk dezelfde actiegroep voor alle waarschuwingsregels is gedefinieerd in 'ContosoRG'. Actieregels kunnen u voor het vereenvoudigen van dit proces met de mogelijkheid voor het definiëren van acties op schaal, zodat een actiegroep die kan worden geactiveerd voor een waarschuwing gegenereerd op het geconfigureerde bereik. Ga terug naar het vorige voorbeeld, kunt het team nu een actieregel definiëren op ContosoRG' die dezelfde actiegroep voor alle waarschuwingen gegenereerd binnen deze wordt geactiveerd.

> [!NOTE]
> Actieregels op dit moment niet van toepassing op waarschuwingen van de Health-Service.

## <a name="configuring-an-action-rule"></a>Configureren van een actieregel voor

U kunt de functie openen door te selecteren **acties kunt beheren,** van de startpagina in Azure Monitor-waarschuwingen. Selecteer vervolgens **actieregels (Preview)** . Kunt u ze openen door te selecteren **actieregels (preview)** vanuit het dashboard van de landingspagina voor waarschuwingen.

![Actieregels van de startpagina van Azure Monitor](media/alerts-action-rules/action-rules-landing-page.png)

Selecteer **+ nieuwe actieregel**. 

![Actieregel voor het nieuwe toevoegen](media/alerts-action-rules/action-rules-new-rule.png)

U kunt ook kunt u ook een actieregel maken tijdens het configureren van een waarschuwingsregel.

![Actieregel voor het nieuwe toevoegen](media/alerts-action-rules/action-rules-alert-rule.png)

U ziet nu de actie regel voor het maken van stroom openen. Configureer de volgende elementen: 

![Nieuwe actie regel voor het maken van stroom](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>Scope

Kies eerst het bereik, dat wil zeggen, doelresource, resourcegroep of abonnement. U hebt ook de mogelijkheid voor meervoudige selectie een combinatie van een van de bovenstaande (binnen één abonnement). 

![Actie regelbereik](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filtercriteria

Daarnaast kunt u filters wilt verfijnen op een specifieke subset van de waarschuwingen op het gedefinieerde bereik definiëren. 

De filters die beschikbaar zijn: 

* **Ernst**: Selecteer een of meer waarschuwingen ernstcategorieën. Ernst = Sev1 betekent dat de actieregel van toepassing op alle waarschuwingen met urgentie als Sev1.
* **Service bewaken**: Filteren op basis van de oorspronkelijke monitoring-service. Dit is ook meervoudige selectie. Bijvoorbeeld-controleservice = 'Application Insights' betekent dat de actieregel van toepassing op alle 'Application Insights' op basis van waarschuwingen.
* **Resourcetype**: Filteren op basis van een specifiek brontype. Dit is ook meervoudige selectie. Bijvoorbeeld, resourcetype = 'Virtuele Machines' betekent dat de actieregel van toepassing op alle virtuele Machines is.
* **Regel-ID waarschuwen**: Hiermee kunt u filteren op specifieke regels voor waarschuwingen met behulp van de Resource Manager-ID van de waarschuwingsregel.
* **Voorwaarde controleren**: Filter voor waarschuwing exemplaren met 'Fired' of 'Omgezet' als de voorwaarde van de monitor.
* **Beschrijving**: Reguliere expressie die overeenkomen met in de beschrijving die is gedefinieerd als onderdeel van de waarschuwingsregel.
* **Waarschuwingscontext (payload)** : Reguliere expressie die overeenkomt met binnen de [waarschuwingscontext](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) velden van een waarschuwingsexemplaar.

Deze filters worden toegepast in combinatie met elkaar. Bijvoorbeeld, als ik 'Resourcetype' = 'Virtuele Machines' en 'Prioriteit' = 'Sev0', dan kan ik voor alle 'Sev0' waarschuwingen hebt gefilterd op mijn virtuele machines. 

![Actie regel filters](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Configuratie van onderdrukking of actie

Vervolgens configureert u de actieregel voor waarschuwingsonderdrukking of ondersteuning voor groepen van actie. U kunt beide niet kiezen. De configuratie van de omgaat met alle waarschuwingen exemplaren die overeenkomt met de eerder gedefinieerde bereik en filters.

#### <a name="suppression"></a>Onderdrukking

Als u selecteert **onderdrukking**, de duur voor de onderdrukking van de acties en meldingen te configureren. Kies een van de volgende opties:
* **Vanaf nu (altijd)** : Alle meldingen onderdrukt voor onbepaalde tijd.
* **Op een gepland tijdstip**: Meldingen binnen een begrensde periode onderdrukken.
* **Met een terugkeerpatroon**: Op een terugkerend schema, wat dagelijks, wekelijks of maandelijks zijn kan te onderdrukken.

![Onderdrukking van de actie-regel](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Actiegroep

Als u selecteert **actiegroep** in de wisselknop, een bestaande actiegroep toevoegen of een nieuwe maken. 

> [!NOTE]
> U kunt slechts één actiegroep koppelen aan een actieregel voor.

![Actie van de actiegroep-regel](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Regeldetails actie

Tot slot de volgende details voor de actieregel configureren
* Name
* Resourcegroep waarin deze worden opgeslagen
* Description 

## <a name="example-scenarios"></a>Voorbeeldscenario's

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Scenario 1: Onderdrukking van waarschuwingen op basis van ernst

Contoso wil meldingen voor alle Sev4 meldingen op alle virtuele machines in hun abonnement 'ContosoSub' elke weekend onderdrukken.

**Oplossing:** Maakt een actieregel met
* Scope = 'ContosoSub'
* Filters
    * Ernst = 'Sev4'
    * Resourcetype = 'Virtuele Machines'
* Onderdrukking van met het terugkeerpatroon ingesteld op wekelijks en "Zaterdag" en "Zondag" gecontroleerd

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Scenario 2: Onderdrukking van waarschuwingen op basis van de context van waarschuwing (payload)

Contoso wil om meldingen te onderdrukken voor alle waarschuwingen gegenereerd voor 'Computer-01 logboekbestanden' in 'ContosoSub' voor onbepaalde tijd als het onderhoud ondergaat.

**Oplossing:** Maakt een actieregel met
* Scope = 'ContosoSub'
* Filters
    * Service bewaken = 'Log Analytics'
    * Waarschuwingscontext (payload) bevat Computer-01
* Onderdrukking van ingesteld op ' vanaf nu (altijd)'

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Scenario 3: Actiegroep die zijn gedefinieerd op een resourcegroep

Contoso heeft gedefinieerd [een waarschuwing voor metrische gegevens op een abonnementsniveau](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor), maar het definiëren van de acties die speciaal voor waarschuwingen die zijn gegenereerd op basis van de resourcegroep 'ContosoRG' activeren.

**Oplossing:** Maakt een actieregel met
* Scope = 'ContosoRG'
* Er zijn geen filters
* Action Group set to 'ContosoActionGroup'

> [!NOTE]
> **Actiegroepen gedefinieerd in actieregels en regels voor waarschuwingen werken onafhankelijk van elkaar, met geen deduplicatie**. In het scenario dat hierboven wordt beschreven als er een actiegroep gedefinieerd voor de waarschuwingsregel, deze wordt geactiveerd in combinatie met de actiegroep die is gedefinieerd in de actieregel. 

## <a name="managing-your-action-rules"></a>De actieregels beheren

U kunt weergeven en beheren van uw actieregels in de lijst zoals hieronder wordt weergegeven.

![Actieregels lijstweergave](media/alerts-action-rules/action-rules-list-view.png)

Hier kunt u inschakelen/uitschakelen/verwijderen actieregels op schaal door het selectievakje ernaast te selecteren. Te klikken op de actieregel van een opent u de configuratiepagina, zodat u kunt voor het bijwerken van de definitie en het inschakelen/uitschakelen.

## <a name="best-practices"></a>Aanbevolen procedures

Meld u waarschuwingen die zijn gemaakt met de ['aantal resultaten'](alerts-unified-log.md) optie genereren **slechts één exemplaar van de waarschuwing** met behulp van de hele zoekresultaat (dit kan bijvoorbeeld op meerdere computers). In dit scenario, als een actieregel maakt gebruik van het filter Waarschuwingscontext (payload), fungeert de op het exemplaar van de waarschuwing, zolang er een overeenkomst is. In scenario 2 zoals eerder beschreven als de zoekresultaten de waarschuwing gegenereerd voor zowel 'Computer-01' en 'Computer-02', bevatten de volledige melding is onderdrukt (dat wil zeggen, er is geen melding helemaal gegenereerd voor Computer-02).

![Actieregels en waarschuwingen (aantal resultaten)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Aanbevolen gebruik te maken van naar waarschuwingen voor aan de actieregels van de, we adviseren u om te maken van waarschuwingen met de ['meting van metrische gegevens'](alerts-unified-log.md) optie. Deze optie gebruikt, zijn afzonderlijke exemplaren van de waarschuwing gegenereerd op basis van het veld groep is gedefinieerd. In scenario 2, klikt u vervolgens afzonderlijke exemplaren van de waarschuwing gegenereerd voor Computer-01 en Computer-02. Met de actieregel in het scenario beschreven, wordt alleen de melding voor Computer-01 worden onderdrukt terwijl de melding voor Computer-02 nu doorgaan met het normale manier worden geactiveerd.

![Actieregels en waarschuwingen (aantal resultaten)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Veelgestelde vragen

* V. Tijdens het configureren van een actieregel voor de, graag ik wil om te zien van alle mogelijke actie overlappende regels zodat ik voorkomen dat dubbele meldingen. Is het mogelijk om dit te doen?

    A. Wanneer u een bereik definiëren tijdens het configureren van een actieregel voor de, ziet u een lijst met actieregels voor die op hetzelfde bereik overlappen (indien aanwezig). Deze elkaar overlappen, kan een van de volgende opties zijn:
    * Een exacte overeenkomst: Bijvoorbeeld, zijn de regel van de actie die u wilt definiëren en een actieregel voor de overlappende in hetzelfde abonnement.
    * Een subset: Bijvoorbeeld, de actieregel die u wilt definiëren is op een abonnement en de actieregel van de overlappende is op een resourcegroep binnen het abonnement.
    * Een hoofdverzameling: Bijvoorbeeld, de regel van de actie die u wilt definiëren op een resourcegroep is, en de actieregel van de overlappende is op het abonnement met de resourcegroep.
    * Een kruispunt: Bijvoorbeeld, de regel van de actie die u wilt definiëren, is op 'VM1' en 'VM2' en de actieregel van de overlappende is op 'VM2' en 'VM3'.

    ![Overlappende actieregels](media/alerts-action-rules/action-rules-overlapping.png)

* V. Tijdens het configureren van een waarschuwingsregel is het misschien mogelijk om te weten of er zijn al actieregels gedefinieerd die reageren op de waarschuwingsregel die ik ben definiëren?

    A. Nadat u hebt de doelresource gedefinieerd voor de waarschuwingsregel, ziet u de lijst met actieregels die worden uitgevoerd op hetzelfde bereik (indien aanwezig) door te klikken op 'Weergave geconfigureerd acties' onder de sectie 'Acties'. Deze lijst is ingevuld op basis van de volgende scenario's voor het bereik:
    * Een exacte overeenkomst: Bijvoorbeeld, zijn de waarschuwingsregel die u wilt definiëren en een actieregel voor de op hetzelfde abonnement.
    * Een subset: Bijvoorbeeld, de waarschuwingsregel die u wilt definiëren, is op een abonnement en de actieregel is op een resourcegroep binnen het abonnement.
    * Een hoofdverzameling: Bijvoorbeeld, de waarschuwingsregel die u wilt definiëren, is op een resourcegroep en de actieregel is op het abonnement met de resourcegroep.
    * Een kruispunt: Bijvoorbeeld, de waarschuwingsregel die u wilt definiëren, is op 'VM1' en 'VM2' en de actieregel is op 'VM2' en 'VM3'.
    
    ![Overlappende actieregels](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

* V. Kan ik de waarschuwingen die door een actieregel zijn onderdrukt zien?

    A. In de [waarschuwingen lijstpagina](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), er is een extra kolom die kan worden gekozen met de naam 'onderdrukking Status'. Als de melding voor een exemplaar van de waarschuwing is onderdrukt, zou deze die status weergegeven in de lijst.

    ![Onderdrukte waarschuwing exemplaren](media/alerts-action-rules/action-rules-suppressed-alerts.png)

* V. Als er een actieregel met een actiegroep en de andere onderdrukking actief op hetzelfde bereik is, wat gebeurt er?

    A. **Onderdrukking van altijd voorrang op hetzelfde bereik**.

* V. Wat gebeurt er als ik een resource in twee afzonderlijke actieregels worden bewaakt? Krijg ik een of twee meldingen? Bijvoorbeeld 'VM2' in dit scenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1'
      action rule 'AR2' defined for 'VM2' and 'VM3' with action group 'AG1'

    A. Voor elke waarschuwing voor 'VM1' en 'VM3', zou de actiegroep 'AG1' één keer worden geactiveerd. Voor elke waarschuwing op 'VM2' actiegroep 'AG1' tweemaal zou worden geactiveerd (**actieregels acties niet ongedaan maken dupliceren**). 

* V. Wat gebeurt er als ik een resource bewaakt in twee afzonderlijke actieregels hebt en een voor actie bij een andere voor onderdrukking aanroepen? Bijvoorbeeld, 'VM2' in dit scenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1' 
      action rule 'AR2' defined for 'VM2' and 'VM3' with suppression

    A. Voor elke waarschuwing voor 'VM1', zou de actiegroep 'AG1' één keer worden geactiveerd. Acties en meldingen voor elke waarschuwing over 'VM2' en 'VM3' moeten worden onderdrukt. 

* V. Wat gebeurt er als ik een waarschuwingsregel en een een actieregel gedefinieerd voor dezelfde resource verschillende actiegroepen aanroepen? Bijvoorbeeld, 'VM1' in dit scenario:

      alert rule  'rule1' on          'VM1' with action group 'AG2'
      action rule 'AR1'   defined for 'VM1' with action group 'AG1' 
 
    A. Voor elke waarschuwing voor 'VM1', zou de actiegroep 'AG1' één keer worden geactiveerd. Wanneer de regel 'rule1' van de waarschuwing wordt geactiveerd, wordt het 'AG2' daarnaast ook activeren. **Actiegroepen gedefinieerd in actieregels en regels voor waarschuwingen werken onafhankelijk van elkaar, met geen deduplicatie**. 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over waarschuwingen in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)

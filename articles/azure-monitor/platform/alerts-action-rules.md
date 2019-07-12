---
title: Actieregels voor Azure Monitor-waarschuwingen
description: Inzicht krijgen in wat actieregels in Azure Monitor zijn en over het configureren en deze beheren.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: df069ee398ea2937f03765b10576061b5e541390
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67656722"
---
# <a name="action-rules-preview"></a>Actieregels (preview)

Actieregels kunnen u definiëren of acties op een Azure Resource Manager-bereik (Azure-abonnement, resourcegroep of doelbron) onderdrukken. Ze hebben verschillende filters die help u beperken de specifieke subset van de waarschuwing die u wilt om te reageren op.

## <a name="why-and-when-should-you-use-action-rules"></a>Waarom en wanneer moet u actieregels gebruiken?

### <a name="suppression-of-alerts"></a>Onderdrukking van waarschuwingen

Er zijn veel scenario's waarin het nuttig om de meldingen die waarschuwingen genereren te onderdrukken. Deze scenario's variëren van onderdrukking tijdens gepland onderhoud onderdrukking buiten kantooruren. Bijvoorbeeld, het team dat verantwoordelijk is voor **ContosoVM** wil meldingen van waarschuwingen onderdrukken voor de toekomstige weekend omdat **ContosoVM** gepland onderhoud ondergaat. 

Hoewel elke waarschuwingsregel die geconfigureerd op kunt uitschakelen door het team **ContosoVM** handmatig (en het onderhoud opnieuw inschakelen), het is niet een eenvoudig proces. Actieregels helpen u bij het definiëren van waarschuwingsonderdrukking op schaal met de mogelijkheid om de periode van onderdrukking flexibel configureren. In het vorige voorbeeld, kan het team één actieregel definiëren op **ContosoVM** die worden onderdrukt alle waarschuwingsmeldingen voor het weekend.


### <a name="actions-at-scale"></a>Acties op schaal

Hoewel regels voor waarschuwingen u bij het definiëren van de actiegroep die wordt geactiveerd helpen wanneer de waarschuwing wordt gegenereerd, hebben klanten vaak een algemene actiegroep via hun bereik van bewerkingen. Bijvoorbeeld, een team dat verantwoordelijk is voor de resourcegroep **ContosoRG** definieert waarschijnlijk dezelfde actiegroep voor alle waarschuwingsregels is gedefinieerd in **ContosoRG**. 

Actieregels kunnen u dit proces te vereenvoudigen. Door het definiëren van acties op schaal, kan een actiegroep worden geactiveerd voor een waarschuwing die gegenereerd op het geconfigureerde bereik. In het vorige voorbeeld, kan het team één actieregel definiëren op **ContosoRG** die dezelfde actiegroep voor alle waarschuwingen gegenereerd binnen deze wordt geactiveerd.

> [!NOTE]
> Actieregels op dit moment niet van toepassing op Azure Service Health-waarschuwingen.

## <a name="configuring-an-action-rule"></a>Configureren van een actieregel voor

U kunt de functie openen door te selecteren **acties kunt beheren,** uit de **waarschuwingen** landingspagina in Azure Monitor. Selecteer **actieregels (preview)** . U kunt de regels openen door te selecteren **actieregels (preview)** vanuit het dashboard van de landingspagina voor waarschuwingen.

![Actieregels van de startpagina van Azure Monitor](media/alerts-action-rules/action-rules-landing-page.png)

Selecteer **+ nieuwe actieregel**. 

![Actieregel voor het nieuwe toevoegen](media/alerts-action-rules/action-rules-new-rule.png)

U kunt ook een actieregel voor de maken bij het configureren van een waarschuwingsregel.

![Actieregel voor het nieuwe toevoegen](media/alerts-action-rules/action-rules-alert-rule.png)

U ziet nu de flow-pagina voor het maken van actieregels voor. Configureer de volgende elementen: 

![Nieuwe actie regel voor het maken van stroom](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>Scope

Kies eerst de scope (Azure-abonnement, resourcegroep of doelbron). U kunt ook meerdere: Selecteer een combinatie van bereiken binnen één abonnement.

![Actie regelbereik](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filtercriteria

U kunt ook filters definiëren om te beperken ze naar een specifieke subset van de waarschuwingen. 

De filters die beschikbaar zijn: 

* **Ernst**: De optie te selecteren van een of meer waarschuwingen ernstcategorieën. **Ernst = Sev1** betekent dat het actieregel van toepassing is voor alle waarschuwingen die zijn ingesteld op Sev1.
* **Service bewaken**: Een filter op basis van de oorspronkelijke monitoring-service. Dit filter is ook meervoudige selectie. Bijvoorbeeld, **-controleservice = 'Application Insights'** betekent dat de actieregel van toepassing op alle waarschuwingen op basis van een Application Insights.
* **Resourcetype**:  Een filter op basis van een specifiek brontype. Dit filter is ook meervoudige selectie. Bijvoorbeeld, **resourcetype = 'Virtuele Machines'** betekent dat de actieregel van toepassing op alle virtuele machines.
* **Regel-ID waarschuwen**: Een optie om te filteren op specifieke regels voor waarschuwingen met behulp van de Resource Manager-ID van de waarschuwingsregel.
* **Voorwaarde controleren**:  Een filter voor waarschuwing exemplaren in beide gevallen **Fired** of **opgelost** als de voorwaarde van de monitor.
* **Beschrijving**: Een reguliere expressie (reguliere expressie)-overeenkomst die een tekenreeksovereenkomst op basis van de beschrijving definieert, gedefinieerd als onderdeel van de waarschuwingsregel. Bijvoorbeeld, **beschrijving bevat 'prod'** komt overeen met alle waarschuwingen met de tekenreeks 'productie' in de bijbehorende beschrijvingen.
* **Ontvang een waarschuwing Context (payload)** : Een regex-overeenkomst die een tekenreeksovereenkomst op basis van de context van de waarschuwing velden van de nettolading van een waarschuwing definieert. Bijvoorbeeld, **waarschuwen context (payload) bevat 'Computer-01'** komt overeen met alle waarschuwingen waarvan nettoladingen bevatten de tekenreeks 'Computer-01'.

Deze filters worden toegepast in combinatie met elkaar. Als u bijvoorbeeld **resourcetype ' virtuele Machines =** en **ernst ' Sev0 =** , en u hebt gefilterd voor alle **Sev0** waarschuwingen op uw virtuele machines. 

![Actie regel filters](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Configuratie van onderdrukking of actie

Configureer vervolgens de actieregel voor waarschuwingsonderdrukking of ondersteuning voor groepen van actie. U kunt beide niet kiezen. De configuratie van de omgaat met alle waarschuwingen exemplaren die overeenkomen met de eerder gedefinieerde bereik en filters.

#### <a name="suppression"></a>Onderdrukking

Als u selecteert **onderdrukking**, de duur voor de onderdrukking van de acties en meldingen te configureren. Kies een van de volgende opties:
* **Vanaf nu (altijd)** : Alle meldingen onderdrukt voor onbepaalde tijd.
* **Op een gepland tijdstip**: Onderdrukt meldingen binnen een begrensde duur.
* **Met een terugkeerpatroon**: Onderdrukt meldingen volgens een terugkerend dagelijks, wekelijks of maandelijks schema.

![Onderdrukking van de actie-regel](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Actiegroep

Als u selecteert **actiegroep** in de wisselknop, een bestaande actiegroep toevoegen of een nieuwe maken. 

> [!NOTE]
> U kunt slechts één actiegroep koppelen aan een actieregel voor.

![Toevoegen of nieuwe actieregel maken met de actiegroep selecteren](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Regeldetails actie

Laatste, configureer de volgende details voor de actieregel:
* Name
* Resourcegroep waarin het wordt opgeslagen
* Description 

## <a name="example-scenarios"></a>Voorbeeldscenario's

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Scenario 1: Onderdrukking van waarschuwingen op basis van ernst

Contoso wil om meldingen voor alle Sev4 waarschuwingen op alle virtuele machines in het abonnement te onderdrukken **ContosoSub** elke weekend.

**Oplossing:** Maakt een actieregel met:
* Bereik = **ContosoSub**
* Filters
    * Ernst = **Sev4**
    * Resourcetype = **virtuele Machines**
* Onderdrukking van met het terugkeerpatroon ingesteld op wekelijks, en **zaterdag** en **zondag** gecontroleerd

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Scenario 2: Onderdrukking van waarschuwingen op basis van de context van waarschuwing (payload)

Contoso wil om meldingen te onderdrukken voor alle waarschuwingen die worden gegenereerd voor logboekbestanden **Computer-01** in **ContosoSub** voor onbepaalde tijd zoals deze wordt er onderhoud.

**Oplossing:** Maakt een actieregel met:
* Bereik = **ContosoSub**
* Filters
    * Service bewaken = **Log Analytics**
    * Ontvang een waarschuwing Context (payload) bevat **Computer-01**
* Onderdrukking van ingesteld op **vanaf nu (altijd)**

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Scenario 3: Actiegroep die zijn gedefinieerd op een resourcegroep

Contoso heeft gedefinieerd [een waarschuwing voor metrische gegevens op een abonnementsniveau](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor). Maar het bedrijf wil de acties definiëren die speciaal voor waarschuwingen die zijn gegenereerd op basis van de resourcegroep activeert **ContosoRG**.

**Oplossing:** Maakt een actieregel met:
* Bereik = **ContosoRG**
* Er zijn geen filters
* Actiegroep ingesteld op **ContosoActionGroup**

> [!NOTE]
> *Actiegroepen gedefinieerd in actieregels en regels voor waarschuwingen werken onafhankelijk van elkaar, met geen Ontdubbeling.* In het scenario beschreven als een actiegroep die u eerder voor de waarschuwingsregel is gedefinieerd, wordt in combinatie met de actiegroep die is gedefinieerd in de actieregel. 

## <a name="managing-your-action-rules"></a>De actieregels beheren

U kunt weergeven en beheren van uw actieregels in de lijst:

![Actieregels lijstweergave](media/alerts-action-rules/action-rules-list-view.png)

Hier kunt u inschakelen, uitschakelen of verwijderen van actieregels op schaal door het selectievakje ernaast. Wanneer u een actieregel selecteert, wordt de configuratiepagina wordt geopend. De pagina kunt u de definitie van de actieregel bijwerken en in- of uitschakelen.

## <a name="best-practices"></a>Aanbevolen procedures

Meld u waarschuwingen die u met maakt de [aantal resultaten](alerts-unified-log.md) optie slechts één exemplaar van de waarschuwing genereren met behulp van de hele zoekresultaat (die mogelijk worden verdeeld over meerdere computers). In dit scenario, als een actieregel maakt gebruik van de **Waarschuwingscontext (payload)** worden gefilterd, fungeert deze op het exemplaar van de waarschuwing, zolang er een overeenkomst is. In Scenario 2, eerder beschreven, als de lijst met zoekresultaten voor de gegenereerde waarschuwing beide bevatten **Computer-01** en **Computer-02**, de volledige melding wordt onderdrukt. Er is geen melding gegenereerd voor **Computer-02** helemaal.

![Actieregels en waarschuwingen (aantal resultaten)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Waarschuwingen met maken voor het beste gebruik van waarschuwingen aan de actieregels van de, de [meting van metrische gegevens](alerts-unified-log.md) optie. Afzonderlijke exemplaren van de waarschuwing worden gegenereerd door deze optie, op basis van de gedefinieerde groep-veld. Vervolgens, in Scenario 2, worden de afzonderlijke exemplaren van de waarschuwing gegenereerd voor **Computer-01** en **Computer-02**. Vanwege de actieregel die is beschreven in het scenario, alleen de melding voor **Computer-01** wordt onderdrukt. De melding voor **Computer-02** moet worden gestart die normaal werken blijft.

![Actieregels en waarschuwingen (aantal resultaten)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Veelgestelde vragen

### <a name="while-im-configuring-an-action-rule-id-like-to-see-all-the-possible-overlapping-action-rules-so-that-i-avoid-duplicate-notifications-is-it-possible-to-do-that"></a>Terwijl ik een actieregel configureert ben, graag ik wil om te zien van alle mogelijke overlappende actieregels, zodat ik voorkomen dat dubbele meldingen. Is het mogelijk om dat te doen?

Nadat u een bereik definiëren bij het configureren van een actieregel voor de, ziet u een lijst van actieregels die elkaar overlappen in hetzelfde bereik (indien aanwezig). Deze elkaar overlappen, kan een van de volgende opties zijn:

* Een exacte overeenkomst: Bijvoorbeeld, zijn de regel van de actie die u definieert en een actieregel voor de overlappende in hetzelfde abonnement.
* Een subset: Bijvoorbeeld, de actieregel die u definieert is op een abonnement en de actieregel van de overlappende is op een resourcegroep binnen het abonnement.
* Een hoofdverzameling: Bijvoorbeeld, de actieregel die u definieert is op een resourcegroep en de actieregel van de overlappende is op het abonnement met de resourcegroep.
* Een kruispunt: De regel van de actie die u definieert is bijvoorbeeld op **VM1** en **VM2**, en de actieregel van de overlappende is op **VM2** en **VM3**.

![Overlappende actieregels](media/alerts-action-rules/action-rules-overlapping.png)

### <a name="while-im-configuring-an-alert-rule-is-it-possible-to-know-if-there-are-already-action-rules-defined-that-might-act-on-the-alert-rule-im-defining"></a>Ik ben een waarschuwingsregel configureren, is het mogelijk om te weten dat als er al actieregels gedefinieerd die kan reageren op de waarschuwingsregel die ik ben definiëren?

Nadat u de doelresource gedefinieerd voor de waarschuwingsregel, ziet u de lijst met actieregels voor die op hetzelfde bereik (indien aanwezig) door te selecteren **weergave geconfigureerd acties** onder de **acties** sectie. Deze lijst is ingevuld op basis van de volgende scenario's voor het bereik:

* Een exacte overeenkomst: Bijvoorbeeld, zijn de waarschuwingsregel die u definieert en een actieregel voor de op hetzelfde abonnement.
* Een subset: Bijvoorbeeld, de waarschuwingsregel die u definieert is op een abonnement en de actieregel is op een resourcegroep binnen het abonnement.
* Een hoofdverzameling: Bijvoorbeeld, de waarschuwingsregel die u definieert is op een resourcegroep en de actieregel is op het abonnement met de resourcegroep.
* Een kruispunt: De waarschuwingsregel die u definieert is bijvoorbeeld op **VM1** en **VM2**, en de actieregel is op **VM2** en **VM3**.
    
![Overlappende actieregels](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

### <a name="can-i-see-the-alerts-that-have-been-suppressed-by-an-action-rule"></a>Kan ik de waarschuwingen die door een actieregel zijn onderdrukt zien?

In de [waarschuwingen lijstpagina](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), kunt u een extra kolom met de naam **onderdrukking Status**. Als de melding voor een exemplaar van de waarschuwing is onderdrukt, zou deze die status weergegeven in de lijst.

![Onderdrukte waarschuwing exemplaren](media/alerts-action-rules/action-rules-suppressed-alerts.png)

### <a name="if-theres-an-action-rule-with-an-action-group-and-another-with-suppression-active-on-the-same-scope-what-happens"></a>Als er een actieregel met een actiegroep en de andere onderdrukking actief op hetzelfde bereik is, wat gebeurt er?

Onderdrukking van altijd voorrang op hetzelfde bereik.

### <a name="what-happens-if-i-have-a-resource-thats-monitored-in-two-separate-action-rules-do-i-get-one-or-two-notifications-for-example-vm2-in-the-following-scenario"></a>Wat gebeurt er als ik een resource die wordt bewaakt in twee afzonderlijke actieregels? Krijg ik een of twee meldingen? Bijvoorbeeld, **VM2** in het volgende scenario:

      "action rule AR1 defined for VM1 and VM2 with action group AG1
      action rule AR2 defined for VM2 and VM3 with action group AG1"

Voor elke waarschuwing voor VM1 en VM3 zou de actiegroep AG1 één keer worden geactiveerd. Voor elke waarschuwing op **VM2**, actiegroep AG1 zou twee keer worden geactiveerd omdat actieregels acties niet ontdubbelen. 

### <a name="what-happens-if-i-have-a-resource-monitored-in-two-separate-action-rules-and-one-calls-for-action-while-another-for-suppression-for-example-vm2-in-the-following-scenario"></a>Wat gebeurt er als ik een resource bewaakt in twee afzonderlijke actieregels hebt en een voor actie bij een andere voor onderdrukking aanroepen? Bijvoorbeeld, **VM2** in het volgende scenario:

      "action rule AR1 defined for VM1 and VM2 with action group AG1 
      action rule AR2 defined for VM2 and VM3 with suppression"

Voor elke waarschuwing voor VM1, zou de actiegroep AG1 één keer worden geactiveerd. Acties en meldingen voor elke waarschuwing op VM2 en VM3 worden onderdrukt. 

### <a name="what-happens-if-i-have-an-alert-rule-and-an-action-rule-defined-for-the-same-resource-calling-different-action-groups-for-example-vm1-in-the-following-scenario"></a>Wat gebeurt er als ik een waarschuwingsregel en een een actieregel gedefinieerd voor dezelfde resource verschillende actiegroepen aanroepen? Bijvoorbeeld, **VM1** in het volgende scenario:

      "alert rule rule1 on VM1 with action group AG2
      action rule AR1 defined for VM1 with action group AG1" 
 
Voor elke waarschuwing voor VM1, zou de actiegroep AG1 één keer worden geactiveerd. Wanneer er een waarschuwingsregel 'rule1' wordt geactiveerd, wordt deze AG2 daarnaast ook activeren. Actiegroepen gedefinieerd in actieregels en regels voor waarschuwingen werken onafhankelijk van elkaar, met geen Ontdubbeling. 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over waarschuwingen in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)

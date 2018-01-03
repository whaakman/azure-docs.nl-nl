---
title: Wat zijn de instellingen voor automatisch schalen? | Microsoft Docs
description: Detail van instellingen voor automatisch schalen en hoe ze werken.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: cff2be1818417a19f36da08d8c2eaa227bb945ec
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="understand-autoscale-settings"></a>Instellingen voor automatisch schalen begrijpen
Instellingen voor automatisch schalen kunnen u om te controleren of dat u hebt de juiste hoeveelheid resources voor het afhandelen van de fluctuerende belasting van uw toepassing wordt uitgevoerd. U kunt de instellingen voor automatisch schalen die worden geactiveerd configureren op basis van de metrische gegevens die aan load of dat de prestaties of trigger op een geplande datum en tijd. In dit artikel wordt een gedetailleerd overzicht van de anatomie van een instelling voor automatisch schalen. Het artikel begint met het schema en de eigenschappen van een instelling begrijpen en vervolgens doorloopt de verschillende profieltypen die kunnen worden geconfigureerd en tot slot wordt beschreven hoe welk profiel moet worden uitgevoerd op elk moment worden geëvalueerd door automatisch schalen.

## <a name="autoscale-setting-schema"></a>Schema voor automatisch schalen instellen
Ter illustratie van het schema van de instelling voor automatisch schalen, is de volgende instelling voor automatisch schalen wordt gebruikt. Het is belangrijk te weten dat deze instelling voor automatisch schalen heeft:
- Een profiel 
- Deze heeft twee metrische regels in dit profiel. een voor scale-out en één voor de schaal in.
- De regel scale-out wordt geactiveerd wanneer de virtuele-machineschaalset van gemiddelde Percentage CPU-metriek groter dan 85% gedurende de afgelopen 10 minuten is.
- De regel voor schaal wordt geactiveerd wanneer de virtuele-machineschaalset van gemiddelde minder dan 60 is % gedurende de afgelopen minuut.

> [!NOTE]
> Een instelling kan meerdere profielen hebt, Ga naar de [profielen](#autoscale-profiles) sectie voor meer informatie.
> Een profiel kan ook meerdere scale-out hebben en schaal in regels gedefinieerd, Ga naar de [evaluatie sectie](#autoscale-evaluation) om te zien hoe ze worden geëvalueerd

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Sectie | Elementnaam | Beschrijving |
| --- | --- | --- |
| Instelling | Id | Van de instelling voor automatisch schalen bron-ID. Instellingen voor automatisch schalen zijn een Azure Resource Manager-resource. |
| Instelling | naam | De naam van de instelling voor automatisch schalen. |
| Instelling | location | De locatie van de instelling voor automatisch schalen. Deze locatie kan afwijken van de locatie van de bron wordt geschaald. |
| properties | targetresourceuri op | De resource-ID van de bron wordt geschaald. U kunt slechts één instelling voor automatisch schalen per resource hebben. |
| properties | Profielen | Een instelling voor automatisch schalen bestaat uit een of meer profielen. Telkens wanneer de engine voor het automatisch schalen die wordt uitgevoerd, wordt uitgevoerd een profiel. |
| profiel | naam | De naam van het profiel, kunt u de naam waarmee u het profiel te identificeren. |
| profiel | Capacity.maximum | De maximale capaciteit toegestaan. Hiermee zorgt u ervoor dat automatisch schalen, bij het uitvoeren van dit profiel niet kan worden uitgebreid uw resource boven dit nummer. |
| profiel | Capacity.minimum | De minimale capaciteit toegestaan. Hiermee zorgt u ervoor dat automatisch schalen, bij het uitvoeren van dit profiel niet kan worden uitgebreid uw resource onder dit nummer. |
| profiel | Capacity.Default | Als er een probleem bij het lezen van de metriek resource (in dit geval de cpu van 'vmss1') en de huidige capaciteit lager dan de standaardcapaciteit vervolgens is om te controleren of de beschikbaarheid van de resource, automatisch schalen kan worden geschaald-out op de standaardwaarde. Als de huidige capaciteit al hoger dan de standaardcapaciteit is, automatisch schalen wordt niet schaal aan. |
| profiel | regels | Automatisch schalen schaalt automatisch tussen de maximale en minimale capaciteit van de regels in het profiel. U kunt meerdere regels in een profiel hebben. Het basisscenario is om twee regels: een om te bepalen wanneer scale-out en de andere om te bepalen wanneer scale in. |
| Regel | metricTrigger | De metrische voorwaarde van de regel wordt gedefinieerd. |
| metricTrigger | metricName | De naam van de metrische gegevens. |
| metricTrigger |  metricResourceUri | De resource-ID van de resource die de metrische gegevens verzendt. In de meeste gevallen is dit hetzelfde is als de bron wordt geschaald. In sommige gevallen kan kan het verschillen, bijvoorbeeld een virtuele-machineschaalset op basis van het aantal berichten in een opslagwachtrij kan worden geschaald. |
| metricTrigger | TimeGrain | De duur van de metrische steekproeven. Bijvoorbeeld: TimeGrain = 'PT1M' betekent dat de metrische gegevens moet worden geaggregeerd elke 1 minuut met behulp van de aggregatiemethode die is opgegeven in "statistiek." |
| metricTrigger | statistiek | De aggregatiemethode binnen de periode timeGrain. Bijvoorbeeld, statistiek = 'Gemiddeld' en timeGrain = 'PT1M' betekent dat de metrische gegevens moet elke 1 minuut kunnen samenvoegen met het gemiddelde te nemen. Deze eigenschap bepaalt hoe de metriek is actieve. |
| metricTrigger | Waarde voor TimeWindow | De hoeveelheid tijd zoekt metrische gegevens. Bijvoorbeeld: de waarde voor timeWindow = 'PT10M' betekent dat elke keer automatisch schalen die wordt uitgevoerd, wordt opgevraagd metrische gegevens voor de afgelopen 10 minuten. Het tijdvenster kunt metrische gegevens over uw genormaliseerd en reageren op tijdelijke pieken voorkomt. |
| metricTrigger | TimeAggregation van | De aggregatiemethode die wordt gebruikt voor het cumuleren van de steekproef metrische gegevens. Bijvoorbeeld, TimeAggregation van = 'Gemiddeld' moet aggregeren de steekproef metrische gegevens door het gemiddelde te nemen. In het voorbeeld hierboven tien voorbeelden 1 minuut duren en gemiddelde ze. |
| Regel | scaleAction | De actie die moet worden uitgevoerd wanneer de metricTrigger van de regel wordt geactiveerd. |
| scaleAction | richting | 'Worden verhoogd' scale-out, 'Minder' schaal in|
| scaleAction | waarde | Hoeveel vergroten of verkleinen van de capaciteit van de resource |
| scaleAction | cooldown | De hoeveelheid tijd moet worden gewacht na een schaalaanpassing voordat schalen opnieuw. Bijvoorbeeld, als cooldown = 'PT10M' dan nadat een schaalaanpassing optreedt, automatisch schalen niet proberen te schalen opnieuw voor een andere 10 minuten. De cooldown is dat de metrische gegevens stabiel na het toevoegen of verwijderen van exemplaren. |

## <a name="autoscale-profiles"></a>Profielen voor automatisch schalen

Er zijn drie soorten profielen voor automatisch schalen:

1. **Reguliere profiel:** meest voorkomende profiel. Als u niet hoeft te schalen van uw resource anders op basis van de dag van de week, of op een bepaalde dag, alleen moet u een reguliere-profiel in de instelling voor automatisch schalen instellen. Dit profiel kan vervolgens worden geconfigureerd met metrische regels die bepalen wanneer scale-out en wanneer scale in. U hebt slechts één reguliere profiel gedefinieerd.

    Een voorbeeldprofiel voor het gebruikt eerder in dit artikel is een voorbeeld van een reguliere-profiel. Komen die niet aan het is ook mogelijk in te stellen van een profiel schalen naar een aantal statische exemplaar voor uw resource.

2. **Vaste datum profiel:** met de reguliere profiel is gedefinieerd, Stel dat u hebt een belangrijke gebeurtenis die afkomstig zijn van op 26 December 2017 (PST) en u wilt dat de minimum/maximum voor capaciteit van de resource op die dag afwijken, maar nog steeds op dezelfde metrische gegevens schalen . In dit geval moet u een profiel voor een vaste datum toevoegen aan lijst van de instelling profielen. Het profiel is geconfigureerd om te worden uitgevoerd op de dag van de gebeurtenis. Voor de dag, wordt de reguliere profiel uitgevoerd.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
3. **Terugkeerpatroon van profiel:** dit type profiel kunt u om ervoor te zorgen dat dit profiel altijd op een bepaalde dag van de week wordt gebruikt. Terugkeerpatroon profielen hebben alleen een begin-tijdstip, als gevolg hiervan ze worden uitgevoerd totdat het volgende profiel voor het terugkeerpatroon of vaste datum profiel is ingesteld op starten. Een met slechts één terugkeerpatroon profiel instelling voor automatisch schalen wordt uitgevoerd dat profiel zelfs als er een reguliere profiel dat is gedefinieerd in dezelfde instelling. De onderstaande twee voorbeelden wordt het gebruik van dit profiel:

    **Voorbeeld 1: werkdag versus. Tijdens het weekend** Stel dat in het weekend u wilt dat de maximale capaciteit moet 4 maar op weekdagen, aangezien u meer load verwacht u wilt dat de maximale capaciteit 10. De instelling zou in dit geval twee terugkeerpatroon profielen, een worden uitgevoerd op het weekend en anderzijds op weekdagen bevatten.
    De instelling eruit als volgt:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    De instelling van de voorgaande bekijkt, zult u merken dat elk profiel terugkeerpatroon heeft een planning, dit schema wordt bepaald wanneer het profiel wordt uitgevoerd. Het profiel niet meer uitvoeren wanneer het tijd voor het uitvoeren van een ander profiel.

    In de vorige instelling 'weekdayProfile' ingesteld 12: 00, waardoor dit profiel wordt uitgevoerd op maandag op 12.am op maandag begint. Het blijft uitvoeren tot en met zaterdag 12a.m. als 'weekendProfile' is gepland om te beginnen met de uitvoering.

    **Voorbeeld 2: kantooruren** eens op een ander voorbeeld, misschien wilt u metrische drempelwaarde hebben = "x" tijdens kantooruren, 9 uur 17: 00 uur, en vervolgens naar 17: 00 uur naar 09: 00 de volgende dag, wilt u de metrische drempelwaarde moet 'y'.
    De instelling eruit als volgt:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Door te kijken naar de vorige instelling, begint 'businessHoursProfile' om 9 uur worden uitgevoerd op maandag en wordt uitgevoerd tot 17: 00 omdat als 'nonBusinessHoursProfile' wordt uitgevoerd. De 'nonBusinessHoursProfile' wordt uitgevoerd totdat het 9 uur Tuesday en vervolgens de 'businessHoursProfile' overneemt. Dit wordt herhaald totdat vrijdag 5 uur, op dat moment 'nonBusinessHoursProfile' wordt uitgevoerd naar maandag 09: 00 omdat de 'businessHoursProfile' begint niet uitvoeren tot maandag 09: 00
    
> [!Note]
> Automatisch schalen UX in de Azure portal wordt afgedwongen eindtijden voor terugkeerpatroon profielen en begint met het uitvoeren van de instelling voor automatisch schalen standaardprofiel Between terugkeerpatroon profielen.
    
## <a name="autoscale-evaluation"></a>Evaluatie voor automatisch schalen
Gegeven die automatisch schalen, kunnen instellingen hebben meerdere profielen voor automatisch schalen kan, en elk profiel meerdere metrische regels is het belangrijk om te begrijpen hoe een instelling voor automatisch schalen wordt geëvalueerd. Elke keer dat het automatisch schalen taak wordt uitgevoerd het begint met het kiezen van het profiel dat van toepassing is, nadat u hebt gekozen voor automatisch schalen die het profiel evalueert het minimum, maximum aantal waarden en metrische regels in het profiel en beslist als een schaalactie nodig is.

### <a name="which-profile-will-autoscale-pick"></a>Welk profiel worden automatisch schalen kiezen?
- Automatisch schalen zoekt eerst naar een datum-profiel dat is geconfigureerd voor nu uitvoeren als er is vastgesteld, automatisch schalen deze uitvoert. Als er meerdere vaste datum profielen die moeten worden uitgevoerd, wordt automatisch schalen het eerste item geselecteerd.
- Als er geen profielen vaste datum zijn, automatisch schalen terugkeerpatroon profielen, kijkt als gevonden, klikt u vervolgens het deze uitvoert.
- Als er geen vaste of het uitvoeren van het profiel voor reguliere terugkeerpatroon profielen en vervolgens automatisch schalen.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Hoe beoordeelt automatisch schalen meerdere regels

Wanneer automatisch schalen welk profiel moet bepaalt worden uitgevoerd, begint deze met het evalueren van de scale-out-regel in het profiel (regels met richting = 'Verhogen').
- Als een of meer regels voor scale-out worden geactiveerd, berekent automatisch schalen die de nieuwe capaciteit bepaald door de scaleAction van elk van deze regels. Vervolgens deze kan worden geschaald-out voor de maximale lengte van de capaciteit om te controleren of de beschikbaarheid van de service.
- Bijvoorbeeld: als er een virtuele-machineschaalset weergeven met een huidige capaciteit van 10 en er twee regels voor scale-out zijn; een die capaciteit met 10% wordt verhoogd, en een capaciteit met 3 verhoogd. De eerste regel resulteert in een nieuwe capaciteit van 11 en de tweede regel zou leiden tot een capaciteit van 13. Om te controleren of de beschikbaarheid van de service kiest automatisch schalen de actie die het resultaat in de maximale capaciteit, dus de tweede regel is gekozen.

Als er geen scale-out-regels worden geactiveerd, automatisch schalen evalueert alle schaal in regels (regels met richting = 'Verkleinen'). Automatisch schalen duurt slechts een actie schaal in als alle schaal in regels worden geactiveerd.
- Automatisch schalen berekend nieuwe capaciteit bepaald door de scaleAction van elk van deze regels. Vervolgens wordt de schaalactie die in het maximum van de capaciteit resulteert om te controleren of de beschikbaarheid van de service gekozen.
- Bijvoorbeeld: als er een virtuele-machineschaalset weergeven met een huidige capaciteit van 10 en er twee scale in regels zijn; een die capaciteit vermindert met 50% en een die capaciteit vermindert met 3. De eerste regel resulteert in een nieuwe capaciteit van 5 en de tweede regel zou leiden tot een capaciteit van 7. Om te controleren of de beschikbaarheid van de service kiest automatisch schalen de actie die het resultaat in de maximale capaciteit, dus de tweede regel is gekozen.

## <a name="next-steps"></a>Volgende stappen
Voor meer verwijzen informatie over automatisch schalen naar de volgende bronnen:

* [Overzicht van automatisch schalen](monitoring-overview-autoscale.md)
* [Azure Monitor voor automatisch schalen die de algemene metrische gegevens](insights-autoscale-common-metrics.md)
* [Aanbevolen procedures voor het Azure-Monitor voor automatisch schalen](insights-autoscale-best-practices.md)
* [Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook waarschuwingsmeldingen](insights-autoscale-to-webhook-email.md)
* [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931953.aspx)

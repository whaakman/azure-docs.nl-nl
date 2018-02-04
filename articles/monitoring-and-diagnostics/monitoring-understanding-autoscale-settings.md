---
title: Informatie over instellingen voor automatisch schalen in Azure | Microsoft Docs
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
ms.openlocfilehash: 73c79ec4ee1beb5220e088421c78ffffd932eef1
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="understand-autoscale-settings"></a>Inzicht in instellingen voor automatisch schalen
Instellingen voor automatisch schalen zorgen ervoor dat u de juiste hoeveelheid resources hebt voor het afhandelen van de fluctuerende belasting van uw toepassing wordt uitgevoerd. U kunt de instellingen voor automatisch schalen op basis van metrische gegevens die aangeven welke gegevens worden geladen of prestaties of geactiveerde worden geactiveerd op een geplande datum en tijd kunt configureren. In dit artikel wordt een gedetailleerd overzicht van de anatomie van een instelling voor automatisch schalen. Het artikel begint met het schema en de eigenschappen van een instelling, en vervolgens doorloopt de verschillende profieltypen die kunnen worden geconfigureerd. Ten slotte het artikel wordt beschreven hoe de functie voor automatisch schalen in Azure evalueert welk profiel moet worden uitgevoerd op een bepaald moment.

## <a name="autoscale-setting-schema"></a>Schema voor automatisch schalen instellen
Ter illustratie van het schema van de instelling voor automatisch schalen, is de volgende instelling voor automatisch schalen wordt gebruikt. Het is belangrijk te weten dat deze instelling voor automatisch schalen heeft:
- Een profiel. 
- Twee metrische regels in dit profiel: één voor scale-out en één voor de schaal.
  - De regel scale-out wordt geactiveerd wanneer de virtuele-machineschaalset van gemiddelde percentage CPU-metriek groter dan 85% gedurende de afgelopen 10 minuten is.
  - De regel voor schaal wordt geactiveerd wanneer de virtuele-machineschaalset van gemiddelde minder dan 60 procent is voor de afgelopen minuut.

> [!NOTE]
> Een instelling kan meerdere profielen hebben. Zie voor meer informatie, de [profielen](#autoscale-profiles) sectie. Een profiel kan ook hebben meerdere scale-out en schaal in regels gedefinieerd. Zie hoe ze worden geëvalueerd, de [evaluatie](#autoscale-evaluation) sectie.

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
| properties | targetResourceUri | De resource-ID van de bron wordt geschaald. U kunt slechts één instelling voor automatisch schalen per resource hebben. |
| properties | Profielen | Een instelling voor automatisch schalen bestaat uit een of meer profielen. Telkens wanneer de engine voor het automatisch schalen die wordt uitgevoerd, wordt uitgevoerd een profiel. |
| profiel | naam | De naam van het profiel. U kunt elke willekeurige naam waarmee u het profiel te identificeren. |
| profiel | Capacity.maximum | De maximale capaciteit toegestaan. Hiermee zorgt u ervoor dat automatisch schalen, bij het uitvoeren van dit profiel niet kan worden uitgebreid uw resource boven dit nummer. |
| profiel | Capacity.minimum | De minimale capaciteit toegestaan. Hiermee zorgt u ervoor dat automatisch schalen, bij het uitvoeren van dit profiel niet kan worden uitgebreid uw resource onder dit nummer. |
| profiel | Capacity.default | Als er een probleem bij het lezen van de metriek resource (in dit geval de CPU van 'vmss1'), en de huidige capaciteit lager dan de standaardwaarde is, schaalt automatisch schalen uit op de standaardwaarde. Dit is om te controleren of de beschikbaarheid van de resource. Als de huidige capaciteit al hoger dan de standaardcapaciteit is, automatisch schalen niet kan worden uitgebreid. |
| profiel | regels | Automatisch schalen schaalt automatisch tussen de maximale en minimale capaciteit, met behulp van de regels in het profiel. U kunt meerdere regels in een profiel hebben. Er zijn doorgaans twee regels: een om te bepalen wanneer moet worden uitgebreid, en de andere om te bepalen wanneer om in te schalen. |
| regel | metricTrigger | De metrische voorwaarde van de regel wordt gedefinieerd. |
| metricTrigger | metricName | De naam van de metrische gegevens. |
| metricTrigger |  metricResourceUri | De resource-ID van de resource die de metrische gegevens verzendt. In de meeste gevallen is dit hetzelfde is als de bron wordt geschaald. In sommige gevallen kan kan deze afwijken. U kunt bijvoorbeeld een virtuele-machineschaalset op basis van het aantal berichten in een opslagwachtrij schalen. |
| metricTrigger | TimeGrain | De duur van de metrische steekproeven. Bijvoorbeeld: **TimeGrain = 'PT1M'** betekent dat de metrische gegevens moeten worden samengevoegd elke 1 minuut, met behulp van de opgegeven in het element statistiek aggregatiemethode. |
| metricTrigger | statistiek | De aggregatiemethode binnen de periode timeGrain. Bijvoorbeeld: **statistiek = 'Gemiddelde'** en **timeGrain = 'PT1M'** betekent dat de metrische gegevens moeten worden samengevoegd elke 1 minuut, door het gemiddelde te nemen. Deze eigenschap bepaalt hoe de metriek is actieve. |
| metricTrigger | Waarde voor TimeWindow | De hoeveelheid tijd zoekt metrische gegevens. Bijvoorbeeld: **waarde voor timeWindow = 'PT10M'** betekent dat elke keer automatisch schalen die wordt uitgevoerd, wordt opgevraagd metrische gegevens voor de afgelopen 10 minuten. Het tijdvenster kunt metrische gegevens over uw genormaliseerd, en reageren op tijdelijke pieken voorkomt. |
| metricTrigger | timeAggregation | De aggregatiemethode die wordt gebruikt voor het cumuleren van de steekproef metrische gegevens. Bijvoorbeeld: **TimeAggregation van = 'Gemiddelde'** moet de steekproef metrische gegevens samenvoegen door het gemiddelde te nemen. In het voorgaande geval tien voorbeelden 1 minuut duren en gemiddelde ze. |
| regel | scaleAction | De actie die moet worden uitgevoerd wanneer de metricTrigger van de regel wordt geactiveerd. |
| scaleAction | richting | 'Verhogen' als u wilt uitbreiden of 'Verkleinen' schaal in.|
| scaleAction | waarde | Hoeveel vergroten of verkleinen van de capaciteit van de resource. |
| scaleAction | cooldown | De hoeveelheid tijd moet worden gewacht na een schaalaanpassing voordat schalen opnieuw. Bijvoorbeeld, als **cooldown = 'PT10M'**, automatisch schalen probeert niet te schalen opnieuw voor een andere 10 minuten. De cooldown is dat de metrische gegevens stabiel na het toevoegen of verwijderen van exemplaren. |

## <a name="autoscale-profiles"></a>Profielen voor automatisch schalen

Er zijn drie soorten profielen voor automatisch schalen:

- **Reguliere profiel:** het meest voorkomende profiel. Als u niet hoeft te schalen van uw resource op basis van de dag van de week of op een bepaalde dag, kunt u een reguliere-profiel gebruiken. Dit profiel kan vervolgens worden geconfigureerd met metrische regels die bepalen wanneer moet worden uitgebreid en wanneer om in te schalen. U hebt slechts één reguliere profiel gedefinieerd.

    Een voorbeeldprofiel voor het gebruikt eerder in dit artikel is een voorbeeld van een reguliere-profiel. Houd er rekening mee dat het is ook mogelijk om in te stellen van een profiel voor het schalen van een aantal statische exemplaar voor uw resource.

- **Vaste datum profiel:** dit profiel is bedoeld voor speciale gevallen. Stel dat u hebt een belangrijke gebeurtenis die afkomstig zijn van op 26 December 2017 (PST). Wilt u de minimale en maximale capaciteit van de resource op die dag, maar nog steeds op dezelfde metrische gegevens schaal andere zijn. In dit geval moet u een profiel voor een vaste datum toevoegen aan de instelling lijst met profielen. Het profiel is geconfigureerd om te worden uitgevoerd op de dag van de gebeurtenis. Voor de dag, automatisch schalen die gebruikmaakt van het normale profiel.

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
    
- **Terugkeerpatroon van profiel:** dit type profiel kunt u om ervoor te zorgen dat dit profiel altijd op een bepaalde dag van de week wordt gebruikt. Terugkeerpatroon profielen hebben alleen een begin-tijdstip. Ze worden uitgevoerd totdat het volgende profiel voor het terugkeerpatroon of vaste datum profiel is ingesteld om te starten. Een instelling voor automatisch schalen met slechts één terugkeerpatroon profiel wordt uitgevoerd dat profiel, zelfs als er een reguliere profiel dat is gedefinieerd in dezelfde instelling. De volgende twee voorbeelden laten zien hoe dit profiel wordt gebruikt:

    **Voorbeeld 1: Weekdagen versus tijdens het weekend**
    
    Stel dat in het weekend, wilt u de maximale capaciteit moet 4. Op weekdagen, omdat u meer load verwacht u de maximale capaciteit 10. De instelling zou in dit geval twee terugkeerpatroon profielen, een worden uitgevoerd op het weekend en anderzijds op weekdagen bevatten.
    De instelling ziet er als volgt:

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

    De voorgaande instelling ziet u dat elk profiel terugkeerpatroon een planning heeft. Dit schema bepaalt wanneer het profiel wordt gestart met. Het profiel stopt wanneer is het tijd om het uitvoeren van een ander profiel.

    In de vorige instelling 'weekdayProfile' ingesteld om 12:00 uur op maandag begint. Dit betekent dat dit profiel wordt gestart op maandag om 12:00 uur. Gaat door totdat zaterdag om 12:00 uur, wanneer 'weekendProfile' volgens de planning wilt uitvoeren.

    **Voorbeeld 2: kantooruren**
    
    Stel dat u wilt één metrische drempelwaarde tijdens kantooruren (9:00 uur tot 5:00) en een ander voor alle andere tijden hebben. De instelling eruit als volgt:
    
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
    
    De instelling van de voorgaande toont 'businessHoursProfile' begint op maandag uitgevoerd om 9:00 uur en 17:00 uur blijft. Als 'nonBusinessHoursProfile' wordt gestart. De 'nonBusinessHoursProfile' wordt uitgevoerd tot en met 9:00 uur op dinsdag en vervolgens de 'businessHoursProfile' overneemt opnieuw. Dit wordt herhaald tot en met vrijdag op 17:00 uur. "NonBusinessHoursProfile" wordt op dat moment helemaal tot aan de maandag uitgevoerd om 9:00 uur.
    
> [!Note]
> De gebruikersinterface automatisch schalen in Azure portal wordt afgedwongen eindtijden voor terugkeerpatroon profielen en begint met het uitvoeren van de instelling voor automatisch schalen standaardprofiel Between terugkeerpatroon profielen.
    
## <a name="autoscale-evaluation"></a>Evaluatie voor automatisch schalen
Gezien het feit dat de instellingen voor automatisch schalen kunnen meerdere profielen hebben, en elk profiel kan meerdere metrische regels hebben, is het belangrijk te begrijpen hoe een instelling voor automatisch schalen wordt geëvalueerd. Telkens wanneer de taak voor automatisch schalen die wordt uitgevoerd, begint deze door het kiezen van het profiel dat van toepassing is. Vervolgens automatisch schalen evalueert de minimale en maximale waarden en metrische regels in het profiel en beslist als een schaalactie nodig is.

### <a name="which-profile-will-autoscale-pick"></a>Welk profiel worden automatisch schalen kiezen?

De volgende reeks voor automatisch schalen gebruikt om op te halen van het profiel:
1. Eerst wordt gezocht voor alle vaste datum-profiel dat is geconfigureerd voor het nu uitvoeren. Als er, automatisch schalen die worden uitgevoerd. Als er meerdere vaste datum profielen die moeten worden uitgevoerd, selecteert automatisch schalen eerste.
2. Als er geen profielen vaste datum zijn, kijkt automatisch schalen terugkeerpatroon profielen. Als een terugkeerpatroon-profiel wordt gevonden, wordt uitgevoerd.
3. Als er geen vaste datum of een terugkeerpatroon profielen zijn, uitgevoerd voor automatisch schalen die het normale profiel.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Hoe beoordeelt automatisch schalen meerdere regels

Nadat automatisch schalen welk profiel moet worden uitgevoerd bepaalt, resulteert de scale-out-regels in het profiel (dit zijn de regels met **richting = 'Verhogen'**).

Als een of meer regels voor scale-out worden geactiveerd, automatisch schalen berekent de nieuwe capaciteit bepaald door de **scaleAction** van elk van deze regels. Vervolgens het uitgeschaald naar het maximale aantal die capaciteiten, zodat de beschikbaarheid van de service.

Bijvoorbeeld, Stel dat er een virtuele-machineschaalset is ingesteld met een huidige capaciteit van 10. Er zijn twee scale-out regels: een capaciteit met 10 procent verhoogd, en een die capaciteit met 3 aantallen wordt verhoogd. De eerste regel resulteert in een nieuwe capaciteit van 11 en de tweede regel zou leiden tot een capaciteit van 13. Beschikbaarheid van de service, zodat kiest automatisch schalen de actie die het resultaat in de maximale capaciteit, dus de tweede regel is gekozen.

Als er geen scale-out-regels worden geactiveerd, automatisch schalen evalueert alle schaal in regels (regels met **richting = 'Verkleinen'**). Automatisch schalen duurt slechts een actie schaal in als alle schaal in regels worden geactiveerd.

Automatisch schalen berekent de nieuwe capaciteit bepaald door de **scaleAction** van elk van deze regels. Vervolgens wordt de schaalactie die in het maximum van de capaciteit resulteert om te controleren of de beschikbaarheid van de service gekozen.

Bijvoorbeeld, Stel dat er een virtuele-machineschaalset is ingesteld met een huidige capaciteit van 10. Er zijn twee scale regels: één dat minder capaciteit 50 procent en één die capaciteit met 3 aantallen verlaagd. De eerste regel resulteert in een nieuwe capaciteit van 5 en de tweede regel zou leiden tot een capaciteit van 7. Beschikbaarheid van de service, zodat kiest automatisch schalen de actie die het resultaat in de maximale capaciteit, dus de tweede regel is gekozen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over automatisch schalen door te verwijzen naar het volgende:

* [Overzicht van automatisch schalen](monitoring-overview-autoscale.md)
* [Azure Monitor voor automatisch schalen die de algemene metrische gegevens](insights-autoscale-common-metrics.md)
* [Aanbevolen procedures voor het Azure-Monitor voor automatisch schalen](insights-autoscale-best-practices.md)
* [Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook waarschuwingsmeldingen](insights-autoscale-to-webhook-email.md)
* [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931953.aspx)

---
title: Informatie over de instellingen voor automatisch schalen in Azure Monitor
description: Een gedetailleerd overzicht van instellingen voor automatisch schalen en hoe deze werken. Is van toepassing op virtuele Machines, Cloudservices, Web-Apps
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 2347d82b8c2f5a08b944577e5b06cde3b68617b3
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53385003"
---
# <a name="understand-autoscale-settings"></a>Inzicht in instellingen voor automatisch schalen
Instellingen voor automatisch schalen kunt u ervoor zorgen dat u hebt de juiste hoeveelheid resources die worden uitgevoerd om de wisselende belasting van uw toepassing te voldoen. U kunt de instellingen voor automatisch schalen op basis van metrische gegevens die wijzen op belasting of prestaties of trigger wordt geactiveerd op een geplande datum en tijd configureren. In dit artikel wordt een gedetailleerde Kijk op de anatomie van een instelling voor automatisch schalen. Het artikel begint met het schema en de eigenschappen van een instelling en klikt u vervolgens helpt bij de verschillende profieltypen die kunnen worden geconfigureerd. Ten slotte het artikel wordt beschreven hoe de functie voor automatisch schalen in Azure evalueert profiel om uit te voeren op een bepaald moment.

## <a name="autoscale-setting-schema"></a>Schema voor automatisch schalen instellen
Ter illustratie van het schema van de instelling voor automatisch schalen, wordt de volgende instelling voor automatisch schalen gebruikt. Het is belangrijk te weten dat deze instelling voor automatisch schalen heeft:
- Een profiel. 
- Twee metrische regels in dit profiel: één voor scale-out, en één voor inschalen.
  - De scale-out-regel wordt geactiveerd wanneer de virtuele-machineschaalset gemiddeld percentage CPU-metriek groter dan 85 procent voor de afgelopen 10 minuten is.
  - De regel voor inschalen wordt geactiveerd wanneer het gemiddelde van de virtuele-machineschaalset is minder dan 60 procent voor de afgelopen minuut.

> [!NOTE]
> Een instelling kan meerdere profielen hebben. Zie voor meer informatie, de [profielen](#autoscale-profiles) sectie. Een profiel kan ook zijn meerdere regels voor scale-out en schaal regels gedefinieerd. Als u wilt zien hoe ze worden geëvalueerd, Zie de [evaluatie](#autoscale-evaluation) sectie.

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

| Sectie | De naam van element | Description |
| --- | --- | --- |
| Instelling | Id | De instelling voor automatisch schalen van resource-ID. Instellingen voor automatisch schalen vormen een Azure Resource Manager-resource. |
| Instelling | naam | De naam van de instelling voor automatisch schalen. |
| Instelling | location | De locatie van de instelling voor automatisch schalen. Deze locatie kan afwijken van de locatie van de resource die wordt geschaald. |
| properties | targetResourceUri | De resource-ID van de resource die wordt geschaald. U kunt slechts één instelling voor automatisch schalen per resource hebben. |
| properties | Profielen | Een instelling voor automatisch schalen bestaat uit een of meer profielen. Telkens wanneer de engine voor automatisch schalen wordt uitgevoerd, uitvoeren het van één profiel. |
| profiel | naam | De naam van het profiel. U kunt een willekeurige naam die helpt u bij het identificeren van het profiel. |
| profiel | Capacity.maximum | De maximale capaciteit toegestaan. Het zorgt ervoor dat voor automatisch schalen, bij het uitvoeren van dit profiel niet omhoog uw resource boven dit getal schalen. |
| profiel | Capacity.minimum | De opgegeven Minimumcapaciteit toegestaan. Het zorgt ervoor dat voor automatisch schalen, bij het uitvoeren van dit profiel niet omhoog uw resource onder dit getal schalen. |
| profiel | Capacity.Default | Als er een probleem opgetreden bij het lezen van de metrische resource gegevens (in dit geval de CPU van 'vmss1'), en de huidige capaciteit lager dan de standaardwaarde is, wordt de functie voor automatisch schalen uitgeschaald op de standaardwaarde. Dit is om te controleren of de beschikbaarheid van de resource. Als de huidige capaciteit al hoger dan de standaardcapaciteit is, schaalt automatisch schalen niet. |
| profiel | regels | Automatisch schalen wordt automatisch aangepast tussen de maximale en minimale capaciteit, met behulp van de regels in het profiel. U kunt meerdere regels in een profiel hebben. Er zijn doorgaans twee regels: een om te bepalen wanneer om uit te schalen, en de andere om te bepalen wanneer om in te schalen. |
| regel | metricTrigger | Hiermee definieert u de metrische gegevens van de regel. |
| metricTrigger | MetricName | De naam van de metrische gegevens. |
| metricTrigger |  metricResourceUri | De resource-ID van de resource die de metrische gegevens verzendt. In de meeste gevallen is dit hetzelfde als de resource die wordt geschaald. In sommige gevallen kan deze afwijken. U kunt bijvoorbeeld een virtuele-machineschaalset op basis van het aantal berichten in een opslagwachtrij schalen. |
| metricTrigger | timeGrain | De duur van de metrische steekproef nemen. Bijvoorbeeld, **TimeGrain = "PT1M"** betekent dat de metrische gegevens moeten worden samengevoegd om de minuut, met behulp van de aggregatiemethode die is opgegeven in het element statistiek. |
| metricTrigger | statistiek | De samenvoegingsmethode binnen de timeGrain-periode. Bijvoorbeeld, **statistiek = "Gemiddelde"** en **timeGrain = "PT1M"** betekent dat de metrische gegevens moeten worden samengevoegd om de minuut, door het gemiddelde te nemen. Deze eigenschap bepaalt hoe de metrische gegevens verzameld. |
| metricTrigger | timeWindow | De hoeveelheid tijd terugkijken voor metrische gegevens. Bijvoorbeeld, **timeWindow = "PT10M"** betekent dat telkens wanneer automatisch schalen wordt uitgevoerd, vraagt het metrische gegevens voor de afgelopen 10 minuten. De periode kan uw metrische gegevens moeten worden genormaliseerd, en voorkomt u reageren op tijdelijke pieken. |
| metricTrigger | timeAggregation | De aggregatiemethode die wordt gebruikt om de metrische. Bijvoorbeeld, **TimeAggregation = "Gemiddelde"** moet de metrische aggregeren op basis van het gemiddelde te nemen. In het geval is voorgaande, de tien voorbeelden van 1 minuut duren en gemiddelde ze. |
| regel | scaleAction | De actie die moet worden uitgevoerd wanneer de metricTrigger van de regel wordt geactiveerd. |
| scaleAction | richting | "Verhogen' als u wilt uitschalen, of 'Verkleinen' om te schalen in.|
| scaleAction | waarde | Hoeveel vergroten of verkleinen van de capaciteit van de resource. |
| scaleAction | afkoeltijd | De hoeveelheid tijd moet wachten na een schaalbewerking voordat opnieuw kan worden geschaald. Bijvoorbeeld, als **afkoeltijd = "PT10M"**, automatisch schalen wordt niet getracht te schalen opnieuw voor een andere 10 minuten. De afkoeltijd is om de metrische waarden na het toevoegen of verwijderen van exemplaren. |

## <a name="autoscale-profiles"></a>Profielen voor automatisch schalen

Er zijn drie typen profielen voor automatisch schalen:

- **Reguliere profiel:** De meest voorkomende profiel. Als u niet nodig hebt voor het schalen van uw resource, gebaseerd op de dag van de week of op een bepaalde dag, kunt u een reguliere-profiel gebruiken. Dit profiel kan vervolgens worden geconfigureerd met metrische regels die bepalen wanneer een om uit te schalen en wanneer om in te schalen. U mag slechts één reguliere profiel gedefinieerd hebben.

    De voorbeeldprofiel gebruikt eerder in dit artikel is een voorbeeld van een reguliere-profiel. Houd er rekening mee dat het is ook mogelijk om in te stellen van een profiel om te schalen naar een aantal statische instanties voor uw resource.

- **Vaste datum profiel:** Dit profiel is bedoeld voor speciale gevallen. Stel dat u hebt een belangrijke gebeurtenis die afkomstig zijn van op 26 December 2017 (PST). Wilt u de minimale en maximale capaciteit van uw resource worden verschillende op die dag, maar nog steeds schaal op dezelfde metrische gegevens. In dit geval moet u een vaste datum profiel toevoegen aan uw instelling als u de lijst met profielen. Het profiel is geconfigureerd om te worden uitgevoerd op de dag van de gebeurtenis. Voor een andere dag voor automatisch schalen gebruikmaakt van het normale profiel.

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
    
- **Terugkeerpatroon profiel:** Dit type profiel kunt u om ervoor te zorgen dat dit profiel altijd wordt gebruikt op een bepaalde dag van de week. Terugkeerpatroon profielen hebben alleen een begintijd. Ze worden uitgevoerd tot het volgende terugkeerpatroon profiel of vaste datum profiel is ingesteld om te starten. Een instelling voor automatisch schalen met slechts één terugkeerpatroon profiel wordt uitgevoerd dat profiel, zelfs als er een reguliere-profiel in dezelfde instelling gedefinieerd. De volgende twee voorbeelden laten zien hoe dit profiel wordt gebruikt:

    **Voorbeeld 1: Weekdagen versus weekenden**
    
    Stel dat tijdens het weekend, kunt u de maximale capaciteit moet 4. Op weekdagen, omdat u meer laden verwacht, wilt u de maximale capaciteit 10. De instelling zou in dit geval twee profielen voor terugkeerpatroon, een om uit te voeren op tijdens het weekend en de andere op weekdagen bevatten.
    De instelling er zo uit:

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

    De instelling van de voorgaande laat zien dat elk profiel terugkeerpatroon een schedule heeft. Dit schema bepaalt wanneer het profiel wordt gestart met. Het profiel stopt wanneer het is nu tijd om uit te voeren van een ander profiel.

    In de vorige instelling is 'weekdayProfile' bijvoorbeeld ingesteld op maandag begint om 12:00 uur. Dit betekent dat dit profiel wordt uitgevoerd op maandag om 12:00 uur. Het blijft tot en met zaterdag om 12:00 uur, wanneer 'weekendProfile' beginnen met het uitvoeren is gepland.

    **Voorbeeld 2: kantooruren**
    
    Stel dat u wilt één metrische drempelwaarde tijdens kantooruren (9:00 uur tot 5:00 uur) en een andere classificatie voor alle andere tijden hebben. De instelling zou er als volgt:
    
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
    
    De instelling van de voorgaande ziet u dat 'businessHoursProfile' wordt uitgevoerd op maandag om 9:00 uur en 17:00 uur blijft. Als 'nonBusinessHoursProfile' wordt uitgevoerd. De 'nonBusinessHoursProfile' wordt uitgevoerd tot en met 9:00 uur op dinsdag de "businessHoursProfile" vervolgens heeft ten opzichte van het opnieuw. Dit wordt herhaald tot en met vrijdag om 17:00 uur. "NonBusinessHoursProfile" uitvoert op dat moment helemaal aan maandag om 9:00 uur.
    
> [!Note]
> De gebruikersinterface voor automatisch schalen in Azure portal wordt afgedwongen eindtijden voor terugkeerpatroon profielen, en begint met het uitvoeren van de instelling voor automatisch schalen standaardprofiel tussen terugkeerpatroon profielen.
    
## <a name="autoscale-evaluation"></a>Evaluatie van de functie voor automatisch schalen
Gezien het feit dat instellingen voor automatisch schalen kunnen meerdere profielen, en elk profiel kan meerdere metrische regels hebben, is het belangrijk om te begrijpen hoe een instelling voor automatisch schalen wordt geëvalueerd. Telkens wanneer de taak voor automatisch schalen wordt uitgevoerd, begint deze door het kiezen van het profiel dat van toepassing is. Vervolgens voor automatisch schalen wordt geëvalueerd als de minimale en maximale waarden en metrische regels in het profiel en besluit als een schaalactie nodig is.

### <a name="which-profile-will-autoscale-pick"></a>Profiel kiest voor automatisch schalen?

Automatisch schalen maakt gebruik van de volgende procedure om op te halen van het profiel:
1. Eerst wordt gezocht voor alle vaste datum-profiel dat is geconfigureerd voor het nu uitvoeren. Als er, voor automatisch schalen wordt uitgevoerd. Als er meerdere vaste datum profielen die zijn moet worden uitgevoerd, wordt het eerste item geselecteerd in voor automatisch schalen.
2. Als er geen profielen vaste datum zijn, kijkt voor automatisch schalen terugkeerpatroon profielen. Als een recurrence-profiel wordt gevonden, deze wordt uitgevoerd.
3. Als er geen vaste datum of terugkeerpatroon profielen zijn, uitgevoerd voor automatisch schalen het reguliere profiel.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Hoe beoordeelt voor automatisch schalen meerdere regels

Nadat de functie voor automatisch schalen wordt vastgesteld welke profiel om uit te voeren, de scale-out-regels in het profiel worden geëvalueerd (dit zijn regels met **richting = "Verhogen"**).

Als een of meer regels voor scale-out worden geactiveerd, automatisch schalen wordt berekend voor de nieuwe capaciteit bepaald door de **scaleAction** van elk van deze regels. Vervolgens wordt er opgeschaald om naar het maximum van de capaciteit, om te controleren of de beschikbaarheid van de service.

Bijvoorbeeld: Stel dat er wordt een virtuele-machineschaalset met een huidige capaciteit van 10. Er zijn twee regels van de scale-out: één die capaciteit verhoogd met 10 procent en één die capaciteit met 3 aantallen wordt verhoogd. De eerste regel resulteert in een nieuwe capaciteit van 11 en de tweede regel zou leiden tot een capaciteit van 13. Om te garanderen de beschikbaarheid van de service, kiest voor automatisch schalen de actie die ertoe leidt dat de maximale capaciteit, dus de tweede regel wordt gekozen.

Als er geen scale-out-regels worden geactiveerd, automatisch schalen wordt geëvalueerd als alle schaal regels (regels met **richting = "Verkleinen"**). Een inschaalactie nodig voor automatisch schalen alleen als alle regels schaal worden geactiveerd.

Automatisch schalen wordt berekend voor de nieuwe capaciteit bepaald door de **scaleAction** van elk van deze regels. Vervolgens wordt de schaalactie die in het maximum van de capaciteit resulteert om te controleren of de beschikbaarheid van de service gekozen.

Bijvoorbeeld: Stel dat er wordt een virtuele-machineschaalset met een huidige capaciteit van 10. Er zijn twee regels voor inschalen: één die capaciteit met 50 procent verlaagd en één die capaciteit vermindert door 3 telt. De eerste regel resulteert in een nieuwe capaciteit van 5 en de tweede regel zou leiden tot een capaciteit van 7. Om te garanderen de beschikbaarheid van de service, kiest voor automatisch schalen de actie die ertoe leidt dat de maximale capaciteit, dus de tweede regel wordt gekozen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over automatisch schalen door te verwijzen naar het volgende:

* [Overzicht van automatisch schalen](../azure-monitor/platform/autoscale-overview.md)
* [Azure Monitor voor automatisch schalen de algemene metrische gegevens](../azure-monitor/platform/autoscale-common-metrics.md)
* [Aanbevolen procedures voor automatisch schalen van Azure Monitor](../azure-monitor/platform/autoscale-best-practices.md)
* [Acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook waarschuwingsmeldingen](../azure-monitor/platform/autoscale-webhook-email.md)
* [REST-API voor automatisch schalen](https://msdn.microsoft.com/library/dn931953.aspx)

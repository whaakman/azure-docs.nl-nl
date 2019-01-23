---
title: Geavanceerd automatisch schalen met Azure Virtual Machines
description: Maakt gebruik van Resource Manager en VM Scale Sets met meerdere regels en -profielen die e-mailbericht verzenden en webhook-URL's met schaalacties aanroepen.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2016
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 074f090d295ef5eafad48e57f68dad019bf7eab9
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470032"
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>Geavanceerd automatisch schalen configureren met behulp van Resource Manager-sjablonen voor VM-Schaalsets
U kunt in- en scale-out in Virtual Machine Scale Sets op basis van prestaties metrische drempels, door een terugkerend schema, of door een bepaalde datum. U kunt ook e-mail en webhook-meldingen voor schaalacties configureren. In dit scenario ziet u een voorbeeld van het configureren van al deze objecten met behulp van Resource Manager-sjabloon voor een VM-Schaalset.

> [!NOTE]
> Hoewel in dit scenario de stappen beschreven voor VM-Schaalsets worden, de dezelfde informatie geldt voor automatisch schalen [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts) Voor een eenvoudige schaalset in/uit-instelling op een VM-Schaalset zijn gebaseerd op een eenvoudige prestaties metrische gegevens zoals CPU, verwijzen naar de [Linux](../../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-cli.md) en [Windows](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) documenten
>
>

## <a name="walkthrough"></a>Walkthrough
In dit scenario gebruiken we [Azure Resource Explorer](https://resources.azure.com/) configureren en bijwerken van de instelling voor automatisch schalen voor een schaalset. Azure Resource Explorer is een eenvoudige manier voor het beheren van Azure-resources via Resource Manager-sjablonen. Als u niet bekend bent met Azure Resource Explorer hulpprogramma, leest u [deze inleiding](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Een nieuwe schaalset met een instelling voor automatisch schalen te implementeren. Dit artikel wordt gebruikgemaakt van de Azure QuickStart-galerie, met een Windows-schaalset met een sjabloon voor automatisch schalen. Schaalsets voor Linux op dezelfde manier werken.
2. Nadat de schaalset is gemaakt, gaat u naar de resource van de set schaal van Azure Resource Explorer. U ziet het volgende onder het knooppunt Microsoft.Insights.

    ![Azure Explorer](media/autoscale-virtual-machine-scale-sets/azure_explorer_navigate.png)

    De uitvoering van de sjabloon voor een standaard-instelling voor automatisch schalen is gemaakt met de naam **'autoscalewad'**. Aan de rechterkant vindt u de volledige definitie van deze instelling voor automatisch schalen. In dit geval wordt de instelling voor automatisch schalen wordt geleverd met een CPU % op basis van scale-out en schalen in de regel.  

3. U kunt nu meer profielen en regels op basis van de specifieke vereisten of schema toevoegen. We maken een instelling voor automatisch schalen met drie profielen. Bekijk voor meer informatie over profielen en regels voor automatisch schalen, [aanbevolen procedures voor automatisch schalen](autoscale-best-practices.md).  

    | Profielen & regels | Description |
    |--- | --- |
    | **Profiel** |**Op basis van metrische gegevens en prestaties** |
    | Regel |Aantal berichten Service Bus-wachtrij > x |
    | Regel |Aantal berichten Service Bus-wachtrij < y |
    | Regel |CPU-percentage > n |
    | Regel |CPU-percentage < p |
    | **Profiel** |**Weekdag uur's ochtends (geen regels)** |
    | **Profiel** |**Product starten dag (geen regels)** |

4. Hier volgt een hypothetische vergroten/verkleinen scenario die we voor deze procedure ook gebruiken.

    * **Belasting op basis van** -ik wil graag uit of in te schalen op basis van de belasting van mijn toepassing die wordt gehost op mijn set.* schaal
    * **Grootte van de wachtrij bericht** -ik gebruik een Service Bus-wachtrij voor de binnenkomende berichten voor mijn toepassing. Ik gebruik het aantal berichten en de CPU-percentage van de wachtrij en een standaardprofiel voor het activeren van een schaalactie als een van het aantal berichten of CPU treffers in de threshold.* configureren
    * **Tijd van de week en dag** -ik wil een wekelijks terugkerende 'tijd van de dag' op basis van profiel met de naam 'Doordeweekse dag's ochtends uur'. Op basis van historische gegevens, weet ik is het beter dat een bepaald aantal VM-exemplaren voor het afhandelen van mijn toepassing laden tijdens deze tijd.*
    * **Speciale datums** -ik heb een profiel 'Product starten dag' toegevoegd. Ik plan vooruit voor specifieke datums, zodat mijn toepassing gereed om af te handelen van de belasting die verschuldigd marketing aankondigingen en is als we een nieuw product in de App.* plaatst
    * *De laatste twee profielen kunnen ook andere prestaties metrische gegevens op basis van regels binnen deze hebben. In dit geval geen besloot en in plaats daarvan op basis van regels om te vertrouwen op de prestaties van standaardgegevens. Regels zijn optioneel voor de terugkerende en op basis van datum-profielen.*

    Prioriteitsaanduiding van de profielen en regels voor automatisch schalen-engine wordt ook vastgelegd in de [aanbevolen procedures voor automatisch schalen](autoscale-best-practices.md) artikel.
    Raadpleeg voor een lijst met algemene metrische gegevens voor automatisch schalen, [algemene metrische gegevens voor automatisch schalen](autoscale-common-metrics.md)

5. Zorg dat u zich op de **lezen/schrijven** modus in Resource Explorer

    ![Autoscalewad, instelling standaard voor automatisch schalen](media/autoscale-virtual-machine-scale-sets/autoscalewad.png)

6. Klik op bewerken. **Vervang** het element 'profielen' in de instelling voor automatisch schalen met de volgende configuratie:

    ![Profielen](media/autoscale-virtual-machine-scale-sets/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
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
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
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
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
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
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Zie voor ondersteunde velden en hun waarden [voor automatisch schalen REST API-documentatie](https://msdn.microsoft.com/library/azure/dn931928.aspx). De instelling voor automatisch schalen, bevat nu de drie profielen zoals eerder is uitgelegd.

7. Ten slotte kijken naar de functie voor automatisch schalen **melding** sectie. Meldingen over automatisch schalen zodat u drie dingen als een scale-out of in de actie met succes wordt geactiveerd.
   - De beheerder en co-beheerders van uw abonnement
   - E-een groep gebruikers
   - Activeren van een webhook-aanroep. Wanneer er wordt gestart, webhook stuurt metagegevens van de voorwaarde voor automatisch schalen en de bron van de schaalset. Zie voor meer informatie over de nettolading van de webhook voor automatisch schalen, [Webhook configureren & e-mailmeldingen voor automatisch schalen](autoscale-webhook-email.md).

   Voeg het volgende toe ter vervanging van automatisch schalen instelling uw **melding** element waarvan de waarde null is

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]

   ```

   Raak **plaatsen** knop in Resource Explorer om bij te werken van de instelling voor automatisch schalen.

U kunt een instelling voor automatisch schalen op een VM-Schaalset bevatten meerdere profielen voor schalen en schalen van meldingen zijn bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
Gebruik deze koppelingen voor meer informatie over automatisch schalen.

[Automatisch schalen met Schaalsets voor virtuele machines oplossen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Algemene metrische gegevens voor automatisch schalen](autoscale-common-metrics.md)

[Aanbevolen procedures voor automatisch schalen van Azure](autoscale-best-practices.md)

[Automatisch schalen met behulp van PowerShell beheren](../../azure-monitor/platform/powershell-quickstart-samples.md#create-and-manage-autoscale-settings)

[Automatisch schalen met behulp van CLI beheren](cli-samples.md#autoscale)

[Webhook & e-mailmeldingen voor automatisch schalen configureren](autoscale-webhook-email.md)

[Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings) sjabloonverwijzing

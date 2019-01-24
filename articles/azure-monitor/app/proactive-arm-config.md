---
title: Azure Application Insights-instellingen voor slimme detectie regel configureren met Azure Resource Manager-sjablonen | Microsoft Docs
description: Geautomatiseerd beheer en de configuratie van Azure Application Insights-regels voor slimme detectie met Azure Resource Manager-sjablonen
services: application-insights
documentationcenter: ''
author: harelbr
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/19/2018
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: edfd908166e4334bdfda0f043cba727cb0370405
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853592"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Regels voor slimme detectie van Application Insights met behulp van Azure Resource Manager-sjablonen beheren

Regels voor slimme detectie in Application Insights kunnen worden beheerd en geconfigureerd met behulp van [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).
Deze methode kan worden gebruikt bij het implementeren van nieuwe Application Insights-resources met Azure Resource Manager-automatisering, of voor het wijzigen van de instellingen van bestaande resources.

## <a name="smart-detection-rule-configuration"></a>Configuratie van de regel voor slimme detectie

U kunt de volgende instellingen voor een regel voor slimme detectie configureren:
- Als de regel is ingeschakeld (de standaardwaarde is **waar**.)
- Als e-mailberichten moeten worden verzonden naar de abonnementseigenaren, bijdragers en lezers wanneer een detectie is gevonden (de standaardwaarde is **waar**.)
- Eventuele aanvullende e-mailontvangers die een melding krijgen moeten wanneer er een detectie is gevonden.

Als u wilt toestaan dat de regelinstellingen via Azure Resource Manager configureren, de configuratie van de regel voor slimme detectie is nu beschikbaar als een binnenste bron binnen de Application Insights-resource met de naam **ProactiveDetectionConfigs**.
Elke regel voor slimme detectie kan worden geconfigureerd met unieke meldingsinstellingen, voor maximale flexibiliteit.

## <a name="examples"></a>Voorbeelden

Hieronder vindt u enkele voorbeelden die laten zien hoe u configureert de instellingen van regels voor slimme detectie met behulp van Azure Resource Manager-sjablonen.
Alle voorbeelden verwijzen naar een Application Insights-resource met de naam _"myApplication"_, en aan de 'afhankelijkheid lange duur van de regel voor slimme detectie", die intern heet _"longdependencyduration"_.
Zorg ervoor dat ter vervanging van de naam van de Application Insights-resource, en om op te geven van de interne naam van de relevante Slimme detectie. Controleer de onderstaande tabel voor een lijst van de bijbehorende interne Azure Resource Manager-namen voor elke regel voor slimme detectie.

### <a name="disable-a-smart-detection-rule"></a>Een regel voor slimme detectie uitschakelen

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>E-mailmeldingen verzenden voor een regel voor slimme detectie uitschakelen

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>Toevoegen van aanvullende e-mailontvangers voor een regel voor slimme detectie

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```

## <a name="smart-detection-rule-names"></a>Namen van de regel voor slimme detectie

Hieronder ziet u een tabel met de namen van Slimme detectie zoals ze worden weergegeven in de portal, samen met hun interne namen, die moet worden gebruikt in de Azure Resource Manager-sjabloon.

> [!NOTE]
> Regels voor slimme detectie gemarkeerd als Preview-versie bieden geen ondersteuning voor e-mailmeldingen. Daarom kunt u alleen de eigenschap enabled voor deze regels ingesteld. 

| Azure portal regelnaam | Interne naam
|:---|:---|
| Langzame paginalaadtijd | slowpageloadtime |
| Langzame reactietijd voor de server | slowserverresponsetime |
| Lange duur van de afhankelijkheid | longdependencyduration |
| Degradatie van de serverreactietijd | degradationinserverresponsetime |
| Degradatie van afhankelijkheidsduur | degradationindependencyduration |
| Degradatie traceren ernst breedteverhouding (preview) | extension_traceseveritydetector |
| Abnormale toename van uitzonderingen (preview) | extension_exceptionchangeextension |
| Mogelijk geheugenlek gedetecteerd (preview) | extension_memoryleakextension |
| Potentiële beveiligingsproblemen gedetecteerd (preview) | extension_securityextensionspackage |
| Resource-gebruik probleem gedetecteerd (preview) | extension_resourceutilizationextensionspackage |

## <a name="who-receives-the-classic-alert-notifications"></a>Wie de (klassiek) waarschuwingsmeldingen ontvangen?

In deze sectie is alleen van toepassing op klassieke waarschuwingen voor slimme detectie en helpt u optimaliseren van uw meldingen van waarschuwingen om ervoor te zorgen dat alleen de gewenste geadresseerden meldingen ontvangen. Meer informatie geven over het verschil tussen de [klassieke waarschuwingen] (.. /platform/Alerts-Classic.Overview.MD en de nieuwe ervaring voor waarschuwingen verwijzen naar de [waarschuwingen overzichtsartikel](../platform/alerts-overview.md). Slimme detectie waarschuwingen op dit moment alleen ondersteuning voor de klassieke waarschuwingen optreden. De enige uitzondering hierop is [slimme detectiewaarschuwingen in Azure cloud services](./proactive-cloud-services.md). Melding voor slimme detectiewaarschuwingen over Azure cloud services voor het beheren van waarschuwing maken gebruik van [actiegroepen](../platform/action-groups.md).

* We raden het gebruik van specifieke ontvangers voor slimme detectie en klassieke waarschuwingen.

* Voor slimme detectiewaarschuwingen, de **bulksgewijs/groep** selectievakje, indien ingeschakeld, verzendt naar gebruikers met de rol van eigenaar, bijdrager of lezer in het abonnement. In feite _alle_ gebruikers met toegang tot het abonnement de Application Insights-resource in het bereik en meldingen ontvangt. 

> [!NOTE]
> Als u momenteel gebruikmaakt van de **bulksgewijs/groep** selectievakje, en uitschakelen, kunt u zich niet meer herstellen van de wijziging.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatisch detecteren:

- [Foutafwijkingen](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Geheugenlekken](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Prestatieafwijkingen](../../azure-monitor/app/proactive-performance-diagnostics.md)
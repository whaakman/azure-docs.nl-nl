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
ms.date: 06/26/2019
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: 6bb89eec0b4905e101bed87d3d3fc617dec589e0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477865"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Regels voor slimme detectie van Application Insights met behulp van Azure Resource Manager-sjablonen beheren

Regels voor slimme detectie in Application Insights kunnen worden beheerd en geconfigureerd met behulp van [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).
Deze methode kan worden gebruikt bij het implementeren van nieuwe Application Insights-resources met Azure Resource Manager-automatisering, of voor het wijzigen van de instellingen van bestaande resources.

## <a name="smart-detection-rule-configuration"></a>Configuratie van de regel voor slimme detectie

U kunt de volgende instellingen voor een regel voor slimme detectie configureren:
- Als de regel is ingeschakeld (de standaardwaarde is **waar**.)
- Als e-mailberichten naar gebruikers die zijn gekoppeld aan van het abonnement moeten worden verzonden [Monitoring Reader](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) en [bewaking Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rollen wanneer er een detectie is gevonden (de standaardwaarde is **waar**.)
- Eventuele aanvullende e-mailontvangers die een melding krijgen moeten wanneer er een detectie is gevonden.
    -  E-mailconfiguratie is niet beschikbaar voor regels voor slimme detectie gemarkeerd als _preview_.

Als u wilt toestaan dat de regelinstellingen via Azure Resource Manager configureren, de configuratie van de regel voor slimme detectie is nu beschikbaar als een binnenste bron binnen de Application Insights-resource met de naam **ProactiveDetectionConfigs**.
Elke regel voor slimme detectie kan worden geconfigureerd met unieke meldingsinstellingen, voor maximale flexibiliteit.

## 

## <a name="examples"></a>Voorbeelden

Hieronder vindt u enkele voorbeelden die laten zien hoe u configureert de instellingen van regels voor slimme detectie met behulp van Azure Resource Manager-sjablonen.
Alle voorbeelden verwijzen naar een Application Insights-resource met de naam _"myApplication"_ , en aan de 'afhankelijkheid lange duur van de regel voor slimme detectie", die intern heet _"longdependencyduration"_ .
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

### <a name="failure-anomalies-v2-non-classic-alert-rule"></a>Fout bij afwijkingen v2 (niet klassiek) waarschuwingsregel

Deze Azure Resource Manager-sjabloon ziet u een afwijkende fouten v2-waarschuwingsregel configureren met een prioriteit 2. Deze nieuwe versie van de waarschuwingsregel afwijkende fouten maakt deel uit van de nieuwe Azure platform waarschuwingen en vervangt de klassieke versie die wordt buiten gebruik gesteld als onderdeel van de [klassieke waarschuwingen buitengebruikstellingsproces](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "microsoft.alertsmanagement/smartdetectoralertrules",
            "apiVersion": "2019-03-01",
            "name": "Failure Anomalies - my-app",
            "properties": {
                  "description": "Detects a spike in the failure rate of requests or dependencies",
                  "state": "Enabled",
                  "severity": "2",
                  "frequency": "PT1M",
                  "detector": {
                  "id": "FailureAnomaliesDetector"
                  },
                  "scope": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/MyResourceGroup/providers/microsoft.insights/components/my-app"],
                  "actionGroups": {
                        "groupIds": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourcegroups/MyResourceGroup/providers/microsoft.insights/actiongroups/MyActionGroup"]
                  }
            }
        }
    ]
}
```

> [!NOTE]
> Deze Azure Resource Manager-sjabloon is uniek is voor de waarschuwingsregel voor afwijkende fouten-v2 en is anders dan de andere klassieke regels voor slimme detectie beschreven in dit artikel.   

## <a name="smart-detection-rule-names"></a>Namen van de regel voor slimme detectie

Hieronder ziet u een tabel met de namen van Slimme detectie zoals ze worden weergegeven in de portal, samen met hun interne namen, die moet worden gebruikt in de Azure Resource Manager-sjabloon.

> [!NOTE]
> Regels voor slimme detectie gemarkeerd als _preview_ bieden geen ondersteuning voor e-mailmeldingen. Daarom kunt u alleen instellen de _ingeschakeld_ eigenschap voor deze regels. 

| Azure portal regelnaam | Interne naam
|:---|:---|
| Langzame paginalaadtijd | slowpageloadtime |
| Trage reactietijd | slowserverresponsetime |
| Lange afhankelijkheidsduur | longdependencyduration |
| Vermindering in responstijd van de server | degradationinserverresponsetime |
| Degradatie van afhankelijkheidsduur | degradationindependencyduration |
| Degradatie traceren ernst breedteverhouding (preview) | extension_traceseveritydetector |
| Abnormale toename van uitzonderingen (preview) | extension_exceptionchangeextension |
| Mogelijk geheugenlek gedetecteerd (preview) | extension_memoryleakextension |
| Potentiële beveiligingsproblemen gedetecteerd (preview) | extension_securityextensionspackage |
| Ongebruikelijke stijging in het dagelijkse gegevensvolume (preview) | extension_billingdatavolumedailyspikeextension |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatisch detecteren:

- [Foutafwijkingen](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Geheugenlekken](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Prestatieafwijkingen](../../azure-monitor/app/proactive-performance-diagnostics.md)

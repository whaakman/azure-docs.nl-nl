---
title: Over het integreren van het algemene schema van de waarschuwing met Logic Apps
description: Informatie over het maken van een logische app die gebruikmaakt van het algemene waarschuwing schema voor het afhandelen van al uw waarschuwingen.
author: ananthradhakrishnan
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 13cb3880662e1665b03dd63f009645acbe97fc75
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734893"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Over het integreren van het algemene schema van de waarschuwing met Logic Apps

In dit artikel wordt beschreven hoe u een logische app die gebruikmaakt van het algemene waarschuwing schema voor het afhandelen van al uw waarschuwingen te maken.

## <a name="overview"></a>Overzicht

De [algemene waarschuwing schema](https://aka.ms/commonAlertSchemaDocs) biedt een gestandaardiseerde en uitbreidbare JSON-schema voor alle uw verschillende typen waarschuwingen. Het algemene schema van de waarschuwing is vooral nuttig zijn bij het programmatisch – via webhooks, runbooks en logic apps gebruikt. In dit artikel laten we zien hoe een enkele logische app voor het afhandelen van al uw waarschuwingen worden geschreven. Dezelfde principes kunnen worden toegepast op andere programmatische methoden. De logische app die wordt beschreven in dit artikel maakt u goed gedefinieerde variabelen voor de ['essentiële' velden](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields), en ook wordt beschreven hoe u kunt verwerken [type waarschuwing](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) specifieke logica.


## <a name="prerequisites"></a>Vereisten 

In dit artikel wordt ervan uitgegaan dat de lezer bekend bent met 
* Instellen van regels voor waarschuwingen ([metric](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [log](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [activiteitenlogboek](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Instellen van [actiegroepen](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* Inschakelen van de [algemene waarschuwing schema](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) uit binnen actiegroepen

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Een logische app gebruik van het algemene schema van de waarschuwing maken

1. Ga als volgt de [stappen uit om te maken van uw logische app](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app). 

1.  Selecteer de trigger: **Wanneer een HTTP-aanvraag wordt ontvangen**.

    ![Triggers voor logische app](media/action-groups-logic-app/logic-app-triggers.png "triggers voor logische app")

1.  Selecteer **bewerken** te wijzigen van de HTTP-aanvraag als trigger.

    ![HTTP-aanvraag triggers](media/action-groups-logic-app/http-request-trigger-shape.png "triggers voor HTTP-aanvragen")


1.  Kopieer en plak het volgende schema:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. Selecteer **+** **nieuwe stap** en kies vervolgens **een actie toevoegen**.

    ![Een actie toevoegen](media/action-groups-logic-app/add-action.png "een actie toevoegen")

1. In deze fase, kunt u verschillende connectors (Microsoft Teams, Slack, Salesforce, enzovoort) op basis van uw specifieke zakelijke vereisten toevoegen. U kunt de 'essentiële velden' out-of-the-box. 

    ![Essential fields](media/alerts-common-schema-integrations/logic-app-essential-fields.png "Essential fields")
    
    U kunt ook kunt u voorwaardelijke logica op basis van het type waarschuwing met de optie 'Expression' maken.

    ![Logische app expressie](media/alerts-common-schema-integrations/logic-app-expressions.png "Logic app-expressie")
    
     De ['monitoringService' veld](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) kunt u voor het aanduiden van het type waarschuwing op basis van dat u de voorwaardelijke logica kunt maken.

    
    Bijvoorbeeld, het onderstaande codefragment controleert als de waarschuwing waarschuwing voor een Application Insights op basis van is en als dit het geval is de lijst met zoekresultaten afgedrukt. Else, it prints 'NA'.

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     Meer informatie over [expressies logic app](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions).

    


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over actiegroepen](../../azure-monitor/platform/action-groups.md).
* [Meer informatie over de algemene waarschuwing schema](https://aka.ms/commonAlertSchemaDocs).


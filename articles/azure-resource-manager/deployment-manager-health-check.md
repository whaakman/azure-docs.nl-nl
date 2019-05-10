---
title: Introduceren de gezondheid van integratie van implementatie naar Azure Deployment Manager
description: Beschrijft hoe u een service implementeren via een groot aantal regio's met Azure Deployment Manager. Hier ziet u veilige implementatiemethoden om te controleren of de stabiliteit van uw implementatie te bevestigen voordat het uitrollen van naar alle regio's.
services: azure-resource-manager
documentationcenter: na
author: mumian
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/19
ms.author: jgao
ms.openlocfilehash: 1006cc902cf4b6f763d86165a039a1fec5dc97a1
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467086"
---
# <a name="introduce-health-integration-rollout-to-azure-deployment-manager-public-preview"></a>Introduceren de gezondheid van integratie van implementatie naar Azure Deployment Manager (preview-versie)

[Azure Deployment Manager](./deployment-manager-overview.md) Hiermee kunt u gefaseerde implementaties van Azure Resource Manager-resources. De resources worden regio door regio geïmplementeerd op een geordende wijze. De geïntegreerde statuscontrole van Azure Deployment Manager kunt bewaken implementaties en automatisch stop problematische implementaties, zodat u kunt oplossen en de schaal van de impact te verminderen. Deze functie kan service niet beschikbaar zijn veroorzaakt door een regressie in de updates te verminderen.

## <a name="health-monitoring-providers"></a>Providers van statusbewaking

Als u wilt de gezondheid van integratie zo eenvoudig mogelijk te maken, is Microsoft gewerkt met enkele van de belangrijkste service voor health monitoring bedrijven om te voorzien van een oplossing voor eenvoudige kopiëren/plakken statuscontroles integreren in uw implementaties. Als u een health monitor nog niet is gebruikt, zijn deze fantastische oplossingen te beginnen met:

| ![Azure-implementatie manager health monitor-provider datadog](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-datadog.svg) | ![Azure-implementatie manager health monitor-provider site24x7](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-site24x7.svg) | ![Azure-implementatie manager health monitor-provider wavefront](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-wavefront.svg) |
|-----|------|------|
|Datadog, de toonaangevende monitoring en analytics platform voor moderne cloudomgevingen. Zie [hoe Datadog kan worden geïntegreerd met Azure Deployment Manager](https://www.datadoghq.com/azure-deployment-manager/).|Site24x7, de alles-in-één privé en publieke cloud services-oplossing voor controle. Zie [hoe Site24x7 kan worden geïntegreerd met Azure Deployment Manager](https://www.site24x7.com/azure/adm.html).| Wavefront, de monitoring en analytics platform voor omgevingen met meerdere cloud-toepassingen. Zie [hoe Wavefront kan worden geïntegreerd met Azure Deployment Manager](https://go.wavefront.com/wavefront-adm/).|

## <a name="how-service-health-is-determined"></a>Hoe de status van de service wordt bepaald

[Voor Health monitoring providers](#health-monitoring-providers) bieden verschillende mechanismen voor het bewaken van services en wordt u gewaarschuwd van eventuele problemen met de service controleren. [Azure Monitor](/services/monitor/) volgt een voorbeeld van een dergelijke aanbieding. Azure Monitor kan worden gebruikt om waarschuwingen te maken wanneer bepaalde drempelwaarden worden overschreden. Bijvoorbeeld, oploopt het geheugen en CPU-gebruik buiten het verwachte niveau wanneer u een nieuwe update op uw service implementeert. Wanneer wordt weergegeven, kunt u corrigerende maatregelen kunt nemen.

Deze health-providers bieden doorgaans REST-API's zodat de status van de monitors van uw service programmatisch kan worden onderzocht. De REST API's kunt ofwel overstappen met een eenvoudige in orde/slecht signaal (bepaald door de HTTP-responscode) en/of gedetailleerde informatie over de signalen die het ontvangt.

De nieuwe *statuscontrole* stap in Azure Deployment Manager kunt u HTTP-codes die wijzen op een gezonde service declareren of, voor complexere REST-resultaten, kunt u ook reguliere expressies die, als ze overeenkomen, wijzen op een gezonde opgeven het antwoord.

De stroom voor het uitvoeren van setup met statuscontroles van Azure Deployment Manager:

1. Maak uw statusmonitors via een provider van de health-service van uw keuze.
1. Een of meer statuscontrole stappen maken als onderdeel van uw implementatie van Azure Deployment Manager. Vul de statuscontrole stappen met de volgende informatie:

    1. De URI voor de REST-API voor de status van uw bewaakt (zoals gedefinieerd door de health-serviceprovider).
    1. Verificatie-informatie. Op dit moment alleen API-sleutel-verificatie wordt ondersteund.
    1. [HTTP-statuscodes](https://www.wikipedia.org/wiki/List_of_HTTP_status_codes) of reguliere expressies die in orde antwoord definiëren. Houd er rekening mee dat ook die reguliere expressies leveren kunt, die alle moet overeenkomt met het antwoord om te worden beschouwd als in orde is, of u expressies die overeenkomende moet voor de reacties bieden kan op in orde beschouwd. Beide methoden worden ondersteund.

    De volgende Json wordt een voorbeeld:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

1. De statuscontrole stappen worden aangeroepen op het juiste moment bij uw Azure Deployment Manager-implementatie. In het volgende voorbeeld wordt een stap status controleren wordt aangeroepen in de **postDeploymentSteps** van **stepGroup2**.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

Als u wilt nemen een voorbeeld, Zie [zelfstudie: Controle van gatewayservicestatus gebruiken in Azure Deployment Manager](./deployment-manager-health-check.md).

## <a name="phases-of-a-health-check"></a>Fasen van de statuscontrole

Op dit moment weet Azure Deployment Manager hoe u query's voor de status van uw service en wat fasen bij uw implementatie om dit te doen. Azure Deployment Manager kan echter ook voor een grondige configuratie van de timing van deze controles. Een stap statuscontrole wordt uitgevoerd in 3 opeenvolgende fasen, die allemaal kunnen worden geconfigureerd duur zijn: 

1. Wachten

    1. Nadat de bewerking voor een implementatie is voltooid, virtuele machines kunnen worden opgestart, opnieuw configureren op basis van nieuwe gegevens, of zelfs voor de eerste keer wordt gestart. Het duurt ook tijd voor services om te beginnen met het verzenden van health-signalen en worden samengevoegd door de provider voor statuscontrole in iets nuttig. Tijdens dit proces tumultuous, kan het niet verstandig om te controleren voor de status van de service nadat de update nog niet een stabiele status heeft bereikt. De service kan immers worden vibrerende tussen de statussen in orde is en niet in orde als de resources vereffenen. 
    1. Status van de service wordt niet gecontroleerd tijdens de fase wachten. Dit wordt gebruikt om te worden gebruikt om de geïmplementeerde resources de tijd voor het maken van voordat u begint met de health-proces. 
1. Elastisch

    1. Omdat het niet mogelijk om te weten in alle gevallen hoelang nodig resources is om te maken voordat ze een stabiel, de elastische fase kunt u een flexibele tijd tussen wanneer de resources mogelijk instabiel zijn en wanneer ze vereist voor het onderhouden van een stabiele in orde zijn status.
    1. Wanneer de elastische fase begint met, begint Azure Deployment Manager periodiek polling van de opgegeven REST-eindpunt voor de servicestatus. Het polling-interval kan worden geconfigureerd. 
    1. Als de health monitor weer wordt geleverd met signalen die aangeeft dat de service niet in orde is, wordt deze signalen worden genegeerd, blijft de elastische fase en polling blijft. 
    1. Zodra de health monitor weer met signalen die aangeeft of de service in orde is, de elastische fase eindigt en de fase HealthyState begint. 
    1. De duur van de opgegeven voor de elastische fase is dus de maximale hoeveelheid tijd die kan worden besteed polling voor servicestatus voordat een antwoord in orde wordt beschouwd als verplicht. 
1. HealthyState

    1. Status van de service is tijdens de fase HealthyState voortdurend gepeild met hetzelfde interval als de elastische fase. 
    1. De service wordt verwacht in orde signalen van de health monitoring-provider voor de gehele duur van de opgegeven onderhouden. 
    1. Als er op elk gewenst moment een reactie niet in orde wordt gedetecteerd, wordt Azure Deployment Manager de volledige implementatie stoppen en de uitvoering van de signalen beschadigde service REST-antwoord retourneren.
    1. Zodra de duur van de HealthyState is afgelopen, de statuscontrole is voltooid en implementatie blijft met de volgende stap.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het integreren van statuscontrole in Azure Deployment Manager. Doorgaan met het volgende artikel voor meer informatie over het implementeren van met Deployment Manager.

> [!div class="nextstepaction"]
> [Zelfstudie: statuscontrole in Azure Deployment Manager integreren](./deployment-manager-tutorial-health-check.md)
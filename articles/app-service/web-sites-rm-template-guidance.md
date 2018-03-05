---
title: Hulp bij het implementeren van Azure-web-apps met sjablonen | Microsoft Docs
description: Aanbevelingen voor het maken van Azure Resource Manager-sjablonen voor het implementeren van web-apps.
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: tomfitz
ms.openlocfilehash: eff0b0e6258217fa8fbf7f15606f98d70fecd7c5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>Hulp bij het implementeren van web-apps met Azure Resource Manager-sjablonen

Dit artikel bevat aanbevelingen voor het maken van Azure Resource Manager-sjablonen voor het implementeren van App Service-oplossingen. Deze aanbevelingen kunt u veelvoorkomende problemen voorkomen.

## <a name="define-dependencies"></a>Afhankelijkheden definiëren

Afhankelijkheden definiëren voor Web-Apps vereist een goed begrip van de wisselwerking tussen de bronnen in een Web-App. Als u afhankelijkheden in een verkeerde volgorde opgeeft, mag u implementatiefouten veroorzaken of maak een zeldzame situatie die de implementatie blijft hangen.

> [!WARNING]
> Als u een site-uitbreiding van MS Deploy in de sjabloon opneemt, moet u alle configuratie-resources instellen als afhankelijk van de bron MS Deploy. Wijzigingen in de configuratie ervoor zorgen dat de site asynchroon opnieuw te starten. Maken van de configuratie-resources afhankelijk zijn van MS Deploy, zorgt u ervoor dat MS Deploy is voltooid voordat de site opnieuw wordt opgestart. Zonder deze afhankelijkheden, de site mogelijk opnieuw wordt opgestart tijdens het implementatieproces van MS Deploy. Zie voor een voorbeeldsjabloon [Wordpress-sjabloon met Web implementeren afhankelijkheid](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

De volgende afbeelding toont de volgorde van de afhankelijkheid voor verschillende App Service-bronnen.

![Web-app-afhankelijkheden](media/web-sites-rm-template-guidance/web-dependencies.png)

Implementeren van resources in de volgende volgorde:

**Tier 1**
* App Service-plan
* Andere verwante resources, zoals databases of storage-accounts

**Tier 2**
* Web-app - app service-abonnement is afhankelijk van
* Application Insights die zijn gericht op de server-farm -, is afhankelijk van app service-abonnement

**Tier 3**
* Besturingselement voor de gegevensbron - web-app is afhankelijk van
* Site-uitbreiding MSDeploy -, is afhankelijk van web-app
* Application Insights die zijn gericht op de server-farm -, is afhankelijk van web-app

**Tier 4**
* App Service-certificaat - afhankelijk van broncodebeheer of MSDeploy als een aanwezig; anders is afhankelijk van web-app
* Configuratie-instellingen (verbindingsreeksen, web configuratiewaarden, app-instellingen) - afhankelijk van broncodebeheer of MSDeploy als een aanwezig; anders is afhankelijk van web-app

**Tier 5**
* Naam bindingen host - afhankelijk is van het certificaat, indien aanwezig; anders wordt een bron op een hoger niveau
* Site-uitbreidingen - is afhankelijk van configuratie-instellingen, indien aanwezig; anders wordt een bron op een hoger niveau

Uw oplossing bevat doorgaans slechts een deel van deze bronnen en lagen. Voor ontbrekende lagen lagere resources worden toegewezen aan de volgende hogere laag.

Het volgende voorbeeld ziet deel van een sjabloon. Configuratie van de waarde voor de verbinding is afhankelijk van de extensie MSDeploy. De extensie MSDeploy, is afhankelijk van de web-app en database.

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="find-information-about-msdeploy-errors"></a>Voor meer informatie over MSDeploy-fouten

Als MSDeploy maakt gebruik van Resource Manager-sjabloon, zijn de implementatie-foutberichten moeilijk te begrijpen. Voor meer informatie na een mislukte implementatie, probeert u de volgende stappen uit:

1. Ga naar de site [Kudu-console](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Navigeer naar de map op `D:\home\LogFiles\SiteExtensions\MSDeploy`.
3. Zoek naar de *appManagerStatus.xml* en *appManagerLog.xml* bestanden. Het eerste bestand registreert de status. Het tweede bestand registreert informatie over de fout. Als de fout niet duidelijk is, kunt u het opnemen wanneer vragen voor meer informatie over het forum.

## <a name="unique-web-app-name"></a>Unieke web-appnaam

De naam voor uw web-app moet wereldwijd uniek zijn. U kunt een naamgevingsconventie die uniek zijn, of kunt u de [uniqueString functie](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) om te helpen bij het genereren van een unieke naam op.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor een zelfstudie over het implementeren van web-apps met een sjabloon [inrichten en implementeren van microservices zoals verwacht in Azure](app-service-deploy-complex-application-predictably.md).
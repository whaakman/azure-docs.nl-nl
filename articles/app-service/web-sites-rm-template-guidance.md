---
title: Hulp bij het implementeren van Azure-web-apps met behulp van sjablonen | Microsoft Docs
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
ms.openlocfilehash: 8c29cf5a65e9587b281a6000b5b4eff47f11da91
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807319"
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Hulp bij het implementeren van web-apps met behulp van Azure Resource Manager-sjablonen

Dit artikel bevat aanbevelingen voor het maken van Azure Resource Manager-sjablonen voor het implementeren van Azure App Service-oplossingen. Deze aanbevelingen kunt u veelvoorkomende problemen voorkomen.

## <a name="define-dependencies"></a>Afhankelijkheden definiëren

Afhankelijkheden voor web-apps definiëren vereist een goed begrip van de wisselwerking tussen de bronnen in een web-app. Als u afhankelijkheden in een verkeerde volgorde opgeeft, kunt u implementatiefouten veroorzaken of maak een zeldzame situatie die de implementatie blijft hangen.

> [!WARNING]
> Als u een MSDeploy-site-uitbreiding in de sjabloon opneemt, moet u alle configuratie-resources instellen als afhankelijk van de bron MSDeploy. Wijzigingen in de configuratie ervoor zorgen dat de site asynchroon opnieuw te starten. Maken van de configuratie-resources afhankelijk zijn van MSDeploy, zorgt u ervoor dat MSDeploy is voltooid voordat de site opnieuw wordt opgestart. Zonder deze afhankelijkheden, de site mogelijk opnieuw wordt opgestart tijdens de implementatie van MSDeploy. Zie voor een voorbeeldsjabloon [WordPress-sjabloon met Web implementeren afhankelijkheid](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

De volgende afbeelding toont de volgorde van de afhankelijkheid voor verschillende App Service-bronnen:

![Web-app-afhankelijkheden](media/web-sites-rm-template-guidance/web-dependencies.png)

Implementeren van resources in de volgende volgorde:

**Laag 1**
* App Service-abonnement.
* Alle andere verwante resources, zoals databases of storage-accounts.

**Tier 2**
* Web app--is afhankelijk van de App Service-abonnement.
* Azure Application Insights-instantie die gericht is op de server-farm is afhankelijk van de App Service-abonnement.

**Tier 3**
* Besturingselement--voor de gegevensbron is afhankelijk van de web-app.
* Site-uitbreiding MSDeploy--is afhankelijk van de web-app.
* Application Insights-exemplaar dat gericht is op de server-farm is afhankelijk van de web-app.

**Tier 4**
* App Service-certificaat is afhankelijk van broncodebeheer of MSDeploy als beide aanwezig is. Anders wordt deze afhankelijk is van de web-app.
* Configuratie-instellingen (verbindingsreeksen, web.config-waarden, app-instellingen)--hangt af van broncodebeheer of MSDeploy als een aanwezig is. Anders wordt deze afhankelijk is van de web-app.

**Tier 5**
* Naam bindingen--host is afhankelijk van het certificaat, indien aanwezig. Anders wordt deze afhankelijk is van een bron op een hoger niveau.
* Site-uitbreidingen--is afhankelijk van configuratie-instellingen, indien aanwezig. Anders wordt deze afhankelijk is van een bron op een hoger niveau.

Uw oplossing bevat doorgaans slechts een deel van deze bronnen en lagen. Voor ontbrekende lagen lagere resources worden toegewezen aan de eerstvolgende hogere laag.

Het volgende voorbeeld ziet deel van een sjabloon. De waarde van de configuratie van de verbinding-tekenreeks is afhankelijk van de extensie MSDeploy. De extensie MSDeploy, is afhankelijk van de web-app en database. 

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Zie voor een voorbeeld van kant-en-klaar die gebruikmaakt van de bovenstaande code [sjabloon: een eenvoudige Umbraco Web-App bouwen](https://github.com/Azure/azure-quickstart-templates/tree/master/umbraco-webapp-simple).

## <a name="find-information-about-msdeploy-errors"></a>Voor meer informatie over MSDeploy-fouten

Als MSDeploy maakt gebruik van Resource Manager-sjabloon, zijn de implementatie-foutberichten moeilijk te begrijpen. Voor meer informatie na een mislukte implementatie, probeert u de volgende stappen uit:

1. Ga naar de site [Kudu-console](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Blader naar de map in D:\home\LogFiles\SiteExtensions\MSDeploy.
3. Zoek naar de appManagerStatus.xml en appManagerLog.xml-bestanden. Het eerste bestand registreert de status. Het tweede bestand registreert informatie over de fout. Als de fout niet duidelijk is, kunt u het opnemen wanneer u voor meer informatie over het forum vraagt.

## <a name="choose-a-unique-web-app-name"></a>Kies een unieke web-app-naam

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
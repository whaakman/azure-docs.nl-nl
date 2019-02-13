---
title: Hulp bij het implementeren van apps met sjablonen - Azure App Service | Microsoft Docs
description: Aanbevelingen voor het maken van Azure Resource Manager-sjablonen voor de implementatie van web-apps.
services: app-service
documentationcenter: app-service
author: tfitzmac
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 780d2134aa00f828a614af6938978e24df3534cd
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105108"
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Hulp bij het implementeren van web-apps met behulp van Azure Resource Manager-sjablonen

Dit artikel bevat aanbevelingen voor het maken van Azure Resource Manager-sjablonen voor de implementatie van Azure App Service-oplossingen. Deze aanbevelingen kunt u voorkomen dat bekende problemen.

## <a name="define-dependencies"></a>Afhankelijkheden definiëren

Afhankelijkheden voor web-apps definiëren, vereist een goed begrip van de interactie van de resources in een web-app. Als u afhankelijkheden in een verkeerde volgorde opgeeft, kunt u leiden tot fouten bij de implementatie of maak een zeldzame situatie die de implementatie blijft hangen.

> [!WARNING]
> Als u een MSDeploy-site-uitbreiding in uw sjabloon opneemt, moet u alle configuratieresources instellen als afhankelijk van de MSDeploy-resource. Wijzigingen in de configuratie ervoor zorgen dat de site opnieuw te starten asynchroon. Door het maken van de van configuratieresources afhankelijk zijn van MSDeploy, zorgt u ervoor dat MSDeploy is voltooid voordat de site opnieuw wordt opgestart. Zonder deze afhankelijkheden, kan de site opnieuw starten tijdens het implementatieproces van MSDeploy. Zie voor een voorbeeldsjabloon [WordPress-sjabloon met Web implementeren afhankelijkheid](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

De volgende afbeelding toont de volgorde van de afhankelijkheid voor verschillende App Service-resources:

![Web-app-afhankelijkheden](media/web-sites-rm-template-guidance/web-dependencies.png)

Implementeren van resources in de volgende volgorde:

**Tier 1**
* App Service-plan.
* Alle andere gerelateerde resources, zoals databases of storage-accounts.

**Tier 2**
* Web app, is afhankelijk van de App Service-plan.
* Azure Application Insights-instantie die gericht is op de server-farm, is afhankelijk van de App Service-plan.

**Tier 3**
* Broncodebeheer, is afhankelijk van de web-app.
* MSDeploy-site-extensie, is afhankelijk van de web-app.
* Application Insights-exemplaar dat is gericht op de server-farm, is afhankelijk van de web-app.

**Tier 4**
* App Service-certificaat is afhankelijk van broncodebeheer of MSDeploy als een van beide aanwezig is. Anders wordt deze afhankelijk is van de web-app.
* Configuratie-instellingen (verbindingsreeksen, web.config waarden, app-instellingen), is afhankelijk van broncodebeheer of MSDeploy als beide aanwezig is. Anders wordt deze afhankelijk is van de web-app.

**Tier 5**
* Naam bindingen--host is afhankelijk van het certificaat, indien aanwezig. Anders wordt deze afhankelijk is van een hoger niveau resource.
* Site-extensies--is afhankelijk van configuratie-instellingen, indien aanwezig. Anders wordt deze afhankelijk is van een hoger niveau resource.

Uw oplossing bevat doorgaans slechts een deel van deze resources en lagen. Voor de ontbrekende lagen lagere resources worden toegewezen aan de eerstvolgende hogere laag.

In het volgende voorbeeld ziet u deel van een sjabloon. De waarde van de configuratie van de tekenreeks verbinding, is afhankelijk van de MSDeploy-extensie. De extensie MSDeploy, is afhankelijk van de web-app en database. 

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

Zie voor een kant-en-klaar-voorbeeld dat gebruikmaakt van de bovenstaande code, [sjabloon: Een eenvoudige Umbraco-Web-App bouwen](https://github.com/Azure/azure-quickstart-templates/tree/master/umbraco-webapp-simple).

## <a name="find-information-about-msdeploy-errors"></a>Meer informatie over MSDeploy-fouten

Als uw Resource Manager-sjabloon maakt gebruik van MSDeploy, kunnen de implementatie-foutberichten moeilijk te begrijpen zijn. Voor meer informatie na een mislukte implementatie, probeert u de volgende stappen uit:

1. Ga naar de site [Kudu-console](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Blader naar de map in D:\home\LogFiles\SiteExtensions\MSDeploy.
3. Zoek naar de appManagerStatus.xml en appManagerLog.xml-bestanden. Het eerste bestand registreert de status. De tweede bestand registreert informatie over de fout. Als de fout niet duidelijk is, kunt u het opnemen wanneer u om hulp op het forum vraagt.

## <a name="choose-a-unique-web-app-name"></a>Kies een unieke web-app-naam

De naam voor uw web-app moet globaal uniek zijn. U kunt een naamgevingsconventie die waarschijnlijk uniek te zijn, of kunt u de [uniqueString functie](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) om te helpen bij het genereren van een unieke naam.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="deploy-web-app-certificate-from-key-vault"></a>Web-app-certificaat uit Key Vault implementeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als de sjabloon bevat een [Microsoft.Web/certificates](/azure/templates/microsoft.web/certificates) resource voor SSL-binding en het certificaat dat is opgeslagen in een Key Vault, moet u ervoor zorgen dat de App Service-identiteit kan toegang tot het certificaat.

In de globale Azure heeft de App Service-service-principal-ID van **abfa0a7c-a6b6-4736-8310-5855508787cd**. Om toegang te verlenen tot Key Vault voor de App service-principal, gebruikt u:

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
  -VaultName KEY_VAULT_NAME `
  -ServicePrincipalName abfa0a7c-a6b6-4736-8310-5855508787cd `
  -PermissionsToSecrets get `
  -PermissionsToCertificates get
```

In Azure Government, heeft de App Service-service-principal-ID van **6a02c803-dafd-4136-b4c3-5a6f318b4714**. Gebruik deze ID in het voorgaande voorbeeld.

Selecteer in uw Key Vault, **certificaten** en **genereren/importeren** om het certificaat te uploaden.

![Certificaat importeren](media/web-sites-rm-template-guidance/import-certificate.png)

In de sjabloon, geef de naam van het certificaat voor de `keyVaultSecretName`.

Zie voor een voorbeeldsjabloon [implementeert u een Web-App-certificaat uit Key Vault-geheim en deze gebruiken voor het maken van SSL-binding](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een zelfstudie over het implementeren van web-apps met een sjabloon, [inrichten en implementeren van microservices zoals verwacht in Azure](deploy-complex-application-predictably.md).
* Zie voor meer informatie over de JSON-syntaxis en eigenschappen voor het woord brontypen in sjablonen, [verwijzing naar de Azure Resource Manager-sjabloon](/azure/templates/).

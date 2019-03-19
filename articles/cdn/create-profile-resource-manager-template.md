---
title: 'Snelstart: een Azure Content Delivery Network-profiel en een eindpunt met behulp van Resource Manager-sjablonen maken | Microsoft Docs'
description: Informatie over het maken van een Azure Content Delivery Network-profiel en een eindpunt met behulp van Resource Manager-sjablonen
services: cdn
documentationcenter: ''
author: senthuransivananthan
manager: danielgi
editor: ''
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 5c393916bc977f4e0bc51913bdb2dfbbd6677c97
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57727582"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint-using-resource-manager-template"></a>Quickstart: Een Azure CDN-profiel en een eindpunt met behulp van Resource Manager-sjabloon maken

In deze quickstart implementeert u een Azure Resource Manager-sjabloon met behulp van CLI. De sjabloon die u implementeert een CDN-profiel en een CDN-eindpunt voor de front-end van uw webtoepassing.
Het moet ongeveer tien minuten duren voordat deze stappen uitvoeren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prequisites"></a>Prequisites

Voor de toepassing van deze Quick Start, moet u een Web-App om te gebruiken als de oorsprong hebben. De voorbeeld-webtoepassing die wordt gebruikt in deze Quick Start is geïmplementeerd op https://cdndemo.azurewebsites.net

Zie voor meer informatie, [een statische HTML-web-app maken in Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-html).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Alle resources moeten worden geïmplementeerd in dezelfde resourcegroep bevinden.

Maak de resourcegroep op de locatie die u selecteert. In dit voorbeeld toont het maken van een resourcegroep met de naam cdn in de locatie VS-Oost.

```bash
az group create --name cdn --location eastus
```

![Nieuwe resourcegroep](./media/create-profile-resource-manager-template/cdn-create-resource-group.png)

## <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken

In deze stap maakt u een sjabloon voor bestanden die worden geïmplementeerd.

In dit voorbeeld begeleidt bij een scenario voor het algemene Website versnelling, maar er zijn veel andere instellingen die kunnen worden geconfigureerd. Deze instellingen zijn beschikbaar in een verwijzing naar de Azure Resource Manager-sjabloon. Zie Naslaginformatie voor [CDN-profiel](https://docs.microsoft.com/en-us/azure/templates/microsoft.cdn/2017-10-12/profiles) en [CDN-profiel-eindpunt](https://docs.microsoft.com/en-us/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints).

Houd er rekening mee dat Microsoft CDN biedt geen ondersteuning voor de lijst met inhoudstypen wijzigen.

De sjabloon opslaan als **resource-manager-cdn.json**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "cdnProfileSku": {
            "type": "string",
            "allowedValues": [
                "Standard_Microsoft",
                "Standard_Akamai",
                "Standard_Verizon",
                "Premium_Verizon"
            ]
        },
        "endpointOriginHostName": {
            "type": "string"
        }
    },
    "variables": {
        "profile": {
            "name": "[replace(toLower(parameters('cdnProfileSku')), '_', '-')]"
        },
        "endpoint": {
            "name": "[replace(toLower(parameters('endpointOriginHostName')), '.', '-')]",
            "originHostName": "[parameters('endpointOriginHostName')]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Cdn/profiles",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[variables('profile').name]",
            "sku": {
                "name": "[parameters('cdnProfileSku')]"
            }
        },
        {
            "dependsOn": [
                "[resourceId('Microsoft.Cdn/profiles', variables('profile').name)]"
            ],
            "type": "Microsoft.Cdn/profiles/endpoints",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[concat(variables('profile').name, '/', variables('endpoint').name)]",
            "properties": {
                "hostName": "[concat(variables('endpoint').name, '.azureedge.net')]",
                "originHostHeader": "[variables('endpoint').originHostName]",
                "isHttpAllowed": true,
                "isHttpsAllowed": true,
                "queryStringCachingBehavior": "IgnoreQueryString",
                "origins": [
                    {
                        "name": "[replace(variables('endpoint').originHostName, '.', '-')]",
                        "properties": {
                            "hostName": "[variables('endpoint').originHostName]",
                            "httpPort": 80,
                            "httpsPort": 443
                        }
                    }
                ],
                "contentTypesToCompress": [
                    "application/eot",
                    "application/font",
                    "application/font-sfnt",
                    "application/javascript",
                    "application/json",
                    "application/opentype",
                    "application/otf",
                    "application/pkcs7-mime",
                    "application/truetype",
                    "application/ttf",
                    "application/vnd.ms-fontobject",
                    "application/xhtml+xml",
                    "application/xml",
                    "application/xml+rss",
                    "application/x-font-opentype",
                    "application/x-font-truetype",
                    "application/x-font-ttf",
                    "application/x-httpd-cgi",
                    "application/x-javascript",
                    "application/x-mpegurl",
                    "application/x-opentype",
                    "application/x-otf",
                    "application/x-perl",
                    "application/x-ttf",
                    "font/eot",
                    "font/ttf",
                    "font/otf",
                    "font/opentype",
                    "image/svg+xml",
                    "text/css",
                    "text/csv",
                    "text/html",
                    "text/javascript",
                    "text/js",
                    "text/plain",
                    "text/richtext",
                    "text/tab-separated-values",
                    "text/xml",
                    "text/x-script",
                    "text/x-component",
                    "text/x-java-source"
                ],
                "isCompressionEnabled": true,
                "optimizationType": "GeneralWebDelivery"
            }
        }
    ],
    "outputs": {
        "cdnUrl": {
            "type": "string",
            "value": "[concat('https://', variables('endpoint').name, '.azureedge.net')]"
        }
    }
}
```

## <a name="create-the-resources"></a>De resources maken

Implementeer de sjabloon met behulp van Azure CLI. U wordt gevraagd voor 2-invoer:

**cdnProfileSku** -de CDN-provider die u wilt gebruiken. De opties zijn:

* Standard_Microsoft
* Standard_Akamai
* Standard_Verizon
* Premium_Verizon.

**endpointOriginHostName** -het eindpunt dat moet worden verzonden via het CDN, bijvoorbeeld cdndemo.azurewebsites.net.

```bash
az group deployment create --resource-group cdn --template-file arm-cdn.json
```

![Resource Manager-sjabloon implementeren](./media/create-profile-resource-manager-template/cdn-deploy-resource-manager.png)

## <a name="view-the-cdn-profile"></a>Het CDN-profiel weergeven

```bash
az cdn profile list --resource-group cdn -o table
```

![CDN-profiel weergeven](./media/create-profile-resource-manager-template/cdn-view-profile.png)

## <a name="view-the-cdn-endpoint-for-the-profile-standard-microsoft"></a>Het CDN-eindpunt voor de standard-microsoft-profiel weergeven

```bash
az cdn endpoint list --profile-name standard-microsoft --resource-group cdn -o table
```

![CDN-eindpunt weergeven](./media/create-profile-resource-manager-template/cdn-view-endpoint.png)

De hostnaam gebruiken om de inhoud weer te geven. Bijvoorbeeld toegang tot https://cdndemo-azurewebsites-net.azureedge.net met uw browser.

## <a name="clean-up"></a>Opruimen

Als u de resourcegroep verwijdert, worden alle resources die zijn geïmplementeerd automatisch in het verwijderd.

```bash
az group delete --name cdn
```

![Resourcegroep verwijderen](./media/create-profile-resource-manager-template/cdn-delete-resource-group.png)

## <a name="references"></a>Verwijzingen

* CDN-profiel - [naslaginformatie over Azure Resource Manager-sjabloon](https://docs.microsoft.com/en-us/azure/templates/microsoft.cdn/2017-10-12/profiles)
* CDN-eindpunt - [sjabloonverwijzing voor Azure Resource Manager-documentatie](https://docs.microsoft.com/en-us/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toevoegen van een aangepast domein aan uw CDN-eindpunt, de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Zelfstudie: Een aangepast domein toevoegen aan uw Azure CDN-eindpunt](cdn-map-content-to-custom-domain.md)

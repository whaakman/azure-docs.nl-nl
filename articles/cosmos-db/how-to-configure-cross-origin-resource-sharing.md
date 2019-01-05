---
title: Cross-Origin Resource Sharing (CORS) in Azure Cosmos DB
description: In dit artikel wordt beschreven hoe u Cross-Origin Resource Sharing (CORS) configureren in Azure Cosmos DB met behulp van Azure portal en Azure Resource Manager-sjablonen.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2018
ms.author: dech
ms.openlocfilehash: 08cd8b332a34075d8e2f939aa9015aa1b0a33a07
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036151"
---
# <a name="cross-origin-resource-sharing-cors-in-azure-cosmos-db"></a>Cross-Origin Resource Sharing (CORS) in Azure Cosmos DB 

Cross-Origin Resource Sharing (CORS) is een HTTP-functie waarmee een webtoepassing die wordt uitgevoerd in een bepaald domein te krijgen tot bronnen in een ander domein. Webbrowsers implementeren een beveiligingsbeperking bekend als het beleid voor zelfde oorsprong die voorkomt een webpagina van aanroepen van API's in een ander domein dat. CORS biedt echter een veilige manier om toe te staan van het oorspronkelijke domein voor het aanroepen van API's in een ander domein. Core SQL-API in Azure Cosmos DB biedt nu ondersteuning voor Cross-Origin Resource Sharing (CORS) met behulp van de header 'allowedOrigins'. Nadat u de CORS-ondersteuning voor uw Azure Cosmos-account inschakelen, worden alleen geverifieerde aanvragen geëvalueerd om te bepalen of ze zijn toegestaan volgens de regels die u hebt opgegeven.

U kunt de Cross-origin resource sharing (CORS)-instelling van de Azure-portal of van een Azure Resource Manager-sjabloon configureren. Core SQL-API in Azure Cosmos DB biedt ondersteuning voor een JavaScript-bibliotheek die werkt in Node.js en browser gebaseerde omgevingen. Deze bibliotheek kan nu profiteren van de CORS-ondersteuning als u de Gateway gebruikt. Er is geen client-side-configuratie die nodig zijn om deze functie te gebruiken. Met CORS-ondersteuning, resources vanuit een browser rechtstreeks toegang tot Azure Cosmos DB via de [JavaScript-bibliotheek](https://www.npmjs.com/package/@azure/cosmos) of rechtstreeks vanuit de [REST-API](https://docs.microsoft.com/rest/api/cosmos-db/) voor eenvoudige bewerkingen. 

## <a name="enable-cors-support-from-azure-portal"></a>CORS-ondersteuning van Azure-portal inschakelen

Gebruik de volgende stappen uit om in te schakelen Cross-Origin-Resource delen met behulp van Azure portal:

1. Navigeer naar uw Azure cosmos DB-account. Open de **CORS** blade.

2. Geef een door komma's gescheiden lijst met oorsprongen die cross-origin-aanroepen naar uw Azure Cosmos DB-account kunt maken. Bijvoorbeeld, `https://www.mydomain.com`, `https://mydomain.com`, `https://api.mydomain.com`. U kunt ook een jokerteken gebruiken '\*' op het toestaan van alle oorsprongen en selecteer **indienen**. 

   > [!NOTE]
   > U kunt jokertekens op dit moment niet gebruiken als onderdeel van de domeinnaam. Bijvoorbeeld `https://*.mydomain.net` indeling wordt nog niet ondersteund. 
   
   ![Cross origin-resources delen met behulp van Azure portal inschakelen](./media/how-to-configure-cross-origin-resource-sharing/enable-cross-origin-resource-sharing-using-azure-portal.png)
 
## <a name="enable-cors-support-from-resource-manager-template"></a>Inschakelen van CORS-ondersteuning van Resource Manager-sjabloon

Toevoegen om in te schakelen CORS met behulp van Resource Manager-sjabloon, de sectie "cors" met "allowedOrigins" eigenschap aan een bestaande sjabloon. De volgende JSON is een voorbeeld van een sjabloon die een nieuwe Azure Cosmos-account met CORS ingeschakeld maakt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "name": "test",
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "apiVersion": "2015-04-08",
            "location": "East US 2",
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "Session",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "test-eastus2",
                        "failoverPriority": 0,
                        "locationName": "East US 2"
                    }
                ],
                "cors": [
                    {
                        "allowedOrigins": "*"
                    }
                ]
            },
            "dependsOn": [
            ]
        }
    ]
}
```

## <a name="using-the-azure-cosmos-db-javascript-library-from-a-browser"></a>Met behulp van de Azure Cosmos DB JavaScript-bibliotheek vanuit een browser

De Azure Cosmos DB JavaScript-bibliotheek heeft vandaag, alleen de CommonJS-versie van de bibliotheek geleverd met het pakket. U hebt voor het gebruik van deze bibliotheek vanuit de browser, een hulpprogramma zoals Rollup of Webpack gebruiken om te maken van een browser compatibele bibliotheek. Bepaalde Node.js-clientbibliotheken hebt browser mocks voor hen. Hier volgt een voorbeeld van een webpack config-bestand met de benodigde mock-instellingen.

```javascript
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  node: {
    net: "mock",
    tls: "mock"
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
```
 
Hier volgt een [codevoorbeeld](https://github.com/christopheranderson/cosmos-browser-sample) die TypeScript en Webpack gebruikt met de Azure Cosmos DB JavaScript SDK-bibliotheek voor het maken van een takenlijst-app waarmee realtime updates wordt verzonden wanneer nieuwe items worden gemaakt.
Als een best practice, moet u niet de primaire sleutel gebruiken om te communiceren met Azure Cosmos DB vanuit de browser. In plaats daarvan de resource-tokens gebruiken om te communiceren. Zie voor meer informatie over brontokens [beveiligen van toegang tot Azure Cosmos DB](secure-access-to-data.md#resource-tokens) artikel.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over andere manieren om uw Azure Cosmos-account te beveiligen, de volgende artikelen:

* [Een firewall configureren voor Azure Cosmos DB](how-to-configure-firewall.md) artikel.

* [Virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos DB-account configureren](how-to-configure-vnet-service-endpoint.md)
    


---
title: Sjablonen voor Azure-implementatie koppelen | Microsoft Docs
description: Beschrijft hoe gekoppelde sjablonen in een Azure Resource Manager-sjabloon gebruiken om een sjabloonoplossing modulaire te maken. Laat zien hoe parameterwaarden doorgeven, geeft u een parameterbestand en dynamisch gemaakte URL's.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources
Uit binnen een Azure Resource Manager-sjabloon, kunt u koppelen aan een andere sjabloon waarmee u uw implementatie in een set afbreken gericht, doel-specifieke sjablonen. Net als bij een toepassing in verschillende codeklassen ontleedt, biedt afbreken voordelen wat betreft testen, hergebruiken en leesbaarheid.  

U kunt de parameters van een sjabloon van de belangrijkste doorgeven aan een gekoppelde sjabloon en deze parameters rechtstreeks kunnen toewijzen aan parameters of variabelen die worden weergegeven door de aanroepende sjabloon. De gekoppelde sjabloon kunt ook een uitvoervariabele doorgeven terug naar de bronsjabloon inschakelen van een wederzijdse gegevensuitwisseling tussen de sjablonen.

## <a name="linking-to-a-template"></a>Koppelen aan een sjabloon
U maakt een koppeling tussen twee sjablonen door een implementatie-bron in de belangrijkste sjabloon die naar de gekoppelde sjabloon verwijst toe te voegen. U stelt de **templateLink** eigenschap met de URI van de gekoppelde sjabloon. U kunt parameterwaarden kan opgeven voor de gekoppelde sjabloon rechtstreeks in uw sjabloon of in een parameterbestand. Het volgende voorbeeld wordt de **parameters** eigenschap rechtstreeks een parameterwaarde opgeven.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Net als andere brontypen, kunt u de afhankelijkheden tussen de gekoppelde sjabloon en andere resources instellen. Daarom als voor andere bronnen vereist een waarde voor de uitvoer van de gekoppelde sjabloon, kunt u ervoor dat de gekoppelde sjabloon voordat deze is geïmplementeerd. Of als de gekoppelde sjabloon is afhankelijk van andere bronnen, kunt u ervoor zorgen dat andere resources worden geïmplementeerd voordat u de gekoppelde sjabloon. U kunt een waarde wordt opgehaald uit een gekoppelde sjabloon met de volgende syntaxis:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

De Resource Manager-service moet toegang kunnen krijgen tot de gekoppelde sjabloon. U kunt een lokaal bestand of een bestand dat is alleen beschikbaar op uw lokale netwerk voor de gekoppelde sjabloon opgeven. U kunt alleen een URI-waarde die een bevat opgeven **http** of **https**. Een mogelijkheid is het plaatsen van de gekoppelde sjabloon in een opslagaccount en de URI gebruiken voor het item, zoals weergegeven in het volgende voorbeeld:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Hoewel de gekoppelde sjabloon moet extern beschikbaar zijn, hoeft deze niet algemeen beschikbaar is voor het publiek. U kunt uw sjabloon toevoegen aan een persoonlijke opslagaccount die toegankelijk is voor alleen de eigenaar van het opslagaccount. Vervolgens maakt u een shared access signature (SAS)-token om toegang te maken tijdens de implementatie. U toevoegen deze SAS-token aan de URI voor de gekoppelde sjabloon. Zie voor stappen over het instellen van een sjabloon in een opslagaccount en een SAS-token te genereren, [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md) of [resources met Resource Manager-sjablonen te implementeren en Azure CLI](resource-group-template-deploy-cli.md). 

Het volgende voorbeeld ziet een bovenliggende-sjabloon die is gekoppeld aan een andere sjabloon. De gekoppelde sjabloon wordt geopend met een SAS-token dat is doorgegeven als parameter.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Hoewel het token is doorgegeven als een beveiligde tekenreeks, wordt de URI van de gekoppelde sjabloon, met inbegrip van de SAS-token in de implementatiebewerkingen vastgelegd. Om te beperken van blootstelling, instellen dat een verlopen voor de token.

Resource Manager verwerkt elke gekoppelde sjabloon als een afzonderlijke implementatie. In de geschiedenis van de implementatie voor de resourcegroep ziet u afzonderlijke implementaties voor de bovenliggende en geneste sjablonen.

![implementatiegeschiedenis](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a>Koppelen aan een parameterbestand.
Het volgende voorbeeld wordt de **parametersLink** eigenschap in op de koppeling naar een parameterbestand.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

De URI-waarde voor het gekoppelde parameterbestand mag geen een lokaal bestand, en vergezeld gaan van een **http** of **https**. De parameter-bestand kan ook worden beperkt tot toegang via een SAS-token.

## <a name="using-variables-to-link-templates"></a>Gebruik van variabelen sjablonen koppelen
De voorgaande voorbeelden toonden vastgelegde URL waarden voor de sjabloon-koppelingen. Deze methode werkt mogelijk voor een eenvoudige sjabloon, maar werkt niet goed bij het werken met een groot aantal modulaire sjablonen. U kunt in plaats daarvan een statische variabele waarin een basis-URL voor de belangrijkste sjabloon maken en vervolgens de URL's voor de gekoppelde sjablonen van deze basis-URL dynamisch maken. Het voordeel van deze benadering is kunt u gemakkelijk verplaatsen of de sjabloon vertakken omdat u alleen hoeft te wijzigen van de statische variabele in de belangrijkste sjabloon. De belangrijkste sjabloon het juiste URI's in de sjabloon opgesplitste is geslaagd.

Het volgende voorbeeld laat zien hoe u een basis-URL om te maken van twee URL's voor de gekoppelde sjablonen (**sharedTemplateUrl** en **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

U kunt ook [deployment()](resource-group-template-functions-deployment.md#deployment) ophalen van de basis-URL voor de huidige sjabloon en gebruik die voor de URL voor andere sjablonen op dezelfde locatie bevinden. Deze methode is handig als de locatie van uw sjabloon (mogelijk vanwege versioning wijzigt) of u wilt voorkomen dat URL's in het sjabloonbestand hard coderen. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Compleet voorbeeld
Het volgende voorbeeld sjablonen weergeven een vereenvoudigde rangschikking van gekoppelde sjablonen ter illustratie van enkele concepten uitgelegd die in dit artikel. Wordt ervan uitgegaan dat de sjablonen zijn toegevoegd aan de container in een opslagaccount met openbare toegang uitgeschakeld. De gekoppelde sjabloon retourneert een waarde naar de belangrijkste sjabloon in de **levert** sectie.

De **parent.json** bestand bestaat uit:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

De **helloworld.json** bestand bestaat uit:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

In PowerShell kunt u een token verkrijgen voor de container en implementeren van de sjablonen met:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

In Azure CLI 2.0, moet u een token verkrijgen voor de container en implementeren van de sjablonen met de volgende code:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het definiëren van de implementatievolgorde voor uw resources, [afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md)
* Zie voor informatie over het definiëren van één resource maar veel exemplaren van het maken, [meerdere exemplaren van resources in Azure Resource Manager maken](resource-group-create-multiple.md)


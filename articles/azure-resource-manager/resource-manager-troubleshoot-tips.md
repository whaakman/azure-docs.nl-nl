---
title: Fouten van de Azure-implementatie begrijpen | Microsoft Docs
description: Beschrijft hoe u meer informatie over Azure-implementatiefouten.
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Fout in de implementatie, azure-implementatie implementeren in azure
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-azure-deployment-errors"></a>Fouten van de Azure-implementatie begrijpen
Dit onderwerp beschrijft implementatiefouten en hoe u meer informatie over een fout kunt detecteren. Zie voor oplossingen voor algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Twee soorten fouten
Er zijn twee typen fouten die u kunt ontvangen:

* Validatiefouten
* Implementatiefouten

De volgende afbeelding ziet u het logboek voor een abonnement. Hiermee geeft u twee implementaties. In een implementatie van de sjabloon validatie is mislukt (**valideren**) en niet worden voortgezet. In de andere implementatie van de sjabloon gevalideerd, maar is mislukt bij het maken van de resources (**schrijven implementaties**). 

![foutcode weergeven](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Validatiefouten worden veroorzaakt door scenario's die vóór de implementatie kunnen worden bepaald. Ze bevatten syntaxisfouten in de sjabloon of het implementeren van resources die uw abonnement quota's zou overschrijden. Implementatiefouten worden veroorzaakt door de voorwaarden die tijdens de implementatie optreden. Ze bevatten toegang wilt krijgen tot een resource die parallel wordt geïmplementeerd.

Beide soorten fouten retourneren een foutcode die u gebruikt voor het oplossen van de implementatie. Beide soorten fouten worden weergegeven in de [activiteitenlogboek](resource-group-audit.md). Validatiefouten worden echter niet weergegeven in de geschiedenis van uw implementatie omdat de implementatie is nooit gestart.

## <a name="determine-error-code"></a>Foutcode bepalen

U kunt meer informatie over een fout door te kijken naar het foutbericht en de foutcode. De [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md) artikel geeft oplossingen-foutcode. Dit onderwerp leest hoe de Azure portal gebruiken voor het detecteren van de foutcode.

### <a name="validation-errors"></a>Validatiefouten

Bij het implementeren via de portal, ziet u een validatiefout opgetreden na het verzenden van uw waarden.

![Fout bij de portal validatie weergeven](./media/resource-manager-troubleshoot-tips/validation-error.png)

Selecteer het bericht voor meer informatie. In de volgende afbeelding ziet u een **InvalidTemplateDeployment** fout en een bericht verschijnt dat een beleid voor implementatie wordt geblokkeerd.

![validatie details weergeven](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Implementatiefouten

Als de bewerking is geslaagd, maar niet tijdens de implementatie, ziet u de fout in de meldingen. Selecteer de melding.

![Fout bij wijzigingsbericht](./media/resource-manager-troubleshoot-tips/notification.png)

Er is meer informatie over de implementatie. Selecteer de optie voor meer informatie over de fout.

![implementatie is mislukt](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Ziet u de foutmelding en foutcodes. Er zijn twee foutcodes. De eerste foutcode (**implementatie mislukt**) is een algemene fout die biedt geen informatie die u nodig hebt voor het oplossen van de fout. De tweede foutcode (**StorageAccountNotFound**) bevat de details die u nodig hebt. 

![Details van fouten](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Inschakelen van logboekregistratie voor foutopsporing
Soms moet u meer informatie over de aanvraag en -antwoord om te ontdekken wat er mis ging. U kunt met behulp van PowerShell of Azure CLI aanvragen dat aanvullende informatie wordt geregistreerd tijdens een implementatie.

- PowerShell

   In PowerShell, stelt u de **DeploymentDebugLogLevel** parameter naar alle, ResponseContent of RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Controleer de aanvraag van inhoud met de volgende cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Of het antwoord inhoud met:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Deze informatie kunt u bepalen of een waarde in de sjabloon niet correct wordt ingesteld.

- Azure CLI

   Controleer de implementatiebewerkingen met de volgende opdracht:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Geneste sjabloon

   Meld u foutopsporingsgegevens voor een geneste sjabloon met de **debugSetting** element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Maken van een sjabloon voor het oplossen van problemen
In sommige gevallen is de eenvoudigste manier om op te lossen, de sjabloon voor het testen van de onderdelen hiervan. U kunt maken als een vereenvoudigde sjabloon waarmee u zich kunt richten op het onderdeel dat u denkt dat de fout wordt veroorzaakt. Stel bijvoorbeeld dat u ontvangt een fout opgetreden bij het verwijzen naar een resource. In plaats van te moeten omgaan met een volledige sjabloon, een sjabloon die als resultaat geeft het onderdeel dat wordt veroorzaakt door het probleem te maken. Dit kunt u bepalen of u doorgeeft aan de juiste parameters met sjabloonfuncties correct en ophalen van de resource die u verwacht.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Of stel fouten implementatie die u van mening zijn gerelateerd bent aan afhankelijkheden niet correct instellen. Test uw sjabloon door in vereenvoudigde sjablonen op te splitsen. Maak eerst een sjabloon die slechts één bron (zoals een SQL-Server) implementeert. Als u zeker dat u die correct gedefinieerd resource hebt, kunt u een resource die afhankelijk zijn van deze (zoals een SQL-Database) toevoegen. Wanneer u deze twee bronnen onjuist is gedefinieerd hebt, moet u andere afhankelijke resources (zoals controlebeleid) toevoegen. Verwijder de resourcegroep om te controleren of u voldoende testen van de afhankelijkheden tussen elke testimplementatie. 

## <a name="check-deployment-sequence"></a>Controleer de volgorde van de implementatie

Veel implementatiefouten gebeuren wanneer een onverwachte reeks resources worden geïmplementeerd. Deze fouten zich voordoen als afhankelijkheden niet correct zijn ingesteld. Wanneer er ontbreekt een vereiste afhankelijkheid, kunt u één resource probeert te gebruiken van een waarde voor een andere bron, maar de andere nog niet bestaat. U krijgt een foutmelding weergegeven dat een bron is niet gevonden. U kunt dit type fout afwisselend tegenkomen, omdat het tijdstip waarop de implementatie voor elke resource kan verschillen. Uw eerste poging tot het implementeren van uw resources slaagt bijvoorbeeld omdat een vereiste bron willekeurig tijdstip is voltooid. Echter, de tweede poging mislukt, omdat de vereiste bron is niet op tijd voltooid. 

Maar u wilt voorkomen dat afhankelijkheden die nodig zijn niet instellen. Wanneer u onnodige afhankelijkheden hebt, kunt u de duur van de implementatie verlengen door te voorkomen dat de bronnen die niet afhankelijk van elkaar worden geïmplementeerd parallel. U kunt bovendien circulaire afhankelijkheden die de implementatie blokkeren maken. De [verwijzing](resource-group-template-functions-resource.md#reference) functie maakt een impliciete afhankelijkheid voor de bron waarnaar wordt verwezen, wanneer deze resource is geïmplementeerd in dezelfde sjabloon. Daarom kunnen er meer afhankelijkheden dan de afhankelijkheden die zijn opgegeven de **dependsOn** eigenschap. De [resourceId](resource-group-template-functions-resource.md#resourceid) functie niet maken van een impliciete afhankelijkheid of valideren dat de resource bestaat.

Wanneer u afhankelijkheid problemen ondervindt, moet u meer inzicht krijgen in de volgorde van de resource-implementatie. De volgorde van implementatiebewerkingen weergeven:

1. Selecteer de geschiedenis van de implementatie voor de resourcegroep.

   ![Geschiedenis van implementatie selecteren](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Selecteer een implementatie van de geschiedenis en selecteer **gebeurtenissen**.

   ![gebeurtenissen van de implementatie selecteren](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Controleer de volgorde van gebeurtenissen voor elke resource. Let op de status van elke bewerking. De volgende afbeelding ziet u bijvoorbeeld drie storage-accounts die geïmplementeerd parallel. U ziet dat de drie storage-accounts op hetzelfde moment worden gestart.

   ![Parallelle implementatie](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   De volgende afbeelding ziet u drie opslagaccounts die niet zijn geïmplementeerd parallel. Het tweede storage-account is afhankelijk van het eerste storage-account en het derde storage-account is afhankelijk van de tweede storage-account. Daarom wordt het eerste opslagaccount gestart, geaccepteerd en voltooid voordat de volgende is gestart.

   ![sequentiële implementatie](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Zelfs voor complexere scenario's, kunt u dezelfde techniek gebruiken om te detecteren wanneer de implementatie wordt gestart en voltooid voor elke resource. Bekijk de gebeurtenissen van uw implementatie om te zien als de volgorde anders is dan u verwacht. Zo ja, Herzie de afhankelijkheden voor deze bron.

Resource Manager identificeert circulaire afhankelijkheden tijdens de validatie van de sjabloon. Retourneert een foutbericht weergegeven waarin wordt vermeld specifiek een circulaire afhankelijkheid bestaat. Voor het oplossen van een circulaire afhankelijkheid:

1. In de sjabloon de bron die is geïdentificeerd in de circulaire afhankelijkheid te vinden. 
2. Voor die bron onderzoekt de **dependsOn** eigenschap en het gebruik van de **verwijzing** om te zien welke resources afhankelijk van is de functie. 
3. Controleer deze bronnen om te zien welke bronnen ze afhankelijk zijn. Volg de afhankelijkheden totdat er een resource die afhankelijk van de oorspronkelijke bron is.
5. Voor de resources die zijn betrokken bij de circulaire afhankelijkheid zorgvuldig al gebruik van de **dependsOn** eigenschap voor het identificeren van eventuele afhankelijkheden die niet nodig zijn. Verwijder deze afhankelijkheden. Als u niet zeker weet dat er een afhankelijkheid is vereist, probeert u het verwijdert. 
6. De sjabloon opnieuw implementeert.

Verwijderen van de waarden uit de **dependsOn** eigenschap kan fouten veroorzaken wanneer u de sjabloon implementeert. Als u er een fout optreedt, voegt u de afhankelijkheid terug naar de sjabloon. 

Als deze aanpak circulaire afhankelijkheid niet wordt opgelost, kunt u deel uitmaakt van uw implementatie logica verplaatsen naar onderliggende resources (zoals extensies of configuratie-instellingen). Deze onderliggende resources implementeren na de resources die zijn betrokken bij de circulaire afhankelijkheid configureren. Stel bijvoorbeeld dat u twee virtuele machines implementeert, maar u moet eigenschappen instellen voor elk criterium die verwijzen naar de andere. U kunt deze implementeren in de volgende volgorde:

1. vm1
2. vm2
3. Extensie op vm1, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm1 die van vm2 krijgt.
4. Extensie op vm2, is afhankelijk van vm1 en vm2. De extensie waarden ingesteld op vm2 van van vm1 krijgt.

Dezelfde manier werkt voor App Service-apps. Overweeg configuratiewaarden verplaatsen naar een onderliggende resource van resource voor de app. U kunt twee web-apps in de volgende volgorde kunt implementeren:

1. webapp1
2. webapp2
3. configuratie voor webapp1, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp2.
4. configuratie voor webapp2, is afhankelijk van webapp1 en webapp2. Het bevat app-instellingen met waarden uit webapp1.


## <a name="next-steps"></a>Volgende stappen
* Zie voor oplossingen voor algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het controleren van acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
* Zie voor meer informatie over acties om de fouten tijdens de implementatie, [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).

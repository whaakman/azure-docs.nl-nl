---
title: Problemen met Azure Data Factory
description: Informatie over het oplossen van problemen bij het gebruik van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: douglasl
robots: noindex
ms.openlocfilehash: 5b9e0a30658e1cb8fe0f83d55c04f120637babaf
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549201"
---
# <a name="troubleshoot-data-factory-issues"></a>Problemen met Data Factory oplossen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. 

In dit artikel vindt u tips voor probleemoplossing voor problemen bij het gebruik van Azure Data Factory. In dit artikel de lijst bevat niet alle mogelijke problemen bij het gebruik van de service, maar het geeft sommige problemen en algemene tips voor probleemoplossing.   

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Fout: Het abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.DataFactory'
Als u dit foutbericht ontvangt, is de resourceprovider van Azure Data Factory niet geregistreerd op uw computer. Ga als volgt te werk:

1. Start Azure PowerShell.
2. Meld u aan bij uw Azure-account met de volgende opdracht.

    ```powershell
    Connect-AzAccount
    ```
3. Voer de volgende opdracht om de Azure Data Factory-provider te registreren.

    ```powershell        
    Register-AzResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Probleem: Niet gemachtigd om een Data Factory-cmdlet uit te voeren
Waarschijnlijk gebruikt u een verkeerd Azure-account of -abonnement met de Azure PowerShell. Gebruik de volgende cmdlets om het juiste Azure-account en -abonnement te selecteren voor gebruik met de Azure PowerShell.

1. Verbinding maken-AzAccount - gebruik de juiste gebruikers-ID en wachtwoord
2. Get-AzSubscription - weergeven van alle abonnementen voor het account.
3. Selecteer AzSubscription &lt;abonnementsnaam&gt; -Selecteer het juiste abonnement. Gebruik hetzelfde account dat u gebruikt om te maken van een data factory in Azure portal.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Probleem: Niet Express-installatie van Data Management Gateway gestart vanuit Azure portal
Voor de snelle installatie voor Data Management Gateway is Internet Explorer vereist of een webbrowser die compatibel is met Microsoft ClickOnce. Als u de snelle installatie niet kunt starten, voert u een van de volgende handelingen uit:

* Internet Explorer of een met Microsoft ClickOnce compatibele webbrowser gebruiken.

    Als u met Chrome werkt, gaat u naar de [Chrome Web Store](https://chrome.google.com/webstore/), zoekt u op het trefwoord 'ClickOnce', en kiest en installeert u een van de ClickOnce-extensies.

    Doe hetzelfde voor Firefox (Installeer de invoegtoepassing). Klik op de knop Menu openen op de werkbalk (drie horizontale lijnen in de rechterbovenhoek). Klik vervolgens op Invoegtoepassingen, zoek op het trefwoord 'ClickOnce', kies een van de ClickOnce-extensies en installeer deze.
* Gebruik de **handmatige installatie** koppeling die wordt weergegeven op de dezelfde blade in de portal. U kunt deze aanpak gebruiken om te downloaden van bestand voor installatie en het handmatig uitvoeren. Nadat de installatie voltooid is, ziet u het dialoogvenster Data Management Gateway-configuratie. Kopieer de **sleutel** uit het portalscherm en gebruik deze in Configuratiebeheer om de gateway handmatig bij de service te registreren.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Probleem: Geen verbinding maken met on-premises SQL Server
Start **Data Management Gateway Configuratiebeheer** op de gatewaycomputer en gebruik de **probleemoplossing** tabblad voor het testen van de verbinding met SQL Server vanaf de gatewaycomputer. Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Probleem: Invoersegmenten blijven de wachtstatus behouden
De segmenten mogelijk **wachten** staat vanwege verschillende redenen. Een veelvoorkomende reden is dat de **externe** eigenschap niet is ingesteld op **waar**. Een gegevensset die is geproduceerd buiten het bereik van Azure Data Factory moet worden gemarkeerd met **externe** eigenschap. Deze eigenschap geeft aan dat de gegevens extern en niet worden gedekt door pijplijnen in de data factory. De gegevenssegmenten worden gemarkeerd als **Gereed** wanneer de gegevens beschikbaar zijn in het desbetreffende archief.

Raadpleeg het volgende voorbeeld voor het gebruik van de eigenschap **external**. U kunt optioneel opgeven **externalData*** wanneer u external instelt op true.

Raadpleeg het artikel [Datasets](data-factory-create-datasets.md) (Gegevenssets) voor meer informatie over deze eigenschap.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

U kunt het probleem oplossen door de eigenschap **external** en de optionele sectie **externalData** toe te voegen aan de JSON-definitie van de invoertabel en de tabel vervolgens opnieuw te maken.

### <a name="problem-hybrid-copy-operation-fails"></a>Probleem: Hybride kopieerbewerking is mislukt
Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor stappen voor het oplossen van problemen bij het kopiëren van/naar een on-premises gegevens opslaan met behulp van de Data Management Gateway.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Probleem: On-demand HDInsight inrichten mislukt
Wanneer u een gekoppelde service van het type HDInsightOnDemand, moet u een linkedServiceName opgeven die naar een Azure Blob-opslag verwijst. De service Data Factory gebruikt deze opslag om logboekbestanden en ondersteunende bestanden voor het HDInsight-cluster op aanvraag op te slaan.  Soms mislukt het inrichten van een HDInsight-cluster op aanvraag en wordt de volgende fout weergegeven:

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Deze fout geeft meestal aan dat de locatie van het opslagaccount, opgegeven in de linkedServiceName, zich niet bevindt in hetzelfde datacenter waar de inrichting van HDInsight plaatsvindt. Voorbeeld: als uw data factory in VS-West is en de Azure-opslag in VS-Oost is, de inrichting van op aanvraag mislukt in VS-West.

Daarnaast is er een tweede JSON-eigenschap, additionalLinkedServiceNames, waar extra opslagaccounts in HDInsight op aanvraag kunnen worden opgegeven. Deze extra gekoppelde opslagaccounts moeten zich op dezelfde locatie als het HDInsight-cluster, of deze mislukt met de dezelfde fout.

### <a name="problem-custom-net-activity-fails"></a>Probleem: Aangepaste .NET-activiteit is mislukt
Zie [fouten opsporen in een pijplijn met een aangepaste activiteit](data-factory-use-custom-activities.md#troubleshoot-failures) voor gedetailleerde stappen.

## <a name="use-azure-portal-to-troubleshoot"></a>Azure-portal gebruiken om op te lossen
### <a name="using-portal-blades"></a>Met behulp van portal-blades
Zie [de pijplijn bewaken](data-factory-build-your-first-pipeline-using-editor.md#monitor-a-pipeline) voor stappen.

### <a name="using-monitor-and-manage-app"></a>App voor controle en beheer gebruiken
Zie [bewaken en beheren van data factory-pijplijnen met behulp van de Monitor and Manage App](data-factory-monitor-manage-app.md) voor meer informatie.

## <a name="use-azure-powershell-to-troubleshoot"></a>Azure PowerShell gebruiken om op te lossen
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure PowerShell gebruiken voor het oplossen van een fout
Zie [Monitor Data Factory-pijplijnen met behulp van Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) voor meer informatie.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: https://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png

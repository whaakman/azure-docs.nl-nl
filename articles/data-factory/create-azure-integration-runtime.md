---
title: Azure integratieruntime maken in Azure Data Factory | Microsoft Docs
description: Leer hoe u Azure integratieruntime maken in Azure Data Factory, dat wordt gebruikt om te kopiëren van gegevens en transformatieactiviteiten verzending.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 4b166ded3dcef4a89951eb81f7f1b321f89a0e67
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57576035"
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Over het maken en configureren van Azure Integration Runtime
De Integration Runtime (IR) is de rekeninfrastructuur die door Azure Data Factory gebruikt om u te bieden mogelijkheden voor gegevensintegratie in verschillende netwerkomgevingen. Zie voor meer informatie over IR [integratieruntime](concepts-integration-runtime.md).

Azure-IR biedt een volledig beheerde rekenkracht om uit te voeren systeemeigen gegevens verkeer en de verzending van de activiteiten voor gegevenstransformatie voor compute-services zoals HDInsight. Het wordt gehost in Azure-omgeving en ondersteunt een verbinding met resources in openbare netwerkomgeving met openbaar toegankelijke eindpunten.

Dit document beschrijft hoe u kunt maken en configureren van Azure Integration Runtime. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-azure-ir"></a>Standaard Azure IR
Elke data factory heeft standaard een Azure IR in de back-end die ondersteuning biedt voor bewerkingen op cloud gegevensarchieven en compute-services in openbare netwerken. De locatie van deze Azure-IR is automatisch oplossen. Als **connectVia** eigenschap niet is opgegeven in de definitie van de gekoppelde service, de standaard Azure IR wordt gebruikt. U moet alleen een Azure IR expliciet maken als u wilt de locatie van de IR expliciet worden gedefinieerd, of als u graag zou willen vrijwel groep de uitvoeringen van activiteit op verschillende IRs voor beheer van doel. 

## <a name="create-azure-ir"></a>Azure IR maken
Integratieruntime kan worden gemaakt met behulp van de **Set AzDataFactoryV2IntegrationRuntime** PowerShell-cmdlet. Voor het maken van een Azure IR, geeft u de naam, locatie en geef de opdracht. Hier volgt een voorbeeldopdracht in een Azure IR maken met de locatie ingesteld op 'West-Europa':

```powershell
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Voor Azure IR gebruiken, het type moet worden ingesteld op **beheerde**. U hoeft geen compute-details opgeven, omdat het is volledig flexibel beheerd in de cloud. Geef compute details, zoals de grootte van knooppunt- en knooppunt tellen als u wilt maken van Azure-SSIS IR. Zie voor meer informatie, [maken en configureren van Azure-SSIS IR](create-azure-ssis-integration-runtime.md).

U kunt een bestaande Azure-IR als u wilt wijzigen van de locatie met de cmdlet Set-AzDataFactoryV2IntegrationRuntime PowerShell configureren. Zie voor meer informatie over de locatie van een Azure IR [Inleiding tot integratieruntime](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Gebruik Azure IR

Nadat een Azure IR is gemaakt, kunt u ernaar kunt verwijzen in de definitie van de gekoppelde Service. Hieronder wordt een voorbeeld van hoe u kunt verwijzen naar de Azure Integration Runtime hierboven hebt gemaakt van een gekoppelde Azure Storage-Service:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=...",
          "type": "SecureString"
        }
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor het maken van andere typen integration runtime:

- [Zelf-hostende integratie-runtime maken](create-self-hosted-integration-runtime.md)
- [Azure-SSIS integratieruntime maken](create-azure-ssis-integration-runtime.md)
 

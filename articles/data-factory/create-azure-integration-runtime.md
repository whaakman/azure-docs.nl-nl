---
title: Integratie van Azure runtime maken in Azure Data Factory | Microsoft Docs
description: "Informatie over het runtime van de integratie van Azure maken in Azure Data Factory, die wordt gebruikt voor het kopiëren van gegevens en het verzenden van de transformatie-activiteiten."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: spelluru
ms.openlocfilehash: e4e9416d75d5887de059a6fcfc66683940e3e6fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Het maken en configureren van Azure integratie Runtime
De integratie Runtime (IR) is de beheerinfrastructuur gebruikt door Azure Data Factory om te bieden mogelijkheden voor de integratie in verschillende netwerkomgevingen. Zie voor meer informatie over IR [integratie runtime](concepts-integration-runtime.md).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Data Factory V1 documentatie](v1/data-factory-introduction.md).

Azure IR biedt een volledig beheerde compute om uit te voeren systeemeigen verkeer en de verzending gegevens activiteiten voor gegevenstransformatie voor compute services zoals HDInsight. Het wordt gehost in Azure-omgeving en biedt ondersteuning voor verbinding maken met resources in openbare netwerkomgeving met openbare toegankelijk eindpunten.

Dit document wordt geïntroduceerd, kunt u maken en configureren van Azure integratie Runtime. 

## <a name="default-azure-ir"></a>Standaard Azure IR
Elke gegevensfactory heeft standaard een Azure-IR in de back-end die ondersteuning biedt voor bewerkingen op cloud gegevens worden opgeslagen en compute services in openbaar netwerk. De locatie van die Azure-IR is automatisch oplossen. Als **connectVia** eigenschap is niet opgegeven in de definitie van de gekoppelde service, de opgegeven Azure-IR wordt gebruikt. U moet alleen een Azure-IR expliciet maken wanneer u wilt de locatie van de IR expliciet te definiëren, of als u wilt groep vrijwel uitvoeringen van de activiteit op verschillende belastingdienst voor management doel. 

## <a name="create-azure-ir"></a>Azure IR maken
Integratie Runtime kan worden gemaakt met de **Set AzureRmDataFactoryV2IntegrationRuntime** PowerShell-cmdlet. Voor het maken van een Azure-IR u de naam, locatie en geef de opdracht. Hier volgt een voorbeeld van een opdracht voor het maken van een Azure-IR met de locatie ingesteld op 'West-Europa':

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Voor Azure IR, het type moet worden ingesteld op **beheerde**. U hoeft niet te compute details opgeven omdat deze volledig beheerd Elastisch in de cloud. Geef compute details zoals knooppunt en de knooppuntgrootte van het waarop u wilt maken van Azure SSIS-IR tellen Zie voor meer informatie [maken en configureren van Azure SSIS-IR](create-azure-ssis-integration-runtime.md).

U kunt een bestaande Azure-IR om de locatie met de cmdlet Set-AzureRmDataFactoryV2IntegrationRuntime PowerShell te wijzigen. Zie voor meer informatie over de locatie van een Azure-IR [Inleiding tot integratie runtime](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Gebruik Azure IR

Zodra een Azure-IR is gemaakt, kunt u het raadplegen in de definitie van de gekoppelde Service. Hieronder wordt een voorbeeld van hoe u kunt verwijzen naar de Runtime van de integratie van Azure gemaakt hierboven van een gekoppelde Azure Storage-Service:  

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
Zie de volgende artikelen voor het maken van andere typen runtimes integratie:

- [Zelf-hostende integratie-runtime maken](create-self-hosted-integration-runtime.md)
- [Azure-SSIS-integratie runtime maken](create-azure-ssis-integration-runtime.md)
 

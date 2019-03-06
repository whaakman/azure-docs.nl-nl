---
title: Enterprise Edition voor de Azure-SSIS-Integratieruntime inrichten | Microsoft Docs
description: In dit artikel beschrijft de functies van de Enterprise Edition voor de Azure-SSIS Integration Runtime en over het inrichten van het
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: d2b06d044f68972ef72dd9b53401980e84ef779f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440927"
---
# <a name="provision-enterprise-edition-for-the-azure-ssis-integration-runtime"></a>Enterprise-editie voor de Azure-SSIS-Integratieruntime inrichten

De Enterprise-editie van de Azure-SSIS Integration Runtime kunt u de volgende geavanceerde gebruiken en premium-functies:
-   Onderdelen van de gegevens vastleggen (CDC) wijzigen
-   Oracle en Teradata SAP BW-connectors
-   SQL Server Analysis Services (SSAS) en Azure Analysis Services (AAS)-connectors en transformaties
-   Fuzzy groepering en Fuzzy opzoektabellen transformaties
-   Term extractie en termijn Lookup transformaties

Sommige van deze functies vereisen dat u voor het installeren van extra onderdelen voor het aanpassen van de Azure-SSIS-IR. Zie voor meer informatie over het installeren van extra onderdelen [aangepaste installatie voor de Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Enterprise-functies

| **Enterprise-functies** | **Beschrijvingen** |
|---|---|
| CDC-onderdelen | De CDC-bron, controletaak en transformatie van splitsen zijn vooraf geïnstalleerd in de Azure-SSIS IR Enterprise-editie. Als u wilt verbinding maken met Oracle, moet u ook de CDC Designer en de Service op een andere computer installeren. |
| Oracle-connectors | Het Oracle Connection Manager, de bron en bestemming zijn vooraf geïnstalleerd in de Enterprise-editie van Azure-SSIS IR. U moet ook het installeren van het stuurprogramma Oracle aanroepen Interface (OCI) en configureer indien nodig, Oracle Transport netwerk substraat (TNS) op de Azure-SSIS-IR. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. |
| Teradata-connectors | U moet de Teradata Connection Manager, de bron en bestemming, evenals het Teradata parallelle vervoerder (TPT) API- en Teradata ODBC-stuurprogramma installeren op de Azure-SSIS IR Enterprise-editie. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. |
| SAP BW-connectors | De SAP BW Connection Manager, de bron en bestemming zijn vooraf geïnstalleerd in de Azure-SSIS IR Enterprise-editie. U moet ook de SAP BW-stuurprogramma installeren op de Azure-SSIS-IR. Deze connectors bieden ondersteuning voor SAP BW 7.0 of eerdere versies. Voor verbinding met latere versies van SAP BW of andere SAP-producten, kunt u kopen en installeren van SAP-connectors van derden ISV's op de Azure-SSIS-IR. Zie voor meer informatie over het installeren van extra onderdelen [aangepaste installatie voor de Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Analysis Services-onderdelen               | De Data Mining Model Training bestemming, de bestemming dimensie verwerken en de bestemming voor het verwerken van partitie, evenals de Data Mining Query transformatie zijn vooraf geïnstalleerd in de Enterprise-editie van Azure-SSIS IR. Al deze onderdelen ondersteuning voor SQL Server Analysis Services (SSAS), maar alleen de partitie verwerken bestemming biedt ondersteuning voor Azure Analysis Services (AAS). Voor verbinding met SSAS, moet u ook [referenties voor Windows-verificatie configureren in SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Naast deze onderdelen worden de Analysis Services uitvoeren DDL-taak, de Analysis Services verwerkt-taak en de Data Mining querytaak ook vooraf op de Azure-SSIS IR Standard/Enterprise-editie. |
| Fuzzy groepering en Fuzzy opzoektabellen transformaties  | De transformaties Fuzzy groepering en Fuzzy opzoektabellen zijn vooraf geïnstalleerd in de Azure-SSIS IR Enterprise-editie. Deze onderdelen ondersteunen zowel SQL Server en Azure SQL Database voor het opslaan van referentiegegevens. |
| Term extractie en termijn Lookup transformaties | De transformaties Term extractie en het opzoeken van de termijn zijn vooraf geïnstalleerd in de Azure-SSIS IR Enterprise-editie. Deze onderdelen ondersteunen zowel SQL Server en Azure SQL Database voor het opslaan van referentiegegevens. |

## <a name="instructions"></a>Instructies

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1.  Download en installeer [Azure PowerShell](/powershell/azure/install-az-ps).

2.  Wanneer u inrichten of opnieuw configureren van de Azure-SSIS IR met PowerShell, voer `Set-AzDataFactoryV2IntegrationRuntime` met **Enterprise** als de waarde voor de **Edition** parameter voordat u begint met de Azure-SSIS-IR. Hier volgt een voorbeeld van een script:

    ```powershell
    $MyAzureSsisIrEdition = "Enterprise"

    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                               -Name $MyAzureSsisIrName
                                               -ResourceGroupName $MyResourceGroupName
                                               -Edition $MyAzureSsisIrEdition

    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                                 -Name $MyAzureSsisIrName
                                                 -ResourceGroupName $MyResourceGroupName
    ```

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste instellingen voor de Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Over het ontwikkelen van een betaalde versie of aangepaste onderdelen voor de Azure-SSIS integratieruntime in licentie gegeven](how-to-develop-azure-ssis-ir-licensed-components.md)

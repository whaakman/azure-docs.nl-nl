---
title: Enterprise-editie inrichten voor de integratie van Azure SSIS Runtime | Microsoft Docs
description: In dit artikel beschrijft de functies van de Enterprise Edition voor op de Azure-SSIS-integratie-Runtime en het inrichten
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 73997345895bc54f54db1d66c0c6c24c24153dd2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268106"
---
# <a name="provision-enterprise-edition-for-the-azure-ssis-integration-runtime"></a>Inrichten Enterprise Edition voor op de Runtime Azure SSIS-integratie

De Enterprise-editie van de Runtime Azure SSIS-integratie kunt u de volgende geavanceerde gebruiken en premium-functies:
-   Gegevens vastleggen CDC ()-onderdelen wijzigen
-   Oracle, Teradata en SAP BW-connectors
-   SQL Server Analysis Services (SSAS) en Azure Analysis Services (AAS) connectors en transformaties
-   Fuzzy groepering en Fuzzy opzoektabellen transformaties
-   Uitpakken van de termijn en Term Lookup transformaties

Sommige van deze functies vereisen dat u voor het installeren van extra onderdelen voor het aanpassen van de Azure-SSIS-IR Zie voor meer informatie over het installeren van extra onderdelen [aangepaste installatie voor de integratie van Azure SSIS runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Enterprise-functies

| **Enterprise-functies** | **Beschrijvingen** |
|---|---|
| CDC-onderdelen | De CDC-bron, controletaak en transformatie van de splitser vooraf op de Enterprise-editie van Azure SSIS IR zijn geïnstalleerd. Als u wilt verbinding maken met Oracle, moet u ook de CDC Designer en de Service op een andere computer installeren. |
| Oracle-connectors | De Oracle-Verbindingsbeheer, de bron en bestemming vooraf zijn op de Azure-SSIS IR Enterprise-editie. U moet ook het stuurprogramma Oracle aanroepen Interface (OCI) installeren en configureren van Oracle Transport netwerk substraat (TNS) op de Azure-SSIS-IR indien nodig Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. |
| Connectors voor Teradata | U moet de Teradata-Verbindingsbeheer, bron en bestemming, evenals het Teradata parallelle verzender (TPT) API en Teradata ODBC-stuurprogramma op de Enterprise-editie van Azure SSIS IR installeren. Zie [Aangepaste instelling voor de Azure-SSIS-integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md) voor meer informatie. |
| Connectors voor SAP BW | De SAP BW Connection Manager, de bron en bestemming zijn voor de Enterprise-editie van Azure SSIS IR vooraf. U moet ook de SAP BW-stuurprogramma op de Azure-SSIS-IR installeren Deze connectors ondersteuning voor SAP BW 7.0 of eerdere versies. Voor verbinding met latere versies van SAP BW of andere producten SAP, kunt u koopt en SAP-connectors van onafhankelijke softwareleveranciers op de Azure-SSIS-IR installeren Zie voor meer informatie over het installeren van extra onderdelen [aangepaste installatie voor de integratie van Azure SSIS runtime](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Analysis Services-onderdelen               | De Data Mining Model Training bestemming, de bestemming dimensie verwerken en de bestemming voor het verwerken van partitie, evenals de Data Mining Query transformatie zijn op de Enterprise-editie van Azure SSIS IR vooraf. Al deze onderdelen ondersteuning voor SQL Server Analysis Services (SSAS), maar alleen de partitie verwerken bestemming ondersteunt Azure Analysis Services (AAS). Voor verbinding met SSAS, moet u ook [referenties voor Windows-verificatie configureren in SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Naast deze onderdelen worden de Analysis Services uitvoeren DDL-taak, de taak verwerken van Analysis Services en voor de Data Mining querytaak ook vooraf op de Azure-SSIS IR Standard/Enterprise-editie. |
| Fuzzy groepering en Fuzzy opzoektabellen transformaties  | De transformaties Fuzzy groepering en Fuzzy opzoektabellen zijn vooraf op de Azure-SSIS IR Enterprise-editie geïnstalleerd. Deze onderdelen ondersteunen zowel SQL Server en Azure SQL Database voor het opslaan van referentiegegevens. |
| Uitpakken van de termijn en Term Lookup transformaties | De Term extractie en Term Lookup transformaties zijn vooraf geïnstalleerd op de Azure-SSIS IR Enterprise-editie. Deze onderdelen ondersteunen zowel SQL Server en Azure SQL Database voor het opslaan van referentiegegevens. |

## <a name="instructions"></a>Instructies

1.  Download en installeer [Azure PowerShell (versie 5,4 of hoger)](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018).

2.  Wanneer u inrichten of opnieuw configureren van de Azure-SSIS-IR met PowerShell, voer `Set-AzureRmDataFactoryV2IntegrationRuntime` met **Enterprise** als de waarde voor de **Edition** parameter voordat u begint met de Azure-SSIS-IR Hier volgt een voorbeeld van een script:

    ```powershell
    $MyAzureSsisIrEdition = "Enterprise"

    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                               -Name $MyAzureSsisIrName
                                               -ResourceGroupName $MyResourceGroupName
                                               -Edition $MyAzureSsisIrEdition

    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                                 -Name $MyAzureSsisIrName
                                                 -ResourceGroupName $MyResourceGroupName
    ```

## <a name="next-steps"></a>Volgende stappen

-   [Aangepaste instellingen voor de integratie van Azure SSIS runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Het ontwikkelen van betaald of aangepaste onderdelen voor de integratie van Azure SSIS runtime licentie](how-to-develop-azure-ssis-ir-licensed-components.md)

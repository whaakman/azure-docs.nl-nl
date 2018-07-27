---
title: Zakelijke continuïteit en noodherstel aanbevelingen voor herstel (BCDR) voor Azure-SSIS Integration Runtime | Microsoft Docs
description: In dit artikel bevat een overzicht van zakelijke continuïteit en noodherstel recovery aanbevelingen voor Azure-SSIS Integration Runtime.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/26/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 37347df2d543116085f52fed76c692b60fac2ad6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285735"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-recommendations-for-azure-ssis-integration-runtime"></a>Zakelijke continuïteit en noodherstel aanbevelingen voor herstel (BCDR) voor Azure-SSIS Integration Runtime

Ten behoeve van herstel na noodgevallen, kunt u stoppen van uw Azure-SSIS integratieruntime in de regio waarin deze momenteel wordt uitgevoerd en schakel over naar een andere regio opnieuw te starten. Het is raadzaam dat u [Azure gekoppelde regio's](../best-practices-availability-paired-regions.md) voor dit doel.

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u herstel na noodgevallen hebt ingeschakeld voor uw Azure SQL Database-server als de server een storing op hetzelfde moment heeft. Zie voor meer informatie, [overzicht van bedrijfscontinuïteit met Azure SQL Database](../sql-database/sql-database-business-continuity.md).

- Als u een virtueel netwerk in de huidige regio gebruikt, moet u een ander virtueel netwerk in de nieuwe regio gebruiken om uw Azure-SSIS integratieruntime verbinding te maken. Zie voor meer informatie, [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).

- Als u een aangepaste installatie gebruikt, moet u mogelijk om voor te bereiden van een andere SAS-URI voor de blob-container waarin uw aangepaste setup-script en de bijbehorende bestanden, zodat deze blijven toegankelijk tijdens een storing. Zie voor meer informatie, [configureren van een aangepaste installatie op een Azure-SSIS integratieruntime](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="steps"></a>Stappen

Volg deze stappen voor uw Azure-SSIS IR stopt, de IR overschakelen naar een nieuwe regio en probeer het opnieuw.

1. Stop de IR in de oorspronkelijke regio.

2. De volgende opdracht uit in PowerShell om bij te werken van de IR aanroepen

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Zie voor meer informatie over deze PowerShell-opdracht [maken van de Azure-SSIS integratieruntime in Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. De IR opnieuw starten.

## <a name="next-steps"></a>Volgende stappen

Houd rekening met deze andere configuratieopties voor de Azure-SSIS-IR:

- [De Azure-SSIS Integration Runtime voor hoge prestaties configureren](configure-azure-ssis-integration-runtime-performance.md)

- [Setup voor de Azure-SSIS integratieruntime aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Enterprise-editie voor de Azure-SSIS-Integratieruntime inrichten](how-to-configure-azure-ssis-ir-enterprise-edition.md)

---
title: 'PowerShell: Inschakelen BYOK TDE - Azure SQL Database Managed Instance | Microsoft Docs'
description: Informatie over het configureren van een Azure SQL beheerd exemplaar voor het starten van BYOK transparante gegevensversleuteling (TDE) gebruiken voor versleuteling-at-rest met behulp van PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: d428c74a486c4bcb6ea1568fbb8abd813960c708
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974981"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Transparent Data Encryption in een beheerd exemplaar met behulp van uw eigen sleutel uit Azure Key Vault beheren

Met dit PowerShell-voorbeeldscript configureert transparante gegevensversleuteling (TDE) in Bring Your Own Key-scenario voor Azure SQL Managed Instance, met behulp van een sleutel uit Azure Key Vault. Zie voor meer informatie over de TDE met Bring Your Own Key (BYOK) ondersteuning, [TDE Bring Your Own Key to Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

Als u wilt installeren en gebruiken van de PowerShell lokaal, vereist deze zelfstudie ook preview-versie van pakket AzureRM.Sql PowerShell *4.11.6-preview*. Voer de volgende opdracht uit om deze te installeren: `Install-Module -Name AzureRM.Sql -RequiredVersion 4.11.6-preview -AllowPrerelease`

## <a name="sample-scripts"></a>Voorbeeldscripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).

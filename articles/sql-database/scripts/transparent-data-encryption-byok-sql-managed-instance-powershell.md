---
title: 'PowerShell: Inschakelen van BYOK TDE - Azure SQL Database Managed Instance | Microsoft Docs'
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
ms.date: 04/19/2019
ms.openlocfilehash: 8eb924b3dc6ff912db402596c763dd69b85147a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390712"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault-preview"></a>Transparent Data Encryption in een beheerd exemplaar met behulp van uw eigen sleutel uit Azure Key Vault (Preview) beheren

Met dit PowerShell-voorbeeldscript configureert transparante gegevensversleuteling (TDE) in het scenario voor Bring Your Own Key (preview) voor Azure SQL Managed Instance, met behulp van een sleutel uit Azure Key Vault. Zie voor meer informatie over de TDE met Bring Your Own Key (BYOK) ondersteuning, [TDE Bring Your Own Key to Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Vereisten

- Een bestaand beheerd exemplaar. Zie [PowerShell gebruiken om u te maken van een Azure SQL Database managed instance](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Met behulp van beide PowerShell lokaal of met behulp van Azure Cloud Shell vereist 1.1.1-preview AZ PowerShell of een latere versie van de Preview-versie. Als u upgraden wilt, raadpleegt u [Azure PowerShell-module installeren](/powershell/azure/install-az-ps), of Voer de onderstaande voorbeeld van een script om de module te installeren.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-scripts"></a>Voorbeeldscripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).

---
title: 'PowerShell: BYOK TDE-Azure SQL Database beheerd exemplaar inschakelen | Microsoft Docs'
description: Meer informatie over het configureren van een Azure SQL Managed instance om te beginnen met het gebruik van BYOK Transparent Data Encryption (TDE) voor versleuteling-at-rest met behulp van Power shell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 04/19/2019
ms.openlocfilehash: d8f0f4a8e603a9040d166b00682077cff23abd8d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569688"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault-preview"></a>Transparent Data Encryption beheren in een beheerd exemplaar met uw eigen sleutel vanuit Azure Key Vault (preview-versie)

Met dit Power shell-voorbeeld script wordt Transparent Data Encryption (TDE) in Bring Your Own Key (preview)-scenario geconfigureerd voor een Azure SQL Managed instance, met behulp van een sleutel van Azure Key Vault. Zie [TDE Bring your own Key naar Azure SQL](../transparent-data-encryption-byok-azure-sql.md)voor meer informatie over de ondersteuning van TDE met Bring your own Key (BYOK).

## <a name="prerequisites"></a>Vereisten

- Een bestaand beheerd exemplaar. Zie [Power shell gebruiken om een Azure SQL database beheerd exemplaar te maken](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u Power shell lokaal of Azure Cloud Shell gebruikt, is AZ Power shell 1.1.1-Preview of een latere preview-versie vereist. Als u een upgrade wilt uitvoeren, raadpleegt u [Azure PowerShell-module installeren](/powershell/azure/install-az-ps)of voert u het onderstaande voorbeeld script uit om de module te installeren.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-scripts"></a>Voorbeeldscripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).

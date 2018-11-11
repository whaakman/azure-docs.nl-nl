---
title: Key Vault integreren met SQL Server op Windows-VM's in Azure (klassiek) | Microsoft Docs
description: Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure Key Vault. In dit onderwerp wordt uitgelegd hoe het gebruik van Azure Key Vault-integratie met SQL Server virtuele machines in het klassieke implementatiemodel maken.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5e156166ec9e085cf7a3c2dcb72aa9e87bbadf1
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244934"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Azure Key Vault-integratie configureren voor SQL Server op Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassiek](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Overzicht
Er zijn meerdere functies van SQL Server-versleuteling, zoals [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [versleuteling op kolom (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), en [back-upversleuteling](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van versleuteling moeten u de cryptografische sleutels die u voor versleuteling gebruikt opslaan en beheren. De service Azure Key Vault (AKV) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een beveiligde en maximaal beschikbare locatie. De [SQL Server-Connector](https://www.microsoft.com/download/details.aspx?id=45344) kan SQL Server om deze sleutels uit Azure Key Vault te gebruiken.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Als u SQL Server on-premises virtuele machines worden uitgevoerd, zijn er [stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-computer](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in virtuele Azure-machines, kunt u tijd besparen met behulp van de *Azure Key Vault-integratie* functie. Met een paar Azure PowerShell-cmdlets voor deze functie inschakelt, kunt u de configuratie die nodig zijn voor een SQL-VM voor toegang tot uw key vault automatiseren.

Wanneer deze functie is ingeschakeld, installeert en deze automatisch de SQL Server-Connector, configureert u de EKM-provider voor toegang tot Azure Key Vault, maakt u de referentie zodat u toegang krijgt tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde on-premises hebt bekeken, kunt u zien dat deze functie automatiseert de stappen 2 en 3. Het enige wat dat u moet nog steeds handmatig doen is het maken van de key vault en sleutels. Van daaruit is de volledige installatie van uw SQL-VM geautomatiseerd. Als deze functie kan deze installatie is voltooid, kunt u T-SQL-instructies gebruikt om te beginnen met het versleutelen van uw databases of back-ups zoals u gewend kunt uitvoeren.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Configureren van Azure Sleutelkluis-integratie
PowerShell gebruiken voor Azure Key Vault-integratie configureren. De volgende secties bevatten een overzicht van de vereiste parameters en vervolgens een PowerShell-voorbeeldscript.

### <a name="install-the-sql-server-iaas-extension"></a>De SQL Server IaaS-extensie installeren
Eerste, [installeren van de SQL Server IaaS-extensie](../classic/sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Inzicht in de invoerparameters
De volgende tabel bevat de vereiste parameters voor de PowerShell-script uitvoeren in de volgende sectie.

| Parameter | Beschrijving | Voorbeeld |
| --- | --- | --- |
| **$akvURL** |**De URL voor key vault** |"https://contosokeyvault.vault.azure.net/" |
| **$spName** |**Service-Principal-naam** |"fde2b411-33d5-4e11-af04eb07b669ccf2" |
| **$spSecret** |**Service-Principal-geheim** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=" |
| **$credName** |**Referentienaam**: de Azure Sleutelkluis-integratie maakt een referentie in SQL Server, zodat de virtuele machine toegang heeft tot de sleutelkluis. Kies een naam voor deze referentie. |"mycred1" |
| **$vmName** |**Naam van virtuele machine**: de naam van een eerder gemaakte SQL-VM. |"myvmname" |
| **$serviceName** |**Servicenaam**: naam van de Service in de Cloud die is gekoppeld aan de SQL-VM. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>Integratie van Azure Sleutelkluis met PowerShell
De **New-AzureVMSqlServerKeyVaultCredentialConfig** cmdlet maakt u een configuratieobject voor de functie Azure Key Vault-integratie. De **Set AzureVMSqlServerExtension** configureert u deze integratie met de **KeyVaultCredentialSettings** parameter. De volgende stappen laten zien hoe deze opdrachten gebruiken.

1. In Azure PowerShell eerst configureren de invoerparameters die zijn opgegeven met de specifieke waarden zoals beschreven in de voorgaande secties in dit onderwerp. Het volgende script is een voorbeeld.
   
        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Het volgende script vervolgens gebruiken voor het configureren en inschakelen van Azure Sleutelkluis-integratie.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

De SQL IaaS Agent-extensie wordt de SQL-VM bijwerken met deze nieuwe configuratie.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]


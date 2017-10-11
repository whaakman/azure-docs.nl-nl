---
title: Sleutelkluis integreert met SQL Server op Windows-machines in Azure (klassiek) | Microsoft Docs
description: Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure Sleutelkluis. Dit onderwerp wordt uitgelegd hoe u Azure Sleutelkluis-integratie met SQL Server-virtuele machines in het klassieke implementatiemodel maken.
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
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
ms.openlocfilehash: 2a9ac5763bb934bd0646e47c3936f7bdd0d603b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Configureren van Azure Sleutelkluis-integratie voor SQL Server op virtuele Machines in Azure (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassiek](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Overzicht
Er zijn meerdere onderdelen van SQL Server-versleuteling, zoals [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kolom: versleuteling op bestandsniveau (wissen)](https://msdn.microsoft.com/library/ms173744.aspx), en [back-upversleuteling](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van versleuteling hoeft te beheren en opslaan van de cryptografische sleutels die u voor versleuteling gebruikt. De service Azure Key Vault (Azure Sleutelkluis) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een locatie met veilige en maximaal beschikbaar. De [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) kan SQL-Server in het gebruik van deze sleutels van Azure Sleutelkluis.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Als u SQL Server met lokale virtuele machines uitvoert, zijn er [stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-machine](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in Azure VM's, kunt u tijd besparen met behulp van de *Azure Sleutelkluis-integratie* functie. Met een paar Azure PowerShell-cmdlets voor deze functie inschakelt, kunt u de configuratie die nodig zijn voor een SQL-VM voor toegang tot de sleutelkluis automatiseren.

Wanneer deze functie is ingeschakeld, en deze automatisch de SQL Server-Connector installeert, configureert u de EKM-provider voor toegang tot Azure Sleutelkluis, maakt u de referentie zodat u toegang tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde lokale bekeken, ziet u dat deze functie stap 2 en 3 automatiseert. Het enige dat u toch wilt handmatig doen is de sleutelkluis en de sleutels te maken. Van daaruit wordt de volledige installatie van de SQL-VM geautomatiseerd. Zodra deze functie kan voor deze installatie is voltooid, kunt u de T-SQL-instructies om te beginnen met het versleutelen van uw databases of back-ups normaal uitvoeren.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Configureren van Azure Sleutelkluis-integratie
PowerShell gebruiken voor het configureren van Azure Sleutelkluis-integratie. De volgende secties bevatten een overzicht van de vereiste parameters en vervolgens een PowerShell-voorbeeldscript.

### <a name="install-the-sql-server-iaas-extension"></a>De SQL Server IaaS-uitbreiding installeren
Eerst [installeren van de SQL Server IaaS-extensie](../classic/sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>De invoerparameters begrijpen
De volgende tabel bevat de vereiste parameters voor de PowerShell-script uitvoeren in de volgende sectie.

| Parameter | Beschrijving | Voorbeeld |
| --- | --- | --- |
| **$akvURL** |**De sleutelkluis-URL** |'https://contosokeyvault.vault.azure.net/' |
| **$spName** |**Service-Principal-naam** |'fde2b411 - 33d 5-4e11-af04eb07b669ccf2' |
| **$spSecret** |**Service-Principal-geheim** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =" |
| **$credName** |**Referentienaam**: de Azure Sleutelkluis-integratie maakt een referentie in SQL Server, zodat de virtuele machine toegang heeft tot de sleutelkluis. Kies een naam voor deze referentie. |'mycred1' |
| **$vmName** |**Naam van virtuele machine**: de naam van een eerder gemaakte SQL-VM. |'myvmname' |
| **$serviceName** |**Servicenaam**: naam van de Cloudservice die is gekoppeld aan de SQL VM. |'mycloudservicename' |

### <a name="enable-akv-integration-with-powershell"></a>Inschakelen van Azure Sleutelkluis-integratie met PowerShell
De **nieuw AzureVMSqlServerKeyVaultCredentialConfig** cmdlet maakt een configuratieobject voor de functie Azure Sleutelkluis-integratie. De **Set AzureVMSqlServerExtension** configureert u deze integratie met de **KeyVaultCredentialSettings** parameter. De volgende stappen laten zien hoe deze opdrachten gebruiken.

1. In Azure PowerShell eerst configureren de invoerparameters met uw specifieke waarden zoals beschreven in de voorgaande secties in dit onderwerp. Het volgende script is een voorbeeld.
   
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

De SQL-uitbreiding voor IaaS-Agent wordt de SQL VM bijgewerkt met deze nieuwe configuratie.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]


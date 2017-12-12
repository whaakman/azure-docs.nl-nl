---
title: HPC Pack compute clusterknooppunten beheren | Microsoft Docs
description: Meer informatie over hulpprogramma's van PowerShell-script wilt toevoegen, verwijderen, starten en stoppen van HPC Pack 2012 R2-cluster-rekenknooppunten in Azure
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 2ad67efecf9a688ac3e7ccd7cc32576e9a46d1f5
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Het aantal en de beschikbaarheid van rekenknooppunten in een HPC Pack-cluster in Azure beheren
Als u een HPC Pack 2012 R2-cluster in Azure VM's hebt gemaakt, kunt u manieren eenvoudig toevoegen, verwijderen, (ingericht) starten of stoppen (deprovision) sommige compute knooppunt VM's in het cluster. Hiertoe deze taken, Azure PowerShell-scripts die zijn geïnstalleerd op het hoofdknooppunt VM worden uitgevoerd. Deze scripts kunnen u bepalen het aantal en de beschikbaarheid van uw clusterbronnen HPC Pack zodat u de kosten kunt bepalen.

> [!IMPORTANT] 
> In dit artikel geldt alleen voor HPC Pack 2012 R2-clusters in Azure gemaakt met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> De PowerShell-scripts die worden beschreven in dit artikel zijn niet beschikbaar in HPC Pack 2016.

## <a name="prerequisites"></a>Vereisten
* **HPC Pack 2012 R2-cluster in Azure VM's**: een HPC Pack 2012 R2-cluster maken in het klassieke implementatiemodel. U kunt bijvoorbeeld de implementatie automatiseren met behulp van de virtuele machine met HPC Pack 2012 R2-afbeelding in Azure Marketplace en een Azure PowerShell-script. Zie voor informatie en vereisten [een HPC-Cluster maken met het implementatiescript HPC Pack IaaS](hpcpack-cluster-powershell-script.md).
  
    Na de implementatie, het knooppunt vinden management scripts in de map % CCP\_HOME % bin-map op het hoofdknooppunt. Elk van de scripts worden uitgevoerd als een beheerder.
* **Azure publish certificaat voor het bestand of het beheer van instellingen**: U moet een van de volgende opties in het hoofdknooppunt te doen:
  
  * **De Azure Import bestand publicatie-instellingen**. U doet dit door de volgende Azure PowerShell-cmdlets worden uitgevoerd op het hoofdknooppunt:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Het Azure-beheercertificaat configureren op het hoofdknooppunt**. Als u het cer-bestand hebt, importeer het in het certificaatarchief CurrentUser\My en voer de volgende Azure PowerShell-cmdlet voor uw Azure-omgeving (AzureCloud of AzureChinaCloud):
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Rekenknooppunt virtuele machines toevoegen
Toevoegen van rekenknooppunten met de **toevoegen HpcIaaSNode.ps1** script.

### <a name="syntax"></a>Syntaxis
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameters
* **ServiceName**: naam van de cloudservice die nieuwe berekening knooppunt virtuele machines worden toegevoegd aan.
* **ImageName**: Azure VM-installatiekopienaam, die kan worden verkregen via de Azure portal of Azure PowerShell-cmdlet **Get-AzureVMImage**. De afbeelding moet voldoen aan de volgende vereisten:
  
  1. Een Windows-besturingssysteem moet worden geïnstalleerd.
  2. HPC Pack moet worden geïnstalleerd in de rol van de compute-knooppunt.
  3. De afbeelding moet een persoonlijke installatiekopie in de categorie van de gebruiker, niet een openbare Azure VM-installatiekopie.
* **Aantal**: aantal rekenknooppunt virtuele machines worden toegevoegd.
* **InstanceSize**: grootte van het rekenknooppunt virtuele machines.
* **DomainUserName**: gebruiker domeinnaam, die wordt gebruikt voor de nieuwe virtuele machines toevoegen aan het domein.
* **DomainUserPassword**: wachtwoord van de domeingebruiker.
* **NodeNameSeries** (optioneel): naamgeving patroon voor de rekenknooppunten. De indeling moet &lt; *hoofdmap\_naam*&gt;&lt;*Start\_getal*&gt;%. Bijvoorbeeld: MyCN % 10% betekent een reeks de compute-knooppuntnamen vanaf MyCN11. Als niet wordt opgegeven, wordt het script het geconfigureerde knooppunt naming reeksen in de HPC-cluster gebruikt.

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld wordt 20 grootte grote rekenknooppunt virtuele machines in de cloudservice *hpcservice1*, op basis van de VM-installatiekopie *hpccnimage1*.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Rekenknooppunt virtuele machines verwijderen
Verwijderen van rekenknooppunten met de **verwijderen HpcIaaSNode.ps1** script.

### <a name="syntax"></a>Syntaxis
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters
* **Naam**: namen van de clusterknooppunten worden verwijderd. Jokertekens worden ondersteund. De naam van de parameter is Name. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.
* **Knooppunt**: de HpcNode-object voor de knooppunten moet worden verwijderd, die kunnen worden verkregen via de HPC PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.
* **DeleteVHD** (optioneel): instelling van de gekoppelde schijven verwijderen voor de virtuele machines die zijn verwijderd.
* **Force** (optioneel): instelling om af te dwingen offline HPC-knooppunten voordat u ze verwijdert.
* **Bevestig** (optioneel): vragen om bevestiging voordat de opdracht wordt uitgevoerd.
* **WhatIf**: de instelling voor het beschrijven van wat er gebeurt als u de opdracht uitvoert zonder de opdracht daadwerkelijk wordt uitgevoerd.

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld zorgt ervoor dat offline de knooppunten met namen die beginnen *HPCNode-CN -* en ze verwijdert u de knooppunten en hun gekoppelde schijven.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Rekenknooppunt virtuele machines starten
Start de rekenknooppunten met de **Start HpcIaaSNode.ps1** script.

### <a name="syntax"></a>Syntaxis
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameters
* **Naam**: namen van de clusterknooppunten worden gestart. Jokertekens worden ondersteund. De naam van de parameter is Name. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.
* **Knooppunt**-object voor de knooppunten om te worden gestart, die kunnen worden verkregen via de HPC PowerShell-cmdlet de HpcNode [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld begint knooppunten met namen die beginnen *HPCNode-CN -*.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Rekenknooppunt virtuele machines stoppen
Stoppen van rekenknooppunten met de **Stop HpcIaaSNode.ps1** script.

### <a name="syntax"></a>Syntaxis
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameters
* **Naam**-namen van de clusterknooppunten moet worden gestopt. Jokertekens worden ondersteund. De naam van de parameter is Name. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.
* **Knooppunt**: de HpcNode-object voor de knooppunten om te stoppen, die kunnen worden verkregen via de HPC PowerShell-cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). De naam van de parameter is een knooppunt. U kunt niet beide opgeven de **naam** en **knooppunt** parameters.
* **Force** (optioneel): instelling om af te dwingen offline HPC-knooppunten voordat u ze stopt.

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld zorgt ervoor dat offline knooppunten met namen die beginnen *HPCNode-CN -* en stopt de knooppunten.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor het automatisch vergroten of verkleinen van de clusterknooppunten volgens de huidige werkbelasting van jobs en taken op het cluster, [automatisch vergroten of verkleinen van de clusterbronnen HPC Pack in Azure volgens de clusterbelasting](hpcpack-cluster-node-autogrowshrink.md).


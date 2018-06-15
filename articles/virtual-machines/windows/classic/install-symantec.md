---
title: Symantec Endpoint Protection installeren op een Windows virtuele machine in Azure | Microsoft Docs
description: Informatie over het installeren en configureren van de uitbreiding van de beveiliging Symantec Endpoint Protection op een nieuwe of bestaande Azure virtuele machine gemaakt met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: bd96cb975bfc30b2561a98a950a9dd3fc060fa54
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30913338"
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Symantec Endpoint Protection installeren en configureren op een Windows VM
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

In dit artikel laat zien hoe installeren en configureren van de Symantec Endpoint Protection-client op een bestaande virtuele machine (VM) met Windows Server. Deze volledige client omvat services zoals virussen en bescherming tegen spyware, firewall en inbraakdetectie te voorkomen. De client wordt geïnstalleerd als een uitbreiding van de beveiliging met behulp van de VM-Agent.

Als u een bestaand abonnement van Symantec voor een on-premises oplossing hebt, kunt u het beveiligen van uw virtuele machines in Azure. Als u niet een klant nog, kunt u zich aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing [Symantec Endpoint Protection op van Microsoft Azure-platform][Symantec]. Deze pagina bevat ook koppelingen naar licentie-informatie en instructies voor het installeren van de client als u al een Symantec-klant bent.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection installeren op een bestaande virtuele machine
Voordat u begint, moet u het volgende:

* De Azure PowerShell-module, versie 0.8.2 of hoger op de computer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de **Get-Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell][PS]. Aanmelden bij uw Azure-abonnement met `Add-AzureAccount`.
* De VM-Agent op de virtuele Machine van Azure wordt uitgevoerd.

Controleer eerst of dat de VM-Agent al is geïnstalleerd op de virtuele machine. Vul de cloudservicenaam en de naam van virtuele machine en voer de volgende opdrachten bij een beheerdersniveau Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens.

> [!TIP]
> Als u de cloudservice en de namen van de virtuele machine niet weet, voert u **Get-AzureVM** voor een lijst met de namen voor alle virtuele machines in uw huidige abonnement.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Als de **write-host** opdracht geeft **True**, de VM-Agent is geïnstalleerd. Als deze wordt weergegeven in **False**, Zie de instructies en een koppeling naar de download in de Azure blogbericht [VM-Agent en -extensies: Part 2][Agent].

Als de VM-Agent is geïnstalleerd en voer deze opdrachten de Symantec Endpoint Protection-agent te installeren.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Controleren of de uitbreiding van de beveiliging Symantec is geïnstalleerd en bijgewerkt is:

1. Meld u bij de virtuele machine. Zie voor instructies [hoe meld u aan bij een virtuele Machine Running Windows Server][Logon].
2. Voor Windows Server 2008 R2, klikt u op **Start > Symantec Endpoint Protection**. Typ voor Windows Server 2012 of Windows Server 2012 R2, vanuit het startscherm **Symantec**, en klik vervolgens op **Symantec Endpoint Protection**.
3. Van de **Status** tabblad van de **Status Symantec Endpoint Protection** venster updates toepassen of opnieuw opstarten indien nodig.

## <a name="additional-resources"></a>Aanvullende resources
[Hoe meld u aan bij een virtuele Machine met WindowsServer][Logon]

[Azure VM-extensies en functies][Ext]

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493

---
title: Symantec Endpoint Protection installeren op een Windows-VM in Azure | Microsoft Docs
description: Informatie over het installeren en configureren van de Symantec Endpoint Protection-security-extensie op een nieuwe of bestaande Azure-VM gemaakt met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz
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
ms.author: roiyz
ms.openlocfilehash: 65b52c88741e618e8048451370918b06db73a651
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58014419"
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Symantec Endpoint Protection installeren en configureren op een Windows VM
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Dit artikel ziet u hoe u kunt installeren en configureren van de Symantec Endpoint Protection-client op een bestaande virtuele machine (VM) met Windows Server. Deze volledige client bevat-services zoals virus- en bescherming tegen spyware-, firewall- en inbraakdetectiehandtekeningen preventie. De client is geïnstalleerd als een beveiligingsuitbreiding van met behulp van de VM-Agent.

Als u een bestaand abonnement van Symantec voor een on-premises oplossing hebt, kunt u het beveiligen van uw Azure virtual machines. Als u nog niet een klant bent, kunt u zich aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing [Symantec Endpoint Protection op van Microsoft Azure-platform][Symantec]. Deze pagina bevat ook koppelingen naar de licentie-informatie en instructies voor het installeren van de client als u al een Symantec-klant bent.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection installeren op een bestaande virtuele machine
Voordat u begint, moet u het volgende:

* De Azure PowerShell-module, versie 0.8.2 of hoger: Schakel op de computer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de **Get-Module-azure | format-table versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell][PS]. Meld u aan bij uw Azure-abonnement met `Add-AzureAccount`.
* De VM-Agent op de Azure-machine.

Controleer eerst of dat de VM-Agent al is geïnstalleerd op de virtuele machine. Vul in de cloud service-naam en de naam van de virtuele machine en voer de volgende opdrachten in een beheerder op serverniveau Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens in, met inbegrip van de < en > tekens.

> [!TIP]
> Als u niet weet de service in de cloud en de namen van virtuele machines wat, voert u **Get-AzureVM** om de namen voor alle virtuele machines in uw huidige abonnement weer te geven.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Als de **write-host** opdracht geeft **waar**, de VM-Agent is geïnstalleerd. Als deze wordt weergegeven **False**, Zie de instructies en een koppeling naar de download in het Azure-blog-bericht [VM-Agent en -extensies - deel 2][Agent].

Als de VM-Agent is geïnstalleerd, moet u deze opdrachten voor het installeren van de Symantec Endpoint Protection-agent uitvoeren.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Controleren of de Symantec-security-extensie is geïnstalleerd en bijgewerkt is:

1. Meld u aan de virtuele machine. Zie voor instructies [aanmelden bij een virtuele Machine Running Windows Server][Logon].
2. Voor Windows Server 2008 R2, klikt u op **Start > Symantec Endpoint Protection**. Typ voor Windows Server 2012 of Windows Server 2012 R2, vanuit het startscherm **Symantec**, en klik vervolgens op **Symantec Endpoint Protection**.
3. Uit de **Status** tabblad van de **Status Symantec Endpoint Protection** venster updates toepassen of opnieuw opstarten indien nodig.

## <a name="additional-resources"></a>Aanvullende resources
[Aanmelden bij een virtuele Machine met WindowsServer][Logon]

[Azure VM-extensies en functies][Ext]

<!--Link references-->
[Symantec]: https://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:../windows/classic/tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: https://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:../windows/classic/connect-logon.md

[Ext]: https://go.microsoft.com/fwlink/p/?linkid=390493

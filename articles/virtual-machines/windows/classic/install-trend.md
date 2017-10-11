---
title: Trend Micro grondige Security installeren op een virtuele machine | Microsoft Docs
description: In dit artikel wordt beschreven hoe installeren en configureren van Trend Micro beveiliging op een virtuele machine gemaakt met het klassieke implementatiemodel in Azure.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: 911b8f12472dcbda3e6bfeb8c97bf1d04a63e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Trend Micro Deep Security installeren en configureren als een service op een Windows VM
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

In dit artikel leest u hoe installeren en configureren van Trend Micro grondige Security als een Service op een nieuwe of bestaande virtuele machine (VM) met Windows Server. Uitgebreide beveiliging als een Service omvat anti-malwarebeveiliging, een firewall, een inbraakdetectie preventie van systeem en integriteit bewaken.

De client is als een uitbreiding van beveiliging via de VM-Agent geïnstalleerd. Op een nieuwe virtuele machine installeren u de beveiligingsagent grondige als de VM-Agent automatisch door de Azure-portal gemaakt wordt.

Een bestaande virtuele machine gemaakt met behulp van de klassieke portal, de Azure CLI of PowerShell hebben mogelijk niet een VM-agent. Voor een bestaande virtuele machine die niet de VM-Agent, die u wilt downloaden en installeer het eerst. In dit artikel komen beide situaties.

Als u een abonnement van de Trend Micro voor een on-premises oplossing hebt, kunt u het beveiligen van uw virtuele machines in Azure. Als u niet een klant nog, kunt u zich aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing het trendanalyse Micro blogbericht [Microsoft Azure VM-Agent-extensie voor grondige Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>De grondige beveiligingsagent installeren op een nieuwe virtuele machine

De [Azure-portal](http://portal.azure.com) kunt u de uitbreiding van de beveiliging Trend Micro installeren wanneer u een installatiekopie van het **Marketplace** voor het maken van de virtuele machine. Als u één virtuele machine maakt, is met behulp van de portal een eenvoudige manier om de beveiliging van de Trend Micro toevoegen.

Met behulp van een item in de **Marketplace** een wizard waarmee u kunt van de virtuele machine instellen wordt geopend. U gebruikt de **instellingen** blade, het derde venster van de wizard voor het installeren van de Trend Micro security-extensie.  Raadpleeg voor algemene instructies [maken van een virtuele machine waarop Windows wordt uitgevoerd in de Azure portal](tutorial.md).

Wanneer de **instellingen** blade van de wizard de volgende stappen uitvoeren:

1. Klik op **extensies**, klikt u vervolgens op **-extensie toevoegen** in het volgende venster.

   ![Beginnen met het toevoegen van de extensie][1]

2. Selecteer **grondige beveiligingsagent** in de **nieuwe resource** deelvenster. Klik in het deelvenster grondige beveiligingsagent **maken**.

   ![Grondige beveiligingsagent identificeren][2]

3. Voer de **Tenant-id** en **Tenant activering wachtwoord** voor de extensie. Desgewenst kunt u een **beveiliging beleids-id**. Klik vervolgens op **OK** om toe te voegen van de client.

   ![Geef details op extensie][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>De grondige beveiligingsagent installeren op een bestaande virtuele machine
Als u wilt de agent installeert op een bestaande virtuele machine, moet u de volgende items:

* De Azure PowerShell-module, versie 0.8.2 of nieuwer, is geïnstalleerd op uw lokale computer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met behulp van de **Get-Module azure | tabel opmaken versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](/powershell/azure/overview). Aanmelden bij uw Azure-abonnement met `Add-AzureAccount`.
* De VM-Agent is geïnstalleerd op de virtuele doelmachine.

Controleer eerst of de VM-Agent is al geïnstalleerd. Vul de cloudservicenaam en de naam van virtuele machine en voer de volgende opdrachten bij een beheerdersniveau Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als u de cloudservice en de naam van de virtuele machine niet weet, voert u **Get-AzureVM** om weer te geven die gegevens voor alle virtuele machines in uw huidige abonnement.

Als de **write-host** opdracht retourneert **True**, de VM-Agent is geïnstalleerd. Als het resultaat **False**, Zie de instructies en een koppeling naar de download in de Azure blogbericht [VM-Agent en -extensies: Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Als de VM-Agent is geïnstalleerd, moet u deze opdrachten uitvoeren.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
Het duurt enkele minuten duren voordat de agent wilt uitvoeren wanneer deze is geïnstalleerd. Daarna moet u grondige beveiliging op de virtuele machine activeren zodat deze kan worden beheerd door een grondige Security Manager. Zie de volgende artikelen voor aanvullende instructies:

* Trend van artikel over deze oplossing [Instant-On Cloud Security voor Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* Een [Windows PowerShell-voorbeeldscript](http://go.microsoft.com/fwlink/?LinkId=404100) voor het configureren van de virtuele machine
* [Instructies](http://go.microsoft.com/fwlink/?LinkId=404099) voor het voorbeeld

## <a name="additional-resources"></a>Aanvullende bronnen
[Aanmelden met een virtuele machine met Windows Server]

[Azure VM-extensies en functies]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Aanmelden met een virtuele machine met Windows Server]:connect-logon.md
[Azure VM-extensies en functies]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

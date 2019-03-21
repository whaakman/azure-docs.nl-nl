---
title: Trend Micro Deep Security installeren op een virtuele machine | Microsoft Docs
description: In dit artikel wordt beschreven hoe u installeren en configureren van Trend Micro beveiliging op een virtuele machine gemaakt met het klassieke implementatiemodel in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: roiyz
ms.openlocfilehash: 0bb49d3d75b88f197a42e02f03f89480563537ef
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005474"
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Trend Micro Deep Security installeren en configureren als een service op een Windows VM
[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]
Dit artikel leest u hoe om te installeren en configureren van Trend Micro Deep Security als een Service op een nieuwe of bestaande virtuele machine (VM) met Windows Server. Deep Security als een Service bevat anti-malwarebeveiliging, een firewall, een inbraakpreventiesysteem en bestandsintegriteit controleren.

De client is geïnstalleerd als een beveiligingsuitbreiding van via de VM-Agent. Op een nieuwe virtuele machine maakt installeren u de Deep Security-Agent als de VM-Agent automatisch door de Azure-portal gemaakt wordt.

Een bestaande virtuele machine gemaakt met behulp van Azure portal, de Azure CLI of PowerShell wellicht een VM-agent. Voor een bestaande virtuele machine waarop de VM-Agent niet is, moet u download en installeer deze eerst. In dit artikel bevat informatie over beide situaties.

Als u een abonnement van Trend Micro voor een on-premises oplossing hebt, kunt u het beveiligen van uw Azure virtual machines. Als u nog niet een klant bent, kunt u zich aanmelden voor een proefabonnement. Zie voor meer informatie over deze oplossing, de Trend Micro blogbericht [Microsoft Azure VM Agent-extensie voor Deep Security](https://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>De Deep Security-Agent installeren op een nieuwe virtuele machine

De [Azure-portal](https://portal.azure.com) kunt u de Trend Micro-security-extensie installeren wanneer u een installatiekopie van het **Marketplace** te maken van de virtuele machine. Als u één virtuele machine maakt, is met behulp van de portal een eenvoudige manier om de beveiliging van Trend Micro toevoegen.

Met behulp van een item in de **Marketplace** een wizard waarmee u bij het helpt van de virtuele machine instellen wordt geopend. U gebruikt de **instellingen** blade, het derde venster van de wizard voor het installeren van de beveiligingsuitbreiding Trend Micro.  Raadpleeg voor algemene instructies [maken van een virtuele machine waarop Windows wordt uitgevoerd in Azure portal](../windows/classic/tutorial.md).

Wanneer de **instellingen** blade van de wizard, voer de volgende stappen uit:

1. Klik op **extensies**, klikt u vervolgens op **-extensie toevoegen** in het volgende deelvenster.

   ![Beginnen met het toevoegen van de extensie][1]

2. Selecteer **Deep Security Agent** in de **nieuwe resource** deelvenster. Klik in het deelvenster Deep Security Agent **maken**.

   ![Identificeren van Deep Security-Agent][2]

3. Voer de **Tenant-id** en **activeringswachtwoord voor de Tenant** voor de extensie. Desgewenst kunt u een **Security beleids-id**. Klik vervolgens op **OK** om toe te voegen van de client.

   ![Geef details van de extensie][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>De Deep Security-Agent installeren op een bestaande virtuele machine
Voor het installeren van de agent op een bestaande virtuele machine, moet u de volgende items:

* De Azure PowerShell-module, versie 0.8.2 of nieuwer, is geïnstalleerd op uw lokale computer. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met behulp van de **Get-Module-azure | format-table versie** opdracht. Zie voor instructies en een koppeling naar de nieuwste versie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview). Meld u aan bij uw Azure-abonnement met `Add-AzureAccount`.
* De VM-Agent geïnstalleerd op de virtuele doelmachine.

Controleer eerst of dat de VM-Agent al is geïnstalleerd. Vul in de cloud service-naam en de naam van de virtuele machine en voer de volgende opdrachten in een beheerder op serverniveau Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens in, met inbegrip van de < en > tekens.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Als u niet weet de service in de cloud en de naam van de virtuele machine wat, voert u **Get-AzureVM** om die gegevens voor alle virtuele machines in uw huidige abonnement weer te geven.

Als de **write-host** opdracht retourneert **waar**, de VM-Agent is geïnstalleerd. Als het resultaat **False**, Zie de instructies en een koppeling naar de download in het Azure-blog-bericht [VM-Agent en -extensies - deel 2](https://go.microsoft.com/fwlink/p/?LinkId=403947).

Als de VM-Agent is geïnstalleerd, moet u deze opdrachten uitvoeren.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
Het duurt enkele minuten om de agent te starten met het uitvoeren als deze is geïnstalleerd. Hierna moet u Deep Security activeren op de virtuele machine, zodat deze kan worden beheerd door een Deep Security Manager. Zie de volgende artikelen voor aanvullende instructies:

* Trend van artikel over deze oplossing [Instant-On Cloud Security for Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=404101)
* Een [Windows PowerShell-script voorbeeld](https://go.microsoft.com/fwlink/?LinkId=404100) aan de virtuele machine configureren
* [Instructies](https://go.microsoft.com/fwlink/?LinkId=404099) voor het voorbeeld

## <a name="additional-resources"></a>Aanvullende resources
[Hoe u aan te melden met een virtuele machine met Windows Server]

[Azure VM-extensies en functies]

<!-- Image references -->
[1]: ./media/trend/new_vm_Blade3.png
[2]: ./media/trend/find_SecurityAgent.png
[3]: ./media/trend/SecurityAgentDetails.png

<!-- Link references -->
[Hoe u aan te melden met een virtuele machine met Windows Server]:../windows/classic/connect-logon.md
[Azure VM-extensies en functies]: https://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

---
title: Configureren van WinRM na het maken van virtuele Azure-machine | Microsoft Docs
description: Hoe het configureren van Windows Remote Management (WinRM) na het maken van een virtuele machine van Azure worden gehost.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: pbutlerm
ms.openlocfilehash: 6350d5bde737c46cf14a9aef75a7ec57260a6afa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53197141"
---
# <a name="configure-winrm-after-virtual-machine-creation"></a>Configureren van WinRM na het maken van virtuele machines

In dit artikel wordt uitgelegd hoe het configureren van een bestaande Azure gehoste virtuele machine (VM) om in te schakelen WinRM via HTTPS.  Deze configuratie is van toepassing alleen op Windows gebaseerde virtuele machines en het volgende proces voor verificatie in twee stappen vereist:

1. Netwerkverkeer via de poort voor de WinRM via HTTPS-protocol inschakelen.  U configureert deze instelling voor uw virtuele machine in Azure portal.
2. Configureer de virtuele machine om in te schakelen WinRM door het uitvoeren van de opgegeven PowerShell-scripts.


## <a name="enabling-port-traffic"></a>Inschakelen van netwerkverkeer via de poort

De WinRM via HTTPS-protocol gebruikt poort 5896, die niet standaard op de vooraf geconfigureerde Windows VM's die worden aangeboden via Azure Marketplace is ingeschakeld. Als wilt inschakelen dit protocol, gebruikt u de volgende stappen uit in een nieuwe regel toevoegen aan de netwerkbeveiligingsgroep (NSG) met de [Azure-portal](https://portal.azure.com).  Zie voor meer informatie over nsg's [beveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview).

1.  Navigeer naar de blade **virtuele machines >**  <*vm-naam*>  **> Instellingen/netwerken**.
2.  Klik op de naam van de NSG (in dit voorbeeld **testvm11002**) om de eigenschappen ervan weer te geven:

    ![Network security-groepseigenschappen](./media/nsg-properties.png)
 
3. Onder **instellingen**, selecteer **inkomende beveiligingsregels** om deze blade weer te geven.
4. Klik op **+ toevoegen** te maken van een nieuwe regel met de naam `WinRM_HTTPS` voor TCP-poort 5986.

    ![Inkomende beveiligingsregel toevoegen](./media/nsg-new-rule.png)

5. Klik op **OK** wanneer u bent klaar waarden opgeven.  De lijst met regels voor binnenkomende beveiliging moet de volgende nieuwe vermeldingen bevatten.

    ![Lijst met regels voor binnenkomende network security](./media/nsg-new-inbound-listing.png)


## <a name="configure-vm-to-enable-winrm"></a>Virtuele machine om in te schakelen WinRM configureren 

Gebruik de volgende stappen uit om te schakelen en de functie Windows Extern beheer configureren op uw Windows-VM.   

1. Een extern bureaublad verbinding met uw VM wordt gehost op Azure.  Zie voor meer informatie, [hoe u verbinding maken met en meld u aan met een Azure-machine waarop Windows wordt uitgevoerd](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).  De resterende stappen wordt uitgevoerd op de virtuele machine.
2. De volgende bestanden worden gedownload en deze opslaan naar een map op de virtuele machine:
    - [ConfigureWinRM.ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1)
    - [makecert.exe](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe)
    - [winrmconf.cmd](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd)
3. Open de **PowerShell-Console** met verhoogde bevoegdheden (**als Administrator uitvoeren**). 
4. Voer de volgende opdracht, waarbij u de vereiste parameter opgeeft: de volledig gekwalificeerde domeinnaam (FQDN) voor uw virtuele machine: <br/>
   `ConfigureWinRM.ps1 <vm-domain-name>`

    ![PowerShell-configuratiescript 1](./media/powershell-file1.png)

    Met dit script is afhankelijk van de andere twee bestanden worden in dezelfde map.


## <a name="next-steps"></a>Volgende stappen

Als u WinRM hebt geconfigureerd, bent u klaar om te [implementeren van uw VM op basis van de bijbehorende VHD's](./cpp-deploy-vm-vhd.md).

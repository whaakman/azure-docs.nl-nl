---
title: Azure Quick Start - Een Windows VM CLI maken | Microsoft Docs
description: Ontdek snel hoe u virtuele Windows-machines maakt met de Azure CLI.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5939af4cf071664415d57ccbaee3413d61aa69e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-cli"></a>Een virtuele Windows-machine maken met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze handleiding staat informatie over het gebruik van de Azure CLI om een virtuele machine te implementeren met Windows Server 2016. Als de implementatie is voltooid, maken we verbinding met de server en installeren we ISS.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep maken met [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). 

In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt. In dit voorbeeld wordt *azureuser* voor de naam van een gebruiker met beheerdersrechten en *myPassword12* als het wachtwoord gebruikt. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving. Deze waarden zijn nodig als u verbinding maakt met de virtuele machine.

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

Wanneer de virtuele machine is gemaakt, toont de Azure CLI informatie die lijkt op de informatie in het volgende voorbeeld. Noteer het `publicIpAaddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Standaard worden alleen RDP-verbindingen toegestaan naar virtuele Windows-machines die zijn geïmplementeerd in Azure. Als deze virtuele machine wordt gebruikt als een webserver, moet u poort 80 openen voor verkeer vanaf internet. Gebruik de opdracht [az vm open-port](/cli/azure/vm#open-port) om de gewenste poort te openen.  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om een sessie met een extern bureaublad te starten voor de virtuele machine. Vervang het IP-adres door het openbare IP-adres van de virtuele machine. Wanneer u hierom wordt gevraagd, typt u de referenties die zijn gebruikt bij het maken van de virtuele machine.

```bash 
mstsc /v:Public IP Address
```

## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

U bent nu aangemeld bij de VM van Azure en er is nog maar één regel code van PowerShell nodig om IIS te installeren en de regel voor de lokale firewall in te schakelen om webverkeer toe te staan. Open een PowerShell-prompt en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het openbare IP-adres dat u hierboven hebt gedocumenteerd. 

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze Snel starten hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkbeveiligingsgroepregel gemaakt en een webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)

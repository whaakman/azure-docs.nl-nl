---
title: 'Snelstart: Een virtuele Windows-machine maken met Azure PowerShell | Microsoft Docs'
description: In deze snelstart leert u hoe u Azure PowerShell gebruikt om een virtuele Windows-machine te maken
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3036c53fa95b40ac0bfc7dbe5bed69581236918d
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411880"
---
# <a name="quickstart-create-a-windows-virtual-machine-with-the-azure-cli"></a>Quickstart: Een virtuele Windows-machine maken met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze snelstart wordt beschreven hoe u de Azure CLI gebruikt voor het implementeren van een virtuele machine (VM) in Azure waarop Windows Server 2016 wordt uitgevoerd. Om uw VM in actie te zien, voert u een externe bureaubladsessie voor de virtuele machine uit en installeert u de IIS-webserver.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.30 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt. In dit voorbeeld wordt *azureuser* voor de naam van een gebruiker met beheerdersrechten en *myPassword12* als het wachtwoord gebruikt. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving. Deze waarden zijn nodig wanneer u verbinding maakt met de virtuele machine.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. In het volgende voorbeeld van uitvoer ziet u dat het maken van de virtuele machine is geslaagd.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

Noteer uw eigen `publicIpAddress` in de uitvoer van uw virtuele machine. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stappen.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Standaard worden alleen verbindingen met een extern bureaublad geopend wanneer u een virtuele Windows-machine in Azure maakt. Gebruik [az vm open-port](/cli/azure/vm#az_vm_open_port) om TCP-poort 80 te openen voor gebruik met de IIS-webserver:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om een sessie met een extern bureaublad te starten vanaf uw lokale computer. Vervang het IP-adres door het openbare IP-adres van de virtuele machine. Wanneer u hierom wordt gevraagd, voert u de referenties in die zijn gebruikt toen de VM is gemaakt:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Webserver installeren

Als u uw VM in actie wilt zien, installeert u de IIS-webserver. Open een PowerShell-prompt op de virtuele machine en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wanneer u klaar bent, sluit u de externe-bureaubladverbinding met de virtuele machine.

## <a name="view-the-web-server-in-action"></a>De webserver in actie zien

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Gebruik het openbare IP-adres van uw VM dat is verkregen in een vorige stap. In het volgende voorbeeld ziet u de IIS-standaardwebsite:

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkpoort geopend voor internetverkeer en een eenvoudige webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)

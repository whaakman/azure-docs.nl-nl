---
title: Een Windows-machine maken op Azure-Stack met Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele machine van Windows op de Stack van Azure met Azure CLI
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/19/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 5665af14b9b0d0705b68c8a27c593b19c31b053e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="quickstart-create-a-windows-virtual-machine-in-azure-stack-using-azure-cli"></a>Snelstartgids: een virtuele Windows-machine maken in Azure-Stack met Azure CLI

Azure CLI wordt gebruikt voor het maken en beheren van Azure-Stack-bronnen vanaf de opdrachtregel. In dit artikel laat zien hoe Azure CLI gebruiken voor het maken en toegang tot een Windows Server 2016 virtuele machine in Azure-Stack.

## <a name="prerequisites"></a>Vereisten

* Zorg ervoor dat uw Azure-Stack-operator de installatiekopie van het 'WindowsServer 2016' is toegevoegd aan de Stack van Azure marketplace.

* Azure-Stack is vereist voor een specifieke versie van Azure CLI maken en beheren van de resources. Als u geen Azure CLI geconfigureerd voor Azure-Stack, volg de stappen voor [Azure CLI installeren en configureren](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container in welke Azure-Stack resources worden geïmplementeerd en beheerd. Geïntegreerd systeem, uitvoeren van de SDK of de Azure-Stack de [az groep maken](/cli/azure/group#az_group_create) opdracht om een resourcegroep te maken. Waarden voor de variabelen in dit document zijn toegewezen, kunt u deze waarden gebruiken of nieuwe waarden toewijzen. Het volgende voorbeeld maakt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving. Deze waarden zijn nodig om verbinding te maken met de virtuele machine.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Wanneer de virtuele machine wordt gemaakt, de *PublicIPAddress* parameter wordt uitgevoerd. Noteer dit adres omdat u deze toegang krijgt tot de virtuele machine nodig hebt.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Standaard mogen alleen RDP-verbindingen met een Windows virtuele machine is geïmplementeerd in Azure-Stack. Als deze virtuele machine wordt gebruikt als een webserver, moet u poort 80 openen voor verkeer vanaf internet. Gebruik de opdracht [az vm open-port](/cli/azure/vm#open-port) om de gewenste poort te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine

Gebruik de volgende opdracht om een sessie met een extern bureaublad te starten voor de virtuele machine. Vervang het IP-adres door het openbare IP-adres van de virtuele machine. Wanneer u hierom wordt gevraagd, typt u de referenties die zijn gebruikt bij het maken van de virtuele machine.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>IIS installeren met behulp van PowerShell

Na het aanmelden op de virtuele machine in Azure, kunt u één regel PowerShell IIS installeren en activeren van de lokale firewallregel webverkeer toestaan. Open een PowerShell-prompt en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

U kunt een webbrowser van uw keuze om weer te geven van de standaardwelkomstpagina van IIS. Gebruik het openbare IP-adres is beschreven in de vorige sectie de standaardpagina te bezoeken.

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#az_group_delete) opdracht voor het verwijderen van de resourcegroep, de virtuele machine, en alle bijbehorende resources.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U kunt een eenvoudige Windows virtuele machine hebt geïmplementeerd in deze snelstartgids. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

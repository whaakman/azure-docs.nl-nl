---
title: Een Windows-machine maken op Azure-Stack met Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele machine van Windows op de Stack van Azure met Azure CLI
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 36930cc38b6c1933b58651f6c63e7d5d453c447d
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-windows-virtual-machine-on-azure-stack-using-azure-cli"></a>Een Windows-machine maken op Azure-Stack met Azure CLI

Azure CLI wordt gebruikt voor het maken en beheren van Azure-Stack-bronnen vanaf de opdrachtregel. Deze handleiding gegevens met Azure CLI voor het maken van een Windows Server 2016 virtuele machine in Azure-Stack. Zodra de virtuele machine is gemaakt, wordt u verbinding met extern bureaublad, IIS installeren en weergeven van de standaardwebsite. 

## <a name="prerequisites"></a>Vereisten 

* Zorg ervoor dat uw Azure-Stack-operator de installatiekopie van het 'WindowsServer 2016' is toegevoegd aan de Stack van Azure marketplace.  

* Azure-Stack is vereist voor een specifieke versie van Azure CLI maken en beheren van de resources. Als u geen Azure CLI geconfigureerd voor Azure-Stack, volg de stappen voor [Azure CLI installeren en configureren](azure-stack-connect-cli.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container in welke Azure-Stack resources worden geïmplementeerd en beheerd. Geïntegreerd systeem, uitvoeren van de SDK of de Azure-Stack de [az groep maken](/cli/azure/group#create) opdracht om een resourcegroep te maken. We waarden voor alle variabelen in dit document hebt toegewezen, kunt u deze als is of een andere waarde toe te wijzen. Het volgende voorbeeld maakt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord. Werk deze waarden bij met waarden die geschikt zijn voor uw omgeving. Deze waarden zijn nodig om verbinding te maken met de virtuele machine.

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

Wanneer de virtuele machine wordt gemaakt, noteer de *PublicIPAddress* parameter dat wordt uitgevoerd, die u gebruiken wilt voor toegang tot de virtuele machine.
 
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

U bent nu aangemeld bij de VM van Azure en er is nog maar één regel code van PowerShell nodig om IIS te installeren en de regel voor de lokale firewall in te schakelen om webverkeer toe te staan. Open een PowerShell-prompt en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>De welkomstpagina van IIS weergeven

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het openbare IP-adres dat u hierboven hebt gedocumenteerd. 

![Standaardsite van IIS](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png) 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U kunt een eenvoudige Windows virtuele machine hebt geïmplementeerd in deze snelstartgids. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

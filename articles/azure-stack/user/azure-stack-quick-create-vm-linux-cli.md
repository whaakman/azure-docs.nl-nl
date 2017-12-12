---
title: Een virtuele Linux-machine maken met behulp van Azure CLI in Azure-Stack | Microsoft Docs
description: Een virtuele Linux-machine maken met CLI in Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 21F7D599-1FEC-4827-A5C3-06495C5F53A4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: ea0bc72c03c7c51f79b838493eb2f6d3efe4f8f7
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-linux-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Een virtuele Linux-machine maken met behulp van Azure CLI in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

Azure CLI wordt gebruikt voor het maken en beheren van Azure-Stack-bronnen vanaf de opdrachtregel. Deze Quick Start-gegevens met Azure CLI voor het maken van een virtuele Linux-machine in Azure-Stack.  Zodra de virtuele machine is gemaakt, een webserver is geïnstalleerd en poort 80 is geopend voor webverkeer.

## <a name="prerequisites"></a>Vereisten 

* Zorg ervoor dat uw Azure-Stack-operator de installatiekopie van het 'Ubuntu Server 16.04 LTS' is toegevoegd aan de Stack van Azure marketplace. 

* Azure-Stack is vereist voor een specifieke versie van Azure CLI maken en beheren van de resources. Als u geen Azure CLI geconfigureerd voor Azure-Stack, aanmelden bij de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) en volg de stappen voor het [installeren Azure CLI en configureren](azure-stack-connect-cli.md).

* Een openbare SSH-sleutel met de naam id_rsa.pub moet worden gemaakt in de SSH-map van uw Windows-gebruikersprofiel. Zie voor gedetailleerde informatie over het maken van SSH-sleutels [maken van SSH-sleutels op Windows](../../virtual-machines/linux/ssh-from-windows.md). 

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
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Als u klaar is, wordt de opdracht parameters voor de virtuele machine uitvoeren.  Noteer de *PublicIPAddress*, aangezien u deze verbinding maken en beheren van uw virtuele machine gebruiken.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Standaard worden alleen SSH-verbindingen toegestaan naar virtuele Linux-machines die zijn geïmplementeerd in Azure. Als deze virtuele machine wordt gebruikt als een webserver, moet u poort 80 openen voor verkeer vanaf internet. Gebruik de opdracht [az vm open-port](/cli/azure/vm#open-port) om de gewenste poort te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>SSH in uw virtuele machine

Op een systeem waarop SSH is geïnstalleerd, gebruikt u de volgende opdracht om verbinding te maken met de virtuele machine. Als u in Windows werkt, kan [Putty](http://www.putty.org/) worden gebruikt om de verbinding tot stand te brengen. Zorg ervoor dat u met de juiste openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld is het IP-adres 192.168.102.36.

```bash
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>NGINX installeren

Gebruik het volgende bash-script om pakketbronnen bij te werken en het meest recente NGINX-pakket te installeren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven

Nu NGINX is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van NGINX weer te geven. Zorg ervoor dat u de standaardpagina bezoekt met het *publicIpAddress* dat u hierboven hebt gedocumenteerd. 

![Standaardsite van NGINX](./media/azure-stack-quick-create-vm-linux-cli/nginx.png) 

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U kunt een eenvoudige virtuele Linux-machine hebt geïmplementeerd in deze snelstartgids. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).


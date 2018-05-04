---
title: Een virtuele Linux-machine maken met behulp van Azure CLI in Azure-Stack | Microsoft Docs
description: Een virtuele Linux-machine maken met CLI in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 21F7D599-1FEC-4827-A5C3-06495C5F53A4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 90b36183ba32e75e06d434098d26cb10f3736373
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Snelstartgids: een virtuele Linux-server-machine maken met behulp van Azure CLI in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt een Ubuntu Server 16.04 TNS virtuele machine maken met behulp van de Azure CLI. Volg de stappen in dit artikel maken en gebruiken van een virtuele machine. In dit artikel hebt u ook de stappen voor het:

* Verbinding maken met de virtuele machine met een externe client.
* De NGINX-webserver installeren en de standaardstartpagina weergeven.
* Niet-gebruikte resources opschonen.

## <a name="prerequisites"></a>Vereisten

* **Een Linux-installatiekopie in de Stack van Azure marketplace**

   De Stack van Azure marketplace bevat geen standaard een Linux-installatiekopie. Ophalen van de Azure-Stack-operator om te bieden de **Ubuntu Server 16.04 LTS** u moet de installatiekopie. De operator kunt de stappen in de [downloaden marketplace-items van Azure naar Azure Stack](../azure-stack-download-azure-marketplace-item.md) artikel.

* Azure-Stack is vereist voor een specifieke versie van de Azure CLI maken en beheren van de resources. Als u geen Azure CLI geconfigureerd voor Azure-Stack, aanmelden bij de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) en volg de stappen voor het [ Azure CLI installeren en configureren](azure-stack-version-profiles-azurecli2.md).

* Een openbare SSH-sleutel met de naam id_rsa.pub opgeslagen in de SSH-map van uw Windows-gebruikersprofiel. Zie voor gedetailleerde informatie over het maken van SSH-sleutels [maken van SSH-sleutels op Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin u kunt implementeren en beheren van resources voor Azure-Stack. Geïntegreerd systeem, uitvoeren van de SDK of de Azure-Stack de [az groep maken](/cli/azure/group#az_group_create) opdracht om een resourcegroep te maken.

>[!NOTE]
 Waarden zijn voor de variabelen in de codevoorbeelden toegewezen. Als u wilt, kunt u echter nieuwe waarden toewijzen.

Het volgende voorbeeld maakt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord van de gebruiker. Deze waarden te wijzigen in iets dat geschikt is voor uw omgeving.

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

Het openbare IP-adres wordt geretourneerd als de **PublicIpAddress** parameter. Noteer dit adres omdat u deze toegang krijgt tot de virtuele machine nodig hebt.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Omdat deze virtuele machine uitvoeren van de IIS-webserver wilt, moet u poort 80 voor Internet-verkeer te openen. Gebruik de opdracht [az vm open-port](/cli/azure/vm#open-port) om de gewenste poort te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Verbinding maken met de virtuele machine via SSH

Verbinding maken met de virtuele machine vanaf een clientcomputer met SSH geïnstalleerd. Als u op een Windows-client werkt, gebruikt u [Putty](http://www.putty.org/) om de verbinding te maken. Voor verbinding met de virtuele machine, gebruik de volgende opdracht:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>De NGINX-webserver installeren

Om te werken pakket resources en het meest recente NGINX-pakket installeert, voer het volgende script:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven

Met NGINX geïnstalleerd, en poort 80 is geopend op de virtuele machine, kunt u toegang tot de webserver met het openbare IP-adres van de virtuele machine. Open een webbrowser en blader naar ```http://<public IP address>```.

![NGINX web server welkomstpagina](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Resources opschonen

Opschonen van de resources die u niet langer nodig. U kunt de [az groep verwijderen](/cli/azure/group#az_group_delete) opdracht om te verwijderen van deze resources. Voer de volgende opdracht voor het verwijderen van de resourcegroep en alle bijbehorende resources:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een basic virtuele machine van een Linux-server met een webserver geïmplementeerd. Voor meer informatie over virtuele machines van Azure-Stack blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

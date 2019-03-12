---
title: Een Linux-machine maken met behulp van Azure CLI in Azure Stack | Microsoft Docs
description: Een virtuele Linux-machine maken met CLI in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/14/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: b3fbd91ddf06d4515ee55d24ba9330d0af6dc124
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57540249"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Snelstartgids: een virtuele Linux-server-machine maken met behulp van Azure CLI in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt een Ubuntu Server 16.04 LTS virtuele machine maken met behulp van de Azure CLI. Volg de stappen in dit artikel te maken en gebruiken van een virtuele machine. Dit artikel vindt u ook de stappen voor het:

* Verbinding maken met de virtuele machine met een externe client.
* De NGINX-webserver installeren en de standaard-startpagina weergeven.
* Niet-gebruikte resources opschonen.

## <a name="prerequisites"></a>Vereisten

* **Een installatiekopie van Linux in Azure Stack marketplace**

   De Azure Stack marketplace bevat geen standaard een installatiekopie van Linux. Ophalen van de Azure Stack-operators voor de **Ubuntu Server 16.04 LTS** installatiekopie u nodig hebt. De operator kan gebruiken de stappen in de [marketplace-items van Azure naar Azure Stack downloaden](../azure-stack-download-azure-marketplace-item.md) artikel.

* Azure Stack is vereist voor een specifieke versie van de Azure CLI om te maken en beheren van de resources. Als u geen Azure-CLI geconfigureerd voor Azure Stack, aanmelden bij de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of een Windows-externe client als u [verbonden zijn via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) en volg de stappen voor het [ Azure CLI installeren en configureren](azure-stack-version-profiles-azurecli2.md).

* Een openbare SSH-sleutel met de naam id_rsa.pub opgeslagen in de map .SSH van uw Windows-gebruikersprofiel. Zie voor gedetailleerde informatie over het maken van SSH-sleutels [het maken van SSH-sleutels in Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin u kunt implementeren en beheren van Azure Stack-resources. Geïntegreerd systeem, uitvoeren van uw development kit of de Azure Stack de [az-groep maken](/cli/azure/group#az-group-create) opdracht om een resourcegroep te maken.

>[!NOTE]
 Waarden zijn voor de variabelen in de codevoorbeelden toegewezen. U kunt echter nieuwe waarden toewijzen als u wilt.

Het volgende voorbeeld wordt een resourcegroep met de naam myResourceGroup in de lokale locatie.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van de [az vm maken](/cli/azure/vm#az-vm-create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam myVM. In dit voorbeeld wordt Demouser voor de naam van een gebruiker met beheerdersrechten en Demouser@123 als het wachtwoord van de gebruiker. Deze waarden die geschikt is voor uw omgeving te wijzigen.

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

Het openbare IP-adres wordt geretourneerd in de **PublicIpAddress** parameter. Noteer dit adres omdat u deze nodig om toegang tot de virtuele machine.

## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer

Omdat deze virtuele machine wordt gebruikt als de IIS webserver uit te voeren, moet u poort 80 openen voor internetverkeer. Gebruik de opdracht [az vm open-port](/cli/azure/vm) om de gewenste poort te openen.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>SSH gebruiken om te verbinden met de virtuele machine

Vanaf een clientcomputer met SSH is geïnstalleerd, verbinding maken met de virtuele machine. Als u op een Windows-client werkt, gebruikt u [Putty](https://www.putty.org/) om de verbinding te maken. Voor verbinding met de virtuele machine, gebruikt u de volgende opdracht:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>De NGINX-webserver installeren

Als u wilt bijwerken pakketresources en het meest recente NGINX-pakket installeert, voer het volgende script:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>De welkomstpagina van NGINX weergeven

Met NGINX is geïnstalleerd en poort 80 is geopend op uw virtuele machine, kunt u toegang tot de webserver met behulp van het openbare IP-adres van de virtuele machine. Open een webbrowser en Ga naar ```http://<public IP address>```.

![Welkomstpagina van NGINX web server](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de resources die u niet meer nodig. U kunt de [az group delete](/cli/azure/group#az-group-delete) opdracht om deze resources te verwijderen. Als u wilt verwijderen van de resourcegroep en alle bijbehorende resources, moet u de volgende opdracht uitvoeren:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een eenvoudige virtuele machine van een Linux-server met een webserver geïmplementeerd. Voor meer informatie over virtuele machines van Azure Stack, blijven [overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md).

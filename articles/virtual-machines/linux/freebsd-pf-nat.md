---
title: FreeBSD van pakketfilter gebruiken om u te maken van een firewall in Azure | Microsoft Docs
description: Informatie over het implementeren van een NAT-firewall met behulp van FreeBSD PF in Azure.
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: cd777291a1321eabf4efe0d7b9b101f932d9398b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Het gebruik van FreeBSD Packet Filter maken van een veilige firewall in Azure
Dit artikel bevat een NAT-firewall met behulp van de FreeBSD verpakker Filter via Azure Resource Manager-sjabloon voor algemene scenario met web server implementeren.

## <a name="what-is-pf"></a>Wat is PF?
PF (pakketfilter, ook geschreven pf) is een centrale onderdeel van de software voor het gebruik van een BSD gelicentieerde stateful pakketfilter. PF sindsdien snel heeft ontwikkeld en nu heeft verschillende voordelen ten opzichte van andere beschikbare firewalls. NAT (Network Address Translation) wordt PF na één dag, klikt u vervolgens pakketplanner en actieve wachtrijbeheer zijn geïntegreerd in PF, door de ALTQ integreren en waardoor het worden geconfigureerd via de PF configuratie. Functies zoals pfsync en CARP voor failover en redundantie, authpf voor sessieverificatie en ftp-proxy te vereenvoudigen het moeilijk FTP-protocol, gebruik hebt PF. ook uitgebreid Kortom, is PF een krachtige en veelzijdige firewall. 

## <a name="get-started"></a>Aan de slag
Als u geïnteresseerd bent in een beveiligde firewall instellen in de cloud voor uw webservers en vervolgens aan de slag. U kunt ook de scripts in deze Azure Resource Manager-sjabloon gebruikt voor het instellen van uw netwerktopologie toepassen.
De Azure Resource Manager-sjabloon instellen van een FreeBSD virtuele machine die wordt uitgevoerd NAT /redirection met PF en twee FreeBSD virtuele machines met de webserver Nginx geïnstalleerd en geconfigureerd. Naast de NAT voor het uitgaande verkeer van de twee web servers uitvoert, de virtuele machine van NAT/omleiding onderschept HTTP-aanvragen en hen omleidt naar de twee webservers round-robin toewijst. De persoonlijke niet-routeerbare IP-adresruimte 10.0.0.2/24 maakt gebruik van het VNet en kunt u de parameters van de sjabloon. De Azure Resource Manager-sjabloon wordt ook een routetabel gedefinieerd voor het hele VNet, een verzameling van afzonderlijke routes die worden gebruikt is voor het overschrijven van Azure standaardroutes op basis van het IP-doeladres. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Implementeren via Azure CLI
U moet de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login). Maak een resourcegroep maken met [az group create](/cli/azure/group#create). Het volgende voorbeeld wordt een Resourcegroepnaam `myResourceGroup` in de `West US` locatie.

```azurecli
az group create --name myResourceGroup --location westus
```

Vervolgens implementeert u de sjabloon [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) met [az implementatie maken](/cli/azure/group/deployment#create). Download [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) onder hetzelfde pad en uw eigen waarden resource, zoals definiëren `adminPassword`, `networkPrefix`, en `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Na ongeveer 5 minuten, krijgt u de informatie van `"provisioningState": "Succeeded"`. Vervolgens kunt u ssh aan de frontend VM (NAT) of toegang Nginx-webserver in een browser met het openbare IP-adres of FQDN-naam van de frontend VM (NAT). Het volgende voorbeeld worden FQDN-naam en openbare IP-adres dat aan de frontend VM (NAT) toegewezen de `myResourceGroup` resourcegroep. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Volgende stappen
Wilt u voor het instellen van uw eigen NAT in Azure? Open Source gratis maar krachtige? PF is dan een goede keuze. Met behulp van de sjabloon [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), hoeft u slechts vijf minuten de voor het instellen van een NAT-firewall met round-robin van taakverdeling met behulp van FreeBSD PF in Azure voor veelvoorkomende scenario met web server. 

Als u weten van het aanbod van FreeBSD in Azure wilt, raadpleegt u [Inleiding tot FreeBSD op Azure](freebsd-intro-on-azure.md).

Als u meer weten over PF wilt, raadpleegt u [FreeBSD handboek](https://www.freebsd.org/doc/handbook/firewalls-pf.html) of [PF User's Guide](https://www.freebsd.org/doc/handbook/firewalls-pf.html).

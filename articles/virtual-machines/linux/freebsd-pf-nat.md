---
title: Pakketfilter van FreeBSD gebruiken om te maken van een firewall in Azure | Microsoft Docs
description: Meer informatie over het implementeren van een NAT-firewall met behulp van FreeBSD PF in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: KylieLiang
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 8400fa12e8776834f71740f809df18753291601f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992302"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Pakketfilter van FreeBSD gebruiken om u te maken van een beveiligde firewall in Azure
In dit artikel maakt u kennis over het implementeren van een NAT-firewall met behulp van FreeBSD Packer Filter via Azure Resource Manager-sjabloon voor algemene web server-scenario.

## <a name="what-is-pf"></a>Wat is PF?
PF (pakketfilter, pf ook geschreven) is een BSD gelicentieerde stateful pakketfilter, een centraal onderdeel van de software voor het gebruik. PF rechtenbeheer aangezien snel en nu heeft diverse voordelen ten opzichte van andere beschikbare firewalls. NAT (Network Address Translation) wordt PF technologiekosten, klikt u vervolgens pakketplanner en actieve wachtrijbeheer zijn geïntegreerd in PF, door de ALTQ integreren en het worden geconfigureerd via PF van configuratie maken. Functies zoals pfsync en CARP voor failover en redundantie, authpf voor sessieverificatie en ftp-proxy om de netwerkfunctie het moeilijk FTP-protocol, hebben ook uitgebreide PF. Kort gezegd, is PF een krachtige en uitgebreide firewall. 

## <a name="get-started"></a>Aan de slag
Als u geïnteresseerd bent in het instellen van een beveiligde firewall in de cloud voor uw webservers en vervolgens aan de slag. U kunt ook de scripts die in deze Azure Resource Manager-sjabloon voor het instellen van uw netwerktopologie.
De Azure Resource Manager-sjabloon instellen van een FreeBSD virtuele machine die wordt uitgevoerd NAT /redirection gebruik van PF en twee FreeBSD virtuele machines met de Nginx-webserver geïnstalleerd en geconfigureerd. Naast het uitvoeren van NAT voor het uitgaande verkeer van twee web-servers, de NAT/omleiding van virtuele machine worden onderschept HTTP-aanvragen en hen omleidt naar de twee webservers in round robin besturingsaanvraag. Het VNet maakt gebruik van de persoonlijke niet-routeerbare IP-adresruimte 10.0.0.2/24 en u kunt de parameters van de sjabloon wijzigen. De Azure Resource Manager-sjabloon worden ook een routetabel voor de hele VNet dat is een verzameling van afzonderlijke routes hebben prioriteit boven Azure standaardroutes op basis van de IP-doeladres waarmee wordt gedefinieerd. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Implementeren via Azure CLI
U moet de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login). Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). Het volgende voorbeeld wordt een Resourcegroepnaam `myResourceGroup` in de `West US` locatie.

```azurecli
az group create --name myResourceGroup --location westus
```

Vervolgens implementeert u de sjabloon [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) met [az group deployment maken](/cli/azure/group/deployment#az_group_deployment_create). Download [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) onder hetzelfde pad en definieer uw eigen waarden resource, zoals `adminPassword`, `networkPrefix`, en `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Na ongeveer vijf minuten, krijgt u de gegevens van `"provisioningState": "Succeeded"`. Vervolgens kunt u ssh naar het front-end VM (NAT) of de toegang tot Nginx-webserver in een browser met behulp van de openbare IP-adres of FQDN-naam van de front-end VM (NAT). Het volgende voorbeeld worden FQDN-naam en openbare IP-adres aan de front-end VM (NAT toegewezen) in de `myResourceGroup` resourcegroep. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Volgende stappen
Wilt u uw eigen NAT in Azure instellen? Open Source, gratis, maar krachtige? PF is een goede keuze. Met behulp van de sjabloon [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), hoeft u alleen de vijf minuten het instellen van een NAT-firewall met round robin van taakverdeling met behulp van FreeBSD PF in Azure voor veelvoorkomende web server-scenario. 

Als u weten van de aanbieding van FreeBSD in Azure wilt, raadpleegt u [Inleiding tot FreeBSD op Azure](freebsd-intro-on-azure.md).

Als u meer weten over PF wilt, raadpleegt u [FreeBSD handboek voor](https://www.freebsd.org/doc/handbook/firewalls-pf.html) of [PF-gebruikershandleiding](https://www.freebsd.org/doc/handbook/firewalls-pf.html).

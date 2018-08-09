---
title: Azure CLI-voorbeeldscript - Webverkeer beperken | Microsoft Docs
description: 'Azure CLI-voorbeeldscript: maak een toepassingsgateway met een Web Application Firewall en een schaalset voor virtuele machines die gebruikmaakt van OWASP-regels om verkeer te beperken.'
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 76c45c985f95979caf6a00a7d2e3a36d9ec3ff77
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422923"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Webverkeer beperken met de Azure CLI

Dit script maakt een toepassingsgateway met een Web Application Firewall die gebruikmaakt van een schaalset voor virtuele machines die is ingesteld voor back-endservers. De Web Application Firewall beperkt webverkeer op basis van OWASP-regels. Nadat het script is uitgevoerd, kunt u de toepassingsgateway testen met behulp van het openbare IP-adres.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, de toepassingsgateway en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az-net) | Hiermee maakt u een virtueel netwerk. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create) | Hiermee maakt u een subnet in een virtueel netwerk. |
| [az network public-ip create](https://docs.microsoft.com/en-us/cli/azure/network/public-ip?view=azure-cli-latest) | Hiermee maakt u het openbare IP-adres voor de toepassingsgateway. |
| [az network application-gateway create](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway?view=azure-cli-latest) | Maak een toepassingsgateway. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Hiermee maakt u een schaalset voor virtuele machines. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Hiermee maakt u een opslagaccount. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Hiermee maakt u een opslagaccount. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network_public_ip_show) | Hiermee haalt u het openbare IP-adres van de toepassingsgateway op. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor toepassingsgateways kunnen worden gevonden in [Documentatie over Azure Application Gateway](../cli-samples.md).

---
title: Maken, wijzigen of verwijderen van een virtueel netwerk-TAP - Azure CLI | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een virtueel netwerk met de Azure CLI te TIKKEN.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/18/2018
ms.author: kaanan
ms.openlocfilehash: 3d95a9ea555cceda82530eb5c487eeb993c1a678
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188557"
---
# <a name="work-with-a-virtual-network-tap-using-the-azure-cli"></a>Werken met een virtueel netwerk-TAP met de Azure CLI

Met Azure virtual network TAP (Terminal Access Point) kunt u continu stroom netwerkverkeer van uw virtuele machine naar een netwerk-pakket collector of analytics-hulpprogramma. De collector of analytics-hulpprogramma wordt geleverd door een [virtueel netwerkapparaat](https://azure.microsoft.com/solutions/network-appliances/) partner. Zie voor een lijst met oplossingen van partners die zijn gevalideerd om te werken met het virtuele netwerk-TAP's [partneroplossingen](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions). 

## <a name="create-a-virtual-network-tap-resource"></a>Een virtueel netwerk-TAP-resource maken

Lezen [vereisten](virtual-network-tap-overview.md#prerequisites) voordat u een virtueel netwerk-TAP-resource maken. U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de Azure-opdrachtregelinterface (CLI) van uw computer. De Azure Cloud Shell is een gratis interactieve shell, waarvoor de Azure CLI installeren op uw computer. U moet zich aanmelden bij Azure met een account met de juiste [machtigingen](virtual-network-tap-overview.md#permissions). In dit artikel gebruikmaken van Azure CLI versie 2.0.46 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Virtueel netwerk-TAP is momenteel beschikbaar als een uitbreiding. Voor het installeren van de extensie die u wilt uitvoeren `az extension add -n virtual-network-tap`. Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

1. De ID van uw abonnement in een variabele die wordt gebruikt in een latere stap ophalen:

   ```azurecli-interactive
   subscriptionId=$(az account show \
   --query id \
   --out tsv)
   ```

2. Stel de abonnements-id die u gebruiken wilt om te maken van een virtueel netwerk-TAP-resource.

   ```azurecli-interactive
   az account set --subscription $subscriptionId
   ```

3. De abonnements-ID die u gebruiken gaat om te maken van een virtueel netwerk-TAP-resource opnieuw te registreren. Als u een registratie-fout optreedt wanneer u een Tik op een resource maakt, moet u de volgende opdracht uitvoeren:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Network --subscription $subscriptionId
   ```

4. Als het doel voor het virtuele netwerk, tikt u op de netwerkinterface op het virtuele netwerkapparaat voor collector of analyseprogramma-

   - Haal de IP-adresconfiguratie van de virtueel-netwerkapparaat van de netwerkinterface in een variabele die wordt gebruikt in een latere stap. De ID is het eindpunt wordt samenvoegen van het verkeer te TIKKEN. Het volgende voorbeeld wordt de ID van de *ipconfig1* IP-configuratie voor een netwerkinterface met de naam *myNetworkInterface*, in een resourcegroep met de naam *myResourceGroup*:

      ```azurecli-interactive
       IpConfigId=$(az network nic ip-config show \
       --name ipconfig1 \
       --nic-name myNetworkInterface \
       --resource-group myResourceGroup \
       --query id \
       --out tsv)
      ```

   - Maak het virtuele netwerk te TIKKEN in westcentralus azure-regio met behulp van de ID van de IP-configuratie als de bestemming en een optionele eigenschap. De poort bevat de doelpoort op IP-configuratie voor de netwerkinterface waar het verkeer te TIKKEN wordt ontvangen:  

      ```azurecli-interactive
       az network vnet tap create \
       --resource-group myResourceGroup \
       --name myTap \
       --destination $IpConfigId \
       --port 4789 \
       --location westcentralus
      ```

5. Als de bestemming voor het virtuele netwerk op TIKT is een interne azure load balancer:
  
   - Ophalen van de front-end IP-adresconfiguratie van de interne Azure load balancer in een variabele die wordt gebruikt in een latere stap. De ID is het eindpunt wordt samenvoegen van het verkeer te TIKKEN. Het volgende voorbeeld wordt de ID van de *frontendipconfig1* -front-end-IP-configuratie voor een load balancer met de naam *myInternalLoadBalancer*, in een resourcegroep met de naam  *myResourceGroup*:

      ```azurecli-interactive
      FrontendIpConfigId=$(az network lb frontend-ip show \
      --name frontendipconfig1 \
      --lb-name myInternalLoadBalancer \
      --resource-group myResourceGroup \
      --query id \
      --out tsv)
      ```
   - Maak het virtuele netwerk met behulp van de ID van de front-end-IP-configuratie als de bestemming en een optionele eigenschap te TIKKEN. De poort bevat de doelpoort op front-end-IP-configuratie waar het verkeer te TIKKEN wordt ontvangen:  

      ```azurecli-interactive
      az network vnet tap create \
      --resource-group myResourceGroup \
      --name myTap \
      --destination $FrontendIpConfigId \
      --port 4789 \
     --location westcentralus
     ```

6. Controleer of het maken van het virtuele netwerk te TIKKEN:

   ```azurecli-interactive
   az network vnet tap show \
   --resource-group myResourceGroup
   --name myTap
   ```

## <a name="add-a-tap-configuration-to-a-network-interface"></a>Een Tik-configuratie toevoegen aan een netwerkinterface

1. De ID van een bestaand virtueel netwerk Tik resource ophalen. Het volgende voorbeeld wordt een virtueel netwerk met de naam te TIKKEN *myTap* in een resourcegroep met de naam *myResourceGroup*:

   ```azurecli-interactive
   tapId=$(az network vnet tap show \
   --name myTap \
   --resource-group myResourceGroup \
   --query id \
   --out tsv)
   ```

2. Maak een Tik-configuratie op de netwerkinterface van de bewaakte virtuele machine. Het volgende voorbeeld wordt een Tik-configuratie voor een netwerkinterface met de naam *myNetworkInterface*:

   ```azurecli-interactive
   az network nic vtap-config create \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --vnet-tap $tapId \
   --name mytapconfig \
   --subscription subscriptionId
   ```

3. Controleer of het maken van de configuratie te TIKKEN:

   ```azurecli-interactive
   az network nic vtap-config show \
   --resource-group myResourceGroup \
   --nic-name myNetworkInterface \
   --name mytapconfig \
   --subscription subscriptionId
   ```

## <a name="delete-the-tap-configuration-on-a-network-interface"></a>Verwijderen van de configuratie te TIKKEN op een netwerkinterface

   ```azure-cli-interactive
   az network nic vtap-config delete \
   --resource-group myResourceGroup \
   --nic myNetworkInterface \
   --name myTapConfig \
   --subscription subscriptionId
   ```

## <a name="list-virtual-network-taps-in-a-subscription"></a>Lijst met virtuele netwerk op tikt in een abonnement

   ```azurecli-interactive
   az network vnet tap list
   ```

## <a name="delete-a-virtual-network-tap-in-a-resource-group"></a>Een virtueel netwerk-TAP in een resourcegroep verwijderen

   ```azurecli-interactive
   az network vnet tap delete \
   --resource-group myResourceGroup \
   --name myTap
   ```

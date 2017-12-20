---
title: Een interne load balancer maken - Azure CLI | Microsoft Docs
description: Informatie over hoe u met Azure CLI een interne load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 920ddecbf81296fd83606f2908e432f5327d4b7e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Een interne load balancer maken met behulp van Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sjabloon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>De oplossing implementeren met Azure CLI

De volgende stappen laten zien hoe u Azure Resource Manager gebruikt om een internetgerichte load balancer te maken met behulp van CLI. Met Azure Resource Manager wordt elke resource afzonderlijk gemaakt en geconfigureerd, en vervolgens samengevoegd om een resource te maken.

U moet de volgende objecten maken en configureren om een load balancer te implementeren:

* **Front-end-IP-configuratie**: bevat openbare IP-adressen voor inkomend netwerkverkeer
* **Back-endadresgroep**: bevat netwerkinterfaces (NIC's) waardoor de virtuele machines netwerkverkeer kunnen ontvangen van de load balancer
* **Regels voor taakverdeling**: bevat regels die een openbare poort op de load balancer toewijst aan een poort in de back-endadresgroep
* **Inkomende NAT-regels**: bevat regels die een openbare poort op de load balancer toewijst aan een poort voor een specifieke virtuele machine in de back-endadresgroep
* **Tests**: bevat statuscontroles die worden gebruikt om de beschikbaarheid van exemplaren van virtuele machines in de back-endadresgroep te controleren

Zie [Azure Resource Manager-ondersteuning voor load balancer](load-balancer-arm.md) voor meer informatie.

## <a name="set-up-cli-to-use-resource-manager"></a>CLI instellen voor het gebruik van Resource Manager

1. Zie [Install and configure the Azure CLI](../cli-install-nodejs.md) (Azure CLI installeren en configureren) als u Azure CLI nog nooit hebt gebruikt. Volg de instructies tot het punt waar u uw Azure-account en -abonnement selecteert.
2. Voer de opdracht **azure config mode** als volgt uit om over te schakelen naar de modus Resource Manager.

    ```azurecli
    azure config mode arm
    ```

    Verwachte uitvoer:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Stapsgewijs een interne load balancer maken

1. Meld u aan bij Azure.

    ```azurecli
    azure login
    ```

    Voer uw Azure-referenties in wanneer dit wordt gevraagd.

2. Wijzig de opdrachthulpprogramma's in de modus Azure Resource Manager.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Alle resources in Azure Resource Manager zijn gekoppeld aan een resourcegroep. Maak een resourcegroep als u dit nog niet hebt gedaan.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Een interne load balancer-set maken

1. Een interne load balancer maken

    In het volgende scenario wordt een resourcegroep met de naam nrprg gemaakt in de regio VS - oost.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Alle resources voor een interne load balancer, zoals virtuele netwerken en virtuele subnetten van netwerken, moeten zich in dezelfde resourcegroep en in dezelfde regio bevinden.

2. Maak een front-end-IP-adres voor de interne load balancer.

    Het IP-adres dat u gebruikt, moet zich binnen het subnetbereik van het virtuele netwerk bevinden.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Maak de back-endadresgroep.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Nadat u een front-end-IP-adres en een back-endadresgroep hebt gedefinieerd, kunt u load balancer-regels, inkomende NAT-regels en aangepaste statuscontroles maken.

4. Maak een load balancer-regel voor de interne load balancer.

    Wanneer u de vorige stappen hebt gevolgd, maakt de opdracht een load balancer-regel voor het luisteren naar poort 1433 in de front-endpool en verzendt deze netwerkverkeer met taakverdeling naar de back-endadresgroep, waarbij ook gebruik wordt gemaakt van poort 1433.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Maak inkomende NAT-regels.

    Inkomende NAT-regels worden gebruikt om eindpunten in een load balancer te maken die naar een specifiek exemplaar van een virtuele machine gaan. Met de vorige stappen zijn twee NAT-regels voor een extern bureaublad gemaakt.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Maak statuscontroles voor de load balancer.

    Een statuscontrole controleert alle exemplaren van de virtuele machines om ervoor te zorgen dat deze netwerkverkeer kunnen verzenden. Het exemplaar van een virtuele machine met mislukte testcontroles wordt uit de load balancer verwijderd totdat deze weer online komt en een testcontrole bepaalt of deze in orde is.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Het Microsoft Azure-platform gebruikt een statisch, openbaar routeerbaar IPv4-adres voor diverse beheerscenario's. Het IP-adres is 168.63.129.16. Dit IP-adres mag niet door firewalls worden geblokkeerd. Dit kan onverwacht gedrag veroorzaken.
    > Wat Azure interne taakverdeling betreft, wordt dit IP-adres gebruikt door bewakingstests van de load balancer om de status van virtuele machines in een set met gelijke taakverdeling te bepalen. Als er een netwerkbeveiligingsgroep wordt gebruikt om het verkeer naar virtuele Azure-machines in een set met intern gelijke taakverdeling te beperken of wordt toegepast op een subnet in een virtueel netwerk, zorg er dan voor dat er een netwerkbeveiligingsregel wordt toegevoegd om verkeer van 168.63.129.16 toe te staan.

## <a name="create-nics"></a>NIC's maken

U moet NIC's maken (of bestaande wijzigen) en deze koppelen aan NAT-regels, load balancer-regels en tests.

1. Maak een NIC met de naam *lb-nic1-be*, en koppel deze vervolgens aan de NAT-regel *rdp1* en de back-endadresgroep *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    Verwachte uitvoer:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Maak een NIC met de naam *lb-nic2-be*, en koppel deze vervolgens aan de NAT-regel *rdp2* en de back-endadresgroep *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Maak een virtuele machine met de naam *DB1* en koppel deze vervolgens aan de NIC met de naam *lb-nic1-be*. Voordat onderstaande opdracht wordt uitgevoerd, wordt er een opslagaccount met de naam *web1nrp* gemaakt.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Virtuele machines in een load balancer moeten zich in dezelfde beschikbaarheidsset bevinden. Gebruik `azure availset create` om een beschikbaarheidsset te maken.

4. Maak een virtuele machine (VM) met de naam *DB2* en koppel deze vervolgens aan de NIC met de naam *lb-nic2-be*. Voordat onderstaande opdracht wordt uitgevoerd, wordt er een opslagaccount met de naam *web1nrp* gemaakt.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Een load balancer verwijderen

Gebruik de volgende opdracht om een load balancer te verwijderen:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Volgende stappen

[Een distributiemodus voor load balancer configureren met bron-IP-affiniteit](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)


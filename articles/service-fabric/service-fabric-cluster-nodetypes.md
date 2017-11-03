---
title: Azure Service Fabric-knooppunttypen en de virtuele machine schalen sets | Microsoft Docs
description: Meer informatie over hoe Azure Service Fabric-knooppunt typen hebben betrekking op de virtuele-machineschaalset wordt ingesteld en op afstand verbinding maken met een schaal exemplaar of clusterknooppunt.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 2bd3053d645d9acd4850fddf7f27237ff954e8c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-knooppunttypen en de virtuele machine sets schalen
Virtuele-machineschaalsets zijn een Azure compute-resource. U kunt schaalsets gebruiken om te implementeren en beheren van een verzameling van virtuele machines als een set. Stel een afzonderlijke scale ingesteld voor elk knooppunttype dat u in een Azure Service Fabric-cluster opgeeft. U kunt onafhankelijk elk knooppunttype omhoog of omlaag schalen, hebben verschillende sets van poorten openen en gebruiken van andere capaciteitsmetrieken.

De volgende afbeelding ziet een cluster met twee knooppunttypen met de naam FrontEnd en BackEnd. Elk knooppunttype heeft vijf knooppunten.

![Een cluster met twee knooppunttypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>VM-scale set instanties worden toegewezen aan knooppunten
Zoals u in de voorgaande afbeelding, scale set instanties op exemplaar 0 start en vervolgens te verhogen door 1. De nummering wordt doorgevoerd in de knooppuntnamen. Bijvoorbeeld, is knooppunt BackEnd_0 exemplaar 0 van de back-end-scale set. Deze bepaalde schaalset heeft vijf exemplaren, met de naam BackEnd_0, BackEnd_1 BackEnd_2, BackEnd_3 en BackEnd_4.

Wanneer u een schaalset opschalen, wordt een nieuw exemplaar gemaakt. De nieuwe scale set-exemplaarnaam is meestal dat de schaal naam plus het volgende instantienummer ingesteld. In ons voorbeeld is het BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Scale set netwerktaakverdelers worden toegewezen aan knooppunttypen en sets schalen
Als u uw cluster in de Azure portal geïmplementeerd of de steekproef Azure Resource Manager-sjabloon gebruikt, worden alle bronnen van een resourcegroep worden weergegeven. Hier ziet u de load balancers voor elk scale set of een knooppunt. De naam van de load balancer gebruikt de volgende indeling: **LB -&lt;naam knooppunttype&gt;**. Een voorbeeld is de Load Balancer-sfcluster4doc-0, zoals wordt weergegeven in de volgende afbeelding:

![Resources][Resources]
## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele machine scale set-exemplaar of een clusterknooppunt
Stel een afzonderlijke scale ingesteld voor elk knooppunt dat u hebt gedefinieerd in een cluster. U kunt de knooppunttypen onafhankelijk omhoog of omlaag schalen. U kunt ook andere VM-SKU's gebruiken. In tegenstelling tot de single instance virtuele machines hebben niet scale set exemplaren hun eigen virtuele IP-adressen. Dit kan lastig zijn wanneer u zoekt een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar.

Ga voor een IP-adres en de poort die u gebruiken kunt voor externe verbinding met een specifiek exemplaar, moet u de volgende stappen uitvoeren.

**Stap 1**: het virtuele IP-adres vinden voor het knooppunttype met het ophalen van de binnenkomende NAT-regels voor Remote Desktop Protocol (RDP).

Eerst ophalen van de binnenkomende NAT-regels waarden die zijn gedefinieerd als onderdeel van de resourcedefinitie voor `Microsoft.Network/loadBalancers`.

Selecteer in de Azure-portal op de pagina load balancer **instellingen** > **binnenkomende NAT-regels**. Hiermee krijgt u het IP-adres en poort die u gebruiken kunt voor externe verbinding met de eerste schaal instantie ingesteld. 

![Load balancer][LBBlade]

In de volgende afbeelding, het IP-adres en poort zijn **104.42.106.156** en **3389**.

![NAT-regels][NATRules]

**Stap 2**: de poort die u gebruiken kunt voor externe verbinding met de specifieke scale set exemplaar of het knooppunt niet vinden.

Schaalset exemplaren kaart aan knooppunten. Gebruik de informatie van de set schalen om te bepalen van de exacte poort te gebruiken.

Poorten zijn toegewezen in oplopende volgorde die overeenkomt met het exemplaar van de set schaal. Voor het vorige voorbeeld van het type FrontEnd-knooppunt in de volgende tabel geeft een lijst van de poorten voor elk van de exemplaren van de vijf knooppunten. Dezelfde toewijzing van toepassing op uw scale set-exemplaar.

| **Virtuele-machineschaalset exemplaar** | **Poort** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

**Stap 3**: extern verbinding maken met het specifieke scale set-exemplaar.

De volgende afbeelding ziet verbinding met extern bureaublad verbinding maken met de FrontEnd_1 scale set exemplaar:

![Verbinding met extern bureaublad][RDP]

## <a name="change-the-rdp-port-range-values"></a>Wijzig de waarden voor het RDP-poort

### <a name="before-cluster-deployment"></a>Voordat de implementatie van het cluster
Bij het instellen van het cluster met een Resource Manager-sjabloon, geef het bereik in `inboundNatPools`.

Ga naar de resourcedefinitie voor `Microsoft.Network/loadBalancers`. Zoek de beschrijving voor de `inboundNatPools`.  Vervang de `frontendPortRangeStart` en `frontendPortRangeEnd` waarden.

![inboundNatPools waarden][InboundNatPools]

### <a name="after-cluster-deployment"></a>Na implementatie van het cluster
De waarden voor het RDP-poort wijzigen na de implementatie van het cluster is complexer. Om ervoor te zorgen dat u de virtuele machines niet recyclen, gebruikt u Azure PowerShell nieuwe waarden in te stellen. 

> [!NOTE]
> Zorg ervoor dat u Azure PowerShell 1.0 of hoger op uw computer geïnstalleerd. Als u geen Azure Powershell 1.0 of hoger hebt, raden wij aan dat u de stappen beschreven in [hoe Azure PowerShell installeren en configureren.](/powershell/azure/overview)

1. Aanmelden bij uw Azure-account. Als de volgende PowerShell-opdracht is mislukt, controleert u of PowerShell juist is geïnstalleerd.

    ```
    Login-AzureRmAccount
    ```

2. Voor meer informatie over de load balancer en de waarden voor de beschrijving van `inboundNatPools`, voert u de volgende code:

    ```
    Get-AzureRmResource -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
    ```

3. Stel `frontendPortRangeEnd` en `frontendPortRangeStart` op de waarden die u wilt.

    ```
    $PropertiesObject = @{
        #Property = value;
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name> -ApiVersion <use the API version that is returned> -Force
    ```

## <a name="change-the-rdp-user-name-and-password-for-nodes"></a>De RDP-gebruikersnaam en wachtwoord voor knooppunten wijzigen

Het wachtwoord wilt wijzigen voor alle knooppunten van een specifiek knooppunttype, moet u de volgende stappen uitvoeren. Deze wijzigingen worden toegepast op alle huidige en toekomstige knooppunten in de schaalset.

1. Open PowerShell als beheerder. 
2. Als u wilt aanmelden en uw abonnement voor de sessie te selecteren, moet u de volgende opdrachten uitvoeren. Wijzig de `SUBSCRIPTIONID` -parameter voor uw abonnement-ID. 

    ```powershell
    Login-AzureRmAccount
    Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
    ```

3. Voer het volgende script. Gebruik de relevante `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME`, en `PASSWORD` waarden. De `USERNAME` en `PASSWORD` waarden zijn de nieuwe referenties in de toekomst RDP-sessies te gebruiken. 

    ```powershell
    $nodeTypeName = 'NODETYPENAME'
    $resourceGroup = 'RESOURCEGROUP'
    $publicConfig = @{'UserName' = 'USERNAME'}
    $privateConfig = @{'Password' = 'PASSWORD'}
    $extName = 'VMAccessAgent'
    $publisher = 'Microsoft.Compute'
    $node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
    $node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

    Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
    ```

## <a name="next-steps"></a>Volgende stappen
* Zie de [overzicht van de functie 'Overal implementeren' en een vergelijking met Azure beheerde clusters](service-fabric-deploy-anywhere.md).
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* Meer informatie over de [Service Fabric SDK en aan de slag](service-fabric-get-started.md).

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png

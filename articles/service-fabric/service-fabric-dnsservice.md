---
title: Azure Service Fabric-DNS-service | Microsoft Docs
description: Gebruik Service Fabric van DNS-service voor het detecteren van microservices van binnen het cluster.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-Service in Azure Service Fabric
De DNS-Service is een optionele systeemservice die u in uw cluster inschakelen kunt voor het detecteren van andere services met behulp van het DNS-protocol.

Veel services, vooral beperkte services, kunnen een bestaande URL-naam, en kunnen deze met behulp van de standaard DNS-protocol (in plaats van het protocol Naming Service) op te lossen is het wenselijk zijn, met name in 'lift- en verschuiven'-scenario's. De DNS-service kunt u DNS-namen toewijzen aan een servicenaam en daarom eindpunt IP-adressen omzetten. 

DNS-namen van de DNS-service wordt toegewezen aan servicenamen, op zijn beurt worden opgelost door de Naming Service om te retourneren van het service-eindpunt. De DNS-naam voor de service is opgegeven op het moment van maken. 

![Service-eindpunten][0]

## <a name="enabling-the-dns-service"></a>De DNS-service inschakelen
U moet eerst de DNS-service in het cluster wilt inschakelen. Haal de sjabloon voor het cluster dat u wilt implementeren. Kunt u ofwel de [voorbeeldsjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) of een Resource Manager-sjabloon maken. U kunt de DNS-service met de volgende stappen inschakelen:

1. Controleer of de `apiversion` is ingesteld op `2017-07-01-preview` voor de `Microsoft.ServiceFabric/clusters` resource, en als dat niet bijwerken zoals weergegeven in het volgende fragment:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu de DNS-service inschakelen door het volgende toe te voegen `addonFeatures` sectie na de `fabricSettings` sectie zoals weergegeven in het volgende fragment: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Zodra u de sjabloon voor het cluster hebt bijgewerkt met de voorgaande wijzigingen, ze toepassen en kunt u de upgrade voltooid. Als u klaar is, de DNS-service wordt gestart in het cluster die wordt aangeroepen `fabric:/System/DnsService` onder sectie van de systeem-service in de Service Fabric explorer. 

U kunt ook de DNS-service via de portal inschakelen op het moment van het maken van het cluster. De DNS-service kan worden ingeschakeld door het selectievakje voor `Include DNS service` in de `Cluster configuration` menu zoals weergegeven in de volgende schermafbeelding:

![Het inschakelen van DNS-service via de portal][2]


## <a name="setting-the-dns-name-for-your-service"></a>De DNS-naam voor uw service instellen
Als de DNS-service wordt uitgevoerd in uw cluster, kunt u instellen een DNS-naam voor uw services declaratief voor services die standaard in de `ApplicationManifest.xml` of via Powershell-opdrachten.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Instellen van de DNS-naam voor een standaardservice in de ApplicationManifest.xml
Open het project in Visual Studio of uw favoriete editor en open de `ApplicationManifest.xml` bestand. Ga naar de sectie standaard services en voor elke service toevoegen de `ServiceDnsName` kenmerk. Het volgende voorbeeld laat zien hoe de DNS-naam van de service naar instellen`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Zodra de toepassing is geïmplementeerd, ziet u het service-exemplaar in Service Fabric explorer de DNS-naam voor dit exemplaar, zoals wordt weergegeven in de volgende afbeelding: 

![Service-eindpunten][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Instellen van de DNS-naam voor een service met behulp van Powershell
U kunt de DNS-naam voor een service instellen bij het maken met behulp van de `New-ServiceFabricService` Powershell. Het volgende voorbeeld wordt een nieuwe staatloze service met de DNS-naam`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Met behulp van DNS in uw services
Als u meer dan één service implementeert, kunt u de eindpunten van andere services om te communiceren via een DNS-naam vinden. De DNS-service is alleen van toepassing op stateless services, omdat het DNS-protocol kan niet met stateful services communiceren. Voor stateful services, kunt u de ingebouwde omgekeerde proxy voor http-aanroepen om aan te roepen van een bepaalde service-partitie.

De volgende code toont het aanroepen van een andere service, die is gewoon een gewone http-aanroep waarin u de poort en een optionele pad opgeven als onderdeel van de URL.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over servicecommunicatie binnen het cluster met [verbinding maken en te communiceren met services](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG

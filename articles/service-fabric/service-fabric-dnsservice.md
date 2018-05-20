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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 656aed1f1fbd3294c4318520058ace480fd2219c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-Service in Azure Service Fabric
De DNS-Service is een optionele systeemservice die u in uw cluster inschakelen kunt voor het detecteren van andere services met behulp van het DNS-protocol. 

Veel services, vooral beperkte services, kunnen een bestaande URL-naam, en kunnen deze met behulp van de standaard DNS-protocol (in plaats van het protocol Naming Service) op te lossen is het wenselijk zijn, met name in 'lift- en verschuiven'-scenario's. De DNS-service kunt u DNS-namen toewijzen aan een servicenaam en daarom eindpunt IP-adressen omzetten. 

DNS-namen van de DNS-service wordt toegewezen aan servicenamen, op zijn beurt worden opgelost door de Naming Service om te retourneren van het service-eindpunt. De DNS-naam voor de service is opgegeven op het moment van maken.

![Service-eindpunten](./media/service-fabric-dnsservice/dns.png)

Dynamische poorten worden niet ondersteund door de DNS-service. Gebruik op te lossen services beschikbaar gesteld via dynamische poorten de [reverse proxy-service](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>De DNS-service inschakelen
Wanneer u een cluster met behulp van de portal maakt, de DNS-service is standaard ingeschakeld in de **omvatten DNS-service** selectievakje op het **clusterconfiguratie** menu:

![Het inschakelen van DNS-service via de portal][2]

Als u de portal niet gebruikt voor het maken van het cluster of u een bestaand cluster bijwerkt, moet u de DNS-service in een sjabloon inschakelen:

- Als u wilt een nieuw cluster implementeert, kunt u ofwel de [voorbeeldsjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) of maak een eigen Resource Manager-sjabloon. 
- Voor het bijwerken van een bestaand cluster kunt u navigeren naar de resourcegroep van het cluster op de portal en klik op **automatiseringsscript** werken met een sjabloon die overeenkomt met de huidige status van het cluster en andere bronnen in de groep. Zie voor meer informatie, [de sjabloon exporteren uit resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group).

Nadat u een sjabloon hebt, kunt u de DNS-service met de volgende stappen inschakelen:

1. Controleer of de `apiversion` is ingesteld op `2017-07-01-preview` of hoger voor de `Microsoft.ServiceFabric/clusters` resource, en als dat niet bijwerken zoals weergegeven in het volgende fragment:

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

3. Zodra u de sjabloon voor het cluster hebt bijgewerkt met de voorgaande wijzigingen, ze toepassen en kunt u de upgrade voltooid. Wanneer de upgrade is voltooid, begint de DNS-service in uw cluster wordt uitgevoerd. De servicenaam is `fabric:/System/DnsService`, en u vindt deze onder de **System** sectie in Service Fabric explorer. 


## <a name="setting-the-dns-name-for-your-service"></a>De DNS-naam voor uw service instellen
Als de DNS-service wordt uitgevoerd in uw cluster, kunt u instellen een DNS-naam voor uw services declaratief voor services die standaard in de `ApplicationManifest.xml` of via PowerShell-opdrachten.

De DNS-naam voor uw service kan worden omgezet in het cluster. Het is raadzaam dat u een schematische van `<ServiceDnsName>.<AppInstanceName>`, bijvoorbeeld `service1.application1`. Hiermee zorgt u ervoor de uniekheid van de DNS-naam in het cluster. Als een toepassing wordt geïmplementeerd met behulp van Docker compose, services DNS-namen met behulp van deze schematische automatisch worden toegewezen.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Instellen van de DNS-naam voor een standaardservice in de ApplicationManifest.xml
Open het project in Visual Studio of uw favoriete editor en open de `ApplicationManifest.xml` bestand. Ga naar de sectie standaard services en voor elke service toevoegen de `ServiceDnsName` kenmerk. Het volgende voorbeeld laat zien hoe de DNS-naam van de service naar instellen `service1.application1`

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
U kunt de DNS-naam voor een service instellen bij het maken met behulp van de `New-ServiceFabricService` Powershell. Het volgende voorbeeld wordt een nieuwe staatloze service met de DNS-naam `service1.application1`

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
Als u meer dan één service implementeert, kunt u de eindpunten van andere services om te communiceren via een DNS-naam vinden. De DNS-service is alleen van toepassing op stateless services, omdat het DNS-protocol kan niet met stateful services communiceren. Voor stateful services, kunt u de ingebouwde [reverse proxy-service](./service-fabric-reverseproxy.md) voor HTTP-aanroepen van de partitie van een bepaalde service aanroepen. Dynamische poorten worden niet ondersteund door de DNS-service. U kunt de omgekeerde proxy gebruiken om op te lossen services die gebruikmaken van dynamische poorten.

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

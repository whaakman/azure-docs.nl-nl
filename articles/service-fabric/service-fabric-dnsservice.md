---
title: Azure Service Fabric-DNS-service | Microsoft Docs
description: Gebruik van Service Fabric-dns-service voor het detecteren van microservices van binnen het cluster.
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
ms.date: 7/20/2018
ms.author: msfussell
ms.openlocfilehash: 70ea33f2c3e3a79d1754b72d12e8fc27689e38ea
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387935"
---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-Service in Azure Service Fabric
De DNS-Service is een optionele systeemservice waarmee u in uw cluster inschakelen kunt voor het detecteren van andere services met behulp van het DNS-protocol. 

Veel services, met name services in containers, kunnen worden opgevraagd via een reeds bestaande URL. Kunnen omzetten van deze services met behulp van de standaard DNS-protocol in plaats van het Service Fabric Naming-Service-protocol wordt is wenselijk. De DNS-service kunt u DNS-namen worden toegewezen aan een servicenaam en kan daarom oplossen eindpunt IP-adressen. Deze functionaliteit de draagbaarheid van services in containers worden bewaard in verschillende platforms en kunt u 'lift and shift' scenario's eenvoudiger, doordat u gebruik bestaande service-URL's in plaats van met code te herschrijven als u wilt gebruikmaken van de Naming-Service. 

Servicenamen, die op zijn beurt worden opgelost door de Naming-Service om te retourneren van het service-eindpunt kaarten DNS-namen van de DNS-service. De DNS-naam voor de service wordt geleverd op het moment dat wordt gemaakt. Het volgende diagram laat zien hoe de DNS-service werkt voor stateless services.

![Service-eindpunten](./media/service-fabric-dnsservice/stateless-dns.png)

Beginnen met Service Fabric versie 6.3, is het Service Fabric-DNS-protocol uitgebreid om op te nemen van een schema voor het adresseren van gepartitioneerde stateful services. Deze extensies maken het mogelijk is om op te lossen IP-adressen van een specifieke partitie met behulp van een combinatie van stateful service DNS-naam en de naam van de partitie. Alle drie partitioneringsschema's worden ondersteund:

- Met de naam partitioneren
- Variabele partitioneren
- Singleton-partities

Het volgende diagram laat zien hoe de DNS-service werkt voor gepartitioneerde stateful services.

![stateful service-eindpunten](./media/service-fabric-dnsservice/stateful-dns.png)

Dynamische poorten worden niet ondersteund door de DNS-service. Om op te lossen services beschikbaar gesteld op dynamische poorten, de [reverse proxy-service](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>De DNS-service inschakelen
Wanneer u een cluster met behulp van de portal maakt, de DNS-service is standaard ingeschakeld in de **omvatten DNS-service** selectievakje op de **clusterconfiguratie** menu:

![Het inschakelen van DNS-service via de portal](./media/service-fabric-dnsservice/enable-dns-service.png)

Als u niet de portal gebruikt om uw cluster te maken of als u een bestaand cluster bijwerkt, moet u de DNS-service in een sjabloon inschakelen:

- Als u wilt een nieuw cluster hebt geïmplementeerd, kunt u ofwel de [-voorbeeldsjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) of maak uw eigen Resource Manager-sjabloon. 
- Voor het bijwerken van een bestaand cluster, kunt u navigeren naar de resourcegroep van het cluster in de portal en klik op **automatiseringsscript** te werken met een sjabloon die overeenkomt met de huidige status van het cluster en andere resources in de groep. Zie voor meer informatie, [de sjabloon exporteren uit resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group).

Nadat u een sjabloon hebt, kunt u de DNS-service met de volgende stappen inschakelen:

1. Controleer of de `apiversion` is ingesteld op `2017-07-01-preview` of hoger voor de `Microsoft.ServiceFabric/clusters` resource, en als dat niet het geval is, werken zoals wordt weergegeven in het volgende voorbeeld:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu de DNS-service inschakelen op een van de volgende manieren:

   - Zodat de DNS-service met de standaardinstellingen toe te voegen aan de `addonFeatures` sectie binnen de `properties` sectie zoals wordt weergegeven in het volgende voorbeeld:

       ```json
           "properties": {
              ...

              "addonFeatures": [
                "DnsService"
              ],
              ...
           }
       ```
   - Toevoegen voor het inschakelen van de service met dan standaardinstellingen, een `DnsService` sectie aan de `fabricSettings` sectie binnen de `properties` sectie. In dit geval u niet nodig om toe te voegen aan de DNS-service `addonFeatures`. Zie voor meer informatie over de eigenschappen die kunnen worden ingesteld voor de DNS-Service, [DNS-Service-instellingen](./service-fabric-cluster-fabric-settings.md#dnsservice).

       ```json
           "properties": {
             ...  
             "fabricSettings": [
               ...
               {
                 "name": "DnsService",
                 "parameters": [
                   {
                     "name": "IsEnabled",
                     "value": "true"
                   },
                   {
                     "name": "PartitionSuffix",
                     "value": "--"
                   },
                   {
                     "name": "PartitionPrefix",
                     "value": "--"
                   }
                 ]
               },
               ...
              ]
            }
       ```
1. Zodra u de clustersjabloon met uw wijzigingen bijgewerkt hebt, past deze toe en kunt u de upgrade voltooid. Wanneer de upgrade is voltooid, wordt de DNS-service wordt uitgevoerd in uw cluster. De servicenaam is `fabric:/System/DnsService`, en u vindt deze onder de **System** sectie in Service Fabric explorer. 


## <a name="setting-the-dns-name-for-your-service"></a>De DNS-naam voor uw service instellen
U kunt een DNS-naam voor uw services declaratief voor services die standaard in het bestand ApplicationManifest.xml of instellen via PowerShell-opdrachten.

De DNS-naam voor uw service wordt omgezet in het cluster, dus is het belangrijk om ervoor te zorgen dat de DNS-naam in het cluster. 

Het is raadzaam dat u een schema voor naamgeving van `<ServiceDnsName>.<AppInstanceName>`, bijvoorbeeld `service1.application1`. Als een toepassing wordt geïmplementeerd met behulp van Docker compose, services worden automatisch toegewezen met behulp van dit schema voor naamgeving van DNS-namen.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>De DNS-naam voor een standaardservice instellen in de ApplicationManifest.xml
Open uw project in Visual Studio, of uw favoriete editor en open het bestand ApplicationManifest.xml. Ga naar de sectie van de standaard-services en voor elke service toevoegen de `ServiceDnsName` kenmerk. Het volgende voorbeeld laat zien hoe het instellen van de DNS-naam van de service `service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
      <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
```
Wanneer de toepassing is geïmplementeerd, ziet u de service-exemplaar in de Service Fabric explorer de DNS-naam voor dit exemplaar, zoals wordt weergegeven in de volgende afbeelding: 

![Service-eindpunten](./media/service-fabric-dnsservice/service-fabric-explorer-dns.png)

Het volgende voorbeeld wordt de DNS-naam voor een stateful service `statefulsvc.app`. De service maakt gebruik van een benoemde partitieschema. U ziet dat de partitienamen van de kleine letters. Dit is een vereiste voor partities die worden opgenomen in de DNS-query's; Zie voor meer informatie, [maken van DNS-query's op een stateful service partitie](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#preview-making-dns-queries-on-a-stateful-service-partition).

```xml
    <Service Name="Stateful1" ServiceDnsName="statefulsvc.app" />
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="2" MinReplicaSetSize="2">
        <NamedPartition>
         <Partition Name="partition1" />
         <Partition Name="partition2" />
        </NamedPartition>
      </StatefulService>
    </Service>
```

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Instellen van de DNS-naam voor een service met behulp van Powershell
U kunt de DNS-naam voor een service instellen bij het maken van met behulp van de `New-ServiceFabricService` Powershell-opdracht. Het volgende voorbeeld wordt een nieuwe stateless service met de DNS-naam `service1.application1`

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

## <a name="preview-making-dns-queries-on-a-stateful-service-partition"></a>[Preview] DNS-query's maken op de servicepartitie van een stateful
Beginnen met Service Fabric versie 6.3, de Service Fabric-DNS-service biedt ondersteuning voor query's voor servicepartities.

Voor de partities die worden gebruikt voor DNS-query's, de volgende naamsbeperkingen zijn van toepassing:

   - Partitienamen moet voldoen aan het DNS-beleid.
   - Meerdere label partitienamen (die punt bevatten '.', in de naam) mogen niet worden gebruikt.
   - Partitienamen moeten kleine letters.

DNS-query's die zijn gericht op een partitie zijn als volgt ingedeeld:

```
    <First-Label-Of-Partitioned-Service-DNSName><PartitionPrefix><Target-Partition-Name>< PartitionSuffix>.<Remaining- Partitioned-Service-DNSName>
```
Waar:

- *First-Label-Of-Partitioned-Service-DNSName* is het eerste deel van de DNS-naam van uw service.
- *PartitionPrefix* is een waarde die kan worden ingesteld in de sectie de DNS-service van het clustermanifest of door middel van het cluster Resource Manager-sjabloon. De standaardwaarde is '-'. Zie voor meer informatie, [DNS-Service-instellingen](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Doel partitienaam* is de naam van de partitie. 
- *PartitionSuffix* is een waarde die kan worden ingesteld in de sectie de DNS-service van het clustermanifest of door middel van het cluster Resource Manager-sjabloon. De standaardwaarde is een lege tekenreeks. Zie voor meer informatie, [DNS-Service-instellingen](./service-fabric-cluster-fabric-settings.md#dnsservice).
- *Resterende-gepartitioneerd-Service-DNSName* is het resterende gedeelte van de DNS-naam van uw service.

De volgende voorbeelden ziet u DNS-query's voor gepartitioneerde services die worden uitgevoerd op een cluster met standaardinstellingen voor `PartitionPrefix` en `PartitionSuffix`: 

- Om op te lossen '0'-partitie van een service met de DNS-naam `backendrangedschemesvc.application` die gebruikmaakt van een ranged partitieschema, gebruikt u `backendrangedschemesvc-0.application`.
- Om op te lossen van de partitie 'first' van een service met DNS-naam `backendnamedschemesvc.application` die gebruikmaakt van een benoemde partitieschema, gebruikt u `backendnamedschemesvc-first.application`.

De DNS-service wordt het IP-adres van de primaire replica van de partitie. Als er geen partitie is opgegeven, retourneert de service het IP-adres van de primaire replica van een willekeurig geselecteerde partitie.

## <a name="using-dns-in-your-services"></a>Met behulp van DNS in uw services
Als u meer dan één service implementeert, kunt u de eindpunten van andere services om te communiceren met met behulp van een DNS-naam vinden. De DNS-service werkt voor stateless services en, in Service Fabric versie 6.3 en hoger voor stateful services. Voor stateful services die worden uitgevoerd op eerdere versies van Service Fabric 6.3, kunt u de ingebouwde [reverse proxy-service](./service-fabric-reverseproxy.md) voor http-aanroepen voor het aanroepen van de partitie van een bepaalde service. 

Dynamische poorten worden niet ondersteund door de DNS-service. U kunt de reverse proxy-service gebruiken om op te lossen van de services die gebruikmaken van dynamische poorten.

De volgende code toont hoe u aan te roepen een stateless service via DNS. Het is gewoon een gewone http-aanroep waar u de DNS-naam, de poort en eventuele optionele pad opgeven als onderdeel van de URL.

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

De volgende code toont een oproep op een specifieke partitie van een stateful service. In dit geval bevat de naam van de DNS-naam van de partitie (Partitie1). De aanroep wordt ervan uitgegaan dat een cluster met de standaardwaarden voor de `PartitionPrefix` en `PartitionSuffix`.

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
            Uri uri = new Uri("http://service2-partition1.application1:8080/api/values");
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

## <a name="known-issues"></a>Bekende problemen
* Voor Service Fabric versie 6.3 en hoger is er een probleem met DNS-zoekacties voor de servicenamen met een koppelteken in de DNS-naam. Volgen voor meer informatie over dit probleem, de volgende [GitHub-probleem](https://github.com/Azure/service-fabric-issues/issues/1197). Een oplossing voor dit wordt binnenkort in de volgende 6.3 update. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over communicatie binnen het cluster met [verbinding maken en te communiceren met services](service-fabric-connect-and-communicate-with-services.md)


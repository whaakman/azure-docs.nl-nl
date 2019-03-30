---
title: Implementatie van Azure Service Fabric-toepassing | Microsoft Docs
description: Gebruik de APIs FabricClient om te implementeren en verwijderen van toepassingen in Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: aljo
ms.openlocfilehash: 408ef5abeed238a2bf4437bea0b77b6768961f53
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661153"
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Implementeren en verwijderen van toepassingen met behulp van FabricClient
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Zodra een [toepassingstype is ingepakt][10], deze gereed is voor implementatie in een Azure Service Fabric-cluster. Implementatie omvat de volgende drie stappen:

1. Het toepassingspakket uploaden naar de installatiekopieopslag
2. Het toepassingstype te registreren
3. Het toepassingspakket verwijderen uit de installatiekopieopslag
4. Exemplaar van de toepassing maken

Nadat een toepassing wordt geïmplementeerd en een exemplaar in het cluster wordt uitgevoerd, kunt u een exemplaar van de toepassing en het bijbehorende toepassingstype verwijderen. Als u wilt een toepassing volledig te verwijderen uit het cluster omvat de volgende stappen uit:

1. Verwijderen (of verwijderen) op de actieve toepassingsexemplaar
2. Registratie van het toepassingstype ongedaan maken als u deze niet meer nodig hebt

Als u Visual Studio gebruikt voor het implementeren en opsporen van fouten in toepassingen op uw lokale ontwikkelcluster, worden automatisch de voorgaande stappen verwerkt via een PowerShell-script.  Met dit script wordt gevonden de *Scripts* map van het project voor een toepassing. In dit artikel bevat achtergrondinformatie over wat dit script doet zodat u dezelfde bewerkingen buiten Visual Studio kunt uitvoeren. 
 
## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Verbinding maken met het cluster met het maken van een [FabricClient](/dotnet/api/system.fabric.fabricclient) exemplaar voordat u een van de codevoorbeelden in dit artikel uitvoert. Voor voorbeelden van het verbinding maken met een lokaal ontwikkelcluster of een extern cluster of het cluster is beveiligd met behulp van Azure Active Directory, X509 certificaten of raadpleegt u de Active Directory Windows [verbinding maken met een beveiligd cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). Voor verbinding met het lokale ontwikkelcluster, voert u het volgende uit:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Het toepassingspakket uploaden
Stel dat u maken en een toepassing met de naam *MyApplication* in Visual Studio. Standaard is de naam van het toepassingen die worden vermeld in de ApplicationManifest.xml 'MyApplicationType'.  De, dit toepassingspakket de benodigde toepassingsmanifest, servicemanifesten en code/config/gegevenspakketten bevat, bevindt zich in *C:\Users\&lt; gebruikersnaam&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Uploaden van het toepassingspakket is ondergebracht in een locatie die toegankelijk is voor de interne Service Fabric-onderdelen. Service Fabric wordt gecontroleerd of het toepassingspakket tijdens de registratie van het toepassingspakket. Als u wilt controleren of het toepassingspakket lokaal (dat wil zeggen, voordat het uploaden van), gebruikt de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

De [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API het toepassingspakket uploadt naar de installatiekopieopslag van het cluster. 

Als het toepassingspakket groot is en/of veel bestanden, kunt u [comprimeert](service-fabric-package-apps.md#compress-a-package) en kopieer het naar het archief van de installatiekopie met behulp van PowerShell. De compressie vermindert de grootte en het aantal bestanden.

Zie [inzicht in de verbindingsreeks van de installatiekopie-store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de installatiekopieopslag en de installatiekopie te slaan.

## <a name="register-the-application-package"></a>Registreren van het toepassingspakket
Het toepassingstype en de versie is gedeclareerd in het manifest van de toepassing beschikbaar voor gebruik wanneer het toepassingspakket is geregistreerd. Het systeem leest het pakket in de vorige stap hebt geüpload, controleert of het pakket, de inhoud van het pakket verwerkt en de verwerkte pakket worden gekopieerd naar de locatie van een intern systeem.  

De [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registreert de toepassing Typ in het cluster en het toegankelijk maken voor implementatie.

De [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API bevat informatie over alle soorten toepassingen is geregistreerd. U kunt deze API gebruiken om te bepalen wanneer de registratie is voltooid.

## <a name="remove-an-application-package-from-the-image-store"></a>Een toepassingspakket verwijderen uit de installatiekopieopslag
Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de registratie van de toepassing is geslaagd.  Toepassingspakketten verwijderen uit de installatiekopieopslag vrijgemaakt systeemresources.  Niet-gebruikte toepassingspakketten disk-opslag verbruikt en leidt tot prestatieproblemen van toepassing. Het toepassingspakket wissen van de installatiekopie store via de [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="create-an-application-instance"></a>Exemplaar van een toepassing maken
U kunt instantiëren vanuit elk toepassingstype dat is geregistreerd met behulp van een toepassing de [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. De naam van elke toepassing moet beginnen met de *' fabric: "* -schema en moet uniek zijn voor elk exemplaar (binnen een cluster). Standaardservices die zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing worden ook gemaakt.

Meerdere exemplaren van een toepassing kunnen worden gemaakt voor een bepaalde versie van het type van een geregistreerde toepassing. Elk exemplaar geïsoleerd, met een eigen werkmap en een reeks van processen wordt uitgevoerd.

Om te zien die met de naam toepassingen en services worden uitgevoerd in het cluster, voert u de [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) en [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API's.

## <a name="create-a-service-instance"></a>Een service-exemplaar maken
U kunt exemplaar maken van een service van een service met behulp van de [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Als de service is gedeclareerd als een standaardservice in het toepassingsmanifest, wordt de service wordt gestart wanneer de toepassing wordt gestart.  Aanroepen van de [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API voor een service die al wordt geïnstantieerd een uitzondering van het type FabricException met een foutcode met een waarde van FabricErrorCode.ServiceAlreadyExists wordt geretourneerd.

## <a name="remove-a-service-instance"></a>Een service-exemplaar verwijderen
Wanneer een service-exemplaar niet meer nodig hebt is, kunt u deze verwijderen uit de actieve exemplaar van de toepassing door het aanroepen van de [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Met deze bewerking kan niet ongedaan worden gemaakt en de status van service kan niet worden hersteld.

## <a name="remove-an-application-instance"></a>Exemplaar van een toepassing verwijderen
Wanneer een exemplaar van de toepassing niet meer nodig hebt is, kunt u permanent verwijderen deze met behulp van de naam van de [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) worden alle services die deel uitmaken van de toepassing ook, permanent verwijderen van de status van alle service automatisch verwijderd.

> [!WARNING]
> Met deze bewerking kan niet ongedaan worden gemaakt en de status van toepassing kan niet worden hersteld.

## <a name="unregister-an-application-type"></a>Registratie van een toepassingstype
Wanneer een bepaalde versie van een toepassingstype niet meer nodig hebt is, moet u wel een registratie ongedaan maken die bepaalde versie van de toepassing met behulp van de [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. Registratie van niet-gebruikte versies van toepassingstypen opslagruimte die wordt gebruikt door de installatiekopieopslag worden vrijgegeven. Een versie van een toepassingstype kan ongedaan worden, zolang er zijn geen toepassingen zijn gemaakt op basis van die versie van het toepassingstype en geen in behandeling toepassingsupgrades verwijzen naar deze versie van het toepassingstype.

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiëren-ServiceFabricApplicationPackage vraagt om een ImageStoreConnectionString
De Service Fabric-SDK-omgeving moet de juiste standaardwaarden instellen al hebben. Maar indien nodig, de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die door de Service Fabric-cluster wordt gebruikt. U kunt de ImageStoreConnectionString vinden in het clustermanifest opgehaald met de [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) en Get-ImageStoreConnectionStringFromClusterManifest opdrachten:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel van de Service Fabric SDK PowerShell-module uitmaakt, wordt gebruikt om op te halen van de verbindingsreeks van de installatiekopie-store.  Voor het importeren van de SDK-module, voert u de volgende uit:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


De ImageStoreConnectionString is gevonden in het clustermanifest:

```xml
<ClusterManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Zie [inzicht in de verbindingsreeks van de installatiekopie-store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de installatiekopieopslag en de installatiekopie te slaan.

### <a name="deploy-large-application-package"></a>Grote toepassingspakket implementeren
Probleem: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API een time-out optreedt voor een grote toepassing-pakket (volgorde van GB).
Probeer:
- Geef een hogere time-out voor [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) methode, met `timeout` parameter. De time-out is standaard 30 minuten.
- Controleer de netwerkverbinding tussen de broncomputer en de cluster. Als de verbinding langzaam is, kunt u overwegen een machine met een betere netwerkverbinding.
Als de client-computer in een andere regio dan het cluster, kunt u met behulp van een client-computer in een dichter of dezelfde regio als het cluster.
- Controleer als u externe beperking worden bereikt. Wanneer de installatiekopieopslag is geconfigureerd voor gebruik van azure-opslag, kunnen uploaden bijvoorbeeld kan worden beperkt.

Probleem: Uploaden van pakket is voltooid, maar [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API een time-out optreedt. Probeer:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u kopieert naar de installatiekopieopslag.
De compressie vermindert de grootte en het aantal bestanden, die op zijn beurt wordt de hoeveelheid netwerkverkeer en werken die Service Fabric moet uitvoeren. De uploadbewerking mogelijk langzamer (met name als u de tijd compressie opnemen), maar het register en het toepassingstype opheffen van de registratie sneller zijn.
- Geef een hogere time-out voor [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API met `timeout` parameter.

### <a name="deploy-application-package-with-many-files"></a>Toepassingspakket implementeren met veel bestanden
Probleem: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) time-out optreedt voor een toepassingspakket met veel bestanden (volgorde van duizenden).
Probeer:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u kopieert naar de installatiekopieopslag. De compressie vermindert het aantal bestanden.
- Geef een hogere time-out voor [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) met `timeout` parameter.

## <a name="code-example"></a>Voorbeeld van code
Het volgende voorbeeld een toepassingspakket worden gekopieerd naar de installatiekopieopslag, richt het toepassingstype exemplaar van een toepassing maakt, maakt u een service-exemplaar, Hiermee verwijdert u het exemplaar van de toepassing, ongedaan maken-bepalingen voor het toepassingstype en Hiermee verwijdert u de toepassingspakket uit de installatiekopieopslag.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the application package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)

[Introductie van service Fabric-status](service-fabric-health-introduction.md)

[Problemen vaststellen en oplossen van een Service Fabric-service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Een toepassing modelleren in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md

---
title: Implementatie van Azure Service Fabric-toepassing | Microsoft Docs
description: Gebruik de FabricClient APIs te implementeren en toepassingen verwijderen in Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 39d768f08305b8dcc18146b119f5bdc83a8d35d4
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Implementeren en toepassingen die gebruikmaken van FabricClient verwijderen
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Eenmaal een [toepassingstype zijn ingepakt][10], deze gereed is voor implementatie in een Azure Service Fabric-cluster. Implementatie omvat de volgende drie stappen:

1. Het toepassingspakket uploaden naar de image store
2. Het toepassingstype registreren
3. Het toepassingspakket verwijderen uit de image store
4. Maken van het toepassingsexemplaar

Nadat een toepassing wordt geïmplementeerd en een exemplaar in het cluster wordt uitgevoerd, kunt u het toepassingsexemplaar van de en het toepassingstype verwijderen. Omvat de volgende stappen om een toepassing volledig te verwijderen uit het cluster:

1. De uitgevoerd verwijderen (of verwijderen) toepassingsexemplaar
2. Hef de registratie van het toepassingstype als u deze niet langer nodig hebt

Als u Visual Studio gebruikt voor het implementeren en foutopsporing van toepassingen op uw lokale ontwikkeling-cluster, worden automatisch alle voorgaande stappen afgehandeld via een PowerShell-script.  Dit script is gevonden in de *Scripts* map van het toepassingsproject. Dit artikel vindt achtergrond op script doet zodat u dezelfde bewerkingen buiten Visual Studio kunt uitvoeren. 
 
## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Verbinding maken met het cluster met het maken van een [FabricClient](/dotnet/api/system.fabric.fabricclient) exemplaar voordat u een van de codevoorbeelden in dit artikel uitvoert. Voor voorbeelden van verbinding maken met een lokaal ontwikkelcluster of een externe cluster of cluster die zijn beveiligd met Azure Active Directory, X509 certificaten of Windows Active Directory-Zie [verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). Voor verbinding met het lokaal ontwikkelcluster, voert u de volgende:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Het toepassingspakket uploaden
Stel dat u maakt en een toepassing met de naam van het pakket *Mijntoepassing* in Visual Studio. Standaard is de naam van het toepassingstype die worden vermeld in de ApplicationManifest.xml 'MyApplicationType'.  Het toepassingspakket dat de benodigde toepassingsmanifest, manifesten service en config-code-gegevens pakketten bevat, bevindt zich in *C:\Users\&lt; gebruikersnaam&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Uploaden van het toepassingspakket plaatst het in een locatie die toegankelijk is voor de interne Service Fabric-onderdelen. Service Fabric controleert of het toepassingspakket tijdens de registratie van het toepassingspakket. Als u wilt controleren of het toepassingspakket lokaal (dat wil zeggen, voordat u uploadt), gebruiken de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

De [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API het toepassingspakket uploadt naar het archief van de cluster-installatiekopie. 

Als het toepassingspakket groot is en/of veel bestanden, kunt u [comprimeert](service-fabric-package-apps.md#compress-a-package) en kopieer het naar het archief van de installatiekopie met behulp van PowerShell. De compressie vermindert de grootte en het aantal bestanden.

Zie [inzicht in de verbindingsreeks van de image store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de image store en de installatiekopie te slaan.

## <a name="register-the-application-package"></a>Registreren van het toepassingspakket
Het toepassingstype en de versie is gedeclareerd in het toepassingsmanifest worden beschikbaar voor gebruik wanneer het toepassingspakket is geregistreerd. Het systeem leest het pakket dat is geüpload in de vorige stap, controleert of het pakket, de inhoud van het pakket verwerkt en de verwerkte pakket gekopieerd naar de locatie van een intern systeemprobleem.  

De [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API registers de toepassing, typt u in het cluster en beschikbaar gesteld voor implementatie.

De [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API bevat informatie over alle toepassingstypen is geregistreerd. U kunt deze API gebruiken om te bepalen wanneer de registratie is uitgevoerd.

## <a name="remove-an-application-package-from-the-image-store"></a>Een toepassingspakket verwijderen uit de image store
Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de toepassing is geregistreerd.  Toepassingspakketten verwijderen uit de image store vrijgemaakt systeembronnen.  Schijfopslag verbruikt en leidt tot een toepassing prestatieproblemen in ongebruikte toepassingspakketten houden. Het toepassingspakket wissen van de image store via de [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="create-an-application-instance"></a>Maak een toepassingsinstantie
U kunt een toepassing vanuit elk toepassingstype dat is geregistreerd met behulp van instantiëren de [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. De naam van elke toepassing moet beginnen met de *"fabric: '* schema en moet uniek zijn voor elk toepassingsexemplaar (binnen een cluster). Standaardservices die zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing worden ook gemaakt.

Meerdere exemplaren van een toepassing kunnen worden gemaakt voor een bepaalde versie van een geregistreerde toepassingstype. Elk toepassingsexemplaar wordt uitgevoerd in een geïsoleerde omgeving met een eigen werkmap en het aantal processen.

Om te zien die met de naam toepassingen en services worden uitgevoerd in het cluster, voer de [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) en [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API's.

## <a name="create-a-service-instance"></a>Een service-exemplaar maken
U kunt een service van een service type met instantiëren de [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Als de service is gedeclareerd als een standaardservice in het toepassingsmanifest, wordt de service wordt gestart wanneer de toepassing wordt gestart.  Het aanroepen van de [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API voor een service die al wordt geïnstantieerd wordt een uitzondering van type met een foutcode met een waarde van FabricErrorCode.ServiceAlreadyExists FabricException geretourneerd.

## <a name="remove-a-service-instance"></a>Verwijderen van een service-exemplaar
Wanneer een service-exemplaar niet langer nodig is, kunt u deze verwijderen uit de uitgevoerd toepassingsexemplaar door het aanroepen van de [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Deze bewerking kan niet ongedaan worden gemaakt en de status van service kan niet worden hersteld.

## <a name="remove-an-application-instance"></a>Verwijderen van instantie van een toepassing
Wanneer een toepassingsexemplaar niet meer nodig is, u kunt definitief wordt verwijderd met behulp van de naam van de [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) alle services die deel uitmaken van de toepassing ook, permanent verwijderen van de status van alle service wordt automatisch verwijderd.

> [!WARNING]
> Deze bewerking kan niet ongedaan worden gemaakt en de status van toepassing kan niet worden hersteld.

## <a name="unregister-an-application-type"></a>Registratie van een toepassingstype
Wanneer een bepaalde versie van het type van een toepassing niet meer nodig is, moet u wel een registratie ongedaan maken die specifieke versie van de toepassing gebruikt de [Unregister ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. De registratie van niet-gebruikte versies van toepassingstypen versies opslagruimte die wordt gebruikt door de image store. Een versie van het type van een toepassing kunt ongedaan worden gemaakt als geen toepassingen zijn gemaakt op basis van die versie van het toepassingstype en er geen updates in behandeling toepassing verwijzen naar die versie van het toepassingstype.

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiëren ServiceFabricApplicationPackage vraagt om een ImageStoreConnectionString
De Service Fabric SDK-omgeving moet al de juiste standaardwaarden instellen. Maar indien nodig, de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die het Service Fabric-cluster wordt gebruikt. U kunt de ImageStoreConnectionString vinden in het clustermanifest opgehaald met de [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) en Get-ImageStoreConnectionStringFromClusterManifest-opdrachten:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel uitmaakt van de Service Fabric SDK PowerShell-module, waarmee de image store-verbindingsreeks ophalen.  Voor het importeren van de SDK-module worden uitgevoerd:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


De ImageStoreConnectionString is gevonden in het clustermanifest:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Zie [inzicht in de verbindingsreeks van de image store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de image store en de installatiekopie te slaan.

### <a name="deploy-large-application-package"></a>Grote toepassingspakket implementeren
Probleem: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API een time-out voor een groot pakket (in volgorde van GB).
Probeer een van:
- Geef een hogere time-out voor [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) methode met `timeout` parameter. Standaard is de time-out voor 30 minuten.
- Controleer de netwerkverbinding tussen de bron- en cluster. Als de verbinding langzaam is, kunt u overwegen een machine met een betere netwerkverbinding.
Als de clientcomputer zich in een andere regio dan het cluster, overweeg het gebruik van een client-computer in een dichter of dezelfde regio als het cluster.
- Controleer als u externe beperking zijn raken. Wanneer de image store is geconfigureerd voor gebruik van azure-opslag, kunnen uploaden bijvoorbeeld kan worden beperkt.

Probleem: Upload-pakket is voltooid, maar [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API een time-out optreedt. Probeer een van:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u naar de image store kopieert.
De compressie vermindert de grootte en het aantal bestanden dat op zijn beurt reduceert de hoeveelheid verkeer van en werken die Service Fabric moet uitvoeren. De uploadbewerking mogelijk langzamer (met name als u de tijd compressie opnemen), maar het register en het toepassingstype ongedaan maken van de registratie zijn sneller.
- Geef een hogere time-out voor [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API met `timeout` parameter.

### <a name="deploy-application-package-with-many-files"></a>Toepassingspakket implementeren met veel bestanden
Probleem: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) time-out voor een toepassingspakket met veel bestanden (volgorde van duizendtallen).
Probeer een van:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u naar de image store kopieert. De compressie vermindert het aantal bestanden.
- Geef een hogere time-out voor [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) met `timeout` parameter.

## <a name="code-example"></a>Voorbeeld van code
In het volgende voorbeeld toepassingspakketten kopieert naar de image store, voorziet van het type van de instantie van een toepassing maakt, maakt u een service-exemplaar, verwijdert het toepassingsexemplaar, ongedaan maken met de bepalingen van het toepassingstype en verwijdert het het toepassingspakket van de image store.

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
[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)

[Service Fabric health Inleiding](service-fabric-health-introduction.md)

[Vaststellen en oplossen van een Service Fabric-service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model van een toepassing in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md

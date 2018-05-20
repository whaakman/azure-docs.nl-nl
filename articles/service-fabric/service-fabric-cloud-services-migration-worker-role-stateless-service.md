---
title: Azure Cloud Services-apps converteren naar microservices | Microsoft Docs
description: Deze handleiding vergelijkt Cloud Services-Web- en werkrollen en Service Fabric stateless services om te migreren van Cloud-Services naar Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c6bdd6f88c9008a8d9c15d22bdcf263190424649
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Hulp bij het converteren van Web- en werkrollen naar Service Fabric stateless services
Dit artikel wordt beschreven hoe u uw Cloud Services-Web- en werkrollen migreren naar Service Fabric stateless services. Dit is het eenvoudigste migratiepad van Cloud-Services naar Service Fabric voor toepassingen waarvan de algehele architectuur gaat ongeveer hetzelfde blijft.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Cloudserviceproject naar Service Fabric-toepassingsproject
 Een Cloud Service-project en een project Service Fabric-toepassing hebben een vergelijkbare structuur en beide vertegenwoordigen de implementatie-eenheid voor uw toepassing - dat wil zeggen, ze elk definiëren het volledige pakket dat is geïmplementeerd als uw toepassing wilt uitvoeren. Een Cloud Service-project bevat een of meer Web- of werkrollen. Een Service Fabric-toepassing-project bevat ook een of meer services. 

Het verschil is dat het project Service in de Cloud de implementatie van de toepassing met een VM-implementatie worden gekoppeld en dus VM-configuratie-instellingen in dit bevat dat het project Service Fabric-toepassing wordt alleen gedefinieerd voor een toepassing die wordt geïmplementeerd op een reeks bestaande virtuele machines in een Service Fabric-cluster. Het Service Fabric-cluster zelf wordt slechts eenmaal geïmplementeerd via een Resource Manager-sjabloon of via de Azure-portal en meerdere Service Fabric-toepassingen kunnen worden geïmplementeerd.

![Vergelijking van service Fabric en Cloud Services-project][3]

## <a name="worker-role-to-stateless-service"></a>Werkrol met service staatloze
Conceptueel gezien vertegenwoordigt een Werkrol een staatloze werkbelasting, wat betekent dat elke instantie van de werkbelasting is identiek en aanvragen kunnen worden doorgestuurd naar een willekeurig exemplaar op elk gewenst moment. Elk exemplaar wordt niet verwacht te onthouden van de vorige aanvraag. Status die is van invloed op de werkbelasting wordt beheerd door een externe Statusopslag, zoals Azure Table Storage of Azure Documentdb. Dit soort werkbelasting is in Service Fabric vertegenwoordigd door een Stateless Service. De eenvoudigste manier om te migreren van een Werkrol naar Service Fabric kan worden gedaan door de Werkrol code converteren naar een Stateless Service.

![Werkrol met Service staatloze][4]

## <a name="web-role-to-stateless-service"></a>Webrol in staatloze service
Net als bij de Werkrol, een Webrol ook vertegenwoordigt een staatloze werkbelasting, en zo in dat geval het te kan worden toegewezen aan een stateless Service Fabric-service. Echter, in tegenstelling tot webrollen, Service Fabric ondersteunt geen IIS. Als u wilt migreren van een web vereist toepassing van een Webrol bij een stateless service dat eerste verplaatsen naar een webframework dat automatisch kan worden gehost en is niet afhankelijk van IIS of System.Web, zoals ASP.NET Core 1.

| **Toepassing** | **Ondersteund** | **Migratiepad** |
| --- | --- | --- |
| ASP.NET-webformulieren |Nee |Converteren naar ASP.NET Core 1 MVC |
| ASP.NET MVC |Met migratie |Upgrade naar ASP.NET Core 1 MVC |
| ASP.NET Web API |Met migratie |Automatische gehoste-server of ASP.NET Core 1 gebruiken |
| ASP.NET Core 1 |Ja |N/A |

## <a name="entry-point-api-and-lifecycle"></a>Vermelding punt API en de levenscyclus
Werkrol en Service Fabric-service-API's bieden vergelijkbare toegangspunten: 

| **Toegangspunt** | **Werkrol** | **Service Fabric-service** |
| --- | --- | --- |
| Verwerken |`Run()` |`RunAsync()` |
| VM starten |`OnStart()` |N/A |
| VM stoppen |`OnStop()` |N/A |
| Open listener voor aanvragen van clients |N/A |<ul><li> `CreateServiceInstanceListener()` voor stateless</li><li>`CreateServiceReplicaListener()` voor stateful</li></ul> |

### <a name="worker-role"></a>Werkrol
```csharp

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Staatloze service Fabric-Service
```csharp

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Hebben beide een onderdrukking primaire 'uitvoeren' waarin voordat deze wordt verwerkt. Service Fabric-services combineren `Run`, `Start`, en `Stop` in een enkel toegangspunt `RunAsync`. De service moet beginnen met werken wanneer `RunAsync` wordt gestart en mag niet meer werken wanneer de `RunAsync` van de methode CancellationToken ontvangt een signaal. 

Er zijn enkele belangrijke verschillen tussen de levenscyclus van en de levensduur van Service Fabric- en werkrollen services:

* **Levenscyclus:** het belangrijkste verschil is dat een Werkrol een virtuele machine en dus de levenscyclus is gekoppeld aan de virtuele machine, waaronder gebeurtenissen voor wanneer de virtuele machine wordt gestart en gestopt. Een Service Fabric-service heeft een levenscyclus dat is gescheiden van de levenscyclus van de virtuele machine, zodat deze bevat geen gebeurtenissen voor wanneer de host, VM of machine wordt gestart en gestopt, omdat ze niet zijn gerelateerd.
* **Levensduur:** een Werkrol-exemplaar wordt gerecycled als de `Run` methode afgesloten. De `RunAsync` methode in een Service Fabric-service evenwel kan worden uitgevoerd voor voltooien en het service-exemplaar blijven. 

Service Fabric bevat een toegangspunt optionele communicatie-instellingen voor services die naar aanvragen van clients luisteren. De RunAsync- en communicatie toegangspunt zijn optionele onderdrukkingen in Service Fabric-services - uw service kunt luistert alleen op aanvragen van clients, of een lus verwerking of beide alleen worden uitgevoerd - dat is waarom de RunAsync-methode is toegestaan om af te sluiten zonder het service-exemplaar opnieuw te starten omdat deze blijven mogelijk om te luisteren naar aanvragen van clients.

## <a name="application-api-and-environment"></a>Application-API en de omgeving
De Cloud Services-omgeving API bevat informatie en functionaliteit voor de huidige VM-instantie, evenals informatie over andere VM-rolexemplaren. Service Fabric bevat informatie met betrekking tot de runtime en enige informatie over het knooppunt van een service op wordt uitgevoerd. 

| **Taak omgeving** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Configuratie-instellingen en Wijzigingsmelding |`RoleEnvironment` |`CodePackageActivationContext` |
| Lokale opslag |`RoleEnvironment` |`CodePackageActivationContext` |
| Informatie over endpoint |`RoleInstance` <ul><li>Huidige instantie: `RoleEnvironment.CurrentRoleInstance`</li><li>Andere functies en -exemplaar: `RoleEnvironment.Roles`</li> |<ul><li>`NodeContext` voor het huidige knooppuntadres</li><li>`FabricClient` en `ServicePartitionResolver` voor detectie van de service-eindpunt</li> |
| Omgeving emulatie |`RoleEnvironment.IsEmulated` |N/A |
| Gelijktijdige wijzigingsgebeurtenis |`RoleEnvironment` |N/A |

## <a name="configuration-settings"></a>Configuratie-instellingen
Configuratie-instellingen in de Cloud-Services zijn ingesteld voor een VM-rol en gelden voor alle exemplaren van die VM-rol. Deze instellingen zijn ingesteld in ServiceConfiguration.*.cscfg bestanden sleutel-waardeparen en toegankelijk zijn via RoleEnvironment. In Service Fabric toepassen instellingen afzonderlijk voor elke service en elke toepassing, in plaats van aan een VM, omdat een virtuele machine kan meerdere services en toepassingen hosten. Een service bestaat uit drie pakketten:

* **Code:** bevat van de service uitvoerbare bestanden, binaire bestanden, dll's en andere bestanden die een service nodig heeft om uit te voeren.
* **Config:** alle configuratiebestanden en instellingen voor een service.
* **Gegevens:** statische bestanden die zijn gekoppeld aan de service.

Elk van deze pakketten zijn onafhankelijk samengestelde en bijgewerkte. Net als bij Cloud Services, een configuratiepakket toegankelijk zijn via een programma via een API en gebeurtenissen zijn beschikbaar op de hoogte van de service van een pakket configuratiewijziging wilt. Een bestand Settings.xml kan worden gebruikt voor de sleutel / waarde-configuratie en toegang op programmeerniveau vergelijkbaar met de sectie van de app-instellingen van een App.config-bestand. In tegenstelling tot Cloud-Services, kan een Service Fabric-configuratiepakket echter bevatten alle configuratiebestanden in een willekeurige indeling, of het XML, JSON, YAML of een aangepaste binaire indeling. 

### <a name="accessing-configuration"></a>Toegang tot configuratie
#### <a name="cloud-services"></a>Cloud Services
Configuratie-instellingen van ServiceConfiguration.*.cscfg toegankelijk zijn via `RoleEnvironment`. Deze instellingen zijn algemeen beschikbaar voor alle rolinstanties in dezelfde Cloud Service-implementatie.

```csharp

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Elke service heeft een eigen afzonderlijke configuratie-pakket. Er is geen ingebouwde methode voor globale configuratie-instellingen toegankelijk door alle toepassingen in een cluster. Wanneer u Service-Fabric speciale Settings.xml configuratiebestand binnen een configuratiepakket gebruikt, kunnen waarden in Settings.xml worden overschreven op toepassingsniveau, die op toepassingsniveau configuratie-instellingen mogelijk te maken.

Configuratie-instellingen zijn toegangspogingen binnen elk exemplaar van de service via de service `CodePackageActivationContext`.

```csharp

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Update-Configuratiegebeurtenissen
#### <a name="cloud-services"></a>Cloud Services
De `RoleEnvironment.Changed` gebeurtenis alle rolinstanties waarschuwen wanneer een wijziging in de omgeving, zoals een wijziging in de configuratie optreedt wordt gebruikt. Dit wordt configuratie-updates gebruiken zonder rolinstanties recycling of opnieuw starten van een werkproces gebruikt.

```csharp

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
Elk van de drie pakkettypen in een service - Code, configuratie en gegevens - gebeurtenissen die een service-exemplaar wordt gewaarschuwd wanneer er een pakket wordt bijgewerkt, toegevoegd of verwijderd hebben. Een service, kan meerdere pakketten van elk type bevatten. Een service kan bijvoorbeeld meerdere config pakketten elk afzonderlijk samengestelde en kan worden uitgebreid. 

Deze gebeurtenissen zijn beschikbaar voor wijzigingen in servicepakketten gebruiken zonder het service-exemplaar opnieuw te starten.

```csharp

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Starten van de taken
Starten van de taken zijn acties die worden uitgevoerd voordat een toepassing wordt gestart. Een taak starten wordt meestal gebruikt voor setup scripts uitvoeren met verhoogde bevoegdheden. Ondersteuning voor opstarten taken zowel Cloudservices als Service Fabric. Het belangrijkste verschil is dat in de Cloud-Services, een taak starten is gekoppeld aan een virtuele machine omdat deze deel uitmaakt van een rolexemplaar dat in Service Fabric een starten van de taak is gekoppeld aan een service, is niet gekoppeld aan een bepaalde virtuele machine.

| Service Fabric | Cloud Services |
| --- | --- | --- |
| Locatie van de configuratie |ServiceDefinition.csdef |
| Bevoegdheden |'beperkt' of 'verhoogde' |
| Sequentiëren |'eenvoudige', 'achtergrond', 'voorgrond' |

### <a name="cloud-services"></a>Cloud Services
In de Cloud Services een toegangspunt opstarten geconfigureerd per rol in ServiceDefinition.csdef. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
In Service Fabric is een toegangspunt starten van de per-service in ServiceManifest.xml geconfigureerd:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Een opmerking over ontwikkelomgeving
Zowel Cloudservices als Service Fabric zijn geïntegreerd met Visual Studio met projectsjablonen en ondersteuning voor foutopsporing, configureren en implementeren van zowel lokaal als in Azure. Zowel Cloudservices als Service Fabric bieden ook een runtime-omgeving voor lokale ontwikkeling. Het verschil is dat tijdens de runtime van de ontwikkeling van Service in de Cloud de Azure-omgeving waarop deze wordt uitgevoerd emuleert, een emulator maakt geen gebruik van Service Fabric - gebruikmaakt van de volledige Service Fabric-runtime. De Service Fabric-omgeving die u op uw lokale ontwikkelcomputer uitvoert is dezelfde omgeving die wordt uitgevoerd in de productieomgeving.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Service Fabric Reliable Services en de fundamentele verschillen tussen Cloudservices en architectuur van Service Fabric-toepassing om te begrijpen hoe om te profiteren van de volledige set van Service Fabric-functies.

* [Aan de slag met Service Fabric Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Conceptuele handleiding voor de verschillen tussen Cloudservices en Service Fabric](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png

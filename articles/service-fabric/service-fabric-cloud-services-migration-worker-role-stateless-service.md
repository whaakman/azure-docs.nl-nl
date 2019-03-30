---
title: Converteren van Azure Cloud Services-apps in Service Fabric | Microsoft Docs
description: Deze handleiding worden Cloud Services-Web- en werkrollen en Service Fabric stateless services om te helpen migreren uit Cloud Services naar Service Fabric vergeleken.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 10fb44b0e76282ad78e7687beaa2e50e819e5cd9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667715"
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Handleiding voor Web- en werkrollen converteren naar de stateless Service Fabric-services
In dit artikel wordt beschreven hoe u uw Cloud Services-Web- en werkrollen migreren naar Service Fabric stateless services. Dit is het eenvoudigste migratiepad van Cloud Services naar Service Fabric voor toepassingen waarvan de algehele architectuur gaat ongeveer hetzelfde blijven.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Cloud Service-project naar Service Fabric-toepassingsproject
 Een Cloud Service-project en een Service Fabric-toepassing-project hebt een vergelijkbare structuur en beide vertegenwoordigen de implementatie-eenheid voor uw toepassing - dat wil zeggen, krijgen deze allemaal definiëren het volledige pakket dat wordt geïmplementeerd als uw toepassing wilt uitvoeren. Een Cloud Service-project bevat een of meer Web- of werkrollen. Op dezelfde manier, een Service Fabric-toepassingsproject bevat een of meer services. 

Het verschil is dat de implementatie van de toepassing met een VM-implementatie worden gekoppeld en dus VM-configuratie-instellingen bevat, het project Service in de Cloud terwijl de Service Fabric-toepassingsproject definieert u alleen een toepassing die wordt geïmplementeerd op een set bestaande virtuele machines in een Service Fabric-cluster. Het Service Fabric-cluster zelf is alleen één keer geïmplementeerd via een Resource Manager-sjabloon of via de Azure-portal en meerdere Service Fabric-toepassingen kunnen worden geïmplementeerd.

![Vergelijking van service Fabric en Cloud Services-project][3]

## <a name="worker-role-to-stateless-service"></a>Werkrol met stateless service
Conceptueel gezien, vertegenwoordigt een Werkrol een staatloze werkbelasting, wat betekent dat elk exemplaar van de werkbelasting is identiek zijn en aanvragen kunnen worden doorgestuurd naar een willekeurig exemplaar op elk gewenst moment. Elk exemplaar wordt niet verwacht te onthouden van de vorige aanvraag. Status die van invloed op de werkbelasting wordt beheerd door een externe Statusopslag, zoals Azure Table Storage of Azure Document DB. Dit type workload wordt in Service Fabric vertegenwoordigd door een Stateless Service. De eenvoudigste manier voor het migreren van een Werkrol in Service Fabric kan worden gedaan door code van de Werkrol converteren naar een Stateless Service.

![Werkrol naar de Stateless Service][4]

## <a name="web-role-to-stateless-service"></a>Webrol in stateless service
Net als bij Werkrol, een Webrol ook vertegenwoordigt een staatloze werkbelasting, en zo in dat geval het te kan worden toegewezen aan een stateless Service Fabric-service. Echter, in tegenstelling tot webrollen, Service Fabric biedt geen ondersteuning IIS. Als u wilt migreren van een web vereist de toepassing van een Webrol naar een stateless service eerste verplaatsen naar een webframework dat zelf kan worden gehost en niet afhankelijk is van IIS-of System.Web, zoals ASP.NET Core-1.

| **Toepassing** | **Ondersteund** | **Migratiepad** |
| --- | --- | --- |
| ASP.NET Web Forms |Nee |Converteren naar ASP.NET Core, 1 MVC |
| ASP.NET MVC |Met de migratie |Een upgrade uit naar ASP.NET Core-1 MVC |
| ASP.NET Web-API |Met de migratie |Gebruik zelf-hostend server of ASP.NET Core 1 |
| ASP.NET Core 1 |Ja |N/A |

## <a name="entry-point-api-and-lifecycle"></a>Vermelding punt API en de levenscyclus
Werkrol en Service Fabric service-API's bieden vergelijkbare toegangspunten: 

| **Toegangspunt** | **Werkrol** | **Service Fabric-service** |
| --- | --- | --- |
| Verwerken |`Run()` |`RunAsync()` |
| VM starten |`OnStart()` |N/A |
| VM stoppen |`OnStop()` |N/A |
| Open-listener voor aanvragen van clients |N/A |<ul><li> `CreateServiceInstanceListener()` voor staatloze</li><li>`CreateServiceReplicaListener()` voor stateful</li></ul> |

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

### <a name="service-fabric-stateless-service"></a>Service Fabric Stateless Service
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

Een primaire "uitvoeren" onderdrukking waarin om te beginnen met verwerking hebben. Service Fabric-services combineren `Run`, `Start`, en `Stop` in een enkel ingangspunt `RunAsync`. Uw service moet beginnen te werken wanneer `RunAsync` wordt gestart, en moet worden stopgezet werken wanneer de `RunAsync` van de methode CancellationToken wordt doorgegeven. 

Er zijn enkele belangrijke verschillen tussen de levenscyclus en levensduur van werkrollen en Service Fabric-services:

* **Levenscyclus:** Het belangrijkste verschil is dat een Werkrol een virtuele machine is en dus de levenscyclus is gekoppeld aan de virtuele machine, waaronder gebeurtenissen voor wanneer de virtuele machine wordt gestart en gestopt. Een Service Fabric-service heeft een levenscyclus dat is gescheiden van de levenscyclus van de virtuele machine, zodat deze omvatten geen gebeurtenissen voor wanneer de host virtuele machine of computer wordt gestart en gestopt, omdat ze niet zijn gerelateerd.
* **Levensduur:** Een werknemer rolinstantie wordt gerecycled als de `Run` methode afgesloten. De `RunAsync` methode in een Service Fabric-service kan echter tot voltooiing uitgevoerd en het service-exemplaar blijven. 

Service Fabric biedt een ingangspunt voor optionele communicatie-instellingen voor de services die naar aanvragen van clients luisteren. De RunAsync en de communicatie toegangspunt zijn optioneel onderdrukkingen in Service Fabric-services - uw-service kiezen alleen luisteren naar aanvragen van clients of alleen een lus verwerking, of beide worden uitgevoerd - daarom zijn de methode RunAsync is toegestaan om af te sluiten zonder opnieuw te starten het service-exemplaar, omdat deze mogelijk blijven om te luisteren naar aanvragen van clients.

## <a name="application-api-and-environment"></a>Toepassing API en de omgeving
De Cloud Services-omgeving API biedt informatie en -functionaliteit voor de huidige VM-exemplaar, evenals informatie over andere instanties van de virtuele machine. Service Fabric biedt informatie met betrekking tot de runtime en enkele gegevens over het knooppunt van een service die momenteel wordt uitgevoerd op. 

| **Omgeving-taak** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Configuratie-instellingen en Wijzigingsmelding |`RoleEnvironment` |`CodePackageActivationContext` |
| Lokale opslag |`RoleEnvironment` |`CodePackageActivationContext` |
| Informatie over endpoint |`RoleInstance` <ul><li>Huidige instantie: `RoleEnvironment.CurrentRoleInstance`</li><li>Andere functies en -exemplaar: `RoleEnvironment.Roles`</li> |<ul><li>`NodeContext` voor het huidige knooppuntadres</li><li>`FabricClient` en `ServicePartitionResolver` voor service-eindpuntdetectie</li> |
| Emulatie van omgeving |`RoleEnvironment.IsEmulated` |N/A |
| Gebeurtenis gelijktijdige wijzigen |`RoleEnvironment` |N/A |

## <a name="configuration-settings"></a>Configuratie-instellingen
Configuratie-instellingen in Cloud Services worden ingesteld voor een VM-rol en van toepassing op alle exemplaren van deze VM-rol. Deze instellingen zijn sleutel-waardeparen instellen in ServiceConfiguration.*.cscfg bestanden en zijn toegankelijk via RoleEnvironment. In Service Fabric toepassen instellingen afzonderlijk op elke service en naar elke toepassing, in plaats van een virtuele machine, omdat een virtuele machine van meerdere services en toepassingen hosten kunt. Een service bestaat uit drie pakketten:

* **Code:** bevat de service uitvoerbare bestanden en binaire bestanden, dll's en andere bestanden met een service nodig heeft om uit te voeren.
* **Config:** alle configuratiebestanden en instellingen voor een service.
* **Gegevens:** statische bestanden die zijn gekoppeld aan de service.

Elk van deze pakketten kan onafhankelijke versies en upgrades zijn. Net als bij Cloud Services, een configuratiepakket kan worden benaderd via een programma met een API en gebeurtenissen zijn beschikbaar voor de kennis van de service van een pakket configuratiewijziging. Een bestand Settings.xml kan worden gebruikt voor sleutel / waarde-configuratie en toegang op programmeerniveau die vergelijkbaar is met het gedeelte van de app instellingen van een App.config-bestand. In tegenstelling tot Cloud Services, kan een Service Fabric-configuratiepakket echter bevatten alle configuratiebestanden in elke indeling of het XML, JSON, YAML of een aangepaste binaire indeling. 

### <a name="accessing-configuration"></a>Toegang tot de configuratie
#### <a name="cloud-services"></a>Cloud Services
Configuratie-instellingen van ServiceConfiguration.*.cscfg is toegankelijk via `RoleEnvironment`. Deze instellingen zijn wereldwijd beschikbaar voor alle rolinstanties in dezelfde Cloudservice-implementatie.

```csharp

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Elke service heeft een eigen afzonderlijke configuratie-pakket. Er is geen ingebouwd mechanisme voor globale configuratie-instellingen toegankelijk door alle toepassingen in een cluster. Bij het gebruik van Service Fabric speciale Settings.xml configuratiebestand binnen een configuratiepakket, kunnen de waarden in Settings.xml worden overschreven op toepassingsniveau, waardoor op toepassingsniveau configuratie-instellingen mogelijk.

Configuratie-instellingen zijn toegang binnen elk service-exemplaar via van de service `CodePackageActivationContext`.

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
De `RoleEnvironment.Changed` gebeurtenis aan alle rolinstanties waarschuwen wanneer een wijziging in de omgeving, zoals een wijziging in de configuratie optreedt wordt gebruikt. Dit wordt gebruikt om het verbruiken van configuratie-updates zonder instanties van de recycling of opnieuw starten van een werkproces.

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
Elk van de drie pakkettypen in een service - Code, configuratie en gegevens - zijn gebeurtenissen die een service-exemplaar waarschuwen wanneer een pakket wordt bijgewerkt, toegevoegd of verwijderd. Een service kan meerdere pakketten van elk type bevatten. Een service kan bijvoorbeeld meerdere config pakketten, elk apart is samengesteld en kan worden geüpgraded. 

Deze gebeurtenissen zijn beschikbaar voor wijzigingen in de service-pakketten gebruiken zonder het service-exemplaar opnieuw te starten.

```csharp

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Opstarttaken
Opstarttaken zijn acties die worden uitgevoerd voordat een toepassing wordt gestart. Een opstarttaak wordt doorgaans gebruikt setup-scripts met verhoogde bevoegdheden uit te voeren. Ondersteuning voor opstarten taken zowel Cloud Services en Service Fabric. Het belangrijkste verschil is dat in Cloud Services, een opstarttaak is gebonden aan een virtuele machine omdat deze deel uitmaakt van een rolinstantie, terwijl in Service Fabric een opstarttaak is gekoppeld aan een service, die is niet gekoppeld aan een bepaalde virtuele machine.

| Service Fabric | Cloud Services |
| --- | --- |
| Locatie van de configuratie |ServiceDefinition.csdef |
| Bevoegdheden |"beperkt" of "uitgebreide" |
| Sequentiëren |'eenvoudige', "achtergrond", "voorgrond" |

### <a name="cloud-services"></a>Cloud Services
In Cloud Services is een toegangspunt opstarten geconfigureerd per rol in ServiceDefinition.csdef. 

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
In Service Fabric is een toegangspunt opstarten geconfigureerd per service in ServiceManifest.xml:

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
Zowel Cloud Services en Service Fabric zijn geïntegreerd met Visual Studio-projectsjablonen en ondersteuning voor foutopsporing, configureren en implementeren van zowel lokaal als in Azure. Zowel Cloud Services en Service Fabric bieden ook een lokale ontwikkeling runtime-omgeving. Het verschil is dat tijdens de ontwikkeling van Cloud Service runtime de Azure-omgeving waarop deze wordt uitgevoerd simuleert, een emulator maakt geen gebruik van Service Fabric - de volledige Service Fabric-runtime wordt gebruikt. De Service Fabric-omgeving die u op uw lokale ontwikkelcomputer uitvoert is dezelfde omgeving die in productie wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Service Fabric Reliable Services en de fundamentele verschillen tussen Cloud Services en architectuur van Service Fabric-toepassing om te begrijpen hoe u kunt profiteren van de volledige set van Service Fabric-functies.

* [Aan de slag met Service Fabric Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Algemene handleiding voor de verschillen tussen Cloud Services en Service Fabric](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png

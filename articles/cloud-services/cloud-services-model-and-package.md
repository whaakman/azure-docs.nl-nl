---
title: Wat is een cloudservicemodel en -pakket | Microsoft Docs
description: Beschrijving van de cloud-servicemodel (csdef, cscfg-bestand) en het pakket (.cspkg) in Azure
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: 9c9f7dfd9ecbf085da19fc010e497caef8c18629
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917308"
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Wat is het servicemodel van Cloud en hoe ik een pakket?
Een cloudservice is gemaakt op basis van drie onderdelen, de servicedefinitie *(.csdef)*, de configuratie van de service *(.cscfg)*, en een servicepakket *(.cspkg)*. Zowel de **ServiceDefinition.csdef** en **ServiceConfig.cscfg** bestanden zijn gebaseerd op XML en beschrijven de structuur van de cloudservice en hoe deze geconfigureerd, samen met de naam het model. De **ServicePackage.cspkg** is een zipbestand dat is gegenereerd op basis van de **ServiceDefinition.csdef** en bevat onder andere alle vereiste binaire bestanden op basis van afhankelijkheden. Azure maakt een cloudservice van zowel de **ServicePackage.cspkg** en de **ServiceConfig.cscfg**.

Zodra de service in de cloud wordt uitgevoerd in Azure, kunt u het configureren via de **ServiceConfig.cscfg** bestand, maar kan de definitie niet wijzigen.

## <a name="what-would-you-like-to-know-more-about"></a>Wat wilt u meer weten?
* Ik wil meer informatie over de [ServiceDefinition.csdef](#csdef) en [ServiceConfig.cscfg](#cscfg) bestanden.
* Ik al weet over dat uitleggen [enkele voorbeelden](#next-steps) op wat ik kunt configureren.
* Ik wil maken de [ServicePackage.cspkg](#cspkg).
* Ik gebruik Visual Studio en ik wil...
  * [Een cloudservice maken][vs_create]
  * [Een bestaande cloudservice configureren][vs_reconfigure]
  * [Een project voor de Service in de Cloud implementeren][vs_deploy]
  * [Extern bureaublad in een cloud service-exemplaar][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
De **ServiceDefinition.csdef** bestand Hiermee geeft u de instellingen die worden gebruikt door Azure voor een cloudservice configureren. De [Definitieschema voor Azure-Service (.csdef-bestand)](/previous-versions/azure/reference/ee758711(v=azure.100)) biedt de toegestane indeling voor een service-definitiebestand. Het volgende voorbeeld ziet u de instellingen die kunnen worden gedefinieerd voor de Web- en werknemersrollen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

U kunt verwijzen naar de [Definitieschema voor Service](/previous-versions/azure/reference/ee758711(v=azure.100)) voor een beter begrip van het XML-schema dat hier wordt gebruikt, maar hier volgt een korte uitleg van enkele van de elementen:

**Sites**  
Bevat de definities voor websites of webtoepassingen toepassingen die worden gehost in IIS7.

**InputEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt om contact op met de cloudservice.

**InternalEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt door de rolinstanties om te communiceren met elkaar.

**ConfigurationSettings**  
Bevat de instellingsdefinities voor de functies van een specifieke rol.

**Certificaten**  
Bevat de definities voor certificaten die nodig zijn voor een rol. Het vorige codevoorbeeld ziet u een certificaat dat wordt gebruikt voor de configuratie van de Azure-verbinding.

**LocalResources**  
Bevat de definities voor de resources voor lokale opslag. Een lokale opslag-resource is een gereserveerde map op het bestandssysteem van de virtuele machine waarop een exemplaar van een rol wordt uitgevoerd.

**Importeren**  
Bevat de definities voor de geïmporteerde modules. Het vorige codevoorbeeld ziet u de modules voor verbinding met extern bureaublad en Azure-verbinding.

**Opstarten**  
Bevat taken die worden uitgevoerd wanneer de rol wordt gestart. De taken zijn gedefinieerd in een .cmd of uitvoerbaar bestand.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
De configuratie van de instellingen voor uw cloudservice wordt bepaald door de waarden in de **ServiceConfiguration.cscfg** bestand. U geeft het aantal exemplaren dat u voor elke rol in dit bestand wilt implementeren. De waarden voor de configuratie-instellingen die u hebt gedefinieerd in het servicedefinitiebestand worden toegevoegd aan het configuratiebestand van de service. De vingerafdrukken instellen voor van beheercertificaten die gekoppeld aan de cloudservice zijn worden ook toegevoegd aan het bestand. De [configuratieschema voor Azure-Service (.cscfg-bestand)](/previous-versions/azure/reference/ee758710(v=azure.100)) biedt de toegestane indeling voor een service-configuratiebestand.

Het configuratiebestand van de service wordt niet geleverd met de toepassing, maar wordt geüpload naar Azure als een afzonderlijk bestand en wordt gebruikt voor het configureren van de cloudservice. U kunt een nieuwe service-configuratiebestand uploaden zonder uw cloudservice opnieuw te implementeren. De configuratiewaarden voor de cloudservice kunnen worden gewijzigd terwijl de service in de cloud wordt uitgevoerd. Het volgende voorbeeld ziet u de configuratie-instellingen die kunnen worden gedefinieerd voor de Web- en werknemersrollen:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

U kunt verwijzen naar de [configuratieschema Service](/previous-versions/azure/reference/ee758710(v=azure.100)) voor een beter beeld van het XML-schema dat hier wordt gebruikt, maar hier volgt een korte uitleg van de elementen:

**Exemplaren**  
Hiermee configureert u het aantal actieve instanties voor de rol. Om te voorkomen dat uw cloudservice mogelijk niet langer beschikbaar is tijdens upgrades, is het raadzaam dat u meer dan één exemplaar van uw web gerichte rollen implementeren. Meer dan één exemplaar implementeert, u voldoet aan de richtlijnen in de [Azure Compute-Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/), die garandeert dat externe connectiviteit van 99,95% voor internetgerichte rollen wanneer twee of meer rolinstanties geïmplementeerd voor een service.

**ConfigurationSettings**  
Hiermee configureert u de instellingen voor de exemplaren die worden uitgevoerd voor een rol. De naam van de `<Setting>` elementen moeten overeenkomen met de instellingsdefinities van de in het servicedefinitiebestand.

**Certificaten**  
Hiermee configureert u de certificaten die worden gebruikt door de service. Het vorige codevoorbeeld laat zien over het definiëren van het certificaat voor de RemoteAccess-module. De waarde van de *vingerafdruk* kenmerk moet worden ingesteld op de vingerafdruk van het certificaat te gebruiken.

<p/>

> [!NOTE]
> De vingerafdruk van het certificaat kan worden toegevoegd aan het configuratiebestand met behulp van een teksteditor. Of de waarde kan worden toegevoegd op het **certificaten** tabblad van de **eigenschappen** pagina van de functie in Visual Studio.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Poorten voor rolinstanties definiëren
Met Azure kunt slechts één beginpunt voor een Webrol. Wat betekent dat al het verkeer via een IP-adres plaatsvindt. U kunt uw websites voor het delen van een poort door het configureren van de host-header om de aanvraag om de juiste locatie te configureren. U kunt ook uw toepassingen om te luisteren naar bekende poorten op het IP-adres configureren.

Het volgende voorbeeld ziet u de configuratie voor een Webrol aan een toepassing website en webtoepassingen. De website is geconfigureerd als de standaardlocatie voor de vermelding op poort 80 en de web-apps worden geconfigureerd voor het ontvangen van aanvragen van een alternatieve host-header met de naam 'mail.mysite.cloudapp.net'.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Wijzigen van de configuratie van een rol
U kunt de configuratie van uw cloudservice bijwerken terwijl deze wordt uitgevoerd in Azure, zonder de service offline. Als u wilt wijzigen van configuratie-informatie, kunt u een nieuw configuratiebestand uploaden of bewerken van het configuratiebestand in plaats en pas deze toe aan uw actieve service. De volgende wijzigingen kunnen worden gemaakt in de configuratie van een service:

* **De waarden van configuratie-instellingen wijzigen**  
  Wanneer een configuratie-instelling van wijzigingen, een rolinstantie kunt kiezen om de wijziging toepassen terwijl het exemplaar online is, of het exemplaar probleemloos herhalen en toepassen van de wijziging bij het exemplaar offline is.
* **Wijzigen van de service-topologie van de rolinstanties**  
  Wijzigingen in de netwerktopologie, zijn niet van invloed op exemplaren die worden uitgevoerd, behalve wanneer een exemplaar wordt verwijderd. Alle resterende exemplaren hebben doorgaans niet nodig worden gerecycled; u kunt echter recyclen rolinstanties in reactie op een wijziging in de topologie.
* **Vingerafdruk van het certificaat wijzigen**  
  U kunt alleen een certificaat bijwerken wanneer een rolinstantie offline is. Als een certificaat is toegevoegd, verwijderd of gewijzigd terwijl een rolinstantie online is, wordt Azure zonder problemen het exemplaar offline voor het bijwerken van het certificaat en weer online brengen nadat de wijziging voltooid is.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Wijzigingen in de configuratie met de Service Runtime-gebeurtenissen verwerken
De [Azure Runtime Library](/previous-versions/azure/reference/mt419365(v=azure.100)) bevat de [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee741722(v=azure.100)) naamruimte, waarmee u klassen voor interactie met de Azure-omgeving van een rol. De [RoleEnvironment](/previous-versions/azure/reference/ee773173(v=azure.100)) klasse definieert de volgende gebeurtenissen die worden gegenereerd voor en na een wijziging in de configuratie:

* **[Wijzigen van](/previous-versions/azure/reference/ee758134(v=azure.100)) gebeurtenis**  
  Dit gebeurt voordat de wijziging wordt toegepast op een opgegeven exemplaar van een rol zodat u een kans om de rolinstanties indien nodig.
* **[Gewijzigd](/previous-versions/azure/reference/ee758129(v=azure.100)) gebeurtenis**  
  Treedt op nadat de wijziging in de configuratie is toegepast op een opgegeven exemplaar van een rol.

> [!NOTE]
> Omdat het certificaat wijzigingen worden altijd de exemplaren van een rol offline, doen ze niet de gebeurtenissen RoleEnvironment.Changing of RoleEnvironment.Changed verhogen.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Als u wilt een toepassing als een cloudservice in Azure implementeert, moet u eerst inpakken van de toepassing in de juiste indeling. U kunt de **CSPack** opdrachtregel-hulpprogramma (geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/)) te maken van het pakketbestand als alternatief voor Visual Studio.

**CSPack** de inhoud van het servicedefinitiebestand en het configuratiebestand gebruikt voor het definiëren van de inhoud van het pakket. **CSPack** genereert van een toepassing-pakketbestand (.cspkg) die u uploaden naar Azure met behulp van kunt de [Azure-portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Standaard wordt het pakket met de naam `[ServiceDefinitionFileName].cspkg`, maar u kunt een andere naam opgeven met behulp van de `/out` optie van **CSPack**.

**CSPack** bevindt zich in  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (op windows) is beschikbaar door het uitvoeren van de **Microsoft Azure-opdrachtprompt** snelkoppeling die is geïnstalleerd met de SDK.  
> 
> Voer het programma CSPack.exe zelf om te zien van de documentatie over de mogelijke switches en opdrachten.
> 
> 

<p />

> [!TIP]
> Uitvoeren van uw cloudservice lokaal in de **Microsoft Azure-Rekenemulator**, gebruikt u de **/copyonly** optie. Deze optie kopieert de binaire bestanden voor de toepassing naar een map lay-out van waaruit ze kunnen worden uitgevoerd in de rekenemulator.
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a>Van de voorbeeldopdracht naar een cloudservice verpakken
Het volgende voorbeeld wordt een toepassingspakket met de gegevens voor een Webrol. De opdracht geeft u het servicedefinitiebestand moet worden gebruikt, de map waar de binaire bestanden kunnen worden gevonden, en de naam van het pakketbestand.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Als de toepassing zowel een Webrol en een werkrol bevat, wordt de volgende opdracht gebruikt:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Waar de variabelen worden als volgt gedefinieerd:

| Variabele | Value |
| --- | --- |
| \[DirectoryName\] |De submap onder de hoofdmap van het project die het .csdef-bestand van de Azure-project bevat. |
| \[ServiceDefinition\] |De naam van het servicedefinitiebestand. Standaard is dit bestand de naam ServiceDefinition.csdef. |
| \[OutputFileName\] |De naam voor de gegenereerde pakketbestand. Dit is normaal gesproken ingesteld op de naam van de toepassing. Als er geen bestandsnaam is opgegeven, het toepassingspakket is gemaakt als \[ApplicationName\].cspkg. |
| \[RoleName\] |De naam van de rol zoals gedefinieerd in het servicedefinitiebestand. |
| \[RoleBinariesDirectory] |De locatie van de binaire bestanden voor de rol. |
| \[VirtualPath\] |De fysieke mappen voor elke virtuele pad dat is gedefinieerd in de sectie van de site van de definitie van de service. |
| \[PhysicalPath\] |De fysieke mappen van de inhoud voor elke virtuele pad dat is gedefinieerd in het siteknooppunt van de definitie van de service. |
| \[RoleAssemblyName\] |De naam van het binaire bestand voor de rol. |

## <a name="next-steps"></a>Volgende stappen
Ik ben een cloud service-pakket wordt gemaakt en ik wil...

* [Extern bureaublad voor een cloud service-exemplaar instellen][remotedesktop]
* [Een project voor de Service in de Cloud implementeren][deploy]

Ik gebruik Visual Studio en ik wil...

* [Een nieuwe cloudservice maken][vs_create]
* [Een bestaande cloudservice configureren][vs_reconfigure]
* [Een project voor de Service in de Cloud implementeren][vs_deploy]
* [Extern bureaublad voor een cloud service-exemplaar instellen][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop-new-portal.md
[vs_remote]: cloud-services-role-enable-remote-desktop-visual-studio.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md

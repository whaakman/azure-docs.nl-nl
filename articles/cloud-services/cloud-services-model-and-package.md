---
title: Wat is er een Service in de Cloud-model en het pakket | Microsoft Docs
description: Hierin worden de cloud-servicemodel (csdef, cscfg-bestand) en het pakket (.cspkg) in Azure
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: b7210c944e2f99aacdc2f554409552007286c5da
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Wat is het Cloudservice-model en hoe ik dit pakket?
Een cloudservice is gemaakt op basis van drie onderdelen, de servicedefinitie *(.csdef)*, de serviceconfiguratie *(.cscfg)*, en een servicepakket *(.cspkg)*. Zowel de **ServiceDefinition.csdef** en **ServiceConfig.cscfg** bestanden zijn XML- en beschrijven de structuur van de cloudservice en de manier waarop deze geconfigureerd; genoemd op het model. De **ServicePackage.cspkg** is een zipbestand dat is gegenereerd op basis van de **ServiceDefinition.csdef** en onder andere bevat alle vereiste binaire gebaseerde afhankelijkheden. Azure maakt een cloudservice van zowel de **ServicePackage.cspkg** en de **ServiceConfig.cscfg**.

Zodra de cloudservice in Azure wordt uitgevoerd, kunt u het configureren via de **ServiceConfig.cscfg** bestand, maar kan de definitie niet wijzigen.

## <a name="what-would-you-like-to-know-more-about"></a>Wat wilt u meer weten?
* Ik wil meer informatie over de [ServiceDefinition.csdef](#csdef) en [ServiceConfig.cscfg](#cscfg) bestanden.
* Ik heb al weet over die, geef me [enkele voorbeelden](#next-steps) op ik kunt configureren.
* Ik wil maken de [ServicePackage.cspkg](#cspkg).
* Ik gebruik Visual Studio en ik wil...
  * [Maak een cloudservice][vs_create]
  * [Configureren van een bestaande cloudservice][vs_reconfigure]
  * [Een Cloud Service-project implementeert][vs_deploy]
  * [Extern bureaublad in een cloud service-exemplaar][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
De **ServiceDefinition.csdef** bestand geeft de instellingen die door Azure worden gebruikt om een cloudservice te configureren. De [Azure Service definitie Schema (.csdef-bestand)](https://msdn.microsoft.com/library/azure/ee758711.aspx) biedt de toegestane notatie voor een servicedefinitiebestand. Het volgende voorbeeld ziet u de instellingen die kunnen worden gedefinieerd voor de Web- en werkrollen rollen:

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

U kunt verwijzen naar de [Service definitie Schema](https://msdn.microsoft.com/library/azure/ee758711.aspx) voor een beter inzicht in het XML-schema hier gebruikt, maar hier volgt een korte uitleg van enkele van de elementen:

**Sites**  
Bevat de definities voor websites of web-toepassingen die worden gehost in IIS7.

**InputEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt voor het contact op met de cloudservice.

**InternalEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt door de rolinstanties met elkaar communiceren.

**ConfigurationSettings**  
Bevat de instellingsdefinities voor onderdelen van een specifieke rol.

**Certificaten**  
Bevat de definities voor certificaten die nodig zijn voor een rol. Het vorige codevoorbeeld ziet u een certificaat dat wordt gebruikt voor de configuratie van de Azure-verbinding.

**LocalResources**  
Bevat de definities voor resources voor lokale opslag. Een lokale opslagbron is een gereserveerde map op het bestandssysteem van de virtuele machine waarop een exemplaar van een rol wordt uitgevoerd.

**Invoer**  
Bevat de definities voor geïmporteerde modules. Het vorige codevoorbeeld toont de modules voor verbinding met extern bureaublad en Azure-verbinding.

**Opstarten**  
Bevat taken die worden uitgevoerd wanneer de functie wordt gestart. De taken zijn gedefinieerd in een .cmd of uitvoerbaar bestand.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
De configuratie van de instellingen voor uw cloudservice wordt bepaald door de waarden in de **ServiceConfiguration.cscfg** bestand. U opgeven het aantal exemplaren die u wilt implementeren voor elke rol in dit bestand. De waarden voor de configuratie-instellingen die u hebt gedefinieerd in het servicedefinitiebestand worden toegevoegd aan het configuratiebestand van de service. De vingerafdrukken instellen voor van beheercertificaten die gekoppeld aan de cloudservice zijn worden ook toegevoegd aan het bestand. De [configuratieschema voor Azure-Service (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx) biedt de toegestane notatie voor een service-configuratiebestand.

Het configuratiebestand van de service niet wordt verpakt met de toepassing, maar is geüpload naar Azure als een afzonderlijk bestand en wordt gebruikt om de cloudservice te configureren. U kunt een nieuwe service-configuratiebestand zonder uw cloudservice opnieuw uploaden. De configuratiewaarden voor de cloudservice kunnen worden gewijzigd terwijl de cloudservice wordt uitgevoerd. Het volgende voorbeeld ziet u de configuratie-instellingen die kunnen worden gedefinieerd voor de Web- en werkrollen rollen:

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

U kunt verwijzen naar de [Service configuratieschema](https://msdn.microsoft.com/library/azure/ee758710.aspx) voor een beter inzicht in het XML-schema hier gebruikt, maar hier volgt een korte uitleg van de elementen:

**Exemplaren**  
Hiermee configureert u het aantal actieve exemplaren voor de rol. Als u wilt voorkomen dat uw cloudservice mogelijk niet beschikbaar tijdens upgrades, wordt het aanbevolen dat u meer dan één exemplaar van de functies van uw web gerichte implementeert. Door het implementeren van meer dan één exemplaar u voldoet aan de richtlijnen in de [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), die wordt gegarandeerd dat externe connectiviteit van 99,95% voor internetgerichte rollen wanneer twee of meer rolinstanties zijn geïmplementeerd voor een service.

**ConfigurationSettings**  
Hiermee configureert u de instellingen voor de exemplaren die worden uitgevoerd voor een rol. De naam van de `<Setting>` elementen moeten overeenkomen met de instellingsdefinities in het servicedefinitiebestand.

**Certificaten**  
Hiermee configureert u de certificaten die worden gebruikt door de service. Het vorige codevoorbeeld toont het definiëren van het certificaat voor de Remote Access-module. De waarde van de *vingerafdruk* kenmerk moet worden ingesteld op de vingerafdruk van het certificaat te gebruiken.

<p/>

> [!NOTE]
> De vingerafdruk van het certificaat kan worden toegevoegd aan het configuratiebestand met een teksteditor. Of de waarde mag worden toegevoegd aan de **certificaten** tabblad van de **eigenschappen** pagina van de rol in Visual Studio.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Poorten voor rolinstanties definiëren
Azure kan slechts één toegangspunt aan een Webrol. Dit betekent dat alle verkeer vindt plaats via één IP-adres. U kunt uw websites als u wilt een poort delen door het configureren van de host-header voor de aanvraag naar de juiste locatie configureren. U kunt ook uw toepassingen om te luisteren naar bekende poorten van het IP-adres configureren.

Het volgende voorbeeld ziet u de configuratie voor een Webrol aan een toepassing website en webtoepassingen. De website is geconfigureerd als de standaardlocatie voor de vermelding op poort 80 en de webtoepassingen zijn geconfigureerd voor het ontvangen van aanvragen van een andere host-header 'mail.mysite.cloudapp.net' wordt genoemd.

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
U kunt de configuratie van uw cloudservice bijwerken terwijl deze wordt uitgevoerd in Azure, zonder de service offline te halen. Om te wijzigen van configuratie-informatie, kunt u uploaden van een nieuw configuratiebestand of de configuratiebestand in-place bewerken en toepassen op uw actieve service. De volgende wijzigingen kunnen worden aangebracht in de configuratie van een service:

* **De waarden van configuratie-instellingen wijzigen**  
  Wanneer een configuratie-instelling wijzigingen een rolinstantie kunt de wijziging toepassen terwijl het exemplaar online is of het exemplaar probleemloos recyclen en toepassen van de wijziging tijdens het exemplaar offline is.
* **Het wijzigen van de service-topologie van de rolinstanties**  
  Wijzigingen in de netwerktopologie hebben geen invloed op de exemplaren die worden uitgevoerd, behalve wanneer een exemplaar wordt verwijderd. Alle resterende exemplaren in het algemeen hoeft niet te worden gerecycled; u kunt echter recyclen rolinstanties in reactie op een topologiewijziging in de.
* **Vingerafdruk van het certificaat wijzigen**  
  U kunt een certificaat alleen bijwerken wanneer een rolinstantie offline is. Als een certificaat is toegevoegd, verwijderd of gewijzigd terwijl een rolinstantie online is, wordt Azure probleemloos het exemplaar offline voor het bijwerken van het certificaat en weer online brengen nadat de wijziging aangebracht is.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Wijzigingen in de configuratie met gebeurtenissen van de Runtime-Service verwerken
De [Azure-Runtime-bibliotheek](https://msdn.microsoft.com/library/azure/mt419365.aspx) bevat de [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) naamruimte, waardoor de klassen voor interactie met de Azure-omgeving van een rol. De [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) klasse definieert de volgende gebeurtenissen die worden gegenereerd voor en na een wijziging in de configuratie:

* **[Het wijzigen van](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) gebeurtenis**  
  Dit gebeurt voordat de wijziging in de configuratie wordt toegepast op een opgegeven exemplaar van een rol zodat u de kans te nemen de exemplaren van de rol, indien nodig.
* **[Gewijzigd](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) gebeurtenis**  
  Deze gebeurtenis treedt op nadat de wijziging in de configuratie is toegepast op een opgegeven exemplaar van een rol.

> [!NOTE]
> Omdat wijzigingen certificaat altijd de exemplaren van een functie offline halen, komen ze niet de gebeurtenissen RoleEnvironment.Changing of RoleEnvironment.Changed verhogen.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Als u wilt een toepassing als een cloudservice in Azure implementeert, moet u eerst inpakken van de toepassing in de juiste indeling. U kunt de **CSPack** opdrachtregelprogramma (geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/)) voor het maken van het pakketbestand als alternatief voor Visual Studio.

**CSPack** de inhoud van het servicedefinitiebestand en serviceconfiguratiebestand gebruikt voor het definiëren van de inhoud van het pakket. **CSPack** genereert een toepassingspakketbestand (.cspkg) die u uploaden naar Azure met behulp van kunt de [Azure-portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Standaard het pakket met de naam `[ServiceDefinitionFileName].cspkg`, maar u kunt een andere naam opgeven met behulp van de `/out` optie **CSPack**.

**CSPack** bevindt zich op  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (op windows) is beschikbaar door het uitvoeren van de **Microsoft Azure-opdrachtprompt** snelkoppeling die is geïnstalleerd met de SDK.  
> 
> Voer het programma CSPack.exe zelfstandig Zie documentatie over de mogelijke switches en opdrachten.
> 
> 

<p />

> [!TIP]
> Uitvoeren van uw cloudservice lokaal in de **Microsoft Azure Compute-Emulator**, gebruiken de **/copyonly** optie. Deze optie kopieert de binaire bestanden voor de toepassing aan de indeling van een directory waaruit ze kunnen worden uitgevoerd in de rekenemulator.
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a>Van de voorbeeldopdracht aan het pakket een cloudservice
Het volgende voorbeeld wordt een toepassingspakket met de informatie voor een Webrol. De opdracht geeft u het servicedefinitiebestand moet worden gebruikt, de map waar de binaire bestanden kunnen worden gevonden, en de naam van het pakket.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Als de toepassing zowel een Webrol en een werkrol bevat, worden de volgende opdracht wordt gebruikt:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Waar worden de variabelen als volgt gedefinieerd:

| Variabele | Waarde |
| --- | --- |
| \[DirectoryName\] |De submap onder de hoofdmap van het project die het csdef-bestand van de Azure-project bevat. |
| \[ServiceDefinition\] |De naam van het servicedefinitiebestand. Standaard is dit bestand ServiceDefinition.csdef naam. |
| \[OutputFileName\] |De naam van het gegenereerde pakketbestand. Dit is normaal gesproken ingesteld op de naam van de toepassing. Als er geen bestandsnaam is opgegeven, wordt het toepassingspakket gemaakt als \[ApplicationName\].cspkg. |
| \[Rolnaam\] |De naam van de rol, zoals gedefinieerd in het servicedefinitiebestand. |
| \[RoleBinariesDirectory] |De locatie van de binaire bestanden voor de rol. |
| \[VirtualPath\] |De fysieke mappen voor elke virtuele pad dat is gedefinieerd in de sectie Sites van de definitie van de service. |
| \[PhysicalPath\] |De fysieke mappen van de inhoud voor elke virtuele pad dat is gedefinieerd in het siteknooppunt van de definitie van de service. |
| \[RoleAssemblyName\] |De naam van het binaire bestand voor de rol. |

## <a name="next-steps"></a>Volgende stappen
Ik ben een cloud service-pakket maken en ik wil...

* [Extern bureaublad instellen voor een cloud service-exemplaar][remotedesktop]
* [Een Cloud Service-project implementeert][deploy]

Ik gebruik Visual Studio en ik wil...

* [Maak een nieuwe cloudservice][vs_create]
* [Configureren van een bestaande cloudservice][vs_reconfigure]
* [Een Cloud Service-project implementeert][vs_deploy]
* [Extern bureaublad instellen voor een cloud service-exemplaar][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop-new-portal.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md

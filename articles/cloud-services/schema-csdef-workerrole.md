---
title: Azure Cloud Services-naam Def. WorkerRole Schema | Microsoft Docs
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 41cd46bc-c479-43fa-96e5-d6c83e4e6d89
caps.latest.revision: "55"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 0171c7254db5855f0eccd19ae9938249d8966edd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-workerrole-schema"></a>Azure Cloud Services-definitie WorkerRole Schema
De functie Azure worker is een rol die is nuttig voor algemene ontwikkeling en achtergrondverwerking voor een Webrol kan uitvoeren.

De standaardextensie voor het servicedefinitiebestand is csdef.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Basic-service definitie schema voor een werkrol.
De basisindeling van het servicedefinitiebestand met een werkrol is als volgt.

```xml
<ServiceDefinition …>
  <WorkerRole name="<worker-role-name>" vmsize="<worker-role-size>" enableNativeCodeExecution="[true|false]">
    <Certificates>
      <Certificate name="<certificate-name>" storeLocation="[CurrentUser|LocalMachine]" storeName="[My|Root|CA|Trust|Disallow|TrustedPeople|TrustedPublisher|AuthRoot|AddressBook|<custom-store>" />
    </Certificates>
    <ConfigurationSettings>
      <Setting name="<setting-name>" />
    </ConfigurationSettings>
    <Endpoints>
      <InputEndpoint name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<local-port-number>" port="<port-number>" certificate="<certificate-name>" loadBalancerProbe="<load-balancer-probe-name>" />
      <InternalEndpoint name="<internal-endpoint-name" protocol="[http|tcp|udp|any]" port="<port-number>">
         <FixedPort port="<port-number>"/>
         <FixedPortRange min="<minium-port-number>" max="<maximum-port-number>"/>
      </InternalEndpoint>
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">
         <AllocatePublicPortFrom>
            <FixedPortRange min="<minium-port-number>" max="<maximum-port-number>"/>
         </AllocatePublicPortFrom>
      </InstanceInputEndpoint>
    </Endpoints>
    <Imports>
      <Import moduleName="[RemoteAccess|RemoteForwarder|Diagnostics]"/>
    </Imports>
    <LocalResources>
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    </LocalResources>
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    <Runtime executionContext="[limited|elevated]">
      <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
      </Environment>
      <EntryPoint>
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>
         <ProgramEntryPoint commandLine="<application>" setReadyOnProcessStart="[true|false]" "/>
      </EntryPoint>
    </Runtime>
    <Startup priority="<for-internal-use-only>”>
      <Task commandLine="" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">
        <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
        </Environment>
      </Task>
    </Startup>
    <Contents>
      <Content destination="<destination-folder-name>" >
        <SourceDirectory path="<local-source-directory>" />
      </Content>
    </Contents>
  </WorkerRole>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-elementen
Het servicedefinitiebestand bevat deze elementen, uitgebreid beschreven in de volgende secties in dit onderwerp:

[WorkerRole](#WorkerRole)

[ConfigurationSettings](#ConfigurationSettings)

[Instelling](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Eindpunten](#Endpoints)

[Invoereindpunt](#InputEndpoint)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Certificaten](#Certificates)

[Certificaat](#Certificate)

[Invoer](#Imports)

[Importeren](#Import)

[Runtime](#Runtime)

[Omgeving](#Environment)

[EntryPoint](#EntryPoint)

[NetFxEntryPoint](#NetFxEntryPoint)

[ProgramEntryPoint](#ProgramEntryPoint)

[Variabele](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[Opstarten](#Startup)

[Taak](#Task)

[Inhoud](#Contents)

[Inhoud](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WorkerRole"></a>WorkerRole
De `WorkerRole` element wordt een rol die is nuttig voor algemene ontwikkeling en achtergrondverwerking kan uitvoeren voor een Webrol beschreven. Een service kan nul of meer werkrollen bevatten.

De volgende tabel beschrijft de kenmerken van de `WorkerRole` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. De naam voor de werkrol. De naam van de rol moet uniek zijn.|
|enableNativeCodeExecution|Booleaanse waarde|Optioneel. De standaardwaarde is `true`; systeemeigen code worden uitgevoerd en volledig vertrouwen zijn standaard ingeschakeld. Dit kenmerk instelt op `false` uit te schakelen van de uitvoering van de native code voor de werkrol en gebruik in plaats daarvan Azure gedeeltelijk vertrouwen.|
|vmsize|Tekenreeks|Optioneel. Stel deze waarde wijzigen van de grootte van de virtuele machine die wordt toegewezen aan deze rol. De standaardwaarde is `Small`. Zie voor een lijst met grootten van mogelijke virtuele machines en hun kenmerken, [grootten van virtuele machines voor Cloud Services](cloud-services-sizes-specs.md).|

##  <a name="ConfigurationSettings"></a>ConfigurationSettings
De `ConfigurationSettings` -element worden beschreven voor het verzamelen van configuratie-instellingen voor een werkrol. Dit element is het bovenliggende lid van de `Setting` element.

##  <a name="Setting"></a>Instelling
De `Setting` -element een naam en waarde-paar dat Hiermee geeft u een configuratie-instelling voor een exemplaar van een rol worden beschreven.

De volgende tabel beschrijft de kenmerken van de `Setting` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een unieke naam voor de configuratie-instelling.|

De configuratie-instellingen voor een functie zijn naam- en waardeparen die zijn gedeclareerd in het servicedefinitiebestand en in het configuratiebestand van de service.

##  <a name="LocalResources"></a>LocalResources
De `LocalResources` element beschrijft de verzameling van resources voor lokale opslag voor een werkrol. Dit element is het bovenliggende lid van de `LocalStorage` element.

##  <a name="LocalStorage"></a>LocalStorage
De `LocalStorage` element identificeert een lokale opslag-bron die ruimte bestandssysteem voor de service tijdens runtime biedt. Een rol kan nul of meer resources voor lokale opslag definiëren.

> [!NOTE]
>  De `LocalStorage` element kan worden weergegeven als een onderliggend element van de `WorkerRole` element ter ondersteuning van compatibiliteit met eerdere versies van de Azure SDK.

De volgende tabel beschrijft de kenmerken van de `LocalStorage` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een unieke naam voor het lokale archief.|
|cleanOnRoleRecycle|Booleaanse waarde|Optioneel. Hiermee wordt aangegeven of het lokale archief worden gewist wanneer de functie opnieuw wordt opgestart. Standaardwaarde is `true`.|
|sizeInMb|int|Optioneel. De gewenste hoeveelheid opslagruimte toewijzen voor het lokale archief, in MB. Als niet wordt opgegeven, is de toegewezen ruimte voor de opslag van de standaard 100 MB. De minimale hoeveelheid opslagruimte die kan worden toegewezen, is 1 MB.<br /><br /> De maximale grootte van de lokale bronnen is afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [grootten van virtuele machines voor Cloud Services](cloud-services-sizes-specs.md).|

De naam van de map die is toegewezen aan de lokale opslagbronnen overeenkomt met de opgegeven waarde voor het kenmerk name.

##  <a name="Endpoints"></a>Eindpunten
De `Endpoints` element beschrijft de verzameling van interne invoer (extern) en exemplaar invoer eindpunten voor een rol. Dit element is het bovenliggende lid van de `InputEndpoint`, `InternalEndpoint`, en `InstanceInputEndpoint` elementen.

Invoer- en interne eindpunten worden afzonderlijk toegewezen. Een service kan een totaal van 25 invoer, intern, en invoer eindpunten die kunnen worden verdeeld over de 25 rollen toegestaan in een service-exemplaar. Bijvoorbeeld, als 5 functies hebt u 5 invoereindpunten per rol kunt toewijzen of kunt u 25 invoereindpunten voor één rol toewijzen of kunt u 1 invoereindpunt elke 25-rollen toewijzen.

> [!NOTE]
>  Elke rol geïmplementeerd vereist één exemplaar per rol. De standaardwaarde voor een abonnement ingericht is beperkt tot 20 kernen en dus is beperkt tot 20 exemplaren van een rol. Als uw toepassing meer exemplaren vereist dan is standaard ingericht Zie [facturering, beheer van abonnementen en Quota ondersteuning](https://azure.microsoft.com/support/options/) voor meer informatie over het verhogen van uw quotum.

##  <a name="InputEndpoint"></a>Invoereindpunt
De `InputEndpoint` -element worden beschreven van een extern eindpunt naar een werkrol.

U kunt meerdere eindpunten die deel uitmaken van een combinatie van HTTP, HTTPS, UDP en TCP-eindpunten definiëren. U kunt een ander poortnummer dat u voor een invoereindpunt kiest opgeven, maar de poortnummers die is opgegeven voor elke rol in de service moeten uniek zijn. Bijvoorbeeld, als u opgeeft dat een rol maakt gebruik van poort 80 voor HTTP en poort 443 voor HTTPS, kan vervolgens u een tweede rol poort 8080 voor HTTP en poort 8043 gebruikt voor HTTPS.

De volgende tabel beschrijft de kenmerken van de `InputEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een unieke naam voor het externe eindpunt.|
|Protocol|Tekenreeks|Vereist. Het transportprotocol voor het externe eindpunt. Mogelijke waarden zijn voor een werkrol `HTTP`, `HTTPS`, `UDP`, of `TCP`.|
|poort|int|Vereist. De poort voor het externe eindpunt. U kunt elk gewenst poortnummer opgeven, maar de poortnummers die is opgegeven voor elke rol in de service moeten uniek zijn.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).|
|certificaat|Tekenreeks|Vereist voor een HTTPS-eindpunt. De naam van een certificaat dat is gedefinieerd door een `Certificate` element.|
|LokalePoort|int|Optioneel. Hiermee geeft u een poort die wordt gebruikt voor interne verbindingen op het eindpunt. De `localPort` -kenmerk wijst de externe poort op het eindpunt met een interne poort op een rol. Dit is nuttig in scenario's waarin een rol met een interne onderdeel op een poort communiceren moet dat verschilt van de versie die wordt blootgesteld extern.<br /><br /> Als niet wordt opgegeven, de waarde van `localPort` is hetzelfde als de `port` kenmerk. Stel de waarde van `localPort` naar ' * ' voor het automatisch toewijzen van een niet-toegewezen poort die wordt gedetecteerd met behulp van de runtime-API.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).<br /><br /> De `localPort` kenmerk is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.|
|ignoreRoleInstanceStatus|Booleaanse waarde|Optioneel. Wanneer de waarde van dit kenmerk is ingesteld op `true`, wordt de status van een service genegeerd en het eindpunt wordt niet door de load balancer verwijderd. De waarde instelt op `true` nuttig voor het opsporen van bezet exemplaren van een service. De standaardwaarde is `false`. **Opmerking:** een eindpunt kan nog steeds verkeer ontvangen zelfs wanneer de functie niet gereed is.|
|loadBalancerProbe|Tekenreeks|Optioneel. De naam van de load balancer-test die zijn gekoppeld aan het invoereindpunt. Zie voor meer informatie [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md).|

##  <a name="InternalEndpoint"></a>InternalEndpoint
De `InternalEndpoint` -element worden beschreven van een interne eindpunt een werkrol. Een intern eindpunt is alleen beschikbaar voor andere rolinstanties die wordt uitgevoerd in de service; het is niet beschikbaar is voor clients buiten de service. Een werkrol kan maximaal vijf HTTP, UDP of TCP interne eindpunten hebben.

De volgende tabel beschrijft de kenmerken van de `InternalEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een unieke naam voor het interne eindpunt.|
|Protocol|Tekenreeks|Vereist. Het protocol-transport voor het interne eindpunt. Mogelijke waarden zijn `HTTP`, `TCP`, `UDP`, of `ANY`.<br /><br /> Een waarde van `ANY` geeft aan dat elk protocol elke poort is toegestaan.|
|poort|int|Optioneel. De poort die wordt gebruikt voor interne taakverdeling verbindingen op het eindpunt. Een eindpunt maakt gebruik van twee poorten gelijke taakverdeling. De poort die wordt gebruikt voor het openbare IP-adres en de poort op de privé IP-adres gebruikt. Dit zijn meestal worden ze ingesteld op dezelfde, maar u kunt verschillende poorten gebruiken.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).<br /><br /> De `Port` kenmerk is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.|

##  <a name="InstanceInputEndpoint"></a>InstanceInputEndpoint
De `InstanceInputEndpoint` element beschrijft een invoereindpunt exemplaar naar een werkrol. Een invoereindpunt exemplaar is gekoppeld aan een specifieke rol-exemplaar met behulp van poort doorsturen in de load balancer. Elk exemplaar invoer-eindpunt is toegewezen aan een specifieke poort uit een reeks mogelijke poorten. Dit element is het bovenliggende lid van de `AllocatePublicPortFrom` element.

De `InstanceInputEndpoint` element is alleen beschikbaar met de versie van de Azure SDK 1.7 of hoger.

De volgende tabel beschrijft de kenmerken van de `InstanceInputEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een unieke naam voor het eindpunt.|
|LokalePoort|int|Vereist. Hiermee geeft u de interne poort op die alle rolinstanties luistert naar om binnenkomend verkeer doorgestuurd ontvangen van de load balancer. Mogelijke waarden liggen tussen 1 en 65535 op.|
|Protocol|Tekenreeks|Vereist. Het protocol-transport voor het interne eindpunt. Mogelijke waarden zijn `udp` en `tcp`. Gebruik `tcp` voor http/https-verkeer op basis van.|

##  <a name="AllocatePublicPortFrom"></a>AllocatePublicPortFrom
De `AllocatePublicPortFrom` -element worden beschreven van het bereik van openbare poort die door externe klanten kan worden gebruikt voor toegang tot een invoereindpunt voor elk exemplaar. Het poortnummer van de openbare (VIP) is uit dit bereik toegewezen en toegewezen aan elke afzonderlijke rol exemplaar eindpunt tijdens de implementatie van de tenant en update. Dit element is het bovenliggende lid van de `FixedPortRange` element.

De `AllocatePublicPortFrom` element is alleen beschikbaar met de versie van de Azure SDK 1.7 of hoger.

##  <a name="FixedPort"></a>FixedPort
De `FixedPort` element geeft de poort voor het interne eindpunt, zodat load balanced verbindingen op het eindpunt.

De `FixedPort` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPort` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|poort|int|Vereist. De poort voor het interne eindpunt. Dit heeft hetzelfde effect als de instelling voor de `FixedPortRange` min en max op dezelfde poort.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).|

##  <a name="FixedPortRange"></a>FixedPortRange
De `FixedPortRange` element geeft u het bereik van poorten die zijn toegewezen aan de interne eindpunt of invoereindpunt exemplaar en de poort voor load gebruikt sets met gelijke taakverdeling verbindingen op het eindpunt.

> [!NOTE]
>  De `FixedPortRange` element werkt er anders, afhankelijk van het element waarvan deze deel uitmaakt. Wanneer de `FixedPortRange` element heeft de `InternalEndpoint` element, deze wordt geopend. alle poorten op de load balancer binnen het bereik van de kenmerken min en max voor alle virtuele machines waarop de rol wordt uitgevoerd. Wanneer de `FixedPortRange` element heeft de `InstanceInputEndpoint` element, deze wordt geopend. slechts één poort binnen het bereik van de kenmerken min en max op elke virtuele machine waarop de functie wordt uitgevoerd.

De `FixedPortRange` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPortRange` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|min.|int|Vereist. De minimale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).|
|Maximum aantal|Tekenreeks|Vereist. De maximale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, op (Azure SDK 1.7 of hoger).|

##  <a name="Certificates"></a>Certificaten
De `Certificates` -element worden beschreven voor het verzamelen van certificaten voor een werkrol. Dit element is het bovenliggende lid van de `Certificate` element. Een rol kan een onbeperkt aantal gekoppelde certificaten hebben. Zie voor meer informatie over het gebruik van het element certificaten [wijzigen van het bestand servicedefinitie met een certificaat](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a>Certificaat
De `Certificate` -element worden beschreven van een certificaat dat is gekoppeld aan een werkrol.

De volgende tabel beschrijft de kenmerken van de `Certificate` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. Een naam voor dit certificaat wordt gebruikt om te verwijzen naar deze wanneer deze gekoppeld aan een HTTPS is `InputEndpoint` element.|
|storeLocation|Tekenreeks|Vereist. De locatie van het certificaatarchief waar dit certificaat kan worden gevonden op de lokale computer. Mogelijke waarden zijn `CurrentUser` en `LocalMachine`.|
|storeName|Tekenreeks|Vereist. De naam van het certificaatarchief waar dit certificaat bevindt zich op de lokale computer. Mogelijke waarden zijn de namen van de ingebouwde store `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, of de naam van een aangepast archief. Als de naam van een aangepast archief is opgegeven, wordt de store automatisch gemaakt.|
|permissionLevel|Tekenreeks|Optioneel. Hiermee geeft u de machtigingen voor gegeven aan de rol wordt verwerkt. Als u wilt dat alleen verhoogde processen kunnen toegang tot de persoonlijke sleutel, geeft u vervolgens `elevated` machtiging. `limitedOrElevated`machtiging kan alle rol processen voor toegang tot de persoonlijke sleutel. Mogelijke waarden zijn `limitedOrElevated` en `elevated`. De standaardwaarde is `limitedOrElevated`.|

##  <a name="Imports"></a>Invoer
De `Imports` -element worden beschreven van een verzameling van import-modules voor een werkrol die onderdelen aan het gastbesturingssysteem toevoegen. Dit element is het bovenliggende lid van de `Import` element. Dit element is optioneel en een rol kan slechts één runtime blok hebben.

De `Imports` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

##  <a name="Import"></a>Importeren
De `Import` element bevat een module die u wilt toevoegen aan het gastbesturingssysteem.

De `Import` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Import` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Modulenaam|Tekenreeks|Vereist. De naam van de module te importeren. Geldige importeren modules zijn:<br /><br /> -RemoteAccess<br />-RemoteForwarder<br />-Diagnostische gegevens<br /><br /> De modules RemoteAccess en RemoteForwarder kunnen u uw rolexemplaar voor extern bureaublad-verbindingen configureren. Zie voor meer informatie [verbinding met extern bureaublad inschakelen](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> De Diagnostics-module kunt u voor het verzamelen van diagnostische gegevens voor een rolexemplaar van|

##  <a name="Runtime"></a>Runtime
De `Runtime` -element worden beschreven van een verzameling instellingen voor de omgevingsvariabelen voor een werkrol die de runtime-omgeving van het proces van het Azure-host te bepalen. Dit element is het bovenliggende lid van de `Environment` element. Dit element is optioneel en een rol kan slechts één runtime blok hebben.

De `Runtime` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Runtime` element:

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|executionContext|Tekenreeks|Optioneel. Hiermee geeft u de context waarin het functie-proces wordt gestart. De standaardcontext is `limited`.<br /><br /> -   `limited`: Het proces wordt gestart zonder Administrator-bevoegdheden.<br />-   `elevated`: Het proces wordt gestart met administratorbevoegdheden.|

##  <a name="Environment"></a>Omgeving
De `Environment` -element worden beschreven van een verzameling instellingen voor de omgevingsvariabelen voor een werkrol. Dit element is het bovenliggende lid van de `Variable` element. Een rol kan een onbeperkt aantal omgeving variabelen set hebben.

##  <a name="Variable"></a>Variabele
De `Variable` element geeft een omgevingsvariabele worden ingesteld in het gastbesturingssysteem.

De `Variable` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Variable` element:

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|Tekenreeks|Vereist. De naam van de omgevingsvariabele in te stellen.|
|waarde|Tekenreeks|Optioneel. De waarde in te stellen voor de omgevingsvariabele. U moet een kenmerkwaarde opnemen of een `RoleInstanceValue` element.|

##  <a name="RoleInstanceValue"></a>RoleInstanceValue
De `RoleInstanceValue` element geeft het xPath van waaruit de waarde van de variabele ophalen.

De volgende tabel beschrijft de kenmerken van de `RoleInstanceValue` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|XPath|Tekenreeks|Optioneel. Het locatiepad van de implementatie-instellingen voor het exemplaar. Zie voor meer informatie [configuratievariabelen met XPath](cloud-services-role-config-xpath.md).<br /><br /> U moet een kenmerkwaarde opnemen of een `RoleInstanceValue` element.|

##  <a name="EntryPoint"></a>EntryPoint
De `EntryPoint` element geeft het toegangspunt voor een rol. Dit element is het bovenliggende lid van de `NetFxEntryPoint` elementen. Deze elementen kunnen u een toepassing dan de standaard WaWorkerHost.exe om te fungeren als de functie-ingangspunt opgeven.

De `EntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="NetFxEntryPoint"></a>NetFxEntryPoint
De `NetFxEntryPoint` element Hiermee geeft u het programma voor een rol worden uitgevoerd.

> [!NOTE]
>  De `NetFxEntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `NetFxEntryPoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|AssemblyName|Tekenreeks|Vereist. Het pad en de naam van de assembly met het toegangspunt. Het pad is ten opzichte van de map  **\\%ROLEROOT%\Approot** (Geef geen  **\\%ROLEROOT%\Approot** in `commandLine`, wordt ervan uitgegaan). **% ROLEROOT %** een omgevingsvariabele blijft door Azure en de locatie van de basismap voor uw rol vertegenwoordigt. De  **\\%ROLEROOT%\Approot** map vertegenwoordigt de toepassingsmap voor uw rol.|
|targetFrameworkVersion|Tekenreeks|Vereist. De versie van .NET framework waarop de assembly is gebouwd. Bijvoorbeeld `targetFrameworkVersion="v4.0"`.|

##  <a name="ProgramEntryPoint"></a>ProgramEntryPoint
De `ProgramEntryPoint` element Hiermee geeft u het programma voor een rol worden uitgevoerd. De `ProgramEntryPoint` element kunt u een programma-ingangspunt niet op basis van een .NET-assembly opgeven.

> [!NOTE]
>  De `ProgramEntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `ProgramEntryPoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|commandLine|Tekenreeks|Vereist. Het pad, de bestandsnaam en de opdrachtregelargumenten op van het programma uit te voeren. Het pad is ten opzichte van de map **%ROLEROOT%\Approot** (Geef geen **%ROLEROOT%\Approot** in de opdrachtregel, wordt ervan uitgegaan). **% ROLEROOT %** een omgevingsvariabele blijft door Azure en de locatie van de basismap voor uw rol vertegenwoordigt. De **%ROLEROOT%\Approot** map vertegenwoordigt de toepassingsmap voor uw rol.<br /><br /> Als het programma wordt beëindigd, wordt de rol gerecycled, dus doorgaans het programma voort te zetten, in plaats van een programma dat alleen wordt gestart en wordt uitgevoerd een eindige taak wordt ingesteld.|
|setReadyOnProcessStart|Booleaanse waarde|Vereist. Hiermee geeft u op of de rolinstantie wordt gewacht op het programma vanaf de opdrachtregel om aan te geven dat deze is gestart. Deze waarde moet worden ingesteld op `true` op dit moment. De waarde instelt op `false` is gereserveerd voor toekomstig gebruik.|

##  <a name="Startup"></a>Opstarten
De `Startup` -element worden beschreven van een verzameling taken die worden uitgevoerd wanneer de functie wordt gestart. Dit element is het bovenliggende lid van de `Variable` element. Zie voor meer informatie over het gebruik van de rol starten van de taken [het starten van de taken configureren](cloud-services-startup-tasks.md). Dit element is optioneel en een rol kan slechts één opstarten blok hebben.

De volgende tabel beschrijft het kenmerk van de `Startup` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Prioriteit|int|Alleen voor intern gebruik.|

##  <a name="Task"></a>Taak
De `Task` element bepaalt starten van de taak die uitgevoerd wordt wanneer de functie wordt gestart. Starten van de taken kunnen worden gebruikt voor taken uitvoeren die voorbereiden van de rol voor deze installatie uitvoeren software-onderdelen of andere toepassingen uitvoert. Taken uitvoeren in de volgorde waarin ze worden weergegeven in de `Startup` elementblok.

De `Task` element is alleen beschikbaar via de Azure SDK versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Task` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|commandLine|Tekenreeks|Vereist. Een script, zoals een CMD-bestand met de opdrachten om uit te voeren. Starten van de opdracht en de batch-bestanden moeten worden opgeslagen in de indeling met ANSI. Bestandsindelingen die een bytevolgordemarkering-markering aan het begin van het bestand instellen verwerkt niet correct.|
|executionContext|Tekenreeks|Hiermee geeft u de context waarin het script wordt uitgevoerd.<br /><br /> -   `limited`[Standaard]: uitvoeren met de dezelfde machtigingen als de rol die host is het proces.<br />-   `elevated`: Uitvoeren met administrator-bevoegdheden.|
|taskType|Tekenreeks|Hiermee geeft u het uitvoeringsgedrag van de opdracht.<br /><br /> -   `simple`[Standaard] – systeem wacht tot de taak om af te sluiten voordat andere taken worden gestart.<br />-   `background`– Systeem wacht niet totdat de taak om af te sluiten.<br />-   `foreground`– Vergelijkbaar met de achtergrond, behalve de rol niet totdat alle taken van de voorgrond afsluit opnieuw wordt opgestart.|

##  <a name="Contents"></a>Inhoud
De `Contents` -element worden beschreven voor het verzamelen van inhoud voor een werkrol. Dit element is het bovenliggende lid van de `Content` element.

De `Contents` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="Content"></a>Inhoud
De `Content` element wordt gedefinieerd voor de bronlocatie van de inhoud wordt gekopieerd naar de virtuele machine van Azure en het doelpad waarnaar deze is gekopieerd.

De `Content` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `Content` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Bestemming|Tekenreeks|Vereist. De locatie op de virtuele machine van Azure die de inhoud wordt geplaatst. Deze locatie is ten opzichte van de map **%ROLEROOT%\Approot**.|

Dit element is het bovenliggende element van de `SourceDirectory` element.

##  <a name="SourceDirectory"></a>SourceDirectory
De `SourceDirectory` element wordt gedefinieerd voor de lokale map waarin de inhoud wordt gekopieerd. Dit element gebruiken om op te geven van de lokale inhoud kopiëren naar de virtuele machine van Azure.

De `SourceDirectory` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `SourceDirectory` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Pad|Tekenreeks|Vereist. Relatief of absoluut pad van een lokale map waarvan de inhoud wordt gekopieerd naar de virtuele machine van Azure. Uitbreiding van omgevingsvariabelen in het pad van de wordt ondersteund.|

## <a name="see-also"></a>Zie ook
[Cloud-Service (klassiek) definitie Schema](schema-csdef-file.md)
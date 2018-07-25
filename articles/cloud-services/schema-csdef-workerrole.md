---
title: Azure Cloud Services Def. WorkerRole Schema | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 04/14/2015
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 41cd46bc-c479-43fa-96e5-d6c83e4e6d89
caps.latest.revision: 55
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 2e487bd3fda787cf9f869cc352de4c97d5c1678b
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002174"
---
# <a name="azure-cloud-services-definition-workerrole-schema"></a>Azure Cloud Services-definitie WorkerRole Schema
De Azure-werkrol is een rol die is handig voor het ontwikkelen van gegeneraliseerde en achtergrondverwerking kan uitvoeren voor een Webrol.

De standaardextensie voor het servicedefinitiebestand is .csdef.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Definitieschema voor Basic-service voor een werkrol.
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
Het servicedefinitiebestand bevat deze elementen in detail in de volgende secties in dit onderwerp beschreven:

[WorkerRole](#WorkerRole)

[ConfigurationSettings](#ConfigurationSettings)

[Instelling](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Eindpunten](#Endpoints)

[InputEndpoint](#InputEndpoint)

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

##  <a name="WorkerRole"></a> WorkerRole
De `WorkerRole` element beschrijft een rol die is handig voor het ontwikkelen van gegeneraliseerde en achtergrondverwerking kan uitvoeren voor een Webrol. Een service kan nul of meer werkrollen bevatten.

De volgende tabel beschrijft de kenmerken van de `WorkerRole` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. De naam voor de werkrol. De naam van de rol moet uniek zijn.|
|enableNativeCodeExecution|booleaans|Optioneel. De standaardwaarde is `true`; systeemeigen uitvoering van code en volledig vertrouwen zijn standaard ingeschakeld. Dit kenmerk instelt op `false` systeemeigen code uitvoeren voor de werkrol uitschakelen, en gebruik in plaats daarvan Azure gedeeltelijk vertrouwen.|
|vmsize|tekenreeks|Optioneel. Stel deze waarde wijzigen van de grootte van de virtuele machine die wordt toegewezen aan deze rol. De standaardwaarde is `Small`. Zie voor een lijst van mogelijke VM-grootten en hun kenmerken, [VM-groottes voor Cloud Services](cloud-services-sizes-specs.md).|

##  <a name="ConfigurationSettings"></a> ConfigurationSettings
De `ConfigurationSettings` -element worden beschreven voor het verzamelen van configuratie-instellingen voor een werkrol. Dit element is het bovenliggende lid van de `Setting` element.

##  <a name="Setting"></a> Instelling
De `Setting` -element een naam en waarde-paar dat Hiermee geeft u een configuratie-instelling voor een exemplaar van een rol worden beschreven.

De volgende tabel beschrijft de kenmerken van de `Setting` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een unieke naam voor de configuratie-instelling.|

De configuratie-instellingen voor een rol zijn naam / waarde-paren die zijn gedeclareerd in het servicedefinitiebestand en stel in het configuratiebestand van de service.

##  <a name="LocalResources"></a> LocalResources
De `LocalResources` element beschrijving van de verzameling van resources voor lokale opslag voor een werkrol. Dit element is het bovenliggende lid van de `LocalStorage` element.

##  <a name="LocalStorage"></a> LocalStorage
De `LocalStorage` element identificeert een lokale opslag-resource waarmee de ruimte die bestandssysteem voor de service tijdens runtime. Een rol kan nul of meer resources voor lokale opslag definiëren.

> [!NOTE]
>  De `LocalStorage` element kan worden weergegeven als een onderliggende site van de `WorkerRole` element ter ondersteuning van compatibiliteit met eerdere versies van de Azure SDK.

De volgende tabel beschrijft de kenmerken van de `LocalStorage` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een unieke naam voor het lokale archief.|
|cleanOnRoleRecycle|booleaans|Optioneel. Geeft aan of het lokale archief moet worden verwijderd wanneer de rol opnieuw wordt opgestart. Standaardwaarde is `true`.|
|sizeInMb|int|Optioneel. De gewenste hoeveelheid opslagruimte om toe te wijzen voor het lokale archief, in MB. Indien niet opgegeven, is de standaard-opslagruimte toegewezen 100 MB. De minimale hoeveelheid opslagruimte die kan worden toegewezen is 1 MB.<br /><br /> De maximale grootte van de lokale bronnen is afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie, [VM-groottes voor Cloud Services](cloud-services-sizes-specs.md).|

De naam van de map die is toegewezen aan de lokale opslag-resource komt overeen met de opgegeven waarde voor het kenmerk name.

##  <a name="Endpoints"></a> Eindpunten
De `Endpoints` -element worden beschreven voor het verzamelen van invoer (extern), interne en exemplaar invoer-eindpunten voor een rol. Dit element is het bovenliggende lid van de `InputEndpoint`, `InternalEndpoint`, en `InstanceInputEndpoint` elementen.

Invoer- en interne eindpunten worden afzonderlijk toegewezen. Een service kan een totaal van 25 invoer, intern, en exemplaar invoer eindpunten die kunnen worden toegewezen voor de 25 rollen toegestaan in een service. Bijvoorbeeld, als 5 rollen hebben u 5 invoereindpunten per rol kunt toewijzen of kunt u 25 invoereindpunten voor één rol toewijzen of kunt u 1 invoereindpunt elke 25 rollen toewijzen.

> [!NOTE]
>  Elke rol die geïmplementeerd is één exemplaar per rol vereist. De standaardwaarde voor een abonnement ingericht is beperkt tot 20 kernen en dus is beperkt tot 20 exemplaren van een rol. Als uw toepassing meer instanties vereist, dan wordt geleverd door de standaard Zie provisioning [ondersteuning voor facturering, Abonnementsbeheer en Quota](https://azure.microsoft.com/support/options/) voor meer informatie over het vergroten van uw quotum.

##  <a name="InputEndpoint"></a> Invoereindpunt
De `InputEndpoint` -element een extern eindpunt naar een werkrol worden beschreven.

U kunt meerdere eindpunten die zijn een combinatie van HTTP, HTTPS, UDP en TCP-eindpunten definiëren. U kunt een ander poortnummer die u voor een invoereindpunt kiest opgeven, maar de poortnummers die is opgegeven voor elke rol in de service moet uniek zijn. Bijvoorbeeld, als u opgeeft dat een functie maakt gebruik van poort 80 voor HTTP en poort 443 voor HTTPS, kan vervolgens geeft u een tweede functie poort 8080 voor HTTP en poort 8043 gebruikt voor HTTPS.

De volgende tabel beschrijft de kenmerken van de `InputEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een unieke naam voor het externe eindpunt.|
|protocol|tekenreeks|Vereist. Het transportprotocol voor het externe eindpunt. Mogelijke waarden zijn voor een werkrol `HTTP`, `HTTPS`, `UDP`, of `TCP`.|
|poort|int|Vereist. De poort voor het externe eindpunt. U kunt opgeven dat een ander poortnummer die u kiest, maar de poortnummers die is opgegeven voor elke rol in de service moet uniek zijn.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|
|certificaat|tekenreeks|Vereist voor een HTTPS-eindpunt. De naam van een certificaat dat is gedefinieerd door een `Certificate` element.|
|localPort|int|Optioneel. Hiermee geeft u een poort die wordt gebruikt voor interne verbindingen op het eindpunt. De `localPort` kenmerk wijst de externe poort op het eindpunt met een interne poort op een rol. Dit is handig in scenario's waarin een rol met een interne onderdeel op een andere poort communiceren moet dat verschilt van de naam die wordt blootgesteld extern.<br /><br /> Indien niet opgegeven, de waarde van `localPort` is hetzelfde als de `port` kenmerk. Stel de waarde van `localPort` naar ' * ' voor het automatisch toewijzen van een niet-toegewezen poort die kan worden gedetecteerd met behulp van de runtime-API.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).<br /><br /> De `localPort` kenmerk is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.|
|ignoreRoleInstanceStatus|booleaans|Optioneel. Als de waarde van dit kenmerk is ingesteld op `true`, de status van een service wordt genegeerd en het eindpunt niet worden verwijderd door de load balancer. De waarde instelt op `true` handig voor foutopsporing bezet exemplaren van een service. De standaardwaarde is `false`. **Opmerking:** een eindpunt kan nog steeds verkeer ontvangen, zelfs wanneer de rol niet gereed is.|
|loadBalancerProbe|tekenreeks|Optioneel. De naam van de load balancer-test die zijn gekoppeld aan het invoereindpunt. Zie voor meer informatie, [LoadBalancerProbe Schema](schema-csdef-loadbalancerprobe.md).|

##  <a name="InternalEndpoint"></a> InternalEndpoint
De `InternalEndpoint` -element een intern eindpunt dat aan een werkrol worden beschreven. Een intern eindpunt dat is alleen beschikbaar voor andere rolinstanties in de service. het is niet beschikbaar is voor clients buiten de service. Een werkrol kan maximaal vijf HTTP, UDP of TCP interne eindpunten hebben.

De volgende tabel beschrijft de kenmerken van de `InternalEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een unieke naam voor het interne eindpunt.|
|protocol|tekenreeks|Vereist. Het transportprotocol voor het interne eindpunt. Mogelijke waarden zijn `HTTP`, `TCP`, `UDP`, of `ANY`.<br /><br /> Een waarde van `ANY` geeft aan dat elk protocol, een willekeurige poort is toegestaan.|
|poort|int|Optioneel. De poort die wordt gebruikt voor interne taakverdeling verbindingen op het eindpunt. Een gelijke taakverdeling eindpunt maakt gebruik van twee poorten. De poort die wordt gebruikt voor het openbare IP-adres en de poort die wordt gebruikt op het privé IP-adres. Deze zijn meestal worden ze ingesteld op dezelfde, maar u kunt kiezen voor het gebruik van verschillende poorten.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).<br /><br /> De `Port` kenmerk is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.|

##  <a name="InstanceInputEndpoint"></a> InstanceInputEndpoint
De `InstanceInputEndpoint` -element een invoereindpunt exemplaar naar een werkrol worden beschreven. Een invoereindpunt exemplaar is gekoppeld aan een specifieke rol-exemplaar met behulp van doorsturen via poort in de load balancer. Een invoereindpunt voor elke instantie wordt toegewezen aan een specifieke poort uit een verscheidenheid aan mogelijke poorten. Dit element is het bovenliggende lid van de `AllocatePublicPortFrom` element.

De `InstanceInputEndpoint` element is alleen beschikbaar via de Azure SDK-versie 1.7 of hoger.

De volgende tabel beschrijft de kenmerken van de `InstanceInputEndpoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een unieke naam voor het eindpunt.|
|localPort|int|Vereist. Hiermee geeft u de interne poort die door alle rolinstanties aan om te kunnen ontvangen van binnenkomend verkeer doorgestuurd van de load balancer luistert. Mogelijke waarden liggen tussen 1 en 65535 op.|
|protocol|tekenreeks|Vereist. Het transportprotocol voor het interne eindpunt. Mogelijke waarden zijn `udp` en `tcp`. Gebruik `tcp` voor http/https op basis van verkeer.|

##  <a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom
De `AllocatePublicPortFrom` -element het bereik van openbare poort die door externe klanten kan worden gebruikt voor toegang tot een invoereindpunt voor elke instantie worden beschreven. Het openbare (VIP)-poortnummer dat is toegewezen uit dit bereik en toegewezen aan elke afzonderlijke rol exemplaar eindpunt tijdens de implementatie van de tenant en update. Dit element is het bovenliggende lid van de `FixedPortRange` element.

De `AllocatePublicPortFrom` element is alleen beschikbaar via de Azure SDK-versie 1.7 of hoger.

##  <a name="FixedPort"></a> FixedPort
De `FixedPort` element Hiermee geeft u de poort voor het interne eindpunt, welke Hiermee wordt de met gelijke taakverdeling verbindingen op het eindpunt.

De `FixedPort` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPort` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|poort|int|Vereist. De poort voor het interne eindpunt. Dit heeft hetzelfde effect als de instelling de `FixedPortRange` min en max op dezelfde poort.<br /><br /> Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|

##  <a name="FixedPortRange"></a> FixedPortRange
De `FixedPortRange` element Hiermee geeft u het bereik van poorten die zijn toegewezen aan de interne eindpunt of een invoereindpunt exemplaar en stelt de poort die wordt gebruikt voor load balanced verbindingen op het eindpunt.

> [!NOTE]
>  De `FixedPortRange` element werkt anders, afhankelijk van het element waarin deze zich bevindt. Wanneer de `FixedPortRange` element heeft de `InternalEndpoint` -element, wordt alle poorten op de load balancer binnen het bereik van de kenmerken min en max voor alle virtuele machines waarop de rol wordt uitgevoerd. Wanneer de `FixedPortRange` element heeft de `InstanceInputEndpoint` -element, wordt slechts één poort binnen het bereik van de kenmerken min en max op elke virtuele machine met de rol.

De `FixedPortRange` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `FixedPortRange` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|min.|int|Vereist. De minimale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|
|max|tekenreeks|Vereist. De maximale poort in het bereik. Mogelijke waarden liggen tussen 1 en 65535, inclusief (Azure SDK versie 1.7 of hoger).|

##  <a name="Certificates"></a> Certificaten
De `Certificates` -element worden beschreven voor het verzamelen van certificaten voor een werkrol. Dit element is het bovenliggende lid van de `Certificate` element. Een rol kan een onbeperkt aantal gekoppelde certificaten hebben. Zie voor meer informatie over het gebruik van het element certificaten [wijzigen van de definitie van de Service van het bestand met een certificaat](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a> Certificaat
De `Certificate` -element een certificaat dat is gekoppeld aan een werkrol worden beschreven.

De volgende tabel beschrijft de kenmerken van de `Certificate` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. Een naam op voor dit certificaat, dat wordt gebruikt om te verwijzen naar deze wanneer deze gekoppeld aan een HTTPS is `InputEndpoint` element.|
|onder andere storeLocation|tekenreeks|Vereist. De locatie van het certificaatarchief waar dit certificaat kan worden gevonden op de lokale computer. Mogelijke waarden zijn `CurrentUser` en `LocalMachine`.|
|storeName|tekenreeks|Vereist. De naam van het certificaatarchief waar dit certificaat bevindt zich op de lokale computer. Mogelijke waarden zijn onder andere de namen van de ingebouwde store `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, of de naam van een aangepast archief. Als de naam van een aangepast archief is opgegeven, wordt de store automatisch gemaakt.|
|permissionLevel|tekenreeks|Optioneel. Hiermee geeft u de machtigingen voor toegang krijgen tot de rol-processen. Als u wilt dat alleen met verhoogde bevoegdheden processen kunnen toegang tot de persoonlijke sleutel, geeft u vervolgens `elevated` machtiging. `limitedOrElevated` machtiging is mogelijk alle rol processen voor toegang tot de persoonlijke sleutel. Mogelijke waarden zijn `limitedOrElevated` en `elevated`. De standaardwaarde is `limitedOrElevated`.|

##  <a name="Imports"></a> Invoer
De `Imports` -element een verzameling importeren van modules voor een werkrol die onderdelen aan het gastbesturingssysteem toevoegen worden beschreven. Dit element is het bovenliggende lid van de `Import` element. Dit element is optioneel en een rol kan slechts één runtime-blok hebben.

De `Imports` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

##  <a name="Import"></a> Importeren
De `Import` element Hiermee geeft u een module toevoegen aan het gastbesturingssysteem.

De `Import` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Import` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|moduleName|tekenreeks|Vereist. De naam van de module te importeren. Geldige import-modules zijn:<br /><br /> -RemoteAccess<br />-RemoteForwarder<br />-Diagnostics<br /><br /> De modules RemoteAccess en RemoteForwarder kunnen u uw rolinstantie voor verbindingen met extern bureaublad configureren. Zie voor meer informatie [verbinding met extern bureaublad inschakelen](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> De module voor diagnostische gegevens kunt u voor het verzamelen van diagnostische gegevens voor een rolinstantie|

##  <a name="Runtime"></a> Runtime
De `Runtime` element een verzameling van omgeving variabele instellingen voor een werkrol die de runtime-omgeving van het Azure-host-proces bepalen wordt beschreven. Dit element is het bovenliggende lid van de `Environment` element. Dit element is optioneel en een rol kan slechts één runtime-blok hebben.

De `Runtime` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Runtime` element:

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|executionContext|tekenreeks|Optioneel. Hiermee geeft u de context waarin het proces van de rol wordt gestart. De standaardcontext is `limited`.<br /><br /> -   `limited` : Het proces wordt gestart zonder Administrator-bevoegdheden.<br />-   `elevated` : Het proces wordt gestart met Administrator-bevoegdheden.|

##  <a name="Environment"></a> omgeving
De `Environment` -element een verzameling van instellingen voor de omgevingsvariabelen voor een werkrol worden beschreven. Dit element is het bovenliggende lid van de `Variable` element. Een rol mogelijk een willekeurig aantal omgevingsvariabelen die worden ingesteld.

##  <a name="Variable"></a> Variabele
De `Variable` element Hiermee geeft u een omgevingsvariabele instellen in het gastbesturingssysteem.

De `Variable` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Variable` element:

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|naam|tekenreeks|Vereist. De naam van de omgevingsvariabele om in te stellen.|
|waarde|tekenreeks|Optioneel. De waarde om in te stellen voor de omgevingsvariabele. U moet een waardekenmerk bevatten of een `RoleInstanceValue` element.|

##  <a name="RoleInstanceValue"></a> RoleInstanceValue
De `RoleInstanceValue` element Hiermee geeft u het xPath waaruit u wilt ophalen van de waarde van de variabele.

De volgende tabel beschrijft de kenmerken van de `RoleInstanceValue` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|XPath|tekenreeks|Optioneel. Pad naar de locatie van de implementatie-instellingen voor het exemplaar. Zie voor meer informatie, [configuratievariabelen met XPath](cloud-services-role-config-xpath.md).<br /><br /> U moet een waardekenmerk bevatten of een `RoleInstanceValue` element.|

##  <a name="EntryPoint"></a> EntryPoint
De `EntryPoint` element Hiermee geeft u het toegangspunt voor een rol. Dit element is het bovenliggende lid van de `NetFxEntryPoint` elementen. Deze elementen kunnen u een toepassing dan de standaard WaWorkerHost.exe om te fungeren als de functie-ingangspunt opgeven.

De `EntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="NetFxEntryPoint"></a> NetFxEntryPoint
De `NetFxEntryPoint` element Hiermee geeft u het programma voor een rol uit te voeren.

> [!NOTE]
>  De `NetFxEntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `NetFxEntryPoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Assembly-naam|tekenreeks|Vereist. Het pad en de naam van de assembly waarin het toegangspunt. Het pad is ten opzichte van de map  **\\%ROLEROOT%\Approot** (Geef geen  **\\%ROLEROOT%\Approot** in `commandLine`, wordt ervan uitgegaan). **% ROLEROOT** wordt een omgevingsvariabele onderhouden door Azure en staat voor de locatie van de basismap voor uw rol. De  **\\%ROLEROOT%\Approot** map vertegenwoordigt de map voor uw rol.|
|targetFrameworkVersion|tekenreeks|Vereist. De versie van .NET framework waarop de assembly is gemaakt. Bijvoorbeeld `targetFrameworkVersion="v4.0"`.|

##  <a name="ProgramEntryPoint"></a> ProgramEntryPoint
De `ProgramEntryPoint` element Hiermee geeft u het programma voor een rol uit te voeren. De `ProgramEntryPoint` element kunt u een programma ingangspunt gebruikt dat niet is gebaseerd op een .NET-assembly opgeven.

> [!NOTE]
>  De `ProgramEntryPoint` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `ProgramEntryPoint` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Opdrachtregel|tekenreeks|Vereist. Het pad, bestandsnaam en opdrachtregelargumenten op van het programma uit te voeren. Het pad is ten opzichte van de map **%ROLEROOT%\Approot** (Geef geen **%ROLEROOT%\Approot** in de opdrachtregel, wordt ervan uitgegaan). **% ROLEROOT** wordt een omgevingsvariabele onderhouden door Azure en staat voor de locatie van de basismap voor uw rol. De **%ROLEROOT%\Approot** map vertegenwoordigt de map voor uw rol.<br /><br /> Als het programma wordt beëindigd, wordt de rol gerecycled, dus meestal het programma om door te gaan om uit te voeren, in plaats van een programma dat alleen wordt gestart en wordt uitgevoerd een eindige taak instellen.|
|setReadyOnProcessStart|booleaans|Vereist. Hiermee geeft u op of de rolinstantie wacht tot het programma vanaf de opdrachtregel om aan te geven dat deze is gestart. Deze waarde moet worden ingesteld op `true` op dit moment. Als de waarde instelt op `false` is gereserveerd voor toekomstig gebruik.|

##  <a name="Startup"></a> Opstarten
De `Startup` -element worden beschreven van een verzameling taken die worden uitgevoerd wanneer de rol wordt gestart. Dit element mag het bovenliggende lid van de `Variable` element. Zie voor meer informatie over het gebruik van de rol opstarttaken [opstarttaken configureren](cloud-services-startup-tasks.md). Dit element is optioneel en een rol kan slechts één opstarten blok hebben.

De volgende tabel beschrijft het kenmerk van de `Startup` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|prioriteit|int|Alleen voor intern gebruik.|

##  <a name="Task"></a> Taak
De `Task` element Hiermee geeft u de opstarttaak die plaatsvindt wanneer de rol wordt gestart. Opstarttaken kunnen worden gebruikt om de taken uitvoeren die voorbereiden van de rol voor deze installatie van software-onderdelen of andere toepassingen worden uitgevoerd. Taken uitvoeren in de volgorde waarin ze worden weergegeven in de `Startup` element blokkeren.

De `Task` element is alleen beschikbaar via de Azure SDK-versie 1.3 of hoger.

De volgende tabel beschrijft de kenmerken van de `Task` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|Opdrachtregel|tekenreeks|Vereist. Een script, zoals een CMD-bestand met de opdrachten om uit te voeren. Opdracht en batch opstartbestanden moeten worden opgeslagen in de ANSI-indeling. Bestandsindelingen die een markering bytevolgorde die is ingesteld op het begin van het bestand verwerkt niet correct.|
|executionContext|tekenreeks|Hiermee geeft u de context waarin het script wordt uitgevoerd.<br /><br /> -   `limited` [Standaard] – uitvoeren met de dezelfde bevoegdheden als de rol die het proces host.<br />-   `elevated` – Uitgevoerd met beheerdersbevoegdheden.|
|taskType|tekenreeks|Hiermee geeft u het uitvoeringsgedrag van de opdracht.<br /><br /> -   `simple` [Standaard] – systeem wacht tot de taak om af te sluiten voordat andere taken worden gestart.<br />-   `background` – Systeem wacht niet totdat de taak om af te sluiten.<br />-   `foreground` – Vergelijkbaar met de achtergrond, behalve de rol niet totdat alle taken van voorgrond afsluit opnieuw wordt opgestart.|

##  <a name="Contents"></a> Inhoud
De `Contents` -element worden beschreven voor het verzamelen van inhoud voor een werkrol. Dit element is het bovenliggende lid van de `Content` element.

De `Contents` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

##  <a name="Content"></a> Inhoud
De `Content` element wordt gedefinieerd voor de locatie van inhoud wordt gekopieerd naar de virtuele machine van Azure en het doelpad waarnaar deze is gekopieerd.

De `Content` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `Content` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|destination|tekenreeks|Vereist. De locatie op de Azure-machine waarop de inhoud wordt geplaatst. Deze locatie is ten opzichte van de map **%ROLEROOT%\Approot**.|

Dit element is het bovenliggende element van de `SourceDirectory` element.

##  <a name="SourceDirectory"></a> SourceDirectory
De `SourceDirectory` element wordt gedefinieerd voor de lokale map waarin inhoud wordt gekopieerd. Dit element gebruiken om op te geven van de lokale inhoud kopiëren naar de virtuele machine van Azure.

De `SourceDirectory` element is alleen beschikbaar via de Azure SDK-versie 1.5 of hoger.

De volgende tabel beschrijft de kenmerken van de `SourceDirectory` element.

| Kenmerk | Type | Beschrijving |
| --------- | ---- | ----------- |
|pad|tekenreeks|Vereist. Relatieve of absolute pad van een lokale map waarvan de inhoud wordt gekopieerd naar de virtuele machine van Azure. Uitbreiding van omgevingsvariabelen in het mappad wordt ondersteund.|

## <a name="see-also"></a>Zie ook
[(Klassiek) Definitieschema voor cloud Service](schema-csdef-file.md)
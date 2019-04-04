---
title: Overzichtskaart van XPath voor configuratie van cloud Services-rol | Microsoft Docs
description: De verschillende XPath-instellingen kunt u in de configuratie van cloud service rol instellingen doorgeven als een omgevingsvariabele.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: 53a262af421dd986e6b70af173a6e8b3f7c06f64
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918430"
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Configuratie-rolinstellingen beschikbaar als een omgevingsvariabele met XPath
U kunt in de cloud service werknemer of web rol servicedefinitiebestand runtime-configuratiewaarden weergeven als omgevingsvariabelen. De volgende waarden voor XPath worden ondersteund (die overeenkomen met de API-waarden).

Deze XPath-waarden zijn ook beschikbaar via de [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee773173(v=azure.100)) bibliotheek. 

## <a name="app-running-in-emulator"></a>App die wordt uitgevoerd in de emulator
Geeft aan dat de app wordt uitgevoerd in de emulator.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@emulated" |
| Code |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>Implementatie-id
Hiermee haalt u de implementatie-ID voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@id" |
| Code |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Rol-id
Hiermee haalt u de huidige rol-ID voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Code |var-id = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Domein bijwerken
Hiermee haalt u het updatedomein van het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>Foutdomein
Hiermee haalt u het foutdomein van het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Rolnaam
Hiermee haalt u de naam van de rol van de exemplaren.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Code |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Configuratie-instelling
Hiermee haalt de waarde van de opgegeven configuratie-instelling.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code |de instelling var = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |

## <a name="local-storage-path"></a>Pad van de lokale opslag
Hiermee haalt u het pad van de lokale opslag voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Grootte van de lokale opslag
Hiermee haalt u de grootte van de lokale opslag voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Eindpunt-protocol
Hiermee haalt u de eindpunt-protocol voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code |var b RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protocol. |

## <a name="endpoint-ip"></a>Eindpunt IP
Hiermee haalt u de IP-adres van het opgegeven eindpunt.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code |var adres RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Poort van het eindpunt
Hiermee haalt u de poort van het eindpunt voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code |var poort RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |

## <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld van een werkrol die een opstarttaak met een omgevingsvariabele met de naam maakt `TestIsEmulated` ingesteld op de [ @emulated xpath-waarde](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) bestand.

Maak een [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) pakket.

Schakel [extern bureaublad](cloud-services-role-enable-remote-desktop-new-portal.md) voor een rol.


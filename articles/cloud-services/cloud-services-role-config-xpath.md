---
title: Cloud Services-rol config XPath-referentieoverzicht | Microsoft Docs
description: De verschillende XPath-instellingen kunt u in de cloud van de rol serviceconfiguratie instellingen weergeven als een omgevingsvariabele.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: fd6efac829d3fd9e2840362b8d2ff423add566d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Configuratie-rolinstellingen weergeven als een omgevingsvariabele met XPath
In de cloud service worker of web rol servicedefinitiebestand kan configuratiewaarden runtime worden blootgesteld als omgevingsvariabelen. De volgende waarden voor XPath worden ondersteund (die overeenkomen met de API-waarden).

Deze XPath-waarden zijn ook beschikbaar via de [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) bibliotheek. 

## <a name="app-running-in-emulator"></a>App uitgevoerd in de emulator
Hiermee wordt aangegeven dat de app wordt uitgevoerd in de emulator.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@emulated" |
| Code |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>Implementatie-ID
Hiermee haalt u de implementatie-ID voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@id" |
| Code |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Rol-ID
Hiermee haalt u de huidige rol-ID voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Code |var-id = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Bijwerken van domein
Hiermee haalt u het updatedomein van het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>Foutdomein
Het foutdomein van het exemplaar wordt opgehaald.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Rolnaam
Haalt de naam van de rol van de exemplaren.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Code |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Configuratie-instelling
Haalt de waarde van de opgegeven configuratie-instelling.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value' |
| Code |de instelling var = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |

## <a name="local-storage-path"></a>Pad voor lokale opslag
Het pad voor lokale opslag voor het exemplaar wordt opgehaald.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path' |
| Code |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>De grootte van de lokale opslag
Haalt de grootte van de lokale opslag voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB' |
| Code |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Protocol voor eindpunt
Haalt de eindpunt-protocol voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/eindpunten/het eindpunt [@name= '1']/@protocol' |
| Code |var b RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 = ']. Protocol; |

## <a name="endpoint-ip"></a>Eindpunt IP
Het opgegeven eindpunt IP-adres opgehaald.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/eindpunten/het eindpunt [@name= '1']/@address' |
| Code |adres var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 = ']. IPEndpoint.Address |

## <a name="endpoint-port"></a>Eindpuntpoort
Haalt de eindpuntpoort voor het exemplaar.

| Type | Voorbeeld |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/CurrentInstance/eindpunten/het eindpunt [@name= '1']/@port' |
| Code |poort var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 = ']. IPEndpoint.Port; |

## <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld van een werkrol die u een taak starten met een omgevingsvariabele maakt `TestIsEmulated` ingesteld op de [ @emulated xpath-waarde](#app-running-in-emulator). 

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

Schakel [extern bureaublad](cloud-services-role-enable-remote-desktop.md) voor een rol.


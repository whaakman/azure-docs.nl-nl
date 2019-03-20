---
title: Azure Service Fabric-toepassingsgeheimen beheren | Microsoft Docs
description: Meer informatie over het beveiligen van geheime waarden in een Service Fabric-toepassing (platform-agnostische).
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: 77aa6d48f499a94f8fe2f3ef4e2444a34d3b3176
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57894730"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>Versleutelde geheimen in Service Fabric-toepassingen beheren
Deze handleiding doorloopt u de stappen voor het beheren van geheimen in een Service Fabric-toepassing. Geheimen kunnen gevoelige informatie, zoals storage-verbindingsreeksen, wachtwoorden, of andere waarden die niet moeten worden verwerkt als tekst zonder opmaak zijn.

Met behulp van versleutelde geheimen in een Service Fabric-toepassing bestaat uit drie stappen:
* Stel een versleutelingscertificaat en geheimen versleutelen.
* Versleutelde geheimen in een toepassing opgeven.
* Versleutelde geheimen van de servicecode ontsleutelen.

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>Instellen van een versleutelingscertificaat en geheimen versleutelen
Een versleutelingscertificaat instellen en gebruiken voor het versleutelen van geheimen varieert tussen Windows en Linux.
* [Stel een versleutelingscertificaat en geheimen op Windows-clusters te versleutelen.][secret-management-windows-specific-link]
* [Stel een versleutelingscertificaat en geheimen op Linux-clusters te versleutelen.][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>Versleutelde geheimen in een toepassing opgeven
De vorige stap wordt beschreven hoe u een geheim met een certificaat voor versleutelen en produceren van een tekenreeks met base 64-codering voor gebruik in een toepassing. Deze base-64 gecodeerde tekenreeks kan worden opgegeven als een versleutelde [parameter] [ parameters-link] in Settings.xml van een service of als een versleutelde [omgevingsvariabele] [ environment-variables-link] in ServiceManifest.xml van een service.

Geef een versleutelde [parameter] [ parameters-link] in van uw service Settings.xml-configuratiebestand met de `IsEncrypted` kenmerk ingesteld op `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
Geef een versleutelde [omgevingsvariabele] [ environment-variables-link] in ServiceManifest.xml-bestand van uw service met de `Type` kenmerk ingesteld op `Encrypted`:
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

### <a name="inject-application-secrets-into-application-instances"></a>Toepassingsgeheimen injecteren in instanties van een toepassing
Implementatie naar verschillende omgevingen moet in het ideale geval zijn als geautomatiseerde mogelijk. Dit kan worden bereikt door het uitvoeren van geheime codering in een build-omgeving en het geven van de versleutelde geheimen als parameters bij het maken van exemplaren van de toepassing.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Onderdrukbare parameters gebruiken in Settings.xml
Het configuratiebestand Settings.xml kunt onderdrukbare parameters die tijdens de aanmaak van de toepassing kunnen worden opgegeven. Gebruik de `MustOverride` kenmerk in plaats van een waarde voor een parameter op te geven:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Als u wilt overschrijven van waarden in Settings.xml, declareert u een onderdrukkingsparameter voor de service in ApplicationManifest.xml:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Nu de waarde kan worden opgegeven als een *parametr aplikace* bij het maken van een exemplaar van de toepassing. Het maken van exemplaar van een toepassing kan scripts worden gebruikt met behulp van PowerShell, of zijn geschreven in C#, zo eenvoudig worden geïntegreerd in een buildproces.

Met behulp van PowerShell, de parameter wordt doorgegeven aan de `New-ServiceFabricApplication` opdracht als een [hash-tabel](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Met behulp van C#, parameters voor de toepassing zijn opgegeven in een `ApplicationDescription` als een `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-encrypted-secrets-from-service-code"></a>Versleutelde geheimen van de servicecode decoderen
De API's voor toegang tot [parameters] [ parameters-link] en [omgevingsvariabelen] [ environment-variables-link] toestaan voor eenvoudig ontsleuteling van versleutelde waarden. Omdat de gecodeerde tekenreeks informatie over het certificaat wordt gebruikt voor versleuteling bevat, hoeft u niet handmatig opgeven van het certificaat. Het certificaat moet alleen worden geïnstalleerd op het knooppunt dat de service wordt uitgevoerd op.

```csharp
// Access decrypted parameters from Settings.xml
ConfigurationPackage configPackage = FabricRuntime.GetActivationContext().GetConfigurationPackageObject("Config");
bool MySecretIsEncrypted = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].IsEncrypted;
if (MySecretIsEncrypted)
{
    SecureString MySecretDecryptedValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue();
}

// Access decrypted environment variables from ServiceManifest.xml
// Note: you do not have to call any explicit API to decrypt the environment variable.
string MyEnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [toepassing en Servicebeveiliging](service-fabric-application-and-service-security.md)

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md

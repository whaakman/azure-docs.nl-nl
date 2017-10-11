---
title: Het beheren van geheimen in Service Fabric-toepassingen | Microsoft Docs
description: In dit artikel wordt beschreven hoe secure geheime waarden in een Service Fabric-toepassing.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: d71924cda8bb3bffbe221946d80dba150359e38e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a>Het beheren van geheimen in Service Fabric-toepassingen
Deze handleiding leert u de stappen voor het beheren van geheimen in een Service Fabric-toepassing. Geheimen mag gevoelige informatie, zoals de storage-verbindingsreeksen, wachtwoorden of andere waarden die niet moeten worden behandeld als tekst zonder opmaak.

Deze handleiding maakt gebruik van Azure Sleutelkluis sleutels en geheimen beheren. Echter, *met* geheimen in een toepassing is cloud platform-networkdirect naar toepassingen kunnen worden geïmplementeerd naar een cluster overal worden gehost. 

## <a name="overview"></a>Overzicht
De aanbevolen manier voor het beheren van configuratie-instellingen is via [service configuratiepakketten][config-package]. Configuratiepakketten zijn samengestelde en bij te werken via beheerde rollende upgrades met health-validatie en voor automatisch terugdraaien. Dit is voorkeur naar globale configuratie vermindert de kans op een globale Services uitvallen. Versleutelde geheimen zijn geen uitzondering. Service Fabric bevat ingebouwde functies voor het versleutelen en ontsleutelen van waarden in een pakket Settings.xml configuratiebestand met behulp van certificaatversleuteling.

Het volgende diagram illustreert de basisprincipes voor beheer van de geheime in een Service Fabric-toepassing:

![overzicht van geheime management][overview]

Er zijn vier hoofdstappen in deze overdracht:

1. Verkrijgen van een certificaat voor het uitwisselen van gegevens.
2. Installeer het certificaat in het cluster.
3. Versleutelen geheime waarden bij het implementeren van een toepassing met het certificaat en een service Settings.xml configuratiebestand injecteren.
4. Versleutelde waarden buiten Settings.xml lezen door te ontsleutelen met hetzelfde certificaat uitwisselen. 

[Azure Sleutelkluis] [ key-vault-get-started] wordt hier gebruikt als een locatie voor de veilige opslag voor certificaten en als een manier om certificaten zijn geïnstalleerd op de Service Fabric-clusters in Azure. Als u niet in Azure implementeert, hoeft u geen Sleutelkluis gebruiken voor het beheren van geheimen in Service Fabric-toepassingen.

## <a name="data-encipherment-certificate"></a>Gegevens uitwisselen certificaat
Een certificaat voor het uitwisselen van gegevens wordt uitsluitend gebruikt voor versleuteling en ontsleuteling van de configuratie van de waarden in een service Settings.xml en wordt niet gebruikt voor de verificatie of het ondertekenen van de gecodeerde tekst. Het certificaat moet voldoen aan de volgende vereisten:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* Het certificaat sleutelgebruik gegevenscodering (10) moet bevatten en mag geen serververificatie of clientverificatie bevatten. 
  
  Bij het maken van een zelfondertekend certificaat met behulp van PowerShell, bijvoorbeeld de `KeyUsage` vlag moet worden ingesteld op `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Installeer het certificaat in het cluster
Dit certificaat moet worden geïnstalleerd op elk knooppunt in het cluster. Deze wordt gebruikt tijdens runtime voor het ontsleutelen van waarden in een service Settings.xml zijn opgeslagen. Zie [het maken van een cluster met Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] voor installatie-instructies. 

## <a name="encrypt-application-secrets"></a>Toepassing geheimen versleutelen
De Service Fabric SDK bevat ingebouwde geheime versleuteling en ontsleuteling functies. Geheime waarden kunnen worden versleuteld op het moment van gebouwd ontsleuteld en gelezen via een programma in de servicecode. 

De volgende PowerShell-opdracht wordt gebruikt voor het versleutelen van een geheim. Met deze opdracht versleutelt alleen de waarde; Dit gebeurt **niet** Meld u aan de gecodeerde tekst. U moet hetzelfde certificaat uitwisselen die is geïnstalleerd in uw cluster voor het produceren van gecodeerde tekst voor geheime waarden gebruiken:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

De resulterende base 64-tekenreeks bevat zowel de geheime gecodeerde tekst, evenals informatie over het certificaat dat is gebruikt om ze te versleutelen.  De base-64 gecodeerde tekenreeks kan worden ingevoegd in een parameter in Settings.xml-configuratiebestand voor uw service met de `IsEncrypted` -kenmerk ingesteld op `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Toepassing geheimen invoeren in toepassingsexemplaren
In het ideale geval implementatie in verschillende omgevingen worden als geautomatiseerde mogelijk. Dit kan worden bereikt door geheime versleuteling uitvoeren in een build-omgeving en het geven van de versleutelde geheimen als parameters bij het maken van exemplaren van een toepassing.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Onderdrukbare parameters in Settings.xml gebruiken
Het configuratiebestand Settings.xml kunt overschrijfbare parameters die tijdens de aanmaak van de toepassing kunnen worden opgegeven. Gebruik de `MustOverride` kenmerk in plaats van een waarde voor een parameter op te geven:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

U kunt met het overschrijven van waarden in Settings.xml, declareren een onderdrukking-parameter voor de service in ApplicationManifest.xml:

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

Nu de waarde kan worden opgegeven als een *toepassing parameter* bij het maken van een exemplaar van de toepassing. Maken van een toepassingsexemplaar kan scripts worden gebruikt met behulp van PowerShell, of geschreven in C#, voor eenvoudige integratie in een buildproces.

Met behulp van PowerShell, wordt de parameter doorgegeven aan de `New-ServiceFabricApplication` opdracht als een [hash-tabel](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Met C#, parameters voor de toepassing zijn opgegeven in een `ApplicationDescription` als een `NameValueCollection`:

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

## <a name="decrypt-secrets-from-service-code"></a>Ontsleutelen van geheimen van servicecode
Services in Service Fabric onder netwerkservice standaard op Windows worden uitgevoerd en geen toegang hebt tot de certificaten zijn geïnstalleerd op het knooppunt zonder extra instellingen opgeven.

Wanneer met behulp van een certificaat voor het uitwisselen van gegevens, moet u ervoor dat netwerkservice of de gebruikersaccount van de service wordt uitgevoerd toegang heeft tot de persoonlijke sleutel van het certificaat. Service Fabric verwerkt door de toegang voor uw service automatisch te verlenen als u configureert om dit te doen. Deze configuratie kan worden gedaan in ApplicationManifest.xml door gebruikers- en beveiligingsbeleid voor certificaten te definiëren. In het volgende voorbeeld is de Netwerkservice-account lezen toegang krijgen tot een gedefinieerd door de vingerafdruk van certificaat:

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> Bij het kopiëren van de vingerafdruk van een certificaat uit de store-module certificate in Windows, wordt een onzichtbare teken aan het begin van de vingerafdruk tekenreeks geplaatst. Dit onzichtbaar teken kan een fout ontstaan bij het vinden van een certificaat met vingerafdruk, dus deze extra teken verwijderen.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Gebruik application geheimen in servicecode
De API voor het openen van configuratiewaarden uit Settings.xml in een configuratiepakket kunt u gemakkelijk te ontsleutelen van waarden die u hebt de `IsEncrypted` -kenmerk ingesteld op `true`. Aangezien de gecodeerde tekst informatie over het certificaat dat wordt gebruikt voor versleuteling bevat, hoeft u niet handmatig het certificaat niet vinden. Het certificaat moet alleen worden geïnstalleerd op het knooppunt dat de service wordt uitgevoerd op. Te roepen de `DecryptValue()` methode voor het ophalen van de oorspronkelijke geheime waarde:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [met toepassingen met andere machtigingen](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png

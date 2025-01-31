---
title: Kestrel gebruiken om een HTTPS-eindpunt toe te voegen aan een Service Fabric-app in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u met behulp van Kestrel een HTTPS-eindpunt toevoegt aan een ASP.NET Core front-end webservice en hoe u de toepassing implementeert in een cluster.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/17/2019
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 48dd09bf70e99adc250027df872266bea39a786b
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302412"
---
# <a name="tutorial-add-an-https-endpoint-to-an-aspnet-core-web-api-front-end-service-using-kestrel"></a>Zelfstudie: Een HTTPS-eindpunt toevoegen aan een front-end-service van ASP.NET Core Web-API met behulp van Kestrel

Deze zelfstudie is deel drie van een serie.  U leert hoe u HTTPS kunt inschakelen in een ASP.NET Core-service die wordt uitgevoerd in Service Fabric. Wanneer u klaar bent, hebt u een stemtoepassing met een ASP.NET Core web-front-end en HTTPS-functionaliteit die luistert op poort 443. Als u de stemtoepassing niet handmatig wilt maken in [Een .NET Service Fabric-toepassing bouwen](service-fabric-tutorial-deploy-app-to-party-cluster.md), kunt u [de broncode downloaden](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) voor de voltooide toepassing.

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * Een HTTPS-eindpunt in de service definiëren
> * Kestrel configureren voor gebruik van HTTPS
> * Het SSL-certificaat installeren op de externe clusterknooppunten
> * Netwerkservice toegang geven tot de persoonlijke sleutel van het certificaat
> * Poort 443 openen in de Azure-load balancer
> * De toepassing implementeren in een extern cluster

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een .NET Service Fabric-toepassing bouwen](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [De toepassing implementeren in een extern cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Een HTTPS-eindpunt toevoegen aan een front-end-service van ASP.NET Core
> * [CI/CD configureren met behulp van Azure-pijplijnen](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Installeer Visual Studio 2019](https://www.visualstudio.com/) versie 15.5 of hoger met de **Azure-ontwikkeling** en **ASP.NET en webontwikkeling** werkbelastingen.
* [Installeer de Service Fabric-SDK](service-fabric-get-started.md).

## <a name="obtain-a-certificate-or-create-a-self-signed-development-certificate"></a>Haal een certificaat op of maak een zelfondertekend ontwikkelingscertificaat.

Gebruik voor productietoepassingen een certificaat van een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority). U kunt voor ontwikkelings- en testdoeleinden een zelfondertekend certificaat maken en gebruiken. De Service Fabric SDK bevat het *CertSetup.ps1*-script, dat een zelfondertekend certificaat maakt en dat certificaat vervolgens importeert naar het `Cert:\LocalMachine\My`-certificaatarchief. Open een opdrachtprompt als beheerder en voer de volgende opdracht uit voor het maken van een certificaat met het onderwerp 'CN=mytestcert':

```powershell
PS C:\program files\microsoft sdks\service fabric\clustersetup\secure> .\CertSetup.ps1 -Install -CertSubjectName CN=mytestcert
```

Als u al een PFX-certificaatbestand hebt, voert u de volgende opdracht uit om het certificaat te importeren in het `Cert:\LocalMachine\My`-certificaatarchief:

```powershell

PS C:\mycertificates> Import-PfxCertificate -FilePath .\mysslcertificate.pfx -CertStoreLocation Cert:\LocalMachine\My -Password (ConvertTo-SecureString "!Passw0rd321" -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

## <a name="define-an-https-endpoint-in-the-service-manifest"></a>Een HTTPS-eindpunt in het servicemanifest definiëren

Start Visual Studio als een **beheerder** en open de Voting-oplossing. Open *VotingWeb/PackageRoot/ServiceManifest.xml* vanuit Solution Explorer. Het servicemanifest definieert de service-eindpunten.  Zoek de sectie **Eindpunten** en bewerk het bestaande eindpunt met de naam 'ServiceEndpoint'.  Wijzig de naam in 'EndpointHttps', stel het protocol in op *https*, het type op *Invoer* en het poortnummer op *443*.  Sla uw wijzigingen op.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="configure-kestrel-to-use-https"></a>Kestrel configureren voor gebruik van HTTPS

Open in Solution Explorer het bestand *VotingWeb/VotingWeb.cs*.  Configureer Kestrel voor gebruik van HTTPS en zoek naar het certificaat in het `Cert:\LocalMachine\My`-archief. Voeg het volgende toe met behulp van instructies:

```csharp
using System.Net;
using Microsoft.Extensions.Configuration;
using System.Security.Cryptography.X509Certificates;
```

Werk de `ServiceInstanceListener` bij voor gebruik van het nieuwe eindpunt *EndpointHttps* dat luistert op poort 443. Als u de webhost configureert om de Kestrel-server te gebruiken, moet u Kestrel configureren om te luisteren naar IPv6-adressen op alle netwerkinterfaces: `opt.Listen(IPAddress.IPv6Any, port, listenOptions => {...}`.

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Voeg ook de volgende methode toe zodat Kestrel het certificaat kan vinden in het `Cert:\LocalMachine\My`-archief met behulp van het onderwerp.  

Vervang '&lt;your_CN_value&gt;' door 'mytestcert' als u een zelfondertekend certificaat hebt gemaakt met de vorige PowerShell-opdracht, of gebruik de algemene naam van uw certificaat.

```csharp
private X509Certificate2 GetCertificateFromStore()
{
    var store = new X509Store(StoreName.My, StoreLocation.LocalMachine);
    try
    {
        store.Open(OpenFlags.ReadOnly);
        var certCollection = store.Certificates;
        var currentCerts = certCollection.Find(X509FindType.FindBySubjectDistinguishedName, "CN=<your_CN_value>", false);
        return currentCerts.Count == 0 ? null : currentCerts[0];
    }
    finally
    {
        store.Close();
    }
}
```

## <a name="give-network-service-access-to-the-certificates-private-key"></a>Netwerkservice toegang geven tot de persoonlijke sleutel van het certificaat

In de vorige stap hebt u het certificaat geïmporteerd in het `Cert:\LocalMachine\My`-archief op de ontwikkelcomputer.  Het account dat de service (standaard NETWORK SERVICE) nu expliciet geven toegang tot de persoonlijke sleutel van het certificaat. U kunt deze stap handmatig doen (met het hulpprogramma certlm.msc), maar het is beter om uit te voeren automatisch een PowerShell-script door [configureren van een opstartscript](service-fabric-run-script-at-service-startup.md) in de **SetupEntryPoint** van het servicemanifest.

### <a name="configure-the-service-setup-entry-point"></a>Het toegangspunt voor service-instellingen configureren

Open *VotingWeb/PackageRoot/ServiceManifest.xml* vanuit Solution Explorer.  Voeg in de sectie **CodePackage** het **SetupEntryPoint**-knooppunt toe en vervolgens een **ExeHost**-knooppunt.  Stel in **ExeHost** de optie **Programma** in op 'Setup.bat' en **WorkingFolder** op 'CodePackage'.  Wanneer de VotingWeb-service wordt gestart, wordt het Setup.bat-script uitgevoerd in de map CodePackage voordat VotingWeb.exe wordt gestart.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Setup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>

    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

### <a name="add-the-batch-and-powershell-setup-scripts"></a>De batch- en PowerShell-installatiescripts toevoegen

Als u PowerShell wilt uitvoeren vanaf het eindpunt **SetupEntryPoint**, kunt u PowerShell.exe uitvoeren in een batchbestand dat naar een PowerShell-bestand verwijst. Voeg eerst het batchbestand aan het serviceproject toe.  Klik in Solution Explorer met de rechtermuisknop op **VotingWeb**, selecteer **Toevoegen**->**Nieuw item** en voeg een nieuw bestand toe met de naam 'Setup.bat'.  Bewerk het *Setup.bat*-bestand en voeg de volgende opdracht toe:

```bat
powershell.exe -ExecutionPolicy Bypass -Command ".\SetCertAccess.ps1"
```

Wijzig de eigenschappen van het *Setup.bat*-bestand zodat **Naar uitvoermap kopiëren** wordt ingesteld op 'Kopiëren indien nieuwer'.

![Bestandseigenschappen instellen][image1]

Klik in Solution Explorer met de rechtermuisknop op **VotingWeb**, selecteer **Toevoegen**->**Nieuw item** en voeg een nieuw bestand toe met de naam 'SetCertAccess.ps1'.  Bewerk het *SetCertAccess.ps1*-bestand en voeg het volgende script toe:

```powershell
$subject="mytestcert"
$userGroup="NETWORK SERVICE"

Write-Host "Checking permissions to certificate $subject.." -ForegroundColor DarkCyan

$cert = (gci Cert:\LocalMachine\My\ | where { $_.Subject.Contains($subject) })[-1]

if ($cert -eq $null)
{
    $message="Certificate with subject:"+$subject+" does not exist at Cert:\LocalMachine\My\"
    Write-Host $message -ForegroundColor Red
    exit 1;
}elseif($cert.HasPrivateKey -eq $false){
    $message="Certificate with subject:"+$subject+" does not have a private key"
    Write-Host $message -ForegroundColor Red
    exit 1;
}else
{
    $keyName=$cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName

    $keyPath = "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys\"
    $fullPath=$keyPath+$keyName
    $acl=(Get-Item $fullPath).GetAccessControl('Access')


    $hasPermissionsAlready = ($acl.Access | where {$_.IdentityReference.Value.Contains($userGroup.ToUpperInvariant()) -and $_.FileSystemRights -eq [System.Security.AccessControl.FileSystemRights]::FullControl}).Count -eq 1

    if ($hasPermissionsAlready){
        Write-Host "Account $userGroup already has permissions to certificate '$subject'." -ForegroundColor Green
        return $false;
    } else {
        Write-Host "Need add permissions to '$subject' certificate..." -ForegroundColor DarkYellow

        $permission=$userGroup,"Full","Allow"
        $accessRule=new-object System.Security.AccessControl.FileSystemAccessRule $permission
        $acl.AddAccessRule($accessRule)
        Set-Acl $fullPath $acl

        Write-Output "Permissions were added"

        return $true;
    }
}

```

Wijzig de eigenschappen van het bestand *SetCertAccess.ps1*, zodat **Naar uitvoermap kopiëren** wordt ingesteld op 'Kopiëren indien nieuwer'.

### <a name="run-the-setup-script-as-a-local-administrator"></a>Het installatiescript uitvoeren als lokale beheerder

Het uitvoerbare bestand voor het invoerpunt van de service-installatie wordt standaard uitgevoerd onder dezelfde referenties als Service Fabric (meestal het NetworkService-account). Voor *SetCertAccess.ps1* zijn beheerdersbevoegdheden vereist. In het toepassingsmanifest kunt u de beveiligingsmachtigingen voor het uitvoeren van het opstartscript onder een lokaal beheerdersaccount wijzigen.

Open *Voting/ApplicationPackageRoot/ApplicationManifest.xml* vanuit Solution Explorer. Maak eerst een **Principals**-sectie en voeg een nieuwe gebruiker toe (bijvoorbeeld 'SetupAdminUser'). Voeg het gebruikersaccount SetupAdminUser toe aan de systeemgroep Administrators.
Vervolgens configureert u in de sectie VotingWebPkg **ServiceManifestImport** een **RunAsPolicy** om de principal SetupAdminUser toe te passen op het invoerpunt van de installatie. Dit beleid laat Service Fabric weten dat het bestand Setup.bat wordt uitgevoerd als SetupAdminUser (met beheerdersbevoegdheden).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren

In Solution Explorer, selecteer de **Voting** toepassing en stel de **toepassings-URL** eigenschap in op ' https:\//localhost:443 '.

Sla alle bestanden op en druk op F5 om de toepassing lokaal uit te voeren.  Nadat de toepassing heeft geïmplementeerd, opent een webbrowser naar https:\//localhost:443. Als u een zelfondertekend certificaat gebruikt, ziet u een waarschuwing dat uw pc de beveiliging van deze website niet vertrouwt.  Ga door naar de webpagina.

![Stemtoepassing][image2]

## <a name="install-certificate-on-cluster-nodes"></a>Certificaat installeren op clusterknooppunten

Voordat u de toepassing in Azure implementeert, installeert u het certificaat in het `Cert:\LocalMachine\My`-archief alle externe clusterknooppunten.  Services kunnen worden verplaatst naar andere knooppunten van het cluster.  Wanneer de front-end webservice wordt gestart op een clusterknooppunt, zoekt het opstartscript naar het certificaat en worden er toegangsmachtigingen geconfigureerd.

Exporteer als eerste het certificaat naar een PFX-bestand. Open de toepassing certlm.msc en navigeer naar **Persoonlijk**>**Certificaten**.  Klik met de rechtermuisknop op het certificaat *mytestcert* en selecteer **Alle taken**>**Exporteren**.

![Certificaat exporteren][image4]

Kies in de wizard Exporteren **Ja, de persoonlijke sleutel exporteren** en kies de indeling Personal Information Exchange (PFX).  Exporteer het bestand naar *C:\Users\sfuser\votingappcert.pfx*.

Vervolgens installeert u het certificaat op het externe cluster met behulp van de [toevoegen AzServiceFabricApplicationCertificate](/powershell/module/az.servicefabric/Add-azServiceFabricApplicationCertificate) cmdlet.

> [!Warning]
> Een zelfondertekend certificaat volstaat voor ontwikkel- en testtoepassingen. Gebruik voor productietoepassingen in plaats van een zelfondertekend certificaat een certificaat van een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority).

```powershell
Connect-AzAccount

$vaultname="sftestvault"
$certname="VotingAppPFX"
$certpw="!Password321#"
$groupname="voting_RG"
$clustername = "votinghttps"
$ExistingPfxFilePath="C:\Users\sfuser\votingappcert.pfx"

$appcertpwd = ConvertTo-SecureString -String $certpw -AsPlainText -Force

Write-Host "Reading pfx file from $ExistingPfxFilePath"
$cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2 $ExistingPfxFilePath, $certpw

$bytes = [System.IO.File]::ReadAllBytes($ExistingPfxFilePath)
$base64 = [System.Convert]::ToBase64String($bytes)

$jsonBlob = @{
   data = $base64
   dataType = 'pfx'
   password = $certpw
   } | ConvertTo-Json

$contentbytes = [System.Text.Encoding]::UTF8.GetBytes($jsonBlob)
$content = [System.Convert]::ToBase64String($contentbytes)

$secretValue = ConvertTo-SecureString -String $content -AsPlainText -Force

# Upload the certificate to the key vault as a secret
Write-Host "Writing secret to $certname in vault $vaultname"
$secret = Set-AzureKeyVaultSecret -VaultName $vaultname -Name $certname -SecretValue $secretValue

# Add a certificate to all the VMs in the cluster.
Add-AzServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $secret.Id -Verbose
```

## <a name="open-port-443-in-the-azure-load-balancer"></a>Poort 443 openen in de Azure-load balancer

Open poort 443 in de load balancer als dat nog niet is gebeurd.

```powershell
$probename = "AppPortProbe6"
$rulename="AppPortLBRule6"
$RGname="voting_RG"
$port=443

# Get the load balancer resource
$resource = Get-AzResource | Where {$_.ResourceGroupName –eq $RGname -and $_.ResourceType -eq "Microsoft.Network/loadBalancers"}
$slb = Get-AzLoadBalancer -Name $resource.Name -ResourceGroupName $RGname

# Add a new probe configuration to the load balancer
$slb | Add-AzLoadBalancerProbeConfig -Name $probename -Protocol Tcp -Port $port -IntervalInSeconds 15 -ProbeCount 2

# Add rule configuration to the load balancer
$probe = Get-AzLoadBalancerProbeConfig -Name $probename -LoadBalancer $slb
$slb | Add-AzLoadBalancerRuleConfig -Name $rulename -BackendAddressPool $slb.BackendAddressPools[0] -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -Probe $probe -Protocol Tcp -FrontendPort $port -BackendPort $port

# Set the goal state for the load balancer
$slb | Set-AzLoadBalancer
```

## <a name="deploy-the-application-to-azure"></a>De toepassing implementeren in Azure

Sla alle bestanden op, schakel over van Fouten opsporen naar Uitbrengen en druk op F6 om opnieuw op te bouwen.  Klik in Solution Explorer met de rechtermuisknop op **Stemmen** en selecteer **Publiceren**. Selecteer het verbindingseindpunt van het cluster dat is gemaakt in [Een toepassing implementeren op een cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md) of selecteer een ander cluster.  Klik op **Publiceren** om de toepassing naar het externe cluster te publiceren.

Nadat de toepassing is geïmplementeerd, opent u een webbrowser en navigeert u naar [https://mycluster.region.cloudapp.azure.com:443](https://mycluster.region.cloudapp.azure.com:443) (werk de URL bij met het verbindingseindpunt voor uw cluster). Als u een zelfondertekend certificaat gebruikt, ziet u een waarschuwing dat uw pc de beveiliging van deze website niet vertrouwt.  Ga door naar de webpagina.

![Stemtoepassing][image3]

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een HTTPS-eindpunt in de service definiëren
> * Kestrel configureren voor gebruik van HTTPS
> * Het SSL-certificaat installeren op de externe clusterknooppunten
> * Netwerkservice toegang geven tot de persoonlijke sleutel van het certificaat
> * Poort 443 openen in de Azure-load balancer
> * De toepassing implementeren in een extern cluster

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [CI/CD configureren met behulp van Azure-pijplijnen](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

[image1]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/SetupBatProperties.png
[image2]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppLocal.png
[image3]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppAzure.png
[image4]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/ExportCert.png

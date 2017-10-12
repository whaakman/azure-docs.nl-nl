---
title: Een Azure Service Fabric-containertoepassing maken | Microsoft Docs
description: Maak uw eerste Windows-containertoepassing in Azure Service Fabric.  Bouw een Docker-installatiekopie met een Python-toepassing, push de installatiekopie naar een containerregister, en bouw en implementeer een Service Fabric-containertoepassing.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2017
ms.author: ryanwi
ms.openlocfilehash: 025bde02b3f342ec3399d51819d1fa8a91f11374
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Uw eerste Service Fabric-containertoepassing maken in Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Er zijn geen wijzigingen in uw toepassing vereist om een bestaande toepassing in een Windows-container uit te voeren in een Service Fabric-cluster. Dit artikel helpt u bij het maken van een Docker-installatiekopie met een Python [Flask](http://flask.pocoo.org/)-webtoepassing en het implementeren ervan in een Service Fabric-cluster.  U gaat uw containertoepassing ook delen via [Azure Container Registry](/azure/container-registry/).  In dit artikel wordt ervan uitgegaan dat u de basisbeginselen kent van Docker. Meer informatie over Docker kunt u lezen in het [Docker-overzicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Vereisten
Een ontwikkelcomputer waarop wordt uitgevoerd:
* Visual Studio 2015 of Visual Studio 2017.
* [Service Fabric SDK en hulpprogramma's](service-fabric-get-started.md).
*  Docker voor Windows.  [Download Docker CE voor Windows (stabiel)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Nadat u Docker hebt geïnstalleerd en gestart, klikt u met de rechtermuisknop op het systeemvakpictogram en selecteert u **Overschakelen naar Windows-containers**. Dit is vereist voor het uitvoeren van Docker-installatiekopieën op basis van Windows.

Een Windows-cluster met drie of meer knooppunten die worden uitgevoerd op Windows Server 2016 met containers - [Een cluster maken](service-fabric-cluster-creation-via-portal.md) of [Service Fabric gratis uitproberen](https://aka.ms/tryservicefabric).

Een register in Azure Container Registry - [Een containerregister maken](../container-registry/container-registry-get-started-portal.md) in uw Azure-abonnement.

## <a name="define-the-docker-container"></a>De Docker-container definiëren
Bouw een installatiekopie op basis van de [Python-installatiekopie](https://hub.docker.com/_/python/) die zich in de Docker-hub bevindt.

Definieer uw Docker-container in een Dockerfile. Het bestand Dockerfile bevat instructies voor het instellen van de omgeving in de container, het laden van de toepassing die u wilt uitvoeren en het toewijzen van poorten. Het Dockerfile is de invoer van de opdracht `docker build` waarmee de installatiekopie wordt gemaakt.

Maak een lege map en maak het bestand *Dockerfile* (zonder extensie). Voeg het volgende toe aan het bestand *Dockerfile* en sla de wijzigingen op:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Lees het [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) voor meer informatie.

## <a name="create-a-simple-web-application"></a>Een eenvoudige webtoepassing maken
Maak een Flask-toepassing die luistert op poort 80 en 'Hallo Wereld!' retourneert.  Maak in dezelfde map het bestand *requirements.txt*.  Voeg het volgende toe en sla de wijzigingen op:
```
Flask
```

Maak ook het bestand *app.py* en voeg het volgende toe:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-the-image"></a>De installatiekopie bouwen
Voer de opdracht `docker build` uit om de installatiekopie te maken waarmee de webtoepassing wordt uitgevoerd. Open een PowerShell-venster en navigeer naar de map met het bestand Dockerfile. Voer de volgende opdracht uit:

```
docker build -t helloworldapp .
```

Met deze opdracht wordt de nieuwe installatiekopie gebouwd met behulp van de instructies in het Dockerfile, en krijgt de installatiekopie de naam (-t tagging) `hallowereldapp´. Bij het bouwen van een installatiekopie wordt de basisinstallatiekopie uit de Docker-hub gehaald en wordt er een nieuwe installatiekopie gemaakt waarmee de toepassing wordt toegevoegd boven op de basisinstallatiekopie.  

Nadat de buildopdracht is voltooid, voert u de opdracht `docker images` uit om de gegevens van de nieuwe installatiekopie te bekijken:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren
Controleer de installatiekopie eerst lokaal voordat u deze naar het containerregister pusht.  

Voer de toepassing uit:

```
docker run -d --name my-web-site helloworldapp
```

Bij *naam* kunt de actieve container een naam geven (in plaats van de container-id).

Zodra de container is gestart, zoekt u het bijbehorende IP-adres zodat u vanuit een browser verbinding kunt maken met de actieve container:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd, bijvoorbeeld http://172.31.194.61. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

Als u de container wilt stoppen, voert u dit uit:

```
docker stop my-web-site
```

De container verwijderen van de ontwikkelcomputer:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-the-image-to-the-container-registry"></a>De installatiekopie naar het containerregister pushen
Nadat u hebt gecontroleerd of de container actief is op de ontwikkelcomputer, pusht u de installatiekopie naar het register in Azure Container Registry.

Voer ``docker login`` uit om u bij uw containerregister aan te melden met uw [registerreferenties](../container-registry/container-registry-authentication.md).

In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario. Of u kunt zich aanmelden met uw gebruikersnaam en wachtwoord van het register.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Met de volgende opdracht maakt u een tag (of alias) van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de installatiekopie in de naamruimte ```samples``` geplaatst om overbodige items in de hoofdmap van het register te voorkomen.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

De installatiekopie naar het containerregister pushen:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-the-containerized-service-in-visual-studio"></a>De beperkte service maken in Visual Studio
De Service Fabric SDK en hulpprogramma's bieden een servicesjabloon waarmee u een containertoepassing kunt maken.

1. Start Visual Studio.  Selecteer **Bestand** > **Nieuw** > **Project**.
2. Selecteer **Service Fabric-toepassing**, geef deze de naam MyFirstContainer en klik op **OK**.
3. Selecteer **Gastcontainer** in de lijst met **servicesjablonen**.
4. Voer bij **Naam van installatiekopie** het volgende in: myregistry.azurecr.io/samples/helloworldapp. Dit is de installatiekopie die u naar uw containeropslagplaats hebt gepusht.
5. Geef de service een naam en klik op **OK**.

## <a name="configure-communication"></a>Communicatie configureren
De containerservice heeft een eindpunt voor communicatie nodig. Voeg een `Endpoint`-element met het protocol, de poort en het type toe aan het bestand ServiceManifest.xml. Voor deze snelstartgids luistert de containerservice naar poort 8081.  In dit voorbeeld wordt een ingestelde poort 8081 gebruikt.  Als er geen poort is opgegeven, wordt een willekeurige poort uit het poortbereik van de toepassing gekozen.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Als u een eindpunt opgeeft, publiceert Service Fabric het eindpunt naar de Naming-service.  Andere services die in dit cluster worden uitgevoerd, kunnen deze container dan omzetten.  U kunt ook communicatie van container naar container laten plaatsvinden met behulp van een [omgekeerde proxy](service-fabric-reverseproxy.md).  Communicatie wordt uitgevoerd door de omgekeerde proxy de HTTP-poort voor luisteren en de naam van de services waarmee u wilt communiceren door te geven als omgevingsvariabelen.

## <a name="configure-and-set-environment-variables"></a>Omgevingsvariabelen configureren en instellen
Er kunnen omgevingsvariabelen worden opgegeven voor ieder codepakket in het servicemanifest. Deze functie is beschikbaar voor alle services, ongeacht of ze zijn geïmplementeerd als containers, processen of uitvoerbare gastbestanden. U kunt waarden van omgevingsvariabelen overschrijven in het toepassingsmanifest of ze opgeven als toepassingsparameters tijdens de implementatie.

Het volgende XML-fragment voor het servicemanifest toont een voorbeeld van het opgeven van omgevingsvariabelen voor een codepakket:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

Deze omgevingsvariabelen kunnen worden overschreven in het toepassingsmanifest:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Poort-naar-host-toewijzing voor containers en container-naar-container-detectie configureren
Configureer een hostpoort voor communicatie met de container. De poortbinding wijst de poort toe waarop de service binnen de container luistert naar een poort op de host. Voeg een element `PortBinding` toe aan het element `ContainerHostPolicies` van het bestand ApplicationManifest.xml.  Voor dit artikel geldt: `ContainerPort` is 80 (de container gebruikt poort 80, zoals opgegeven in het bestand Dockerfile) en `EndpointRef` is 'Guest1TypeEndpoint' (het eindpunt dat eerder is gedefinieerd in het servicemanifest).  Binnenkomende aanvragen naar de service op poort 8081 worden toegewezen aan poort 80 in de container.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a>Verificatie containerregister configureren
Configureer de verificatie van het containerregister `RepositoryCredentials` door toe te voegen aan `ContainerHostPolicies` van het bestand ApplicationManifest.xml. Voeg het account en wachtwoord toe aan het containerregister myregistry.azurecr.io, waardoor de service voor de containerinstallatiekopie uit de opslagplaats kan downloaden.

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

We raden aan het wachtwoord van de opslagplaats te versleutelen door middel van een versleutelingscertificaat dat naar alle knooppunten van het cluster wordt geïmplementeerd. Wanneer Service Fabric het servicepakket naar het cluster implementeert, wordt het versleutelingscertificaat gebruikt om de versleutelde tekst te ontsleutelen.  De cmdlet Invoke-ServiceFabricEncryptText wordt gebruikt om de gecodeerde tekst voor het wachtwoord te maken, die wordt toegevoegd aan het bestand ApplicationManifest.xml.

Het volgende script maakt een nieuw zelfondertekend certificaat en exporteert het naar een PFX-bestand.  Het certificaat wordt geïmporteerd in een bestaande sleutelkluis en geïmplementeerd naar het Service Fabric-cluster.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export to PFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import the certificate to an existing key vault.  The key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add the certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
Versleutel het wachtwoord met behulp van de cmdlet [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps).

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Vervang het wachtwoord door de versleutelde tekst die wordt geretourneerd door de cmdlet [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) en stel `PasswordEncrypted` in op 'true'.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-isolation-mode"></a>Isolatiemodus configureren
Windows ondersteunt twee isolatiemodi voor containers: proces en Hyper-V. Met de procesisolatiemodus delen alle containers die worden uitgevoerd op dezelfde hostcomputer de kernel met de host. Met de Hyper-V-isolatiemodus hebben de kernels een scheiding tussen elke Hyper-V-container en de containerhost. De isolatiemodus is in het manifestbestand van de toepassing opgegeven in het element `ContainerHostPolicies`. De isolatiemodi die kunnen worden opgegeven zijn `process`, `hyperv` en `default`. De standaardmodus voor isolatie wordt standaard ingesteld op `process` op Windows Server-hosts en op `hyperv` op hosts met Windows 10. Het volgende codefragment toont hoe de isolatiemodus wordt opgegeven in het manifestbestand van de toepassing.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a>Resourcebeheer configureren
[Resourcebeheer](service-fabric-resource-governance.md) beperkt de resources die de container op de host kan gebruiken. Het element `ResourceGovernancePolicy`, dat is opgegeven in het toepassingsmanifest, wordt gebruikt om resourcebeperkingen te declareren voor een servicecodepakket. Er kunnen resourcebeperkingen worden ingesteld voor de volgende resources: geheugen, MemorySwap, CpuShares (relatief CPU-gewicht), MemoryReservationInMB, BlkioWeight (relatief BlockIO-gewicht).  In dit voorbeeld krijgt het servicepakket Guest1Pkg één kern op de clusterknooppunten waar het wordt geplaatst.  Geheugenlimieten zijn absoluut, dus het codepakket wordt beperkt tot 1024 MB aan geheugen (en een gegarandeerde flexibele reservering hierop). Codepakketten (containers of processen) kunnen niet meer geheugen toewijzen dan deze limiet. Een poging dit toch te doen, leidt tot een Onvoldoende geheugen-uitzondering. Voor een effectieve handhaving van resourcebeperkingen moeten voor alle pakketten binnen een servicepakket geheugenlimieten zijn opgegeven.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-the-container-application"></a>De containertoepassing implementeren
Sla al uw wijzigingen op en bouw de toepassing. Klik in Solution Explorer met de rechtermuisknop op **MyFirstContainer** en selecteer **Publish** om uw toepassing te publiceren.

Voer bij **Verbindingseindpunt** het beheereindpunt voor het cluster in.  Bijvoorbeeld: 'containercluster.westus2.cloudapp.azure.com:19000'. U vindt het eindpunt voor de clientverbinding op de blade Overzicht voor het cluster in [Azure Portal](https://portal.azure.com).

Klik op **Publish**.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een webhulpprogramma voor het inspecteren en beheren van toepassingen en knooppunten in een Service Fabric-cluster. Open een browser, ga naar http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ en volg de implementatie van de toepassing.  De toepassing wordt geïmplementeerd, maar heeft een foutstatus totdat de installatiekopie is gedownload op de clusterknooppunten (wat enige tijd kan duren, afhankelijk van de grootte van de installatiekopie): ![Fout][1]

De toepassing is gereed bij een ```Ready```-status: ![Gereed][2]

Open een browser en ga naar http://containercluster.westus2.cloudapp.azure.com:8081. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

## <a name="clean-up"></a>Opruimen
Zolang het cluster actief is, worden er kosten in rekening gebracht. Overweeg daarom [het cluster te verwijderen](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party-clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) worden na een paar uur automatisch verwijderd.

Nadat u de installatiekopie naar het containerregister hebt gepusht, kunt u de lokale installatiekopie op de ontwikkelcomputer verwijderen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Volledig voorbeeld van de manifesten voor de Fabric Service-toepassing en -service
Dit zijn de volledige manifesten voor de service en toepassing die in dit artikel worden gebruikt.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a>Tijdsinterval configureren voor geforceerd beëindigen van container

U kunt een tijdsinterval configureren, zodat de runtime die tijd wacht voordat de container wordt verwijderd nadat het verwijderen van een service (of het verplaatsen naar een ander knooppunt) is gestart. Als u een tijdsinterval configureert, wordt de `docker stop <time in seconds>` opdracht verzonden naar de container.   Zie [docker stop](https://docs.docker.com/engine/reference/commandline/stop/) voor meer informatie. Het tijdsinterval dat moet worden gewacht, kunt u opgeven in de sectie `Hosting`. Het volgende fragment van een clustermanifest laat zien hoe u het wachtinterval instelt:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
Het standaardtijdsinterval is 10 seconden. Aangezien deze configuratie dynamisch is, wordt de time-out bijgewerkt bij een configuratie-upgrade van het cluster. 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a>De runtime configureren voor het verwijderen van ongebruikte containerinstallatiekopieën

U kunt het Service Fabric-cluster configureren voor het verwijderen van ongebruikte containerinstallatiekopieën van het knooppunt. Met deze configuratie kunt u schijfruimte vrijmaken als er te veel containerinstallatiekopieën aanwezig zijn op het knooppunt.  Om deze functie in te schakelen, past u de sectie `Hosting` in het clustermanifest aan zoals wordt weergegeven in het volgende fragment: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

De installatiekopieën die niet moeten worden verwijderd, kunt u opgeven met de parameter `ContainerImagesToSkip`. 



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Lees de zelfstudie [Een .NET-toepassing implementeren in een container](service-fabric-host-app-in-a-container.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

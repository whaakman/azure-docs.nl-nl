---
title: Veilig verbinding maken met een Azure Service Fabric-cluster | Microsoft Docs
description: Hierin wordt beschreven hoe u verifieert de clienttoegang tot een Service Fabric-cluster en over het beveiligen van communicatie tussen clients en een cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2019
ms.author: ryanwi
ms.openlocfilehash: 55564de4a3c5ff2d3ba3ddc5e68fa3d1b2d51e71
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296389"
---
# <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster

Wanneer een client verbinding met het knooppunt van een Service Fabric-cluster maakt, gebruikt de client kan worden geverifieerd en veilige communicatie tot stand gebracht met behulp van certificaat beveiligings- of Azure Active Directory (AAD). Deze verificatie zorgt ervoor dat alleen geautoriseerde gebruikers toegang het cluster tot kunnen en toepassingen geïmplementeerde en beheertaken uitvoeren.  Certificaat of de AAD-beveiliging moet zijn eerder ingeschakeld op het cluster als het cluster is gemaakt.  Zie voor meer informatie over clusterbeveiligingsscenario's [Cluster security](service-fabric-cluster-security.md). Als u verbinding met een cluster dat is beveiligd met certificaten maakt, [instellen van het clientcertificaat](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) op de computer die verbinding met het cluster maakt. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Verbinding maken met een beveiligd cluster met behulp van Azure Service Fabric-CLI (sfctl)

Er zijn een aantal verschillende manieren verbinding maken met een beveiligd cluster met behulp van de Service Fabric CLI (sfctl). Als u ter verificatie een clientcertificaat gebruikt, moeten de certificaatgegevens overeenkomen met een certificaat dat is geïmplementeerd in de clusterknooppunten. Als uw certificaat heeft certificeringsinstanties (CA's), moet u ook opgeven de vertrouwde certificeringsinstanties.

U kunt verbinding maken met een cluster met de `sfctl cluster select` opdracht.

Clientcertificaten kunnen worden opgegeven in twee verschillende manieren, als een certificaat en het sleutelpaar of als een enkel PFX-bestand. Voor het PEM-bestanden beveiligd met een wachtwoord, wordt u automatisch gevraagd het wachtwoord op te geven. Als u het clientcertificaat als een PFX-bestand hebt verkregen, moet u eerst het PFX-bestand converteren naar een PEM-bestand met de volgende opdracht. 

```bash
openssl pkcs12 -in your-cert-file.pfx -out your-cert-file.pem -nodes -passin pass:your-pfx-password
```

Als uw pfx-bestand niet beveiligd met een wachtwoord is, gebruikt u pass - passin: voor de laatste parameter zijn.

Als u het clientcertificaat als een pem-bestand, geef het pad in de `--pem` argument. Bijvoorbeeld:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Wachtwoord beveiligd pem-bestanden wordt u gevraagd het wachtwoord voordat u een opdracht uitvoert.

Als u een certificaat, sleutel paar gebruiken de `--cert` en `--key` argumenten voor de bestandspaden aan elk respectieve bestand opgeven.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Certificaten die worden gebruikt voor het beveiligen van test- of ontwikkelingsomgevingen clusters mislukt soms validatie van het servercertificaat. Als u wilt omzeilen certificaatcontrole, geef de `--no-verify` optie. Bijvoorbeeld:

> [!WARNING]
> Gebruik niet de `no-verify` optie bij het verbinden met productie Service Fabric-clusters.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

U kunt bovendien paden naar de mappen van vertrouwde CA-certificaten of afzonderlijke certificaten opgeven. Als u wilt deze paden opgeven, gebruikt u de `--ca` argument. Bijvoorbeeld:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Nadat u verbinding hebt gemaakt, kunt u moet kunnen [andere sfctl-opdrachten uitvoeren](service-fabric-cli.md) om te communiceren met het cluster.

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a>Verbinding maken met een cluster met behulp van PowerShell
Voordat u bewerkingen op een cluster via PowerShell uitvoeren, eerst een verbinding maken met het cluster. De clusterverbinding wordt gebruikt voor alle volgende opdrachten in de opgegeven PowerShell-sessie.

### <a name="connect-to-an-unsecure-cluster"></a>Verbinding maken met een onbeveiligd cluster

Voor verbinding met een onbeveiligd cluster, geeft u het adres van de cluster-eindpunt naar de **Connect-ServiceFabricCluster** opdracht:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Verbinding maken met een beveiligd cluster met behulp van Azure Active Directory

Voor verbinding met een beveiligd cluster die gebruikmaakt van Azure Active Directory om cluster beheerderstoegang te verlenen, bieden de vingerafdruk van het cluster en gebruikt de *AzureActiveDirectory* vlag.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligd cluster met behulp van een clientcertificaat
Voer de volgende PowerShell-opdracht verbinding maken met een beveiligd cluster die gebruikmaakt van clientcertificaten om de beheerderstoegang te verlenen. 

#### <a name="connect-using-certificate-common-name"></a>Verbinding maken met behulp van de algemene naam van certificaat
Geef een algemene naam van de cluster-certificaat en de algemene naam van het clientcertificaat dat voor Clusterbeheer machtigingen heeft. Details van het certificaat moeten overeenkomen met een certificaat op de clusterknooppunten.

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName <certificate common name>  `
    -FindType FindBySubjectName `
    -FindValue <certificate common name> `
    -StoreLocation CurrentUser `
    -StoreName My 
```
*ServerCommonName* is de algemene naam van het certificaat van de server geïnstalleerd op de clusterknooppunten. *FindValue* is de algemene naam van het certificaat voor de Beheerclient. Wanneer de parameters worden ingevuld, wordt de opdracht ziet eruit zoals in het volgende voorbeeld:
```powershell
$ClusterName= "sf-commonnametest-scus.southcentralus.cloudapp.azure.com:19000"
$certCN = "sfrpe2eetest.southcentralus.cloudapp.azure.com"

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName $certCN  `
    -FindType FindBySubjectName `
    -FindValue $certCN `
    -StoreLocation CurrentUser `
    -StoreName My 
```

#### <a name="connect-using-certificate-thumbprint"></a>Verbinding maken met behulp van de vingerafdruk van certificaat
Geef de vingerafdruk van het cluster en de vingerafdruk van het clientcertificaat dat voor Clusterbeheer machtigingen heeft. Details van het certificaat moeten overeenkomen met een certificaat op de clusterknooppunten.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `  
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `  
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* is de vingerafdruk van het certificaat van de server geïnstalleerd op de clusterknooppunten. *FindValue* is de vingerafdruk van het certificaat voor de Beheerclient.  Wanneer de parameters worden ingevuld, wordt de opdracht ziet eruit zoals in het volgende voorbeeld:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `  
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `  
          -StoreLocation CurrentUser -StoreName My 
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a>Verbinding maken met een beveiligd cluster met behulp van Windows Active Directory
Als uw zelfstandige cluster wordt geïmplementeerd met behulp van AD-beveiliging, verbinding met het cluster door de switch 'WindowsCredential' toe te voegen.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a>Verbinding maken met een cluster met behulp van de APIs FabricClient
De Service Fabric SDK bevat de [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) klasse voor Clusterbeheer. Voor het gebruik van de APIs FabricClient, het Microsoft.ServiceFabric NuGet-pakket te downloaden.

### <a name="connect-to-an-unsecure-cluster"></a>Verbinding maken met een onbeveiligd cluster

Verbinding maken met een externe niet-beveiligd cluster, een FabricClient-exemplaar maken en geeft u het clusteradres:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Voor de code die wordt uitgevoerd binnen een cluster, bijvoorbeeld in een betrouwbare Service maakt een FabricClient *zonder* adres van het cluster op te geven. FabricClient maakt verbinding met de lokale management-gateway op het knooppunt dat de code wordt momenteel uitgevoerd, waardoor een extra netwerk hop is voorkomen.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligd cluster met behulp van een clientcertificaat

De knooppunten in het cluster moeten geldige certificaten waarvan de algemene naam of DNS-naam in het SAN wordt weergegeven in de [RemoteCommonNames eigenschap](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) instellen op [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Deze stappen te volgen, kunt wederzijdse verificatie tussen de client en de clusterknooppunten.

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a>Verbinding maken met een beveiligd cluster interactief met behulp van Azure Active Directory

Het volgende voorbeeld maakt gebruik van Azure Active Directory voor identiteits- en server clientcertificaat voor de identiteit van server.

Dialoogvenster verschijnt een venster automatisch voor interactief aanmelden bij het verbinding maken met het cluster.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a>Verbinding maken met een beveiligd cluster niet-interactief met behulp van Azure Active Directory

Het volgende voorbeeld is afhankelijk van Microsoft.IdentityModel.Clients.ActiveDirectory, versie: 2.19.208020213.

Zie voor meer informatie over het ophalen van de AAD-tokens, [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Verbinding maken met een beveiligd cluster zonder voorafgaande metagegevens kennis met Azure Active Directory

Het volgende voorbeeld wordt een niet-interactieve ophalen van tokens, maar dezelfde aanpak kan worden gebruikt voor het bouwen van een aangepaste interactieve token acquisition-ervaring. De Azure Active Directory-metagegevens die nodig zijn voor het ophalen van tokens worden gelezen uit de configuratie van het cluster.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Verbinding maken met een beveiligd cluster met behulp van Service Fabric Explorer
Bereiken [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) voor een bepaald cluster, wijst u uw browser naar:

`http://<your-cluster-endpoint>:19080/Explorer`

De volledige URL is ook beschikbaar in het cluster essentials-deelvenster van de Azure-portal.

Om verbinding te maken met een beveiligd cluster op Windows- of OS X met behulp van een browser, kunt u het certificaat importeren en de browser wordt u gevraagd voor het certificaat te gebruiken om verbinding te maken met het cluster.  Op Linux-machines moet het certificaat worden geïmporteerd met behulp van geavanceerde browserinstellingen (elke browser heeft verschillende mechanismen) en Ga naar de certificaatlocatie op de schijf. Lezen [instellen van een clientcertificaat](#connectsecureclustersetupclientcert) voor meer informatie.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Verbinding maken met een beveiligd cluster met behulp van Azure Active Directory

Als u wilt verbinding maken met een cluster die is beveiligd met AAD, wijst u uw browser naar:

`https://<your-cluster-endpoint>:19080/Explorer`

U wordt automatisch gevraagd zich kunnen aanmelden met AAD.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligd cluster met behulp van een clientcertificaat

Als u wilt verbinding maken met een cluster die is beveiligd met certificaten, wijst u uw browser naar:

`https://<your-cluster-endpoint>:19080/Explorer`

U wordt automatisch gevraagd een clientcertificaat te selecteren.

<a id="connectsecureclustersetupclientcert"></a>

## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Instellen van een certificaat op de externe computer

Ten minste twee certificaten moeten worden gebruikt voor het beveiligen van het cluster, een voor het cluster en de server-certificaat en een voor clienttoegang.  Het is raadzaam om ook extra secundaire certificaten en clientcertificaten toegang te gebruiken.  Als u wilt beveiligen de communicatie tussen een client en het knooppunt van een cluster met Certificaatbeveiliging, moet u eerst het verkrijgen en installeren van het clientcertificaat. Het certificaat kan worden geïnstalleerd in het persoonlijke (Mijn) archief van de lokale computer of de huidige gebruiker.  U moet ook de vingerafdruk van het servercertificaat zodat de client kan het cluster verifiëren.

* In Windows: Dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw persoonlijke archief, `Certificates - Current User\Personal\Certificates`, te installeren. U kunt ook de PowerShell-opdracht gebruiken:

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
            -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
            -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

    Als het een zelfondertekend certificaat, dat u wilt importeren naar archief van de 'vertrouwde personen' van uw computer voordat u dit certificaat gebruiken kunt om verbinding met een beveiligd cluster te maken.

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
    -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
    -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

* Op de Mac: Dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw Sleutelhanger te installeren.

## <a name="next-steps"></a>Volgende stappen

* [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](service-fabric-cluster-upgrade.md)
* [Beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Introductie van service Fabric-status-model](service-fabric-health-introduction.md)
* [Beveiliging van toepassingen en RunAs](service-fabric-application-runas-security.md)
* [Aan de slag met de Service Fabric-CLI](service-fabric-cli.md)

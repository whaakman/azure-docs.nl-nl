---
title: Veilige verbinding met een Azure Service Fabric-cluster | Microsoft Docs
description: Hierin wordt beschreven hoe u verifieert de clienttoegang tot een Service Fabric-cluster en het beveiligen van communicatie tussen clients en een cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2018
ms.author: ryanwi
ms.openlocfilehash: 15ea4cbc02a0311b26e75ae7156c42f6bc2b9b82
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster

Wanneer een client verbinding met een Service Fabric-clusterknooppunt maakt, kan de client geverifieerde en beveiligde communicatie tot stand gebracht via certificaat beveiligings- of Azure Active Directory (AAD) zijn. Deze verificatie zorgt ervoor dat alleen geautoriseerde gebruikers toegang het cluster tot kunnen en toepassingen geïmplementeerde en beheertaken uitvoeren.  Certificaat of AAD-beveiliging moet zijn eerder ingeschakeld op het cluster als het cluster is gemaakt.  Zie voor meer informatie over scenario's voor Toepassingsbeveiliging cluster [beveiliging Cluster](service-fabric-cluster-security.md). Als u verbinding met een cluster dat is beveiligd met certificaten maakt, [instellen van het clientcertificaat](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) op de computer die verbinding met het cluster maakt. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Verbinding maken met een beveiligde cluster met behulp van Azure Service Fabric CLI (sfctl)

Er zijn een aantal verschillende manieren verbinding maken met een beveiligde cluster met behulp van de Service Fabric CLI (sfctl). Als u ter verificatie een clientcertificaat gebruikt, moeten de certificaatgegevens overeenkomen met een certificaat dat is geïmplementeerd in de clusterknooppunten. Als uw certificaat heeft certificeringsinstanties (CA's), moet u bovendien de vertrouwde certificeringsinstanties opgeven.

U kunt verbinding maken met een cluster met de `sfctl cluster select` opdracht.

Clientcertificaten kunnen worden opgegeven in twee verschillende manieren, als een certificaat en het sleutelpaar, of als een enkel pem-bestand. Voor een wachtwoord beveiligd `pem` bestanden, wordt u gevraagd automatisch om het wachtwoord invoeren.

Als u het clientcertificaat als een pem-bestand, geef het bestandspad in de `--pem` argument. Bijvoorbeeld:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Wachtwoord beveiligd pem-bestanden worden gevraagd om het wachtwoord voordat een opdracht uit te voeren.

Als u een certificaat, sleutel paar gebruik de `--cert` en `--key` argumenten voor de bestandspaden aan elk respectieve bestand opgeven.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Certificaten die worden gebruikt voor het beveiligen van clusters voor test- of dev mislukt soms validatie van het servercertificaat. Als u wilt omzeilen certificaatverificatie, geef de `--no-verify` optie. Bijvoorbeeld:

> [!WARNING]
> Gebruik niet de `no-verify` optie bij het verbinden met productie Service Fabric-clusters.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Bovendien kunt u paden voor mappen van vertrouwde CA-certificaten klant of afzonderlijke certificaten. Deze paden, gebruikt u de `--ca` argument. Bijvoorbeeld:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Nadat u verbinding maakt, moet u kunnen [uitvoeren van andere opdrachten sfctl](service-fabric-cli.md) om te communiceren met het cluster.

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a>Verbinding maken met een cluster met behulp van PowerShell
Voordat u bewerkingen op een cluster via PowerShell uitvoeren, eerst een verbinding met het cluster. De verbinding van het cluster wordt gebruikt voor alle volgende opdrachten in de opgegeven PowerShell-sessie.

### <a name="connect-to-an-unsecure-cluster"></a>Verbinding maken met een niet-beveiligde cluster

Voor verbinding met een niet-beveiligde cluster, geeft u het adres van de cluster-eindpunt voor de **Connect-ServiceFabricCluster** opdracht:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Verbinding maken met een beveiligde cluster met Azure Active Directory

Geef de vingerafdruk van het cluster-certificaat voor verbinding met een beveiligde cluster dat gebruik maakt van Azure Active Directory cluster beheerderstoegang verlenen, en gebruik de *AzureActiveDirectory* vlag.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligde cluster met behulp van een clientcertificaat
Voer de volgende PowerShell-opdracht verbinding maken met een beveiligde cluster dat gebruik maakt van clientcertificaten beheerderstoegang verlenen. Geef de vingerafdruk van het cluster-certificaat en de vingerafdruk van het clientcertificaat dat voor Clusterbeheer machtigingen heeft. De details van het certificaat moeten overeenkomen met een certificaat op de clusterknooppunten.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* is de vingerafdruk van het certificaat van de server geïnstalleerd op de clusterknooppunten. *FindValue* de vingerafdruk van het clientcertificaat voor de beheerder.
Als de parameters zijn ingevuld, wordt de opdracht ziet eruit als in het volgende voorbeeld: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a>Verbinding maken met een beveiligde cluster met behulp van Windows Active Directory
Als uw zelfstandige cluster wordt geïmplementeerd met behulp van AD-beveiligingsgroep, kunt u verbinding met het cluster door de switch 'WindowsCredential' toe te voegen.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a>Verbinding maken met een cluster met behulp van de FabricClient APIs
De Service Fabric SDK bevat de [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) klasse voor Clusterbeheer. Voor het gebruik van de FabricClient APIs ophalen het Microsoft.ServiceFabric NuGet-pakket.

### <a name="connect-to-an-unsecure-cluster"></a>Verbinding maken met een niet-beveiligde cluster

Verbinding maken met een externe onbeveiligde cluster, een FabricClient-exemplaar maken en geeft u het clusteradres:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Voor de code die wordt uitgevoerd binnen een cluster, bijvoorbeeld in een betrouwbare Service maakt een FabricClient *zonder* opgeven van het clusteradres. FabricClient maakt verbinding met de lokale management-gateway op het knooppunt dat de code wordt uitgevoerd, waardoor een extra netwerk-hop voorkomen.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligde cluster met behulp van een clientcertificaat

De knooppunten in het cluster moeten geldige certificaten waarvan de algemene naam of DNS-naam in de SAN wordt weergegeven in de [RemoteCommonNames eigenschap](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) ingesteld op [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Dit proces, schakelt u wederzijdse verificatie tussen de client en de clusterknooppunten.

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

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a>Verbinding maken met een beveiligde cluster interactief met Azure Active Directory

Het volgende voorbeeld wordt een Azure Active Directory voor identiteits- en clientcertificaat voor de identiteit van server.

Dialoogvenster verschijnt een venster automatisch voor interactieve aanmelding wanneer verbinding wordt gemaakt met het cluster.

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

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a>Verbinding maken met een beveiligde cluster niet-interactief met Azure Active Directory

Het volgende voorbeeld is afhankelijk van Microsoft.IdentityModel.Clients.ActiveDirectory, versie: 2.19.208020213.

Zie voor meer informatie over AAD-token verkrijgen [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

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

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Verbinding maken met een beveiligde cluster zonder voorafgaande metagegevens kennis met Azure Active Directory

Het volgende voorbeeld wordt een niet-interactieve token verkrijgen, maar dezelfde manier kan worden gebruikt voor het bouwen van een aangepaste interactieve token overname-ervaring. De Azure Active Directory-metagegevens die nodig zijn voor de aanschaf van token worden gelezen uit de configuratie van het cluster.

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

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Verbinding maken met een beveiligde cluster met behulp van Service Fabric Explorer
Bereiken [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) voor een bepaald cluster, wijst u uw browser:

`http://<your-cluster-endpoint>:19080/Explorer`

De volledige URL is ook beschikbaar in het cluster essentials deelvenster van de Azure portal.

U kunt het certificaat importeren voor het verbinden met een beveiligde cluster op Windows of OS X met een browser, en de browser wordt u gevraagd het certificaat moet worden gebruikt voor verbinding met het cluster.  Op Linux-machines moet het certificaat met geavanceerde browserinstellingen (elke browser heeft verschillende mechanismen) worden geïmporteerd en wijs de certificaatlocatie tehe op schijf.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Verbinding maken met een beveiligde cluster met Azure Active Directory

Voor verbinding met een cluster dat is beveiligd met AAD, wijst u uw browser:

`https://<your-cluster-endpoint>:19080/Explorer`

U wordt automatisch aanmelden met AAD gevraagd.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Verbinding maken met een beveiligde cluster met behulp van een clientcertificaat

Voor verbinding met een cluster dat is beveiligd met certificaten, wijst u uw browser:

`https://<your-cluster-endpoint>:19080/Explorer`

U wordt automatisch gevraagd een clientcertificaat te selecteren.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Instellen van een certificaat op de externe computer
Ten minste twee certificaten moeten worden gebruikt voor het beveiligen van het cluster, een voor het cluster en de server-certificaat en een voor clienttoegang.  U wordt aangeraden ook aanvullende secundaire certificaten en client access certificaten te gebruiken.  Als u wilt beveiligen de communicatie tussen een client en een clusterknooppunt met behulp van de beveiliging van certificate, moet u eerst downloaden en installeren van het clientcertificaat. Het certificaat kan worden geïnstalleerd in het persoonlijke (Mijn) archief van de lokale computer of de huidige gebruiker.  U moet ook de vingerafdruk van het servercertificaat zodat de client het cluster kan worden geverifieerd.

Voer de volgende PowerShell-cmdlet voor het instellen van het certificaat op de computer van waaruit u toegang het cluster tot.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Als het een zelfondertekend certificaat, moet u deze naar de computer 'vertrouwde personen' store importeren voordat u dit certificaat kunt verbinding maken met een beveiligde cluster.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a>Volgende stappen

* [Het upgradeproces service Fabric-Cluster en de verwachtingen van u](service-fabric-cluster-upgrade.md)
* [Het beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Status van de Fabric-service model Inleiding](service-fabric-health-introduction.md)
* [Toepassingsbeveiliging en RunAs](service-fabric-application-runas-security.md)
* [Aan de slag met de Service Fabric-CLI](service-fabric-cli.md)

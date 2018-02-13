---
title: Een op Windows Azure Service Fabric-cluster beveiligen met behulp van certificaten | Microsoft Docs
description: Veilige communicatie binnen een Azure Service Fabric-cluster voor zelfstandige of on-premises, evenals tussen clients en het cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: ee1a2eeeda95b03b185090841cf93c4183c5fce2
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>Een zelfstandige cluster op Windows beveiligen met behulp van X.509-certificaten
In dit artikel wordt beschreven hoe een beveiligde communicatie tussen de verschillende knooppunten van uw zelfstandige Windows-cluster. Ook wordt beschreven hoe u verificatie van clients die verbinding met dit cluster maken met behulp van X.509-certificaten. Verificatie zorgt ervoor dat alleen geautoriseerde gebruikers kunnen toegang krijgen het cluster en de geïmplementeerde toepassingen tot en beheertaken uitvoeren. Certificaatbeveiliging moet worden ingeschakeld op het cluster als het cluster is gemaakt.  

Zie voor meer informatie over clusterbeveiliging,, zoals een knooppunt naar beveiliging, client-naar-node beveiliging en op rollen gebaseerde toegangsbeheer, [security scenario's](service-fabric-cluster-security.md).

## <a name="which-certificates-do-you-need"></a>Welke certificaten hebt u nodig?
Beginnen met, [download het pakket van de Service Fabric voor Windows Server](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) op een van de knooppunten in het cluster. In het gedownloade pakket, moet u een bestand ClusterConfig.X509.MultiMachine.json vinden. Open het bestand en Raadpleeg de sectie voor beveiliging onder de sectie eigenschappen:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClusterCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames": "Root"
            }
        ]
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Deze sectie beschrijft de certificaten die u nodig hebt voor het beveiligen van uw zelfstandige Windows-cluster. Als u een cluster-certificaat opgeeft, stel de waarde van ClusterCredentialType naar _X509_. Als u een servercertificaat voor externe verbindingen opgeeft, stelt u de ServerCredentialType op _X509_. Hoewel dit niet verplicht, raden wij aan dat u beide van deze certificaten voor een goed beveiligde cluster. Als u deze waarden ingesteld op *X509*, moet u ook de bijbehorende certificaten opgeven of de Service Fabric er een uitzondering gegenereerd. In sommige gevallen wilt u mogelijk alleen opgeven de _ClientCertificateThumbprints_ of de _ReverseProxyCertificate_. In deze scenario's, u hoeft niet in te stellen _ClusterCredentialType_ of _ServerCredentialType_ naar _X509_.


> [!NOTE]
> Een [vingerafdruk](https://en.wikipedia.org/wiki/Public_key_fingerprint) is de primaire identiteit van een certificaat. Zie voor meer informatie over de vingerafdruk van de certificaten die u maakt, [een vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
> 
> 

De volgende tabel vindt u de certificaten die u nodig hebt van uw cluster-instellingen:

| **CertificateInformation instelling** | **Beschrijving** |
| --- | --- |
| ClusterCertificate |Aanbevolen voor een testomgeving. Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt twee verschillende certificaten, een primaire en een secundaire voor een upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die van de secundaire in de ThumbprintSecondary-variabelen. |
| ClusterCertificateCommonNames |Aanbevolen voor een productie-omgeving. Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt één of twee cluster certificaat algemene namen gebruiken. De CertificateIssuerThumbprint overeenkomt met de vingerafdruk van de verlener van dit certificaat. Als meer dan één certificaat met dezelfde algemene naam wordt gebruikt, kunt u meerdere verlener vingerafdrukken opgeven.|
| ClusterCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat dat overeenkomt met de verlener van het certificaat van het cluster. Algemene naam en de naam van het bijbehorende store in deze sectie in plaats van de vingerafdruk van de uitgever onder ClusterCertificateCommonNames geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk rollover cluster verleende certificaten. Meerdere uitgevers van certificaten kunnen worden opgegeven als meer dan één cluster certificaat wordt gebruikt. Een leeg IssuerCommonName whitelists alle certificaten in de bijbehorende opgeslagen opgegeven onder X509StoreNames.|
| ServerCertificate |Aanbevolen voor een testomgeving. Dit certificaat wordt geverifieerd op de client als er wordt geprobeerd verbinding maken met dit cluster. Voor het gemak kunt u hetzelfde certificaat gebruiken voor ClusterCertificate en ServerCertificate. U kunt twee verschillende servercertificaten, een primaire en een secundaire voor een upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die van de secundaire in de ThumbprintSecondary-variabelen. |
| ServerCertificateCommonNames |Aanbevolen voor een productie-omgeving. Dit certificaat wordt geverifieerd op de client als er wordt geprobeerd verbinding maken met dit cluster. De CertificateIssuerThumbprint overeenkomt met de vingerafdruk van de verlener van dit certificaat. Als meer dan één certificaat met dezelfde algemene naam wordt gebruikt, kunt u meerdere verlener vingerafdrukken opgeven. Voor het gemak kunt u hetzelfde certificaat gebruiken voor ClusterCertificateCommonNames en ServerCertificateCommonNames. U kunt één of twee algemene servercertificaatnamen gebruiken. |
| ServerCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat dat overeenkomt met de verlener van het servercertificaat. Algemene naam en de naam van het bijbehorende store in deze sectie in plaats van de vingerafdruk van de uitgever onder ServerCertificateCommonNames geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk overschakeling van de server verleende certificaten. Meerdere uitgevers van certificaten kunnen worden opgegeven als meer dan één servercertificaat wordt gebruikt. Een leeg IssuerCommonName whitelists alle certificaten in de bijbehorende opgeslagen opgegeven onder X509StoreNames.|
| ClientCertificateThumbprints |Deze verzameling certificaten installeren op de geverifieerde clients. U kunt een aantal andere client-certificaten die zijn geïnstalleerd op de computers die u wilt toestaan toegang tot het cluster hebben. De vingerafdruk van elk certificaat in de variabele CertificateThumbprint instellen. Als u IsAdmin ingesteld op *true*, de client met dit certificaat geïnstalleerd op deze beheerder kan doen beheeractiviteiten op het cluster. Als IsAdmin *false*, de client met dit certificaat de acties die zijn alleen toegestaan voor de toegangsrechten voor gebruikers, meestal het kenmerk alleen-lezen kan uitvoeren. Zie voor meer informatie over functies [op rollen gebaseerde toegangsbeheer (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| ClientCertificateCommonNames |De algemene naam van het certificaat van de eerste client ingesteld voor de CertificateCommonName. De CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat. Zie voor meer informatie over algemene namen en de verlener, [werken met certificaten](https://msdn.microsoft.com/library/ms731899.aspx). |
| ClientCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat dat overeenkomt met de verlener van het clientcertificaat (zowel admin en niet-beheerders rollen). Algemene naam en de naam van het bijbehorende store in deze sectie in plaats van de vingerafdruk van de uitgever onder ClientCertificateCommonNames geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk overschakeling van de client verleende certificaten. Meerdere uitgevers van certificaten kunnen worden opgegeven als meer dan één clientcertificaat wordt gebruikt. Een leeg IssuerCommonName whitelists alle certificaten in de bijbehorende opgeslagen opgegeven onder X509StoreNames.|
| ReverseProxyCertificate |Aanbevolen voor een testomgeving. Dit optioneel certificaat kan worden opgegeven als u wilt beveiligen uw [omgekeerde proxy](service-fabric-reverseproxy.md). Zorg ervoor dat reverseProxyEndpointPort is ingesteld in nodeTypes als u dit certificaat gebruiken. |
| ReverseProxyCertificateCommonNames |Aanbevolen voor een productie-omgeving. Dit optioneel certificaat kan worden opgegeven als u wilt beveiligen uw [omgekeerde proxy](service-fabric-reverseproxy.md). Zorg ervoor dat reverseProxyEndpointPort is ingesteld in nodeTypes als u dit certificaat gebruiken. |

Hier volgt een voorbeeld van de configuratie van het cluster waar het cluster, server en clientcertificaten zijn verleend. Voor server-cluster/reverseProxy certificaten, de vingerafdruk en de algemene naam kunnen niet worden geconfigureerd samen voor hetzelfde certificaattype.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "10-2017",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClusterCertificateIssuerStores": [
                    {
                        "IssuerCommonName": "ClusterIssuer1",
                        "X509StoreNames" : "Root"
                    },
                    {
                        "IssuerCommonName": "ClusterIssuer2",
                        "X509StoreNames" : "Root"
                    }
                ],
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName",
                      "CertificateIssuerThumbprint": "7c fc 91 97 13 16 8d ff a8 ee 71 2b a2 f4 62 62 00 03 49 0d"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-rollover"></a>Overschakeling van certificaat
Wanneer u de algemene naam van een certificaat in plaats van een vingerafdruk gebruikt, vereist geen certificaat rollover de upgrade van een cluster configuratie. Zorg ervoor dat de nieuwe vingerafdruk lijst met de oude lijst in de polygoonring verlener vingerafdruk upgrades. U moet eerst een upgrade van een configuratie met de nieuwe verlener vingerafdrukken doen en installeer vervolgens de nieuwe certificaten (certificaat van de cluster of de server en verleende certificaten) in het archief. Houd het oude certificaat van de verlener in het certificaatarchief voor ten minste twee uur na de installatie van het nieuwe certificaat van de verlener.
Als u van verlener winkels gebruikmaakt, klikt u vervolgens moet geen upgrade van de configuratie worden uitgevoerd voor de overschakeling van de certificaatverlener. Het nieuwe certificaat van de verlener installeren met een laatste vervaldatum in het certificaatarchief van de bijbehorende en verwijder de oude uitgeverscertificaat na enkele uren.

## <a name="acquire-the-x509-certificates"></a>De X.509-certificaten verkrijgen
Als u wilt beveiligen van communicatie binnen het cluster, moet u eerst x.509-certificaten verkrijgen voor de clusterknooppunten. Als u wilt beperken verbinding met dit cluster gemachtigde machines/gebruikers, moet u bovendien verkrijgen en installeren van certificaten voor de clientcomputers.

Voor clusters die productie-workloads worden uitgevoerd, gebruikt u een [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority)-x.509-certificaat voor het beveiligen van het cluster heeft ondertekend. Zie voor meer informatie over het verkrijgen van deze certificaten [het verkrijgen van een certificaat](http://msdn.microsoft.com/library/aa702761.aspx).

Voor clusters die u voor testdoeleinden gebruikt, kunt u een zelfondertekend certificaat gebruiken.

## <a name="optional-create-a-self-signed-certificate"></a>Optioneel: Een zelfondertekend certificaat maken
Een manier om een zelfondertekend certificaat maken dat correct kan worden beveiligd is het gebruik van het script CertSetup.ps1 in de Service Fabric SDK-map in de map C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure. Dit bestand als u wilt de standaardnaam van het certificaat wijzigen bewerken. (Zoek naar de waarde CN = ServiceFabricDevClusterCert.) Voer dit script als `.\CertSetup.ps1 -Install`.

Nu u het certificaat exporteren naar een pfx-bestand met een wachtwoord beveiligd. De vingerafdruk van het certificaat eerst worden opgehaald. 
1. Van de **Start** menu uitvoeren **computercertificaten beheren**. 

2. Ga naar de **Local Computer\Personal** map en zoek het certificaat dat u hebt gemaakt. 

3. Dubbelklik op het certificaat te openen, selecteer de **Details** tabblad en schuif omlaag naar de **vingerafdruk** veld. 

4. Verwijder de spaties en de vingerafdrukwaarde kopiëren naar de volgende PowerShell-opdracht. 

5. Wijzig de `String` waarde naar een geschikte beveiligd wachtwoord te beveiligen en voer het volgende in PowerShell:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Voer de volgende PowerShell-opdracht voor de details van een certificaat geïnstalleerd op de computer:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

U kunt ook als u een Azure-abonnement hebt, volg de stappen in [Service Fabric-cluster maken met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="install-the-certificates"></a>Installeer de certificaten
Nadat u certificaten hebt, kunt u deze installeren op de clusterknooppunten. Uw knooppunten moeten hebben de nieuwste Windows PowerShell 3.x daarop geïnstalleerd. Herhaal deze stappen op elk knooppunt voor het cluster en de servercertificaten en eventuele secundaire certificaten.

1. Kopieer het .pfx-bestand of de bestanden naar het knooppunt.

2. Open een PowerShell-venster als beheerder en voer de volgende opdrachten. Vervang *$pswd* met het wachtwoord dat u gebruikt voor het maken van dit certificaat. Vervang *$PfxFilePath* met het volledige pad van het pfx gekopieerd naar dit knooppunt.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Nu het toegangsbeheer voor dit certificaat zo instellen dat de Service Fabric-proces wordt uitgevoerd onder het account Netwerkservice gebruikt, kan worden gebruikt door het volgende script wordt uitgevoerd. Geef de vingerafdruk van het certificaat en **netwerkservice** voor het serviceaccount. U kunt controleren of de ACL's op het certificaat correct zijn door het openen van het certificaat in **Start** > **computercertificaten beheren** en kijken **alle taken**  >  **Persoonlijke sleutels beheren**.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions, and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine-related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current ACL of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ACE to the ACL of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new ACL
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate
    get-acl $keyFullPath| fl
    ```
4. Herhaal de vorige stappen voor elk servercertificaat. Ook kunt u deze stappen voor het installeren van de clientcertificaten op de machines die u wilt toestaan toegang tot het cluster.

## <a name="create-the-secure-cluster"></a>Het beveiligde cluster maken
Nadat u de Beveiligingssectie van het bestand ClusterConfig.X509.MultiMachine.json configureert, kunt u doorgaan met de [maken van het cluster](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster) sectie voor het configureren van de knooppunten en het zelfstandige cluster maken. Moet het bestand ClusterConfig.X509.MultiMachine.json gebruiken bij het maken van het cluster. De opdracht kan er bijvoorbeeld als volgt uitzien:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Nadat u de Windows beveiligde zelfstandige cluster correct functioneert en geverifieerde clients verbinding kunnen maken hebt ingesteld hebt, volg de stappen in de sectie [verbinding maken met een cluster met behulp van PowerShell](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell) tot stand te brengen. Bijvoorbeeld:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Vervolgens kunt u andere PowerShell-opdrachten voor gebruik met dit cluster uitvoeren. Bijvoorbeeld, u kunt uitvoeren [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) om een lijst met knooppunten op dit beveiligde cluster weer te geven.


Als u wilt verwijderen van het cluster, verbinding maken met het knooppunt in het cluster waar u het Service Fabric-pakket hebt gedownload, open een opdrachtregel en gaat u naar de pakketmap. Voer nu de volgende opdracht:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Configuratie van een onjuist certificaat mogelijk voorkomen dat het cluster komende tijdens de implementatie. Om op te sporen zelf beveiligingsproblemen, zoeken in de Event Viewer-groep **logboeken toepassingen en Services** > **Microsoft Service Fabric**.
> 
> 


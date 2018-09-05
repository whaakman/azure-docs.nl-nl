---
title: Beveiligen van een Azure Service Fabric-cluster in Windows met behulp van certificaten | Microsoft Docs
description: Beveiligde communicatie binnen een Azure Service Fabric-cluster voor zelfstandige of on-premises, evenals tussen clients en het cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: b32e9628b80705a99a2a8cf77534ed37a47515c8
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669093"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>Een zelfstandige cluster beveiligen op Windows met behulp van X.509-certificaten
In dit artikel wordt beschreven hoe u beveiligde communicatie tussen de verschillende knooppunten van uw zelfstandige Windows-cluster. Ook wordt beschreven hoe u verificatie van clients die verbinding met dit cluster maken met behulp van X.509-certificaten. Verificatie zorgt ervoor dat alleen geautoriseerde gebruikers kunnen toegang krijgen het cluster en de geïmplementeerde toepassingen tot en beheertaken uitvoeren. Certificaatbeveiliging moet worden ingeschakeld op het cluster als het cluster is gemaakt.  

Zie voor meer informatie over de clusterbeveiliging, zoals beveiliging van knooppunt-naar-knooppunt, client-naar-knooppunt beveiliging en op rollen gebaseerd toegangsbeheer, [clusterbeveiligingsscenario's](service-fabric-cluster-security.md).

## <a name="which-certificates-do-you-need"></a>Welke certificaten hebt u nodig?
Te beginnen met [downloadpakket van de Service Fabric voor Windows Server](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) op een van de knooppunten in uw cluster. In het gedownloade pakket, moet u een bestand ClusterConfig.X509.MultiMachine.json vinden. Open het bestand en Raadpleeg de sectie voor beveiliging onder de sectie met eigenschappen:

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

Deze sectie beschrijft de certificaten die u nodig hebt voor het beveiligen van uw zelfstandige Windows-cluster. Als u een clustercertificaat opgeeft, stelt u de waarde van ClusterCredentialType naar _X509_. Als u een servercertificaat voor externe verbindingen opgeeft, stelt u de ServerCredentialType op _X509_. Hoewel dit niet verplicht is, wordt u aangeraden dat u beide certificaten voor een goed beveiligde cluster hebt. Als u deze waarden ingesteld op *X509*, moet u ook de bijbehorende certificaten opgeven of de Service Fabric een uitzondering genereert. In sommige scenario's kunt u alleen opgeeft de _ClientCertificateThumbprints_ of de _ReverseProxyCertificate_. In deze scenario's, moet u niet instellen _ClusterCredentialType_ of _ServerCredentialType_ naar _X509_.


> [!NOTE]
> Een [vingerafdruk](https://en.wikipedia.org/wiki/Public_key_fingerprint) is de primaire identiteit van een certificaat. Als u wilt weten de vingerafdruk van de certificaten die u maakt, Zie [een vingerafdruk van een certificaat ophalen](https://msdn.microsoft.com/library/ms734695.aspx).
> 
> 

De volgende tabel vindt u de certificaten die u nodig hebt van de cluster-instellingen:

| **CertificateInformation instelling** | **Beschrijving** |
| --- | --- |
| ClusterCertificate |Aanbevolen voor een testomgeving. Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt twee verschillende certificaten, een primaire en een secundaire gebruiken voor de upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die de secundaire database in de variabelen ThumbprintSecondary. |
| ClusterCertificateCommonNames |Aanbevolen voor een productie-omgeving. Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt één of twee cluster certificaat algemene namen gebruiken. De CertificateIssuerThumbprint komt overeen met de vingerafdruk van de verlener van dit certificaat. Als meer dan één certificaat met dezelfde algemene naam wordt gebruikt, kunt u meerdere verlener vingerafdrukken opgeven.|
| ClusterCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat komt overeen met de uitgever van het clustercertificaat. Algemene naam en de bijbehorende winkelnaam in deze sectie in plaats van de vingerafdruk van de uitgever onder ClusterCertificateCommonNames op te geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk clustercertificaten verlener. Meerdere certificaatverleners kunnen worden opgegeven als meer dan één cluster-certificaat wordt gebruikt. Een lege IssuerCommonName accounttoewijzing alle certificaten in de bijbehorende winkels opgegeven onder X509StoreNames.|
| ServerCertificate |Aanbevolen voor een testomgeving. Dit certificaat wordt weergegeven op de client als er wordt geprobeerd verbinding maken met dit cluster. Voor het gemak kunt u hetzelfde certificaat gebruiken voor ClusterCertificate en ServerCertificate. U kunt twee verschillende certificaten, een primaire en een secundaire gebruiken voor de upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie vingerafdruk en die de secundaire database in de variabelen ThumbprintSecondary. |
| ServerCertificateCommonNames |Aanbevolen voor een productie-omgeving. Dit certificaat wordt weergegeven op de client als er wordt geprobeerd verbinding maken met dit cluster. De CertificateIssuerThumbprint komt overeen met de vingerafdruk van de verlener van dit certificaat. Als meer dan één certificaat met dezelfde algemene naam wordt gebruikt, kunt u meerdere verlener vingerafdrukken opgeven. Voor het gemak kunt u hetzelfde certificaat gebruiken voor ClusterCertificateCommonNames en ServerCertificateCommonNames. U kunt één of twee algemene namen van servercertificaten gebruiken. |
| ServerCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat komt overeen met de uitgever van het servercertificaat. Algemene naam en de bijbehorende winkelnaam in deze sectie in plaats van de vingerafdruk van de uitgever onder ServerCertificateCommonNames op te geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk rollover server verleende certificaten. Meerdere certificaatverleners kunnen worden opgegeven als meer dan één servercertificaat wordt gebruikt. Een lege IssuerCommonName accounttoewijzing alle certificaten in de bijbehorende winkels opgegeven onder X509StoreNames.|
| ClientCertificateThumbprints |Deze reeks certificaten installeren op de geverifieerde clients. U kunt een aantal andere client-certificaten zijn geïnstalleerd op de machines die u wilt toestaan dat toegang tot het cluster hebben. De vingerafdruk van elk certificaat in de variabele CertificateThumbprint instellen. Als u IsAdmin ingesteld op *waar*, de client met dit certificaat geïnstalleerd op deze beheerder kan doen beheeractiviteiten op het cluster. Als IsAdmin *false*, de client met dit certificaat de acties die zijn alleen toegestaan voor gebruikerstoegang rechten, doorgaans alleen-lezen kan uitvoeren. Zie voor meer informatie over rollen [Role-Based Access Control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| ClientCertificateCommonNames |De algemene naam van het eerste certificaat ingesteld voor de CertificateCommonName. De CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat. Zie voor meer informatie over algemene namen en de uitgever, [werken met certificaten](https://msdn.microsoft.com/library/ms731899.aspx). |
| ClientCertificateIssuerStores |Aanbevolen voor een productie-omgeving. Dit certificaat komt overeen met de uitgever van het clientcertificaat (zowel niet-beheerders als beheerder rollen). Algemene naam en de bijbehorende winkelnaam in deze sectie in plaats van de vingerafdruk van de uitgever onder ClientCertificateCommonNames op te geven, kunt u de uitgever opgeven.  Hierdoor kunt gemakkelijk rollover client verleende certificaten. Meerdere certificaatverleners kunnen worden opgegeven als meer dan één clientcertificaat wordt gebruikt. Een lege IssuerCommonName accounttoewijzing alle certificaten in de bijbehorende winkels opgegeven onder X509StoreNames.|
| ReverseProxyCertificate |Aanbevolen voor een testomgeving. Deze optionele certificaat kan worden opgegeven als u wilt voor het beveiligen van uw [omgekeerde proxy](service-fabric-reverseproxy.md). Zorg ervoor dat reverseProxyEndpointPort in nodeTypes is ingesteld als u dit certificaat. |
| ReverseProxyCertificateCommonNames |Aanbevolen voor een productie-omgeving. Deze optionele certificaat kan worden opgegeven als u wilt voor het beveiligen van uw [omgekeerde proxy](service-fabric-reverseproxy.md). Zorg ervoor dat reverseProxyEndpointPort in nodeTypes is ingesteld als u dit certificaat. |

Hier volgt een voorbeeldconfiguratie voor het cluster waar de cluster-, server- en clientcertificaten is opgegeven. Worden cluster/server/reverseProxy-certificaten de vingerafdruk en de algemene naam kunnen niet geconfigureerd samen voor hetzelfde certificaattype.

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

## <a name="certificate-rollover"></a>Rollover van certificaten
Wanneer u de algemene naam van een certificaat in plaats van een vingerafdruk gebruiken, rollover van certificaten zijn vereist om de upgrade van een cluster-configuratie. Zorg ervoor dat de nieuwe vingerafdruk lijst met de oude lijst samen verlener vingerafdruk-upgrades. U moet eerst een upgrade configuratie uit te voeren met de vingerafdrukken van de nieuwe verlener en installeer vervolgens de nieuwe certificaten (cluster/servercertificaat en verleende certificaten) in de store. Houd het oude certificaat van de verlener in het certificaatarchief voor ten minste twee uur na de installatie van het nieuwe uitgeverscertificaat.
Als u van verlener winkels gebruikmaakt, klikt u vervolgens moet geen configuratie-upgrade worden uitgevoerd rollover van de uitgever van certificaten. Het nieuwe uitgeverscertificaat met een vervaldatum van het laatste installeren in het certificaatarchief van de bijbehorende en verwijder het oude uitgeverscertificaat na een paar uur.

## <a name="acquire-the-x509-certificates"></a>De X.509-certificaten verkrijgen
Als u wilt beveiligen van communicatie binnen het cluster, moet u eerst om op te halen van X.509-certificaten voor de clusterknooppunten. Als u wilt verbinding met dit cluster gemachtigde machines/gebruikers beperken, moet u bovendien het verkrijgen en installeren van certificaten voor de clientcomputers.

Voor clusters die productieworkloads worden uitgevoerd, gebruikt u een [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority)-ondertekend X.509-certificaat voor het beveiligen van het cluster. Zie voor meer informatie over het verkrijgen van deze certificaten [hoe u een certificaat verkrijgen](http://msdn.microsoft.com/library/aa702761.aspx).

Voor clusters die u voor test-doeleinden gebruikt, kunt u een zelfondertekend certificaat gebruiken.

## <a name="optional-create-a-self-signed-certificate"></a>Optioneel: Een zelfondertekend certificaat maken
Eén manier om u te maken van een zelfondertekend certificaat die correct kan worden beveiligd is met het script CertSetup.ps1 in de Service Fabric-SDK-map in de map C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure. Dit bestand als u wilt de standaardnaam van het certificaat wijzigen bewerken. (Kijk naar de waarde CN = ServiceFabricDevClusterCert.) Voer dit script als `.\CertSetup.ps1 -Install`.

Nu het certificaat exporteren naar een pfx-bestand met een wachtwoord beveiligd. Haal eerst de vingerafdruk van het certificaat. 
1. Uit de **Start** in het menu uitvoeren **computercertificaten beheren**. 

2. Ga naar de **Local Computer\Personal** map en zoek het certificaat dat u hebt gemaakt. 

3. Dubbelklik op het certificaat te openen, selecteert u de **Details** tabblad, en schuif omlaag naar de **vingerafdruk** veld. 

4. Verwijder de spaties en kopieer de vingerafdrukwaarde in de volgende PowerShell-opdracht. 

5. Wijzig de `String` waarde naar een geschikte beveiligd wachtwoord te beveiligen, en voer de volgende in PowerShell:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Als u wilt zien van de details van een certificaat geïnstalleerd op de computer, voer de volgende PowerShell-opdracht:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

U kunt ook als u een Azure-abonnement hebt, volg de stappen in [maken van een Service Fabric-cluster met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="install-the-certificates"></a>De certificaten installeren
Nadat u de certificaten hebt, kunt u deze installeren op de clusterknooppunten. Uw knooppunten moeten hebben de nieuwste versie van Windows PowerShell 3.x die erop is geïnstalleerd. Herhaal deze stappen op elk knooppunt voor het cluster en servercertificaten en eventuele secundaire certificaten.

1. Kopieer het .pfx-bestand of de bestanden naar het knooppunt.

2. Open een PowerShell-venster als beheerder en voer de volgende opdrachten. Vervang *$pswd* met het wachtwoord die u hebt gebruikt om dit certificaat te maken. Vervang *$PfxFilePath* met het volledige pad van het .pfx-bestand gekopieerd naar dit knooppunt.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Nu het toegangsbeheer voor dit certificaat zo instellen dat de Service Fabric-proces, dat wordt uitgevoerd onder het account Network Service, kan worden gebruikt door het volgende script is uitgevoerd. Geef de vingerafdruk van het certificaat en **netwerkservice** voor het serviceaccount. U kunt controleren of de ACL's op het certificaat juist zijn door het openen van het certificaat in **Start** > **computercertificaten beheren** en kijken **alle taken**  >  **Persoonlijke sleutels beheren**.
   
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
4. Herhaal de vorige stappen voor elke servercertificaat. U kunt deze stappen ook gebruiken voor het installeren van de clientcertificaten op de machines die u wilt toestaan dat toegang tot het cluster.

## <a name="create-the-secure-cluster"></a>Het beveiligde cluster maken
Nadat u de Beveiligingssectie van het bestand ClusterConfig.X509.MultiMachine.json hebt geconfigureerd, kunt u doorgaan met de [maken van het cluster](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster) sectie voor het configureren van de knooppunten en een zelfstandige cluster maken. Houd er rekening mee aan het bestand ClusterConfig.X509.MultiMachine.json gebruiken bij het maken van het cluster. De opdracht kan er bijvoorbeeld als volgt uitzien:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Nadat u het veilige zelfstandige Windows hebt-cluster uit te voeren en de geverifieerde clients verbinding maken met deze hebt ingesteld, volgt u de stappen in de sectie [verbinding maken met een cluster met behulp van PowerShell](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell) tot stand te brengen. Bijvoorbeeld:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Vervolgens kunt u andere PowerShell-opdrachten om te werken met dit cluster uitvoeren. Bijvoorbeeld, u kunt uitvoeren [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om een lijst met knooppunten in het beveiligde cluster weer te geven.


Als u wilt verwijderen van het cluster, verbinding maken met het knooppunt in het cluster waar u de Service Fabric-pakket hebt gedownload, opent u een opdrachtregel en gaat u naar de pakketmap. Voer nu de volgende opdracht:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> De configuratie van onjuist certificaat kan voorkomen dat het cluster tijdens de implementatie te komen. Als u wilt vaststellen zelf beveiligingsproblemen met zich mee, zoeken in de Event Viewer groep **logboeken toepassingen en Services** > **Microsoft Service Fabric**.
> 
> 


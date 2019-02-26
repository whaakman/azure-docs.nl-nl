---
title: Azure Service Fabric best practices voor beveiliging | Microsoft Docs
description: Aanbevolen procedures voor Azure Service Fabric-beveiliging.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 350aef037f019733e02331623758c14a3c64ab50
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804989"
---
# <a name="azure-service-fabric-security"></a>Azure Service Fabric-beveiliging 

Voor meer informatie over [aanbevolen beveiligingsprocedures voor Azure](https://docs.microsoft.com/azure/security/), Bekijk [aanbevolen beveiligingsprocedures voor Azure Service Fabric](https://docs.microsoft.com/azure/security/azure-service-fabric-security-best-practices)

## <a name="key-vault"></a>Key Vault

[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) is de aanbevolen geheimen management-service voor Azure Service Fabric-toepassingen en -clusters.
> [!NOTE]
> Als certificaten/geheimen in een Key Vault wordt geïmplementeerd voor een virtuele-Machineschaalset opgehaald als een virtuele Machine-geheim voor het instellen van schaal, moet de Key Vault en de virtuele-Machineschaalset dezelfde locatie bevindt.

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>Maken van de certificeringsinstantie uitgegeven certificaat voor Service Fabric

Een Azure Key Vault-certificaat kan worden gemaakt of geïmporteerd in een Key Vault. Wanneer een Key Vault-certificaat is gemaakt, wordt de persoonlijke sleutel gemaakt binnen de Key Vault en nooit worden blootgesteld aan de eigenaar van het certificaat. Dit zijn de manieren om te maken van een certificaat in Key Vault:

- Maak een zelfondertekend certificaat voor het maken van een openbaar / persoonlijk sleutelpaar en deze koppelen aan een certificaat. Het certificaat zal worden ondertekend door een eigen sleutel. 
- Maak een nieuw certificaat handmatig te maken van een openbaar / persoonlijk sleutelpaar en een X.509-aanvraag voor Certificaatondertekening genereren. De aanvraag voor Certificaatondertekening kan worden ondertekend door uw Statusregistratie-instanties of de certificeringsinstantie (CA). De ondertekende x509 certificaat kan worden samengevoegd met de in behandeling sleutel worden gekoppeld aan het voltooien van het certificaat KV in Key Vault. Hoewel deze methode zijn meer stappen vereist, biedt u met betere beveiliging omdat de persoonlijke sleutel is gemaakt in en tot Key Vault beperkt. Dit wordt uitgelegd in het onderstaande diagram. 

Beoordeling [Aanmaakmethoden voor Azure Key Vault-certificaat](https://docs.microsoft.com/azure/key-vault/create-certificate) voor meer informatie.

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>Key Vault-certificaten implementeren op Service Fabric-cluster virtuele-machineschaalsets

Gebruik voor de implementatie van certificaten van een CO-locaties keyvault aan een virtuele-Machineschaalset, virtuele-Machineschaalset [osProfile](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile). Hier volgen de sjablooneigenschappen van de Resource Manager:

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> De kluis moet zijn ingeschakeld voor de sjabloonimplementatie van Resource Manager.

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>Een lijst met ACL (Access Control) van toepassing op uw certificaat voor uw Service Fabric-cluster

[Extensies voor virtuele-Machineschaalset](https://docs.microsoft.com/cli/azure/vmss/extension?view=azure-cli-latest) publisher Microsoft.Azure.ServiceFabric wordt gebruikt voor het configureren van de beveiliging van uw knooppunten.
Als u wilt een ACL toepassen op uw certificaten voor de processen van uw Service Fabric-Cluster, gebruikt u de volgende sjablooneigenschappen van de Resource Manager:

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>Een certificaat voor Service Fabric-cluster beveiligen door algemene naam

Voor het beveiligen van uw Service Fabric-cluster door certificaat `Common Name`, gebruikt u de sjablooneigenschap Resource Manager- [certificateCommonNames](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames), als volgt:

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> Service Fabric-clusters wordt het eerste geldige certificaat dat wordt gevonden in het certificaatarchief van de host gebruikt. Dit is op Windows, het certificaat met de meest recente verlopende datum die overeenkomt met de vingerafdruk van de algemene naam en -Verlenersleutel.

Azure-domeinen, zoals *\<uw SUBDOMEIN\>. cloudapp.azure.com of \<uw SUBDOMEIN\>. trafficmanager.net, eigendom zijn van Microsoft. Certificeringsinstanties geeft geen certificaten voor domeinen voor onbevoegde gebruikers. De meeste gebruikers moet bij een registrar koopt van een domein of een beheerder zijn geautoriseerd domein, voor een certificeringsinstantie voor uitgifte u een certificaat met de algemene naam.

Voor meer informatie over het configureren van DNS-Service voor uw domein worden omgezet in een Microsoft-IP-adres, het configureren van bekijken [Azure DNS voor het hosten van uw domein](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

> [!NOTE]
> Nadat het delegeren van de naamservers van domeinen naar de naamservers van uw Azure DNS-zone toevoegen de volgende twee records aan de DNS-Zone:
> - Een ' een ' record voor domein APEX die niet een `Alias record set` alle IP-adressen uw aangepaste domein wordt opgelost.
> - Een 'C'-record voor Microsoft sub domeinen u ingericht die niet een `Alias record set`. U kunt bijvoorbeeld uw Traffic Manager of de DNS-naam van de Load Balancer gebruiken.

Bijwerken van de portal om een aangepaste DNS-naam voor uw Service Fabric-Cluster weer te geven `"managementEndpoint"`, bijwerken van de volgende eigenschappen van de Service Fabric Cluster Resource Manager-sjabloon:

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>Versleutelen van Service Fabric-pakket geheime waarden

Algemene waarden die zijn versleuteld in Service Fabric-pakketten zijn referenties voor Azure Container Registry (ACR), omgevingsvariabelen, instellingen en Volume van de Azure-invoegtoepassing opslagaccountsleutels.

Naar [een versleutelingscertificaat instellen en geheimen op Windows-clusters versleutelen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-windows):

Een zelfondertekend certificaat voor het versleutelen van uw geheim genereren:

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

Volg de instructies in [certificaten van Key Vault implementeren met Service Fabric-cluster virtuele machine-schaalsets](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) Key Vault-certificaten implementeren naar uw Service Fabric-Cluster Virtual Machine Scale Sets.

Versleutelen van het geheim met de volgende PowerShell-opdracht, en werk vervolgens het manifest van uw Service Fabric-toepassing met de versleutelde waarde:

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Naar [een versleutelingscertificaat instellen en geheimen op Linux-clusters versleutelen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-linux):

Een zelfondertekend certificaat voor het versleutelen van uw geheimen genereren:

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

Volg de instructies in [certificaten van Key Vault implementeren met Service Fabric-cluster virtuele machine-schaalsets](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) naar uw Service Fabric-Cluster Virtual Machine Scale Sets.

Versleutelen van het geheim met de volgende opdrachten en werk vervolgens uw Service Fabric Application Manifest met de versleutelde waarde:

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

Na het versleutelen van uw beveiligde waarden [versleutelde geheimen in Service Fabric-toepassing opgeven](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application), en [ontsleutelen versleutelde geheimen van de servicecode](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code).

## <a name="authenticate-service-fabric-applications-to-azure-resources-using-managed-service-identity-msi"></a>Service Fabric-toepassingen het Azure-Resources met behulp van Managed Service Identity (MSI) verifiëren

Zie voor meer informatie over beheerde identiteiten voor Azure-resources, [wat is beheerde identiteiten voor Azure-resources?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-does-it-work).
Azure Service Fabric-clusters worden gehost op de Virtual Machine Scale Sets, die ondersteuning bieden voor [beheerde Service-identiteit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-managed-identities-for-azure-resources).
Voor een lijst met services die MSI-bestand kan worden gebruikt om te verifiëren als u wilt, Zie [Azure-Services die ondersteuning bieden voor Azure Active Directory-verificatie](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-azure-ad-authentication).


Om in te schakelen systeem beheerde identiteit toegewezen tijdens het maken van een schaalset voor virtuele machines of een bestaande schaalset voor virtuele machines, declareert u de volgende `"Microsoft.Compute/virtualMachinesScaleSets"` eigenschap:

```json
"identity": { 
    "type": "SystemAssigned"
}
```
Zie [wat is beheerde identiteiten voor Azure-resources?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vmss#system-assigned-managed-identity) voor meer informatie.

Als u hebt gemaakt een [gebruiker toegewezen beheerde identiteit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm#create-a-user-assigned-managed-identity), de volgende resource in de sjabloon toe te wijzen aan uw virtuele-machineschaalset declareren. Vervang `\<USERASSIGNEDIDENTITYNAME\>` beheerd door de naam van de gebruiker toegewezen identiteit die u hebt gemaakt:

```json
"identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
        "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
    }
}
```

Voordat u uw Service Fabric toepassing kan maken gebruik van een beheerde identiteit, moeten machtigingen worden verleend aan de Azure-Resources die deze moet worden geverifieerd bij.
De volgende opdrachten verlenen toegang tot een Azure-Resource:

```bash
principalid=$(az resource show --id /subscriptions/<YOUR SUBSCRIPTON>/resourceGroups/<YOUR RG>/providers/Microsoft.Compute/virtualMachineScaleSets/<YOUR SCALE SET> --api-version 2018-06-01 | python -c "import sys, json; print(json.load(sys.stdin)['identity']['principalId'])")

az role assignment create --assignee $principalid --role 'Contributor' --scope "/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/<PROVIDER NAME>/<RESOURCE TYPE>/<RESOURCE NAME>"
```

In de code van uw Service Fabric-toepassing, verkrijgt u een toegangstoken voor Azure Resource Manager doordat een op REST alle vergelijkbaar met het volgende:

```bash
access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true | python -c "import sys, json; print json.load(sys.stdin)['access_token']")

```

Uw Service Fabric-app kunt vervolgens het toegangstoken gebruiken om te verifiëren bij Azure-Resources die ondersteuning bieden voor Active Directory.
Het volgende voorbeeld ziet u hoe u dit doet voor Cosmos DB-resource:

```bash
cosmos_db_password=$(curl 'https://management.azure.com/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/Microsoft.DocumentDB/databaseAccounts/<YOUR ACCOUNT>/listKeys?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer $access_token" | python -c "import sys, json; print(json.load(sys.stdin)['primaryMasterKey'])")
```

## <a name="windows-defender"></a>Windows Defender 

Windows Defender antivirus is standaard geïnstalleerd op Windows Server 2016. Zie voor meer informatie, [Windows Defender Antivirus op Windows Server 2016](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016). De gebruikersinterface wordt standaard geïnstalleerd op een aantal SKU's, maar is niet vereist. Om te beperken prestaties impact en resourceverbruik overhead in rekening gebracht door Windows Defender, en als uw beveiligingsbeleid kunnen u processen en paden voor open source-software, uitsluiten declareert u de volgende virtuele Machine schaal ingesteld extensie-Resource Sjablooneigenschappen Manager uw Service Fabric-cluster uitsluiten van scans:


```json
 {
    "name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
    "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
            "AntimalwareEnabled": "true",
            "Exclusions": {
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
            },
            "RealtimeProtectionEnabled": "true",
            "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
            }
        },
        "protectedSettings": null
    }
}
```

> [!NOTE]
> Raadpleeg de documentatie van uw anti-malware voor configuratieregels als u geen van Windows Defender gebruikmaakt. Windows Defender wordt niet ondersteund op Linux.

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op VM's of computers waarop Windows Server wordt uitgevoerd: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md).
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
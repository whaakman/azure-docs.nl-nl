---
title: Een Azure Service Fabric-cluster maken van een sjabloon | Microsoft Docs
description: In dit artikel wordt beschreven hoe voor het instellen van een beveiligde Service Fabric-cluster in Azure met behulp van Azure Resource Manager en Azure Key Vault Azure Active Directory (Azure AD) voor clientverificatie.
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 47152d05eb7e31e7fe1f35e33a10fe8e903e21e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Maken van een Service Fabric-cluster met behulp van Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Deze stapsgewijze handleiding helpt u bij het instellen van een beveiligde Azure Service Fabric-cluster in Azure met Azure Resource Manager. We erkennen dat het artikel lang is. Niettemin, tenzij u al grondig bekend met de inhoud bent, moet u de stappen zorgvuldig.

De handleiding wordt ingegaan op de volgende procedures:

* Het instellen van een Azure sleutelkluis voor certificaten voor een cluster en de toepassing beveiliging uploaden
* Maken van een beveiligde cluster in Azure met Azure Resource Manager
* Verifiëren van gebruikers met behulp van Azure Active Directory (Azure AD) voor Clusterbeheer

Een beveiligde cluster is een cluster dat onbevoegde toegang tot beheertaken uit te voeren voorkomt. Dit omvat implementeren, bijwerken en verwijderen van toepassingen, services en de gegevens die ze bevatten. Een niet-beveiligde cluster is een cluster iedereen kan verbinding maken met op elk gewenst moment en beheerbewerkingen uitvoeren. Hoewel het mogelijk te maken van een niet-beveiligde cluster, raden we u aan een beveiligde cluster te maken vanaf het begin. Omdat een niet-beveiligde cluster later niet kan worden beveiligd, kan een nieuw cluster moet worden gemaakt.

Het concept van het maken van beveiligde clusters is hetzelfde, of ze Linux of Windows-clusters. Zie voor meer informatie en helper scripts voor het maken van beveiligde Linux-clusters, [maken van beveiligde clusters onder Linux](#secure-linux-clusters).

## <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account
Maakt gebruik van deze handleiding [Azure PowerShell][azure-powershell]. Wanneer u een nieuwe PowerShell-sessie start, zich aanmelden bij uw Azure-account en uw abonnement te selecteren voordat u Azure-opdrachten uitvoeren.

Meld u bij uw Azure-account:

```powershell
Login-AzureRmAccount
```

Selecteer uw abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Een sleutelkluis instellen
Deze sectie wordt beschreven voor het maken van een sleutelkluis voor een Service Fabric-cluster in Azure en voor Service Fabric-toepassingen. Voor een volledige handleiding voor Azure Sleutelkluis, raadpleegt u de [Sleutelkluis instructiehandleiding][key-vault-get-started].

Service Fabric gebruikt x.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven. U Sleutelkluis gebruiken voor het beheren van certificaten voor Service Fabric-clusters in Azure. Wanneer een cluster is geïmplementeerd in Azure, wordt de Azure-resourceprovider die verantwoordelijk is voor het maken van Service Fabric-clusters certificaten ophaalt uit Sleutelkluis en installeert ze op het cluster virtuele machines.

Het volgende diagram illustreert de relatie tussen Azure Sleutelkluis, een Service Fabric-cluster en de Azure-resourceprovider die gebruikmaakt van certificaten die zijn opgeslagen in een sleutelkluis bij het maken van een cluster:

![Diagram van de installatie van het certificaat][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Een resourcegroep maken
De eerste stap is om een resourcegroep specifiek voor uw sleutelkluis te maken. Het is raadzaam dat u de sleutelkluis in een eigen resourcegroep geplaatst. Deze actie kunt u de brongroepen berekeningen en opslag, met inbegrip van de resourcegroep die uw Service Fabric-cluster bevat zonder verlies van uw sleutels en geheimen verwijderen. De resourcegroep die de sleutelkluis bevat _moet zich in dezelfde regio_ als het cluster dat wordt gebruikt.

Als u van plan bent voor het implementeren van clusters in meerdere regio's, het is raadzaam dat u de naam van de resourcegroep en de sleutel op een manier die welke regio hoort aangeeft bij-kluis.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
De uitvoer ziet er als volgt:

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-the-new-resource-group"></a>Een sleutelkluis maken in de nieuwe resourcegroep
De sleutelkluis _moet zijn ingeschakeld voor de implementatie van_ om toe te staan van de resourceprovider voor compute certificaten van het verkrijgen en installeren op virtuele machine-exemplaren:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

De uitvoer ziet er als volgt:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Gebruik een bestaande sleutelkluis

Gebruik een bestaande sleutelkluis u _moet inschakelen voor de implementatie van_ om toe te staan van de resourceprovider voor compute certificaten van het verkrijgen en installeren op de clusterknooppunten:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-your-key-vault"></a>Voeg certificaten aan de sleutelkluis

Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie voor meer informatie over hoe de certificaten worden gebruikt in Service Fabric, [scenario's voor beveiliging van Service Fabric-cluster][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Dit certificaat is vereist voor het beveiligen van een cluster en onbevoegde toegang tot deze voorkomen. Dit biedt een clusterbeveiliging op twee manieren:

* Cluster-verificatie: knooppunt naar communicatie voor cluster federation verifieert. Alleen knooppunten die u hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* Server-verificatie: verifieert de eindpunten van het cluster-beheer met een management-client, zodat de management-client, weet deze communiceert met het echte cluster. Dit certificaat biedt ook een met SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, kan worden geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* De onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster. Deze overeenkomst is vereist voor een met SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de. cloudapp.azure.com domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om beveiligingsredenen toepassing. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van configuratiewaarden die van toepassing.
* Codering van gegevens over knooppunten tijdens de replicatie.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certificaten voor Azure-resource provider opmaak
U kunt toevoegen en gebruiken van persoonlijke sleutel (PFX-bestanden) rechtstreeks via de sleutelkluis. De Azure compute resourceprovider vereist echter sleutels worden opgeslagen in een speciale notatie JSON (JavaScript Object)-indeling. De indeling omvat het .pfx-bestand als een base 64 gecodeerde tekenreeks en het wachtwoord van de persoonlijke sleutel. Deze om vereisten te voldoen, moeten de sleutels worden geplaatst in een JSON-tekenreeks en vervolgens opgeslagen als een 'geheimen' in de sleutelkluis.

Om dit proces vereenvoudigen een [PowerShell-module is beschikbaar op GitHub][service-fabric-rp-helpers]. Ga als volgt te werk voor het gebruik van de module:

1. De volledige inhoud van de opslagplaats downloaden naar een lokale map.
2. Ga naar de lokale map.
2. Importeer de module ServiceFabricRPHelpers in uw PowerShell-venster:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

De `Invoke-AddCertToKeyVault` opdracht in deze PowerShell-module automatisch de persoonlijke sleutel van een certificaat in een JSON-tekenreeks indelingen en geüpload naar de sleutelkluis. Gebruik de opdracht de cluster-certificaat en een extra toepassingscertificaten toevoegen aan de sleutelkluis. Herhaal deze stap voor elke extra certificaten die u wilt installeren in uw cluster.

#### <a name="uploading-an-existing-certificate"></a>Een bestaand certificaat uploaden

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Als er een fout optreedt, zoals wordt weergegeven, betekent dit meestal dat er een conflict resource-URL. Los het conflict op door de sleutelkluis-naam te wijzigen.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Nadat het conflict opgelost is, wordt de uitvoer ziet als volgt:

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>U moet de drie voorgaande tekenreeksen, CertificateThumbprint SourceVault en CertificateURL, voor het instellen van een beveiligde Service Fabric-cluster en het ophalen van de toepassingscertificaten die u voor de beveiliging van toepassingen gebruikt. Als u niet de tekenreeksen opslaat, kan het lastig zijn om op te halen ze later een query uitgevoerd op de sleutelkluis.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault"></a>Een zelfondertekend certificaat maken en uploaden naar de sleutelkluis

Als u al uw certificaten naar de sleutelkluis hebt geüpload, moet u deze stap overslaan. Deze stap is voor een nieuw zelfondertekend certificaat genereren en uploaden naar de sleutelkluis. Nadat u de parameters in het volgende script wijzigen en vervolgens uit te voeren, moet u worden gevraagd om het wachtwoord voor een certificaat.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Als er een fout optreedt, zoals wordt weergegeven, betekent dit meestal dat er een conflict resource-URL. Los het conflict op door wijzigen sleutelkluisnaam, de naam van de RG, enzovoort.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Nadat het conflict opgelost is, wordt de uitvoer ziet als volgt:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>U moet de drie voorgaande tekenreeksen, CertificateThumbprint SourceVault en CertificateURL, voor het instellen van een beveiligde Service Fabric-cluster en het ophalen van de toepassingscertificaten die u voor de beveiliging van toepassingen gebruikt. Als u niet de tekenreeksen opslaat, kan het lastig zijn om op te halen ze later een query uitgevoerd op de sleutelkluis.

 Op dit moment u moet beschikken over de volgende elementen:

* De resourcegroep voor sleutelkluis.
* De sleutelkluis en de bijbehorende URL (aangeroepen SourceVault in de voorgaande PowerShell-uitvoer).
* Het certificaat voor serververificatie cluster en de bijbehorende URL in de sleutelkluis.
* De toepassingscertificaten en de URL's in de sleutelkluis.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor clientverificatie

Azure AD kan organisaties (bekend als tenants) voor het beheren van de gebruikerstoegang tot toepassingen. Toepassingen worden onderverdeeld in die met een webgebaseerde gebruikersinterface voor aanmelding en die een native client-ervaring. In dit artikel gaan we ervan uit dat u al een tenant hebt gemaakt. Als u niet hebt, starten door te lezen [een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

Een Service Fabric-cluster biedt verschillende toegangspunten naar de beheerfunctionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure AD-toepassingen toegang tot het cluster, een webtoepassing en een systeemeigen toepassing.

Om te vereenvoudigen enkele van de stappen voor het configureren van Azure AD met een Service Fabric-cluster, hebben wij een set Windows PowerShell-scripts.

> [!NOTE]
> U moet de volgende stappen voltooien voordat u het cluster maakt. Omdat de scripts verwacht dat de clusternamen van de- en eindpunten, worden de waarden moeten worden gepland en niet de waarden die u al hebt gemaakt.

1. [Download de scripts] [ sf-aad-ps-script-download] op uw computer.
2. Met de rechtermuisknop op het zip-bestand, selecteer **eigenschappen**, selecteer de **blokkering** selectievakje en klik vervolgens op **toepassen**.
3. Pak het gecomprimeerde bestand uit.
4. Voer `SetupApplications.ps1`, en geef de tenant-id, de clusternaam en de WebApplicationReplyUrl als parameters. Bijvoorbeeld:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    U kunt uw TenantId vinden door het uitvoeren van de PowerShell-opdracht `Get-AzureSubscription`. Deze opdracht wordt uitgevoerd, wordt de TenantId voor elk abonnement weergegeven.

    Clusternaam wordt gebruikt voor het voorvoegsel van de Azure AD-toepassingen die zijn gemaakt door het script. Dit hoeft niet de werkelijke clusternaam exact overeenkomen. Dit is uitsluitend bedoeld om het Azure AD-artefacten toewijzen aan het Service Fabric-cluster dat ze worden gebruikt met eenvoudiger.

    WebApplicationReplyUrl is het standaardeindpunt die Azure AD aan uw gebruikers retourneert nadat ze Voltooi de aanmelding. Stel het eindpunt van de Service Fabric Explorer voor uw cluster, dat standaard is dit eindpunt:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    U wordt gevraagd aan te melden bij een account met voor de Azure AD-tenant beheerdersbevoegdheden. Nadat u zich aanmeldt, wordt het script maakt de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de tenant-toepassingen in de [klassieke Azure-portal][azure-classic-portal], ziet u twee nieuwe vermeldingen:

   * *Clusternaam*\_Cluster
   * *Clusternaam*\_Client

   Het script wordt de JSON die wordt vereist door de Azure Resource Manager-sjabloon wanneer u het cluster in de volgende sectie, maken dus is het een goed idee om het PowerShell-venster open houden afgedrukt.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Een Service Fabric-cluster Resource Manager-sjabloon maken
In deze sectie worden de uitvoer van de voorgaande PowerShell-opdrachten in een Service Fabric-cluster Resource Manager-sjabloon gebruikt.

Voorbeeld Resource Manager-sjablonen zijn beschikbaar in de [Azure snel starten-sjablonengalerie op GitHub][azure-quickstart-templates]. Deze sjablonen kunnen worden gebruikt als een beginpunt voor uw cluster-sjabloon.

### <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken
Deze handleiding bevat de [beveiligde 5 knooppunten] [ service-fabric-secure-cluster-5-node-1-nodetype] voorbeeldsjabloon en sjabloonparameters. Download `azuredeploy.json` en `azuredeploy.parameters.json` op uw computer en opent u beide bestanden in uw favoriete teksteditor.

### <a name="add-certificates"></a>Certificaten toevoegen
U kunt certificaten toevoegen aan een cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Het is raadzaam dat u de sleutelkluis waarden in een Resource Manager-sjabloonbestand parameters plaatsen. In dat geval voorkomt de Resource Manager sjabloonbestand herbruikbare en gratis waarden specifieke aan een implementatie.

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Alle certificaten toevoegen aan de virtuele machine scale set osProfile
Elk certificaat dat geïnstalleerd in het cluster moet worden geconfigureerd in de sectie osProfile van de schaal set resource (Microsoft.Compute/virtualMachineScaleSets). Deze actie geeft u een opdracht van de resourceprovider voor het installeren van het certificaat op de virtuele machines. Deze installatie omvat zowel het certificaat van het cluster en toepassing beveiligingscertificaten die u wilt gebruiken voor uw toepassingen:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Het Service Fabric-cluster-certificaat configureren
Het certificaat voor verificatie van cluster moet worden geconfigureerd in zowel de Service Fabric-clusterbron (Microsoft.ServiceFabric/clusters) en de Service Fabric-extensie voor de virtuele-machineschaalset wordt ingesteld in de bron van de virtuele machine scale set. Deze regeling kan de Service Fabric resourceprovider configureren voor gebruik voor verificatie van de cluster en serververificatie voor eindpunten voor beheer.

##### <a name="virtual-machine-scale-set-resource"></a>Virtuele-machineschaalset resource:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service Fabric-resource:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Invoegen van de configuratie van Azure AD
De Azure AD-configuratie die u eerder hebt gemaakt, kan worden ingevoegd rechtstreeks in uw Resource Manager-sjabloon. Echter, raden wij u eerst de waarden in een parameterbestand te houden van de Resource Manager sjabloon herbruikbare en gratis van waarden die specifiek zijn voor een implementatie uitpakken.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < een ' configureren-arm"></a>Sjabloonparameters Resource Manager configureren
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since the link seems not to be redirecting correctly --->
Gebruik tot slot de uitvoerwaarden van de sleutelkluis en de Azure AD PowerShell-opdrachten voor het vullen van het parameterbestand:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Op dit moment u moet beschikken over de volgende elementen:

* Sleutelkluis-resourcegroep
  * Key Vault
  * Certificaat voor serververificatie cluster
  * Gegevens uitwisselen certificaat
* Azure Active Directory-tenant
  * Azure AD-toepassing voor het web gebaseerde beheer en de Service Fabric Explorer
  * Azure AD-toepassing voor beheer van native client
  * Gebruikers en hun rollen
* Service Fabric-cluster Resource Manager-sjabloon
  * Certificaten die zijn geconfigureerd met behulp van de sleutelkluis
  * Azure Active Directory wordt uitgevoerd

Het volgende diagram illustreert waar uw sleutelkluis en de configuratie van Azure AD in uw Resource Manager-sjabloon inpassen.

![Afhankelijkheidskaart van Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Het cluster maken
U bent nu klaar voor het maken van het cluster met behulp van [Azure-resource sjabloonimplementatie][resource-group-template-deploy].

#### <a name="test-it"></a>Testen
Gebruik de volgende PowerShell-opdracht voor het testen van uw Resource Manager-sjabloon met een parameterbestand met:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Implementeren
Als de Resource Manager-sjabloon-test is geslaagd, gebruikt u de volgende PowerShell-opdracht Resource Manager-sjabloon implementeren met een parameterbestand met:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en de beheerder. U kunt de rollen toewijzen met behulp van de [klassieke Azure-portal][azure-classic-portal].

1. In de Azure portal, gaat u naar uw tenant en selecteer vervolgens **toepassingen**.
2. Selecteer de webtoepassing heeft een naam zoals `myTestCluster_Cluster`.
3. Klik op de **gebruikers** tabblad.
4. Selecteer een gebruiker wilt toewijzen en klik op de **toewijzen** knop aan de onderkant van het scherm.

    ![Gebruikers toewijzen aan rollen knop][assign-users-to-roles-button]
5. Selecteer de rol toewijzen aan de gebruiker.

    ![In het dialoogvenster 'Gebruikers toewijzen'][assign-users-to-roles-dialog]

> [!NOTE]
> Zie voor meer informatie over functies in Service Fabric [toegangsbeheer op basis van rollen voor Service Fabric-clients](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused the wrong redirection of the link --->

## <a name="create-secure-clusters-on-linux"></a>Beveiligde clusters onder Linux maken
Zodat u het proces eenvoudiger we hebt opgegeven een [helper script](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Voordat u dit script helper gebruiken, zorg ervoor dat u al Azure-opdrachtregelinterface (CLI) geïnstalleerd en is in het pad. Zorg ervoor dat het script de machtigingen om uit te voeren heeft door het uitvoeren van `chmod +x cert_helper.py` nadat deze is gedownload. De eerste stap is aan te melden bij uw Azure-account met CLI met de `azure login` opdracht. Na het aanmelden bij uw Azure-account, moet u het script helper gebruikt met uw door CA ondertekend certificaat, zoals de volgende opdracht:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

De parameter - ifile kan duren voordat een pfx-bestand of een .pem-bestand als invoer met het certificaattype (pfx of pem of ss als het een zelf-ondertekend certificaat).
De parameter -h uit de help-tekst wordt afgedrukt.


Met deze opdracht retourneert de volgende drie tekenreeksen als uitvoer:

* SourceVaultID die de id voor de nieuwe KeyVault ResourceGroup deze voor u gemaakt
* CertificateUrl voor toegang tot het certificaat
* CertificateThumbprint die wordt gebruikt voor verificatie

Het volgende voorbeeld ziet u hoe u de opdracht:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Uitvoeren van de voorgaande opdracht hebt u de drie tekenreeksen als volgt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

De onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster. Deze overeenkomst is vereist voor een met SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat kan niet verkrijgen van een Certificeringsinstantie voor de `.cloudapp.azure.com` domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Deze onderwerpen worden de items die u maken van een beveiligde Service Fabric-cluster (zonder Azure AD), wilt zoals beschreven op [configureren Resource Manager-Sjabloonparameters](#configure-arm). U kunt verbinding maken met het beveiligde cluster door de instructies voor [verifiëren clienttoegang tot een cluster](service-fabric-connect-to-secure-cluster.md). Linux-clusters bieden geen ondersteuning voor Azure AD-verificatie. U kunt admin en client-rollen toewijzen, zoals beschreven in de [rollen toewijzen aan gebruikers](#assign-roles) sectie. Wanneer u de beheerder en client rollen voor een Linux-cluster opgeven, hebt u certificaatvingerafdrukken om verificatie te voorzien. U specifieke de naam van de certificaathouder geen omdat geen validatie van certificaatketen of certificaatintrekkingslijst wordt uitgevoerd.

Als u een zelfondertekend certificaat gebruiken wilt voor het testen, kunt u hetzelfde script gebruiken voor het genereren van een. U kunt vervolgens het certificaat uploaden naar uw sleutelkluis door de vlag `ss` in plaats van hiervoor de certificaatpad en de naam van het certificaat. Bijvoorbeeld, Zie de volgende opdracht voor het maken en uploaden van een zelfondertekend certificaat:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Met deze opdracht retourneert de dezelfde drie tekenreeksen: SourceVault CertificateUrl en CertificateThumbprint. Vervolgens kunt u in de tekenreeksen beveiligde Linux-cluster en een locatie waar het zelfondertekende certificaat is geplaatst. U moet het zelfondertekende certificaat verbinding maken met het cluster. U kunt verbinding maken met het beveiligde cluster door de instructies voor [verifiëren clienttoegang tot een cluster](service-fabric-connect-to-secure-cluster.md).

De onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster. Deze overeenkomst is vereist voor een met SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat kan niet verkrijgen van een Certificeringsinstantie voor de `.cloudapp.azure.com` domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

U kunt ook opgeven met de parameters van het script helper in de Azure portal, zoals beschreven in de [een cluster maken in de Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) sectie.

## <a name="next-steps"></a>Volgende stappen
U hebt op dit moment een beveiligde cluster met Azure Active Directory verstrekken management-verificatie. Vervolgens [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en meer informatie over hoe [toepassing geheimen beheren](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Problemen met Azure Active Directory in te stellen voor clientverificatie
Als u een probleem ondervindt terwijl u Azure AD voor clientverificatie instelt, raadpleegt u de mogelijke oplossingen in deze sectie.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer vraagt u een certificaat selecteren
#### <a name="problem"></a>Probleem
Nadat u zich aanmeldt met succes naar Azure AD in Service Fabric Explorer, geeft de browser naar de startpagina maar gevraagd of u een certificaat selecteren.

![Dialoogvenster voor SFX certificaat selecteren][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reden
De gebruiker is niet een rol in de Azure AD-toepassing voor cluster toegewezen. Azure AD-verificatie mislukt dus bij Service Fabric-cluster. Service Fabric Explorer valt terug op verificatie via certificaat.

#### <a name="solution"></a>Oplossing
Volg de instructies voor het instellen van Azure AD en gebruikersrollen toewijzen. Ook wordt aangeraden dat u 'Gebruikerstoewijzing vereist voor toegang tot de app,' inschakelt als `SetupApplications.ps1` biedt.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Verbinding met PowerShell mislukt met een fout: "de opgegeven referenties zijn ongeldig."
#### <a name="problem"></a>Probleem
Wanneer u verbinding maken met het cluster met behulp van 'AzureActiveDirectory' beveiligingsmodus, nadat u zich aanmeldt met succes naar Azure AD PowerShell, de verbinding is mislukt met een fout: "de opgegeven referenties zijn ongeldig."

#### <a name="solution"></a>Oplossing
Deze oplossing is hetzelfde als het vorige item.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer een fout retourneert wanneer u zich aanmeldt: 'AADSTS50011'
#### <a name="problem"></a>Probleem
Wanneer u probeert aan te melden bij Azure AD in Service Fabric Explorer, de pagina een fout geretourneerd: ' AADSTS50011: het antwoordadres &lt;url&gt; komt niet overeen met de antwoordadressen geconfigureerd voor de toepassing: &lt;guid&gt;. "

![Antwoordadres SFX komt niet overeen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reden
De cluster (web)-toepassing die staat voor Service Fabric Explorer probeert te verifiëren met Azure AD en als onderdeel van de aanvraag biedt de retour Omleidings-URL. Maar de URL niet wordt vermeld in de Azure AD-toepassing **antwoord-URL** lijst.

#### <a name="solution"></a>Oplossing
Op de **configureren** tabblad toevoegen van de toepassing cluster (web) de URL van het Service Fabric Explorer naar de **antwoord-URL** lijst of vervangen door een van de items in de lijst. Wanneer u klaar bent, sla de wijziging.

![Web application antwoord-url][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Verbinding maken met het cluster met behulp van Azure AD-verificatie via PowerShell
Als u wilt verbinding maken met de Service Fabric-cluster, gebruikt u in het volgende voorbeeld van de PowerShell-opdracht:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Zie voor meer informatie over de cmdlet Connect-ServiceFabricCluster, [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kan ik de dezelfde Azure AD-tenant in meerdere clusters gebruiken?
Ja. Maar vergeet niet de URL van Service Fabric Explorer toevoegen aan uw cluster (web)-toepassing. Service Fabric Explorer anders werkt niet.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Waarom moet ik een servercertificaat terwijl Azure AD is ingeschakeld?
FabricClient en FabricGateway uitvoeren een wederzijdse verificatie. Tijdens de verificatie van Azure AD, Azure AD-integratie zorgt voor een identiteit van de client naar de server en het servercertificaat wordt gebruikt om te controleren of de identiteit van de server. Zie voor meer informatie over Service Fabric-certificaten [X.509-certificaten en Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png


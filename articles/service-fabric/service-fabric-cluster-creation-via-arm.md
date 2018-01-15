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
ms.date: 12/07/2017
ms.author: chackdan
ms.openlocfilehash: e5dd1ebd290c950c7f2bda3dae088f3ee7f836fd
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Maken van een Service Fabric-cluster met behulp van Azure Resource Manager 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Deze stapsgewijze handleiding helpt u bij het instellen van een beveiligde Azure Service Fabric-cluster in Azure met Azure Resource Manager. We erkennen dat het artikel lang is. Niettemin, tenzij u al grondig bekend met de inhoud bent, moet u de stappen zorgvuldig.

De handleiding wordt ingegaan op de volgende procedures:

* Hoofdconcepten die u rekening houden uitschakelen moet voordat u een service fabric-cluster implementeert.
* Maken van een cluster in Azure met behulp van service fabric-Resource Manager-modules.
* Instellen van Azure Active Directory (Azure AD) voor het verifiëren van gebruikers voor het uitvoeren van beheerbewerkingen op het cluster.
* Een aangepaste Azure Resource Manager-sjabloon voor uw cluster ontwerpen en implementeren.

## <a name="key-concepts-to-be-aware-of"></a>Belangrijkste concepten moet weten
In Azure Service fabric vereist die u met een x509 certificaat voor het beveiligen van uw cluster en de eindpunten. Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Voor client access/management bewerkingen uitvoert op het cluster, inclusief implementeren, bijwerken en verwijderen van toepassingen, services en de gegevens die ze bevatten, kunt u certificaten of Azure Active Directory-referenties. Het gebruik van Azure Active Directory wordt ten zeerste aangeraden, omdat dit de enige manier om te voorkomen dat delen van certificaten op de clients.  Zie voor meer informatie over hoe de certificaten worden gebruikt in Service Fabric, [scenario's voor beveiliging van Service Fabric-cluster][service-fabric-cluster-security].

Service Fabric gebruikt x.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven. U gebruikt [Sleutelkluis] [ key-vault-get-started] voor het beheren van certificaten voor Service Fabric-clusters in Azure. 


### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Deze certificaten (een primaire en eventueel een secundaire) zijn vereist voor het beveiligen van een cluster en onbevoegde toegang tot deze voorkomen. Dit biedt een clusterbeveiliging op twee manieren:

* **Cluster-verificatie:** knooppunt naar communicatie voor cluster federation verifieert. Alleen knooppunten die u hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* **Server-verificatie:** verifieert de eindpunten van het cluster-beheer met een management-client, zodat de management-client, weet deze communiceert met het echte cluster en niet een 'man in het midden'. Dit certificaat biedt ook een met SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten. Deze certificaten hebben doorgaans extensies .pfx of .pem  
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, kan worden geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* De **onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster**. Deze overeenkomst is vereist voor een met SSL voor het HTTPS-eindpunt voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de *. cloudapp.azure.com domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>Azure Active Directory instellen voor clientverificatie (optioneel, maar aanbevolen)

Azure AD kan organisaties (bekend als tenants) voor het beheren van de gebruikerstoegang tot toepassingen. Toepassingen worden onderverdeeld in die met een webgebaseerde gebruikersinterface voor aanmelding en die een native client-ervaring. In dit artikel gaan we ervan uit dat u al een tenant hebt gemaakt. Als u niet hebt, starten door te lezen [een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

Een Service Fabric-cluster biedt verschillende toegangspunten naar de beheerfunctionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure AD-toepassingen toegang tot het cluster, een webtoepassing en een systeemeigen toepassing.

Meer informatie over hoe u verderop in dit document instelt.

### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om beveiligingsredenen toepassing. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van configuratiewaarden die van toepassing.
* Codering van gegevens over knooppunten tijdens de replicatie.

Het concept van het maken van beveiligde clusters is hetzelfde, of ze Linux of Windows-clusters. 

### <a name="client-authentication-certificates-optional"></a>Certificaten voor clientverificatie (optioneel)
Een aantal extra certificaten kan worden opgegeven voor de beheerder of gebruiker clientbewerkingen. Het certificaat van de cluster heeft standaard beheerdersbevoegdheden voor de client. Deze extra clientcertificaten mag niet worden geïnstalleerd in het cluster, maar moet worden opgegeven als het wordt toegestaan in de configuratie van het cluster, moeten echter worden geïnstalleerd op de clientcomputers verbinding maken met het cluster en eventuele beheer uitvoeren bewerkingen.


## <a name="prerequisites"></a>Vereisten 
Het concept van het maken van beveiligde clusters is hetzelfde, of ze Linux of Windows-clusters. Deze handleiding bevat informatie over het gebruik van azure powershell of azure CLI om nieuwe clusters te maken. De vereiste onderdelen zijn 

-  [Azure PowerShell 4.1 en hoger] [ azure-powershell] of [Azure CLI 2.0 en hoger][azure-CLI].
-  u kunt meer informatie vinden op de service fabric-modules hier - [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) en [az SF CLI-module](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Service fabric DB-module gebruiken voor het implementeren van een cluster

In dit document, zouden we het service fabric RM powershell gebruiken en CLI-module voor het implementeren van een cluster, de powershell of de opdracht van de module CLI kunt u meerdere scenario's. Laat het ons doorlopen die elk van de. Kies het scenario dat u van mening bent beste dat aan uw behoeften voldoet. 

- Maak een nieuw cluster - zelfondertekend certificaat met behulp van een systeem gegenereerd
    - Een standaardsjabloon voor het cluster
    - Gebruik een sjabloon die u al hebt
- Maak een nieuw cluster - gebruik van een certificaat dat u al bezit
    - Een standaardsjabloon voor het cluster
    - Gebruik een sjabloon die u al hebt

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Maken van nieuwe cluster - zelfondertekend certificaat met behulp van een systeem gegenereerd

Gebruik de volgende opdracht voor het maken van de cluster, hebt u wilt dat het systeem een zelfondertekend certificaat genereren en deze gebruiken voor het beveiligen van uw cluster. Met deze opdracht wordt een primaire cluster-certificaat dat wordt gebruikt voor clusterbeveiliging en voor het instellen van beheerderstoegang om uit te voeren met behulp van dat certificaat beheerbewerkingen ingesteld.

### <a name="login-in-to-azure"></a>aanmelden in bij Azure.

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```
#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Gebruik de standaard 5 knooppunt 1 nodetype sjabloon die wordt geleverd in de module voor het instellen van het cluster

Gebruik de volgende opdracht voor het maken van een cluster snel door te geven van de minimale parameters

De sjabloon die wordt gebruikt, is beschikbaar op de [azure service fabric-sjabloon voorbeelden: windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

De onderstaande opdrachten uit werkt voor het maken van Windows en Linux-clusters, hoeft u te dienovereenkomstig opgeven van het besturingssysteem. De powershell / CLI-opdrachten ook het certificaat in de opgegeven in theCertificateOutputFolder levert. De opdracht wordt in ook andere parameters zoals VM-SKU.

```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!1"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"



az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Gebruik de aangepaste sjabloon die u al hebt 

Als u maken van een aangepaste sjabloon aanpassen aan uw behoeften wilt, is het raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [azure service fabric-sjabloon voorbeelden](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [uw cluster-sjabloon aanpassen] [ customize-your-cluster-template] hieronder.

Als u al een aangepaste sjabloon hebt en zorg ervoor dat zijn aan Controleer alle drie certificaat gerelateerde parameters in de sjabloon en het parameterbestand zijn als volgt benoemde en -waarden null als volgt.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de declare-instructies op de juiste waarden. CLI ondersteunt alle andere parameters die ondersteuning biedt voor de bovenstaande powershell-opdracht.

```CLI

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Maken van nieuwe cluster - met een certificaat dat u hebt gekocht bij een Certificeringsinstantie of u al hebt.

Gebruik de volgende opdracht voor het maken van de cluster, hebt u een certificaat dat u gebruiken wilt voor het beveiligen van uw cluster met.

Als dit een door CA ondertekend certificaat dat u uiteindelijk gebruik voor andere doeleinden dan ook, wordt aanbevolen dat u een afzonderlijke resourcegroep specifiek voor uw sleutelkluis bieden. Het is raadzaam dat u de sleutelkluis in een eigen resourcegroep geplaatst. Deze actie kunt u de brongroepen berekeningen en opslag, met inbegrip van de resourcegroep die uw Service Fabric-cluster bevat zonder verlies van uw sleutels en geheimen verwijderen. **De resourcegroep die de sleutelkluis bevat _moet zich in dezelfde regio_ als het cluster dat wordt gebruikt.**


#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module"></a>Gebruik de standaard 5 knooppunt 1 nodetype sjabloon die wordt geleverd in de module
De sjabloon die wordt gebruikt, is beschikbaar op de [azure-voorbeelden: windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

```Powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!1" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser

```

#### <a name="use-the-custom-template-that-you-have"></a>Gebruik de aangepaste sjabloon die u hebt 
Als u maken van een aangepaste sjabloon aanpassen aan uw behoeften wilt, is het raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [azure service fabric-sjabloon voorbeelden](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [uw cluster-sjabloon aanpassen] [ customize-your-cluster-template] hieronder.

Als u al een aangepaste sjabloon hebt en zorg ervoor dat zijn aan Controleer alle drie certificaat gerelateerde parameters in de sjabloon en het parameterbestand zijn als volgt benoemde en -waarden null als volgt.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword #certPassword

```

Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de declare-instructies op de juiste waarden.

```CLI

declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-keyvault"></a>Een verwijzing naar het geheim die u al hebt geüpload naar de keyvault gebruiken

Gebruik een bestaande sleutelkluis u _moet inschakelen voor de implementatie van_ om toe te staan van de resourceprovider voor compute certificaten van het verkrijgen en installeren op de clusterknooppunten:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment


$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroup -SecretIdentifier $secretID -TemplateFile $templateFile -ParameterFile $templateParmfile 

```
Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de declare-instructies op de juiste waarden.

```cli

declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifieraz $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 

```

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

U wordt gevraagd aan te melden bij een account met voor de Azure AD-tenant beheerdersbevoegdheden. Nadat u zich aanmeldt, wordt het script maakt de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de tenant-toepassingen in de [Azure-portal][azure-portal], ziet u twee nieuwe vermeldingen:

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

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Een Service Fabric-cluster Resource Manager-sjabloon maken
Deze sectie is voor gebruikers die aangepaste willen Service Fabric-cluster Resource Manager-sjabloon schrijft. Zodra u een sjabloon hebt, kunt u nog steeds teruggaan en de powershell of CLI-modules gebruiken om u te implementeren. 

Voorbeeld Resource Manager-sjablonen zijn beschikbaar in de [Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als een beginpunt voor uw cluster-sjabloon.

### <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken
Deze handleiding bevat de [beveiligde 5 knooppunten] [ service-fabric-secure-cluster-5-node-1-nodetype] voorbeeldsjabloon en sjabloonparameters. Download `azuredeploy.json` en `azuredeploy.parameters.json` op uw computer en opent u beide bestanden in uw favoriete teksteditor.

### <a name="add-certificates"></a>Certificaten toevoegen
U kunt certificaten toevoegen aan een cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Toevoegen van die sleutelkluis parameters en waarden in een parameterbestand van de Resource Manager-sjabloon (azuredeploy.parameters.json). 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Alle certificaten toevoegen aan de virtuele machine scale set osProfile
Elk certificaat dat geïnstalleerd in het cluster moet worden geconfigureerd in de sectie osProfile van de schaal set resource (Microsoft.Compute/virtualMachineScaleSets). Deze actie geeft u een opdracht van de resourceprovider voor het installeren van het certificaat op de virtuele machines. Deze installatie omvat zowel het certificaat van het cluster en toepassing beveiligingscertificaten die u wilt gebruiken voor uw toepassingen:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
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

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Voeg dat de certificaatgegevens van de virtuele-machineschaalset resource ingesteld:
```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
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

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Gegevens van het certificaat toevoegen aan de Service Fabric-cluster-bron:
```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
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

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD-configuratie voor het gebruik van Azure AD voor clienttoegang toevoegen

U kunt de configuratie van Azure AD s toevoegen aan cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Die Azure AD-parameters en waarden in een parameterbestand (azuredeploy.parameters.json) van een Resource Manager-sjabloon toevoegen.

```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
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

### <a name="populate-the-parameter-file-with-the-values"></a>De parameter-bestand met de waarden vullen.
Gebruik tot slot de uitvoerwaarden van de sleutelkluis en de Azure AD powershell-opdrachten voor het vullen van het parameterbestand:

Als u de Azure service fabric RM powershell-modules gebruiken wilt, klikt u vervolgens hoeft niet u vult de certificaatgegevens van het cluster, als u het systeem voor het genereren van de zelf ondertekend certificaat voor de clusterbeveiliging, net als null houden. 

> [!NOTE]
> Voor de RM-modules kunnen worden opgepikt en deze leeg parameterwaarden vult de namen van parameters veel overeenkomen met de namen hieronder
>

```json
        "clusterCertificateThumbprint": {
            "value": ""
        },
        "clusterCertificateUrlValue": {
            "value": ""
        },
        "sourceVaultvalue": {
            "value": ""
        },
```

Als u met behulp van certificaten van de toepassing of een bestaand cluster dat u hebt geüpload naar de keyvault gebruikt, moet u deze gegevens ophalen en deze vullen 

De RM-modules hebt de mogelijkheid om te geneate niet voor u de configuratie van de Azure AD. dus als u de Azure AD gebruiken voor toegang van clients wilt, u deze vullen moet.

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

### <a name="test-your-template"></a>Testen van uw sjabloon  
Gebruik de volgende PowerShell-opdracht voor het testen van uw Resource Manager-sjabloon met een parameterbestand:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Als u problemen ondervindt en cryptisch berichten ophalen, gebruikt u '-fouten opsporen in ' als een optie.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Het volgende diagram illustreert waar uw sleutelkluis en de configuratie van Azure AD in uw Resource Manager-sjabloon inpassen.

![Afhankelijkheidskaart van Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-the-cluster-using-azure-resource-template"></a>Het cluster met behulp van Azure-resource-sjabloon maken 

U kunt nu implementeren u cluster met behulp van de stappen die eerder in het document wordt beschreven of hebt u de waarden in het parameterbestand ingevuld, klikt u bent nu klaar voor het maken van het cluster met behulp van [Azure-resource sjabloonimplementatie] [ resource-group-template-deploy] rechtstreeks.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Als u problemen ondervindt en cryptisch berichten ophalen, gebruikt u '-fouten opsporen in ' als een optie.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en de beheerder. U kunt de rollen toewijzen met behulp van de [Azure-portal][azure-portal].

1. Selecteer in de Azure-portal uw tenant op de rechterbovenhoek.

    ![Selecteer de knop tenant][select-tenant-button]
2. Selecteer **Azure Active Directory** op het linkertabblad en selecteer 'bedrijfstoepassingen'.
3. Selecteer 'Alle toepassingen', en zoeken en selecteer de webtoepassing heeft een naam zoals `myTestCluster_Cluster`.
4. Klik op de **gebruikers en groepen** tabblad.

    ![Tabblad gebruikers en groepen][users-and-groups-tab]
5. Klik op de **gebruiker toevoegen** knop op de nieuwe pagina, selecteert u een gebruiker en de rol toewijzen en klik vervolgens op de **Selecteer** knop aan de onderkant van de pagina.

    ![Gebruikers toewijzen aan de pagina serverfuncties][assign-users-to-roles-page]
6. Klik op de **toewijzen** knop aan de onderkant van de pagina.

    ![Bevestiging van de toewijzing toevoegen][assign-users-to-roles-confirm]

> [!NOTE]
> Zie voor meer informatie over functies in Service Fabric [toegangsbeheer op basis van rollen voor Service Fabric-clients](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Probleemoplossing bij het instellen van Azure Active Directory
Azure AD in te stellen en dit gebruikt, kan lastig zijn, zodat u hier volgen een aantal tips op wat u doen kunt om het probleem voor foutopsporing.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer vraagt u een certificaat selecteren
#### <a name="problem"></a>Probleem
Nadat u zich aanmeldt met succes naar Azure AD in Service Fabric Explorer, geeft de browser naar de startpagina maar gevraagd of u een certificaat selecteren.

![Dialoogvenster SFX-certificaat][sfx-select-certificate-dialog]

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
Selecteer 'App registraties' in AAD pagina, selecteer uw cluster-toepassing en selecteer vervolgens de **antwoord-URL's** knop. De URL van Service Fabric Explorer toevoegen aan de lijst op de pagina 'Antwoord-URL's ', of vervangen door een van de items in de lijst. Wanneer u klaar bent, sla de wijziging.

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

## <a name="next-steps"></a>Volgende stappen
U hebt op dit moment een beveiligde cluster met Azure Active Directory verstrekken management-verificatie. Vervolgens [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en meer informatie over hoe [toepassing geheimen beheren](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric
[customize-your-cluster-template]: service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[select-tenant-button]: ./media/service-fabric-cluster-creation-via-arm/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-via-arm/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png


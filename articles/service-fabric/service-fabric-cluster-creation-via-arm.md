---
title: Een Azure Service Fabric-cluster maken vanaf een sjabloon | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een beveiligd Service Fabric-cluster in Azure instellen met behulp van Azure Resource Manager, Azure Key Vault en Azure Active Directory (Azure AD) voor clientauthenticatie.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: aljo
ms.openlocfilehash: cad98954c89c37e57d44abf2af54e903a1b4a740
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504920"
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Een Service Fabric-cluster maken met behulp van Azure Resource Manager 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Deze stapsgewijze handleiding helpt u bij het instellen van een beveiligd Azure Service Fabric-cluster in Azure met behulp van Azure Resource Manager. We erkennen dat het artikel lang is. Echter, tenzij u al grondig bekend met de inhoud bent, moet u zorgvuldig elke stap uitvoeren.

De handleiding wordt ingegaan op de volgende procedures:

* Belangrijkste concepten die u moet rekening mee moet houden voordat u een Service Fabric-cluster implementeert.
* Het maken van een cluster in Azure met behulp van Service Fabric-Resource Manager-modules.
* Instellen van Azure Active Directory (Azure AD) voor het verifiëren van gebruikers voor het uitvoeren van beheerbewerkingen op het cluster.
* Een aangepaste Azure Resource Manager-sjabloon voor uw cluster ontwerpen en implementeren.

## <a name="key-concepts-to-be-aware-of"></a>Belangrijkste concepten rekening mee moet houden
Service Fabric mandaten in Azure, die u gebruikt een x509 certificaat voor het beveiligen van uw cluster en de eindpunten. Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Voor de client toegang/uitvoeren van beheerbewerkingen op het cluster, inclusief implementeren, bijwerken en verwijderen van toepassingen, services en de gegevens die ze bevatten, kunt u certificaten of Azure Active Directory-referenties. Het gebruik van Azure Active Directory wordt ten zeerste aangeraden, omdat dat de enige manier om te voorkomen dat delen van certificaten op de clients.  Zie voor meer informatie over het gebruik van certificaten in Service Fabric [Service Fabric-clusterbeveiligingsscenario's][service-fabric-cluster-security].

Service Fabric maakt gebruik van X.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven. U gebruikt [Key Vault] [ key-vault-get-started] voor het beheren van certificaten voor Service Fabric-clusters in Azure. 


### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Deze certificaten (één primair knooppunt en eventueel een secundaire) zijn vereist voor het beveiligen van een cluster en voorkomt ongeoorloofde toegang tot deze. Het biedt beveiliging van clusters op twee manieren:

* **Cluster-verificatie:** knooppunt-naar-knooppunt communicatie voor cluster federation verifieert. Alleen de knooppunten die hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* **Server-verificatie:** verifieert de eindpunten voor clusterbeheer aan een management-client, zodat de management-client weet dat deze communiceert met de echte cluster en niet een 'man in the middle'. Dit certificaat biedt ook een SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten. Deze certificaten hebben doorgaans extensies .pfx of .pem  
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange (PFX)-bestand.
* De **onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster**. Deze overeenkomst is vereist om te voorzien van een SSL voor de HTTPS-eindpunt voor beheer van het cluster en de Service Fabric Explorer. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de *. cloudapp.azure.com domein. U hebt voor uw cluster een aangepaste domeinnaam nodig. Wanneer u een certificaat van een CA aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>Azure Active Directory instellen voor clientverificatie (optioneel, maar aanbevolen)

Azure AD kan organisaties (bekend als tenants) voor het beheren van toegang tot toepassingen. Toepassingen worden onderverdeeld in die een webgebaseerde UI voor aanmelden en die met een systeemeigen client-ervaring. In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u niet hebt gedaan, starten door te lezen [hoe u een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure AD-toepassingen voor het beheren van toegang tot het cluster, een webtoepassing en een systeemeigen toepassing.

Meer informatie over hoe u verderop in dit document instelt.

### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster voor de toepassing uit veiligheidsoverwegingen. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van waarden van de configuratie van toepassing.
* Versleuteling van gegevens tussen knooppunten tijdens de replicatie.

Het concept van het maken van beveiligde clusters is hetzelfde, ongeacht of ze Linux zijn of Windows-clusters. 

### <a name="client-authentication-certificates-optional"></a>Certificaten voor clientverificatie (optioneel)
Een willekeurig aantal extra certificaten kan worden opgegeven voor de beheerder of gebruiker clientbewerkingen. Het clustercertificaat heeft standaard beheerdersbevoegdheden voor de client. Deze aanvullende clientcertificaten mag niet worden geïnstalleerd in het cluster, maar moet worden opgegeven als wordt toegestaan in de configuratie van het cluster, maar ze nodig hebben om te worden geïnstalleerd op de clientcomputers verbinding maken met het cluster en alle beheertaken uitvoeren bewerkingen.


## <a name="prerequisites"></a>Vereiste onderdelen 
Het concept van het maken van beveiligde clusters is hetzelfde, ongeacht of ze Linux zijn of Windows-clusters. Deze handleiding bevat informatie over het gebruik van Azure PowerShell of Azure CLI om nieuwe clusters te maken. De vereisten zijn:

-  [Azure PowerShell 4.1 en hoger] [ azure-powershell] of [Azure CLI 2.0 en hoger][azure-CLI].
-  u kunt meer informatie vinden op de Service Fabric-modules hier: [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) en [az SF CLI-module](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Service Fabric-DB-module gebruiken om een cluster te implementeren

In dit document, zullen worden gebruikt voor de Service Fabric-RM-powershell en CLI-module voor het implementeren van een cluster, PowerShell of de CLI-opdracht-module kunt u meerdere scenario's. Laat het ons doorlopen voor elk van deze. Kies het scenario dat u beste denkt voldoet aan uw behoeften. 

- Maak een nieuw cluster 
    - zelfondertekend certificaat met behulp van een systeem gegenereerd
    - met een certificaat dat u al eigenaar bent

U kunt een standaardsjabloon voor het cluster of een sjabloon die u al gebruiken

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Nieuw cluster maken - zelfondertekend certificaat met behulp van een systeem gegenereerd

Gebruik de volgende opdracht cluster maken als u wilt dat het systeem een zelfondertekend certificaat genereren en deze gebruiken voor het beveiligen van uw cluster. Met deze opdracht stelt u een primaire cluster-certificaat dat wordt gebruikt voor clusterbeveiliging en het instellen van beheerderstoegang management-bewerkingen met behulp van dat certificaat uit te voeren.

### <a name="login-to-azure"></a>Meld u aan bij Azure

```PowerShell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
```

```CLI
azure login
az account set --subscription $subscriptionId
```
#### <a name="use-the-default-5-node-1-node-type-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Gebruik de standaard 5 knooppunt 1 knooppunt type sjabloon die wordt geleverd in de module voor het instellen van het cluster

Gebruik de volgende opdracht voor het maken van een cluster snel, door de minimale parameters op te geven

De sjabloon die wordt gebruikt, is beschikbaar op de [voorbeeldsjablonen van Azure Service Fabric: windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

De opdrachten onder geschikt is voor het maken van Windows en Linux-clusters, u hoeft op te geven van het besturingssysteem dienovereenkomstig. De PowerShell/CLI-opdrachten wordt ook het certificaat in de opgegeven CertificateOutputFolder; echter Zorg ervoor dat certificatenmap al hebt gemaakt. De opdracht wordt ook andere parameters, zoals VM-SKU.

> [!NOTE]
> De onderstaande Powershell opdracht werkt alleen met Azure Resource Manager PowerShell versie 6.1 >. Om te controleren of de huidige versie van Azure Resource Manager PowerShell-versie, voer de volgende PowerShell-opdracht 'Get-Module AzureRM'. Volg deze koppeling voor het upgraden van uw Azure Resource Manager PowerShell-versie. https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.3.0
>
>
```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password123!@#" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password4321!@#" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser
```

```CLI
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!4321"
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

Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [aanpassen van de clustersjabloon] [ customize-your-cluster-template] onderstaande sectie.

Als u al een aangepaste sjabloon hebt, zorg ervoor dat zijn dubbele-Controleer of alle drie certificaat gerelateerde parameters in de sjabloon en de parameter-bestand zijn met de naam als volgt en waarden null als volgt.

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


```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de instructies declare in de juiste waarden. CLI biedt ondersteuning voor alle andere parameters die ondersteuning biedt voor de bovenstaande PowerShell-opdracht.

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


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Nieuw cluster maken - met een certificaat dat u hebt gekocht van een CA of u al hebt

Gebruik de volgende opdracht om te maken van de cluster, hebt u een certificaat dat u gebruiken wilt voor het beveiligen van uw cluster met.

Als dit een CA ondertekend certificaat dat u uiteindelijk wordt gebruik voor andere doeleinden dan ook, klikt u vervolgens het verdient aanbeveling dat u een afzonderlijke resourcegroep specifiek voor uw key vault bieden. Het is raadzaam dat u de key vault in een eigen resourcegroep geplaatst. Deze actie kunt u de reken- en -resourcegroepen op, met inbegrip van de resourcegroep waarin uw Service Fabric-cluster, zonder verlies van uw sleutels en geheimen verwijderen. **De resourcegroep waarin uw key vault _moet zich in dezelfde regio_ als het cluster dat wordt gebruikt.**


#### <a name="use-the-default-5-node-1-node-type-template-that-ships-in-the-module"></a>Gebruik de standaard 5 knooppunt 1 knooppunt type sjabloon die wordt geleverd in de module
De sjabloon die wordt gebruikt, is beschikbaar op de [voorbeelden van Azure: Windows-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) en [Ubuntu-sjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 
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

#### <a name="use-the-custom-template-that-you-have"></a>De aangepaste sjabloon die u hebt gebruikt 
Als u maken van een aangepaste sjabloon wilt voor uw behoeften, het is raadzaam dat u begint met een van de sjablonen die beschikbaar zijn op de [voorbeeldsjablonen van Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Volg de richtlijnen en uitleg [aanpassen van de clustersjabloon] [ customize-your-cluster-template] onderstaande sectie.

Als u al een aangepaste sjabloon hebt, zorg ervoor dat zijn dubbele-Controleer of alle drie certificaat gerelateerde parameters in de sjabloon en de parameter-bestand zijn met de naam als volgt en waarden null als volgt.

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


```PowerShell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword
```

Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de instructies declare in de juiste waarden.

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

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-key-vault"></a>Een verwijzing naar de geheime sleutel die u al hebt geüpload naar de key vault gebruiken

Gebruik een bestaande sleutelkluis u _moet inschakelen voor de implementatie van_ om toe te staan van de compute resourceprovider certificaten van het verkrijgen en installeren op clusterknooppunten:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -SecretIdentifier $secretId -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```
Hier volgt de equivalente CLI-opdracht hetzelfde te doen. Wijzig de waarden in de instructies declare in de juiste waarden.

```CLI
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier az $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor clientverificatie

Azure AD kan organisaties (bekend als tenants) voor het beheren van toegang tot toepassingen. Toepassingen worden onderverdeeld in die een webgebaseerde UI voor aanmelden en die met een systeemeigen client-ervaring. In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u niet hebt gedaan, starten door te lezen [hoe u een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure AD-toepassingen voor het beheren van toegang tot het cluster, een webtoepassing en een systeemeigen toepassing.

Ter vereenvoudiging van enkele van de stappen voor het configureren van Azure AD met een Service Fabric-cluster, hebben we een set Windows PowerShell-scripts gemaakt.

> [!NOTE]
> U moet de volgende stappen uitvoeren voordat u het cluster maakt. Omdat de scripts verwacht clusternamen en eindpunten, worden de waarden moeten worden gepland en niet de waarden die u al hebt gemaakt.

1. [De scripts downloaden] [ sf-aad-ps-script-download] op uw computer.
2. Met de rechtermuisknop op het zip-bestand, selecteer **eigenschappen**, selecteer de **opheffen van blokkeringen** selectievakje en klik vervolgens op **toepassen**.
3. Pak het gecomprimeerde bestand uit.
4. Voer `SetupApplications.ps1`, en geef de tenant-id, de clusternaam en de WebApplicationReplyUrl als parameters. Bijvoorbeeld:

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
```

U kunt uw tenant-id vinden door het uitvoeren van de PowerShell-opdracht `Get-AzureSubscription`. Deze opdracht wordt uitgevoerd, wordt de tenant-id voor elk abonnement weergegeven.

Clusternaam is gebruikt voor het toevoegen van de Azure AD-toepassingen die zijn gemaakt door het script. Het hoeft niet precies overeenkomen met de naam van het daadwerkelijke cluster. Het is uitsluitend bedoeld om het Azure AD-artefacten worden toegewezen aan de Service Fabric-cluster dat ze worden gebruikt met eenvoudiger.

WebApplicationReplyUrl is de standaardeindpunt dat Azure AD aan uw gebruikers retourneert nadat ze klaar zijn aangemeld. Dit eindpunt als de Service Fabric Explorer-eindpunt voor uw cluster, dit is standaard ingesteld:

https://&lt;cluster_domain&gt;: 19080/Explorer

U wordt gevraagd of u zich aanmeldt bij een account dat beheerdersrechten voor de Azure AD-tenant heeft. Nadat u zich hebt aangemeld, wordt het script maakt de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de toepassingen van de tenant in de [Azure-portal][azure-portal], ziet u twee nieuwe vermeldingen:

   * *Clusternaam*\_Cluster
   * *Clusternaam*\_Client

Het script af te drukken de JSON die wordt vereist door de Azure Resource Manager-sjabloon wanneer u het cluster in de volgende sectie, maakt dus is het een goed idee om Houd het PowerShell-venster geopend.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Een Service Fabric-cluster resource manager-sjabloon maken
In deze sectie is voor gebruikers die tot aangepaste ontwerpen van een Service Fabric-cluster resource manager-sjabloon. Zodra u een sjabloon hebt, kunt u nog steeds teruggaan en de modules PowerShell of CLI gebruiken om het te implementeren. 

Voorbeeld van Resource Manager-sjablonen zijn beschikbaar in de [Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als uitgangspunt voor de clustersjabloon voor het.

### <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken
Maakt gebruik van deze handleiding de [5-knooppunten beveiligd cluster] [ service-fabric-secure-cluster-5-node-1-nodetype] van de voorbeeldsjabloon en sjabloonparameters. Download `azuredeploy.json` en `azuredeploy.parameters.json` op uw computer en opent u beide bestanden in uw favoriete teksteditor.

### <a name="add-certificates"></a>Certificaten toevoegen
U kunt certificaten toevoegen aan een cluster resource manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels bevat. Deze sleutelkluis parameters en waarden toevoegen in een Resource Manager bestand met sjabloonparameters (azuredeploy.parameters.json). 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Alle certificaten toevoegen aan de virtuele machine scale set osProfile
Elk certificaat dat geïnstalleerd in het cluster moet worden geconfigureerd in de sectie osProfile van de scale set resource (Microsoft.Compute/virtualMachineScaleSets). Deze actie geeft de resourceprovider voor het installeren van het certificaat op de virtuele machines. Deze installatie bevat zowel het clustercertificaat en application security certificaten die u wilt gebruiken voor uw toepassingen:

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
Het verificatiecertificaat van het cluster moet worden geconfigureerd in zowel de Service Fabric-clusterresource (Microsoft.ServiceFabric/clusters) en de Service Fabric-extensie voor virtuele-machineschaalset wordt ingesteld in de virtual machine scale set-resource. Deze benadering kunt de Service Fabric-resourceprovider te configureren voor gebruik voor cluster-verificatie en -serververificatie voor eindpunten voor beheer.

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Voegt dat de gegevens van het certificaat de virtuele-machineschaalset set resource:
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
                  "commonNames": ["[parameters('certificateCommonName')]"],
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

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Informatie over het certificaat aan de Service Fabric-cluster-bron toevoegen:
```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD-configuratie voor het gebruik van Azure AD voor clienttoegang toevoegen

U kunt de Azure AD-configuratie toevoegen aan cluster Resource Manager-sjabloon door te verwijzen naar de sleutelkluis waarin de certificaatsleutels. Deze Azure AD-parameters en waarden toevoegen in een Resource Manager bestand met sjabloonparameters (azuredeploy.parameters.json).

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
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

### <a name="populate-the-parameter-file-with-the-values"></a>Vul in het parameterbestand met de waarden
Gebruik tot slot de uitvoerwaarden van de van de key vault en Azure AD PowerShell-opdrachten voor het vullen van het parameterbestand.

Als u van plan bent de Azure service fabric DB PowerShell-modules gebruiken, klikt u vervolgens hoeft niet u voor het vullen van de gegevens van het cluster. Als u wilt dat het systeem voor het genereren van de eigen ondertekend certificaat voor clusterbeveiliging, houd ze net als null. 

> [!NOTE]
> Voor de RM-modules te halen en deze leeg parameterwaarden te vullen, de namen van parameters veel overeenkomen met de namen hieronder

```json
"clusterCertificateThumbprint": {
    "value": ""
},
"certificateCommonName": {
    "value": ""
},
"clusterCertificateUrlValue": {
    "value": ""
},
"sourceVaultvalue": {
    "value": ""
},
```

Als u van certificaten van de toepassing gebruikmaakt of een bestaand cluster dat u hebt geüpload naar de key vault gebruikt, moet u deze gegevens te halen en deze vullen.

De RM-modules hebt niet de mogelijkheid voor het genereren van de configuratie van de Azure AD bij u past, dus als u van plan bent de Azure AD gebruiken voor toegang van clients, moet u deze vullen.

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

### <a name="test-your-template"></a>De sjabloon voor het testen  
Gebruik de volgende PowerShell-opdracht voor het testen van uw Resource Manager-sjabloon met een parameterbestand:

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Als u problemen ondervindt en cryptisch berichten ophalen, gebruikt u '-fouten opsporen in ' als een optie.

```PowerShell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Het volgende diagram illustreert waar uw key vault en Azure AD-configuratie in de Resource Manager-sjabloon aanpassen.

![Kaart van de Resource Manager-afhankelijkheden][cluster-security-arm-dependency-map]


## <a name="encrypting-the-disks-attached-to-your-windows-cluster-nodevirtual-machine-instances"></a>Versleutelen van de schijven die zijn gekoppeld aan uw windows cluster-knooppunten/virtuele machine-instanties

Voor het versleutelen van de schijven (station van het besturingssysteem en andere beheerde schijven) die zijn gekoppeld aan uw knooppunten, maken we gebruik van de Azure Disk Encryption. Azure Disk Encryption is een nieuwe functie waarmee u [versleutelen van uw Windows-virtuele-machineschijven](service-fabric-enable-azure-disk-encryption-windows.md). Azure Disk Encryption maakt gebruik van de industrienorm [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows om volumeversleuteling voor het volume met het besturingssysteem te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw key vault-abonnement. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag. 

## <a name="encrypting-the-disks-attached-to-your-linux-cluster-nodevirtual-machine-instances"></a>Versleutelen van de schijven die zijn gekoppeld aan uw Linux-cluster knooppunten/virtuele machine-instanties

Voor het versleutelen van de schijven (schijf en andere beheerde schijven) die zijn gekoppeld aan uw knooppunten, maken we gebruik van de Azure Disk Encryption. Azure Disk Encryption is een nieuwe functie waarmee u [versleutelen van uw Linux virtuele-machineschijven](service-fabric-enable-azure-disk-encryption-linux.md). Azure Disk Encryption maakt gebruik van de industrienorm [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor de gegevensschijven te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw key vault-abonnement. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag. 


## <a name="create-the-cluster-using-azure-resource-template"></a>Het cluster met behulp van Azure resource-sjabloon maken 

U kunt nu u-cluster implementeren met behulp van de stappen eerder in het document of hebt u de waarden in het parameterbestand ingevuld, klikt u vervolgens u bent nu klaar om te maken van het cluster met behulp van [Azure-resource-sjabloonimplementatie] [ resource-group-template-deploy] rechtstreeks.

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Als u problemen ondervindt en cryptisch berichten ophalen, gebruikt u '-fouten opsporen in ' als een optie.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Gebruikers aan rollen toewijzen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en -beheerder. U kunt de rollen toewijzen met behulp van de [Azure-portal][azure-portal].

1. In de Azure-portal, selecteert u uw tenant in de rechterbovenhoek.

    ![Selecteer de knop tenant][select-tenant-button]
2. Selecteer **Azure Active Directory** op de tab linksboven en selecteer 'Enterprise Application'.
3. 'Alle Application' en selecteer vervolgens zoeken en selecteer de web-App heeft een naam, zoals `myTestCluster_Cluster`.
4. Klik op de **gebruikers en groepen** tabblad.

    ![Tabblad gebruikers en groepen][users-and-groups-tab]
5. Klik op de **gebruiker toevoegen** knop op de nieuwe pagina, selecteert u een gebruiker en de rol wilt toewijzen en klik op de **Selecteer** knop aan de onderkant van de pagina.

    ![Gebruikers toewijzen aan de pagina serverfuncties][assign-users-to-roles-page]
6. Klik op de **toewijzen** knop aan de onderkant van de pagina.

    ![Bevestiging van toewijzing toevoegen][assign-users-to-roles-confirm]

> [!NOTE]
> Zie voor meer informatie over functies in Service Fabric, [rollen gebaseerd toegangsbeheer voor Service Fabric-clients](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Problemen bij het instellen van Azure Active Directory
Instellen van Azure AD en het gebruik van het kunnen lastig zijn, zodat hier een aantal tips zijn over wat u doen kunt om op te sporen van het probleem.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer vraagt u om een certificaat te selecteren
#### <a name="problem"></a>Probleem
Nadat u zich hebt aangemeld met succes aan Azure AD in Service Fabric Explorer, geeft de browser naar de startpagina, maar een bericht u vraagt om een certificaat selecteren.

![Dialoogvenster SFX-certificaat][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reden
De gebruiker is niet een rol in de Azure AD-toepassing voor cluster toegewezen. Azure AD-verificatie mislukt dus op Service Fabric-cluster. Service Fabric Explorer gebruikgemaakt van verificatie via certificaat.

#### <a name="solution"></a>Oplossing
Volg de instructies voor het instellen van Azure AD en gebruikersrollen toewijzen. Ook aangeraden u inschakelen 'Gebruikerstoewijzing vereist voor toegang tot app,' als `SetupApplications.ps1` heeft.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Verbinding met PowerShell mislukt met fout: "de opgegeven referenties zijn ongeldig."
#### <a name="problem"></a>Probleem
Als u PowerShell gebruiken om verbinding met het cluster met behulp van de beveiligingsmodus "AzureActiveDirectory", nadat u zich hebt aangemeld met succes aan Azure AD, mislukt de verbinding met een fout: "de opgegeven referenties zijn ongeldig."

#### <a name="solution"></a>Oplossing
Deze oplossing is hetzelfde als de vorige waarde.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer retourneert een fout opgetreden tijdens het aanmelden: "AADSTS50011"
#### <a name="problem"></a>Probleem
Wanneer u zich aanmeldt bij Azure AD in Service Fabric Explorer, de pagina een fout geretourneerd: ' AADSTS50011: het antwoordadres op dat &lt;url&gt; komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing: &lt;guid&gt;. "

![Antwoordadres op dat u SFX komt niet overeen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reden
Het (web)-clustertoepassing met Service Fabric Explorer probeert te verifiëren bij Azure AD en biedt als onderdeel van de aanvraag de geretourneerde Omleidings-URL. Maar de URL is niet opgenomen in de Azure AD-toepassing **antwoord-URL** lijst.

#### <a name="solution"></a>Oplossing
Selecteer 'App-registraties' in AAD-pagina, selecteert u uw toepassing voor het cluster en selecteer vervolgens de **antwoord-URL's** knop. Op de pagina 'Antwoord-URL's ', de URL van de Service Fabric Explorer toevoegen aan de lijst of een van de items in de lijst met vervangen. Wanneer u klaar bent, sla de wijziging.

![Web application antwoord-url][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Verbinding maken met het cluster met behulp van Azure AD-verificatie via PowerShell
Als u wilt verbinding maken met de Service Fabric-cluster, gebruikt u het volgende voorbeeld van de PowerShell-opdracht:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Zie voor meer informatie over de cmdlet Connect-ServiceFabricCluster, [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kan ik dezelfde Azure AD-tenant in meerdere clusters gebruiken?
Ja. Maar vergeet niet de URL van de Service Fabric Explorer toevoegen aan uw (web)-clustertoepassing. Anders, Service Fabric Explorer werkt niet.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Waarom kan ik nog steeds moet een certificaat voor Azure AD is ingeschakeld?
FabricClient en FabricGateway uit te voeren een wederzijdse verificatie. Tijdens de verificatie van Azure AD, Azure AD-integratie zorgt voor de identiteit van een client naar de server en het servercertificaat wordt gebruikt om te controleren of de identiteit van de server. Zie voor meer informatie over Service Fabric certificaten [X.509-certificaten en Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligd cluster met Azure Active Directory bieden management-verificatie. Volgende [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en leer hoe u [toepassingsgeheimen beheren](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
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


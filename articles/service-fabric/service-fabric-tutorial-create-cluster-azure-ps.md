---
title: Maken van een Service Fabric-cluster in Azure | Microsoft Docs
description: Meer informatie over het maken van een Windows- of Linux Service Fabric-cluster in Azure met behulp van PowerShell
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/03/2017
ms.author: cristyg
ms.custom: mvc
ms.openlocfilehash: 8b744f6b33a04a707c56e6fc8a4638118170b668
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-secure-cluster-in-azure-by-using-powershell"></a>Maken van een beveiligde cluster in Azure met behulp van PowerShell
In dit artikel is de eerste in een reeks zelfstudies die laten u hoe u een .NET-toepassing naar de cloud verplaatst zien met behulp van Azure Service Fabric-clusters en containers. Informatie over het maken van een Service Fabric-cluster (Windows of Linux) die in Azure wordt uitgevoerd in de volgende stappen. Wanneer u klaar bent, hebt u een beveiligde cluster dat wordt uitgevoerd in de cloud waarmee u toepassingen kunt implementeren.

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installeer de [Service Fabric SDK](service-fabric-get-started.md).
- Installeer [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). (Indien nodig, [Azure PowerShell installeren](/powershell/azure/overview) of [bijwerken naar een nieuwere versie](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0#update-azps).)


## <a name="create-a-service-fabric-cluster"></a>Een Service Fabric-cluster maken

Dit script één knooppunt maakt, wordt een voorbeeld bekijken van Service Fabric cluster. Een zelfondertekend certificaat beveiligt u het cluster. Het script wordt gemaakt van het certificaat samen met het cluster en vervolgens wordt het certificaat in een sleutelkluis. Clusters met één knooppunt dan één virtuele machine kan niet worden geschaald en u preview clusters niet upgraden naar nieuwere versies.

Voor het berekenen van de kosten die zijn gemaakt door een Service Fabric-cluster in Azure wordt uitgevoerd, gebruikt u de [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/).
Zie voor meer informatie over het maken van Service Fabric-clusters [Service Fabric-cluster maken met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Open een PowerShell-console, aanmelden bij Azure en selecteer het abonnement dat u wilt implementeren van het cluster in:

   ```PowerShell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

## <a name="cluster-parameters"></a>Clusterparameters

   Dit script maakt gebruik van de volgende parameters en -concepten. De parameters aan uw behoeften aanpassen.

   | Parameter       | Beschrijving | Voorgestelde waarde |
   | --------------- | ----------- | --------------- |
   | Locatie | De Azure-regio waar u het cluster hebt geïmplementeerd. | Bijvoorbeeld: *westeurope*, *Oost-Aziatische*, of *eastus* |
   | Naam     | De naam van het cluster dat u wilt maken. De naam moet uit 4-23 tekens bestaan en kan alleen kleine letters, cijfers en afbreekstreepjes hebben. | Bijvoorbeeld: *bobs sfpreviewcluster* |
   | resourceGroupName   | De naam van de resourcegroep waarin u wilt maken van het cluster. | Bijvoorbeeld: *myresourcegroup* |
   | VmSku  | De virtuele machine SKU moet worden gebruikt voor de knooppunten. | Een geldige SKU voor virtuele machine |
   | OS  | De virtuele machine OS moet worden gebruikt voor de knooppunten. | Een geldige virtuele machine OS |
   | keyVaultName | De naam van de nieuwe sleutelkluis wilt koppelen aan het cluster. | Bijvoorbeeld: *mykeyvault* |
   | ClusterSize | Het aantal virtuele machines in uw cluster (kan *1* of *3-99*).| Geef slechts één virtuele machine voor een preview-cluster |
   | CertificateSubjectName | De onderwerpnaam van het certificaat moet worden gemaakt. | De indeling heeft:  *<name>* . *<location>* . cloudapp.azure.com |

### <a name="default-parameter-values"></a>Standaardwaarden voor parameters
**Virtuele Machine**: optionele instellingen. Als u ze niet opgeeft, de gebruikersnaam van de beheerder standaard *vmadmin* en PowerShell wordt u gevraagd om een wachtwoord voor de virtuele machine voordat het cluster wordt gemaakt.

**Poorten**: standaard ingesteld op de poorten 80 en 8081. U kunt extra poorten opgeven door de richtlijnen voor [poorten in Service Fabric-clusters](https://docs.microsoft.com/en-us/azure/service-fabric/create-load-balancer-rule).

**Diagnostische gegevens**: standaard ingeschakeld.

**DNS-service**: standaard niet ingeschakeld.

**Omgekeerde proxy**: standaard niet ingeschakeld.

## <a name="create-the-cluster-with-your-parameters"></a>Het cluster te maken met de parameters

Nadat u van de parameters die aan uw behoeften besluit, voer de volgende opdracht om beveiligde Service Fabric-cluster en het certificaat te genereren.

U kunt dit script om op te nemen van aanvullende parameters kunt wijzigen. Zie voor meer informatie over parameters op voor het maken van het cluster de [nieuw AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster.md) cmdlet.

>[!NOTE]
>Voordat u deze opdracht uitvoert, moet u een map waarin u het certificaat van de cluster kunt opslaan.

```PowerShell

# Set the certificate variables. This creates and encrypts a password that Service Fabric will use.
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force

# You must create the folder where you want to store the certificate on your machine before you start this step.
$certfolder="c:\mycertificates\"

# Set the variables for common values. Change the values to fit your needs.
$clusterloc="WestUS"
$clustername = "mysfcluster"
$groupname="mysfclustergroup"       
$vmsku = "Standard_D2_v2"
$vaultname = "mykeyvault"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. The possible values are 1 and 3-99.
$clustersize=1

# Create the Service Fabric cluster and its self-signed certificate. The OS you specify here lets you use this cluster with any applications that are also using containers.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
```

Het maken van het proces kan enkele minuten duren. Nadat de configuratie is voltooid, wordt deze informatie over het cluster in Azure gemaakt. Het certificaat van het cluster ook gekopieerd naar de map - CertificateOutputFolder op het pad dat u hebt opgegeven voor deze parameter.

Noteer de **ManagementEndpoint** URL voor uw cluster, zoals de volgende URL is mogelijk: https://mycluster.westeurope.cloudapp.azure.com:19080.

## <a name="import-the-certificate"></a>Het certificaat importeren

Wanneer het cluster is gemaakt, voert u de volgende opdracht om ervoor te zorgen dat u het zelfondertekende certificaat kunt:

```PowerShell

# To connect to the cluster, install the certificate into the Personal (My) store of the current user on your computer.
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
-FilePath C:\mycertificates\mysfclustergroup20170531104310.pfx `
-Password $certpwd
```

Met deze opdracht wordt het certificaat op de huidige gebruiker van de computer geïnstalleerd. U moet dit certificaat voor toegang tot Service Fabric Explorer en bekijk de status van uw cluster.


## <a name="view-your-cluster-optional"></a>Weergeven van uw cluster (optioneel)

Nadat u het geïmporteerde certificaat en het cluster hebt, kunt u verbinding met het cluster en de status bekijken. Er zijn meerdere manieren verbinding maken via Service Fabric Explorer of PowerShell.

### <a name="service-fabric-explorer"></a>Service Fabric Explorer
U kunt de status van het cluster via Service Fabric Explorer weergeven. Om dit te doen, blader naar de **ManagementEndpoint** URL voor uw cluster, en selecteer vervolgens het certificaatbestand dat u op uw computer opgeslagen.

>[!NOTE]
>Wanneer u een Service Fabric Explorer opent, ziet u een certificaatfout als u een zelfondertekend certificaat. In de rand, hebt u klikt u op **Details**, en klik vervolgens op de **gaat u naar de webpagina** koppeling. In Chrome, hebt u klikt u op **Geavanceerd**, en klik vervolgens op de **gaan** koppeling.

### <a name="powershell"></a>PowerShell

De Service Fabric PowerShell-module biedt veel cmdlets voor het beheren van Service Fabric-clusters, toepassingen en services. Gebruik de [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet verbinding maken met de beveiligde cluster. De vingerafdruk en verbinding eindpunt certificaatdetails vindt u in de uitvoer van de vorige stap.

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
-KeepAliveIntervalInSec 10 `
-X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-StoreLocation CurrentUser -StoreName My
```

U kunt ook controleren of u verbonden bent en dat het cluster is in orde met behulp van de [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.

```PowerShell
Get-ServiceFabricClusterHealth
```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe beveiligde Service Fabric-cluster maken in Azure met behulp van PowerShell.

Vervolgens gaat u door naar de volgende zelfstudie voor meer informatie over het implementeren van een bestaande toepassing:
> [!div class="nextstepaction"]
> [Implementeer een bestaande .NET-toepassing met Docker Compose](service-fabric-host-app-in-a-container.md)

 

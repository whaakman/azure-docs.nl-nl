---
title: HPC Pack 2016 cluster in Azure | Microsoft Docs
description: Informatie over het implementeren van een HPC Pack 2016-cluster in Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 03/09/2018
ms.author: danlep
ms.openlocfilehash: 91f067de33d1ff4bc272773e3db49de47fac2feb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Een HPC Pack 2016-cluster in Azure implementeren

Volg de stappen in dit artikel voor het implementeren van een [Microsoft HPC Pack 2016 Update 1](https://technet.microsoft.com/library/cc514029) cluster in Azure virtuele machines. HPC Pack is oplossing van Microsoft gratis HPC gebaseerd op Microsoft Azure en Windows Server-technologieën en biedt ondersteuning voor een breed bereik van HPC-workloads.

Gebruik een van de [Azure Resource Manager-sjablonen](https://github.com/MsHpcPack/HPCPack2016) voor het implementeren van het cluster HPC Pack 2016. U hebt verschillende mogelijkheden van de clustertopologie met verschillende aantallen en typen van cluster hoofdknooppunten en rekenknooppunten.

## <a name="prerequisites"></a>Vereisten

### <a name="pfx-certificate"></a>PFX-certificaat

Een cluster met Microsoft HPC Pack 2016 vereist een Personal Information Exchange (PFX)-certificaat voor het beveiligen de communicatie tussen de HPC-knooppunten. Het certificaat moet voldoen aan de volgende vereisten:

* Er moet een persoonlijke sleutel sleuteluitwisseling
* Sleutelgebruik bevat digitale handtekening en sleutelcodering
* Uitgebreid sleutelgebruik bevat clientverificatie en serververificatie

Als u geen al een certificaat dat aan deze vereisten voldoet, kunt u het certificaat aanvragen bij een certificeringsinstantie (CA). Ook de volgende opdrachten gebruiken voor het genereren van het zelfondertekende certificaat op basis van het besturingssysteem waarop u de opdracht uitvoert. Vervolgens het certificaat exporteren als een PFX-bestand met wachtwoord zijn beveiligd met persoonlijke sleutel.

* **Voor Windows 10 of Windows Server 2016**, moet u de **nieuw SelfSignedCertificate** PowerShell-cmdlet als volgt:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **Voor besturingssystemen ouder dan Windows 10 of Windows Server 2016**, downloaden de [zelf-ondertekend certificaat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) van het Microsoft Script Center. Pak de inhoud en voer de volgende opdrachten bij een PowerShell-prompt:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

Nadat het certificaat is gemaakt in het archief van de huidige gebruiker, gebruikt u de module Certificaten het certificaat te exporteren als een PFX-bestand met wachtwoord zijn beveiligd met persoonlijke sleutel. U kunt ook exporteren voor het certificaat met behulp van de [Export Pfxcertificate](/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps) PowerShell-cmdlet.

### <a name="upload-certificate-to-an-azure-key-vault"></a>Upload het certificaat naar een Azure sleutelkluis

Voordat u het HPC-cluster implementeert, uploadt u het PFX-certificaat moet een [Azure sleutelkluis](../../key-vault/index.yml) als een geheim en de volgende informatie voor gebruik tijdens de implementatie-record: **kluisnaam**, **kluis resourcegroep**, **certificaat-URL**, en **certificaatvingerafdruk**.

Hier volgt een PowerShell-voorbeeldscript voor het certificaat uploaden, maken van de sleutelkluis en de vereiste gegevens te genereren. Zie voor meer informatie over het uploaden van een certificaat naar een Azure sleutelkluis [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md).

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Ondersteunde topologieën

Kies een van de [Azure Resource Manager-sjablonen](https://github.com/MsHpcPack/HPCPack2016) voor het implementeren van het cluster HPC Pack 2016. Hieronder vindt u op hoog niveau architecturen van drie voorbeeldtopologieën cluster. Hoge beschikbaarheid topologieën bevatten meerdere head clusterknooppunten.

1. Cluster met hoge beschikbaarheid met Active Directory-domein

    ![HA-cluster in AD-domein](./media/hpcpack-2016-cluster/haad.png)


2. Cluster met hoge beschikbaarheid zonder Active Directory-domein

    ![HA cluster zonder AD-domein](./media/hpcpack-2016-cluster/hanoad.png)

3. Cluster met slechts één hoofdknooppunt

   ![Cluster met één hoofdknooppunt](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Een cluster implementeren

Kies een sjabloon voor het maken van het cluster en klik op **implementeren in Azure**. In de [Azure-portal](https://portal.azure.com), Geef parameters op voor de sjabloon, zoals beschreven in de volgende stappen uit. Elke sjabloon wordt gemaakt van alle Azure-resources die vereist zijn voor de HPC-clusterinfrastructuur. Resources omvatten een Azure-netwerk, openbare IP-adres, load balancer (alleen voor een cluster met hoge beschikbaarheid), netwerkinterfaces, beschikbaarheidssets, opslagaccounts en virtuele machines.

### <a name="step-1-select-the-subscription-location-and-resource-group"></a>Stap 1: Selecteer het abonnement, de locatie en de resourcegroep

De **abonnement** en de **locatie** moet hetzelfde zijn die u hebt opgegeven dat wanneer u het PFX-certificaat geüpload (Zie vereisten). Het is raadzaam dat u een andere maakt **resourcegroep** voor de implementatie.

### <a name="step-2-specify-the-parameter-settings"></a>Stap 2: Geef de parameterinstellingen

Invoeren of wijzigen van waarden voor de sjabloonparameters. Klik op het pictogram naast elke parameter voor help-informatie. Zie ook de richtlijn voor [beschikbare VM-grootten](sizes.md).

Geef de waarden die u hebt vastgelegd in de vereisten voor de volgende parameters: **kluisnaam**, **kluis resourcegroep**, **certificaat-URL**, en **certificaatvingerafdruk**.

### <a name="step-3-review-terms-and-create"></a>Stap 3. Voorwaarden controleren en maken
Bekijk de voorwaarden en bepalingen die zijn gekoppeld aan de sjabloon. Als u akkoord gaat, klikt u op **aankoop** implementatie te starten.

Afhankelijk van de clustertopologie kan implementatie 30 minuten duren of langer duren.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
1. Nadat het cluster HPC Pack wordt geïmplementeerd, gaat u naar de [Azure-portal](https://portal.azure.com). Klik op **resourcegroepen**, en zoek de resourcegroep waarin het cluster is geïmplementeerd. U vindt het hoofdknooppunt van virtuele machines.

    ![De hoofdknooppunten cluster in de portal](./media/hpcpack-2016-cluster/clusterhns.png)

2. Klik op één hoofdknooppunt (in een cluster met hoge beschikbaarheid, klik op een van de hoofdknooppunten). In **overzicht**, vindt u de openbare IP-adres of de volledige DNS-naam van het cluster.

    ![Cluster-verbindingsinstellingen](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Klik op **Connect** Meld u aan bij een van de hoofdknooppunten met extern bureaublad met uw beheerder van de opgegeven gebruikersnaam. Als het cluster dat u hebt geïmplementeerd in een Active Directory-domein, de gebruikersnaam van het formulier is \<privateDomainName >\\\<adminUsername > (bijvoorbeeld hpc.local\hpcadmin).

## <a name="next-steps"></a>Volgende stappen
* Verzenden van taken naar uw cluster. Zie [taken verzenden naar een HPC Pack HPC-cluster in Azure](hpcpack-cluster-submit-jobs.md) en [een HPC Pack 2016-cluster in Azure met Azure Active Directory beheren](hpcpack-cluster-active-directory.md).


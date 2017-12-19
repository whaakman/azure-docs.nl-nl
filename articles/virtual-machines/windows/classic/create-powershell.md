---
title: Maken van een Windows-VM met PowerShell | Microsoft Docs
description: Windows virtuele machines maakt met Azure PowerShell en het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: af672a873b33cbd0b6151fd564e84c96f0b6e1e3
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a>Een Windows-machine maken met PowerShell en het klassieke implementatiemodel
> [!div class="op_single_selector"]
> * [Azure portal - Windows](tutorial.md)
> * [PowerShell - Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Deze stappen ziet u hoe u kunt een set van Azure PowerShell-opdrachten die maken en vooraf configureren van een op basis van Windows Azure virtuele machine met behulp van een benadering bouwsteen aanpassen. U kunt dit proces te snel een opdrachtset voor een nieuwe Windows gebaseerde virtuele machine maken en uitbreiden van een bestaande implementatie of te maken van meerdere sets die snel van een aangepaste ontwikkelen en testen of de IT pro-omgeving bouwen.

Deze stappen volgt een benadering invullen-in-the-lege waarden voor het maken van Azure PowerShell-opdrachtsets. Deze methode kan nuttig zijn als u niet bekend met PowerShell bent of u alleen weten welke waarden wilt wilt opgeven voor een geslaagde configuratie. Geavanceerde PowerShell-gebruikers kunnen de opdrachten en vervang hun eigen waarden voor de variabelen (de regels die beginnen met '$').

Als u dit nog niet hebt gedaan, gebruikt u de instructies in [installeren en configureren van Azure PowerShell](/powershell/azure/overview) Azure PowerShell te installeren op uw lokale computer. Vervolgens opent u een Windows PowerShell-opdrachtprompt.

## <a name="step-1-add-your-account"></a>Stap 1: Uw account toevoegen
1. Typ het volgende achter de PowerShell-prompt **Add-AzureAccount** en klik op **Enter**. 
2. Typ in het e-mailadres dat is gekoppeld aan uw Azure-abonnement en klikt u op **doorgaan**. 
3. Typ in het wachtwoord voor uw account. 
4. Klik op **aanmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Stap 2: Uw abonnement en storage-account instellen
Uw Azure-abonnement en storage-account instellen door het uitvoeren van deze opdrachten bij de opdrachtprompt van Windows PowerShell. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens lang zijn en de juiste namen.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

U kunt de naam van het juiste abonnement ophalen uit de eigenschap SubscriptionName voor de uitvoer van de **Get-AzureSubscription** opdracht. U kunt de juiste opslagaccountnaam ophalen van de eigenschap Label van de uitvoer van de **Get-AzureStorageAccount** opdracht, na het uitvoeren van de **Select-AzureSubscription** opdracht.

## <a name="step-3-determine-the-imagefamily"></a>Stap 3: De ImageFamily bepalen
Vervolgens moet u bepalen welke waarde ImageFamily of het Label voor de specifieke afbeelding overeenkomt met de Azure virtuele machine die u wilt maken. U kunt de lijst met beschikbare ImageFamily waarden met deze opdracht ophalen.

    Get-AzureVMImage | select ImageFamily -Unique

Hier volgen enkele voorbeelden van waarden ImageFamily voor Windows-computers:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* SQL Server 2012 SP1 Enterprise in WindowsServer 2012

Als u de installatiekopie die u zoekt vinden, opent u een nieuw exemplaar van de editor van uw keuze of PowerShell Integrated Scripting Environment (ISE). Kopieer het volgende in het nieuwe tekstbestand of de PowerShell ISE, vervangen door de waarde ImageFamily.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

In sommige gevallen is de naam van de installatiekopie in de eigenschap Label in plaats van de ImageFamily-waarde. Als u de installatiekopie die u zoekt met de eigenschap ImageFamily niet vinden, moet u de afbeeldingen weergeven door de eigenschap Label met deze opdracht.

    Get-AzureVMImage | select Label -Unique

Als u de juiste installatiekopie met deze opdracht kunt vinden, opent u een nieuw exemplaar van de editor van uw keuze of de PowerShell ISE. Kopieer de volgende tekst in het nieuwe tekstbestand of de PowerShell ISE, vervangen door de waarde voor het Label.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Stap 4: De opdrachtset bouwen
Maken van de rest van de opdracht die is ingesteld door de juiste reeks blokken hieronder kopiëren naar uw nieuw tekstbestand of de ISE en invullen van de waarden van variabelen en verwijderen van de < en > tekens. Zie de twee [voorbeelden](#examples) aan het einde van dit artikel voor een idee van het eindresultaat.

Start de opdracht die is ingesteld door een van deze twee opdracht blokken (vereist) te kiezen.

Optie 1: Geef de naam van een virtuele machine en een grootte.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Optie 2: Geef een naam, de grootte en de naam van de beschikbaarheidsset.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Zie voor de InstanceSize waarden voor D, DS of G-serie virtuele machines, [virtuele Machine en Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Als u een Enterprise-overeenkomst met Software Assurance hebt en u van plan bent om te profiteren van de Windows Server [hybride gebruiken voordeel](https://azure.microsoft.com/pricing/hybrid-use-benefit/), toevoegen de **- LicenseType** -parameter voor de **nieuw AzureVMConfig** -cmdlet voor het doorgeven van de waarde **Windows_Server** voor de typische gebruiksvoorbeeld.  Zorg ervoor dat u gebruikmaakt van een installatiekopie die u hebt geüpload; u niet een standaardinstallatiekopie uit de galerie gebruiken met het voordeel van hybride gebruiken.
> 
> 

Optioneel, voor een zelfstandige Windows-computer het lokale administrator-account en wachtwoord opgeven.

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Kies een sterk wachtwoord. De sterkte Zie [wachtwoord Checker: met behulp van sterke wachtwoorden](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Eventueel om de Windows-computer toevoegen aan een bestaand Active Directory-domein, geef het lokale administrator-account en wachtwoord, het domein en de naam en het wachtwoord van een domeinaccount.

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Zie voor extra vooraf configuratieopties voor virtuele machines op basis van Windows, de syntaxis voor de **Windows** en **WindowsDomain** parametersets [toevoegen AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).

Desgewenst toewijzen de virtuele machine, een specifiek IP-adres, een statische DIP genoemd.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

U kunt controleren of een specifiek IP-adres beschikbaar met:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Optioneel, de virtuele machine aan een specifiek subnet in een Azure-netwerk toewijzen.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

Voeg desgewenst één gegevensschijf aan de virtuele machine.

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Voor een Active Directory-domeincontroller instellen $hcaching op 'Geen'.

Voeg desgewenst de virtuele machine aan een bestaande set voor het externe verkeer met gelijke taakverdeling.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Ten slotte, kies een van deze blokken vereist opdracht voor het maken van de virtuele machine.

Optie 1: Maak de virtuele machine in een bestaande cloudservice.

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

De korte naam van de cloudservice is de naam die wordt weergegeven in de lijst met Cloud-Services in de Azure-portal of in de lijst met resourcegroepen in de Azure portal.

Optie 2: Maak de virtuele machine in een bestaande cloudservice en het virtuele netwerk.

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Stap 5: Voer de opdrachtset
Controleer de Azure PowerShell-opdrachtenset die u in een teksteditor of de PowerShell ISE die bestaan uit meerdere blokken van opdrachten uit stap 4 hebt gemaakt. Zorg ervoor dat u alle benodigde variabelen hebt opgegeven en of ze de juiste waarden hebben. Ook voor zorgen dat u alle hebt verwijderd de < en > tekens.

Als u van een teksteditor gebruikmaakt, de opdrachtenset naar het Klembord kopiëren en vervolgens met de rechtermuisknop op uw Windows PowerShell-opdrachtprompt openen. Hiermee wordt de opdrachtenset als een reeks PowerShell-opdrachten uitgeven en maakt u uw virtuele machine van Azure. U kunt ook de opdracht ingesteld in de PowerShell ISE uitvoeren.

Als u deze virtuele machine opnieuw of een vergelijkbare maakt, kunt u:

* Sla deze opdracht ingesteld als het bestand in een PowerShell-script (*.ps1).
* Opslaan van deze opdracht ingesteld als een Azure Automation-runbook in de **Automation-Accounts** sectie van de Azure-portal.

## <a id="examples"></a>Voorbeelden
Hier vindt u twee voorbeelden van het gebruik van de bovenstaande stappen voor het bouwen van Azure PowerShell-opdrachtsets die op basis van Windows Azure virtuele machines maken.

### <a name="example-1"></a>Voorbeeld 1
Ik moet een PowerShell opdracht ingesteld op de eerste virtuele machine maken voor een Active Directory-domeincontroller die:

* Maakt gebruik van de installatiekopie van het Windows Server 2012 R2 Datacenter.
* De naam AZDC1 heeft.
* Een zelfstandige computer is.
* Heeft een schijf aanvullende gegevens van 20 GB.
* Het statische IP-adres 192.168.244.4 heeft.
* In het back-end-subnet van het virtuele netwerk AZDatacenter is.
* In de cloudservice van Azure TailspinToys is.

Hier volgt de bijbehorende Azure PowerShell-opdracht ingesteld op deze virtuele machine maken met lege regels tussen elk blok omwille van de leesbaarheid.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Voorbeeld 2
Ik moet een PowerShell opdracht ingesteld op een virtuele machine maken voor een line-of-business-server die:

* Maakt gebruik van de installatiekopie van het Windows Server 2012 R2 Datacenter.
* De naam LOB1 heeft.
* Is een lid van het domein corp.contoso.com.
* Heeft een schijf aanvullende gegevens van 200 GB.
* In het front-end-subnet van het virtuele netwerk AZDatacenter is.
* In de cloudservice van Azure TailspinToys is.

Hier volgt de bijbehorende Azure PowerShell-opdracht voor het maken van deze virtuele machine ingesteld.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Volgende stappen
Als u een besturingssysteemschijf die groter is dan 127 GB nodig hebt, kunt u [Vouw het station OS](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


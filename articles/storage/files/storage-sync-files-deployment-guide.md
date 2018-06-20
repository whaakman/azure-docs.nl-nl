---
title: Implementeren van Azure File-synchronisatie (preview) | Microsoft Docs
description: Informatie over het implementeren van Azure bestand synchronisatie van begin tot eind.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: wgries
ms.openlocfilehash: f1230cbc4d654bfb59bb328ed7d75c6fa76ff10c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268246"
---
# <a name="deploy-azure-file-sync-preview"></a>Azure File-synchronisatie (preview) implementeren
Gebruik Azure bestand Sync (preview) te centraliseren bestandsshares van uw organisatie in Azure-bestanden, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Azure File-synchronisatie transformeert Windows Server in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB en NFS FTPS gebruiken. U kunt zoveel caches als u over de hele wereld nodig hebben.

Wij raden u lezen [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md) en [Planning voor de implementatie van een Azure-bestand Sync](storage-sync-files-planning.md) voordat u de stappen in dit artikel wordt beschreven.

## <a name="prerequisites"></a>Vereisten
* Een Azure storage-account en een Azure-bestand delen in dezelfde regio die u wilt implementeren Azure File-synchronisatie. Zie voor meer informatie:
    - [Beschikbaarheid in regio's](storage-sync-files-planning.md#region-availability) voor Azure File-synchronisatie.
    - [Een opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor een stapsgewijze beschrijving van het maken van een opslagaccount.
    - [Een bestandsshare maken](storage-how-to-create-file-share.md) voor een stapsgewijze beschrijving van het maken van een bestandsshare.
* Ten minste één ondersteund exemplaar van Windows Server of Windows Server-cluster te synchroniseren met het synchroniseren van Azure-bestand. Zie voor meer informatie over ondersteunde versies van Windows Server [interoperabiliteit met Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).
* Zorg ervoor dat PowerShell 5.1 is geïnstalleerd op uw Windows-Server. Zorg ervoor dat u ten minste worden uitgevoerd als u van Windows Server 2012 R2 gebruikmaakt, 5.1 PowerShell. \*. U kunt deze controle op Windows Server 2016 veilig overslaan als PowerShell 5.1 de standaard versie out-of-box is. U kunt controleren dat u PowerShell 5.1 worden uitgevoerd op Windows Server 2012 R2. \* door te kijken naar de waarde van de **PSVersion** eigenschap van de **$PSVersionTable** object:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Als de waarde van de PSVersion minder dan 5.1 is. \*, worden als het geval is bij de meeste nieuwe installaties van Windows Server 2012 R2, u kunt vervolgens gemakkelijk upgraden door te downloaden en installeren van [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Het juiste pakket downloaden en installeren voor Windows Server 2012 R2 is **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Azure File-synchronisatie ondersteunt nog geen PowerShell 6 + op Windows Server 2012 R2 of Windows Server 2016.
* De AzureRM PowerShell-module op de servers die u wilt gebruiken met het synchroniseren van Azure-bestand. Zie voor meer informatie over het installeren van de AzureRM PowerShell-modules [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Altijd het beste de nieuwste versie van de Azure PowerShell-modules gebruiken. 

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Voorbereiden van Windows Server voor gebruik met Azure File-synchronisatie
Voor elke server die u gebruiken met het synchroniseren van Azure-bestand wilt, met inbegrip van elk serverknooppunt in een failovercluster uitschakelen **verbeterde beveiliging van Internet Explorer**. Dit is alleen vereist voor de registratie van de oorspronkelijke server. U kunt deze weer inschakelen nadat de server is geregistreerd.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Open Serverbeheer.
2. Klik op **lokale Server**:  
    !['Lokale Server' aan de linkerkant van de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. Op de **eigenschappen** subpane, selecteer de koppeling voor **verbeterde beveiliging van Internet Explorer**.  
    ![Het deelvenster 'IE verbeterde beveiliging' in de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. In de **verbeterde beveiliging van Internet Explorer** dialoogvenster, **uit** voor **beheerders** en **gebruikers**:  
    ![De verbeterde beveiliging van Internet Explorer-pop-upvenster met 'Uit' geselecteerd](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Als u wilt de verbeterde beveiliging van Internet Explorer uitschakelen, voer het volgende vanuit een PowerShell-sessie met verhoogde bevoegdheid:

```PowerShell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="install-the-azure-file-sync-agent"></a>De Azure-bestand Sync-agent installeren
De Azure-bestand Sync-agent is een downloadbare pakket waarmee Windows-Server moet worden gesynchroniseerd met een Azure-bestandsshare. 

# <a name="portaltabportal"></a>[Portal](#tab/portal)
U kunt downloaden via de agent uit de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Wanneer het downloaden is voltooid, dubbelklikt u op het MSI-pakket voor het starten van de installatie van de Azure-bestand Sync-agent.

> [!Important]  
> Als u gebruiken van Azure File-synchronisatie met een failovercluster wilt, moet u de Azure-bestand Sync-agent geïnstalleerd op elk knooppunt in het cluster.

U wordt aangeraden dat u het volgende doen:
- Laat het standaardinstallatiepad (C:\Program Files\Azure\StorageSyncAgent), het oplossen van problemen en serveronderhoud vereenvoudigen.
- Schakel Microsoft Update om Azure bestand Sync up-to-date te houden. Alle updates optreden naar de Azure bestand Sync-agent, met inbegrip van functie-updates en hotfixes, van Microsoft Update. Het is raadzaam om de meest recente update installeren om te synchroniseren van Azure-bestand. Zie voor meer informatie [Azure bestand Sync updatebeleid](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Wanneer de installatie van de Azure-bestand Sync-agent is voltooid, wordt de gebruikersinterface van de registratie Server automatisch wordt geopend. Hebt u een synchronisatie opslagservice voordat registrating; Zie de volgende sectie voor het maken van een opslag-Sync-Service.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Voer de volgende PowerShell-code voor het downloaden van de juiste versie van de Azure-bestand Sync-agent voor uw besturingssysteem en installeer het op uw systeem.

```PowerShell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="deploy-the-storage-sync-service"></a>De synchronisatieservice opslag implementeren 
De implementatie van Azure bestand synchronisatie begint met het plaatsen van een **Sync opslagservice** resource in een resourcegroep van de geselecteerde abonnement. U wordt aangeraden inrichten als enkele van deze indien nodig. U maakt een vertrouwensrelatie tussen de servers en deze bron en een server kan alleen worden geregistreerd bij een opslag-Sync-Service. Als gevolg hiervan is het aanbevolen om zo veel opslag Synchronisatieservices als nodig is voor afzonderlijke groepen van servers te implementeren. Houd er rekening mee dat servers van andere opslag Synchronisatieservices kunnen niet worden gesynchroniseerd met elkaar.

> [!Note]
> De synchronisatieservice opslag overgenomen machtigingen van het abonnement en de resourcegroep die is geïmplementeerd in. Het is raadzaam dat u zorgvuldig controleren wie toegang heeft tot het. Entiteiten met schrijftoegang kunnen beginnen met het synchroniseren van nieuwe sets van bestanden van servers geregistreerd bij deze opslag service synchroniseren en ervoor zorgen dat gegevens naar de Azure-opslag die toegankelijk is voor deze stromen.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Voor het implementeren van een opslag-Sync-Service, gaat u naar de [Azure-portal](https://portal.azure.com/), klikt u op *nieuw* en zoek vervolgens naar Azure File-synchronisatie. Selecteer in de lijst met zoekresultaten **Azure bestand Sync (preview)**, en selecteer vervolgens **maken** openen de **implementeren opslag Sync** tabblad.

Voer de volgende gegevens in het deelvenster dat wordt geopend:

- **Naam**: een unieke naam (per abonnement) voor de synchronisatieservice opslag.
- **Abonnement**: het abonnement waarin u wilt maken van de Storage-Sync-Service. Afhankelijk van de strategie van de configuratie van uw organisatie wellicht u de toegang tot een of meer abonnementen. Een Azure-abonnement is de meest eenvoudige container voor facturering voor elke cloudservice (zoals Azure-bestanden).
- **Resourcegroep**: een resourcegroep is een logische groep van Azure-resources, zoals een opslagaccount of een opslag-Sync-Service. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken voor Azure File-synchronisatie. (Aangeraden resourcegroepen gebruiken als containers voor het isoleren van resources voor uw organisatie, zoals HR-resources of resources voor een bepaald project groeperen logisch.)
- **Locatie**: de regio waarin u wilt implementeren van Azure File-synchronisatie. Alleen ondersteunde regio's zijn beschikbaar in deze lijst.

Wanneer u klaar bent, selecteert u **maken** voor het implementeren van de Storage-Sync-Service.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Voordat de interactie met de Azure-bestand Sync-cmdlets, moet u een DLL-bestand importeren en de context van een Azure-bestand synchronisatie management te maken. Dit is nodig omdat de Azure-bestand Sync-cmdlets nog niet deel uit van de AzureRM PowerShell-module zijn.

> [!Note]  
> Het pakket StorageSync.Management.PowerShell.Cmdlets.dll, waarin de Azure-bestand Sync-cmdlets (opzettelijk) bevat een cmdlet met een niet-goedgekeurde term (`Login`). De naam van de `Login-AzureRmStorageSync` is gekozen op basis van de `Login-AzureRmAccount` alias in de module AzureRM PowerShell cmdlet. Dit foutbericht (en de cmdlet) verwijderd, de Azure-bestand Sync-agent wordt toegevoegd aan de AzureRM PowerShell-module.

```PowerShell
$acctInfo = Login-AzureRmAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzureRmResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Nadat u hebt gemaakt dat de context van de Azure-bestand synchronisatie met de `Login-AzureRmStorageSync` cmdlet, kunt u de Storage-Sync-Service. Zorg ervoor dat u `<my-storage-sync-service>` met de gewenste naam van uw opslag Sync-Service.

```PowerShell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>WindowsServer registreren met Storage Sync-Service
Uw Windows-Server registreren met een opslag-synchronisatieservice stelt een vertrouwensrelatie tussen uw server (of een cluster) en de opslag-synchronisatieservice. Een server kan slechts aan één opslag-Sync-Service worden geregistreerd en kunnen synchroniseren met andere servers en Azure-bestandsshares die zijn gekoppeld aan dezelfde opslag-Sync-Service.

> [!Note]
> Serverregistratie uw Azure-referenties gebruikt voor het maken van een vertrouwensrelatie tussen de Storage-Sync-Service en Windows Server, maar vervolgens de server maakt en maakt gebruik van de eigen identiteit die is geldig als de server blijft ingeschreven en de huidige Shared Access Signature-token (SAS-opslag) is geldig. Een nieuwe SAS-token kan niet worden verleend aan de server wanneer de server geregistreerd is, dus verwijderen van de mogelijkheid van de server voor toegang tot Azure-bestandsshares, synchronisatie wordt gestopt.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
De gebruikersinterface van de registratie-Server wordt automatisch geopend na de installatie van de Azure-bestand Sync-agent. Als dit niet het geval is, kunt u deze handmatig openen vanuit de bestandslocatie: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Wanneer de gebruikersinterface van de registratie-Server wordt geopend, selecteert u **aanmelden** om te beginnen.

Nadat u zich aanmeldt, wordt u gevraagd om de volgende informatie:

![Een schermopname van de gebruikersinterface van de registratie van Server](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-abonnement**: het abonnement dat de synchronisatieservice opslag bevat (Zie [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service)). 
- **Resourcegroep**: de resourcegroep met de Storage-Sync-Service.
- **Opslag-synchronisatieservice**: de naam van de Storage-Sync-Service die u wilt registreren.

Nadat u de juiste gegevens hebt geselecteerd, selecteert u **registreren** om de inschrijving van de server te voltooien. Als onderdeel van het registratieproces wordt u gevraagd voor een extra aanmelden.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Een groep voor synchronisatie en een cloudeindpunt maken
Een groep voor synchronisatie definieert de synchronisatie-topologie voor een set bestanden. Eindpunten in een groep voor synchronisatie zijn gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt waarmee een Azure-bestandsshare en één of server-eindpunten bevatten. Een servereindpunt vertegenwoordigt een pad op de geregistreerde server. Een server zijn server-eindpunten in meerdere synchronisatiegroepen. U kunt zoveel synchronisatiegroepen behoefte aan om de topologie van uw gewenste synchronisatie op de juiste wijze te beschrijven.

Een cloudeindpunt is een verwijzing naar een Azure-bestandsshare. Alle server-eindpunten worden gesynchroniseerd met een cloudeindpunt, waardoor het cloudeindpunt de hub. Het opslagaccount voor de Azure-bestandsshare moet zich bevinden in dezelfde regio bevinden als de Storage-Sync-Service. Het geheel van de Azure-bestandsshare worden gesynchroniseerd, met één uitzondering: de speciale map die vergelijkbaar is met de verborgen "System Volume Information"-map op een NTFS-volume worden ingericht. Deze map heet '. SystemShareInformation'. Deze bevat metagegevens belangrijk synchroniseren die niet meer gesynchroniseerd tot andere eindpunten. Gebruik of verwijder deze niet!

> [!Important]  
> U kunt wijzigingen aanbrengen in een cloudeindpunt of het servereindpunt in de groep voor synchronisatie en hebben de bestanden die zijn gesynchroniseerd met de andere eindpunten in de groep voor synchronisatie. Als u een wijziging van het eindpunt van de cloud (Azure-bestandsshare) rechtstreeks, moeten wijzigingen eerst worden gedetecteerd door een Azure-bestand Sync-taak voor het detecteren van wijzigen. Een wijziging detectie-taak wordt gestart voor een cloudeindpunt slechts eenmaal per 24 uur. Zie voor meer informatie [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection).

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Maken van een groep voor synchronisatie in de [Azure-portal](https://portal.azure.com/), gaat u naar uw Storage-Sync-Service en selecteer vervolgens **+ groep voor synchronisatie**:

![Maak een nieuwe synchronisatie-groep in de Azure portal](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Voer de volgende informatie om u te maken van een groep voor synchronisatie met een cloudeindpunt in het deelvenster dat wordt geopend:

- **Synchronisatie-groepsnaam**: de naam van de groep voor synchronisatie moet worden gemaakt. Deze naam moet uniek zijn binnen de synchronisatieservice opslag, maar mag de naam die logisch is voor u.
- **Abonnement**: het abonnement waarbij u de Storage-Sync-Service in geïmplementeerd [implementeren van de synchronisatieservice opslag](#deploy-the-storage-sync-service).
- **Storage-account**: als u selecteert **storage-account selecteren**, een ander venster weergegeven waarin u het opslagaccount met de Azure-bestandsshare die u synchroniseren wilt met kunt selecteren.
- **Azure-bestandsshare**: de naam van de Azure-bestandsshare die u wilt synchroniseren.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
De volgende PowerShell voor het maken van de groep voor synchronisatie worden uitgevoerd. Vervang `<my-sync-group>` met de gewenste naam van de groep voor synchronisatie.

```PowerShell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Wanneer de groep voor synchronisatie met succes is gemaakt, kunt u uw cloudeindpunt. Zorg ervoor dat u `<my-storage-account>` en `<my-file-share>` met de verwachte waarden.

```PowerShell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzureRmStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzureStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Een servereindpunt maken
Een servereindpunt vertegenwoordigt een specifieke locatie op een geregistreerde server, zoals een map op een volume van de server. Een servereindpunt moet een pad op een geregistreerde server (in plaats van een gekoppelde share) en voor het gebruik van cloud tiering, moet het pad op een niet-volume. NAS-opslag (NAS) wordt niet ondersteund.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Een servereindpunt toevoegen, gaat u naar de zojuist gemaakte sync-groep en selecteer vervolgens **servereindpunt toevoegen**.

![Een nieuw servereindpunt in het deelvenster van de groep sync toevoegen](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

In de **servereindpunt toevoegen** deelvenster, voer de volgende informatie om een servereindpunt te maken:

- **Geregistreerde server**: de naam van de server of cluster waar u wilt maken van de server het eindpunt.
- **Pad**: de Windows Server-pad moet worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud Tiering**: overschakelen naar de in- of uitschakelen, cloud tiering. Met cloud tiering worden zelden gebruikt of de bestanden zijn geopend kunt tiers worden verdeeld aan Azure-bestanden.
- **Vrije ruimte op volume**: de hoeveelheid vrije ruimte te reserveren op het volume waarop het servereindpunt zich bevindt. Bijvoorbeeld, als vrije ruimte op volume is ingesteld op 50% op een volume dat een eindpunt met één server is, is ongeveer de helft van de hoeveelheid gegevens gelaagd op Azure-bestanden. Ongeacht of cloud tiering is ingeschakeld, worden uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Selecteer om de server het eindpunt toe **maken**. Uw bestanden worden nu gesynchroniseerd tussen uw Azure-bestandsshare en Windows Server. 

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Voer de volgende PowerShell-opdrachten om te maken van het servereindpunt en zorg ervoor dat u `<your-server-endpoint-path>` en `<your-volume-free-space>` met de gewenste waarden.

```PowerShell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Voorbereiden met het synchroniseren van Azure-bestand
De aanbevolen stappen om vrij te geven op Azure bestand Sync voor de eerste met nul uitvaltijd behoud volledige kwaliteit en toegangsbeheerlijst (ACL) zijn als volgt:
 
1. Implementeer een opslag-Sync-Service.
2. Maak een groep voor synchronisatie.
3. Azure File-Sync-agent installeren op de server met de volledige gegevensset.
4. Registreren van die server en een servereindpunt maken op de share. 
5. Kunnen synchronisatie kan het volledige uploaden naar de Azure-bestandsshare (cloudeindpunt).  
6. Nadat de initiële uploaden voltooid is, kunt u Azure bestand Sync-agent installeren op elk van de resterende servers.
7. Nieuwe bestandsshares maken op elk van de resterende servers.
8. Maak server-eindpunten op nieuwe bestandsshares met cloud opslaglagenbeleid, indien gewenst. (Deze stap vereist extra opslagruimte beschikbaar is voor de eerste installatie).
9. Laat Azure bestand Sync-agent voor een snelle herstellen van de volledige naamruimte zonder de werkelijke gegevensoverdracht. Nadat de volledige naamruimte-synchronisatie wordt synchronisatie-engine vult u ruimte op de lokale schijf op basis van de cloud opslaglagenbeleid voor het servereindpunt. 
10. Zorg ervoor dat synchronisatie is voltooid en testen van uw topologie desgewenst. 
11. Gebruikers en toepassingen naar deze nieuwe share omleiden.
12. Desgewenst kunt u eventuele dubbele shares op de servers verwijderen.
 
Als u geen extra opslag voor de eerste voorbereiding hebt en wilt koppelen aan de bestaande shares, kunt u de gegevens in de Azure-bestandsshares vooraf is ingevuld. Deze aanpak wordt voorgesteld, en alleen als u kan uitvaltijd accepteren en geen gegevens wijzigen op de servershares absoluut tijdens het eerste voorbereidingsproces beschreven waarborgen. 
 
1. Zorg ervoor dat de gegevens op een van de server niet tijdens het voorbereidingsproces beschreven wijzigen.
2. Vooraf zaad Azure bestand deelt met de server-gegevens met behulp van een hulpprogramma voor gegevensoverdracht via het SMB bijvoorbeeld Robocopy, direct SMB-kopie. Aangezien AzCopy niet van gegevens via het SMB uploaden biedt zodat deze kan niet worden gebruikt voor het vooraf seeding.
3. Azure File-synchronisatie-topologie maken met de gewenste server-eindpunten die verwijst naar de bestaande shares.
4. Kunnen synchronisatie afstemmingsproces op alle eindpunten te voltooien. 
5. Zodra de afstemming is voltooid, kunt u shares voor wijzigingen kunt openen.
 
Zorg ervoor dat momenteel enkele beperkingen - benadering vooraf seeding heeft 
- Volledige fidelity op bestanden blijven niet behouden. Bijvoorbeeld verbroken bestanden ACL's en tijdstempels.
- Gegevens wijzigen op de server voordat de synchronisatie-topologie is volledig en actief kunnen veroorzaakt een conflict veroorzaken op de server-eindpunten.  
- Nadat het cloudeindpunt is gemaakt, wordt een proces voor het detecteren van de bestanden in de cloud voordat u begint de initiële synchronisatie uitgevoerd in Azure File-synchronisatie. De tijd om dit proces te voltooien, is afhankelijk van de diverse factoren zoals netwerksnelheid, beschikbare bandbreedte en het aantal bestanden en mappen. Voor de ruwe schatting in de preview-versie-detectie wordt ongeveer op 10 bestanden per seconde worden uitgevoerd.  Daarom, zelfs als vooraf seeding snel wordt uitgevoerd, de totale tijd om op te halen van een volledig actief systeem mogelijk aanzienlijk langer wanneer gegevens vooraf geseede in de cloud.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Een DFS-replicatie (DFS-R)-implementatie migreren naar Azure File-synchronisatie
Een DFS-R-implementatie migreren naar Azure File-synchronisatie:

1. Maak een synchronisatie-groep staat voor de DFS-R-topologie die u wilt vervangen.
2. Start op de server met de volledige set gegevens in uw DFS-R-topologie te migreren. Azure File-synchronisatie installeren op die server.
3. Registreren van die server en maak een servereindpunt voor de eerste server worden gemigreerd. Schakel geen cloud tiering.
4. Alle het synchroniseren van gegevens naar uw Azure-bestandsshare (cloudeindpunt) laten.
5. Installeren en registreren van de Azure-bestand Sync-agent op elk van de resterende DFS-R-servers.
6. Uitschakelen van DFS-R. 
7. Maak een servereindpunt op elk van de DFS-R-servers. Schakel geen cloud tiering.
8. Zorg ervoor dat synchronisatie is voltooid en testen van uw topologie desgewenst.
9. Buiten gebruik stellen van DFS-R.
10. Cloud lagen kan nu worden ingeschakeld op een willekeurig servereindpunt naar wens.

Zie voor meer informatie [Azure bestand Sync interop met Distributed File System (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen of verwijderen van een servereindpunt van het Azure-synchronisatie](storage-sync-files-server-endpoint.md)
- [Registreren of de registratie van een server met het synchroniseren van Azure-bestand](storage-sync-files-server-registration.md)
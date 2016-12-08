---
title: Aan de slag met Azure File Storage in Windows | Microsoft Docs
description: Sla bestandsgegevens met Azure File Storage op in de cloud en koppel de bestandsshare in de cloud vanuit een virtuele machine van Azure of vanuit een on-premises toepassing waarop Windows wordt uitgevoerd.
services: storage
documentationcenter: .net
author: mine-msft
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/18/2016
ms.author: minet
translationtype: Human Translation
ms.sourcegitcommit: bc97472a07ac4c27c60fbe2cb803f2360a3362c4
ms.openlocfilehash: af8f20beb8899ff1df733d1a4c544c75c6889ea6


---
# <a name="get-started-with-azure-file-storage-on-windows"></a>Aan de slag met Azure File Storage in Windows
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

[!INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

Zie [How to use Azure File Storage with Linux](storage-how-to-use-files-linux.md) (Azure File Storage gebruiken met Linux) voor meer informatie over het gebruik van File Storage met Linux.

Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over de schaalbaarheids- en prestatiedoelen van File Storage.

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

## <a name="video-using-azure-file-storage-with-windows"></a>Video: Azure File Storage gebruiken met Windows
Hier volgt een video waarin u ziet hoe u Azure-bestandsshares maakt en gebruikt in Windows.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-File-Storage-with-Windows/player]
> 
> 

## <a name="about-this-tutorial"></a>Over deze zelfstudie
In deze zelfstudie maakt u kennis met de basisprincipes van het werken met Microsoft Azure File Storage. In deze zelfstudie leert u het volgende:

* Azure Portal of PowerShell gebruiken om een nieuwe bestandsshare in Azure te maken, een map toe te voegen, een lokaal bestand te uploaden naar de share en de bestanden in de map weer te geven.
* De bestandsshare koppelen op dezelfde manier waarop u een SMB-share koppelt.
* De clientbibliotheek van Azure Storage voor .NET gebruiken om de bestandsshare te openen vanuit een on-premises toepassing. Een consoletoepassing maken en de volgende acties uitvoeren met de bestandsshare:
  * De inhoud van een bestand in de share naar het consolevenster schrijven.
  * Een quotum (maximumgrootte) voor de bestandsshare instellen.
  * Een Shared Access Signature maken voor een bestand dat gebruikmaakt van een gedeeld toegangsbeleid dat voor de share is gedefinieerd.
  * Een bestand kopiëren naar een ander bestand in hetzelfde opslagaccount.
  * Een bestand kopiëren naar een blob in hetzelfde opslagaccount.
* Metrische gegevens van Azure Storage gebruiken voor het oplossen van problemen

File Storage wordt nu ondersteund voor alle opslagaccounts. U kunt dus een bestaand opslagaccount gebruiken of een nieuw opslagaccount maken. Zie [How to create a storage account](storage-create-storage-account.md#create-a-storage-account) (Een opslagaccount maken) voor informatie over het maken van een nieuw opslagaccount.

## <a name="use-the-azure-portal-to-manage-a-file-share"></a>Azure Portal gebruiken om een bestandsshare te beheren
Met [Azure Portal](https://portal.azure.com) beschikken klanten over een gebruikersinterface voor het beheren van bestandsshares. Vanuit de portal kunt u het volgende:

* De bestandsshare maken
* Bestanden uploaden naar en downloaden van de bestandsshare
* Het werkelijke gebruik van elke bestandsshare controleren
* Het quotum van de sharegrootte aanpassen
* De opdracht `net use` uitvoeren om de bestandsshare te koppelen vanuit een Windows-client

### <a name="create-file-share"></a>Bestandsshare maken
1. Meld u aan bij Azure Portal.
2. Klik in het navigatiemenu op **Opslagaccounts** of **Opslagaccounts (klassiek)**.
   
    ![Schermafbeelding van het maken van een bestandsshare in de portal](./media/storage-dotnet-how-to-use-files/files-create-share-0.png)
3. Kies uw opslagaccount.
   
    ![Schermafbeelding van het maken van een bestandsshare in de portal](./media/storage-dotnet-how-to-use-files/files-create-share-1.png)
4. Kies de service Bestanden.
   
    ![Schermafbeelding van het maken van een bestandsshare in de portal](./media/storage-dotnet-how-to-use-files/files-create-share-2.png)
5. Klik op Bestandsshares en volg de koppeling om uw eerste bestandsshare te maken.
   
    ![Schermafbeelding van het maken van een bestandsshare in de portal](./media/storage-dotnet-how-to-use-files/files-create-share-3.png)
6. Vul de naam en de grootte (maximaal 5120 GB) van de bestandsshare in om uw eerste bestandsshare te maken. Wanneer de bestandsshare is gemaakt, kunt u deze koppelen vanuit elk bestandssysteem dat SMB 2.1 of SMB 3.0 ondersteunt.
   
    ![Schermafbeelding van het maken van een bestandsshare in de portal](./media/storage-dotnet-how-to-use-files/files-create-share-4.png)

### <a name="upload-and-download-files"></a>Bestanden uploaden en downloaden
1. Kies een bestaande bestandsshare.
   
    ![Schermafbeelding van het uploaden en downloaden van bestanden vanuit de portal](./media/storage-dotnet-how-to-use-files/files-upload-download-1.png)
2. Klik op **Uploaden** om de gebruikersinterface voor het uploaden van bestanden te openen.
   
    ![Schermafbeelding van het uploaden van bestanden vanuit de portal](./media/storage-dotnet-how-to-use-files/files-upload-download-2.png)
3. Klik met de rechtermuisknop op een bestand en kies **Downloaden** om het te downloaden naar een lokale bestemming.
   
    ![Schermafbeelding van het downloaden van een bestand vanuit de portal](./media/storage-dotnet-how-to-use-files/files-upload-download-3.png)

### <a name="manage-file-share"></a>Bestandsshare beheren
1. Klik op **Quotum** als u de grootte van de bestandsshare wilt wijzigen (maximaal 5120 GB).
   
    ![Schermafbeelding van de configuratie van het quotum van de bestandsshare](./media/storage-dotnet-how-to-use-files/files-manage-1.png)
2. Klik op **Verbinden** om de opdrachtregel op te halen voor het koppelen van de bestandsshare vanuit Windows.
   
    ![Schermafbeelding van het koppelen van de bestandsshare](./media/storage-dotnet-how-to-use-files/files-manage-2.png)
   
    ![Schermafbeelding van het koppelen van de bestandsshare](./media/storage-dotnet-how-to-use-files/files-manage-3.png)
   
   > [!TIP]
   > Als u de toegangssleutel voor opslag voor de koppeling wilt weten, klikt u in uw opslagaccount op **Instellingen** en vervolgens op **Toegangssleutels**.
   > 
   > 
   
    ![Schermafbeelding van het zoeken van de toegangssleutel voor het opslagaccount.](./media/storage-dotnet-how-to-use-files/files-manage-4.png)
   
    ![Schermafbeelding van het zoeken van de toegangssleutel voor het opslagaccount.](./media/storage-dotnet-how-to-use-files/files-manage-5.png)

## <a name="use-powershell-to-manage-a-file-share"></a>PowerShell gebruiken voor het beheren van een bestandsshare
U kunt ook Azure PowerShell gebruiken om bestandsshares te maken en te beheren.

### <a name="install-the-powershell-cmdlets-for-azure-storage"></a>PowerShell-cmdlets voor Azure Storage installeren
U bereidt het gebruik van PowerShell voor door de Azure PowerShell-cmdlets te downloaden en te installeren. Zie [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Azure PowerShell installeren en configureren) voor het installatiepunt en de installatie-instructies.

> [!NOTE]
> Het wordt aanbevolen om de meest recente Azure PowerShell-module te downloaden en te installeren, of hiernaar te upgraden.
> 
> 

Open een Azure PowerShell-venster door op **Start** te klikken en **Windows PowerShell** te typen. In het PowerShell-venster wordt de Azure Powershell-module voor u geladen.

### <a name="create-a-context-for-your-storage-account-and-key"></a>Een context maken voor uw opslagaccount en -sleutel
Nu gaat u de context van het opslagaccount maken. De context bestaat uit de naam en accountsleutel van het opslagaccount. Voor instructies voor het kopiëren van de accountsleutel uit de [Azure Portal](https://portal.azure.com) raadpleegt u [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys) (Toegangssleutels voor opslag weergeven en kopiëren).

Vervang `storage-account-name` en `storage-account-key` in het volgende voorbeeld door de naam en sleutel van uw opslagaccount.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

### <a name="create-a-new-file-share"></a>Een nieuwe bestandsshare maken
Maak vervolgens de nieuwe share met de naam `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

U hebt nu een bestandsshare in File Storage. Nu gaan we een map en een bestand toevoegen.

> [!IMPORTANT]
> De naam van de bestandsshare mag alleen uit kleine letters bestaan. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.
> 
> 

### <a name="create-a-directory-in-the-file-share"></a>Een map maken in de bestandsshare
Maak vervolgens een map in de share. In het volgende voorbeeld wordt de map `CustomLogs` genoemd.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

### <a name="upload-a-local-file-to-the-directory"></a>Een lokaal bestand uploaden naar de map
Upload nu een lokaal bestand naar de map. In het volgende voorbeeld wordt een bestand geüpload vanuit `C:\temp\Log1.txt`. Bewerk het bestandspad zo dat het wijst naar een geldig bestand op uw lokale computer.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

### <a name="list-the-files-in-the-directory"></a>De bestanden in de map weergeven
Als u het bestand in de map wilt zien, kunt u alle bestanden in de map weergeven. Deze opdracht retourneert de bestanden en eventuele submappen in de map CustomLogs.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile retourneert een lijst met bestanden en mappen voor elk mapobject dat daaraan is doorgegeven. "Get-AzureStorageFile -Share $s" retourneert een lijst met bestanden en mappen in de hoofdmap. Voor een lijst met bestanden in een submap moet u de submap doorgeven aan Get-AzureStorageFile. Het volgende gebeurt: het eerste deel van de opdracht, tot aan de pipe, retourneert een mapexemplaar van de submap CustomLogs. Daarna wordt dit doorgegeven via Get-AzureStorageFile, en worden de bestanden en mappen in CustomLogs geretourneerd.

### <a name="copy-files"></a>Bestanden kopiëren
Vanaf versie 0.9.7 van Azure PowerShell kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Hieronder ziet u hoe u deze kopieerbewerkingen uitvoert met PowerShell-cmdlets.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```

## <a name="mount-the-file-share"></a>De bestandsshare koppelen
Dankzij ondersteuning voor SMB 3.0 ondersteunt File Storage nu versleuteling en permanente ingangen vanuit SMB 3.0-clients. Ondersteuning voor versleuteling houdt in dat SMB 3.0-clients een bestandsshare vanaf elke locatie kunnen koppelen, onder andere vanaf:

* Een virtuele machine van Azure in dezelfde regio (wordt ook ondersteund door SMB 2.1)
* Een virtuele machine van Azure in een andere regio (alleen SMB 3.0)
* Een on-premises clienttoepassing (alleen SMB 3.0)

Wanneer een client File Storage opent, hangt de gebruikte SMB-versie af van de SMB-versie die door het besturingssysteem wordt ondersteund. In de onderstaande tabel vindt u een overzicht van de ondersteuning voor Windows-clients. Raadpleeg dit blog voor meer informatie over [SMB-versies](http://blogs.technet.com/b/josebda/archive/2013/10/02/windows-server-2012-r2-which-version-of-the-smb-protocol-smb-1-0-smb-2-0-smb-2-1-smb-3-0-or-smb-3-02-you-are-using.aspx).

| Windows-client | Ondersteunde SMB-versie |
|:--- |:--- |
| Windows 7 |SMB 2.1 |
| Windows Server 2008 R2 |SMB 2.1 |
| Windows 8 |SMB 3.0 |
| Windows Server 2012 |SMB 3.0 |
| Windows Server 2012 R2 |SMB 3.0 |
| Windows 10 |SMB 3.0 |

### <a name="mount-the-file-share-from-an-azure-virtual-machine-running-windows"></a>De bestandsshare koppelen vanaf een virtuele machine in Azure waarop Windows wordt uitgevoerd
Als voorbeeld van het koppelen van een Azure-bestandsshare maken we nu een virtuele machine in Azure waarop Windows wordt uitgevoerd en maken we op afstand verbinding om de share te koppelen.

1. Maak eerst een nieuwe virtuele machine in Azure door de instructies te volgen in [Create a Windows virtual machine in the Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Een virtuele machine voor Windows maken in Azure Portal).
2. Maak daarna op afstand verbinding met de virtuele machine door de instructies te volgen in [Log on to a Windows virtual machine using the Azure Portal](../virtual-machines/virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Aanmelden bij een virtuele machine met Windows met behulp van Azure Portal).
3. Open een PowerShell-venster op de virtuele machine.

### <a name="persist-your-storage-account-credentials-for-the-virtual-machine"></a>De referenties van het opslagaccount voor de virtuele machine persistent maken
Voordat u de bestandsshare kunt koppelen, moet u eerst de referenties van het opslagaccount op de virtuele machine persistent maken. Met deze stap kan Windows automatisch opnieuw verbinding maken met de bestandsshare wanneer de virtuele machine opnieuw wordt opgestart. Als u de accountreferenties persistent wilt maken, voert u de opdracht `cmdkey` uit vanuit het PowerShell-venster op de virtuele machine. Vervang `<storage-account-name>` door de naam van uw opslagaccount en `<storage-account-key>` door de sleutel van uw opslagaccount.

```
cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>
```

Windows maakt nu opnieuw verbinding met de bestandsshare wanneer de virtuele machine opnieuw wordt opgestart. U kunt controleren of de share opnieuw is verbonden door de opdracht `net use` uit te voeren vanuit een PowerShell-venster.

Referenties worden alleen persistent in de context waarin `cmdkey` wordt uitgevoerd. Als u een toepassing ontwikkelt die als een service wordt uitgevoerd, moet u ook de referenties in die context persistent maken.

### <a name="mount-the-file-share-using-the-persisted-credentials"></a>De bestandsshare koppelen met behulp van de persistente referenties 
Wanneer u een externe verbinding met de virtuele machine hebt, kunt u de opdracht `net use` met de volgende syntaxis uitvoeren om de bestandsshare te koppelen. Vervang `<storage-account-name>` door de naam van uw opslagaccount en `<share-name>` door de naam van de File Storage-share.

```
net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>

example :
net use z: \\samples.file.core.windows.net\logs
```

Omdat u de referenties van het opslagaccount in de vorige stap persistent hebt gemaakt, hoeft u ze niet op te geven met de opdracht `net use`. Als u uw referenties nog niet persistent hebt gemaakt, neem ze dan als een parameter op die wordt doorgegeven aan de opdracht `net use`, zoals wordt weergegeven in het volgende voorbeeld.

```
net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /u:<storage-account-name> <storage-account-key>

example :
net use z: \\samples.file.core.windows.net\logs /u:samples <storage-account-key>
```

Nu kunt u de File Storage-share, net zoals ieder ander station, gebruiken vanaf de virtuele machine. U kunt standaardbestandsopdrachten uitvoeren vanaf de opdrachtprompt, of de gekoppelde share en de inhoud ervan bekijken vanuit de Verkenner. Ook met behulp van de standaard Windows-API's voor bestands-I/O, zoals de API’s die worden verstrekt door de [System.IO-naamruimten](http://msdn.microsoft.com/library/gg145019.aspx) in het .NET Framework, kunt u code uitvoeren op de virtuele machine die toegang heeft tot de bestandsshare.

U kunt de bestandsshare ook koppelen vanuit een functie die wordt uitgevoerd in een Azure-cloudservice, door op afstand verbinding te maken met die functie.

### <a name="mount-the-file-share-from-an-on-premises-client-running-windows"></a>De bestandsshare koppelen vanuit een on-premises client waarop Windows wordt uitgevoerd
Als u de bestandsshare wilt koppelen vanuit een on-premises client, moet u eerst de volgende stappen uitvoeren:

* Installeer een versie van Windows die ondersteuning biedt voor SMB 3.0. Windows gebruikt SMB 3.0-versleuteling om gegevens veilig over te brengen tussen uw on-premises client en de Azure-bestandsshare in de cloud.
* Open internettoegang voor poort 445 (TCP uitgaand) in uw lokale netwerk, zoals door het SMB-protocol wordt vereist.

> [!NOTE]
> Sommige internetproviders blokkeren poort 445, dus misschien moet u contact opnemen met uw serviceprovider.
> 
> 

## <a name="develop-with-file-storage"></a>Ontwikkelen met File Storage
Voor het schrijven van code die File Storage aanroept, kunt u de opslagclientbibliotheken voor .NET en Java of de REST API van Azure Storage gebruiken. In het voorbeeld in deze sectie ziet u hoe u met een bestandsshare werkt door vanuit een eenvoudige consoletoepassing die wordt uitgevoerd op het bureaublad gebruik te maken van de [Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/mt347887.aspx).

### <a name="create-the-console-application-and-obtain-the-assembly"></a>De consoletoepassing maken en de assembly verkrijgen
Ga als volgt te werk om een nieuwe consoletoepassing te maken in Visual Studio en het NuGet-pakket te installeren dat de Azure Storage-clientbibliotheek bevat:

1. In Visual Studio kiest u **Bestand > Nieuw Project** en vervolgens kiest u **Windows > Consoletoepassing** uit de lijst met Visual C#-sjablonen.
2. Geef een naam op voor de consoletoepassing en klik op **OK**.
3. Wanneer het project is gemaakt, klikt u met de rechtermuisknop op het project in Solution Explorer en kiest u **NuGet-pakketten beheren**. Zoek online naar 'WindowsAzure.Storage' en klik op **Installeren** om de Azure Storage-clientbibliotheek voor .NET-pakketten en -afhankelijkheden te installeren.

In de codevoorbeelden in dit artikel wordt ook gebruikgemaakt van de [Configuration Manager-bibliotheek van Microsoft Azure](https://msdn.microsoft.com/library/azure/mt634646.aspx), om de opslagverbindingsreeks op te halen uit het bestand app.config in de consoletoepassing. Met Azure Configuration Manager kunt u de verbindingsreeks in runtime ophalen, ongeacht of de toepassing wordt uitgevoerd in Microsoft Azure of vanuit een bureaublad-, mobiele of webtoepassing.

U installeert het Azure Configuration Manager-pakket door in Solution Explorer met de rechtermuisknop op het project te klikken en **NuGet-pakketten beheren** te kiezen. Zoek online naar 'ConfigurationManager' en klik op **Installeren** om het pakket te installeren.

Het gebruik van Azure Configuration Manager is optioneel. U kunt ook een API gebruiken, zoals de [ConfigurationManager-klasse](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) van .NET Framework.

### <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Uw opslagaccountreferenties opslaan in het bestand app.config
Vervolgens slaat u uw referenties op in het bestand app.config van het project. Bewerk het bestand app.config zo dat het op het volgende voorbeeld lijkt, waarbij u `myaccount` vervangt door de naam van het opslagaccount, en `mykey` door de sleutel van het opslagaccount.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> De nieuwste versie van de Azure-opslagemulator biedt geen ondersteuning voor File Storage. De verbindingsreeks werkt alleen met File Storage als de reeks verwijst naar een Azure-opslagaccount in de cloud.
> 
> 

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen
Open het bestand `program.cs` in Solution Explorer en plaats de volgende naamruimtedeclaraties boven in het bestand.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage
using Microsoft.WindowsAzure.Storage.File; // Namespace for File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="access-the-file-share-programmatically"></a>Via een programma toegang krijgen tot de bestandsshare
Vervolgens voegt u de volgende code aan de methode `Main()` (na de hierboven weergegeven code) toe om de verbindingsreeks op te halen. Met deze code wordt een verwijzing opgehaald naar het bestand dat we eerder hebben gemaakt en wordt de inhoud ervan weergegeven in het consolevenster.

```csharp
// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Voer de consoletoepassing uit om de uitvoer te zien.

### <a name="set-the-maximum-size-for-a-file-share"></a>De maximale grootte voor een bestandsshare instellen
Vanaf versie 5.x van de Azure Storage-clientbibliotheek kunt u het quotum (de maximumgrootte) voor een bestandsshare instellen, in gigabytes. U kunt ook controleren hoeveel gegevens er momenteel zijn opgeslagen in de share.

Door het quotum voor een share in te stellen, kunt u de totale grootte instellen van de bestanden die zijn opgeslagen in de share. Als de totale grootte van de bestanden in de share het quotum voor de share overschrijdt, kunnen clients bestaande bestanden niet meer vergroten en geen nieuwe bestanden meer maken, tenzij deze bestanden leeg zijn.

In onderstaand voorbeeld ziet u hoe u het huidige gebruik voor een share controleert en een quotum voor de share instelt.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Een Shared Access Signature genereren voor een bestand of bestandsshare
Vanaf versie 5.x van de Azure Storage-clientbibliotheek kunt u een Shared Access Signature (SAS) genereren voor een bestandsshare of voor een afzonderlijk bestand. U kunt op een bestandsshare ook een beleid voor gedeelde toegang maken om handtekeningen voor gedeelde toegang te beheren. Het maken van een beleid voor gedeelde toegang wordt aanbevolen omdat dit een manier biedt om de SAS in te trekken als deze verdacht is.

In het volgende voorbeeld wordt een beleid voor gedeelde toegang gemaakt op een share en wordt dat beleid vervolgens gebruikt om de beperkingen te verstrekken voor een SAS voor een bestand in de share.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Voor meer informatie over het maken en gebruiken van handtekeningen voor gedeelde toegang, raadpleegt u [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Shared Access Signatures (SAS) gebruiken) en [Create and use a SAS with Blob storage](storage-dotnet-shared-access-signature-part-2.md) (Een SAS maken en gebruiken met Blob Storage).

### <a name="copy-files"></a>Bestanden kopiëren
Vanaf versie 5.x van de Azure Storage-clientbibliotheek kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. In de volgende secties ziet u hoe u deze kopieerbewerkingen uitvoert via een programma.

U kunt ook AzCopy gebruiken om een bestand naar een ander bestand te kopiëren of om een blob naar een bestand te kopiëren of omgekeerd. Zie [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md) (Gegevensoverdracht met het AzCopy-opdrachtregelprogramma).

> [!NOTE]
> Als u een blob kopieert naar een bestand of een bestand naar een blob, moet u een Shared Access Signature (SAS) gebruiken om het bronobject te verifiëren, zelfs als u binnen hetzelfde opslagaccount kopieert.
> 
> 

**Een bestand kopiëren naar een ander bestand**

In het volgende voorbeeld wordt een bestand gekopieerd naar een ander bestand in dezelfde share. Omdat met deze kopieerbewerking bestanden in hetzelfde opslagaccount worden gekopieerd, kunt u verificatie met gedeelde sleutel gebruiken om de kopie te maken.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Een bestand kopiëren naar een blob**

In het volgende voorbeeld wordt een bestand gemaakt en vervolgens gekopieerd naar een blob in hetzelfde opslagaccount. In het voorbeeld wordt een SAS voor het bronbestand gemaakt, die tijdens de kopieerbewerking door de service wordt gebruikt om toegang tot het bronbestand te verifiëren.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Op dezelfde manier kunt u een blob naar een bestand kopiëren. Als het bronobject een blob is, maakt u een SAS om tijdens de kopieerbewerking de toegang tot de blob te verifiëren.

## <a name="troubleshooting-file-storage-using-metrics"></a>Problemen met File Storage oplossen met metrische gegevens
Azure Storage Analytics ondersteunt nu metrische gegevens voor File Storage. Met metrische gegevens kunt u aanvragen volgen en problemen diagnosticeren.

U kunt metrische gegevens voor File Storage inschakelen vanuit de [Azure Portal](https://portal.azure.com). U kunt metrische gegevens ook inschakelen via een programma. Daarvoor roept u de bewerking Set File Service Properties aan via de REST API of een van de analogen daarvan in de Storage-clientbibliotheek.

In het volgende codevoorbeeld ziet u hoe u de Storage-clientbibliotheek voor .NET gebruikt om metrische gegevens in te schakelen voor File Storage.

Voeg eerst de volgende `using`-instructies toe aan de reeds eerder vermelde instructies in het bestand program.cs:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Blob, Table en Queue Storage gebruiken het gedeelde type `ServiceProperties` in de `Microsoft.WindowsAzure.Storage.Shared.Protocol`-naamruimte. File Storage heeft echter een eigen type, het type `FileServiceProperties`, in de `Microsoft.WindowsAzure.Storage.File.Protocol`-naamruimte. De code kan echter alleen worden gecompileerd als er vanuit uw code naar beide naamruimten wordt verwezen.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read the metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

U kunt ook het [artikel voor het oplossen van problemen met Azure-bestanden](storage-troubleshoot-file-connection-problems.md) raadplegen voor richtlijnen voor end-to-end-probleemoplossing. 

## <a name="file-storage-faq"></a>Veelgestelde vragen over File Storage
1. **Wordt verificatie op basis van Active Directory door File Storage ondersteund?**
   
    Op dit moment wordt verificatie op basis van AD of ACL's nog niet ondersteund, maar deze mogelijkheid staat wel in onze lijst met functieaanvragen. Op dit moment worden Azure Storage-accountsleutels gebruikt voor verificatie naar de bestandsshare. We bieden een tijdelijke oplossing met Shared Access Signatures (SAS) via de REST API of de clientbibliotheken. Met SAS kunt u tokens met specifieke machtigingen genereren die geldig zijn gedurende een bepaald tijdsinterval. U kunt bijvoorbeeld een token genereren met alleen-lezen toegang tot een bepaald bestand. Iedereen die over dit token beschikt gedurende de tijd dat het geldig is, heeft alleen-lezen toegang tot dit bestand.
   
    SAS wordt alleen ondersteund via de REST API of clientbibliotheken. Wanneer u de bestandsshare koppelt via het SMB-protocol, kunt u geen SAS gebruiken om toegang tot de inhoud ervan te delegeren.
2. **Zijn Azure-bestandsshares publiekelijk zichtbaar via internet of zijn ze alleen toegankelijk vanuit Azure?**
   
    Zolang poort 445 (TCP uitgaand) is geopend en de client het SMB 3.0-protocol ondersteunt (*bijvoorbeeld* Windows 8 of Windows Server 2012), is uw bestandsshare beschikbaar via internet.  
3. **Telt het netwerkverkeer tussen een virtuele machine van Azure en een bestandsshare mee als externe bandbreedte die wordt verrekend met het abonnement?**
   
    Als de bestandsshare en de virtuele machine zich in verschillende regio's bevinden, wordt het verkeer tussen hen in rekening gebracht als externe bandbreedte.
4. **Is het netwerkverkeer gratis tussen een virtuele machine en een bestandsshare in dezelfde regio?**
   
    Ja. Het is gratis als het verkeer plaatsvindt in dezelfde regio.
5. **Is de verbinding vanaf on-premises virtuele machines met Azure File Storage afhankelijk van Azure ExpressRoute?**
   
    Nee. Als u niet over ExpressRoute beschikt, hebt u nog steeds toegang tot de bestandsshare vanaf een on-premises computer, zolang poort 445 (TCP uitgaand) is geopend voor internettoegang. Indien gewenst kunt u ExpressRoute echter met File Storage gebruiken.
6. **Is een 'File Share Witness' voor een failovercluster een van de toepassingen voor Azure File Storage?**
   
    Dit wordt momenteel niet ondersteund.
7. **File Storage wordt op dit moment toch alleen gerepliceerd via LRS of GRS?**  
   
    We willen RA-GRS ook gaan ondersteunen, maar we kunnen nog niet zeggen op welke termijn dit gaat gebeuren.
8. **Wanneer kan ik bestaande opslagaccounts gebruiken voor Azure File Storage?**
   
    Azure File Storage is nu ingeschakeld voor alle opslagaccounts.
9. **Wordt een naamswijziging ook toegevoegd aan de REST API?**
   
    Naamswijzigingen worden nog niet ondersteund in onze REST API.
10. **Zijn geneste shares mogelijk, dus een share onder een share?**
    
    Nee. De bestandsshare is het virtuele stuurprogramma dat u kunt koppelen. Geneste shares worden dus niet ondersteund.
11. **Is het mogelijk om alleen-lezen of alleen-schrijven machtigingen op te geven voor mappen in de share?**
    
    Dit niveau van controle over machtigingen is niet mogelijk wanneer u de bestandsshare koppelt via SMB. U kunt dit echter wel bereiken door een Shared Access Signature (SAS) te maken via de REST API of clientbibliotheken.  
12. **Het uitpakken van bestanden in File Storage ging erg traag. Wat moet ik doen?**
    
    Als u veel bestanden wilt overbrengen naar File Storage, kunt u beter gebruikmaken van AzCopy, Azure Powershell (Windows) of de Azure CLI (Linux/Unix). Deze hulpprogramma's zijn geoptimaliseerd voor netwerkoverdracht.
13. **Er is een patch beschikbaar voor het oplossen van prestatieproblemen met Azure-bestanden**
    
    Het Windows-team heeft onlangs een patch uitgebracht voor een prestatieprobleem dat zich voordoet wanneer de klant Azure File Storage opent vanuit Windows 8.1 of Windows Server 2012 R2. Meer informatie vindt u in het bijbehorende KB-artikel [Slow performance when you access Azure Files Storage from Windows 8.1 or Server 2012 R2](https://support.microsoft.com/en-us/kb/3114025) (Prestatieproblemen wanneer u Azure File Storage opent vanuit Windows 8.1 of Server 2012 R2).
14. **Azure File Storage gebruiken met IBM MQ**
    
    IBM heeft een document uitgegeven om IBM MQ-klanten te helpen bij de configuratie van Azure File Storage met hun service. Voor meer informatie raadpleegt u [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service) (IBM MQ-wachtrijbeheer voor meerdere instanties instellen met Microsoft Azure File-service.
15. **Hoe kan ik fouten bij Azure File Storage oplossen?**
    
    U kunt het [artikel voor het oplossen van problemen met Azure-bestanden](storage-troubleshoot-file-connection-problems.md) raadplegen voor richtlijnen voor end-to-end-probleemoplossing.               

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure File Storage.

### <a name="conceptual-articles-and-videos"></a>Conceptuele artikelen en video's
* [Azure File Storage: een naadloos SMB-bestandssysteem voor Windows en Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure File Storage gebruiken met Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Hulpprogramma-ondersteuning voor File Storage
* [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md)
* [AzCopy gebruiken met Microsoft Azure Storage](storage-use-azcopy.md)
* [De Azure CLI gebruiken met Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="reference"></a>Naslaginformatie
* [Naslaginformatie over de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Naslaginformatie over REST API voor bestandsservices](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Blogberichten
* [Azure File storage is now generally available (Azure File Storage is nu algemeen beschikbaar)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage (Een kijkje achter de schermen van Azure File Storage)](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (Introductie van Microsoft Azure File-service)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Persisting connections to Microsoft Azure Files (Permanente verbindingen met Microsoft Azure-bestanden)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)


<!--HONumber=Nov16_HO4-->



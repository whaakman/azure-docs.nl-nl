<properties
   pageTitle="Data Lake Store .NET SDK gebruiken om toepassingen te ontwikkelen | Microsoft Azure"
   description="Azure Data Lake Store .NET SDK gebruiken om toepassingen te ontwikkelen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>


# Aan de slag met Azure Data Lake Store met .NET SDK

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Lees hoe u met de [Azure Data Lake Store .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) basisbewerkingen uitvoert, zoals het maken van mappen, het uploaden en downloaden van gegevensbestanden enzovoort. Zie [Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake.

## Vereisten

* **Visual Studio 2013 of 2015**. In onderstaande instructies wordt Visual Studio 2015 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store-account**. Zie voor instructies over het maken van een account [Aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)

* **Een Azure Active Directory-toepassing maken**. U gebruikt de Azure AD-toepassing om de Data Lake Store-toepassing te verifiëren in Azure AD. Er zijn verschillende manieren om te verifiëren in Azure AD, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Verifiëren met Data Lake Store met behulp van Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## Een .NET-toepassing maken

1. Open Visual Studio en maak een consoletoepassing.

2. Klik in het menu **File** op **New** en klik vervolgens op **Project**.

3. In **New Project** typt of selecteert u de volgende waarden:

  	| Eigenschap | Waarde                       |
  	|----------|-----------------------------|
  	| Category | Templates/Visual C#/Windows |
  	| Template | Console Application         |
  	| Name     | CreateADLApplication        |

4. Klik op **OK** om het project aan te maken.

5. Voeg de Nuget-pakketten toe aan het project.

    1. Klik in Solution Explorer met de rechtermuisknop op de projectnaam en klik op **Manage NuGet Packages**.
    2. Controleer op het tabblad **Nuget Package Manager** of **Package source** is ingesteld op **nuget.org** en of het selectievakje **Include prerelease** is ingeschakeld.
    3. Zoek en installeer de volgende NuGet-pakketten:

        * `Microsoft.Azure.Management.DataLake.Store` - In deze zelfstudie wordt gebruikgemaakt van v0.12.5-preview.
        * `Microsoft.Azure.Management.DataLake.StoreUploader` - In deze zelfstudie wordt gebruikgemaakt van v0.10.6-preview.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - In deze zelfstudie wordt gebruikgemaakt van v2.2.8-preview.

        ![Een Nuget-bron toevoegen](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. Sluit de **Nuget Package Manager**.

6. Open **Program.cs**, verwijder de bestaande code en neem de volgende instructies op om verwijzingen naar naamruimten toe te voegen.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Declareer de variabelen zoals hieronder weergegeven, en geef de waarden op voor de naam van de Data Lake Store en de naam van de resourcegroep die al bestaat. Ook het lokale pad en de bestandsnaam die u hier opgeeft, moeten al bestaan op de computer. Voeg het volgende codefragment toe na de naamruimtedeclaraties.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

In de rest van het artikel ziet u het gebruik van de beschikbare .NET-methoden voor het uitvoeren van bewerkingen, zoals verificatie, het uploaden van bestanden enzovoort.

## Authentication

### Als u gebruikmaakt van verificatie door eindgebruikers (aanbevolen voor deze zelfstudie)

Gebruik dit met een bestaande systeemeigen Azure AD-clienttoepassing. Hieronder wordt er u een aangeboden. Door voor deze aanpak te kiezen, kunt u deze zelfstudie sneller voltooien.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Een aantal dingen die u moet weten over het bovenstaande fragment.

* Omdat dit fragment gebruikmaakt van een Azure AD-domein en -client-id die standaard beschikbaar zijn voor alle Azure-abonnementen, kunt u deze zelfstudie sneller voltooien. U kunt **dit fragment dus in zijn huidige vorm in uw toepassing gebruiken**.
* Als u echter uw eigen Azure AD-domein- en toepassingsclient-id wilt gebruiken, moet u een systeemeigen Azure AD-toepassing maken en vervolgens het Azure AD-domein, de client-id en omleidings-URI gebruiken voor de toepassing die u hebt gemaakt. Zie [Een Active Directory-toepassing maken](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) voor instructies.

>[AZURE.NOTE] De instructies in de bovenstaande koppelingen zijn voor een Azure AD-webtoepassing. De stappen zijn echter precies hetzelfde, ook als u in plaats daarvan een systeemeigen clienttoepassing maakt. 

### Als u gebruikmaakt van service-naar-serviceverificatie met clientgeheim 

U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim of de clientsleutel voor een toepassing/service-principal. Gebruik dit met een bestaande [Azure AD-toepassing voor webtoepassingen](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### Als u gebruikmaakt van service-naar-serviceverificatie met certificaat

Een derde mogelijkheid is door het volgende codefragment te gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het certificaat van een toepassing/service-principal. Gebruik dit met een bestaande [Azure AD-toepassing voor webtoepassingen](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## Clientobjecten maken

Met het volgende codefragment worden het Data Lake Store-account en clientobjecten voor het bestandssysteem gemaakt. Deze worden gebruikt voor het verzenden van aanvragen naar de service.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## Alle Data Lake Store-accounts binnen een abonnement weergeven

Het volgende codefragment bevat alle Data Lake Store-accounts binnen een bepaald Azure-abonnement.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## Een map maken

Het volgende codefragment bevat de methode `CreateDirectory`, die u kunt gebruiken voor het maken van een map in een Date Lake Store-account.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## Bestand uploaden

Het volgende codefragment bevat de methode `UploadFile`, die u kunt gebruiken voor het uploaden van bestanden naar een Date Lake Store-account.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader` Ondersteunt recursief uploaden en downloaden tussen een lokaal bestandspad en een Date Lake Store-bestandspad.    

## Bestands- of mapinformatie ophalen

Het volgende codefragment bevat de methode `GetItemInfo`, die u kunt gebruiken voor het ophalen van bestands- of mapinformatie die beschikbaar is in Data Lake Store. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## Bestanden of mappen weergeven

Het volgende codefragment bevat de methode `ListItem`, die u kunt gebruiken voor het weergeven van de bestanden en mappen in een Date Lake Store-account.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## Bestanden samenvoegen

Het volgende codefragment bevat de methode `ConcatenateFiles`, die u kunt gebruiken voor het samenvoegen van bestanden. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## Toevoegen aan een bestand

Het volgende codefragment bevat de methode `AppendToFile`, die u kunt gebruiken voor het toevoegen van gegevens aan een bestand dat al is opgeslagen in een Date Lake Store-account.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## Bestand downloaden

Het volgende codefragment bevat de methode `DownloadFile`, die u kunt gebruiken voor downloaden van een bestand in een Date Lake Store-account.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## Volgende stappen

- [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Naslaginformatie over Data Lake Store .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx)
- [Naslaginformatie over Data Lake Store REST](https://msdn.microsoft.com/library/mt693424.aspx)



<!--HONumber=Sep16_HO5-->



<properties 
   pageTitle="Aan de slag met Data Lake Analytics met .NET SDK | Azure" 
   description="Informatie over het gebruik van de SDK voor .NET voor het maken van Data Lake Store-accounts, het maken van Data Lake Analytics-taken en het verzenden van taken die zijn geschreven in U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/22/2016"
   ms.author="edmaca"/>


# Zelfstudie: Aan de slag met Data Lake Analytics met .NET SDK

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Informatie over het gebruik van de Azure .NET SDK voor het maken van Azure Data Lake Analytics-accounts, het definiëren van Data Lake Analytics-taken in [U-SQL](data-lake-analytics-u-sql-get-started.md), en het verzenden van taken naar Data Lake Analytics-accounts. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

In deze zelfstudie gaat u een C#-consoletoepassing ontwikkelen, die een U-SQL-script bevat dat een bestand met door tabs gescheiden waarden (TSV) leest en converteert naar een bestand met door komma's gescheiden waarden (CSV). Om de zelfstudie te volgen met andere ondersteunde hulpprogramma’s klikt u op de tabbladen boven aan deze sectie.

##Vereisten

Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

- **Visual Studio 2015, Visual Studio 2013 update 4 of Visual Studio 2012 met Visual C++ geïnstalleerd**.
- **Microsoft Azure SDK voor .NET versie 2.5 of hoger**.  U kunt dit installeren met het [Webplatforminstallatieprogramma](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs)**. 
- Maak een Azure Active Directory (AAD)-toepassing en haal de **Client-ID**, **Tenant-ID**, en **sleutel** ervan op. Zie [Create Active Directory application and service principal using portal](../resource-group-create-service-principal-portal.md) (Een Active Directory-toepassing en service-principal maken met de portal) voor meer informatie over AAD-toepassingen en instructies voor het verkrijgen van een client-ID. Zodra u de toepassing hebt gemaakt en de sleutel hebt gegenereerd, zullen de antwoord-URI en sleutel ook beschikbaar zijn vanuit de portal.


##Consoletoepassing maken

In deze zelfstudie verwerkt u een aantal zoeklogboeken.  Het zoeklogboek kan worden opgeslagen in de Data Lake Store of Azure Blob-opslag. 

Een voorbeeld van een zoeklogboek is gekopieerd naar een openbare Azure Blob-container. In de toepassing wordt het bestand naar uw werkstation gedownload en vervolgens geüpload naar het Data Lake Store-standaardaccount.

**Een toepassing maken**

1. Open Visual Studio.
2. Maak een C#-consoletoepassing.
3. Open de NuGet Package Management-console en voer de volgende opdrachten uit:

        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
        Install-Package WindowsAzure.Storage

4. Voeg een nieuw bestand genaamd **SampleUSQLScript.txt** toe aan het project en plak het volgende U-SQL-script. Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) voor meer informatie over U-SQL.

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dit U-SQL-script leest het brongegevensbestand met **Extractors.Tsv()** en maakt vervolgens een CSV-bestand met **Outputters.Csv()**. 
    
    In het C#-programma moet u het bestand **/Samples/Data/SearchLog.tsv** en de map **/Output/** voorbereiden.    
    
    Het is eenvoudiger om relatieve paden te gebruiken voor bestanden die zijn opgeslagen in Data Lake-standaardaccounts. Maar u kunt ook absolute paden gebruiken.  Bijvoorbeeld 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    U dient absolute paden te gebruiken om toegang te krijgen tot bestanden in gekoppelde Storage-accounts.  De syntaxis voor bestanden die zijn opgeslagen in het gekoppelde Azure Storage-account is:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Er is een bekend probleem met de Azure Data Lake-service.  Als de voorbeeldapp wordt onderbroken of als er een fout in optreedt, moet u de Data Lake Store- & Data Lake Analytics-accounts die door het script worden gemaakt mogelijk handmatig verwijderen.  Als u niet vertrouwd bent met de Portal, leest u eerst [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voordat u aan de slag gaat.       
       
5. Plak de volgende code in Program.cs:

        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System;
        using System.Collections.Generic;
        using System.IO;
        
        namespace SdkSample
        {
          class Program
          {
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            private static string _adlaAccountName;
            private static string _adlsAccountName;
            private static string _resourceGroupName;
            private static string _location;
            private static string _tenantId;
            private static string _subId;
            private static string _clientId;
            private static string _clientKey;
        
            private static void Main(string[] args)
            {
              _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
              _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>"; // TODO: Replace this value with the name for a NEW Analytics account.
              _resourceGroupName = "<RESOURCE-GROUP>"; // TODO: Replace this value. This resource group should already exist.
              _location = "East US 2";
              _tenantId = "<TENANT-ID>";
              _subId = "<SUBSCRIPTION-ID>";
              _clientId = "<CLIENT-ID>";
              _clientKey = "<CLIENT-KEY>";
        
              string localFolderPath = @"c:\temp\"; // TODO: Make sure this exists and contains SampleUSQLScript.txt.
              var tokenCreds = Authenticate(_tenantId, _clientId, _clientKey);
        
              SetupClients(tokenCreds, _subId); 
        
              // Run sample scenarios
              WaitForNewline("Authenticated.", "Creating NEW accounts.");
              CreateAccounts();
              WaitForNewline("Accounts created.", "Preparing the source data file.");
        
              // Transfer the source file from a public Azure Blob container to Data Lake Store.
              CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
              blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
              UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
              WaitForNewline("Source data file prepared.", "Submitting a job.");
        
              // Submit the job
              Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
              WaitForNewline("Job submitted.", "Waiting for job completion.");
        
              // Wait for job completion
              WaitForJob(jobId);
              WaitForNewline("Job completed.", "Downloading job output.");
        
              // Download job output
              DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
              WaitForNewline("Job output downloaded.", "Deleting accounts.");
        
              // Delete accounts
              _adlaClient.Account.Delete(_resourceGroupName, _adlaAccountName);
              _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
        
              WaitForNewline("Accounts deleted. You can now exit.");
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
              Console.WriteLine(reason + "\r\nPress ENTER to continue...");
        
              Console.ReadLine();
        
              if (!String.IsNullOrWhiteSpace(nextAction))
                Console.WriteLine(nextAction);
            }
        
            public static TokenCredentials Authenticate(string tenantId, string clientId, string clientKey)
            {
              var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + _tenantId);
              var creds = new ClientCredential(_clientId, _clientKey);
              var tokenAuthResult = authContext.AcquireTokenAsync("https://management.core.windows.net/", creds).Result;
        
              return new TokenCredentials(tokenAuthResult.AccessToken);
            }
        
            public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
            {
              _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
              _adlaClient.SubscriptionId = subscriptionId;
        
              _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);
        
              _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(tokenCreds);
        
              _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
              _adlsClient.SubscriptionId = subscriptionId;
        
              _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }
        
            public static void CreateAccounts()
            {
              //ADLS account first, ADLA requires an ADLS account
              var adlsParameters = new DataLakeStoreAccount(location: _location);
              _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
              var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(_adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
              var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: _adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
              var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: _location);
              _adlaClient.Account.Create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
              var script = File.ReadAllText(scriptPath);
        
              var jobId = Guid.NewGuid();
              var properties = new USqlJobProperties(script);
              var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
              var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);
        
              return jobId;
            }
        
            public static JobResult WaitForJob(Guid jobId)
            {
              var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
              while (jobInfo.State != JobState.Ended)
              {
                jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
              }
              return jobInfo.Result.Value;
            }
        
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
              var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
              var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
              var uploader = new DataLakeStoreUploader(parameters, frontend);
              uploader.Execute();
            }
        
            public static void DownloadFile(string srcPath, string destPath)
            {
              var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
              var fileStream = new FileStream(destPath, FileMode.Create);
        
              stream.CopyTo(fileStream);
              fileStream.Close();
              stream.Close();
            }
          }
        }

6. Druk op **F5** om de toepassing uit te voeren.

## Zie ook

- Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
- Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
- Zie [U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) om aan de slag te gaan met het ontwikkelen van U-SQL-toepassingen.
- Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) om U-SQL te leren.
- Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.
- Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor een overzicht van Data Lake Analytics.



<!--HONumber=Sep16_HO3-->



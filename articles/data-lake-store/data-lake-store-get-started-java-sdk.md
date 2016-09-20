<properties
   pageTitle="Data Lake Store Java SDK gebruiken om toepassingen te ontwikkelen | Azure"
   description="Data Lake Store Java SDK gebruiken om toepassingen te ontwikkelen"
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
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Aan de slag met Azure Data Lake Store met Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Lees hoe u met de Azure Data Lake Store Java SDK een Azure Data Lake-account maakt en basisbewerkingen uitvoert. U kunt onder andere mappen maken, gegevensbestanden uploaden en downloaden en uw account verwijderen. Zie [Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake.

## Azure Data Lake Store Java SDK

Volg deze koppelingen voor de downloadlocatie voor de Java SDK voor Data Lake Store en naslaginformatie over de Java SDK. Voor deze zelfstudie hoeft u de SDK niet te downloaden en het referentiedocument niet te volgen. Deze koppelingen zijn uitsluitend ter informatie.

* De broncode voor de Java SDK voor Data Lake Store is beschikbaar op [GitHub](https://github.com/Azure/azure-sdk-for-java).
* Naslaginformatie over Java SDK voor Data Lake Store is beschikbaar op [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/).

## Vereisten

* Java Development Kit (JDK) 8 (met Java-versie 1.8).
* IntelliJ of een andere geschikte Java-ontwikkelomgeving. Dit is optioneel, maar wordt wel aanbevolen. In onderstaande instructies wordt IntelliJ gebruikt.
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Active Directory-toepassing maken**. Er zijn twee manieren waarop u kunt verifiëren met behulp van Azure Active Directory: **interactief** en **niet-interactief**. De vereisten zijn afhankelijk van de manier waarop u wilt verifiëren.
    * **Voor interactieve verificatie** moet u in Azure Active Directory een **Native Client-toepassing** maken. Wanneer u de toepassing hebt gemaakt, haalt u de volgende waarden op met betrekking tot de toepassing.
        - **Client-id** en **omleidings-URI** voor de toepassing ophalen
        - Gedelegeerde machtigingen instellen

    * **Voor niet-interactieve verificatie** (dat in dit artikel wordt gebruikt) moet u in Azure Active Directory een **webtoepassing** maken. Wanneer u de toepassing hebt gemaakt, haalt u de volgende waarden op met betrekking tot de toepassing.
        - **Client-id**, **clientgeheim** en **omleidings-URI** voor de toepassing ophalen
        - Gedelegeerde machtigingen instellen
        - Wijs de Azure Active Directory-toepassing toe aan een rol. De rol kan zich bevinden op het niveau van het bereik waarmee u toegang wilt geven aan de Azure Active Directory-toepassing. U kunt de toepassing bijvoorbeeld toewijzen op abonnementsniveau of op het niveau van een resourcegroep.

    Zie [Een Active Directory-toepassing en service-principal maken met portal](../resource-group-create-service-principal-portal.md) als u wilt weten hoe u deze waarden ophaalt, de machtigingen instelt en rollen toewijst.

## Hoe verifieer ik met Azure Active Directory?

In onderstaand codefragment vindt u code voor **niet-interactieve** verificatie, waarbij de toepassing zijn eigen referenties verstrekt.

Deze zelfstudie werkt alleen wanneer u uw toepassing machtigt om resources te maken in Azure. Het wordt **ten zeerste aangeraden** dat u deze toepassing voor het doel van deze zelfstudie alleen Inzender-rechten geeft voor een nieuwe, ongebruikte en lege resourcegroep in uw Azure-abonnement.

## Een Java-toepassing maken

1. Open IntelliJ en maak een nieuw Java-project met de **Command Line Application**-sjabloon. Voltooi de wizard om het project te maken.

2. Ga naar **Bestand** -> **Projectstructuur** -> **Modules** (onder Projectinstellingen) -> **Afhankelijkheden** -> **+** -> **Bibliotheek** -> **In Maven**.

3. Zoek naar de volgende Maven-pakketten en voeg deze toe aan uw project:

    * com.microsoft.azure:azure-mgmt-datalake-store:1.0.0-beta1.2
    * com.microsoft.azure:azure-mgmt-datalake-store-uploader:1.0.0-beta1.2
    * com.microsoft.azure:azure-client-authentication:1.0.0-beta2

4. Ga vanuit het linkerdeelvenster naar **src**, **main**, **java**, **\<pakketnaam>** en open **Main.java**. Vervang het bestaande codeblok door de volgende code. Geef ook de waarden op voor parameters die in het codefragment worden genoemd, zoals **localFolderPath**, **DATA-LAKE-STORE-NAME**, **RESOURCE-GROUP-NAME** en vervang de tijdelijke aanduidingen voor **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** en **SUBSCRIPTION-ID** met de gegevens over uw abonnement en de bijbehorende Azure Active Directory. Zie [de Azure-handleiding voor het maken van service-principals](../resource-group-authenticate-service-principal.md) voor meer informatie over het vinden van deze gegevens.

    Deze code doorloopt het proces waarin een Data Lake Store-account wordt gemaakt, bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en het account tot slot wordt verwijderd.

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreAccountManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.implementation.DataLakeStoreFileSystemManagementClientImpl;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.store.uploader.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;

        public class Main {
            final static String ADLS_ACCOUNT_NAME = <DATA-LAKE-STORE-NAME>;
            final static String RESOURCE_GROUP_NAME = "<RESOURCE-GROUP-NAME>";
            final static String LOCATION = "East US 2";
            final static String TENANT_ID = "<TENANT-ID>";
            final static String SUBSCRIPTION_ID =  "<SUBSCRIPTION-ID>";
            final static String CLIENT_ID = "<CLIENT-ID>";
            final static String CLIENT_SECRET = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

            private static DataLakeStoreAccountManagementClientImpl _adlsClient;
            private static DataLakeStoreFileSystemManagementClientImpl _adlsFileSystemClient;

            public static void main(String[] args) throws Exception {
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(CLIENT_ID, TENANT_ID, CLIENT_SECRET, null);
                SetupClients(creds);

                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");

                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", SUBSCRIPTION_ID));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.accounts().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.name());
                }
                WaitForNewline("Account(s) displayed.", "Uploading file.");

                // Upload a file to Data Lake Store: file1.csv
                UploadFile(localFolderPath + "file1.csv", "/file1.csv");
                WaitForNewline("File uploaded.", "Appending newline.");

                // Append newline to file1.csv
                AppendToFile("/file1.csv", "\r\n");
                WaitForNewline("Newline appended.", "Creating file.");

                // Create a new file in Data Lake Store: file2.csv
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("File created.", "Concatenating files.");

                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");

                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlsClient.withSubscriptionId(SUBSCRIPTION_ID);
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }

            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.withLocation(LOCATION);

                _adlsClient.accounts().create(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME, adlsParameters);
            }

            // Create file
            public static void CreateFile(String path) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path);
            }

            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().create(ADLS_ACCOUNT_NAME, path, bytesContents, force);
            }

            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, AdlsErrorException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.fileSystems().append(ADLS_ACCOUNT_NAME, path, bytesContents);
            }

            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().concat(ADLS_ACCOUNT_NAME, destFilePath, srcFilePaths);
            }

            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, AdlsErrorException {
                _adlsFileSystemClient.fileSystems().delete(ADLS_ACCOUNT_NAME, filePath);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().getFileStatus(ADLS_ACCOUNT_NAME, path).getBody().fileStatus();
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, AdlsErrorException {
                return _adlsFileSystemClient.fileSystems().listFileStatus(ADLS_ACCOUNT_NAME, directoryPath).getBody().fileStatuses().fileStatus();
            }

            // Upload file
            public static void UploadFile(String srcPath, String destPath) throws Exception {
                UploadParameters parameters = new UploadParameters(srcPath, destPath, ADLS_ACCOUNT_NAME);
                FrontEndAdapter frontend = new DataLakeStoreFrontEndAdapterImpl(ADLS_ACCOUNT_NAME, _adlsFileSystemClient);
                DataLakeStoreUploader uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.execute();
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, AdlsErrorException {
                InputStream stream = _adlsFileSystemClient.fileSystems().open(ADLS_ACCOUNT_NAME, srcPath).getBody();

                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();

                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }

                pWriter.println(fileContents);
                pWriter.close();
            }

            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.accounts().delete(RESOURCE_GROUP_NAME, ADLS_ACCOUNT_NAME);
            }
        }


6. Voer de toepassing uit. Volg de aanwijzingen om de toepassing uit te voeren en te voltooien.

## Volgende stappen

- [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=sep16_HO2-->



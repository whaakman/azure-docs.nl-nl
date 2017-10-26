---
title: '.NET SDK: Bestandssysteembewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Gebruik Azure Data Lake Store .NET SDK om bestandssysteembewerkingen uit te voeren op Data Lake Store, bijvoorbeeld om mappen te maken enzovoort.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: nitinme
ms.openlocfilehash: 7f6319dcf1ae66a686dd1c2ea2810b3041183098
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Bestandssysteembewerkingen in Azure Data Lake Store met behulp van .NET SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

In dit artikel leert u hoe u .NET SDK gebruikt voor het uitvoeren van bestandssysteembewerkingen in Data Lake Store. Voorbeelden van bestandssysteembewerkingen zijn het maken van mappen in een Data Lake Store-account, het uploaden van bestanden, het downloaden van bestanden, enzovoort.

Als u wilt weten hoe u .NET SDK gebruikt voor het uitvoeren van accountbeheerbewerkingen in Data Lake Store, raadpleegt u [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013, 2015 of 2017**. In onderstaande instructies wordt Visual Studio 2017 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store-account**. Zie voor instructies over het maken van een account [Aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Een .NET-toepassing maken
1. Open Visual Studio en maak een consoletoepassing.
2. Klik in het menu **File** op **New** en klik vervolgens op **Project**.
3. In **New Project** typt of selecteert u de volgende waarden:

   | Eigenschap | Waarde |
   | --- | --- |
   | Category |Templates/Visual C#/Windows |
   | Template |Console Application |
   | Name |CreateADLApplication |
4. Klik op **OK** om het project te maken.
5. Voeg de NuGet-pakketten toe aan het project.

   1. Klik in Solution Explorer met de rechtermuisknop op de projectnaam en klik op **Manage NuGet Packages**.
   2. Controleer op het tabblad **NuGet Package Manager** of **Package source** is ingesteld op **nuget.org** en of het selectievakje **Include prerelease** is ingeschakeld.
   3. Zoek en installeer de volgende NuGet-pakketten:

      * `Microsoft.Azure.Management.DataLake.Store`: in deze zelfstudie wordt gebruikgemaakt van v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: in deze zelfstudie wordt gebruikgemaakt van v2.2.12.

        ![Een NuGet-bron toevoegen](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Een nieuw Azure Data Lake-account maken")
   4. Sluit de **NuGet Package Manager**.
6. Open **Program.cs**, verwijder de bestaande code en neem de volgende instructies op om verwijzingen naar naamruimten toe te voegen.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Declareer de variabelen zoals hieronder wordt weergegeven en vervang de tijdelijke aanduidingen door waarden. Zorg er ook voor dat het lokale pad en de bestandsnaam die u hier opgeeft al bestaan op de computer.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static async void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

In de rest van het artikel ziet u het gebruik van de beschikbare .NET-methoden voor het uitvoeren van bewerkingen, zoals verificatie, het uploaden van bestanden enzovoort.

## <a name="authentication"></a>Authentication

* Zie [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Eindgebruikersverificatie met Data Lake Store met behulp van .NET SDK) voor de verificatie van eindgebruikers voor uw toepassing.
* Zie [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van .NET SDK) voor service-naar-serviceverificatie voor uw toepassing.


## <a name="create-client-objects"></a>Clientobjecten maken
Met het volgende codefragment worden het Data Lake Store-account en clientobjecten voor het bestandssysteem gemaakt. Deze worden gebruikt voor het verzenden van aanvragen naar de service.

    // Create client objects
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

## <a name="create-a-directory"></a>Een map maken
Voeg in volgende methode toe aan de klasse. Het codefragment bevat de methode `CreateDirectory()`, die u kunt gebruiken voor het maken van een map in een Date Lake Store-account.

    // Create a directory
    public static void CreateDirectory(string path)
    {
            _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `CreateDirectory()` aan te roepen.

    CreateDirectory(remoteFolderPath);
    Console.WriteLine("Created a directory in the Data Lake Store account. Press any key to continue ...");
    Console.ReadLine();

## <a name="upload-a-file"></a>Bestand uploaden
Voeg in volgende methode toe aan de klasse. Het codefragment toont de methode `UploadFile()`, die u kunt gebruiken voor het uploaden van bestanden naar een Date Lake Store-account. De SDK ondersteunt recursief uploaden en downloaden tussen een lokaal bestandspad en een Data Lake Store-bestandspad.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `UploadFile()` aan te roepen.

    UploadFile(localFilePath, remoteFilePath, true);
    Console.WriteLine("Uploaded file in the directory. Press any key to continue...");
    Console.ReadLine();
    

## <a name="get-file-or-directory-info"></a>Bestands- of mapinformatie ophalen
Het volgende codefragment bevat de methode `GetItemInfo()`, die u kunt gebruiken voor het ophalen van bestands- of mapinformatie die beschikbaar is in Data Lake Store.

    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `GetItemInfo()` aan te roepen.

    
    var fileProperties = GetItemInfo(remoteFilePath);
    Console.WriteLine("The owner of the file at the path is:", fileProperties.Owner);
    Console.WriteLine("Press any key to continue...");
    Console.ReadLine();

## <a name="list-file-or-directories"></a>Bestanden of mappen weergeven
Het volgende codefragment bevat de methode `ListItems()`, die u kunt gebruiken voor het weergeven van de bestanden en mappen in een Date Lake Store-account.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `ListItems()` aan te roepen.

    var itemList = ListItems(remoteFolderPath);
    var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
    Console.WriteLine(String.Join("\r\n", fileMenuItems));
    Console.WriteLine("Files and directories listed. Press any key to continue ...");
    Console.ReadLine();

## <a name="concatenate-files"></a>Bestanden samenvoegen
Het volgende codefragment bevat de methode `ConcatenateFiles()`, die u kunt gebruiken voor het samenvoegen van bestanden.

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `ConcatenateFiles()` aan te roepen. In dit fragment wordt ervan uitgegaan dat u nog een bestand hebt geüpload naar het Data Lake Store-account en dat het pad van het bestand is opgegeven in de tekenreeks *anotherRemoteFilePath*.

    string[] stringOfFiles = new String[] {remoteFilePath, anotherRemoteFilePath};
    string destFilePath = Path.Combine(remoteFolderPath, "Concatfile.txt");
    ConcatenateFiles(stringOfFiles, destFilePath);
    Console.WriteLine("Files concatinated. Press any key to continue ...");
    Console.ReadLine();

## <a name="append-to-a-file"></a>Toevoegen aan een bestand
Het volgende codefragment bevat de methode `AppendToFile()`, die u kunt gebruiken voor het toevoegen van gegevens aan een bestand dat al is opgeslagen in een Date Lake Store-account.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `AppendToFile()` aan te roepen.

    AppendToFile(remoteFilePath, "123");
    Console.WriteLine("Content appended. Press any key to continue ...");
    Console.ReadLine();

## <a name="download-a-file"></a>Bestand downloaden
Het volgende codefragment bevat de methode `DownloadFile()`, die u kunt gebruiken voor downloaden van een bestand in een Date Lake Store-account.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:true);
    }

Voeg het volgende codefragment aan de methode `Main()` toe om methode `DownloadFile()` aan te roepen.

    DownloadFile(destFilePath, localFilePath);
    Console.WriteLine("File downloaded. Press any key to continue ...");
    Console.ReadLine();

## <a name="see-also"></a>Zie ook
* [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Naslaginformatie over Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

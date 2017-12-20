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
ms.date: 11/02/2017
ms.author: nitinme
ms.openlocfilehash: a5d446986f810993d65c7e73eb95eeb2283c39a3
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
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
Dit codevoorbeeld beschikbaar [in GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) doorloopt het proces waarin bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en een aantal bestanden uit het archief wordt verwijderd. In dit gedeelte van het artikel komen de belangrijkste onderdelen van de code aan bod.

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

      * `Microsoft.Azure.DataLake.Store`: in deze zelfstudie wordt versie 1.0.0 gebruikt.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: in deze zelfstudie wordt versie 2.3.1 gebruikt.
    
    Sluit de **NuGet Package Manager**.

6. Open **Program.cs**, verwijder de bestaande code en neem de volgende instructies op om verwijzingen naar naamruimten toe te voegen.

        using System;
        using System.IO;using System.Threading;
        using System.Linq;
        using System.Text;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.DataLake.Store;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Declareer de variabelen zoals hieronder wordt weergegeven en vervang de tijdelijke aanduidingen door waarden. Zorg er ook voor dat het lokale pad en de bestandsnaam die u hier opgeeft al bestaan op de computer.

        namespace SdkSample
        {
            class Program
            {
                private static string _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; //Replace this value with the name of your existing Data Lake Store account.        
            }
        }

In de rest van het artikel ziet u het gebruik van de beschikbare .NET-methoden voor het uitvoeren van bewerkingen, zoals verificatie, het uploaden van bestanden enzovoort.

## <a name="authentication"></a>Authentication

* Zie [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Eindgebruikersverificatie met Data Lake Store met behulp van .NET SDK) voor de verificatie van eindgebruikers voor uw toepassing.
* Zie [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van .NET SDK) voor service-naar-serviceverificatie voor uw toepassing.


## <a name="create-client-object"></a>Clientobject maken
Met het volgende codefragment wordt het clientobject voor het bestandssysteem voor Data Lake Store gemaakt. Dit object wordt gebruikt voor het verzenden van aanvragen naar de service.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsAccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Een bestand en map maken
Voeg het volgende codefragment toe aan uw toepassing. Met dit codefragment wordt een bestand toegevoegd, evenals bovenliggende mappen die niet bestaan.

    // Create a file - automatically creates any parent directories that don't exist
    
    string fileName = "/Test/testFilename1.txt";
    using (var streamWriter = new StreamWriter(client.CreateFile(fileName, IfExists.Overwrite)))
    {
        streamWriter.WriteLine("This is test data to write");
        streamWriter.WriteLine("This is line 2");
    }

## <a name="append-to-a-file"></a>Toevoegen aan een bestand
Met het volgende codefragment worden gegevens toegevoegd aan het einde van een bestaand bestand in het Data Lake Store-account.

    // Append to existing file
    using (var streamWriter = new StreamWriter(client.GetAppendStream(fileName)))
    {
        streamWriter.WriteLine("This is the added line");
    }

## <a name="read-a-file"></a>Een bestand lezen
Met het volgende codefragment wordt de inhoud van een bestand in Data Lake Store gelezen.

    //Read file contents
    using (var readStream = new StreamReader(client.GetReadStream(fileName)))
    {
        string line;
        while ((line = readStream.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

## <a name="get-file-properties"></a>Bestandseigenschappen opvragen
Het volgende codefragment retourneert de eigenschappen die zijn gekoppeld aan een bestand of map.

    // Get file properties
    var directoryEntry = client.GetDirectoryEntry(fileName);
    PrintDirectoryEntry(directoryEntry);

De definitie van de methode `PrintDirectoryEntry` is beschikbaar als onderdeel van de voorbeeld-[-code op Github](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted). 

## <a name="rename-a-file"></a>De naam van een bestand wijzigen
In het volgende codefragment wordt de naam gewijzigd van een bestaand bestand in een Data Lake Store-account.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Een map opsommen
In het volgende codefragment worden de mappen in een Data Lake Store-account opgesomd.

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

De definitie van de methode `PrintDirectoryEntry` is beschikbaar als onderdeel van de voorbeeld-[-code op Github](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted).

## <a name="delete-directories-recursively"></a>Mappen recursief verwijderen
Met het volgende codefragment worden een map, en alle bijbehorende submappen, recursief verwijderd.

    // Delete a directory and all it's subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Voorbeelden
Hier volgen enkele voorbeelden van het gebruik van de bestandssysteem-SDK voor Data Lake Store.
* [Eenvoudig voorbeeld op Github](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Geavanceerd voorbeeld op Github](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Zie ook
* [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Naslaginformatie over Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

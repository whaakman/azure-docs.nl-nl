---
title: '.NET SDK: Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Gebruik Azure Data Lake Storage Gen1 .NET SDK voor het uitvoeren van bestandssysteembewerkingen in Data Lake Storage Gen1 zoals om mappen te maken enzovoort.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 929ce0d984e53586c46f15f21b9e5c90c6a34771
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402299"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 met .NET SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

In dit artikel leert u hoe u uitvoeren van bestandssysteembewerkingen in Data Lake Storage Gen1 met .NET SDK. Bestandssysteembewerkingen zijn het maken van mappen in een Data Lake Storage Gen1 account, het uploaden van bestanden, het downloaden van bestanden, enzovoort.

Zie voor instructies over het uitvoeren van accountbeheerbewerkingen in Data Lake Storage Gen1 met .NET SDK [accountbeheerbewerkingen in Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013, 2015 of 2017**. In onderstaande instructies wordt Visual Studio 2017 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Storage Gen1 account**. Zie voor instructies over het maken van een account [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Een .NET-toepassing maken
Dit codevoorbeeld beschikbaar [in GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) doorloopt het proces waarin bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en een aantal bestanden uit het archief wordt verwijderd. In dit gedeelte van het artikel komen de belangrijkste onderdelen van de code aan bod.

1. Open Visual Studio en maak een consoletoepassing.
2. Klik in het menu **File** op **New** en klik vervolgens op **Project**.
3. In **New Project** typt of selecteert u de volgende waarden:

   | Eigenschap | Waarde |
   | --- | --- |
   | Categorie |Templates/Visual C#/Windows |
   | Template |Console Application |
   | Name |CreateADLApplication |

4. Klik op **OK** om het project aan te maken.

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
                private static string _adlsg1AccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net";        
            }
        }

In de rest van het artikel ziet u het gebruik van de beschikbare .NET-methoden voor het uitvoeren van bewerkingen, zoals verificatie, het uploaden van bestanden enzovoort.

## <a name="authentication"></a>Verificatie

* Zie voor verificatie van eindgebruikers voor uw toepassing, [eindgebruikersverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md).
* Zie voor service-naar-serviceverificatie voor uw toepassing, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-object"></a>Clientobject maken
Het volgende fragment wordt de Data Lake Storage Gen1 clientobject, dat wordt gebruikt voor het verzenden van aanvragen naar de service.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsg1AccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Een bestand en map maken
Voeg het volgende codefragment toe aan uw toepassing. Met dit codefragment wordt een bestand toegevoegd, evenals bovenliggende mappen die niet bestaan.

    // Create a file - automatically creates any parent directories that don't exist
    // The AdlsOutputStream preserves record boundaries - it does not break records while writing to the store
    using (var stream = client.CreateFile(fileName, IfExists.Overwrite))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is test data to write.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);

        textByteArray = Encoding.UTF8.GetBytes("This is the second line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="append-to-a-file"></a>Toevoegen aan een bestand
Het volgende codefragment worden gegevens toegevoegd aan een bestaand bestand in Data Lake Storage Gen1-account.

    // Append to existing file
    using (var stream = client.GetAppendStream(fileName))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="read-a-file"></a>Een bestand lezen
Het volgende codefragment wordt de inhoud van een bestand in Data Lake Storage Gen1 gelezen.

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

De definitie van de `PrintDirectoryEntry` methode is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted). 

## <a name="rename-a-file"></a>De naam van een bestand wijzigen
Het volgende codefragment wijzigt de naam van een bestaand bestand in een Data Lake Storage Gen1-account.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Een map opsommen
Het volgende codefragment worden opgesomd mappen in een Data Lake Storage Gen1-account

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

De definitie van de `PrintDirectoryEntry` methode is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted).

## <a name="delete-directories-recursively"></a>Mappen recursief verwijderen
Met het volgende codefragment worden een map, en alle bijbehorende submappen, recursief verwijderd.

    // Delete a directory and all its subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Voorbeelden
Hier volgen enkele voorbeelden over het gebruik van de Data Lake Storage Gen1 bestandssysteem-SDK.
* [Eenvoudig voorbeeld op GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Geavanceerd voorbeeld op GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Zie ook
* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Naslaginformatie over Data Lake Storage Gen1 .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Volgende stappen
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)

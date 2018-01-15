---
title: '.NET SDK: Accountbeheerbewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Azure Data Lake Store .NET SDK gebruiken voor het uitvoeren van accountbeheerbewerkingen in de Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 682e8c9fc9e7f16d6b69cd73535c76e6c53c6b49
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="account-management-operations-on-azure-data-lake-store-using-net-sdk"></a>Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK
> [!div class="op_single_selector"]
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

In dit artikel leert u hoe u .NET SDK gebruikt voor het uitvoeren van accountbeheerbewerkingen in Data Lake Store. Voorbeelden van accountbeheerbewerkingen zijn het maken van een Data Lake Store-account, het weergeven van de accounts in een Azure-abonnement, het verwijderen van de accounts enzovoort.

Als u wilt weten hoe u .NET SDK gebruikt voor het uitvoeren van gegevensbeheerbewerkingen in Data Lake Store, raadpleegt u [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-data-operations-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013, 2015 of 2017**. In onderstaande instructies wordt Visual Studio 2017 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-net-application"></a>Een .NET-toepassing maken
1. Open Visual Studio en maak een consoletoepassing.
2. Klik in het menu **File** op **New** en klik vervolgens op **Project**.
3. In **New Project** typt of selecteert u de volgende waarden:

   | Eigenschap | Waarde |
   | --- | --- |
   | Category |Templates/Visual C#/Windows |
   | Template |Console Application |
   | Name |CreateADLApplication |
4. Klik op **OK** om het project aan te maken.
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

7. Declareer de variabelen en vervang de tijdelijke aanduidingen door waarden. Zorg er ook voor dat het lokale pad en de bestandsnaam die u opgeeft al bestaan op de computer.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; 
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; 
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";                    
                }
            }
        }

In de rest van het artikel ziet u het gebruik van de beschikbare .NET-methoden voor het uitvoeren van bewerkingen, zoals verificatie, het uploaden van bestanden enzovoort.

## <a name="authentication"></a>Verificatie

* Zie [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Eindgebruikersverificatie met Data Lake Store met behulp van .NET SDK) voor de verificatie van eindgebruikers voor uw toepassing.
* Zie [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van .NET SDK) voor service-naar-serviceverificatie voor uw toepassing.

## <a name="create-client-object"></a>Clientobject maken
In het volgende codefragment wordt het clientobject voor het Data Lake Store-account gemaakt dat wordt gebruikt voor het uitgeven van accountbeheeraanvragen voor de service, zoals het maken of verwijderen van accounts.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-store-account"></a>Een Data Lake Store-account maken
In het volgende codefragment wordt een Data Lake Store-account gemaakt in het Azure-abonnement dat u hebt opgegeven tijdens het maken van het clientobject voor het Data Lake Store-account.

    // Create Data Lake Store account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Alle Data Lake Store-accounts binnen een abonnement weergeven
Voeg de volgende methode toe aan uw klassedefinitie. Het volgende codefragment bevat alle Data Lake Store-accounts binnen een bepaald Azure-abonnement.

    // List all Data Lake Store accounts within the subscription
    public static List<DataLakeStoreAccountBasic> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccountBasic>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="delete-a-data-lake-store-account"></a>Een Data Lake Store-account verwijderen
In het volgende fragment wordt het Data Lake Store-account verwijderd dat u eerder hebt gemaakt.

    // Delete Data Lake Store account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Zie ook
* [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-data-operations-net-sdk.md)
* [Naslaginformatie over Data Lake Store .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

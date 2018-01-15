---
title: 'Verificatie van de eindgebruiker: .NET SDK met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de eindgebruiker verificatie met Data Lake Store met Azure Active Directory met .NET SDK
services: data-lake-store
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: ea71743e775da8cfd85ecbd20e9c5981a198e05c
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="end-user-authentication-with-data-lake-store-using-net-sdk"></a>Verificatie van de eindgebruiker met Data Lake Store met .NET SDK
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET-SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

In dit artikel leert u hoe u met de .NET SDK eindgebruiker Authentication uitvoeren met Azure Data Lake Store. Zie voor de verificatie van de service-naar-service met Data Lake Store met .NET SDK, [authentication Service-naar-service met Data Lake Store met .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013, 2015 of 2017**. In onderstaande instructies wordt Visual Studio 2017 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maak een 'Systeemeigen' Azure Active Directory-toepassing**. U moet voltooid van de stappen in [eindgebruiker verificatie met Data Lake Store met Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

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

6. Open **Program.cs**
7. Het gebruik van replease instructies met de volgende regels:

    ```csharp
    using System;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Collections.Generic;
            
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```     

## <a name="end-user-authentication"></a>Verificatie van de eindgebruiker
In dit fragment in uw clienttoepassing .NET toevoegen. Vervang de tijdelijke aanduiding voor waarden met de waarden die zijn opgehaald van een systeemeigen Azure AD-toepassing (aangeduid met de vereiste). In dit fragment kunt u verificatie van uw toepassing **interactief** met Data Lake Store, wat betekent dat u wordt gevraagd uw Azure-referenties invoeren.

Het volgende fragment voor eenvoudig te gebruiken, worden de standaardwaarden voor client-ID en omleidings-URI die geldig zijn voor een Azure-abonnement gebruikt. In het volgende fragment hoeft u alleen de waarde opgeven voor uw tenant-ID. U kunt met behulp van de instructies op de Tenant-ID ophalen [ophalen van de tenant-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
    
- De functie Main() vervangen door de volgende code:

    ```csharp
    private static void Main(string[] args)
    {
        //User login via interactive popup
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string MY_DOCUMENTS = System.Environment.GetFolderPath(System.Environment.SpecialFolder.MyDocuments);
        string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");
        var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
        var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
        var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
    }
    ```

Een aantal dingen te weten over het bovenstaande codefragment:

* De voorgaande-codefragment maakt gebruik van een hulpfuncties `GetTokenCache` en `GetCreds_User_Popup`. De code voor deze helperfuncties is beschikbaar [hier op Github](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#gettokencache).
* Als u de zelfstudie sneller zijn voltooid, gebruikt het fragment een systeemeigen toepassing client-ID die is standaard beschikbaar voor alle Azure-abonnementen. U kunt **dit fragment dus in zijn huidige vorm in uw toepassing gebruiken**.
* Als u echter uw eigen Azure AD-domein- en toepassingsclient-id wilt gebruiken, moet u een systeemeigen Azure AD-toepassing maken en vervolgens het Azure AD-tenant-ID, de client-ID en omleidings-URI gebruiken voor de toepassing die u hebt gemaakt. Zie [Een Active Directory-toepassing voor verificatie van eindgebruikers maken met Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md) voor instructies.

  
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe eindgebruikers verificatie gebruiken om te verifiëren met Azure Data Lake Store met .NET SDK. U kunt nu de volgende artikelen die over het gebruik van de .NET SDK communiceren werken met Azure Data Lake Store bekijken.

* [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Bewerkingen van de gegevens in Data Lake Store met .NET SDK](data-lake-store-data-operations-net-sdk.md)


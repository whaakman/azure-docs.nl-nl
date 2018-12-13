---
title: 'Verificatie van eindgebruikers: .NET-SDK met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met .NET SDK
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 62b7e293468a8b92f22ee1a5fd5b38cd15cfafbf
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53261531"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>Verificatie van de eindgebruiker met Azure Data Lake Storage Gen1 met .NET SDK
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET-SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

In dit artikel leert u over het gebruik van de .NET SDK doet eindgebruikersverificatie met Azure Data Lake Storage Gen1. Zie voor service-naar-serviceverificatie met Data Lake Storage Gen1 met .NET SDK, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013, 2015 of 2017**. In onderstaande instructies wordt Visual Studio 2017 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Native'**. U moet zijn voltooid de stappen in [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Een .NET-toepassing maken
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

      * `Microsoft.Azure.Management.DataLake.Store`: in deze zelfstudie wordt gebruikgemaakt van v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: in deze zelfstudie wordt gebruikgemaakt van v2.2.12.

        ![Een NuGet-bron toevoegen](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Een nieuw Azure Data Lake-account maken")
   4. Sluit de **NuGet Package Manager**.

6. Open **Program.cs**
7. Vervang de met behulp van instructies met de volgende regels:

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
Dit codefragment in uw .NET-client-toepassing toevoegen. Vervang de tijdelijke aanduiding door de waarden die zijn opgehaald uit een systeemeigen Azure AD-toepassing (weergegeven als vereiste). Dit codefragment kunt u verifiëren van uw toepassing **interactief** met Data Lake Storage Gen1, wat betekent dat u wordt gevraagd uw Azure-referenties in te voeren.

Voor het gebruiksgemak gebruikt gebruikt het volgende codefragment standaardwaarden voor de client-ID en omleidings-URI die geldig zijn voor alle Azure-abonnement. In het volgende codefragment hoeft u alleen voor de waarde voor uw tenant-ID. U kunt met behulp van de instructies op de Tenant-ID ophalen [ophalen van de tenant-ID](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    
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

Een aantal dingen die u moet weten over het bovenstaande codefragment:

* Het bovenstaande codefragment maakt gebruik van een ondersteunende functies `GetTokenCache` en `GetCreds_User_Popup`. De code voor deze helperfuncties is beschikbaar [hier op GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#gettokencache).
* Het fragment maakt gebruik van een systeemeigen toepassing client-ID die standaard beschikbaar zijn voor alle Azure-abonnementen is, kunt u de zelfstudie sneller voltooien. U kunt **dit fragment dus in zijn huidige vorm in uw toepassing gebruiken**.
* Als u echter uw eigen Azure AD-domein- en toepassingsclient-id wilt gebruiken, moet u een systeemeigen Azure AD-toepassing maken en vervolgens het Azure AD-tenant-ID, de client-ID en omleidings-URI gebruiken voor de toepassing die u hebt gemaakt. Zie [maken van een Active Directory-toepassing voor verificatie van eindgebruikers met Data Lake Storage Gen1](data-lake-store-end-user-authenticate-using-active-directory.md) voor instructies.

  
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u verificatie van eindgebruikers om u te verifiëren met Azure Data Lake Storage Gen1 met .NET SDK. U kunt nu de volgende artikelen die bespreken hoe u de .NET SDK gebruiken om te werken met Azure Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met .NET SDK](data-lake-store-data-operations-net-sdk.md)


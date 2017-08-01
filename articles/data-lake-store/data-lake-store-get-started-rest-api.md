---
title: Aan de slag met Azure Data Lake Store met de REST-API | Microsoft Docs
description: WebHDFS REST-API&quot;s gebruiken om bewerkingen uit te voeren in Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: dc2c8f58e0a2faf1b00f4903148328a5141a8637
ms.contentlocale: nl-nl
ms.lasthandoff: 06/07/2017


---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Aan de slag met Azure Data Lake Store met REST-API's
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

In dit artikel leest u hoe u WebHDFS REST-API's en Data Lake Store REST-API's gebruikt voor accountbeheer en om bestandssysteembewerkingen uit te voeren in Azure Data Lake Store. Azure Data Lake Store beschikt over eigen REST-API's voor accountbeheerbewerkingen. Omdat Data Lake Store echter compatibel is met het HDFS- en Hadoop-ecosysteem, wordt ook het gebruik van WebHDFS REST-API's voor bestandssysteembewerkingen ondersteund.

> [!NOTE]
> Raadpleeg de [Naslaginformatie over REST API's van Azure Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx) voor gedetailleerde informatie over de REST-API-ondersteuning van Data Lake Store.
> 
> 

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Active Directory-toepassing maken**. U gebruikt de Azure AD-toepassing om de Data Lake Store-toepassing te verifiëren in Azure AD. Er zijn verschillende manieren om te verifiëren in Azure AD, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Eindgebruikersverificatie](data-lake-store-end-user-authenticate-using-active-directory.md) of [Service-to-serviceverificatie](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.
* [cURL](http://curl.haxx.se/). In dit artikel wordt cURL gebruikt om te laten zien hoe u REST API-aanroepen maakt voor een Data Lake Store-account.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe verifieer ik met Azure Active Directory?
Er zijn twee benaderingen voor verificatie met Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Eindgebruikersverificatie (interactief)
In dit scenario wordt de gebruiker via de toepassing gevraagd om zich te melden en worden alle bewerkingen uitgevoerd in de context van de gebruiker. Voer de volgende stappen uit voor interactieve verificatie.

1. Leid de gebruiker via de toepassing om naar de volgende URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI> moet zijn gecodeerd om te worden gebruikt in een URL. Dus https://localhost, gebruikt u `https%3A%2F%2Flocalhost`)
   > 
   > 
   
    Voor deze zelfstudie kunt u de waarden van de tijdelijke aanduiding in bovenstaande URL vervangen en deze in de adresbalk van de webbrowser plakken. U wordt omgeleid om u te verifiëren met uw Azure-aanmelding. Wanneer u bent aangemeld, wordt het antwoord weergegeven in de adresbalk van de browser. Het antwoord heeft de volgende indeling:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Leg de autorisatiecode uit het antwoord vast. Voor deze zelfstudie kunt u de autorisatiecode uit de adresbalk van de webbrowser kopiëren en deze in de POST-aanvraag doorgeven aan het eindpunt van het token, zoals hieronder wordt weergegeven:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > In dit geval hoeft de \<REDIRECT-URI> niet te worden gecodeerd.
   > 
   > 
3. Het antwoord is een JSON-object dat een toegangstoken (bijvoorbeeld `"access_token": "<ACCESS_TOKEN>"`) en een vernieuwingstoken (bijvoorbeeld `"refresh_token": "<REFRESH_TOKEN>"`) bevat. Uw toepassing gebruikt het toegangstoken om toegang te krijgen tot Azure Data Lake Store en het vernieuwingstoken om een nieuw toegangstoken op te halen wanneer het oude is verlopen.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Wanneer het toegangstoken is verlopen, kunt u als volgt met het vernieuwingstoken een nieuw toegangstoken aanvragen:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Zie [De stroom voor autorisatiecodetoekenning](https://msdn.microsoft.com/library/azure/dn645542.aspx) voor meer informatie over interactieve gebruikersverificatie.

### <a name="service-to-service-authentication-non-interactive"></a>Service-naar-serviceverificatie (niet interactief)
In dit scenario verstrekt de toepassing zijn eigen referenties om bewerkingen uit te voeren. Daarvoor moet u een POST-aanvraag uitgeven, zoals in het voorbeeld hieronder. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van deze aanvraag bevat een verificatietoken (in de onderstaande uitvoer aangegeven met `access-token`) die u vervolgens gaat doorgeven met de REST-API-aanroepen. Sla dit verificatietoken op in een tekstbestand. U hebt het verderop in dit artikel nodig.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel wordt de **niet-interactieve** benadering gebruikt. Zie [Service-naar-service-aanroepen met referenties](https://msdn.microsoft.com/library/azure/dn645543.aspx) voor meer informatie over niet-interactieve (service-naar-service) aanroepen.

## <a name="create-a-data-lake-store-account"></a>Een Data Lake Store-account maken
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://msdn.microsoft.com/library/mt694078.aspx) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Vervang \<`REDACTED`\> in de bovenstaande opdracht door het verificatietoken dat u eerder hebt opgehaald. De nettolading van de aanvraag voor deze opdracht zit in het bestand **input.json** dat is opgegeven voor de parameter `-d` hierboven. De inhoud van het bestand input.json ziet er ongeveer als volgt uit:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappen maken in een Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

Vervang \<`REDACTED`\> in de bovenstaande opdracht door het verificatietoken dat u eerder hebt opgehaald. Met deze opdracht maakt u een map **mytempdir** onder de hoofdmap van uw Data Lake Store-account.

Als de bewerking is geslaagd, wordt een antwoord van de volgende strekking weergegeven:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Mappen weergeven in Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

Vervang \<`REDACTED`\> in de bovenstaande opdracht door het verificatietoken dat u eerder hebt opgehaald.

Als de bewerking is geslaagd, wordt een antwoord van de volgende strekking weergegeven:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Gegevens uploaden naar een Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

In de bovenstaande syntaxis geeft de parameter **-T** de locatie aan van het bestand dat u uploadt.

De uitvoer lijkt op het volgende:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a>Gegevens lezen uit een Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File) wordt gedefinieerd.

Het lezen van gegevens uit een Data Lake Store is een proces dat uit twee stappen bestaat.

* Eerst dient u een GET-aanvraag in voor het eindpunt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Deze retourneert een locatie waarnaar u de volgende GET-aanvraag moet verzenden.
* Vervolgens dient u de GET-aanvraag in voor het eindpunt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Hiermee wordt de inhoud van het bestand weergegeven.

Omdat de invoerparameters in de eerste en tweede stap echter gelijk zijn, kunt u de parameter `-L` gebruiken om de eerste aanvraag in te dienen. `-L` combineert in feite twee aanvragen in één en zorgt ervoor dat cURL de aanvraag opnieuw uitvoert op de nieuwe locatie. Ten slotte wordt de uitvoer van alle aanroepen van de aanvraag weergegeven, zoals u hieronder kunt zien. Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Bestandsnamen wijzigen in een Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory) wordt gedefinieerd.

Als u de naam van een bestand wil wijzigen, gebruikt u de volgende cURL-opdracht. Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Een bestand verwijderen uit een Data Lake Store-account
Deze bewerking is gebaseerd op de WebHDFS REST-API-aanroep die [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht als u een bestand wilt verwijderen. Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Een Data Lake Store-account verwijderen
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://msdn.microsoft.com/library/mt694075.aspx) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht als u een Data Lake Store-account wilt verwijderen. Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Zie ook
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)



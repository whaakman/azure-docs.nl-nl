---
title: C++ zelfstudie voor Azure Cosmos DB | Microsoft Docs
description: Dit is een C++ zelfstudie waarmee u een C++ database en -consoletoepassing maakt met Azure Cosmos DB-SDK voor C++. Azure Cosmos DB is een databaseservice op wereldwijde schaal.
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: da969e3f619c9703ea0c02a148f11a9509d6e988
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-sql-api"></a>Azure Cosmos DB: Zelfstudie C++ console toepassingen voor de SQL-API
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

Welkom bij de C++-zelfstudie voor de SQL-API van Azure Cosmos DB SDK voor C++ goedgekeurde. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren. Een van deze resources is een C++ database.

Deze snelstartgids komen aan bod:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw toepassing instellen
* Een C++ Azure Cosmos DB-database maken
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* Een document vervangen
* Een document verwijderen
* De C++ Azure Cosmos DB-database verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/stalker314314/sql-apiCpp). Zie [De volledige oplossing gebruiken](#GetSolution) voor beknopte instructies.

Tijd om aan de slag te gaan.

## <a name="prerequisites-for-the-c-tutorial"></a>Vereisten voor de C++-zelfstudie
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Visual Studio 2017](https://www.visualstudio.com/downloads/), met de C++ taalonderdelen geïnstalleerd. Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Uw C++-toepassing instellen](#SetupC++).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>Stap 2: uw C++-toepassing instellen
1. Open Visual Studio en klik in het menu **Bestand** op **Nieuw** en vervolgens op **Project**. 
2. Vouw in het venster **Nieuw project**, in het deelvenster **Geïnstalleerd**, **Visual C++** uit, klik op **Win32** en klik vervolgens op **Win32-consoletoepassing**. Noem het project hellodocumentdb en klik vervolgens op **OK**. 
   
    ![Schermafbeelding van de wizard Nieuw project](media/sql-api-cpp-get-started/hello.png)
3. Klik zodra de Win32-toepassingswizard is gestart op **Voltooien**.
4. Open nadat het project is gemaakt het NuGet-pakketbeheer door met de rechtermuisknop op het project **hellodocumentdb** te klikken in **Solution Explorer** en vervolgens op **NuGet-pakketten beheren** te klikken. 
   
    ![Schermafbeelding van NuGet-pakketten beheren in het projectmenu](media/sql-api-cpp-get-started/nuget.png)
5. Klik in het tabblad **NuGet: hellodocumentdb** op **Bladeren** en zoek naar *documentdbcpp*. Selecteer DocumentDbCPP in de resultaten, zoals weergegeven in de volgende schermafbeelding. Met dit pakket worden referenties naar C++ REST SDK geïnstalleerd, een afhankelijkheid voor de DocumentDbCPP.  
   
    ![Schermafbeelding waarin het DocumentDbCpp-pakket is gemarkeerd](media/sql-api-cpp-get-started/cpp.png)
   
    Zodra de pakketten aan uw project zijn toegevoegd, kunt u code gaan schrijven.   

## <a id="Config"></a>Stap 3: verbindingsgegevens kopiëren vanuit Azure Portal voor uw Azure Cosmos DB-database
Open [Azure Portal](https://portal.azure.com) en ga naar het Azure Cosmos DB-databaseaccount dat u hebt gemaakt. U hebt de URI en de primaire sleutel uit Azure Portal nodig in de volgende stap om verbinding te maken vanaf uw C++-codefragment. 

![Azure Cosmos DB-URI en sleutels in Azure Portal](media/sql-api-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Stap 4: verbinding maken met een Azure Cosmos DB-account
1. Voeg de volgende kopteksten en naamruimtes toe aan uw broncode, na `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Voeg daarna de volgende code toe aan de functie Main en vervang de accountconfiguratie en primaire sleutel, zodat ze overeenkomen met uw Azure Cosmos DB-instellingen uit stap 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Nu dat u de code voor het initialiseren van de client, laten we in het werken met Azure Cosmos DB resources hebben.

## <a id="CreateDBColl"></a>Stap 5: een C++-database en -verzameling maken
Voordat u deze stap uitvoert, behandelen we eerst hoe een database, verzameling en documenten met elkaar communiceren, voor het geval Azure Cosmos DB nieuw voor u is. Een [database](sql-api-resources.md#databases) is een logische container voor documentopslag, gepartitioneerd in verzamelingen. Een [verzameling](sql-api-resources.md#collections) is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica. Meer informatie over het hiërarchische resourcemodel en concepten voor Azure Cosmos DB vindt u in [Hiërarchisch Azure Cosmos DB-resourcemodel en -concepten](sql-api-resources.md).

Voeg de volgende code toe aan het eind van de main-functie om een database en bijbehorende verzameling te maken. Hiermee maakt u een database genaamd 'FamilyRegistry' en een verzameling genaamd 'FamilyCollection' met behulp van de klantconfiguratie die u in de vorige stap hebt ingesteld.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Stap 6: een document maken
[Documenten](sql-api-resources.md#documents) bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een document invoegen in Azure Cosmos DB. U kunt een document maken door de volgende code aan het einde van de main-functie te plakken. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

Kort samengevat maakt u met deze code een Azure Cosmos DB-database, -verzameling en -documenten, waarop u query's kunt toepassen in Documentverkenner in Azure Portal. 

![C++-zelfstudie: diagram waarin u de hiërarchische relatie ziet tussen het account, de database, de verzameling en de documenten](media/sql-api-cpp-get-started/docs.png)

## <a id="QueryDB"></a>Stap 7: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor [uitgebreide query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. De volgende voorbeeldcode bevat een query die is gemaakt met de SQL-syntaxis. Deze query kan worden uitgevoerd voor de documenten die in de vorige stap zijn gemaakt.

De functie omvat als argumenten de unieke id of resource-id voor de database en de verzameling, samen met het clientdocument. Voeg deze code toe voor de main-functie.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>Stap 8: een document vervangen
Azure Cosmos DB ondersteunt het vervangen van JSON-documenten, zoals in de volgende code wordt gedemonstreerd. Voeg deze code toe na de functie executesimplequery.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>Stap 9: een document verwijderen
Azure Cosmos DB ondersteunt het verwijderen van JSON-documenten. U kunt deze documenten verwijderen door de volgende code te kopiëren en achter de instructie replacedocument te plakken. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>Stap 10: een database verwijderen
Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten enz.) verwijderd.

Kopieer en plak het volgende codefragment (functie cleanup) na de functie deletedocument om de database en alle onderliggende resources te verwijderen.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Stap 11: uw C++-consoletoepassing volledig uitvoeren
U hebt nu code toegevoegd om verschillende Azure Cosmos DB-resources te maken, aan te passen, te verwijderen en er query's op toe te passen.  Nu is het tijd om alles te verbinden door calls toe te voegen aan de verschillende functies vanuit de main-functie in hellodocumentdb.cpp, samen met een aantal diagnostische berichten.

U doet dit door de main-functie van uw toepassing te vervangen door de volgende code. Deze overschrijft de account_configuration_uri en primary_key die u in de code hebt geplakt in stap 3, dus bewaar deze regel of kopieer en plak de waarden opnieuw vanuit de portal. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

U zou nu uw code moeten kunnen bouwen en uitvoeren in Visual Studio door op F5 te drukken of naar het terminalvenster te gaan, de toepassing te zoeken en het uitvoerbare bestand uit te voeren. 

U ziet de uitvoer van uw GetStarted-app. De uitvoer moet overeenkomen met de volgende schermafbeelding.

![Uitvoer Azure Cosmos DB C++ toepassing](media/sql-api-cpp-get-started/console.png)

Gefeliciteerd. U hebt de C++ zelfstudie voltooid en beschikt nu over uw eerste Azure Cosmos DB-consoletoepassing.

## <a id="GetSolution"></a>De volledige C++-zelfstudieoplossing ophalen
Als u een GetStarted-oplossing wilt bouwen die alle voorbeelden uit dit artikel bevat, hebt u het volgende nodig:

* [Azure Cosmos DB-account][create-account].
* De [GetStarted](https://github.com/stalker314314/DocumentDBCpp)-oplossing die beschikbaar is via GitHub.

## <a name="next-steps"></a>Volgende stappen
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Meer informatie over het programmeermodel vindt u in de sectie Ontwikkelen van de [pagina met Azure Cosmos DB-documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/).

[create-account]: create-sql-api-dotnet.md#create-account



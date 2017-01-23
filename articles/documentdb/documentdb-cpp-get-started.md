---
title: NoSQL C++-zelfstudie voor DocumentDB | Microsoft Docs
description: Een NoSQL C++-zelfstudie waarmee u een C++-database en -consoletoepassing maakt met de DocumentDB SDK voor C++. DocumentDB is een NoSQL-databaseservice op planetaire schaal.
services: documentdb
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: aasthan
translationtype: Human Translation
ms.sourcegitcommit: 16bff1b5708652a75ea603f596c864901b12a88d
ms.openlocfilehash: f622b9a35c370148a3472fa6924a50933d59601e


---
# <a name="nosql-c-tutorial-documentdb-c-console-application"></a>NoSQL C++-zelfstudie: een DocumentDB C++-consoletoepassing
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Welkom bij de C++-zelfstudie over de Azure DocumentDB SDK voor C++. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u DocumentDB-resources kunt maken en er query’s op kunt uitvoeren. Een van deze resources is een C++-database.

De volgende onderwerpen komen aan bod:

* Een DocumentDB-account maken en er verbinding mee maken
* Uw toepassing instellen
* Een C++-database voor DocumentDB maken
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* Een document vervangen
* Een document verwijderen
* Een C++-database voor DocumentDB verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/stalker314314/DocumentDBCpp). Zie [De volledige oplossing gebruiken](#GetSolution) voor beknopte instructies.

Wanneer u de C++-zelfstudie hebt voltooid, gebruikt u de stemknoppen onder aan deze pagina om feedback te verzenden. 

Als u graag rechtstreeks contact wilt opnemen, kunt u uw e-mailadres aan uw reactie toevoegen of [hier contact met ons opnemen](https://www.research.net/r/8BKRJ3Z). 

Tijd om aan de slag te gaan.

## <a name="prerequisites-for-the-c-tutorial"></a>Vereisten voor de C++-zelfstudie
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis Azure-proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/) met de C++-taalonderdelen geïnstalleerd.

## <a name="step-1-create-a-documentdb-account"></a>Stap 1: een DocumentDB-account maken
U maakt om te beginnen een DocumentDB-account. Als u al een account hebt dat u wilt gebruiken, kunt u verder naar de stap [Uw C++-toepassing instellen](#SetupNode).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idsetupcastep-2-set-up-your-c-application"></a><a id="SetupC++"></a>Stap 2: uw C++-toepassing instellen
1. Open Visual Studio en klik in het menu **Bestand** op **Nieuw** en vervolgens op **Project**. 
2. Vouw in het venster **Nieuw project**, in het deelvenster **Geïnstalleerd**, **Visual C++** uit, klik op **Win32** en klik vervolgens op **Win32-consoletoepassing**. Noem het project hellodocumentdb en klik vervolgens op **OK**. 
   
    ![Schermafbeelding van de wizard Nieuw project](media/documentdb-cpp-get-started/hellodocumentdb.png)
3. Klik zodra de Win32-toepassingswizard is gestart op **Voltooien**.
4. Open nadat het project is gemaakt het NuGet-pakketbeheer door met de rechtermuisknop op het project **hellodocumentdb** te klikken in **Solution Explorer** en vervolgens op **NuGet-pakketten beheren** te klikken. 
   
    ![Schermafbeelding van NuGet-pakketten beheren in het projectmenu](media/documentdb-cpp-get-started/nuget.png)
5. Klik in het tabblad **NuGet: hellodocumentdb** op **Bladeren** en zoek naar *documentdbcpp*. Selecteer DocumentDbCPP in de resultaten, zoals weergegeven in de volgende schermafbeelding. Met dit pakket worden referenties naar C++ REST SDK geïnstalleerd, een afhankelijkheid voor de DocumentDbCPP.  
   
    ![Schermafbeelding waarin het DocumentDbCpp-pakket is gemarkeerd](media/documentdb-cpp-get-started/documentdbcpp.png)
   
    Zodra de pakketten aan uw project zijn toegevoegd, kunt u code gaan schrijven.   

## <a name="a-idconfigastep-3-copy-connection-details-from-azure-portal-for-your-documentdb-database"></a><a id="Config"></a>Stap 3: verbindingsgegevens kopiëren vanuit Azure Portal voor uw DocumentDB-database
Open [Azure Portal](https://portal.azure.com) en ga naar het NoSQL-databaseaccount (DocumentDB) dat u hebt gemaakt. U hebt de URI en de primaire sleutel uit Azure Portal nodig in de volgende stap om verbinding te maken vanaf uw C++-codefragment. 

![DocumentDB-URI en sleutels in Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a name="a-idconnectastep-4-connect-to-a-documentdb-account"></a><a id="Connect"></a>Stap 4: verbinding maken met een DocumentDB-account
1. Voeg de volgende kopteksten en naamruimtes toe aan uw broncode, na `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Voeg daarna de volgende code toe aan uw main-functie en vervang de accountconfiguratie en primaire sleutel zodat ze overeenkomen met uw DocumentDB-instellingen uit stap 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Nu u beschikt over de code om de DocumentDB-client opnieuw te initialiseren, kunt u zich verder verdiepen in het werken met DocumentDB-resources.

## <a name="a-idcreatedbcollastep-5-create-a-c-database-and-collection"></a><a id="CreateDBColl"></a>Stap 5: een C++-database en -verzameling maken
Voordat we deze stap uitvoeren, behandelen we eerst hoe een database, verzameling en documenten met elkaar communiceren, voor het geval dat DocumentDB nieuw voor u is. Een [database](documentdb-resources.md#databases) is een logische container voor documentopslag, gepartitioneerd in verzamelingen. Een [verzameling](documentdb-resources.md#collections) is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica. Meer informatie over het hiërarchisch bronmodel en concepten voor DocumentDB vindt u in [Hiërarchisch DocumentDB-resourcemodel en -concepten](documentdb-resources.md).

Voeg de volgende code toe aan het eind van de main-functie om een database en bijbehorende verzameling te maken. Hiermee maakt u een database genaamd 'FamilyRegistry' en een verzameling genaamd 'FamilyCollection' met behulp van de klantconfiguratie die u in de vorige stap hebt ingesteld.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a name="a-idcreatedocastep-6-create-a-document"></a><a id="CreateDoc"></a>Stap 6: een document maken
[Documenten](documentdb-resources.md#documents) bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een document invoegen in DocumentDB. U kunt een document maken door de volgende code aan het einde van de main-functie te plakken. 

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

Kort samengevat maakt deze code een DocumentDB-database, -verzameling en -documenten, waarop u query’s kunt toepassing in Documentverkenner in Azure Portal. 

![C++-zelfstudie: diagram waarin u de hiërarchische relatie ziet tussen het account, de database, de verzameling en de documenten](media/documentdb-cpp-get-started/documentdbdocs.png)

## <a name="a-idquerydbastep-7-query-documentdb-resources"></a><a id="QueryDB"></a>Stap 7: een query uitvoeren op DocumentDB-resources
DocumentDB biedt ondersteuning voor [uitgebreide query's](documentdb-sql-query.md) in de JSON-documenten die zijn opgeslagen in verzamelingen. De volgende voorbeeldcode bevat een query die is gemaakt met de DocumentDB SQL-syntaxis. Deze query kan worden uitgevoerd voor de documenten die we in de vorige stap hebben gemaakt.

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

## <a name="a-idreplaceastep-8-replace-a-document"></a><a id="Replace"></a>Stap 8: een document vervangen
DocumentDB ondersteunt het vervangen van JSON-documenten, zoals in de volgende code wordt gedemonstreerd. Voeg deze code toe na de functie executesimplequery.

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

## <a name="a-iddeleteastep-9-delete-a-document"></a><a id="Delete"></a>Stap 9: een document verwijderen
DocumentDB ondersteunt het verwijderen van JSON-documenten. U kunt deze documenten verwijderen door de volgende code te kopiëren en achter de functie replacedocument te plakken. 

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

## <a name="a-iddeletedbastep-10-delete-a-database"></a><a id="DeleteDB"></a>Stap 10: een database verwijderen
Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten enz.) verwijderd.

Kopieer en plak het volgende codefragment (functie cleanup) na de functie deletedocument om de database en alle onderliggende resources te verwijderen.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a name="a-idrunastep-11-run-your-c-application-all-together"></a><a id="Run"></a>Stap 11: uw C++-consoletoepassing volledig uitvoeren
U hebt nu code toegevoegd om verschillende DocumentDB-resources te maken, aan te passen, te verwijderen en er query’s op toe te passen.  Nu is het tijd om alles te verbinden door calls toe te voegen aan de verschillende functies vanuit de main-functie in hellodocumentdb.cpp, samen met een aantal diagnostische berichten.

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

![Uitvoer DocumentDB C++-toepassing](media/documentdb-cpp-get-started/docdbconsole.png)

Gefeliciteerd. U hebt de C++-zelfstudie voltooid en u beschikt nu over uw eerste DocumentDB-consoletoepassing.

## <a name="a-idgetsolutionaget-the-complete-c-tutorial-solution"></a><a id="GetSolution"></a>De volledige C++-zelfstudieoplossing ophalen
Als u een GetStarted-oplossing wilt bouwen die alle voorbeelden uit dit artikel bevat, hebt u het volgende nodig:

* [DocumentDB-account][documentdb-create-account].
* De [GetStarted](https://github.com/stalker314314/DocumentDBCpp)-oplossing die beschikbaar is via GitHub.

## <a name="next-steps"></a>Volgende stappen
* Informatie over [het bewaken van een DocumentDB-account](documentdb-monitor-accounts.md).
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Meer informatie over het programmeermodel vindt u in de sectie Ontwikkelen van de pagina [DocumentDB-documentatie](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md





<!--HONumber=Jan17_HO1-->



---
title: Azure Cosmos DB beheren in Azure Storage Explorer
description: Leer Azure Cosmos DB beheren in Azure Storage Explorer.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: 18f580f1eae31c9bf3626e100217467bb48ca881
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Azure Cosmos DB beheren in Azure Storage Explorer (preview)

Het gebruik van Azure Cosmos DB in Azure Storage Explorer stelt gebruikers in staat om Azure Cosmos DB entiteiten te beheren, gegevens te manipuleren, en opgeslagen procedures en triggers bij te werken samen met andere Azure entiteiten zoals opslagblobs en wachtrijen. U kunt nu hetzelfde hulpprogramma gebruiken om uw verschillende Azure entiteiten op één plek te beheren. Op dit moment ondersteunt Azure Storage Explorer SQL-, MongoDB-, Graph- en Table-accounts.

In dit artikel kunt u lezen hoe u Storage Explorer kunt gebruiken om Azure Cosmos DB te beheren.


## <a name="prerequisites"></a>Vereisten

Een Azure Cosmos DB-account voor de SQL API <!--or MongoDB API-->. Als u geen account hebt, kunt u er een maken in de Azure-portal, zoals beschreven in [Azure Cosmos DB: een SQL API-web-app ontwikkelen met .NET en de Azure Portal](create-sql-api-dotnet.md).

## <a name="installation"></a>Installatie

Installeer hier de nieuwste Azure Storage Explorer-bits: [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), nu ondersteunen we Windows, Linux en MAC-versie.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

1. Klik nadat u **Azure Storage Explorer** heb geïnstalleerd op het **invoegtoepassing**pictogram aan de linkerkant, zoals in de volgende afbeelding:
       
   ![Pictogram invoegtoepassing](./media/storage-explorer/plug-in-icon.png)
 
2. Selecteer **Een Azure-account toevoegen** en klik op **Aanmelden**.

   ![Verbinding maken met Azure-abonnement](./media/storage-explorer/connect-to-azure-subscription.png)

2. Selecteer **Aanmelden** in het **aanmeldingsvenster van Azure** en voer uw Azure-referenties in.

    ![Aanmelden](./media/storage-explorer/sign-in.png)

3. Selecteer uw abonnement in de lijst en klik op **Toepassen**.

    ![Toepassen](./media/storage-explorer/apply-subscription.png)

    Het Verkenner-venster wordt bijgewerkt en de accounts in het geselecteerde abonnement worden weergegeven.

    ![Lijst met accounts](./media/storage-explorer/account-list.png)

    U hebt uw **Cosmos DB-account** gekoppeld aan uw Azure-abonnement.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Verbinding maken met Azure Cosmos DB met behulp van een verbindingsreeks

Een andere manier van verbinding maken met een Azure Cosmos DB is het gebruik van een verbindingsreeks. Gebruik de volgende stappen om verbinding te maken met behulp van een verbindingsreeks.

1. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Cosmos DB-accounts** en kies **Verbinding maken met Cosmos DB...**

    ![Verbinding maken met Cosmos DB met een verbindingsreeks](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. Momenteel is er alleen ondersteuning voor de SQL- en Table-API. Kies een API, plak uw **verbindingsreeks**, voer het **accountlabel** in, klik op **Volgende** om het overzicht te controleren en klik vervolgens op **Verbinden** om verbinding te maken met de Azure Cosmos DB-account. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string) voor informatie over het ophalen van de verbindingsreeks.

    ![Verbindingsreeks](./media/storage-explorer/connection-string.png)

## <a name="connect-to-azure-cosmos-db-by-using-local-emulator"></a>Verbinding maken met Azure Cosmos DB met behulp van een lokale emulator
Gebruik de volgende stappen uit om verbinding te maken met een Cosmos Azure DB via een emulator. Momenteel is er alleen ondersteuning voor SQL-accounts.
1. Installeer de emulator en start deze op. Zie [Cosmos DB Emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator) voor informatie over het installeren van Emulator.
2. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Cosmos DB-accounts** en kies **Verbinding maken met Cosmos DB Emulator...**

    ![Verbinding maken met Cosmos DB Emulator](./media/storage-explorer/emulator-entry.png)

3. Momenteel is er alleen ondersteuning voor de SQL-API. Plak uw **verbindingsreeks**, voer het **accountlabel** in, klik op **Volgende** om het overzicht te controleren en klik vervolgens op **Verbinden** om verbinding te maken met de Azure Cosmos DB-account. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string) voor informatie over het ophalen van de verbindingsreeks.

    ![Het dialoogvenster Verbinding maken met Cosmos DB Emulator](./media/storage-explorer/emulator-dialog.png)



## <a name="azure-cosmos-db-resource-management"></a>Azure Cosmos DB-resourcebeheer

U kunt een Azure Cosmos DB account beheren door de volgende bewerkingen uit te voeren:
* Open het account in de Azure portal
* Voeg de resource toe aan de lijst Snelle toegang
* Resources zoeken en vernieuwen
* Databases maken en verwijderen
* Verzamelingen maken en verwijderen
* Documenten maken, bewerken, verwijderen en filteren
* Opgeslagen procedures, triggers en door gebruikers gedefinieerde functies beheren

### <a name="quick-access-tasks"></a>Snelle toegang tot taken

Door met de rechtermuisknop op een abonnement in het deelvenster Verkenner te klikken, kunt u veel actietaken snel uitvoeren:

* Klik met de rechtermuisknop op een Azure Cosmos DB-account of een database. U kunt **Openen in Portal** kiezen en de bron beheren in de browser op de Azure-portal.

     ![Openen in portal](./media/storage-explorer/open-in-portal.png)

* U kunt ook Azure Cosmos DB-account, database, verzameling toevoegen aan **Snelle toegang**.
* Met **Zoeken vanaf hier** kunt u naar sleutelwoorden zoeken onder het geselecteerde pad.

    ![zoeken vanaf hier](./media/storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Database- en verzamelingbeheer
#### <a name="create-a-database"></a>Een database maken 
-   Klik met de rechtermuisknop op het Azure Cosmos DB-account, kies **Database maken**, voer de naam van de database in en druk op **Enter**.
       
    ![Database maken](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Een database verwijderen
- Klik met de rechtermuisknop op de database, klik op **Database verwijderen** en klik in het pop-upvenster op **Ja**. Het databaseknooppunt wordt verwijderd en het Azure Cosmos DB die Cosmos-account wordt automatisch vernieuwd.

    ![Database1 verwijderen](./media/storage-explorer/delete-database1.png)  

    ![Database2 verwijderen](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Een verzameling maken
1. Klik met de rechtermuisknop op de database, kies **Verzameling maken**en geef de volgende informatie op: **Verzameling-id**, **Opslagcapaciteit**, enzovoort. Klik op **OK** om de regel op te slaan. 

    ![Verzameling1 maken](./media/storage-explorer/create-collection.png)

    ![Verzameling2 maken](./media/storage-explorer/create-collection2.png) 

2. Selecteer **Onbeperkt** om de partitiesleutel te kunnen opgeven. Klik vervolgens op **OK** om te voltooien.

    Als bij het maken van een verzameling een partitiesleutel wordt gebruikt, kan de partitiesleutelwaarde van de verzameling niet meer worden gewijzigd. Zie [Ontwerp voor partitioneren](partition-data.md#designing-for-partitioning) voor informatie over partitiesleutelinstellingen.

    ![Partitiesleutel](./media/storage-explorer/partitionkey.png)

#### <a name="delete-a-collection"></a>Een verzameling verwijderen
- Klik met de rechtermuisknop op de verzameling, klik op **Verzameling verwijderen**en klik vervolgens op **Ja** in het pop-upvenster. 

    Het verzamelingsknooppunt wordt verwijderd en de database wordt automatisch vernieuwd.

    ![Verzameling verwijderen](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Documentbeheer

#### <a name="create-and-modify-documents"></a>Documenten maken en wijzigen
- Als u een nieuw document wilt maken, opent u **Documenten** in het linker venster, klikt u op **Nieuw document**, bewerkt u de inhoud in het rechter deelvenster en klikt u vervolgens op **Opslaan**. U kunt ook een bestaand document bijwerken en vervolgens op **Opslaan** klikken. Wijzigingen kunnen worden genegeerd door op **Negeren** te klikken.

    ![Document](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Een document verwijderen
- Klik op de knop **Verwijderen** om het geselecteerde document te verwijderen.

#### <a name="query-for-documents"></a>Query voor documenten
- Bewerk het documentfilter door een [SQL query](sql-api-sql-query.md) in te voeren en op **Toepassen** te klikken.

    ![Documentfilter](./media/storage-explorer/document-filter.png)



### <a name="graph-management"></a>Grafiekbeheer

#### <a name="create-and-modify-vertex"></a>Hoekpunt maken en wijzigen
1. Als u een nieuw hoekpunt wilt maken, opent u **Grafiek** vanuit het linkervenster, klikt u op **Nieuw hoekpunt**, bewerkt u de inhoud en klikt u vervolgens op **OK**.    
2. Als u een bestaand hoekpunt wilt wijzigen, klikt u op het penpictogram in het rechterdeelvenster.   

    ![Graph](./media/storage-explorer/vertex.png)

#### <a name="delete-a-graph"></a>Een grafiek verwijderen
- Als u een hoekpunt wilt verwijderen, klikt u op het prullenbakpictogram naast de naam van het hoekpunt.

#### <a name="filter-for-graph"></a>Filteren op grafiek
- Bewerk het grafiekfilter door een [Gremlin-query](gremlin-support.md) in te voeren en op **Filter toepassen** te klikken.

    ![Grafiekfilter](./media/storage-explorer/graph-filter.png)

### <a name="table-management"></a>Tabelbeheer

#### <a name="create-and-modify-table"></a>Tabel maken en wijzigen
1. Ga als volgt te werk om een nieuwe tabel te maken: open **Entiteiten** vanuit het linkerdeelvenster, klik op **Toevoegen**, bewerk de inhoud in **Entiteit toevoegen**, voeg een eigenschap toe door te klikken op de knop **Eigenschap toevoegen** en klik vervolgens op **Invoegen**.
2. Als u een tabel wilt wijzigen, klikt u op **Bewerken**, wijzigt u de inhoud en klikt u vervolgens op **Bijwerken**.

    ![Tabel](./media/storage-explorer/table.png)

#### <a name="import-and-export-table"></a>Tabel importeren en exporteren
1. Als u wilt importeren, klikt u op de knop **Importeren** en kiest u een bestaande tabel.
2. Als u wilt exporteren, klikt u op de knop **Exporteren** en kiest u een bestemming.

    ![Tabel importeren en exporteren](./media/storage-explorer/table-import-export.png)

#### <a name="delete-entities"></a>Entiteiten verwijderen
- Selecteer de entiteiten en klik op knop **Verwijderen**.

    ![Tabel verwijderen](./media/storage-explorer/table-delete.png)

#### <a name="query-table"></a>Een query uitvoeren op een tabel
- Klik op de knop **Query**, voer queryvoorwaarden in en klik op de knop **Query uitvoeren**. Sluit het queryvenster door te klikken op de knop **Query sluiten**.

    ![Query uitvoeren op tabel](./media/storage-explorer/table-query.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Opgeslagen procedures, triggers en UDF's beheren
* Als u een opgeslagen procedure wilt maken, klikt u met de rechtermuisknop in de linkerboomstructuur op **Opgeslagen procedure**, kiest u **Opgeslagen procedure maken**, voert u links een naam in, typt u de opgeslagen-procedurescripts in het rechter venster en klikt u vervolgens op **Maken**. 
* U kunt ook bestaande opgeslagen procedures bewerken door erop te dubbelklikken, de procedure te bewerken en op **Bijwerken** te klikken om de wijzigingen op te slaan of op **Negeren** om de wijziging te annuleren.

    ![Opgeslagen procedure](./media/storage-explorer/stored-procedure.png)
* De bewerkingen voor **triggers** en **UDF** zijn vergelijkbaar met die voor **opgeslagen procedures**.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de volgende video om te zien hoe u Azure Cosmos DB in Azure Storage Explorer kunt gebruiken: [Azure Cosmos DB gebruiken in Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Meer informatie over Opslagverkenner en verbinding maken met meer services vindt u in [Aan de slag met Storage Explorer (preview)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).


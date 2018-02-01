---
title: Azure Cosmos DB beheren in Azure Storage Explorer
description: Leer Azure Cosmos DB beheren in Azure Storage Explorer.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: 
author: jejiang
manager: omafnan
editor: 
tags: Azure Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: Jejiang
ms.openlocfilehash: baa7eee614159f9c6af493aff74b27773471f7d7
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Azure Cosmos DB beheren in Azure Storage Explorer (preview)

Het gebruik van Azure Cosmos DB in Azure Storage Explorer stelt gebruikers in staat om Azure Cosmos DB entiteiten te beheren, gegevens te manipuleren, en opgeslagen procedures en triggers bij te werken samen met andere Azure entiteiten zoals opslagblobs en wachtrijen. U kunt nu hetzelfde hulpprogramma gebruiken om uw verschillende Azure entiteiten op één plek te beheren. Op dit moment ondersteunt Azure Storage Explorer SQL <!--and MongoDB--> accounts. De Azure Storage Explorer werkt niet met de lokale emulator van Azure Cosmos DB. 

In dit artikel kunt u lezen hoe u Storage Explorer kunt gebruiken om Azure Cosmos DB te beheren.


## <a name="prerequisites"></a>Vereisten

Een Azure Cosmos DB-account voor de SQL API <!--or MongoDB API-->. Als u geen account hebt, kunt u er een maken in de Azure-portal, zoals beschreven in [Azure Cosmos DB: een SQL API-web-app ontwikkelen met .NET en de Azure Portal](create-sql-api-dotnet.md).

## <a name="installation"></a>Installatie

Installeer hier de nieuwste Azure Storage Explorer-bits: [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), nu ondersteunen we Windows, Linux en MAC-versie.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

1. Klik nadat u **Azure Storage Explorer** heb geïnstalleerd op het pictogram **invoegtoepassing** aan de linkerkant, zoals in de volgende afbeelding.
       
   ![Pictogram invoegtoepassing](./media/storage-explorer/plug-in-icon.png)
 
2. Selecteer **Een Azure-account toevoegen** en klik op **Aanmelden**.

   ![Verbinding maken met Azure-abonnement](./media/storage-explorer/connect-to-azure-subscription.png)

2. Selecteer **Aanmelden** in het **aanmeldingsvenster van Azure** en voer uw Azure-referenties in.

    ![Aanmelden](./media/storage-explorer/sign-in.png)

3. Selecteer uw abonnement in de lijst en klik op **Toepassen**.

    ![Toepassen](./media/storage-explorer/apply-subscription.png)

    Het Verkenner-venster wordt bijgewerkt en de accounts in het geselecteerde abonnement worden weergegeven.

    ![Lijst met accounts](./media/storage-explorer/account-list.png)

    U hebt uw **Azure Cosmos DB-account** gekoppeld aan uw Azure-abonnement.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Verbinding maken met Azure Cosmos DB met behulp van een verbindingsreeks

Een andere manier van verbinding maken met een Azure Cosmos DB is het gebruik van een verbindingsreeks. Gebruik de volgende stappen om verbinding te maken met behulp van een verbindingsreeks.

1. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Azure Cosmos DB-accounts**, en kies **Verbinding maken met Azure Cosmos DB...**

    ![Verbinding maken met Azure Cosmos DB met een verbindingsreeks](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. Kies de juiste **Standaardervaring** voor uw accounttype,<!--either--> **DocumentDB**<!--or **MongoDB**-->, plak uw **verbindingsreeks**en klik op **OK** om verbinding te maken met uw Azure Cosmos DB-account. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string) voor informatie over het ophalen van de verbindingsreeks.

    ![Verbindingsreeks](./media/storage-explorer/connection-string.png)

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
Klik met de rechtermuisknop op het Azure Cosmos DB-account, kies **Database maken**, voer de naam van de database in en druk op **Enter**.

![Database maken](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Een database verwijderen
Klik met de rechtermuisknop op de database, klik op **Database verwijderen** en klik in het pop-upvenster op **Ja**. Het databaseknooppunt wordt verwijderd en het Azure Cosmos DB die Cosmos-account wordt automatisch vernieuwd.

![Database1 verwijderen](./media/storage-explorer/delete-database1.png)  

![Database2 verwijderen](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Een verzameling maken
Klik met de rechtermuisknop op de database, kies **Verzameling maken**en geef de volgende informatie op: **Verzameling-id**, **Opslagcapaciteit**, enzovoort. Klik op **OK** om de regel op te slaan. Zie [Ontwerp voor partitioneren](partition-data.md#designing-for-partitioning) voor informatie over partitiesleutelinstellingen.

Als bij het maken van een verzameling een partitiesleutel wordt gebruikt, kan de partitiesleutelwaarde van de verzameling niet meer worden gewijzigd.

![Verzameling1 maken](./media/storage-explorer/create-collection.png)

![Verzameling2 maken](./media/storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Een verzameling verwijderen
Klik met de rechtermuisknop op de verzameling, klik op **Verzameling verwijderen**en klik vervolgens op **Ja** in het pop-upvenster. 

Het verzamelingsknooppunt wordt verwijderd en de database wordt automatisch vernieuwd.  

![Verzameling verwijderen](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Documentbeheer

#### <a name="create-and-modify-documents"></a>Documenten maken en wijzigen
Als u een nieuw document wilt maken, opent u **Documenten** in het linker venster, klikt u op **Nieuw document**, bewerkt u de inhoud in het rechter deelvenster en klikt u vervolgens op **Opslaan**. U kunt ook een bestaand document bijwerken en vervolgens op **Opslaan** klikken. Wijzigingen kunnen worden genegeerd door op **Negeren** te klikken.

![Document](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Een document verwijderen
Klik op de knop **Verwijderen** om het geselecteerde document te verwijderen.
#### <a name="query-for-documents"></a>Query voor documenten
Bewerk het documentfilter door een [SQL query](sql-api-sql-query.md) in te voeren en op **Toepassen** te klikken.

![Filteren](./media/storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Opgeslagen procedures, triggers en UDF's beheren
* Als u een opgeslagen procedure wilt maken, klikt u met de rechtermuisknop in de linkerboomstructuur op **Opgeslagen procedure**, kiest u **Opgeslagen procedure maken**, voert u links een naam in, typt u de opgeslagen-procedurescripts in het rechter venster en klikt u vervolgens op **Maken**. 
* U kunt ook bestaande opgeslagen procedures bewerken door erop te dubbelklikken, de procedure te bewerken en op **Bijwerken** te klikken om de wijzigingen op te slaan of op **Negeren** om de wijziging te annuleren.

![Opgeslagen procedure](./media/storage-explorer/stored-procedure.png)

* De bewerkingen voor **Triggers** en **UDF** zijn vergelijkbaar met die voor **Opgeslagen procedures**.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de volgende video om te zien hoe u Azure Cosmos DB in Azure Storage Explorer kunt gebruiken: [Azure Cosmos DB gebruiken in Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Meer informatie over Opslagverkenner en verbinding maken met meer services vindt u in [Aan de slag met Storage Explorer (preview)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).


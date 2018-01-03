---
title: Azure Cosmos DB in Azure Opslagverkenner beheren
description: Informatie over het beheren van Azure Cosmos-database in Azure Storage Explorer.
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
ms.openlocfilehash: fa91630674151ac434c7f97fa2795e47bb38f16f
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Beheren van Azure Cosmos DB in Azure Opslagverkenner (Preview)

Met behulp van Azure DB die Cosmos in Azure Storage Explorer, kunnen gebruikers Azure Cosmos DB entiteiten beheren, het manipuleren van gegevens, het bijwerken van opgeslagen procedures en triggers samen met andere Azure entiteiten zoals Storage-blobs en wachtrijen. U kunt nu hetzelfde hulpprogramma gebruiken voor het beheren van uw andere Azure entiteiten op één plek. Op dit moment ondersteunt Azure Storage Explorer SQL <!--and MongoDB--> accounts. De Azure Storage Explorer werkt niet met de Azure Cosmos DB lokale Emulator. 

In dit artikel kunt u informatie over het gebruik van Storage Explorer voor het beheren van Azure Cosmos DB.


## <a name="prerequisites"></a>Vereisten

Een Azure DB die Cosmos-account voor de SQL-API <!--or MongoDB API-->. Als u geen account hebt, kunt u een in de Azure portal maken zoals beschreven in [Azure Cosmos DB: bouwen van een SQL-API-web-app met .NET- en de Azure-portal](create-sql-api-dotnet.md).

## <a name="installation"></a>Installatie

Installeer de nieuwste Azure Storage Explorer-bits hier: [Azure Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/), nu we ondersteuning voor Windows, Linux en MAC-versie.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

1. Na de installatie van de **Azure Opslagverkenner**, klikt u op de **invoegtoepassing** pictogram aan de linkerkant zoals weergegeven in de volgende afbeelding.
       
   ![Pictogram invoegtoepassing](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/plug-in-icon.png)
 
2. Selecteer **een Azure-Account toevoegen**, en klik vervolgens op **aanmelden**.

   ![Verbinding maken met Azure-abonnement](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-azure-subscription.png)

2. In de **aanmelden bij Azure** dialoogvenster, **aanmelden**, en voer uw Azure-referenties.

    ![Aanmelden](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/sign-in.png)

3. Selecteer uw abonnement in de lijst en klik vervolgens op **toepassen**.

    ![Aanvragen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/apply-subscription.png)

    Het Verkenner-deelvenster updates en de accounts in het geselecteerde abonnement worden weergegeven.

    ![Lijst met accounts](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/account-list.png)

    U hebt verbonden met uw **Azure Cosmos DB account** naar uw Azure-abonnement.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Maak verbinding met Azure Cosmos-database met behulp van een verbindingsreeks

Een andere manier van verbinding maken met een Cosmos Azure DB is het gebruik van een verbindingsreeks. Gebruik de volgende stappen uit om te verbinden via een verbindingsreeks.

1. Vinden **Local en gekoppelde** in het linkerdeelvenster, met de rechtermuisknop op **Azure Cosmos DB Accounts**, kies **verbinding maken met Azure Cosmos DB...**

    ![verbinding maken met Azure Cosmos DB met verbindingsreeks](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-db-by-connection-string.png)

2. Kies de juiste **ervaring standaard** voor uw accounttype <!--either--> **DocumentDB** <!--or **MongoDB**-->, plakken uw **verbindingsreeks**, en vervolgens Klik op **OK** om te verbinden Azure DB die Cosmos-account. Zie voor informatie over het ophalen van de verbindingsreeks, [de verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![verbindingsreeks](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Azure DB Cosmos-bronbeheer

U kunt een Azure DB die Cosmos-account beheren op deze manier volgende bewerkingen:
* Open de account in de Azure portal
* Voeg de resource toe aan de lijst met Snelweergavetoegang
* Zoek-en vernieuwen
* Databases maken en verwijderen
* Verzamelingen maken en verwijderen
* Maken, bewerken, verwijderen en filteren
* Opgeslagen procedures, triggers en gebruiker gedefinieerde functies beheren

### <a name="quick-access-tasks"></a>Taken voor snelle toegang

Door met de rechtermuisknop op een abonnement in het deelvenster Explorer, kunt u veel snelle actietaken uitvoeren:

* Met de rechtermuisknop op een Cosmos-DB Azure-account of een database, kunt u **openen in de Portal** en beheren van de resource in de browser op de Azure-portal.

     ![Open in de portal](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/open-in-portal.png)

* U kunt ook Azure DB die Cosmos-account, de database, de verzameling toevoegen **Snelweergavetoegang**.
* **Zoek hier** kunt zoeken met trefwoorden onder het geselecteerde pad.

    ![Hier zoeken](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Database en verzameling management
#### <a name="create-a-database"></a>Een database maken 
Met de rechtermuisknop op de Azure DB die Cosmos-account, kiest u **Create Database**, invoer databasenaam van de en druk op **Enter** te voltooien.

![Database maken](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Een database verwijderen
Met de rechtermuisknop op de database, klikt u op **Database verwijderen**, en klik op **Ja** in het pop-upvenster. De databaseknooppunt is verwijderd en de Azure DB die Cosmos-account automatisch worden vernieuwd.

![Database1 verwijderen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database1.png)  

![Database2 verwijderen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Een verzameling maken
Met de rechtermuisknop op uw database, kiest u **verzameling maken**, en geef vervolgens de volgende informatie zoals **verzamelings-ID**, **opslagcapaciteit**, enzovoort. Klik op **OK** om de regel op te slaan. Zie voor informatie over belangrijke partitie-instellingen, [ontwerp voor het partitioneren van](partition-data.md#designing-for-partitioning).

Als een partitiesleutel wordt gebruikt bij het maken van een verzameling, nadat de maken is voltooid, kan de waarde voor de partitiesleutel kan niet worden gewijzigd voor de verzameling.

![Collection1 maken](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection.png)

![Collection2 maken](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Een verzameling verwijderen
Met de rechtermuisknop op de verzameling, klikt u op **verzameling verwijderen**, en klik vervolgens op **Ja** in het pop-upvenster. 

De verzameling-knooppunt is verwijderd en de database automatisch worden vernieuwd.  

![Verzameling verwijderen](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Documentbeheer

#### <a name="create-and-modify-documents"></a>Documenten maken en wijzigen
Voor het maken van een nieuw document openen **documenten** in het linkerdeelvenster klikt u op **Nieuw Document**, de inhoud in het rechterdeelvenster bewerken en klik vervolgens op **opslaan**. U kunt ook een bestaand document bijwerken en klik vervolgens op **opslaan**. Wijzigingen kunnen worden genegeerd door te klikken op **negeren**.

![Document](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/document.png)

#### <a name="delete-a-document"></a>Een document verwijderen
Klik op de **verwijderen** knop om te verwijderen van het geselecteerde document.
#### <a name="query-for-documents"></a>Query voor documenten
Het documentfilter bewerken door te voeren een [SQL-query](sql-api-sql-query.md) en klik vervolgens op **toepassen**.

![Filteren](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Opgeslagen procedures, triggers en UDF's beheren
* Voor het maken van een opgeslagen procedure in het linkerdeelvenster, met de rechtermuisknop op **opgeslagen Procedure**, kies **Create opgeslagen Procedure**, voer een naam in de linkerkant, typt u de opgeslagen procedure-scripts in het rechtervenster, en vervolgens Klik op **maken**. 
* U kunt ook bestaande opgeslagen procedures bewerken door te dubbelklikken op, om de update en klik vervolgens op **bijwerken** wilt opslaan of klik op **negeren** om de wijziging te annuleren.

![Opgeslagen procedure](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/stored-procedure.png)

* De bewerkingen voor **Triggers** en **UDF** zijn vergelijkbaar met die voor **opgeslagen Procedures**.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de volgende video om te zien hoe u Azure Cosmos-database in Azure Storage Explorer gebruiken: [Azure Cosmos-database gebruiken in Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Meer informatie over Opslagverkenner en verbinding maken met meer services in [aan de slag met Opslagverkenner (Preview)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).


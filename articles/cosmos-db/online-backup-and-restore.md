---
title: Automatische, online back-up en gegevens op aanvraag voor herstellen in Azure Cosmos DB
description: Dit artikel wordt beschreven hoe automatische, online back-up en op aanvraag gegevens werkt in Azure Cosmos DB herstellen.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 8b5b56e39e1b9830d5b998ace2a384d6878cd510
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041812"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Online back-up en gegevens op aanvraag voor herstellen in Azure Cosmos DB

Azure Cosmos DB maakt back-ups van uw gegevens automatisch met regelmatige intervallen. De automatische back-ups worden genomen zonder gevolgen voor de prestaties of beschikbaarheid van de databasebewerkingen. De back-ups apart zijn opgeslagen in een storage-service, en deze back-ups wereldwijd voor bescherming tegen regionale rampen zijn gerepliceerd. De automatische back-ups zijn nuttig in scenario's als u per ongeluk verwijderen of bijwerken van uw Azure Cosmos-account, een database of een container en het herstel van gegevens later nodig.

## <a name="automatic-and-online-backups"></a>Automatische en online back-ups

Met Azure Cosmos DB zijn niet alleen uw gegevens, maar ook de back-ups van uw gegevens maximaal redundante en tegen regionale rampen. De automatische back-ups op dat moment gaat om de vier uur en op elk gewenst moment tijd wordt opgelost, de meest recente twee back-ups worden opgeslagen. Als u hebt per ongeluk verwijderd of beschadigd van uw gegevens, moet u contact opnemen [ondersteuning van Azure](https://azure.microsoft.com/support/options/) binnen acht uur zodat het team van Azure Cosmos DB kunt u de gegevens herstelt vanaf de back-ups.

De back-ups zijn uitgevoerd zonder gevolgen voor de prestaties of beschikbaarheid van uw toepassing. Azure Cosmos DB kunt u gegevensback-up op de achtergrond zonder eventuele extra ingerichte doorvoer (ru's) verbruikt of die betrekking hebben op de prestaties en beschikbaarheid van uw database.

Azure Cosmos DB slaat automatische back-ups in Azure Blob-opslag terwijl de werkelijke gegevens lokaal aanwezig is in Azure Cosmos DB. Om te waarborgen van de lage latentie, de momentopname van de back-up wordt opgeslagen in Azure Blob-opslag in dezelfde regio als de huidige schrijfregio (of een van de schrijfregio's, hebt u een configuratie met meerdere masters) van uw Cosmos DB database-account. Voor bescherming tegen regionale na noodgevallen, wordt elke momentopname van de back-upgegevens in Azure Blob-opslag opnieuw gerepliceerd naar een andere regio via geo-redundante opslag (GRS). De regio waarnaar de back-up worden gerepliceerd is gebaseerd op de regio van de gegevensbron en de regionaal paar die zijn gekoppeld aan de bronregio. Zie voor meer informatie, de [lijst met geografisch redundante paren van Azure-regio's](../best-practices-availability-paired-regions.md) artikel. U kunt geen rechtstreeks toegang hebben tot deze back-up. Azure Cosmos DB wordt deze back-up alleen gebruiken als een back-up herstellen wordt gestart.
De volgende afbeelding ziet u hoe een Azure Cosmos-container met alle drie primaire fysieke partities in VS-West is een back-up in een externe Azure Blob Storage-account in VS-West en vervolgens gerepliceerd naar VS-Oost:

![Periodieke volledige back-ups van alle Cosmos DB-entiteiten in GRS Azure Storage](./media/online-backup-and-restore/automatic-backup.png)

## <a name="options-to-manage-your-own-backups"></a>Opties voor het beheren van uw eigen back-ups

Met Azure Cosmos DB SQL API-accounts, kunt u ook uw eigen back-ups behouden met behulp van een van de volgende methoden:

* Gebruik [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) gegevens periodiek te verplaatsen naar een opslag van uw keuze.

* Azure Cosmos DB gebruiken [wijzigingenfeed](change-feed.md) te lezen van gegevens periodiek voor volledige back-ups, evenals incrementele wijzigingen, en op te slaan in uw eigen opslag.

## <a name="backup-retention-period"></a>Bewaarperiode voor back-up

Azure Cosmos DB duurt momentopnamen van uw gegevens om de vier uur. Op elk moment worden alleen de laatste twee momentopnamen worden bewaard. Echter, als de container of de database wordt verwijderd, behoudt Azure Cosmos DB de bestaande momentopnamen van een bepaalde container of de database gedurende 30 dagen.

## <a name="restoring-data-from-online-backups"></a>Herstellen van gegevens uit de online back-ups

Per ongeluk verwijderen of wijzigen van gegevens kan gebeuren in een van de volgende scenario's:  

* Het gehele Azure-Cosmos-account is verwijderd

* Een of meer Azure Cosmos-databases zijn verwijderd

* Een of meer Azure Cosmos-containers zijn verwijderd

* Azure Cosmos-items (bijvoorbeeld documenten) in een container zijn verwijderd of gewijzigd. Dit specifieke geval wordt meestal aangeduid als 'beschadiging van gegevens'.

* De database van een gedeelde aanbieding of containers in een gedeelde aanbieding-database zijn verwijderd of beschadigd

Azure Cosmos DB kunt herstellen van gegevens in de bovenstaande scenario's. Het herstelproces wordt altijd gemaakt voor een nieuw Azure Cosmos-account voor het opslaan van de herstelde gegevens. De naam van het nieuwe account, indien niet opgegeven, heeft de indeling `<Azure_Cosmos_account_original_name>-restored1`. De laatste cijfers wordt verhoogd als meerdere herstelacties worden uitgevoerd. U kunt gegevens niet herstellen naar een vooraf gemaakte Azure-Cosmos-account.

Wanneer een Azure Cosmos-account wordt verwijderd, kunnen we de gegevens herstellen bij een account met dezelfde naam, mits de accountnaam niet gebruikt wordt. In dergelijke gevallen is het aanbevolen om het niet het account opnieuw maken nadat u hebt verwijderd, omdat deze niet alleen wordt voorkomen de herstelde gegevens dat voor het gebruik van dezelfde naam, maar ook het juiste account is uit de moeilijker te detecteren. 

Wanneer een Azure Cosmos-database wordt verwijderd, is het mogelijk om de gehele database of een subset van de containers in die database te herstellen. Het is ook mogelijk om te selecteren van containers voor databases en herstel ze en alle de herstelde gegevens wordt geplaatst in een nieuw Azure Cosmos-account.

Wanneer een of meer items in een container per ongeluk zijn verwijderd of gewijzigd (de gegevens beschadigd case), moet u om op te geven van de tijd om naar te herstellen. Tijd is van de essentie voor deze aanvraag. Omdat de container actief is, wordt nog steeds de back-up uitgevoerd, dus als u na de bewaarperiode (de standaardwaarde is de acht uur) wacht voordat de back-ups wordt overschreven. In het geval van verwijdert, worden uw gegevens worden niet meer opgeslagen omdat ze niet worden overschreven door de back-cyclus. Back-ups voor de verwijderde databases of containers worden voor 30 dagen opgeslagen.

Als u de doorvoer op het databaseniveau van de inrichten (dat wil zeggen, waarbij een set van containers deelt de ingerichte doorvoer), het proces van back-up en herstel in dit geval gebeuren op het niveau van de volledige database en niet op het niveau van de afzonderlijke containers. In dergelijke gevallen kunt kan u een subset van containers om terug te zetten niet worden gebruikt.

## <a name="migrating-data-to-the-original-account"></a>Gegevens migreren naar de oorspronkelijke account

Het voornaamste doel van het herstellen van de gegevens is een manier om te herstellen van gegevens die u niet verwijderen of per ongeluk wijzigen. Daarom is het raadzaam eerst de inhoud van de herstelde gegevens om te controleren of dat deze bevat wat u verwacht te inspecteren. Vervolgens worden gebruikt voor het migreren van de gegevens terug naar de primaire-account. Hoewel het mogelijk met gebruik van de herstelde account als de live-account, is het geen aanbevolen optie als u werkbelastingen voor productie.  

Hier volgen de verschillende manieren om gegevens te migreren naar de oorspronkelijke Azure Cosmos-account:

* Met behulp van [hulpprogramma voor gegevensmigratie Cosmos DB](import-data.md)
* Met behulp van [Azure Data Factory]( ../data-factory/connector-azure-cosmos-db.md)
* Met behulp van [wijzigingenfeed](change-feed.md) in Azure Cosmos DB 
* Aangepaste code schrijven

Verwijder de teruggezette gebruikersaccounts zodra u klaar bent migreren, omdat ze de lopende kosten in rekening gebracht.

## <a name="next-steps"></a>Volgende stappen

Vervolgens vindt u informatie over hoe u gegevens uit een Azure Cosmos-account te herstellen of informatie over het migreren van gegevens naar een Azure Cosmos-account

* Om te maken van een herstelpunt aanvragen, contact op met ondersteuning voor Azure, [bestand een ticket van de Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Het herstellen van gegevens uit een Azure Cosmos-account](how-to-backup-and-restore.md)
* [Gebruik Cosmos DB-wijzigingenfeed](change-feed.md) om gegevens te verplaatsen met Azure Cosmos DB.
* [Gebruik Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) om gegevens te verplaatsen met Azure Cosmos DB.


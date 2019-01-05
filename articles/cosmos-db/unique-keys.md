---
title: Unieke sleutels in Azure Cosmos DB gebruiken
description: Informatie over het gebruik van unieke sleutels in uw Azure Cosmos DB-database
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sngun
ms.openlocfilehash: 20e5c96110f07d8eaec218ed167c87a48fd65782
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037375"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Unieke sleutels in Azure Cosmos DB

Unieke sleutels bieden u de mogelijkheid om een laag van de integriteit van gegevens toevoegen aan een Cosmos-container. Bij het maken van een Cosmos-container maakt u een unieke sleutel beleid. Met unieke sleutels u ervoor zorgen dat een of meer waarden in een logische partitie (u kunt garanderen uniekheid per [partitiesleutel](partition-data.md)). Als u een container met een unieke sleutel beleid hebt gemaakt, wordt voorkomen dat het maken van een nieuwe (of bijgewerkte) dubbele items in een logische partitie, zoals opgegeven door de unieke key-beperking. De partitiesleutel gecombineerd met de unieke sleutel garanties Uniekheid van een item binnen het bereik van de container.

Neem bijvoorbeeld een Cosmos-container met e-mailadres als unieke key-beperking en `CompanyID` als de partitiesleutel. Door het configureren van de gebruiker e-mailadres dat een unieke sleutel, zorgt u ervoor elk item heeft een uniek e-mailadres binnen een bepaalde `CompanyID`. Twee objecten kunnen niet worden gemaakt met dubbele e-mailadressen en de dezelfde waarde voor de partitiesleutel.  

Als u bieden gebruikers de mogelijkheid om te maken van meerdere items met hetzelfde e-mailadres, maar niet de dezelfde naam van de eerste, laatste en e-mailadres wilt, kunt u extra paden kan toevoegen aan het beleid voor unieke sleutels. In plaats van het maken van een unieke sleutel op basis van het e-mailadres, kunt u ook een unieke sleutel maken met een combinatie van de voornaam, achternaam en e-mailadres (een samengestelde unieke sleutel). In dit geval elke unieke combinatie van de drie waarden binnen een bepaalde `CompanyID` is toegestaan. De container kan bijvoorbeeld items met de volgende waarden waarin de unieke key-beperking is naleven van elk item bevatten.

|CompanyID|Voornaam|Achternaam|E-mailadres|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Als u een ander item met de combinaties die worden vermeld in de bovenstaande tabel invoegen probeert, ontvangt u een foutbericht dat aangeeft dat de unieke key-beperking niet is voldaan. Ontvangt u een van beide "Resource met de opgegeven ID of naam bestaat al' of 'Resource met de opgegeven ID, naam of unieke index al bestaat' als een bericht geretourneerd.  

## <a name="defining-a-unique-key"></a>Een unieke sleutel definiëren

Alleen bij het maken van een Cosmos-container, kunt u unieke sleutels definiëren. Een unieke sleutel is afgestemd op een logische partitie. In het vorige voorbeeld, als u de container op basis van de postcode partitioneren verschijnen hebben dubbele items in elke logische partitie. Bij het maken van unieke sleutels zijn, houd rekening met de volgende eigenschappen:

* U kunt een bestaande container voor het gebruik van een andere unieke sleutel niet bijwerken. Nadat een container is gemaakt met een unieke sleutel beleid, kunnen het beleid met andere woorden, kan niet worden gewijzigd.

* Als u een unieke sleutel voor een bestaande container instellen wilt, hebt u een nieuwe container maken met de unieke key-beperking en de juiste hulpprogramma voor gegevensmigratie gebruiken om te verplaatsen van de gegevens van bestaande container naar de nieuwe container. Voor SQL-containers, gebruikt u de [hulpprogramma voor gegevensmigratie](import-data.md) om gegevens te verplaatsen. Gebruik voor MongoDB-containers, [mongoimport.exe of mongorestore.exe](mongodb-migrate.md) om gegevens te verplaatsen.

* Een unieke sleutel beleid kan maximaal 16 pad waarden hebben (bijvoorbeeld: /firstName, /lastName, / adres/postcode). Het beleid voor elke unieke sleutels kan maximaal 10 unique key-beperkingen of combinaties en de gecombineerde paden voor elke beperking unique-index mag niet groter zijn dan 60 bytes. In het vorige voorbeeld, voornaam, achternaam en e-mailadres bij elkaar zijn slechts één beperking en drie van de 16 mogelijke paden wordt gebruikt.

* Wanneer een container een unieke sleutel beleid, aanvraag-eenheid (RU) kosten in rekening gebracht heeft voor het maken, zijn bijwerken en verwijderen een item enigszins hoger.

* Sparse unieke sleutels worden niet ondersteund. Als er enkele waarden uniek pad ontbreken, worden ze behandeld als null-waarden, die Neem deel aan de beperking voor uniekheid. Daarom kan kan er alleen worden één item met null-waarde om te voldoen aan deze beperking.

* De unieke sleutelnamen zijn hoofdlettergevoelig. Neem bijvoorbeeld een container met de unieke key-beperking ingesteld op /address/zipcode. Als uw gegevens een veld met de naam postcode heeft, voegt het Cosmos DB 'null' als de unieke sleutel, omdat "Postcode" niet hetzelfde als "Postcode is". Vanwege deze hoofdlettergevoeligheid kunnen niet alle records met ZipCode worden ingevoegd omdat de 'null' dubbele strijdig zijn met de unieke key-beperking.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [logische partities](partition-data.md)

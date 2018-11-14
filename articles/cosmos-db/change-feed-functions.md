---
title: Het gebruik van Azure Cosmos DB-wijzigingenfeed met Azure Functions
description: Gebruik Azure Cosmos DB-wijzigingenfeed met Azure Functions
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rimman
ms.openlocfilehash: ece1c123cad8403358dcc3b3626bf4b2cd756e25
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629006"
---
# <a name="how-to-use-azure-cosmos-db-change-feed-with-azure-functions"></a>Het gebruik van Azure Cosmos DB-wijzigingenfeed met Azure Functions

Als u Azure Functions, de eenvoudigste manier om verbinding maken met de wijzigingenfeed is om toe te voegen een [Azure Cosmos DB-trigger](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger) aan uw Azure Functions-app. Wanneer u een Cosmos DB-trigger in een Azure Functions-app maakt, u verbinding maken met de Cosmos-container selecteren en de functie wordt geactiveerd wanneer u iets in de container worden gewijzigd.

Triggers kunnen worden gemaakt in de Azure Functions-portal of in de Azure Cosmos DB-portal of programmatisch. Zie voor meer informatie, [serverless database computing met behulp van Azure Cosmos DB en Azure Functions](serverless-computing-database.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region"></a>Hoe configureer ik Azure functions om te lezen uit een bepaalde regio?

Het is mogelijk voor het definiëren van de [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations) bij het gebruik van de Azure Cosmos DB-trigger om op te geven van een lijst met regio's. Het is dezelfde tijdens het aanpassen van de ConnectionPolicy, zodat de trigger gelezen uit de gewenste regio's. In het ideale geval wilt u lezen van de dichtstbijzijnde regio waar uw Azure-functies wordt geïmplementeerd.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Wat is de standaardgrootte van batches in Azure Functions?

De standaardwaarde is 100 items voor elke aanroep van Azure Functions. Dit nummer is echter worden geconfigureerd in de function.json-bestand. Hier is voltooid [lijst met configuratieopties](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Als u lokaal ontwikkelt, werkt u de toepassingsinstellingen in het bestand local.settings.json.

### <a name="i-am-monitoring-a-container-and-reading-its-change-feed-however-i-dont-get-all-the-inserted-documents-some-items-are-missing"></a>Ik ben een container monitoring en de wijzigingenfeed lezen, maar ik krijg geen de ingevoegde documenten, enkele items ontbreken?

Zorg ervoor dat er geen andere Azure-functie voor het lezen van de dezelfde container met de dezelfde lease-container. De ontbrekende documenten worden verwerkt door de andere Azure-functies die ook van de dezelfde lease gebruikmaken.

Dus als u meerdere Azure Functions om te lezen dat hetzelfde wijzigingenfeed maakt, moet verschillende lease containers gebruiken of de configuratie "leasePrefix" gebruiken voor het delen van dezelfde container. Wanneer u change feed processor-bibliotheek kunt u meerdere exemplaren van uw Azure-functie starten en de SDK de documenten tussen verschillende exemplaren automatisch voor u wordt verdeeld.

### <a name="azure-cosmos-item-is-updated-every-second-and-i-dont-get-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Azure Cosmos-item per seconde is bijgewerkt, en ik niet alle wijzigingen in Azure Functions luisteren als u wilt wijzigen van de feed?

Azure Functions controleert de feed voor wijzigingen continu met een standaard maximale vertraging van vijf seconden wijzigen. Als er zijn geen wijzigingen in behandeling om te lezen, of als er wijzigingen in behandeling zijn nadat de trigger wordt toegepast, wordt de functie deze meteen gelezen. Echter, als er geen wijzigingen in behandeling zijn, de functie wacht 5 seconden en of er meer wijzigingen.

Als uw document meerdere wijzigingen in het hetzelfde interval die nodig de Trigger was worden gecontroleerd op nieuwe wijzigingen ontvangt, ontvangt u mogelijk de nieuwste versie van het document en niet de tussenliggende uit.

Als u wilt aan te vragen voor feed voor minder dan vijf seconden wijzigen, bijvoorbeeld voor elke seconde u kunt de polling-tijd 'feedPollDelay' configureren, Zie [de volledige configuratie](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations). Dit is gedefinieerd in milliseconden met een standaardwaarde van 5000. Polling voor minder dan 1 seconde mogelijk is, maar niet aanbevolen wordt omdat begint u met behulp van meer CPU en geheugen.

### <a name="can-multiple-azure-functions-read-one-containers-change-feed"></a>Kan meerdere Azure-functies van één container wijzigingenfeeds lezen?

Ja. Meerdere Azure-functies kunnen dezelfde container wijzigingenfeeds lezen. De Azure-functies moet echter een afzonderlijke "leaseCollectionPrefix" gedefinieerd hebben.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-in-a-batch-of-10-documents-and-i-get-an-error-at-seventh-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-document-ie-seventh-document-in-my-next-feed"></a>Als ik verwerking change feed met behulp van Azure Functions, in een batch van 10 documenten, en er een foutbericht op zevende Document verschijnt. De laatste drie documenten worden in dat geval niet verwerkt hoe kan ik start verwerking uit het document (Internet Explorer, zevende document) in de volgende feed?

Voor het afhandelen van de fout, is het aanbevolen patroon inpakken van uw code met probeer catch-blok en, als u bent iteratie van de lijst met documenten, elke herhaling verpakken in een eigen probeer-catch-blok. Bekijk de fout en plaats van dat document in een wachtrij (dead-letter uitvoeren) en definieer vervolgens logica voor het werken met de documenten die de fout heeft geproduceerd. Met deze methode als u een batch 200-document en slechts één document is mislukt, hebt hoeft u niet te weggooien van de hele batch.

In het geval van fout, moet u het controlepunt terug naar begin niet terugspoelen zal anders u kunt houden ophalen die documenten uit wijzigingenfeed. Denk eraan dat change feed houdt de laatste schermopname van de documenten, omdat u de laatste module kunnen verloren gaan de vorige momentopname van het document. wijzigingenfeed blijft slechts één van de laatste versie van het document en in andere processen kunnen afkomstig zijn en wijzigen van het document.

Als u uw code herstellen houden, vindt u binnenkort geen documenten op dead-letter-wachtrij. Azure Functions wordt automatisch aangeroepen door change feed system en controlepunt intern wordt beheerd door Azure-functie. Als u wilt het controlepunt terugdraaien en elk aspect van het beheren, moet u rekening houden met behulp van de wijziging feed Processor-SDK.

### <a name="are-there-any-extra-costs-for-using-the-azure-cosmos-db-trigger"></a>Zijn er extra kosten voor het gebruik van de Azure Cosmos DB-Trigger?

De Azure Cosmos DB-Trigger maakt intern gebruik van de Change Feed Processor-bibliotheek. Daarom is er een extra verzameling met de naam van de verzameling Leases voor het onderhouden van de status en gedeeltelijke controlepunten vereist. Deze statusbeheer is nodig om te kunnen dynamisch schalen en doorgaan als u wilt stoppen van uw Azure-functies en gaan met de verwerking op een later tijdstip. Zie voor meer informatie, [over het werken met change feed processor-bibliotheek](change-feed-processor.md).

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over de wijzigingenfeed in de volgende artikelen:

* [Overzicht van de wijzigingenfeed](change-feed.md)
* [Manieren om te lezen wijzigingenfeed](read-change-feed.md)
* [Met behulp van de change feed processor-bibliotheek](change-feed-processor.md)
* [Over het werken met change feed processor-bibliotheek](change-feed-processor.md)
* [Serverless database computing met behulp van Azure Cosmos DB en Azure Functions](serverless-computing-database.md)
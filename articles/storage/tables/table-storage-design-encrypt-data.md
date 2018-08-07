---
title: Versleutelen van gegevens van Azure storage-tabel | Microsoft Docs
description: Meer informatie over versleuteling van de tabel gegevens in Azure storage.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.component: tables
ms.openlocfilehash: 8a7b61705f6efc5a76212fdd1345a022f58e0686
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526463"
---
# <a name="encrypt-table-data"></a>Versleutelen van gegevens in een tabel
De clientbibliotheek voor .NET Azure Storage ondersteunt versleuteling van de entiteitseigenschappen tekenreeks voor invoegen en vervangbewerkingen. De gecodeerde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na ontsleuteling.    

Gebruikers moeten de eigenschappen moeten worden versleuteld opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een van beide op te geven een kenmerk [EncryptProperty] (voor POCO-entiteiten die zijn afgeleid van TableEntity) of een conflictoplosser versleuteling in Aanvraagopties. Een oplossing versleuteling is een gemachtigde die ervoor zorgt dat een partitiesleutel, een rijsleutel en een naam van de eigenschap retourneert een Booleaanse waarde die aangeeft of deze eigenschap moet worden versleuteld. Tijdens het versleutelen gebruikt de clientbibliotheek van deze informatie om te bepalen of voor het versleutelen van een eigenschap tijdens het schrijven naar de kabel. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld, als X, vervolgens versleutelen eigenschap A; anders eigenschappen A en B. te versleutelen) Het is niet nodig is om deze informatie tijdens het lezen of uitvoeren van query's entiteiten geven.

## <a name="merge-support"></a>Ondersteuning voor samenvoegen

Samenvoegen wordt momenteel niet ondersteund. Omdat een subset met eigenschappen kan zijn gecodeerd eerder met behulp van een andere sleutel, wordt gewoon samenvoegen van de nieuwe eigenschappen en het bijwerken van de metagegevens van de resulteert in gegevensverlies. Samenvoegen van een vereist extra service aanroepen om te lezen de reeds bestaande entiteit van de service of met behulp van een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.     

Zie voor meer informatie over het versleutelen van gegevens uit de apparaattabel [Client-Side-versleuteling en Microsoft Azure Storage in Azure Key Vault](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Waardoor relaties worden gemaakt](table-storage-design-modeling.md)
- [Waardoor relaties worden gemaakt](table-storage-design-modeling.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
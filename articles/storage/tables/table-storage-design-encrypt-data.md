---
title: Azure storage tabelgegevens versleutelen | Microsoft Docs
description: Meer informatie over versleuteling van de tabel gegevens in Azure-opslag.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/11/2018
ms.author: sngun
ms.openlocfilehash: 082e8a54cc8625a4bbdea2157f73874dbc86fde2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660815"
---
# <a name="encrypt-table-data"></a>Tabelgegevens versleutelen
De .NET Azure Storage-clientbibliotheek biedt ondersteuning voor versleuteling van entiteitseigenschappen tekenreeks voor invoegen en vervang bewerkingen. De versleutelde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na de decodering.    

Gebruikers moeten de eigenschappen moeten worden gecodeerd opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een kenmerk [EncryptProperty] (voor POCO-entiteiten die zijn afgeleid van TableEntity) of een omzetter versleuteling in de aanvraag-opties. Een oplossing versleuteling is een gemachtigde die ervoor zorgt dat een partitiesleutel, rijsleutel en de naam van eigenschap retourneert een Booleaanse waarde die aangeeft of de eigenschap die moet worden versleuteld. Tijdens het versleutelen gebruikt de clientbibliotheek van deze informatie om te bepalen of voor het versleutelen van een eigenschap tijdens het schrijven naar de kabel. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld als X, vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en B.) Het is niet nodig om deze informatie tijdens het lezen of het uitvoeren van query's entiteiten te geven.

## <a name="merge-support"></a>Ondersteuning voor samenvoegen

Samenvoegen wordt momenteel niet ondersteund. Omdat een subset van eigenschappen mogelijk zijn versleuteld eerder met een andere sleutel, resulteert gewoon de nieuwe eigenschappen samenvoegen en bijwerken van de metagegevens van de gegevens verloren gaan. Samenvoegen van een vereist extra service aanroepen om te lezen van de vooraf bestaande entiteit van de service of met een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.     

Zie voor meer informatie over het coderen van tabelgegevens [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Volgende stappen

- [Ontwerppatronen voor tabel](table-storage-design-patterns.md)
- [Relaties te modelleren](table-storage-design-modeling.md)
- [Relaties te modelleren](table-storage-design-modeling.md)
- [Ontwerp voor wijziging van gegevens](table-storage-design-for-modification.md)
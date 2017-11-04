---
title: Inleiding tot Azure-Stack storage
description: Meer informatie over Azure-Stack-opslag
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 092aba28-04bc-44c0-90e1-e79d82f4ff42
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: 8777aa486a627cf8b2d8ba443e115638354d10da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-stack-storage"></a>Inleiding tot Azure-Stack storage

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="overview"></a>Overzicht
Stack aan Azure Storage is een set van opslagservices cloud met inbegrip van Blobs, tabellen en wachtrijen die consistent met Azure Storage-services zijn.

## <a name="azure-stack-storage-services"></a>Azure Storage-Stack-services
Azure Stack-opslag biedt de volgende drie services:

* **Blob Storage** 

    BLOB storage worden ongestructureerde objectgegevens opgeslagen. Een blob kan elk type tekst of binaire gegevens zijn, zoals een document, mediabestand of toepassingsinstallatieprogramma.
* **Table Storage** 

    Table storage worden gestructureerde gegevenssets opgeslagen. Table Storage is een gegevensarchief met NoSQL-sleutelkenmerk, waarmee snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens mogelijk is.
* **Queue Storage** 

    Queue storage biedt betrouwbare berichten voor de verwerking van de werkstroom en voor communicatie tussen onderdelen van cloud-services.

Een Azure-Stack storage-account is een veilig account waarmee u toegang tot services in Azure Stack Storage. Uw opslagaccount biedt een unieke naamruimte voor uw opslagresources. Het volgende diagram toont de relaties tussen de Stack van Azure storage-resources in een opslagaccount:

![Overzicht van Azure Storage-Stack](media/azure-stack-storage-overview/AzureStackStorageOverview.png)


### <a name="blob-storage"></a>Blob Storage

Blob storage biedt een effectieve en schaalbare oplossing voor gebruikers met een grote hoeveelheid ongestructureerde objectgegevens op te slaan in de cloud. U kunt Blob Storage gebruiken voor het opslaan van inhoud zoals:

* Documenten
* Sociale gegevens, zoals foto's, video's, muziek en blogs
* Back-ups van bestanden, computers, databases en apparaten
* Afbeeldingen en tekst voor webtoepassingen
* Configuratiegegevens voor cloudtoepassingen
* Big data, zoals logboeken en andere grote gegevenssets

Elke blob is georganiseerd in een container. Containers bieden ook een handige manier om beveiligingsbeleid toe te wijzen aan groepen objecten. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs, tot de limiet van opslagaccount bevatten.

BLOB storage biedt drie typen blobs: 
* **Blok-blobs** 

    Blok-blobs zijn geoptimaliseerd voor streaming en opslag van cloudobjecten en zijn een goede keuze voor het opslaan van documenten, mediabestanden, back-ups enzovoort.
* **Toevoeg-blobs** 

    Toevoeg-blobs zijn vergelijkbaar met blok-blobs, maar deze zijn geoptimaliseerd voor toevoegbewerkingen. Een toevoeg-blob kan alleen worden bijgewerkt door een nieuw blok aan het eind toe te voegen. Toevoeg-blobs zijn een goede keuze voor scenario's zoals logboekregistratie, waarbij alleen aan het eind van de blob nieuwe gegevens moeten worden geschreven.
* **Pagina-blobs** 

    Pagina-blobs zijn geoptimaliseerd voor de vertegenwoordiging van IaaS-schijven en ondersteuning van willekeurige schrijfbewerkingen die maximaal 1 TB groot is. Een virtuele machine van Azure-Stack aangesloten IaaS-schijf een VHD die is opgeslagen als een pagina-blob is.


### <a name="table-storage"></a>Table Storage
Moderne toepassingen vereisen vaak gegevensopslag met meer schaalbaarheid en flexibiliteit dan vorige softwaregeneraties vereisten. Table Storage biedt maximaal beschikbare, sterk schaalbare opslag, zodat uw toepassing automatisch kan schalen om aan de behoeften van de gebruiker te voldoen. Table Storage is de NoSQL-sleutelkenmerkopslag van Microsoft. Deze service heeft een ontwerp zonder schema, wat deze opslag onderscheidt van traditionele relationele databases. Met een schemaloze gegevensopslag kunt u uw gegevens eenvoudig aanpassen als de behoeften van uw toepassing veranderen. Table Storage is eenvoudig te gebruiken, zodat ontwikkelaars snel toepassingen kunnen maken.

Table Storage is een sleutelkenmerkopslag, wat betekent dat elke waarde in een tabel wordt opgeslagen met een getypeerde eigenschapsnaam. De naam van de eigenschap kan worden gebruikt om te filteren en selectiecriteria op te geven. Een verzameling eigenschappen en hun waarden vormen samen een entiteit. Omdat Table Storage schemaloos is, kunnen twee entiteiten in dezelfde tabel verschillende verzamelingen eigenschappen hebben. Dit kunnen verschillende typen eigenschappen zijn.

U kunt Table Storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. Voor de huidige internetgebaseerde toepassingen bieden NoSQL-databases zoals Table Storage een populaire alternatief voor traditionele relationele databases.

Een opslagaccount kan een onbeperkt aantal tabellen bevatten en een tabel kan een willekeurig aantal entiteiten, tot de capaciteitslimiet van het opslagaccount bevatten.

### <a name="queue-storage"></a>Queue Storage
Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt een betrouwbare berichtenoplossing voor asynchrone communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt ook ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Een opslagaccount kan een onbeperkt aantal wachtrijen bevatten en een wachtrij kan een onbeperkt aantal berichten, tot de capaciteitslimiet van het opslagaccount bevatten. Afzonderlijke berichten kunnen maximaal 64 kB groot zijn.

## <a name="next-steps"></a>Volgende stappen
* [Consistent zijn met Azure storage: verschillen en overwegingen](azure-stack-acs-differences.md)

* Zie voor meer informatie over Azure Storage, [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)


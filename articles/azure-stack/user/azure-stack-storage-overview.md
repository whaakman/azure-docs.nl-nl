---
title: Inleiding tot Azure Stack-opslag
description: Meer informatie over Azure Stack-opslag
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: 092aba28-04bc-44c0-90e1-e79d82f4ff42
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: 13fdf3257ed44212f45eeb3d2820a2022f54d777
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585235"
---
# <a name="introduction-to-azure-stack-storage"></a>Inleiding tot Azure Stack-opslag

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="overview"></a>Overzicht

Azure Stack-opslag is een set van cloud storage services met Blobs, tabellen en wachtrijen die consistent met Azure Storage-services zijn.

## <a name="azure-stack-storage-services"></a>Azure Stack-Storage-services

Azure Stack-opslag biedt de volgende drie services:

- **Blob Storage**

    BLOB storage worden ongestructureerde objectgegevens opgeslagen. Een blob kan elk type tekst of binaire gegevens zijn, zoals een document, mediabestand of toepassingsinstallatieprogramma.

- **Table Storage**

    Table storage worden gestructureerde gegevenssets opgeslagen. Table Storage is een gegevensarchief met NoSQL-sleutelkenmerk, waarmee snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens mogelijk is.

- **Queue Storage**

    Queue storage biedt betrouwbare berichten voor werkstroomverwerking en voor communicatie tussen onderdelen van cloudservices.

Een storage-account van Azure Stack is een veilig account waarmee u toegang tot services in Azure Stack-opslag. Uw opslagaccount biedt een unieke naamruimte voor uw opslagresources. Het volgende diagram toont de relaties tussen de Azure Stack-storage-resources in een storage-account:

![Overzicht van Azure Stack-Storage](media/azure-stack-storage-overview/AzureStackStorageOverview.png)

### <a name="blob-storage"></a>Blob Storage

Blob-opslag biedt voor gebruikers met een grote hoeveelheid ongestructureerde objectgegevens op te slaan in de cloud, een doeltreffende en schaalbare oplossing. U kunt blob storage gebruiken voor het opslaan van inhoud, zoals:

- Documenten
- Sociale gegevens, zoals foto's, video's, muziek en blogs
- Back-ups van bestanden, computers, databases en apparaten
- Afbeeldingen en tekst voor webtoepassingen
- Configuratiegegevens voor cloudtoepassingen
- Big data, zoals logboeken en andere grote gegevenssets

Elke blob is georganiseerd in een container. Containers bieden ook een handige manier om beveiligingsbeleid toe te wijzen aan groepen objecten. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs, tot de limiet van storage-account bevatten.

BLOB storage biedt drie typen blobs:

- **Blok-blobs**

    Blok-blobs zijn geoptimaliseerd voor het streamen en opslag van cloudobjecten en zijn een goede keuze voor het opslaan van documenten, mediabestanden, back-ups en enzovoort.

- **Toevoeg-blobs**

    Toevoeg-blobs zijn vergelijkbaar met blok-blobs, maar deze zijn geoptimaliseerd voor toevoegbewerkingen. Een toevoeg-blob kan alleen worden bijgewerkt door een nieuw blok aan het eind toe te voegen. Toevoeg-blobs zijn een goede keuze voor scenario's zoals logboekregistratie, waarbij alleen aan het eind van de blob nieuwe gegevens moeten worden geschreven.

- **Pagina-blobs**

    Pagina-blobs zijn geoptimaliseerd voor de vertegenwoordiging van IaaS-schijven en ondersteuning van willekeurige schrijfbewerkingen die maximaal 1 TB groot is. Een virtuele machine van Azure Stack aangesloten IaaS-schijf een VHD die is opgeslagen als pagina-blob is.

### <a name="table-storage"></a>Table Storage

Moderne toepassingen vereisen vaak gegevensopslag met meer schaalbaarheid en flexibiliteit dan vorige softwaregeneraties vereisten. Table Storage biedt maximaal beschikbare, sterk schaalbare opslag, zodat uw toepassing automatisch kan schalen om aan de behoeften van de gebruiker te voldoen. Tabelopslag is van Microsoft NoSQL-sleutel/kenmerkopslag--deze heeft een schemaloos ontwerp, wat deze verschillende van traditionele relationele databases. Met een schemaloze gegevensopslag kunt u uw gegevens eenvoudig aanpassen als de behoeften van uw toepassing veranderen. Table Storage is eenvoudig te gebruiken, zodat ontwikkelaars snel toepassingen kunnen maken.

Tabelopslag is een sleutelkenmerkopslag, wat betekent dat elke waarde in een tabel met een getypeerde eigenschapsnaam is opgeslagen. De naam van de eigenschap kan worden gebruikt om te filteren en selectiecriteria op te geven. Een verzameling eigenschappen en hun waarden vormen samen een entiteit. Omdat table storage schemaloos is, twee entiteiten in dezelfde tabel verschillende verzamelingen eigenschappen kunnen bevatten en de eigenschappen van verschillende typen kunnen worden.

U kunt table storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. Voor huidige internetgebaseerde toepassingen bieden NoSQL-databases zoals table storage een populaire alternatief voor traditionele relationele databases.

Een opslagaccount kan een onbeperkt aantal tabellen bevatten en een tabel mag een willekeurig aantal entiteiten, tot de capaciteitslimiet van het opslagaccount.

### <a name="queue-storage"></a>Queue Storage

Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue storage biedt een betrouwbare berichtenoplossing voor asynchrone communicatie tussen toepassingsonderdelen, ongeacht of ze worden uitgevoerd in de cloud, op het bureaublad, op een on-premises server of op een mobiel apparaat. Queue Storage biedt ook ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

Een opslagaccount kan een onbeperkt aantal wachtrijen bevatten en een wachtrij kan een onbeperkt aantal berichten, tot de capaciteitslimiet van het opslagaccount bevatten. Afzonderlijke berichten kunnen maximaal 64 kB groot zijn.

## <a name="next-steps"></a>Volgende stappen

- [Azure consistente: verschillen en overwegingen](azure-stack-acs-differences.md)

- Zie voor meer informatie over Azure Storage, [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)

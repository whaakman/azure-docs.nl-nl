---
title: Gegevensbronnen registreren in Azure Data Catalog
description: In dit artikel ziet u informatie over het registreren van gegevensbronnen in Azure Data Catalog, met inbegrip van de metagegevensvelden tijdens de registratie hebt uitgepakt.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 413f9340432f39d60ccdb43557616b4123132873
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404891"
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Gegevensbronnen registreren in Azure Data Catalog
## <a name="introduction"></a>Inleiding
Azure Data Catalog is een volledig beheerde cloudservice die als een systeem van registratie en detectie van bedrijfsgegevensassets fungeert. Met andere woorden, Data Catalog helpt gebruikers ontdekken, begrijpen en gebruiken van gegevensbronnen en het helpt organisaties meer waarde halen uit hun bestaande gegevens. De eerste stap bij het maken van een gegevensbron kan worden gedetecteerd via Data Catalog is voor het registreren van die gegevensbron.

## <a name="register-data-sources"></a>Gegevensbronnen registreren
Registratie is het proces van het extraheren van metagegevens van de gegevensbron en die gegevens kopiëren naar de Data Catalog-service. De gegevens blijven waar ze zich momenteel bevinden, onder het beheer van de groep beheerders en het beleid van het huidige systeem.

Voor het registreren van een gegevensbron, het volgende doen:
1. Start het registratiehulpprogramma voor Data Catalog gegevensbronnen in de Azure Data Catalog-portal. 
2. Meld u aan met uw werk- of schoolaccount met dezelfde Azure Active Directory-referenties die u gebruiken om aan te melden bij de portal.
3. Selecteer de gegevensbron die u wilt registreren.

Zie voor meer details in stappen, de [aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie.

Nadat u de gegevensbron hebt geregistreerd, wordt de catalogus houdt de locatie en de metagegevens van de indexen. Gebruikers kunnen zoeken, bladeren en de gegevensbron detecteren en gebruik vervolgens de locatie te verbinden met behulp van de toepassing of het hulpprogramma van hun keuze.

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen
Zie voor een lijst met ondersteunde gegevensbronnen, [Data Catalog DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Structurele metagegevens
Als u een gegevensbron registreert, haalt het registratiehulpprogramma voor informatie over de structuur van de objecten die u selecteert. Deze informatie wordt aangeduid als de structurele metagegevens.

Deze structurele metagegevens bevat voor alle objecten van het object locatie, zodat gebruikers die de gegevens te detecteren die informatie gebruiken kunnen om te verbinden met het object in de clienthulpprogramma's van hun keuze. Andere structurele metagegevens objectnaam en het type bevat, en typ de naam van kenmerk/kolom en gegevens.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens
Het hulpprogramma voor registratie extraheert naast de structurele metagegevens core, die wordt opgehaald uit de gegevensbron, beschrijvende metagegevens. Voor SQL Server Analysis Services en SQL Server Reporting Services, wordt deze metagegevens opgehaald uit de beschrijving van eigenschappen die worden weergegeven door deze services. Voor SQL Server, de waarden geleverd met behulp van de ms\_beschrijving uitgebreide eigenschap wordt opgehaald. Het hulpprogramma voor registratie van gegevensbron worden voor Oracle-Database, de kolom Opmerkingen geëxtraheerd uit de gehele\_tabblad\_opmerkingen bekijken.

Naast de beschrijvende metagegevens die zijn geëxtraheerd uit de gegevensbron, kunnen gebruikers de beschrijvende metagegevens invoeren met behulp van het hulpprogramma voor registratie. Gebruikers kunnen labels toevoegen en ze experts voor de objecten die wordt geregistreerd kunnen identificeren. Deze beschrijvende metagegevens is gekopieerd naar de Data Catalog-service, samen met de structurele metagegevens.

## <a name="include-previews"></a>Preview-versies bevatten
Standaard worden alleen metagegevens geëxtraheerd uit gegevensbronnen en gekopieerd naar de Data Catalog-service, maar wat die een gegevensbron is vaak eenvoudiger gemaakt wanneer u een voorbeeld van de gegevens in die het rapport kunt weergeven.

Met behulp van het hulpprogramma voor registratie van Data Catalog-gegevensbron, kunt u een voorbeeld van de momentopname van de gegevens opnemen in elke tabel en de weergave die is geregistreerd. Als u ervoor kiest om op te nemen previews tijdens de registratie, bevat het hulpprogramma voor registratie maximaal 20 records uit elke tabel en de weergave. Deze momentopname wordt vervolgens gekopieerd naar de catalogus, samen met de metagegevens van de structurele en beschrijvend.

> [!NOTE]
> Brede tabellen met een groot aantal kolommen mogelijk minder dan 20 records die zijn opgenomen in de preview.
>
>

## <a name="include-data-profiles"></a>Gegevens-profielen bevatten
Net zoals inclusief voorlopige versies waardevolle context voor gebruikers die voor gegevensbronnen in Data Catalog bieden kunnen zoeken, kunt met inbegrip van een gegevensprofiel maken het gemakkelijker om te begrijpen gedetecteerde gegevensbronnen.

Met behulp van het hulpprogramma voor registratie van Data Catalog-gegevensbron, kunt u een gegevensprofiel voor elke tabel en de weergave die is geregistreerd kunt opnemen. Als u ervoor kiest om op te nemen van een gegevensprofiel tijdens de registratie, het hulpprogramma voor registratie statistische gegevens over de gegevens bevat in elke tabel en de weergave, met inbegrip van:

* Het aantal rijen en de grootte van de gegevens in het object.
* De datum voor de meest recente update van de gegevens en de object-schema.
* Het aantal null-records en afzonderlijke waarden voor kolommen.
* Het minimum, maximum, gemiddelde en standaarddeviatie waarden voor kolommen.

Deze statistische gegevens worden vervolgens gekopieerd naar de catalogus, samen met de metagegevens van de structurele en beschrijvend.

> [!NOTE]
> Tekst en datum kolommen omvatten geen gemiddelde of standaarddeviatie statistieken in het gegevensprofiel van hun.
>
>

## <a name="update-registrations"></a>Bijwerken van registraties
Het registreren van een gegevensbron kunt u in Data Catalog kunnen worden gedetecteerd wanneer u de metagegevens en een optionele preview tijdens de registratie hebt uitgepakt. Als de gegevensbron moet worden bijgewerkt in de catalogus (bijvoorbeeld, als het schema van een object is gewijzigd, tabellen oorspronkelijk uitgesloten opgenomen worden moeten of u wilt bijwerken van de gegevens die zijn opgenomen in de Preview-versies), kan het registratiehulpprogramma voor gegevensbronnen opnieuw worden uitgevoerd.

Een al geregistreerd gegevensbron opnieuw te registreren, voert een "upsert"-bewerking merge: bestaande objecten zijn bijgewerkt en nieuwe objecten worden gemaakt. Alle metagegevens die door gebruikers via de portal voor Data Catalog worden bewaard.

## <a name="summary"></a>Samenvatting
Omdat het structurele en beschrijvende metagegevens opgehaald uit een gegevensbron met de catalogusservice, registreert de gegevensbron in Data Catalog, de gegevens eenvoudiger te detecteren en begrijpen. Nadat u de gegevensbron hebt geregistreerd, kunt u aantekeningen maken, beheren en te ontdekken met behulp van de Data Catalog-portal.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het registreren van gegevensbronnen, de [aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie.

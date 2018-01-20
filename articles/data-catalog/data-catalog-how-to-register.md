---
title: Gegevensbronnen registreren in Azure Data Catalog | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u met de registratie van gegevensbronnen in Azure Data Catalog, met inbegrip van de metagegevensvelden tijdens de registratie hebt uitgepakt.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 48b13eef0960afb4aab68923fb97b5b9c14a3d9f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Gegevensbronnen registreren in Azure Data Catalog
## <a name="introduction"></a>Inleiding
Azure Data Catalog is een volledig beheerde cloudservice die als een systeem van registratie en detectie van zakelijke gegevensbronnen fungeert. Met andere woorden, Data Catalog helpt mensen detecteren, begrijpen en gebruiken van gegevensbronnen en het helpt organisaties Haal meer uit hun bestaande gegevens. De eerste stap bij het maken van een gegevensbron kan worden gedetecteerd via Data Catalog is die gegevensbron registreren.

## <a name="register-data-sources"></a>Gegevensbronnen registreren
Registratie is het proces van metagegevens extraheren uit de gegevensbron en die gegevens kopiëren naar de Data Catalog-service. De gegevens blijven waar ze zich momenteel bevinden, onder het beheer van de groep beheerders en het beleid van het huidige systeem.

Voor het registreren van een gegevensbron, het volgende doen:
1. Start het hulpprogramma registratie Data Catalog voor gegevensbronnen in de portal voor Azure Data Catalog. 
2. Aanmelden met uw werk of school-account met dezelfde Azure Active Directory-referenties die u aan te melden bij de portal gebruiken.
3. Selecteer de gegevensbron die u wilt registreren.

Zie voor meer stapsgewijze informatie, de [aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie.

Nadat u de gegevensbron hebt geregistreerd, wordt de catalogus houdt de locatie en de metagegevens van de indexen. Gebruikers kunnen zoeken, bladert, en de gegevensbron detecteren en gebruik vervolgens de locatie verbinding kunnen maken met behulp van de toepassing of het hulpprogramma van hun keuze.

## <a name="supported-data-sources"></a>Ondersteunde gegevensbronnen
Zie voor een lijst met ondersteunde gegevensbronnen, [Data Catalog DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Structurele metagegevens
Wanneer u een gegevensbron registreert, haalt het registratiehulpprogramma informatie over de structuur van de objecten die u selecteert. Deze informatie wordt aangeduid als structurele metagegevens.

Deze structurele metagegevens bevat voor alle objecten, de locatie van het object, zodat gebruikers die de gegevens te detecteren die informatie verbinding maken met het object in de clienthulpprogramma's van hun keuze kunnen gebruiken. Andere structurele metagegevens bevat objectnaam en -type en typ kenmerk-/ kolomnaam en gegevens.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens
Naast de structurele metagegevens core, die wordt opgehaald uit de gegevensbron, haalt het registratiehulpprogramma van gegevensbronnen beschrijvende metagegevens. Deze metagegevens is voor SQL Server Analysis Services en SQL Server Reporting Services afkomstig van de beschrijving van eigenschappen die worden weergegeven door deze services. Voor SQL Server, die zijn opgegeven met behulp van de ms\_beschrijving uitgebreide eigenschap wordt opgehaald. Het hulpprogramma voor registratie van gegevensbron extraheert voor Oracle-Database, de kolom opmerkingen van de volledige\_tabblad\_opmerkingen weergeven.

Naast de beschrijvende metagegevens die wordt opgehaald uit de gegevensbron, kunnen gebruikers de beschrijvende metagegevens invoeren met behulp van het registratiehulpprogramma van gegevensbronnen. Gebruikers kunnen tags toevoegen en ze kunnen identificeren deskundigen voor de objecten die wordt geregistreerd. Deze beschrijvende metagegevens wordt gekopieerd naar de gegevenscatalogus service samen met de structurele metagegevens.

## <a name="include-previews"></a>Voorbeelden zijn
Standaard worden alleen metagegevens opgehaald uit de gegevensbronnen en gekopieerd naar de Data Catalog-service, maar wat die een gegevensbron wordt vaak eenvoudiger gemaakt wanneer u een voorbeeld van de gegevens erin kunt weergeven.

U kunt een voorbeeld van de momentopname van de gegevens via het registratiehulpprogramma van Data Catalog gegevensbronnen opnemen in elke tabel en de weergave die is geregistreerd. Als u kiest previews zijn tijdens de registratie, bevat het registratiehulpprogramma maximaal 20 records uit elke tabel en de weergave. Deze momentopname wordt vervolgens gekopieerd naar de catalogus samen met de structurele en beschrijvende metagegevens.

> [!NOTE]
> Wide tabellen met een groot aantal kolommen hebben mogelijk minder dan 20 records die zijn opgenomen in de preview.
>
>

## <a name="include-data-profiles"></a>Profielen van de gegevens bevatten
Net zoals inclusief voorbeelden u waardevolle context voor gebruikers die gegevensbronnen in de catalogus met gegevens zoeken bieden kunnen, kunt met inbegrip van een profiel van de gegevens u ze gemakkelijker te begrijpen gedetecteerde gegevensbronnen.

U kunt een profiel van de gegevens voor elke tabel en de weergave die is geregistreerd kunt opnemen met behulp van het registratiehulpprogramma van Data Catalog gegevensbronnen. Als u kiest een profiel van de gegevens zijn tijdens de registratie, het registratiehulpprogramma statistische gegevens over de gegevens bevat in elke tabel en de weergave, met inbegrip van:

* Het aantal rijen en de grootte van de gegevens in het object.
* De datum voor de meest recente update van de gegevens en het schema van het object.
* Het aantal null records en verschillende waarden voor kolommen.
* De minimum, maximum, gemiddelde en standaarddeviatie waarden voor kolommen.

Deze statistische gegevens worden vervolgens gekopieerd naar de catalogus samen met de structurele en beschrijvende metagegevens.

> [!NOTE]
> Tekst- en datum kolommen opnemen gemiddelde of standaarddeviatie statistieken niet in hun profiel gegevens.
>
>

## <a name="update-registrations"></a>Registraties bijwerken
Registreren van een gegevensbron maakt het gevonden in de catalogus met gegevens wanneer u de metagegevens en optionele preview tijdens de registratie hebt uitgepakt. Als de gegevensbron moet worden bijgewerkt in de catalogus (bijvoorbeeld als het schema van een object is gewijzigd, tabellen oorspronkelijk uitgesloten opgenomen worden moeten of u wilt bijwerken van de gegevens die opgenomen in de voorbeelden), kan het registratiehulpprogramma van gegevensbronnen opnieuw worden uitgevoerd.

Opnieuw registreren van een reeds geregistreerde gegevensbron voert een "upsert" samenvoegbewerking: bestaande objecten worden bijgewerkt en nieuwe objecten zijn gemaakt. Alle metagegevens die door gebruikers via de portal van de catalogus met gegevens worden bewaard.

## <a name="summary"></a>Samenvatting
Omdat het structurele en beschrijvende metagegevens uit een gegevensbron wordt gekopieerd naar de catalogusservice, gemakkelijker het registreren van de gegevensbron in Data Catalog de gegevens worden gedetecteerd en begrepen. Nadat u de gegevensbron hebt geregistreerd, kunt u aantekeningen maken, beheren en te ontdekken met behulp van de Data Catalog-portal.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het registreren van gegevensbronnen, de [aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie.

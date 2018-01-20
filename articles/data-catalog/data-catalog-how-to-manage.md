---
title: Beheren van gegevensassets in Azure Data Catalog | Microsoft Docs
description: Het artikel wordt uitgelegd hoe u instellingen voor zichtbaarheid en de eigenaar te worden van gegevensassets die zijn geregistreerd in Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 5a4b2b5734bf8bfbbc45a65b02362d1fa37b1a87
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Gegevensassets in Azure Data Catalog beheren
## <a name="introduction"></a>Inleiding
Azure Data Catalog is ontworpen voor detectie van de gegevensbron, zodat u gemakkelijk detecteren en begrijpen van de gegevensbronnen die u moet analyses uitvoeren en beslissingen te nemen. Deze mogelijkheden voor detectie maken de grootste impact wanneer u en andere gebruikers kunnen vinden en begrijpen waren van een breed scala aan gegevensbronnen beschikbaar. Met de volgende elementen in gedachten is het standaardgedrag van Data Catalog voor alle geregistreerde gegevensbronnen zijn zichtbaar voor en kunnen worden gedetecteerd door alle gebruikers van de catalogus.

Data Catalog biedt niet krijgt u toegang tot de gegevens zelf. Gegevenstoegang wordt bepaald door de eigenaar van de gegevensbron. U kunt met Data Catalog gegevensbronnen detecteren en weergeven van de metagegevens met betrekking tot de bronnen die zijn geregistreerd in de catalogus.

Er zijn mogelijk situaties echter waar gegevensbronnen alleen zichtbaar voor specifieke gebruikers of leden van specifieke groepen zijn moeten. In dergelijke gevallen kunnen gebruikers eigenaar worden van geregistreerde gegevensassets in de catalogus en vervolgens bepalen de zichtbaarheid van de activa die ze eigenaar.

> [!NOTE]
> De functionaliteit die is beschreven in dit artikel is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De editie Free biedt geen mogelijkheden voor eigenaar en beperking gegevens asset-zichtbaarheid.
>
>

## <a name="manage-ownership-of-data-assets"></a>Eigendom van gegevensassets beheren
Standaard zijn de gegevensassets die zijn geregistreerd in Data Catalog zonder eigenaar. Een gebruiker met machtigingen voor toegang tot de catalogus kan detecteren en aantekeningen toevoegen aan deze activa. Gebruikers kunnen eigenaar worden van gegevensassets zonder eigenaar en vervolgens de zichtbaarheid van de activa die ze eigenaar te beperken.

Wanneer u het eigendom van een activum gegevens in Data Catalog is alleen gebruikers die zijn geautoriseerd door de eigenaren van de asset detecteren en weergeven van de metagegevens en alleen de eigenaren van de activa kunnen verwijderen uit de catalogus.

> [!NOTE]
> Data Catalog eigendom is van invloed op alleen de metagegevens die zijn opgeslagen in de catalogus. Eigenaar verleent alle machtigingen op de onderliggende gegevensbron.
>
>

### <a name="take-ownership"></a>Eigenaar worden
Gebruikers kunnen eigenaar worden van gegevensassets door het selecteren van de **eigenaar** optie in de portal voor Data Catalog. Geen speciale machtigingen zijn vereist om de eigenaar van een activum zonder eigenaar gegevens. Elke gebruiker kan de eigenaar van een activum zonder eigenaar gegevens.

### <a name="add-owners-and-co-owners"></a>Eigenaar en mede-eigenaren toevoegen
Gewoon als een gegevensasset is al het eigendom, andere gebruikers kunnen geen eigenaar. Ze moeten worden toegevoegd als mede-eigenaren door de eigenaar van een bestaande. Eigenaar kunt extra gebruikers of beveiligingsgroepen toevoegen als mede-eigenaren.

> [!NOTE]
> Het is een best practice om ten minste twee personen als eigenaars voor een willekeurige gegevensasset in eigendom van.
>
>

### <a name="remove-owners"></a>Eigenaars verwijderen
Net zoals een activumeigenaar mede-eigenaren toevoegen kunt, kunt een activumeigenaar mede-eigenaar verwijderen.

Een asset-eigenaar die zichzelf als eigenaar van een verwijderd kan niet meer beheren voor de asset. Als de activumeigenaar zichzelf als eigenaar van een verwijderd en er geen andere mede-eigenaars, wordt de asset terug naar de status zonder eigenaar.

## <a name="control-visibility"></a>Zichtbaarheid besturingselement
Gegevensasset eigenaren kunnen bepalen de zichtbaarheid van de gegevensassets die ze eigenaar zijn. Als u wilt beperken zichtbaarheid als de standaardconfiguratie, waarbij alle gebruikers van Data Catalog detecteren en weergeven van de gegevensasset, de activumeigenaar de zichtbaarheidsinstelling van kunt schakelen **iedereen** naar **eigenaars en deze gebruikers** in de eigenschappen voor de asset. Eigenaars kunnen vervolgens specifieke gebruikers en beveiligingsgroepen toevoegen.

> [!NOTE]
> Indien mogelijk moeten asset eigendom en zichtbaarheid machtigingen worden toegewezen aan beveiligingsgroepen en niet naar afzonderlijke gebruikers.
>
>

## <a name="catalog-administrators"></a>Beheerders van de catalogus
Data Catalog beheerders zijn impliciet mede-eigenaren van alle activa in de catalogus. Asset eigenaars kunnen visibility niet verwijderen uit beheerders en beheerders kunnen beheren, eigendom en de zichtbaarheid van alle gegevensassets in de catalogus.

## <a name="summary"></a>Samenvatting
De Data Catalog crowdsourcing-model voor detectie van metagegevens en activa kan alle catalogusgebruikers bijdragen en detecteren. De Standard-editie van Data Catalog is ontworpen voor het eigenaarschap en beheer beperken de zichtbaarheid van en het gebruik van specifieke gegevensassets.

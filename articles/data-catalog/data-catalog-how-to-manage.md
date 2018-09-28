---
title: Gegevensassets in Azure Data Catalog beheren
description: Het artikel ziet u hoe u voor het beheren van inzicht en het eigendom van de gegevensassets die in Azure Data Catalog zijn geregistreerd.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 46af78dae6d5c1607f56d36732adce5e677e0c65
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407635"
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Gegevensassets in Azure Data Catalog beheren
## <a name="introduction"></a>Inleiding
Azure Data Catalog is ontworpen voor detectie van de gegevensbron, zodat u eenvoudig kunt detecteren en begrijpen van de gegevensbronnen die u wilt uitvoeren van analyses en beslissingen te nemen. Deze detectie-mogelijkheden kunt de grootste impact als u en andere gebruikers kunnen vinden en informatie over het breedste aanbod van beschikbare gegevensbronnen. Met deze elementen in gedachten is het standaardgedrag van Data Catalog voor alle geregistreerde gegevensbronnen niet zichtbaar voor en kunnen worden gedetecteerd door alle gebruikers van de catalogus.

Data Catalog geeft geen toegang tot de gegevens zelf. Toegang tot gegevens wordt bepaald door de eigenaar van de gegevensbron. Met Data Catalog kunt u gegevensbronnen detecteren en weergeven van de metagegevens die zijn gerelateerd aan de bronnen die zijn geregistreerd in de catalogus.

Mogelijk zijn er situaties echter waarin gegevensbronnen alleen zichtbaar voor specifieke gebruikers of specifieke groepen zijn moeten. In dergelijke scenario's, kunnen gebruikers eigenaar worden van geregistreerde gegevensassets in de catalogus en vervolgens bepalen de zichtbaarheid van de activa die waarvan ze eigenaar.

> [!NOTE]
> De functionaliteit die worden beschreven in dit artikel is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De Free Edition biedt mogelijkheden voor het eigendom en beperken gegevens-asset-zichtbaarheid.
>
>

## <a name="manage-ownership-of-data-assets"></a>Eigendom van gegevensassets beheren
Standaard zijn de gegevensassets die zijn geregistreerd in Data Catalog zonder eigenaar. Elke gebruiker met machtiging voor toegang tot de catalogus kan detecteren en aantekeningen toevoegen aan deze apparaten. Gebruikers kunnen eigenaar worden van gegevensassets zonder eigenaar en beperken de zichtbaarheid van de activa die waarvan ze eigenaar zijn.

Wanneer een gegevensasset in Data Catalog eigenaar is, alleen gebruikers die zijn gemachtigd door de eigenaars kunnen de asset detecteren en de metagegevens bekijken en alleen de eigenaren van de asset kunnen verwijderen uit de catalogus.

> [!NOTE]
> Data Catalog eigendom is van invloed op alleen de metagegevens die zijn opgeslagen in de catalogus. Eigenaar verleent alle machtigingen voor de onderliggende gegevensbron.
>
>

### <a name="take-ownership"></a>Eigenaar worden
Gebruikers kunnen eigenaar worden van gegevensassets door het selecteren van de **eigenaar** optie in de Data Catalog-portal. Er zijn geen speciale machtigingen moeten eigenaar worden van een activum zonder eigenaar gegevens. Elke gebruiker kan de eigenaar van een activum zonder eigenaar gegevens.

### <a name="add-owners-and-co-owners"></a>Eigenaars en mede-eigenaars toevoegen
Gewoon als een gegevensasset is al eigendom, andere gebruikers kunnen geen eigenaar. Ze moeten worden toegevoegd als mede-eigenaars door de eigenaar van een bestaande. Een eigenaar kunt extra gebruikers of beveiligingsgroepen toevoegen als mede-eigenaren.

> [!NOTE]
> Het is aanbevolen dat ten minste twee personen als eigenaren voor eigendom gegevensasset.
>
>

### <a name="remove-owners"></a>Eigenaren verwijderen
Net als de eigenaar van een asset mede-eigenaars toevoegen kunt, kan de eigenaar van een asset een mede-eigenaar kunt verwijderen.

De asset kan niet meer beheren door de eigenaar van een asset die zichzelf als eigenaar van een verwijderd. Als de eigenaar van de asset zichzelf als eigenaar van een verwijderd en er geen andere mede-eigenaars zijn, wordt de asset naar een zonder eigenaar status terugkeert.

## <a name="control-visibility"></a>Besturingselement voor zichtbaarheid
Gegevensasset eigenaren kunnen de zichtbaarheid van de gegevensassets die ze beheren. Als u wilt beperken zichtbaarheid als de standaardconfiguratie, waarbij alle Data Catalog-gebruikers kunnen detecteren en weergeven van de gegevensasset, de eigenaar van de asset de zichtbaarheidsinstelling van kunt schakelen **iedereen** naar **eigenaars en deze gebruikers** in de eigenschappen voor de asset. Eigenaren kunnen vervolgens specifieke gebruikers en beveiligingsgroepen toevoegen.

> [!NOTE]
> Indien mogelijk, moeten machtigingen voor het eigendom en de zichtbaarheid van asset worden toegewezen aan beveiligingsgroepen en niet naar afzonderlijke gebruikers.
>
>

## <a name="catalog-administrators"></a>Catalogusbeheerders
Data Catalog-beheerders zijn impliciet mede-eigenaars van alle activa in de catalogus. Asseteigenaren zichtbaarheid niet verwijderen van beheerders en beheerders kunnen beheren, eigendom en de zichtbaarheid voor alle gegevens in de catalogus.

## <a name="summary"></a>Samenvatting
De Data Catalog crowdsourcing-model op metagegevens en gegevens detecteren van gegevensmiddelen kan alle gebruikers van de catalogus te dragen en te detecteren. De Standard-editie van Data Catalog is ontworpen voor het eigendom en het beheer te beperken van de zichtbaarheid van en het gebruik van specifieke gegevensassets.

---
title: De zakelijke woordenlijst instellen voor een bepaald tags in Azure Data Catalog | Microsoft Docs
description: "Procedures voor artikel de zakelijke woordenlijst in Azure Data Catalog is gemarkeerd voor het definiëren en met behulp van een algemene zakelijke woordenlijst aan tag geregistreerde gegevensassets."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: a80b7fd0c21851a6670431e9b8647ca5cf5f51ec
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>De zakelijke woordenlijst instellen voor een bepaald tagging
## <a name="introduction"></a>Inleiding
Azure Data Catalog kunnen gegevensbron detectie, zodat u eenvoudig kunt detecteren en begrijpen van de gegevensbronnen die u moet analyses uitvoeren en beslissingen te nemen. Deze mogelijkheden maken de grootste impact wanneer u deze kunt vinden en begrijpen van een breed scala aan gegevensbronnen beschikbaar.

Een functie van Data Catalog die bijdraagt aan beter inzicht in gegevens van de activa is labels. U kunt met behulp van de labels trefwoorden koppelen aan een asset of een kolom, die op zijn beurt het detecteren van de asset vergemakkelijkt via zoeken of bladeren. Labels kunt u eenvoudig overzicht van de context en de bedoeling van de asset.

Echter kan tagging soms problemen veroorzaken voor zichzelf. Enkele voorbeelden van labels kan leiden tot problemen zijn:

* Het gebruik van afkortingen op bepaalde activa- en uitgebreide tekst op andere. Deze inconsistentie belemmert de detectie van activa, zelfs als de bedoeling is voor het taggen van de activa met dezelfde tag.
* Mogelijke verschillen in de zin, afhankelijk van de context. Bijvoorbeeld een label aangeroepen *omzet* op een klant gegevensset omzet door de klant kan betekenen, maar dezelfde tag van een kwartaal verkoop gegevensset omzet per kwartaal voor het bedrijf kan betekenen.  

Data Catalog bevat om te helpen deze en andere vergelijkbare uitdagingen adres, een zakelijke woordenlijst.

Een organisatie kan met behulp van de zakelijke woordenlijst van Data Catalog, documenteer belangrijke zakelijke voorwaarden en de bijbehorende definities voor het maken van een algemene zakelijke woordenlijst. Deze governance kunt consistentie in het gebruik van gegevens in de hele organisatie. Nadat u een term in de zakelijke woordenlijst is gedefinieerd, kan deze kan worden toegewezen aan een gegevensasset in de catalogus. Deze aanpak *geregeld tagging*, is dezelfde manier als labels.

## <a name="glossary-availability-and-privileges"></a>Verklarende woordenlijst beschikbaarheid en bevoegdheden
De zakelijke woordenlijst is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De gratis editie van Data Catalog bevat geen een verklarende woordenlijst en biedt geen mogelijkheden voor geregeld labelen.

U hebt toegang tot de zakelijke woordenlijst via de **verklarende woordenlijst** optie in het navigatiemenu Data Catalog-portal.  

![Toegang tot de zakelijke woordenlijst](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Data Catalog beheerders en leden van de rol van de beheerders verklarende woordenlijst kunnen maken, bewerken en verwijderen van termen in de in de zakelijke woordenlijst. Alle gebruikers van Data Catalog kunnen weergeven, de term definities en activa met termen in de tag.

![Toevoegen van een nieuwe term voor de verklarende woordenlijst](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Verklarende woordenlijst voorwaarden maken
Data Catalog beheerders en beheerders van de verklarende woordenlijst termen in de maken door te klikken op de **nieuwe Term** knop. Elke term verklarende woordenlijst bevat de volgende velden:

* De definitie van een bedrijf naar de term
* Een beschrijving op die worden vastgelegd van het beoogde gebruik of de bedrijfsregels voor de asset of kolom
* Een lijst met belanghebbenden die de meeste over de term kennen
* De term bovenliggende definieert de hiërarchie waarin de term is ingedeeld

## <a name="glossary-term-hierarchies"></a>Verklarende woordenlijst term hiërarchieën
Met behulp van de zakelijke woordenlijst van Data Catalog, een organisatie kan de zakelijke woordenlijst beschrijven als een hiërarchie van termen en deze een classificatie van termen die beter de zakelijke taxonomie vertegenwoordigt kunt maken.

Een term moet op een bepaald niveau van de hiërarchie uniek zijn. Dubbele namen zijn niet toegestaan. Er is geen limiet aan het aantal niveaus in een hiërarchie, maar een hiërarchie is vaak gemakkelijker te begrijpen wanneer er drie niveaus of minder zijn.

Het gebruik van hiërarchieën in de zakelijke woordenlijst is optioneel. De bovenliggende term veld leeg laten voor termen, maakt een platte (niet-hiërarchische)-lijst met voorwaarden in de woordenlijst.  

## <a name="tagging-assets-with-glossary-terms"></a>Labels van activa met termen in de
Nadat de termen zijn gedefinieerd in de catalogus, wordt de ervaring van activa-labels geoptimaliseerd om te zoeken naar de verklarende woordenlijst voor een gebruiker typt u een label. De Data Catalog-portal geeft een lijst met termen in de overeenkomende kiezen. Als de gebruiker term in een woordenlijst in de lijst selecteert, wordt de term voor de activa toegevoegd als een tag (ook wel een tag verklarende woordenlijst). De gebruiker kan ook kiezen voor het maken van een nieuwe code door een term die zich niet in de woordenlijst in te voeren (ook wel een label voor de gebruiker).

![Gegevensasset gemarkeerd met één gebruikerstag en labels voor twee verklarende woordenlijst](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Labels van de gebruiker zijn het enige type label wordt ondersteund in de gratis editie van Data Catalog.
>
>

### <a name="hover-behavior-on-tags"></a>Beweeg de muisaanwijzer gedrag tags
In de portal voor Data Catalog zijn de twee typen labels visueel uniek en aanwezig verschillende aanwijzen gedrag. Wanneer u de muisaanwijzer op het label van een gebruiker, ziet u de labeltekst en de gebruiker of gebruikers die de tag hebt toegevoegd. Als u de muisaanwijzer op een label verklarende woordenlijst, ziet u ook de definitie van de term en een koppeling naar het openen van de zakelijke woordenlijst om weer te geven van de volledige definitie van de termijn.

### <a name="search-filters-for-tags"></a>Zoekfilters voor tags
Verklarende woordenlijst tags en gebruiker tags zijn beide doorzoekbare en u ze als filters in een zoekopdracht kunt toepassen.

## <a name="summary"></a>Samenvatting
Met behulp van de zakelijke woordenlijst in Azure Data Catalog en het geregeld labelen kunnen, kunt u identificeren, beheren en gegevensassets detecteren op een consistente manier. De zakelijke woordenlijst kunt leren van de zakelijke woordenlijst door leden van de organisatie verhogen. De verklarende woordenlijst biedt ook ondersteuning voor het vastleggen zinvolle metadata die asset detectie en inzicht vereenvoudigt.

## <a name="next-steps"></a>Volgende stappen
* [REST-API-documentatie voor bedrijfsactiviteiten verklarende woordenlijst](https://msdn.microsoft.com/library/mt708855.aspx)

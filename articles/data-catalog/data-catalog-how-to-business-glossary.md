---
title: De zakelijke woordenlijst voor bestuurbaar tagging in Azure Data Catalog instellen
description: Artikel markeren van de zakelijke woordenlijst in Azure Data Catalog voor het definiëren en met behulp van een algemeen bedrijfsjargon op tag geregistreerde gegevensassets.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 9c5d7ac792f2a68d6187c7c2831b1e7404f41f30
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409419"
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>De zakelijke woordenlijst instellen voor geregeld tagging
## <a name="introduction"></a>Inleiding
Azure Data Catalog kunt gegevensbron detectie, zodat u eenvoudig kunt detecteren en begrijpen van de gegevensbronnen die u wilt uitvoeren van analyses en beslissingen te nemen. Deze mogelijkheden kunt de grootste impact als u deze kunt vinden en informatie over het breedste aanbod van beschikbare gegevensbronnen.

Een Data Catalog-functie die beter inzicht in gegevens van de activa stimulansen is tagging. U kunt met behulp van tags, trefwoorden koppelen aan een asset of een kolom, wat op zijn beurt het gemakkelijker maakt voor het detecteren van de asset via zoeken of bladeren. Tags kunt u gemakkelijk inzicht in de context en de bedoeling van de asset.

Echter kan tags soms problemen veroorzaken zelf. Enkele voorbeelden van problemen die tags kan zijn:

* Het gebruik van de afkortingen van sommige items en uitgebreide tekst op anderen. Deze inconsistentie belemmert de detectie van assets, zelfs als het doel is voor het taggen van de activa met dezelfde tag.
* Mogelijke variaties in de zin, afhankelijk van context. Bijvoorbeeld, een label met de naam *omzet* van een klant die in de gegevensset kan dit betekenen dat opbrengst per klant, maar dezelfde tag op een per kwartaal sales-gegevensset kan dit betekenen dat elk kwartaal omzet voor het bedrijf.  

Zodat deze en andere vergelijkbare problemen bevat Data Catalog een zakelijke woordenlijst.

Met behulp van de zakelijke woordenlijst van Data Catalog, kan een organisatie belangrijke bedrijfstermen en de bijbehorende definities om te maken van een algemeen bedrijfsjargon document. Deze governance kunt consistentie in de gebruikte gegevens in de hele organisatie. Nadat een term in de zakelijke woordenlijst is gedefinieerd, kan deze worden toegewezen aan een gegevensasset in de catalogus. Deze aanpak *geregeld tagging*, is de dezelfde methode als labels.

## <a name="glossary-availability-and-privileges"></a>Verklarende woordenlijst voor beschikbaarheid en bevoegdheden
De zakelijke woordenlijst is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De gratis editie van Data Catalog bevat geen een woordenlijst en biedt geen mogelijkheden voor bestuurbaar tagging.

U hebt toegang tot de zakelijke woordenlijst via de **verklarende woordenlijst** optie in het navigatiemenu van de Data Catalog-portal.  

![Toegang tot de zakelijke woordenlijst](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Data Catalog-beheerders en leden van de rol van de beheerders verklarende woordenlijst kunnen maken, bewerken en verwijderen van termen in de woordenlijst in de zakelijke woordenlijst. Alle Data Catalog-gebruikers kunnen de term definities en activa met termen in de tag weergeven.

![Een nieuwe woordenlijstterm toevoegen](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Het maken van Woordenlijsttermen
Data Catalog-beheerders en woordenlijstbeheerders Woordenlijsttermen kunnen maken door te klikken op de **nieuwe Term** knop. Elke woordenlijstterm bevat de volgende velden:

* De bedrijfsdefinitie van een voor de term
* Een beschrijving op die worden vastgelegd van het beoogde gebruik of de bedrijfsregels voor de asset of kolom
* Een lijst met belanghebbenden die met de meeste weten aan de term
* De bovenliggende term, waarin de hiërarchie waarin de term is georganiseerd

## <a name="glossary-term-hierarchies"></a>Verklarende woordenlijst term hiërarchieën
Met behulp van de zakelijke woordenlijst van Data Catalog, een organisatie kan de bedrijfsjargon worden beschreven als een hiërarchie van de voorwaarden en dat een classificatie van termen die beter de zakelijke taxonomie vertegenwoordigt kan worden gemaakt.

Een term moet op een bepaald niveau van de hiërarchie uniek zijn. Dubbele namen zijn niet toegestaan. Er is geen limiet voor het aantal niveaus in een hiërarchie, maar een hiërarchie is vaak gemakkelijker te begrijpen wanneer er drie niveaus of minder zijn.

Het gebruik van hiërarchieën in de zakelijke woordenlijst is optioneel. De bovenliggende term veld leeg laten voor Woordenlijsttermen, maakt een vast (niet-hiërarchische)-lijst met voorwaarden in de woordenlijst.  

## <a name="tagging-assets-with-glossary-terms"></a>Labels van activa met Woordenlijsttermen
Nadat Woordenlijsttermen zijn gedefinieerd in de catalogus, wordt de ervaring van de activa-tagging geoptimaliseerd om te zoeken naar de verklarende woordenlijst van een gebruiker typt u een tag. De Data Catalog-portal geeft een lijst van overeenkomende termen om uit te kiezen. Als de gebruiker een woordenlijstterm in de lijst selecteert, wordt de term als een label (ook wel een woordenlijsttag) toegevoegd aan de asset. De gebruiker kunt ook een nieuw label maken door te typen van een term die zich niet in de woordenlijst (ook wel een gebruikerstag genoemd).

![Gegevensasset gemarkeerd met een label en tags van twee verklarende woordenlijst](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Gebruiker-tags zijn het enige type label wordt ondersteund in de gratis editie van Data Catalog.
>
>

### <a name="hover-behavior-on-tags"></a>Beweeg de muisaanwijzer gedrag van tags
In de portal voor Data Catalog zijn de twee typen codes visueel uniek en werkt deze verschillende aanwijzen gedrag. Wanneer u een label aanwijst, ziet u de tekst van de tag en de gebruikers of gebruikers die de code hebt toegevoegd. Wanneer u de muisaanwijzer over een woordenlijsttag, ziet u ook de definitie van de woordenlijstterm en een koppeling naar het openen van de zakelijke woordenlijst om de volledige definitie van de periode weer te geven.

### <a name="search-filters-for-tags"></a>Zoekfilters voor tags
Verklarende woordenlijst tags en gebruiker tags zijn beide doorzoekbaar en kunt u deze toepassen als filters in een zoekopdracht.

## <a name="summary"></a>Samenvatting
Met behulp van de zakelijke woordenlijst in Azure Data Catalog en het bestuurbaar tagging kunt, kunt u identificeren, beheren en gegevens detecteren op een consistente manier. De zakelijke woordenlijst kunt leren van de zakelijke woordenlijst door leden van de organisatie verhogen. De verklarende woordenlijst biedt ook ondersteuning voor het vastleggen van nuttige metagegevens die assetdetectie en -inzicht vereenvoudigt.

## <a name="next-steps"></a>Volgende stappen
* [REST API-documentatie voor bedrijfsactiviteiten verklarende woordenlijst](https://msdn.microsoft.com/library/mt708855.aspx)

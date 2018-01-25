---
title: Aangepaste velden in de Azure Log Analytics | Microsoft Docs
description: De functie voor de aangepaste velden van Log Analytics kunt u uw eigen doorzoekbare velden maken van logboekanalyse records die aan de eigenschappen van een record die worden verzameld toevoegen.  In dit artikel beschrijft het proces voor het maken van een aangepast veld en een gedetailleerde uitleg biedt een voorbeeld van de gebeurtenis.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: f0f3311f35f954f81560cad21e7f0e3bc850a094
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="custom-fields-in-log-analytics"></a>Aangepaste velden in Log Analytics
De **aangepaste velden** functie van Log Analytics kunt u bestaande records in logboekanalyse uitbreiden door uw eigen doorzoekbare velden toe te voegen.  Aangepaste velden worden automatisch ingevuld van gegevens van andere eigenschappen in dezelfde record hebt uitgepakt.

![Overzicht van aangepaste velden](media/log-analytics-custom-fields/overview.png)

Het onderstaande voorbeeldrecord heeft bijvoorbeeld nuttige gegevens in de gebeurtenisbeschrijving overspoeld.  Deze gegevens in te pakken op afzonderlijke eigenschappen, wordt het beschikbaar gemaakt voor deze acties als sorteren en filteren.

![Meld u knop Zoeken](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> In de Preview bent u beperkt tot 100 aangepaste velden in uw werkruimte.  Deze limiet zal worden uitgebreid wanneer deze functie algemeen beschikbaar is bereikt.
> 
> 

## <a name="creating-a-custom-field"></a>Maken van een aangepast veld
Bij het maken van een aangepast veld, logboekanalyse moet begrijpen welke gegevens worden gebruikt voor het vullen van de waarde ervan.  Een technologie van Microsoft Research aangeroepen FlashExtract wordt gebruikt deze gegevens snel kan identificeren.  In plaats van dat u expliciete instructies te geven, leert logboekanalyse over de gegevens die u wilt ophalen van voorbeelden die u opgeeft.

De volgende secties vindt de procedure voor het maken van een aangepast veld.  Aan de onderkant van dit artikel is een overzicht van de extractie van een steekproef.

> [!NOTE]
> Het aangepaste veld wordt gevuld wanneer records die overeenkomt met de opgegeven criteria worden toegevoegd aan Log Analytics, zodat er alleen op records verzameld nadat het aangepaste veld is gemaakt.  Het aangepaste veld wordt niet worden toegevoegd aan de records die al in het gegevensarchief wanneer deze wordt gemaakt.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Stap 1: records waarvoor het aangepaste veld identificeren
De eerste stap is het identificeren van de records die u krijgt het aangepaste veld.  U kunt beginnen met een [standaard logboek zoeken](log-analytics-log-searches.md) en selecteer vervolgens een record om te fungeren als het model dat Log Analytics leert uit.  Wanneer u opgeeft dat u wilt ophalen van gegevens naar een aangepast veld de **veld extractie Wizard** is geopend, waarin u valideren en Verfijn de criteria.

1. Ga naar **logboek zoeken** en gebruik een [query voor het ophalen van de records](log-analytics-log-searches.md) waarvoor het aangepaste veld.
2. Selecteer een record die Log Analytics wordt gebruikt om te fungeren als een model voor het extraheren van gegevens voor het aangepaste veld.  U kunt de gegevens die u wilt extraheren uit deze record wordt geïdentificeerd en Log Analytics gebruikt deze informatie om te bepalen van de logica voor het vullen van het aangepaste veld voor alle soortgelijke records.
3. Klik op de knop aan de linkerkant van een teksteigenschap van de record en selecteer **velden uit te pakken**.
4. De **veld extractie Wizard wordt geopend**, en de geselecteerde record wordt weergegeven in de **Main voorbeeld** kolom.  Het aangepaste veld worden gedefinieerd voor de records met dezelfde waarden in de eigenschappen die zijn geselecteerd.  
5. Als de selectie niet precies wat u zoekt is, selecteert u aanvullende velden om de criteria te beperken.  Als u wilt wijzigen de waarden voor de criteria, moet u annuleren en selecteer een andere record die overeenkomt met de criteria die u wilt.

### <a name="step-2---perform-initial-extract"></a>Stap 2: initiële extract uitvoeren.
Zodra u de records waarvoor het aangepaste veld hebt geïdentificeerd, kunt u de gegevens die u wilt ophalen identificeert.  Log Analytics gebruikt deze informatie om vergelijkbare patronen in soortgelijke records identificeren.  In de stap hierna kunt u zich kunt valideren van de resultaten en vindt u meer informatie voor logboekanalyse moet worden gebruikt in de analyse.

1. Markeer de tekst in de voorbeeldrecord dat u wilt vullen van het aangepaste veld.  U wordt vervolgens het dialoogvenster een naam voor het veld op te geven en voor het uitvoeren van het eerste extract worden gepresenteerd.  De tekens  **\_CF** automatisch worden toegevoegd.
2. Klik op **extraheren** om uit te voeren van een analyse van verzamelde records.  
3. De **samenvatting** en **zoekresultaten** secties worden de resultaten van het extract weergegeven, zodat u kunt de nauwkeurigheid inspecteren.  **Samenvatting** de criteria die wordt gebruikt voor het identificeren van records en een telling voor elk van de gegevenswaarden geïdentificeerd worden weergegeven.  **Zoekresultaten** biedt een gedetailleerde lijst met records die voldoen aan de criteria.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Stap 3: Controleer de juistheid van het extract en aangepast veld maken
Zodra u het eerste extract hebt uitgevoerd, weergegeven logboekanalyse de resultaten op basis van gegevens die al is verzameld.  Als de resultaten zien er nauwkeurige kunt u het aangepaste veld met niets verdere zakelijk maken.  Als dat niet het geval is, klikt u vervolgens kunt u de resultaten verfijnen zodat logboekanalyse de logica kunnen verbeteren.

1. Als alle waarden in het eerste extract niet juist zijn, klikt u op de **bewerken** pictogram naast een onnauwkeurige opnemen en selecteer **wijzigen van deze markering** voor het wijzigen van de selectie.
2. De vermelding wordt gekopieerd naar de **aanvullende voorbeelden gegeven** sectie onder de **Main voorbeeld**.  U kunt de markering hier aanpassen zodat logboekanalyse inzicht in de selectie die moet hebt opgegeven.
3. Klik op **extraheren** voor het gebruik van deze nieuwe gegevens om te evalueren van de bestaande records.  De resultaten kunnen worden gewijzigd voor records dan die u zojuist hebt gewijzigd op basis van deze nieuwe bedrijfsinformatie.
4. Doorgaan met het toevoegen van correcties totdat alle records in het extract correct identificeren van de gegevens voor het vullen van het nieuwe aangepaste veld.
5. Klik op **opslaan extraheren** wanneer u tevreden bent met de resultaten.  Het aangepaste veld is nu gedefinieerd, maar dit zal niet worden toegevoegd aan records nog.
6. Wachten op nieuwe records die overeenkomt met de opgegeven criteria om te worden verzameld en voer de zoekopdracht logboek vervolgens opnieuw. Nieuwe records moeten het aangepaste veld hebben.
7. Het aangepaste veld zoals andere record, eigenschap gebruiken.  U kunt deze gebruiken om gegevens te cumulatieve en groep en zelfs gebruiken voor het produceren van nieuwe inzichten.

## <a name="viewing-custom-fields"></a>Aangepaste velden weergeven
U kunt een lijst van alle aangepaste velden weergeven in uw beheergroep vanuit de **geavanceerde instellingen** menu van de werkruimte voor logboekanalyse in de Azure portal.  Selecteer **gegevens** en vervolgens **aangepaste velden** voor een lijst met alle aangepaste velden in uw werkruimte.  

![Aangepaste velden](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Een aangepast veld verwijderen
Er zijn twee manieren om te verwijderen van een aangepast veld.  De eerste is de **verwijderen** optie voor elk veld wanneer de volledige lijst bekijken, zoals hierboven is beschreven.  De andere methode is het ophalen van een record en klik op de knop aan de linkerkant van het veld.  Het menu wordt een optie voor het verwijderen van het aangepaste veld hebben.

## <a name="sample-walkthrough"></a>Voorbeeld-overzicht
De volgende sectie wordt uitgelegd een compleet voorbeeld van het maken van een aangepast veld.  Dit voorbeeld wordt opgehaald met de servicenaam van de in Windows-gebeurtenissen die duiden op een service de status wordt gewijzigd.  Dit is afhankelijk van gebeurtenissen die zijn gemaakt door de Service Control Manager in het systeemlogboek op Windows-computers.  Als u dit voorbeeld volgt wilt, moet u [verzamelen van gebeurtenissen voor het systeemlogboek](log-analytics-data-sources-windows-events.md).

We voert u de volgende query om te retourneren van alle gebeurtenissen van Service Control Manager met een gebeurtenis-ID van 7036 die de gebeurtenis die aangeeft van een service starten of stoppen.

![Query’s uitvoeren](media/log-analytics-custom-fields/query.png)

We Selecteer vervolgens een record met gebeurtenis-ID 7036.

![Bronrecord](media/log-analytics-custom-fields/source-record.png)

We willen dat de naam van de service die wordt weergegeven in de **RenderedDescription** eigenschap en selecteer de knop naast deze eigenschap.

![Haal velden](media/log-analytics-custom-fields/extract-fields.png)

De **veld extractie Wizard** wordt geopend, en de **EventLog** en **EventID** velden zijn geselecteerd in de **Main voorbeeld** kolom.  Hiermee wordt aangegeven dat het aangepaste veld voor gebeurtenissen van het logboek met een gebeurtenis-ID van 7036 worden gedefinieerd.  Dit is voldoende dus moeten we er geen andere velden selecteren.

![Voorbeeld van de belangrijkste](media/log-analytics-custom-fields/main-example.png)

Selecteren we de naam van de service in de **RenderedDescription** eigenschap en gebruik **Service** om de servicenaam te identificeren.  Het aangepaste veld moet worden aangeroepen **Service_CF**.

![De titel](media/log-analytics-custom-fields/field-title.png)

We zien dat de servicenaam correct is geïdentificeerd voor sommige records, maar niet voor andere gebruikers.   De **zoekresultaten** weergeven die deel uitmaken van de naam van de **WMI-prestatieadapter** is niet geselecteerd.  De **samenvatting** blijkt dat vier records met **DPRMA** service onjuist opgenomen een extra word en twee records geïdentificeerd **Modules Installer** in plaats van **Windows Modules Installer**.  

![Zoekresultaten](media/log-analytics-custom-fields/search-results-01.png)

We beginnen met de **WMI-prestatieadapter** record.  We klikt u op het bewerkingspictogram en vervolgens **wijzigen van deze markering**.  

![Markering wijzigen](media/log-analytics-custom-fields/modify-highlight.png)

We de markering voor het woord verhogen **WMI** en voer het extract.  

![Nog een voorbeeld](media/log-analytics-custom-fields/additional-example-01.png)

We zien dat de vermeldingen voor **WMI-prestatieadapter** zijn opgelost, en die informatie logboekanalyse ook gebruikt voor het corrigeren van de records voor **Windows Module Installer**.  We zien de **samenvatting** echter sectie **DPMRA** nog steeds niet wordt aangeduid correct.

![Zoekresultaten](media/log-analytics-custom-fields/search-results-02.png)

We Schuif naar een record met de DPMRA-service en hetzelfde proces gebruiken om op te lossen die record.

![Nog een voorbeeld](media/log-analytics-custom-fields/additional-example-02.png)

 Wanneer we de extractie uitvoert, kunnen we zien dat alle onze resultaten nu juist zijn.

![Zoekresultaten](media/log-analytics-custom-fields/search-results-03.png)

We zien die **Service_CF** is gemaakt, maar nog niet is toegevoegd aan alle records.

![Oorspronkelijke aantal](media/log-analytics-custom-fields/initial-count.png)

Na enige tijd is verstreken, zodat nieuwe gebeurtenissen worden verzameld, zien we dat die de **Service_CF** veld is nu toegevoegd aan de records die overeenkomen met de criteria.

![Definitieve resultaten](media/log-analytics-custom-fields/final-results.png)

We kunnen nu het aangepaste veld zoals een andere record eigenschap gebruiken.  Ter illustratie we een query maken die door de nieuwe groepen **Service_CF** veld om te controleren welke services het meest actief zijn.

![Groeperen op query](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om query's op basis van aangepaste velden voor de criteria samen te stellen.
* Monitor [aangepaste logboekbestanden](log-analytics-data-sources-custom-logs.md) die u met behulp van aangepaste velden parseren.


---
title: Kolom splitsen op voorbeeld transformatie met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de transformatie 'Kolom splitsen door voorbeeld'
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 5d624735a91d0828c4ac3796bde6c17acf6e131a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="split-column-by-example-transformation"></a>Kolom splitsen door voorbeeld transformatie
De inhoud van een kolom op zinvolle grenzen splitst deze transformatie predictively zonder gebruikersinvoer van. Het algoritme gesplitste selecteert de grenzen na het analyseren van de inhoud van de kolom. Deze grenzen kunnen worden gedefinieerd door
* Een vaste scheidingsteken
* Meerdere, willekeurige scheidingstekens verschijnen in het bijzonder contexten, of,
* Gegevenspatronen of een bepaald type entiteit

Gebruikers kunnen ook beheren voor het splitsen gedrag in de geavanceerde modus wanneer zij de scheidingstekens opgeven of door vindt u voorbeelden van het gewenste splitsen.

In theorie gesplitste bewerkingen kunnen ook worden uitgevoerd in de Workbench met behulp van een reeks *kolom afgeleid door voorbeeld* transformaties. Als er meerdere kolommen, kan afleiding van elk van deze afzonderlijk zelfs met door voorbeeld benadering echter zijn veel tijd in beslag. Voorspeld gesplitste kan eenvoudig splitsen zonder dat de gebruiker hoeft te bevatten voorbeelden voor elk van de kolommen. 

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

1. Selecteer de kolom die u wilt splitsen. 
2. Selecteer **kolom splitsen door voorbeeld** van de **transformeert** menu. Of, met de rechtermuisknop op de koptekst van de geselecteerde kolom en selecteer **kolom splitsen door voorbeeld** in het contextmenu. De transformatie-Editor wordt geopend en nieuwe kolommen worden toegevoegd naast de geselecteerde kolom. Op dit moment de Workbench analyseert de invoerkolom, bepaalt gesplitste grenzen en een programma voor het splitsen van de kolom, zoals weergegeven in het raster synthesizes. Het kunstmatige programma wordt uitgevoerd met de rijen in de kolom. Scheidingstekens, worden indien aanwezig, uitgesloten van het uiteindelijke resultaat.
3. U kunt klikken op de **geavanceerde modus** voor een betere controle over de split-transformatie. 
4. Controleer de uitvoer en klik op **OK** te accepteren van de transformatie. 

De transformatie is erop gericht voor het produceren van hetzelfde aantal resulterende kolommen voor alle rijen. Als een rij kan niet op de bepaald grenzen worden gesplitst, is het resultaat *null* voor alle kolommen standaard. Dit gedrag kan worden gewijzigd in de **geavanceerde modus**.

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus
**Geavanceerde modus** biedt een rijkere ervaring voor het splitsen van kolommen. 

Selecteren **houden scheidingsteken kolommen** bevat de scheidingstekens in het uiteindelijke resultaat. Scheidingstekens niet standaard opgenomen.

Geven **scheidingstekens** overschrijft de logica van de selectie automatisch scheidingsteken. Meerdere scheidingstekens, één in elke regel kunnen worden opgegeven als **scheidingstekens**. Al deze tekens worden gebruikt als scheidingstekens voor de kolom splitsen.

Soms splitsen van een waarde op bepaald grenzen produceert verschillend aantal kolommen dan de meerderheid van anderen. In deze gevallen **vullen richting** wordt gebruikt om te bepalen van de volgorde waarin de kolommen moeten worden ingevuld.

Te klikken op **voorgestelde voorbeelden** geeft de representatieve rijen waarvoor een voorbeeld van gesplitste moeten bieden. Gebruiker kan klikken op de **Up** pijl rechts van de voorgestelde rij ter bevordering van de rij als voorbeeld. 

De gebruiker kan **kolom verwijderen** of **nieuwe Invoegkolommen** door met de rechtermuisknop op de koptekst van de **voorbeelden tabel**.

Gebruiker kunt kopiëren en plakken van een cel waarden naar de andere om te voorzien van een voorbeeld van splitsen.

Gebruikers kan schakelen tussen de **standaardmodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppelen wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de parameterselecties voor de en de gebruiker voorbeelden is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie zodat we kunnen het probleem te begrijpen. Als de gebruiker niet wil gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 


### <a name="editing-an-existing-transformation"></a>Een bestaande transformatie bewerken

Gebruikers kunnen bewerken in een bestaande **kolom door voorbeeld Split** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **geavanceerde modus**, en de voorbeelden die zijn opgegeven tijdens het maken van de transformatie worden weergegeven.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Voorbeelden van het splitsen van een vaste, één teken scheidingsteken

Het is gebruikelijk voor velden worden gescheiden door een scheidingsteken voor één vaste, zoals een komma in een CSV-indeling. De transformatie gesplitste probeert automatisch afleiden uit deze scheidingstekens. Bijvoorbeeld, in het volgende scenario deze automatisch afleidt de '. ' als scheidingsteken.

### <a name="splitting-ip-addresses"></a>Het splitsen van IP-adressen

De waarden in de eerste kolom worden predictively opgesplitst in vier kolommen.

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Voorbeelden van splitsen op meerdere scheidingstekens in bepaalde contexten

Gegevens van de gebruiker, omvat mogelijk veel verschillende scheidingstekens scheiden van verschillende velden. Bovendien slechts enkele exemplaren van een tekenreeks in scheidingsteken dat wordt mogelijk een scheidingsteken, maar niet alle. In het volgende geval de set met scheidingstekens vereist is bijvoorbeeld '-',', 'en': ' om de gewenste uitvoer te produceren. Echter niet elk exemplaar van de ': ' moet een splitspunt omdat er geen wilt opsplitsen van de tijd, maar blijft deze in één kolom. De transformatie gesplitste infereert scheidingstekens binnen de context waarin ze in de ingevoerde gegevens in plaats van alle mogelijke exemplaar van het scheidingsteken voorkomen. De transformatie is ook rekening houden met algemene gegevenstypen zoals datums en tijden.   

### <a name="splitting-store-opening-timings"></a>Store openen tijdsinstellingen splitsen

De waarden in de volgende *tijdsinstellingen* kolom ophalen predictively splitsen in negen weergegeven in de tabel onder deze kolommen.

|Tijdsinstellingen|
|:-----|
|Maandag tot vrijdag: 7:00 uur - 18:00 uur, zaterdag: 9:00 uur - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 9:00 uur - 18:00 uur, zaterdag: 4:00 uur - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30 am - 7:00 uur, zaterdag: 3:00 uur - 2:30 pm, zondag: gesloten|
|Maandag tot vrijdag: 8:00 uur - 18:00 uur, zaterdag: 2:00 uur - 15:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 4:00 uur - 7:00 uur, zaterdag: 9:00 uur - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30 am - 4:30 uur, zaterdag: 9:00 uur - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 5:30 uur - 18:30 uur, zaterdag: 05:00:00 - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30 uur - 8:30 uur, zaterdag: 6:00 uur - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:00 uur - 21:00 uur, zaterdag: 9:00 - 8:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 10:00 uur - 9:30 uur, zaterdag: 9:30 am - 15:00 uur, zondag: gesloten|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|maandag|vrijdag|7:00 uur|18:00 uur|zaterdag|9:00 uur|17:00 uur|zondag|Gesloten|
|maandag|vrijdag|9:00 uur|18:00 uur|zaterdag|4:00 uur|4:00 uur|zondag|Gesloten|
|maandag|vrijdag|8:30 uur|19:00 uur|zaterdag|3:00 uur|2:30 pm|zondag|Gesloten|
|maandag|vrijdag|8:00 uur|18:00 uur|zaterdag|2:00 uur|15:00 uur|zondag|Gesloten|
|maandag|vrijdag|4:00 uur|19:00 uur|zaterdag|9:00 uur|4:00 uur|zondag|Gesloten|
|maandag|vrijdag|8:30 uur|4:30 pm|zaterdag|9:00 uur|17:00 uur|zondag|Gesloten|
|maandag|vrijdag|5:30 uur|18:30:00 uur|zaterdag|5:00 uur|4:00 uur|zondag|Gesloten|
|maandag|vrijdag|8:30 uur|8:30 pm|zaterdag|6:00 uur|17:00 uur|zondag|Gesloten|
|maandag|vrijdag|8:00 uur|21:00 uur|zaterdag|9:00 uur|8:00 uur|zondag|Gesloten|
|maandag|vrijdag|10:00 uur|9:30 pm|zaterdag|9:30:00 uur|15:00 uur|zondag|Gesloten|

### <a name="splitting-iis-log"></a>Splitsen IIS-logboek

Hier volgt een voorbeeld van meerdere willekeurige scheidingstekens. Dit voorbeeld bevat tevens een contextuele scheidingsteken '/', die niet binnen de URL's moeten worden verdeeld of bestandspaden. Het is omslachtig om uit te voeren dit verdelen met veel *kolom afgeleid door voorbeeld* transformaties en voorbeelden geven voor elk veld. Met de transformatie gesplitste uitvoeren we in de voorspellende splitsen zonder dat geen voorbeelden.

|logtext|
|:-----|
|192.128.138.20--[Oct-16-2016 16:22:33-0200] 'GET /images/picture.gif HTTP/1.1' 234 343 www.yahoo.com 'http://www.example.com/' ' Mozilla/4.0 (compatibel; MSIE 4)' '-'|
|10.128.72.213--[Oct-17-2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com 'http://www.sample.com/' ' Mozilla/5.0 (MSIE)' '-'|
|192.165.71.165--[november-12-2016 14:22:44 -0500] 'GET /sample.ico HTTP/1.1' 342 7342 www.facebook.com '-' ' Mozilla/5.0 (Windows; U; Windows NT 5.1; RV:1.7.3) ' '-'|
|10.166.64.165 - - [23/Nov/2016 01:52:45 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|192.167.1.193--[Jan/16/2017 22:34:56 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com 'http://www.illustration.com/index.html' ' Mozilla/5.0 (Windows)' '-'|
|192.147.76.193--Jan/28/2017 26:36:16 +0800 'GET /search.php HTTP/1.1' 400 1777 www.bing.com '-' ' Mozilla/4.0 (compatibel; MSIE 6.0; Windows NT 5.1)' '-'|
|192.166.64.165 - - [23/Mar/2017 01:55:25 -0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla/5.0 (Windows)" "-"|
|11.167.1.193--[april-16/2017 11:34:36 +0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com 'http://www.illustration.com/index.html' ' Mozilla/5.0 (Windows)' '-'|

Met deze eigenschap wordt opgedeeld in:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|Oct-16-2016|16:22:33|-0200|GET|images/Picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4.0|compatibel zijn. MSIE 4|
|10.128.72.213|Oct-17-2016|12:43:12|+0300|GET|news/stuff.html|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|12/Nov/2016|14:22:44|-0500|GET|sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; RV:1.7.3|
|10.166.64.165|23/Nov/2016|01:52:45|-0800|GET|Style.CSS|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|Jan-16/2017|22:34:56|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|Jan/28/2017|26:36:16|+0800|GET|Search.php|HTTP|1.1|400|1777|www.bing.com|-|Mozilla|4.0|compatibel zijn. MSIE 6.0; Windows NT 5.1|
|192.166.64.165|Mrt-23/2017|01:55:25|-0800|GET|Style.CSS|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|16/Apr/2017|11:34:36|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Voorbeelden van het splitsen van zonder scheidingstekens
In sommige gevallen, er zijn geen daadwerkelijke scheidingstekens en gegevensvelden aaneengesloten naast elkaar kunnen optreden. In dit geval detecteert de Split-transformatie automatisch patronen in de gegevens waarschijnlijk splitspunten afleiden. Bijvoorbeeld, in het volgende scenario willen we het bedrag scheiden van het valutatype en gesplitste automatisch de grens tussen de numerieke en niet-numerieke gegevens als de splitspunt afleidt.

### <a name="splitting-amount-with-currency-symbol"></a>Bedrag valutasymbool splitsen

|Bedrag|Amount_1|Amount_2|
|:-----|:-----|:-----|
|\$14|$|14|
|£9|£|9|
|\$34|$|34|
|€ 18|€ |18|
|\$42|$|42|
|\$7|$|7|
|£42|£|42|
|\$16|$|16|
|€ 16|€ |16|
|\$15|$|15|
|\$16|$|16|
|€ 64|€ |64|

In het volgende voorbeeld graag willen we gewichtswaarden scheiden van de eenheden. Opnieuw de splitsing Deductie detecteert automatisch de zinvolle grens en geeft de voorkeur aan deze via andere mogelijke scheidingstekens, zoals de '. ' teken. 

### <a name="splitting-weights-with-units"></a>Gewicht met eenheden splitsen

|Gewicht|Weight_1|Weight_2|
|:-----|:-----|:-----|
|2.27KG|2.27|KG|
|1L|1|L|
|2.5 KG|2.5|KG|
|2KG|2|KG|
|1.7KGA|1.7|KGA|
|3KG|3|KG|
|2KG|2|KG|
|125G|125|G|
|500G|500|G|
|1.5KGA|1.5|KGA|

## <a name="technical-notes"></a>Technische opmerkingen

De functie Split-transformatie is gebaseerd op de **voorspellende programma synthese** techniek. In deze techniek worden gegevenstransformatie-programma's op basis van de ingevoerde gegevens automatisch geleerd. De programma's zijn gemaakt in een domein-specifieke taal. De DSL is gebaseerd op de velden die in het bijzonder reguliere expressie contexten optreden en scheidingstekens. Meer informatie over deze technologie kan worden gevonden een [recente publicatie over dit onderwerp](https://www.microsoft.com/en-us/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 

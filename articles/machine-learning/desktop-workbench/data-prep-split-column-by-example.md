---
title: Kolom splitsen per voorbeeld transformatie met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de transformatie 'Kolom splitsen per voorbeeld'
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 3edf49484e5bc05a297b8d8969632fb902aa1714
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953732"
---
# <a name="split-column-by-example-transformation"></a>Kolom splitsen per voorbeeld transformatie

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


De inhoud van een kolom in duidelijke grenzen splitst deze transformatie predictively zonder invoer van de gebruiker. De Split-algoritme selecteert de grenzen na het analyseren van de inhoud van de kolom. Deze grenzen kunnen worden gedefinieerd door
* Een vaste scheidingsteken
* Meerdere, willekeurige scheidingstekens worden weergegeven in het bijzonder contexten, of,
* , Gegevenspatronen of een bepaald type entiteit

Gebruikers kunnen ook het splitsen gedrag in de geavanceerde modus waar ze kunnen de scheidingstekens opgeven of door vindt u voorbeelden van het splitsen van gewenste beheren.

In theorie, gesplitste bewerkingen kunnen ook worden uitgevoerd in de Workbench met behulp van een reeks *kolom afleiden per voorbeeld* transformeert. Als er meerdere kolommen, kan die is afgeleid van de referenties die afzonderlijk ook gebruikt door de voorbeeld-aanpak echter zijn zeer tijd in beslag nemen. Voorspellende splitsen kunt eenvoudig zonder de gebruiker hoeft voor voorbeelden van elk van de kolommen te splitsen. 

## <a name="how-to-perform-this-transformation"></a>Het uitvoeren van deze transformatie

1. Selecteer de kolom die u wilt splitsen. 
2. Selecteer **kolom splitsen per voorbeeld** uit de **transformeert** menu. Of, met de rechtermuisknop op de kop van de geselecteerde kolom en selecteert u **kolom splitsen per voorbeeld** in het contextmenu. Hiermee opent u de Editor transformeren en nieuwe kolommen worden toegevoegd naast de geselecteerde kolom. Op dit moment de Workbench analyseert de invoerkolom, bepaalt de grenzen van splitsen en synthetiseert een programma op de kolom splitsen, zoals weergegeven in het raster. Het synchronisatieprogramma wordt uitgevoerd op basis van alle rijen in de kolom. Scheidingstekens, zijn indien van toepassing, uitgesloten van het uiteindelijke resultaat.
3. U kunt klikken op de **geavanceerde modus** voor een betere controle over de split-transformatie. 
4. Bekijk de uitvoer en klikt u op **OK** de transformatie accepteert. 

De transformatie is erop gericht voor het produceren van hetzelfde aantal resulterende kolommen voor alle rijen. Als een rij kan niet op de vastgestelde grenzen worden gesplitst, produceert *null* voor alle kolommen in standaard. Dit gedrag kan worden gewijzigd in de **geavanceerde modus**.

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus
**Geavanceerde modus** biedt een rijkere ervaring voor het splitsen van kolommen. 

Selecteren **houden scheidingsteken kolommen** de scheidingstekens bevat in het uiteindelijke resultaat. De scheidingstekens zijn niet standaard opgenomen.

Op te geven **scheidingstekens** overschrijft de logica van de selectie automatisch scheidingsteken. Meerdere scheidingstekens in elke regel kunnen worden opgegeven als **scheidingstekens**. Al deze tekens worden gebruikt als scheidingsteken voor de kolom splitsen.

Soms splitsen van een waarde op bepaald grenzen produceert verschillend aantal kolommen dan de meerderheid van anderen. In deze gevallen **vullen richting** wordt gebruikt om te bepalen van de volgorde waarin de kolommen moeten worden ingevuld.

Te klikken op **voorgestelde voorbeelden** geeft de representatieve rijen voor welke gebruiker moeten een voorbeeld van de gesplitste bieden. Gebruiker kan klikken op de **van** pijl aan de rechterkant van de voorgestelde rij ter bevordering van de rij als voorbeeld. 

De gebruiker kan **kolom verwijderen** of **nieuwe Invoegkolommen** met de rechtermuisknop op de kop van de **voorbeelden tabel**.

Gebruiker kunt kopiëren en plakken van een cel waarden naar een andere om te voorzien van een voorbeeld van splitsen.

Gebruiker kan schakelen tussen de **Basismodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppeling wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de parameter-opties en de gebruiker voorbeelden is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie naar ons beter te begrijpen van het probleem. Als de gebruiker wil geen gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 


### <a name="editing-an-existing-transformation"></a>Een bestaande transformatie bewerken

Een gebruiker kunt bewerken van een bestaande **kolom splitsen per voorbeeld** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **geavanceerde modus**, en de voorbeelden die zijn opgegeven tijdens het maken van de transformatie worden weergegeven.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Voorbeelden van het splitsen van een vaste, één teken scheidingsteken

Het is gebruikelijk voor gegevensvelden die moeten worden gescheiden door een enkele vaste scheidingsteken, zoals een komma in een CSV-indeling. De Split-transformatie wil deze scheidingstekens automatisch afleiden. Bijvoorbeeld, in het volgende scenario voor het automatisch afleidt de '. ' als scheidingsteken.

### <a name="splitting-ip-addresses"></a>Splitsen IP-adressen

De waarden in de eerste kolom worden predictively onderverdeeld in vier kolommen.

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Voorbeelden van splitsen op meerdere scheidingstekens in bepaalde contexten

Gegevens van de gebruiker kan veel verschillende scheidingstekens scheiden van verschillende velden bevatten. Bovendien slechts enkele exemplaren van een scheidingsteken tekenreeks mogelijk, maar niet alle een scheidingsteken. Bijvoorbeeld, in het volgende geval de set met scheidingstekens vereist is '-',', 'en': ' om de gewenste uitvoer te produceren. Echter niet elk exemplaar van de ': ' moet een splitspunt omdat we niet wilt splitsen van de tijd, maar blijft deze in één kolom. De Split-transformatie infereert scheidingstekens binnen de context waarin deze zich in de ingevoerde gegevens in plaats van elke mogelijke exemplaar van het scheidingsteken voordoen. Er is ook de transformatie op de hoogte van veelvoorkomende gegevenstypen, zoals datums en tijden.   

### <a name="splitting-store-opening-timings"></a>Store openen tijdsinstellingen splitsen

De waarden in de volgende *tijdsinstellingen* kolom ophalen predictively splitsen in negen kolommen in de tabel daaronder wordt weergegeven.

|Tijdsinstellingen|
|:-----|
|Maandag tot vrijdag: 07:00:00 - 18:00:00 uur, zaterdag: 09:00:00 - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 09:00:00 - 18:00:00 uur, zaterdag: 4:00 am - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30:00 uur - 19:00 uur, zaterdag: 3:00 am - 2:30 pm, zondag: gesloten|
|Maandag tot vrijdag: 8:00 am - 6:00 uur, zaterdag: 2:00 uur - 15:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 4:00 uur - 19:00 uur, zaterdag: 9:00 am - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30 am - 4:30 pm, zaterdag: 09:00:00 - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 05:30:00 - 18:30:00 uur, zaterdag: 05:00:00 - 4:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:30:00 uur - 8:30 pm, zaterdag: 06:00:00 - 17:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 8:00 am - 21:00 uur, zaterdag: 9:00 - 8:00 uur, zondag: gesloten|
|Maandag tot vrijdag: 10:00-9:30:00, zaterdag: 9:30:00: 15:00 uur, zondag: gesloten|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Maandag|Vrijdag|7:00 uur|18:00:00 uur|Zaterdag|9:00 uur|17:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|9:00 uur|18:00:00 uur|Zaterdag|4:00 uur|4:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|8:30:00 uur|19:00 uur|Zaterdag|3:00 uur|2:30 pm|Zondag|Gesloten|
|Maandag|Vrijdag|8:00 uur|18:00:00 uur|Zaterdag|2:00 uur|15:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|4:00 uur|19:00 uur|Zaterdag|9:00 uur|4:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|8:30:00 uur|4:30 pm|Zaterdag|9:00 uur|17:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|5:30 uur|18:30:00 uur|Zaterdag|5:00 uur|4:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|8:30:00 uur|8:30 pm|Zaterdag|6:00 uur|17:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|8:00 uur|9:00 uur|Zaterdag|9:00 uur|8:00 uur|Zondag|Gesloten|
|Maandag|Vrijdag|10:00 uur|9:30 pm|Zaterdag|9:30 uur|15:00 uur|Zondag|Gesloten|

### <a name="splitting-iis-log"></a>Splitsen IIS-logboek

Hier volgt een voorbeeld van meerdere willekeurige scheidingstekens. In dit voorbeeld bevat ook een contextuele scheidingsteken '/', die niet moeten worden verdeeld in de URL's of bestandspaden. Het is omslachtig om uit te voeren deze splitsen met veel *kolom afleiden per voorbeeld* transformaties en gebruiksvoorbeelden voor elk veld. Met behulp van de transformatie splitsen kunnen we uitvoeren de voorspellende splitsing zonder eventuele voorbeelden.

|logtext|
|:-----|
|192.128.138.20--[16-oktober 2016 16:22:33-0200] "GET /images/picture.gif HTTP/1.1" 234 343 www.yahoo.com "http://www.example.com/" "Mozilla/4.0 (compatibel; MSIE 4)""-"|
|10.128.72.213--[17 oktober/2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com "http://www.sample.com/" "Mozilla 5.0 (MSIE)" "-"|
|192.165.71.165--[Nov-12-2016 14:22:44 -0500] "GET /sample.ico HTTP/1.1" 342 7342 www.facebook.com "-" "Mozilla 5.0 (Windows; U; Windows NT 5.1; RV:1.7.3) ""-"|
|10.166.64.165--[23-november 2016 01:52:45-0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla 5.0 (Windows)" "-"|
|192.167.1.193--[16/Jan/2017 22:34:56 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com 'http://www.illustration.com/index.html""Mozilla 5.0 (Windows)""-"|
|192.147.76.193--28/Jan/2017 26:36:16 +0800 "GET /search.php HTTP/1.1" 400 1777 www.bing.com "-" "Mozilla/4.0 (compatibel; MSIE 6.0; Windows NT 5.1)""-"|
|192.166.64.165--[23 maart/2017 01:55:25-0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "Mozilla 5.0 (Windows)" "-"|
|11.167.1.193--[16/Apr/2017 11:34:36 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com 'http://www.illustration.com/index.html""Mozilla 5.0 (Windows)""-"|

Met deze eigenschap wordt gesplitst in:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|16-oktober 2016|16:22:33|-0200|GET|images/Picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4.0|compatibel zijn. MSIE 4|
|10.128.72.213|17-oktober 2016|12:43:12|+0300|GET|news/stuff.html|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|November-12-2016|14:22:44|-0500|GET|Sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; RV:1.7.3|
|10.166.64.165|23-november 2016|01:52:45|-0800|GET|Style.CSS|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|16/Jan/2017|22:34:56|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|28/Jan/2017|26:36:16|+0800|GET|Search.php|HTTP|1.1|400|1777|www.bing.com|-|Mozilla|4.0|compatibel zijn. MSIE 6.0; Windows NT 5.1|
|192.166.64.165|23-maart-2017|01:55:25|-0800|GET|Style.CSS|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|16/Apr/2017|11:34:36|+0200|GET|js/ads.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Voorbeelden van het splitsen van zonder scheidingstekens
In sommige gevallen, er zijn geen daadwerkelijke scheidingstekens en gegevensvelden vlakdiagram naast elkaar kunnen optreden. In dit geval, detecteert de Split-transformatie automatisch patronen in de gegevens afleiden waarschijnlijk splitspunten. Bijvoorbeeld, in het volgende scenario willen we het bedrag van het valutatype scheiden en Split automatisch de grens tussen de cijfers en niet-numerieke gegevens als de splitspunt afleidt.

### <a name="splitting-amount-with-currency-symbol"></a>Hoeveelheid met valutasymbool splitsen

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

In het volgende voorbeeld willen we de waarden te scheiden van de eenheden. Opnieuw de Split-Deductie detecteert automatisch de grens van de betekenis en geeft de voorkeur aan deze via andere mogelijke scheidingstekens, zoals de '. ' teken. 

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

## <a name="technical-notes"></a>Opmerkingen bij de technische

De functie Split-transformatie is gebaseerd op de **voorspellende Program Synthesis** techniek. In deze techniek, zijn de programma's voor transformatie op basis van de ingevoerde gegevens automatisch geleerd. De programma's zijn gemaakt in een domein-specifieke taal. De DSL is gebaseerd op de velden die in het bijzonder reguliere expressie contexten optreden en scheidingstekens. Meer informatie over deze technologie kan worden gevonden een [recente publicatie van dit onderwerp](https://www.microsoft.com/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 

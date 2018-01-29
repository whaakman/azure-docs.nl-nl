---
title: Kolom afgeleid door voorbeeld transformatie met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de kolom door voorbeeld afgeleid transformatie
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
ms.openlocfilehash: 7ee5a720b12152c27a96ee18f1b11e5fc03a531a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="derive-column-by-example-transformation"></a>Kolom afgeleid door voorbeeld transformatie

De **kolom afgeleid door voorbeeld** transformatie kan gebruikers een afleiding van een of meer bestaande kolommen met behulp van de gebruiker opgegeven voorbeelden van het afgeleide resultaat maken. De afgeleide mag een combinatie van de ondersteunde tekenreeks, de datum en het aantal transformaties. 

Na de tekenreeks, datum en aantal transformaties worden ondersteund:

**Tekenreeks transformaties:** 

De subtekenreeks inclusief intelligent extractie van het aantal en de datums, samenvoeging geval manipulatie, toewijzing Constantewaarden.

**Datum transformaties:** 

De datumnotatie wijzigt, worden de onderdelen van een datum uitpakken, toewijzing van tijd tot tijd opslaglocaties.

De datum-transformaties zijn vrij algemeen met enkele belangrijke beperkingen:
* Tijdzones worden niet ondersteund.
* Sommige algemene indelingen die niet ondersteund worden:
    * ISO 8601 week van jaar-indeling (bijvoorbeeld ' 2009-W53-7') 
    * UNIX epoche tijd.
* Alle indelingen zijn hoofdlettergevoelig (met name '4 uur' wordt niet herkend als een tijd Hoewel "4 uur").

**Aantal transformaties:** 

Afronden, Floor, maximum, Binning, vullen met nullen of spatie, afdeling of vermenigvuldigen met een macht van 1000.

**Samengestelde transformaties:** 

Elke combinatie van een tekenreeks, getal of datum transformaties.

## <a name="how-to-use-this-transformation"></a>Het gebruik van deze transformatie

Deze transformatie, Ga als volgt te werk:
1. Selecteer een of meer kolommen die u wilt de waarde van afgeleid. 
2. Selecteer **kolom afgeleid door voorbeeld** van de **transformeert** menu. Of klik met de rechtermuisknop op de kop van een van de geselecteerde kolommen en selecteer **kolom afgeleid door voorbeeld** in het contextmenu. De transformatie-Editor wordt geopend en wordt een nieuwe kolom toegevoegd naast de juiste meest geselecteerde kolom. Geselecteerde kolommen kunnen worden geïdentificeerd door de selectievakjes in de kolomkoppen. Toevoegen en verwijderen van kolommen uit de selectie kunnen worden gedaan met behulp van de selectievakjes in de kolomkoppen.
3. Typ een voorbeeld van de *uitvoer* op basis van een rij- en druk op invoeren. Op dit moment analyseert de Workbench de invoerkolom, evenals de opgegeven uitvoer naar een programma dat de opgegeven invoer naar uitvoer Transformeren kunt bootsen. Het kunstmatige programma wordt uitgevoerd met de rijen in het gegevensraster. Voor niet-eenduidige en gecompliceerd gevallen mogelijk meerdere voorbeelden nodig. Afhankelijk van of u zich in de standaardmodus of geavanceerde modus, kunnen meerdere voorbeelden op verschillende manieren worden opgegeven.
4. Controleer de uitvoer en klik op **OK** te accepteren van de transformatie.

### <a name="transform-editor-basic-mode"></a>Editor transformeren: standaardmodus

Basic-modus biedt een inline bewerkingsfuncties in het gegevensraster. U kunt voorbeelden van de uitvoer opgeven door te navigeren naar de cel van belang en de waarde te typen. 

De workbench analyses van de gegevens en proberen te identificeren van de rand aanvragen die moeten worden gecontroleerd door de gebruiker. Terwijl de gegevens wordt geanalyseerd, **analyseren van gegevens** wordt weergegeven in de koptekst van de transformatie-Editor. Een van de analyse is voltooid, ofwel **Nee suggesties** of, **revisie van de volgende voorgestelde rij** wordt weergegeven in de koptekst. U kunt navigeren via de edge-aanvragen door te klikken op **revisie van de volgende voorgestelde rij**. Als de waarde onjuist voor een rij is, moet u in de juiste waarde key als extra voorbeeld. 

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus

Geavanceerde modus biedt een rijkere ervaring voor kolommen die zijn afgeleid door voorbeeld. Alle voorbeelden worden weergegeven op één plek. U kunt ook de rand gevallen in een oogopslag bekijken door te klikken op **voorgestelde voorbeelden**. 

U kunt in de geavanceerde modus rijen toevoegen als een van de Voorbeeldrij door te dubbelklikken op de rij in het raster. Een een rij als een van de Voorbeeldrij wordt gekopieerd, kunt u ook de gegevens in de kolommen in de gegevensbron waarmee een voorbeeld van een synthetische bewerken. Op deze manier kunt u aanvragen die niet zich momenteel in de voorbeeldgegevens toevoegen.

Gebruikers kan schakelen tussen de **standaardmodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppelen wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de gebruiker voorbeelden is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie zodat we kunnen het probleem te begrijpen. Als de gebruiker niet wil gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 

### <a name="editing-existing-transformation"></a>Bestaande transformatie bewerken

Gebruikers kunnen bewerken in een bestaande **kolom afgeleid door voorbeeld** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **geavanceerde modus**, en de voorbeelden die zijn opgegeven tijdens het maken van de transformatie worden weergegeven.

## <a name="examples-of-string-transformations-by-example"></a>Voorbeelden van tekenreeks transformaties door voorbeeld


>[!NOTE] 
>De waarden in **vet** vertegenwoordigen de voorbeelden die zijn opgegeven om te kunnen voltooien van de transformatie in de gegevensset weergegeven.


### <a name="s1-extracting-file-names-from-file-paths"></a>S1. Bestandsnamen extraheren uit bestandspaden

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Invoer|Uitvoer|
|:-----|:-----|
|C:\Python35\Tools\pynche\TypeinViewer.py|**TypeinViewer.py**|
|C:\Python35\Tools\pynche\webcolors.txt|webcolors.txt|
|C:\Python35\Tools\pynche\websafe.txt|websafe.txt|
|C:\Python35\Tools\pynche\X\rgb.txt|rgb.txt|
|C:\Python35\Tools\pynche\X\xlicense.txt|xlicense.txt|
|C:\Python35\Tools\Scripts\2to3.py|2to3.PY|
|C:\Python35\Tools\Scripts\analyze_dxp.py|**analyze_dxp.py**|
|C:\Python35\Tools\Scripts\byext.py|byext.PY|
|C:\Python35\Tools\Scripts\byteyears.py|byteyears.PY|
|C:\Python35\Tools\Scripts\checkappend.py|checkappend.py|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. Case manipulatie tijdens de extractie van tekenreeks

Aantal voorbeelden die vereist voor deze aanvraag zijn: 3

|Invoer|Uitvoer|
|:-----|:-----|
|KERSTDAGEN CT & ONBESTELBARE einde;  NIEUWE HANNOVER; Station 332; 2015-12-10 @ 17:10:52;|**New Hanover**|
|BRIAR pad & WHITEMARSH RG;  HATFIELD TOWNSHIP; Station 345; 2015-12-10 @ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-station: STA27;|**Norristown**|
|AIRY ST & ST KOOLRAAP;  NORRISTOWN; Station 308 bis; 2015-12-10 @ 16:47:36;|**Norristown**|
|CHERRYWOOD CT & ONBESTELBARE einde;  LAGERE POTTSGROVE; Station 329; 2015-12-10 @ 16:56:52;|Lagere Pottsgrove|
|CANNON AVE & W 9de ST;  LANSDALE; Station 345; 2015-12-10 @ 15:39:04;|Lansdale|
|LAURIER AVE & OAKDALE AVE;  HORSHAM; Station 352; 2015-12-10 @ 16:46:48;|Horsham|
|Extern bureaublad COLLEGEVILLE & LYWISKI RD;  SKIPPACK; Station 336; 2015-12-10 @ 16:17:05;|Skippack|
|MAIN ST & oude SUMNEYTOWN SNOEK;  LAGERE SALFORD; Station 344; 2015-12-10 @ 16:51:42;|Lagere Salford|
|BLUEROUTE & RAMP I476 NB NAAR CHEMISCHE RD; PLYMOUTH; 2015-12-10 @ 17:35:41;|Plymouth|
|RT202 PKWY & KNAPP RD; MONTGOMERY; 2015-12-10 @ 17:33:50;|Montgomery|
|SOORT RD & COLWELL RG; PLYMOUTH; 2015-12-10 @ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. De datumnotatie manipulatie tijdens de extractie van tekenreeks

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|:-----|:-----|
|MONTGOMERY AVE & WOODSIDE RD;  LAGERE MERION; Station 313; 2015-12-11 @ 04:11:35;|**12 november 2015 4 uur**|
|DREYCOTT RG & W LANCASTER AVE;  LAGERE MERION; Station 313; 2015-12-11 @ 01:29:52;|12 Nov 2015 1AM|
|EXTERN BUREAUBLAD-FABRIEK VOOR E-LEVERING & CONSHOHOCKEN STATUS RD; LAGERE MERION; 2015-12-11 @ 07:29:58;|12 Nov 2015 7AM|
|PENN VALLEY RD & MANOR RD;  LAGERE MERION; Station 313; 2015-12-10 @ 20:53:30;|12 Oct 2015 8 uur|
|BELMONT AVE & OVERHILL RD; LAGERE MERION; 2015-12-10 @ 23:02:27;|12 Oct 2015 23: 00 uur|
|W MONTGOMERY AVE & PENNSWOOD RD; LAGERE MERION; 2015-12-10 @ 19:25:22;|12 Oct 2015 19 uur|
|ROSEMONT AVE & ONBESTELBARE einde;  LAGERE MERION; Station 313; 2015-12-10 @ 18:43:07;|12 Oct 2015 18: 00 uur|
|AVIGNON DR & ONBESTELBARE einde; LAGERE MERION; 2015-12-10 @ 20:01:29-station: STA24;|12 Oct 2015 8 uur|

### <a name="s4-concatenating-strings"></a>S4. Tekenreeksen samenvoegen

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

>[!NOTE] 
>In dit voorbeeld · speciaal teken Hiermee geeft u spaties in de kolom uitvoer.

|Voornaam|Initialen|Achternaam|Uitvoer|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda·· Lohmann|
|Claudio|A|Kauwen|**Claudio·A·Chew**|
|Sarah-Jane|S|Smith|Sarah-Jane·S·Smith|
|Brandi||Blumenthal|Brandi·· Blumenthal|
|Jesusita|V|Reis|Jesusita·R·Journey|
|Hermina||Hults|Hermina·· Hults|
|Anne-Marie|W|Jones|Anne-Marie·W·Jones|
|Rico||Ropp|Rico··Ropp|
|Lauren-May||Fullmer|Lauren-May··Fullmer|
|Tony|D|Maine|Marc·T·Maine|
|Angie||Adelman|Angie··Adelman|
|John-Paul||Smith|John-Paul··Smith|
|Nummer|W|Staller|Song·W·Staller|
|Jill||Jefferies|Jill··Jefferies|
|Ruby-Grace|M|Simmons|Ruby-Grace·M·Simmons|

### <a name="s5-generating-initials"></a>S5. Initialen genereren

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Volledige naam|Uitvoer|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio kauwen|C.C.|
|Sarah ANS Smith|S.S.|
|Brandi Blumenthal|B.B.|
|Jesusita Journey|J.J.|
|Hermina Hults|H.H.|
|Anne Marie Jones|A.J.|
|Rico Ropp|R.R.|
|Lauren mei Fullmer|L.F.|
|Tony Maine|M.M.|
|Angie Adelman|A.A.|
|Jeroen Paul Smith|**J.S.**|
|Nummer Staller|S.S.|
|Jill Jefferies|J.J.|
|Ruby respijtperiode Simmons|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Toewijzing van constante waarden

Aantal voorbeelden die vereist voor deze aanvraag zijn: 3

|Administratieve geslacht|Uitvoer|
|:-----|:-----:|
|Man|**0**|
|Vrouw|**1**|
|Onbekend|**2**|
|Vrouw|1|
|Vrouw|1|
|Man|0|
|Onbekend|2|
|Man|0|
|Vrouw|1|

## <a name="examples-of-number-transformations-by-example"></a>Voorbeelden van aantal transformaties door voorbeeld

>[!NOTE] 
>De waarden in **vet** vertegenwoordigen de voorbeelden die zijn opgegeven om te kunnen voltooien van de transformatie in de gegevensset weergegeven.


### <a name="n1-rounding-to-nearest-10"></a>N1. Afronden op het dichtstbijzijnde 10

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. Omlaag naar de dichtstbijzijnde 10 afronden

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Invoer|Uitvoer|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. Afronden op het dichtstbijzijnde 0,05

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Invoer|Uitvoer|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Met binning

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|-----:|:-----:|
|20.16|**20-25**|
|14.32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. Schalen door 1000

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|-----:|-----:|
|-243|**-243000**|
|-12.5|-12500|
|-2345.23292|-2345232.92|
|-1202.3433|-1202343.3|
|1202.3433|1202343.3|

### <a name="n6-padding"></a>N6. Opvulling

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Coderen|Uitvoer|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Voorbeelden van datum transformaties door voorbeeld

>[!NOTE] 
>De waarden in **vet** vertegenwoordigen de voorbeelden die zijn opgegeven om te kunnen voltooien van de transformatie in de gegevensset weergegeven.


### <a name="d1-extracting-date-parts"></a>D1. Het uitpakken van de onderdelen van een datum

Deze onderdelen van een datum zijn geëxtraheerd met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in hun respectieve transformatie.

|Datum en tijd|werkdag|Datum|Maand|Jaar|Uur|Minuut|Tweede|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**Fri**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17-Jan-1990 13:32:01|wo|17|jan|1990|13|32|01|
|14-Feb-2034 05:36:07|di|14|feb|2034|5|36|07|
|14-Mar-2002 13:16:16|do|14|mrt|2002|13|16|16|
|21-Jan-1985 05:44:43|ma|21|jan|1985|5|44|**43**|
|16-Aug-1985 01:11:56|vr|16|aug|1985|1|11|56|
|20-Dec-2033 18:36:29|di|20|dec|2033|18|36|29|
|16-Jul-1984 10:21:59|ma|16|jul|1984|10|21|59|
|13-Jan-2038 10:59:36|wo|13|jan|2038|10|59|36|
|14-Aug-1982 15:13:54|za|14|aug|1982|15|13|54|
|22-Nov-2030 08:18:08|vr|22|nov|2030|8|18|08|
|21-Oct-1997 08:42:58|di|21|okt|1997|8|42|58|
|28-Nov-2006 14:19:15|di|28|nov|2006|14|19|15|
|29-Apr-2031 04:59:45|di|29|apr|2031|4|59|45|
|29-Jan-2032 02:38:36|do|29|jan|2032|2|38|36|
|11-May-2028 15:31:52|do|11|mei|2028|15|31|52|
|15-Jul-1977 12:45:39|vr|15|jul|1977|12|45|39|
|27-Jan-2029 05:55:41|za|27|jan|2029|5|55|41|
|03-Mar-2024 10:17:49|zo|3|mrt|2024|10|17|49|
|14-Apr-2010 00:23:13|wo|14|apr|2010|0|23|13|

### <a name="d2-formatting-dates"></a>D2. Datumnotatie

Deze datum formattings zijn gedaan met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in hun respectieve transformatie.

|Datum en tijd|Opmaak 1|Opmaak 2|Format3|Format4|Format5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**1/31/2031**|**Vrijdag 31 januari 2031**|**01312031 5:54**|**1-31/2031 5:54 UUR**|**Q1 2031**|
|17-Jan-1990 13:32:01|1/17/1990|Woensdag 17 januari 1990|01171990 13:32|17-1-1990 13:32 UUR|Q1 1990|
|14-Feb-2034 05:36:07|2/14/2034|Tuesday 14 februari 2034|02142034 5:36|2-14/2034 5:36 UUR|Q1 2034
|14-Mar-2002 13:16:16|3/14/2002|Donderdag 14 maart 2002|03142002 13:16|3-14/2002 13:16 UUR|Q1 2002
|21-Jan-1985 05:44:43|1/21/1985|Maandag 21 januari 1985|01211985 5:44|1-21/1985 5:44 UUR|Q1 1985
|16-Aug-1985 01:11:56|8/16/1985|Vrijdag 16 augustus 1985|08161985 1:11|8-16/1985 1:11 UUR|Q3 1985
|20-Dec-2033 18:36:29|12/20/2033|Tuesday 20 December 2033|12202033 18:36|20-12-2033 6:36 UUR|Q4 2033
|16-Jul-1984 10:21:59|7/16/1984|Maandag tot 16 juli 1984|07161984 10:21|16/7/1984 10:21:00|Q3 1984
|13-Jan-2038 10:59:36|1/13/2038|Woensdag 13 januari 2038|01132038 10:59|1-13/2038 10:59 UUR|Q1 2038
|14-Aug-1982 15:13:54|8/14/1982|Zaterdag 14 augustus 1982|08141982 15:13|8-14/1982 15:13:00|Q3 1982
|22-Nov-2030 08:18:08|11/22/2030|Vrijdag 22 November 2030|11222030 8:18|11-22/2030 08:18:00|Q4 2030
|21-Oct-1997 08:42:58|10/21/1997|Tuesday 21 oktober 1997|10211997 8:42|21/10/1997 8:42 UUR|Q4 1997
|28-Nov-2006 14:19:15|11/28/2006|Tuesday 28 November 2006|11282006 14:19|11-28-2006 14:19 UUR|Q4 2006
|29-Apr-2031 04:59:45|4/29/2031|Tuesday 29 April 2031|04292031 4:59|4-29/2031 4:59 UUR|Q2 2031
|29-Jan-2032 02:38:36|1/29/2032|Donderdag 29 januari 2032|01292032 2:38|1/29/2032 2:38 UUR|Q1 2032
|11-May-2028 15:31:52|5/11/2028|Donderdag 11 mei 2028|05112028 15:31|11/5/2028 3:31 PM|Q2 2028
|15-Jul-1977 12:45:39|7/15/1977|Vrijdag 15 juli 1977|07151977 12:45|7-15/1977 12:45 UUR|Q3 1977
|27-Jan-2029 05:55:41|1/27/2029|Zaterdag 27 januari 2029|01272029 5:55|1/27/2029 5:55 UUR|Q1 2029
|03-Mar-2024 10:17:49|3/3/2024|Zondag 3 maart 2024|03032024 10:17|3-3/2024 10:17:00|Q1 2024
|14-Apr-2010 00:23:13|4/14/2010|Woensdag 14 April 2010|04142010 0:23|4-14-2010 12:23 AM|Q2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Toewijzing van tijd tot tijd punten

Deze datum/tijd in periode toewijzingen zijn gedaan met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in hun respectieve transformatie.

|Datum en tijd|Period(seconds)|Period(MINUTES)|Periode (twee uur)|Periode (30 minuten)|
|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**0-20**|**45-60**|**5AM-7AM**|**5:30-6:00**|
|17-Jan-1990 13:32:01|**0-20**|30-45|1PM-3PM|13:30-14:00|
|14-Feb-2034 05:36:07|0-20|30-45|5AM-7AM|5:30-6:00|
|14-Mar-2002 13:16:16|0-20|15-30|1PM-3PM|13:00-13:30|
|21-Jan-1985 05:44:43|40-60|30-45|5AM-7AM|5:30-6:00|
|16-Aug-1985 01:11:56|40-60|0-15|1AM-3AM|1:00-1:30|
|20-Dec-2033 18:36:29|20-40|30-45|5PM-7PM|18:30-19:00|
|16-Jul-1984 10:21:59|40-60|15-30|9AM-11AM|10:00-10:30|
|13-Jan-2038 10:59:36|20-40|45-60|9AM-11AM|10:30-11:00|
|14-Aug-1982 15:13:54|40-60|0-15|3PM-5PM|15:00-15:30|
|22-Nov-2030 08:18:08|0-20|15-30|7AM-9AM|8:00-8:30|
|21-Oct-1997 08:42:58|40-60|30-45|7AM-9AM|8:30-9:00|
|28-Nov-2006 14:19:15|0-20|15-30|1PM-3PM|14:00-14:30|
|29-Apr-2031 04:59:45|40-60|45-60|3AM-5AM|4:30-5:00|
|29-Jan-2032 02:38:36|20-40|30-45|1AM-3AM|2:30-3:00|
|11-May-2028 15:31:52|40-60|30-45|3PM-5PM|15:30-16:00|
|15-Jul-1977 12:45:39|20-40|45-60|11AM-1PM|12:30-13:00|
|27-Jan-2029 05:55:41|40-60|45-60|5AM-7AM|5:30-6:00|
|03-Mar-2024 10:17:49|40-60|15-30|9AM-11AM|10:00-10:30|
|14-Apr-2010 00:23:13|0-20|15-30|11PM-1AM|0:00-0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Voorbeelden van samengestelde transformaties door voorbeeld

|tripduration|starttime|Start station-id|station breedtegraad starten|station lengtegraad starten|UserType|Kolom|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Abonnee|**Een abonnee gekozen een fiets vanaf station 107 lat/long (42.363,-71.088) op 08 Jan 2016 om ongeveer 4 uur. De duur reis zijn 61 minuten**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Klant|Een klant gekozen een fiets vanaf station 74 lat/long (42.373,-71.119) op 17 Jan 2016 op ongeveer 9: 00 uur. De duur reis zijn 61 minuten|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee gekozen een fiets vanaf station 176 lat/long (42.387,-71.119) op 25 Jan 2016 op ongeveer 8 uur. De duur reis zijn 62 minuten|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Abonnee|Een abonnee gekozen een fiets vanaf station 107 lat/long (42.363,-71.088) op 08 Jan 2016 op ongeveer 10 uur. De duur reis zijn 63 minuten|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Abonnee|Een abonnee gekozen een fiets vanaf station 68 lat/long (42.365,-71.103) op 15 Jan 2016 middags ongeveer 7. De duur reis is 64 minuten|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Abonnee|Een abonnee gekozen een fiets vanaf station 115 lat/long (42.388,-71.119) op 22 januari 2016 op ongeveer 18: 00 uur. De duur reis is 64 minuten|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Abonnee|Een abonnee gekozen een fiets vanaf station 178 lat/long (42.360,-71.101) op 18 Jan 2016 op ongeveer 9: 00 uur. De duur reis zijn 68 minuten|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee gekozen een fiets vanaf station 176 lat/long (42.387,-71.119) op 14 Jan 2016 op ongeveer 8 uur. De duur reis zijn 69 minuten|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Abonnee|Een abonnee gekozen een fiets vanaf station 141 lat/long (42.364,-71.082) op 13 januari 2016 om ongeveer 10 uur. De duur reis zijn 69 minuten|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee gekozen een fiets vanaf station 176 lat/long (42.387,-71.119) op 15 Jan 2016 op ongeveer 8 uur. De duur reis zijn 69 minuten|


## <a name="technical-notes"></a>Technische opmerkingen

### <a name="conditional-transformations"></a>Voorwaardelijke transformaties
In sommige gevallen kan een losse transformatie is niet gevonden die overeenkomt met de gegeven voorbeelden. In dergelijke gevallen probeert kolom afgeleid door voorbeeld transformeren groeperen van de invoer op basis van bepaalde patroon en meer informatie over afzonderlijke transformatie voor elke groep. We noemen dit **voorwaardelijke transformatie**. **Voorwaardelijke transformatie** alleen voor transformaties met één kolom invoer wordt uitgevoerd. 

### <a name="reference"></a>Referentie
Meer informatie over de transformatie tekenreeks door voorbeeld technologie vindt u in [deze publicatie](https://www.microsoft.com/en-us/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).

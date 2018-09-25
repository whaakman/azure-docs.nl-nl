---
title: Kolom afleiden per voorbeeld transformatie met behulp van Azure Machine Learning Workbench
description: Het referentiedocument voor de transformatie 'Kolom afleiden per voorbeeld'
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
ms.openlocfilehash: 311fd39792274ef01b1b03fdf8252eb7ac93c922
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978848"
---
# <a name="derive-column-by-example-transformation"></a>Kolom afleiden per voorbeeld transformatie

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



De **kolom afleiden per voorbeeld** transformatie kan gebruikers een afleiding van een of meer bestaande kolommen met behulp van de gebruiker opgegeven voorbeelden van het afgeleide resultaat maken. De afgeleide kan elke combinatie van de ondersteunde tekenreeks, de datum en het aantal transformaties zijn. 

De volgende tekenreeks en datum aantal transformaties worden ondersteund:

**Tekenreeks transformaties:** 

De subtekenreeks met inbegrip van intelligente extractie van aantal en de datums, samenvoegen, manipulatie van de aanvraag, toewijzing Constantewaarden.

**Gegevenstransformaties:** 

Datumnotatie gewijzigd, worden de onderdelen van een datum extraheren, toewijzing van tijd tot tijd opslaglocaties.

De datum-transformaties zijn vrij algemeen met enkele belangrijke beperkingen:
* Tijdzones worden niet ondersteund.
* Sommige algemene indelingen die niet ondersteund worden:
    * ISO 8601-week van jaar-indeling (bijvoorbeeld ' 2009-W53-7") 
    * UNIX epoche-tijd.
* Alle indelingen zijn hoofdlettergevoelig (met name "4 am" wordt niet herkend als een tijd Hoewel "4 AM").

**Aantal transformaties:** 

Ronde, verdieping, maximum, Binning, vullen met nullen of spatie, afdeling of vermenigvuldigen met een macht van 1000.

**Samengestelde transformaties:** 

Een combinatie van tekenreeks, getal of datum transformaties.

## <a name="how-to-use-this-transformation"></a>Het gebruik van deze transformatie

Als u wilt uitvoeren tijdens deze transformatie, de volgende stappen uit:
1. Selecteer een of meer kolommen die u wilt de waarde van afleiden. 
2. Selecteer **kolom afleiden per voorbeeld** uit de **transformeert** menu. Of klik met de rechtermuisknop op de kop van een van de geselecteerde kolommen en selecteer **kolom afleiden per voorbeeld** in het contextmenu. De transformatie-Editor wordt geopend en wordt een nieuwe kolom toegevoegd naast de juiste meest geselecteerde kolom. Geselecteerde kolommen kunnen worden geïdentificeerd door de selectievakjes in de kolomkoppen. Toevoegen en verwijderen van kolommen uit de selectie kunnen worden gedaan met behulp van de selectievakjes in de kolomkoppen.
3. Typ een voorbeeld van de *uitvoer* invoeren op basis van een rij, en druk op. Op dit moment analyseert de Workbench de invoerkolom, evenals de opgegeven uitvoer naar een programma dat de opgegeven invoer in de uitvoer kunt transformeren bootsen. Het synchronisatieprogramma wordt uitgevoerd op basis van alle rijen in het gegevensraster. Voor niet-eenduidige en complexe gevallen mogelijk meerdere voorbeelden nodig. Afhankelijk van of u zich in de standaardmodus of geavanceerde modus, kunnen meerdere voorbeelden op verschillende manieren worden opgegeven.
4. Bekijk de uitvoer en klikt u op **OK** de transformatie accepteert.

### <a name="transform-editor-basic-mode"></a>Editor transformeren: standaardmodus

Basic-modus biedt een inline-ervaring in het gegevensraster bewerken. U kunt vindt u voorbeelden van de uitvoer door te navigeren naar de cel van belang en de waarde te typen. 

De workbench analyseert de gegevens en er wordt geprobeerd om te identificeren van de randgevallen die moeten worden gecontroleerd door de gebruiker. Terwijl de gegevens wordt geanalyseerd, **analyseren van gegevens** wordt weergegeven in de kop van de Editor transformeren. Een van de analyse is voltooid, ofwel **geen suggesties** of **volgende voorgestelde rij controleren** wordt weergegeven in de header. U kunt door de randgevallen navigeren door te klikken op **volgende voorgestelde rij controleren**. Als de waarde onjuist voor een rij is, moet u in de juiste waarde sleutel als nog een voorbeeld. 

### <a name="transform-editor-advanced-mode"></a>Editor transformeren: geavanceerde modus

Geavanceerde modus biedt een rijkere ervaring voor het afleiden van kolommen per voorbeeld. Alle voorbeelden worden weergegeven op één plek. U kunt ook de randgevallen op één locatie bekijken door te klikken op **voorgestelde voorbeelden**. 

In de geavanceerde modus, kunt u een rij toevoegen als een van de Voorbeeldrij door te dubbelklikken op de rij in het raster. Een een rij als een van de Voorbeeldrij is gekopieerd, kunt u ook de gegevens in de kolommen in de gegevensbron te maken van een voorbeeld van een synthetische bewerken. Op deze manier kunt u aanvragen die niet op dit moment aanwezig zijn in de voorbeeldgegevens toevoegen.

Gebruiker kan schakelen tussen de **Basismodus** en de **geavanceerde modus** door te klikken op de koppelingen in de Editor transformeren.

### <a name="transform-editor-send-feedback"></a>Editor transformeren: Feedback verzenden

Te klikken op de **feedback verzenden** koppeling wordt geopend de **Feedback** dialoogvenster met het Opmerkingen vooraf ingevuld met de voorbeelden-gebruiker is opgegeven. Gebruiker moet de inhoud van het opmerkingenvak bekijken en vindt u meer informatie naar ons beter te begrijpen van het probleem. Als de gebruiker wil geen gegevens delen met Microsoft, gebruiker de vooraf ingevulde bijvoorbeeld gegevens verwijdert voordat u op de **Feedback verzenden** knop. 

### <a name="editing-existing-transformation"></a>Bestaande transformatie bewerken

Een gebruiker kunt bewerken van een bestaande **kolom afleiden per voorbeeld** transformeren door te selecteren **bewerken** optie van de transformatie-stap. Te klikken op **bewerken** opent de Editor transformeren in **geavanceerde modus**, en de voorbeelden die zijn opgegeven tijdens het maken van de transformatie worden weergegeven.

## <a name="examples-of-string-transformations-by-example"></a>Voorbeelden van tekenreeks transformaties per voorbeeld


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
|C:\Python35\Tools\Scripts\checkappend.py|checkappend.PY|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. De aanvraag manipulatie tijdens het uitpakken van de tekenreeks

Aantal voorbeelden die vereist voor deze aanvraag zijn: 3

|Invoer|Uitvoer|
|:-----|:-----|
|KERSTDAGEN CT & DEAD einde;  NIEUWE HANNOVER; Station 332; 2015-12-10 \@ 17:10:52;|**New Hanover**|
|BRIAR pad & WHITEMARSH LN;  HATFIELD TOWNSHIP; Station 345; 2015-12-10 \@ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 \@ 14:39:21-station: STA27;|**Norristown**|
|AIRY ST & ST KOOLRAAP;  NORRISTOWN; Station 308 bis; 2015-12-10 \@ 16:47:36;|**Norristown**|
|CHERRYWOOD CT & DEAD einde;  LAGERE POTTSGROVE; Station 329; 2015-12-10 \@ 16:56:52;|Lagere Pottsgrove|
|CANNON AVE & W 9de ST;  LANSDALE; Station 345; 2015-12-10 \@ 15:39:04;|Lansdale|
|LAURIER AVE ops & LAAN OAKDALE;  HORSHAM; Station 352; 2015-12-10 \@ 16:46:48;|Horsham|
|Extern bureaublad COLLEGEVILLE & LYWISKI RD;  SKIPPACK; Station 336; 2015-12-10 \@ 16:17:05;|Skippack|
|MAIN ST & oude SUMNEYTOWN naar PIKE;  LAGERE SALFORD; Station 344; 2015-12-10 \@ 16:51:42;|Lagere Salford|
|BLUEROUTE &AMP; RAMP I476 NB VOOR CHEMISCHE RD; PLYMOUTH; 2015-12-10 \@ 17:35:41;|Plymouth|
|RT202 PKWY &AMP; KNAPP RD; MONTGOMERY; 2015-12-10 \@ 17:33:50;|Montgomery|
|SOORT RD &AMP; COLWELL LN; PLYMOUTH; 2015-12-10 \@ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. Datumnotatie manipulatie tijdens het uitpakken van de tekenreeks

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|:-----|:-----|
|MONTGOMERY AVE & RD WOODSIDE;  LAGERE MERION; Station 313; 2015-12-11 \@ 04:11:35;|**12 november 2015 4 uur**|
|LN DREYCOTT & W LANCASTER AVE;  LAGERE MERION; Station 313; 2015-12-11 \@ 01:29:52;|12 november 2015 1 uur|
|EXTERN BUREAUBLAD E LEVERING FABRIEK &AMP; CONSHOHOCKEN STATUS RD; LAGERE MERION; 2015-12-11 \@ 07:29:58;|12 november 2015 7 uur|
|PENN VALLEY RD & RD MANOR;  LAGERE MERION; Station 313; 2015-12-10 \@ 20:53:30;|12 oktober 2015 8 uur|
|BELMONT AVE &AMP; RD OVERHILL; LAGERE MERION; 2015-12-10 \@ 23:02:27;|12 oktober 2015 23: 00 uur|
|W MONTGOMERY AVE &AMP; RD PENNSWOOD; LAGERE MERION; 2015-12-10 \@ 19:25:22;|12 oktober 2015 19 uur|
|ROSEMONT AVE & DEAD einde;  LAGERE MERION; Station 313; 2015-12-10 \@ 18:43:07;|12 oktober 2015 18: 00 uur|
|AVIGNON DR & DEAD einde; LAGERE MERION; 2015-12-10 \@ 20:01:29-station: STA24;|12 oktober 2015 8 uur|

### <a name="s4-concatenating-strings"></a>S4. Tekenreeksen samenvoegen

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

>[!NOTE] 
>In dit voorbeeld waren speciaal teken Hiermee geeft u spaties in de uitvoerkolom.

|Voornaam|Initialen|Achternaam|Uitvoer|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda·· Lohmann|
|Claudio|A|Kauwen|**Claudio·A·Chew**|
|Sarah ANS|S|Smith|Sarah-Jane·S·Smith|
|Brandi||Blumenthal|Brandi·· Blumenthal|
|Jesusita|R|Traject|Jesusita·R·Journey|
|Hermina||Hults|Hermina·· Hults|
|Anne Marie|W|Jones|Anne-Marie·W·Jones|
|Rico||Ropp|Rico··Ropp|
|Lauren mei||Fullmer|Lauren May·· Fullmer|
|Marc|T|Maine|Marc· T· Maine|
|Angie||Adelman|Angie··Adelman|
|John Paul||Smith|John Paul·· De Vries|
|Nummer|W|Staller|Song· W· Staller|
|Marie||Jefferies|Jill·· Jefferies|
|Ruby-respijtperiode|M|Simmons|Ruby-Grace· M· Simmons|

### <a name="s5-generating-initials"></a>S5. De initialen genereren

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Volledige naam|Uitvoer|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio kauwen|C.C.|
|Sarah Jan de Vries|S.S.|
|Brandi Blumenthal|B.B.|
|Jesusita traject|J.J.|
|Hermina Hults|H.H.|
|Anne Marie Jones|A.J.|
|Rico Ropp|R.R.|
|Lauren mei Fullmer|L.F.|
|Marc Maine|M.M.|
|Angie Adelman|A.A.|
|John Paul Smith|**J.S.**|
|Nummer Staller|S.S.|
|Marie Jefferies|J.J.|
|Ruby-respijtperiode Simmons|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Constante waarden toewijzen

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

## <a name="examples-of-number-transformations-by-example"></a>Voorbeelden van getaltransformaties per voorbeeld

>[!NOTE] 
>De waarden in **vet** vertegenwoordigen de voorbeelden die zijn opgegeven om te kunnen voltooien van de transformatie in de gegevensset weergegeven.


### <a name="n1-rounding-to-nearest-10"></a>N1. Afronden naar de dichtstbijzijnde 10

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. Afronding omlaag naar dichtstbijzijnde 10

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Invoer|Uitvoer|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. Afronden naar de dichtstbijzijnde 0,05

Aantal voorbeelden die vereist voor deze aanvraag zijn: 2

|Invoer|Uitvoer|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Binning

Aantal voorbeelden die vereist voor deze aanvraag zijn: 1

|Invoer|Uitvoer|
|-----:|:-----:|
|20.16|**20-25**|
|14.32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. Schalen met 1000

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

|Code|Uitvoer|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Voorbeelden van gegevenstransformaties per voorbeeld

>[!NOTE] 
>De waarden in **vet** vertegenwoordigen de voorbeelden die zijn opgegeven om te kunnen voltooien van de transformatie in de gegevensset weergegeven.


### <a name="d1-extracting-date-parts"></a>D1. Onderdelen van een datum extraheren

Deze onderdelen zijn geëxtraheerd met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in de transformatie van hun respectieve.

|DateTime|Weekdag|Date|Maand|Jaar|Uur|Minuut|Seconde|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**Vr**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17-Jan-1990 13:32:01|wo|17|jan|1990|13|32|01|
|14-Feb-2034 05:36:07|di|14|feb|2034|5|36|07|
|14 maart 2002 13:16:16|do|14|mrt|2002|13|16|16|
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
|11-mei-2028 15:31:52|do|11|Mei|2028|15|31|52|
|15-Jul-1977 12:45:39|vr|15|jul|1977|12|45|39|
|27-Jan-2029 05:55:41|za|27|jan|2029|5|55|41|
|03-maart-2024 10:17:49|zo|3|mrt|2024|10|17|49|
|14-Apr-2010 00:23:13|wo|14|apr|2010|0|23|13|

### <a name="d2-formatting-dates"></a>D2. Datumnotatie

Deze datum formattings zijn uitgevoerd met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in de transformatie van hun respectieve.

|DateTime|Opmaak 1|Opmaak 2|Format3|Format4|Format5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**1/31/2031**|**Vrijdag 31 januari 2031**|**01312031 5:54**|**1-31/2031 5:54 UUR**|**Q1 2031**|
|17-Jan-1990 13:32:01|1/17/1990|Woensdag 17 januari 1990|01171990 13:32|1 17 1990 1:32 UUR|Q1 1990|
|14-Feb-2034 05:36:07|2/14/2034|Dinsdag 14 februari 2034|02142034 5:36|14/2/2034 5:36 UUR|Q1 2034
|14 maart 2002 13:16:16|3/14/2002|Donderdag 14 maart 2002|03142002 13:16|14/3/2002 13:16 UUR|Q1 2002
|21-Jan-1985 05:44:43|1/21/1985|Maandag 21 januari 1985|01211985 5:44|21/1/1985 5:44 UUR|Q1 1985
|16-Aug-1985 01:11:56|8/16/1985|Vrijdag 16 augustus 1985|08161985 1:11|8-16/1985 1:11 UUR|Q3 1985
|20-Dec-2033 18:36:29|12/20/2033|Dinsdag 20 December 2033|12202033 18:36|20-12/2033 6:36 UUR|Q4 2033
|16-Jul-1984 10:21:59|7/16/1984|Maandag tot 16 juli 1984|07161984 10:21|16/7/1984 10:21 UUR|Q3 1984
|13-Jan-2038 10:59:36|1/13/2038|Woensdag 13 januari 2038|01132038 10:59|13/1/2038 10:59 UUR|Q1 2038
|14-Aug-1982 15:13:54|8/14/1982|Zaterdag 14 augustus 1982|08141982 15:13|14/8/1982 15:13:00|Q3 1982
|22-Nov-2030 08:18:08|11/22/2030|Vrijdag 22 November 2030|11222030 8:18|22/11/2030 8:18:00 UUR|Q4 2030
|21-Oct-1997 08:42:58|10/21/1997|Dinsdag 21 oktober 1997|10211997 8:42|21/10/1997 8:42 UUR|Q4 1997
|28-Nov-2006 14:19:15|11/28/2006|Dinsdag 28 November 2006|11282006 14:19|28/11/2006 14:19 UUR|Q4 2006
|29-Apr-2031 04:59:45|4/29/2031|Dinsdag 29 April 2031|04292031 4:59|29/4/2031 4:59 UUR|Q2 2031
|29-Jan-2032 02:38:36|1/29/2032|Donderdag 29 januari 2032|01292032 2:38|29/1/2032 2:38 UUR|Q1 2032
|11-mei-2028 15:31:52|5/11/2028|Donderdag 11 mei 2028|05112028 15:31|11/5/2028 15:31 UUR|Q2 2028
|15-Jul-1977 12:45:39|7/15/1977|Vrijdag, 15 juli 1977|07151977 12:45|15/7/1977 12:45 UUR|Q3 1977
|27-Jan-2029 05:55:41|1/27/2029|Zaterdag 27 januari 2029|01272029 5:55|27/1/2029 05:55:00|Q1 2029
|03-maart-2024 10:17:49|3/3/2024|Zondag 3 maart 2024|03032024 10:17|3-3/2024 10:17 UUR|Q1 2024
|14-Apr-2010 00:23:13|4/14/2010|Woensdag 14 April 2010|04142010 0:23|4-14-2010 12:23:00 UUR|Q2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Toewijzing van tijd tot tijd punten

Deze datum/tijd naar periode toewijzingen zijn uitgevoerd met behulp van verschillende door voorbeeld transformaties op dezelfde gegevensset. Vet tekenreeksen vertegenwoordigen in de voorbeelden die zijn opgegeven in de transformatie van hun respectieve.

|DateTime|Period(seconds)|Period(MINUTES)|Periode (twee uur)|Periode (30 minuten)|
|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**0-20**|**45-60**|**5AM-7AM**|**5:30-6:00**|
|17-Jan-1990 13:32:01|**0-20**|30-45|1PM-3PM|13:30-14:00|
|14-Feb-2034 05:36:07|0-20|30-45|5AM-7AM|5:30-6:00|
|14 maart 2002 13:16:16|0-20|15-30|1PM-3PM|13:00-13:30|
|21-Jan-1985 05:44:43|40-60|30-45|5AM-7AM|5:30-6:00|
|16-Aug-1985 01:11:56|40-60|0-15|1AM-3AM|1:00-1:30|
|20-Dec-2033 18:36:29|20-40|30-45|5PM-7PM|18:30-19:00|
|16-Jul-1984 10:21:59|40-60|15-30|9: 00 - 11 AM|10:00-10:30|
|13-Jan-2038 10:59:36|20-40|45-60|9: 00 - 11 AM|10:30-11:00|
|14-Aug-1982 15:13:54|40-60|0-15|3PM-5PM|15:00-15:30|
|22-Nov-2030 08:18:08|0-20|15-30|7AM-9AM|8:00-8:30|
|21-Oct-1997 08:42:58|40-60|30-45|7AM-9AM|8:30-9:00|
|28-Nov-2006 14:19:15|0-20|15-30|1PM-3PM|14:00-14:30|
|29-Apr-2031 04:59:45|40-60|45-60|3AM-5AM|4:30-5:00|
|29-Jan-2032 02:38:36|20-40|30-45|1AM-3AM|2:30-3:00|
|11-mei-2028 15:31:52|40-60|30-45|3PM-5PM|15:30-16:00|
|15-Jul-1977 12:45:39|20-40|45-60|11AM-1PM|12:30-13:00|
|27-Jan-2029 05:55:41|40-60|45-60|5AM-7AM|5:30-6:00|
|03-maart-2024 10:17:49|40-60|15-30|9: 00 - 11 AM|10:00-10:30|
|14-Apr-2010 00:23:13|0-20|15-30|11PM-1AM|0:00-0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Voorbeelden van samengestelde transformaties per voorbeeld

|tripduration|StartTime|Start station id|Start station latitude|Start station lengtegraad|UserType|Kolom|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Abonnee|**Een abonnee een fiets uit station 107 lat/lange (42.363,-71.088) op 08 Jan 2016 om ongeveer 4 uur opgehaald. De duur van de tocht zijn 61 minuten**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Klant|Een klant een fiets uit station 74 lat/lange (42.373,-71.119) op 17 Jan 2016 om ongeveer 9 uur opgehaald. De duur van de tocht zijn 61 minuten|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee een fiets uit station 176 lat/lange (42.387,-71.119) op 25 januari 2016 om ongeveer 8 uur opgehaald. De duur van de tocht zijn 62 minuten|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Abonnee|Een abonnee een fiets uit station 107 lat/lange (42.363,-71.088) op 08 Jan 2016 om ongeveer 10 uur opgehaald. De duur van de tocht is 63 minuten|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Abonnee|Een abonnee een fiets uit station 68 lat/lange (42.365,-71.103) op 15 januari 2016 om ongeveer 7 uur opgehaald. De duur van de tocht is 64 minuten|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Abonnee|Een abonnee een fiets uit station 115 lat/lange (42.388,-71.119) op 22 januari 2016 op ongeveer 18: 00 uur opgehaald. De duur van de tocht is 64 minuten|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Abonnee|Een abonnee een fiets uit station 178 lat/lange (42.360,-71.101) op 18 januari 2016 om ongeveer 9 uur opgehaald. De duur van de tocht zijn 68 minuten|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee een fiets uit station 176 lat/lange (42.387,-71.119) op 14 Jan 2016 om ongeveer 8 uur opgehaald. De duur van de tocht zijn 69 minuten|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Abonnee|Een abonnee een fiets uit station 141 lat/lange (42.364,-71.082) op 13 januari 2016 om ongeveer 10 uur opgehaald. De duur van de tocht zijn 69 minuten|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Abonnee|Een abonnee een fiets uit station 176 lat/lange (42.387,-71.119) op 15 januari 2016 om ongeveer 8 uur opgehaald. De duur van de tocht zijn 69 minuten|


## <a name="technical-notes"></a>Opmerkingen bij de technische

### <a name="conditional-transformations"></a>Voorwaardelijke transformaties
In sommige gevallen kan een enkele transformatie is niet gevonden die overeenkomt met de opgegeven voorbeelden. In dergelijke gevallen probeert kolom afleiden per voorbeeld transformeren voor het groeperen van de invoer op basis van bepaalde patroon en informatie over afzonderlijke transformatie voor elke groep. We noemen dit **voorwaardelijke transformatie**. **Voorwaardelijke transformatie** alleen voor transformaties met één kolom invoer wordt uitgevoerd. 

### <a name="reference"></a>Referentie
Meer informatie over de transformatie tekenreeks door een voorbeeld van de technologie kan worden gevonden [deze publicatie](https://www.microsoft.com/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).

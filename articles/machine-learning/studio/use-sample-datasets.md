---
title: Gebruik de voorbeeldgegevenssets in Machine Learning Studio | Microsoft Docs
description: Beschrijvingen van de gegevenssets in samplemodellen opgenomen in Machine Learning Studio gebruikt. U kunt deze voorbeeldgegevenssets gebruiken voor uw experimenten.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 03a0b844-e8a7-4896-996f-d3c7a0db7a50
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: garye
ms.openlocfilehash: 3339d3b3bab2c0ce210950c178ef03fc6c0ecab9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-studio"></a>De voorbeeldgegevenssets in Azure Machine Learning Studio gebruiken
[top]: #machine-learning-sample-datasets

Wanneer u een nieuwe werkruimte in Azure Machine Learning maakt, worden een aantal voorbeeldgegevenssets en experimenten standaard opgenomen. Veel van deze voorbeeldgegevenssets worden gebruikt door de voorbeeld-modellen in de [AI-galerie van Azure](http://gallery.cortanaintelligence.com/). Andere zijn opgenomen als voorbeelden van verschillende soorten gegevens doorgaans gebruikt in machine learning.

Sommige van deze gegevenssets zijn beschikbaar in Azure Blob-opslag. Voor deze gegevenssets bevat de volgende tabel een directe koppeling. U kunt deze gegevenssets in uw experimenten gebruiken met behulp van de [importgegevens] [ import-data] module.

De rest van deze voorbeeldgegevenssets zijn beschikbaar in uw werkruimte onder **gegevenssets opgeslagen**. U kunt dit vinden in het modulepalet links van het experimentcanvas in Machine Learning Studio.
U kunt een van deze gegevenssets in uw eigen experimenten gebruiken door naar het experimentcanvas te slepen.


[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

<table>

<tr>
  <th>Naam van de gegevensset</th>
  <th>Beschrijving van gegevensset</th>
</tr>

<tr>
  <td>Volwassenen inventarisering Income binaire classificatie gegevensset</td>
  <td>
Een subset van de database 1994 telling, met behulp van werkende volwassenen ouder zijn dan 16 met een aangepaste inkomsten index met > 100.
<p></p>
<b>Gebruik:</b> classificeren van met demografische gegevens te voorspellen of een persoon meer dan 50 K per jaar verdient personen.
<p></p>
<b>Verwante Research:</b> Kohavi, R., Becker, B., (1996). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Luchthaven Codes gegevensset</td>
  <td>
VS luchthaven codes.
<p></p>
Deze gegevensset bevat een rij voor elke luchthaven VS, waarbij de luchthaven id-nummer en de naam samen met de locatie plaats en provincie.
  </td>
</tr>

<tr>
  <td>Auto price data (Raw)</td>
  <td>
Informatie over auto's door merk en model, inclusief de prijs van functies, zoals het aantal cilinders en MPG, evenals een verzekering risicoscore.
<p></p>
De risicoscore is in eerste instantie aan prijs voor automatisch gekoppeld. Vervolgens wordt het werkelijke risico's in een proces dat bekend is bij actuarissen als symboling aangepast. Een waarde van + 3 geeft aan dat de automatische riskant, en een waarde van -3 dat deze waarschijnlijk veilig is.
<p></p>
<b>Gebruik:</b> voorspellen van de risicoscore door onderdelen, met regressie of multivariate classificatie. 
<p></p>
<b>Verwante Research:</b> Schlimmer, J.C. (1987). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Fiets verhuur UCI gegevensset</td>
  <td>
UCI fiets verhuur gegevensset op basis van echte gegevens van kapitaal Bikeshare bedrijf dat een fiets verhuur-netwerk in Washington DC onderhoudt.
<p></p>
De gegevensset heeft een rij voor elk uur van elke dag in 2011 en 2012 voor een totaal van 17,379 rijen. Het bereik van elk uur fiets huren ligt tussen 1 en 977.

  </td>
</tr>

<tr>
  <td>Factuur poorten RGB-afbeelding</td>
  <td>
Openbaar afbeeldingsbestand converteren naar CSV-gegevens.
<p></p>
De code voor het omzetten van de installatiekopie is opgegeven de <strong>kwantisatiefouten gebruik K-Means clustering kleur</strong> detailpagina model.
  </td>
</tr>

<tr>
  <td>Bloed donatie gegevens</td>
  <td>
Een subset van gegevens uit de database van de donor bloed van de bloed transfusie Service Center van Hsin Chu stad, Taiwan.
<p></p>
Donor gegevens omvatten de maanden sinds de laatste donatie), en frequentie of het totale aantal donaties, tijd sinds laatste donatie en hoeveelheid laatste officieel gedoneerd.
<p></p>
<b>Gebruik:</b> het doel is om te voorspellen via classificatie of de donor gedoneerd bloed in maart 2007, waarbij 1 geeft aan een donor tijdens de doelperiode en 0 dat niet donor. 
<p></p>
<b>Verwante Research:</b> Yeh I.C., (2008). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke
<p></p>
Yeh, ik-Cheng, Yang, koning-Jang en Ting, Tao-Ming ' Knowledge detectie op RFM model Bernoulli-reeks met ' Expert systemen met toepassingen, 2008 <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr>
  <td>Gegevens en</td>
  <td>
Een van drie kanker-gerelateerde gegevenssets geleverd door de Oncology Institute die vaak in machine learning-documentatie verschijnt. Diagnostische gegevens met functies van een laboratorium-analyse van ongeveer 300 weefsel voorbeelden combineert.
<p></p>
<b>Gebruik:</b> classificeren van het type van kanker, op basis van 9 kenmerken, waarvan sommige zijn lineaire en andere categorische. 
<p></p>
<b>Verwante Research:</b> Wohlberg, W.H., straat, W.N. & Mangasarian, O.L. (1995). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>En kanker functies <td>
De gegevensset bevat informatie voor 102K verdachte regio's (kandidaten) van volledige-installatiekopieën, door 117 functies beschreven. De functies zijn eigen en hun betekenis niet door de beveiligingsbeheerder gegevensset (Siemens gezondheidszorg) wordt getoond. 
  </td>
</tr>

<tr>
  <td>En kanker Info</td>
  <td>
De gegevensset bevat aanvullende informatie voor elke verdachte regio van de volledige installatiekopie. Elk voorbeeld bevat informatie (bijvoorbeeld label patiënt coördinaten van de patch ten opzichte van de volledige afbeelding-ID) over het nummer van de bijbehorende rij in de gegevensset en kanker functies. Elke geduld heeft een aantal voorbeelden. Voorbeelden zijn positief voor patiënten die beschikken over een kanker, en sommige negatief zijn. Voor patiënten die niet beschikken over een kanker, zijn alle voorbeelden negatief. De gegevensset heeft 102K voorbeelden. De gegevensset is gericht, 0,6% van de punten positief zijn, de rest negatief zijn. De gegevensset is door Siemens gezondheidszorg beschikbaar gesteld.
  </td>
</tr>

<tr>
  <td>CRM Appetency Labels gedeeld</td>
  <td>
Labels van de uitdaging KDD kop 2009 klant relatie voorspelling (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr>
  <td>CRM verloop Labels gedeeld</td>
  <td>
Labels van de uitdaging KDD kop 2009 klant relatie voorspelling (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr>
  <td>Gedeelde CRM-gegevensset</td>
  <td>
Deze gegevens zijn afkomstig van de uitdaging KDD kop 2009 klant relatie voorspelling (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).
<p></p>
De gegevensset bevat 50K klanten van het bedrijf Frans telecommunicatie oranje. Elke klant heeft 230 geanonimiseerde functies, 190 waarvan numerieke worden en 40 zijn categorische. De functies zijn zeer verspreid.
  </td>
</tr>

<tr>
  <td>CRM Upselling Labels gedeeld</td>
  <td>
Labels van de uitdaging KDD kop 2009 klant relatie voorspelling (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td>Energie-efficiëntie regressie gegevens</td>
  <td>
Een verzameling gesimuleerde energie-profielen, op basis van 12 ander gebouw vormen. De gebouwen van elkaar worden onderscheiden door acht functies. Dit omvat glas gebied, de glas gebied distributie en de afdrukstand.
<p></p>
<b>Gebruik:</b> regressie of classificatie gebruiken om te voorspellen van de energie-efficiëntie classificatie gebaseerde als een van twee antwoorden in de werkelijke waarden. Voor meerdere klasse-indeling, is het afronden van de variabele in antwoord op het dichtstbijzijnde gehele getal. 
<p></p>
<b>Verwante Research:</b> Xifara, A. & Tsanas, A. (2012). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Vlucht uitstelt gegevens</td>
  <td>
Passagiers vlucht tijdige prestatiegegevens die afkomstig zijn uit de verzameling TranStats van de Verenigde Staten Afdeling transport (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">tijdige</a>).
<p></p>
De gegevensset bevat informatie over de periode April oktober 2013. Voordat u uploadt naar Azure Machine Learning Studio, is als volgt de gegevensset verwerkt:
<ul>
  <li>De gegevensset is gefilterd ten aanzien van alleen de 70 drukste luchthavens de Europese VS</li>
  <li>Geannuleerde vlucht zijn gelabeld als door meer dan 15 minuten vertraagd</li>
  <li>Omgereden vlucht zijn gefilterd.</li>
  <li>De volgende kolommen zijn geselecteerd: jaar, maand, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, geannuleerd</li>
</ul>
</td>
</tr>

<tr>
  <td>Vlucht over tijdige prestaties (Raw)</td>
  <td>
Records van het vliegtuig vlucht ontvangsten en afwijkingen in de Verenigde Staten van oktober 2011.
<p></p>
<b>Gebruik:</b> vertragingen te voorspellen. 
<p></p>
<b>Verwante Research:</b> van VS afdeling transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td>Bosbrandgegevens</td>
  <td>
Bevat de weergegevens, zoals temperatuur en vochtigheid indexen en o-snelheid. De gegevens worden opgehaald uit een gebied van noordoosten van Portugal, gecombineerd met records van bosbranden.
<p></p>
<b>Gebruik:</b> dit is een taak moeilijk regressie waar het doel is het gebrande gebied van bosbranden voorspellen. 
<p></p>
<b>Verwante Research:</b> Cortez, P., & Morais, A. (2008). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke
<p></p>
[Cortez en Morais, 2007] P. Cortez en A. Morais. Een Data Mining benadering voorspellen bosbranden met behulp van meteorologische gegevens. In J. Neves, Dhr F. Santos en J. Machado Eds., nieuwe Trends in kunstmatige Intelligence, de procedures van de 13 EPIA 2007 - Portugees Conferentie over kunstmatige intelligentie, December, Guimarães, Portugal p. 512-523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Beschikbaar op: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr>
  <td>Duits creditcard UCI gegevensset</td>
  <td>
De gegevensset UCI Statlog Duits creditcard (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Duits + tegoed + gegevens</a>), met behulp van het bestand german.data.
<p></p>
De gegevensset classificeert personen, beschreven door een set kenmerken als lage of hoge kredietrisico's. Elk voorbeeld vertegenwoordigt een persoon. Er zijn functies van 20, numerieke en categorische, en een binaire label (tegoed risico waarde). Hoge tegoed risico vermeldingen label hebben = 2, lage tegoed risico vermeldingen label hebben = 1. De kosten van een voorbeeld van een laag risico zo hoog onjuiste is 1, terwijl de kosten van het onjuiste een voorbeeld van een hoog risico zo klein 5 is.
  </td>
</tr>

<tr>
  <td>IMDB Filmtitels</td>
  <td>
De gegevensset bevat informatie over films die zijn geclassificeerd in Twitter tweets: IMDB film-ID, naam film genre en productie jaar. Er zijn 17K films in de gegevensset. De gegevensset is geïntroduceerd in het artikel 'S. Dooms, T. De Pessemier en L. Martens. MovieTweetings: een classificatie van gegevensset film verzameld van Twitter. Workshop over Crowdsourcing en menselijke berekeningen voor Recommender systemen, CrowdRec op RecSys 2013."
  </td>
</tr>

<tr>
  <td>IRIS twee klassegegevens</td>
  <td>
Dit is mogelijk de beste bekende database worden gevonden in de documentatie van de erkenning patroon. De gegevensset is relatief klein, met 50 voorbeelden van Bloemblad metingen van drie iris die.
<p></p>
<b>Gebruik:</b> voorspellen van het type van de iris van de metingen.  
<p></p>
<b>Verwante Research:</b> Fisher, R.A. (1988). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Film Tweets</td>
  <td>
De gegevensset is een uitgebreide versie van de gegevensset film Tweetings. De gegevensset heeft 170K classificaties voor films, opgehaald uit gestructureerde tweets op Twitter. Elk exemplaar vertegenwoordigt een tweet en is een tuple: gebruikersnaam, IMDB film-ID, classificatie, timestamp, aantal favorieten voor deze tweet en aantal retweets van deze tweet. De gegevensset is beschikbaar gesteld door A. gezegd, S. Dooms, B. Loni en D. Tikk voor Recommender systemen uitdaging 2014.
  </td>
</tr>

<tr>
  <td>MPG-gegevens voor verschillende auto 's</td>
  <td>
Deze gegevensset is een enigszins gewijzigde versie van de gegevensset die is geleverd door de bibliotheek StatLib van Carnegie Mellon universiteit. De gegevensset is gebruikt in het 1983 American statistische koppeling.
<p></p>
De gegevens worden brandstofverbruik voor verschillende auto's in mijl per gallon. Dit omvat ook informatie zoals het aantal cilinders, engine verplaatsing, paardenkracht, totale gewicht en versnelling.
<p></p>
<b>Gebruik:</b> voorspellen brandstofverbruik op basis van drie waarden discrete kenmerken en vijf doorlopende kenmerken. 
<p></p>
<b>Verwante Research:</b> StatLib Carnegie Mellon universiteit, (1993). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Pima Indianen Diabetes binaire classificatie gegevensset</td>
  <td>
Een subset van gegevens van de National Institute Diabetes en database spijsverteringskanaal en nier worden toegekend. De gegevensset is te concentreren op vrouwelijke patiënten van Pima Indiase erfgoed gefilterd. De gegevens omvatten medische gegevens zoals glucose en of niveaus, evenals lifestyle factoren.
<p></p>
<b>Gebruik:</b> voorspellen of het onderwerp diabetes (binaire classificatie) heeft. 
<p></p>
<b>Verwante Research:</b> Sigillito, V. (1990). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml '</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke </td>
</tr>

<tr>
  <td>Klantgegevens restaurant</td>
  <td>
Een set van metagegevens over klanten, waaronder demografische gegevens en voorkeuren.
<p></p>
<b>Gebruik:</b> deze gegevensset gebruiken in combinatie met de andere twee restaurant gegevenssets, te trainen en te testen van een systeem recommender. 
<p></p>
<b>Verwante Research:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en wetenschappelijke van de Computer.
  </td>
</tr>

<tr>
  <td>Gegevens van de functie restaurant</td>
  <td>
Een set van metagegevens over restaurant en hun functies, zoals voeding, consumentenscenario-stijl, en locatie.
<p></p>
<b>Gebruik:</b> deze gegevensset gebruiken in combinatie met de andere twee restaurant gegevenssets, te trainen en te testen van een systeem recommender. 
<p></p>
<b>Verwante Research:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en wetenschappelijke van de Computer.
  </td>
</tr>

<tr>
  <td>Restaurant classificaties</td>
  <td>
Classificaties krijgen door gebruikers restaurant op een schaal van 0 tot 2 bevat.
<p></p>
<b>Gebruik:</b> deze gegevensset gebruiken in combinatie met de andere twee restaurant gegevenssets, te trainen en te testen van een systeem recommender. 
<p></p>
<b>Verwante Research:</b> Bache, K. en Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en wetenschappelijke van de Computer.
  </td>
</tr>

<tr>
  <td>Stalen Annealing meerdere klasse-gegevensset</td>
  <td>
Deze gegevensset bevat een reeks van records uit staal aanhechten proefversies. Deze bevat de fysieke kenmerken (breedte, dikte, type (gesloten, blad, etc.) van de resulterende stalen typen.
<p></p>
<b>Gebruik:</b> een van twee numerieke klassenkenmerken; hardheid of sterkte te voorspellen. U kunt ook correlaties tussen kenmerken analyseren.
<p></p>
Staalsoorten Volg een standaard set gedefinieerd door SAE en andere organisaties. U zoekt naar een specifieke 'hoogwaardige' (de klassevariabele) en wilt weten over de waarden die nodig zijn. 
<p></p>
<b>Verwante Research:</b> pond, D. & Buntine, W. (NA). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens en Computer wetenschappelijke
<p></p>
Een handig handleiding voor staalsoorten vindt u hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td>Telescoop gegevens</td>
  <td>
Record van hoge energie gamma partikel barst samen met achtergrondruis beide gesimuleerd met behulp van een Monte Carlo-proces.
<p></p>
De bedoeling van de simulatie is voor het verbeteren van de nauwkeurigheid van de kijkers voor Cherenkov-gamma op basis van een compleet. Dit wordt gedaan met behulp van statistische methoden onderscheid maken tussen de gewenste signaal (Cherenkov straling douches) en achtergrondruis (hadronic douches gestart door cosmic roggen in de lucht hoofdletters).
<p></p>
De gegevens zijn vooraf verwerkt een lang cluster maken met de lange as is gericht op het midden van de camera. De kenmerken van deze knop met weglatingstekens (vaak Hillas parameters genoemd) zijn voorbeelden van de parameters van de afbeelding die kunnen worden gebruikt voor onderscheid.
<p></p>
<b>Gebruik:</b> voorspellen of afbeelding van Welkom signaal of achtergrond ruis vertegenwoordigt.
<p></p>
<b>Opmerkingen:</b> nauwkeurigheid van de eenvoudige indeling is niet relevant zijn voor deze gegevens sinds het classificeren van een gebeurtenis achtergrond zoals signaal hoger is dan het classificeren van een signaalgebeurtenis achtergrond. Voor een vergelijking van verschillende classificaties moet de ROC-grafiek worden gebruikt. De kans op een gebeurtenis achtergrond accepteren als signaal onder een van de volgende drempelwaarden moet: 0,01, 0,02, 0,05, 0,1 of 0,2.
<p></p>
Merk ook op dat het aantal gebeurtenissen van de achtergrond (voor hadronic douches h) onderschat is. De klasse h of ruis vertegenwoordigt in de echte metingen het merendeel van de gebeurtenissen. 
<p></p>
<b>Verwante Research:</b> Bock, R.K. (1995). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Universiteit van Californië, School van gegevens </td>
</tr>

<tr>
  <td>Dataset weer</td>
  <td>
Elk uur weer grond metingen uit NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">samenvoegen van gegevens van 201304 naar 201310</a>).
<p></p>
De weergegevens worden uitgevoerd vanaf een luchthaven weer stations, die betrekking hebben op de periode April oktober 2013 opmerkingen behandeld. Voordat u uploadt naar Azure Machine Learning Studio, is als volgt de gegevensset verwerkt:
<ul>
  <li>Station weer-id's zijn toegewezen aan de bijbehorende luchthaven id 's</li>
  <li>Weer stations die niet zijn gekoppeld aan de 70 drukste luchthavens zijn gefilterd.</li>
  <li>De datumkolom is opgesplitst in afzonderlijke jaar, maand en dag kolommen</li>
  <li>De volgende kolommen zijn geselecteerd: AirportID, jaar, maand, dag, tijd, tijdzone, SkyCondition, zichtbaarheid, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, Windsnelheid, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Wikipedia SP 500 Dataset</td>
  <td>
Gegevens is afgeleid van Wikipedia (Engelstalig) (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) op basis van de artikelen van elk bedrijf S & P 500 opgeslagen als XML-gegevens.
<p></p>
Voordat u uploadt naar Azure Machine Learning Studio, is als volgt de gegevensset verwerkt:
<ul>
  <li>Uitpakken van tekstinhoud voor elke specifieke bedrijf</li>
  <li>Wiki opmaak verwijderen</li>
  <li>Verwijder de niet-alfanumerieke tekens</li>
  <li>Alle tekst converteren naar kleine letters</li>
  <li>Bekende bedrijf categorieën zijn toegevoegd</li>
</ul>
<p></p>
Merk op dat voor sommige een artikel bedrijven kan niet worden gevonden, zodat het aantal records minder dan 500 is.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td>
De gegevensset bevat gegevens van de klant en aanwijzingen over hun reactie op een directe postadres campagne. Elke rij vertegenwoordigt een klant. De gegevensset bevat negen functies over gebruiker demografische gegevens en het verleden gedrag en drie kolommen label (bezoekt, conversie en hoeven te besteden aan).  Ga naar is een binaire kolom waarin wordt aangegeven dat een klant die na de campagne. Conversie geeft aan dat een klant iets gekocht. Hoeven te besteden aan het bedrag dat is uitgegeven.  De gegevensset is beschikbaar gesteld door Kevin Hillstrom voor MineThatData e-Analytics en Data Mining uitdaging.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td>
Functies van de test-voorbeelden in de RCV1 V2 Reuters nieuws gegevensset. De gegevensset heeft 781K nieuwsartikelen samen met de id's (eerste kolom van de gegevensset). Elk artikel is tokenized, stopworded, en voort. De gegevensset is door David beschikbaar gesteld. D. Leistra.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td>
Functies van training voorbeelden in de RCV1 V2 Reuters nieuws gegevensset. De gegevensset heeft 23K nieuwsartikelen samen met de id's (eerste kolom van de gegevensset). Elk artikel is tokenized, stopworded, en voort. De gegevensset is door David beschikbaar gesteld. D. Leistra.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td>
Gegevensset van de KDD kop 1999 kennis detectie en gegevensanalyse van hulpprogramma's voor concurrentie (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).
<p></p>
De gegevensset is gedownload en opgeslagen in Azure Blob storage (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) en omvat zowel trainings- en testdoeleinden gegevenssets. De gegevensset training heeft ongeveer 126K rijen en 43 kolommen, met inbegrip van de labels. Drie kolommen deel uitmaken van de labelgegevens en 40 kolommen, die bestaan uit numerieke en tekenreeks/categorische-functies zijn beschikbaar voor het model te trainen. De testgegevens heeft ongeveer 22,5 K voorbeelden met dezelfde 43 kolommen als in de trainingsgegevens testen.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td>
Onderwerp-toewijzingen voor nieuwsartikelen in de RCV1 V2 Reuters nieuws gegevensset. Een nieuwsbericht kan worden toegewezen aan verschillende onderwerpen. De indeling van elke rij is '&lt;onderwerpnaam&gt; &lt;document-id&gt; 1". De gegevensset bevat 2.6M onderwerp toewijzingen. De gegevensset is door David beschikbaar gesteld. D. Leistra.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td>
Deze gegevens zijn afkomstig van de uitdaging KDD kop 2010 studenten prestaties evaluatie (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">studenten prestatieanalyse</a>). De gegevens die gebruikt is de trainingset Algebra_2008_2009 (Stamper, J., Niculescu-Mizil, A., Ritter, S., drempel, G.J. & Koedinger, K.R. (2010). Algebra I 2008-2009. Uitdaging gegevensset van KDD kop 2010 educatieve Data Mining uitdaging. Zoeken op het <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> of <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.
<p></p>
De gegevensset is gedownload en opgeslagen in Azure Blob storage (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) en logboekbestanden van een student Bijles system bevat. De opgegeven functies behoren probleem-ID en de korte beschrijving, student-ID, tijdstempel en het aantal pogingen de student vóór het oplossen van het probleem op de juiste manier. De oorspronkelijke gegevensset bevat records 8,9 M; Deze gegevensset is gezet dat op de eerste 100K rijen. De gegevensset heeft 23 door tabs gescheiden kolommen van verschillende typen: numerieke categorische, en een tijdstempel.
  </td>
</tr>

</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

---
title: De voorbeeldgegevenssets gebruiken
titleSuffix: Azure Machine Learning Studio
description: Beschrijvingen van de gegevenssets die worden gebruikt in de voorbeeld-modellen die zijn opgenomen in Machine Learning Studio. U kunt deze voorbeeldgegevenssets gebruiken voor de experimenten nodig hebben.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/19/2018
ms.openlocfilehash: 55cf124baf7614813620602212856cc586b721df
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244331"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-studio"></a>De voorbeeldgegevenssets in Azure Machine Learning Studio gebruiken
[top]: #machine-learning-sample-datasets

Wanneer u een nieuwe werkruimte in Azure Machine Learning maken, worden een aantal voorbeeldgegevenssets en experimenten standaard opgenomen. Veel van deze voorbeelden van gegevenssets worden gebruikt door de voorbeeld-modellen in de [Azure AI Gallery](http://gallery.azure.ai/). Andere resources zijn opgenomen als voorbeelden van verschillende typen gegevens meestal gebruikt in machine learning.

Sommige van deze gegevenssets zijn beschikbaar in Azure Blob-opslag. Voor deze gegevenssets moet bevat de volgende tabel een directe koppeling. U kunt deze gegevenssets in uw experimenten gebruiken met behulp van de [importgegevens] [ import-data] module.

De rest van deze voorbeelden van gegevenssets zijn beschikbaar in uw werkruimte onder **opgeslagen gegevenssets**. U kunt dit vinden in het modulepalet links van het experimentcanvas in Machine Learning Studio.
U kunt een van deze gegevenssets gebruiken in uw eigen experiment door deze te slepen naar het experimentcanvas.




<table>

<tr>
  <th>Naam van de gegevensset</th>
  <th>Beschrijving van gegevensset</th>
</tr>

<tr>
  <td>Volwassenen telling inkomsten binaire classificatie-gegevensset</td>
  <td>
Een subset van de database 1994 telling, met behulp van volwassenen werken via de leeftijd van 16 met een aangepaste inkomsten index > 100.
<p></p>
<b>Gebruik:</b> Classificeren van mensen met behulp van demografische gegevens om te voorspellen of een persoon jaaromzet van meer dan 50K per jaar.
<p></p>
<b>Gerelateerde Research:</b> Kohavi, R., Becker, B., (1996). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Luchthaven Codes gegevensset</td>
  <td>
VS luchthaven codes.
<p></p>
Deze gegevensset bevat één rij voor elke luchthaven VS, bieden de luchthaven ID-nummer en de naam, samen met de locatie plaats en provincie.
  </td>
</tr>

<tr>
  <td>Auto's prijs data (Raw)</td>
  <td>
Informatie over auto's door merk en model, met inbegrip van de prijs, functies, zoals het aantal cilinders en MPG, evenals een verzekering risicoscore.
<p></p>
De risicoscore is in eerste instantie gekoppeld automatisch prijs. Vervolgens wordt het werkelijke risico's in een proces dat bekend is dat zowel actuarissen als symboling aangepast. Een waarde van + 3 geeft aan dat de automatische riskante, en een waarde van-3 dat deze waarschijnlijk veilig is.
<p></p>
<b>Gebruik:</b> Voorspel de risicoscore van functies, met behulp van regressie of een multidimensionale indeling. 
<p></p>
<b>Gerelateerde Research:</b> Schlimmer, J.C. (1987). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Fiets verhuur UCI-gegevensset</td>
  <td>
UCI fiets verhuur gegevensset die is gebaseerd op echte gegevens van kapitaal Bikeshare-bedrijf dat een fiets huur-netwerk in Washington D.C. onderhoudt.
<p></p>
De gegevensset heeft een rij voor elk uur van elke dag in 2011 en 2012, voor een totaal van 17,379 rijen. Het bereik van de huurfietsen per uur ligt tussen 1 en 977.

  </td>
</tr>

<tr>
  <td>Bill Gates RGB-afbeelding</td>
  <td>
Openbaar beschikbare afbeeldingsbestand geconverteerd naar CSV-gegevens.
<p></p>
De code voor het omzetten van de installatiekopie is opgegeven in de <strong>kwantisatiefouten K-Means clustering met kleur</strong> pagina met details van het model.
  </td>
</tr>

<tr>
  <td>Bloed donatie gegevens</td>
  <td>
Een subset van gegevens uit de database van de donateurs bloed van de bloed transfusie Service Center van Hsin Chu stad, Taiwan.
<p></p>
Donateurs gegevens omvatten de maanden sinds de laatste donatie), en frequentie, of het totale aantal donaties, de tijd sinds de laatste donatie en hoeveelheid gedoneerd bloed.
<p></p>
<b>Gebruik:</b> Het doel is om te voorspellen via classificatie of de donateurs gedoneerd bloed in maart 2007, waarbij 1 geeft aan een donateurs tijdens de doelperiode en 0 dat een niet-donateurs. 
<p></p>
<b>Gerelateerde Research:</b> Yeh, I.C., (2008). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen
<p></p>
Yeh, ik-Cheng, Yang, King-Jang en Appclassifica, label-PN-roaming, "Knowledge discovery op RFM-model met behulp van Bernoulli reeks,"Expert bedrijfssystemen met behulp van toepassingen, 2008 <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr>
  <td>En kanker gegevens</td>
  <td>
Een van drie kanker productspecifieke gegevenssets geleverd door het Oncology Institute die vaak in de documentatie over machine learning wordt weergegeven. Diagnostische gegevens met de functies van een laboratorium-analyse van ongeveer 300 weefsel voorbeelden combineert.
<p></p>
<b>Gebruik:</b> Classificeren van het type van kanker, op basis van 9 kenmerken, waarvan sommige zijn lineair en sommige categorische zijn. 
<p></p>
<b>Gerelateerde Research:</b> Wohlberg, W.H., Street, W.N., & Mangasarian, O.L. (1995). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>En kanker functies <td>
De gegevensset bevat informatie over verdachte 102K regio's (kandidaten) van röntgenfoto installatiekopieën, door 117 functies beschreven. De functies zijn eigen en hun betekenis niet aan het licht komt door de gegevensset makers (Siemens gezondheidszorg). 
  </td>
</tr>

<tr>
  <td>En kanker Info</td>
  <td>
De gegevensset bevat aanvullende informatie voor elke verdachte regio van röntgenfoto afbeelding. Elk voorbeeld wordt informatie (bijvoorbeeld: label, patiënt-ID,-coördinaten van de patch ten opzichte van de installatiekopie van het hele) over het nummer van de bijbehorende rij in de gegevensset en kanker functies. Elke patiënt heeft een aantal voorbeelden. Voor patiënten die beschikken over een kanker, enkele voorbeelden zijn positieve en negatieve zijn. Alle voorbeelden zijn voor patiënten die niet beschikken over een kanker, negatief zijn. De gegevensset heeft 102K voorbeelden. De gegevensset is gericht, 0,6% van de punten positief zijn, de overige negatief zijn. De gegevensset is beschikbaar door Siemens gezondheidszorg.
  </td>
</tr>

<tr>
  <td>CRM Appetency Labels gedeeld</td>
  <td>
Labels van de KDD Cup 2009 klant relatie voorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr>
  <td>CRM verloop Labels gedeeld</td>
  <td>
Labels van de KDD Cup 2009 klant relatie voorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr>
  <td>CRM-gegevensset gedeeld</td>
  <td>
Deze gegevens zijn afkomstig van de KDD Cup 2009 klant relatie voorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip">orange_small_train.data.zip</a>).
<p></p>
De gegevensset bevat 50K klanten uit de Frans telecombedrijf oranje. Elke klant heeft 230 geanonimiseerde functies, 190 hiervan numerieke zijn en 40 categorische zijn. De functies zijn zeer verspreid.
  </td>
</tr>

<tr>
  <td>CRM Upselling Labels gedeeld</td>
  <td>
Labels van de KDD Cup 2009 klant relatie voorspelling uitdaging (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td>Energie-efficiëntie regressie-gegevens</td>
  <td>
Een verzameling van gesimuleerde energie-profielen, op basis van 12 ander gebouw vormen. De gebouwen van elkaar worden onderscheiden door acht functies. Dit omvat ruit gebied, de ruit gebied distributie en de afdrukstand.
<p></p>
<b>Gebruik:</b> Regressie of classificatie gebruiken voor het voorspellen van de energie-efficiëntie classificatie op basis van een van twee antwoorden in de werkelijke waarden. Is voor de ROC-indeling, ronde de variabele antwoord op het dichtstbijzijnde gehele getal. 
<p></p>
<b>Gerelateerde Research:</b> Xifara, A. & Tsanas, A. (2012). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Flight Data uitgesteld</td>
  <td>
Passagiersvlucht op tijd prestatiegegevens die zijn overgenomen uit de verzameling van TranStats van de Verenigde Staten Ministerie van transport (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">op tijd</a>).
<p></p>
De gegevensset bevat informatie over de periode van April-oktober 2013. Voordat u uploadt naar Azure Machine Learning Studio, is de gegevensset als volgt verwerkt:
<ul>
  <li>De gegevensset is gefilterd zodat alleen de 70 drukste luchthavens in de Verenigde Staten</li>
  <li>Geannuleerde vluchten zijn gelabeld als meer dan 15 minuten vertraagd</li>
  <li>Omgereden vluchten zijn gefilterd.</li>
  <li>De volgende kolommen zijn geselecteerd: Year, Month, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Canceled</li>
</ul>
</td>
</tr>

<tr>
  <td>Flight prestatiegegevens omtrent op tijd (Raw)</td>
  <td>
Records van het vliegtuig flight ontvangsten en afwijkingen in de Verenigde Staten van oktober 2011.
<p></p>
<b>Gebruik:</b> Flight vertragingen te voorspellen. 
<p></p>
<b>Gerelateerde Research:</b> Van VS afdeling van transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time"> http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time </a>.
  </td>
</tr>

<tr>
  <td>Bosbrandgegevens</td>
  <td>
Bevat gegevens van de weersomstandigheden, zoals temperatuur en vochtigheid indexen en windsnelheid. De gegevens is afkomstig uit een gebied van noordoosten van Portugal, gecombineerd met records van forest wordt geactiveerd.
<p></p>
<b>Gebruik:</b> Dit is een taak die moeilijk regressie, waarbij het doel is om te voorspellen van het gebrande gebied van bosbranden. 
<p></p>
<b>Gerelateerde Research:</b> Cortez, P., & Morais, A. (2008). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen
<p></p>
[Cortez and Morais, 2007] P. Cortez and A. Morais. Een Data Mining benadering voor het voorspellen bosbranden Meteorological gegevens gebruiken. In J. Neves, M. f kan zijn. Santos en J. Machado Eds., nieuwe Trends in kunstmatige intelligentie, procedures van de 13 EPIA 2007 - Portugees Conferentie van kunstmatige intelligentie, December Guimarães, Portugal, p. 512-523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Beschikbaar op: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf"> http://www.dsi.uminho.pt/~pcortez/fires.pdf </a>.
  </td>
</tr>

<tr>
  <td>Duits creditcard UCI-gegevensset</td>
  <td>
De gegevensset UCI Statlog (Duits creditcard) (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Duits + tegoed + gegevens</a>), met behulp van het bestand german.data.
<p></p>
De gegevensset worden geclassificeerd personen, dat wordt beschreven door een set kenmerken, zoals lage of hoge kredietrisico's. Elk voorbeeld vertegenwoordigt een persoon. Er zijn 20 functies, numerieke en categorische, en een binaire label (tegoed risico waarde). Hoge tegoed risico vermeldingen label hebben = 2, lage tegoed risico vermeldingen label hebben = 1. De kosten voor een voorbeeld van een laag risico zo hoog onjuiste is 1, terwijl de kosten voor een voorbeeld van een hoog risico zo klein onjuiste 5 is.
  </td>
</tr>

<tr>
  <td>IMDB Filmtitels</td>
  <td>
De gegevensset bevat informatie over films die zijn geclassificeerd in tweets van Twitter: IMDB film-ID, de naam van film, genre en productie jaar. Er zijn 17K films in de gegevensset. De gegevensset is geïntroduceerd in het document 'S. Dooms, T. De Pessemier en L. Martens. MovieTweetings: een film beoordeling van de gegevensset die worden verzameld van Twitter. Workshop op Crowdsourcing en menselijke berekeningen voor Recommender systemen, CrowdRec tijdens RecSys 2013."
  </td>
</tr>

<tr>
  <td>Gegevens van twee iris-klasse</td>
  <td>
Dit is mogelijk de beste bekende database kunnen worden gevonden in de documentatie van de spraakherkenning patroon. De gegevensset is relatief klein, met 50 voorbeelden van petal metingen van drie iris die.
<p></p>
<b>Gebruik:</b> Het type iris van de metingen te voorspellen.  
<p></p>
<b>Gerelateerde Research:</b> Fisher, R.A. (1988). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Tweets van de film</td>
  <td>
De gegevensset is een uitgebreide versie van de film Tweetings-gegevensset. De gegevensset heeft 170K beoordelingen van films, geëxtraheerd uit gestructureerde tweets op Twitter. Elk exemplaar een tweet vertegenwoordigt en is een tuple: gebruikers-ID, IMDB film-ID, classificatie, timestamp, aantal favorieten voor deze tweet en het aantal retweets van deze tweet. De gegevensset is beschikbaar gesteld door A. gezegd, S. Dooms, B. Loni en D. Tikk voor Recommender systemen Challenge 2014.
  </td>
</tr>

<tr>
  <td>MPG-gegevens voor verschillende auto 's</td>
  <td>
Deze gegevensset is een enigszins gewijzigde versie van de gegevensset die is geleverd door de bibliotheek StatLib van Carnegie Mellon University. De gegevensset is gebruikt in het 1983 American statistische koppeling.
<p></p>
De gegevens van het verbruik stimuleren voor verschillende auto's in mijl per gallon bevat. Dit omvat ook informatie zoals het aantal cilinders, engine verplaatsing, paardenkracht, totaal gewicht en versnelling.
<p></p>
<b>Gebruik:</b> Voorspel brandstof economie op basis van drie waarden discrete kenmerken en vijf doorlopende kenmerken. 
<p></p>
<b>Gerelateerde Research:</b> StatLib, Carnegie Mellon University, (1993). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Pima Indianen Diabetes binaire classificatie-gegevensset</td>
  <td>
Een subset van gegevens uit de National Institute of Diabetes en spijsverteringskanaal en nier ziekten database. De gegevensset is zich richten op belangrijke vrouwelijke patiënten van Pima Indiase erfgoed gefilterd. De gegevens bevatten medische gegevens, zoals glucose en of niveaus, evenals levensstijl factoren.
<p></p>
<b>Gebruik:</b> Voorspel of het onderwerp diabetes (binaire classificatie) heeft. 
<p></p>
<b>Gerelateerde Research:</b> Sigillito, V. (1990). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml"</a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen </td>
</tr>

<tr>
  <td>Gegevens van de klant restaurant</td>
  <td>
Een set van metagegevens over klanten, waaronder demografische gegevens en voorkeuren.
<p></p>
<b>Gebruik:</b> Deze gegevensset gebruikt in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een systeem recommender. 
<p></p>
<b>Gerelateerde Research:</b> Bache, K. and Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen.
  </td>
</tr>

<tr>
  <td>Gegevens van de functie restaurant</td>
  <td>
Een set van metagegevens over restaurants en hun functies, zoals food, doen-stijl, en locatie.
<p></p>
<b>Gebruik:</b> Deze gegevensset gebruikt in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een systeem recommender. 
<p></p>
<b>Gerelateerde Research:</b> Bache, K. and Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen.
  </td>
</tr>

<tr>
  <td>Restaurant beoordelingen</td>
  <td>
Bevat de classificaties die zijn opgegeven door gebruikers restaurants op een schaal van 0 tot 2.
<p></p>
<b>Gebruik:</b> Deze gegevensset gebruikt in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een systeem recommender. 
<p></p>
<b>Gerelateerde Research:</b> Bache, K. and Lichman, M. (2013). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen.
  </td>
</tr>

<tr>
  <td>Stalen Annealing ROC-gegevensset</td>
  <td>
Deze gegevensset bevat een reeks records uit staal aanhechten proefversies. Deze bevat de fysieke kenmerken (breedte, dikte, type (spoel, blad, enzovoort) van de resulterende stalen typen.
<p></p>
<b>Gebruik:</b> Een van twee numerieke klassenkenmerken; voorspellen hardheid of sterkte. U kunt ook correlatie tussen kenmerken analyseren.
<p></p>
Stalen cursusopdrachten gaat u als volgt een standaard set gedefinieerd door SAE en andere organisaties. U op zoek bent naar een specifieke "Geavanceerde" (de klassevariabele) en wilt weten over de waarden die nodig zijn. 
<p></p>
<b>Gerelateerde Research:</b> Sterling, D. & Buntine, W. (NA). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens en computerwetenschappen
<p></p>
Een nuttig handleiding voor het staal cursusopdrachten vindt u hier: <a href="https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf">https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td>Telescoop gegevens</td>
  <td>
Record van hoge energie gamma particle pieken samen met achtergrondgeluiden beide worden gesimuleerd met behulp van een financiële Monte Carlo-proces.
<p></p>
Het doel van de simulatie is voor het verbeteren van de nauwkeurigheid van volledig is ontwikkeld op basis van atmospheric Cherenkov gamma kijkers. Dit wordt gedaan met behulp van statistische methoden wilt onderscheiden van de gewenste signaal (Cherenkov straling douches) en achtergrondgeluiden (hadronic douches gestart door cosmic roggen in de lucht hoofdletters).
<p></p>
De gegevens zijn vooraf verwerkte een lang cluster maakt met de lange as is gericht op het midden van de camera. De kenmerken van deze ellips (ook wel Hillas parameters) zijn een van de parameters van de afbeelding die kunnen worden gebruikt voor discriminatie.
<p></p>
<b>Gebruik:</b> Voorspel of afbeelding van Welkom signaal of achtergrond ruis vertegenwoordigt.
<p></p>
<b>Opmerkingen:</b> Eenvoudige classificatie nauwkeurigheid is sinds het classificeren van een gebeurtenis achtergrond signaal is slechter dan een signaalgebeurtenis als achtergrond voor het classificeren niet zinvol is voor deze gegevens. Voor een vergelijking van verschillende classificaties, moet de ROC-grafiek worden gebruikt. De kans op een gebeurtenis van de achtergrond accepteren als signaal minder dan een van de volgende drempelwaarden zijn moet: 0,01, 0,02, 0,05, 0.1 of 0.2.
<p></p>
Houd er ook rekening mee dat het aantal achtergrond-gebeurtenissen (voor hadronic douches h) is onderschat. In real-metingen vertegenwoordigt de klasse uur of ruis het merendeel van de gebeurtenissen. 
<p></p>
<b>Gerelateerde Research:</b> Bock, R.K. (1995). UCI Machine Learning-opslagplaats <a href="http://archive.ics.uci.edu/ml"> http://archive.ics.uci.edu/ml </a>. Irvine, CA: Universiteit van Californië, van Schoolgegevens </td>
</tr>

<tr>
  <td>De gegevensset weer</td>
  <td>
Per uur weer op basis van land opmerkingen van NOAA (<a href="http://az754797.vo.msecnd.net/data/WeatherDataset.csv">samengevoegde gegevens van 201304 naar 201310</a>).
<p></p>
De weergegevens bevat informatie over observaties uit luchthaven weer stations, die betrekking hebben op de periode van April-oktober 2013. Voordat u uploadt naar Azure Machine Learning Studio, is de gegevensset als volgt verwerkt:
<ul>
  <li>Weerstation-id's zijn toegewezen aan de bijbehorende luchthaven id 's</li>
  <li>Weather stations die niet zijn gekoppeld aan de 70 drukste luchthavens zijn gefilterd.</li>
  <li>De datumkolom is splitsen in afzonderlijke kolommen voor jaar, maand en dag</li>
  <li>De volgende kolommen zijn geselecteerd: AirportID, jaar, maand, dag, tijd, tijdzone, SkyCondition, zichtbaarheid, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, windsnelheid, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Wikipedia SP 500 Dataset</td>
  <td>
Gegevens worden afgeleid van Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) op basis van de artikelen van elk S & P 500-bedrijf, opgeslagen als XML-gegevens.
<p></p>
Voordat u uploadt naar Azure Machine Learning Studio, is de gegevensset als volgt verwerkt:
<ul>
  <li>Uitpakken van tekstinhoud voor elk bedrijf dat specifieke</li>
  <li>Wiki opmaak verwijderen</li>
  <li>Verwijder de niet-alfanumerieke tekens</li>
  <li>Alle tekst geconverteerd naar kleine letters</li>
  <li>Bekende bedrijf categorieën zijn toegevoegd</li>
</ul>
<p></p>
Houd er rekening mee dat voor sommige een artikel bedrijven kan niet worden gevonden, zodat het aantal records minder dan 500 is.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td>
De gegevensset bevat de gegevens van de klant en aanwijzingen over hun reactie op een directe adressenlijst campagne. Elke rij vertegenwoordigt een klant. De gegevensset bevat negen functies over gebruiker demografische gegevens en eerdere gedrag en drie kolommen te labelen (bezoekt, conversie en uitgaven).  Ga naar is een binaire kolom waarin wordt aangegeven dat een klant heeft bezocht nadat de campagne. Conversie geeft aan dat een klant iets gekocht. Besteden aan het bedrag dat is uitgegeven.  De gegevensset is beschikbaar door Kevin Hillstrom voor MineThatData e-Analytics en Data Mining uitdaging.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td>
Functies van de voorbeelden test in de gegevensset Reuters RCV1-V2-nieuws. De gegevensset heeft nieuwsartikelen 781K samen met de id's (eerste kolom van de gegevensset). Elk artikel tokenized, stopworded, en voort. De gegevensset is beschikbaar door David. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td>
Functies van training voorbeelden in de gegevensset Reuters RCV1-V2-nieuws. De gegevensset heeft nieuwsartikelen 23K samen met de id's (eerste kolom van de gegevensset). Elk artikel tokenized, stopworded, en voort. De gegevensset is beschikbaar door David. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td>
Gegevensset van de KDD Cup 1999 Knowledge Discovery and datamining extra competitie (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).
<p></p>
De gegevensset is gedownload en opgeslagen in Azure Blob-opslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) en omvat zowel trainings- en testsets. De gegevensset training heeft ongeveer 126K rijen en 43 kolommen, met inbegrip van de labels. Drie kolommen maken deel uit van de labelgegevens en 40 kolommen, die bestaat uit een tekenreeks/categorische en numerieke functies, zijn beschikbaar voor het model te trainen. De testgegevens heeft ongeveer 22,5 K voorbeelden met dezelfde 43 kolommen zoals in de trainingsgegevens testen.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td>
Onderwerp-toewijzingen voor nieuwsartikelen in de gegevensset Reuters RCV1-V2-nieuws. Een nieuwsbericht kan worden toegewezen aan verschillende onderwerpen. De indeling van elke rij is '&lt;onderwerpnaam&gt; &lt;document-id&gt; 1". De gegevensset bevat 2,6 miljoen onderwerp toewijzingen. De gegevensset is beschikbaar door David. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td>
Deze gegevens zijn afkomstig van de KDD Cup 2010 Student prestaties evaluatie uitdaging (<a href="http://www.kdd.org/kdd-cup/view/kdd-cup-2010-student-performance-evaluation">student prestatie-evaluatie</a>). De gegevens die worden gebruikt, is de trainingsset Algebra_2008_2009 (Stamper, J., Niculescu-Mizil, A., Ritter, S., Gordon, G.J. & Koedinger, K.R. (2010). Wiskundige ik 2008-2009. Uitdaging gegevensset van de KDD Cup 2010 educatieve Data Mining uitdaging. Zoeken op <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a>.
<p></p>
De gegevensset is gedownload en opgeslagen in Azure Blob-opslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) en logboekbestanden van een student tutoring system bevat. De opgegeven functies omvatten probleem-ID en een korte beschrijving, student-ID, tijdstempel en het aantal pogingen de studenten die vóór het oplossen van het probleem in de juiste manier. De oorspronkelijke gegevensset heeft 8,9 miljoen records. Deze gegevensset is down-voorbeelden worden gemaakt op de eerste 100K-rijen. De gegevensset heeft 23 door tabs gescheiden kolommen van verschillende typen: Numeriek, categorische, en de tijdstempel.
  </td>
</tr>

</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

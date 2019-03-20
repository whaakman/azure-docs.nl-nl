---
title: Mogelijkheden in Azure Sentinel Preview jacht | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de mogelijkheden van Azure Sentinel jacht gebruiken.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: adedc8bc1f574ae089f2a11033fab4f390c57a9a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087020"
---
# <a name="hunt-for-threats-with-in-azure-sentinel-preview"></a>Hunt voor bedreigingen met Azure Sentinel Preview-versie

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Als u een onderzoeker die wil proactief over beveiligingsrisico's op zoek bent, jacht Azure Sentinel krachtige zoek- en query's om te zoeken op beveiligingsrisico's in uw organisatie gegevensbronnen. Maar uw systemen en beveiligingsapparaten bergen van gegevens die moeilijk te parseren en te filteren in zinvolle gebeurtenissen worden gegenereerd. Voor de beveiliging analisten zoekt proactief nieuwe afwijkingen die niet zijn gedetecteerd door uw beveiligings-apps, Azure Sentinel' ingebouwde jacht query's helpen u bij de juiste vragen te vinden van problemen in de gegevens die u al in uw netwerk hebt. 

Bijvoorbeeld, een ingebouwde query biedt gegevens over de meest ongebruikelijk processen die worden uitgevoerd op uw infrastructuur - u helemaal niet wilt dat een waarschuwing over telkens wanneer ze worden uitgevoerd, kan deze worden volledig onschuldig uitziet, maar kunt u eens in de query in bepaalde gevallen om te controleren of th ere's iets ongebruikelijke. 



Met Azure Sentinel jacht, kunt u profiteren van de volgende mogelijkheden:

- Ingebouwde query's: Als u aan de slag te gaan, biedt een introductiepagina vooraf geladen queryvoorbeelden die zijn ontworpen om u te helpen aan de slag en raken met de tabellen en de querytaal. Deze ingebouwde jacht query's zijn ontwikkeld door Microsoft-beveiligingsonderzoekers op continue basis toe te voegen nieuwe query's, en verder aanpassen bestaande query's bieden u een beginpunt voor het zoeken naar nieuwe detecties en nagaan waar u wilt zoeken naar beginnen de begin van nieuwe aanvallen. 

- Krachtige querytaal met IntelliSense: Gebaseerd op een querytaal waarmee u beschikt over de flexibiliteit die u nodig hebt om te zoeken naar een hoger niveau.

- Maak uw eigen bladwijzers: Tijdens het zoeken kan u overeenkomsten of bevindingen, dashboards of activiteiten die ongebruikelijk of verdachte gehoord. Gebruik de bladwijzer-functionaliteit om deze items markeren, zodat u kunt later terugkeren naar deze in de toekomst. Bladwijzers kunnen u items opslaan voor later, om te worden gebruikt voor het maken van een aanvraag voor onderzoek. Zie voor meer informatie over bladwijzers gebruiken [bladwijzers in jacht].

- Notitieblokken gebruiken voor het automatiseren van onderzoek: Notitieblokken zijn stapsgewijze playbooks die u helpt bij de stappen van een onderzoek naar en zoeken naar een baan kunt bouwen.  Notitieblokken bevatten alle jacht stappen in een herbruikbare playbook die kunnen worden gedeeld met anderen in uw organisatie. 
- De opgeslagen gegevens op te vragen: De gegevens zijn toegankelijk in de tabellen voor u om op te vragen. U kunt bijvoorbeeld een query maken van het proces, DNS-gebeurtenissen en vele andere typen gebeurtenissen.

- Koppelingen naar de community: Benut de kracht van de community groter te vinden van extra query's en gegevensbronnen.
 
## <a name="get-started-hunting"></a>Aan de slag jacht

1. Klik in de portal voor Azure Sentinel **jacht**.
  ![Azure Sentinel jacht wordt gestart](media/tutorial-hunting/hunting-start.png)

2. Wanneer u opent de **jacht** pagina, de jacht query's worden weergegeven in een enkele tabel. De tabel geeft een lijst van alle query's die zijn geschreven door de Microsoft-team van beveiligingsanalisten, evenals eventuele extra query die u hebt gemaakt of gewijzigd. Elke query bevat een beschrijving van wat het voor hunts en wat voor soort gegevens op wordt uitgevoerd. Deze sjablonen zijn gegroepeerd op hun verschillende tactieken - de pictogrammen aan de rechterkant categoriseren van het type bedreigingen, zoals initiële toegang, persistentie en exfiltratie. U kunt deze jacht query sjablonen met behulp van een van de velden filteren. U kunt elke query aan uw favorieten opslaan. Door op te slaan een query aan uw Favorieten, door de query automatisch elke keer wordt uitgevoerd de **jacht** pagina wordt geopend. U kunt uw eigen jacht query of kloon maken en aanpassen van een bestaande sjabloon voor de query van zoeken. 
 
2. Klik op **query uitvoeren** in de jacht Querydetails pagina voor het uitvoeren van query's zonder de jacht pagina af te sluiten.  Het aantal overeenkomsten wordt in de tabel weergegeven. Bekijk de lijst met jacht query's en hun overeenkomsten. Bekijk welke fase in de kill chain de overeenkomst gekoppeld is.

3. Een kort overzicht van de onderliggende query uitvoeren in het detailvenster van de query of klik op **queryresultaat weergeven** te openen van de query in Log Analytics. Bekijk de resultaten voor de query aan de onderkant.

4.  Klik op de rij en selecteer **toevoegen bladwijzer** om toe te voegen van de rijen moeten worden onderzocht - u kunt dit doen voor alles wat er verdacht uitziet. 

5. Ga vervolgens terug naar het hoofdvenster **jacht** pagina en klik op de **bladwijzers** tabblad om te bekijken van alle verdachte activiteiten. 

6. Selecteer een bladwijzer en klik vervolgens op **onderzoeken** openen van de ervaring voor het onderzoeken. U kunt de bladwijzers filteren. Bijvoorbeeld, als u een campagne onderzoekt, kunt u een label voor de campagne maken en filter vervolgens de bladwijzers die op basis van de campagne.

1. Nadat u welke query jacht biedt waardevolle inzichten in het mogelijke aanvallen ontdekt, kunt u ook aangepaste detectie regels op basis van uw query en maak die inzichten duidelijk als waarschuwingen aan uw security incident beantwoorders maken.

 

## <a name="query-language"></a>Querytaal 

Zoeken in Azure Sentinel is gebaseerd op Azure Log Analytics-querytaal. Zie voor meer informatie over de querytaal en de ondersteunde operators [Query Language Reference](https://docs.loganalytics.io/docs/Language-Reference/).

## <a name="public-hunting-query-github-repository"></a>Openbare jacht query GitHub-opslagplaats

Bekijk de [jacht query opslagplaats](https://github.com/Azure/Orion). Bijdragen en voorbeelden van query's gedeeld door onze klanten gebruiken.

 

## <a name="sample-query"></a>Voorbeeldquery

Een typische query begint met een tabelnaam wordt opgegeven gevolgd door een reeks van operators, gescheiden door \|.

In het voorbeeld hierboven, beginnen met de tabel een naam SecurityEvent en toevoegen van elementen doorgesluisd naar behoefte.

1. Definieer een tijdfilter om te controleren van alleen de records van de afgelopen zeven dagen.

2. Een filter toevoegen in de query om alleen gebeurtenis-ID 4688 weer te geven.

3. Een filter in de query op de opdrachtregel bevat alleen de exemplaren van cscript.exe toevoegen.

4. Alleen de kolommen die u geïnteresseerd bent in het verkennen en de resultaten tot en met 1000, beperken en klikt u op project **query uitvoeren**.
5. Klik op de groene driehoek en voer de query uit. U kunt de query testen en uitvoeren om te zoeken naar afwijkend gedrag.

## <a name="useful-operators"></a>Nuttige operatoren

De querytaal is een krachtig en bevat veel beschikbaar operators, enkele nuttige operators worden hier weergegeven:

**waar** -Filter een tabel naar de subset rijen die voldoet aan een predicaat.

**samenvatten** -produceren van een tabel die de inhoud van de invoertabel samenvoegt.

**JOIN** -samenvoegen van de rijen van twee tabellen om te vormen een nieuwe tabel door de overeenkomende waarden van de opgegeven kolommen uit elke tabel.

**aantal** -retourneert het aantal records in de invoerrecordset.

**Top** -retourneren de eerste N-records die zijn gesorteerd op basis van de opgegeven kolommen.

**limiet** -retourneren tot het opgegeven aantal rijen.

**Project** : Selecteer de kolommen die moeten worden opgenomen, wijzigen of verwijderen en nieuwe berekende kolommen worden ingevoegd.

**uitbreiden** : berekende kolommen maken en deze toevoegen aan de resultatenset.

**makeset** -retourneert een dynamische (JSON)-matrix van de set met verschillende waarden markering wordt in de groep

**Zoek** -rijen die overeenkomen met een predicaat in een set met tabellen zoeken.

## <a name="save-a-query"></a>Een query opslaan

U kunt maken of wijzigen van een query en sla deze op als uw eigen query of delen met gebruikers die zich in dezelfde tenant.

   ![Query opslaan](./media/tutorial-hunting/save-query.png)

Maak een nieuwe jacht-query:

1. Klik op **nieuwe query** en selecteer **opslaan**.
2. Vul in de lege velden in en selecteer **opslaan**.

   ![Nieuwe query](./media/tutorial-hunting/new-query.png)

Klonen en wijzig een bestaande jacht-query:

1. Selecteer de query van zoeken in de tabel die u wilt wijzigen.
2. Selecteer het beletselteken (...) in de regel van de query die u wilt wijzigen, en selecteer **query klonen**.

   ![query klonen](./media/tutorial-hunting/clone-query.png)
 

3. Wijzigen van de query en selecteert u **maken**.

   ![aangepaste query](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een onderzoek jacht uitvoert met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:


- [Notitieblokken gebruiken voor het uitvoeren van geautomatiseerde jacht campagnes](notebooks.md)
- [Bladwijzers gebruiken om op te slaan interessante gegevens terwijl jacht](bookmarks.md)
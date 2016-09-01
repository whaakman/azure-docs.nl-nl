<properties
    pageTitle="Gegevens importeren in Azure Search met de indexeerfuncties in de Azure-portal | Microsoft Azure | Gehoste service voor zoeken in de cloud"
    description="Het gebruik van indexeerfuncties in de Azure-portal."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="06/08/2016"
    ms.author="heidist"/>

# Gegevens importeren in Azure Search via de portal

De Azure-portal bevat de opdracht **Gegevens importeren** op het Azure-dashboard Zoeken om gegevens in een index te laden. De opdracht is afhankelijk van de ingebouwde indexeerfuncties die een bestaande gegevensbron verkennen en documenten maakt en uploadt op basis van een rijenset die uit de gegevensbron is opgehaald.

Gegevens importeren in de wizard bestaat uit 3 delen:

- een gegevensbronverbinding
- een doelindex waarin gegevens worden geüpload (in de meeste gevallen kan deze door de wizard worden gegenereerd)
- een schema dat nu of regelmatig wordt uitgevoerd

Als u een indexeerfunctie of de opdracht **Gegevens importeren** wilt gebruiken, moet uw primaire gegevensbron een van de volgende ondersteunde gegevensbronnen zijn: Azure SQL Database, relationele SQL Server-database op een Azure VM of Azure DocumentDB.

U kunt slechts importeren uit één tabel, weergave of gelijkwaardige gegevensstructuur. Mogelijk moet u deze gegevensstructuur eerst in uw toepassingsgegevensbron maken om de juiste metagegevens en gegevensinvoer in uw zoekindex te verkrijgen.

U kunt deze werkstroom met voorbeeldgegevens uitproberen. Ga naar [Aan de slag met Azure Search in de Azure-portal](search-get-started-portal.md) om te beginnen.

##Gegevensimport configureren

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Open het servicedashboard van uw Azure Search-service. U kunt het dashboard op verschillende manieren vinden.
    - Klik in de snelbalk op **Start**. De startpagina heeft tegels voor alle services in uw abonnement. Klik op de tegel om het servicedashboard te openen.
    - Klik in de snelbalk op **Browse All (Door alle bladeren) ** > **Filteren op** > **Services zoeken** om uw zoekservice in de lijst te vinden.

3. In het servicedashboard ziet u een opdrachtbalk bovenin, inclusief de opdracht **Gegevens importeren**. Klik op **Gegevens importeren** om de blade Gegevens importeren te openen.

4. Klik op **Verbinden met uw gegevens** om een gegevensbrondefinitie op te geven die door een indexeerfunctie wordt gebruikt. Een aantal opties:
    -   Een bestaande gegevensbron verwijst naar een gegevensbrondefinitie die u eerder hebt gemaakt voor een indexeerfunctie. Als u al indexeerfuncties hebt gedefinieerd in uw zoekservice, kunt u een gegevensbrondefinitie van een andere invoer wijzigen.
    -   Azure SQL wordt gebruikt om een gegevensbronverbinding op te geven naar een SQL-database op Azure of een SQL Server-database op een Azure VM.
    -   DocumentDB wordt gebruikt om een gegevensbronverbinding op te geven voor dit gegevensbrontype.

   Voor Azure SQL en DocumentDB moet de database in uw abonnement staan. Als u een verbindingsreeks weet, kunt u deze plakken, maar u kunt ook een gegevensbron kiezen die eerder is gemaakt door iemand met schrijfbevoegdheden voor uw abonnement.

5. Klik op **Doelindex aanpassen** om de standaardindex te voltooien.
    - De **sleutel** is vereist. Het veld dat u voor de sleutel selecteert, moet een tekenreeksveld met unieke waarden zijn.
    - De veldnaam en het veldtype worden doorgaans automatisch ingevuld. U kunt het gegevenstype wijzigen.
    - Selecteer de kenmerken voor elk veld:
        - Met Ophalen mogelijk wordt het veld in de zoekresultaten weergegeven.
        - Met Filterbaar kunt u in filterexpressies naar het veld verwijzen.
        - Met Sorteerbaar kan het veld in een sortering worden gebruikt.
        - Met Facetable (geschikt voor facetten) kunt u het veld gebruiken voor meervoudige navigatie.
        - Met Doorzoekbaar kunt u in het veld zoeken in volledige tekst.
    - Klik op het tabblad **Analyse** als u een taalanalyse op veldniveau wilt opgeven. Zie [Een index voor documenten in meerdere talen maken](search-language-support.md) voor meer informatie.
    - Klik op **Suggestie** als u Automatisch aanvullen of automatisch aangevulde querysuggesties wilt inschakelen.

6. Klik op **Uw gegevens importeren** om de importbewerking uit te voeren met de optie Nu uitvoeren of om een terugkerend schema in te stellen.

Tijdens het uitvoeren van de gegevensimportbewerking is op de achtergrond een indexeerfunctie gemaakt. U kunt onderdelen van de indexeerfunctie direct bewerken.

##Een bestaande indexeerfunctie bewerken

Dubbelklik in het servicedashboard op de tegel Indexeerfunctie om een lijst met alle indexeerfuncties voor uw abonnement weer te geven. Dubbelklik op een van de indexeerfuncties om deze uit te voeren, te bewerken of te verwijderen. U kunt de index vervangen door een andere bestaande index, de gegevensbron wijzigen en opties instellen voor de drempelwaarden van fouten tijdens het indexeren.

##Een bestaande index bewerken

In Azure Search moet de index opnieuw worden gemaakt bij structurele updates voor de index. Het opnieuw maken van de index bestaat uit het verwijderen van de index, het opnieuw samenstellen van de index en het opnieuw laden van de gegevens. Structurele updates zijn onder andere het wijzigen van een gegevenstype en het wijzigen van de naam of verwijderen van een veld.

Bewerkingen waarvoor de index niet opnieuw hoeft te worden gemaakt, zijn onder andere het toevoegen van een nieuw veld, wijzigen van scoringsprofielen, wijziging van de suggestiefunctie en wijzigen van de taalanalysefunctie. Zie [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Index bijwerken) voor meer informatie.



<!--HONumber=ago16_HO4-->



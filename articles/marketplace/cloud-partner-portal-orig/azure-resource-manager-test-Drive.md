---
title: Azure Resource Manager-Test Drive | Microsoft Docs
description: Bouw een Markeplace proefrit met Azure Resource MAnager
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick .Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 19e3141bf7d28c7f781be21aedf14222e35bfd6c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809963"
---
<a name="azure-resource-manager-test-drive"></a>Azure Resource Manager-Test Drive
=================================

Dit artikel is voor uitgevers die hebben hun aanbieding op Azure Marketplace, of die op AppSource, maar wilt maken van de Test Drive met alleen Azure-resources.

Een Azure Resource Manager (Azure Resource Manager)-sjabloon is een gecodeerde container van Azure-resources die u naar beste voor uw oplossing ontwerpt. Als u niet bekend bent met welke Resource Manager-sjabloon is, Lees [inzicht krijgen in ARM-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) en [ARM-sjablonen maken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) om ervoor te zorgen dat u weet hoe u bouwen en testen van uw eigen sjablonen.

Wat Test Drive doet, is dat deze de opgegeven Resource Manager-sjabloon wordt en maakt een implementatie van alle bronnen die van die Resource Manager-sjabloon in een resourcegroep nodig.

Als u kiest voor het bouwen van een Azure Resource Manager Test Drive, worden de vereisten voor u zijn:

- Bouwen, testen en upload uw Test Drive Resource Manager-sjabloon.
- Configureer alle vereiste metagegevens en -instellingen om in te schakelen uw Test Drive.
- Uw aanbod opnieuw publiceren met Test Drive is ingeschakeld.

<a name="how-to-build-an-azure-resource-manager-test-drive"></a>Over het bouwen van een Azure Resource Manager Test Drive
------------------------------

Het belangrijkste gedeelte over het bouwen van een Azure Resource Manager Test Drive is te definiëren welke scenario('s) u wilt dat uw klanten ondervinden. Weet u die een firewallproduct en u wilt demonstreren hoe goed het verwerken van script-injectieaanvallen? Weet u die een opslagproduct en u wilt demonstreren hoe snel en eenvoudig uw oplossing kunt u bestanden comprimeren?

Controleer borgstelling besteden aan een voldoende hoeveelheid tijd evalueren wat de beste manieren zijn om te pronken met uw product zijn. Specifiek rond de vereiste resources moet u, als het ontwerpen van de Resource Manager-sjabloon voldoende eenvoudiger maakt.

Om door te gaan met ons voorbeeld firewall, is de architectuur mogelijk moet u een openbare IP-URL voor uw service en een andere openbare IP-URL voor de website die uw firewall beveiligt. Elke IP is geïmplementeerd op een virtuele Machine en verbonden, samen met een netwerkbeveiligingsgroep + netwerkinterface.

Nadat u het gewenste pakket van resources hebt ontworpen, wordt nu geleverd schrijven en het bouwen van de Test Drive Resource Manager-sjabloon.

<a name="writing-test-drive-resource-manager-templates"></a>Test Drive Resource Manager-sjablonen schrijven
--------------------------------

Implementaties van Test Drive wordt uitgevoerd in een modus voor volledig geautomatiseerd en vanwege die, Test Drive sjablonen gelden enkele beperkingen die hieronder worden beschreven.

### <a name="parameters"></a>Parameters

De meeste sjablonen hebben een aantal parameters. Parameters definiëren namen van voorbeeldresources, resources grootten (bijvoorbeeld de typen van storage-accounts of grootten van virtuele machines), gebruikersnamen en wachtwoorden, DNS-namen enzovoort. Wanneer u oplossingen met behulp van Azure portal implementeert, kunt u handmatig alle deze parameters invullen, beschikbare DNS-namen of namen van opslagaccounts kiezen en de enzovoort.

![Lijst met parameters in een Azure Resource Manager](./media/azure-resource-manager-test-drive/param1.png)

Test Drive werkt echter in een volledig automatische modus, zonder menselijke tussenkomst, zodat deze biedt alleen ondersteuning voor een beperkte set van de parameter-categorieën. Als een parameter in de Test Drive Resource Manager-sjabloon niet kunnen worden onderverdeeld in een van de ondersteunde categorieën, moet u **deze parameter vervangen door een variabele of een constante waarde.**

U kunt een geldige naam voor de parameters, Test Drive herkent categorie parameter met behulp van de metagegevens van het type waarde. U **moet opgeven voor elke sjabloonparameter metagegevenstype**, de sjabloon wordt anders niet worden gevalideerd:

    "parameters": {
      ...
      "username": {
        "type": "string",
        "metadata": {
          "type": "username"
        }
      },
      ...
    }

Het is ook belangrijk om te weten dat **alle parameters zijn optioneel**, dus als u\'t wilt gebruiken, u\'t moet.

### <a name="accepted-parameter-metadata-types"></a>Metagegevens van geaccepteerde parametertypen

| Metagegevenstype   | Parametertype  | Beschrijving     | Voorbeeldwaarde    |
|---|---|---|---|---|
| **BaseUri**     | tekenreeks          | Basis-URI van het implementatiepakket| [https://\<\..\>.blob.core.windows.net/\<\..\>](#) |
| **gebruikersnaam**    | tekenreeks          | Nieuwe willekeurige gebruikersnaam.| admin68876      |
| **Wachtwoord**    | beveiligde tekenreeks    | Nieuwe willekeurig wachtwoord | LP! ACS\^2kh     |
| **Sessie-id**   | tekenreeks          | Unieke Test Drive-sessie-ID (GUID)    | b8c8693e-5673-449c-bAdd-257a405a6dee |

#### <a name="username"></a>gebruikersnaam

Test Drive wordt geïnitialiseerd voor deze parameter met een **Base Uri** van het implementatiepakket, zodat u deze parameter gebruiken kunt om samen te stellen van de Uri van elk bestand dat is opgenomen in het pakket.

    "parameters": {
      ...
      "baseuri": {
        "type": "string",
        "metadata": {
          "type": "baseuri",
          "description": "Base Uri of the deployment package."
        }
      },
      ...
    }

In de sjabloon, kunt u deze parameter te maken van een Uri van elk bestand in uw Test Drive-implementatiepakket. In het volgende voorbeeld toont het maken van een Uri van de gekoppelde sjabloon:

    "templateLink": {
      "uri": "[concat(parameters('baseuri'),'templates/solution.json')]",
      "contentVersion": "1.0.0.0"
    }

#### <a name="username"></a>gebruikersnaam

Test Drive wordt geïnitialiseerd voor deze parameter met een nieuwe willekeurige gebruikersnaam:

    "parameters": {
      ...
      "username": {
        "type": "string",
        "metadata": {
          "type": "username",
          "description": "Solution admin name."
        }
      },
      ...
    }

Voorbeeldwaarde:

    admin68876

U kunt willekeurige of constant gebruikersnamen gebruiken voor uw oplossing.

#### <a name="password"></a>wachtwoord

Test Drive wordt geïnitialiseerd voor deze parameter met een nieuwe willekeurig wachtwoord:

    "parameters": {
      ...
      "password": {
        "type": "securestring",
        "metadata": {
          "type": "password",
          "description": "Solution admin password."
        }
      },
      ...
    }

Voorbeeldwaarde:

    Lp!ACS^2kh

U kunt willekeurige of constante wachtwoorden gebruiken voor uw oplossing.

#### <a name="session-id"></a>Sessie-ID

Deze parameter Test Drive initialiseren met een unieke GUID die Test Drive-sessie-ID vertegenwoordigt:

    "parameters": {
      ...
      "sessionid": {
        "type": "string",
        "metadata": {
          "type": "sessionid",
          "description": "Unique Test Drive session id."
        }
      },
      ...
    }

Voorbeeldwaarde:

    b8c8693e-5673-449c-badd-257a405a6dee

Als dit nodig is, kunt u deze parameter voor het aanduiden van de Test Drive-sessie.

### <a name="unique-names"></a>Unieke namen

Sommige Azure-resources, zoals storage-accounts of DNS-namen, globaal unieke namen vereist.

Dit betekent dat telkens wanneer de Test Drive wordt het Resource Manager-sjabloon geïmplementeerd, worden er een **nieuwe resourcegroep met een unieke naam** voor alle bijbehorende\' resources **.** Daarom is vereist om te gebruiken de [uniquestring](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions#uniquestring) functie samengevoegd met de namen van variabelen voor de resourcegroep-id's voor het genereren van willekeurige unieke waarden:

      "variables": {
      ...
      "domainNameLabel": "[concat('contosovm',uniquestring(resourceGroup().id))]",
      "storageAccountName": "[concat('contosodisk',uniquestring(resourceGroup().id))]",
      ...
    }

Zorg ervoor dat u de parameter/variabele tekenreeksen (\'contosovm\') met een unieke tekenreeks van de uitvoer (\'resourceGroup () .id\'), omdat dit de uniekheid en betrouwbaarheid van elke variabele garandeert.

Bijvoorbeeld, de meeste resourcenamen mag niet beginnen met een cijfer, maar unieke tekenreeksfunctie kan retourneren een tekenreeks, die met een cijfer begint. Dus als u een unieke tekenreeks van onbewerkte uitvoer gebruikt, uw implementaties mislukken. 

U vindt meer informatie over naamgevingsregels voor resource- en -beperkingen in [in dit artikel] (https://docs.microsoft.com/azure/guidance/guidance-naming-conventions).

### <a name="deployment-location"></a>Implementatielocatie

U kunt u met Test Drive beschikbaar maken in verschillende Azure-regio's. Het idee is dat een gebruiker naar de dichtstbijzijnde regio, om te voorzien van de gebruikerservaring beast kiezen.

Test Drive maakt een exemplaar van het testlab, wordt altijd wordt een resourcegroep in de regio kiezen door een gebruiker, en vervolgens de sjabloon voor de implementatie wordt uitgevoerd in de context van deze groep. De sjabloon moet dus de implementatielocatie kiezen uit resourcegroep:

    "variables": {
      ...
      "location": "[resourceGroup().location]",
      ...
    }

En gebruik vervolgens deze locatie voor elke resource voor een specifiek exemplaar van de testomgeving:

    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[variables('location')]",
        ...
      },
      {
        "type": "Microsoft.Network/publicIPAddresses",
        "location": "[variables('location')]",
        ...
      },
      {
        "type": "Microsoft.Network/virtualNetworks",
        "location": "[variables('location')]",
        ...
      },
      {
        "type": "Microsoft.Network/networkInterfaces",
        "location": "[variables('location')]",
        ...
      },
      {
        "type": "Microsoft.Compute/virtualMachines",
        "location": "[variables('location')]",
        ...
      }
    ]

U moet ervoor zorgen dat uw abonnement is toegestaan voor het implementeren van alle resources die u wilt implementeren in elk van de regio's die u selecteert. Ook moet u ervoor zorgen dat uw installatiekopieën voor virtuele machines beschikbaar in alle regio's die u wilt inschakelen zijn, de sjabloon voor de implementatie werkt anders niet voor in sommige regio's.

### <a name="outputs"></a>Uitvoer

Normaal gesproken met Resource Manager-sjablonen, kunt u implementeren zonder deze produceert geen uitvoer. Dit is omdat u weet dat alle waarden die u kunt sjabloonparameters vullen en kunt u de eigenschappen van elke resource altijd handmatig controleren.

Voor de Test Drive Resource Manager-sjablonen, het\'s belangrijk om terug te keren naar Test Drive-alle informatie die is vereist voor het ophalen van een toegang tot het lab (Website URI's, virtuele Machine hostnamen gebruikersnamen en wachtwoorden). Zorg ervoor dat alle uitvoernamen worden gelezen omdat deze variabelen worden gepresenteerd aan de klant.

Er zijn geen beperkingen met betrekking tot de uitvoer van de sjabloon. Vergeet Test Drive zet alle waarden in de uitvoer **tekenreeksen**, dus als u een object naar de uitvoer verzendt, een gebruiker JSON ziet tekenreeks.

Voorbeeld:

    "outputs": {
      "Host Name": {
        "type": "string",
        "value": "[reference(variables('pubIpId')).dnsSettings.fqdn]"
      },
      "User Name": {
        "type": "string",
        "value": "[parameters('adminName')]"
      },
      "Password": {
        "type": "string",
        "value": "[parameters('adminPassword')]"
      }
    }

### <a name="subscription-limits"></a>Limieten voor een abonnement

Nog iets wat u in aanmerking moet nemen is-abonnement en Servicelimieten. Bijvoorbeeld, als u implementeren van maximaal tien virtuele machines van de 4-core wilt, moet u om te controleren of het abonnement dat u voor uw testomgeving kunt u gebruikmaken van 40 kernen.

U vindt meer informatie over Azure-abonnement en Servicelimieten in [in dit artikel](https://docs.microsoft.com/azure/azure-subscription-service-limits). Controleer of uw abonnement kan worden verwerkt als meerdere Test Drives kunnen worden uitgevoerd op hetzelfde moment, de \# kernen vermenigvuldigd met het totale aantal gelijktijdige Test Drives die kunnen worden uitgevoerd.

### <a name="what-to-upload"></a>Wat u wilt uploaden

Test Drive Resource Manager-sjabloon is geüpload als een zip-bestand, waaronder verschillende implementatie-artefacten, maar moet één bestand met de naam **main-template.json**. Dit bestand is de sjabloon van Azure Resource Manager-implementatie en Test Drive gebruikt voor het instantiëren van een Lab.

Als u aanvullende bronnen buiten dit bestand hebt, u ernaar kunt verwijzen als een externe resource in de sjabloon of kunt u de resource in het zip-bestand opnemen.

Tijdens de publicatie-certificering, Test Drive wordt uitgepakt van het implementatiepakket en de inhoud in een interne Test Drive blob-container wordt geplaatst. De containerstructuur is inclusief de structuur van uw implementatiepakket:

| Package.zip                       | Test Drive blob-container         |
|---|---|
main-template.json                | [https://\<\.... \>.blob.core.windows.net/\<\.... \>/main-template.json](#)  |
 Templates/Solution.JSON           | [https://\<\.... \>.blob.core.windows.net/\<\.... \>/templates/solution.json](#) |
| scripts/warmup.ps1                | [https://\<\.... \>.blob.core.windows.net/\<\.... \>/scripts/warmup.ps1](#)  |


We noemen een Uri zijn die van deze basis-Uri van de blob-container. Elke revisie van uw praktijksessie heeft een eigen blob-container, en daarom elke revisie van uw praktijksessie heeft een eigen basis-Uri. Test Drive kunt u een basis-Uri van uw uitgepakte implementatiepakket doorgeven in de sjabloon via Sjabloonparameters.

<a name="transforming-template-examples-for-test-drive"></a>Sjabloonvoorbeelden voor de Test Drive transformeren
---------------------------------------------

Het proces van het omzetten van een architectuur van resources in een Test Drive Resource Manager-sjabloon kan worden uitdaging zijn. Om dit proces eenvoudiger we\'ve die voorbeelden van het beste [transformeren huidige implementatiesjablonen hier](./transforming-examples-for-test-drive.md).

<a name="how-to-publish-a-test-drive"></a>Het publiceren van een Test Drive
---------------------------

Nu dat u uw Test Drive gebouwd hebt, wordt dit gedeelte doorloopt samen met elk van de velden die vereist zijn voor u is uw Test Drive publiceren.

![Inschakelen van Test Drive in de gebruikersinterface](./media/azure-resource-manager-test-drive/howtopub1.png)

Het eerste en belangrijkste veld is om te bepalen of u wilt dat de Test Drive of niet ingeschakeld voor uw aanbieding. Wanneer u selecteert **Ja,** vindt u de rest van het formulier in met alle vereiste velden voor u om in te vullen. Wanneer u selecteert **Nee,** het formulier wordt uitgeschakeld en als u opnieuw met de Test Drive uitgeschakeld publiceren, uw Test Drive wordt verwijderd uit de productie.

Opmerking: Als er Tests stations actief gebruikt door gebruikers, deze Test Drives blijft actief totdat de sessie is verlopen.

### <a name="details"></a>Details

De volgende sectie om in te vullen is dat de informatie over uw Test Drive biedt.

![Test Drive gedetailleerde informatie](./media/azure-resource-manager-test-drive/howtopub2.png)

**Beschrijving -** *vereist* dit is waar u de beschrijving van de belangrijkste schrijven over wat er op uw Test Drive. De klant komt hier om te lezen welke scenario's voor uw Test Drive die betrekking over het product hebben wordt. 

**Gebruikershandleiding -** *vereist* dit is de uitgebreide overzicht van uw Test Drive-ervaring. De klant dit wordt geopend en precies wat u wilt laten doen tijdens de proefrit kunt doorlopen. Het is belangrijk dat deze inhoud eenvoudig is te begrijpen en te volgen. (Dit moet een PDF-bestand zijn)

**Test Drive demovideo -** *aanbevolen* is vergelijkbaar met de handmatige gebruiker, het is raadzaam om op te nemen van een video met zelfstudie van uw Test Drive-ervaring. De klant dit eerdere of tijdens de Test Drive wordt gecontroleerd en precies wat u wilt laten doen tijdens de proefrit kunt doorlopen. Het is belangrijk dat deze inhoud eenvoudig is te begrijpen en te volgen.

- **Naam** -titel van uw Video
- **Koppeling** -moet een ingesloten URL van uw buis of video. Voorbeeld voor het ophalen van de ingesloten url vindt u hieronder:
- **Miniatuur** -moet een afbeelding van hoge kwaliteit (533 x 324) pixels zijn. Het verdient aanbeveling om hier een schermafbeelding van een deel van uw Test Drive-ervaring te.

Hieronder ziet u hoe deze velden weergegeven voor uw klant tijdens de Test Drive-ervaring.

![Locatie van Test Drive-velden in de Marketplace-aanbieding](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Technische configuratie

De volgende sectie om in te vullen is waar u uw Test Drive Resource Manager-sjabloon uploaden en definiëren hoe specifiek werk uw Test Drive-exemplaren.

![](./media/azure-resource-manager-test-drive/howtopub5.png)

**Exemplaren -** *vereist* dit is waar u configureren hoeveel exemplaren u wilt, in welke regio('s), en hoe snel uw klanten de Test Drive krijgen.

- **Exemplaren** -selecteert u de regio's is waar u kiezen waar uw Test Drive Resource Manager-sjabloon wordt geïmplementeerd in. Het verdient aanbeveling om op te halen slechts één regio waar u uw klanten op meest verwacht.
- **Hot** -nummer van Test Drive-exemplaren die al zijn geïmplementeerd en wacht op toegang tot per geselecteerde regio. Klanten kunnen onmiddellijk toegang krijgen tot deze Test Drives in plaats van dat moet worden gewacht voor een implementatie. Het verschil is dat deze instanties altijd worden uitgevoerd op uw Azure-abonnement, zodat ze een grotere uptime kosten wordt gebracht. Het wordt ten zeerste aanbevolen om **ten minste één Hot exemplaar**, zoals de meeste van uw klanten niet wilt wachten tot volledige implementaties om te voltooien en dus er een inleverbibliotheek in klanten te worden gebruikt is.
- **Warme** - nummer van Test Drive-exemplaren per regio die zijn geïmplementeerd en vervolgens de virtuele machine is gestopt en opgeslagen in Azure storage. De wachttijd voor Warm-exemplaren is langzamer dan ' hot ' exemplaren, maar de kosten voor actieve tijdsduur van opslag is ook minder duur.
- **Koude** -nummer van Test Drive-exemplaren per regio die mogelijk kunnen worden geïmplementeerd. Koude exemplaren is vereist voor de hele Test Drive Resource Manager-sjabloon te doorlopen van een implementatie op het moment van een klant aanvraagt van de Test Drive, zodat de virtuele machine is langzamer dan Hot of Warm-exemplaren. De verhouding is echter dat u alleen hoeft te betalen voor de duur van Test Drive.

Berekent het totale aantal potentiële gelijktijdige Test Drives u gaat op dit moment beschikbaar te maken en controleer of dat uw quotumlimiet voor uw abonnement kan omgaan met die hoeveelheid gelijktijdige:

**(Aantal regio's geselecteerd x Hot exemplaren) + (nummer van de regio's geselecteerd x warme exemplaren) + (nummer van de regio's geselecteerd x koude exemplaren)**

**Test Drive duur (uren) -** *vereist* duur voor hoe lang de Test Drive blijft actief is, in \# uur. De Test Drive wordt automatisch beëindigd na afloop van deze periode.

**Test Drive Resource Manager-sjabloon:** *vereist* uploaden van uw Resource Manager-sjabloon. Dit is het bestand dat u in de vorige sectie hierboven hebt gemaakt. Naam van de belangrijkste sjabloonbestand: 'main-template.json' en zorg ervoor dat uw Resource Manager-sjabloon bevat parameters die voor de variabelen voor de sleutel die nodig zijn. (Dit moet een ZIP-bestand zijn)

**Toegang tot informatie -** *vereist* nadat de Test Drive wordt door een klant, de toegang tot informatie weergegeven. Deze instructies zijn bedoeld voor het delen van de parameters die handig zijn in uw Test Drive Resource Manager-sjabloon. Gebruiken om op te nemen output-parameters, dubbele gekrulde haken (bijvoorbeeld **{{outputname}}**), en ze correct zal worden ingevoegd in de locatie. (HTML-indeling van tekenreeks wordt aanbevolen hier moeten worden weergegeven in de front-end).

### <a name="test-drive-deployment-subscription-details"></a>Abonnementsgegevens van Test Drive-implementatie

Het laatste gedeelte om in te vullen is om de Test Drives automatisch implementeren door verbinding te maken van uw Azure-abonnement en Azure Active Directory (AD) te kunnen.

![Details van Test Drive implementatie abonnement](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure-abonnements-ID -** *vereist* dit verleent toegang tot Azure-services en de Azure-portal. Het abonnement is waarbij Resourcegebruik wordt gerapporteerd en services worden in rekening gebracht. Als u nog geen een **afzonderlijke** alleen, gaat u verder en maak een Azure-abonnement voor de Test Drives. U kunt Azure abonnement-id's vinden door te melden bij Azure portal en te navigeren naar de abonnementen in het menu aan de linkerkant. (Voorbeeld: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure-abonnementen](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD-Tenant-ID -** *vereist* hebt u een Tenant-ID al beschikbaar u deze hieronder in de eigenschappen vindt -\> Directory-ID.

![Azure Active Directory-eigenschappen](./media/azure-resource-manager-test-drive/subdetails3.png)

Anders maakt u een nieuwe Tenant in Azure Active Directory.

![Lijst met Azure Active Directory-tenants](./media/azure-resource-manager-test-drive/subdetails4.png)

![De organisatie, het domein en het land voor de Azure AD-tenant definiëren](./media/azure-resource-manager-test-drive/subdetails5.png)

![Controleer of de selectie](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD-App-ID -** *vereist* volgende stap is het maken en een nieuwe toepassing registreren. We gebruiken deze toepassing bewerkingen op uw Test Drive-instantie uit te voeren.

1. Ga naar de zojuist gemaakte map of al bestaande directory en Azure Active directory selecteren in het filterdeelvenster.
2. Zoeken naar 'App-registraties' en klik op 'Toevoegen'
3. Geef een toepassingsnaam in.
4. Selecteer het Type als ' Web-app / API '
5. Geef een waarde in de aanmeldings-URL, we hebben gewonnen\'t worden met behulp van dat veld.
6. Klik op maken.
7. Nadat de toepassing is gemaakt, gaat u naar eigenschappen -\> instellen van de toepassing als meerdere tenants en klik op opslaan.

Klik op Opslaan. De laatste stap is om te halen van de toepassings-ID voor deze geregistreerde app en plak deze in het veld van Test Drive hier.

![Details van Azure AD-toepassing-ID](./media/azure-resource-manager-test-drive/subdetails7.png)

Krijgen we de toepassing gebruiken om te implementeren voor het abonnement, moet de toepassing toevoegen als Inzender van het abonnement. De instructies voor deze zijn als hieronder:

1. Navigeer naar de blade abonnementen en selecteer het juiste abonnement dat u voor de Test Drive gebruikt.
2. Klik op de Access Control (IAM).
3. Klik op + toevoegen in de nieuwe blade.
4. De rol Inzender ingesteld.
5. Typ de naam van de AAD-toepassing en selecteert u de AAD-toepassing aan de rol toe te wijzen.
6. Klik op opslaan.

![Een nieuwe Access Control-principal toevoegen](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)

![De machtigingen toevoegen](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)

**Azure AD App Key -** *vereist* het laatste veld voor het genereren van een verificatiesleutel nodig is. Onder sleutels, een beschrijving van de sleutel, toevoegen de duur instellen op nooit verlopen, klikt u vervolgens selecteert u opslaan. Het is **belangrijk** om te voorkomen dat een verlopen sleutels, die wordt verbroken uw test drive in productie. Deze waarde kopiëren en plak deze in uw vereist Test Drive-veld.

![Ziet u de sleutels voor de Azure AD-toepassing](./media/azure-resource-manager-test-drive/subdetails8.png)

<a name="next-steps"></a>Volgende stappen
----------

Nu dat u al uw Test Drive-velden ingevuld hebt, doorlopen en **opnieuw publiceren** uw aanbieding. Wanneer uw Test Drive is verstreken certificering, gaat u een uitgebreid te testen de klantervaring in de **preview** van uw aanbieding. Start een Test uit in de gebruikersinterface en opent u uw Azure-abonnement in Azure portal en controleer of dat uw Test Drives zijn wordt volledig correct is geïmplementeerd.

![Azure Portal](./media/azure-resource-manager-test-drive/subdetails9.png)

Het is belangrijk te weten een Test Drive-instanties niet te verwijderen als deze zijn ingericht voor uw klanten, zodat de Test Drive-service automatisch deze brongroepen opschonen wordt nadat een klant klaar is.

Als u vertrouwd met de Preview-aanbieding, het is nu tijd om **live gaan**! Er is een proces uiteindelijke beoordeling van Microsoft zodra de aanbieding gepubliceerd naar dubbele selectievakje de volledige end-to-end-ervaring is. Als voor een of andere reden de aanbieding wordt geweigerd, wordt er een melding verzonden naar de technische contactpersoon voor uw aanbod waarin wordt uitgelegd wat moet worden opgelost.

Als u meer vragen hebt, zoekt advies voor probleemoplossing of wilt u uw Test Drive meer successen te behalen, gaat u naar [Veelgestelde vragen, probleemoplossing en aanbevolen procedures](./marketing-and-best-practices.md).

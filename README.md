## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft Open Source-gedragscode

Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing.
Zie voor meer informatie de [Code van Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met [ opencode@microsoft.com ](mailto:opencode@microsoft.com) met aanvullende vragen of opmerkingen.

## <a name="contribute-to-azure-technical-documentation"></a>Bijdragen aan technische documentatie voor Azure
Bijdragen vanuit de community (gebruikers, klanten, partners, MSFT werknemers buiten core Azure producteenheden, enz.) en vanuit werknemers in core Azure producteenheden zijn van harte welkom. Op welke manier u kunt bijdragen hangt af van wie u bent:

* **Community - minder ingrijpende updates**: Als u kleine wijzigingen wilt toepassen kunt u het artikel vinden in deze repository of bezoek het artikel op [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) en klik op de **Bewerken** koppeling in het artikel welke u naar de GitHub-bron voor het artikel zal brengen. Vervolgens kunt u aan de hand van de GitHub UI uw updates toepassen. U bent ook van harte welkom om de repository te forken en via uw fork de updates te verzenden.

* **Community - nieuwe artikelen**: Als u deel uitmaakt van de Azure-community en u wilt een nieuw artikel maken, moet u werken met een werknemer zodat het nieuwe artikel via een combinatie van private en publieke repositories kan worden aangemaakt.

* **Werknemers**: Wanneer u een technical writer, programmanager bent of ontwikkelaar van het productteam voor een Azure-service en het is uw taak om bij te dragen aan of het schrijven van technische artikelen, moet u de private repository gebruiken op (https://github.com/MicrosoftDocs/azure-docs-pr). Als u grotere veranderingen toepast aan een bestaand artikel, afbeeldingen toevoegt of wijzigt of een nieuwe artikel aanlevert, zult u de repository moeten forken, Git Bash installeren, een Markdown editor installeren en Git commando's moeten kennen. Bekijk de [handleiding voor de interne bijdragers](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) voor meer informatie.


## <a name="about-your-contributions-to-azure-content"></a>Over uw bijdragen aan Azure-content
### <a name="minor-corrections"></a>Kleine correcties
Kleine correcties of verduidelijkingen die u indient voor documentatie en codevoorbeelden in deze repository vallen onder de [docs.microsoft.com-gebruiksvoorwaarden](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Grotere wijzigingen
Als u een nieuwe of belangrijke wijziging in de documentatie of aan de code-voorbeelden en een pull-request indient, sturen we u een opmerking in GitHub waarin u een Contribution bijdrage License Agreement (CLA) wordt gevraagd in te dienen als u geen medewerker van Microsoft bent. U moet het online formulier invullen voordat we uw pull-aanvraag kunnen accepteren.

## <a name="tools-and-setup"></a>Hulpprogramma's en installatie
Community-gebruikers kunnen de GitHub UI of de repository forken om bij te dragen. Werknemers zullen de [handleiding voor de interne bijdragers](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) moeten bezoeken voor meer informatie over hoe u kunt bijdragen aan de technische documentatie.

## <a name="repository-organization"></a>Organisatie van de repository
De inhoud in de repository azure-docs volgt de documentatiestructuur op https://docs.microsoft.com/azure. Deze bibliotheek bevat twee hoofdmappen:

### <a name="articles"></a>\articles
De map *\articles* bevat de documentatieartikelen die zijn opgemaakt als Markdown-bestanden met de indeling *.md*. Artikelen zijn doorgaans gegroepeerd op Azure-service.

De *\articles* map bevat de *\media* voor mediabestanden voor de hoofdmap-artikelen. Daarbinnen staan submappen met de afbeeldingen voor elk artikel. De servicemappen bevatten een afzonderlijke mediamap voor de artikelen in elke servicemap. De mappen met afbeeldingen voor de artikelen hebben dezelfde naam als het artikelbestand, maar zonder de bestandsextensie *.md*.

### <a name="includes"></a>\includes
U kunt herbruikbare secties met content maken die moeten worden opgenomen in een of meer artikelen. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Hoe u markdown kunt gebruiken om uw onderwerp op te maken
Alle artikelen in deze bibliotheek gebruiken GitHub-Markdown.  Hier volgt een lijst van resources.

* [Basisprincipes van markdown](https://help.github.com/articles/markdown-basics/)
* [Afdrukbare cheatsheet voor markdown](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)


## <a name="labels"></a>Labels
In de publieke azure-docs repository worden automatisch labels toegevoegd aan pull requests om ons te helpen de pull request workflows te managen en u te laten weten wat de status is van uw pull request:

* Contribution License Agreement related
  * cla-not-required: De wijziging is relatief klein en vereist geen CLA.
  * cla-required: De scope van de verandering is relatief groot en vereist dat een CLA wordt ondertekend.
  * cla-signed: De bijdrager heeft de CLA ondertekend, de pull request kan worden doorgezet voor een review.
* Change sent to author: De auteur is ingelicht over de openstaande pull request.
* ready-to-merge: Klaar om te worden bekeken door ons pull request review team



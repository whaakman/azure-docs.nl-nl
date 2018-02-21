## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft Open Source-gedragscode

Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing.
Zie voor meer informatie de [Code van Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met [ opencode@microsoft.com ](mailto:opencode@microsoft.com) met aanvullende vragen of opmerkingen.

## <a name="contribute-to-azure-technical-documentation"></a>Bijdragen aan technische documentatie voor Azure
Bijdragen vanuit de community (gebruikers, klanten, partners, MSFT-werknemers buiten core Azure-producteenheden, enz.) en vanuit werknemers in core Azure-producteenheden zijn van harte welkom. Op welke manier u kunt bijdragen, hangt af van wie u bent:

* **Community - minder ingrijpende updates**: Als u kleine wijzigingen wilt toepassen, kunt u het artikel vinden in deze opslagplaats of het artikel bezoeken op [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure). Als u klikt op de koppeling **Bewerken** in het artikel, wordt u naar de GitHub-bron van het artikel geleid.  Vervolgens kunt u aan de hand van de GitHub-UI uw updates toepassen. U bent ook van harte welkom om de opslagplaats te forken en via uw fork de updates te verzenden.

* **Community - nieuwe artikelen**: Als u deel uitmaakt van de Azure-community en u wilt een nieuw artikel maken, moet u werken met een werknemer zodat het nieuwe artikel via een combinatie van privéopslagplaatsen en openbare opslagplaatsen kan worden aangemaakt.

* **Werknemers**: Wanneer u een technisch schrijver, programmamanager of ontwikkelaar van het productteam voor een Azure-service bent en het is uw taak om bij te dragen aan technische artikelen of deze te schrijven, moet u de privéopslagplaats gebruiken op (https://github.com/MicrosoftDocs/azure-docs-pr).  Als u grotere veranderingen aanbrengt aan een bestaand artikel, afbeeldingen toevoegt of wijzigt of een nieuw artikel aanlevert, zult u de opslagplaats moeten forken, Git Bash installeren, een Markdown-editor installeren en Git--opdrachten moeten kennen. Zie [handleiding voor de interne bijdragers](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) voor meer informatie.


## <a name="about-your-contributions-to-azure-content"></a>Over uw bijdragen aan Azure-content
### <a name="minor-corrections"></a>Kleine correcties
Kleine correcties of verduidelijkingen die u indient voor documentatie en codevoorbeelden in deze opslagplaats vallen onder de [docs.microsoft.com-gebruiksvoorwaarden](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Grotere wijzigingen
Als u een nieuwe of belangrijke wijziging in de documentatie of aan de code-voorbeelden en een pull-aanvraag indient, sturen we u een opmerking in GitHub waarin we u vragen een CLA (Contribution License Agreement) in te dienen als u geen medewerker van Microsoft bent. U moet het online-formulier invullen voordat we uw pull-aanvraag kunnen accepteren.

## <a name="tools-and-setup"></a>Hulpprogramma's en installatie
Community-gebruikers kunnen de GitHub UI of de repository forken om bij te dragen. Werknemers zullen de [handleiding voor de interne bijdragers](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) moeten bezoeken voor meer informatie over hoe u kunt bijdragen aan de technische documentatie.

## <a name="repository-organization"></a>Organisatie van de repository
De inhoud in de opslagplaats azure-docs volgt de documentatiestructuur op https://docs.microsoft.com/azure. Deze bibliotheek bevat twee hoofdmappen:

### <a name="articles"></a>\articles
De map *\articles* bevat de documentatieartikelen die zijn opgemaakt als Markdown-bestanden met de indeling *.md*. Artikelen zijn doorgaans gegroepeerd op Azure-service.

De *\articles* map bevat de *\media* voor mediabestanden voor de hoofdmap-artikelen. Daarbinnen staan submappen met de afbeeldingen voor elk artikel.  De servicemappen bevatten een afzonderlijke mediamap voor de artikelen in elke servicemap. De mappen met afbeeldingen voor de artikelen hebben dezelfde naam als het artikelbestand, maar zonder de bestandsextensie *.md*.

### <a name="includes"></a>\includes
U kunt herbruikbare secties met content maken die moeten worden opgenomen in een of meer artikelen. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Hoe u markdown kunt gebruiken om uw onderwerp op te maken
Alle artikelen in deze bibliotheek gebruiken GitHub-Markdown.  Hier volgt een lijst van resources.

* [Basisprincipes van markdown](https://help.github.com/articles/markdown-basics/)
* [Afdrukbare cheatsheet voor markdown](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)


## <a name="labels"></a>Labels
In de openbare opslagplaats azure-docs worden automatisch labels toegevoegd aan pull-aanvragen om ons te helpen de workflows voor pull-aanvragen te beheren en u te laten weten wat de status is van uw pull-aanvraag:

* Met betrekking tot de CLA
  * CLA niet is vereist: de wijziging is relatief klein en vereist niet dat u zich een CLA aanmeldt.
  * CLA vereist: het bereik van de wijziging is relatief groot en vereist dat u zich een CLA aanmeldt.
  * CLA ondertekend: de inzender de CLA ondertekend, zodat de pull-aanvraag kunt nu verdergaan voor revisie.
* Wijzigen is verzonden naar de auteur: de auteur van de in behandeling zijnde pull-aanvraag is gewaarschuwd.
* gereed voor samenvoegen: gereed voor controle door ons team pull-aanvraag controleren.



De meeste van de tijd verificatiefouten het gevolg van onjuiste of inconsistente configuratie-instellingen. Hier volgen enkele specifieke suggesties voor wat u moet controleren.

* Zorg ervoor dat u hebt niet mist de **opslaan** knop elke locatie. Dit is vaak eenvoudig doen en het resultaat is dat u hebt voor het bekijken van de juiste waarden op een portal-pagina, maar ze daadwerkelijk niet in de Azure-omgeving of Azure AD-toepassing opgeslagen zijn.
* Voor instellingen die zijn geconfigureerd de **toepassingsinstellingen** blade van de Azure portal, zorg ervoor dat de juiste API-app of web-app is geselecteerd als de instellingen zijn ingevoerd.  Ook voor zorgen dat de instellingen worden ingevoerd als **appinstellingen** en niet **verbindingsreeksen**, zoals de indeling van de twee secties vergelijkbaar is.
* Download het manifest opnieuw uit om te controleren of voor een JavaScript-front-end-verificatie, `oauth2AllowImplicitFlow` is gewijzigd naar `true`.
* Controleer of u HTTPS gebruikt waar u de URL's geconfigureerd:
  
  * In de projectcode
  * In de CORS
  * In de Azure-omgeving App-instellingen voor elke API-app en web-app
  * In de instellingen van Azure AD-toepassing.
    
    Houd er rekening mee dat als u een API-app-URL van de portal kopieert, vaak er `http://` en u moet deze handmatig wijzigen `https://`.
* Zorg ervoor dat eventuele codewijzigingen zijn geïmplementeerd. In een oplossing meerdere projecten is het bijvoorbeeld mogelijk voor het wijzigen van de code van het project en kies een van de andere per ongeluk wanneer u van plan bent voor het implementeren van de wijziging.
* Zorg ervoor dat u HTTPS-URL's in uw browser niet HTTP-URL's gaat. Visual Studio maakt standaard profielen met HTTP-URL's publiceren, en dat is wat in de browser wordt geopend nadat u een project implementeert.
* Zorg dat CORS correct is geconfigureerd op de API-app de JavaScript-code roept voor verificatie naar een JavaScript-front-end. Bij twijfel over de vraag of het probleem CORS-gerelateerde probeer ' * ' als de URL van de toegestane oorsprong. 
* Open uw browser de Console hulpprogramma's voor ontwikkelaars tabblad voor meer foutinformatie voor JavaScript-front-end en onderzoeken van HTTP-aanvragen op het netwerk. Console-foutberichten mogelijk misleidend. Als u een bericht met een CORS-fout krijgt, is het werkelijke probleem mogelijk verificatie. U kunt controleren of dit het geval is de app uitgevoerd met verificatie tijdelijk tijdelijk uitgeschakeld.
* Zorg ervoor dat u krijgt zo veel mogelijk gegevens in foutberichten mogelijk door in te stellen voor een .NET API-app [customErrors-modus uit](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Voor een .NET API-app starten een [externe foutopsporingssessie](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), en bekijk de waarden van de variabelen die zijn doorgegeven aan de code die gebruikmaakt van ADAL bearer-token verkrijgen of code waarmee wordt gecontroleerd of de claims op basis van de verwachte service principal-ID. Houd er rekening mee dat uw code kan worden opgepikt configuratiewaarden uit verschillende bronnen, zodat het mogelijk om te zoeken verrassingen op deze manier is. Bijvoorbeeld, als u een typefout gemaakt tijdens `ida:ClientId` als `ida:ClientID` bij het configureren van instellingen voor Azure App Service-omgeving, de code haalt de `ida:ClientId` waarde die op zoek naar van het Web.config-bestand, de Azure App Service-instelling wordt genegeerd. 
* Als dingen niet in een normale Internet Explorer-venster werken, een bestaande aanmelden mogelijk verstorend werkt; InPrivate en probeer Chrome of Firefox.


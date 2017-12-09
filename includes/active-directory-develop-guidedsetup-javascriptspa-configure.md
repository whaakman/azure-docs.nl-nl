
## <a name="register-your-application"></a>Uw toepassing registreren

Er zijn meerdere manieren om een toepassing maken, selecteer een van beide:

### <a name="option-1-register-your-application-express-mode"></a>Optie 1: De toepassing (Express-modus) registreren
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:

1.  Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor *Begeleide instelprocedure* is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="option-2-register-your-application-advanced-mode"></a>Optie 2: De toepassing (geavanceerde modus) registreren

1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor *Begeleide instelprocedure* is uitgeschakeld
4.  Klik op `Add Platform`, selecteer vervolgens`Web`
5. Voeg de `Redirect URL` die overeenkomen met de URL van de toepassing op basis van uw webserver. Zie de secties hieronder voor instructies over het instellen / ophalen van de omleidings-URL in Visual Studio en Python.
6. Klik op *opslaan*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Volg de instructies voor het verkrijgen van de omleidings-URL:
> 1.    In *Solution Explorer*, selecteert u het project en bekijk de `Properties` venster (als u een venster met eigenschappen niet ziet, drukt u op `F4`)
> 2.    Kopieer de waarde van `URL` naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Ga terug naar de *toepassing Registratieportal* en plak de waarde als een `Redirect URL` en klik op 'Opslaan'

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Instelling Omleidings-URL voor Python
> Voor Python, kunt u de web server-poort via de opdrachtregel instellen. Deze begeleide setup gebruikt de poort 8080 voor verwijzing, maar gerust om andere poort beschikbaar te gebruiken. In elk geval Volg de onderstaande instructies voor het instellen van een Omleidings-URL in de registratiegegevens van toepassing:<br/>
> - Ga terug naar de *toepassing-Portal voor Wachtwoordregistratie* en stel `http://localhost:8080/` als een `Redirect URL`, of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* is de aangepaste TCP-poort getal) en klik op 'Opslaan'


#### <a name="configure-your-javascript-spa"></a>Uw JavaScript SPA configureren

1.  Maak een bestand met de naam `msalconfig.js` met de registratiegegevens van de toepassing. Als u Visual Studio gebruikt, selecteert u het project (basismap project), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`. Geef deze de naam`msalconfig.js`
2.  Voeg de volgende code naar uw `msalconfig.js` bestand:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Vervang <code>Enter_the_Application_Id_here</code> met de toepassings-Id die u zojuist hebt geregistreerd
</li>
</ol>

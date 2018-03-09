## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw App

In deze stap moet u de omleidings-URL van de registratiegegevens van de toepassing configureren en vervolgens de toepassings-ID toe te voegen aan uw toepassing JavaScript SPA.

### <a name="configure-redirect-url"></a>Omleidings-URL configureren

Configureer de `Redirect URL` veld met de URL voor uw pagina index.html op basis van uw webserver en klik vervolgens op *Update*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-instructies voor het verkrijgen van de omleidings-URL
> Als u de omleidings-URL:
> 1.    In *Solution Explorer*, selecteert u het project en bekijk de `Properties` venster (als u een venster met eigenschappen niet ziet, drukt u op `F4`)
> 2.    Kopieer de waarde van `URL` naar het Klembord:<br/> ![Projecteigenschappen](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Plak de waarde als een `Redirect URL` Klik boven aan deze pagina `Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Instelling Omleidings-URL voor Python
> Voor Python, kunt u de web server-poort via de opdrachtregel instellen. Deze begeleide setup gebruikt de poort 8080 voor verwijzing, maar gerust om andere poort beschikbaar te gebruiken. In elk geval, gebruik de volgende instructies voor het instellen van een Omleidings-URL in de registratiegegevens van toepassing:<br/>
> Stel `http://localhost:8080/` als een `Redirect URL` boven aan deze pagina of gebruik `http://localhost:[port]/` als u een aangepaste TCP-poort (waar *[poort]* het aangepaste TCP-poortnummer), en klik op 'Bijwerken'

### <a name="configure-your-javascript-spa-application"></a>Uw toepassing JavaScript SPA configureren

1.  Maak een bestand met de naam `msalconfig.js` met de registratiegegevens van de toepassing. Als u Visual Studio gebruikt, selecteert u het project (basismap project), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`. Geef deze de naam `msalconfig.js`
2.  Voeg de volgende code naar uw `msalconfig.js` bestand:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 

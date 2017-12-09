
## <a name="create-an-application-express"></a>Maken van een toepassing (snelle)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het toevoegen van een Omleidings-URL voor uw toepassing

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>De registratiegegevens van uw toepassing toevoegen aan uw oplossing (Geavanceerd)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3.  Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4.  Klik op `Add Platform`, selecteer vervolgens`Web`
5.  Ga terug naar Visual Studio en klik in Solution Explorer, selecteert u het project en kijk in het venster Eigenschappen (als er geen een venster met eigenschappen, druk op F4)
6.  SSL ingeschakeld om te wijzigen`True`
7.  De SSL-URL kopiëren en deze URL toevoegen aan de lijst van de omleidings-URL's in de lijst van de Portal van de registratie van de omleidings-URL's:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
8.  Voeg de volgende in `web.config` zich in de hoofdmap in de sectie `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Vervang `ClientId` met de toepassings-Id die u zojuist hebt geregistreerd
</li>
<li>
Vervang `redirectUri` met de SSL-URL van uw project
</li>
</ol>
<!-- End Docs -->

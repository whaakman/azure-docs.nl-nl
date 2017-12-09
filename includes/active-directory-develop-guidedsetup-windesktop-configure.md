
## <a name="create-an-application-express"></a>Maken van een toepassing (snelle)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>De registratiegegevens van uw toepassing toevoegen aan uw oplossing (Geavanceerd)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4. Klik op `Add Platform`, selecteer daarna `Native Application` en klik op Opslaan
5. Kopiëren van de GUID in toepassings-ID, gaat u terug naar Visual Studio, open `App.xaml.cs` en vervang `your_client_id_here` met de toepassings-ID die u zojuist hebt geregistreerd:

```csharp
private static string ClientId = "your_application_id_here";
```

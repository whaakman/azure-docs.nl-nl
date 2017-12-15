
## <a name="register-your-application"></a>Uw toepassing registreren
U kunt uw toepassing registreren op twee manieren.

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus
U kunt snel uw toepassing registreren als volgt:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Selecteer **een app toevoegen**.

3. In de **toepassingsnaam** Voer een naam voor uw toepassing.

4. Zorg ervoor dat de **Begeleide instelprocedure** selectievakje is geselecteerd en selecteer vervolgens **maken**.

5. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus
Uw toepassing registreren en de registratiegegevens van uw toepassing toevoegen aan uw oplossing, kunt u het volgende:
1. Als u al uw toepassing nog niet hebt geregistreerd, gaat u naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app).

2. Selecteer **een app toevoegen**.

3. In de **toepassingsnaam** Voer een naam voor uw toepassing. 

4. Zorg ervoor dat de **Begeleide instelprocedure** selectievakje is uitgeschakeld en selecteer vervolgens **maken**.

5. Selecteer **toevoegen Platform**, selecteer **systeemeigen toepassing**, en selecteer vervolgens **opslaan**.

6. In de **toepassings-ID** vak, Kopieer de GUID.

7. Ga naar Visual Studio, open de *App.xaml.cs* bestand en vervang `your_client_id_here` met de toepassings-ID die u zojuist hebt geregistreerd en gekopieerd.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="pausing-resetting-or-stopping-the-session"></a>Onderbreken of stoppen van de sessie opnieuw instellen

Als u wilt stoppen van de sessie kunt u tijdelijk de Stop-methode aanroepen. In dat geval stopt alle Volgers en verwerking van de omgeving, zelfs als u ProcessFrame() aanroepen. U kunt vervolgens aanroepen Start() om door te gaan met de verwerking. Wanneer hervat, wordt omgevingsgegevens al is vastgelegd in de sessie bijgehouden.

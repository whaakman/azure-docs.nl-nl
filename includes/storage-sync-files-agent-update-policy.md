Updates op de Azure-bestand Sync-agent worden uitgebracht regelmatig nieuwe functionaliteit toe te voegen en los eventuele problemen die zijn ontdekt. Het is raadzaam dat u Microsoft Update om alle updates met de Azure-bestand Sync-agent zoals we ze brengen configureren. We begrijpen dat sommige organisaties, zoals strikt bepalen en de updates hebt getest. 

Voor implementaties die gebruikmaken van eerdere versies van de Azure-bestand Sync-agent:

- De synchronisatieservice opslag respecteert de hoofdversie van de voorgaande drie maanden na de initiële release van een nieuwe primaire versie. De synchronisatieservice opslag ondersteunen bijvoorbeeld versie 1. \* tot drie maanden na de release van versie 2. \*.
- Nadat de drie maanden zijn verstreken, begint de synchronisatieservice opslag blokkeren geregistreerde Servers die van de verlopen versie gebruikmaken wordt gesynchroniseerd met de synchronisatie-groepen.
- In de drie maanden voor een voorgaande hoofdversie gaat alle oplossingen voor problemen alleen voor de huidige primaire versie.

> [!Note]  
> U ontvangt een pop-upmelding in Azure Portal als u een versie van Azure File Sync gebruikt die binnen de komende drie maanden verloopt.
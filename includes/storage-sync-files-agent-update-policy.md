De Azure-bestand Sync-agent wordt bijgewerkt regelmatig nieuwe functionaliteit toe te voegen en om problemen te verhelpen. We raden aan met het configureren van Microsoft Update om updates voor de Azure-bestand Sync-agent als ze beschikbaar zijn. We begrijpen dat sommige organisaties willen strikt bepalen en de updates hebt getest.

Voor implementaties die gebruikmaken van eerdere versies van de Azure-bestand Sync-agent:

- Na de initiële release van een nieuwe primaire versie respecteert de Storage-Sync-Service de vorige primaire versie voor 3 maanden. De synchronisatieservice opslag ondersteunt bijvoorbeeld versie 1. \* voor 3 maanden na de release van versie 2. \*.
- Nadat de drie maanden zijn verstreken, blokkeert de synchronisatieservice opslag geregistreerde Servers met de verlopen versie wordt gesynchroniseerd met de synchronisatiegroepen.
- In de drie maanden die de vorige primaire versie wordt gehonoreerd, gaat u alle oplossingen voor problemen alleen naar de huidige primaire versie (nieuw).

> [!Note]  
> Als uw versie van Azure bestand Sync binnen drie maanden verloopt, bent u een melding via toast-melding in de Azure portal.

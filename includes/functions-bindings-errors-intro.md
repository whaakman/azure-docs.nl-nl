Azure Functions [triggers en bindingen](..\articles\azure-functions\functions-triggers-bindings.md) communiceren met verschillende Azure-services. Bij het integreren met deze services, er fouten gegenereerd die afkomstig van de API's van de onderliggende Azure-services zijn. Fouten kunnen ook optreden wanneer u probeert te communiceren met andere services van uw functiecode aan te geven met behulp van REST of clientbibliotheken. Als u wilt voorkomen dat gegevens en ervoor te zorgen goede werking van uw functies, is het belangrijk voor het afhandelen van fouten van de bron.

De volgende triggers hebben ondersteuning voor ingebouwde nieuwe pogingen:

* [Azure Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (wachtrij/onderwerp)](../articles/azure-functions/functions-bindings-service-bus.md)

Standaard deze triggers opnieuw kunnen worden uitgevoerd tot vijf keer. Na de vijfde opnieuw proberen, deze triggers een bericht weggeschreven naar een speciaal [onverwerkbare wachtrij](..\articles\azure-functions\functions-bindings-storage-queue.md#trigger---poison-messages). 

Voor de andere Functions-triggers is het niet automatisch opnieuw geprobeerd wanneer er fouten optreden tijdens het uitvoeren van de functie. Om te voorkomen dat verlies van triggergegevens moet een fout opgetreden in de functie, raden wij aan dat u trycatch-blokken in uw functiecode aan te geven om af te vangen van eventuele fouten. Wanneer een fout optreedt, schrijft u de informatie die in de functie is doorgegeven door de trigger met een speciale "onverwerkbare" berichtenwachtrij. Deze aanpak is hetzelfde account gebruikt door de [Blob storage-trigger](..\articles\azure-functions\functions-bindings-storage-blob.md#trigger---poison-blobs). 

Op deze manier kunt u de trigger-gebeurtenissen die kunnen worden verbroken vanwege fouten en probeer ze op een later tijdstip met behulp van een andere functie het verwerken van berichten uit het beheer van onverwerkbare wachtrij met behulp van de opgeslagen gegevens vastleggen.  

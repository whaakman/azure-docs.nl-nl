De Azure File Sync-agent wordt bijgewerkt regelmatig nieuwe functionaliteit toe te voegen en om problemen te verhelpen. Het is raadzaam om dat u bij het configureren van Microsoft Update om updates voor de Azure File Sync-agent als ze beschikbaar.

#### <a name="major-vs-minor-agent-versions"></a>Primaire en secundaire agent-versies
* Primaire agent-versies zijn vaak bevat nieuwe functies en steeds meer hebben als het eerste deel van het versienummer. Bijvoorbeeld: *2.\*.\**
* Secundaire agent-versies worden ook 'patches' genoemd en vaker dan primaire versies worden uitgebracht. Ze bevatten vaak bugfixes en verbeteringen in kleinere, maar er zijn geen nieuwe functies. Bijvoorbeeld:  *\*.3.\**

#### <a name="upgrade-paths"></a>Upgradepaden
Er zijn drie goedgekeurd en getest manieren om de Azure File Sync-agent-updates te installeren. Deze update-werk van de paden voor de primaire en secundaire versies.
1. **(Aanbevolen) Microsoft Update voor het automatisch downloaden en installeren van agentupdates configureren.**  
    We raden altijd aan te nemen van elke Azure File Sync-update om te controleren of dat u toegang hebt tot de meest recente oplossingen voor de server-agent. Microsoft Update is dit proces naadloze door automatisch downloaden en installeren van updates voor u.
2. **Patch uitvoeren voor een bestaande Azure File Sync-agent met behulp van een bestand van de patch voor Microsoft Update of een uitvoerbaar bestand .msp. De meest recente updatepakket van Azure File Sync kan worden gedownload vanaf de [Microsoft Update-catalogus](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Uitvoeren van een uitvoerbaar bestand .msp, werkt de Azure File Sync-installatie met dezelfde methode die automatisch door Microsoft Update wordt gebruikt in de vorige upgrade-pad. Uitvoering van een patch voor Microsoft Update, wordt een in-place upgrade van een Azure File Sync-installatie uitvoeren.
3. **Download het nieuwste installatieprogramma van Azure File Sync-agent van de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Het downloaden van het installatieprogramma is een Microsoft Installer-pakket, of een uitvoerbaar bestand MSI.**  
    Upgrade uitvoeren van een bestaande Azure File Sync-agent-installatie wordt verwijderd van de oudere versie en installeert u de meest recente versie van het gedownloade installatieprogramma. De serverregistratie, synchronisatiegroepen en andere instellingen worden beheerd door het installatieprogramma van Azure File Sync.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Agent garanties voor levenscyclus en wijzigen
Azure File Sync is een cloudservice, waardoor continu introductie van nieuwe functies en functionaliteit. Dit betekent dat een specifieke versie van de Azure File Sync-agent kan alleen worden ondersteund voor een beperkte periode. Het kader van uw implementatie, hebben de volgende regels om te waarborgen dat u voldoende tijd en de melding voor agent-updates of upgrades worden uitgevoerd in een proces voor het beheren van uw wijziging:

- Primaire agent-versies worden ondersteund voor ten minste zes maanden vanaf de datum van de eerste release.
- We garanderen dat er is een overlapping van ten minste drie maanden, tussen de ondersteuning van grote agent-versies. 
- Waarschuwingen worden gegeven voor de geregistreerde servers met behulp van een-op-binnenkort verlopen agent ten minste drie maanden vóór de vervaldatum. U kunt controleren of een geregistreerde server een oudere versie van de agent wordt gebruikt in het gedeelte van de geregistreerde servers van een Opslagsynchronisatieservice.
- De levensduur van een secundaire agentversie is gebonden aan de bijbehorende primaire versie. Bijvoorbeeld, wanneer de agent-versie 3.0 is vrijgegeven, agentversies 2. \* alle worden ingesteld op verlopen samen.

> [!Note]
> Een versie van de agent installeren met een vervaldatum-waarschuwing wordt een waarschuwing weergegeven maar slagen. Probeert te installeren of te verbinden met een verlopen agentversie wordt niet ondersteund en worden geblokkeerd.
De Azure-bestand Sync-agent wordt bijgewerkt regelmatig nieuwe functionaliteit toe te voegen en om problemen te verhelpen. We raden aan met het configureren van Microsoft Update om updates voor de Azure-bestand Sync-agent als ze beschikbaar zijn.

#### <a name="major-vs-minor-agent-versions"></a>Belangrijke versus secundaire agent-versies
* Belangrijke agent-versies zijn vaak bevat nieuwe functies en steeds meer hebben als het eerste deel van het versienummer. Bijvoorbeeld: *2.\*.\**
* Secundaire agent-versies worden ook wel 'patches' en vaker dan primaire versies worden vrijgegeven. Ze bevatten vaak oplossingen voor problemen en kleinere verbeteringen, maar er zijn geen nieuwe functies. Bijvoorbeeld:  *\*.3.\**

#### <a name="upgrade-paths"></a>Upgradepaden
Er zijn drie goedgekeurd en getest manieren de Azure-bestand Sync-agent-updates te installeren. Deze bijwerken paden werk voor primaire en secundaire versies.
1. **(Aanbevolen) Microsoft Update wilt automatisch downloaden en installeren van de agent-updates configureren.**  
    Aangeraden wordt altijd nemen van elke update Azure bestand synchronisatie om te controleren of dat u toegang hebt tot de nieuwste oplossingen voor de server-agent. Microsoft Update is dit proces naadloze door automatisch downloaden en installeren van updates voor u.
2. **Patch voor een bestaande Azure bestand Sync-agent met behulp van een bestand van de patch voor Microsoft Update of een uitvoerbaar .msp. Het meest recente updatepakket van Azure File-synchronisatie kan worden gedownload vanaf de [Microsoft Update-catalogus](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Een uitvoerbaar .msp uitgevoerd, wordt de installatie van de Azure bestand synchronisatie met dezelfde methode automatisch gebruikt door Microsoft Update in de vorige upgradepad bijgewerkt. Toepassen van een patch voor Microsoft Update, wordt een in-place upgrade van een Azure-bestand Sync-installatie uitvoeren.
3. **Download de nieuwste Azure bestand Sync-agent installer uit de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Het downloaden van het installatieprogramma is een Microsoft Installer-pakket of een uitvoerbaar MSI.**  
    Bijwerken van een bestaande Azure bestand Sync-agent-installatie verwijderd van de oudere versie en installeer de nieuwste versie van het gedownloade installatieprogramma. De registratie van de server, synchronisatiegroepen en eventuele andere instellingen die worden beheerd door de Azure-bestand Sync-installatieprogramma.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Wordt gegarandeerd dat agent lifecycle en wijzigingsbeheer
Azure File-synchronisatie is een cloudservice, waardoor continu introductie van nieuwe functies en functionaliteit. Dit betekent dat een specifieke versie van de Azure-bestand Sync-agent kan alleen worden ondersteund voor een beperkte periode. Om te vergemakkelijken van uw implementatie, hebben de volgende regels om te waarborgen dat u voldoende tijd en de melding voor de agent-updates of upgrades worden uitgevoerd in uw proces voor wijzigingsbeheer:

- Belangrijke agent-versies worden ondersteund voor ten minste zes maanden vanaf de datum van eerste release.
- Wordt gegarandeerd dat er is een overlap van ten minste drie maanden tussen de ondersteuning van belangrijke agent-versies. 
- Waarschuwingen worden gegeven voor de geregistreerde servers met behulp van een-op-binnenkort verlopen agent ten minste drie maanden vóór de vervaldatum. U kunt controleren of een geregistreerde server een oudere versie van de agent wordt gebruikt in de sectie geregistreerde servers van een opslag-Sync-Service.
- De levensduur van een secundaire agent-versie is gebonden aan de bijbehorende primaire versie. Bijvoorbeeld wanneer de agent-versie 3.0 wordt uitgebracht, agentversies 2. \* alle worden ingesteld op verlopen samen.

> [!Note]
> De versie van een agent installeren met een waarschuwing voor het vervallen wordt een waarschuwing weergegeven maar slagen. Probeert te installeren of te verbinden met een verlopen agentversie wordt niet ondersteund en worden geblokkeerd.
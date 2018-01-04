
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Weergave virtuele machines die zijn gepland voor onderhoud in de portal

Zodra een golf gepland onderhoud is gepland en meldingen worden verzonden, kunt u de lijst met virtuele machines die worden beïnvloed door het aanstaande onderhoud wave kunt zien. 

U kunt de Azure portal gebruiken en zoekt u naar virtuele machines die zijn gepland voor onderhoud.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in het linkernavigatievenster op **virtuele Machines**.

3. Klik in het deelvenster met virtuele Machines op de **kolommen** knop om de lijst met beschikbare kolommen te openen.

4. Selecteer en voeg de volgende kolommen:

   **Onderhoud** -ziet u de onderhoudsstatus voor de virtuele machine. Hier volgen de mogelijke waarden:
      
      | Waarde | Beschrijving |
      |-------|-------------|
      | Nu beginnen | De VM bevindt zich binnen het onderhoudsvenster selfservice waarmee u de onderhoudsmodus starten zelf. Hieronder vindt u voor het onderhoud op de virtuele machine starten | 
      | Gepland | De virtuele machine is gepland voor onderhoud geen optie waarmee u onderhoud initiëren. U leert van het onderhoudsvenster door te selecteren van het venster automatisch gepland in deze weergave of door te klikken op de virtuele machine | 
      | Voltooid | U hebt gestart en onderhoud voltooid op de virtuele machine. | 
      | Overgeslagen| U hebt geselecteerd om te initiëren onderhoud met niets succes. Niet mogelijk de onderhoudsoptie selfservice gebruiken. Uw virtuele machine moet opnieuw worden opgestart door Azure tijdens de fase gepland onderhoud. | 

   **Onderhoud proactief** -het tijdvenster ziet wanneer u onderhoud zelf op uw virtuele machines kunt starten.
   
   **Gepland onderhoud** -ziet u het tijdvenster wanneer Azure opnieuw wordt opgestart van uw virtuele machine om te kunnen voltooien onderhoud. 




## <a name="notification-and-alerts-in-the-portal"></a>Meldingen en waarschuwingen in de portal

Een planning voor gepland onderhoud communiceert Azure met een e-mailbericht verzenden naar de groep van eigenaar en mede-eigenaren abonnement. U kunt extra ontvangers en kanalen toevoegen aan deze communicatie door het maken van Azure activiteit logboek waarschuwingen. Zie voor meer informatie [Monitor abonnement activiteit met de Azure Activity Log] (... / articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu aan de linkerkant **Monitor**. 
3. In de **Monitor - activiteitenlogboek** deelvenster **waarschuwingen**.
4. In de **Monitor - waarschuwingen** deelvenster, klikt u op **+ toevoegen activiteit logboek waarschuwing**.
5. Vul de gegevens in de **toevoegen activiteit logboek waarschuwing** pagina en zorg ervoor dat u het volgende instellen in **Criteria**: **Type**: onderhoud **Status**: Alle (status niet ingesteld op actief of opgelost) **niveau**: alle
    
Zie voor meer informatie over het configureren van de activiteit logboek waarschuwingen, [logboek waarschuwingen voor de activiteit maken](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Onderhoudsmodus starten op de virtuele machine vanuit de portal

Terwijl u bekijkt de details van de virtuele machine, kunt u zich kunnen meer onderhoud gerelateerde informatie.  
Aan de bovenkant van de VM-detailweergave, worden een nieuwe melding lint toegevoegd als uw VM is opgenomen in een golf gepland onderhoud. Bovendien wordt een nieuwe optie toegevoegd aan het onderhoud indien mogelijk te starten. 


Klik op de melding onderhoud voor de onderhoudspagina met meer informatie over het geplande onderhoud. Daar kunt u zich kunt **start onderhoud** op de virtuele machine.

Zodra u onderhoud gaat uw virtuele machine opnieuw wordt opgestart en de onderhoudsstatus wordt bijgewerkt naar aanleiding van het resultaat binnen enkele minuten.

Als u het venster waar u onderhoud kan starten gemist, is het nog steeds mogelijk om het venster openen wanneer de virtuele machine opnieuw door Azure opgestart. 

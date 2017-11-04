## <a name="download-install-and-register-the-azure-backup-agent"></a>Downloaden, installeren en registreren van de Azure Backup agent
Nadat de Azure Backup-kluis is gemaakt, moet een agent worden geïnstalleerd op elk van uw Windows-machines (Windows Server, Windows-client, System Center Data Protection Manager-server of Azure Backup-Server-machine) waarmee een back-up van gegevens en toepassingen naar Azure.

1. Aanmelden bij de [-beheerportal](https://manage.windowsazure.com/)
2. Klik op **Recovery Services**, selecteert u de back-upkluis die u wilt registreren bij een server. De pagina snel starten voor deze back-upkluis wordt weergegeven.
   
    ![Snel starten](./media/backup-install-agent/quickstart.png)
3. Klik op de pagina snel starten op de **voor Windows Server, System Center Data Protection Manager of Windows client** onder de optie **Agent downloaden**. Klik op **opslaan** om deze te kopiëren naar de lokale computer.
   
    ![Opslaan van agent](./media/backup-install-agent/agent.png)
4. Nadat de agent is geïnstalleerd, dubbelklikt u op MARSAgentInstaller.exe voor het starten van de installatie van de Azure Backup agent. Kies de installatiemap en de tijdelijke map is vereist voor de agent. De opgegeven locatie van de cache moet vrije schijfruimte die ten minste 5% van de back-upgegevens bevatten.
5. Als u een proxyserver gebruiken om te verbinden met het internet, in de **proxyconfiguratie** scherm, voert u de gegevens van de proxy-server. Als u een geverifieerde proxyserver gebruikt, voert u de gegevens van de gebruiker en het wachtwoord in dit scherm.
6. De Azure Backup agent installeert u .NET Framework 4.5 en Windows PowerShell (indien deze nog niet beschikbaar is) om de installatie te voltooien.
7. Nadat de agent is geïnstalleerd, klikt u op de **doorgaan met de registratie** om door te gaan met de werkstroom.
   
   ![Registreren](./media/backup-install-agent/register.png)
8. In het scherm van de referenties kluis Blader naar en selecteer het kluisreferentiebestand die eerder zijn gedownload.
   
    ![Referenties voor de kluis](./media/backup-install-agent/vc.png)
   
    Het kluisreferentiebestand is alleen geldig voor 48 uur (nadat deze gedownload vanuit de portal). Als u treedt er een fout in dit scherm (bijvoorbeeld ' kluisreferentiebestand aangeboden is verlopen'), aanmelding bij de Azure portal en download het kluisreferentiebestand opnieuw.
   
    Zorg ervoor dat het kluisreferentiebestand beschikbaar is op een locatie die toegankelijk zijn voor het installatieprogramma. Als u toegang tot gerelateerde fouten het kluisreferentiebestand te kopiëren naar een tijdelijke locatie op deze machine en probeer het opnieuw.
   
    Als u een ongeldige kluis referentie foutbericht (bijvoorbeeld 'Ongeldige kluisreferenties die worden opgegeven') krijgt het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld met de service voor herstel. Probeer het opnieuw nadat het downloaden van een nieuw kluisreferentiebestand via de portal. Deze fout wordt doorgaans te zien wanneer de gebruiker op de **kluisreferentie downloaden** optie in de Azure-portal snel achter elkaar. In dit geval wordt is alleen het tweede kluisreferentiebestand geldig.
9. In de **versleutelingsinstelling** scherm kunt u een wachtwoordzin genereren of geef een wachtwoordzin (minimaal 16 tekens). Moet u de wachtwoordzin op een veilige locatie opslaan.
   
    ![Versleuteling](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Als u de wachtwoordzin kwijtraakt of vergeet; Microsoft kan niet helpen bij het herstellen van de back-upgegevens. De gebruiker eigenaar is van de wachtwoordzin voor versleuteling en Microsoft heeft geen zichtbaarheid van de wachtwoordzin die wordt gebruikt door de eindgebruiker. Sla het bestand op een veilige locatie als dit nodig tijdens een herstelbewerking wordt uitgevoerd is.
   > 
   > 
10. Nadat u op de **voltooien** knop, de machine naar de kluis is geregistreerd en u bent nu klaar om back-ups op Microsoft Azure.
11. Wanneer u zelfstandige Microsoft Azure Backup kunt u de instellingen die zijn opgegeven tijdens de registratiewerkstroom door te klikken op de **eigenschappen wijzigen** optie in de Azure Backup mmc-modules in.
    
    ![Eigenschappen wijzigen](./media/backup-install-agent/change.png)
    
    Als u Data Protection Manager, kunt u ook de instellingen die zijn opgegeven tijdens de registratiewerkstroom door te klikken op wijzigen de **configureren** optie door te selecteren **Online** onder de **Management** tabblad.
    
    ![Azure Backup configureren](./media/backup-install-agent/configure.png)


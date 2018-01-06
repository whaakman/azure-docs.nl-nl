## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Met behulp van referenties voor de kluis te verifiëren met de Azure Backup-service
Voordat u kunt een back-up van een on-premises server (Windows-client of Windows Server of Data Protection Manager-server) maken naar Azure, de server geverifieerd met een Recovery Services-kluis. Gebruik een kluisreferentiebestand om de server met Azure te verifiëren. Referenties voor de kluis zijn vergelijkbaar met het concept van een ' instellingen '-bestand publiceren in Azure PowerShell gebruikt.

### <a name="what-is-the-vault-credential-file"></a>Wat is het kluisreferentiebestand?
Het kluisreferentiebestand is een certificaat dat de portal voor elke back-upkluis genereert. De portal uploadt de openbare sleutel vervolgens naar de Access Control Service (ACS). De persoonlijke sleutel van het certificaat wordt beschikbaar gesteld aan de gebruiker als onderdeel van de werkstroom die is opgegeven als invoer in de werkstroom van de registratie machine. Hiermee verifieert de machine voor het verzenden van back-upgegevens naar een geïdentificeerde kluis in de Azure Backup-service.

De kluisreferenties worden alleen gebruikt tijdens de registratiewerkstroom. Het is de verantwoordelijkheid van de gebruiker om ervoor te zorgen dat het kluisreferentiebestand niet worden gecompromitteerd. Als het kluisreferentiebestand in handen valt van een rogue-gebruiker, kan het bestand worden gebruikt om andere machines bij dezelfde kluis te registreren. Als de back-upgegevens worden versleuteld met een wachtwoordzin die deel uitmaakt van de klant, kan niet u bestaande back-upgegevens geschonden. Om dit probleem verhelpen, de kluisreferenties verlopen na 48hrs. U kunt de kluisreferenties van een back-upkluis downloaden vaak – maar alleen het meest recente kluisreferentiebestand is van toepassing tijdens de registratiewerkstroom.

### <a name="download-the-vault-credential-file"></a>Download het kluisreferentiebestand
Het kluisreferentiebestand is gedownload via een beveiligd kanaal vanuit de Azure-portal. De Azure Backup-service is niet op de hoogte van de persoonlijke sleutel van het certificaat en de persoonlijke sleutel is niet permanent in de portal of de service. Gebruik de volgende stappen voor het downloaden van het kluisreferentiebestand naar een lokale computer.

1. Open de [Azure-Portal](https://ms.azure.portal.com/)
2. Selecteer op het menu links **alle services** en typt u in de lijst met services **Recovery Services**. Klik op **Recovery Services-kluizen**.

   ![Recovery Services-kluis openen](../articles/backup/media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)
3. Klik op de pagina Quick Start op **kluisreferenties downloaden**. De portal genereert het kluisreferentiebestand wordt beschikbaar gesteld voor downloaden.
   
   ![Downloaden](./media/backup-download-credentials/downloadvc.png)
4. De portal genereert een kluisreferentie met een combinatie van de kluisnaam van de en de huidige datum. Klik op **opslaan** de kluisreferenties downloaden naar de map downloads van het lokale account in of selecteer OpslaanAls in het menu opslaan naar een locatie opgeven voor de kluisreferenties.

### <a name="note"></a>Opmerking
* Zorg ervoor dat de referenties voor de kluis wordt opgeslagen op een locatie die toegankelijk is vanaf uw computer. Als deze is opgeslagen in een bestand share/SMB, controleert de toegangsmachtigingen.
* Het kluisreferentiebestand wordt alleen gebruikt tijdens de registratiewerkstroom.
* Het kluisreferentiebestand na 48hrs verlopen en kan worden gedownload vanuit de portal.
* Raadpleeg de Azure Backup [Veelgestelde vragen over](../articles/backup/backup-azure-backup-faq.md) voor vragen over de werkstroom.


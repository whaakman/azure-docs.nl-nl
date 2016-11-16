## <a name="overview"></a>Overzicht
Azure File Storage is een service die bestandsshares in de cloud aanbiedt met behulp van het standaard [SMB-protocol (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx). Zowel SMB 2.1 als SMB 3.0 wordt ondersteund. Met Azure File Storage kunt u oudere toepassingen die afhankelijk zijn van bestandsshares, snel en zonder kostbare regeneraties naar Azure migreren. Toepassingen die worden uitgevoerd op virtuele machines of in cloudservices van Azure, of op on-premises clients, kunnen een bestandsshare in de cloud koppelen, op dezelfde manier zoals een bureaubladtoepassing een gewone SMB-share koppelt. Daarna kan een willekeurig aantal toepassingsonderdelen de File Storage-share tegelijkertijd koppelen en gebruiken.

Omdat een File Storage-share een standaard SMB-bestandsshare is, hebben toepassingen die in Azure worden uitgevoerd toegang tot de gegevens in de share via I/O API's van het bestandssysteem. Dat betekent dat ontwikkelaars bestaande code en vaardigheden kunnen inzetten voor het migreren van bestaande toepassingen. IT-professionals kunnen PowerShell-cmdlets gebruiken om File Storage-shares te maken, te koppelen en te beheren als onderdeel van het beheer van Azure-toepassingen.

U kunt Azure-bestandsshares maken met [Azure Portal](https://portal.azure.com), de PowerShell-cmdlets van Azure Storage, de clientbibliotheken van Azure Storage of de REST API van Azure Storage. En omdat deze bestandsshares SMB-shares zijn, kunt u ze openen via standaard- en bekende API's van het bestandssysteem.



<!--HONumber=Nov16_HO2-->



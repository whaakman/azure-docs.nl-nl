---
title: Azure beveiligen persoonlijke gegevens in rust met versleuteling | Microsoft Docs
description: In dit artikel maakt deel uit van een reeks helpt u Azure gebruiken om persoonlijke gegevens te beveiligen
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 31e41f9befd9319115e5d147b473756486100c6e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Azure versleutelingstechnologieën: beveiligen van persoonlijke gegevens in rust met versleuteling

In dit artikel helpt u begrijpen en gebruiken van Azure versleutelingstechnologieën om gegevens in rust te beveiligen.

Versleuteling van gegevens in rust is essentieel als een best practice om gevoelige of persoonlijke gegevens te beveiligen en om te voldoen aan nalevings- en privacy-vereisten.
Codering in rust is ontworpen om te voorkomen dat de aanvaller toegang hebben tot de niet-versleutelde gegevens doordat de gegevens worden versleuteld op de schijf.

## <a name="scenario"></a>Scenario 

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse en Baltische veiligheid, evenals Florida. Ter ondersteuning van deze inspanningen is die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk

Het bedrijf gebruikmaakt van Microsoft Azure voor het opslaan van bedrijfsgegevens in de cloud. Dit kan de klant en/of de werknemer informatie zoals omvatten:

- adressen
- telefoonnummers
- BTW-id 's
- creditcardgegevens

Het bedrijf moet de privacy van klanten en werknemers gegevens beveiligen tijdens het maken van gegevens toegankelijk voor deze diensten die u nodig hebt. (zoals salarissen en -reserveringen afdelingen)

De regel cruise onderhoudt ook een grote database van derden en loyaliteit programma leden met persoonlijke gegevens voor de relaties met de huidige en eerdere klanten bijhouden.

### <a name="problem-statement"></a>Probleemformulering

Het bedrijf moet de privacy van klanten en werknemers persoonlijke gegevens beveiligen tijdens het maken van gegevens toegankelijk voor deze diensten die u nodig hebt (zoals salarissen en -reserveringen afdelingen). Deze persoonlijke gegevens wordt opgeslagen buiten het bedrijf beheerd datacenter en bevindt zich niet onder de fysieke controle van het bedrijf.

### <a name="company-goal"></a>Bedrijf-doel

Als onderdeel van een strategie voor een meerlaagse beveiliging van defense-in-depth is het een bedrijf doel om ervoor te zorgen dat alle gegevensbronnen die persoonlijke gegevens bevatten zijn versleuteld, met inbegrip van die zich in de cloud-opslag. Als niet-geautoriseerde personen toegang tot de persoonlijke gegevens krijgen, moet deze in een formulier dat het onleesbaar verschijnt. Versleuteling toepassen, moet eenvoudig of transparante – voor gebruikers en beheerders.

## <a name="solutions"></a>Oplossingen

Azure-services bieden meerdere hulpprogramma's en technologieën voor het beveiligen van persoonlijke gegevens in rust door deze te coderen.

### <a name="azure-key-vault"></a>Azure Key Vault

[Azure Sleutelkluis](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) biedt een veilige opslag voor de sleutels die worden gebruikt voor het versleutelen van gegevens in rust in de Azure-services en is de aanbevolen oplossing voor het opslaan en beheren van sleutels. Sleutelbeheer versleuteling is essentieel voor het beveiligen van opgeslagen gegevens.

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a>Hoe gebruik ik Azure Key Vault voor het beveiligen sleutels die persoonlijke gegevens versleutelen

Voor het gebruik van Azure Sleutelkluis, moet u een abonnement op een Azure-account. U moet ook Azure PowerShell is geïnstalleerd. Stappen omvatten het volgende doen met behulp van PowerShell-cmdlets:

1. Verbinding maken met uw abonnementen

2. Een sleutelkluis maken

3. Een sleutel of geheim toevoegen aan de sleutelkluis

4. Toepassingen die van de sleutelkluis met Azure Active Directory gebruikmaken registreren

5. De toepassingen met de sleutel of geheim autoriseren

Gebruik de PowerShell-CmDlt New-AzureRmKeyVault voor het maken van een sleutelkluis. U kunt een kluisnaam, de naam van een resourcegroep en de geografische locatie wordt toegewezen. U gebruikt de kluisnaam van de bij het beheren van sleutels via andere Cmdlets. Toepassingen die gebruikmaken van de kluis via de REST-API wordt de kluis URI gebruiken.

Azure Sleutelkluis een softwarematig beveiligde sleutel voor u kunt opgeven of u kunt een bestaande sleutel in importeren een. PFX-bestand. U kunt ook geheimen (wachtwoorden) opslaan in de kluis.

U kunt ook een sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat.

Voor gedetailleerde instructies over het gebruik van Azure Sleutelkluis, volg de stappen in [aan de slag met Azure Sleutelkluis.](https://docs.microsoft.com/azure/key-vault/key-vault-get-started)

Zie voor een lijst van PowerShell-Cmdlets gebruikt met Azure Key Vault [AzureRM.KeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>Azure Disk Encryption voor Windows

[Azure Disk Encryption for Windows en Linux IaaS VM's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) beschermt persoonlijke gegevens in rust op Azure virtuele machines en kan worden geïntegreerd met Azure Sleutelkluis. Maakt gebruik van Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows en [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux voor het versleutelen van zowel het besturingssysteem en de gegevensschijven. Azure Disk Encryption wordt ondersteund op Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 en op Windows 8 en Windows 10-clients.

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a>Hoe gebruik ik Azure Disk Encryption persoonlijke gegevens te beveiligen?

Voor het gebruik van Azure Disk Encryption, moet u een abonnement op een Azure-account. Schakel Azure Disk Encryption for Windows en Linux-virtuele machines door het volgende doen:

1. Gebruik de Azure Disk Encryption Resource Manager-sjabloon, PowerShell of de opdrachtregelinterface (CLI) schijfversleuteling inschakelen en geef de configuratie van versleuteling. 

2. Toegang verlenen tot de Azure-platform om te lezen van het materiaal versleuteling van de sleutelkluis.

3. Geef een toepassings-id van de Azure Active Directory (AAD) voor het schrijven van het sleutelmateriaal versleuteling aan de sleutelkluis.

Azure bijwerken van de virtuele machine en de configuratie van de sleutelkluis en de versleutelde virtuele machine instellen.

Wanneer u de sleutelkluis ter ondersteuning van Azure Disk Encryption instelt, kunt u een sleutel versleutelingssleutel (KEK-sleutel) kunt toevoegen voor extra beveiliging en ter ondersteuning van back-up van de versleutelde virtuele machines.

![](media/protect-personal-data-at-rest/create-key.png)

Gedetailleerde instructies voor het specifieke implementatiescenario's beschreven en gebruikerservaringen zijn opgenomen in [Azure Disk Encryption for Windows en Linux IaaS VM's.](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

### <a name="azure-storage-service-encryption"></a>Azure Storage Service-versleuteling

[Azure Storage Service versleuteling (SSE) voor gegevens in rust](https://docs.microsoft.com/azure/storage/storage-service-encryption) helpt u beveiligt en bescherming van uw gegevens om te voldoen aan uw organisatie beveiliging en naleving verplichtingen. Azure Storage automatisch versleutelt uw gegevens met behulp van 256-bits AES-versleuteling vóór behouden blijven van opslag en ontsleuteld voordat ophalen. Deze service is beschikbaar voor Azure Blobs en bestanden.

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a>Hoe gebruik versleuteling van de opslagruimte persoonlijke gegevens te beveiligen?

Om in te schakelen versleuteling van de opslagruimte, het volgende doen:

1. Meld u aan bij de Azure-portal.

2. Selecteer een opslagaccount.

3. Selecteer in de instellingen onder de sectie Blob-Service versleuteling.

4. Selecteer onder de sectie File Service versleuteling.

Nadat u de instelling voor wachtwoordversleuteling op, kunt u in- of uitschakelen van de versleuteling van de opslagruimte.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Nieuwe gegevens worden versleuteld. Gegevens in de bestaande bestanden in dit opslagaccount wordt onversleuteld blijven.

Nadat de codering is ingeschakeld, moet u gegevens kopiëren naar het storage-account met behulp van een van de volgende methoden:

1. Kopieer blobs of bestanden met de [AzCopy-opdrachtregelprogramma](https://docs.microsoft.com/azure/storage/storage-use-azcopy).

2. [Een gebruik van SMB-bestandsshare koppelen](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-windows) zodat u een hulpprogramma zoals Robocopy gebruiken kunt om bestanden te kopiëren.

3. Blob of bestand gegevens kopiëren naar en van blob-opslag of tussen opslagaccounts met [Opslagclientbibliotheken zoals .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs).

4.  Gebruik een [Opslagverkenner](https://docs.microsoft.com/azure/storage/storage-explorers) BLOB's uploaden naar uw storage-account met versleuteling ingeschakeld.

### <a name="transparent-data-encryption"></a>Transparante gegevensversleuteling

Transparent Data Encryption (TDE) is een functie in SQL Azure waarmee u gegevens op de database en de server niveaus kunt versleutelen. TDE is nu standaard ingeschakeld op alle nieuwe databases. TDE voert realtime i/o-versleuteling en ontsleuteling van de gegevens en logboekbestanden.

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a>Hoe gebruik TDE persoonlijke gegevens te beveiligen?

U kunt met behulp van de REST-API of met behulp van PowerShell TDE configureren via de Azure portal. Om in te schakelen TDE op een bestaande database met de Azure Portal, het volgende doen:

1. Ga naar de Azure portal op <https://portal.azure.com> en meld u met uw Azure-beheerder of bijdrager-account.

2. Op de koptekst links, klikt u op om te bladeren en klik op SQL-databases.

3. Met SQL-databases die zijn geselecteerd in het linkerdeelvenster klikt u op de gebruikersdatabase.

4. Klik in de databaseblade alle instellingen.

5. Klik in de blade instellingen op Transparent data encryption deel om het Transparent data encryption blade te openen.

6. De versleutelingsknop gegevens te verplaatsen op de blade Data encryption en klik op opslaan (boven aan de pagina) om toe te passen van de instelling. De coderingsstatus wordt bij benadering de voortgang van de transparante gegevensversleuteling.

![Inschakelen van gegevensversleuteling](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

Instructies over het inschakelen van TDE en informatie over het decoderen van TDE beveiligde databases en meer kunnen worden gevonden in het artikel [Transparent Data Encryption met Azure SQL Database.](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)

## <a name="summary"></a>Samenvatting

Het bedrijf kunt doen doelstelling van het versleutelen van persoonlijke gegevens die zijn opgeslagen in de Azure-cloud. Ze kunnen dit doen met behulp van Azure Disk Encryption complete volumes beveiligen. Dit kan ook de bestanden van besturingssysteem en de gegevensbestanden die persoonlijk gegevens en andere gevoelige gegevens bevatten. Versleuteling van Azure Storage-Service kan worden gebruikt om persoonlijke gegevens die zijn opgeslagen in blobs en bestanden te beveiligen. Voor gegevens die zijn opgeslagen in Azure SQL-databases, beschermt met transparante gegevensversleuteling van niet-geautoriseerde blootstelling van persoonlijke gegevens.

Ter bescherming van de sleutels die worden gebruikt voor het versleutelen van gegevens in Azure, wordt de Azure Sleutelkluis in het bedrijf kan gebruiken. Dit stroomlijnt het beheerproces en maakt het bedrijf controle houdt over sleutels die toegang tot en persoonlijke gegevens te versleutelen.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor probleemoplossing voor Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-tsg)

- [Een virtuele Machine van Azure versleutelen](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Versleuteling van gegevens in Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-encryption)

- [Versleuteling van Azure DB Cosmos-database in rust](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)

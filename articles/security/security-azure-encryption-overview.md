---
title: Overzicht van Azure gegevensversleuteling | Microsoft Docs
description: Meer informatie over verschillende versleutelingsopties voor in Azure
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: barclayn
ms.openlocfilehash: dc1ca62ce184ac290f289975ff609b8240351099
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035093"
---
# <a name="azure-encryption-overview"></a>Overzicht van Azure-codering

Dit artikel bevat een overzicht van hoe versleuteling wordt gebruikt in Microsoft Azure. Hierin worden de belangrijkste gedeelten van versleuteling, met inbegrip van versleuteling-at-rest, codering in vertragingen van vluchten en sleutelbeheer met Azure Key Vault. Elke sectie bevat koppelingen naar meer gedetailleerde informatie.

## <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest

Data-at-rest, bevat gegevens die zich in een permanente opslag op de fysieke media, in elke digitale indeling bevindt. Het medium kan bestanden op het magnetische of optische media, gearchiveerde gegevens en back-ups van gegevens bevatten. Microsoft Azure biedt tal van oplossingen voor gegevensopslag te voldoen aan verschillende vereisten, met inbegrip van bestands-, schijf-, blob- en tabelopslag. Microsoft biedt ook versleuteling om te beveiligen [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [Azure Cosmos DB](../cosmos-db/introduction.md), en Azure Data Lake.

Gegevensversleuteling in rust is beschikbaar voor services in de software als een service (SaaS), platform as a service (PaaS) en de infrastructuur als een cloud service (IaaS) modellen. In dit artikel bevat een overzicht van en vindt u bronnen waarmee u kunt de versleutelingsopties voor Azure-gebruiken.

Zie voor meer gedetailleerde informatie over hoe de gegevens in rust worden versleuteld in Azure, [Azure Data Encryption-at-Rest](azure-security-encryption-atrest.md).

## <a name="azure-encryption-models"></a>Azure-codering modellen

Azure ondersteunt verschillende versleuteling modellen, met inbegrip van server-side-versleuteling die gebruikmaakt van service beheerde sleutels, de klant beheerde sleutels in Key Vault of de klant beheerde sleutels op de klant beheerde hardware. Met client-side encryption kunt u beheren en opslaan van sleutels on-premises of in een andere veilige locatie.

### <a name="client-side-encryption"></a>Clientversleuteling

Client-side-versleuteling wordt uitgevoerd buiten Azure. Het bevat:

- De gegevens versleuteld door een toepassing die wordt uitgevoerd in het datacenter van de klant of door een servicetoepassing.
- Gegevens die al is versleuteld wanneer deze wordt ontvangen door Azure.

Met client-side encryption cloudserviceproviders geen toegang hebt tot de versleutelingssleutels en kunnen deze gegevens niet ontsleutelen. U behoudt de volledige controle van de sleutels.

### <a name="server-side-encryption"></a>Versleuteling aan de serverzijde

De drie modellen van versleuteling op de server bieden verschillende Sleutelbeheer kenmerken, die u op basis van uw vereisten kiezen kunt:

- **Service beheerde sleutels**: biedt een combinatie van controle en het gemak met weinig overhead.

- **Door de klant beheerde sleutels**: hebt u controle over de sleutels, inclusief ondersteuning voor Bring Your Own sleutels (BYOK), of kunt u nieuwe labels te genereren.

- **Service beheerde sleutels in de klant beheerde hardware**: Hiermee kunt u voor het beheren van sleutels in uw eigen opslagplaats buiten Microsoft-besturingselement. Dit kenmerk wordt Host Your Own Key (HYOK) genoemd. Configuratie is echter complexe, en de meeste Azure-services bieden geen ondersteuning voor dit model.

### <a name="azure-disk-encryption"></a>Azure disk encryption

U kunt Windows en Linux-machines beveiligen met behulp van [Azure disk encryption](azure-security-disk-encryption.md), die gebruikmaakt [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technologie- en Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) beide beveiligen besturingssysteemschijven en gegevensschijven met volledige versleuteling voor volumes.

Versleutelingssleutels en geheimen worden beveiligd in uw [Azure Key Vault-abonnement](../key-vault/key-vault-whatis.md). Met behulp van de Azure Backup-service, kunt u back-up en herstel van versleutelde virtuele machines (VM's) die gebruikmaken van versleuteling (KEK-Key Key)-configuratie.

### <a name="azure-storage-service-encryption"></a>Azure Storage Service-versleuteling

Data-at-rest in Azure Blob storage en Azure-bestandsshares kunnen worden versleuteld in zowel server- en client-side '-scenario's.

[Azure Storage Service Encryption (SSE)](../storage/common/storage-service-encryption.md) kunt u automatisch gegevens versleutelen voordat deze zijn opgeslagen en deze automatisch de gegevens ontsleuteld wanneer u deze ophalen. Het proces is volledig transparant voor gebruikers. Storage-Serviceversleuteling maakt gebruik van 256-bits [Advanced Encryption Standard (AES)-codering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), dit is een van de krachtigste blokversleutelingsmethoden die. AES verwerkt transparant versleuteling, ontsleuteling en sleutelbeheer.

### <a name="client-side-encryption-of-azure-blobs"></a>Versleuteling aan de clientzijde van Azure-blobs

U kunt aan de clientzijde uitvoeren versleuteling van Azure-blobs op verschillende manieren.

U kunt de Azure Storage-clientbibliotheek voor .NET NuGet-pakket gebruiken om gegevens te versleutelen binnen uw clienttoepassingen voordat ze worden geüpload naar uw Azure-opslag.

Zie voor meer informatie over en downloaden van de Azure Storage-clientbibliotheek voor .NET NuGet-pakket, [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

Wanneer u versleuteling op de client met Key Vault gebruikt, wordt uw gegevens versleuteld met behulp van een eenmalige symmetrische inhoud versleuteling sleutel (CEK) die wordt gegenereerd door de client-SDK van Azure Storage. De CEK is versleuteld met behulp van een sleutel versleuteling (KEK-Key), wat een symmetrische sleutel of een asymmetrisch sleutelpaar kan zijn. U kunt lokaal beheren of opslaan in Key Vault. De versleutelde gegevens is vervolgens naar Azure Storage geüpload.

Zie voor meer informatie over versleuteling van de client-side met Key Vault en aan de slag met praktische instructies, [zelfstudie: blobs versleutelen en ontsleutelen in Azure Storage met behulp van Key Vault](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Ten slotte kunt u ook de Azure Storage-clientbibliotheek voor Java gebruiken om uit te voeren clientversleuteling voordat u gegevens uploadt naar Azure Storage en de gegevens ontsleutelen wanneer u deze downloaden op de client. Deze bibliotheek biedt ook ondersteuning voor integratie met [Key Vault](https://azure.microsoft.com/services/key-vault/) voor sleutelbeheer voor storage-account.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Versleuteling van data-at-rest met Azure SQL Database

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) is een algemene relationele databaseservice in Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON, ruimtelijke gegevens en XML. SQL Database ondersteunt zowel versleuteling op de server via de functie transparante gegevensversleuteling (TDE) en client-side-codering via de functie altijd versleuteld.

#### <a name="transparent-data-encryption"></a>Transparante gegevensversleuteling

[TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) wordt gebruikt voor het versleutelen van [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), en [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) gegevensbestanden in realtime, met behulp van een Database Gegevensversleutelingsleutel (DEK) , die is opgeslagen in de database-bootrecord voor beschikbaarheid tijdens het herstel.

TDE beschermt gegevens en logboekbestanden bestanden, met behulp van AES en Triple Data Encryption Standard (3DES) versleutelingsalgoritmen. Versleuteling van het databasebestand wordt uitgevoerd op het paginaniveau van de. De pagina's in een versleutelde database worden versleuteld voordat ze worden geschreven naar schijf en worden ontsleuteld wanneer ze worden gelezen in het geheugen. TDE is nu ingeschakeld standaard op de zojuist gemaakte Azure SQL-databases.

#### <a name="always-encrypted-feature"></a>Altijd versleutelde functie

Met de [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) functie in Azure SQL kunt u coderen van gegevens binnen clienttoepassingen voordat het in Azure SQL Database op te slaan. U kunt ook overdracht van beheer van on-premises database aan derden inschakelen en onderhouden van de scheiding tussen degenen die de eigenaar en kunnen de gegevens bekijken en degenen die deze beheren, maar mag geen toegang toe hebben.

#### <a name="cell-level-or-column-level-encryption"></a>Niveau van de cel of kolom versleuteling

Met Azure SQL Database, kunt u symmetrische codering toepassen op een kolom met gegevens met behulp van Transact-SQL. Deze methode wordt aangeroepen [celniveau versleuteling of versleuteling op kolomniveau (CLE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data), omdat u deze gebruiken kunt voor het versleutelen van specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels. In dat geval kunt u meer gedetailleerde versleutelingsfunctie die wordt dan TDE, waarmee gegevens op pagina's worden gecodeerd.

CLE heeft ingebouwde functies die u gebruiken kunt om gegevens te versleutelen met behulp van zowel symmetrisch als asymmetrisch sleutels, de openbare sleutel van een certificaat of een wachtwoordzin met 3DES.

### <a name="cosmos-db-database-encryption"></a>Versleuteling van de cosmos DB-database

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) is de wereldwijd gedistribueerde, multi-model database van Microsoft. Gebruikersgegevens die zijn opgeslagen in Cosmos DB in niet-vluchtige opslag (solid-state drives) is standaard versleuteld. Er zijn geen besturingselementen om te schakelen in- of uitschakelen. Versleuteling-at-rest wordt geïmplementeerd met behulp van diverse beveiligingstechnologieën, met inbegrip van beveiligde opslag van clustersleutels systemen, versleutelde netwerken en cryptografische API's. Versleutelingssleutels worden beheerd door Microsoft en worden gedraaid per interne richtlijnen van Microsoft.

### <a name="at-rest-encryption-in-data-lake"></a>Versleuteling AT-rest in Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) is een bedrijfsbrede opslagruimte voor elk soort gegevens verzameld op één plek voordat een officiële definitie van vereisten of schema. Data Lake Store ondersteunt "op standaard" transparante versleuteling van data-at-rest, die tijdens het maken van uw account is ingesteld. Azure Data Lake Store beheert de sleutels voor u, maar u hebt de mogelijkheid zelf beheren.

Drie soorten sleutels worden gebruikt in versleutelen en ontsleutelen van gegevens: de Hoofdversleutelingssleutel (MEK), Data Gegevensversleutelingsleutel (DEK) en -sleutel voor blok-versleuteling (BEK). De MEK wordt gebruikt voor het versleutelen van de DEK, die is opgeslagen op permanente media, en de BEK wordt afgeleid van de DEK en het gegevensblok. Als u uw eigen sleutels beheren wilt, kunt u de MEK rouleren.

## <a name="encryption-of-data-in-transit"></a>Versleuteling van gegevens die onderweg zijn

Azure biedt veel mechanismen voor het bewaren van gegevens privé tijdens deze verplaatsing van de ene locatie naar een andere.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL-versleuteling in Azure

Microsoft gebruikt de [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) protocol (TLS) om gegevens te beveiligen wanneer deze wordt onderweg tussen de cloudservices en uw klanten. Microsoft-datacenters onderhandelen over de TLS-verbinding met clientsystemen die verbinding met Azure-services maken. TLS biedt sterke verificatie, privacy van berichten en integriteit (waardoor de detectie van bericht is geknoeid, worden onderschept en kunnen worden vervalst), interoperabiliteit, algoritmeflexibiliteit en eenvoudige implementatie en het gebruik.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) beveiligt verbindingen tussen de clientsystemen en Microsoft cloud-services klanten met unieke sleutels. Verbindingen worden ook sleutellengten op basis van een RSA 2048-bits versleuteling gebruiken. Deze combinatie is het moeilijk voor iemand om intercept en toegang tot gegevens die onderweg zijn.

### <a name="azure-storage-transactions"></a>Azure Storage-transacties

Als u een interactie met Azure Storage via Azure portal, hebben alle transacties plaatsvinden via HTTPS. U kunt ook de REST-API voor Storage via HTTPS gebruiken om te communiceren met Azure Storage. U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in de storage-accounts doordat de veilige overdracht die zijn voor het opslagaccount vereist.

Shared Access Signatures ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), die kan worden gebruikt voor toegang tot Azure Storage-objecten overdragen, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruiken van handtekeningen voor gedeelde toegang. Deze aanpak zorgt ervoor dat iedereen die koppelingen met SAS-tokens verzendt het juiste protocol wordt gebruikt.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), die gebruikt voor toegang tot Azure Files-shares, ondersteunt versleuteling en deze is beschikbaar in Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10. Deze regio-overschrijdende toegang en zelfs toegang krijgen tot op het bureaublad.

Clientversleuteling versleutelt de gegevens voordat deze wordt verzonden naar uw Azure Storage-exemplaar, zodat ze zijn versleuteld wanneer deze via het netwerk wordt verzonden.

### <a name="smb-encryption-over-azure-virtual-networks"></a>SMB-versleuteling op virtuele Azure-netwerken 

Met behulp van [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) in virtuele machines die met WindowsServer 2012 of hoger, kunt u gegevens overdrachten beveiligen door het versleutelen van gegevens die onderweg zijn via Azure Virtual Networks. Door het versleutelen van gegevens, kunt u helpen beschermen tegen knoeien en niet kan worden afgeluisterd aanvallen. Beheerders kunnen SMB-versleuteling voor de hele server of alleen bepaalde shares inschakelen.

Standaard dat nadat het SMB-versleuteling is ingeschakeld voor een share of een server, alleen SMB 3.0-clients zijn toegestaan voor toegang tot de versleutelde shares.

## <a name="in-transit-encryption-in-vms"></a>In-transit codering in virtuele machines

Gegevens die onderweg zijn naar, van, en tussen virtuele machines waarop Windows wordt versleuteld in een aantal manieren, afhankelijk van de aard van de verbinding.

### <a name="rdp-sessions"></a>RDP-sessies

U kunt verbinding maken met en aanmelden bij een virtuele machine met behulp van de [Remote Desktop Protocol (RDP)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) vanaf een Windows-clientcomputer of vanaf een Mac met een RDP-client is geïnstalleerd. Gegevens die onderweg zijn via het netwerk in het RDP-sessies kunnen worden beveiligd door TLS.

U kunt ook extern bureaublad verbinding maken met een Linux-VM in Azure.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Beveiligde toegang tot virtuele Linux-machines met SSH

Voor extern beheer, kunt u [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) verbinding maken met virtuele Linux-machines uitvoeren in Azure. SSH is een versleutelde verbinding-protocol waarmee veilige aanmeldingen via niet-beveiligde verbindingen. Het is het standaardprotocol voor de verbinding voor virtuele Linux-machines die worden gehost in Azure. Met behulp van SSH-sleutels voor verificatie, elimineert u de noodzaak van wachtwoorden aan te melden bij. SSH maakt gebruik van een openbaar/persoonlijk sleutelpaar (asymmetrische codering) voor verificatie.

## <a name="azure-vpn-encryption"></a>Azure VPN-versleuteling

U kunt verbinding maken met Azure via een virtueel particulier netwerk die een beveiligde tunnel ter bescherming van de privacy van de gegevens die worden verzonden via het netwerk worden gemaakt.

### <a name="azure-vpn-gateways"></a>Azure VPN-gateways

U kunt een [Azure VPN-gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) voor het verzenden van versleuteld verkeer tussen uw virtuele netwerk en uw on-premises locatie via een openbare verbinding, of om verkeer tussen virtuele netwerken te verzenden.

Site-naar-site VPN-verbindingen gebruiken [IPsec](https://en.wikipedia.org/wiki/IPsec) voor transportversleuteling. Azure VPN-gateways maken gebruik van een reeks standaard voorstellen. U kunt configureren dat Azure VPN-gateways voor het gebruik van een aangepast IPsec/IKE-beleid met specifieke cryptografische algoritmen en belangrijkste sterke punten in plaats van het standaardbeleid voor Azure wordt ingesteld.

### <a name="point-to-site-vpns"></a>Punt-naar-site VPN-verbindingen

Punt-naar-site VPN-verbindingen individuele client computers toegang geven tot een Azure-netwerk. [Het Secure Socket Tunneling Protocol (SSTP)](https://technet.microsoft.com/library/2007.06.cableguy.aspx) wordt gebruikt om de VPN-tunnel te maken. Het kan (de tunnel wordt weergegeven als een HTTPS-verbinding) firewalls passeren. U kunt uw eigen interne openbare-sleutelinfrastructuur (PKI)-basiscertificeringsinstantie (CA) gebruiken voor punt-naar-site-connectiviteit.

U kunt een punt-naar-site VPN-verbinding met een virtueel netwerk configureren met behulp van de Azure-portal met verificatie via certificaat of PowerShell.

Zie voor meer informatie over punt-naar-site VPN-verbindingen met virtuele Azure-netwerken:

[Een punt-naar-site-verbinding met een virtueel netwerk configureren met behulp van certificaatverificatie: Azure portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Een punt-naar-site-verbinding met een virtueel netwerk configureren met behulp van verificatie via certificaat: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>Site-naar-site VPN-verbindingen 

Een site-naar-site VPN-gatewayverbinding kunt u uw on-premises netwerk verbinden met een Azure-netwerk via een IPsec/IKE (IKEv1 of IKEv2) VPN-tunnel. Dit type verbinding vereist een on-premises VPN-apparaat met een extern gerichte openbare IP-adres is toegewezen.

U kunt een site-naar-site VPN-verbinding met een virtueel netwerk configureren met behulp van de Azure portal, PowerShell of Azure CLI.

Zie voor meer informatie:

[Een site-naar-site-verbinding maken in Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Een site-naar-site-verbinding maken in PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Een virtueel netwerk met een site-naar-site VPN-verbinding maken met behulp van CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>In-transit versleuteling in Data Lake

Gegevens tijdens overdracht (ook wel gegevens in beweging genoemd) worden ook altijd versleuteld in Data Lake Store. Naast het versleutelen van gegevens voorafgaand aan het in permanente media op te slaan, de gegevens ook altijd beveiligd tijdens de overdracht met behulp van HTTPS. HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Store REST-interfaces.

Zie voor meer informatie over versleuteling van gegevens die onderweg zijn in Data Lake, [versleuteling van gegevens in Data Lake Store](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-key-vault"></a>Sleutelbeheer met Key Vault

Zonder de juiste beveiliging en beheer van de sleutels, wordt het weergegeven van versleuteling niet kan worden gebruikt. Key Vault is de aanbevolen Microsoft oplossing voor het beheren en beheren van toegang tot de versleutelingssleutels die zijn gebruikt door cloudservices. Machtigingen voor toegang tot sleutels kunnen worden toegewezen aan de services of aan gebruikers via Azure Active Directory-accounts.

Key Vault de hoge kosten organisaties van de noodzaak om te configureren, patchen en onderhouden van hardware security modules (HSM's) en sleutelbeheersoftware. Wanneer u Key Vault gebruikt, houden u kunt controle. Microsoft uw sleutels nooit ziet en toepassingen niet direct de toegang tot hebben. U kunt ook sleutels importeren of genereren in HSM's.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure-beveiliging](security-get-started-overview.md)
- [Overzicht van de beveiliging van Azure-netwerk](security-network-overview.md)
- [Overzicht van de beveiliging van Azure-database](azure-database-security-overview.md)
- [Overzicht van de beveiliging van virtuele machines van Azure](security-virtual-machines-overview.md)
- [Versleuteling van inactieve gegevens](azure-security-encryption-atrest.md)
- [Aanbevolen procedures voor gegevensbeveiliging en -versleuteling](azure-security-data-encryption-best-practices.md)

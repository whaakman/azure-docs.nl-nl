---
title: Overzicht van Azure versleuteling | Microsoft Docs
description: Meer informatie over verschillende opties voor codering in Azure
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
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 00c8b30b5351b7a6e4388b186fab70e3a3357ef4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366304"
---
# <a name="azure-encryption-overview"></a>Overzicht van Azure-versleuteling

Dit artikel bevat een overzicht van hoe de versleuteling wordt gebruikt in Microsoft Azure. Deze heeft de belangrijke gebieden van versleuteling, zoals versleuteling in rust, versleuteling in vlucht en sleutelbeheer met Azure Sleutelkluis. Elke sectie bevat koppelingen naar meer gedetailleerde informatie.

## <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest

Gegevens in rust bevat informatie die zich in de permanente opslag op de fysieke media, in een willekeurige digitale indeling bevindt. Het medium kan bestanden op magnetische of optische media, gearchiveerde gegevens en gegevens back-ups bevatten. Microsoft Azure biedt een aantal oplossingen voor opslag om te voldoen aan verschillende behoeften, waaronder een bestand, de schijf, blobs en tabelopslag. Microsoft biedt ook versleuteling beveiligen [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [Azure Cosmos DB](../cosmos-db/introduction.md), en Azure Data Lake.

Gegevensversleuteling in rust is beschikbaar voor services in de software als een dienst (SaaS), platform als een service (PaaS) en de infrastructuur als een dienst (IaaS) cloud-modellen. Dit artikel bevat een overzicht van en vindt u bronnen waarmee u kunt de Azure versleutelingsopties gebruiken.

Zie voor meer gedetailleerde informatie over hoe de gegevens in rust is versleuteld in Azure, [Azure Data Encryption in rust](azure-security-encryption-atrest.md).

## <a name="azure-encryption-models"></a>Azure versleuteling modellen

Azure ondersteunt verschillende versleuteling modellen, met inbegrip van server-side '-versleuteling die gebruikmaakt van sleutels service beheerd, door de klant beheerd sleutels in de Sleutelkluis of door de klant beheerde sleutels op door de klant bewaakte hardware. Met client-side '-versleuteling te beheren en opslaan van sleutels lokale of beveiligt in een andere locatie.

### <a name="client-side-encryption"></a>Clientversleuteling

Client-side '-versleuteling wordt buiten Azure uitgevoerd. Het bevat:

- De gegevens versleuteld door een toepassing die wordt uitgevoerd in het datacenter van de klant of door een servicetoepassing.
- Gegevens die al is versleuteld wanneer het is ontvangen door Azure.

Met client-side '-versleuteling cloudserviceproviders geen toegang tot de versleutelingssleutels hebt en deze gegevens niet ontsleutelen. U behoudt de volledige controle van de sleutels.

### <a name="server-side-encryption"></a>Versleuteling aan de serverzijde

De drie serverzijde versleuteling-modellen bieden verschillende Sleutelbeheer kenmerken, die u volgens uw vereisten kiezen kunt:

- **Service beheerd sleutels**: biedt een combinatie van controle en het gemak met weinig overhead.

- **De klant beheerd sleutels**: biedt u meer controle over de sleutels, inclusief ondersteuning voor Bring Your Own sleutels (BYOK), of kunt u om nieuwe te genereren.

- **Service beheerd sleutels in de klant bewaakte hardware**: Hiermee kunt u sleutels in uw eigen opslagplaats, buiten Microsoft besturingselement beheren. Dit kenmerk wordt Host uw eigen sleutel (HYOK) genoemd. Configuratie is echter complexe, en de meeste Azure services geen ondersteuning voor dit model.

### <a name="azure-disk-encryption"></a>Azure schijfversleuteling

U kunt Windows- en Linux virtuele machines beveiligen met behulp van [Azure disk encryption](azure-security-disk-encryption.md), die gebruikmaakt [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technologie- en Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) beide beveiligen besturingssysteem-schijven en gegevensschijven met volledige versleuteling voor volumes.

Versleutelingssleutels en geheimen worden beveiligd uw [Azure Key Vault abonnement](../key-vault/key-vault-whatis.md). Met behulp van de Azure Backup-service kunt u back-up en herstel van versleutelde virtuele machines (VM's) die versleuteling KEK Key Key ()-configuratie gebruiken.

### <a name="azure-storage-service-encryption"></a>Azure Storage Service-versleuteling

Gegevens in rust in Azure Blob storage en Azure-bestandsshares kunnen worden versleuteld in zowel server- en clientzijde scenario's.

[Azure Storage Service versleuteling (SSE)](../storage/common/storage-service-encryption.md) kunt u automatisch gegevens versleutelen voordat deze is opgeslagen en deze de gegevens automatisch ontsleuteld wanneer u deze ophalen. Het proces is volledig transparant voor gebruikers. Versleuteling van de opslagruimte gebruikmaakt van 256-bits [Advanced Encryption Standard (AES) versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), die een van de sterkste blokcodering beschikbaar is. AES verwerkt transparant versleuteling, ontsleuteling en sleutelbeheer.

### <a name="client-side-encryption-of-azure-blobs"></a>Versleuteling aan clientzijde van Azure blobs

U kunt aan de clientzijde uitvoeren versleuteling van Azure blobs op verschillende manieren.

U kunt de Azure Storage-clientbibliotheek voor .NET-NuGet-pakket gebruiken om gegevens te versleutelen binnen uw clienttoepassingen voordat het uploaden naar uw Azure-opslag.

Zie voor meer informatie over en download de Azure Storage-clientbibliotheek voor .NET-NuGet-pakket, [Windows Azure-opslagservice 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

Wanneer u versleuteling aan clientzijde met Sleutelkluis gebruikt, wordt uw gegevens versleuteld met een eenmalige symmetrische inhoud versleuteling sleutel (CEK) die wordt gegenereerd door de client-SDK van Azure Storage. De CEK is versleuteld met behulp van een versleuteling KEK Key Key (), wat een symmetrische sleutel of een asymmetrisch sleutelpaar kan zijn. U kunt lokaal beheren of opslaan in de Sleutelkluis. De versleutelde gegevens is vervolgens naar Azure Storage geüpload.

Zie voor meer informatie over client-side '-codering met Sleutelkluis en aan de slag met instructies, [zelfstudie: blobs versleutelen en ontsleutelen in Azure Storage met Sleutelkluis](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Ten slotte kunt u ook de Azure Storage-clientbibliotheek voor Java gebruiken aan clientzijde versleuteling uitvoeren voordat u gegevens uploaden naar Azure Storage en de gegevens ontsleutelen wanneer u dit downloaden naar de client. Deze bibliotheek ondersteunt ook integratie met [Sleutelkluis](https://azure.microsoft.com/services/key-vault/) voor sleutelbeheer storage-account.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Versleuteling van gegevens in rust met Azure SQL Database

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) is een algemene relationele database-service in Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON ruimtelijke, en XML. SQL Database ondersteunt zowel versleuteling serverzijde via de functie Transparent Data Encryption (TDE) als versleuteling aan clientzijde via de functie altijd versleuteld.

#### <a name="transparent-data-encryption"></a>Transparante gegevensversleuteling

[TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) wordt gebruikt voor het versleutelen van [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), en [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) gegevensbestanden in real-time, met behulp van een Database Gegevensversleutelingsleutel (DEK) , die is opgeslagen in de database bootrecord voor beschikbaarheid tijdens het herstel.

TDE beschermt gegevens en logboekbestanden bestanden, met behulp van AES en Triple Data Encryption Standard (3DES) versleutelingsalgoritmen. Versleuteling van het databasebestand wordt uitgevoerd op het paginaniveau van de. De pagina's in een versleutelde database worden versleuteld voordat ze worden geschreven naar schijf en gedecodeerd wanneer ze zijn gelezen in het geheugen. TDE is nu ingeschakeld standaard op de zojuist gemaakte Azure SQL-databases.

#### <a name="always-encrypted-feature"></a>Altijd versleutelde functie

Met de [altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) functie in Azure SQL u gegevens binnen clienttoepassingen voordat u deze opslaan in Azure SQL Database kunt versleutelen. U kunt ook overdracht van beheer van de lokale database aan derden inschakelen en onderhouden van de scheiding tussen degenen die eigendom zijn en kunnen de gegevens bekijken en degenen die het beheren, maar mag geen toegang toe hebben.

#### <a name="cell-level-or-column-level-encryption"></a>Niveau van de cel of kolom versleuteling

U kunt met Azure SQL Database symmetrische codering toepassen op een kolom met gegevens met behulp van Transact-SQL. Deze methode wordt aangeroepen [cel-niveau versleuteling of versleuteling op kolomniveau (wissen)](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data), omdat u deze gebruiken kunt voor het versleutelen van specifieke kolommen of zelfs specifieke cellen van gegevens met verschillende versleutelingssleutels. In dat geval kunt u meer gedetailleerde mogelijkheid van stationsversleuteling dan TDE waarmee gegevens op pagina's worden gecodeerd.

WISSEN bevat ingebouwde functies die u gebruiken kunt om gegevens te versleutelen met behulp van zowel symmetrisch als asymmetrisch sleutels, de openbare sleutel van een certificaat of een wachtwoordzin met 3DES.

### <a name="cosmos-db-database-encryption"></a>Versleuteling van de cosmos DB-database

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) database is globaal gedistribueerd en modellen van Microsoft. Gebruikersgegevens die in Cosmos-database opgeslagen in niet-vluchtige opslag (solid-state-schijven) is standaard versleuteld. Er zijn geen besturingselementen om te schakelen in- of uitschakelen. Codering in rust wordt geïmplementeerd met behulp van een aantal beveiligingstechnologieën, met inbegrip van beveiligde sleutel opslagsystemen, gecodeerde netwerken en cryptografische API's. Coderingssleutels worden beheerd door Microsoft en worden gedraaid per interne richtlijnen van Microsoft.

### <a name="at-rest-encryption-in-data-lake"></a>Codering in rust in Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) is een ondernemingsbrede opslagplaats van elk type gegevens die worden verzameld op één plaats voordat een formele definitie van de vereisten of schema. Biedt ondersteuning voor Data Lake Store ' op standaard ' transparante versleuteling van gegevens in rust, die is ingesteld tijdens het maken van uw account. Azure Data Lake Store beheert de sleutels voor u, maar u hebt de optie voor uzelf beheren.

Drie typen sleutels worden gebruikt in het versleutelen en ontsleutelen van gegevens: de hoofdsleutel voor codering (MEK), Gegevensversleutelingsleutel (DEK) en blok versleuteling sleutel (BEK). De MEK wordt gebruikt voor het versleutelen van de DEK, die is opgeslagen op permanente media, en de BEK is afgeleid van de DEK en het gegevensblok. Als u uw eigen sleutels beheert, kunt u de MEK draaien.

## <a name="encryption-of-data-in-transit"></a>Versleuteling van gegevens die onderweg

Azure biedt veel mechanismen voor het bewaren van gegevens persoonlijke wordt vanaf één locatie naar een andere.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL-versleuteling in Azure

Microsoft gebruikt de [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) protocol (TLS) om gegevens te beschermen als deze wordt onderweg tussen het cloud-services en klanten. Microsoft-datacenters om te onderhandelen over de TLS-verbinding met clientsystemen die verbinding met Azure-services maken. TLS bevat sterke verificatie, privacy van berichten en integriteit (waardoor de detectie van bericht knoeien, onderschept en kunnen worden vervalst) interoperabiliteit, algoritmeflexibiliteit en gebruiksgemak en gebruik.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) beschermt verbindingen tussen klanten clientsystemen en Microsoft cloud-services door unieke sleutels. Verbindingen ook sleutellengten op basis van RSA 2048-bits codering gebruiken. Deze combinatie maakt het moeilijker voor iemand om te onderscheppen en toegang tot gegevens die onderweg.

### <a name="azure-storage-transactions"></a>Azure Storage-transacties

Wanneer u met Azure Storage via de Azure portal werken, hebben alle transacties plaatsvinden via HTTPS. U kunt ook de REST-API voor Storage via HTTPS gebruiken om te communiceren met Azure Storage. U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in opslagaccounts doordat een veilige overdracht die zijn voor het opslagaccount vereist.

Shared Access Signatures ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), die kan worden gebruikt voor toegang tot Azure Storage-objecten delegeren, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang. Deze aanpak zorgt ervoor dat iedereen die koppelingen met SAS-tokens verzendt het juiste protocol wordt gebruikt.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), die gebruikt voor toegang tot Azure Files shares, ondersteunt de versleuteling en deze is beschikbaar in Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10. Deze regio-overschrijdende toegang en zelfs toegang tot op het bureaublad.

Versleuteling aan clientzijde versleutelt de gegevens voordat deze wordt verzonden naar uw Azure Storage-exemplaar, zodat ze zijn versleuteld als ze via het netwerk.

### <a name="smb-encryption-over-azure-virtual-networks"></a>SMB-versleuteling via virtuele netwerken in Azure 

Met behulp van [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) in virtuele machines waarop WindowsServer 2012 of hoger, kunt u gegevens overdrachten beveiligen door het versleutelen van gegevens tijdens de overdracht via Azure virtuele netwerken maken. Door gegevens te coderen, helpen u beschermen tegen knoeien en afgeluisterd aanvallen. SMB-versleuteling voor de hele server of alleen bepaalde shares, kunnen beheerders inschakelen.

Standaard nadat SMB-versleuteling is ingeschakeld voor een share of een SMB 3.0-clients mogen toegang tot de versleutelde shares.

## <a name="in-transit-encryption-in-vms"></a>Codering in virtuele machines in transit

Gegevens die worden verzonden naar, van, en tussen virtuele machines waarop Windows wordt versleuteld in een aantal verschillende manieren, afhankelijk van de aard van de verbinding.

### <a name="rdp-sessions"></a>RDP-sessies

U kunt verbinding maken met en aanmelden bij een virtuele machine met behulp van de [Remote Desktop Protocol (RDP)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) vanuit een Windows-clientcomputer of vanuit een Mac met een RDP-client is geïnstalleerd. Gegevens tijdens de overdracht via het netwerk in het RDP-sessies kunnen worden beveiligd door TLS.

U kunt ook extern bureaublad verbinding maken met een Linux VM in Azure.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Veilige toegang tot virtuele Linux-machines met SSH

Voor extern beheer, kunt u [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) verbinding maken met virtuele Linux-machines in Azure wordt uitgevoerd. SSH is een versleutelde verbindingsprotocol waarmee beveiligde aanmeldingen via niet-beveiligde verbindingen. Het is het standaardprotocol voor de verbinding voor virtuele Linux-machines gehost in Azure. Met behulp van SSH-sleutels voor verificatie, elimineert u de noodzaak voor wachtwoorden aan te melden. SSH maakt gebruik van een openbaar/persoonlijk sleutelpaar (asymmetrische versleuteling) voor verificatie.

## <a name="azure-vpn-encryption"></a>Azure VPN-versleuteling

U kunt verbinding maken met Azure via een virtueel particulier netwerk die een beveiligde tunnel ter bescherming van de privacy van de gegevens worden verzonden via het netwerk worden gemaakt.

### <a name="azure-vpn-gateways"></a>Azure VPN-gateways

U kunt een [Azure VPN-gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) versleuteld verkeer tussen uw virtuele netwerk en uw on-premises-locatie wordt verzonden via een openbare verbinding of verkeer tussen virtuele netwerken te verzenden.

Site-naar-site VPN-verbindingen gebruiken [IPsec](https://en.wikipedia.org/wiki/IPsec) voor transport-versleuteling. Azure VPN-gateways gebruiken een reeks standaard voorstellen. U kunt Azure VPN-gateways voor het gebruik van een aangepast beleid voor IPsec/IKE met specifieke cryptografische algoritmen en de belangrijkste sterkte configureren in plaats van Hiermee stelt u het standaardbeleid voor Azure.

### <a name="point-to-site-vpns"></a>Punt-naar-site VPN-verbindingen

Punt-naar-site VPN-verbindingen toestaan individuele client computers toegang tot een virtuele Azure-netwerk. [De Secure Socket Tunneling Protocol (SSTP) toegestaan](https://technet.microsoft.com/library/2007.06.cableguy.aspx) gebruikt voor het maken van de VPN-tunnel. Deze kunt (de tunnel wordt weergegeven als een HTTPS-verbinding) firewalls passeren. Voor punt-naar-site-verbindingen kunt u uw eigen certificeringsinstantie (CA) van een interne openbare-sleutelinfrastructuur (PKI)-hoofdmap.

U kunt een punt-naar-site VPN-verbinding met een virtueel netwerk configureren met behulp van de Azure-portal met verificatie via certificaat of PowerShell.

Zie voor meer informatie over punt-naar-site VPN-verbindingen met virtuele Azure-netwerken:

[Een punt-naar-site-verbinding met een virtueel netwerk configureren met behulp van certificaatverificatie: Azure-portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Een punt-naar-site-verbinding met een virtueel netwerk configureren met behulp van verificatie via certificaat: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>Site-naar-site VPN-verbindingen 

U kunt een site-naar-site VPN-gateway-verbinding gebruiken om uw on-premises netwerk via een VPN-IPsec/IKE (IKEv1 of IKEv2)-tunnel met een Azure-netwerk. Dit type verbinding vereist een on-premises VPN-apparaat met een extern gericht openbaar IP-adres toegewezen.

U kunt een site-naar-site VPN-verbinding met een virtueel netwerk configureren met behulp van de Azure-portal, PowerShell of Azure CLI.

Zie voor meer informatie:

[Maak een site-naar-site-verbinding in de Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Maak een site-naar-site-verbinding in PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Een virtueel netwerk met een site-naar-site VPN-verbinding maken met CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>Codering in Data Lake in transit

Gegevens tijdens overdracht (ook wel gegevens in beweging genoemd) worden ook altijd versleuteld in Data Lake Store. Naast het versleutelen van gegevens voordat in permanente media opgeslagen is gegevens ook altijd onderweg via HTTPS beveiligd. HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Store REST-interfaces.

Zie voor meer informatie over het coderen van gegevens tijdens de overdracht in Data Lake, [versleuteling van gegevens in Data Lake Store](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-key-vault"></a>Sleutelbeheer met Sleutelkluis

Zonder de juiste beveiliging en beheer van de sleutels, wordt versleuteling onbruikbaar gerenderd. Sleutelkluis is de Microsoft aanbevolen oplossing voor het beheren en beheren van toegang tot de versleutelingssleutels die door de cloudservices gebruikt. Machtigingen voor toegang tot de sleutels kunnen worden toegewezen aan services of gebruikers via Azure Active Directory-accounts.

Sleutelkluis hoge organisaties van de noodzaak om te configureren, patchen en onderhouden hardware security modules (HSM's) en sleutelbeheer software. Wanneer u de Sleutelkluis hebt gebruikt, kunt u besturingselement onderhouden. Microsoft uw sleutels nooit ziet en toepassingen niet direct toegang toe hebben. U kunt ook importeren of genereren van sleutels in HSM's.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure-beveiliging](security-get-started-overview.md)
- [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)
- [Overzicht van Azure-database-beveiliging](azure-database-security-overview.md)
- [Overzicht van virtuele machines in Azure-beveiliging](security-virtual-machines-overview.md)
- [Versleuteling van inactieve gegevens](azure-security-encryption-atrest.md)
- [Aanbevolen procedures voor gegevensbeveiliging en -versleuteling](azure-security-data-encryption-best-practices.md)

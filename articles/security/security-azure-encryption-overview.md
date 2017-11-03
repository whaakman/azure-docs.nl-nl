---
title: Overzicht van Azure versleuteling | Microsoft Docs
description: Meer informatie over verschillende opties voor codering in Azure
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 752568747ab96bc0a9c7fc5f24ff28c3bce4e09f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-encryption-overview"></a>Overzicht van Azure-versleuteling

Dit artikel bevat een overzicht van hoe de versleuteling wordt gebruikt in Microsoft Azure. Deze heeft de belangrijke gebieden van versleuteling, zoals versleuteling in rust, versleuteling in vlucht en sleutelbeheer met Sleutelkluis. Elke sectie bevat koppelingen voor meer gedetailleerde informatie.

## <a name="encryption-of-data-at-rest"></a>Versleuteling van data-at-rest

Gegevens in rust bevat informatie die zich in de permanente opslag op de fysieke media, in een willekeurige digitale indeling bevindt. Het gaat hierbij om bestanden op magnetische of optische media, gearchiveerde gegevens en gegevens back-ups. Microsoft Azure biedt een aantal oplossingen voor opslag om te voldoen aan verschillende behoeften, waaronder een bestand, de schijf, blobs en tabelopslag. Microsoft biedt ook versleuteling beveiligen [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md), en Azure Data Lake.

Gegevensversleuteling in rust is beschikbaar voor services via de Azure-Software-as-a-Service (SaaS)-Platform-as-a-Service (PaaS) en de infrastructuur-as-a-Service (IaaS), cloud modellen. Dit document bevat een overzicht van en vindt u bronnen waarmee u kunt Azure versleutelingsopties gebruiken.

Voor meer discussie gedetailleerde over hoe de gegevens in rust is versleuteld in Azure, raadpleegt u het document met de titel [Azure Data Encryption in rust](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Azure versleuteling modellen

Azure ondersteunt verschillende versleuteling modellen, met inbegrip van serverzijde versleuteling met sleutels die service beheerd, met behulp van de klant beheerd sleutels in Azure Key Vault of met behulp van de klant beheerd sleutels op door de klant bewaakte hardware. Versleuteling aan clientzijde kunt u beheren en opslaan van sleutels lokale of in een andere beveiligde locatie.

### <a name="client-side-encryption"></a>Clientversleuteling

Client-side '-versleuteling wordt buiten Azure uitgevoerd. Versleuteling aan clientzijde omvat:

- Gegevens versleuteld door een toepassing die wordt uitgevoerd in het datacenter van de klant of door een servicetoepassing
- Gegevens die al is versleuteld wanneer het is ontvangen door Azure.

Met client-side '-versleuteling de cloudserviceprovider heeft geen toegang tot de coderingssleutels en deze gegevens niet ontsleutelen. U behoudt de volledige controle van de sleutels.

### <a name="server-side-encryption"></a>Server-side '-versleuteling

De drie serverzijde versleuteling-modellen bieden verschillende Sleutelbeheer kenmerken, die per uw vereisten kunnen worden gekozen.

- **Service beheerd sleutels** voorzien van een combinatie van het besturingselement en het gemak weinig overhead

- **De klant beheerd sleutels** geven u controle over de sleutels, inclusief de mogelijkheid om uw eigen sleutels (BYOK) of om nieuwe te genereren.

- **Service beheerd sleutels in ccustomer controlledhardware** kunt u voor het beheren van sleutels in uw eigen opslagplaats die buiten het beheer van Microsoft. Dit wordt Host uw eigen sleutel (HYOK) genoemd. Configuratie is echter complexe, en de meeste Azure services geen ondersteuning voor dit model.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Windows- en Linux virtuele machines kunnen worden beveiligd met [Azure Disk Encryption](azure-security-disk-encryption.md), dat via de [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) technologie- en Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) zowel besturingssysteem schijven en gegevensschijven met volledige versleuteling voor volumes te beschermen.

Versleutelingssleutels en geheimen worden beveiligd uw [Azure Key Vault](../key-vault/key-vault-whatis.md) abonnement. U kunt back-up en herstel van versleutelde virtuele machines die zijn versleuteld met de KEK-configuratie met behulp van de Azure Backup-service.

### <a name="azure-storage-service-encryption"></a>Versleuteling van Azure Storage-service

Gegevens in rust in Azure storage (Blob en -bestand) kunnen in zowel server- en clientzijde scenario's worden versleuteld.

[Azure Storage-Service: versleuteling](../storage/storage-service-encryption.md) (SSE) kunt automatisch gegevens versleutelen voordat deze wordt opgeslagen en automatisch ontsleuteld wanneer u deze, waardoor het proces volledig transparant gebruikers ophalen. Versleuteling van de opslagruimte gebruikmaakt van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), dit is een van de sterkste blok coderingen beschikbaar en versleuteling, ontsleuteling en Sleutelbeheer op transparante wijze worden verwerkt.

### <a name="client-side-encryption-of-azure-blobs"></a>Versleuteling aan clientzijde van Azure blobs

Versleuteling aan clientzijde van Azure blobs kan op verschillende manieren worden uitgevoerd.

U kunt de Azure Storage-clientbibliotheek voor .NET-NuGet-pakket gebruiken om gegevens te versleutelen binnen uw clienttoepassingen voordat het uploaden naar Azure Storage.

Zie voor meer informatie over en de Azure Storage-clientbibliotheek voor .NET-NuGet-pakket te downloaden, het document met de titel [Windows Azure-opslagservice 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

Wanneer u versleuteling aan clientzijde met Azure Sleutelkluis gebruikt, wordt uw gegevens versleuteld met een eenmalige symmetrische inhoud versleuteling sleutel (CEK) die wordt gegenereerd door de client-SDK van Azure Storage. De CEK is versleuteld met behulp van een versleuteling KEK Key Key (), wat een symmetrische sleutel of een asymmetrisch sleutelpaar kan zijn. U kunt lokaal beheren of opslaan in Azure Sleutelkluis. De versleutelde gegevens is vervolgens naar Azure Storage-service geüpload.

Zie voor meer informatie over client-side '-codering met Azure Sleutelkluis en aan de slag met instructies, het document met de titel [zelfstudie: blobs in Microsoft Azure Storage met Azure Key Vault versleutelen en ontsleutelen](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Ten slotte kunt u ook de Azure Storage-clientbibliotheek voor Java gebruiken om uit te voeren aan de clientzijde versleuteling voordat het uploaden van gegevens naar Azure Storage en de gegevens ontsleutelen wanneer deze wordt gedownload naar de client. Deze bibliotheek ondersteunt ook integratie met [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor sleutelbeheer storage-account.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Versleuteling van gegevens in rust met Azure SQL database

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) is een algemene relationele database-service in Microsoft Azure die ondersteuning biedt voor structuren zoals relationele gegevens, JSON ruimtelijke, en XML. Azure SQL ondersteunt zowel versleuteling serverzijde via de functie Transparent Data Encryption (TDE) als versleuteling aan clientzijde via de functie altijd versleuteld.

#### <a name="transparent-data-encryption"></a>Transparent Data Encryption

[TDE transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) wordt gebruikt voor het versleutelen van [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), en [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) gegevensbestanden in real-time, met behulp van een databaseversleutelingssleutel (DEK) dat is opgeslagen in de database-record voor beschikbaarheid tijdens het herstel worden opgestart.

TDE beschermt gegevens en logboekbestanden bestanden, met behulp van AES en 3DES versleutelingsalgoritmen. Versleuteling van het databasebestand is uitgevoerd op het paginaniveau; de pagina's in een versleutelde database worden versleuteld voordat ze worden geschreven naar schijf en gedecodeerd wanneer ze zijn gelezen in het geheugen. TDE is nu ingeschakeld standaard op de zojuist gemaakte Azure SQL-databases.

#### <a name="always-encrypted"></a>Altijd versleuteld

De [altijd versleuteld](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) functie in Azure SQL kunt u voor het versleutelen van gegevens binnen clienttoepassingen voordat opslaan in Azure SQL Database en kunt u de overdracht van beheer van de lokale database aan derden inschakelen en onderhouden van de scheiding tussen degenen die eigendom zijn en de gegevens en degenen die het beheren, maar mag geen toegang tot deze kunt bekijken.

#### <a name="cellcolumn-level-encryption"></a>Cel/kolom: versleuteling op bestandsniveau

Azure SQL Database kunt u symmetrische codering toepassen op een kolom met gegevens met Transact-SQL. Dit heet [cel versleuteling op bestandsniveau of kolom: versleuteling op bestandsniveau](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (wissen), omdat u deze gebruiken kunt voor het versleutelen van specifieke kolommen of zelfs specifieke cellen van gegevens met verschillende versleutelingssleutels. Hiermee krijgt u meer gedetailleerde mogelijkheid van stationsversleuteling dan TDE waarmee gegevens op pagina's worden gecodeerd.

WISSEN bevat ingebouwde functies die u gebruiken kunt voor het versleutelen van gegevens met behulp van zowel symmetrisch als asymmetrisch sleutels, de openbare sleutel van een certificaat of met een wachtwoordzin met 3DES.

### <a name="cosmos-db-database-encryption"></a>Versleuteling van de cosmos DB-database

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) database is globaal gedistribueerd en modellen van Microsoft. Gegevens van de gebruiker opgeslagen in Cosmos DB in niet-vluchtige opslag (solid-state-schijven) is versleuteld standaard; Er zijn geen besturingselementen om te schakelen in- of uitschakelen. Codering in rust wordt geïmplementeerd met behulp van een aantal beveiligingstechnologieën, met inbegrip van beveiligde sleutel opslagsystemen, gecodeerde netwerken en cryptografische API's. Coderingssleutels worden beheerd door Microsoft en worden gedraaid per interne richtlijnen van Microsoft.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Codering in rust in Azure Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) is een ondernemingsbrede opslagplaats van elk type gegevens die worden verzameld op één plaats voordat een formele definitie van de vereisten of schema. Biedt ondersteuning voor Azure Data Lake Store ' op standaard ' transparante versleuteling van gegevens in rust, die is ingesteld tijdens het maken van uw account. Data Lake Store beheert de sleutels voor u, maar u hebt de optie voor uzelf beheren.

Drie typen sleutels worden gebruikt in het versleutelen en ontsleutelen van gegevens: de hoofdsleutel voor codering (MEK), Gegevensversleutelingsleutel (DEK) en blok versleuteling sleutel (BEK). De MEK wordt gebruikt voor het versleutelen van de DEK, die is opgeslagen op permanente media, en de BEK is afgeleid van de DEK en het gegevensblok. Als u uw eigen sleutels beheert, kunt u de MEK draaien.

## <a name="encryption-of-data-in-transit"></a>Versleuteling van gegevens die onderweg

Azure biedt veel mechanismen voor het bewaren van gegevens persoonlijke wordt vanaf één locatie naar een andere.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL-versleuteling in Azure

Microsoft gebruikt de [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) protocol (TLS) om gegevens te beschermen als deze wordt onderweg tussen het cloud-services en klanten. Microsoft datacenters om te onderhandelen over de TLS-verbinding met clientsystemen die verbinding met Azure-services maken. TLS bevat sterke verificatie, privacy van berichten en integriteit (waardoor de detectie van bericht knoeien, onderschept en kunnen worden vervalst), interoperabiliteit, algoritmeflexibiliteit, gebruiksgemak en gebruik.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is de verbindingen tussen clientsystemen klanten en Microsoft cloudservices worden beschermd door unieke sleutels. Verbindingen ook sleutellengten op basis van RSA 2048-bits codering gebruiken. Deze combinatie maakt het moeilijker voor iemand om te onderscheppen en toegang tot gegevens die in transit.

### <a name="azure-storage-transactions"></a>Azure Storage-transacties

Wanneer u met Azure Storage via de Azure portal werken, hebben alle transacties plaatsvinden via HTTPS. U kunt ook de REST-API voor Storage via HTTPS gebruiken om te communiceren met Azure Storage. U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in opslagaccounts doordat veilige overdracht vereist voor het opslagaccount.

Shared Access Signatures ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), die kan worden gebruikt voor toegang tot Azure Storage-objecten delegeren, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang. Dit zorgt ervoor dat iedereen koppelingen met SAS-tokens verstuurt het juiste protocol wordt gebruikt.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) gebruikt voor toegang tot Azure-bestandsshares ondersteunt de versleuteling en de beschikbaar in Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, regio-overschrijdende toegang, en zelfs toegang krijgen tot op het bureaublad.

Versleuteling aan clientzijde versleutelt de gegevens voordat deze wordt verzonden naar Azure Storage, zodat ze zijn versleuteld als ze via het netwerk.

### <a name="smb-encryption-over-azure-virtual-networks"></a>SMB-versleuteling via virtuele netwerken in Azure 

[SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) in Azure VM's uitgevoerd op Windows Server 2012 en hoger kunt u de mogelijkheid om gegevens overdrachten beveiligen door het versleutelen van gegevens tijdens de overdracht via Azure Virtual Networks als bescherming tegen knoeien en afgeluisterd aanvallen te maken. Beheerders kunnen SMB-versleuteling inschakelen voor de hele server of alleen bepaalde shares.

Standaard zodra SMB-versleuteling is ingeschakeld voor een share of een SMB-3-clients mogen toegang tot de versleutelde shares.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Codering in Azure virtuele Machines in transit

Gegevens die onderweg naar, van, en tussen Azure VM's waarop Windows wordt uitgevoerd is in een aantal verschillende manieren, afhankelijk van de aard van de verbinding versleuteld.

### <a name="rdp-sessions"></a>RDP-sessies

U kunt verbinding maken en meld u aan bij een virtuele machine van Azure met behulp van de [Remote Desktop Protocol](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) vanuit een Windows-clientcomputer of vanuit een Mac met een RDP-client is geïnstalleerd. Gegevens tijdens de overdracht via het netwerk in het RDP-sessies kunnen worden beveiligd door TLS.

U kunt ook extern bureaublad verbinding maken met een Linux VM in Azure.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Veilige toegang tot virtuele Linux-machines met SSH

U kunt [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) verbinding maken met virtuele Linux-machines in Azure wordt uitgevoerd voor extern beheer. SSH is een versleutelde verbindingsprotocol waarmee beveiligde aanmeldingen via niet-beveiligde verbindingen. Het is het standaardprotocol voor de verbinding voor virtuele Linux-machines gehost in Azure. Met behulp van SSH-sleutels voor verificatie, elimineert u de noodzaak voor wachtwoorden aan te melden. SSH maakt gebruik van een openbaar/persoonlijk sleutelpaar (asymmetrische versleuteling) voor verificatie.

## <a name="azure-vpn-encryption"></a>Azure VPN-versleuteling

U kunt verbinding maken met Azure via een virtueel particulier netwerk die een beveiligde tunnel ter bescherming van de privacy van de gegevens worden verzonden via het netwerk worden gemaakt.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Azure VPN-gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) kan worden gebruikt versleuteld verkeer tussen uw virtuele netwerk en uw on-premises-locatie wordt verzonden via een openbare verbinding of verkeer tussen virtuele netwerken te verzenden.

Site-naar-site VPN maakt gebruik van [IPsec](https://en.wikipedia.org/wiki/IPsec) voor transport-versleuteling. Azure VPN-gateways gebruiken een reeks standaard voorstellen. U kunt Azure VPN-gateways voor het gebruik van een aangepast beleid voor IPsec/IKE met specifieke cryptografische algoritmen en de belangrijkste sterkte configureren in plaats van Hiermee stelt u het standaardbeleid voor Azure.

### <a name="point-to-site-vpn"></a>Punt-naar-site VPN

Punt-naar-Site VPN-verbindingen toestaan individuele client computers toegang tot een Azure-netwerk. [De Secure Socket Tunneling-Protocol](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) gebruikt voor het maken van de VPN-tunnel en firewalls (de tunnel wordt weergegeven als een HTTPS-verbinding) kunnen passeren. Voor punt-naar-site-verbindingen kunt u uw eigen interne PKI basis-CA.

U kunt een punt-naar-site VPN-verbinding met een virtueel netwerk met de Azure portal en zonder certificaatverificatie PowerShell configureren.

Zie voor meer informatie over punt-naar-site VPN-verbindingen naar Azure VNets: [een punt-naar-Site-verbinding met een VNet met verificatie van de certificeringsinstantie configureren: Azure-portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) en

[Een punt-naar-Site-verbinding met een VNet met behulp van verificatie via certificaat configureren: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Site-naar-site VPN 

Een site-naar-site-VPN-gatewayverbinding wordt gebruikt om een on-premises netwerk via een IPsec-/IKE-VPN-tunnel (IKEv1 of IKEv2) te verbinden met een virtueel Azure-netwerk. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen.

U kunt een site-naar-site VPN-verbinding met een virtueel netwerk met de Azure-portal, PowerShell of de Azure-opdrachtregelinterface (CLI) configureren.

Lees deze voor meer informatie:

[Maak een Site-naar-Site-verbinding in de Azure portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Maak een Site-naar-Site-verbinding](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Een virtueel netwerk maken met een site-naar-site VPN-verbinding met CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Codering in Azure Data Lake in transit

Gegevens tijdens overdracht (ook wel gegevens in beweging genoemd) worden ook altijd versleuteld in Data Lake Store. Naast het versleutelen van gegevens voorafgaand aan het opslaan op permanente media, worden de gegevens tijdens overdracht ook altijd beveiligd met behulp van HTTPS. HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Store REST-interfaces.

Zie voor meer informatie over het coderen van gegevens tijdens de overdracht in Azure Data Lake, het document met de titel [versleuteling van gegevens in Azure Data Lake Store.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Sleutelbeheer met Azure Sleutelkluis

Zonder de juiste beveiliging en beheer van de sleutels, wordt versleuteling onbruikbaar gerenderd. Azure Sleutelkluis is aanbevolen oplossing voor het beheren en beheren van toegang tot de versleutelingssleutels die wordt gebruikt door cloudservices van Microsoft. Machtigingen voor toegang tot de sleutels kunnen worden toegewezen aan services of gebruikers via Azure Active Directory-accounts.

Azure Sleutelkluis hoge organisaties van de noodzaak om te configureren, patchen en onderhouden Hardware Security Modules (HSM's) en sleutelbeheer software. Met Azure Key Vault Microsoft uw sleutels nooit ziet en toepassingen niet rechtstreeks toegang hebben tot deze; u onderhouden besturingselement. U kunt ook importeren of genereren van sleutels in HSM's.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure-beveiliging](security-get-started-overview.md)
- [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)
- [Overzicht van Azure-database-beveiliging](azure-database-security-overview.md)
- [Overzicht van virtuele machines in Azure-beveiliging](security-virtual-machines-overview.md)
- [Versleuteling van inactieve gegevens](azure-security-encryption-atrest.md)
- [Aanbevolen procedures voor gegevensbeveiliging en -versleuteling](azure-security-data-encryption-best-practices.md)

---
title: Azure Security-Services en technologieën | Microsoft Docs
description: Het artikel bevat een gecureerde lijst van de beveiliging van Azure-services en -technologieën.
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/29/2019
ms.author: barclayn
ms.openlocfilehash: 6f571655bb0501f8ef4018bf47588c8db4cc2a3e
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097658"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Beveiligingsservices en -technologieën die beschikbaar zijn op Azure

In onze gesprekken met de huidige en toekomstige Azure-klanten, we vaak gevraagd "hebt u een lijst met alle beveiligings-gerelateerde services en technologieën die Azure heeft te bieden?"

Bij het evalueren van cloud serviceprovider-opties, is het handig om deze informatie hebt. Daarom hebben we deze lijst om u aan de slag verstrekt.

Na verloop van tijd, wordt deze lijst wijzigt en groeien, net als Azure. Zorg ervoor dat u deze pagina regelmatig op de hoogte van onze services met betrekking tot beveiliging en -technologieën wilt controleren.

## <a name="general-azure-security"></a>Algemene Azure-beveiliging
|Service|Beschrijving|
|--------|--------|
|[Azure&nbsp;Security&nbsp;Center](../security-center/security-center-intro.md)| Een cloud-workload oplossing voor gegevensbescherming waarmee beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor hybride cloudworkloads.|
|[Azure Key Vault](../key-vault/key-vault-overview.md)| Een beveiligd geheimenarchief voor de wachtwoorden, verbindingsreeksen en andere informatie die u nodig hebt om uw apps werken te houden. |
|[Log Analytics](../log-analytics/log-analytics-overview.md)|Een bewakingsservice die Telemetrie en andere gegevens worden verzameld en biedt een query taal en analytics-engine voor het leveren van operationele inzichten voor uw apps en resources. Kan worden gebruikt alleen of met andere services zoals Security Center. |
|[Azure Dev/Test Labs](../devtest-lab/devtest-lab-overview.md)|Een service waarmee ontwikkelaars en testers snel omgevingen maken in Azure zonder verspilling en beheerkosten.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Opslagbeveiliging
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;Storage&nbsp;Service&nbsp;Encryption](../storage/common/storage-service-encryption.md)|Een beveiligingsfunctie die automatisch uw gegevens in Azure storage versleutelt.   |
|[StorSimple hybride opslag versleuteld](../storsimple/storsimple-ova-overview.md)| Een geïntegreerde opslagaccountoplossing waarmee tussen on-premises apparaten en Azure-cloud-opslag opslagtaken.|
|[Azure-Client-Side Encryption](../storage/common/storage-client-side-encryption.md)| Een client-side encryption-oplossing die versleutelt de gegevens binnen clienttoepassingen voordat u uploadt naar Azure Storage; ook ontsleutelt de gegevens tijdens het downloaden. |
| [Azure Storage Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount.  |
|[Azure Storage-Accountsleutels](../storage/common/storage-create-storage-account.md)| Een methode van toegangsbeheer voor Azure-opslag die wordt gebruikt voor verificatie wanneer het opslagaccount dat wordt geopend. |
|[Azure-bestandsshares met SMB 3.0-versleuteling](../storage/files/storage-files-introduction.md)|Een netwerk beveiligingstechnologie waarmee automatische versleuteling voor het Server Message Block (SMB)-protocol voor bestandsdeling. |
|[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/Storage-Analytics)| Een technologie logboekregistratie en metrische gegevens te genereren voor gegevens in uw storage-account. |

<!------>

## <a name="database-security"></a>Databasebeveiliging
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;SQL&nbsp;Firewall](../sql-database/sql-database-firewall-configure.md)|Een netwerk besturingselement onderdeel toegang biedt bescherming tegen aanvallen op het netwerk naar de database. |
|[Azure&nbsp;SQL&nbsp;Cell&nbsp;Level Encryption](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Een database beveiligingstechnologie waarmee versleuteling op een gedetailleerd niveau biedt.  |
| [Azure&nbsp;SQL&nbsp;Connection Encryption](../sql-database/sql-database-control-access.md)|Voor de beveiliging regelt SQL Database toegang met firewallregels die de connectiviteit beperken op basis van IP-adres. Met behulp van verificatiemechanismen moeten gebruikers hun identiteit kunnen aantonen en met autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties. |
| [Azure SQL altijd versleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Gevoelige gegevens, zoals creditcardnummers of nationale identificatienummers (bijvoorbeeld VS burgerservicenummers), die zijn opgeslagen in Azure SQL Database of SQL Server-databases beveiligt.  |
| [Azure&nbsp;SQL&nbsp;Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Een database beveiligingsfunctie die de opslag van een volledige database versleutelt. |
| [Azure SQL Database Auditing](../sql-database/sql-database-auditing.md)|Een database met controle-functie die worden databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in uw Azure storage-account.  |


## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;rol&nbsp;op basis van&nbsp;Access Control](../active-directory/role-based-access-control-configure.md)|Een functie voor toegang is ontworpen om gebruikers toegang krijgen tot alleen de resources die ze vereist voor toegang op basis van hun rol binnen de organisatie zijn.  |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)|Een cloud-gebaseerde authenticatie-opslagplaats die ondersteuning biedt voor een map met meerdere tenants, cloud-gebaseerde en meerdere identity management-services binnen Azure.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Een identity management-service waarmee de controle over hoe klanten gebruikersregistratie, aanmelding, en hun profielen beheren bij het gebruik van Azure-toepassingen.   |
| [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md)| Een cloud-gebaseerde en beheerde versie van Active Directory Domain Services. |
| [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)| Een security inrichten met de diverse verschillende vormen van verificatie en verificatie voordat de toegang tot beveiligde gegevens. |

## <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;Backup](../backup/backup-introduction-to-azure-backup.md)| Een Azure-service gebruikt voor het back-up en herstellen van gegevens in de Azure-cloud. |
| [Azure&nbsp;Site&nbsp;Recovery](../site-recovery/site-recovery-overview.md)|Een online service die worden gerepliceerd van workloads die worden uitgevoerd op fysieke en virtuele machines (VM's) van een primaire site naar een secundaire locatie om in te schakelen van herstel van services na een storing. |

## <a name="networking"></a>Netwerken
|Service|Beschrijving|
|------|--------|
| [Netwerk&nbsp;Security&nbsp;groepen](../virtual-network/virtual-networks-nsg.md)| Een netwerk gebaseerde functie voor toegang met behulp van een 5-tuple waarmee toestaan of weigeren van beslissingen.  |
| [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)| Een netwerkapparaat dat wordt gebruikt als een VPN-eindpunt om toe te staan cross-premises toegang tot Azure Virtual Networks.  |
| [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)|Een geavanceerde web-App load balancer die kan routeren op basis van URL en de SSL-offloading uitvoeren. |
|[De Web application firewall](../application-gateway/waf-overview.md) (WAF)|Een functie van Application Gateway die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt|
| [Azure Load Balancer](../load-balancer/load-balancer-overview.md)|Een TCP/UDP-toepassing network load balancer. |
| [Azure ExpressRoute](../expressroute/expressroute-introduction.md)| Een toegewezen WAN-koppeling tussen on-premises netwerken en virtuele netwerken van Azure. |
| [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)| Een algemene DNS-load balancer.|
| [Azure Application Proxy](../active-directory/active-directory-application-proxy-get-started.md)| Een verificatie front-end die wordt gebruikt voor beveiligde externe toegang voor webtoepassingen die on-premises worden gehost. |
|[Azure Firewall](../firewall/overview.md)|Een beheerde, cloud-gebaseerde beveiliging netwerkservice die worden beveiligd met uw Azure Virtual Network-resources.|
|[Azure DDoS protection](../virtual-network/ddos-protection-overview.md)|In combinatie met aanbevolen procedures voor ontwerp van toepassingen, biedt beveiliging tegen DDoS-aanvallen.|
|[Service-eindpunten voor virtueel netwerk](../virtual-network/virtual-network-service-endpoints-overview.md)|Breidt het privé-adresruimte van uw virtuele netwerk en de identiteit van uw VNet-naar-Azure-services, via een directe verbinding.|
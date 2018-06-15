---
title: Azure-beveiligingsservices en -technologieën | Microsoft Docs
description: Het artikel biedt een samengestelde lijst van de beveiliging van de Azure-services en -technologieën.
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
ms.date: 05/25/2018
ms.author: barclayn
ms.openlocfilehash: e52cee2cb642de6e54270c597e6ed99f7162d0ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641455"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Beveiligingsservices en -technologieën die beschikbaar zijn op Azure

In onze overleg met de huidige en toekomstige Azure-klanten, we vaak gevraagd 'hebt u een lijst met alle services die betrekking hebben op beveiliging en -technologieën die Azure te bieden heeft?'

Bij het evalueren van opties voor cloud serviceprovider, is het handig om deze informatie. Daarom hebben we deze lijst om u op weg verstrekt.

Deze lijst wordt gedurende een periode, wijzigen en toenemen, net zoals Azure biedt. Controleer of deze pagina regelmatig op de hoogte blijven van onze services betrekking hebben op beveiliging en -technologieën.

## <a name="general-azure-security"></a>Algemene Azure-beveiliging
|Service|Beschrijving|
|--------|--------|
|[Azure&nbsp;beveiliging&nbsp;Center](../security-center/security-center-intro.md)| Een cloud werkbelasting oplossing voor gegevensbeveiliging waarmee beveiligingsbeheer en geavanceerde threat protection over hybride cloudwerkbelastingen.|
|[Azure Key Vault](../key-vault/key-vault-overview.md)| Een beveiligde geheimen archief voor de wachtwoorden, verbindingsreeksen en andere informatie die u wilt bewaren van uw apps werken. |
|[Log Analytics](../log-analytics/log-analytics-overview.md)|Een bewakingsservice die Telemetrie en andere gegevens worden verzameld en biedt een query language en analyse-engine voor het leveren van operationeel inzicht voor uw toepassingen en bronnen. Kan worden gebruikt alleen of met andere services zoals Security Center. |
|[Azure ontwikkelen en testen Labs](../devtest-lab/devtest-lab-overview.md)|Een service waarmee ontwikkelaars en testers snel maken omgevingen in Azure tijdens het minimaliseren van verspilling en kosten beheren.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Opslagbeveiliging
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;opslag&nbsp;Service&nbsp;versleuteling](../storage/common/storage-service-encryption.md)|Een beveiligingsfunctie waarmee automatisch uw gegevens in Azure-opslag versleutelt.   |
|[StorSimple versleuteld hybride opslag](../storsimple/storsimple-ova-overview.md)| Een geïntegreerde opslagoplossing die opslagtaken tussen on-premises apparaten en Azure-cloud-opslag beheert.|
|[Azure-Client-side '-versleuteling](../storage/common/storage-client-side-encryption.md)| Een client-side '-versleuteling-oplossing die gegevens binnen clienttoepassingen versleutelt voordat u uploadt naar Azure Storage; decodeert gegevens ook tijdens het downloaden. |
| [Azure Storage Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Een shared access signature biedt gedelegeerde toegang tot bronnen in uw opslagaccount.  |
|[Sleutels voor Azure Storage-Account](../storage/common/storage-create-storage-account.md)| Een methode van toegangsbeheer voor Azure-opslag die wordt gebruikt voor verificatie wanneer het opslagaccount wordt geopend. |
|[Azure-bestandsshares met SMB 3.0-versleuteling](../storage/files/storage-files-introduction.md)|Een beveiligingstechnologie netwerk waarmee de automatische netwerkversleuteling voor de Server Message Block (SMB)-protocol voor bestandsdeling. |
|[Azure Storage Analytics](https://docs.microsoft.com/en-us/rest/api/storageservices/Storage-Analytics)| Een technologie voor registratie en metrische gegevens te genereren voor gegevens in uw opslagaccount. |

<!------>

## <a name="database-security"></a>Databasebeveiliging
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;SQL&nbsp;Firewall](../sql-database/sql-database-firewall-configure.md)|Een netwerk besturingselement onderdeel toegang biedt bescherming tegen aanvallen op het netwerk naar de database. |
|[Azure&nbsp;SQL&nbsp;cel&nbsp;niveau van versleuteling](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Een database beveiligingstechnologie waarmee versleuteling op een gedetailleerd niveau biedt.  |
| [Azure&nbsp;SQL&nbsp;versleuteling voor verbinding](../sql-database/sql-database-control-access.md)|Voor de beveiliging regelt SQL Database toegang met firewallregels die de connectiviteit beperken op basis van IP-adres. Met behulp van verificatiemechanismen moeten gebruikers hun identiteit kunnen aantonen en met autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties. |
| [Azure SQL altijd versleuteling](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Gevoelige gegevens, zoals creditcardnummers of nationale ID-nummers (bijvoorbeeld Amerikaans sofi-nummer), opgeslagen in Azure SQL Database of SQL Server databases beveiligt.  |
| [Azure&nbsp;SQL&nbsp;transparante gegevensversleuteling](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Een database beveiligingsfunctie waarmee de opslag van een volledige database versleutelt. |
| [Azure SQL Database Auditing](../sql-database/sql-database-auditing.md)|Een database controle van de functie die databasegebeurtenissen houdt en schrijft die deze naar een auditlogboek Meld u bij uw Azure storage-account.  |


## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;rol&nbsp;op basis van&nbsp;toegangsbeheer](../active-directory/role-based-access-control-configure.md)|Een onderdeel voor toegangsbeheerfunctie zo ontworpen dat gebruikers toegang krijgen tot alleen de resources die ze vereist voor toegang op basis van hun rol binnen de organisatie zijn.  |
| [Azure Active Directory](../active-directory/active-directory-whatis.md)|Een opslagplaats voor verificatie op basis van een cloud die ondersteuning biedt voor een multitenant, cloudgebaseerde adreslijst en meerdere identity management-services in Azure.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Een identity management-service waarmee u controle over hoe klanten registreren, aanmelden, en hun profielen beheren wanneer u toepassingen op basis van Azure.   |
| [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md)| Een cloud-gebaseerde en beheerde versie van Active Directory Domain Services. |
| [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)| Een voorziening beveiliging die gebruikmaakt van verschillende formulieren verificatie en verificatie uitvoert alvorens het toestaan van toegang tot beveiligde gegevens. |

## <a name="backup-and-disaster-recovery"></a>Back-up en herstel na noodgevallen
|Service|Beschrijving|
|------|--------|
| [Azure&nbsp;back-up](../backup/backup-introduction-to-azure-backup.md)| Een op basis van een Azure-service voor back-up en herstellen van gegevens in de Azure-cloud. |
| [Azure&nbsp;Site&nbsp;herstel](../site-recovery/site-recovery-overview.md)|Een online service die werkbelastingen die worden uitgevoerd op fysieke en virtuele machines (VM's) van een primaire site naar een secundaire locatie voor het herstel van services inschakelen na een storing repliceert. |

## <a name="networking"></a>Netwerken
|Service|Beschrijving|
|------|--------|
| [Netwerk&nbsp;beveiliging&nbsp;groepen](../virtual-network/virtual-networks-nsg.md)| Een netwerk gebaseerde toegangsbeheerfunctie voor toestaan of weigeren beslissingen maken met een 5-tuple.  |
| [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)| Een netwerkapparaat dat wordt gebruikt als een VPN-eindpunt om toe te staan cross-premises toegang tot Azure Virtual Networks.  |
| [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)|Een geavanceerde webtoepassing netwerktaakverdeler die kunnen doorsturen op basis van de URL en de SSL-offloading uitvoeren. |
| [Azure Load Balancer](../load-balancer/load-balancer-overview.md)|Een TCP/UDP-toepassing network load balancer. |
| [Azure ExpressRoute](../expressroute/expressroute-introduction.md)| Een speciale WAN-koppeling tussen on-premises netwerken en virtuele netwerken van Azure. |
| [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)| Een algemene DNS-load balancer.|
| [Azure Application Proxy](../active-directory/active-directory-application-proxy-get-started.md)| Lokale een verificatie front-end gebruikt voor het beveiligen van externe toegang voor webtoepassingen worden gehost. |
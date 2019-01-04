---
title: Vereisten voor Azure Stack Development Kit (ASDK)-implementatie | Microsoft Docs
description: Bekijk de omgevings- en hardwarevereisten vereisten voor Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 22032f9d2e60d3c51546c32df8b98f9633c95535
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726530"
---
# <a name="azure-stack-deployment-planning-considerations"></a>Azure Stack-implementatie planningsoverwegingen
Voordat u de Azure Stack Development Kit (ASDK) implementeert, controleert u uw development kit host-computer voldoet aan de vereisten die worden beschreven in dit artikel.


## <a name="hardware"></a>Hardware
| Onderdeel | Minimum | Aanbevolen |
| --- | --- | --- |
| Schijfstations: Besturingssysteem |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |
| Schijfstations: Algemene development kit gegevens<sup>*</sup>  |4 schijven. Elke schijf biedt minimaal 240 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |4 schijven. Elke schijf biedt een minimum van 400 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |
| COMPUTE: CPU |Dual-Socket: 16 fysieke processorkernen (totaal) |Dual-Socket: 20 fysieke processorkernen (totaal) |
| COMPUTE: Geheugen |192 GB RAM-GEHEUGEN |256 GB RAM-GEHEUGEN |
| COMPUTE: BIOS |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |
| Netwerk: NIC |Windows Server 2012 R2-certificering. Er zijn geen gespecialiseerde functies vereist |Windows Server 2012 R2-certificering. Er zijn geen gespecialiseerde functies vereist |
| Logocertificering HW |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor WindowsServer 2016](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

<sup>*</sup> U moet meer dan dit capaciteit aanbevolen als u van plan bent over het toevoegen van veel van de [marketplace-items](asdk-marketplace-item.md) van Azure.

**Configuratie van gegevensschijfstation:** Alle gegevensstations moeten van hetzelfde type (allemaal SAS, SATA of alle NVMe) en capaciteit. Als harde schijven van het type SAS worden gebruikt, moet u de schijfstations koppelen via één pad (er wordt geen ondersteuning geboden voor MPIO, Multipath).

**Configuratie-opties voor HBA**

* (aanbevolen) Eenvoudige HBA
* RAID HBA – adapter moet worden geconfigureerd in de modus 'passeren'
* RAID HBA – schijven moeten worden geconfigureerd als één schijf, RAID-0

**Typ combinaties van ondersteunde bus en media**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (als het mediatype opgegeven/onbekend<sup>*</sup>)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

<sup>*</sup> RAID-controllers zonder Pass Through-mogelijkheid kunnen het mediatype niet herkennen. Deze controllers markeren zowel HDD als SSD als niet opgegeven. In dat geval wordt de SSD gebruikt als permanente opslag in plaats van de caching-apparaten. Daarom kunt u de development kit op deze SSD's implementeren.

**Voorbeeld van HBA's**: LSI 9207-8i, LSI-9300-8i of LSI-9265-8i in de modus

Er zijn voorbeelden van OEM-configuraties beschikbaar.

## <a name="operating-system"></a>Besturingssysteem
|  | **Vereisten** |
| --- | --- |
| **Versie van het besturingssysteem** |WindowsServer 2016 of hoger. De besturingssysteemversie is niet kritiek voordat de implementatie is gestart, omdat u moet de computer opstart in de VHD die opgenomen in de Azure Stack-installatie. Het besturingssysteem en alle vereiste patches zijn al geïntegreerd in de afbeelding. Gebruik geen sleutels niet activeren van een Windows Server-exemplaren die worden gebruikt in de development kit. |

> [!TIP]
> Na de installatie van het besturingssysteem is geïnstalleerd, kunt u de [Implementatiechecker voor Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te bevestigen dat de hardware voldoet aan alle vereisten voldoet.

## <a name="account-requirements"></a>Accountvereisten
U normaal gesproken implementeren de development kit met een internetverbinding, waar u verbinding met Microsoft Azure maken kunt. In dit geval moet u een Azure Active Directory (Azure AD)-account voor het implementeren van de development kit configureren.

Als uw omgeving niet is verbonden met internet, of als u niet wilt dat Azure AD gebruiken, kunt u Azure Stack implementeren met behulp van Active Directory Federation Services (AD FS). De development kit bevat een eigen AD FS- en Active Directory Domain Services-instanties. Als u implementeert met behulp van deze optie, hebt u geen accounts vooraf instellen.

>[!NOTE]
Als u implementeert met behulp van de AD FS-optie, moet u opnieuw implementeren met Azure Stack om over te schakelen naar Azure AD.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory-accounts
Voor het Azure Stack implementeren met behulp van een Azure AD-account, moet u een Azure AD-account voorbereiden voordat u de PowerShell-script-implementatie uitvoert. Dit account wordt de globale beheerder voor de Azure AD-tenant. Het wordt gebruikt voor het inrichten en het delegeren van toepassingen en service-principals voor alle Azure Stack-services die interactie met Azure Active Directory en Graph API hebben. Het wordt ook gebruikt als de eigenaar van de provider standaardabonnement (die u kunt later wijzigen). U kunt aanmelden bij uw Azure Stack-systeem beheerdersportal met behulp van dit account.

1. Maak een Azure AD-account dat mapbeheerder is voor ten minste één Azure AD. Als u er al een hebt, kunt u die gebruiken. Anders kunt u een gratis op [ https://azure.microsoft.com/free/ ](https://azure.microsoft.com/pricing/free/) (in China, gaat u naar <http://go.microsoft.com/fwlink/?LinkID=717821> in plaats daarvan). Als u van plan later bent [Azure Stack registreren bij Azure](asdk-register.md), moet u ook een abonnement hebben in dit zojuist gemaakte account.
   
    Sla deze referenties voor gebruik als de service-beheerder. Dit account kunt configureren en beheren van resourceclouds, gebruikersaccounts, tenantplannen, quota en prijzen. In de portal kunnen ze websiteclouds, persoonlijke clouds voor virtuele machines en plannen maken en gebruikersabonnementen beheren.
1. Maak ten minste één test-gebruikersaccount in uw Azure AD zodat u kunt zich aanmelden als tenant development kit.
   
   | **Azure Active Directory-account** | **Ondersteund?** |
   | --- | --- |
   | Account voor werk of school met geldig openbaar Azure-abonnement |Ja |
   | Microsoft-account met geldig openbaar Azure-abonnement |Ja |
   | Account voor werk of school met geldig Azure-abonnement van China |Ja |
   | Account voor werk of school met geldig US Government Azure-abonnement |Ja |

Na de implementatie is Azure Active Directory-globale beheerder toestemming niet vereist. Bepaalde bewerkingen is echter mogelijk de referentie van de globale beheerder. Bijvoorbeeld, een resource provider installatieprogramma script of een nieuwe functie die een machtiging worden verleend. U kunt tijdelijk opnieuw installeren van de account hoofdbeheerder machtigingen of gebruik een afzonderlijke globale beheerder-account dat is eigenaar van de *providerabonnement standaard*.

## <a name="network"></a>Netwerk
### <a name="switch"></a>Switch
Een beschikbare poort op een switch voor de ontwikkelcomputer kit.  

De ontwikkelcomputer kit biedt ondersteuning voor verbinding maken met een switchtoegangspoort of trunk-poort. Er zijn geen speciale functies vereist voor de switch. Als u een trunk-poort gebruikt of een VLAN-ID moet configureren, moet u de VLAN-ID als een implementatieparameter opgeven.

### <a name="subnet"></a>Subnet
De development kit machine geen verbinding met de volgende subnetten:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Deze subnetten zijn gereserveerd voor de interne netwerken in de ontwikkelomgeving van de kit.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Alleen IPv4 wordt ondersteund. U kunt geen IPv6-netwerken maken.

### <a name="dhcp"></a>DHCP
Zorg ervoor dat er een DHCP-server beschikbaar is op het netwerk waarmee de NIC verbinding maakt. Als er geen DHCP beschikbaar is, moet u een extra statisch IPv4-netwerk voorbereiden naast het netwerk dat de host gebruikt. U moet dat IP-adres en die gateway opgeven als een implementatieparameter.

### <a name="internet-access"></a>Toegang tot het internet
Azure Stack is toegang tot het Internet, rechtstreeks of via een transparante proxy vereist. Azure Stack biedt geen ondersteuning voor de configuratie van een webproxy waarmee toegang tot Internet. Zowel het host-IP-adres en het nieuwe IP-adres toegewezen aan de AzS-BGPNAT01 (door DHCP of een statisch IP-adres) moet toegang hebben tot Internet. Poorten 80 en 443 worden gebruikt in de domeinen graph.windows.net en login.microsoftonline.com domeinen.


## <a name="next-steps"></a>Volgende stappen
[Het implementatiepakket ASDK downloaden](asdk-download.md)

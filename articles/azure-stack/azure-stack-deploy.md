---
title: Vereisten voor Azure Stack Development Kit implementatie | Microsoft Docs
description: Bekijk de hardware-omgeving en vereisten voor Azure Stack Development Kit (cloud-operator).
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 32a21d9b-ee42-417d-8e54-98a7f90f7311
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 0fa0d00112e731a9f2effd453ba74f5561fca358
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="azure-stack-deployment-prerequisites"></a>Vereisten voor de implementatie van Azure Stack

*Van toepassing op: Azure stapelen Development Kit*

Voordat u implementeert [Azure Stack Development Kit](azure-stack-poc.md), zorg ervoor dat uw computer voldoet aan de volgende vereisten:


## <a name="hardware"></a>Hardware
| Onderdeel | Minimum | Aanbevolen |
| --- | --- | --- |
| Schijfstations: besturingssysteem |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |
| Schijfstations: algemene development kit gegevens * |4 schijven. Elke schijf biedt minimaal 140 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |4 schijven. Elke schijf biedt minimaal 250 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |
| Compute: CPU |Dual-Socket: 12 fysieke processorkernen (totaal) |Dual-Socket: 16 fysieke processorkernen (totaal) |
| Compute: geheugen |96 GB RAM |128 GB RAM-geheugen (dit is de minimale ter ondersteuning van de resourceproviders PaaS).|
| Compute: BIOS |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |
| Netwerk: NIC |Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist |Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist |
| Logocertificering HW |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

\*U moet meer dan dit capaciteit aanbevolen als u van plan bent over het toevoegen van veel van de [marketplace-items](azure-stack-download-azure-marketplace-item.md) van Azure.

**Gegevens schijfstation configuratie:** alle gegevensstations moet van hetzelfde type (alle SAS, alle SATA of alle NVMe) en capaciteit. Als harde schijven van het type SAS worden gebruikt, moet u de schijfstations koppelen via één pad (er wordt geen ondersteuning geboden voor MPIO, Multipath).

**HBA-configuratie-opties**

* (aanbevolen) Eenvoudige HBA
* RAID HBA – adapter moet worden geconfigureerd in de modus 'passeren'
* RAID HBA – schijven moeten worden geconfigureerd als één schijf, RAID-0

**Typ combinaties van ondersteunde bus en media**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (als het mediatype niet is opgegeven/onbekend is\*)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

\*RAID-controllers zonder Pass Through-functionaliteit wordt het mediatype niet herkend. Deze controllers markeren zowel HDD als SSD als Niet-opgegeven. In dat geval wordt de SSD gebruikt als permanente opslag in plaats van het cachegeheugen van apparaten. Daarom kunt u de development kit op de SSD's implementeren.

**Voorbeeld van HBA's**: LSI 9207-8i, LSI-9300-8i of LSI-9265-8i in de modus 'passeren'

Er zijn voorbeelden van OEM-configuraties beschikbaar.

## <a name="operating-system"></a>Besturingssysteem
|  | **Vereisten** |
| --- | --- |
| **De versie van besturingssysteem** |Windows Server 2012 R2 of hoger. De versie van besturingssysteem niet essentieel voordat de implementatie wordt gestart, zoals u moet de computer opstart in de VHD die opgenomen in de Azure-Stack-installatie. Het besturingssysteem en alle vereiste patches zijn al geïntegreerd in de afbeelding. Gebruik geen sleutels voor het activeren van een Windows Server-exemplaren gebruikt in de development kit. |

## <a name="deployment-requirements-check-tool"></a>Vereisten voor de implementatie controleren hulpprogramma
Nadat het besturingssysteem is geïnstalleerd, kunt u de [implementatie Checker voor Azure-Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te bevestigen dat de hardware voldoet aan alle vereisten.

## <a name="account-requirements"></a>Vereisten voor account
U doorgaans implementeren de development kit met een internetverbinding, waar u verbinding met Microsoft Azure maken kunt. In dit geval moet u een account met Azure Active Directory (Azure AD) voor het implementeren van de development kit configureren.

Als uw omgeving niet met internet verbonden is of u niet wilt dat Azure AD gebruikt, kunt u Azure-Stack kunt implementeren met behulp van Active Directory Federation Services (AD FS). De development kit omvat een eigen AD FS en Active Directory Domain Services-exemplaren. Als u deze optie gebruikt implementeert, hebt u niet voor het instellen van accounts tevoren.

>[!NOTE]
Als u implementeert met behulp van de AD FS-optie, moet u Azure-Stack overschakelen naar Azure AD opnieuw implementeren.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory-accounts
Als u wilt implementeren Stack van Azure met behulp van een Azure AD-account, moet u een Azure AD-account voorbereiden voordat u de implementatie van PowerShell-script uitvoeren. Dit account wordt de globale beheerder voor de Azure AD-tenant. Deze wordt gebruikt voor het inrichten en toepassingen en service-principals voor alle Azure-Stack-services die met Azure Active Directory en Graph API communiceren te delegeren. Dit wordt ook gebruikt als de eigenaar van de provider standaardabonnement (die u kunt later wijzigen). U kunt aanmelden bij uw Azure-Stack-systeem beheerdersportal met behulp van dit account.

1. Maak een Azure AD-account dat de directory-beheerder voor ten minste één Azure AD. Als u er al een hebt, kunt u die gebruiken. Anders kunt u een gratis op [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China, gaat u naar <http://go.microsoft.com/fwlink/?LinkID=717821> in plaats daarvan). Als u van plan later bent [Azure Stack registreren bij Azure](azure-stack-register.md), moet u ook een abonnement hebben in de nieuwe account.
   
    Deze referenties voor gebruik opslaan in stap 6 van [implementeren de development kit](azure-stack-run-powershell-script.md). Dit *servicebeheerders*account kan resourceclouds, gebruikersaccounts, tenantplannen, quota en prijzen configureren en beheren. In de portal kunnen ze websiteclouds, persoonlijke clouds voor virtuele machines en plannen maken en gebruikersabonnementen beheren.
2. [Maak](azure-stack-add-new-user-aad.md) ten minste één account zodat u zich kunt aanmelden bij de development kit als een tenant.
   
   | **Azure Active Directory-account** | **Ondersteund?** |
   | --- | --- |
   | Account voor werk of school met geldige openbare Azure-abonnement |Ja |
   | Microsoft-account met geldig openbaar Azure-abonnement |Ja |
   | Account voor werk of school met geldige China Azure-abonnement |Ja |
   | Account voor werk of school met geldige US Government Azure-abonnement |Ja |

## <a name="network"></a>Netwerk
### <a name="switch"></a>Switch
Een beschikbare poort op een switch voor de ontwikkelcomputer kit.  

De ontwikkelcomputer kit ondersteunt verbindingen met een switchpoort toegang of de trunk-poort. Er zijn geen speciale functies vereist voor de switch. Als u een trunk-poort gebruikt of een VLAN-ID moet configureren, moet u de VLAN-ID als een implementatieparameter opgeven. Ziet u voorbeelden in de [lijst met implementatieparameters](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Subnet
De ontwikkelcomputer kit niet verbinden met de volgende subnetten:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Deze subnetten zijn gereserveerd voor de interne netwerken vanuit de ontwikkelomgeving kit.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Alleen IPv4 wordt ondersteund. U kunt geen IPv6-netwerken maken.

### <a name="dhcp"></a>DHCP
Zorg ervoor dat er een DHCP-server beschikbaar is op het netwerk waarmee de NIC verbinding maakt. Als er geen DHCP beschikbaar is, moet u een extra statisch IPv4-netwerk voorbereiden naast het netwerk dat de host gebruikt. U moet dat IP-adres en die gateway opgeven als een implementatieparameter. Ziet u voorbeelden in de [lijst met implementatieparameters](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Toegang tot het internet
Stack Azure vereist toegang tot het Internet, rechtstreeks of via een transparentproxy. Azure-Stack biedt geen ondersteuning voor de configuratie van een webproxy waarmee toegang tot Internet. Zowel het host-IP- en het nieuwe IP-adres toegewezen aan de MAS-BGPNAT01 (door DHCP of statische IP-) moet mogelijk toegang tot Internet. Poorten 80 en 443 worden onder de graph.windows.net en login.microsoftonline.com domeinen gebruikt.


## <a name="next-steps"></a>Volgende stappen
[Het implementatiepakket Azure Stack development kit downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure-Stack development kit implementeren](azure-stack-run-powershell-script.md)


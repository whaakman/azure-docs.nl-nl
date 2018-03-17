---
title: Vereisten voor Azure Stack Development Kit implementatie (ASDK) | Microsoft Docs
description: Controleer de hardware-omgeving en vereisten voor Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 24a766ada1cdd1cb854b6869d571acd73e376327
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="azure-stack-deployment-planning-considerations"></a>Azure planningsoverwegingen Stack-implementatie
Voordat u Azure Stack Development Kit (ASDK) implementeert, zorg er dan voor dat de hostcomputer van development kit voldoet aan de vereisten die in dit artikel wordt beschreven.


## <a name="hardware"></a>Hardware
| Onderdeel | Minimum | Aanbevolen |
| --- | --- | --- |
| Schijfstations: besturingssysteem |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |
| Schijfstations: algemene development kit gegevens<sup>*</sup>  |4 schijven. Elke schijf biedt minimaal 140 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |4 schijven. Elke schijf biedt minimaal 250 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. |
| Compute: CPU |Dual-Socket: 12 fysieke processorkernen (totaal) |Dual-Socket: 16 fysieke processorkernen (totaal) |
| Compute: geheugen |96 GB RAM |128 GB RAM-geheugen (dit is de minimale ter ondersteuning van de resourceproviders PaaS).|
| Compute: BIOS |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |Hyper-V ingeschakeld (met ondersteuning voor SLAT) |
| Netwerk: NIC |Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist |Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist |
| Logocertificering HW |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

<sup>*</sup> U moet meer dan dit capaciteit aanbevolen als u van plan bent over het toevoegen van veel van de [marketplace-items](asdk-marketplace-item.md) van Azure.

**Gegevens schijfstation configuratie:** alle gegevensstations moet van hetzelfde type (alle SAS, alle SATA of alle NVMe) en capaciteit. Als harde schijven van het type SAS worden gebruikt, moet u de schijfstations koppelen via één pad (er wordt geen ondersteuning geboden voor MPIO, Multipath).

**HBA-configuratie-opties**

* (aanbevolen) Eenvoudige HBA
* RAID HBA – adapter moet worden geconfigureerd in de modus 'passeren'
* RAID HBA – schijven moeten worden geconfigureerd als één schijf, RAID-0

**Typ combinaties van ondersteunde bus en media**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID-SSD (als het mediatype niet opgegeven/onbekend is<sup>*</sup>)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

<sup>*</sup> RAID-controllers zonder Pass Through-functionaliteit wordt het mediatype niet herkend. Dergelijke domeincontrollers markeren zowel harde schijven en SSD als niet opgegeven. In dat geval wordt de SSD gebruikt als de permanente opslag in plaats van in cache plaatsen van apparaten. Daarom kunt u de development kit op de SSD's implementeren.

**Voorbeeld van HBA's**: LSI 9207-8i, LSI-9300-8i of LSI-9265-8i in de modus 'passeren'

Er zijn voorbeelden van OEM-configuraties beschikbaar.

## <a name="operating-system"></a>Besturingssysteem
|  | **Vereisten** |
| --- | --- |
| **De versie van besturingssysteem** |Windows Server 2012 R2 of hoger. De versie van besturingssysteem niet essentieel voordat de implementatie wordt gestart, zoals u moet de computer opstart in de VHD die opgenomen in de Azure-Stack-installatie. Het besturingssysteem en alle vereiste patches zijn al geïntegreerd in de afbeelding. Gebruik geen sleutels voor het activeren van een Windows Server-exemplaren gebruikt in de development kit. |

> [!TIP]
> Nadat het besturingssysteem is geïnstalleerd, kunt u de [implementatie Checker voor Azure-Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te bevestigen dat de hardware voldoet aan alle vereisten.

## <a name="account-requirements"></a>Vereisten voor account
U doorgaans implementeren de development kit met een internetverbinding, waar u verbinding met Microsoft Azure maken kunt. In dit geval moet u een account met Azure Active Directory (Azure AD) voor het implementeren van de development kit configureren.

Als uw omgeving niet met internet verbonden is of u niet wilt dat Azure AD gebruikt, kunt u Azure-Stack kunt implementeren met behulp van Active Directory Federation Services (AD FS). De development kit omvat een eigen AD FS en Active Directory Domain Services-exemplaren. Als u deze optie gebruikt implementeert, hebt u niet voor het instellen van accounts tevoren.

>[!NOTE]
Als u implementeert met behulp van de AD FS-optie, moet u Azure-Stack overschakelen naar Azure AD opnieuw implementeren.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory-accounts
Als u wilt implementeren Stack van Azure met behulp van een Azure AD-account, moet u een Azure AD-account voorbereiden voordat u de implementatie van PowerShell-script uitvoeren. Dit account wordt de globale beheerder voor de Azure AD-tenant. Deze wordt gebruikt voor het inrichten en toepassingen en service-principals voor alle Azure-Stack-services die met Azure Active Directory en Graph API communiceren te delegeren. Dit wordt ook gebruikt als de eigenaar van de provider standaardabonnement (die u kunt later wijzigen). U kunt aanmelden bij uw Azure-Stack-systeem beheerdersportal met behulp van dit account.

1. Maak een Azure AD-account dat de directory-beheerder voor ten minste één Azure AD. Als u er al een hebt, kunt u die gebruiken. Anders kunt u een gratis op [ https://azure.microsoft.com/free/ ](http://azure.microsoft.com/pricing/free/) (in China, gaat u naar <http://go.microsoft.com/fwlink/?LinkID=717821> in plaats daarvan). Als u van plan later bent [Azure Stack registreren bij Azure](asdk-register.md), moet u ook een abonnement hebben in de nieuwe account.
   
    Sla deze referenties voor gebruik als de servicebeheerder. Dit account kunt configureren en beheren van de resource-clouds, gebruikersaccounts, tenant plannen, quota en prijzen. In de portal kunnen ze websiteclouds, persoonlijke clouds voor virtuele machines en plannen maken en gebruikersabonnementen beheren.
1. Maak ten minste één test-gebruikersaccount in uw Azure AD zodat u zich bij de development kit als een tenant aanmelden kunt.
   
   | **Azure Active Directory-account** | **Ondersteund?** |
   | --- | --- |
   | Account voor werk of school met geldige openbare Azure-abonnement |Ja |
   | Microsoft-account met geldig openbaar Azure-abonnement |Ja |
   | Account voor werk of school met geldige China Azure-abonnement |Ja |
   | Account voor werk of school met geldige US Government Azure-abonnement |Ja |

## <a name="network"></a>Netwerk
### <a name="switch"></a>Switch
Een beschikbare poort op een switch voor de ontwikkelcomputer kit.  

De ontwikkelcomputer kit ondersteunt verbindingen met een switchpoort toegang of de trunk-poort. Er zijn geen speciale functies vereist voor de switch. Als u een trunk-poort gebruikt of een VLAN-ID moet configureren, moet u de VLAN-ID als een implementatieparameter opgeven.

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
Zorg ervoor dat er een DHCP-server beschikbaar is op het netwerk waarmee de NIC verbinding maakt. Als er geen DHCP beschikbaar is, moet u een extra statisch IPv4-netwerk voorbereiden naast het netwerk dat de host gebruikt. U moet dat IP-adres en die gateway opgeven als een implementatieparameter.

### <a name="internet-access"></a>Toegang tot het internet
Stack Azure vereist toegang tot het Internet, rechtstreeks of via een transparentproxy. Azure-Stack biedt geen ondersteuning voor de configuratie van een webproxy waarmee toegang tot Internet. Zowel het host-IP- en het nieuwe IP-adres toegewezen aan de MAS-BGPNAT01 (door DHCP of statische IP-) moet mogelijk toegang tot Internet. Poorten 80 en 443 worden onder de graph.windows.net en login.microsoftonline.com domeinen gebruikt.


## <a name="next-steps"></a>Volgende stappen
[Het implementatiepakket ASDK downloaden](asdk-download.md)

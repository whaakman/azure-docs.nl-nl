<properties
    pageTitle="Voordat u Azure Stack POC implementeert | Microsoft Azure"
    description="Bekijk de omgevings- en hardwarevereisten voor Azure Stack POC (servicebeheerder)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Vereisten voor de implementatie van Azure Stack

Voordat u Azure Stack POC ([Proof of Concept](azure-stack-poc.md)) implementeert, moet u controleren of uw computer voldoet aan de volgende vereisten.
Deze vereisten gelden alleen voor de Azure Stack POC en kunnen worden gewijzigd voor toekomstige versies.

Het kan ook nuttig zijn om de zelfstudievideo over de implementatie te bekijken:

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-deployment-tutorial]

## Hardware

| Onderdeel | Minimum  | Aanbevolen |
|---|---|---|
| Schijfstations: besturingssysteem | 1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) | 1 besturingssysteemschijf met minimaal 200 GB beschikbaar voor de systeempartitie (SSD of HDD) |
| Schijfstations: algemene Azure Stack POC-gegevens | 4 schijven. Elke schijf biedt minimaal 140 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt. | 4 schijven. Elke schijf biedt minimaal 250 GB capaciteit (SSD of HDD). Alle beschikbare schijven worden gebruikt.|
| Compute: CPU | Dual-Socket: 12 fysieke processorkernen (totaal)  | Dual-Socket: 16 fysieke processorkernen (totaal) |
| Compute: geheugen | 96 GB RAM  | 128 GB RAM |
| Compute: BIOS | Hyper-V ingeschakeld (met ondersteuning voor SLAT)  | Hyper-V ingeschakeld (met ondersteuning voor SLAT) |
| Netwerk: NIC | Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist | Windows Server 2012 R2-certificering vereist voor NIC, geen gespecialiseerde functies vereist |
| Logocertificering HW | [Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Gecertificeerd voor Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

U kunt het [Implementatiechecker voor Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1) gebruiken om te bevestigen dat u voldoet aan de vereisten.

**Configuratie van gegevensschijfstation:** alle gegevensstations moeten van hetzelfde type zijn (allemaal SAS of SATA) en dezelfde capaciteit hebben. Als harde schijven van het type SAS worden gebruikt, moet u de schijfstations koppelen via één pad (er wordt geen ondersteuning geboden voor MPIO, Multipath).

**Configuratie-opties voor HBA**
 
- (aanbevolen) Eenvoudige HBA
- RAID HBA – adapter moet worden geconfigureerd in de modus 'passeren'
- RAID HBA – schijven moeten worden geconfigureerd als één schijf, RAID-0

**Ondersteunde combinaties van bus en mediatype**

-   SATA HDD

-   SAS HDD

-   RAID HDD

-   RAID SSD (als het mediatype niet is opgegeven/onbekend is\*)

-   SATA SSD + SATA HDD

-   SAS SSD + SAS HDD

\* RAID-controllers zonder mogelijkheid om te passeren kunnen het mediatype niet herkennen. Deze controllers markeren zowel HDD als SSD als Niet-opgegeven. In dat geval wordt de SSD gebruikt als permanente opslag in plaats van het cachegeheugen van apparaten. Daarom kunt u de Microsoft Azure Stack POC op deze SSD's implementeren.

**Voorbeeld van HBA's**: LSI 9207-8i, LSI-9300-8i of LSI-9265-8i in de modus 'passeren'

Er zijn voorbeelden van OEM-configuraties beschikbaar.




## Besturingssysteem

| | **Vereisten**  |
|---|---|
| **Versie van het besturingssysteem** | Windows Server 2016 Datacenter Edition **Technical Preview 4** met de meest recente belangrijke updates geïnstalleerd. Een WindowsServer2016Datacenter.vhdx is opgenomen in het downloadpakket. U kunt opstarten naar deze VHDX en deze vervolgens gebruiken als het basisbesturingssysteem voor uw Azure Stack POC-implementatie.|
| **Installatiemethode** | Schone installatie. U kunt de WindowsServer2016Datacenter.vhdx in het implementatiepakket gebruiken om het besturingssysteem snel te installeren op uw computer met de Azure Stack POC. |
| **Aan domein toevoegen?** | Nee. |


## Microsoft Azure Active Directory-accounts

1. Maak een Azure AD-account dat mapbeheerder is voor ten minste één Azure Active Directory. Als u er al een hebt, kunt u die gebruiken. Anders kunt u er gratis een maken op  [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China gaat u in plaats daarvan naar <http://go.microsoft.com/fwlink/?LinkID=717821>.)

    Sla deze aanmeldingsgegevens op zodat u ze kunt gebruiken in stap 6 van [Het PowerShell-script voor implementatie uitvoeren](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Dit *servicebeheerders*account kan resourceclouds, gebruikersaccounts, tenantplannen, quota en prijzen configureren en beheren. In de portal kunnen ze websiteclouds, persoonlijke clouds voor virtuele machines en plannen maken en gebruikersabonnementen beheren.

2. [Maak](azure-stack-add-new-user-aad.md) ten minste één account zodat u zich kunt aanmelden bij de Azure Stack POC als een tenant.

  	| **Azure Active Directory-account**  | **Ondersteund?** |
  	|---|---| 
  	| Organisatie-ID met geldig openbaar Azure-abonnement  | Ja |
  	| Microsoft-account met geldig openbaar Azure-abonnement  | Ja |
  	| Organisatie-ID met geldig Azure-abonnement in China  | Ja |
  	| Organisatie-ID met geldig Azure-abonnement van de Amerikaanse overheid  | Nee |

>[AZURE.NOTE] De Azure Stack POC ondersteunt alleen Azure Active Directory-verificatie.


## Netwerk

### Switch

Een beschikbare poort op een switch voor de computer met de POC.  

De computer met de Azure Stack POC ondersteunt een verbinding met een switchtoegangspoort of trunk-poort. Er zijn geen speciale functies vereist voor de switch. Als u een trunk-poort gebruikt of een VLAN-ID moet configureren, moet u de VLAN-ID als een implementatieparameter opgeven. Bijvoorbeeld:

    DeployAzureStack.ps1 –Verbose –PublicVLan 305

Als u deze parameter opgeeft, wordt alleen de VLAN-ID voor de host en de NATVM ingesteld.

### Subnet

Verbind de computer met de POC niet met de subnetten 192.168.200.0/24, 192.168.100.0/24 of 192.168.133.0/24. Deze zijn gereserveerd voor de interne netwerken in de Microsoft Azure Stack POC-omgeving.

### IPv4/IPv6

Alleen IPv4 wordt ondersteund. U kunt geen IPv6-netwerken maken.

### DHCP

Zorg ervoor dat er een DHCP-server beschikbaar is op het netwerk waarmee de NIC verbinding maakt. Als er geen DHCP beschikbaar is, moet u een extra statisch IPv4-netwerk voorbereiden naast het netwerk dat de host gebruikt. U moet dat IP-adres en die gateway opgeven als een implementatieparameter. Bijvoorbeeld:

    DeployAzureStack.ps1 -Verbose -NATVMStaticIP 10.10.10.10/24 -NATVMStaticGateway 10.10.10.1

### Toegang tot het internet

Zorg ervoor dat de NIC verbinding kan maken met het internet. De host-IP en de nieuwe IP die zijn toegewezen aan de NATVM (door DHCP of een statisch IP-adres) moeten toegang tot het internet hebben. De poorten 80 en 443 worden gebruikt in de domeinen graph.windows.net en login.windows.net.

### Proxy

Als een proxy is vereist in uw omgeving, geeft u het proxyserveradres en de poort op als een implementatieparameter. Bijvoorbeeld:

    DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080

Azure Stack POC biedt geen ondersteuning voor proxyverificatie. 

### Telemetrie

Poort 443 (HTTPS) moet worden geopend voor uw netwerk. Het eindpunt van de client is https://vortex-win.data.microsoft.com.


## Volgende stappen

[Het implementatiepakket voor de Azure Stack POC downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[De Azure Stack POC implementeren](azure-stack-run-powershell-script.md)



<!--HONumber=Sep16_HO3-->



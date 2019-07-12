---
title: Azure Migrate toestel-architectuur | Microsoft Docs
description: Biedt een overzicht van het apparaat Azure Migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: c2c9ca3082aa9c2067a63f8d6304e8a229dac14a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811451"
---
# <a name="azure-migrate-appliance"></a>Azure Migrate-apparaat

Dit artikel wordt beschreven voor het apparaat Azure Migrate. U kunt het apparaat implementeren als u hulpprogramma's voor Azure Migrate-evaluatie en migratie gebruiken om te ontdekken, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. 

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen en workloads en persoonlijke/openbare cloud, virtuele machines naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.



## <a name="appliance-overview"></a>Overzicht van apparaat

De Azure Migrate toestel typen en het gebruik zijn.

**Geïmplementeerd als** | **Gebruikt voor** | **Details**
--- | --- |  ---
Virtuele VMware-machine | De evaluatie van de VMware-VM met het hulpprogramma Azure Migrate-evaluatie.<br/><br/> Zonder agent VMware-VM-migratie met het hulpprogramma voor migratie van Azure-Server migreren | OVA-sjabloon downloaden en importeren met vCenter-Server te maken van de virtuele machine van het apparaat.
Hyper-V-VM | De evaluatie van de Hyper-V-VM met het hulpprogramma Azure Migrate-evaluatie. | ZIP VHD downloaden en importeren in Hyper-V naar de VM-toepassing maken.

## <a name="appliance-access"></a>Apparaat-toegang

Nadat u het apparaat hebt geconfigureerd, kunt u de virtuele machine van het apparaat op afstand openen via TCP-poort 3389. U kunt ook op afstand toegang tot de web-app voor het apparaat, op poort 44368 met URL: ``` https://<appliance-ip-or-name>:44368 ```.

## <a name="appliance-license"></a>Apparaat-licentie
Het apparaat wordt geleverd met een licentie van de evaluatie van Windows Server 2016, die 180 dagen geldig is. Als de evaluatieperiode bijna verlopen is, raden wij u downloaden en implementeren van een nieuw toestel of u de licentie van het besturingssysteem van de virtuele machine van het apparaat te activeren.

## <a name="appliance-agents"></a>Apparaat-agents
Het apparaat heeft deze agents zijn geïnstalleerd.

**Agent** | **Details**
--- | ---
Serverdetectie-agent | Verzamelt configuratiegegevens van on-premises VM's.
Evaluatie van de agent | Profielen voor de on-premises omgeving voor het verzamelen van prestatiegegevens van de virtuele machine.
Migratie-adapter | Regelt de replicatie van virtuele machines en coördineert de communicatie tussen virtuele machines en Azure.
Migratie van gateway | VM-gegevens verzendt gerepliceerd naar Azure.


## <a name="appliance-deployment-requirements"></a>Vereisten voor implementatie van apparaat

- [Beoordeling](migrate-support-matrix-vmware.md#assessment-appliance-requirements) voor een VMware-apparaat en de URL's die het apparaat moet toegang hebben tot de vereisten voor de implementatie.
- [Beoordeling](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) voor een Hyper-V-apparaat en de URL's die het apparaat moet toegang hebben tot de vereisten voor de implementatie.


## <a name="collected-performance-data-vmware"></a>De prestaties van de verzamelde gegevens-VMware

Hier volgt de prestatiegegevens van VMware-VM die het apparaat worden verzameld en verzonden naar Azure.

**Gegevens** | **Teller** | **Evaluatie van de impact**
--- | --- | ---
CPU-gebruik | cpu.usage.average | Aanbevolen VM-grootte/kosten
Geheugengebruik | mem.usage.average | Aanbevolen VM-grootte/kosten
Schijf lezen doorvoer (MB per seconde) | virtualDisk.read.average | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
Schijf schrijven-doorvoer (MB per seconde) | virtualDisk.write.average | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
Schijf lezen-bewerkingen per seconde | virtualDisk.numberReadAveraged.average | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
Schijf schrijven per seconde | virtualDisk.numberWriteAveraged.average  | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
NIC doorvoer (MB per seconde) lezen | NET.Received.Average | Voor een VM-grootte
NIC-schrijfbewerkingen (MB per seconde) | net.transmitted.average  |Voor een VM-grootte


## <a name="collected-metadata-vmware"></a>Verzamelde metagegevens-VMware

Hier is de volledige lijst van de metagegevens van de VMware-VM die het apparaat worden verzameld en verzonden naar Azure.

**Gegevens** | **Teller**
--- | --- 
**MachineDetails** | 
VM-ID | vm.Config.InstanceUuid 
VM-naam | vm.Config.Name
vCenter Server-ID | VMwareClient.Instance.Uuid
Beschrijving van de virtuele machine | vm.Summary.Config.Annotation
De naam van de licentie-product | vm.Client.ServiceContent.About.LicenseProductName
Besturingssysteemtype | vm.SummaryConfig.GuestFullName
Opstarttype | vm.Config.Firmware
Aantal kerngeheugens | vm.Config.Hardware.NumCPU
Geheugen (MB) | vm.Config.Hardware.MemoryMB
Aantal schijven | virtuele machine. Config.Hardware.Device.ToList(). FindAll(x => is VirtualDisk).count
Lijst met schijven grootte | virtuele machine. Config.Hardware.Device.ToList(). FindAll (x = > is VirtualDisk)
Lijst met netwerkadapters | virtuele machine. Config.Hardware.Device.ToList(). FindAll(x => is VirtualEthernet).count
CPU-gebruik | cpu.usage.average
Geheugengebruik |mem.usage.average
**Per details van schijf** | 
De sleutelwaarde schijf | schijf. Sleutel
Dikunit getal | schijf. UnitNumber
Schijf-controller-sleutelwaarde | disk.ControllerKey.Value
Gigabytes ingericht | virtualDisk.DeviceInfo.Summary
Schijfnaam | De waarde die wordt gegenereerd met behulp van de schijf. UnitNumber, schijf. Sleutel, schijf. ControllerKey.VAlue
Leesbewerkingen per seconde | virtualDisk.numberReadAveraged.average
Schrijfbewerkingen per seconde | virtualDisk.numberWriteAveraged.average
Doorvoer (MB per seconde) lezen | virtualDisk.read.average
Doorvoer (MB per seconde) schrijven | virtualDisk.write.average
**Per NIC-details** | 
Naam van de netwerkadapter | nic.Key
MAC-adres | ((VirtualEthernetCard)nic).MacAddress
IPv4-adressen | vm.Guest.Net
IPv6-adressen | vm.Guest.Net
Doorvoer (MB per seconde) lezen | NET.Received.Average
Doorvoer (MB per seconde) schrijven | net.transmitted.average
**Inventarisgegevens van pad** | 
Name | container.GetType().Name
Het type onderliggend object | container.ChildType
Details van verwijzing | container.MoRef
Details van de bovenliggende | Container.Parent
Details van een map per VM | ((Folder)container).ChildEntity.Type
Datacenter-details per VM | ((Datacenter)container).VmFolder
Details van het datacenter per host-map | ((Datacenter)container).HostFolder
Clusterdetails per host | ((ClusterComputeResource)container).Host
Details van de host per VM | ((HostSystem) container). VIRTUELE MACHINE



## <a name="collected-performance-data-hyper-v"></a>De prestaties van de verzamelde gegevens-Hyper-V

Hier volgt de prestatiegegevens van VMware-VM die het apparaat worden verzameld en verzonden naar Azure.

**Klasse Prestatiemeter** | **Teller** | **Evaluatie van de impact**
--- | --- | ---
Virtuele Hyper-V-Hypervisor-Processor | % Gastuitvoeringstijd | Aanbevolen VM-grootte/kosten
Dynamisch geheugen voor Hyper-V virtuele machine | Huidige druk (%)<br/> Gast zichtbaar fysiek geheugen (MB) | Aanbevolen VM-grootte/kosten
Hyper-V virtuele opslagapparaat | Gelezen Bytes per seconde | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
Hyper-V virtuele opslagapparaat | Geschreven Bytes per seconde | Berekening van de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
Hyper-V virtuele netwerkadapter | Bytes ontvangen per seconde | Voor een VM-grootte
Hyper-V virtuele netwerkadapter | Bytes verzonden per seconde | Voor een VM-grootte

- CPU-gebruik is de som van alle gebruik, voor alle virtuele processors die zijn gekoppeld aan een virtuele machine.
- Geheugengebruik is (zware belasting op het huidige * Gast zichtbaar fysiek geheugen) / 100.
- Schijf- en -gebruik waarden worden verzameld van de vermelde Hyper-V-prestatiemeteritems.

## <a name="collected-metadata-hyper-v"></a>Verzamelde metagegevens-Hyper-V

Hier is de volledige lijst van de metagegevens van de Hyper-V-VM die het apparaat worden verzameld en verzonden naar Azure.

**Gegevens** | **WMI-klasse** | **WMI-klasse-eigenschap**
--- | --- | ---
**MachineDetails** | 
Serienummer van BIOS _ Msvm_BIOSElement | BIOSSerialNumber
VM-type (Gen 1 of 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
Weergavenaam van de virtuele machine | Msvm_VirtualSystemSettingData | ElementName
VM-versie | Msvm_ProcessorSettingData | VirtualQuantity
Geheugen (bytes) | Msvm_MemorySettingData | VirtualQuantity
Maximale hoeveelheid geheugen die kan worden gebruikt door de virtuele machine | Msvm_MemorySettingData | Limiet
Dynamisch geheugen is ingeschakeld | Msvm_MemorySettingData | DynamicMemoryEnabled
Besturingssysteem naam/versie/FQDN | Msvm_KvpExchangeComponent | De naam van de GuestIntrinsciExchangeItems gegevens
Energiestatus | Msvm_ComputerSystem | EnabledState
**Per details van schijf** | 
Schijf-ID | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Type virtuele harde schijf | Msvm_VirtualHardDiskSettingData | type
Grootte van virtuele harde schijf | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Bovenliggende virtuele harde schijf | Msvm_VirtualHardDiskSettingData | ParentPath
**Per NIC-details** | 
IP-adressen (synthetische NIC's) | Msvm_GuestNetworkAdapterConfiguration | IPAddresses
DHCP ingeschakeld (synthetische NIC's) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
NIC-ID (synthetische NIC's) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC-MAC-adres (synthetische NIC's) | Msvm_SyntheticEthernetPortSettingData | Adres
NIC-ID (oude NIC's) | MsvmEmulatedEthernetPortSetting gegevens | InstanceID
ID van de MAC NIC (oude NIC's) | MsvmEmulatedEthernetPortSetting gegevens | Adres




## <a name="discovery-and-collection-process"></a>Detectie-en-verzameling

Het apparaat communiceert met de vCenter-Servers en Hyper-V-hosts/cluster met behulp van de volgende procedure.


1. **Detecteren starten**:
    - Wanneer u de detectie op de Hyper-V-apparaat begint, wordt deze communiceert met de Hyper-V-hosts op WinRM poort 5985 (HTTP) en 5986 (HTTPS).
    - Wanneer u detectie van het toestel VMware start, wordt deze communiceert met de vCenter-server op TCP-poort 443 standaard. Als de vCenter-server op een andere poort luistert, kunt u deze configureren in de web-app van het apparaat.
2. **Metagegevens-en prestatiegegevens verzamelen**:
    - Het apparaat maakt gebruik van een sessie Common Information Model (CIM) voor het verzamelen van gegevens van de Hyper-V-VM van de Hyper-V-host op poort 5985 en 5986.
    - Het apparaat communiceert met poort 443 wordt standaard voor het verzamelen van VMware-VM-gegevens van de vCenter-Server.
3. **Gegevens verzenden**: Het apparaat verzendt de verzamelde gegevens naar Azure Migrate-Server-evaluatie en migratie van Azure-Server migreren via SSL-poort 443.
    - Het apparaat verzamelt prestatiegegevens, realtime gebruiksgegevens.
        - Prestatiegegevens worden verzameld voor elke 20 seconden voor VMware en elke 30 seconden voor Hyper-V, voor elke metrische gegevens voor prestaties.
        - De verzamelde gegevens worden samengevoegd tot één gegevenspunt voor tien minuten maken.
        - De piekwaarde voor het gebruik is geselecteerd in alle van de 20/30 tweede gegevenspunten en verzonden naar Azure voor de evaluatie van de berekening.
        - Op basis van de percentielwaarde die is opgegeven in de evaluatie-eigenschappen (50e/90e/95th/99th), de tien minuten durende punten worden gesorteerd in oplopende volgorde en de juiste percentielwaarde wordt gebruikt voor het berekenen van de evaluatie
    - Voor de migratie van servers, het apparaat begint met het verzamelen van gegevens van de virtuele machine en repliceert dit naar Azure.
4. **Evalueren en migreren**: U kunt nu evaluatie maken van de metagegevens die zijn verzameld door het apparaat met behulp van Azure Migrate-Server-evaluatie. U kunt bovendien ook starten voor het migreren van virtuele VMware-machines met behulp van Azure Migrate-servermigratie om zonder agent VM-replicatie.


![Architectuur](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Upgrades van apparaat

Het apparaat is bijgewerkt als de Azure Migrate-agents die worden uitgevoerd op het apparaat zijn bijgewerkt.

- Dit gebeurt automatisch omdat het automatisch bijwerken is standaard ingeschakeld op het apparaat.
- U kunt deze standaardinstelling de agents handmatig bijwerken.
- Automatische updates uitschakelen, stelt u registersleutel apparaat automatisch bijwerken, in HKLM\SOFTWAREMicrosoft\Azure.

### <a name="set-agent-updates-to-manual"></a>Agent-updates ingesteld op handmatig

Voor handmatige updates, zorg ervoor dat de agents op het apparaat bij te werken op hetzelfde moment, met behulp van de **bijwerken** knop voor elke verouderde agent op het apparaat. U kunt de instelling van de update Ga terug naar de automatische updates op elk gewenst moment.

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](tutorial-assess-vmware.md#set-up-the-appliance-vm) kunt u het apparaat instellen voor VMware.
[Informatie over hoe](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) het instellen van het apparaat voor Hyper-V.


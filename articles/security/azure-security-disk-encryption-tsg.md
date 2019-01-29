---
title: Probleemoplossing - Azure Disk Encryption voor IaaS-VM's | Microsoft Docs
description: Dit artikel vindt tips voor probleemoplossing voor Microsoft Azure Disk Encryption voor Windows en Linux IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 01/25/2019
ms.custom: seodec18
ms.openlocfilehash: 0b486831118ace7d2112acf1562f5df4a64d1e1b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55092089"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Probleemoplossingsgids voor Azure Disk Encryption

Deze handleiding is bedoeld voor IT-specialisten, informatiebeveiligingsanalisten en cloudbeheerders die willen Azure Disk Encryption gebruiken. Dit artikel is om te helpen bij het oplossen van schijf-versleuteling-gerelateerde problemen.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Oplossen van problemen met schijfversleuteling van Linux-besturingssysteem

Schijfversleuteling voor Linux-besturingssysteem (OS) moet de besturingssysteemschijf voordat het wordt uitgevoerd via het versleutelingsproces volledige schijf ontkoppelen. Als de schijf, een foutbericht van deze kan niet ontkoppelen ' kan niet ontkoppelen na... ' waarschijnlijk optreedt.

Deze fout kan optreden als de OS-schijfversleuteling is geprobeerd op een omgeving met doel-VM's die is gewijzigd van de ondersteunde aandelen galerijafbeelding. Afwijkingen van de installatiekopie van het ondersteunde kunnen leiden tot problemen met de mogelijkheid van de extensie ontkoppelen van de besturingssysteemschijf. Voorbeelden van afwijkingen kunnen de volgende items zijn:
- Aangepaste installatiekopieën is niet meer overeen met een ondersteund bestandssysteem of het partitieschema.
- Grote toepassingen zoals SAP, MongoDB, Apache Cassandra en Docker worden niet ondersteund wanneer ze geïnstalleerd en worden uitgevoerd in het besturingssysteem voor versleuteling zijn. Azure Disk Encryption is niet afgesloten deze processen veilig zoals vereist ter voorbereiding van de besturingssysteemschijf voor schijfversleuteling. Als er nog steeds actieve processen bestandsingangen openen om de besturingssysteemschijf te houden, is de besturingssysteemschijf kan niet worden ontkoppeld, wat resulteert in een fout opgetreden bij het versleutelen van de besturingssysteemschijf. 
- Aangepaste scripts die worden uitgevoerd in de buurt sluiten tijd aan de versleuteling wordt ingeschakeld, of als een andere wijzigingen worden aangebracht op de virtuele machine tijdens het versleutelingsproces. Dit conflict kan gebeuren als u een Azure Resource Manager-sjabloon definieert meerdere extensies voor het gelijktijdig uitvoeren of als een extensie voor aangepaste scripts of een andere actie wordt gelijktijdig naar schijfversleuteling. Tijdens het serialiseren en te isoleren van deze stappen kunnen los het probleem.
- Security Enhanced Linux (SELinux) nog niet zijn uitgeschakeld voordat u de versleuteling, is ingeschakeld, zodat de stap ontkoppelen is mislukt. SELinux kan worden ingeschakeld nadat de codering is voltooid.
- De besturingssysteemschijf maakt gebruik van een schema logische Volume Manager (LVM). Hoewel beperkte LVM gegevens schijfondersteuning beschikbaar is, is er een besturingssysteemschijf LVM niet.
- Aan de minimale geheugenvereisten niet worden voldaan (7 GB wordt aanbevolen voor versleuteling van de OS-schijf).
- Schijven worden recursief gekoppeld onder de map /mnt/ of elke andere (bijvoorbeeld /mnt/data1, /mnt/data2, /data3 + /data3/data4).
- Andere Azure Disk Encryption [vereisten](azure-security-disk-encryption-prerequisites.md) voor Linux worden niet voldaan.

## <a name="bkmk_Ubuntu14"></a> Bijwerken van de standaard-kernel voor Ubuntu 14.04 TNS

De Ubuntu 14.04 LTS-installatiekopie wordt geleverd met een standaardversie voor de kernel van 4.4. Deze kernelversie is een bekend probleem waarbij van geheugen Killer niet goed wordt beëindigd de opdracht dd tijdens het versleutelingsproces OS. Deze fout is opgelost in de meest recente Azure afgestemd op de Linux-kernel. Om te voorkomen dat deze fout, vóór het inschakelen van versleuteling op de installatiekopie bijwerken naar de [Azure afgestemd op de kernel 4.15](https://packages.ubuntu.com/trusty/linux-azure) of met behulp van de volgende opdrachten:

```
sudo apt-get update
sudo apt-get install linux-azure
sudo reboot
```

Nadat de virtuele machine opnieuw is opgestart in de nieuwe kernel, kan de nieuwe kernelversie worden bevestigd met behulp van:

```
uname -a
```

## <a name="unable-to-encrypt-linux-disks"></a>Kan niet voor het versleutelen van Linux-schijven

In sommige gevallen kan is de Linux-schijfversleuteling lijkt te zijn vastgelopen bij 'OS-schijf versleuteling aan de slag'- en SSH uitgeschakeld. De versleuteling van 3-16 uur eindigen op een afbeelding voorraad kan duren. Als multi-terabyte-formaat gegevensschijven worden toegevoegd, kan dit proces kan dagen duren.

De volgorde van Linux-besturingssysteem schijf versleuteling losgekoppeld tijdelijk van de besturingssysteemschijf. Blok-voor-blokverificatie versleuteling van een schijf met het volledige besturingssysteem, wordt vervolgens uitgevoerd voordat deze het remounts in de versleutelde status. In tegenstelling tot Azure Disk Encryption op Windows kunnen Linux-schijfversleuteling geen voor gelijktijdige gebruik van de virtuele machine terwijl de versleuteling uitgevoerd wordt. De prestatiekenmerken van de virtuele machine kunnen maken van een aanzienlijk verschil zijn in de tijd die nodig is om versleuteling te voltooien. Deze kenmerken zijn de grootte van de schijf en of het opslagaccount is standard of premium (SSD)-opslag.

Vragen om te controleren of de status voor schijfversleuteling, de **ProgressMessage** veld geretourneerd door de [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) opdracht. Terwijl de OS-schijf wordt versleuteld, wordt de virtuele machine wordt de status onderhoud ingeschakeld en schakelt SSH om te voorkomen dat een onderbreking van het continue proces. De **EncryptionInProgress** rapporten voor het merendeel van de tijd weergegeven terwijl de versleuteling uitgevoerd wordt. Enkele uren later een **VMRestartPending** bericht u vraagt om de virtuele machine opnieuw opstarten. Bijvoorbeeld:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Nadat u wordt gevraagd naar de virtuele machine opnieuw opstarten en nadat de virtuele machine opnieuw is opgestart, moet u wachten 2-3 minuten voor het opnieuw opstarten en de laatste stappen voor het doel moet worden uitgevoerd. Het bericht gewijzigd wanneer de codering ten slotte is voltooid. Nadat dit bericht beschikbaar is, wordt verwacht dat het versleutelde station met besturingssysteem klaar voor gebruik en de virtuele machine is gereed om te worden gebruikt.

U wordt aangeraden dat u de virtuele machine terug naar de momentopname- of back-up direct vóór de versleuteling wilt herstellen in de volgende gevallen:
   - Als de takenreeks opnieuw opstarten, hierboven, niet het geval.
   - Als u de opstartgegevens, voortgangsbericht of andere foutindicatoren dat versleuteling-besturingssysteem rapporteren is in het midden van dit proces mislukt. Een voorbeeld van een bericht is de fout "het ontkoppelen is mislukt", die in deze handleiding wordt beschreven.

Evalueer de kenmerken van de virtuele machine en zorg ervoor dat aan alle vereisten wordt voldaan voordat u de volgende poging.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Oplossen van problemen met Azure Disk Encryption achter een firewall
Wanneer verbinding wordt beperkt door een firewall of proxy vereiste netwerkinstellingen security group (NSG), kan het vermogen van de extensie noodzakelijke taken worden onderbroken. Deze wordt onderbroken, kan resulteren in statusberichten, zoals "Status van extensie niet beschikbaar op de virtuele machine." In de verwachte scenario's, is de versleuteling niet kan worden voltooid. De volgende secties zijn enkele veelvoorkomende problemen voor de firewall die u mogelijk onderzoeken.

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Network security groepsinstellingen die worden toegepast moeten wel wilt toestaan dat het eindpunt om te voldoen aan de configuratie van de gedocumenteerde [vereisten](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) voor schijfversleuteling.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault achter een firewall
De virtuele machine moet toegang hebben tot een key vault. Raadpleeg de richtlijnen voor toegang tot de key vault achter een firewall die de [Azure Key Vault](../key-vault/key-vault-access-behind-firewall.md) team houdt. 

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service 
De virtuele machine moet toegang hebben tot de [Azure Instance Metadata service](../virtual-machines/windows/instance-metadata-service.md) eindpunt dat gebruikmaakt van een bekende niet-routeerbare IP-adres (`169.254.169.254`) die kunnen worden gebruikt alleen de virtuele machine.

### <a name="linux-package-management-behind-a-firewall"></a>Linux-Pakketbeheer achter een firewall

Tijdens runtime, is Azure Disk Encryption voor Linux afhankelijk van de doel-distributie pakketbeheersysteem benodigde vereiste componenten installeren voordat het inschakelen van versleuteling. Als de firewall-instellingen te voorkomen de virtuele machine kunnen deze onderdelen te downloaden en installeren dat, worden klikt u vervolgens opeenvolgende fouten verwacht. De stappen voor het configureren van deze pakketbeheersysteem kunnen per distributie verschillen. Red Hat, wanneer er een proxy is vereist, moet u ervoor zorgen dat de abonnement-manager en yum zijn juist ingesteld. Zie voor meer informatie, [het oplossen van problemen met abonnement-manager en yum](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Het oplossen van Windows Server 2016 Server Core

Op Windows Server 2016 Server Core, is het onderdeel bdehdcfg niet standaard beschikbaar zijn. Dit onderdeel is vereist voor Azure Disk Encryption. Deze wordt gebruikt voor het splitsen van het systeemvolume van OS-volume, die slechts één keer voor de levensduur van de virtuele machine wordt uitgevoerd. Deze binaire bestanden zijn niet vereist tijdens latere versleutelingsbewerkingen.

Als tijdelijke oplossing voor dit probleem, kopieert u de volgende vier bestanden vanuit een Windows Server 2016-Data Center VM op dezelfde locatie op Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

   2. Voer de volgende opdracht in:

   ```
   bdehdcfg.exe -target default
   ```

   3. Deze opdracht maakt een systeempartitie 550 MB. Het systeem opnieuw opstarten.

   4. Gebruik DiskPart om te controleren van de volumes en ga daarna verder.  

Bijvoorbeeld:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd informatie over enkele veelvoorkomende problemen in Azure Disk Encryption en hoe deze problemen op te lossen. Zie voor meer informatie over deze service en de mogelijkheden ervan, de volgende artikelen:

- [Schijfversleuteling in Azure Security Center toepassen](../security-center/security-center-apply-disk-encryption.md)
- [Azure gegevensversleuteling in rust](azure-security-encryption-atrest.md)

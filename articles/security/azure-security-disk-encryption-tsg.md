---
title: Probleemoplossing voor Azure Disk Encryption | Microsoft Docs
description: Dit artikel bevat tips voor probleemoplossing voor Microsoft Azure Disk Encryption for Windows en Linux IaaS VM's.
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: devtiw
ms.openlocfilehash: c252bc6aee79ad009684f9d3e62c42529c024109
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Encryption probleemoplossingsgids

Deze handleiding is bedoeld voor IT-professionals, gegevensbeveiligingsanalisten en cloudbeheerders die willen Azure Disk Encryption en moeten er richtlijnen voor het oplossen van problemen met schijf-versleuteling-gerelateerde.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Het oplossen van Linux-besturingssysteem schijfversleuteling

Schijfversleuteling van Linux-besturingssysteem (OS) moet de OS-station voordat het wordt uitgevoerd via het versleutelingsproces volledige schijf ontkoppelen. Als de schijf, een foutbericht van deze kan niet ontkoppelen ' kan niet loskoppelen na... " is het waarschijnlijk om te worden uitgevoerd.

Deze fout wordt waarschijnlijk gebeuren wanneer de OS-schijf-versleuteling wordt toegepast op een doelomgeving VM die is gewijzigd of gewijzigd van de ondersteunde voorraad afbeelding. Voorbeelden van afwijkingen van de ondersteunde installatiekopie die leiden tot met de extensie mogelijkheid problemen kan ontkoppelen van het station OS omvatten het volgende:
- Aangepaste installatiekopieën is niet langer overeenkomen met een ondersteund bestandssysteem of partitieschema.
- Grote toepassingen zoals SAP, MongoDB, Apache Cassandra en Docker worden niet ondersteund wanneer ze geïnstalleerd en wordt uitgevoerd in het besturingssysteem vóór versleuteling zijn.  Azure Disk Encryption is niet veilig afsluiten deze processen zoals ter voorbereiding van het station OS voor schijfversleuteling vereist.  Als er nog steeds actieve processen geopende bestandsingangen op het station OS houden, is de OS-station kan niet worden ontkoppeld, wat resulteert in een fout voor het versleutelen van de OS-schijf. 
- Aangepaste scripts die worden uitgevoerd in de buurt sluiten tijd voor de versleuteling is ingeschakeld of als andere wijzigingen worden aangebracht op de virtuele machine tijdens het versleutelingsproces. Dit conflict kan gebeuren als een Azure Resource Manager-sjabloon definieert meerdere extensies tegelijkertijd uitvoeren, of wanneer een extensie voor aangepaste scripts of een andere actie uitvoert gelijktijdig naar schijf codering. Serialiseren en te isoleren van dergelijke stappen mogelijk los het probleem.
- Security Enhanced Linux (SELinux) is niet uitgeschakeld voordat het inschakelen van versleuteling, zodat de stap ontkoppelen is mislukt. SELinux kan worden ingeschakeld nadat de codering is voltooid.
- De OS-schijf gebruikt het schema van een logische Volume Manager (LVM). Hoewel beperkte ondersteuning voor LVM gegevens schijf beschikbaar is, is een besturingssysteemschijf LVM niet.
- Minimale geheugenvereisten wordt niet voldaan (7 GB wordt voorgesteld voor OS schijfversleuteling).
- Schijven worden recursief gekoppeld onder de map /mnt/ of elkaar (bijvoorbeeld /mnt/data1, /mnt/data2, /data3 + /data3/data4).
- Andere Azure Disk Encryption [vereisten](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) voor Linux wordt niet voldaan.

## <a name="unable-to-encrypt"></a>Kan niet worden versleuteld

In sommige gevallen kan is de Linux-schijfversleuteling lijkt te zijn vastgelopen bij 'OS schijfversleuteling gestart' en SSH uitgeschakeld. De versleuteling tussen eindigen op een vooraf gedefinieerde afbeelding 3-16 uur kan duren. Als meerdere terabyte grootte gegevensschijven worden toegevoegd, kan het proces dagen duren.

De volgorde van Linux-besturingssysteem schijf versleuteling ontkoppelt tijdelijk de OS-schijf. Vervolgens wordt uitgevoerd door blok versleuteling van een schijf met het volledige besturingssysteem, voordat deze het remounts in de versleutelde status. In tegenstelling tot Azure Disk Encryption in Windows, is Linux Disk Encryption niet toegestaan voor gelijktijdige gebruik van de virtuele machine terwijl de versleuteling uitgevoerd wordt. De prestatiekenmerken van de virtuele machine kunnen maken van een aanzienlijk verschil zijn in de benodigde tijd voor volledige versleuteling. Deze kenmerken zijn de grootte van de schijf en wordt aangegeven of het opslagaccount is standaard of premium (SSD)-opslag.

De om versleutelingsstatus te controleren, pollen de **ProgressMessage** veld geretourneerd door de [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) opdracht. Terwijl de OS-schijf wordt versleuteld, de virtuele machine de status van een onderhoudsvenster en SSH om te voorkomen dat een onderbreking van het continue proces wordt uitgeschakeld. De **EncryptionInProgress** rapporten voor het merendeel van de tijd weergegeven, terwijl de versleuteling uitgevoerd wordt. Enkele uren later een **VMRestartPending** gevraagd of u de virtuele machine opnieuw opstarten. Bijvoorbeeld:


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

Nadat u de virtuele machine opnieuw opstarten wordt gevraagd en nadat de virtuele machine opnieuw is opgestart, moet u wachten 2-3 minuten voor het opnieuw opstarten en voor de laatste stappen voor het doel moet worden uitgevoerd. De statuswijzigingen bericht wanneer de codering ten slotte is voltooid. Nadat u dit bericht beschikbaar is, wordt verwacht dat het versleutelde station voor OS klaar voor gebruik en de virtuele machine kan opnieuw worden gebruikt.

U wordt aangeraden dat u de virtuele machine weer naar de momentopname- of back terugzetten-up direct vóór versleuteling in de volgende gevallen:
   - Als de volgorde van opnieuw opstarten beschreven eerder niet het geval.
   - Als de opstartgegevens, voortgangsbericht of andere foutindicatoren dat OS-versleuteling rapporteren is in het midden van dit proces mislukt. Een voorbeeld van een bericht is de fout 'kan niet loskoppelen', die in deze handleiding wordt beschreven.

Herzie de kenmerken van de virtuele machine en zorg ervoor dat aan alle vereisten wordt voldaan voordat het opnieuw probeert.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Het oplossen van Azure Disk Encryption achter een firewall
Wanneer verbinding wordt beperkt door een firewall of proxy-vereiste instellingen voor netwerkbeveiliging groep (NSG), kan de mogelijkheid van de uitbreiding benodigde taken worden onderbroken. Deze onderbreking kan leiden tot statusberichten zoals "Status van extensie niet beschikbaar op de virtuele machine." In de verwachte scenario's, is de versleuteling niet kan worden voltooid. De volgende secties hebben enkele veelvoorkomende problemen voor de firewall die u mogelijk onderzoeken.

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Netwerk groep beveiligingsinstellingen die worden toegepast moeten wel wilt toestaan dat het eindpunt om te voldoen aan de gedocumenteerde netwerkconfiguratie [vereisten](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) voor schijfversleuteling.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Sleutelkluis achter een firewall
De virtuele machine moet toegang kunnen krijgen tot een sleutelkluis. Raadpleeg de richtlijnen voor toegang tot de sleutelkluis achter een firewall die de [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) team onderhoudt.

### <a name="linux-package-management-behind-a-firewall"></a>Linux-pakket management achter een firewall

Tijdens runtime, is Azure Disk Encryption voor Linux is afhankelijk van de doel-distributie pakket-beheersysteem voor het installeren van vereiste componenten van de benodigde voordat u versleuteling inschakelt. Als de firewall-instellingen te voorkomen de virtuele machine kunt downloaden en installeren van deze onderdelen dat, worden volgende fouten verwacht. De stappen voor het configureren van dit pakket-beheersysteem kunnen verschillen per distributiepunt. Op Red Hat, wanneer een proxy vereist is, u moet ervoor zorgen dat de abonnement-manager en yum zijn goed ingesteld. Zie voor meer informatie [het oplossen van problemen met de abonnement-manager en yum](https://access.redhat.com/solutions/189533).  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Het oplossen van Windows Server 2016 Server Core

Het onderdeel bdehdcfg is niet standaard beschikbaar op Windows Server 2016 Server Core. Dit onderdeel is vereist voor Azure Disk Encryption. Deze wordt gebruikt voor het splitsen van het systeemvolume van het volume met het besturingssysteem, die slechts één keer voor de levensduur van de virtuele machine wordt uitgevoerd. Deze binaire bestanden zijn niet vereist tijdens versleutelingsbewerkingen hoger.

Als tijdelijke oplossing dit probleem, Kopieer de volgende 4 bestanden van een Windows Server 2016 Data Center VM naar dezelfde locatie op Server Core:

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

   3. Deze opdracht maakt u een systeempartitie 550 MB. Het systeem opnieuw opstarten.

   4. Gebruik DiskPart om te controleren van de volumes en ga verder.  

Bijvoorbeeld:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="troubleshooting-encryption-status"></a>Coderingsstatus probleemoplossing

Als de coderingsstatus van de verwachte komt niet overeen met wat er in de portal wordt gerapporteerd, raadpleegt u het volgende support-artikel: [coderingsstatus ten onrechte wordt weergegeven op de Azure-beheerportal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por)

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd meer informatie over enkele bekende problemen in Azure Disk Encryption en deze problemen oplossen. Zie voor meer informatie over deze service en de mogelijkheden ervan, de volgende artikelen:

- [Schijfversleuteling in Azure Security Center toepassen](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Een virtuele machine van Azure versleutelen](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure gegevensversleuteling in rust](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)

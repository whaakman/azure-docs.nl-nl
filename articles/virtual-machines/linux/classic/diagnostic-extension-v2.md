---
title: Bewaking van een Linux-VM met een VM-extensie | Microsoft Docs
description: Informatie over het gebruik van de Linux Diagnostic-extensie voor het bewaken van de prestaties en diagnostische gegevens van een Linux-VM in Azure.
services: virtual-machines-linux
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: 13d7594c15959661f3f9c3ab2165739719beac07
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308218"
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>De diagnostische Linux-extensie gebruiken voor het controleren van de prestaties en diagnostische gegevens van een Linux VM

Dit document wordt versie 2.3 van de Linux Diagnostic-extensie beschreven.

> [!IMPORTANT]
> Deze versie is verouderd en kan het zijn niet-gepubliceerde elk gewenst moment na 30 juni 2018. Deze is vervangen door versie 3.0. Zie voor meer informatie de [documentatie voor versie 3.0 van de Linux Diagnostic-extensie](../diagnostic-extension.md).

## <a name="introduction"></a>Inleiding

(**Opmerking**: de Linux Diagnostic-extensie is open source op [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) waarop de meest actuele informatie over de uitbreiding voor het eerst wordt gepubliceerd. U wilt controleren de [GitHub-pagina](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) eerste.)

De Linux Diagnostic-extensie kunt u de monitor van een gebruiker de Linux-VM's die worden uitgevoerd op Microsoft Azure. Het heeft de volgende mogelijkheden:

* Verzamelt en uploadt de informatie over de systeemprestaties van de Linux-VM aan de opslagtabel van de gebruiker, met inbegrip van diagnose- en syslog-informatie.
* Hiermee kunnen gebruikers de metrische gegevens voor gegevens die worden verzameld en geüpload aanpassen.
* Hiermee kunnen gebruikers opgegeven logboekbestanden uploaden naar een aangewezen opslagtabel.

In de huidige versie 2.3 bevat de gegevens:

* Alle Linux Rsyslog-Logboeken, met inbegrip van systeem-, beveiligings- en toepassingslogboeken.
* Alle systeemgegevens die opgegeven voor de [de oplossingen van Cross-Platform van System Center-site](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Door gebruiker opgegeven logboekbestanden.

Deze extensie werkt met zowel het klassieke en Resource Manager-implementatiemodel.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Huidige versie van de extensie en de afschaffing van de oude versies

De meest recente versie van de extensie is **2.3**, en **alle oude versies (2.0, 2.1 en 2.2) worden afgeschaft en niet-gepubliceerde einde van dit jaar (2017)**. Als u de Linux Diagnostic-extensie geïnstalleerd met automatische secundaire versie-upgrade is uitgeschakeld, het raadzaam dat u de extensie verwijderen en opnieuw installeren met de upgrade van de automatische secundaire versie is ingeschakeld. Op klassieke virtuele machines (ASM), kunt u dit doen door op te geven '2.*' als de versie als u de extensie via Azure XPLAT CLI of Powershell installeert. Op ARM-VM's, u kunt dit doen door op te nemen ' "autoUpgradeMinorVersion": true' in de sjabloon voor VM-implementatie. Een nieuwe installatie van de extensie moet ook de secundaire versie van automatisch bijwerken is ingeschakeld.

## <a name="enable-the-extension"></a>De extensie inschakelen

U kunt deze uitbreiding inschakelen met behulp van de [Azure-portal](https://portal.azure.com/#), Azure PowerShell of Azure CLI-scripts.

Als u wilt weergeven en configureren van het systeem en de prestaties gegevens rechtstreeks vanuit de Azure portal, gaat u als volgt [deze stappen uit op de Azure-blog](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

In dit artikel richt zich op het inschakelen en configureren van de uitbreiding met behulp van Azure CLI-opdrachten. Hiermee kunt u lezen en de gegevens rechtstreeks vanuit de storage-tabel weergeeft.

Houd er rekening mee dat de van configuratiemethoden die hier worden beschreven, niet voor de Azure-portal werkt. Als u wilt weergeven en configureren van het systeem en de prestaties gegevens rechtstreeks vanuit de Azure portal, moet de extensie worden ingeschakeld via de portal.

## <a name="prerequisites"></a>Vereisten

* **Azure Linux Agent versie 2.0.6 of hoger**.

  Houd er rekening mee dat de meeste Azure VM Linux galerie met installatiekopieën versie 2.0.6 bevatten of hoger. U kunt uitvoeren **WAAgent-versie** om te controleren welke versie is geïnstalleerd op de virtuele machine. Als u een versie die ouder is dan 2.0.6 van de virtuele machine wordt uitgevoerd, kunt u volgen [deze instructies op GitHub](https://github.com/Azure/WALinuxAgent "instructies") bij te werken.
* **Azure CLI**. Ga als volgt [deze richtlijnen voor het installeren van de CLI](../../../cli-install-nodejs.md) voor het instellen van de Azure CLI-omgeving op uw computer. Nadat u Azure CLI is geïnstalleerd, kunt u de **azure** opdracht uit via de opdrachtregelinterface (Bash, Terminal of opdrachtprompt) voor toegang tot de Azure CLI-opdrachten. Bijvoorbeeld:

  * Voer **azure vm extension set--help** voor gedetailleerde help-informatie.
  * Voer **azure-aanmelding** zich aanmeldt bij Azure.
  * Voer **azure vm-lijst** om alle virtuele machines die u in Azure hebt weer te geven.
* Een storage-account voor het opslaan van de gegevens. U moet de naam van een opslagaccount dat eerder is gemaakt en een toegangssleutel voor het uploaden van de gegevens naar uw opslag.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Gebruik de Azure CLI-opdracht uit om de Linux Diagnostic-extensie

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Scenario 1. De extensie met de standaardset van gegevens inschakelen

In versie 2.3 of hoger bevat de standaardgegevens die worden verzameld:

* Alle Rsyslog-gegevens (inclusief system-, beveiligings- en toepassingslogboeken).  
* Een kernset aan systeemgegevens uit te voeren. Houd er rekening mee dat de hele gegevensset wordt beschreven in de [Platformoplossingen voor System Center-Cross-site](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Als u inschakelen van extra gegevens wilt, gaat u verder met de stappen in de scenario's 2 en 3.

Step 1. Maak een bestand met de naam PrivateConfig.json met de volgende inhoud:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

Stap 2. Uitvoeren **azure vm-extensie instellen vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.\* --private-config-path PrivateConfig.json**.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Scenario 2. Aanpassen van de monitor maatstaven voor prestaties

In deze sectie wordt beschreven hoe u de prestaties en diagnostische gegevenstabel aanpassen.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Ook een bestand met de naam PublicConfig.json maken. Geef de specifieke gegevens die u wenst te verzamelen.

Voor alle ondersteunde providers en variabelen verwijzen naar de [Platformoplossingen voor System Center-Cross-site](https://scx.codeplex.com/wikipage?title=xplatproviders). U kunt meerdere query's en op te slaan in meerdere tabellen door meer query's naar het script toe te voegen.

Standaard worden de gegevens voor Rsyslog altijd verzameld.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Stap 2. Voer **azure vm-extensie instellen vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--private-config-path PrivateConfig.json--public-config-path PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Scenario 3. Uploaden van uw eigen logboekbestanden

Deze sectie wordt beschreven hoe u voor het verzamelen en specifieke logboekbestanden uploaden naar uw opslagaccount. U moet zowel het pad naar het logboekbestand en de naam van de tabel waar u voor het opslaan van uw logboek opgeven. U kunt meerdere logboekbestanden maken door meerdere vermeldingen van het bestand /-tabel toe te voegen aan het script.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Vervolgens maakt u een ander bestand met de naam PublicConfig.json met de volgende inhoud:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

Stap 2. Voer `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json` uit.

Houd er rekening mee dat met deze instelling op de extensie versies voorafgaand aan 2.3 alle logboeken geschreven naar `/var/log/mysql.err` kan worden gedupliceerd op `/var/log/syslog` (of `/var/log/messages` , afhankelijk van de Linux-distributie) ook. Als u voorkomen dat deze dubbele logboekregistratie wilt, kunt u logboekregistratie van uitsluiten `local6` faciliteit-Logboeken in uw rsyslog-configuratie. Dat hangt ervan af op de Linux-distributie, maar op een systeem Ubuntu 14.04 het bestand te wijzigen is `/etc/rsyslog.d/50-default.conf` en kunt u de regel vervangen `*.*;auth,authpriv.none -/var/log/syslog` naar `*.*;auth,authpriv,local6.none -/var/log/syslog`. Dit probleem is opgelost in de meest recente versie van de hotfix van 2.3 (2.3.9007), dus hebt u de versie 2.3 van de extensie, dit probleem niet moet worden toegepast. Als deze wel nog steeds bestaat, zelfs na het opnieuw opstarten van uw virtuele machine, contact met ons opnemen en help ons waarom de meest recente hotfixversie niet automatisch wordt geïnstalleerd.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Scenario 4. De extensie van het verzamelen van alle logboeken stoppen

In deze sectie wordt beschreven hoe u de extensie van het verzamelen van Logboeken stoppen. Houd er rekening mee dat het bewakingsproces van de agent nog steeds actief en werkend zelfs met deze herconfiguratie worden. Als u het bewakingsproces van de agent volledig worden gestopt wilt, kunt u dit doen door de uitbreiding uit te schakelen. De opdracht voor het uitschakelen van de extensie is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Maak een ander bestand met de naam PublicConfig.json met de volgende inhoud:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Stap 2. Voer **azure vm-extensie instellen vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*'--private-config-path PrivateConfig.json--public-config-path PublicConfig.json**.

## <a name="review-your-data"></a>Controleer uw gegevens

De prestaties en diagnostische gegevens worden opgeslagen in een Azure Storage-tabel. Beoordeling [hoe u Azure Table Storage gebruiken met Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) voor meer informatie over toegang tot de gegevens in de storage-tabel met behulp van Azure CLI-scripts.

Bovendien kunt u de volgende hulpprogramma's voor de gebruikersinterface voor toegang tot de gegevens:

1. Visual Studio Server Explorer. Ga naar uw storage-account. Nadat de virtuele machine wordt uitgevoerd voor ongeveer vijf minuten, ziet u de vier standaardtabellen: "LinuxCpu", "LinuxDisk", "LinuxMemory" en 'Linuxsyslog'. Dubbelklik op de tabelnamen van de om de gegevens weer te geven.
1. [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").

![image](./media/diagnostic-extension/no1.png)

Als u fileCfg of perfCfg (zoals beschreven in de scenario's 2 en 3) hebt ingeschakeld, kunt u Visual Studio Server Explorer en Azure Storage Explorer gebruiken om niet-standaard gegevens weer te geven.

## <a name="known-issues"></a>Bekende problemen

* De Rsyslog informatie en het logboekbestand opgegeven door de klant alleen toegankelijk via het uitvoeren van scripts.

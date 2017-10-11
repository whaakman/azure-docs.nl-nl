---
title: Bewaking van een Linux-VM met een VM-extensie | Microsoft Docs
description: Informatie over het gebruik van de diagnostische Linux-extensie voor het bewaken van de prestaties en diagnostische gegevens van een Linux-VM in Azure.
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: b8c6e2e22d8478b6e92e7b7942f15d37a840fed3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>De diagnostische Linux-extensie gebruiken voor het controleren van de prestaties en diagnostische gegevens van een Linux VM

Dit document beschrijft versie 2.3 van de diagnostische Linux-uitbreiding.

> [!IMPORTANT]
> Deze versie is afgeschaft en wordt niet gepubliceerd elk gewenst moment na 30 juni 2018. Het is vervangen door versie 3.0. Zie voor meer informatie de [documentatie voor versie 3.0 van de Linux-extensie voor diagnostische](../diagnostic-extension.md).

## <a name="introduction"></a>Inleiding

(**Opmerking**: de diagnostische Linux-uitbreiding is open source op [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) waarop de meest actuele informatie over de uitbreiding voor het eerst wordt gepubliceerd. U wilt controleren de [GitHub-pagina](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) eerste.)

De Linux-extensie voor diagnostische helpt de monitor van een gebruiker de Linux-virtuele machines die worden uitgevoerd op Microsoft Azure. Deze heeft de volgende mogelijkheden:

* Verzamelt en uploadt de informatie over de systeemprestaties van de Linux-VM aan de gebruiker opslag tabel, met inbegrip van diagnose- en syslog-informatie.
* Hiermee kunnen gebruikers de gegevens metrische gegevens die worden verzameld en geüpload aanpassen.
* Hiermee kunnen gebruikers opgegeven logboekbestanden uploaden naar een tabel aangewezen opslag.

In de huidige versie 2.3 omvatten de gegevens:

* Alle Linux Rsyslog-Logboeken, met inbegrip van systeem, beveiliging en toepassingslogboeken.
* Alle systeemgegevens die opgegeven op [de site van System Center Cross-Platform oplossingen](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Door gebruiker opgegeven logboekbestanden.

Deze extensie werkt met zowel het klassieke als Resource Manager-implementatiemodel.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Huidige versie van de extensie en afschaffing van oude versies

De meest recente versie van de uitbreiding is **2.3**, en **eventuele oude versies (2.0, 2.1 en 2.2) wordt afgeschaft en niet-gepubliceerde einde van dit jaar (2017)**. Als u de extensie Linux diagnose met secundaire versie van Automatische upgrade uitgeschakeld hebt geïnstalleerd, het raadzaam dat u de uitbreiding verwijderen en opnieuw installeren met de upgrade van secundaire versie automatisch ingeschakeld. Op klassieke virtuele machines (ASM), kunt u dit doen door op te geven '2.*' als de versie, als u de uitbreiding via Azure XPLAT CLI of Powershell installeert. Op virtuele machines ARM, u kunt dit doen door op te nemen ' 'autoUpgradeMinorVersion': true' in de VM-sjabloon voor implementatie. Een nieuwe installatie van de extensie, moeten ook moet de secundaire versie van Automatische upgrade-optie is ingeschakeld.

## <a name="enable-the-extension"></a>De uitbreiding inschakelen

U kunt deze uitbreiding inschakelen met behulp van de [Azure-portal](https://portal.azure.com/#), Azure PowerShell of Azure CLI-scripts.

Als u wilt weergeven en configureren van de systeem- en gegevens rechtstreeks vanuit de Azure portal, volg [deze stappen uit op de Azure-blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Dit artikel is gericht op het inschakelen en configureren van de uitbreiding met behulp van Azure CLI-opdrachten. Hiermee kunt u om te lezen en de gegevens rechtstreeks vanuit de tabel voor opslag weergeven.

Houd er rekening mee dat de configuratiemethoden die hier worden beschreven, niet voor de Azure-portal werkt. Als u wilt weergeven en configureren van de systeem- en gegevens rechtstreeks vanuit de Azure portal, moet de extensie worden ingeschakeld via de portal.

## <a name="prerequisites"></a>Vereisten

* **Azure Linux Agent versie 2.0.6 of hoger**.

  Opmerking dat de meeste Azure VM Linux-afbeeldingen versie 2.0.6 bevatten of hoger. U kunt uitvoeren **WAAgent-versie** om te controleren welke versie is geïnstalleerd op de virtuele machine. Als u een versie die ouder is dan 2.0.6 van de virtuele machine wordt uitgevoerd, kunt u volgen [deze instructies op GitHub](https://github.com/Azure/WALinuxAgent "instructies") bij te werken.
* **Azure CLI**. Ga als volgt [in deze richtlijnen voor het installeren van de CLI](../../../cli-install-nodejs.md) voor het instellen van de Azure CLI-omgeving op uw computer. Nadat u Azure CLI hebt geïnstalleerd, kunt u de **azure** via de opdrachtregelinterface (Bash, Terminal of een opdrachtprompt) voor toegang tot de Azure CLI-opdrachten. Bijvoorbeeld:

  * Voer **azure vm-extensie set--help** voor gedetailleerde help-informatie.
  * Voer **azure-aanmelding** aan te melden bij Azure.
  * Voer **azure vm lijst** voor een lijst met alle virtuele machines die u op Azure hebt.
* Een opslagaccount voor het opslaan van de gegevens. U moet de naam van een account dat eerder is gemaakt en een toegangssleutel voor het uploaden van de gegevens naar uw opslag.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Gebruik de Azure CLI-opdracht voor het Linux-extensie voor diagnostische inschakelen

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Scenario 1. De uitbreiding met de standaardgegevensset inschakelen

In versie 2.3 of hoger bevat de gegevens die worden verzameld:

* Alle Rsyslog gegevens (inclusief systeem, beveiliging en toepassingslogboeken).  
* Een kernset aan de basisgegevens van het systeem. Let op: de volledige gegevensset wordt beschreven op de [System Center Cross-Platform oplossingen site](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Als u extra gegevens inschakelen wilt, gaat u verder met de stappen in scenario's 2 en 3.

Step 1. Maak een bestand met de naam PrivateConfig.json met de volgende inhoud:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

Stap 2. Voer  **azure vm-extensie ingesteld vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --persoonlijke configuratiepad PrivateConfig.json**.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Scenario 2. Aanpassen van de monitor maatstaven voor prestaties

Deze sectie beschrijft het aanpassen van de prestaties en diagnostische gegevenstabel.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Ook een bestand met de naam PublicConfig.json maken. Geef de specifieke gegevens die u wenst te verzamelen.

Voor alle ondersteunde providers en variabelen te verwijzen naar de [System Center Cross-Platform oplossingen site](https://scx.codeplex.com/wikipage?title=xplatproviders). U kunt meerdere query's en deze opslaan op meerdere tabellen door meer query's naar het script toe te voegen.

Standaard wordt altijd de Rsyslog gegevens verzameld.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Stap 2. Voer  **azure vm-extensie ingesteld vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--persoonlijke configuratiepad PrivateConfig.json--openbare configuratiepad PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Scenario 3. Uploaden van uw eigen logboekbestanden

Deze sectie wordt beschreven hoe verzamelen en specifieke logboekbestanden uploaden naar uw storage-account. U moet zowel het pad naar het logboekbestand en de naam van de tabel waar u wilt opslaan van uw logboek opgeven. U kunt meerdere logboekbestanden maken door meerdere bestandstabel/vermeldingen toe te voegen aan het script.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Maak een ander bestand met de naam PublicConfig.json met de volgende inhoud:

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

Houd er rekening mee dat met deze instelling op de extensie versies voorafgaand aan 2.3 alle logboeken geschreven naar `/var/log/mysql.err` om te kunnen worden gedupliceerd `/var/log/syslog` (of `/var/log/messages` , afhankelijk van de Linux-distro) ook. Als u voorkomen dat deze dubbele logboekregistratie wilt, kunt u het vastleggen van uitsluiten `local6` faciliteit wordt geregistreerd in de configuratie van uw rsyslog. Dit is afhankelijk van de Linux-distro, maar op een systeem Ubuntu 14.04 het bestand te wijzigen is `/etc/rsyslog.d/50-default.conf` en kunt u de regel vervangen `*.*;auth,authpriv.none -/var/log/syslog` naar `*.*;auth,authpriv,local6.none -/var/log/syslog`. Dit probleem is opgelost in de nieuwste versie van de hotfix van 2.3 (2.3.9007), dus als u de versie van de uitbreiding 2.3 hebt, dit probleem niet moet gebeuren. Als dit wel nog steeds zelfs na het opnieuw opstarten van uw virtuele machine, contact met ons opnemen en help ons waarom de meest recente hotfixversie niet automatisch wordt geïnstalleerd.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Scenario 4. De extensie van de logboekbestanden verzamelen stoppen

Deze sectie beschrijft hoe u de uitbreiding van het verzamelen van Logboeken. Houd er rekening mee dat het bewakingsproces van de agent wordt nog steeds actief en werkend zelfs met deze herconfiguratie. Als u het bewakingsproces van agent volledig worden gestopt wilt, kunt u dit doen door de uitbreiding uit te schakelen. De opdracht de uitbreiding uitschakelen is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

Step 1. Maak een bestand met de naam PrivateConfig.json met de inhoud die is beschreven in Scenario 1. Maak een ander bestand met de naam PublicConfig.json met de volgende inhoud:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Stap 2. Voer  **azure vm-extensie ingesteld vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--persoonlijke configuratiepad PrivateConfig.json--openbare configuratiepad PublicConfig.json**.

## <a name="review-your-data"></a>Controleer uw gegevens

De prestaties en diagnostische gegevens worden opgeslagen in een tabel met Azure Storage. Bekijk [hoe Azure Table Storage gebruiken met Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) voor meer informatie over hoe u toegang tot de gegevens in de tabel opslag met behulp van Azure CLI-scripts.

U kunt bovendien na UI-hulpprogramma's gebruiken voor toegang tot de gegevens:

1. Visual Studio Server Explorer. Ga naar uw storage-account. Nadat de virtuele machine is uitgevoerd gedurende vijf minuten, ziet u de tabellen vier standaard: 'LinuxCpu', 'LinuxDisk', 'LinuxMemory' en 'Linuxsyslog'. Dubbelklik op de tabelnamen om de gegevens weer te geven.
1. [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/ "Azure Opslagverkenner").

![Afbeelding](./media/diagnostic-extension/no1.png)

Als u fileCfg of perfCfg (zoals beschreven in scenario's 2 en 3) hebt ingeschakeld, kunt u Visual Studio Server Explorer en Azure Storage Explorer gebruiken om niet-standaard gegevens te bekijken.

## <a name="known-issues"></a>Bekende problemen

* De Rsyslog informatie en de klant opgegeven logboekbestand kunnen alleen worden benaderd via scripts.

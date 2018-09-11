---
title: Azure Compute - Linux Diagnostic-extensie | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de Azure Linux diagnostische extensie (LAD) voor het verzamelen van metrische gegevens en gebeurtenissen van virtuele Linux-machines uitvoeren in Azure.
services: virtual-machines-linux
author: abhijeetgaiha
manager: sankalpsoni
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: agaiha
ms.openlocfilehash: a51e4e61b8d0a9f9a80acc513dbba32c74372f5e
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348851"
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a>Linux Diagnostic-extensie gebruiken om te controleren, logboeken en metrische gegevens

Dit document beschrijft versie 3.0 en nieuwere van de Linux Diagnostic-extensie.

> [!IMPORTANT]
> Zie voor meer informatie over versie 2.3 en ouder [dit document](../linux/classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Inleiding

De Linux Diagnostic-extensie kunt u een gebruiker monitor de status van een Linux VM wordt uitgevoerd op Microsoft Azure. Het heeft de volgende mogelijkheden:

* Verzamelt metrische gegevens voor prestaties system van de virtuele machine en slaat ze op in een specifieke tabel in een aangewezen storage-account.
* Gebeurtenissen vastleggen in logboek van syslog opgehaald en opgeslagen in een specifieke tabel in het opgegeven opslagaccount.
* Hiermee kunnen gebruikers de metrische gegevens voor gegevens die zijn verzameld en geüpload aanpassen.
* Hiermee kunnen gebruikers om aan te passen de syslog-faciliteiten en de ernst van de gebeurtenissen die zijn verzameld en geüpload.
* Hiermee kunnen gebruikers opgegeven logboekbestanden uploaden naar een aangewezen opslagtabel.
* Biedt ondersteuning voor metrische gegevens en logboekbestanden gebeurtenissen te verzenden naar willekeurige Event hub-eindpunten en blobs JSON-indeling in het opgegeven opslagaccount.

Deze extensie werkt met beide Azure-implementatiemodellen.

## <a name="installing-the-extension-in-your-vm"></a>Installeren van de extensie in uw virtuele machine

U kunt deze uitbreiding inschakelen met behulp van de Azure PowerShell-cmdlets, Azure CLI-scripts of sjablonen voor Azure-implementatie. Zie voor meer informatie, [extensies functies](features-linux.md).

De Azure-portal kan niet worden gebruikt in-of LAD 3.0 configureren. In plaats daarvan installeert en configureert deze versie 2.3. Azure portal grafieken en waarschuwingen kunt u werken met gegevens van beide versies van de extensie.

Deze installatie-instructies en een [downloadbare voorbeeldconfiguratie](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0 te configureren:

* vastleggen en opslaan van de dezelfde metrische gegevens zijn verstrekt door LAD 2.3;
* vastleggen van een nuttige set bestand system metrische gegevens, nieuwe LAD 3.0;
* de syslog-verzameling van het standaard ingeschakeld door LAD 2.3; vastleggen
* Schakel in de Azure portal-ervaring voor grafieken en waarschuwingen voor metrische gegevens van virtuele machine.

De configuratie van de downloadbare is slechts een voorbeeld; deze aan de behoeften van uw eigen behoeften aanpassen.

### <a name="prerequisites"></a>Vereiste onderdelen

* **Azure Linux Agent versie 2.2.0 of hoger**. De meeste Azure VM Linux galerie met installatiekopieën bevatten versie 2.2.7 of hoger. Voer `/usr/sbin/waagent -version` om te controleren welke versie is geïnstalleerd op de virtuele machine. Als de virtuele machine wordt uitgevoerd een oudere versie van de guest-agent, voert u de [deze instructies](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) bij te werken.
* **Azure CLI**. [Instellen van de Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) omgeving op uw computer.
* De opdracht wget, als u deze nog niet hebt: uitvoeren `sudo apt-get install wget`.
* Een bestaande Azure-abonnement en een bestaand opslagaccount in deze gegevens worden opgeslagen.
* Lijst met ondersteunde Linux-distributies is ingeschakeld https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic#supported-linux-distributions

### <a name="sample-installation"></a>Voorbeeld-installatie

Vul in de juiste parameters op de eerste drie regels, en vervolgens voert dit script als root:

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login to Azure first before anything else
az login

# Select the subscription containing the storage account
az account set --subscription <your_azure_subscription_id>

# Download the sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build the VM resource ID. Replace storage account name and resource ID in the public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build the protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 2037-12-31T23:59:00Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

De URL voor de voorbeeldconfiguratie en de inhoud ervan, zijn onderhevig aan wijzigingen. Een kopie van de portalinstellingen JSON-bestand downloaden en aan te passen aan uw behoeften. Alle sjablonen of automatisering van u moet uw eigen exemplaar in plaats van downloaden die URL telkens wanneer te gebruiken.

### <a name="updating-the-extension-settings"></a>De extensie-instellingen bijwerken

Nadat u uw beveiligde of openbare instellingen hebt gewijzigd, implementeert u deze aan de virtuele machine met de dezelfde opdracht. Als gewijzigd in de instellingen, worden de bijgewerkte instellingen worden verzonden naar de extensie. LAD opnieuw geladen de configuratie en zichzelf opnieuw is opgestart.

### <a name="migration-from-previous-versions-of-the-extension"></a>Migratie van vorige versies van de extensie

De meest recente versie van de extensie is **3.0**. **Alle oude versies (2.x) zijn afgeschaft en is mogelijk niet-gepubliceerde op of na 31 juli 2018**.

> [!IMPORTANT]
> Deze extensie introduceert belangrijke wijzigingen aan de configuratie van de extensie. Één van deze wijziging is doorgevoerd ter verbetering van de beveiliging van de extensie. Als gevolg hiervan kan achterwaartse compatibiliteit met 2.x niet worden beheerd. De uitgever van de extensie voor deze extensie is ook anders dan de uitgever voor de 2.x-versies.
>
> Als u wilt migreren van 2.x naar deze nieuwe versie van de extensie, moet u de oude extensie (onder de naam van de oude uitgever) verwijderen en vervolgens versie 3 van de extensie installeren.

Aanbevelingen:

* Installeer de extensie met automatische secundaire versie-upgrade is ingeschakeld.
  * Geef op het klassieke implementatiemodel VM's, '3.*' als de versie als u de extensie via Azure XPLAT CLI of Powershell installeert.
  * Op Azure Resource Manager deployment model voor virtuele machines, voeg ' "autoUpgradeMinorVersion": true' in de sjabloon voor VM-implementatie.
* Gebruik een nieuwe/andere storage-account voor LAD 3.0. Er zijn enkele kleine compatibiliteitsproblemen tussen LAD 2.3 en LAD 3.0 waaruit een lastige account delen:
  * LAD 3.0 slaat syslog-gebeurtenissen in een tabel met een andere naam.
  * De counterSpecifier tekenreeksen voor `builtin` metrische gegevens in LAD 3.0 verschillen.

## <a name="protected-settings"></a>Beveiligde instellingen

Deze set van configuratie-informatie bevat gevoelige informatie die moet worden beveiligd tegen openbaar weer te geven, bijvoorbeeld storage-referenties. Deze instellingen worden verzonden naar en opgeslagen door de uitbreiding in versleutelde vorm.

```json
{
    "storageAccountName" : "the storage account to receive data",
    "storageAccountEndPoint": "the hostname suffix for the cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Naam | Waarde
---- | -----
storageAccountName | De naam van het opslagaccount waarin gegevens worden geschreven door de extensie.
storageAccountEndPoint | (optioneel) Het eindpunt voor het identificeren van de cloud waarin het opslagaccount bestaat. Als deze instelling niet aanwezig is, de openbare cloud van Azure, LAD standaard `https://core.windows.net`. Voor het gebruik van een opslagaccount in Azure Duitsland, Azure Government or Azure China, moet u deze waarde dienovereenkomstig ingesteld.
storageAccountSasToken | Een [Account-SAS-token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) voor services Blob en tabel (`ss='bt'`), geldt voor containers en objecten (`srt='co'`), welke verleent toevoegen, maken, weergeven, bijwerken en schrijfmachtigingen heeft (`sp='acluw'`). Voer *niet* omvatten het toonaangevende-vraagteken (?).
mdsdHttpProxy | (optioneel) HTTP-proxy-informatie die nodig zijn voor de extensie inschakelen voor het verbinding maken met de opgegeven opslagaccount en het eindpunt.
sinksConfig | (optioneel) Details van de alternatieve doelen waarvoor metrische gegevens en gebeurtenissen kunnen worden geleverd. De details van elke gegevenssink ondersteund door de extensie worden behandeld in de volgende secties.


> [!NOTE]
> Bij het implementeren van de uitbreiding met een Azure-implementatiesjabloon, het opslagaccount en SAS-token van tevoren gemaakt en vervolgens doorgegeven aan de sjabloon. U kunt geen implementeren van een virtuele machine, opslagaccount en configureren van de uitbreiding in één sjabloon. Het maken van een SAS-token in een sjabloon wordt momenteel niet ondersteund.

U kunt gemakkelijk de vereiste SAS-token via Azure portal maken.

1. Selecteer de opslagaccount die u wilt dat de extensie te schrijven
1. Selecteer 'Shared access signature"van de instellingen voor een deel van het menu links
1. Controleer de toepasselijke rubrieken zoals eerder beschreven.
1. Klik op de knop 'Generate-SAS'.

![image](./media/diagnostics-linux/make_sas.png)

Kopieer de gegenereerde SAS in het veld storageAccountSasToken. verwijderen van het toonaangevende-vraagteken ("?").

### <a name="sinksconfig"></a>sinksConfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Deze optionele sectie definieert aanvullende bestemmingen waarnaar de extensie verzendt de gegevens die worden verzameld. De matrix 'sink' bevat een object voor elke aanvullende gegevens-sink. Het kenmerk "type" bepaalt de andere kenmerken in het object.

Element | Waarde
------- | -----
naam | Een tekenreeks die wordt gebruikt om te verwijzen naar deze sink elders in de configuratie van de extensie.
type | Het type van sink wordt gedefinieerd. Bepaalt de andere waarden (indien aanwezig) in de exemplaren van dit type.

Versie 3.0 van de Linux Diagnostic-extensie ondersteunt twee typen van de sink: Event hub en JsonBlob.

#### <a name="the-eventhub-sink"></a>De EventHub-sink

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

De vermelding 'sasURL' bevat de volledige URL, met inbegrip van SAS-token voor de Event Hub waarnaar de gegevens moet worden gepubliceerd. LAD vereist een SAS een naamgevingsbeleid waarmee de verzenden claim. Een voorbeeld:

* Maak een Event Hubs-naamruimte met de naam `contosohub`
* Een Event Hub maken in de naamruimte met de naam `syslogmsgs`
* Een gedeeld toegangsbeleid maken in de Event Hub met de naam `writer` waarmee de claim verzenden

Als u hebt gemaakt een SAS goede tot middernacht UTC vanaf 1 januari 2018, is de waarde sasURL mogelijk:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Zie voor meer informatie over het genereren van SAS-tokens voor Event Hubs [deze webpagina](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="the-jsonblob-sink"></a>De JsonBlob-sink

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

Gegevens die zijn omgeleid naar een sink JsonBlob worden opgeslagen in blobs in Azure storage. Elk exemplaar van LAD maakt u een blob elk uur voor de naam van elke sink. Elke blob bevat altijd een syntactisch geldige JSON-matrix van object. Atomisch worden nieuwe vermeldingen toegevoegd aan de matrix. BLOBs worden opgeslagen in een container met dezelfde naam als de sink. De Azure storage-regels voor namen van de blob-containers van toepassing op de namen van JsonBlob sinks: tussen 3 en 63 kleine alfanumerieke ASCII-tekens en streepjes bevatten.

## <a name="public-settings"></a>Openbare-instellingen

Deze structuur bevat verschillende blokken met instellingen die invloed hebben op de informatie die door de extensie worden verzameld. Elke instelling is optioneel. Als u opgeeft `ladCfg`, moet u ook opgeven `StorageAccount`.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "the storage account to receive data",
    "mdsdHttpProxy" : ""
}
```

Element | Waarde
------- | -----
Opslagaccount | De naam van het opslagaccount waarin gegevens worden geschreven door de extensie. Moet dezelfde naam als is opgegeven in de [beveiligde instellingen](#protected-settings).
mdsdHttpProxy | (optioneel) Hetzelfde als in de [beveiligde instellingen](#protected-settings). De waarde van de openbare wordt overschreven door het privé-waarde als instellen. Proxy-instellingen met een geheim, zoals een wachtwoord, in plaats de [beveiligde instellingen](#protected-settings).

De resterende elementen worden beschreven in de volgende secties.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

Deze besturingselementen optioneel structuur PUT voor het verzamelen van metrische gegevens en logboeken voor de levering van de service Azure metrische gegevens en andere gegevens. U moet opgeven of `performanceCounters` of `syslogEvents` of beide. Moet u de `metrics` structuur.

Element | Waarde
------- | -----
eventVolume | (optioneel) Hiermee bepaalt u het aantal partities in de storage-tabel gemaakt. U moet een van de `"Large"`, `"Medium"`, of `"Small"`. Indien niet opgegeven, wordt de standaardwaarde is `"Medium"`.
sampleRateInSeconds | (optioneel) Het standaardinterval tussen verzameling van onbewerkte (unaggregated) metrische gegevens. De kleinste ondersteunde samplefrequentie is 15 seconden. Indien niet opgegeven, wordt de standaardwaarde is `15`.

#### <a name="metrics"></a>metrische gegevens

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Element | Waarde
------- | -----
resourceId | De Azure Resource Manager resource-ID van de virtuele machine of van de virtuele-machineschaalset ingesteld op de virtuele machine deel uitmaakt. Deze instelling moet ook opgegeven als een sink JsonBlob wordt gebruikt in de configuratie.
scheduledTransferPeriod | De frequentie waarmee cumulatieve metrische gegevens moeten worden berekend en overgedragen naar metrische gegevens voor Azure, uitgedrukt als een tijdsinterval IS 8601. De kleinste overdracht-periode is 60 seconden, dat wil zeggen, PT1M. U moet ten minste één scheduledTransferPeriod opgeven.

Voorbeelden van de metrische gegevens opgegeven in de sectie performanceCounters worden verzameld elke 15 seconden of beoordelen expliciet in het voorbeeld voor het prestatiemeteritem gedefinieerd. Als meerdere scheduledTransferPeriod frequenties wordt weergegeven (zoals in het voorbeeld), wordt elke aggregatie wordt afzonderlijk berekend.

#### <a name="performancecounters"></a>PerformanceCounters

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Deze optionele sectie bepaalt u het verzamelen van metrische gegevens. Onbewerkte voorbeelden worden samengevoegd voor elk [scheduledTransferPeriod](#metrics) voor het produceren van deze waarden:

* gemiddelde
* minimum
* maximum
* laatste verzameld waarde
* telling van onbewerkte voorbeelden die worden gebruikt voor het berekenen van de statistische functie

Element | Waarde
------- | -----
sinks | (optioneel) Een door komma's gescheiden lijst met namen van sinks welke LAD verzendt metrische resultaten samengevoegde. Alle samengevoegde metrische gegevens worden gepubliceerd naar elke vermelde sink. Zie [sinksConfig](#sinksconfig). Voorbeeld: `"EHsink1, myjsonsink"`.
type | Hiermee geeft u de werkelijke provider van de metrische gegevens.
klasse | Hiermee geeft u de specifieke metrische gegevens in de naamruimte van de provider, samen met 'item'.
Teller | Samen met "class", identificeert de specifieke metrische gegevens in de naamruimte van de provider.
counterSpecifier | Hiermee geeft u de specifieke metrische gegevens in de naamruimte van de metrische gegevens van Azure.
voorwaarde | (optioneel) Hiermee selecteert u een specifiek exemplaar van het object waaraan de metrische gegevens is van toepassing, of selecteert de aggregatie voor alle instanties van dat object. Zie voor meer informatie de [ `builtin` metrische definities van](#metrics-supported-by-builtin).
sampleRate | Hiermee stelt u de snelheid waarmee onbewerkte voorbeelden voor deze metrische gegevens worden verzameld 8601 interval IS. Als niet is ingesteld, het interval voor gegevensverzameling is ingesteld door de waarde van [sampleRateInSeconds](#ladcfg). De kortste ondersteunde samplefrequentie is 15 seconden (PT15S).
eenheid | Moet een van deze tekenreeksen: "Aantal", 'Bytes', 'Seconden', "Percentage", "CountPerSecond", "BytesPerSecond", 'Milliseconde'. Hiermee definieert u de eenheid voor de metrische gegevens. Gebruikers van de verzamelde gegevens verwacht dat de waarden van de verzamelde gegevens zodat deze overeenkomt met deze eenheid. LAD negeert dit veld.
displayName | Het label (in de taal die is opgegeven door de bijbehorende landinstelling) moet worden gekoppeld aan deze gegevens in Azure metrische gegevens. LAD negeert dit veld.

De counterSpecifier is een identifier. Consumenten van metrische gegevens, zoals de Azure portal voor grafieken en waarschuwingen functie counterSpecifier gebruiken als de "sleutel" waarmee een metrische waarde of een exemplaar van een metrische waarde. Voor `builtin` metrische gegevens, raden wij aan u counterSpecifier waarden die met beginnen `/builtin/`. Als u een specifiek exemplaar van metrische gegevens verzamelt, raden wij dat u de id van het exemplaar koppelen aan de counterSpecifier-waarde. Een aantal voorbeelden:

* `/builtin/Processor/PercentIdleTime` -Niet-actieve tijd met een gemiddelde van alle vcpu 's
* `/builtin/Disk/FreeSpace(/mnt)` -Vrije ruimte voor het bestandssysteem mnt
* `/builtin/Disk/FreeSpace` -Vrije ruimte met een gemiddelde van alle gekoppelde bestandssystemen

LAD noch de Azure-portal wordt verwacht dat de waarde die counterSpecifier moet overeenkomen met een patroon. Wees consequent hoe u counterSpecifier waarden samenstelt.

Wanneer u opgeeft `performanceCounters`, LAD altijd schrijft gegevens naar een tabel in Azure storage. U kunt dezelfde gegevens geschreven naar JSON-blobs en/of Event Hubs, maar kunt u het opslaan van gegevens naar een tabel niet uitschakelen. Alle exemplaren van de diagnostische extensie die is geconfigureerd voor het gebruik van dezelfde naam van het opslagaccount en eindpunt hun logboeken en metrische gegevens toevoegen aan de dezelfde tabel. Als er te veel VM's schrijft naar dezelfde tabelpartitie, Azure kan worden gebruikt voor het beperken van schrijfbewerkingen naar de betreffende partitie. De instelling eventVolume zorgt ervoor dat gegevens worden verspreid over 1 (klein), 10 (gemiddeld) of 100 (groot) verschillende partities. 'Gemiddeld' is meestal voldoende om te controleren of verkeer is niet beperkt. De functie Azure metrische gegevens van de Azure-portal gebruikt de gegevens in deze tabel voor het produceren van grafieken of voor het activeren van waarschuwingen. Naam van de tabel is de samenvoeging van deze tekenreeksen:

* `WADMetrics`
* De 'scheduledTransferPeriod' voor de geaggregeerde waarden die zijn opgeslagen in de tabel
* `P10DV2S`
* Een datum in de vorm "JJJJMMDD', waarmee elke 10 dagen

Voorbeelden zijn onder meer `WADMetricsPT1HP10DV2S20170410` en `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEvents

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Deze optionele sectie bepaalt u het verzamelen van gebeurtenissen van syslog. Als de sectie wordt weggelaten, worden op alle syslog-gebeurtenissen niet vastgelegd.

De verzameling syslogEventConfiguration heeft een vermelding voor elke syslog-faciliteit van belang zijn. Als minSeverity 'NONE' voor een bepaalde opslagruimte, of als deze functie wordt niet weergegeven in het element helemaal, worden er zijn geen gebeurtenissen van deze faciliteit vastgelegd.

Element | Waarde
------- | -----
sinks | Een door komma's gescheiden lijst met namen van sinks waarop afzonderlijke gebeurtenissen worden gepubliceerd. Alle gebeurtenissen die overeenkomen met de beperkingen in syslogEventConfiguration worden gepubliceerd naar elke vermelde sink. Voorbeeld: "EHforsyslog"
voorziening %{facilityname/ | De naam van een syslog-faciliteit (zoals "LOG\_gebruiker ' of ' LOG\_LOCAL0 '). Zie de sectie 'faciliteit' van de [syslog man pagina](http://man7.org/linux/man-pages/man3/syslog.3.html) voor een volledige lijst.
minSeverity | Een ernstniveau syslog (zoals "LOG\_ERR ' of ' LOG\_INFO '). Zie de sectie 'niveau' van de [syslog man pagina](http://man7.org/linux/man-pages/man3/syslog.3.html) voor een volledige lijst. De extensie bevat gebeurtenissen die worden verzonden naar de faciliteit of een hoger niveau van de opgegeven.

Wanneer u opgeeft `syslogEvents`, LAD altijd schrijft gegevens naar een tabel in Azure storage. U kunt dezelfde gegevens geschreven naar JSON-blobs en/of Event Hubs, maar kunt u het opslaan van gegevens naar een tabel niet uitschakelen. Het partitioneren gedrag voor deze tabel is dezelfde als beschreven voor `performanceCounters`. Naam van de tabel is de samenvoeging van deze tekenreeksen:

* `LinuxSyslog`
* Een datum in de vorm "JJJJMMDD', waarmee elke 10 dagen

Voorbeelden zijn onder meer `LinuxSyslog20170410` en `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Deze optionele sectie Hiermee bepaalt u de uitvoering van willekeurige [OMI](https://github.com/Microsoft/omi) query's.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Element | Waarde
------- | -----
naamruimte | (optioneel) De OMI-naamruimte waarin de query moet worden uitgevoerd. Als u niets opgeeft, de standaardwaarde is "root/scx", uitgevoerd door de [System Center platformoverschrijdende Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
query | De OMI-query moet worden uitgevoerd.
tabel | (optioneel) De Azure storage-tabel in de aangewezen storage-account (Zie [beveiligde instellingen](#protected-settings)).
frequency | (optioneel) Het aantal seconden tussen het uitvoeren van de query. Standaardwaarde is 300 (5 minuten); minimumwaarde is 15 seconden.
sinks | (optioneel) Een door komma's gescheiden lijst met namen van aanvullende sinks waarmee onbewerkte voorbeeld metrische resultaten moeten worden gepubliceerd. Geen aggregatie van deze onbewerkte voorbeelden wordt berekend door de extensie of metrische gegevens van Azure.

De 'tabel' of 'sinks', of beide moeten worden opgegeven.

### <a name="filelogs"></a>fileLogs

Hiermee bepaalt u het vastleggen van logboekbestanden. LAD nieuwe regels vastgelegd als ze worden geschreven naar het bestand en schrijft deze naar de tabelrijen en/of alle opgegeven sinks (JsonBlob of Event hub).

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Element | Waarde
------- | -----
bestand | De volledige padnaam van het logboekbestand moet worden gevolgd en vastgelegd. De padnaam moet één bestand; de naam het kan de naam van een map of jokertekens bevatten.
tabel | (optioneel) De Azure storage-tabel in de aangewezen storage-account (zoals aangegeven in de configuratie van de beveiligde), waarin u nieuwe regels van de 'staart"van het bestand worden geschreven.
sinks | (optioneel) Een door komma's gescheiden lijst met namen van aanvullende sinks aan welke regels log is verzonden.

De 'tabel' of 'sinks', of beide moeten worden opgegeven.

## <a name="metrics-supported-by-the-builtin-provider"></a>Metrische gegevens die worden ondersteund door de provider builtin

De ingebouwde metrische provider is een bron van metrische gegevens over de meest interessante aan een breed scala aan gebruikers. Deze metrische gegevens kunnen worden onderverdeeld in vijf brede klassen:

* Processor
* Geheugen
* Netwerk
* Bestandssysteem
* Schijf

### <a name="builtin-metrics-for-the-processor-class"></a>ingebouwde metrische gegevens voor de Processor-klasse

De klasse van de Processor van metrische gegevens biedt informatie over het processorgebruik in de virtuele machine. Bij het samenvoegen van percentages, is het resultaat van het gemiddelde over alle CPU's. In een twee-vCPU-virtuele machine als één vCPU 100% bezig is en de andere 100% is niet actief is, is zou de gerapporteerde PercentIdleTime dit 50. Als elke vCPU 50% voor dezelfde periode bezet is, zou het gerapporteerde resultaat ook 50 zijn. In een vier-vCPU-virtuele machine, met 100% één vCPU bezet en de andere niet-actieve, zou de gerapporteerde PercentIdleTime 75 zijn.

Teller | Betekenis
------- | -------
PercentIdleTime | Percentage tijd tijdens de aggregatie dat processors zijn worden gebruikt voor het uitvoeren van de niet-actieve kernel-lus
PercentProcessorTime | Percentage van de tijd voor het uitvoeren van een niet-inactieve thread
PercentIOWaitTime | Percentage tijd wachten op i/o-bewerkingen zijn voltooid
PercentInterruptTime | Percentage van de tijd voor het uitvoeren van hardware/software-interrupts en DPC's (uitgestelde procedureaanroepen)
PercentUserTime | Van niet-actieve tijd in het venster aggregatie, de hoeveelheid tijd die is doorgebracht in gebruiker meer met normale prioriteit
PercentNiceTime | Niet-actieve tijd, het percentage aan verlaagde (goed) prioriteit uitgegeven
PercentPrivilegedTime | Niet-actieve tijd, het percentage besteed in de modus van bevoegdheden (kernel)

De eerste vier items moeten oplopen tot 100%. De laatste drie items ook som en 100%. ze onderverdelen de som van PercentProcessorTime PercentIOWaitTime en PercentInterruptTime.

Als u wilt één metrische waarde samengevoegd voor alle processors, stel `"condition": "IsAggregate=TRUE"`. Instellen om op te halen van een metrische waarde voor een specifieke, zoals de tweede logische processor van een VM vier vCPU `"condition": "Name=\\"1\\""`. Logische processor getallen zijn in het bereik `[0..n-1]`.

### <a name="builtin-metrics-for-the-memory-class"></a>ingebouwde metrische gegevens voor de klasse geheugen

De klasse geheugen van metrische gegevens bevat informatie over geheugengebruik, voor het wisselbestand en wisselen.

Teller | Betekenis
------- | -------
AvailableMemory | Beschikbaar fysiek geheugen in MiB
PercentAvailableMemory | Beschikbaar fysiek geheugen als percentage van Totaal geheugen
UsedMemory | Gebruik fysiek geheugen (MiB)
PercentUsedMemory | Fysiek geheugen in gebruik als een percentage van Totaal geheugen
PagesPerSec | Totaal wisselbestand (lezen/schrijven)
PagesReadPerSec | Pagina's lezen uit een back-up store (wisselbestand, programmabestand, toegewezen bestand, enz.)
PagesWrittenPerSec | Pagina's die zijn geschreven naar externe opslag (wisselbestand, toegewezen bestand, enz.)
AvailableSwap | Niet-gebruikte wisselruimte (MiB)
PercentAvailableSwap | Niet-gebruikte wisselruimte als percentage van de totale wisselruimte
UsedSwap | Gebruikte wisselruimte (MiB)
PercentUsedSwap | Gebruikte wisselruimte als percentage van de totale wisselruimte

Deze klasse van metrische gegevens heeft slechts één exemplaar. Het voorwaardekenmerk '' heeft geen nuttig instellingen en moet worden weggelaten.

### <a name="builtin-metrics-for-the-network-class"></a>ingebouwde metrische gegevens voor de netwerk-klasse

De netwerk-klasse van metrische gegevens bevat informatie over activiteit op het netwerk op een afzonderlijke netwerkinterfaces sinds het opstarten. LAD maakt bandbreedte metrische gegevens, die kan worden opgehaald van metrische gegevens voor hosts niet beschikbaar.

Teller | Betekenis
------- | -------
BytesTransmitted | Totaal aantal bytes dat is verzonden sinds opstarten
BytesReceived | Totaal aantal bytes dat is ontvangen sinds opstarten
BytesTotal | Totaal aantal bytes verzonden of ontvangen sinds opstarten
PacketsTransmitted | Totale aantal pakketten dat is verzonden sinds opstarten
PacketsReceived | Totale aantal pakketten dat is ontvangen sinds opstarten
TotalRxErrors | Aantal ontvangen ze foutberichten sinds opstarten
TotalTxErrors | Aantal fouten sinds opstarten verzenden
TotalCollisions | Aantal conflicten die zijn gerapporteerd door de netwerkpoorten sinds opstarten

 Hoewel deze klasse wordt verwezen, is LAD ondersteunt geen vastleggen netwerk metrische gegevens die worden samengevoegd voor alle netwerkapparaten. Instellen om op te halen van de metrische gegevens voor een specifieke interface, zoals eth0, `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-the-filesystem-class"></a>ingebouwde metrische gegevens voor de klasse bestandssysteem

De klasse bestandssysteem van metrische gegevens biedt informatie over het gebruik van bestandssysteem. Absolute en procentuele waarden worden gerapporteerd als ze zouden worden weergegeven voor een gewone gebruiker (geen root).

Teller | Betekenis
------- | -------
Vrije ruimte | Beschikbare schijfruimte in bytes
UsedSpace | Gebruikte schijfruimte in bytes
PercentFreeSpace | Percentage vrije ruimte
PercentUsedSpace | Percentage gebruikte ruimte
PercentFreeInodes | Percentage van de niet-gebruikte inodes
PercentUsedInodes | Percentage van toegewezen (in gebruik) inodes opgeteld voor alle bestandssystemen
BytesReadPerSecond | Aantal gelezen bytes per seconde
BytesWrittenPerSecond | Bytes per seconde geschreven
BytesPerSecond | Bytes lezen of schrijven per seconde
ReadsPerSecond | Leesbewerkingen per seconde
WritesPerSecond | Schrijfbewerkingen per seconde
TransfersPerSecond | Lees- of -bewerkingen per seconde

Geaggregeerde waarden voor alle bestandssystemen kunnen worden verkregen door in te stellen `"condition": "IsAggregate=True"`. Waarden voor een specifieke gekoppeld bestandssysteem, zoals ' / mnt ', kunnen worden verkregen door in te stellen `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-the-disk-class"></a>ingebouwde metrische gegevens voor de schijf-klasse

De schijf-klasse van metrische gegevens biedt informatie over het schijfgebruik van het apparaat. Deze statistieken van toepassing op het hele station. Als er meerdere bestandssystemen op een apparaat, zijn de tellers voor het apparaat in feite samengevoegd voor alle mappen.

Teller | Betekenis
------- | -------
ReadsPerSecond | Leesbewerkingen per seconde
WritesPerSecond | Schrijfbewerkingen per seconde
TransfersPerSecond | Totaal aantal bewerkingen per seconde
AverageReadTime | Gemiddeld aantal seconden per leesbewerking
AverageWriteTime | Gemiddeld aantal seconden per schrijfbewerking
AverageTransferTime | Gemiddeld aantal seconden per bewerking
AverageDiskQueueLength | Gemiddeld aantal in de wachtrij schijfbewerkingen
ReadBytesPerSecond | Aantal gelezen bytes per seconde
WriteBytesPerSecond | Aantal bytes per seconde geschreven
BytesPerSecond | Aantal bytes lezen of schrijven per seconde

Geaggregeerde waarden voor alle schijven kunnen worden verkregen door in te stellen `"condition": "IsAggregate=True"`. Als u gegevens voor een specifiek apparaat (bijvoorbeeld/dev/sdf1), stelt `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Installeren en configureren van LAD 3.0 via CLI

Ervan uitgaande dat de beveiligde instellingen zijn in het bestand is PrivateConfig.json en uw openbare configuratie-informatie in PublicConfig.json, voert u deze opdracht uit:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

De opdracht wordt ervan uitgegaan dat u de modus Azure Resource Manager (arm) van de Azure CLI. LAD configureren voor de implementatie van het klassieke model (ASM) virtuele machines, gaat u naar 'asm'-modus (`azure config mode asm`) en laat u de naam van de resourcegroep in de opdracht. Zie voor meer informatie de [platformoverschrijdende CLI-documentatie](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Een voorbeeldconfiguratie LAD 3.0

Op basis van de voorgaande definities, volgt hier een voorbeeldconfiguratie voor het uitbreiding van LAD 3.0 met een verklaring. Als u wilt toepassen in dit voorbeeld aan uw aanvraag, moet u uw eigen storage-accountnaam, account-SAS-token en EventHubs SAS-tokens.

### <a name="privateconfigjson"></a>PrivateConfig.json

Deze persoonlijke instellingen configureren:

* Een storage-account
* een overeenkomende account-SAS-token
* verschillende sinks (JsonBlob of EventHubs met SAS-tokens)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

Deze openbare instellingen veroorzaken LAD aan:

* Percentage processortijd en gebruikte schijfruimte metrische gegevens te uploaden de `WADMetrics*` tabel
* Berichten vanuit de syslog-faciliteit 'gebruiker' en de ernst 'info' uploaden naar de `LinuxSyslog*` tabel
* Onbewerkte OMI queryresultaten (PercentProcessorTime en PercentIdleTime) uploaden naar de benoemde `LinuxCPU` tabel
* Toegevoegde regels in bestand uploaden `/var/log/myladtestlog` naar de `MyLadTestLog` tabel

In elk geval wordt ook gegevens geüpload naar:

* Azure Blob-opslag (de containernaam van de is zoals gedefinieerd in de sink JsonBlob)
* Event hubs-eindpunt (zoals aangegeven in de Event hubs-sink)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

De `resourceId` in de configuratie moet overeenkomen met die van de virtuele machine of de virtuele-machineschaalset instellen.

* Azure-platform metrische gegevens voor grafieken en waarschuwingen weet de resourceId van de virtuele machine waaraan u werkt. Het wordt verwacht dat de gegevens voor uw virtuele machine via de resourceId de lookup-sleutel vinden.
* Als u automatisch schalen van Azure gebruikt, moet de resourceId die worden gebruikt door LAD overeenkomen met de resourceId in de configuratie voor automatisch schalen.
* De resourceId is ingebouwd in de namen van JsonBlobs door LAD geschreven.

## <a name="view-your-data"></a>Uw gegevens weergeven

De Azure portal gebruiken voor prestatiegegevens bekijken of instellen van waarschuwingen:

![image](./media/diagnostics-linux/graph_metrics.png)

De `performanceCounters` gegevens altijd worden opgeslagen in een Azure Storage-tabel. Azure Storage-API's zijn beschikbaar voor vele talen en platforms.

Gegevens worden verzonden naar sinks JsonBlob wordt opgeslagen in blobs in de storage-account met de naam in de [beveiligde instellingen](#protected-settings). U kunt de blob-gegevens met behulp van een Azure Blob Storage-API's verbruiken.

Bovendien kunt u deze UI-hulpprogramma's voor toegang tot de gegevens in Azure Storage:

* Visual Studio Server Explorer.
* [Microsoft Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").

Deze momentopname van een Microsoft Azure Storage Explorer-sessie ziet u de gegenereerde Azure Storage-tabellen en containers in een correct geconfigureerde LAD 3.0-extensie op een test-VM. De installatiekopie komt niet overeen met precies met de [LAD 3.0 voorbeeldconfiguratie](#an-example-lad-30-configuration).

![image](./media/diagnostics-linux/stg_explorer.png)

Zie de relevante [EventHubs documentatie](../../event-hubs/event-hubs-what-is-event-hubs.md) voor meer informatie over het gebruik van berichten die zijn gepubliceerd naar een Event hubs-eindpunt.

## <a name="next-steps"></a>Volgende stappen

* Maak metrische waarschuwingen in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) voor de metrische gegevens die u hebt verzameld.
* Maak [controlegrafieken](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) voor uw metrische gegevens.
* Meer informatie over het [maken van een virtuele-machineschaalset](../linux/tutorial-create-vmss.md) met behulp van de metrische gegevens voor het beheren van automatisch schalen.

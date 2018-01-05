---
title: Azure Compute - extensie Linux diagnose | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de Azure Linux diagnostische extensie (LAD) voor het verzamelen van meetgegevens over en gebeurtenissen van virtuele Linux-machines in Azure wordt uitgevoerd.
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 1eae6d302827c977b9258174dec68fd8f3009a11
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a>Linux-extensie voor diagnostische gebruiken om te controleren van Logboeken en metrische gegevens

Dit document beschrijft versie 3.0 en nieuwere van de diagnostische Linux-uitbreiding.

> [!IMPORTANT]
> Zie voor meer informatie over versie 2.3 en oudere [dit document](./classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Inleiding

De Linux-extensie voor diagnostische helpt de monitor van een gebruiker de status van een Linux-VM uitgevoerd op Microsoft Azure. Deze heeft de volgende mogelijkheden:

* Verzamelt system maatstaven voor prestaties van de virtuele machine en slaat ze op in een specifieke tabel in een aangewezen storage-account.
* Logboekgebeurtenissen moeten worden opgehaald uit syslog en slaat ze op in een specifieke tabel in het opgegeven opslagaccount.
* Hiermee kunnen gebruikers voor het aanpassen van de metrische gegevens die worden verzameld en worden geüpload.
* Hiermee kunnen gebruikers de syslog-installaties en niveaus van gebeurtenissen die worden verzameld en geüpload aanpassen.
* Hiermee kunnen gebruikers opgegeven logboekbestanden uploaden naar een tabel aangewezen opslag.
* Biedt ondersteuning voor het verzenden van metrische gegevens en logboekbestanden gebeurtenissen tot willekeurige EventHub-eindpunten en blobs JSON-indeling in het opgegeven opslagaccount.

Deze extensie werkt met beide Azure-implementatiemodellen.

## <a name="installing-the-extension-in-your-vm"></a>Installeren van de extensie in de virtuele machine

U kunt deze uitbreiding inschakelen met behulp van de Azure PowerShell-cmdlets, Azure CLI-scripts of sjablonen voor Azure-implementatie. Zie voor meer informatie [extensies functies](./extensions-features.md).

De Azure-portal kan niet worden gebruikt in-of LAD 3.0 configureren. In plaats daarvan installeert en configureert deze versie 2.3. Azure portal grafieken en waarschuwingen werken met gegevens van beide versies van de extensie.

Deze installatie-instructies en een [downloadbare voorbeeldconfiguratie](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0 te configureren:

* vastleggen en opslaan van dezelfde metrische gegevens zijn verstrekt door LAD 2.3;
* vastleggen van een handig set file system maatstaven voor nieuwe naar LAD 3.0;
* de standaardcollectie syslog ingeschakeld door LAD 2.3; vastleggen
* Schakel in de Azure portal ervaring voor grafieken en waarschuwen voor VM metrische gegevens.

De configuratie van de downloadbare is slechts een voorbeeld; aanpassen aan uw eigen behoeften.

### <a name="prerequisites"></a>Vereisten

* **Azure Linux Agent versie 2.2.0 of hoger**. De meeste Azure VM Linux galerie met installatiekopieën zijn inclusief versie 2.2.7 of hoger. Voer `/usr/sbin/waagent -version` om te bevestigen dat de versie is geïnstalleerd op de virtuele machine. Als de virtuele machine is een oudere versie van de guest-agent uitgevoerd, voert u de [deze instructies](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) bij te werken.
* **Azure CLI**. [Instellen van de Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) omgeving op uw computer.
* De opdracht wget, als u dit nog niet hebt: Voer `sudo apt-get install wget`.
* Een bestaande Azure-abonnement en een bestaand opslagaccount in het opslaan van de gegevens.

### <a name="sample-installation"></a>Voorbeeld-installatie

Vul in de juiste parameters op de eerste drie regels en voer dit script als hoofdmap:

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
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

De URL voor de voorbeeldconfiguratie en de inhoud nog worden gewijzigd. Een kopie van de instellingen voor de JSON-bestand downloaden en deze aanpassen aan uw behoeften. Alle sjablonen of automation die u samenstellen moet uw eigen exemplaar, in plaats van telkens te downloaden die URL te gebruiken.

### <a name="updating-the-extension-settings"></a>Bijwerken van de extensie-instellingen

Nadat u uw beveiligde of openbare instellingen hebt gewijzigd, implementeert u ze naar de virtuele machine door dezelfde opdracht uitvoert. Als iets in de instellingen hebt gewijzigd, wordt de bijgewerkte instellingen worden verzonden naar de extensie. LAD de configuratie wordt opnieuw geladen en zelf opnieuw gestart.

### <a name="migration-from-previous-versions-of-the-extension"></a>Migratie van eerdere versies van de extensie

De meest recente versie van de uitbreiding is **3.0**. **Eventuele oude versies (2.x) zijn afgeschaft en is mogelijk niet-gepubliceerde op of na 31 juli 2018**.

> [!IMPORTANT]
> Deze extensie introduceert recente wijzigingen aan de configuratie van de extensie. Een dergelijke wijziging is aangebracht voor het verbeteren van de beveiliging van de extensie. Als gevolg hiervan kan achterwaartse compatibiliteit met 2.x niet worden beheerd. De uitgever van de extensie voor deze extensie is ook anders dan de uitgever voor de 2.x-versies.
>
> Als u wilt migreren van 2.x naar deze nieuwe versie van de extensie, moet u de oude uitbreiding (onder de naam van de oude uitgever) verwijderen vervolgens versie 3 van de uitbreiding te installeren.

Aanbevelingen:

* Installeer de uitbreiding met secundaire versie van Automatische upgrade ingeschakeld.
  * Geef op het klassieke implementatiemodel virtuele machines, '3.*' als de versie als u de uitbreiding via Azure XPLAT CLI of Powershell installeert.
  * Op Azure Resource Manager deployment model VM's, bevatten ' 'autoUpgradeMinorVersion': true' in de VM-sjabloon voor implementatie.
* Gebruik een nieuwe/verschillende storage-account voor LAD 3.0. Er zijn enkele kleine compatibiliteitsproblemen tussen LAD 2.3 en LAD 3.0 waardoor een account lastige delen:
  * LAD 3.0 slaat syslog-gebeurtenissen in een tabel met een andere naam.
  * De counterSpecifier tekenreeksen voor `builtin` metrische gegevens verschillen LAD 3.0.

## <a name="protected-settings"></a>Beveiligde instellingen

Deze reeks configuratiegegevens gevoelige informatie bevat die moet worden beschermd tegen openbare weergave, bijvoorbeeld storage-referenties. Deze instellingen zijn verzonden naar en opgeslagen door de uitbreiding in versleutelde vorm.

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
storageAccountEndPoint | (optioneel) Het eindpunt voor het identificeren van de cloud waarin de storage-account bestaat. Als deze instelling niet aanwezig is, de Azure openbare cloud LAD standaard `https://core.windows.net`. Stel deze waarde dienovereenkomstig voor het gebruik van een opslagaccount in de Duitse Azure, Azure Government of Azure China.
storageAccountSasToken | Een [Account-SAS-token](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) voor Blob- en -services (`ss='bt'`), van toepassing op containers en objecten (`srt='co'`), welke verleent toevoegen, maken, weergeven, bijwerken en schrijfmachtigingen (`sp='acluw'`). Voer *niet* voorloopspaties vraagteken (?) bevatten.
mdsdHttpProxy | (optioneel) HTTP-proxy-informatie die nodig zijn voor de extensie inschakelen voor het verbinding maken met het opgegeven opslagaccount en een eindpunt.
sinksConfig | (optioneel) Details van alternatieve doelen waarvoor metrische gegevens en gebeurtenissen kunnen worden geleverd. De specifieke details van elke gegevens sink ondersteund door de extensie worden in de volgende secties besproken.


> [!NOTE]
> Bij het implementeren van de uitbreiding met de sjabloon van een Azure-implementatie, de storage-account en SAS-token van tevoren gemaakt en vervolgens doorgegeven aan de sjabloon. U kan niet een VM, het opslagaccount implementeren en configureren van de uitbreiding in één sjabloon. Maken van een SAS-token binnen een sjabloon is momenteel niet ondersteund.

U kunt gemakkelijk de vereiste SAS-token via de Azure portal opstellen.

1. Selecteer de opslagaccounts voor algemeen gebruik account waarnaar de extensie te schrijven
1. Selecteer "Shared access signature' uit het gedeelte instellingen van het menu links
1. Controleer de toepasselijke rubrieken zoals eerder beschreven.
1. Klik op de knop 'SAS genereren'.

![Afbeelding](./media/diagnostic-extension/make_sas.png)

De gegenereerde SAS kopiëren naar het veld storageAccountSasToken; Verwijder de begin-vraagteken ('? ').

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

Deze optionele secties bevatten aanvullende bestemmingen waarnaar de extensie verzendt de gegevens die worden verzameld. De matrix 'sink' bevat een object voor elke aanvullende gegevens sink. Het kenmerk 'type' bepaalt welke andere kenmerken in het object.

Element | Waarde
------- | -----
naam | Een tekenreeks die wordt gebruikt om te verwijzen naar deze sink elders in de configuratie voor de uitbreiding.
type | Het type sink wordt gedefinieerd. Bepaalt de andere waarden (indien aanwezig) in exemplaren van dit type.

Versie 3.0 van de Linux-extensie voor diagnostische ondersteunt twee typen van de sink: EventHub en JsonBlob.

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

De vermelding 'sasURL' bevat de volledige URL, met inbegrip van SAS-token voor de Event Hub waarvoor gegevens moeten worden gepubliceerd. LAD vereist een SAS naamgeving van een beleid waarmee de verzenden claim. Een voorbeeld:

* Een aangeroepen Event Hubs-naamruimte maken`contosohub`
* Een Event Hub maken in de naamruimte die is aangeroepen`syslogmsgs`
* Een gedeeld toegangsbeleid maken op de Event Hub met de naam `writer` waarmee de claim verzenden

Als u hebt gemaakt een SAS goede tot middernacht UTC op 1 januari 2018, is de waarde sasURL mogelijk:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Zie voor meer informatie over het genereren van SAS-tokens voor Event Hubs [deze webpagina](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="the-jsonblob-sink"></a>De sink JsonBlob

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

Gegevens die zijn omgeleid naar een JsonBlob sink is opgeslagen in blobs in Azure-opslag. Elk exemplaar van LAD een blob elk uur wordt gemaakt voor elke naam sink. Elke blob bevat altijd een ongeldige syntaxis JSON-matrix van object. Nieuwe vermeldingen moment toegevoegd aan de matrix. BLOBs worden opgeslagen in een container met dezelfde naam als de sink. De Azure-opslag-regels voor de namen van de blob-container van toepassing op de namen van JsonBlob put: tussen 3 en 63 kleine alfanumerieke ASCII-tekens of streepjes bevatten.

## <a name="public-settings"></a>Instellingen voor openbare

Deze structuur bevat verschillende blokken van instellingen voor de door de uitbreiding verzamelde gegevens. Elke instelling is optioneel. Als u opgeeft `ladCfg`, moet u ook opgeven `StorageAccount`.

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
mdsdHttpProxy | (optioneel) Hetzelfde als in de [beveiligde instellingen](#protected-settings). De openbare waarde wordt overschreven door de waarde voor de persoonlijke, als ingesteld. Proxy-instellingen met een geheim, zoals een wachtwoord, in plaats de [beveiligde instellingen](#protected-settings).

De overige elementen worden beschreven in de volgende secties.

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

Deze besturingselementen optionele structuur sinks voor het verzamelen van Logboeken voor de levering van de service Azure metrische gegevens en andere gegevens en metrische gegevens. U moet ofwel opgeven `performanceCounters` of `syslogEvents` of beide. Geef de `metrics` structuur.

Element | Waarde
------- | -----
eventVolume | (optioneel) Hiermee bepaalt u het aantal partities in de tabel opslag hebt gemaakt. Moet een van `"Large"`, `"Medium"`, of `"Small"`. Als niet wordt opgegeven, wordt de standaardwaarde `"Medium"`.
sampleRateInSeconds | (optioneel) Het standaardinterval tussen verzameling maatstaven voor onbewerkte (unaggregated). De kleinste ondersteunde samplefrequentie is 15 seconden. Als niet wordt opgegeven, wordt de standaardwaarde `15`.

#### <a name="metrics"></a>metrics

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
resourceId | De Azure Resource Manager resource-ID van de virtuele machine of van de schaal van de virtuele machine ingesteld op de virtuele machine deel uitmaakt. Deze instelling moet ook worden opgegeven als een sink JsonBlob wordt gebruikt in de configuratie.
scheduledTransferPeriod | De frequentie waarmee cumulatieve metrische gegevens moeten worden berekend en overgebracht naar metrische gegevens voor Azure, uitgedrukt als een tijdsinterval 8601 IS. De kleinste overdracht periode is 60 seconden, dat wil zeggen, PT1M. U moet ten minste één scheduledTransferPeriod opgeven.

Voorbeelden van de metrische gegevens opgegeven in de sectie performanceCounters worden verzameld elke 15 seconden of op het voorbeeld beoordelen expliciet gedefinieerd voor de teller. Als meerdere scheduledTransferPeriod frequenties wordt weergegeven (zoals in het voorbeeld), wordt elke aggregatie wordt afzonderlijk berekend.

#### <a name="performancecounters"></a>performanceCounters

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

Deze optionele sectie bepaalt de verzameling van metrische gegevens. Onbewerkte voorbeelden worden samengevoegd voor elk [scheduledTransferPeriod](#metrics) voor het produceren van deze waarden:

* gemiddelde
* minimum
* maximum
* laatste verzameld waarde
* telling van onbewerkte voorbeelden om te berekenen van de statistische functie

Element | Waarde
------- | -----
Put | (optioneel) Een door komma's gescheiden lijst met namen van Put aan welke LAD verzendt metrische resultaten geaggregeerd. Alle samengevoegde metrische gegevens worden gepubliceerd op elke vermelde sink. Zie [sinksConfig](#sinksconfig). Voorbeeld: `"EHsink1, myjsonsink"`.
type | Geeft de werkelijke provider van de metrische gegevens.
klasse | Samen met 'teller', identificeert de specifieke metrische gegevens in de naamruimte van de provider.
Prestatiemeteritems | Samen met 'class', identificeert de specifieke metrische gegevens in de naamruimte van de provider.
counterSpecifier | Identificeert de specifieke metrische gegevens in de naamruimte Azure metrische gegevens.
voorwaarde | (optioneel) Hiermee selecteert u een specifiek exemplaar van het object waarop de metriek is van toepassing of selecteert de aggregatie in alle exemplaren van dat object. Zie voor meer informatie de [ `builtin` metrische definities](#metrics-supported-by-builtin).
sampleRate | IS 8601-interval die bepaalt de snelheid waarmee onbewerkte voorbeelden voor deze metrische gegevens worden verzameld. Als niet is ingesteld, het interval voor gegevensverzameling is ingesteld door de waarde van [sampleRateInSeconds](#ladcfg). De kortste ondersteunde samplefrequentie is 15 seconden (PT15S).
eenheid | Moet een van deze tekenreeksen: 'Count', 'Bytes', 'Seconden', 'Percentage', 'CountPerSecond', 'BytesPerSecond', 'Milliseconde'. Hiermee definieert u de eenheid voor de metriek. Gebruikers van de verzamelde gegevens verwacht de waarden van de verzamelde gegevens overeenkomen met deze eenheid. LAD negeert dit veld.
Weergavenaam | Het label (in de taal die is opgegeven door de bijbehorende landinstelling) moet worden gekoppeld aan deze gegevens in Azure metrische gegevens. LAD negeert dit veld.

De counterSpecifier is een identifier. Consumenten van metrische gegevens, zoals de Azure portal voor grafieken en counterSpecifier waarschuwingen van de functie, gebruiken als de 'sleutel' die een waarde of een exemplaar van een metriek identificeert. Voor `builtin` metrische gegevens, wordt aangeraden gebruik van counterSpecifier-waarden die met beginnen `/builtin/`. Als u een specifiek exemplaar van een metriek verzamelt, raden wij dat u de id van het exemplaar koppelen aan de counterSpecifier-waarde. Een aantal voorbeelden:

* `/builtin/Processor/PercentIdleTime`-Niet-actieve tijd met een gemiddelde van alle vcpu 's
* `/builtin/Disk/FreeSpace(/mnt)`-Vrije ruimte voor het bestandssysteem mnt
* `/builtin/Disk/FreeSpace`-Vrije ruimte met een gemiddelde van alle gekoppelde bestandssystemen

Verwacht dat de waarde counterSpecifier moet overeenkomen met een patroon LAD noch de Azure-portal. In de manier waarop u counterSpecifier waarden samenstelt consistent zijn.

Wanneer u opgeeft `performanceCounters`, LAD altijd schrijft gegevens naar een tabel in Azure-opslag. U kunt dezelfde gegevens geschreven naar JSON-blobs en/of Event Hubs, maar u het opslaan van gegevens naar een tabel niet uitschakelen. Alle exemplaren van de diagnostische uitbreiding geconfigureerd voor het gebruik van dezelfde naam van het opslagaccount en eindpunt hun metrische gegevens en de logboeken toevoegen aan dezelfde tabel. Als er te veel virtuele machines naar dezelfde tabel partitie schrijft, Azure schrijfbewerkingen kunt beperken op die partitie. De instelling eventVolume zorgt ervoor dat de vermeldingen worden verspreid over 1 (klein), 10 (gemiddeld) of 100 (groot) verschillende partities. 'Gemiddeld' is meestal voldoende is om verkeer niet wordt beperkt. De functie Azure metrische gegevens van de Azure portal gebruikt de gegevens in deze tabel voor het produceren van grafieken of voor het activeren van waarschuwingen. De tabelnaam van de is de samenvoeging van deze tekenreeksen:

* `WADMetrics`
* 'ScheduledTransferPeriod' voor de geaggregeerde waarden opgeslagen in de tabel
* `P10DV2S`
* Een datum in de vorm 'JJJJMMDD", wijzigingen van elke 10 dagen

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

Deze optionele sectie Hiermee bepaalt u het verzamelen van gebeurtenissen van syslog. Als de sectie wordt weggelaten, worden helemaal syslog-gebeurtenissen niet vastgelegd.

De verzameling syslogEventConfiguration heeft een vermelding voor elke syslog-faciliteit van belang. Als minSeverity 'NONE' voor een bepaalde opslagruimte, of als faciliteit niet wordt weergegeven in het element helemaal, worden er zijn geen gebeurtenissen van de faciliteit vastgelegd.

Element | Waarde
------- | -----
Put | Een door komma's gescheiden lijst met namen van put die afzonderlijke gebeurtenissen worden gepubliceerd. Alle gebeurtenissen die overeenkomt met de beperkingen in syslogEventConfiguration worden gepubliceerd op elke vermelde sink. Voorbeeld: 'EHforsyslog'
voorziening %{facilityname/ | De naam van een syslog-faciliteit (zoals ' logboek\_gebruiker ' of ' LOG\_LOCAL0 '). Zie de sectie 'faciliteit' van de [syslog man pagina](http://man7.org/linux/man-pages/man3/syslog.3.html) voor een volledige lijst.
minSeverity | Een urgentieniveau syslog (zoals ' logboek\_fout ' of ' LOG\_INFO '). Zie de sectie 'niveau' van de [syslog man pagina](http://man7.org/linux/man-pages/man3/syslog.3.html) voor een volledige lijst. De extensie bevat gebeurtenissen die worden verzonden naar de faciliteit aan of hoger dan het opgegeven niveau.

Wanneer u opgeeft `syslogEvents`, LAD altijd schrijft gegevens naar een tabel in Azure-opslag. U kunt dezelfde gegevens geschreven naar JSON-blobs en/of Event Hubs, maar u het opslaan van gegevens naar een tabel niet uitschakelen. De partitionering gedrag voor deze tabel is hetzelfde als is beschreven voor `performanceCounters`. De tabelnaam van de is de samenvoeging van deze tekenreeksen:

* `LinuxSyslog`
* Een datum in de vorm 'JJJJMMDD", wijzigingen van elke 10 dagen

Voorbeelden zijn onder meer `LinuxSyslog20170410` en `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Deze optionele sectie bepaalt de uitvoering van willekeurige [OMI](https://github.com/Microsoft/omi) query's.

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
naamruimte | (optioneel) De OMI-naamruimte waarbinnen de query moet worden uitgevoerd. Als u niets opgeeft, de standaardwaarde is "root/scx', geïmplementeerd door de [System Center platformoverschrijdende Providers](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
query | De OMI-query moet worden uitgevoerd.
tabel | (optioneel) De tabel Azure-opslag in het opgegeven opslagaccount (Zie [beveiligde instellingen](#protected-settings)).
frequency | (optioneel) Het aantal seconden tussen het uitvoeren van de query. Standaardwaarde is 300 (5 minuten); minimumwaarde is 15 seconden.
Put | (optioneel) Een door komma's gescheiden lijst met namen van aanvullende put waarnaar onbewerkte metrische voorbeeldresultaten moeten worden gepubliceerd. Er is geen aggregatie van deze voorbeelden onbewerkte wordt berekend door de uitbreiding of Azure metrische gegevens.

Een 'tabel' of 'sinks', of beide moeten worden opgegeven.

### <a name="filelogs"></a>fileLogs

Hiermee bepaalt u het vastleggen van logboekbestanden. LAD nieuwe tekstregels vastgelegd als ze zijn geschreven naar het bestand en schrijft deze naar de tabelrijen en/of alle opgegeven Put (JsonBlob of EventHub).

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
bestand | De volledige padnaam van het logbestand moeten worden gecontroleerd en vastgelegd. De padnaam moet één bestand; de naam Deze kan niet de naam van een map of jokertekens bevatten.
tabel | (optioneel) De Azure-opslag-tabel in het opgegeven opslagaccount (zoals opgegeven in de configuratie van beveiligde), waarin u nieuwe regels van de 'staart' van het bestand worden geschreven.
Put | (optioneel) Een door komma's gescheiden lijst met namen van aanvullende put aan welke regels logboek is verzonden.

Een 'tabel' of 'sinks', of beide moeten worden opgegeven.

## <a name="metrics-supported-by-the-builtin-provider"></a>Metrische gegevens die worden ondersteund door de provider builtin

De ingebouwde metrische provider is een bron van de meest interessante bij een uitgebreide set gebruikers metrische gegevens. Deze metrische gegevens kunnen worden onderverdeeld in vijf brede klassen:

* Processor
* Geheugen
* Netwerk
* Bestandssysteem
* Schijf

### <a name="builtin-metrics-for-the-processor-class"></a>ingebouwde metrische gegevens voor de Processor-klasse

De Processor-klasse van metrische gegevens biedt informatie over het processorgebruik in de virtuele machine. Bij het samenvoegen van percentages, is het resultaat het gemiddelde over alle CPU's. In een VM twee vCPU als één vCPU 100% bezig is en de andere 100% niet actief is, is zou de gerapporteerde PercentIdleTime dit 50. Als elke vCPU 50% voor dezelfde periode bezet is, zou het gerapporteerde resultaat ook 50 zijn. In een VM vier vCPU, met één vCPU 100% bezet en de andere niet-actief, zijn de gerapporteerde PercentIdleTime 75.

Prestatiemeteritems | Betekenis
------- | -------
PercentIdleTime | Percentage tijd tijdens het aggregatie-venster dat processors de niet-actieve lus kernel zijn uitgevoerd
percentProcessorTime | Percentage tijd dat een niet-actieve thread wordt uitgevoerd
PercentIOWaitTime | Percentage tijd wachten i/o-bewerkingen te voltooien
PercentInterruptTime | Percentage tijd hardware/software-interrupts en DPC's (uitgestelde procedureaanroepen) wordt uitgevoerd
PercentUserTime | Niet-actieve tijd tijdens het venster aggregatie, het percentage tijd die is doorgebracht in gebruiker meer met normale prioriteit
PercentNiceTime | Niet-actieve tijd voor het percentage die werd doorgebracht met verlaagde (nice) prioriteit
PercentPrivilegedTime | Niet-actieve tijd voor het percentage die is doorgebracht in de modus beschermde (kernel)

De som van de eerste vier items moeten op 100%. De laatste drie prestatiemeteritems ook som en 100%. ze Verdeel de som van PercentProcessorTime PercentIOWaitTime en PercentInterruptTime.

Voor een enkele metrische gegevens geaggregeerd tot alle processors ingesteld `"condition": "IsAggregate=TRUE"`. U kunt een waarde voor een specifieke processor, zoals de tweede logische processor van een VM vier vCPU ingesteld `"condition": "Name=\\"1\\""`. De logische processor-nummers zijn in het bereik `[0..n-1]`.

### <a name="builtin-metrics-for-the-memory-class"></a>ingebouwde metrische gegevens voor de klasse geheugen

De klasse geheugen van de metrische gegevens bevat informatie over geheugengebruik, paginering en wisselen.

Prestatiemeteritems | Betekenis
------- | -------
AvailableMemory | Beschikbaar fysiek geheugen in MiB
PercentAvailableMemory | Beschikbaar fysiek geheugen als percentage van het totale geheugen
UsedMemory | Gebruik fysiek geheugen (MiB)
PercentUsedMemory | Fysiek geheugen in gebruik als een percentage van het totale geheugen
PagesPerSec | Totaal aantal paginering (lezen/schrijven)
PagesReadPerSec | Pagina's lezen uit een back-up store (wisselbestand, programmabestand, toegewezen bestand, enz.)
PagesWrittenPerSec | Pagina's die zijn geschreven naar de back-ups maken store (wisselbestand, toegewezen bestand, enz.)
AvailableSwap | Niet-gebruikte wisselruimte (MiB)
PercentAvailableSwap | Niet-gebruikte wisselruimte als percentage van de totale wisselruimte
UsedSwap | Gebruik wisselruimte (MiB)
PercentUsedSwap | In gebruik wisselruimte als percentage van de totale wisselruimte

Deze klasse van metrische gegevens heeft slechts één exemplaar. Het voorwaardekenmerk '' heeft geen nuttig instellingen en moet worden weggelaten.

### <a name="builtin-metrics-for-the-network-class"></a>ingebouwde metrische gegevens voor de netwerk-klasse

De netwerk-klasse van metrische gegevens bevat informatie over activiteit op het netwerk op een afzonderlijke netwerkinterfaces sinds het opstarten. LAD maakt geen metrische gegevens bandbreedte, die worden opgehaald van de host metrische gegevens beschikbaar.

Prestatiemeteritems | Betekenis
------- | -------
BytesTransmitted | Totaal aantal bytes dat is verzonden sinds het opstarten
BytesReceived | Totaal aantal bytes dat is ontvangen sinds het opstarten
BytesTotal | Totaal aantal bytes verzonden of ontvangen sinds het opstarten
PacketsTransmitted | Totale aantal pakketten dat is verzonden sinds opstarten
PacketsReceived | Totale aantal pakketten ontvangen sinds het opstarten
TotalRxErrors | Aantal ontvangen fouten sinds opstarten
TotalTxErrors | Aantal verzonden fouten sinds opstarten
TotalCollisions | Aantal conflicten die zijn gerapporteerd door de netwerkpoorten sinds opstarten

 Hoewel deze klasse wordt verwezen, is LAD ondersteunt geen vastleggen netwerk metrische gegevens die worden getotaliseerd over alle netwerkapparaten. Instellen om op te halen van de metrische gegevens voor een specifieke interface, zoals eth0, `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-the-filesystem-class"></a>ingebouwde metrische gegevens voor de klasse bestandssysteem

De klasse van het bestandssysteem van metrische gegevens bevat informatie over het gebruik van het bestandssysteem. Absolute en het percentage waarden worden gerapporteerd als ze zouden worden weergegeven voor een gewone gebruiker (geen root).

Prestatiemeteritems | Betekenis
------- | -------
FreeSpace | Beschikbare schijfruimte in bytes
UsedSpace | Gebruikte schijfruimte in bytes
PercentFreeSpace | Percentage vrije ruimte
PercentUsedSpace | Percentage gebruikte ruimte
PercentFreeInodes | Percentage van de niet-gebruikte inodes
PercentUsedInodes | Percentage van toegewezen (in gebruik) inodes getotaliseerd over alle bestandssystemen
BytesReadPerSecond | Gelezen bytes per seconde
BytesWrittenPerSecond | Bytes per seconde geschreven
BytesPerSecond | Bytes lezen of schrijven per seconde
ReadsPerSecond | Leesbewerkingen per seconde
WritesPerSecond | Schrijfbewerkingen per seconde
TransfersPerSecond | Lees- of schrijfbewerkingen per seconde

Geaggregeerde waarden in alle bestandssystemen kunnen worden verkregen door het instellen van `"condition": "IsAggregate=True"`. Waarden voor een specifieke gekoppelde bestandssysteem, zoals ' / mnt ', kan worden verkregen door het instellen van `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-the-disk-class"></a>ingebouwde metrische gegevens voor de schijf-klasse

De klasse van de schijf van metrische gegevens bevat informatie over schijfgebruik van het apparaat. Deze statistieken van toepassing op het hele station. Als er meerdere bestandssystemen op een apparaat, zijn de prestatiemeteritems voor dat apparaat in feite getotaliseerd over alle mappen.

Prestatiemeteritems | Betekenis
------- | -------
ReadsPerSecond | Leesbewerkingen per seconde
WritesPerSecond | Schrijfbewerkingen per seconde
TransfersPerSecond | Totaal aantal bewerkingen per seconde
AverageReadTime | Gemiddeld aantal seconden per leesbewerking
AverageWriteTime | Gemiddeld aantal seconden per schrijfbewerking
AverageTransferTime | Gemiddeld aantal seconden per bewerking
AverageDiskQueueLength | Gemiddeld aantal bewerkingen in de wachtrij schijven
ReadBytesPerSecond | Aantal gelezen bytes per seconde
WriteBytesPerSecond | Aantal bytes per seconde geschreven
BytesPerSecond | Aantal bytes gelezen of geschreven per seconde

Geaggregeerde waarden over alle schijven kunnen worden verkregen door het instellen van `"condition": "IsAggregate=True"`. Als u gegevens voor een specifiek apparaat (bijvoorbeeld/dev/sdf1), ingesteld `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Installeren en configureren van LAD 3.0 via CLI

Ervan uitgaande dat uw beveiligde instellingen zijn in het bestand PrivateConfig.json en uw openbare configuratiegegevens PublicConfig.json, deze opdracht uitvoeren:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

De opdracht wordt ervan uitgegaan dat u de Azure Resource Management-modus (arm) van de Azure CLI. LAD configureren voor de implementatie van het klassieke model (ASM) virtuele machines, overschakelen naar de modus 'asm' (`azure config mode asm`) en de naam van de resourcegroep in de opdracht weglaten. Zie voor meer informatie de [platformoverschrijdende CLI documentatie](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Een voorbeeldconfiguratie LAD 3.0

Op basis van de voorgaande definities, is hier een voorbeeld LAD 3.0 extensie configuratie met een verklaring. Als u wilt toepassen in dit voorbeeld aan uw aanvraag, moet u uw eigen opslagaccountnaam, account-SAS-token en EventHubs-SAS-tokens.

### <a name="privateconfigjson"></a>PrivateConfig.json

Deze persoonlijke instellingen configureren:

* een opslagaccount
* een overeenkomende account-SAS-token
* verschillende Put (JsonBlob of EventHubs met SAS-tokens)

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

Deze instellingen voor openbare veroorzaken LAD naar:

* Percentage processortijd en gebruikte schijfruimte metrische gegevens te uploaden de `WADMetrics*` tabel
* Berichten van syslog-faciliteit 'gebruiker' en de ernst 'info' uploaden naar de `LinuxSyslog*` tabel
* Onbewerkte OMI queryresultaten (PercentProcessorTime en PercentIdleTime) uploaden naar de benoemde `LinuxCPU` tabel
* Toegevoegde regels in bestand uploaden `/var/log/myladtestlog` naar de `MyLadTestLog` tabel

In elk geval gegevens ook naar geüpload:

* Azure Blob-opslag (containernaam is zoals gedefinieerd in de sink JsonBlob)
* EventHubs-eindpunt (zoals opgegeven in de sink EventHubs)

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

De `resourceId` in de configuratie moeten overeenkomen met dat van de virtuele machine of virtuele-machineschaalset ingesteld.

* Azure-platform metrische gegevens voor grafieken en waarschuwingen kent de resourceId van de virtuele machine waarmee u werkt. Het verwacht vinden van de gegevens voor uw virtuele machine met behulp van de resourceId de zoeksleutel.
* Als u Azure automatisch schalen, moet de resourceId van de in de configuratie voor automatisch schalen die overeenkomen met de resourceId van de door LAD gebruikt.
* De resourceId is ingebouwd in de namen van JsonBlobs door LAD geschreven.

## <a name="view-your-data"></a>Bekijk uw gegevens

De Azure portal gebruiken voor prestatiegegevens weergeven of instellen van waarschuwingen:

![Afbeelding](./media/diagnostic-extension/graph_metrics.png)

De `performanceCounters` gegevens altijd worden opgeslagen in een tabel met Azure Storage. Azure Storage-API's zijn beschikbaar voor veel talen en platforms.

Gegevens die worden verzonden naar JsonBlob put worden opgeslagen in blobs in de storage-account met de naam in de [beveiligde instellingen](#protected-settings). U kunt de blobgegevens met behulp van een Azure Blob Storage-API's gebruiken.

Bovendien kunt u deze UI-hulpprogramma's voor toegang tot de gegevens in Azure Storage:

* Visual Studio Server Explorer.
* [Microsoft Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/ "Azure Opslagverkenner").

Deze momentopname van een Microsoft Azure Storage Explorer-sessie ziet u de gegenereerde Azure Storage-tabellen en containers van een juist geconfigureerde LAD 3.0-extensie op een virtuele testmachine. De installatiekopie komt niet overeen met exact met de [LAD 3.0 voorbeeldconfiguratie](#an-example-lad-30-configuration).

![Afbeelding](./media/diagnostic-extension/stg_explorer.png)

Zie de relevante [EventHubs-documentatie](../../event-hubs/event-hubs-what-is-event-hubs.md) voor informatie over het gebruiken van berichten die worden gepubliceerd op een EventHubs-eindpunt.

## <a name="next-steps"></a>Volgende stappen

* Maken van metrische waarschuwingen in [Azure Monitor](../../monitoring-and-diagnostics/insights-alerts-portal.md) van de metrische gegevens die u hebt verzameld.
* Maak [grafieken bewaking](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) voor uw metrische gegevens.
* Meer informatie over hoe [maken van een virtuele-machineschaalset](/azure/virtual-machines/linux/tutorial-create-vmss) metrische gegevens van uw met automatisch schalen controleren.

---
title: De Azure-opslagemulator gebruiken voor ontwikkelen en testen | Microsoft Docs
description: De Azure-opslagemulator biedt een gratis lokale ontwikkelingsomgeving voor het ontwikkelen en testen van uw Azure Storage-toepassingen. Informatie over hoe aanvragen worden geautoriseerd, verbinding maken met de emulator van uw toepassing en het gebruik van de opdrachtregel-hulpprogramma.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/10/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 529612aeecfcea1d775c2f4359c5135ca3c6885e
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052540"
---
# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>De Azure-opslagemulator gebruiken voor ontwikkelen en testen

De Microsoft Azure-opslagemulator biedt een lokale omgeving waarin de services van Azure Blob, wachtrijen en tabellen voor ontwikkelingsdoeleinden worden geëmuleerd. De opslagemulator gebruikt, kunt u uw toepassing meer veiligheid tegen de storage-services lokaal testen zonder dat het maken van een Azure-abonnement of er kosten in rekening. Wanneer u tevreden bent over hoe uw toepassing in de emulator werkt, kunt u overstappen naar een Azure storage-account in de cloud.

## <a name="get-the-storage-emulator"></a>De opslagemulator ophalen
De opslagemulator is beschikbaar als onderdeel van de [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). U kunt ook de opslagemulator installeren met behulp van de [zelfstandig installatieprogramma voor](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (directe download). U moet beheerdersbevoegdheden hebben op uw computer voor het installeren van de opslagemulator.

De opslagemulator wordt momenteel uitgevoerd alleen op Windows. Degenen die u overweegt een opslagemulator voor Linux, een optie is de community onderhouden open-source-opslagemulator [Azurite](https://github.com/azure/azurite).

> [!NOTE]
> Gegevens die zijn gemaakt in een versie van de opslagemulator is niet noodzakelijkerwijs toegankelijk wanneer u een andere versie. Als u nodig hebt om vast te leggen van de gegevens voor de lange termijn, wordt u aangeraden dat u die gegevens opslaan in een Azure storage-account, in plaats van de opslagemulator.
> 
> De opslagemulator is afhankelijk van specifieke versies van de OData-bibliotheken. Vervangen van de OData-dll's die worden gebruikt door de opslagemulator met andere versies wordt niet ondersteund en kan leiden tot onverwacht gedrag. Elke versie van OData ondersteund door de storage-service kan echter worden gebruikt voor het verzenden van aanvragen naar de emulator.

## <a name="how-the-storage-emulator-works"></a>De werking van de opslagemulator
De opslagemulator maakt gebruik van een lokaal exemplaar van Microsoft SQL Server en het lokale bestandssysteem naar Azure storage-services worden geëmuleerd. De opslagemulator maakt standaard gebruik van een database in Microsoft SQL Server 2012 Express LocalDB. U kunt het configureren van de opslagemulator voor toegang tot een lokaal exemplaar van SQL Server in plaats van de LocalDB-instantie. Zie voor meer informatie de [begin- en initialisatie van de opslagemulator](#start-and-initialize-the-storage-emulator) verderop in dit artikel.

De opslagemulator maakt verbinding met SQL Server of LocalDB met behulp van Windows-verificatie.

Enkele verschillen in functionaliteit bestaan tussen de opslagemulator en de Azure storage-services. Zie voor meer informatie over deze verschillen, de [verschillen tussen de opslagemulator en Azure Storage](#differences-between-the-storage-emulator-and-azure-storage) verderop in dit artikel.

## <a name="start-and-initialize-the-storage-emulator"></a>Start en de opslagemulator initialiseren

Start de Azure-opslagemulator:
1. Selecteer de **Start** of drukt u op de **Windows** sleutel.
2. Begin met typen `Azure Storage Emulator`.
3. Selecteer de emulator in de lijst met weergegeven toepassingen.

Wanneer de opslagemulator wordt gestart, verschijnt een opdrachtpromptvenster. U kunt deze consolevenster gebruiken om te starten en stoppen van de opslagemulator, gegevens wissen, status ophalen en de emulator initialiseren. Zie voor meer informatie de [naslaginformatie voor Storage emulator opdrachtregelprogramma](#storage-emulator-command-line-tool-reference) verderop in dit artikel.

Wanneer de emulator wordt gestart, ziet u een pictogram in het systeemvak op de taakbalk van Windows.

Wanneer u de storage emulator opdrachtpromptvenster sluit, blijven de opslagemulator om uit te voeren. Als u wilt openen in het consolevenster Opslagemulator nogmaals, de voorgaande stappen alsof de opslagemulator starten.

De eerste keer dat u de opslagemulator uitvoert is de lokale opslag-omgeving geïnitialiseerd voor u. Het initialisatieproces maakt u een database in LocalDB en behoudt zich HTTP-poorten voor elke service lokale opslag.

De opslagemulator is standaard geïnstalleerd `C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> U kunt de [Microsoft Azure Storage Explorer](http://storageexplorer.com) om te werken met resources voor lokale opslag-emulator. Zoek naar '(ontwikkeling)' onder 'Storage-Accounts' in de structuur van de resources Storage Explorer nadat u hebt geïnstalleerd en de opslagemulator is gestart.
>

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Initialiseren van de opslagemulator voor het gebruik van een andere SQL-database

De storage emulator opdrachtregel-hulpprogramma kunt u de opslagemulator om te verwijzen naar een SQL database-exemplaar dan het standaardexemplaar van LocalDB initialiseren:

1. Open in het consolevenster Opslagemulator zoals beschreven in de [begin- en initialisatie van de opslagemulator](#start-and-initialize-the-storage-emulator) sectie.
1. Typ in het consolevenster de volgende opdracht, waarbij `<SQLServerInstance>` is de naam van de SQL Server-exemplaar. Geef voor het gebruik van LocalDB `(localdb)\MSSQLLocalDb` als de SQL Server-exemplaar.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  U kunt ook de volgende opdracht, die ervoor zorgt de emulator dat gebruiken de standaard SQL Server-exemplaar gebruiken:

  `AzureStorageEmulator.exe init /server .`

  Of u kunt de volgende opdracht uit, initialiseert de database op het standaardexemplaar van LocalDB opnieuw gebruiken:

  `AzureStorageEmulator.exe init /forceCreate`

Zie voor meer informatie over deze opdrachten [naslaginformatie voor Storage emulator opdrachtregelprogramma](#storage-emulator-command-line-tool-reference).

> [!TIP]
> U kunt de [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) voor het beheren van uw SQL Server-exemplaren, met inbegrip van de installatie van LocalDB. In de SMSS **verbinding maken met Server** dialoogvenster, geef `(localdb)\MSSQLLocalDb` in de **servernaam:** veld verbinding maken met de LocalDB-instantie.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Verifiëren van aanvragen voor de opslagemulator
Nadat u hebt geïnstalleerd en de opslagemulator is gestart, kunt u uw code voor het testen. Net als bij Azure Storage in de cloud, moet elke aanvraag die u op basis van de opslagemulator maakt worden gemachtigd, tenzij het een anonieme aanvraag. U kunt aanvragen op basis van de opslagemulator met behulp van gedeelde sleutelverificatie of met een shared access signature (SAS) autoriseren.

### <a name="authorize-with-shared-key-credentials"></a>Toestaan dat met de referenties van de gedeelde sleutel
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over verbindingsreeksen [configureren van Azure Storage-verbindingsreeksen](../storage-configure-connection-string.md).

### <a name="authorize-with-a-shared-access-signature"></a>Autoriseren met een shared access signature
Sommige Azure storage-clientbibliotheken, zoals de Xamarin-clientbibliotheek is alleen ondersteuning voor verificatie met een token shared access signature (SAS). U kunt het SAS-token met behulp van een hulpprogramma zoals maken de [Opslagverkenner](http://storageexplorer.com/) of een andere toepassing die ondersteuning biedt voor verificatie met gedeelde sleutel.

U kunt ook een SAS-token genereren met behulp van Azure PowerShell. Het volgende voorbeeld wordt een SAS-token met volledige machtigingen voor een blob-container gegenereerd:

1. Installeer Azure PowerShell als u dit nog (met behulp van de meest recente versie van de Azure PowerShell cmdlets wordt aanbevolen). Zie voor installatie-instructies [installeren en configureren van Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Open Azure PowerShell en voer de volgende opdrachten, vervangen `CONTAINER_NAME` met een naam van uw keuze:

```powershell
$context = New-AzureStorageContext -Local

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

De resulterende shared access signature URI voor de nieuwe container zijn vergelijkbaar met:

```
http://127.0.0.1:10000/devstoreaccount1/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

De handtekening voor gedeelde toegang gemaakt met dit voorbeeld is geldig gedurende één dag. De handtekening, hebben volledige toegang (lezen, schrijven, verwijderen, lijst) tot de blobs in de container.

Zie voor meer informatie over handtekeningen voor gedeelde toegang [Using shared access signatures (SAS) in Azure Storage](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-the-storage-emulator"></a>Verwerken van resources in de opslagemulator
De service-eindpunten voor de opslagemulator verschillen van die van een Azure storage-account. Het verschil is omdat de lokale computer naamomzetting van het domein waarvoor de eindpunten van de storage-emulator aan lokale adressen worden niet uitgevoerd.

Wanneer u een resource in Azure storage-account kunt oplossen, gebruikt u het volgende schema. De accountnaam is onderdeel van de naam van de URI-host en de bron worden binnenkort aangepakt maakt deel uit van de URI-pad:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

De volgende URI is bijvoorbeeld een geldig adres voor een blob in Azure storage-account:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

De opslagemulator, omdat de lokale computer geen Domeinnaamomzetting, uit de naam van het is echter deel uitmaakt van de URI-pad in plaats van de hostnaam. Gebruik de volgende URI-indeling voor een resource in de opslagemulator:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob in de opslagemulator:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

De service-eindpunten voor de opslagemulator zijn:

* BLOB-service: `http://127.0.0.1:10000/<account-name>/<resource-path>`
* Queue-service: `http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tabelservice: `http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Het account secundaire met RA-GRS-adressering
Vanaf versie 3.1, ondersteunt de opslagemulator leestoegang geografisch redundante replicatie (RA-GRS). Storage-resources in de cloud en in de lokale emulator, kunt u toegang tot de secundaire locatie door toe te voegen - naam van het secundaire. Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob met behulp van de secundaire alleen-lezen in de opslagemulator:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Voor programmatische toegang tot de secundaire met de opslagemulator gebruikt u de Opslagclientbibliotheek voor .NET versie 3.2 of hoger. Zie de [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Naslaginformatie voor Storage emulator opdrachtregel-hulpprogramma
Versie 3.0 vanaf biedt wordt een consolevenster weergegeven wanneer u de Opslagemulator start. Gebruik de opdracht in het consolevenster te starten en stoppen van de emulator, evenals de query voor de status en andere bewerkingen uitvoeren.

> [!NOTE]
> Als u de Microsoft Azure compute emulator geïnstalleerd hebt, wordt een pictogram in systeemvak voor systeem weergegeven wanneer u de Opslagemulator start. Met de rechtermuisknop op het pictogram om een menu dat biedt een grafische manier om te starten en stoppen van de Opslagemulator weer te geven.
>
>

### <a name="command-line-syntax"></a>Opdrachtregelsyntaxis
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Opties
Typ `/help` bij de opdrachtprompt om een lijst met opties te zien.

| Optie | Beschrijving | Opdracht | Argumenten |
| --- | --- | --- | --- |
| **Beginnen** |De opslagemulator wordt gestart. |`AzureStorageEmulator.exe start [-inprocess]` |*-inprocess*: Start de emulator in het huidige proces in plaats van een nieuw proces maken. |
| **Stoppen** |Hiermee stopt u de opslagemulator. |`AzureStorageEmulator.exe stop` | |
| **Status** |De status van de opslagemulator af te drukken. |`AzureStorageEmulator.exe status` | |
| **Wissen** |Hiermee schakelt u de gegevens in alle services op de opdrachtregel opgegeven. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*BLOB*: wist blob-gegevens. <br/>*wachtrij*: wachtrijgegevens worden gewist. <br/>*tabel*: gegevens worden gewist. <br/>*alle*: Hiermee wist u alle gegevens in alle services. |
| **Init** |Voert eenmalig initialisatie voor het instellen van de emulator. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-server Servernaam\exemplaarnaam*: Hiermee geeft u de server die als host fungeert voor de SQL-exemplaar. <br/>*-sqlinstance instanceName*: Hiermee geeft u de naam van het SQL-exemplaar moet worden gebruikt in het standaardexemplaar van de server. <br/>*-forcecreate*: zorgt ervoor dat het maken van de SQL-database, zelfs als deze al bestaat. <br/>*-skipcreate*: het maken van de SQL-database wordt overgeslagen. Dit heeft voorrang op - forcecreate.<br/>*-reserveports*: probeert te reserveren van de HTTP-poorten die zijn gekoppeld aan de services.<br/>*-unreserveports*: pogingen tot het verwijderen van de reserveringen voor de HTTP-poorten die zijn gekoppeld aan de services. Dit heeft voorrang op - reserveports.<br/>*-inprocess*: initialisatie in het huidige proces in plaats van bij het maken van een nieuw proces uitgevoerd. Het huidige proces moet worden gestart met verhoogde bevoegdheden als poort reserveringen wilt wijzigen. |

## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Verschillen tussen de opslagemulator en Azure Storage
Omdat de opslagemulator een geëmuleerde omgeving die wordt uitgevoerd in een lokale SQL-exemplaar is, zijn er verschillen in functionaliteit tussen de emulator en een Azure storage-account in de cloud:

* De opslagemulator ondersteunt slechts één vast account en een bekende verificatiesleutel.
* De opslagemulator is niet een schaalbare opslag-service en biedt geen ondersteuning voor een groot aantal gelijktijdige clients.
* Zoals beschreven in [verwerken van resources in de opslagemulator](#addressing-resources-in-the-storage-emulator), resources, anders worden behandeld in de opslagemulator ten opzichte van een Azure storage-account. Dit verschil is, omdat de domein-naamomzetting is beschikbaar in de cloud, maar niet op de lokale computer.
* Vanaf versie 3.1, ondersteunt het opslagaccount van de emulator leestoegang geografisch redundante replicatie (RA-GRS). Alle accounts hebben RA-GRS wordt ingeschakeld en er is nooit een vertraging tussen de primaire en secundaire replica's in de emulator. De Blob Service-statistieken ophalen, Queue-Service-statistieken ophalen en ophalen van Table Service Stats bewerkingen worden ondersteund op de secundaire-account en retourneert altijd de waarde van de `LastSyncTime` antwoord element als de huidige tijd op basis van de onderliggende SQL-database.
* De File-service en service-eindpunten voor SMB-protocol worden momenteel niet ondersteund in de opslagemulator.
* Als u een versie van de storage-services die nog niet wordt ondersteund door de opslagemulator gebruikt, geeft de opslagemulator een VersionNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldige aanvraag).

### <a name="differences-for-blob-storage"></a>Verschillen voor Blob-opslag
De volgende verschillen van toepassing op Blob-opslag in de emulator:

* De blob voor storage emulator ondersteunt alleen de grootte van maximaal 2 GB.
* De maximale lengte van de blobnaam van een in de opslagemulator is 256 tekens, terwijl de maximale lengte van de blobnaam van een in Azure Storage 1024 tekens is.
* Incrementele kopie kunt momentopnamen van overschreven blobs moeten worden gekopieerd, waardoor een fout geretourneerd voor de service.
* Get Page-bereiken Diff werkt niet tussen momentopnamen die zijn gekopieerd met behulp van incrementeel kopiëren van de Blob.
* Een Blob Put-bewerking kan op basis van een blob die deel uitmaakt van de opslagemulator met een actieve lease slaagt, zelfs als de lease-ID niet in de aanvraag opgegeven is.
* Toevoeg-Blob-bewerkingen worden niet ondersteund door de emulator. Een bewerking uit te voeren op een toevoeg-blob retourneert een fout FeatureNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvraag).

### <a name="differences-for-table-storage"></a>Verschillen voor Table storage
De volgende verschillen naar Table storage in de emulator van toepassing:

* Datumeigenschappen in de tabel-service in de opslagemulator ondersteunt alleen het bereik dat wordt ondersteund door SQL Server 2005 (ze zijn verplicht om later zijn dan 1 januari 1753). Alle datums vóór 1 januari 1753 worden gewijzigd met deze waarde. De precisie van de datums is beperkt tot de precisie van de SQL Server 2005, wat betekent dat de datums nauwkeurige op 1 zijn/300th van een seconde.
* De opslagemulator biedt ondersteuning voor partitie en recordsleutelwaarden sleuteleigenschap waarden van minder dan 512 bytes. Bovendien de totale grootte van de accountnaam, de tabelnaam en de namen van sleuteleigenschappen samen niet groter dan 900 bytes.
* De totale grootte van een rij in een tabel in de opslagemulator is beperkt tot minder dan 1 MB.
* In de storage-emulator, typt u de eigenschappen van de gegevens `Edm.Guid` of `Edm.Binary` ondersteunen alleen de `Equal (eq)` en `NotEqual (ne)` vergelijkingsoperators in query filteren tekenreeksen.

### <a name="differences-for-queue-storage"></a>Verschillen voor Queue storage
Er zijn geen verschillen die specifiek zijn voor Queue-opslag in de emulator.

## <a name="storage-emulator-release-notes"></a>Opmerkingen bij de release van de Storage-emulator

### <a name="version-57"></a>Versie 5.7
Een opgelost waardoor een crash als logboekregistratie is ingeschakeld.

### <a name="version-56"></a>Versie 5.6
* De opslagemulator biedt nu ondersteuning voor versie 2018-03-28 van de storage-services op Blob, Queue en Table service-eindpunten.

### <a name="version-55"></a>Versie 5.5
* De opslagemulator biedt nu ondersteuning voor versie 2017-11-09 van de storage-services op Blob, Queue en Table service-eindpunten.
* Er is ondersteuning toegevoegd voor de blob **gemaakt** eigenschap tijd voor het maken van de blob.

### <a name="version-54"></a>Versie 5.4
Ter verbetering van de stabiliteit van de installatie, probeert de emulator niet meer te reserveren poorten tijdens de installatie. Als poort reserveringen zijn gewenst is, gebruikt u de *- reserveports* optie van de **init** opdracht te geven.

### <a name="version-53"></a>Versie 5.3
De opslagemulator biedt nu ondersteuning voor versie 2017-07-29 van de storage-services op Blob, Queue en Table service-eindpunten.

### <a name="version-52"></a>Versie 5.2
* De opslagemulator biedt nu ondersteuning voor versie 17-04-2017 van de storage-services op Blob, Queue en Table service-eindpunten.
* Een bug opgelost waarbij tabel eigenschapswaarden zijn niet wordt correct gecodeerd.

### <a name="version-51"></a>Versie 5.1
Een bug opgelost waarbij de opslagemulator is retourneren de `DataServiceVersion` -header in sommige antwoorden waar de service niet is.

### <a name="version-50"></a>Versie 5.0
* Het installatieprogramma van de opslagemulator niet meer bestaande MSSQL controleert en .NET Framework wordt geïnstalleerd.
* Het installatieprogramma van de opslagemulator maakt niet langer de database als onderdeel van de installatie. Database wordt nog steeds gemaakt, indien nodig als onderdeel van het opstarten van.
* Database niet meer wordt gemaakt vereist tot misbruik van bevoegdheden.
* Poort reserveringen zijn niet langer nodig voor opstarten.
* Voegt de volgende opties voor `init`: `-reserveports` (hiervoor is uitbreiding van bevoegdheden), `-unreserveports` (hiervoor is uitbreiding van bevoegdheden), `-skipcreate`.
* De Storage Emulator gebruikersinterface optie nu op het pictogram in systeemvak voor systeem wordt gestart van de opdrachtregelinterface. De oude gebruikersinterface is niet meer beschikbaar.
* Sommige DLL's zijn verwijderd of gewijzigd.

### <a name="version-46"></a>Versie 4.6
* De opslagemulator biedt nu ondersteuning voor versie 31-05-2016 van de storage-services op Blob, Queue en Table service-eindpunten.

### <a name="version-45"></a>Versie 4.5
* Een opgelost waardoor initialisatie en de installatie van de opslagemulator mislukken wanneer deze de naam van de database van de back-ups is gewijzigd.

### <a name="version-44"></a>Versie 4.4
* De opslagemulator biedt nu ondersteuning voor versie 2015-12-11 van de storage-services op Blob, Queue en Table service-eindpunten.
* De opslagemulator garbagecollection van blob-gegevens is nu efficiënter wanneer er sprake is van groot aantal blobs.
* Een opgelost waardoor container ACL XML naar iets anders uit hoe de storage-service het allemaal doet worden gevalideerd.
* Een opgelost waardoor soms maximale en minimale datum/tijd-waarden worden gerapporteerd in de verkeerde tijdzone.

### <a name="version-43"></a>Versie 4.3
* De opslagemulator biedt nu ondersteuning voor versie 2015-07-08 van de storage-services op Blob, Queue en Table service-eindpunten.

### <a name="version-42"></a>Versie 4.2
* De opslagemulator biedt nu ondersteuning voor versie 2015-04-05 van de storage-services op Blob, Queue en Table service-eindpunten.

### <a name="version-41"></a>Versie 4.1
* De opslagemulator biedt nu ondersteuning voor versie 2015-02-21 van de storage-services op Blob, Queue en Table service-eindpunten, met uitzondering van de nieuwe toevoeg-Blob-functies.
* Als u een versie van de storage-services die nog niet wordt ondersteund door de opslagemulator gebruikt, wordt in de emulator een zinvolle foutbericht geretourneerd. We raden u aan met behulp van de meest recente versie van de emulator. Als er een fout VersionNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvraag), download de nieuwste versie van de opslagemulator.
* Een bug opgelost waarin een entiteitsgegevens race veroorzaakt tabel onjuist worden tijdens het van gelijktijdige samenvoegbewerkingen.

### <a name="version-40"></a>Versie 4.0
* De opslagemulator uitvoerbaar bestand is gewijzigd in *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Versie 3.2
* De opslagemulator biedt nu ondersteuning voor versie 2014-02-14 van de storage-services op Blob, Queue en Table service-eindpunten. Bestand service-eindpunten worden momenteel niet ondersteund in de opslagemulator. Zie [versiebeheer voor de Azure Storage-Services](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) voor meer informatie over versie 2014-02-14.

### <a name="version-31"></a>Versie 3.1
* Geo-redundante opslag met leestoegang (RA-GRS) wordt nu ondersteund in de opslagemulator. De Blob Service-statistieken ophalen, Queue-Service-statistieken ophalen en ophalen Table Service statistieken-API's worden ondersteund voor de secundaire-account en retourneert de waarde van het antwoord LastSyncTime element altijd als de huidige tijd op basis van de onderliggende SQL-database. Voor programmatische toegang tot de secundaire met de opslagemulator gebruikt u de Opslagclientbibliotheek voor .NET versie 3.2 of hoger. Zie de Microsoft Azure Storage-clientbibliotheek voor .NET-verwijzing voor meer informatie.

### <a name="version-30"></a>Versie 3.0
* De Azure-opslagemulator wordt niet meer verzonden in hetzelfde pakket als de rekenemulator.
* De grafische gebruikersinterface van de storage-emulator is afgeschaft en vervangen door een scriptbare opdrachtregelinterface. Zie voor meer informatie over de opdrachtregelinterface, naslaginformatie voor Storage Emulator Command-Line Tool. De grafische interface blijft aanwezig zijn in versie 3.0, maar deze kunnen alleen worden geopend wanneer de Compute-Emulator is geïnstalleerd met de rechtermuisknop op het pictogram op de taakbalk en selecteert u Storage Emulator gebruikersinterface weergeven.
* Versie 2013-08-15 van de Azure storage-services is nu volledig ondersteund. (Deze versie werd eerder alleen ondersteund door de Opslagemulator versie 2.2.1 Preview-versie.)

## <a name="next-steps"></a>Volgende stappen

* Evalueren van de platformoverschrijdende, de community onderhouden open source-opslagemulator [Azurite](https://github.com/arafato/azurite). 
* [Azure Storage-voorbeelden met behulp van .NET](../storage-samples-dotnet.md) bevat koppelingen naar enkele voorbeelden van code kunt u bij het ontwikkelen van uw toepassing.
* U kunt de [Microsoft Azure Storage Explorer](http://storageexplorer.com) om te werken met resources in uw cloud Storage-account en in de opslagemulator.

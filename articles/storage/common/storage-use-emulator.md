---
title: De Azure-opslagemulator gebruiken voor ontwikkeling en testen | Microsoft Docs
description: De Azure-opslagemulator biedt een gratis lokale ontwikkelingsomgeving voor het ontwikkelen en testen van uw Azure Storage-toepassingen. Informatie over hoe aanvragen worden geverifieerd, verbinding maken met de emulator van uw toepassing en het gebruik van het opdrachtregelprogramma.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: f480b059-df8a-4a63-b05a-7f2f5d1f5c2a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: tamram
ms.openlocfilehash: 7d86d5e8547d977c07cfbb0597b74382172a8472
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>De Azure-opslagemulator gebruiken voor ontwikkeling en testen

De Microsoft Azure-opslagemulator biedt een lokale omgeving waarin de services van Azure Blob, wachtrijen en tabellen voor ontwikkelingsdoeleinden worden geëmuleerd. De opslagemulator gebruikt, kunt u uw toepassing op de storage-services lokaal testen zonder te maken van een Azure-abonnement of mogelijke kosten. Wanneer u tevreden bent over hoe uw toepassing in de emulator werkt, kunt u overschakelen naar het gebruik van een Azure storage-account in de cloud.

## <a name="get-the-storage-emulator"></a>De opslagemulator ophalen
De opslagemulator is beschikbaar als onderdeel van de [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). U kunt ook de opslagemulator installeren met behulp van de [zelfstandig installatieprogramma](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (directe download). U moet beheerdersbevoegdheden hebben op uw computer voor het installeren van de opslagemulator.

De opslagemulator momenteel wordt alleen uitgevoerd op Windows. Die u overweegt een opslagemulator voor Linux, is een optie om de community onderhouden, open-source opslagemulator [Azurite](https://github.com/arafato/azurite).

> [!NOTE]
> Gegevens die zijn gemaakt in een versie van de opslagemulator kan niet worden gegarandeerd toegankelijk zijn voor het gebruik van een andere versie. Als u nodig hebt om uw gegevens voor de lange termijn, raden wij u die gegevens opslaat in Azure storage-account, in plaats van de opslagemulator.
> <p/>
> De opslagemulator is afhankelijk van specifieke versies van de OData-bibliotheken. Vervangen van de OData-dll's die worden gebruikt door de opslagemulator met andere versies wordt niet ondersteund en kan leiden tot onverwacht gedrag. Elke versie van de OData ondersteund door de storage-service kan echter worden gebruikt voor aanvragen verzenden naar de emulator.
>

## <a name="how-the-storage-emulator-works"></a>De werking van de opslagemulator
De opslagemulator maakt gebruik van een lokaal exemplaar van Microsoft SQL Server en het lokale bestandssysteem emuleren Azure storage-services. De opslagemulator maakt standaard gebruik van een database in Microsoft SQL Server 2012 Express LocalDB. U kunt voor het configureren van de opslagemulator voor toegang tot een lokaal exemplaar van SQL Server in plaats van de LocalDB-exemplaar. Zie voor meer informatie de [begin- en initialisatie van de opslagemulator](#start-and-initialize-the-storage-emulator) verderop in dit artikel.

De opslagemulator maakt verbinding met SQL Server of LocalDB met Windows-verificatie.

Een paar verschillen in functionaliteit bestaan tussen de opslagemulator en Azure storage-services. Zie voor meer informatie over deze verschillen, de [verschillen tussen de opslagemulator en Azure Storage](#differences-between-the-storage-emulator-and-azure-storage) verderop in dit artikel.

## <a name="start-and-initialize-the-storage-emulator"></a>Start en de opslagemulator initialiseren
Starten van de Azure-opslagemulator:
1. Selecteer de **Start** of drukt u op de **Windows** sleutel.
1. Begint te typen `Azure Storage Emulator`.
1. Selecteer de emulator van de lijst met toepassingen weergegeven.

Wanneer de opslagemulator wordt gestart, verschijnt een opdrachtpromptvenster. U kunt dit consolevenster starten en stoppen van de opslagemulator, gegevens wissen status ophalen en initialiseren van de emulator. Zie voor meer informatie de [Storage emulator opdrachtregelprogramma verwijzing](#storage-emulator-command-line-tool-reference) verderop in dit artikel.

Wanneer de emulator wordt uitgevoerd, ziet u een pictogram in het Windows-systeemvak.

Wanneer u het opdrachtpromptvenster van storage emulator sluit, moet de opslagemulator wordt nog uitgevoerd. Om te zetten in het venster Opslagemulator console opnieuw, de voorgaande stappen alsof de opslagemulator wordt gestart.

De eerste keer dat u de opslagemulator uitvoert wordt de omgeving voor lokale opslag geïnitialiseerd voor u. Het initialisatieproces maakt een database in LocalDB en HTTP-poorten voor elke service lokale opslag zijn gereserveerd.

De opslagemulator is standaard geïnstalleerd `C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> U kunt de [Microsoft Azure Storage Explorer](http://storageexplorer.com) werken met lokale opslagemulatorresources. Zoek naar '(ontwikkeling)' onder 'Opslagaccounts' in de structuur van de resources Opslagverkenner nadat u hebt geïnstalleerd en de opslagemulator is gestart.
>

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Initialiseren van de opslagemulator voor het gebruik van een andere SQL-database
Het opdrachtregelprogramma van storage emulator kunt u de opslagemulator om te verwijzen naar een SQL database-exemplaar dan het standaardexemplaar LocalDB initialiseren:

1. Open het consolevenster Opslagemulator zoals beschreven in de [begin- en initialisatie van de opslagemulator](#start-and-initialize-the-storage-emulator) sectie.
1. Typ in het consolevenster de volgende opdracht, waarbij `<SQLServerInstance>` is de naam van de SQL Server-exemplaar. Geef voor het gebruik van LocalDB `(localdb)\MSSQLLocalDb` als de SQL Server-instantie.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  U kunt ook de volgende opdracht, die ervoor zorgt de emulator dat gebruiken het standaardexemplaar van SQL Server gebruiken:

  `AzureStorageEmulator.exe init /server .\\`

  Of u kunt de volgende opdracht, waarmee initialiseert de database met het standaardexemplaar LocalDB opnieuw gebruiken:

  `AzureStorageEmulator.exe init /forceCreate`

Zie voor meer informatie over deze opdrachten [Storage emulator opdrachtregelprogramma verwijzing](#storage-emulator-command-line-tool-reference).

> [!TIP]
> U kunt de [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) voor het beheren van uw SQL Server-exemplaren, met inbegrip van de installatie van de LocalDB. In de SMSS **verbinding maken met Server** dialoogvenster opgeven `(localdb)\MSSQLLocalDb` in de **servernaam:** veld verbinding maken met de LocalDB-exemplaar.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Verificatie-aanvragen op basis van de opslagemulator
Nadat u hebt geïnstalleerd en de opslagemulator is gestart, kunt u uw code te vergelijken met het testen. Net als bij Azure Storage in de cloud, moet elke aanvraag die u op basis van de opslagemulator aanbrengt worden geverifieerd, tenzij het een anonieme aanvraag. U kunt aanvragen op basis van de opslagemulator met gedeelde sleutel verificatie of met een shared access signature (SAS) verifiëren.

### <a name="authenticate-with-shared-key-credentials"></a>Verifiëren met referenties van de gedeelde sleutel
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Zie voor meer informatie over verbindingsreeksen [configureren Azure Storage-verbindingsreeksen](../storage-configure-connection-string.md).

### <a name="authenticate-with-a-shared-access-signature"></a>Verificatie met een shared access signature
Sommige clientbibliotheken van Azure storage, zoals de Xamarin-bibliotheek is alleen ondersteuning voor verificatie met een shared access signature (SAS)-token. U kunt het SAS-token met een hulpprogramma als maken de [Opslagverkenner](http://storageexplorer.com/) of een andere toepassing die ondersteuning biedt voor verificatie met gedeelde sleutel.

U kunt ook een SAS-token genereren met behulp van Azure PowerShell. Het volgende voorbeeld wordt een SAS-token met volledige machtigingen voor een blob-container gegenereerd:

1. Installeer Azure PowerShell als u nog niet gedaan hebt (met behulp van de nieuwste versie van Azure PowerShell cmdlets wordt aanbevolen). Zie voor installatie-instructies [installeren en configureren van Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Open Azure PowerShell en voer de volgende opdrachten, vervangen `ACCOUNT_NAME` en `ACCOUNT_KEY==` met uw eigen referenties en `CONTAINER_NAME` met een naam van uw keuze:

```powershell
$context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

De resulterende shared access signature URI voor de nieuwe container moet er ongeveer als:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

De shared access signature gemaakt met dit voorbeeld is geldig voor één dag. De handtekening verleent volledige toegang (lezen, schrijven, verwijderen, lijst) tot de blobs in de container.

Zie voor meer informatie over handtekeningen voor gedeelde toegang [Using shared access signatures (SAS) in Azure Storage](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-the-storage-emulator"></a>Verwerken van resources in de opslagemulator
De service-eindpunten voor de opslagemulator zijn anders dan die van een Azure storage-account. Het verschil is omdat de lokale computer worden naamomzetting van het domein waarvoor de eindpunten van de emulator opslag voor lokale adressen worden niet uitgevoerd.

Wanneer u een resource in Azure storage-account te houden, gebruikt u het volgende schema. De accountnaam is onderdeel van de hostnaam van URI en de bron wordt behandeld maakt deel uit van de URI-pad:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

De volgende URI is bijvoorbeeld een geldig adres voor een blob in Azure storage-account:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

De opslagemulator omdat de lokale computer worden niet uitgevoerd voor naamomzetting van het domein, de accountnaam is echter deel uit van de URI-pad in plaats van de hostnaam. Gebruik de volgende URI-notatie voor een resource in de opslagemulator:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob in de opslagemulator:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

De service-eindpunten voor de opslagemulator zijn:

* BLOB-service:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Queue-service:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tabelservice:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adressen van de secundaire met RA-GRS-account
Vanaf versie 3.1 is ondersteunt de opslagemulator geografisch redundante replicatie van leestoegang (RA-GRS). Storage-resources in de cloud en in de lokale emulator, u kunt toegang tot de secundaire locatie door voegen - ondergeschikt aan de accountnaam. Het volgende adres kan bijvoorbeeld worden gebruikt voor toegang tot een blob met behulp van de secundaire alleen-lezen in de opslagemulator:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Gebruik de Storage-clientbibliotheek voor .NET versie 3.2 of hoger voor programmatische toegang tot de secundaire met de opslagemulator. Zie de [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) voor meer informatie.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Naslaginformatie over Storage emulator opdrachtregelprogramma
Vanaf versie 3.0 biedt wordt een consolevenster weergegeven wanneer u de Opslagemulator start. Gebruik de opdracht in het consolevenster te starten en stoppen van de emulator evenals query voor de status en andere bewerkingen uitvoeren.

> [!NOTE]
> Als u de Microsoft Azure compute emulator geïnstalleerd hebt, wordt een pictogram op de taakbalk weergegeven wanneer u de Opslagemulator openen. Klik met de rechtermuisknop op het pictogram om een menu met een grafische manier om te starten en stoppen van de Opslagemulator zichtbaar te maken.
>
>

### <a name="command-line-syntax"></a>De syntaxis van de opdrachtregel
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Opties
Als u wilt weergeven in de lijst met opties, typt u `/help` bij de opdrachtprompt.

| Optie | Beschrijving | Opdracht | Argumenten |
| --- | --- | --- | --- |
| **Beginnen** |De opslagemulator wordt opgestart. |`AzureStorageEmulator.exe start [-inprocess]` |*-inprocess*: Start de emulator in het huidige proces in plaats van een nieuw proces maken. |
| **Stoppen** |Hiermee stopt de opslagemulator. |`AzureStorageEmulator.exe stop` | |
| **Status** |De status van de opslagemulator afgedrukt. |`AzureStorageEmulator.exe status` | |
| **Wissen** |Hiermee wist u de gegevens in alle services die op de opdrachtregel opgegeven. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*BLOB*: wist blob-gegevens. <br/>*wachtrij*: wist wachtrijgegevens. <br/>*tabel*: tabelgegevens worden gewist. <br/>*alle*: alle gegevens in alle services worden gewist. |
| **Init** |Voert eenmalig initialisatie voor het instellen van de emulator. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-server Servernaam\exemplaarnaam*: Hiermee geeft u op de server die als host fungeert voor de SQL-exemplaar. <br/>*-sqlinstance instanceName*: Hiermee geeft u de naam van het SQL-exemplaar moet worden gebruikt in het standaardexemplaar van de server. <br/>*-forcecreate*: forceert de creatie van de SQL-database, zelfs als deze al bestaat. <br/>*-skipcreate*: maken van de SQL-database wordt overgeslagen. Dit heeft voorrang boven - forcecreate.<br/>*-reserveports*: probeert te reserveren van de HTTP-poorten die zijn gekoppeld aan de services.<br/>*-unreserveports*: pogingen om te verwijderen van de reserveringen voor de HTTP-poorten die zijn gekoppeld aan de services. Dit heeft voorrang boven - reserveports.<br/>*-inprocess*: initialisatie in het huidige proces in plaats van een nieuwe aanmaakproces uitvoert. Het huidige proces moet worden gestart met verhoogde machtigingen als poort reserveringen wilt wijzigen. |

## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Verschillen tussen de opslagemulator en Azure Storage
Omdat de opslagemulator een geëmuleerde omgeving uitgevoerd in een lokale SQL-exemplaar is, zijn er verschillen in functionaliteit tussen de emulator en een Azure storage-account in de cloud:

* De opslagemulator ondersteunt slechts één vaste account en een bekende verificatiesleutel.
* De opslagemulator is geen service van schaalbare opslag en biedt geen ondersteuning voor een groot aantal gelijktijdige clients.
* Zoals beschreven in [adressering bronnen in de opslagemulator](#addressing-resources-in-the-storage-emulator), resources anders in de opslagemulator ten opzichte van een Azure storage-account worden behandeld. Dit verschil is omdat de domein-naamomzetting is beschikbaar in de cloud, maar niet op de lokale computer.
* Vanaf versie 3.1 is ondersteunt het opslagaccount van de emulator geografisch redundante replicatie van leestoegang (RA-GRS). In de emulator alle accounts hebben RA-GRS ingeschakeld en er is nooit een vertraging tussen de primaire en secundaire replica's. De statistieken van Blob-Service ophalen en statistieken van Queue-Service ophalen tabelstatistieken Service ophalen-bewerkingen worden ondersteund op de secundaire account en altijd de waarde als resultaat van de `LastSyncTime` antwoord element als de huidige tijd op basis van de onderliggende SQL-database.
* De File-service en de SMB-protocol service-eindpunten zijn momenteel niet ondersteund in de opslagemulator.
* Als u een versie van de storage-services die nog niet wordt ondersteund door de emulator gebruikt, geeft de opslagemulator een VersionNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldige aanvraag).

### <a name="differences-for-blob-storage"></a>Verschillen voor Blob-opslag
De volgende verschillen naar Blob storage in de emulator van toepassing:

* De blob voor storage emulator ondersteunt alleen de grootte van maximaal 2 GB.
* Incrementele kopie kunt momentopnamen van overschreven blobs moeten worden gekopieerd, waardoor een fout geretourneerd op de service.
* Get-paginabereiken Diff werkt niet tussen gekopieerd incrementele kopie Blob met momentopnamen.
* Een Blob Put-bewerking kan op basis van een blob of aanwezig is op de opslagemulator met een actieve lease slaagt, zelfs als de lease-ID niet in de aanvraag opgegeven is.
* Toevoeg-Blob bewerkingen worden niet ondersteund door de emulator. Poging een bewerking op een toevoeg-blob resulteert in een FeatureNotSupportedByEmulator-fout (HTTP-statuscode 400 - Ongeldige aanvraag).

### <a name="differences-for-table-storage"></a>Verschillen voor Table storage
De volgende verschillen naar Table storage in de emulator van toepassing:

* Datumeigenschappen in de tabel-service in de opslagemulator ondersteunt alleen het bereik dat wordt ondersteund door SQL Server 2005 (ze zijn moet later zijn dan 1 januari 1753 vereist). Alle datums vóór 1 januari 1753 zijn gewijzigd op deze waarde. De precisie van de datums is beperkt tot de precisie van de SQL Server 2005, wat betekent dat datums nauwkeurig op 1 zijn/300th van een seconde.
* De opslagemulator biedt ondersteuning voor partitie sleutel sleuteleigenschap rijwaarden van minder dan 512 bytes. Bovendien mag niet de totale grootte van de accountnaam, de tabelnaam en de namen van de sleuteleigenschap samen 900 bytes overschrijden.
* De totale grootte van een rij in een tabel in de opslagemulator is beperkt tot minder dan 1 MB.
* Typ in de opslagemulator eigenschappen van gegevens `Edm.Guid` of `Edm.Binary` ondersteunen alleen de `Equal (eq)` en `NotEqual (ne)` vergelijkingsoperators in query filteren tekenreeksen.

### <a name="differences-for-queue-storage"></a>Verschillen voor Queue storage
Er zijn geen verschillen specifieke naar Queue storage in de emulator.

## <a name="storage-emulator-release-notes"></a>Opmerkingen bij de release van de opslag-emulator
### <a name="version-52"></a>Versie 5.2
* De opslagemulator biedt nu ondersteuning voor versie 2017-04-17 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten.
* Een fout vastgesteld waar tabel eigenschapswaarden zijn niet wordt correct gecodeerd.

### <a name="version-51"></a>Versie 5.1
* Een bug vast waar de opslagemulator is retourneren de `DataServiceVersion` -header in sommige reacties waarbij de service niet was.

### <a name="version-50"></a>Versie 5.0
* Het installatieprogramma van de emulator opslag niet langer bestaande MSSQL controleert en .NET Framework wordt geïnstalleerd.
* De database als onderdeel van de installatie niet meer wordt gemaakt door het installatieprogramma van de emulator opslag. Database wordt nog steeds gemaakt, indien nodig tijdens het opstarten.
* Database maken niet meer vereist bevoegdheden.
* Poort reserveringen zijn niet langer nodig voor opstarten.
* Voegt de volgende opties voor `init`: `-reserveports` (hogere nodig is), `-unreserveports` (hogere nodig is), `-skipcreate`.
* De opslag Emulator UI optie nu op het pictogram in systeemvak voor systeem wordt gestart vanaf de opdrachtregelinterface. De gebruikersinterface van de oude is niet meer beschikbaar.
* Sommige DLL's zijn verwijderd of hernoemd.

### <a name="version-46"></a>Versie 4.6
* De opslagemulator biedt nu ondersteuning voor versie 2016-05-31 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten.

### <a name="version-45"></a>Versie 4.5
* Een bug initialisatie en de installatie van de opslagemulator mislukken wanneer deze de naam van de back-database is gewijzigd, waardoor vast.

### <a name="version-44"></a>4.4-versie
* De opslagemulator biedt nu ondersteuning voor versie 2015-12-11 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten.
* De opslagemulator garbagecollection van blob-gegevens is nu veel efficiënter wanneer omgaan met een groot aantal blobs.
* Vaste een bug waardoor container ACL XML naar iets anders van hoe de storage-service deze biedt worden gevalideerd.
* Een fout die soms veroorzaakt max en min datum-/ tijdwaarden worden gerapporteerd in de verkeerde tijdzone vast.

### <a name="version-43"></a>Versie 4.3
* De opslagemulator biedt nu ondersteuning voor versie 2015-07-08 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten.

### <a name="version-42"></a>Versie 4.2
* De opslagemulator biedt nu ondersteuning voor versie 2015-04-05 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten.

### <a name="version-41"></a>Versie 4.1
* De opslagemulator biedt nu ondersteuning voor versie 2015-02-21 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten, met uitzondering van de nieuwe functies voor toevoeg-Blob.
* Als u een versie van de storage-services die nog niet wordt ondersteund door de emulator gebruikt, wordt een zinvolle foutbericht geretourneerd in de emulator. U wordt aangeraden de nieuwste versie van de emulator. Als er een fout VersionNotSupportedByEmulator (HTTP-statuscode 400 - Ongeldige aanvraag) optreden, download de nieuwste versie van de opslagemulator.
* Een fout vastgesteld waarin een entiteitsgegevens race veroorzaakt tabel om te worden onjuist bij gelijktijdige merge-bewerkingen.

### <a name="version-40"></a>Versie 4.0
* De opslagemulator uitvoerbare bestand is gewijzigd in *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Versie 3.2
* De opslagemulator biedt nu ondersteuning voor versie 2014-02-14 van de storage-services voor blobs, wachtrijen en tabellen service-eindpunten. Service-eindpunten die bestand zijn momenteel niet ondersteund in de opslagemulator. Zie [versiebeheer voor de Azure Storage-Services](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) voor meer informatie over versie 2014-02-14.

### <a name="version-31"></a>Versie 3.1
* Geografisch redundante opslag met leestoegang (RA-GRS) wordt nu ondersteund in de opslagemulator. De statistieken van Blob-Service ophalen, statistieken voor Queue-Service ophalen en ophalen van tabel statistieken API's worden ondersteund voor de secundaire account en wordt altijd de waarde van het element LastSyncTime-antwoord geretourneerd als de huidige tijd op basis van de onderliggende SQL-database. Gebruik de Storage-clientbibliotheek voor .NET versie 3.2 of hoger voor programmatische toegang tot de secundaire met de opslagemulator. Zie de Microsoft Azure Storage-clientbibliotheek voor .NET-verwijzing voor meer informatie.

### <a name="version-30"></a>Versie 3.0
* De Azure-opslagemulator is niet langer in hetzelfde pakket als de rekenemulator verzonden.
* De grafische gebruikersinterface van de opslag-emulator is vervangen door een scriptbare opdrachtregelinterface. Zie voor meer informatie over de opdrachtregelinterface, Storage Emulator Naslaggids hulpprogramma. De grafische interface blijven aanwezig zijn in versie 3.0, maar kan alleen worden benaderd wanneer de Compute-Emulator is geïnstalleerd met de rechtermuisknop op het pictogram op de taakbalk en selecteer Storage Emulator UI weergeven.
* Versie 2013-08-15 van de Azure storage-services wordt nu volledig ondersteund. (Deze versie is eerder alleen ondersteund door de Opslagemulator versie 2.2.1 Preview.)

## <a name="next-steps"></a>Volgende stappen

* De opslagemulator op verschillende platforms en community onderhouden open-source evalueren [Azurite](https://github.com/arafato/azurite). 
* [Voorbeelden van Azure Storage met .NET](../storage-samples-dotnet.md) bevat koppelingen naar diverse codevoorbeelden die u gebruiken kunt bij het ontwikkelen van uw toepassing.
* U kunt de [Microsoft Azure Storage Explorer](http://storageexplorer.com) met resources in uw cloud-opslagaccount en in de opslagemulator.

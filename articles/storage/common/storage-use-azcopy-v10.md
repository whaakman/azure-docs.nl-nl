---
title: Gegevens kopiëren of verplaatsen naar Azure Storage met behulp van AzCopy v10 (Preview) | Microsoft Docs
description: Gebruik van de v10 AzCopy-opdrachtregelprogramma (Preview) te verplaatsen of kopiëren van gegevens naar of van blob-, data lake- en bestandsinhoud. Gegevens kopiëren naar Azure Storage van lokale bestanden en gegevens binnen of tussen opslagaccounts kopiëren. Migreer uw gegevens eenvoudig naar Azure Storage.
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 02/24/2019
ms.author: artemuwka
ms.subservice: common
ms.openlocfilehash: ca7081bdfedae3abb5ec426a9d3ec0a7867a2ef9
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337014"
---
# <a name="transfer-data-with-azcopy-v10-preview"></a>Gegevens overdragen met AzCopy v10 (Preview)

AzCopy v10 (Preview) is het opdrachtregelprogramma voor het kopiëren van gegevens naar of van Microsoft Azure BLOB Storage en File storage. AzCopy v10 biedt een opdrachtregelinterface die vernieuwde en nieuwe architectuur voor betrouwbare gegevens brengt. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren met behulp van AzCopy.

## <a name="whats-new-in-azcopy-v10"></a>Wat is er nieuw in AzCopy v10

- Synchroniseert bestandssystemen naar Azure Blob-opslag of vice versa. Gebruik `azcopy sync <source> <destination>`. Ideaal voor scenario's voor incrementele kopie.
- Biedt ondersteuning voor Azure Data Lake Storage Gen2 API's. Gebruik `myaccount.dfs.core.windows.net` als een URI naar de Data Lake Storage Gen2 API's aanroepen.
- Ondersteunt het kopiëren van een hele account (alleen voor Blob-service) naar een ander account.
- Maakt gebruik van de nieuwe [Put-blokken van URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API's ter ondersteuning van accounts exemplaar. De gegevensoverdracht is sneller, omdat de overdracht naar de client is niet vereist.
- Geeft een lijst of verwijdert u bestanden en blobs in een opgegeven pad.
- Biedt ondersteuning voor jokertekenpatronen in een pad, en--uitsluiten vlaggen.
- Maakt een volgorde van de taak en een bijbehorende logboekbestand met elk exemplaar van AzCopy. U kunt bekijken en vorige taken opnieuw starten en mislukte taken hervatten. AzCopy wordt ook automatisch opnieuw geprobeerd een overdracht na een storing.
- Verbeteringen van de algemene prestaties functies.

## <a name="download-and-install-azcopy"></a>Download en installeer AzCopy

### <a name="latest-preview-version-v10"></a>Meest recente preview-versie (v10)

De meest recente preview-versie van AzCopy downloaden:
- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

### <a name="latest-production-version-v81"></a>Nieuwste productieversie (v8.1)

Download de [nieuwste productieversie van AzCopy voor Windows](https://aka.ms/downloadazcopy).

### <a name="azcopy-supporting-table-storage-service-v73"></a>AzCopy Table storage-service (v7.3) ondersteunen

Download de [AzCopy v7.3 kopiëren van gegevens naar/van Microsoft Azure Table storage-service ondersteunt](https://aka.ms/downloadazcopynet).

## <a name="post-installation-steps"></a>Stappen na de installatie

AzCopy v10 zijn vereist om een installatie. Open uw favoriete opdrachtregelprogramma en blader naar de map waar `azcopy.exe` zich bevindt. Indien nodig, kunt u de locatie van de AzCopy-map toevoegen aan het systeempad voor gebruiksgemak.

## <a name="authentication-options"></a>Verificatieopties

AzCopy v10 ondersteunt de volgende opties bij het verifiëren met Azure Storage:
- **Azure Active Directory** (ondersteund voor **Blob-en Data Lake Storage Gen2**). Gebruik ```.\azcopy login``` zich aanmelden met Azure Active Directory.  De gebruiker moet beschikken over ['Blob Gegevensbijdrager voor' rol](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) te schrijven naar Blob storage met Azure Active Directory-verificatie. Voor verificatie via beheerde identiteiten voor Azure-resources, gebruikt u `azcopy login --identity`.
- **Gedeelde SAS-tokens [ondersteunde voor Blob- en Bestandsservices]**. Het token shared access signature (SAS) toegevoegd aan de blobpad op de opdrachtregel om het te gebruiken. U kunt met de Azure portal, SAS-tokens genereren [Opslagverkenner](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/), [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageblobsastoken), of andere hulpprogramma's van uw keuze. Zie voor meer informatie, [voorbeelden](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).

## <a name="getting-started"></a>Aan de slag

> [!TIP]
> **Liever een grafische gebruikersinterface?**
>
> [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), een bureaublad-client die het beheren van Azure Storage-gegevens, vereenvoudigt nu AzCopy wordt gebruikt om te versnellen gegevensoverdracht naar en uit Azure Storage.
>
> Inschakelen van AzCopy in Storage Explorer onder de **Preview** menu.
> ![AzCopy inschakelen als een engine voor overdracht in Azure Storage Explorer](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

AzCopy v10 heeft een zelf gedocumenteerde syntaxis. Wanneer u zich hebt aangemeld bij Azure Active Directory, ziet de algemene syntaxis er als volgt uit:

```azcopy
.\azcopy <command> <arguments> --<flag-name>=<flag-value>

# Examples if you have logged into the Azure Active Directory:
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile"
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/container"

# Examples if you're using SAS tokens to authenticate:
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container?sastoken" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile?sastoken"
```

Hier volgt hoe krijgt u een lijst van beschikbare opdrachten:

```azcopy
.\azcopy --help
# To use the alias instead
.\azcopy -h
```

Als u wilt zien van de help-pagina en voorbeelden voor een specifieke opdracht, kunt u de volgende opdracht uitvoeren:

```azcopy
.\azcopy <cmd> --help
# Example:
.\azcopy cp -h
```

## <a name="create-a-blob-container-or-file-share"></a>Een blob-container of bestandsshare maken 

**Een blob-container maken**

```azcopy
.\azcopy make "https://account.blob.core.windows.net/container-name"
```

**Een bestandsshare maken**

```azcopy
.\azcopy make "https://account.file.core.windows.net/share-name"
```

**Een blobcontainer maken met behulp van Azure Data Lake Storage Gen2**

Als u al hiërarchische naamruimten ingeschakeld op uw Blob storage-account, kunt u de volgende opdracht uit om te maken van een nieuwe blob-container voor het uploaden van bestanden.

```azcopy
.\azcopy make "https://account.dfs.core.windows.net/top-level-resource-name"
```

## <a name="copy-data-to-azure-storage"></a>Gegevens kopiëren naar Azure Storage

Gebruik de opdracht kopiëren om over te dragen gegevens van de bron naar de bestemming. De bron- of mag a:
- Lokaal bestandssysteem
- Azure-Blob/virtuele Directory/Container URI
- Azure-bestand/map/bestandsshare URI
- Azure Data Lake Storage Gen2 bestandssysteem/map/bestand URI

```azcopy
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
# Using the alias instead 
.\azcopy cp <source path> <destination path> --<flag-name>=<flag-value>
```

De volgende opdracht worden alle bestanden onder de map geüpload `C:\local\path` recursief naar de container `mycontainer1`, het maken van het `path` Active directory in de container:

```azcopy
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

De volgende opdracht worden alle bestanden onder de map geüpload `C:\local\path` (zonder recursing in de submappen) naar de container `mycontainer1`:

```azcopy
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/mycontainer1<sastoken>"
```

Ga voor meer voorbeelden, gebruik de volgende opdracht:

```azcopy
.\azcopy cp -h
```

## <a name="copy-data-between-two-storage-accounts"></a>Kopiëren van gegevens tussen twee opslagaccounts

Maakt gebruik van kopiëren van gegevens tussen twee opslagaccounts de [blok plaatsen van URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, en maakt geen gebruik van netwerkbandbreedte van de client-computer. Gegevens worden gekopieerd tussen twee Azure Storage-servers rechtstreeks, terwijl AzCopy gewoon de kopieerbewerking wordt georganiseerd. Deze optie is momenteel alleen beschikbaar voor Blob-opslag.

Als u wilt kopiëren van gegevens tussen twee opslagaccounts, gebruik de volgende opdracht:
```azcopy
.\azcopy cp "https://myaccount.blob.core.windows.net/<sastoken>" "https://myotheraccount.blob.core.windows.net/<sastoken>" --recursive=true
```

> [!NOTE]
> Met deze opdracht wordt het inventariseren van alle blob-containers en kopieer deze naar de doelaccount. Op dit moment ondersteunt AzCopy v10 alleen blok-blobs tussen twee opslagaccounts kopiëren. Alle andere storage-account-objecten (zoals toevoeg-blobs, pagina-blobs, bestanden, tabellen en wachtrijen) wordt overgeslagen.

## <a name="copy-a-vhd-image-to-a-storage-account"></a>Een VHD-installatiekopie kopiëren naar een opslagaccount

AzCopy v10 standaard worden gegevens geüpload naar blok-blobs. Echter, als een bronbestand heeft een `.vhd` extensie, AzCopy v10 wordt teruggezet op het uploaden naar een pagina-blob. Deze actie is niet op dit moment kunnen worden geconfigureerd.

## <a name="sync-incremental-copy-and-delete-blob-storage-only"></a>Synchronisatie: incrementele kopie en verwijderen (alleen voor Blob storage)

De synchronisatieopdracht synchroniseert inhoud van een bronmap naar een map in het doel, vergelijken van bestandsnamen en tijdstempels het laatst is gewijzigd. Met deze bewerking ook de optionele verwijdering van de doel-bestanden als die nog niet bestaan in de bron wanneer het `--delete-destination=prompt|true` vlag wordt geleverd. Het gedrag van het verwijderen is standaard uitgeschakeld. 

> [!NOTE] 
> Gebruik de `--delete-destination` markering op in de waarschuwing. Schakel de [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) functie voordat u synchroon om te voorkomen dat onbedoelde verwijderingen in uw account verwijderen dergelijk gedrag mogelijk maken. 

> Wanneer `--delete-destination` is ingesteld op true, AzCopy worden bestanden verwijderd die niet zijn opgenomen in de bron van de bestemming zonder een prompt van de gebruiker. Als u worden gevraagd om bevestiging wilt, gebruikt u `--delete-destination=prompt`.

Als u wilt synchroniseren van uw lokale systeem naar een opslagaccount, gebruik de volgende opdracht:

```azcopy
.\azcopy sync "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

U kunt ook een blob-container naar een lokaal bestandssysteem synchroniseren:

```azcopy
# The SAS token isn't required for Azure Active Directory authentication.
.\azcopy sync "https://account.blob.core.windows.net/mycontainer1" "C:\local\path" --recursive=true
```

Met deze opdracht synchroniseert stapsgewijs de bron naar de bestemming op basis van de laatst gewijzigde tijdstempels. Als u toevoegen of verwijderen van een bestand in de bron, doet AzCopy v10 hetzelfde als in de bestemming. Voordat u deze verwijdert vraagt AzCopy u om te bevestigen.

## <a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Voor het configureren van de proxy-instellingen voor AzCopy v10, stelt u de variabele https_proxy omgeving met behulp van de volgende opdracht uit:

```cmd
# For Windows:
set https_proxy=<proxy IP>:<proxy port>
# For Linux:
export https_proxy=<proxy IP>:<proxy port>
# For MacOS
export https_proxy=<proxy IP>:<proxy port>
```

### <a name="optimize-throughput"></a>Optimaliseren van doorvoer

Stel de omgevingsvariabele AZCOPY_CONCURRENCY_VALUE te configureren van het aantal gelijktijdige aanvragen, en voor het beheren van het verbruik van prestatie- en doorvoer. De waarde is standaard ingesteld op 300. Vermindering van de waarde, wordt de bandbreedte en CPU-gebruik door AzCopy v10 beperkt.

```cmd
# For Windows:
set AZCOPY_CONCURRENCY_VALUE=<value>
# For Linux:
export AZCOPY_CONCURRENCY_VALUE=<value>
# For MacOS
export AZCOPY_CONCURRENCY_VALUE=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank then the default value is currently in use
```

### <a name="change-the-location-of-the-log-files"></a>Wijzig de locatie van de logboekbestanden

U kunt de locatie van de logboekbestanden indien nodig, of om te voorkomen dat de besturingssysteemschijf wordt ingenomen wijzigen.

```cmd
# For Windows:
set AZCOPY_LOG_LOCATION=<value>
# For Linux:
export AZCOPY_LOG_LOCATION=<value>
# For MacOS
export AZCOPY_LOG_LOCATION=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank, then the default value is currently in use
```
### <a name="change-the-default-log-level"></a>Het standaardniveau wijzigen 

AzCopy logboek-niveau is standaard ingesteld op INFO. Als u beperken, het detailniveau van logboekbestanden om schijfruimte te besparen wilt, overschrijft de instelling via ``--log-level`` optie. Beschikbare logboekniveaus zijn: Fouten opsporen, INFO, waarschuwing, fout, PANIEK en onherstelbare fout.

### <a name="review-the-logs-for-errors"></a>Bekijk de logboeken voor fouten

De volgende opdracht ontvangt alle fouten met UPLOADFAILED status van het logboek 04dc9ca9-158f-7945-5933-564021086c79:

```azcopy
cat 04dc9ca9-158f-7945-5933-564021086c79.log | grep -i UPLOADFAILED
```
## <a name="troubleshooting"></a>Problemen oplossen

AzCopy v10 logboekbestanden en plan bestanden voor elke taak gemaakt. U kunt de logboeken kunt gebruiken om te onderzoeken en oplossen van mogelijke problemen. De logboeken van de status van de fout (UPLOADFAILED, COPYFAILED en DOWNLOADFAILED), bevat het volledige pad en de reden van de fout. De taaklogboeken van de en plan-bestanden bevinden zich in de map % USERPROFILE\\.azcopy map op Windows of $HOME\\.azcopy map op Mac en Linux.

> [!IMPORTANT]
> Bij het indienen van een aanvraag voor het Microsoft Support (of het probleem met betrekking tot een derde partij op te lossen), delen de geredigeerde versie van de opdracht die u wilt uitvoeren. Dit zorgt ervoor dat de SAS niet per ongeluk worden gedeeld met iedereen. U vindt de geredigeerde versie aan het begin van het logboekbestand.

### <a name="view-and-resume-jobs"></a>Weergeven en taken hervatten

Elke overdrachtbewerking wordt een AzCopy-taak maken. Gebruik de volgende opdracht om de geschiedenis van taken weer te geven:

```azcopy
.\azcopy jobs list
```

Als u wilt weergeven van statistieken voor de taak, gebruik de volgende opdracht:

```azcopy
.\azcopy jobs show <job-id>
```

Als u wilt filteren de overdrachten op status, gebruik de volgende opdracht:

```azcopy
.\azcopy jobs show <job-id> --with-status=Failed
```

Gebruik de volgende opdracht uit om te hervatten van een taak is mislukt/geannuleerd. Met deze opdracht maakt gebruik van de id samen met het SAS-token. Het is niet persistent uit veiligheidsoverwegingen:

```azcopy
.\azcopy jobs resume <jobid> --sourcesastokenhere --destinationsastokenhere
```

## <a name="next-steps"></a>Volgende stappen

Als u vragen of problemen met algemene feedback te sturen, dient u ze [op GitHub](https://github.com/Azure/azure-storage-azcopy.).



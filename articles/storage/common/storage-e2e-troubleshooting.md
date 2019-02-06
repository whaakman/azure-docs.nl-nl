---
title: Azure Storage oplossen van problemen met diagnostische gegevens en Message Analyzer | Microsoft Docs
description: Een zelfstudie om aan te tonen end-to-end oplossen van problemen met Azure Storage Analytics, AzCopy en Microsoft Message Analyzer
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5a160ef767909814e363dbb692e58c30783aaf6f
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746299"
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>End-to-end problemen oplossen met behulp van Azure Storage metrische gegevens en logboekregistratie, AzCopy en Message Analyzer
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Is een belangrijke vaardigheden voor het bouwen en de client-toepassingen met Microsoft Azure Storage ondersteuning vaststellen en oplossen van problemen. Door de gedistribueerde aard van een Azure-toepassing, het vaststellen van en het oplossen van fouten en prestatieproblemen mogelijk complexer dan in traditionele-omgevingen.

In deze zelfstudie laten we zien hoe u bepaalde fouten die mogelijk invloed hebben op prestaties en los deze fouten op voor end-to-end kunt identificeren met behulp van hulpprogramma's van Microsoft en Azure Storage, om te optimaliseren van de clienttoepassing.

Deze zelfstudie bevat een praktische verkenning van een end-to-end-scenario voor het oplossen van problemen. Zie voor een uitgebreide algemene handleiding voor het oplossen van Azure storage-toepassingen, [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Hulpprogramma's voor het oplossen van Azure Storage-toepassingen
Om op te lossen met behulp van Microsoft Azure Storage-clienttoepassingen, kunt u een combinatie van hulpprogramma's om te bepalen wanneer er een probleem is opgetreden en wat de oorzaak van het probleem mogelijk. Tot deze hulpmiddelen behoren onder meer:

* **Azure Storage Analytics**. [Azure Storage Analytics](/rest/api/storageservices/Storage-Analytics) metrische gegevens en logboekregistratie voor Azure Storage biedt.
  
  * **Metrische opslaggegevens** transactie metrische gegevens en metrische gegevens over capaciteit voor uw opslagaccount worden bijgehouden. Met metrische gegevens, kunt u bepalen hoe uw toepassing wordt uitgevoerd op basis van tal van verschillende metingen. Zie [tabelschema van metrische gegevens van Storage Analytics](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) voor meer informatie over de typen van metrische gegevens met Storage Analytics wordt bijgehouden.
  * **Logboekregistratie van opslag** elke aanvraag vastgelegd in de Azure Storage-services in een logboek serverzijde. Het logboek worden gedetailleerde gegevens voor elke aanvraag, met inbegrip van de bewerking die wordt uitgevoerd, de status van de bewerking en latentie informatie bijgehouden. Zie [Storage Analytics logboekindeling](/rest/api/storageservices/Storage-Analytics-Log-Format) voor meer informatie over de aanvraag- en gegevens die worden geschreven naar de logboeken door Storage Analytics.

* **Azure-portal**. U kunt metrische gegevens en logboekregistratie configureren voor uw opslagaccount in de [Azure-portal](https://portal.azure.com). U kunt ook diagrammen en grafieken die laten zien hoe uw toepassing wordt uitgevoerd na verloop van tijd weergeven en configureren van waarschuwingen om u te waarschuwen als uw toepassing wordt uitgevoerd anders zijn dan verwacht voor een opgegeven waarde.
  
    Zie [een opslagaccount in Azure portal controleren](storage-monitor-storage-account.md) voor informatie over het configureren van bewaking in Azure portal.
* **AzCopy**. Serverlogboeken voor Azure Storage worden opgeslagen als blobs, zodat u AzCopy gebruiken kunt om te kopiëren van blobs in de logboeken naar een lokale map voor analyse met behulp van Microsoft Message Analyzer. Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) voor meer informatie over AzCopy.
* **Microsoft Message Analyzer**. Message Analyzer is een hulpprogramma dat logboekbestanden verbruikt en wordt weergegeven logboekgegevens in een visuele indeling waarmee u eenvoudig kunt filter, zoeken en groep-logboekgegevens in nuttig sets die u gebruiken kunt om fouten en problemen met de prestaties te analyseren. Zie [Microsoft Message Analyzer-besturingssysteem handleiding](https://technet.microsoft.com/library/jj649776.aspx) voor meer informatie over Message Analyzer.

## <a name="about-the-sample-scenario"></a>Over de voorbeeldscenario
Voor deze zelfstudie, kijken we naar de een scenario waarbij Azure Storage metrics geeft aan dat een laag percentage slagingspercentage voor een toepassing die Azure storage aanroept. De lage percentage voltooid percentage metrische gegevens (weergegeven als **PercentSuccess** in de [Azure-portal](https://portal.azure.com) en in de tabellen voor metrische gegevens) worden bijgehouden bewerkingen die slagen, maar die een HTTP-statuscode die groter is dan 299 retourneren. In de logboekbestanden van de server-side-opslag, deze bewerkingen worden geregistreerd met de transactiestatus van de **ClientOtherErrors**. Zie voor meer informatie over de metriek lage percentage voltooid [prestatiegegevens geven lage PercentSuccess of vermeldingen in het logboek analytics hebt bewerkingen met de status van ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure Storage-bewerkingen kunnen retourneren HTTP-statuscodes groter is dan 299 als onderdeel van hun normale functionaliteit. Maar deze fouten in sommige gevallen geven aan dat u mogelijk uw clienttoepassing voor betere prestaties te optimaliseren.

In dit scenario zult wij beschouwen als een laag percentage slagingspercentage iets minder dan 100% zijn. U kunt echter een ander niveau van de metrische gegevens kiezen op basis van uw behoeften. Het is verstandig dat tijdens het testen van uw toepassing, de tolerantie van een basislijn voor uw belangrijkste prestatiemetrieken. Bijvoorbeeld, u kunt bepalen, op basis van testen, dat uw toepassing een consistente percentage slagingspercentage van 90% of 85% mag hebben. Als uw metrische gegevens ziet u dat de toepassing is die afwijken van dit getal, dan kunt u onderzoeken wat kan worden veroorzaakt door de toename.

Voor onze voorbeeldscenario nadat we hebben vastgesteld dat het percentage voltooid percentage metrische waarde lager is dan 100%, bespreken we de logboeken om te vinden van de fouten die overeenstemmen met de metrische gegevens en deze gebruiken om te achterhalen wat de oorzaak van het lagere tarief voor percentage voltooid. We kijken om precies fouten in het bereik van 400. Vervolgens we beter aansluiten een 404 (niet gevonden)-fouten bekijken moet.

### <a name="some-causes-of-400-range-errors"></a>Enkele oorzaken van fouten van 400-bereik
De volgende voorbeelden ziet u een voorbeeld van enkele fouten 400-bereik voor aanvragen voor Azure Blob Storage en hun mogelijke oorzaken. Een van deze fouten, evenals de fouten in de 300 bereik en het bereik 500 kan bijdragen aan een laag percentage voltooid-tarief.

Houd er rekening mee dat in de lijsten hieronder ver ligt voltooid zijn. Zie [Status en foutcodes](https://msdn.microsoft.com/library/azure/dd179382.aspx) op MSDN voor meer informatie over algemene Azure Storage-fouten en fouten die specifiek zijn voor elk van de storage-services.

**Status 404 (niet gevonden) codevoorbeelden**

Treedt op wanneer een leesbewerking op basis van een container of blob mislukt, omdat de blob of container is niet gevonden.

* Treedt op als een container of blob is verwijderd door een andere client voordat deze aanvraag.
* Treedt op als u een API-aanroep die wordt gemaakt van de container of blob nadat u hebt gecontroleerd of deze bestaat. De APIs CreateIfNotExists gebeld HEAD eerst om te controleren of er sprake van de container of blob; Als deze niet bestaat, een 404-fout wordt geretourneerd en vervolgens een tweede PUT-aanroep is gedaan om te schrijven van de container of blob.

**Statuscode 409 (Conflict)-voorbeelden**

* Treedt op als u een API maken voor het maken van een nieuwe container of blob, zonder te controleren of aanwezigheid eerst, en een container of blob met die naam al bestaat.
* Treedt op als een container wordt verwijderd en u probeert te maken van een nieuwe container met dezelfde naam voordat de verwijderingsbewerking voltooid is.
* Treedt op als u een lease op een container of blob opgeven en er al een lease aanwezig zijn is.

**Statuscode 412 (voorwaarde is mislukt) voorbeelden**

* Treedt op wanneer de voorwaarde die is opgegeven door een voorwaardelijke header niet is voldaan.
* Treedt op wanneer de opgegeven lease-ID komt niet overeen met de lease-ID op de container of blob.

## <a name="generate-log-files-for-analysis"></a>Genereren van logboekbestanden voor analyse
In deze zelfstudie gebruiken we Message Analyzer om te werken met drie verschillende typen van logbestanden, maar u ervoor kiezen kunt om te werken met een van de volgende:

* De **serverlogboek**, die wordt gemaakt wanneer u Azure Storage-logboekregistratie inschakelen. Logboek van de server bevat gegevens over elke bewerking die wordt aangeroepen voor een van de services van Azure Storage - blob, wachtrij, tabel en bestand. Logboek van de server geeft aan welke bewerking is aangeroepen en welke statuscode is geretourneerd, evenals andere informatie over de aanvraag en respons.
* De **.NET-clientlogboek**, die wordt gemaakt wanneer u logboekregistratie van client-side binnen uw .NET-toepassing inschakelen. Het clientlogboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord.
* De **HTTP-netwerk het traceringslogboek**, verzamelt gegevens over HTTP/HTTPS-aanvraag en respons gegevens, inclusief voor bewerkingen op basis van Azure Storage. In deze zelfstudie genereren we de netwerktracering via Message Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Server-logboekregistratie en metrische gegevens configureren
Eerst, we moet configureren voor Azure Storage-logboekregistratie en metrische gegevens, zodat we de gegevens van de clienttoepassing voor het analyseren van hebben. U kunt logboekregistratie en metrische gegevens op verschillende manieren - configureren de [Azure-portal](https://portal.azure.com), met behulp van PowerShell, of via een programma. Zie [metrische opslaggegevens inschakelen en weergeven van metrische gegevens](https://msdn.microsoft.com/library/azure/dn782843.aspx) en [vastleggen van Storage inschakelen en toegang krijgen tot logboekgegevens](https://msdn.microsoft.com/library/azure/dn782840.aspx) op MSDN voor meer informatie over het configureren van logboekregistratie en metrische gegevens.

**Via de Azure-portal**

Het configureren van logboekregistratie en metrische gegevens voor uw storage account met de [Azure-portal](https://portal.azure.com), volg de instructies op [een opslagaccount in Azure portal controleren](storage-monitor-storage-account.md).

> [!NOTE]
> Het is niet mogelijk om in te stellen minuut metrische gegevens met behulp van de Azure portal. We raden echter aan dat u ze stelt voor het doel van deze zelfstudie, en voor het onderzoeken van prestatieproblemen met uw toepassing. U kunt één minuut metrische gegevens met behulp van PowerShell, zoals hieronder wordt weergegeven of programmatisch met behulp van de storage-clientbibliotheek kunt instellen.
> 
> Houd er rekening mee dat de minuut metrische gegevens, alleen per uur metrische gegevens kan niet worden weergegeven door de Azure portal.
> 
> 

**Via PowerShell**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u wilt aan de slag met PowerShell voor Azure, Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

1. Gebruik de [toevoegen AzAccount](/powershell/module/servicemanagement/azure/add-azureaccount) cmdlet aan uw account Azure-gebruiker toevoegen aan het PowerShell-venster:
   
    ```powershell
    Add-AzAccount
    ```

2. In de **aanmelden bij Microsoft Azure** venster, typt u het e-mailadres en wachtwoord die zijn gekoppeld aan uw account. Azure verifieert de referentiegegevens en slaat deze op. Vervolgens wordt het venster gesloten.
3. Stel het standaardopslagaccount naar het opslagaccount dat u voor de zelfstudie gebruikt door het uitvoeren van deze opdrachten in de PowerShell-venster:
   
    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Logboekregistratie van opslag voor de Blob-service inschakelen:
   
    ```powershell
    Set-AzStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. Inschakelen van metrische gegevens van storage voor de Blob-service, waardoor ervoor dat u instelt **- MetricsType** naar `Minute`:
   
    ```powershell
    Set-AzStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET-clientzijde logboekregistratie configureren
Schakel .NET diagnostische gegevens in het configuratiebestand van de toepassing (web.config of app.config) voor het configureren van client-side-logboekregistratie voor een .NET-toepassing. Zie [Client-side logboekregistratie met de .NET-Opslagclientbibliotheek](https://msdn.microsoft.com/library/azure/dn782839.aspx) en [Client-side logboekregistratie met de Microsoft Azure Storage SDK voor Java](https://msdn.microsoft.com/library/azure/dn782844.aspx) op MSDN voor meer informatie.

Het client-side-logboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord.

De Opslagclientbibliotheek slaat client-side-logboekgegevens in de opgegeven locatie in het configuratiebestand van de toepassing (web.config of app.config).

### <a name="collect-a-network-trace"></a>Een netwerktracering verzamelen
U kunt Message Analyzer gebruiken voor het verzamelen van een HTTP/HTTPS-netwerktracering terwijl u de clienttoepassing wordt uitgevoerd. Message Analyzer gebruikt [Fiddler](http://www.telerik.com/fiddler) op de back-end. Voordat u de netwerktracering hebt verzameld, wordt u aangeraden dat u Fiddler om vast te leggen van niet-versleutelde HTTPS-verkeer configureren:

1. Installeer [Fiddler](http://www.telerik.com/download/fiddler).
2. Start Fiddler.
3. Selecteer **hulpprogramma's | Opties voor fiddler**.
4. Zorg ervoor dat in het dialoogvenster opties **vastleggen HTTPS maakt verbinding** en **HTTPS-verkeer ontsleutelen** beide zijn geselecteerd, zoals hieronder wordt weergegeven.

![Fiddler-opties configureren](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Voor deze zelfstudie verzamelen en een netwerktracering eerst opslaan in Message Analyzer en vervolgens een analysis-sessie voor het analyseren van de tracering en de logboeken maken. Voor het verzamelen van een netwerktracering in Message Analyzer:

1. Selecteer in Message Analyzer, **bestand | Snelle Trace | Niet-versleutelde HTTPS**.
2. De tracering worden onmiddellijk toegepast. Selecteer **stoppen** de tracering stoppen zodat we deze trace opslag alleen verkeer kunt configureren.
3. Selecteer **bewerken** de traceringssessie bewerken.
4. Selecteer de **configureren** koppelen aan de rechterkant van de **Microsoft-Pef-WebProxy** ETW-provider.
5. In de **geavanceerde instellingen** dialoogvenster, klikt u op de **Provider** tabblad.
6. In de **Hostname Filter** veld, geeft u de eindpunten van uw opslag, gescheiden door spaties. Bijvoorbeeld, kunt u uw eindpunten als volgt; Wijzig `storagesample` op de naam van uw storage-account:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Het dialoogvenster sluiten en klik op **opnieuw** om te beginnen met het verzamelen van de tracering met het filter hostname aanwezig is, zodat alleen Azure Storage-netwerkverkeer is opgenomen in de tracering.

> [!NOTE]
> Nadat u klaar bent met het verzamelen van uw netwerk-trace, wordt aangeraden dat u de instellingen die u mogelijk zijn gewijzigd in Fiddler te ontsleutelen HTTPS-verkeer herstellen. Schakel in het dialoogvenster Opties voor Fiddler de **vastleggen HTTPS maakt verbinding** en **HTTPS-verkeer ontsleutelen** selectievakjes.
> 
> 

Zie [met behulp van de functies voor het traceren van Network](https://technet.microsoft.com/library/jj674819.aspx) op Technet voor meer informatie.

## <a name="review-metrics-data-in-the-azure-portal"></a>Bekijk metrische gegevens in Azure portal
Zodra uw toepassing actief is geweest gedurende een bepaalde periode, kunt u lezen de grafieken met metrische gegevens die worden weergegeven in de [Azure-portal](https://portal.azure.com) om te zien hoe uw service heeft zijn uitgevoerd.

Navigeer naar uw opslagaccount in Azure portal. Standaard een controle grafiek met de **percentage geslaagde** metrische gegevens op de accountblade wordt weergegeven. Als u de grafiek om verschillende metrische gegevens weer te geven eerder hebt gewijzigd, voegt u toe de **percentage geslaagde** metrische gegevens.

U ziet nu **percentage geslaagde** in de controle grafiek, samen met andere metrische gegevens u hebt toegevoegd. In het scenario dat we vervolgens onderzoeken door het analyseren van de logboeken in Message Analyzer, is het percentage slagingspercentage iets minder dan 100%.

Zie voor meer informatie over het toevoegen en aanpassen van grafieken met metrische gegevens [grafieken met metrische gegevens aanpassen](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Het duurt even voordat uw metrische gegevens worden weergegeven in de Azure-portal nadat u metrische opslaggegevens inschakelen. Dit komt doordat het uurtarief metrische gegevens voor het vorige uur worden niet weergegeven in de Azure-portal totdat het huidige uur is verstreken. Bovendien worden minuut metrische gegevens momenteel niet weergegeven in de Azure-portal. Daarom afhankelijk van wanneer u metrische gegevens inschakelen, het duurt maximaal twee uur om te zien van metrische gegevens.
> 
> 

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>AzCopy gebruiken om te kopiëren van Logboeken van server naar een lokale map
Azure-opslag geschreven logboekgegevens van de server tot blobs, terwijl metrische gegevens worden geschreven naar tabellen. Logboek blobs zijn beschikbaar in de bekende `$logs` container voor uw storage-account. Logboek blobs zijn hiërarchisch benoemde door jaar, maand, dag en uur, zodat u het bereik van de tijd die u wilt onderzoeken eenvoudig kan vinden. Bijvoorbeeld, in de `storagesample` account, de container voor de blobs in de logboeken voor 01-02-2015 van 8 en 9 uur 's middags, `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. De afzonderlijke blobs in deze container zijn met de naam sequentieel worden verwerkt, beginnend met `000000.log`.

U kunt de AzCopy-opdrachtregelprogramma voor het downloaden van deze server-side-logboekbestanden naar een locatie van uw keuze op uw lokale computer. U kunt bijvoorbeeld de volgende opdracht gebruiken voor het downloaden van de logboekbestanden voor blob-bewerkingen die plaatsgevonden op 2 januari 2015 naar de map heeft `C:\Temp\Logs\Server`; vervangen `<storageaccountname>` met de naam van uw opslagaccount en `<storageaccountkey>` door de toegangssleutel voor uw account :

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy is beschikbaar voor downloaden op de [Azure Downloads](https://azure.microsoft.com/downloads/) pagina. Zie voor meer informatie over het gebruik van AzCopy [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

Zie voor meer informatie over het downloaden van server-side-Logboeken, [logboekgegevens downloaden Storage Logging](https://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Microsoft Message Analyzer gebruiken voor het analyseren van logboekgegevens
Microsoft Message Analyzer is een hulpprogramma voor het vastleggen, weergeven en analyseren van protocol verkeer, gebeurtenissen en andere systemen of toepassingen berichten in scenario's voor het oplossen van problemen en diagnostische berichten. Message Analyzer ook kunt u laden, aggregeren en analyseren van gegevens uit logboek en opgeslagen logboekbestanden voor tracering. Zie voor meer informatie over Message Analyzer, [Microsoft Message Analyzer-besturingssysteem handleiding](https://technet.microsoft.com/library/jj649776.aspx).

Message Analyzer bevat activa voor de Azure-opslag die u helpen bij het analyseren van de server, client en netwerklogboeken. In deze sectie wordt besproken hoe u kunt deze hulpprogramma's gebruiken om het probleem van laag percentage voltooid in de opslaglogboeken.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Download en installeer Message Analyzer en de Azure Storage-activa
1. Download [Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) van het Microsoft Downloadcentrum en voer het installatieprogramma.
2. Message Analyzer starten.
3. Uit de **extra** in het menu **Asset Intelligence-beheerder**. In de **Asset Intelligence-beheerder** dialoogvenster, selecteer **Downloads**, vervolgens te filteren op **Azure Storage**. U ziet de activa van de Azure-opslag, zoals wordt weergegeven in de volgende afbeelding.
4. Klik op **synchronisatie alle weergegeven objecten** voor het installeren van de Azure Storage-activa. De beschikbare elementen zijn onder andere:
   * **Kleurenregels voor Azure Storage:** Kleurenregels voor Azure Storage kunnen u speciale filters definiëren waarmee u kleur, tekst en lettertypen gebruiken om te markeren van berichten die specifieke informatie in een tracering bevatten.
   * **Azure Storage-grafieken:** Azure Storage-grafieken zijn vooraf gedefinieerde grafieken die een graph-logboekgegevens van de server. Houd er rekening mee dat voor het gebruik van Azure Storage-grafieken op dit moment, u alleen logboek van de server in het raster analyse laden mogelijk.
   * **Azure Storage-Parsers:** De Azure Storage-parsers parseren van de Azure Storage client-, server- en HTTP-Logboeken om ze in het raster analyse weer te geven.
   * **Azure Storage-Filters:** Azure Storage-filters zijn vooraf gedefinieerde criteria die u gebruiken kunt voor uw gegevens in het raster analyse op te vragen.
   * **Azure Storage weergave-indelingen:** Azure Storage weergavelay-outs zijn vooraf gedefinieerde kolom lay-outs en groeperingen kunt zien in het raster analyse.
5. Message Analyzer opnieuw nadat u de activa hebt geïnstalleerd.

![Message Analyzer Asset Intelligence-beheerder](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Installeer alle van de Azure Storage-activa die worden weergegeven voor de doeleinden van deze zelfstudie.
> 
> 

### <a name="import-your-log-files-into-message-analyzer"></a>Importeren van uw logboekbestanden in Message Analyzer
U kunt al uw opgeslagen logboekbestanden (serverzijde, client-side en netwerk) importeren in één sessie in Microsoft Message Analyzer voor analyse.

1. Op de **bestand** in Microsoft Message Analyzer, klik daarna op **nieuwe sessie**, en klik vervolgens op **leeg sessie**. In de **nieuwe sessie** dialoogvenster, Geef een naam voor de analysis-sessie. In de **sessiegegevens** deelvenster, klikt u op de **bestanden** knop.
2. Klik op het netwerk traceren om gegevens te laden die zijn gegenereerd door Message Analyzer, **bestanden toevoegen**, blader naar de locatie waar u uw .matp-bestand van uw web-traceringssessie hebt opgeslagen, selecteert u het bestand .matp en klikt u op **Open**.
3. Voor het laden van de server-side-logboekgegevens, klikt u op **bestanden toevoegen**, blader naar de locatie waar u uw server-side-Logboeken hebt gedownload, selecteert u de logboekbestanden voor het tijdsbereik dat u wilt analyseren, en klikt u op **Open**. Vervolgens in de **sessiegegevens** deelvenster, stelt u de **tekst logboekconfiguratie** vervolgkeuzelijst voor elke server-side-logboekbestand te **AzureStorageLog** om ervoor te zorgen dat Microsoft Message Analyzer kan het logboekbestand correct worden geparseerd.
4. Voor het laden van de client-side-logboekgegevens, klikt u op **bestanden toevoegen**, blader naar de locatie waar u uw client-side-Logboeken hebt opgeslagen, selecteert u de logboekbestanden die u wilt analyseren, en klikt u op **Open**. Vervolgens in de **sessiegegevens** deelvenster, stelt u de **tekst logboekconfiguratie** vervolgkeuzelijst voor elke client-side-logboekbestand te **AzureStorageClientDotNetV4** om ervoor te zorgen dat Microsoft Message Analyzer kan het logboekbestand correct worden geparseerd.
5. Klik op **Start** in de **nieuwe sessie** dialoogvenster laden en parseren van de logboekgegevens. De logboekgegevens worden weergegeven in het raster Message Analyzer analyse.

De volgende afbeelding ziet u een voorbeeld-sessie die is geconfigureerd met de server, client en netwerk-logboekbestanden voor tracering.

![Message Analyzer-sessie configureren](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Houd er rekening mee dat Message Analyzer logboekbestanden in het geheugen geladen. Als u een groot aantal logboekgegevens hebt, wordt u om op te halen van de beste prestaties van Message Analyzer filteren.

Bepaal eerst het tijdsbestek dat u geïnteresseerd bent in het controleren van, en dit tijdsbestek zo klein mogelijk te houden. In veel gevallen wilt u een periode van minuten of uren maximaal bekijken. Importeer de kleinste set logboeken die aan uw behoeften kan voldoen.

Als u nog steeds een grote hoeveelheid gegevens hebt, kunt klikt u vervolgens u om op te geven van een sessie te filteren dat uw logboekgegevens voordat u deze te laden. In de **sessie Filter** Schakel de **bibliotheek** klikken om een vooraf gedefinieerde filter kiezen; bijvoorbeeld, kiest u **globale tijd Filter I** filters uit de Azure-opslag om te filteren op een bepaalde periode. Vervolgens kunt u de filtercriteria om op te geven van het eerste en laatste timestamp voor het interval dat u wilt zien. U kunt ook filteren op een bepaalde statuscode te wijzigen; u kunt bijvoorbeeld alleen logboekvermeldingen waar de statuscode 404 wordt geladen.

Zie voor meer informatie over het importeren van gegevens aan het logboek in Microsoft Message Analyzer [bij het ophalen van gegevens voor het clientbericht](https://technet.microsoft.com/library/dn772437.aspx) op TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>De clientaanvraag-ID gebruiken voor het correleren van gegevens uit een bestand log
De Azure Storage-clientbibliotheek genereert automatisch een unieke client-aanvraag-ID voor elke aanvraag. Deze waarde wordt geschreven naar het logboek van de client, het logboek voor de server en de netwerktracering, zodat u deze gebruiken kunt voor het correleren van gegevens in alle drie logboeken binnen Message Analyzer. Zie [clientaanvraag-ID](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) vragen voor meer informatie over de client ID.

De volgende secties wordt beschreven hoe u de lay-van een vooraf geconfigureerde en aangepaste outweergaven gebruiken om te correleren en groepsgegevens op basis van de client-request-ID.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Selecteer een weergave-indeling om weer te geven in het raster analyse
De activa van de opslag voor Message Analyzer omvatten Azure Storage-weergave indelingen, die zijn vooraf geconfigureerde weergaven die u gebruiken kunt om uw gegevens met nuttige groeperingen en kolommen voor verschillende scenario's weer te geven. U kunt ook aangepaste weergave-indelingen maken en opslaan voor hergebruik.

De afbeelding hieronder toont de **lay-out** menu beschikbaar door te selecteren **lay-out** op het lint van de werkbalk. De weergave-indelingen voor Azure Storage zijn gegroepeerd onder de **Azure Storage** knooppunt in het menu. U kunt zoeken naar `Azure Storage` in het zoekvak om te filteren op Azure-opslag alleen indelingen bekijken. U kunt ook de ster naast een weergave-indeling om een favoriet maken en weer te geven aan de bovenkant van het menu selecteren.

![Weergavemenu-indeling](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Begint met, selecteer **gegroepeerd op ClientRequestID en Module**. Deze groepen weergeven lay-out zich gegevens uit alle drie logboeken eerst op client-request-ID en klik vervolgens op bron-logboekbestand (of **Module** in Message Analyzer). In deze weergave kunt u inzoomen op een bepaalde client-aanvraag-ID en gegevens uit alle drie logboekbestanden voor die clientaanvraag-id.

De afbeelding hieronder toont deze weergave toegepast op de voorbeeldgegevens logboek wordt weergegeven, met een subset van kolommen die worden weergegeven. U kunt zien dat voor een bepaalde client-aanvraag-ID, het raster analyse worden gegevens van het logboek van de client, serverlogboek en netwerktracering weergegeven.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Verschillende logboekbestanden hebben verschillende kolommen, zodat wanneer gegevens uit meerdere logboekbestanden wordt weergegeven in het raster analyse, enkele kolommen geen gegevens voor een bepaalde rij bevat kunnen. Bijvoorbeeld, in de gaten, de client log rijen niet meer weergeven geen gegevens voor de **Timestamp**, **TimeElapsed**, **bron**, en **bestemming**kolommen, omdat deze kolommen niet bestaan in het logboek van de client, maar nog aanwezig zijn in de netwerk-trace. Op dezelfde manier de **Timestamp** kolom timestamp gegevens uit het logboek voor de server worden weergegeven, maar er zijn geen gegevens weergegeven voor de **TimeElapsed**, **bron**, en  **Bestemming** kolommen die geen deel uitmaken van het logboek voor de server.
> 
> 

Naast het gebruik van de weergave-indelingen van Azure Storage, kunt u ook definiëren en opslaan van uw eigen weergave-indelingen. U kunt andere gewenste velden voor het groeperen van gegevens selecteren en de groepering opslaan als onderdeel van uw aangepaste lay-out ook.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Kleurenregels toepassen op het raster analyse
De opslag-activa ook kleurenregels die bieden een visueel element betekent dat voor het identificeren van verschillende typen fouten in het raster analyse. De van de vooraf gedefinieerde kleurenregels van toepassing op HTTP-fouten, zodat ze alleen voor de server-logboek- en netwerk-trace weergegeven.

Selecteren om toe te passen hoe zij kleuren, **kleurenregels** op het lint van de werkbalk. Hier ziet u de Azure Storage-kleurenregels in het menu. Selecteer voor deze zelfstudie **clientfouten (StatusCode tussen 400 en 499)**, zoals wordt weergegeven in de volgende afbeelding.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Naast het gebruik van de regels van de kleur van Azure Storage, kunt u ook definiëren en opslaan van uw eigen kleurenregels.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Groeperen en filteren logboekgegevens op 400-bereik fouten gevonden
Nu we groeperen en filteren van de logboekgegevens als u wilt zoeken van alle fouten in het bereik van 400.

1. Zoek de **StatusCode** kolom in het raster analyse met de rechtermuisknop op de kolom Titel en selecteer **groep**.
2. Vervolgens groeperen op de **ClientRequestId** kolom. U ziet dat de gegevens in het raster Analysis nu zijn ingedeeld door de statuscode en client-aanvraag-ID.
3. Het venster Filter hulpprogramma weergegeven als deze nog niet wordt weergegeven. Selecteer op de werkbalk van het lint **hulpprogramma Windows**, klikt u vervolgens **weergavefilter**.
4. Het om logboekgegevens te filteren om alleen 400-bereik fouten weer te geven, toevoegen de volgende filtercriteria voldoen aan de **weergavefilter** venster en klikt u op **toepassen**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

De volgende afbeelding toont de resultaten van deze groeperen en filteren. Uitbreiden van de **ClientRequestID** veld onder de groepering voor statuscode 409, ziet u bijvoorbeeld een bewerking die leidde tot deze statuscode.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Nadat dit filter is toegepast, ziet u dat rijen van het logboek van de client zijn uitgesloten, als de client logboek bevat geen een **StatusCode** kolom. Te beginnen met de server en de netwerk-traceerlogboeken vinden 404-fouten gecontroleerd en vervolgens wordt geretourneerd en wordt naar het clientlogboek om te controleren van de clientbewerkingen die hebben geleid tot ze.

> [!NOTE]
> U kunt filteren op de **StatusCode** kolom en nog steeds gegevens weergeven van alle drie logboeken, met inbegrip van het logboek van de client, als u een expressie toevoegt aan het filter met logboekvermeldingen waar de statuscode null is. Kan dit filterexpressie, gebruikt u:
> 
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
> 
> Dit filter retourneert alle rijen van de client logboek- en alleen de rijen van de serverlogboek- en HTTP-logboek waar de statuscode groter is dan 400. Als u deze op de weergave-indeling gegroepeerd op aanvraag-ID en -module toepast, kunt u zoeken of bladeren door de logboekvermeldingen te vinden die waar alle drie logboeken worden weergegeven.   
> 
> 

### <a name="filter-log-data-to-find-404-errors"></a>De logboekgegevens filter om te zoeken 404-fouten
De opslag-activa zijn vooraf gedefinieerde filters die u gebruiken kunt om te beperken van logboekgegevens om de fouten of trends die u zoekt te vinden. Vervolgens gaat we twee vooraf gedefinieerde filters toepassen: één waarmee de server en de netwerk-traceerlogboeken voor 404-fouten worden gefilterd en één filter de gegevens op een opgegeven tijdperiode.

1. Het venster Filter hulpprogramma weergegeven als deze nog niet wordt weergegeven. Selecteer op de werkbalk van het lint **hulpprogramma Windows**, klikt u vervolgens **weergavefilter**.
2. Selecteer in het venster Filter **bibliotheek**, en zoeken op `Azure Storage` vinden van de Azure Storage-filters. Selecteer het filter voor **404 (niet gevonden)-berichten in alle logboeken**.
3. Weergave de **bibliotheek** menu opnieuw, en zoek en selecteer de **wereldwijde tijdfilter**.
4. Bewerk de tijdstempels die wordt weergegeven in het filter aan het bereik dat u wilt weergeven. Dit helpt om te beperken van het bereik van gegevens te analyseren.
5. Het filter moet worden weergegeven die vergelijkbaar is met het volgende voorbeeld. Klik op **toepassen** het filter toepassen op het raster analyse.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analyseren van uw logboekgegevens
Nu dat u hebt gegroepeerd en gefilterd van uw gegevens, kunt u de details van afzonderlijke aanvragen die 404-fouten gegenereerd kunt onderzoeken. De gegevens is in de huidige weergave-indeling gegroepeerd op de clientaanvraag-ID, klikt u vervolgens door log-bron. Omdat we op aanvragen waarin het veld StatusCode 404 bevat filtert, bekijken we alleen de server en traceringsgegevens voor netwerk, niet de logboekgegevens van de client.

De volgende afbeelding ziet u een specifieke aanvraag wanneer een Blob ophalen-bewerking resulteerde in een 404-fout omdat de blob niet bestaat. Houd er rekening mee dat sommige kolommen zijn verwijderd uit de standard-weergave om de relevante gegevens weer te geven.

![Gefilterde Server en netwerk-traceerlogboeken](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Er wordt vervolgens deze clientaanvraag-ID correleren met de logboekgegevens van de client om te zien welke acties de client duurt toen de fout is opgetreden. U kunt voor deze sessie om de client-logboekgegevens, die wordt geopend in een tweede tabblad weer te geven van een nieuwe Analysis-rasterweergave weergegeven:

1. Kopieer eerst de waarde van de **ClientRequestId** veld naar het Klembord. U kunt dit doen door een rij selecteren, zoeken naar de **ClientRequestId** veld met de rechtermuisknop op de gegevenswaarde en het kiezen van **kopie 'ClientRequestId'**.
2. Selecteer op de werkbalk van het lint **nieuwe Viewer**en selecteer vervolgens **Analysis Grid** om een nieuw tabblad te openen. Het nieuwe tabblad bevat alle gegevens in uw logboekbestanden, zonder te groeperen, filteren of kleurenregels.
3. Selecteer op de werkbalk van het lint **lay-out**en selecteer vervolgens **alle kolommen van de .NET Client** onder de **Azure Storage** sectie. Deze weergave-indeling bevat gegevens van de client logboek, evenals de traceerlogboeken server en het netwerk. Standaard deze wordt gesorteerd op de **MessageNumber** kolom.
4. Zoek vervolgens het logboek van de client voor de client-request-ID. Selecteer op de werkbalk van het lint **berichten vinden**, geeft u een aangepast filter op de clientaanvraag-ID in de **vinden** veld. Gebruik deze syntaxis voor het filter, uw eigen clientaanvraag-ID op te geven:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

Message Analyzer zoekt en selecteert de eerste logboekvermelding waarvan de zoekcriteria overeenkomt met de client-request-ID. In het logboek van de client, zijn er meerdere vermeldingen voor elke clientaanvraag-ID, dus mogelijk om ze te groeperen op de **ClientRequestId** veld zodat u gemakkelijk om te zien ze alles bij elkaar. De volgende afbeelding ziet u alle berichten in het logboek van de client voor de opgegeven client aanvraag-ID.

![404-fouten van client logboek weergeven](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Met behulp van de gegevens die in de weergave-indelingen op deze twee tabbladen worden weergegeven, kunt u de gegevens van aanvragen om te bepalen wat de fout hebben veroorzaakt analyseren. U kunt ook zoeken op aanvragen die voorafgegaan van dit bestand om te zien als een eerdere gebeurtenis kan hebben geleid tot de 404-fout. U kunt bijvoorbeeld de logboekvermeldingen voor client voorafgaand aan deze aanvraag-ID van de client om te bepalen of de blob is mogelijk verwijderd of als de fout is veroorzaakt door de clienttoepassing een CreateIfNotExists-API aanroepen in een container of blob bekijken. In het logboek van de client, vindt u van de blob-adres in de **beschrijving** veld; in de server en de logboeken met traceringen, deze informatie wordt weergegeven in de **samenvatting** veld.

Zodra u weet het adres van de blob die i/o de 404-fout dat, kunt u verder onderzoek. Als u de logboekvermeldingen voor andere berichten die zijn gekoppeld aan bewerkingen op dezelfde blob zoekt, kunt u controleren of de entiteit die door de client eerder verwijderd.

## <a name="analyze-other-types-of-storage-errors"></a>Andere soorten Opslagfouten analyseren
Nu dat u bekend bent met Message Analyzer gebruiken om uw logboekgegevens te analyseren, kunt u analyseren andere typen fouten met de weergave-indelingen, kleurenregels en zoeken/filteren. In de tabellen hieronder vindt u enkele problemen die kunnen optreden en de filtercriteria kunt u ze kunt vinden. Zie voor meer informatie over het maken van filters en de Message Analyzer taal filteren [berichtgegevens filteren](https://technet.microsoft.com/library/jj819365.aspx).

| Om dit te onderzoeken... | Filterexpressie gebruiken... | Expressie is van toepassing op logboek (Client-, Server-, netwerk, alle) |
| --- | --- | --- |
| Onverwachte vertragingen bij de levering van berichten in een wachtrij |AzureStorageClientDotNetV4.Description bevat 'Opnieuw proberen is bewerking mislukt.' |Client |
| HTTP-toename in percentthrottlingerror aan |HTTP. Response.StatusCode == 500 &#124; &#124; HTTP. Response.StatusCode 503 == |Netwerk |
| Toename in percenttimeouterror aan |HTTP. Response.StatusCode 500 == |Netwerk |
| Toename in percenttimeouterror aan (alle) |* StatusCode 500 == |Alle |
| Toename in percentnetworkerror aan |AzureStorageClientDotNetV4.EventLogEntry.Level   < 2 |Client |
| HTTP-fout 403 (verboden) berichten |HTTP. Response.StatusCode 403 == |Netwerk |
| HTTP 404 (niet gevonden) berichten |HTTP. Response.StatusCode 404 == |Netwerk |
| 404 (alle) |* StatusCode 404 == |Alle |
| Shared Access Signature (SAS) autorisatie probleem |AzureStorageLog.RequestStatus == "SASAuthorizationError" |Netwerk |
| HTTP 409 (Conflict) berichten |HTTP. Response.StatusCode 409 == |Netwerk |
| 409 (alle) |*StatusCode   == 409 |Alle |
| Lage PercentSuccess of analytics logboekvermeldingen hebben bewerkingen met de status van ClientOtherErrors |AzureStorageLog.RequestStatus == "ClientOtherError" |Server |
| Nagle   Warning |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) en (AzureStorageLog.RequestPacketSize < 1460) en (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Server |
| Bereik van de tijd in de logboeken-Server en het netwerk |#Timestamp > = 2014-10-20T16:36:38 en #Timestamp < = 2014-10-20T16:36:39 |Netwerk-server |
| Bereik van de tijd in Server-logboeken |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 en AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Server |

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over het oplossen van problemen met end-to-end-scenario's in Azure Storage:

* [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md) (Bewaken, diagnosticeren en problemen oplossen in Microsoft Azure Storage)
* [Storage Analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Een opslagaccount in Azure portal controleren](storage-monitor-storage-account.md)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Microsoft Message Analyzer besturingssysteem-handleiding](https://technet.microsoft.com/library/jj649776.aspx)

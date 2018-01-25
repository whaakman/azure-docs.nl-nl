---
title: Het oplossen van Azure Storage met diagnostische gegevens & Message Analyzer | Microsoft Docs
description: Een zelfstudie voor end-to-end oplossen met Azure Storage Analytics, AzCopy en Microsoft Message Analyzer te demonstreren
services: storage
documentationcenter: dotnet
author: tamram
manager: timlt
ms.assetid: 643372a3-1c07-4e88-b4ef-042512a43086
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: tamram
ms.openlocfilehash: ee0e4671c31e97816576735b7bd2ee2f1629323e
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>End-to-end problemen oplossen met behulp van Azure Storage metrische gegevens en logboekregistratie, AzCopy en Message Analyzer
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Is een belangrijke kwalificatie voor bouwen en de client-toepassingen met Microsoft Azure Storage ondersteunende opsporen en oplossen. Als gevolg van de gedistribueerde aard van een Azure-toepassing, opsporen en oplossen van fouten en prestatieproblemen mogelijk complexer dan in traditionele omgevingen.

In deze zelfstudie ziet u hoe u bepaalde fouten die mogelijk van invloed op prestaties en los deze fouten op end-to-end identificeren met behulp van hulpprogramma's van Microsoft en Azure Storage, om te optimaliseren van de clienttoepassing.

Deze zelfstudie biedt een praktische exploratie van een end-to-end voor probleemoplossing scenario. Zie voor een gedetailleerde conceptuele handleiding voor probleemoplossing van Azure storage-toepassingen, [Monitor, vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Hulpprogramma's voor probleemoplossing van Azure Storage-toepassingen
Voor het oplossen van clienttoepassingen op Microsoft Azure Storage, kunt u een combinatie van hulpprogramma's om te bepalen wanneer er een probleem is opgetreden en wat de oorzaak van het probleem mogelijk. Tot deze hulpmiddelen behoren onder meer:

* **Azure Storage Analytics**. [Azure Storage Analytics](/rest/api/storageservices/Storage-Analytics) metrische gegevens en logboekregistratie voor Azure Storage biedt.
  
  * **Metrische gegevens Storage** houdt transactie metrische gegevens en capaciteitsmetrieken voor uw opslagaccount. Met metrische gegevens, kunt u bepalen hoe uw toepassing wordt uitgevoerd volgens een verscheidenheid aan andere maatregelen. Zie [Storage Analytics metrische gegevens tabelschema](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) voor meer informatie over de typen van metrische gegevens die worden bijgehouden door Storage Analytics.
  * **Opslag logboekregistratie** elke aanvraag naar de Azure Storage-services in een logboek serverzijde wordt geregistreerd. Het logboek houdt gedetailleerde gegevens voor elke aanvraag, waaronder de bewerking die wordt uitgevoerd, de status van de bewerking, en de latentie-informatie. Zie [Storage Analytics logboekindeling](/rest/api/storageservices/Storage-Analytics-Log-Format) voor meer informatie over de aanvraag- en gegevens die worden geschreven naar de logboeken door Storage Analytics.

> [!NOTE]
> Storage-accounts met een replicatietype van de Zone-redundante opslag (ZRS) ondersteuning voor metrische gegevens en logboekregistratie. ZRS klassieke accounts bieden geen ondersteuning voor metrische gegevens of de registratie. Zie voor meer informatie over ZRS [Zone-redundante opslag](storage-redundancy.md#zone-redundant-storage). 

* **Azure-portal**. U kunt metrische gegevens en logboekregistratie configureren voor uw opslagaccount in de [Azure-portal](https://portal.azure.com). U kunt ook grafieken en diagrammen die laten zien hoe uw toepassing wordt uitgevoerd na verloop van tijd weergeven en configureren van waarschuwingen om u te waarschuwen als uw toepassing wordt uitgevoerd anders dan verwacht voor een opgegeven waarde.
  
    Zie [een opslagaccount in de Azure-portal bewaken](storage-monitor-storage-account.md) voor informatie over het configureren van de bewaking in de Azure portal.
* **AzCopy**. Server-logboeken voor Azure Storage worden opgeslagen als blobs, zodat u AzCopy gebruiken kunt de blobs logboek kopiëren naar een lokale map voor analyse met behulp van Microsoft Message Analyzer. Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) voor meer informatie over AzCopy.
* **Microsoft Message Analyzer**. Berichtanalyse is een hulpprogramma dat logboekbestanden verbruikt en bevat de logboekgegevens visual die het eenvoudiger maakt om te filteren, zoeken en groep-logboekgegevens in nuttig sets die u gebruiken kunt om fouten en prestatieproblemen te analyseren. Zie [Microsoft Message Analyzer besturingssysteem Guide](http://technet.microsoft.com/library/jj649776.aspx) voor meer informatie over Message Analyzer.

## <a name="about-the-sample-scenario"></a>Over het voorbeeldscenario
Voor deze zelfstudie kijken we naar de een scenario waarbij Azure Storage metrische gegevens staat voor een laag percentage slagingspercentage voor een toepassing die Azure storage aanroept. De lage percentage geslaagd snelheid metriek (weergegeven als **PercentSuccess** in de [Azure-portal](https://portal.azure.com) en in de tabellen metrische gegevens) houdt bewerkingen die slagen, maar die een HTTP-statuscode die groter is dan 299 retourneren. In de logboekbestanden serverzijde opslag deze bewerkingen worden geregistreerd met een transactiestatus **ClientOtherErrors**. Zie voor meer informatie over de metriek lage percentage geslaagd [metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure Storage-bewerkingen kunnen retourneren HTTP-statuscodes groter is dan 299 als onderdeel van hun normale functionaliteit. Maar deze fouten in sommige gevallen geven aan dat u mogelijk uw clienttoepassing voor verbeterde prestaties te optimaliseren.

In dit scenario hebt we kunt u een laag percentage slagingspercentage moet iets minder dan 100%. U kunt echter een ander niveau van de metrische kiezen aan uw wensen. We raden aan dat tijdens het testen van uw toepassing, u een tolerantie basislijn voor uw belangrijkste prestatiemetrieken maakt. Bijvoorbeeld, u kunt bepalen, op basis van testen, dat uw toepassing een consistente percentage slagingspercentage van 90% of 85% moet hebben. Als uw metrische gegevens ziet u dat de toepassing is die afwijken van dit nummer en vervolgens kunt u onderzoeken wat de oorzaak van de verhoging.

In ons scenario voorbeeld wanneer we hebt ingesteld dat de frequentie waarmee percentage geslaagd metriek lager dan 100 is % bespreken we de logboeken om de fouten die overeenstemmen met de metrische gegevens en ze gebruiken om te achterhalen wat de oorzaak van het onderste percentage slagingspercentage vinden. Zullen we specifiek fouten in het bereik van 400. Vervolgens je we beter foutmeldingen 404 (niet gevonden).

### <a name="some-causes-of-400-range-errors"></a>Enkele oorzaken van fouten 400-bereik
De volgende voorbeelden ziet u een voorbeeld van een aantal fouten 400-bereik voor aanvragen op basis van Azure Blob Storage en hun mogelijke oorzaken. Een van deze fouten, evenals de fouten in het bereik 300 en het bereik 500, kan leiden tot een laag percentage slagingspercentage.

Houd er rekening mee dat de lijsten hieronder verre voltooid zijn. Zie [Status en foutcodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) op MSDN voor meer informatie over Azure Storage met algemene fouten en fouten die specifiek zijn voor elk van de storage-services.

**Voorbeelden van status Code 404 (niet gevonden)**

Deze gebeurtenis treedt op wanneer een leesbewerking op basis van een container of blob is mislukt omdat het blob of de container is niet gevonden.

* Deze gebeurtenis treedt op als een container of blob is verwijderd door een andere client voordat deze aanvraag.
* Deze gebeurtenis treedt op als u een API-aanroep die wordt gemaakt van de container of blob nadat u hebt gecontroleerd of deze bestaat. De CreateIfNotExists APIs gebeld HEAD eerst om te controleren op het bestaan van de container of blob; Als deze niet bestaat, een 404-fout wordt geretourneerd en vervolgens wordt een tweede PUT-aanroep uitgevoerd om de container of blob te schrijven.

**Status 409 (Conflict) codevoorbeelden**

* Deze gebeurtenis treedt op als u een API maken voor het maken van een nieuwe container of blob, zonder eerst aanwezigheid controleren, en een container of blob met die naam al bestaat.
* Deze gebeurtenis treedt op als een container wordt verwijderd en u probeert te maken van een nieuwe container met dezelfde naam voordat de verwijderingsbewerking voltooid is.
* Deze gebeurtenis treedt op als u een lease op een container of blob opgeven en er al een lease aanwezig is.

**Statuscode 412 (voorwaarde is mislukt) voorbeelden**

* Treedt op wanneer de voorwaarde die is opgegeven door een voorwaardelijke kop niet is voldaan.
* Treedt op wanneer de lease-ID die is opgegeven, komt niet overeen met de ID van de lease op de container of blob.

## <a name="generate-log-files-for-analysis"></a>Logboekbestanden voor analyse te genereren
In deze zelfstudie kun je Message Analyzer werkt met drie verschillende typen van logbestanden, hoewel u werkt met een van deze kan kiezen:

* De **serverlogboek**, die wordt gemaakt wanneer u Azure Storage-logboekregistratie inschakelt. Logboek van de server bevat gegevens over elke bewerking aangeroepen voor een van de services van Azure Storage - blob, queue, table en bestand. Logboek van de server geeft aan welke bewerking is aangeroepen en welke statuscode geretourneerd, evenals andere informatie over de aanvraag en -antwoord is.
* De **.NET clientlogboek**, die wordt gemaakt wanneer u logboekregistratie van client-side in uw .NET-toepassing inschakelen. De clientlogboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord.
* De **HTTP netwerk traceerlogboek**, verzamelt gegevens op HTTP/HTTPS-aanvraag en antwoord gegevens, inclusief voor bewerkingen op Azure Storage. In deze zelfstudie genereert de netwerktracering via Message Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Logboekregistratie op de server en metrische gegevens configureren
Eerst moet we voor het configureren van Azure Storage-logboekregistratie en metrische gegevens, zodat er gegevens van de clienttoepassing te analyseren. U kunt logboekregistratie en metrische gegevens in tal van manieren - configureren de [Azure-portal](https://portal.azure.com), met behulp van PowerShell, of via een programma. Zie [metrische gegevens Storage inschakelen en weergeven van metrische gegevens](http://msdn.microsoft.com/library/azure/dn782843.aspx) en [opslag vastleggen inschakelen en toegang tot logboekgegevens](http://msdn.microsoft.com/library/azure/dn782840.aspx) op MSDN voor meer informatie over het configureren van logboekregistratie en metrische gegevens.

**Via de Azure-portal**

Voor het configureren van logboekregistratie en metrische gegevens voor uw storage account met de [Azure-portal](https://portal.azure.com), volg de instructies voor [een opslagaccount in de Azure-portal bewaken](storage-monitor-storage-account.md).

> [!NOTE]
> Het is niet mogelijk in te stellen minuut metrische gegevens met behulp van de Azure-portal. We raden echter aan dat u ze stelt voor deze zelfstudie, en voor het onderzoeken van prestatieproblemen met uw toepassing. Minuut metrische gegevens met behulp van PowerShell, zoals hieronder wordt weergegeven of programmatisch met behulp van de storage-clientbibliotheek kunt u instellen.
> 
> Houd er rekening mee dat de minuut metrische gegevens en alleen per uur metrische gegevens kan niet worden weergegeven door de Azure portal.
> 
> 

**Via PowerShell**

Als u wilt beginnen met PowerShell voor Azure, Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

1. Gebruik de [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0) cmdlet uw Azure gebruikersaccount toevoegen aan het PowerShell-venster:
   
    ```powershell
    Add-AzureAccount
    ```

2. In de **aanmelden bij Microsoft Azure** venster, typ de e-mailadres en wachtwoord die zijn gekoppeld aan uw account. Azure verifieert de referentiegegevens en slaat deze op. Vervolgens wordt het venster gesloten.
3. Stel het standaardopslagaccount naar het opslagaccount dat u voor de zelfstudie gebruikt door het uitvoeren van deze opdrachten in het venster PowerShell:
   
    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Logboekregistratie van opslag voor de Blob-service inschakelen:
   
    ```powershell
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. Opslag metrische gegevens voor de Blob-service, om ervoor te zorgen instellen inschakelen **- MetricsType** naar `Minute`:
   
    ```powershell
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET-clientzijde logboekregistratie configureren
Schakel .NET diagnostische gegevens in het configuratiebestand van de toepassing (web.config of app.config) voor het configureren van client-side '-logboekregistratie voor een .NET-toepassing. Zie [clientzijde logboekregistratie met de Storage-clientbibliotheek voor .NET](http://msdn.microsoft.com/library/azure/dn782839.aspx) en [clientzijde logboekregistratie met de Microsoft Azure-opslag-SDK voor Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) op MSDN voor meer informatie.

De client-side '-logboek bevat gedetailleerde informatie over hoe de client de aanvraag wordt voorbereid en ontvangt en verwerkt het antwoord.

De Storage-clientbibliotheek worden client-side '-logboekgegevens opgeslagen in de opgegeven locatie in het configuratiebestand van de toepassing (web.config of app.config).

### <a name="collect-a-network-trace"></a>Een nieuwe netwerktracering verzamelen
U kunt Message Analyzer gebruiken voor het verzamelen van een HTTP/HTTPS-netwerktracering terwijl de clienttoepassing wordt uitgevoerd. Message Analyzer gebruikt [Fiddler](http://www.telerik.com/fiddler) op de back-end. Voordat u de netwerktracering hebt verzameld, wordt u aangeraden Fiddler om vast te leggen van niet-versleutelde HTTPS-verkeer te configureren:

1. Installeer [Fiddler](http://www.telerik.com/download/fiddler).
2. Start Fiddler.
3. Selecteer **extra | Opties voor fiddler**.
4. Zorg ervoor dat in het dialoogvenster Opties voor **HTTPS-Maak verbinding vastleggen** en **HTTPS-verkeer ontsleutelen** beide zijn geselecteerd, zoals hieronder wordt weergegeven.

![Fiddler opties configureren](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Voor de zelfstudie verzamelen en een nieuwe netwerktracering eerst opslaan in Message Analyzer en maken van een sessie analyse voor het analyseren van de tracering en de logboeken. Naar een netwerktracering in Message Analyzer verzamelen:

1. Selecteer in Message Analyzer **bestand | Snelle Trace | Niet-versleuteld HTTPS**.
2. De tracering begint onmiddellijk. Selecteer **stoppen** stoppen van de tracering zodat we deze trace-opslag alleen verkeer kunt configureren.
3. Selecteer **bewerken** de traceringssessie bewerken.
4. Selecteer de **configureren** koppeling rechts van de **Microsoft-Pef-WebProxy** ETW-provider.
5. In de **geavanceerde instellingen** dialoogvenster, klikt u op de **Provider** tabblad.
6. In de **Hostname Filter** veld, geeft u de eindpunten van uw opslag, gescheiden door spaties. Bijvoorbeeld, kunt u uw eindpunten als volgt; Wijzig `storagesample` op de naam van uw opslagaccount:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Het dialoogvenster sluiten en klik op **opnieuw** om te beginnen met het verzamelen van de tracering met het filter hostname aanwezig is, zodat alleen Azure Storage-netwerkverkeer wordt opgenomen in de tracering.

> [!NOTE]
> Nadat u klaar bent met het verzamelen van uw netwerk-trace, wordt aangeraden dat u de instellingen die u hebt gewijzigd in Fiddler om te ontsleutelen HTTPS-verkeer te herstellen. Schakel in het dialoogvenster Fiddler opties uit de **vastleggen HTTPS Maak verbinding** en **HTTPS-verkeer ontsleutelen** selectievakjes uit.
> 
> 

Zie [met behulp van de functies voor het traceren van netwerk](http://technet.microsoft.com/library/jj674819.aspx) op Technet voor meer informatie.

## <a name="review-metrics-data-in-the-azure-portal"></a>Controleer de metrische gegevens in de Azure portal
Zodra uw toepassing actief is geweest gedurende een periode, kunt u lezen de grafieken metrische gegevens die worden weergegeven in de [Azure-portal](https://portal.azure.com) om te zien hoe uw service heeft is uitvoert.

Eerst, gaat u naar uw opslagaccount in de Azure-portal. Standaard een controle grafiek met de **Verwerkingsfrequentie** metriek wordt weergegeven op de blade-account. Als u de grafiek om andere metrische gegevens weer te geven eerder hebt gewijzigd, voegt u de **Verwerkingsfrequentie** metriek.

U ziet nu **Verwerkingsfrequentie** in de bewakingsgrafiek samen met andere metrische gegevens u hebt toegevoegd. In het scenario dat we naast onderzoeken door analyseren in de logboeken in Message Analyzer, is het percentage slagingspercentage enigszins minder dan 100%.

Zie voor meer informatie over het toevoegen en aanpassen van metrische gegevens grafieken [grafieken van de metrische gegevens aanpassen](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Het kan even duren voor de metrische gegevens worden weergegeven in de Azure portal, nadat u opslag metrische gegevens inschakelen. Dit komt omdat elk uur metrische gegevens voor het vorige uur niet in de Azure-portal weergegeven worden totdat het huidige uur is verstreken. Bovendien worden minuut metrische gegevens momenteel niet weergegeven in de Azure portal. Dus afhankelijk van wanneer het inschakelen van metrische gegevens, duurt maximaal twee uur om metrische gegevens te bekijken.
> 
> 

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>AzCopy gebruiken om te kopiëren van serverlogboeken naar een lokale map
Azure Storage schrijft logboekgegevens van de server voor blobs, tijdens metrische gegevens worden geschreven naar tabellen. Logboek blobs zijn beschikbaar in de bekende `$logs` container voor uw opslagaccount. Logboek blobs worden hiërarchisch benoemd met jaar, maand, dag en uur, zodat u het bereik van de tijd die u wilt onderzoeken gemakkelijk kan vinden. Bijvoorbeeld, in de `storagesample` account, de container voor de blobs logboek voor 01-02-2015 8 en 9 uur, `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. De afzonderlijke blobs in deze container zijn benoemde opeenvolgend, beginnend met `000000.log`.

U kunt het opdrachtregelhulpprogramma AzCopy deze logboekbestanden serverzijde downloaden naar een locatie van uw keuze op uw lokale machine. U kunt bijvoorbeeld de volgende opdracht gebruiken voor het downloaden van de logboekbestanden voor blob-bewerkingen die plaatsgevonden op 2 januari 2015 naar de map heeft `C:\Temp\Logs\Server`; vervangen `<storageaccountname>` met de naam van uw opslagaccount en `<storageaccountkey>` door uw toegangssleutel voor account:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy is beschikbaar voor downloaden op de [Azure downloadt](https://azure.microsoft.com/downloads/) pagina. Zie voor meer informatie over het gebruik van AzCopy [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

Zie voor meer informatie over het downloaden van serverzijde logboeken [logboekgegevens downloaden opslag logboekregistratie](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Gebruik Microsoft Message Analyzer logboekgegevens analyseren
Microsoft Message Analyzer is een hulpprogramma voor het vastleggen, weergeven en analyseren van verkeer, gebeurtenissen en andere systemen of toepassingen berichten in scenario's voor probleemoplossing en diagnostische messaging protocol. Message Analyzer ook kunt u laden, aggregeren en analyseren van gegevens van logboekbestanden en traceerbestanden opgeslagen. Zie voor meer informatie over Message Analyzer [Microsoft Message Analyzer besturingssysteem Guide](http://technet.microsoft.com/library/jj649776.aspx).

Berichtanalyse omvat activa voor Azure Storage die u helpen bij het analyseren van de server, client en netwerk-Logboeken. In deze sectie bespreken we het gebruik van deze hulpprogramma's voor het oplossen van het probleem van laag percentage slagen in de logboeken van de opslag.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Download en installeer Message Analyzer en de Azure Storage-bedrijfsmiddelen
1. Download [Message Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) van het Microsoft Download Center en voert u het installatieprogramma.
2. Start de Message Analyzer.
3. Van de **extra** selecteert u **Asset Manager**. In de **Asset Manager** dialoogvenster Selecteer **downloadt**, vervolgens filteren op **Azure Storage**. U ziet de activa van de Azure-opslag, zoals wordt weergegeven in de volgende afbeelding.
4. Klik op **Sync alle weergegeven objecten** voor het installeren van de activa van Azure Storage. De beschikbare elementen zijn onder andere:
   * **Azure Storage kleur regels:** Azure Storage kleur regels kunt u speciale filters definiëren waarmee u berichten die specifieke informatie in een tracering bevatten te markeren met behulp van kleur, tekst en lettertype stijlen.
   * **Azure Storage-grafieken:** Azure Storage kolomdiagrammen zijn vooraf gedefinieerde diagrammen die in een grafiek weergegeven logboekgegevens van de server. Houd er rekening mee dat voor het gebruik van Azure Storage-grafieken op dit moment moet u alleen logboek van de server in het raster Analysis geladen mogelijk.
   * **Azure Storage Parsers:** de Azure Storage-parsers de Azure Storage client, de server en de HTTP-logboeken parseren om weer te geven in het raster analyse.
   * **Azure Storage-Filters:** Azure Storage-filters zijn vooraf gedefinieerde criteria die u gebruiken kunt voor het opvragen van uw gegevens in het raster analyse.
   * **Azure Storage weergave-indelingen:** Azure Storage weergave-indelingen zijn vooraf gedefinieerde kolomindelingen en groeperingen in het raster analyse.
5. Opnieuw opstarten Message Analyzer nadat u de assets hebt geïnstalleerd.

![Message Analyzer Asset Manager](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Installeer alle van de Azure Storage-assets weergegeven voor de doeleinden van deze zelfstudie.
> 
> 

### <a name="import-your-log-files-into-message-analyzer"></a>Importeren van de logboekbestanden in Message Analyzer
U kunt alle opgeslagen logboekbestanden (serverzijde, client-side en netwerk) importeren in één sessie in Microsoft Message Analyzer voor analyse.

1. Op de **bestand** menu van Microsoft Message Analyzer, klikt u op **nieuwe sessie**, en klik vervolgens op **leeg sessie**. In de **nieuwe sessie** dialoogvenster een naam voor uw analyse-sessie. In de **Sessiedetails** -scherm, klikt u op de **bestanden** knop.
2. Klik op het netwerk trace om gegevens te laden die worden gegenereerd door Message Analyzer, **bestanden toevoegen**, blader naar de locatie waar u het bestand .matp van uw web-traceringssessie hebt opgeslagen, selecteert u het bestand .matp en klikt u op **Open**.
3. Klik op de server-side ' om logboekgegevens te laden, **bestanden toevoegen**, blader naar de locatie waar u uw logboeken serverzijde hebt gedownload, selecteert u de logboekbestanden voor de periode die u wilt analyseren, en klikt u op **Open**. Vervolgens in de **Sessiedetails** deelvenster, stelt u de **tekst logboekconfiguratie** vervolgkeuzelijst voor elke serverzijde logboekbestand **AzureStorageLog** om ervoor te zorgen dat Microsoft Message Analyzer correct het logboekbestand kan parseren.
4. Klik op de client-side ' om logboekgegevens te laden, **bestanden toevoegen**, blader naar de locatie waar u uw client-side '-Logboeken hebt opgeslagen, selecteert u de logboekbestanden die u wilt analyseren, en klikt u op **Open**. Vervolgens in de **Sessiedetails** deelvenster, stelt u de **tekst logboekconfiguratie** vervolgkeuzelijst voor elke client-side-logboekbestand te **AzureStorageClientDotNetV4** om ervoor te zorgen dat Microsoft Message Analyzer correct het logboekbestand kan parseren.
5. Klik op **Start** in de **nieuwe sessie** dialoogvenster laden en parseren van de logboekgegevens. De logboekgegevens worden weergegeven in het raster Message Analyzer analyse.

De volgende afbeelding toont een voorbeeld-sessie die is geconfigureerd met de server, client en netwerk-logboekbestanden voor tracering.

![Message Analyzer sessie configureren](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Houd er rekening mee dat Message Analyzer logboekbestanden in het geheugen laadt. Als er een groot aantal logboekgegevens, wilt u het filter om de beste prestaties van Message Analyzer.

Bepaal eerst het tijdsbestek dat u geïnteresseerd bent in het controleren van, en deze periode zo klein mogelijk te houden. In veel gevallen wordt u wilt een periode van minuten of uren maximaal bekijken. Importeer de kleinste set logboeken die uw behoeften.

Als u nog steeds een grote hoeveelheid gegevens aan het logboek hebt, vervolgens mogelijk wilt u een sessie om te filteren dat uw logboekgegevens voordat u het laden. In de **isStrictMode** de optie de **bibliotheek** klikken om een vooraf gedefinieerde filter kiezen; bijvoorbeeld kiezen **globale tijd Filter I** filters uit de Azure-opslag kunt u filteren op een tijdsinterval. Vervolgens kunt u de filtercriteria om op te geven van het eerste en laatste tijdstempel voor het interval dat u wilt zien. U kunt ook filteren op een bepaalde statuscode; u kunt bijvoorbeeld alleen logboekvermeldingen waar de statuscode 404 wordt geladen.

Zie voor meer informatie over het importeren van gegevens aan het logboek in Microsoft Message Analyzer [berichtgegevens bij het ophalen van](http://technet.microsoft.com/library/dn772437.aspx) op TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>De aanvraag-ID van client correleren Logbestandsgegevens gebruiken
De Azure Storage-clientbibliotheek genereert automatisch een unieke client aanvraag-ID voor elke aanvraag. Deze waarde is geschreven naar het clientlogboek, logboek van de server en de netwerktracering, zodat u deze gebruiken kunt voor het correleren van gegevens over alle drie logboeken binnen Message Analyzer. Zie [aanvraag-ID van Client](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) aanvragen voor aanvullende informatie over de client ID.

De volgende secties wordt beschreven hoe u voor het gebruik van vooraf geconfigureerde en aangepaste indeling weergaven met elkaar correleren en gegevens groeperen op basis van de client-request-ID.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Selecteer een weergave-indeling moet worden weergegeven in het raster analyse
De opslag-elementen voor Message Analyzer bevatten Azure Storage-weergave indelingen, die zijn vooraf geconfigureerde weergaven die u gebruiken kunt om uw gegevens met nuttig groeperingen en kolommen voor verschillende scenario's weer te geven. U kunt ook aangepaste weergave-indelingen maken en opslaan voor hergebruik.

De afbeelding hieronder bevat de **lay-out** menu beschikbaar door het selecteren van **lay-out** vanuit het lint werkbalk. De weergave-indelingen voor Azure Storage zijn gegroepeerd onder de **Azure Storage** knooppunt in het menu. U kunt zoeken naar `Azure Storage` in het zoekvak te filteren op Azure Storage indelingen alleen bekijken. U kunt ook de ster naast een te maken het een favoriet en weer te geven aan de bovenkant van het menu Weergave-indeling selecteren.

![Weergavemenu-indeling](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Selecteer allereerst **gegroepeerd op ClientRequestID en Module**. Deze groepen weergeven lay-out zich gegevens uit alle drie logboeken eerst op client-request-ID, vervolgens op de bron-logboekbestand (of **Module** in Berichtanalyse). Aan deze weergave kunt u inzoomen op een bepaalde client-aanvraag-ID en er gegevens van alle drie logboekbestanden voor die clientaanvraag-id.

De afbeelding hieronder bevat de weergave van deze indeling toegepast op de voorbeeldgegevens logboek wordt weergegeven, met een subset met kolommen weergegeven. U kunt zien dat voor een bepaalde client-aanvraag-ID, het raster Analysis gegevens worden weergegeven in de clientlogboek, serverlogboek en netwerktracering.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Verschillende logboekbestanden hebben verschillende kolommen, zodat wanneer gegevens uit meerdere logboekbestanden wordt weergegeven in het raster analyse, sommige kolommen mag niet alle gegevens voor een bepaalde rij bevatten. Bijvoorbeeld, in de bovenstaande afbeelding client logboek rijen niet weergeven alle gegevens voor de **tijdstempel**, **TimeElapsed**, **bron**, en **bestemming** kolommen, omdat deze kolommen niet bestaan in het clientlogboek, maar bestaan in de netwerk-trace. Op dezelfde manier de **tijdstempel** kolom tijdstempelgegevens van het logboek van de server wordt weergegeven, maar geen gegevens weergegeven voor de **TimeElapsed**, **bron**, en **bestemming** kolommen die geen deel uitmaken van het logboek van de server.
> 
> 

Naast het gebruik van de weergave-indelingen van Azure Storage, kunt u ook definiëren en slaat uw eigen weergave-indelingen. U kunt andere gewenste velden voor het groeperen van gegevens selecteren en de groepering op te slaan als onderdeel van uw aangepaste indeling ook.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Kleur regels toepassen op het raster analyse
De opslag activa ook kleur, deze regels een visuele betekent dat bieden voor het identificeren van verschillende typen fouten in het raster analyse. De kleur van de vooraf gedefinieerde regels van toepassing op HTTP-fouten, zodat ze alleen voor de tracering van logboek- en server worden weergegeven.

Als kleur regels toepassen, selecteert u **kleur regels** vanuit het lint werkbalk. Hier ziet u de Azure Storage kleur regels in het menu. Selecteer voor de zelfstudie **Client fouten (StatusCode tussen 400 en 499)**, zoals wordt weergegeven in de volgende afbeelding.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Naast het gebruik van de regels van de kleur van Azure Storage, kunt u ook definiëren en uw eigen regels kleur opslaan.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Groeperen en filteren Logboekgegevens 400 bereik fouten vinden
Vervolgens we groeperen en filteren van de logboekgegevens om alle fouten in het bereik van 400.

1. Zoek de **StatusCode** kolom in het raster analyse met de rechtermuisknop op de kolom Titel en selecteer **groep**.
2. Vervolgens groeperen op de **ClientRequestId** kolom. U ziet dat de gegevens in het raster Analysis nu zijn georganiseerd door statuscode en client aanvraag-ID.
3. Het venster Filter hulpprogramma weergegeven als deze nog niet wordt weergegeven. Selecteer op het lint werkbalk **hulpprogramma Windows**, klikt u vervolgens **Filter voor**.
4. Het om logboekgegevens te filteren om alleen 400 bereik fouten weer te geven, toevoegen de volgende filtercriteria voldoen aan de **Filter voor** venster en klik op **toepassen**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

De volgende afbeelding ziet de resultaten van deze groeperen en filteren. Uitbreiden van de **ClientRequestID** veld onder de groepering voor statuscode 409, ziet u bijvoorbeeld een bewerking dat heeft geresulteerd in deze statuscode.

![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Nadat dit filter is toegepast, ziet u dat de rijen van het clientlogboek zijn uitgesloten, als de client logboek bevat geen een **StatusCode** kolom. Allereerst getoetst de server en het netwerk traceerlogboeken vinden 404-fouten, en vervolgens we keert terug naar het clientlogboek de bewerkingen van de client die hebben geleid tot ze onderzoeken.

> [!NOTE]
> U kunt filteren op de **StatusCode** kolom en nog steeds gegevens weergeven van alle drie logboeken, met inbegrip van het clientlogboek voor de, als u een expressie toevoegt aan het filter dat bevat logboekvermeldingen waar de statuscode null is. Kan deze filterexpressie, gebruiken:
> 
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
> 
> Dit filter retourneert alle rijen van de client logboek en alleen rijen uit het logbestand voor de server en de HTTP-logboek waar de statuscode is groter dan 400. Als u deze op de lay-out gegroepeerd op aanvraag-ID van client en de module toepast, kunt u zoeken of blader door de logboekvermeldingen toepassingsgroepen vinden waar alle drie logboeken worden weergegeven.   
> 
> 

### <a name="filter-log-data-to-find-404-errors"></a>De logboekgegevens filter vinden 404-fouten
De opslag activa bevatten vooraf gedefinieerde filters die u gebruiken kunt voor het beperken van de logboekgegevens als u de fouten of trends die u zoekt. Vervolgens twee vooraf gedefinieerde filters moet worden toegepast: één waarmee de server en het netwerk traceerlogboeken voor 404-fouten worden gefilterd en dat de gegevens op een opgegeven tijdperiode filtert.

1. Het venster Filter hulpprogramma weergegeven als deze nog niet wordt weergegeven. Selecteer op het lint werkbalk **hulpprogramma Windows**, klikt u vervolgens **Filter voor**.
2. Selecteer in het venster Filter **bibliotheek**, en zoek op `Azure Storage` vinden van Azure Storage-filters. Selecteer het filter voor **404 (niet gevonden) berichten in alle logboeken**.
3. Weergave de **bibliotheek** menu opnieuw, en zoek en selecteer de **globale tijdfilter**.
4. Bewerk de tijdstempels weergegeven in het filter aan het bereik dat u wilt weergeven. Dit draagt bij tot het beperken van het bereik van gegevens te analyseren.
5. Het filter moet worden weergegeven als in het voorbeeld hieronder. Klik op **toepassen** het filter wilt toepassen op het raster analyse.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Lay-out van Azure Storage weergeven](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>De logboekgegevens analyseren
Nu dat u hebt gegroepeerd en uw gegevens gefilterd, bekijk de details van afzonderlijke aanvragen die 404-fouten gegenereerd. De gegevens worden in de huidige weergave-indeling gegroepeerd per client aanvraag-ID, klikt u vervolgens door de bron-logboek. Aangezien we op aanvragen waarin het veld StatusCode 404 bevat filtert, ziet er alleen de server en traceringsgegevens voor netwerk, niet de logboekgegevens van de client.

De volgende afbeelding ziet een specifieke aanvraag waarin een 404 door een ophalen van Blob-bewerking heeft opgeleverd omdat de blob niet bestaat. Houd er rekening mee dat sommige kolommen zijn verwijderd uit de standaardweergave om de relevante gegevens weer te geven.

![Gefilterde Server en de logboeken met traceringen](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

We je deze client-request-ID vervolgens correleren met de logboekgegevens van de client om te zien welke acties de client de failoverbewerking duurt als de fout is opgetreden. U kunt een nieuwe Analysis rasterweergave voor deze sessie om weer te geven van de client-logboekgegevens, die wordt geopend in een tweede tabblad weergeven:

1. Eerst, kopieert u de waarde van de **ClientRequestId** veld naar het Klembord. U kunt dit doen door een rij selecteren, zoeken naar de **ClientRequestId** veld met de rechtermuisknop op de gegevenswaarde en het kiezen van **kopie 'ClientRequestId'**.
2. Selecteer op het lint werkbalk **nieuwe Viewer**, selecteer daarna **Analysis raster** naar een nieuw tabblad openen. Het nieuwe tabblad bevat alle gegevens in de logboekbestanden, zonder groeperen, filteren of kleur regels.
3. Selecteer op het lint werkbalk **lay-out**, selecteer daarna **kolommen van alle .NET-Client** onder de **Azure Storage** sectie. Deze weergave-indeling bevat gegevens van de client logboek, evenals de traceringslogboeken voor de server en het netwerk. Standaard wordt gesorteerd op de **MessageNumber** kolom.
4. Zoek vervolgens het clientlogboek voor de client-request-ID. Selecteer op het lint werkbalk **berichten vinden**, geeft u een aangepast filter op de client-request-ID in de **vinden** veld. Gebruik deze syntaxis voor het filter, geven de aanvraag-ID van uw eigen client:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

Berichtanalyse zoekt en selecteert de eerste logboekvermelding waarbij de zoekcriteria overeenkomt met de client-request-ID. In het clientlogboek er zijn meerdere vermeldingen voor elke client-request-ID, zodat u ze te groeperen kunt op de **ClientRequestId** veld om te zien alle samen te vereenvoudigen. De volgende afbeelding ziet u alle berichten in het clientlogboek voor de opgegeven client aanvraag-ID.

![Client logboek weergeeft 404-fouten](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

De gegevens weergegeven in de weergave-indelingen op deze twee tabbladen kunt u de gegevens van aanvragen om te bepalen wat de fout hebben veroorzaakt analyseren. U kunt ook zoeken op aanvragen die voorafgegaan van dit programma om te zien als een eerdere gebeurtenis mogelijk hebben geleid tot de 404-fout. U kunt bijvoorbeeld de client logboekvermeldingen voorafgaand aan deze aanvraag-ID van de client om te bepalen of de blob is mogelijk verwijderd of dat de fout is veroorzaakt door de clienttoepassing een CreateIfNotExists-API aanroepen voor een container of blob bekijken. In het clientlogboek vindt u de blob-adres in de **beschrijving** veld; in de server en de logboeken met traceringen, wordt deze informatie verschijnt in de **samenvatting** veld.

Zodra u het adres van de blob die 404-fout heeft opgeleverd weet, kunt u verder onderzoek. Als u de logboekvermeldingen voor andere berichten die zijn gekoppeld aan de bewerkingen op de dezelfde blob zoekt, kunt u controleren of de entiteit die door de client eerder verwijderd.

## <a name="analyze-other-types-of-storage-errors"></a>Andere soorten opslag fouten analyseren
Nu dat u bekend bent met Message Analyzer gebruiken om uw logboekgegevens te analyseren, kunt u andere soorten fouten bij het gebruik van weergave analyseren indelingen, kleur regels en zoeken/filteren. De onderstaande tabellen vindt u enkele problemen kunnen optreden en de filtercriteria kunt u ze kunt vinden. Zie voor meer informatie over het samenstellen van filters en het filteren van taal Berichtanalyse [berichtgegevens filteren](http://technet.microsoft.com/library/jj819365.aspx).

| Voor het onderzoeken van... | Gebruik de filterexpressie... | Expressie is van toepassing op logboek (Client-, Server-, netwerk, alle) |
| --- | --- | --- |
| Onverwachte vertragingen bij de levering van berichten in een wachtrij |AzureStorageClientDotNetV4.Description bevat 'Opnieuw proberen is bewerking mislukt.' |Client |
| HTTP-toename van PercentThrottlingError |HTTP.Response.StatusCode   == 500 &#124;&#124; HTTP.Response.StatusCode == 503 |Netwerk |
| Toename van het PercentTimeoutError |HTTP.Response.StatusCode   == 500 |Netwerk |
| Toename van het PercentTimeoutError (alle) |* StatusCode 500 == |Alle |
| Toename van het PercentNetworkError |AzureStorageClientDotNetV4.EventLogEntry.Level   < 2 |Client |
| HTTP-fout 403 (verboden) berichten |HTTP.Response.StatusCode   == 403 |Netwerk |
| HTTP 404 (niet gevonden) berichten |HTTP.Response.StatusCode   == 404 |Netwerk |
| 404 (alle) |* StatusCode 404 == |Alle |
| Shared Access Signature (SAS) autorisatie probleem |AzureStorageLog.RequestStatus == "SASAuthorizationError" |Netwerk |
| HTTP 409 (Conflict) berichten |HTTP.Response.StatusCode   == 409 |Netwerk |
| 409 (alle) |* StatusCode == 409 |Alle |
| Lage PercentSuccess of analytics logboekvermeldingen hebben bewerkingen met de status van ClientOtherErrors |AzureStorageLog.RequestStatus == "ClientOtherError" |Server |
| Nagle waarschuwing |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) en (AzureStorageLog.RequestPacketSize < 1460) en (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Server |
| Tijdsbereik in Logboeken Server en het netwerk |#Timestamp > = 2014-10-20T16:36:38 en #Timestamp < = 2014-10-20T16:36:39 |Netwerk-server |
| Tijdsbereik in Server-logboeken |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 en AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Server |

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over het oplossen van problemen end-to-end-scenario's in Azure Storage:

* [Controleren, vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)
* [Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Een opslagaccount in de Azure portal controleren](storage-monitor-storage-account.md)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Microsoft Message Analyzer besturingssysteem handleiding](http://technet.microsoft.com/library/jj649776.aspx)

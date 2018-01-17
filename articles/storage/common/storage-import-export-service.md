---
title: Gebruik Azure Import/Export voor het overbrengen van gegevens van en naar Azure Storage | Microsoft Docs
description: Informatie over het maken van importeren en exporteren van taken in de Azure-portal voor het overbrengen van gegevens van en naar Azure Storage.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: muralikk
ms.openlocfilehash: 37860425460496c5fc2451713d1d3ec58ac9106d
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-azure-storage"></a>De Microsoft Azure Import/Export-service gebruiken om gegevens overdragen naar Azure Storage
In dit artikel bieden we Stapsgewijze instructies over het gebruik van Azure Import/Export-service veilig grote hoeveelheden gegevens overdragen naar Azure Blob storage en Azure-bestanden door de back-upfunctie schijven naar een Azure-Datacenter. Deze service kan ook worden gebruikt gegevens overdragen naar Azure storage naar harde schijven en verzenden naar uw on-premises-sites. Gegevens uit een enkele interne SATA harde schijf kunnen worden geïmporteerd naar Azure Blob storage of Azure-bestanden. 

> [!IMPORTANT] 
> Deze dienst accepteert alleen interne SATA harde schijven en SSD's alleen. Er is geen ander apparaat wordt ondersteund. Stuur geen externe HDD's NAS-apparaten, enz., zoals ze worden geretourneerd, indien mogelijk, of anderszins verwijderde.
>
>

Volg de onderstaande stappen te volgen als de gegevens op de schijf moet worden geïmporteerd in Azure Storage.
### <a name="step-1-prepare-the-drives-using-waimportexport-tool-and-generate-journal-files"></a>Stap 1: Voorbereiden van de schijf (s) met hulpprogramma WAImportExport en wijzigingslogboek bestand/s genereren.

1.  Identificeer de gegevens moeten worden geïmporteerd in Azure Storage. Dit kan mappen en bestanden op een lokale server of een netwerkshare zelfstandig zijn.
2.  Afhankelijk van de totale grootte van de gegevens, schaft u het vereiste aantal 2,5 inch SSD of 2,5-inch of 3.5" SATA II of III harde schijven.
3.  Koppelen van de harde schijven die rechtstreeks met SATA of met externe USB-adapters aan een windows-machine.
4.  Maak één NTFS-volume op elke harde schijf en een stationsletter toewijzen aan het volume. Er is geen quorumbron:.
5.  Schakel bits BitLocker-versleuteling op het NTFS-volume voor het inschakelen van versleuteling op de windows-machine. Volg de instructies op https://technet.microsoft.com/en-us/library/cc731549(v=ws.10).aspx.
6.  Volledig gegevens kopiëren naar de versleutelde één NTFS-volumes op schijven met kopiëren & plakken of slepen en neerzetten of Robocopy of een dergelijk hulpmiddel.
7.  WAImportExport V1 van https://www.microsoft.com/en-us/download/details.aspx?id=42659 downloaden
8.  Uitpakken naar de map standaard waimportexportv1. Bijvoorbeeld: C:\WaImportExportV1  
9.  Als Administrator uitvoeren en open een PowerShell of de opdrachtregel en wijzig map in de uitgepakte map. Bijvoorbeeld cd C:\WaImportExportV1
10. Kopieer de volgende opdrachtregel naar een Kladblok en bewerken voor het maken van een opdrachtregel.
  ./WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:***== /t:D /bk:*** /srcdir:D:\ /dstdir:ContainerName/ /skipwrite
    
    /j: de naam van een bestand journaalbestand met extensie .jrn genoemd. Een journal-bestand wordt gegenereerd per schijf en daarom wordt het aanbevolen het serienummer van de schijf gebruiken als de naam van het journaal.
    /SK: sleutel voor azure Storage-Account. / t: Stationsletter van de schijf moeten worden verzonden. Bijvoorbeeld, D /bk: de sleutel is bits referentiekluis van het station /srcdir: stationsletter van de schijf moeten worden verzonden, gevolgd door: \. Bijvoorbeeld: D:\
    /dstdir: de naam van Azure Storage-Container waarnaar de gegevens zijn om te worden geïmporteerd.
    /skipwrite 
    
11. Herhaal stap 10 voor elk van de schijf die moet worden verzonden.
12. Een journal-bestand met de naam die is opgegeven met de parameter /j: wordt gemaakt voor elke run van de opdrachtregel.

### <a name="step-2-create-an-import-job-on-azure-portal"></a>Stap 2: Maak een Import-taak op Azure-Portal.

1. Meld u aan bij https://portal.azure.com/ en onder meer services -> opslag-"taken voor importeren/exporteren" > Klik op **maken voor importeren/exporteren taak**.

2. In de sectie over de grondbeginselen, selecteer 'Importeren in Azure' Voer een tekenreeks voor de taaknaam van de, selecteer een abonnement, typ of Selecteer een resourcegroep. Voer een beschrijvende naam voor de import-taak. Houd er rekening mee dat de naam die u invoert alleen kleine letters, cijfers, afbreekstreepjes mogen en onderstrepingstekens bevatten, moet beginnen met een letter en mag geen spaties bevatten. U de naam die u kiest voor het bijhouden van uw taken terwijl ze zijn uitgevoerd en wanneer ze zijn voltooid.

3. In de sectie details taak het station journaal bestanden uploaden die u hebt verkregen tijdens de stap station ter voorbereiding. Als waimportexport.exe version1 gedefinieerd is gebruikt, moet u een bestand voor elke schijf die u hebt voorbereid te uploaden. Selecteer het opslagaccount waarin de gegevens worden geïmporteerd in de sectie 'Importeren' Storage-account. De afgiftepunt wordt automatisch gevuld op basis van de regio van het geselecteerde opslagaccount.
   
   ![Stap 3 van importtaak - maken](./media/storage-import-export-service/import-job-03.png)
4. In de back-upfunctie sectie gegevens van de provider selecteert in de vervolgkeuzelijst en voer een geldige carrier account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven naar u verzenden zodra de import-taak is voltooid. Geef een volledig en geldig contactpersoon, telefoon, e, adres, stad, zip, staat/proviince en land/regio.
   
5. In de samenvatting wordt Azure DataCenter verzendadres moet worden gebruikt voor het verzenden van schijven naar Azure DC verstrekt. Zorg ervoor dat de taaknaam en het volledige adres worden vermeld op het label van de back-upfunctie. 

6. Klik op OK op de pagina Samenvatting om uit te voeren van Import-taak maken.

### <a name="step-3-ship-the-drives-to-the-azure-datacenter-shipping-address-provided-in-step-2"></a>Stap 3: Verzend de schijf (s) naar de Azure-Datacenter verzendadres vindt u in stap 2.
FedEx, UPS of DHL kan worden gebruikt voor het verzenden van het pakket naar Azure-domeincontroller.

### <a name="step-4-update-the-job-created-in-step2-with-tracking-number-of-the-shipment"></a>Stap 4: De taak die in stap 2 hebt gemaakt met het volgnummer van de verzending bijwerken.
Na de verzending van de schijven, terug naar de **voor importeren/exporteren** pagina in de Azure portal om een) Navigeer bij te werken het volgnummer met behulp van de volgende stappen uit en klik op de import b) Klik op taak **taakstatus en bijhouden van info bijwerken Zodra de stations worden verzonden**. c) Selecteer het selectievakje "Markeren als verzonden"-d) voorziet in het nummer van de provider en bijhouden.
Als het volgnummer niet binnen twee weken bijgewerkt is van de taak wordt gemaakt, verloopt de taak. De voortgang van de taak kan worden gevolgd in het portal-dashboard. Zie de taakstatus van elke in het vorige gedeelte: door [weer te geven de status van uw taak](#viewing-your-job-status).

## <a name="when-should-i-use-the-azure-importexport-service"></a>Wanneer moet ik de Azure Import/Export-service gebruiken?
Overweeg het gebruik van Azure Import/Export-service wanneer uploaden of downloaden van gegevens via het netwerk te langzaam is of meer netwerkbandbreedte ophalen kostbaar is.

U kunt deze service, zoals in scenario's gebruiken:

* Migreren van gegevens naar de cloud: grote hoeveelheden gegevens snel verplaatsen naar Azure en voordelige wijze.
* Inhoudsdistributie: snel gegevens verzenden naar uw klant-sites.
* Back-up: Back-ups van uw on-premises gegevens op te slaan in Azure Storage in beslag nemen.
* Herstel van gegevens: grote hoeveelheid gegevens die zijn opgeslagen in de opslagruimte te herstellen en bezorgd bij uw on-premises locatie.

## <a name="prerequisites"></a>Vereisten
In deze sectie wordt de vereiste onderdelen voor het gebruik van deze service weergeven. Lees deze zorgvuldig door voordat u verzendt uw schijven.

### <a name="storage-account"></a>Storage-account
Er moet een bestaande Azure-abonnement en een of meer opslagaccounts de Import/Export-service gebruiken. Elke taak kan worden gebruikt voor gegevensoverdracht naar of van slechts één opslagaccount. Een enkel voor importeren/exporteren-taak kan niet met andere woorden, meerdere opslagaccounts overbruggen. Zie voor meer informatie over het maken van een nieuw opslagaccount [het maken van een Opslagaccount](storage-create-storage-account.md#create-a-storage-account).

### <a name="data-types"></a>Gegevenstypen
U kunt Azure Import/Export-service gebruiken om gegevens te kopiëren **blok** blobs, **pagina** blobs, of **bestanden**. Als u daarentegen, kunt u alleen exporteren **blok** blobs, **pagina** blobs of **Append** blobs uit Azure-opslag met deze service. De service ondersteunt alleen importeren van de Azure-bestanden in de Azure-opslag. Exporteren van Azure Files is momenteel niet ondersteund.

### <a name="job"></a>Job
Als u wilt beginnen met het importeren of exporteren uit de opslag, moet u eerst een taak maken. Een taak kan een import-taak of een taak voor het exporteren worden uitgevoerd:

* Maak een import-taak wanneer u uw Azure storage-account moet lokale gegevens overdraagt.
* Een exporttaak maken wanneer u wilt overbrengen van gegevens die momenteel zijn opgeslagen in uw storage-account voor de harde schijven die worden verzonden naar Microsoft. Wanneer u een taak maakt, waarschuwen u dat de Import/Export-service dat u een of meer harde schijven voor een Azure-Datacenter wordt verzending.

* Voor een import-taak wordt u harde schijven met uw gegevens verzending.
* Voor een exporttaak wordt u lege harde schijven verzending.
* U kunt maximaal 10 harde schijven per taak kan worden verzonden.

U kunt maken van een import of exporteren van de taak met de Azure portal of de [REST-API van Azure Storage Import/Export](/rest/api/storageimportexport).

> [!Note]
> De RDFE-API's worden niet ondersteund en met 28 februari 2018 en hoger. Om door te gaan met de service migreren naar de [ARM voor importeren/exporteren REST-API's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/storageimportexport/resource-manager/Microsoft.ImportExport/stable/2016-11-01/storageimportexport.json). 

### <a name="waimportexport-tool"></a>WAImportExport hulpprogramma
De eerste stap bij het maken van een **importeren** taak is het voorbereiden van uw schijven die worden verzonden voor importeren. Als u met het voorbereiden van uw schijven, moet u deze verbinding te maken met een lokale server en het hulpprogramma WAImportExport uitvoeren op de lokale server. Dit hulpprogramma WAImportExport vereenvoudigt het kopiëren van uw gegevens naar het station, de gegevens op het station met BitLocker versleutelen en het genereren van het station journaal-bestanden.

De wijzigingslogboek-bestanden opslaan basisinformatie over de taak en het station zoals serienummer van station en de naam van het opslagaccount. Dit logboek-bestand wordt niet opgeslagen op de schijf. Het wordt gebruikt tijdens het maken van de import-taak. Verderop in dit artikel vindt u stapsgewijze informatie over het maken van de taak.

Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssysteem. Zie de [besturingssysteem](#operating-system) sectie voor specifieke OS-versies die worden ondersteund.

Download de nieuwste versie van de [WAImportExport hulpprogramma](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). Zie voor meer informatie over het gebruik van het hulpprogramma WAImportExport de [met het hulpprogramma WAImportExport](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Vorige versie:** kunt u [WAImportExpot V1 downloaden](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) versie van het hulpprogramma en verwijzen naar [WAImportExpot V1 usage guide](storage-import-export-tool-how-to-v1.md). WAImportExpot V1-versie van het hulpprogramma biedt ondersteuning voor **schijven voorbereiden wanneer gegevens al vooraf is geschreven naar de schijf**. Als de sleutel is alleen beschikbaar SAS-sleutel is, moet u WAImportExpot V1-hulpprogramma te gebruiken.

>

### <a name="hard-disk-drives"></a>Harde schijven
Alleen 2,5 inch SSD of 2,5-inch of 3.5" SATA II of III interne harde schijven worden ondersteund voor gebruik met de Import/Export-service. Een taak één importeren/exporteren kan maximaal 10 HDD SSD's en elke afzonderlijke harde schijven per SSD van elke grootte kunnen zijn. Groot aantal schijven kan worden verdeeld over meerdere taken en er is geen beperkingen op het aantal taken dat kan worden gemaakt. 

Voor taken van gegevensimport wordt alleen het eerste gegevensvolume op het station verwerkt. Het gegevensvolume moet zijn geformatteerd met NTFS.

> [!IMPORTANT]
> Externe harde schijven die worden geleverd met een ingebouwde USB-adapter worden niet ondersteund door deze service. Bovendien kan de schijf binnen het hoofdlettergebruik van een externe harde schijf kan niet worden gebruikt; Stuur geen externe HDD's.
> 
> 

Hier volgt een lijst met externe USB-adapters die worden gebruikt om gegevens te kopiëren naar interne harde schijven. Anker 68UPSATAA - 02 door Anker 68UPSHHDS door Startech SATADOCK22UE Orico 6628SUS3 C BK (6628-serie) Thermaltake BlacX Hot Swap SATA externe harde schijf dockingstation (USB 2.0 & eSATA)

### <a name="encryption"></a>Versleuteling
De gegevens op de schijf moeten worden versleuteld met BitLocker-stationsversleuteling. Deze versleuteling beschermt u uw gegevens terwijl het onderweg is.

Er zijn twee manieren om uit te voeren van de versleuteling voor taken van gegevensimport. De eerste manier is om op te geven van de optie bij gebruik van CSV-bestand gegevensset tijdens het uitvoeren van het hulpprogramma WAImportExport tijdens de voorbereiding van het station. De tweede manier is het inschakelen van BitLocker-versleuteling handmatig op de schijf en geef de versleutelingssleutel in de driveset CSV wanneer WAImportExport hulpprogramma vanaf de opdrachtregel wordt uitgevoerd tijdens de voorbereiding van het station.

Voor het exporteren, nadat uw gegevens worden gekopieerd naar de stations de service wordt het station versleutelen met BitLocker voordat deze terug naar de back-upfunctie. De versleutelingssleutel is verleend via de Azure-portal.  

### <a name="operating-system"></a>Besturingssysteem
U kunt een van de volgende 64-bits besturingssystemen voor het voorbereiden van de harde schijf met het hulpprogramma WAImportExport voordat het back-upfunctie het station naar Azure:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, WindowsServer 2012, Windows Server 2012 R2. Alle deze besturingssystemen ondersteunen BitLocker-stationsversleuteling.

### <a name="locations"></a>Locaties
De Azure Import/Export-service ondersteunt kopiëren van gegevens naar en van alle openbare Azure-opslagaccounts. U kunt harde schijven op een van de locaties kan worden verzonden. Als uw storage-account zich in een openbare Azure-locatie die wordt hier niet gespecificeerd, wordt een back-ups van alternatieve locatie worden opgegeven tijdens het maken van de taak met de Azure-portal of de REST-API voor importeren/exporteren.

Back-upfunctie locaties ondersteund:

* VS - oost
* VS - west
* VS - oost 2
* VS - west 2
* VS - midden
* Noord-centraal VS
* Zuid-centraal VS
* West-centraal VS
* Noord-Europa
* West-Europa
* Oost-Azië
* Zuidoost-Azië
* Australië - oost
* Australië - zuidoost
* Japan - west
* Japan - oost
* Centraal-India
* Zuid-India
* West-India
* Canada - midden
* Canada - oost
* Brazilië - zuid
* Korea - centraal
* VS (overheid) - Virginia
* VS (overheid) - Iowa
* US DoD - oost
* US DoD - centraal
* China - oost
* China - noord
* Verenigd Koninkrijk Zuid
* Duitsland - centraal
* Duitsland - noordoost

### <a name="shipping"></a>Verzenden
**Back-ups van stations met Datacenter:**

Bij het maken van een taak importeren of exporteren, krijgt u een back-ups van-adres van een van de ondersteunde locaties voor het verzenden van uw schijven. Opgegeven adres van de back-upfunctie is afhankelijk van de locatie van uw storage-account, maar mogelijk niet hetzelfde zijn als de opslaglocatie voor het account.

FedEx, UPS of DHL kan worden gebruikt voor het verzenden van uw schijven naar het adres van de back-upfunctie.

**Back-upfunctie voor de schijven van het datacenter:**

Bij het maken van een taak importeren of exporteren, moet u opgeven dat een retouradres voor Microsoft levert de stations terug nadat de taak voltooid is. Zorg ervoor dat u een geldig retouradres om te voorkomen dat vertragingen bij het verwerken van opgeven.

De provider moet hebben met het juiste bijhouden om bewakingsketen onderhouden. U moet een geldige FedEx, UPS, opgeven of DHL carrier account dat moet worden gebruikt door Microsoft voor het verzenden van de stations terug. Een nummer FedEx, UPS of DHL is vereist voor back-upfunctie stations terug van de Verenigde Staten en Europa locaties. Een nummer voor de DHL is vereist voor back-upfunctie stations terug vanaf Asia en Australië locaties. Als u geen abonnement hebt, kunt u een [FedEx](http://www.fedex.com/us/oadr/) (voor de Verenigde Staten en Europa) of [DHL](http://www.dhl.com/) (Asia en Australië) carrier-account. Als u al een nummer voor de provider hebt, controleert u of geldig is.

In uw pakketten verzendt, moet u de voorwaarden op volgen [Microsoft Azure-servicevoorwaarden](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Houd er rekening mee dat de fysieke media die u levert mogelijk moet cross internationale grenzen heen. U bent zelf verantwoordelijk voor het garanderen dat uw fysieke media en de gegevens zijn geïmporteerd en/of geëxporteerd in overeenstemming met het toepasselijk recht. Voordat de back-ups van de fysieke media Neem contact op met uw adviseurs om te controleren of uw media en de gegevens kan dit wettelijk naar het geïdentificeerde datacenter worden verzonden. Dit zorgt ervoor dat het Microsoft tijdig is bereikt. Alle pakketten die over internationale grenzen heen moet bijvoorbeeld een commerciële factuur gepaard gaan met het pakket (met uitzondering van kruisende randen binnen de Europese Unie als). U kunt een gevulde kopie van de commerciële factuur van carrier website afdrukken. Voorbeeld van een commerciële facturen zijn [DHL commerciële factuur](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) en [FedEx commerciële factuur](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Zorg ervoor dat Microsoft niet als de exportfunctie is aangegeven.
> 
> 

## <a name="how-does-the-azure-importexport-service-work"></a>Hoe werkt de Azure Import/Export-service?
U kunt gegevens overbrengen tussen uw lokale site en de Azure-opslag met de Azure Import/Export-service door de taken te maken en verzenden van harde schijven naar een Azure-Datacenter. Elke harde schijf die u verzendt, is gekoppeld aan één taak. Elke taak is gekoppeld aan een enkele storage-account. Controleer de [vereisten sectie](#pre-requisites) zorgvuldig voor meer informatie over de details van deze service zoals de ondersteunde gegevenstypen, schijf-typen, locaties en back-upfunctie.

In deze sectie worden beschreven van hoog niveau stappen voor het importeren en exporteren van taken. Verderop in de [Quick Start-sectie](#quick-start), stapsgewijze instructies voor het maken van een importeren en exporteren van de taak is opgegeven.

### <a name="inside-an-import-job"></a>Binnen een import-taak
Op een hoog niveau omvat een import-taak de volgende stappen:

* De gegevens moeten worden geïmporteerd en het aantal stations dat u moet bepalen.
* Identificeer de doellocatie voor blob of bestand voor uw gegevens in Azure-opslag.
* Het hulpprogramma WAImportExport gebruiken om uw gegevens kopiëren naar een of meer harde schijven en ze te versleutelen met BitLocker.
* Maak een import-taak in uw doel storage-account via de Azure portal of de REST-API voor importeren/exporteren. Als u de Azure portal gebruikt, moet u de station journaal-bestanden uploaden.
* Geef de afzender en carrier account dat moet worden gebruikt voor het verzenden van de stations voor u.
* De harde schijven naar het opgegeven tijdens het maken van de taak back-upfunctie adres verzenden.
* Werk de bezorging volgnummer in de taakgegevens importeren en verzenden van de importtaak.
* Stations worden ontvangen en verwerkt in het Azure Datacenter.
* Schijven worden geleverd met uw account carrier adres van de afzender opgegeven in de import-taak.
  
    ![Afbeelding 1:Import taak stroom](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>Binnen een taak voor het exporteren
> [!IMPORTANT]
> De service alleen ondersteuning voor het exporteren van de Azure-Blobs en biedt geen ondersteuning voor het exporteren van de Azure-bestanden...
> 
>

Op een hoog niveau omvat een exporttaak de volgende stappen:

* De gegevens worden geëxporteerd en het aantal stations dat u moet bepalen.
* Identificeer de bron blobs of paden van uw gegevens in Blob storage-container.
* Maak een exporttaak in uw bron storage-account via de Azure portal of de REST-API voor importeren/exporteren.
* Geef de bron blobs of paden van de container van uw gegevens in de taak voor exporteren.
* Geef de retouradres en het nummer van carrier voor moet worden gebruikt voor het verzenden van de stations voor u.
* De harde schijven naar het opgegeven tijdens het maken van de taak back-upfunctie adres verzenden.
* Werk de bezorging volgnummer in de taakgegevens exporteren en verzenden van de taak voor exporteren.
* De stations worden ontvangen en verwerkt in het Azure Datacenter.
* De stations zijn versleuteld met BitLocker; de sleutels zijn beschikbaar via de Azure-portal.  
* De stations worden geleverd met uw account carrier adres van de afzender opgegeven in de import-taak.
  
    ![Afbeelding 2:Export taak stroom](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>De status van de taak en het station weergeven
U kunt de status van de importbewerking volgen of taken exporteren vanuit de Azure-portal. Klik op de **voor importeren/exporteren** tabblad. Een lijst met uw taken wordt weergegeven op de pagina.

![Status van de taak weergeven](./media/storage-import-export-service/jobstate.png)

U ziet een van de volgende statussen van de taak afhankelijk van waar de schijf in het proces is.

| De Status van taak | Beschrijving |
|:--- |:--- |
| Maken | Nadat een taak is gemaakt, wordt de status is ingesteld op maken. Terwijl de taak de status maken wordt, de Import/Export-service wordt ervan uitgegaan dat de schijven nog niet zijn verzonden naar het datacenter. Een taak kan blijven in de status maken voor maximaal twee weken, waarna deze worden automatisch verwijderd door de service. |
| Verzenden | Nadat u uw pakket verzendt, moet u de controle-informatie in de Azure portal bijwerken.  Hierdoor wordt de taak in 'Back-upfunctie'. De taak blijft in de status van de back-ups van twee weken. 
| Ontvangen | Nadat alle stations zijn ontvangen in het datacenter, wordt de taakstatus ingesteld in de ontvangen. |
| Overbrengen | Nadat ten minste één station is begonnen verwerking, wordt de taakstatus worden ingesteld op de overdragen. Zie de sectie station statussen hieronder voor meer informatie. |
| Verpakken | Nadat alle stations verwerking is voltooid, wordt de taak in de status van de verpakking geplaatst totdat de stations worden verzonden naar u terug. |
| Voltooid | Nadat alle stations zijn verzonden naar de klant, als de taak is voltooid zonder fouten, wordt de taak ingesteld op de status voltooid. De taak worden, automatisch verwijderd na 90 dagen in de status voltooid. |
| Gesloten | Nadat alle stations zijn verzonden naar de klant, als er fouten tijdens het verwerken van de taak zijn, wordt de taak ingesteld op de status Closed. De taak worden, automatisch verwijderd na 90 dagen in de status Closed. |

De volgende tabel bevat de levenscyclus van een afzonderlijke schijf als deze door middel van een taak worden geïmporteerd of geëxporteerd overgezet. De huidige status van elke schijf in een taak is nu zichtbaar zijn vanaf de Azure-portal.
De volgende tabel beschrijft elke status die elk station in een taak kan doorgeven.

| Status van station | Beschrijving |
|:--- |:--- |
| Opgegeven | Wanneer de taak is gemaakt vanuit de Azure-portal is de aanvankelijke status voor een station voor een import-taak de status van de opgegeven. Aangezien geen station opgeeft wanneer de taak is gemaakt, is de status van de eerste schijf voor een exporttaak de status Received hebben. |
| Ontvangen | Het station overgangen naar de status Received hebben wanneer de stations die zijn ontvangen van het bedrijf back-upfunctie voor een import-taak is verwerkt door de operator Import/Export-service. Voor een exporttaak is de status van de eerste schijf de status Received hebben. |
| NeverReceived | Het station wordt verplaatst naar de status NeverReceived wanneer het pakket voor een taak binnenkomt, maar het pakket niet het station bevat. Een station kunt ook verplaatsen naar deze status als deze twee weken is sinds de service heeft ontvangen van de back-ups van gegevens, maar het pakket nog niet in het datacenter ontvangen is. |
| Overbrengen | Een station wordt verplaatst naar de status van de overdragen wanneer de service begint met de gegevens van de schijf overbrengen naar Windows Azure Storage. |
| Voltooid | Een station wordt verplaatst naar de status voltooid wanneer de service heeft de gegevens zonder fouten is overgedragen.
| CompletedMoreInfo | Een station wordt verplaatst naar de status CompletedMoreInfo wanneer de service enkele problemen aangetroffen heeft tijdens het kopiëren van gegevens van of naar het station. De informatie kan bestaan fouten, waarschuwingen of informatieve berichten over het overschrijven van blobs.
| ShippedBack | Het station wordt verplaatst naar de status ShippedBack wanneer deze is verzonden vanaf de achterkant van data center adres van de afzender. |

Deze installatiekopie van de Azure-portal wordt de status van de schijf van de taak voor een voorbeeld weergegeven:

![Status van station weergeven](./media/storage-import-export-service/drivestate.png)

De volgende tabel beschrijft de statussen van de fout station en de acties die voor elke status.

| Status van station | Gebeurtenis | Resolutie / de volgende stap |
|:--- |:--- |:--- |
| NeverReceived | Een station dat is gemarkeerd als NeverReceived (omdat deze niet is ontvangen als onderdeel van de verzending van de taak) in een andere verzending binnenkomt. | Het operationele team wordt het station worden verplaatst naar de status Received hebben. |
| N/A | Een station dat geen deel uitmaakt van elke taak komt in het datacenter als onderdeel van een andere taak. | Het station wordt gemarkeerd als een extra schijf en naar de klant wordt geretourneerd wanneer de taak die is gekoppeld aan het oorspronkelijke pakket is voltooid. |

### <a name="time-to-process-job"></a>Tijd tot verwerkingstaak
De hoeveelheid tijd die nodig is voor een taak van import/export, afhankelijk van verschillende factoren, zoals back-ups van tijd varieert, proces taak type, type en grootte van de gegevens worden gekopieerd en de grootte van de schijven die zijn opgegeven. De Import/Export-service beschikt niet over een SLA maar nadat de schijven worden ontvangen. de service wil de kopie in 7 tot 10 dagen voltooien. De REST-API kunt u de voortgang van de taak nauwkeuriger te volgen. Er is een percentage voltooid parameter in de lijst met taken bewerking waarmee een indicatie van de voortgang van de kopie. Bereiken voor ons. Als u een schatting maken om een tijd kritieke voor importeren/exporteren taak te voltooien.

### <a name="pricing"></a>Prijzen
**Station kosten verwerken**

Er is een station verwerking vast bedrag voor elk station verwerkt als onderdeel van het importeren of exporteren van de taak. Zie de details over de [prijzen van Azure Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Back-upfunctie voor kosten**

Tijdens het verzenden van stations naar Azure, betaalt u de kosten van de back-upfunctie voor aan de back-ups van provider. Wanneer Microsoft de stations naar u terugkeert, wordt de kosten van de back-upfunctie voor verrekend met het provider-account die u hebt opgegeven op het moment van de taak maken.

**Transactiekosten**

Er zijn geen transactiekosten bij het importeren van gegevens in Azure Storage. De kosten voor standaard uitgaande zijn van toepassing wanneer gegevens worden geëxporteerd van Blob-opslag. Zie voor meer informatie over transactiekosten [gegevensoverdracht prijzen.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="how-to-import-data-into-azure-file-storage-using-internal-sata-hdds-and-ssds"></a>Het importeren van gegevens in Azure File Storage met interne SATA harde schijven en SSD's?
Volg de onderstaande stappen te volgen als de gegevens op de schijf moet worden geïmporteerd in Azure File Storage.
De eerste stap bij het importeren van gegevens met behulp van de Azure Import/Export-service is het voorbereiden van uw schijven met het hulpprogramma WAImportExport. Volg onderstaande stappen voor het voorbereiden van uw schijven.

1. De gegevens wilt importeren in Azure File Storage identificeren. Dit kan mappen en bestanden op de lokale server of een netwerkshare zelfstandig zijn.  
2. Het aantal stations dat u, afhankelijk van de totale grootte van de gegevens moet bepalen. Het vereiste aantal 2,5 inch SSD of 2,5-inch of 3.5" schaft u SATA II of III harde schijven.
4. Bepaal de mappen en/of de zelfstandige bestanden die moeten worden gekopieerd naar elke harde schijf.
5. De CSV-bestanden voor de gegevensset en driveset maken.
    
  Hieronder volgt een voorbeeld voorbeeld gegevensset CSV-bestand voor het importeren van gegevens als Azure-bestanden:
  
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","fileshare/100M_1.csv.txt",file,rename,"None",None
    "F:\50M_original\","fileshare/",file,rename,"None",None 
    ```
   In het bovenstaande voorbeeld worden 100M_1.csv.txt gekopieerd naar de hoofdmap van de 'bestandsshare'. Als de "bestandsshare' niet bestaat, kunt u een wordt gemaakt. Alle bestanden en mappen onder 50M_original worden gekopieerd naar de bestandsshare. Mapstructuur behouden blijft.

    Meer informatie over [voorbereiden van het CSV-bestand van gegevensset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    


    **Driveset CSV-bestand**

    De waarde van de vlag driveset is een CSV-bestand waarin de lijst met schijven waaraan de stationsletters zijn toegewezen om het hulpprogramma correct kiest de lijst met schijven te worden voorbereid. 

    Hieronder vindt u in het voorbeeld van driveset CSV-bestand:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    In het bovenstaande voorbeeld wordt ervan uitgegaan dat twee schijven zijn gekoppeld en standaard NTFS-volumes met volumeletter G:\ en H:\ zijn gemaakt. Het hulpprogramma wordt opmaken en de schijf die als host fungeert voor H:\ en wordt niet opmaken of de schijf die als host fungeert voor volume G:\ versleutelen versleutelen.

    Meer informatie over [voorbereiden van het CSV-bestand driveset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Gebruik de [WAImportExport hulpprogramma](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) uw gegevens kopiëren naar een of meer harde schijven.
7.  U kunt 'Coderen' opgeven op versleuteling veld in drivset CSV voor het inschakelen van BitLocker-versleuteling op de harde schijf. U kunt ook kan u ook Schakel versleuteling in BitLocker handmatig op de harde schijf en 'AlreadyEncrypted' opgeven en de sleutel in de driveset CSV opgeven tijdens het uitvoeren van het hulpprogramma.

8. De gegevens op de harde schijven of het journaalbestand mag niet worden gewijzigd na het voltooien van de schijfvoorbereiding van de.

> [!IMPORTANT]
> Elke harde schijf die u wilt voorbereiden, resulteert in een journal-bestand. Wanneer u de import-taak met de Azure portal maakt, moet u alle journaal-bestanden van de stations die deel van deze importtaak uitmaken uploaden. Stations zonder journaal bestanden wordt niet verwerkt.
> 
>

Hieronder ziet u de opdrachten en voorbeelden voor het voorbereiden van de harde schijf met WAImportExport hulpprogramma.

WAImportExport hulpprogramma PrepImport opdracht voor de eerste kopieersessie voor het kopiëren van mappen en/of bestanden met een nieuwe kopieersessie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Voorbeeld 1 importeren**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Om **meer stations toevoegen**, een kunt maken van een nieuw driveset-bestand en voer de opdracht zoals hieronder. Geef een nieuwe driveset CSV-bestand voor latere kopie-sessies in de verschillende schijfstations dan is opgegeven in het CSV-bestand InitialDriveset, en opgeven als een waarde in de parameter 'AdditionalDriveSet'. Gebruik de **hetzelfde journaalbestand** een naam en geef een **nieuwe sessie-ID**. De indeling van AdditionalDriveset CSV-bestand is hetzelfde als InitialDriveSet-indeling.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Voorbeeld 2 importeren**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

Om aanvullende gegevens toevoegen aan de dezelfde driveset, WAImportExport hulpprogramma PrepImport opdracht kan worden aangeroepen voor latere kopie sessies aanvullende bestanden en de map te kopiëren: voor latere kopie-sessies door naar de dezelfde harde schijven die is opgegeven in InitialDriveset CSV bestand, geeft de **hetzelfde journaalbestand** een naam en geef een **nieuwe sessie-ID**; is niet nodig om de sleutel van het opslagaccount.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Voorbeeld 3 importeren**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Meer informatie over het gebruik van het hulpprogramma WAImportExport in [harde schijven voorbereiden voor het importeren van](storage-import-export-tool-preparing-hard-drives-import.md).

Ook verwijzen naar de [voorbeeldwerkstroom harde schijven voorbereiden voor een import-taak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) voor stapsgewijze instructies gedetailleerde.  



## <a name="create-an-export-job"></a>Een exporttaak maken
Maak een exporttaak voor het verwittigen van de Import/Export-service dat u zult worden back-upfunctie lege stations voor een of meer tot het datacenter zodat gegevens kunnen worden geëxporteerd vanuit uw storage-account aan de stations en de stations en vervolgens naar u verzonden.

### <a name="prepare-your-drives"></a>Voorbereiden van uw schijven
Eerste controles van volgende worden aanbevolen voor het voorbereiden van uw schijven voor een taak voor het exporteren:

1. Controleer het aantal schijven met behulp van het hulpprogramma WAImportExport PreviewExport opdracht vereist. Zie voor meer informatie [station gebruik bekijken voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx). Kunt u de preview-schijfgebruik voor de blobs die u hebt geselecteerd, op basis van de grootte van de stations die u gaat gebruiken.
2. Controleer of u kunt lezen/schrijven naar de harde schijf die voor de taak voor het exporteren worden verzonden.

### <a name="create-the-export-job"></a>De exporttaak maken
1. Voor het maken van een taak voor het exporteren, gaat u naar meer services -> opslag -> 'taken voor importeren/exporteren' in de Azure portal. Klik op **maken voor importeren/exporteren taak**.
2. In stap 1 basisbeginselen, selecteer 'Exporteren uit Azure' Voer een tekenreeks voor de taaknaam van de, selecteert u een abonnement, typ of Selecteer een resourcegroep. Voer een beschrijvende naam voor de import-taak. Houd er rekening mee dat de naam die u invoert alleen kleine letters, cijfers, afbreekstreepjes mogen en onderstrepingstekens bevatten, moet beginnen met een letter en mag geen spaties bevatten. U gebruikt de naam die u kiest voor het bijhouden van uw taken terwijl ze zijn uitgevoerd en wanneer ze zijn voltooid. Neem contact op met gegevens voor de persoon die verantwoordelijk is voor deze taak voor exporteren opgeven. 

3. Selecteer in stap 2-taakdetails, het opslagaccount dat de gegevens worden geëxporteerd uit in de sectie Storage-account. De afgiftepunt worden automatisch ingevuld op basis van de regio van het geselecteerde opslagaccount. Geef op welke blob-gegevens die u wilt exporteren van uw opslagaccount naar uw lege schijf of schijven. U kunt kiezen om alle blobgegevens in het opslagaccount te exporteren of u kunt opgeven dat blobs of ingesteld van BLOB's om te exporteren.
   
   Een blob om te exporteren, gebruikt u de **gelijk aan** selector en geeft u het relatieve pad naar de blob, beginnend met de containernaam van de. Gebruik *$root* om op te geven van de basiscontainer.
   
   Alle blobs die beginnen met een voorvoegsel gebruikt u de **begint met** selector en geef het voorvoegsel, beginnen met een slash '/'. Het voorvoegsel mogelijk het voorvoegsel van de containernaam van de, de containernaam van de volledige of de containernaam van de volledige gevolgd door het voorvoegsel van de blob-naam.
   
   De volgende tabel ziet u voorbeelden van geldige blob-paden:
   
   | Selector | Blobpad | Beschrijving |
   | --- | --- | --- |
   | Begint met |/ |Exporteert u alle blobs in de storage-account |
   | Begint met |/$root / |Alle blobs in de hoofdmap container exporteert |
   | Begint met |/Book |Alle blobs in elke container die met het voorvoegsel begint exporteert **adresboek** |
   | Begint met |/Music/ |Alle blobs in container exporteert **muziek** |
   | Begint met |hou muziek / |Alle blobs in container exporteert **muziek** die beginnen met het voorvoegsel **favoriete** |
   | Gelijk aan |$root/logo.bmp |Uitvoer-blob **logo.bmp** in de hoofdmap-container |
   | Gelijk aan |videos/story.mp4 |Uitvoer-blob **story.mp4** in de container **video's** |
   
   U kunt de blob-paden in geldige notaties om fouten te voorkomen tijdens de verwerking moet opgeven, zoals weergegeven in deze schermafbeelding.
   
   ![Maak exporttaak voor - stap 3](./media/storage-import-export-service/export-job-03.png)

4. In stap 3 retourneren info verzenden, selecteert u de provider in de vervolgkeuzelijst en voer een geldige carrier account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor het verzenden van de stations voor u zodra de import-taak is voltooid. Geef een volledig en geldig contactpersoon, telefoon, e, adres, stad, zip, staat/proviince en land/regio.
   
 5. In de pagina overzicht wordt Azure DataCenter verzendadres moet worden gebruikt voor het verzenden van schijven naar Azure DC verstrekt. Zorg ervoor dat de taaknaam en het volledige adres worden vermeld op het label van de back-upfunctie. 

6. Klik op OK op de pagina Samenvatting om uit te voeren van Import-taak maken

7. Na de verzending van de schijven, terug naar de **voor importeren/exporteren** pagina op de Azure-portal a) Ga en klik op de import-taak b) klikt u op **bijwerken taakstatus en bij te houden info zodra stations worden verzonden**. 
     c) Selecteer het selectievakje "Markeren als verzonden"-d) voorziet in het nummer van de provider en bijhouden.
    
   Als het volgnummer niet binnen twee weken bijgewerkt is van de taak wordt gemaakt, verloopt de taak.
   
8. U kunt de voortgang van de taak volgen op het portaldashboard. Zie de taakstatus van elke in het vorige gedeelte: door [weer te geven de status van uw taak](#viewing-your-job-status).

   > [!NOTE]
   > Als de blob wordt geëxporteerd gebruikt op het moment wordt van kopiëren naar de harde schijf, wordt de Azure Import/Export-service een momentopname van de blob en kopiëren van de momentopname.
   > 
   > 
 
9. Nadat u de stations met de geëxporteerde gegevens ontvangt, kunt u deze kunt bekijken en de BitLocker-sleutels die worden gegenereerd door de service voor de schijf kopiëren. Ga voor het exporteren van de taak in de Azure-portal en klik op het tabblad voor importeren/exporteren. Selecteer de taak voor het exporteren uit de lijst en klik op de optie voor BitLocker-sleutels. De BitLocker-sleutels worden weergegeven zoals hieronder wordt weergegeven:
   
   ![BitLocker-sleutels voor de exporttaak weergeven](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Ga via de sectie Veelgestelde vragen hieronder als dit de meest voorkomende vragen klanten optreden bij het gebruik van deze service omvat.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Kan ik Azure File storage met behulp van de service Azure Import/Export kopiëren?**

Ja, ondersteunt de Azure Import/Export-service importeren naar Azure File Storage. Exporteren van het Azure-bestanden op dit moment ondersteunt niet.

**Is de Azure Import/Export-service beschikbaar voor abonnementen van de CSP?**

Azure Import/Export-service biedt ondersteuning voor abonnementen van de CSP.

**Kan ik de stap ter voorbereiding van station voor een import-taak overslaan of kan ik een station voorbereiden zonder te kopiëren?**

Een station dat u verzenden wilt voor het importeren van gegevens moet worden voorbereid met het hulpprogramma Azure WAImportExport. U moet het hulpprogramma WAImportExport gebruiken om gegevens te kopiëren naar het station.

**Heb ik nodig om uit te voeren van de schijfvoorbereiding van een bij het maken van een taak voor het exporteren?**

Eerste controles van geen, maar sommige worden aanbevolen. Controleer het aantal schijven met behulp van het hulpprogramma WAImportExport PreviewExport opdracht vereist. Zie voor meer informatie [station gebruik bekijken voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx). Kunt u de preview-schijfgebruik voor de blobs die u hebt geselecteerd, op basis van de grootte van de stations die u gaat gebruiken. Controleer ook gelezen uit en schrijven naar de harde schijf die voor de taak voor het exporteren worden verzonden.

**Wat gebeurt er als ik verstuurt per ongeluk een harde schijf die niet voldoet aan de vereisten voor ondersteunde?**

De Azure-Datacenter wordt het station dat niet voldoet aan de ondersteunde eisen aan u geretourneerd. Als er slechts enkele van de stations in het pakket voldoen aan de ondersteuningsvereisten die stations worden verwerkt en de stations die niet voldoen aan de vereisten voor u worden geretourneerd.

**Kan ik mijn taak annuleren?**

U kunt een taak annuleren wanneer de status ervan wordt maken of back-upfunctie.

**Hoe lang kan ik de status van de voltooide taken weergeven in de Azure portal?**

U kunt de status voor voltooide taken weergeven voor maximaal 90 dagen. Voltooide taken worden na 90 dagen verwijderd.

**Wat moet ik doen als ik wil importeren of exporteren van meer dan 10 schijven?**

Invoer of exporttaak kan verwijzen naar alleen 10 schijven in een enkele taak voor de Import/Export-service. Als u meer dan 10 schijven verzenden wilt, kunt u meerdere taken kunt maken. Stations die gekoppeld aan dezelfde taak zijn moeten samen worden verzonden in hetzelfde pakket.
Microsoft biedt richtlijnen en hulp wanneer gegevenscapaciteit meerdere schijf omvat importtaken. Neem contact op met bulkimport@microsoft.com voor meer informatie

**De service formatteren van de schijven voordat deze wordt teruggezonden?**

Nee. Alle stations zijn versleuteld met BitLocker.

**Kan ik stations voor importeren/exporteren taken kopen van Microsoft?**

Nee. U moet worden geleverd stations voor beide importeren en exporteren van taken.

** Hoe kan ik toegang tot gegevens die zijn geïmporteerd door deze service **

De gegevens onder uw Azure storage-account toegankelijk zijn via Azure Portal of met behulp van een zelfstandige tool Opslagverkenner wordt aangeroepen. https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer 

**Nadat de import-taak is voltooid, hoe worden mijn gegevens eruit in de storage-account? Mijn directory-hiërarchie worden bewaard?**

Als u een harde schijf voorbereidt voor een import-taak, wordt de bestemming opgegeven in het veld DstBlobPathOrPrefix in de gegevensset CSV. Dit is de doelcontainer in het opslagaccount waarvoor gegevens van de vaste schijf worden gekopieerd. Virtuele mappen worden gemaakt voor mappen van de vaste schijf en blobs zijn gemaakt voor bestanden in deze bestemmingscontainer. 

**Als het station bestanden die al bestaan in mijn storage-account heeft, de service overschrijft bestaande blobs of bestanden in mijn storage-account?**

Bij het voorbereiden van het station kunt u opgeven of de doel-bestanden moeten worden overschreven of genegeerd met het veld in het CSV-bestand gegevensset toestand aangeroepen: < naam | Nee overschrijven | overschrijven >. Standaard wordt de service de naam van de nieuwe bestanden en niet overschrijven bestaande blobs of bestanden.

**Het hulpprogramma WAImportExport compatibel is met 32-bits besturingssystemen?**
Nee. Het hulpprogramma WAImportExport is alleen compatibel met 64-bits Windows-besturingssystemen. Raadpleeg de sectie Operating Systems in de [vereisten](#pre-requisites) voor een volledige lijst met ondersteunde versies van het besturingssysteem.

**Moet ik iets anders dan de vaste schijf in mijn pakket opnemen?**

Verzend alleen de vaste schijven. Neem geen items zoals power supply kabels of USB-kabels.

**Heb ik mijn schijven met FedEx of DHL verzenden?**

U kunt schijven naar het datacenter met behulp van een bekende carrier zoals FedEx, DHL, UPS of ons opstuurt verzenden. Voor het verzenden van de stations voor u van het datacenter moet u evenwel een nummer in de Verenigde Staten en de EU FedEx of een nummer in de regio's Asia en Australië DHL.

**Zijn er beperkingen met mijn station internationaal verzenden?**

Houd er rekening mee dat de fysieke media die u levert mogelijk moet cross internationale grenzen heen. U bent zelf verantwoordelijk voor het garanderen dat uw fysieke media en de gegevens zijn geïmporteerd en/of geëxporteerd in overeenstemming met het toepasselijk recht. Voordat de back-ups van de fysieke media Neem contact op met uw adviseurs om te controleren of uw media en de gegevens kan dit wettelijk naar het geïdentificeerde datacenter worden verzonden. Dit helpt ervoor te zorgen dat het Microsoft tijdig is bereikt.

**Wanneer u een taak maakt, is het adres van de back-ups van een locatie die verschilt van de locatie van mijn storage-account. Wat moet ik doen?**

Sommige opslaglocaties account zijn toegewezen aan back-upfunctie voor alternatieve locaties. Eerder kunnen beschikbare back-upfunctie locaties ook worden tijdelijk toegewezen naar alternatieve locaties. Controleer altijd het verzendadres opgegeven tijdens het maken van de taak vóór het verzenden van uw schijven.

**Tijdens het verzenden van mijn station, wordt u gevraagd de provider voor de data center-mailadres en telefoonnummer telefoonnummer. Wat moet ik bieden?**

Het telefoonnummer en de DC-adressen is beschikbaar als onderdeel van taak maken.

**Kan ik de Azure Import/Export-service PST postvakken en SharePoint-gegevens kopiëren naar O365 gebruiken?**

Raadpleeg [importeren PST-bestanden of SharePoint-gegevens op Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kan ik de Azure Import/Export-service offline Mijn back-ups kopiëren naar de Azure Backup-Service gebruiken?**

Raadpleeg [Offlineback-upwerkstroom in Azure Backup](../../backup/backup-azure-backup-import-export.md).

**Wat is het maximum aantal harde schijven voor in één verzending?**

Een willekeurig aantal HDD's kan zich in één verzending en als de schijven tot meerdere taken behoren het beste een) de schijven die zijn gelabeld met de desbetreffende namen van de taak. b) de taken bijwerken met een volgnummer dat wordt voorafgegaan door -1,-2, enzovoort.
  
**Wat is de maximale blok-Blob en pagina-blobgrootte ondersteund door schijf importeren/exporteren?**

De grootte van de maximale blok-Blob is ongeveer 4.768TB of 5,000,000 MB.
Maximale Page Blob-grootte is 1TB.

**Schijf Import/Export biedt ondersteuning voor AES 256-versleuteling?**

Azure Import/Export-service standaard versleuteld met AES 128 bitlocker-versleuteling, maar dit kan worden verhoogd tot AES 256 door handmatig te versleutelen met bitlocker voordat gegevens worden gekopieerd. 

Als u [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), hieronder volgt een voorbeeld van een opdracht
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Als u [WAImportExport hulpprogramma](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) opgeven 'AlreadyEncrypted' en de sleutel in de driveset CSV opgeven.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Volgende stappen

* [Instellen van het hulpprogramma WAImportExport](storage-import-export-tool-how-to.md)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Azure Import exporteren REST-API-voorbeeld](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)


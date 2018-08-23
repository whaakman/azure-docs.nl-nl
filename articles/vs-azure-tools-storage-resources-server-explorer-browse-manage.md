---
title: Blader en storage-resources beheren met behulp van Server Explorer | Microsoft Docs
description: Bladeren door en storage-resources beheren met behulp van Server Explorer
services: visual-studio-online
author: ghogen
manager: douge
assetId: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/24/2017
ms.author: ghogen
ms.openlocfilehash: 74f5508586d073bcccc54894cce6fcde1b83fe18
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057262"
---
# <a name="browse-and-manage-storage-resources-by-using-server-explorer"></a>Blader en storage-resources beheren met behulp van Server Explorer

[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht

Als u Azure-hulpprogramma's voor Microsoft Visual Studio hebt geïnstalleerd, kunt u blob-, wachtrij- en tabelgegevens van uw opslagaccounts bekijken voor Azure. De Azure **opslag** knooppunt in Server Explorer toont gegevens die zich in uw lokale emulator van de opslagaccount en andere Azure storage-accounts.

Als u wilt weergeven Server Explorer in Visual Studio op de menubalk, selecteer **weergave** > **Server Explorer**. De **opslag** knooppunt bevat alle van de storage-accounts die aanwezig zijn in elke Azure-abonnement of het certificaat dat u bent verbonden. Als uw storage-account niet wordt weergegeven, kunt u deze toevoegen door de instructies te volgen [verderop in dit artikel](#add-storage-accounts-by-using-server-explorer).

Starten in de Azure SDK 2.7, kunt u ook Cloud Explorer gebruiken weergeven en beheren van uw Azure-resources. Zie voor meer informatie, [beheren Azure-resources met Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md).

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Weergeven en beheren van opslagbronnen in Visual Studio

Server Explorer geeft een lijst met blobs, wachtrijen en tabellen automatisch in uw opslagaccount van de emulator. Het opslagaccount van de emulator wordt vermeld in Server Explorer onder de **opslag** knooppunt als de **ontwikkeling** knooppunt.

Als u wilt zien van de emulator van het opslagaccount resources, vouw de **ontwikkeling** knooppunt. Als de opslagemulator nog niet is gestart wanneer u de **ontwikkeling** knooppunt, wordt automatisch gestart. Dit proces kan enkele seconden duren. U kunt blijven werken in andere gebieden van Visual Studio, terwijl de storage-emulator wordt gestart.

Als resources wilt weergeven in een opslagaccount, vouwt u het opslagaccount dat knooppunt in Server Explorer waarin u ziet **Blobs**, **wachtrijen**, en **tabellen** knooppunten.

## <a name="work-with-blob-resources"></a>Werken met blob-resources

De **Blobs** knooppunt geeft een lijst van containers voor het geselecteerde opslagaccount. BLOB-containers bevatten blob-bestanden, en u kunt deze blobs ordenen in mappen en submappen. Zie voor meer informatie, [over het gebruik van Blob storage vanuit .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md).

### <a name="to-create-a-blob-container"></a>Een blob-container maken

1. Open het snelmenu voor het **Blobs** knooppunt en selecteert u vervolgens **Blob-Container maken**.
1. In de **Blob-Container maken** dialoogvenster vak, voer de naam van de nieuwe container.  
1. Selecteer Enter op het toetsenbord, of u kunt Klik of tik buiten het naamveld op te slaan van de blob-container.

   > [!NOTE]
   > De naam van de blob-container moet beginnen met een getal (0-9) of een kleine letter (a-z).

### <a name="to-delete-a-blob-container"></a>Een blob-container verwijderen

Open het snelmenu voor de blob-container die u wilt verwijderen en selecteer vervolgens **verwijderen**.

### <a name="to-display-a-list-of-the-items-in-a-blob-container"></a>Om een lijst van de items in een blob-container weer te geven

Open het snelmenu voor de naam van een blob-container in de lijst en selecteer vervolgens **Open**.

Wanneer u de inhoud van een blob-container bekijkt, wordt deze weergegeven op een tabblad bekend als de blob-container-weergave.

![BLOB-container weergeven](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

U kunt de volgende bewerkingen voor blobs uitvoeren met behulp van de knoppen in de rechterbovenhoek van de blob-container-weergave:

* Voer een filterwaarde en toepassen.
* Vernieuw de lijst met blobs in de container.
* Upload een bestand.
* Een blob verwijderen. (Een bestand verwijderen uit een blob-container niet het onderliggende bestand verwijderen. Deze alleen wordt deze verwijderd uit de blob-container.)
* Open een blob.
* Een blob in de lokale computer opslaan.

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Een map of submap maken in een blob-container

1. Kies de blob-container in Cloud Explorer. Selecteer in het containervenster de **Blob uploaden** knop.

1. In de **nieuwe bestand uploaden** in het dialoogvenster, selecteer de **Bladeren** knop om op te geven van het bestand dat u wilt uploaden, en voer vervolgens de naam van een map in de **map (optioneel)** vak.

   ![Een bestand uploaden naar een blob-map](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

   U kunt submappen in container-mappen toevoegen aan de hand van de dezelfde stap. Als u geen naam van een map opgeeft, wordt het bestand geüpload naar het hoogste niveau van de blob-container. Het bestand wordt weergegeven in de opgegeven map in de container.

   ![Map toevoegen aan een blob-container](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Dubbelklik op de map of selecteer Enter om de inhoud van de map te bekijken. Wanneer u zich in de map van de container, u kunt teruggaan naar de hoofdmap van de container door het selecteren van de **Open bovenliggende map** (pijl).

### <a name="to-delete-a-container-folder"></a>Een map container verwijderen

Verwijder alle bestanden in de map.

Omdat in blob-containers virtuele mappen zijn, kunt u een lege map kan niet maken. U ook een map voor het verwijderen van de bestandsinhoud niet verwijderen, maar de volledige inhoud van een map op de map zelf te verwijderen in plaats daarvan moet verwijderen.

### <a name="to-filter-blobs-in-a-container"></a>Voor het filteren van blobs in een container

U kunt filteren, de blobs die worden weergegeven door een algemene voorvoegsel op te geven.

Bijvoorbeeld, als u het voorvoegsel **hello** in het filtertekstvak en selecteer vervolgens de **Execute** (**!**) knop, alleen de blobs die met "Hallo beginnen" weergegeven.

![Tekstvak voor filter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

Het filtertekstvak is hoofdlettergevoelig en biedt geen ondersteuning voor filteren met jokertekens bevatten. BLOBs kunnen alleen met het voorvoegsel worden gefilterd. Het voorvoegsel kan een scheidingsteken bevatten als u een scheidingsteken om te organiseren van blobs in de hiërarchie van een virtuele. Bijvoorbeeld filteren op het voorvoegsel "HelloFabric /" retourneert alle blobs die met deze tekenreeks beginnen.

### <a name="to-download-blob-data"></a>Blob-gegevens downloaden

In de Cloud Explorer, moet u een van de volgende methoden gebruiken:

* Open het snelmenu voor een of meer blobs en selecteer vervolgens **Open**.
* Kies in de blobnaam en selecteer vervolgens de **Open** knop.
* Dubbelklik op de blobnaam van de.

De voortgang van een blob downloaden wordt weergegeven in de **Azure Activity Log** venster.

De blob wordt geopend in de standaardeditor voor dat bestandstype. Als het bestandstype wordt herkend door het besturingssysteem, wordt het bestand wordt geopend in een lokaal geïnstalleerde toepassing. Anders u wordt gevraagd om te kiezen van een toepassing die geschikt is voor het bestandstype van de blob. Het lokale bestand dat wordt gemaakt wanneer u een blob downloaden is gemarkeerd als alleen-lezen.

Blobgegevens worden lokaal opgeslagen in de cache en gecontroleerd aan de hand van de blob tijd van laatste wijziging in Azure Blob-opslag. Als de blob is bijgewerkt, omdat het laatst is gedownload, wordt deze opnieuw gedownload. Anders wordt de blob geladen van de lokale schijf.

Standaard wordt een blob gedownload naar een tijdelijke map. Om blobs te downloaden naar een specifieke map, open het snelmenu voor de geselecteerde blob-namen en selecteer **OpslaanAls**. Wanneer u een blob op deze manier opslaat, het blobbestand is niet geopend en het lokale bestand wordt gemaakt met de kenmerken voor lezen/schrijven.

### <a name="to-upload-blobs"></a>Blobs uploaden

Als u wilt blobs uploaden, selecteert u de **Blob uploaden** wanneer de container is geopend voor weergave in de weergave van blob-container.

U kunt een of meer bestanden te uploaden en u kunt bestanden van elk type uploaden. De **Azure Activity Log** venster toont de voortgang van het uploaden. Zie voor meer informatie over het werken met blob-gegevens [over het gebruik van Azure Blob storage in .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Logboeken die zijn overgebracht naar blobs weergeven

Als u met behulp van Azure Diagnostics logboekgegevens van uw Azure-toepassing en u logboeken naar uw storage-account hebt overgebracht, ziet u containers die Azure voor deze logboeken worden gemaakt. Deze logboeken weergeven in Server Explorer is een eenvoudige manier om te identificeren van problemen met uw toepassing, met name als deze is geïmplementeerd naar Azure.

Zie voor meer informatie over Azure Diagnostics [Logboekregistratiegegevens verzamelen met behulp van Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Aan de URL voor een blob ophalen

Open het snelmenu van de blob en selecteer vervolgens **kopie URL**.

### <a name="to-edit-a-blob"></a>Een blob bewerken

Selecteer de blob en selecteer vervolgens de **Open Blob** knop.

Het bestand is gedownload naar een tijdelijke locatie en geopend op de lokale computer. Upload de blob opnieuw nadat u wijzigingen aanbrengt.

## <a name="work-with-queue-resources"></a>Werken met resources van de wachtrij

Opslagwachtrijen services worden gehost in Azure storage-account. U kunt deze gebruiken om toe te staan uw cloud service-rollen om te communiceren met elkaar en met andere services door een mechanisme voor het doorgeven van berichten. U kunt de wachtrij via een programma openen via een cloudservice en via een webservice voor externe clients. U kunt ook toegang tot de wachtrij via Server Explorer in Visual Studio.

Wanneer u een cloudservice die gebruikmaakt van wachtrijen ontwikkelt, kunt u de Visual Studio gebruiken om te maken van wachtrijen en werken met ze interactief tijdens het ontwikkelen en testen van uw code.

In Server Explorer, kunt u de wachtrijen in een storage-account weergeven, maken en verwijderen van wachtrijen, opent u een wachtrij voor het weergeven van de berichten en berichten toevoegen aan een wachtrij. Wanneer u een wachtrij voor weergave opent, kunt u de afzonderlijke berichten bekijken en u kunt de volgende acties uitvoeren in de wachtrij met behulp van de knoppen in de linkerbovenhoek:

* Vernieuw de weergave van de wachtrij.
* Voeg een bericht toe aan de wachtrij.
* Het bovenste bericht uit de wachtrij verwijderen.
* De hele wachtrij wissen.

De volgende afbeelding ziet u een wachtrij met twee berichten:

![Een wachtrij bekijken](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Zie voor meer informatie over storage services wachtrijen, [aan de slag met Azure Queue storage met .NET](http://go.microsoft.com/fwlink/?LinkID=264702). Zie voor meer informatie over de webservice voor storage services-wachtrijen, [concepten van Queue-Service](http://go.microsoft.com/fwlink/?LinkId=264788). Zie voor informatie over het verzenden van berichten naar een opslagwachtrij voor services met behulp van Visual Studio, [berichten verzenden naar een Opslagwachtrij Services](https://docs.microsoft.com/azure/visual-studio/vs-storage-cloud-services-getting-started-queues).

> [!NOTE]
> Opslagwachtrijen services, verschilt van Azure Service Bus-wachtrijen. Zie voor meer informatie over Service Bus-wachtrijen, [Service Bus-wachtrijen, onderwerpen en abonnementen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-queues-topics-subscriptions).

## <a name="work-with-table-resources"></a>Werken met resources voor table

Met Azure Table Storage kunnen grote hoeveelheden gestructureerde gegevens worden opgeslagen. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen uit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.

### <a name="to-create-a-table"></a>Een tabel maken

1. Selecteer in de Cloud Explorer, de **tabellen** knooppunt van het storage-account en selecteer vervolgens **Create Table**.
1. In de **Create Table** dialoogvenster vak, voer een naam voor de tabel.

### <a name="to-view-table-data"></a>Om gegevens in een tabel weer te geven

1. Open in de Cloud Explorer, de **Azure** knooppunt en open vervolgens de **opslag** knooppunt.
1. Open het knooppunt van het storage-account dat u geïnteresseerd bent en open vervolgens de **tabellen** knooppunt om te zien van een lijst met tabellen voor het opslagaccount.
1. Open het snelmenu voor een tabel en selecteer vervolgens **tabel**.

    ![Een Azure-tabel in Solution Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

De tabel is geordend op entiteiten (weergegeven in rijen) en eigenschappen (die worden weergegeven in kolommen). De volgende afbeelding ziet u bijvoorbeeld entiteiten die worden vermeld in de ontwerpfunctie.

### <a name="to-edit-table-data"></a>Gegevens in een tabel bewerken

In de ontwerpfunctie, open het snelmenu voor een entiteit (één rij) of een eigenschap (één cel) en selecteer vervolgens **bewerken**.

    ![Add or edit a table entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

Entiteiten in één tabel zijn niet vereist om dezelfde set eigenschappen (kolommen). Houd rekening met de volgende beperkingen op weergeven en bewerken van gegevens in een tabel:

* U kunt weergeven of bewerken van binaire gegevens (`type byte[]`), maar kunt u deze opslaan in een tabel.
* U kunt niet bewerken de **PartitionKey** of **RowKey** waarden, omdat die bewerking biedt geen ondersteuning voor Table storage in Azure.
* U kunt geen maken een eigenschap genaamd **Timestamp**. Azure storage-services gebruiken een eigenschap met die naam.
* Als u een **datum-/** waarde, moet u een indeling die geschikt is voor de regio en taal-instellingen van uw computer (bijvoorbeeld MM/DD/JJJJ uu: mm: [AM | PM] voor Amerikaans-Engels).

### <a name="to-add-entities"></a>Entiteiten toevoegen

1. Selecteer in de ontwerpfunctie, de **entiteit toevoegen** knop.

    ![Knop entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. In de **entiteit toevoegen** dialoogvenster vak, voer de waarden van de **PartitionKey** en **RowKey** eigenschappen.

    ![Dialoogvenster entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Voer de waarden zorgvuldig. U kunt ze niet wijzigen nadat u het dialoogvenster sluiten, tenzij u de entiteit verwijderen en opnieuw toevoegen.

### <a name="to-filter-entities"></a>Voor het filteren van entiteiten

U kunt de set entiteiten die worden weergegeven in een tabel als u de opbouwfunctie voor query's aanpassen.

1. Als u wilt openen de opbouwfunctie voor query's, een tabel voor weergave te openen.
1. Selecteer de **opbouwfunctie voor query's** op de werkbalk van de tabelweergave.

    De **opbouwfunctie voor query's** in het dialoogvenster wordt weergegeven. De volgende afbeelding ziet een query die wordt gemaakt in de opbouwfunctie voor query's.

    ![Opbouwfunctie voor query's](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
1. Wanneer u klaar bent voor het bouwen van de query, sluit het dialoogvenster. De resulterende tekstvorm van de query in een tekstvak wordt weergegeven als een filter WCF Data Services.
1. De query wilt uitvoeren, selecteert u het pictogram groene driehoek.

U kunt ook filteren entiteitsgegevens die wordt weergegeven in de ontwerpfunctie als u een filtertekenreeks WCF Data Services rechtstreeks in het filtertekstvak. Dit soort tekenreeks is vergelijkbaar met een SQL WHERE-component maar naar de server als een HTTP-aanvraag is verzonden. Zie voor meer informatie over het maken van tekenreeksen [Constructing tekenreeksen voor de table designer](https://docs.microsoft.com/azure/vs-azure-tools-table-designer-construct-filter-strings).

De volgende afbeelding toont een voorbeeld van een ongeldig filtertekenreeks:

![Filtertekenreeks](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

## <a name="refresh-storage-data"></a>Vernieuwen van gegevens in de opslag

Als Server Explorer-verbinding met maakt of gegevens worden opgehaald uit een opslagaccount, kan de bewerking duren even om te voltooien. Als Server Explorer kan geen verbinding maken, kan de bewerking een time-out. Tijdens het ophalen van gegevens, kunt u blijven werken in andere onderdelen van Visual Studio. Als u wilt annuleren de bewerking als het te lang duurt, selecteer de **vernieuwen stoppen** op de werkbalk van de Server Explorer.

### <a name="to-refresh-blob-container-data"></a>Blob-containergegevens vernieuwen

* Selecteer de **Blobs** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de werkbalk van de Server Explorer.
* Als u wilt vernieuwen in de lijst met blobs die wordt weergegeven, selecteert u de **Execute** knop.

### <a name="to-refresh-table-data"></a>Vernieuwen van gegevens in een tabel

* Selecteer de **tabellen** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de werkbalk van de Server Explorer.
* Als u wilt vernieuwen in de lijst met entiteiten die in de ontwerpfunctie wordt weergegeven, selecteert u de **Execute** knop in de ontwerpfunctie.

### <a name="to-refresh-queue-data"></a>Om wachtrijgegevens te vernieuwen

Selecteer de **wachtrijen** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de werkbalk van de Server Explorer.

### <a name="to-refresh-all-items-in-a-storage-account"></a>Alle items in een storage-account vernieuwen

Kies de accountnaam en selecteer vervolgens de **vernieuwen** op de werkbalk van de Server Explorer.

## <a name="add-storage-accounts-by-using-server-explorer"></a>Storage-accounts toevoegen via Server Explorer

Er zijn twee manieren toevoegen van storage-accounts via Server Explorer. U kunt een storage-account maken in uw Azure-abonnement of u kunt een bestaand opslagaccount koppelen.

### <a name="to-create-a-storage-account-by-using-server-explorer"></a>Het maken van een storage-account via Server Explorer

1. Open in Server Explorer het snelmenu voor het **opslag** knooppunt en selecteert u vervolgens **Storage-Account maken**.

1. In de **Storage-Account maken** dialoogvenster selecteert of Voer de volgende informatie:

   * De Azure-abonnement dat u wilt de storage-account toevoegen.
   * De naam die u wilt gebruiken voor het nieuwe opslagaccount.
   * De regio of affiniteitsgroep (zoals VS-West of Zuidoost-Azië).
   * Het type van de replicatie die u wilt gebruiken voor de storage-account, zoals lokaal redundant.

   ![Een Azure-opslagaccount maken](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Selecteer **Maken**.

Het nieuwe opslagaccount wordt weergegeven in de **opslag** lijst in Solution Explorer.

### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Een bestaand opslagaccount koppelen via Server Explorer

1. Open in Server Explorer het snelmenu voor het Azure **opslag** knooppunt en selecteert u vervolgens **externe opslag koppelen**.

    ![Een bestaand opslagaccount toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
1. In de **Storage-Account maken** dialoogvenster selecteert of Voer de volgende informatie:

   * De naam van het bestaande storage-account dat u wilt koppelen.
   * De sleutel voor het geselecteerde opslagaccount. Deze waarde wordt meestal verzorgd voor u wanneer u een opslagaccount selecteren. Als u Visual Studio om te onthouden van de opslagaccountsleutel wilt, selecteert u de **onthouden accountsleutel** selectievakje.
   * Het protocol te gebruiken voor verbinding met de storage-account, zoals HTTP, HTTPS of een aangepast eindpunt. Zie voor meer informatie over aangepaste eindpunten [over het configureren van verbindingsreeksen](https://msdn.microsoft.com/library/azure/ee758697.aspx).

### <a name="to-view-the-secondary-endpoints"></a>Om de secundaire eindpunten weer te geven

Als u een opslagaccount met behulp van gemaakt de **Read-Access Geo Redundant** replicatieoptie, u kunt de secundaire eindpunten weergeven door het snelmenu voor de naam van het te openen en selecteer vervolgens **eigenschappen**.

![Secundaire eindpunten voor opslag](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Een storage-account verwijderen in Server Explorer

In Server Explorer het snelmenu voor de naam van het openen en selecteer vervolgens **verwijderen**. 

Als u een opslagaccount verwijdert, wordt er ook geen opgeslagen belangrijke gegevens voor dat account verwijderd.

U verwijdert een storage-account vanuit Server Explorer, heeft geen invloed op uw storage-account of alle gegevens die deze bevat. Het gewoon Hiermee verwijdert u de verwijzing in Server Explorer. U kunt een storage-account permanent verwijderen met de [Azure-portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure storage-services, [toegang tot de Azure Storage-Services](https://msdn.microsoft.com/library/azure/ee405490.aspx).

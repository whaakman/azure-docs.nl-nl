---
title: Bladeren en storage-resources beheren met behulp van de Server Explorer | Microsoft Docs
description: Bladeren en beheren van opslagbronnen met behulp van de Server Explorer
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/24/2017
ms.author: kraigb
ms.openlocfilehash: ee91ca168acf2fa0d248e18cce64ac546740a2bd
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="browse-and-manage-storage-resources-by-using-server-explorer"></a>Bladeren en storage-resources beheren met behulp van de Server Explorer

[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht

Als u Azure-hulpprogramma's voor Microsoft Visual Studio hebt geïnstalleerd, kunt u blobs, wachtrijen en tabelgegevens weergeven van uw storage-accounts voor Azure. De Azure **opslag** knooppunt in Server Explorer geeft gegevens in uw lokale emulator opslagaccount en andere Azure storage-accounts.

Als u wilt weergeven Server Explorer in Visual Studio op de menubalk, selecteer **weergave** > **Server Explorer**. De **opslag** knooppunt bevat alle van de storage-accounts bestaan onder elke Azure-abonnement of een certificaat dat u bent verbonden. Als uw storage-account niet wordt weergegeven, kunt u deze toevoegen Volg de instructies [verderop in dit artikel](#add-storage-accounts-by-using-server-explorer).

U start in de Azure SDK 2.7, kunt u ook gebruiken Cloud Explorer weergeven en beheren van uw Azure-resources. Zie voor meer informatie [het beheer van Azure-resources met Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md).

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Weergeven en beheren van opslagbronnen in Visual Studio

Server Explorer wordt automatisch een lijst met blobs, wachtrijen en tabellen in uw opslagaccount van de emulator. Het opslagaccount van de emulator wordt vermeld in Server Explorer onder de **opslag** -knooppunt bevindt als de **ontwikkeling** knooppunt.

Overzicht van de emulator van het opslagaccount resources, vouw de **ontwikkeling** knooppunt. Als de opslagemulator nog niet is begonnen, wanneer u de **ontwikkeling** knooppunt, wordt automatisch gestart. Dit kan enkele seconden duren. U kunt blijven werken in andere gebieden van Visual Studio, terwijl de opslagemulator wordt gestart.

Als u wilt weergeven in een opslagaccount, vouw de storage-account in Server Explorer waarin u zien hoe **Blobs**, **wachtrijen**, en **tabellen** knooppunten.

## <a name="work-with-blob-resources"></a>Werken met blob-bronnen

De **Blobs** knooppunt geeft een lijst met containers voor het geselecteerde opslagaccount. BLOB-containers blob-bestanden bevat en u kunt deze blobs indelen in mappen en submappen. Zie voor meer informatie [hoe Blob storage gebruiken met .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md).

### <a name="to-create-a-blob-container"></a>Een blob-container maken

1. Open het snelmenu voor het **Blobs** knooppunt en selecteer vervolgens **Blob-Container maken**.
1. In de **Blob-Container maken** dialoogvenster en voer de naam van de nieuwe container.  
1. Selecteer Enter op het toetsenbord, of u kunt Klik of tik buiten het naamveld op te slaan van de blob-container.

   > [!NOTE]
   > De naam van de blob-container moet beginnen met een cijfer (0-9) of kleine letter (a-z).

### <a name="to-delete-a-blob-container"></a>Verwijderen van een blob-container

Open het snelmenu voor de blob-container die u wilt verwijderen en selecteer **verwijderen**.

### <a name="to-display-a-list-of-the-items-in-a-blob-container"></a>Een lijst weergeven met de items in een blob-container

Open het snelmenu voor de naam van een blob-container in de lijst en selecteer vervolgens **Open**.

Wanneer u de inhoud van een blob-container bekijkt, wordt deze weergegeven op een tabblad bekend als de weergave van blob-container.

![Weergave van BLOB-container](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

U kunt de volgende bewerkingen op blobs uitvoeren met behulp van de knoppen in de rechterbovenhoek van de blob-Containerweergave:

* Voer een filterwaarde en deze toe te passen.
* Vernieuw de lijst met blobs in de container.
* Bestand uploaden.
* Verwijderen van een blob. (Een bestand van een blob-container niet verwijdert, het onderliggende bestand. Deze alleen verwijderd uit de blob-container.)
* Open een blob.
* Sla een blob naar de lokale computer.

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Een map of submap maken in een blob-container

1. Kies de blob-container in Cloud Explorer. Selecteer in het venster container de **Blob uploaden** knop.

1. In de **nieuwe uploaden** selecteert u de **Bladeren** klikken om het bestand opgeeft dat u wilt uploaden en voer vervolgens de naam van een map in de **map (optioneel)** vak.

   ![Uploaden van een bestand naar een blob-map](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

   U kunt submappen in container-mappen toevoegen door dezelfde stap te volgen. Als u geen naam van een map opgeeft, wordt het bestand geüpload naar het hoogste niveau van de blob-container. Het bestand wordt weergegeven in de opgegeven map in de container.

   ![Map toevoegen aan een blob-container](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Dubbelklik op de map of selecteer Enter om te zien van de inhoud van de map. Wanneer u zich in de container, map, u kunt teruggaan naar de hoofdmap van de container door het selecteren van de **openen van de bovenliggende map** (pijl).

### <a name="to-delete-a-container-folder"></a>Een container map verwijderen

Verwijder alle bestanden in de map.

Omdat in de blob-containers virtuele mappen zijn, kunt u een lege map kan niet maken. U ook een map voor het verwijderen van de bestandsinhoud kan niet worden verwijderd, maar moet in plaats daarvan de volledige inhoud van een map voor het verwijderen van de map zelf verwijderen.

### <a name="to-filter-blobs-in-a-container"></a>Blobs in een container filteren

U kunt de blobs die worden weergegeven door te geven van een algemeen voorvoegsel filteren.

Bijvoorbeeld, als u het voorvoegsel invoeren **hello** in het tekstvak en selecteer vervolgens de **Execute** (**!**) knop, alleen de blobs die met 'Hallo beginnen' wordt weergegeven.

![In het tekstvak filteren](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

Het filtertekstvak is hoofdlettergevoelig en biedt geen ondersteuning voor filteren met jokertekens bevatten. BLOBs kunnen alleen op voorvoegsel worden gefilterd. Het voorvoegsel kan een scheidingsteken bevatten als u een scheidingsteken te organiseren blobs in een virtuele-hiërarchie. Bijvoorbeeld, filteren op het voorvoegsel ' HelloFabric / ' retourneert alle blobs die met deze reeks beginnen.

### <a name="to-download-blob-data"></a>Voor het downloaden van blob-gegevens

In de Cloud Explorer, moet u een van de volgende methoden gebruiken:

* Open het snelmenu voor een of meer blobs en selecteer vervolgens **Open**.
* Kies de blob-naam en selecteer vervolgens de **Open** knop.
* Dubbelklik op de blob-naam.

De voortgang van een blob-download wordt weergegeven in de **Azure Activity Log** venster.

De blob wordt geopend in de editor voor dat bestandstype. Als het besturingssysteem het bestandstype herkent, wordt het bestand wordt geopend in een lokaal geïnstalleerde toepassing. Anders wordt u gevraagd kiest u een toepassing die geschikt is voor het type van de blob. Het lokale bestand dat wordt gemaakt wanneer het downloaden van een blob is gemarkeerd als alleen-lezen.

BLOB-gegevens wordt lokaal opgeslagen in de cache en gecontroleerd met de blob tijd laatste wijziging in Azure Blob-opslag. Als de blob is bijgewerkt sinds het laatst is gedownload, wordt deze opnieuw gedownload. Anders wordt de blob geladen vanaf de lokale schijf.

Standaard wordt een blob naar een tijdelijke map gedownload. Om blobs te downloaden naar een bepaalde map, open het snelmenu voor de geselecteerde blob-namen en selecteer **OpslaanAls**. Wanneer u een blob op deze manier opslaat, het blob-bestand niet is geopend en het lokale bestand is gemaakt met kenmerken lezen/schrijven.

### <a name="to-upload-blobs"></a>Voor het uploaden van BLOB 's

Als u wilt uploaden blobs, selecteer de **Blob uploaden** wanneer de container is geopend voor weergave in de weergave van blob-container.

U kunt een of meer bestanden te uploaden en u kunt bestanden van elk type uploaden. De **Azure Activity Log** venster wordt de voortgang van het uploaden. Zie voor meer informatie over het werken met blob-gegevens [Azure Blob storage gebruiken in .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Logboeken overgebracht naar BLOB's weergeven

Als u Azure Diagnostics gebruikt om gegevens te registreren van uw Azure-toepassing en u logboeken naar uw opslagaccount hebt overgebracht, ziet u containers die Azure voor deze logboeken worden gemaakt. Deze logboeken weergeven in Server Explorer is een eenvoudige manier om u te identificeren van problemen met uw toepassing vooral als deze is geïmplementeerd in Azure.

Zie voor meer informatie over Azure Diagnostics [Logboekregistratiegegevens verzamelen met behulp van Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>De URL voor een blob ophalen

De blob-snelmenu openen en selecteer vervolgens **kopie URL**.

### <a name="to-edit-a-blob"></a>Een blob bewerken

Selecteer de blob en selecteer vervolgens de **Open Blob** knop.

Het bestand is gedownload naar een tijdelijke locatie en geopend op de lokale computer. Uploaden van de blob opnieuw nadat u wijzigingen aanbrengt.

## <a name="work-with-queue-resources"></a>Werken met resources van de wachtrij

Opslagwachtrijen services worden gehost in Azure storage-account. U kunt deze gebruiken om toe te staan van uw cloud service-rollen communiceren met elkaar en met andere services door een mechanisme voor het doorgeven van berichten. U kunt de wachtrij programmatisch benaderen via een cloudservice en via een webservice voor externe clients. U kunt ook toegang tot de wachtrij rechtstreeks via Server Explorer in Visual Studio.

Wanneer u een cloudservice die gebruikmaakt van wachtrijen ontwikkelt, is het raadzaam Visual Studio gebruiken voor het maken van wachtrijen en interactief te kunnen werken met hen tijdens het ontwikkelen en testen van uw code.

In Server Explorer kunt u de wachtrijen weergeven in een opslagaccount, maken en verwijderen van wachtrijen, opent u een wachtrij om weer te geven van de berichten en berichten toevoegen aan een wachtrij. Wanneer u een wachtrij voor weergave opent, kunt u de afzonderlijke berichten bekijken en u kunt de volgende acties uitvoeren in de wachtrij met behulp van de knoppen in de linkerbovenhoek:

* Vernieuw de weergave van de wachtrij.
* Voeg een bericht toe aan de wachtrij.
* Het bovenste bericht uit de wachtrij halen.
* Hiermee schakelt u de volledige wachtrij.

De volgende afbeelding ziet u een wachtrij met twee berichten:

![Een wachtrij weergeven](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Zie voor meer informatie over opslag services wachtrijen, [aan de slag met Azure Queue storage met .NET](http://go.microsoft.com/fwlink/?LinkID=264702). Zie voor informatie over de webservice voor storage services wachtrijen, [concepten van Queue-Service](http://go.microsoft.com/fwlink/?LinkId=264788). Zie voor meer informatie over het verzenden van berichten naar een wachtrij storage-services met behulp van Visual Studio [berichten verzenden naar een wachtrij Storage-Services](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> Storage services wachtrijen zijn verschillend van Azure Service Bus-wachtrijen. Zie voor meer informatie over Service Bus-wachtrijen [Service Bus-wachtrijen, onderwerpen en abonnementen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-queues-topics-subscriptions).

## <a name="work-with-table-resources"></a>Werken met resources van de tabel

Met Azure Table Storage kunnen grote hoeveelheden gestructureerde gegevens worden opgeslagen. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen vanuit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.

### <a name="to-create-a-table"></a>Een tabel maken

1. Selecteer in de Cloud Explorer de **tabellen** knooppunt van het storage-account en selecteer **Create Table**.
1. In de **Create Table** dialoogvenster een naam voor de tabel.

### <a name="to-view-table-data"></a>Tabelgegevens weergeven

1. Open in de Cloud Explorer de **Azure** knooppunt en open vervolgens de **opslag** knooppunt.
1. Open het storage-account-knooppunt dat u bent geïnteresseerd en open vervolgens de **tabellen** knooppunt om een lijst weer van de tabellen voor het opslagaccount.
1. Open het snelmenu voor een tabel en selecteer vervolgens **tabel**.

    ![Een Azure-tabel in Solution Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

De tabel is geordend op entiteiten (weergegeven in rijen) en eigenschappen (weergegeven in de kolommen). De volgende afbeelding ziet u bijvoorbeeld entiteiten die worden vermeld in de ontwerpfunctie.

### <a name="to-edit-table-data"></a>Tabelgegevens bewerken

In de ontwerpfunctie, open het snelmenu voor een entiteit (één rij) of een eigenschap (één cel) en selecteer vervolgens **bewerken**.

    ![Add or edit a table entity](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

Entiteiten in één tabel zijn niet beschikken over dezelfde reeks eigenschappen (kolommen). Houd rekening met de volgende beperkingen voor de tabelgegevens weergeven en bewerken:

* U kunt weergeven of bewerken van binaire gegevens (`type byte[]`), maar kunt u deze opslaan in een tabel.
* Kan niet worden bewerkt de **PartitionKey** of **RowKey** waarden, omdat in Azure-tabelopslag biedt geen ondersteuning voor deze bewerking.
* U kunt geen maken een eigenschap genaamd **tijdstempel**. Azure storage-services gebruiken een eigenschap met die naam.
* Als u een **DateTime** waarde, moet u een indeling die geschikt is voor de regio en taal-instellingen van uw computer volgen (bijvoorbeeld: DD-MM-jjjj: mm: ss [AM | PM] voor Amerikaans Engels).

### <a name="to-add-entities"></a>Entiteiten toevoegen

1. Selecteer in de ontwerpfunctie, de **entiteit toevoegen** knop.

    ![Knop entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. In de **entiteit toevoegen** dialoogvenster en voer de waarden van de **PartitionKey** en **RowKey** eigenschappen.

    ![Dialoogvenster entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Voer de waarden zorgvuldig. U kunt ze niet wijzigen nadat u het dialoogvenster hebt gesloten, tenzij u de entiteit verwijderen en opnieuw toevoegen.

### <a name="to-filter-entities"></a>Voor het filteren van entiteiten

De reeks entiteiten die worden weergegeven in een tabel als u de opbouwfunctie voor query's gebruikt, kunt u aanpassen.

1. Als u wilt de opbouwfunctie voor query's openen, opent u een tabel om weer te geven.
1. Selecteer de **opbouwfunctie voor Query's** op de werkbalk van de tabelweergave.

    De **opbouwfunctie voor Query's** dialoogvenster wordt weergegeven. De volgende afbeelding ziet een query die wordt samengesteld in de opbouwfunctie voor query's.

    ![Opbouwfunctie voor query 's](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
1. Wanneer u klaar bent voor het bouwen van de query en sluit het dialoogvenster. De resulterende tekst van de query in een tekstvak wordt weergegeven als een filter WCF Data Services.
1. De query wilt uitvoeren, selecteer het pictogram groene driehoek.

U kunt ook filteren entiteitsgegevens die wordt weergegeven in de ontwerpfunctie als u een filtertekenreeks in WCF Data Services rechtstreeks in het filtertekstvak. Dit soort tekenreeks is vergelijkbaar met een SQL WHERE-component maar naar de server als een HTTP-aanvraag is verzonden. Zie voor meer informatie over het maken van filtertekenreeksen [construeren van tekenreeksen voor de ontwerpfunctie voor tabellen](https://msdn.microsoft.com/library/azure/ff683669.aspx).

De volgende afbeelding toont een voorbeeld van een geldig filter-tekenreeks:

![Filtertekenreeks](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

## <a name="refresh-storage-data"></a>Storage-gegevens vernieuwen

Als Server Explorer verbinding met maakt of gegevens zijn afkomstig uit een opslagaccount, kan de bewerking duren een minuut om te voltooien. Als Server Explorer kan geen verbinding maken, kan de bewerking time-out. Tijdens het ophalen van gegevens, kunt u blijven werken in andere onderdelen van Visual Studio. Om de bewerking annuleert, als dit te lang duurt, selecteer de **vernieuwen annuleren** op de Server Explorer-werkbalk.

### <a name="to-refresh-blob-container-data"></a>Blob-containergegevens vernieuwen

* Selecteer de **Blobs** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de Server Explorer-werkbalk.
* Als u wilt vernieuwen in de lijst met blobs die wordt weergegeven, selecteert u de **Execute** knop.

### <a name="to-refresh-table-data"></a>Vernieuwen van gegevens in een tabel

* Selecteer de **tabellen** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de Server Explorer-werkbalk.
* Als u wilt vernieuwen in de lijst met entiteiten die wordt weergegeven in de ontwerpfunctie, selecteer de **Execute** knop in de ontwerpfunctie.

### <a name="to-refresh-queue-data"></a>Wachtrijgegevens wilt vernieuwen

Selecteer de **wachtrijen** knooppunt onder **opslag**, en selecteer vervolgens de **vernieuwen** op de Server Explorer-werkbalk.

### <a name="to-refresh-all-items-in-a-storage-account"></a>Alle items in een opslagaccount vernieuwen

Kies de accountnaam en selecteer vervolgens de **vernieuwen** op de Server Explorer-werkbalk.

## <a name="add-storage-accounts-by-using-server-explorer"></a>Storage-accounts toevoegen met behulp van Server Explorer

Er zijn twee manieren storage-accounts toevoegen met behulp van Server Explorer. U kunt een opslagaccount maken in uw Azure-abonnement of kunt u een bestaand opslagaccount koppelen.

### <a name="to-create-a-storage-account-by-using-server-explorer"></a>Een opslagaccount maken met behulp van Server Explorer

1. Open in Server Explorer het snelmenu voor het **opslag** knooppunt en selecteer vervolgens **Storage-Account maken**.

1. In de **Storage-Account maken** dialoogvenster selecteert of Voer de volgende informatie:

   * De Azure-abonnement dat u wilt toevoegen de storage-account.
   * De naam die u wilt gebruiken voor het nieuwe opslagaccount.
   * De regio of affiniteitsgroep (zoals VS-West of Oost-Azië).
   * Het type van de replicatie die u wilt gebruiken voor het opslagaccount, zoals lokaal redundant.

   ![Een Azure-opslagaccount maken](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Selecteer **Maken**.

Het nieuwe opslagaccount wordt weergegeven in de **opslag** lijst in Solution Explorer.

### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Koppelen van een bestaand opslagaccount met behulp van de Server Explorer

1. Open in Server Explorer het snelmenu voor het Azure **opslag** knooppunt en selecteer vervolgens **externe opslag koppelen**.

    ![Een bestaand opslagaccount toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
1. In de **Storage-Account maken** dialoogvenster selecteert of Voer de volgende informatie:

   * De naam van het bestaande account voor de opslag die u wilt koppelen.
   * De sleutel voor het geselecteerde opslagaccount. Deze waarde wordt meestal verzorgd door voor u wanneer u een opslagaccount selecteren. Als u wilt dat Visual Studio te onthouden van de sleutel van het opslagaccount, selecteert u de **accountsleutel onthouden** selectievakje.
   * Het protocol te gebruiken voor verbinding met de storage-account, zoals HTTP, HTTPS of een aangepaste eindpunt. Zie voor meer informatie over aangepaste eindpunten [-verbindingsreeksen configureren hoe](https://msdn.microsoft.com/library/azure/ee758697.aspx).

### <a name="to-view-the-secondary-endpoints"></a>De secundaire eindpunten weergeven

Als u een opslagaccount hebt gemaakt met behulp van de **Read-Access Geo-Redundant** replicatieoptie, u kunt de secundaire eindpunten weergeven door het openen van het snelmenu voor de accountnaam en selecteer vervolgens **eigenschappen**.

![Secundaire opslag-eindpunten](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Verwijderen van een opslagaccount in Server Explorer

Open het snelmenu voor de accountnaam in Server Explorer en selecteer vervolgens **verwijderen**. 

Als u een opslagaccount verwijdert, worden alle opgeslagen sleutelgegevens voor dat account wordt ook verwijderd.

Als u een opslagaccount in Server Explorer verwijdert, dat geen gevolgen voor uw opslagaccount of gegevens die deze bevat. De verwijzing wordt alleen verwijderd in Server Explorer. U een opslagaccount permanent te verwijderen met de [Azure-portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure storage-services, [toegang tot de Azure Storage-Services](https://msdn.microsoft.com/library/azure/ee405490.aspx).

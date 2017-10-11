---
title: Toepassingspakketten op rekenknooppunten - Azure Batch installeren | Microsoft Docs
description: Gebruik de functie voor de toepassing-pakketten van Azure Batch eenvoudig beheren meerdere toepassingen en versies voor de installatie van Batch-rekenknooppunten.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: afcc04c80ec15872a22de5d5969a7ef6a583562f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Toepassingen implementeren op rekenknooppunten met Batch-toepassingspakketten

De functie voor de toepassing-pakketten van Azure Batch biedt eenvoudig beheer van toepassingen van de taak en de implementatie ervan in de rekenknooppunten in uw groep. U kunt met toepassingspakketten kunt uploaden en beheren van meerdere versies van de toepassingen die de taken worden uitgevoerd, met inbegrip van hun ondersteunende bestanden. U kunt vervolgens automatisch implementeren een of meer van deze toepassingen aan de rekenknooppunten in uw groep.

In dit artikel leert u hoe uploaden en beheren in de Azure portal-toepassingspakketten. Vervolgens leert u hoe u ze moet installeren op een pool van rekenknooppunten met de [Batch .NET] [ api_net] bibliotheek.

> [!NOTE]
> 
> Toepassingspakketten worden ondersteund in alle Batch-pools die na 5 juli 2017 zijn gemaakt. De pakketten worden ondersteund in Batch-pools die zijn gemaakt tussen 10 maart 2016 en 5 juli 2017, maar alleen als de pool is gemaakt met behulp van een cloudservice-configuratie. Batch-pools die zijn gemaakt vóór 10 maart 2016 bieden geen ondersteuning voor toepassingspakketten.
>
> De API's voor het maken en beheren van toepassingspakketten deel uitmaken van de [Batch Management .NET] [[api_net_mgmt]] bibliotheek. De API's voor toepassingspakketten installeren op een rekenknooppunt deel uitmaken van de [Batch .NET] [ api_net] bibliotheek.  
>
> De toepassing pakketten functie hier beschreven vervangt de beschikbaar in eerdere versies van de service Batch Apps-functie.
> 
> 

## <a name="application-package-requirements"></a>Vereisten voor Application-pakket
Voor het gebruik van toepassingspakketten, moet u [een Azure Storage-account koppelen](#link-a-storage-account) aan uw Batch-account.

Deze functie is geïntroduceerd [Batch REST-API] [ api_rest] versie 2015-12-01.2.2 en de bijbehorende [Batch .NET] [ api_net] library-versie 3.1.0. Het is raadzaam dat u de nieuwste API-versie altijd gebruiken bij het werken met de Batch.

> [!NOTE]
> Toepassingspakketten worden ondersteund in alle Batch-pools die na 5 juli 2017 zijn gemaakt. De pakketten worden ondersteund in Batch-pools die zijn gemaakt tussen 10 maart 2016 en 5 juli 2017, maar alleen als de pool is gemaakt met behulp van een cloudservice-configuratie. Batch-pools die zijn gemaakt vóór 10 maart 2016 bieden geen ondersteuning voor toepassingspakketten.
>
>

## <a name="about-applications-and-application-packages"></a>Over de toepassingen en toepassingspakketten
In Azure Batch een *toepassing* verwijst naar een set van samengestelde binaire bestanden gebruikt die automatisch kan worden gedownload op de rekenknooppunten in uw groep. Een *toepassingspakket* verwijst naar een *specifieke set* van deze binaire bestanden en vertegenwoordigt een gegeven *versie* van de toepassing.

![Op hoog niveau diagram van toepassingen en toepassingspakketten][1]

### <a name="applications"></a>Toepassingen
Een toepassing in Batch bevat een of meer toepassingen, pakketten en configuratieopties voor de toepassing bevat. Een toepassing kunt bijvoorbeeld opgeven dat de standaardversie voor het pakket van toepassing te installeren op de rekenknooppunten en of de pakketten kunnen worden bijgewerkt of verwijderd.

### <a name="application-packages"></a>Toepassingspakketten
Een toepassingspakket is een ZIP-bestand met de binaire bestanden van de toepassing en de ondersteunende bestanden die vereist zijn voor uw taken de toepassing uit te voeren. Elke toepassingspakket vertegenwoordigt een specifieke versie van de toepassing.

U kunt toepassingspakketten op het niveau van toepassingen en de taak opgeven. U kunt een of meer van deze pakketten en (optioneel) een versie opgeven wanneer u een groep of een taak maakt.

* **Toepassingspakketten groep** zijn geïmplementeerd op *elke* knooppunt in de pool. Toepassingen worden geïmplementeerd als een knooppunt aan een pool wordt toegevoegd en wanneer deze wordt opgestart of hersteld met een installatiekopie.
  
    Toepassingspakketten voor toepassingen zijn geschikt wanneer alle knooppunten in een pool van een taak taken uitvoeren. Een of meer toepassingspakketten kunt u opgeven wanneer u een pool maakt en u kunt toevoegen of bijwerken van een bestaande pool-pakketten. Als u een bestaande pool toepassingspakketten bijwerkt, moet u de knooppunten voor de installatie van het nieuwe pakket opnieuw.
* **Taak toepassingspakketten** zijn alleen geïmplementeerd op een rekenknooppunt uitgevoerd van een taak, net voordat de opdrachtregel van de taak wordt uitgevoerd. Als de opgegeven toepassingspakket en de versie is al op het knooppunt, niet opnieuw gedistribueerd en het bestaande pakket wordt gebruikt.
  
    Taak toepassingspakketten zijn handig in gedeelde groep omgevingen, waarbij verschillende taken worden uitgevoerd op één groep, en de groep is niet verwijderd wanneer een taak is voltooid. Als de job minder taken dan knooppunten in de groep heeft, kunnen toepassingspakketten van taken gegevensoverdracht minimaliseren omdat uw toepassing alleen wordt geïmplementeerd op de knooppunten die taken uitvoeren.
  
    Andere scenario's waarin u van de taak toepassingspakketten profiteren kunnen zijn taken met een grote toepassing, maar voor alleen enkele taken. Een vooraf verwerken fase of een merge-taak, waarbij de toepassing vooraf verwerken of het samenvoegen van zware is, kan bijvoorbeeld profiteren van het gebruik van de taak toepassingspakketten.

> [!IMPORTANT]
> Er gelden beperkingen op het aantal toepassingen en toepassingspakketten in een Batch-account en de maximale grootte van pakket. Zie [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over deze limieten.
> 
> 

### <a name="benefits-of-application-packages"></a>Voordelen van toepassingspakketten
Toepassingspakketten kunnen vereenvoudigen de code in uw Batch-oplossing, en verlagen de overhead voor het beheren van de toepassingen die de taken worden uitgevoerd.

Met toepassingspakketten kunt geen begintaak uw pool een lange lijst met afzonderlijke bronbestanden installeren op de knooppunten opgeven. U hoeft niet te handmatig beheer van meerdere versies van uw toepassingsbestanden in Azure Storage of op de knooppunten. En u hoeft niet te hoeven maken over het genereren van [SAS-URL's](../storage/common/storage-dotnet-shared-access-signature-part-1.md) voor toegang tot de bestanden in uw opslagaccount. Batch werkt op de achtergrond met Azure Storage-toepassingspakketten opslaan en deze rekenknooppunten implementeren.

> [!NOTE] 
> De totale grootte van een begintaak moet kleiner zijn dan of gelijk zijn aan 32.768 tekens, inclusief bronbestanden en omgevingsvariabelen. Als uw begintaak deze limiet overschrijdt, is met behulp van toepassingspakketten een andere optie. U kunt ook maken een ZIP-archief met de bronbestanden, uploaden als een blob naar Azure Storage en pak deze vervolgens vanaf de opdrachtregel van de begintaak. 
>
>

## <a name="upload-and-manage-applications"></a>Uploaden en beheren van toepassingen
U kunt de [Azure-portal] [ portal] of de [Batch Management .NET](batch-management-dotnet.md) bibliotheek voor het beheren van de toepassingspakketten in uw Batch-account. In de volgende secties laten we eerst zien hoe een opslagaccount te koppelen en vervolgens bespreken toe te voegen toepassingen en pakketten en deze te beheren met de portal.

### <a name="link-a-storage-account"></a>Storage-account koppelen
Voor het gebruik van toepassingspakketten, moet u eerst een Azure Storage-account koppelen aan uw Batch-account. Als u een opslagaccount nog niet hebt geconfigureerd, de Azure-portal wordt weergegeven een waarschuwing de eerste keer dat u op de **toepassingen** -tegel in de **Batch-account** blade.

> [!IMPORTANT]
> Batch ondersteunt momenteel *alleen* de **algemeen** opslagaccounttype zoals beschreven in stap 5, [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account)in [over Azure Storage-accounts](../storage/common/storage-create-storage-account.md). Wanneer u een Azure Storage-account aan uw Batch-account koppelt, een koppeling *alleen* een **algemeen** storage-account.
> 
> 

!['Er is geen opslagaccount geconfigureerd' waarschuwing in Azure-portal][9]

De Batch-service gebruikt het bijbehorende opslagaccount voor het opslaan van uw toepassingspakketten. Nadat u de twee accounts hebt gekoppeld, kan de pakketten die zijn opgeslagen in de gekoppelde Storage-account aan uw rekenknooppunten automatisch implementeren door Batch. Als u wilt een Storage-account koppelen aan uw Batch-account, klikt u op **instellingen voor de opslag** op de **waarschuwing** blade en klik vervolgens op **Opslagaccount** op de  **Storage-Account** blade.

![Kies de blade opslagaccount in Azure-portal][10]

Het is raadzaam dat u een opslagaccount maken *specifiek* voor gebruik met uw Batch-account en selecteert u deze hier. Zie voor meer informatie over het maken van een opslagaccount 'Een opslagaccount maken' [over Azure Storage-accounts](../storage/common/storage-create-storage-account.md). Nadat u een opslagaccount hebt gemaakt, kunt u vervolgens deze koppelen aan uw Batch-account met behulp van de **Opslagaccount** blade.

> [!WARNING]
> De Batch-service gebruikt Azure Storage voor het opslaan van uw toepassingspakketten als blok-blobs. U bent [in rekening gebracht als normale] [ storage_pricing] voor de blok-blob-gegevens. Moet u rekening met de grootte en het nummer van uw toepassingspakketten en verwijderen periodiek verouderde pakketten om de kosten kunt minimaliseren.
> 
> 

### <a name="view-current-applications"></a>De huidige toepassingen weergeven
U kunt de toepassingen in uw Batch-account op de **toepassingen** menu-item in het menu links tijdens weer te geven de **Batch-account** blade.

![Toepassingen-tegel][2]

Met deze optie menu opent de **toepassingen** blade:

![Lijst met toepassingen][3]

De **toepassingen** blade geeft de ID van elke toepassing in uw account en de volgende eigenschappen:

* **Pakketten**: het aantal versies die zijn gekoppeld aan deze toepassing.
* **Standaardversie**: de versie van de toepassing wordt geïnstalleerd als u niet een versie aangeven wanneer u de toepassing voor een groep opgeeft. Deze instelling is optioneel.
* **Toestaan dat updates**: de waarde die aangeeft of het pakket updates, verwijderingen en toevoegingen zijn toegestaan. Als deze is ingesteld op **Nee**, pakket bijwerken en verwijderen zijn uitgeschakeld voor de toepassing. Alleen nieuwe toepassingspakketversies kunnen worden toegevoegd. De standaardwaarde is **Ja**.

### <a name="view-application-details"></a>Toepassingdetails weergeven
Als de blade met de details voor een toepassing, schakelt u de toepassing in de **toepassingen** blade.

![App-details][4]

In de blade toepassing details kunt u de volgende instellingen configureren voor uw toepassing.

* **Toestaan dat updates**: opgeven of de toepassingspakketten kunnen worden bijgewerkt of verwijderd. Zie 'Werken of te verwijderen van een toepassingspakket' verderop in dit artikel.
* **Standaardversie**: Geef een toepassingspakket standaard voor het implementeren van rekenknooppunten.
* **Weergavenaam**: Geef een beschrijvende naam die uw Batch-oplossing gebruiken kunt wanneer er bijvoorbeeld informatie over de toepassing weergegeven in de gebruikersinterface van een service die u voor uw klanten via Batch opgeeft.

### <a name="add-a-new-application"></a>Een nieuwe toepassing toevoegen
Een nieuwe toepassing maken, toevoegen van een toepassingspakket en geef een nieuwe, unieke toepassing-ID. Het eerste toepassingspakket dat u met de nieuwe toepassings-ID toevoegen wordt ook de nieuwe toepassing gemaakt.

Klik op **toevoegen** op de **toepassingen** blade openen de **nieuwe toepassing** blade.

![Blade voor een nieuwe toepassing in Azure-portal][5]

De **nieuwe toepassing** blade biedt de volgende velden om op te geven van de instellingen van uw nieuwe toepassing en het application-pakket.

**Toepassings-id**

Dit veld bevat de ID van uw nieuwe toepassing, onderworpen aan de standaardregels voor validatie van Azure Batch-ID is. De regels voor het ontwikkelen van een toepassings-ID zijn als volgt:

* Op Windows-knooppunten, kan de ID een combinatie van alfanumerieke tekens, afbreekstreepjes en onderstrepingstekens bevatten. Op Linux-knooppunten mogen alleen alfanumerieke tekens en onderstrepingstekens bevatten.
* Kan niet meer dan 64 tekens bevatten.
* Moet uniek zijn binnen het Batch-account.
* Letters behouden blijven en hoofdlettergevoelig is.

**Versie**

Dit veld geeft u de versie van het toepassingspakket dat u uploadt. Versietekenreeksen zijn onderworpen aan de volgende validatieregels:

* Op Windows-knooppunten, kan de tekenreeks voor elke combinatie van alfanumerieke tekens, streepjes, onderstrepingstekens en punten bevatten. Op Linux-knooppunten mag de versietekenreeks alleen alfanumerieke tekens en onderstrepingstekens bevatten.
* Kan niet meer dan 64 tekens bevatten.
* Moet uniek zijn binnen de toepassing.
* Letters behouden blijven en hoofdlettergevoelig zijn.

**Application-pakket**

Dit veld geeft de ZIP-bestand dat de binaire bestanden van de toepassing bevat en de ondersteunende bestanden die nodig zijn voor de toepassing uitvoeren. Klik op de **selecteert u een bestand** box of het pictogram van de map te bladeren naar en selecteer een ZIP-bestand met de bestanden van uw toepassing.

Nadat u een bestand hebt geselecteerd, klikt u op **OK** om te beginnen met het uploaden naar Azure Storage. Wanneer het uploaden voltooid is, wordt de portal een melding weergegeven en sluit u de blade. Deze bewerking kan enige tijd duren, afhankelijk van de grootte van het bestand dat u uploadt en de snelheid van uw netwerkverbinding.

> [!WARNING]
> Sluit niet de **nieuwe toepassing** blade voordat het uploaden voltooid is. In dat geval wordt het uploadproces gestopt.
> 
> 

### <a name="add-a-new-application-package"></a>Een nieuw toepassingspakket toevoegen
Selecteer een toepassing in om een nieuwe Pakketversie voor de toepassing van een bestaande toepassing toe de **toepassingen** blade, klikt u op **pakketten**, klikt u vervolgens op **toevoegen** openen de **Toevoegen pakket** blade.

![Toepassing pakket blade toevoegen in Azure-portal][8]

Zoals u ziet de velden overeenkomen met die van de **nieuwe toepassing** blade, maar de **toepassings-id** selectievakje is uitgeschakeld. Net als voor de nieuwe toepassing, geef de **versie** voor het nieuwe pakket, blader naar uw **toepassingspakket** ZIP-bestand en klik vervolgens op **OK** voor het uploaden van het pakket.

### <a name="update-or-delete-an-application-package"></a>Bijwerken of verwijderen van een toepassingspakket
Als u wilt bijwerken of verwijderen van een bestaand toepassingspakket, open de blade met details voor de toepassing, klikt u op **pakketten** openen de **pakketten** blade, klikt u op de **weglatingsteken** in de rij van het toepassingspakket dat u wilt wijzigen, en selecteer de actie die u wilt uitvoeren.

![Bijwerken of verwijderen van pakket in Azure-portal][7]

**Update**

Wanneer u klikt op **Update**, wordt de *updatepakket* blade wordt weergegeven. Deze blade is vergelijkbaar met de *nieuw toepassingspakket* blade echter alleen het veld pakket is ingeschakeld, zodat u kunt het opgeven van een nieuwe ZIP-bestand te uploaden.

![Blade voor update-pakket in Azure-portal][11]

**Verwijderen**

Wanneer u klikt op **verwijderen**u wordt gevraagd om te bevestigen dat de verwijdering van de Pakketversie en Batch wordt het pakket verwijderd uit Azure Storage. Als u de standaardversie van een toepassing, verwijdert de **standaardversie** instelling voor de toepassing is verwijderd.

![Toepassing verwijderen][12]

## <a name="install-applications-on-compute-nodes"></a>Toepassingen installeren op rekenknooppunten
Nu dat u met de Azure portal-toepassingspakketten beheren hebt geleerd, kunt implementeren om de rekenknooppunten en ze worden uitgevoerd met Batch-taken besproken.

### <a name="install-pool-application-packages"></a>Toepassingspakketten voor toepassingen installeren
Geef een of meer toepassingspakket wilt installeren een toepassingspakket op alle rekenknooppunten in een pool, *verwijzingen* voor de pool. De toepassingspakketten die u voor een groep van toepassingen opgeeft zijn geïnstalleerd op elk rekenknooppunt wanneer dat knooppunt aan de pool wordt toegevoegd en wanneer het knooppunt is opnieuw opgestart of hersteld met een installatiekopie.

Geef een of meer in Batch .NET [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] wanneer u een nieuwe groep maakt of voor een bestaande toepassingen. De [ApplicationPackageReference] [ net_pkgref] klasse wordt een toepassings-ID en versie te installeren op een pool van rekenknooppunten.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Als de implementatie van een toepassing pakket om welke reden dan ook mislukt, de Batch-service het knooppunt markeert [onbruikbaar][net_nodestate], en er zijn geen taken zijn gepland voor uitvoering op dat knooppunt. In dit geval moet u **opnieuw** het knooppunt voor de pakketimplementatie van het opnieuw starten. Opnieuw opstarten van het knooppunt kan ook taakplanning opnieuw op het knooppunt.
> 
> 

### <a name="install-task-application-packages"></a>Taak toepassingspakketten installeren
Net als bij een groep kunt u opgeven toepassingspakket *verwijzingen* voor een taak. Wanneer een taak is gepland op een knooppunt worden uitgevoerd, wordt het pakket gedownload en uitgepakt vlak voordat de opdrachtregel van de taak wordt uitgevoerd. Als een opgegeven pakket en -versie is al geïnstalleerd op het knooppunt, wordt het pakket wordt niet gedownload en wordt het bestaande pakket wordt gebruikt.

Configureren van de taak voor het installeren van een toepassingspakket taak [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] eigenschap:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>De geïnstalleerde toepassingen uitvoeren
De pakketten die u hebt opgegeven voor een groep of de taak worden gedownload en uitgepakt naar de map met een naam in de `AZ_BATCH_ROOT_DIR` van het knooppunt. Batch maakt ook een omgevingsvariabele die het pad naar de map met de naam bevat. Uw taak opdrachtregels gebruikt deze omgevingsvariabele bij verwijzingen naar de toepassing op het knooppunt. 

De variabele is op Windows-knooppunten in de volgende indeling:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

De indeling is enigszins verschillen op Linux-knooppunten. Punten (.), afbreekstreepjes (-) en hekjes (#) worden samengevoegd tot onderstrepingstekens in de omgevingsvariabele. Bijvoorbeeld:

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

`APPLICATIONID`en `version` zijn waarden die overeenkomen met de toepassingen en pakketten versie die u hebt opgegeven voor de implementatie. Bijvoorbeeld, als u die versie 2.7 van toepassing opgegeven *blender* moet worden geïnstalleerd op Windows-knooppunten zou uw taak opdrachtregels deze omgevingsvariabele gebruiken voor toegang tot de bestanden:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Geef op Linux-knooppunten, de omgevingsvariabele in deze indeling:

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

Wanneer u toepassingspakketten uploadt, kunt u een standaardversie voor het implementeren van uw rekenknooppunten. Als u een standaardversie voor een toepassing hebt opgegeven, kunt u het versieachtervoegsel weglaten wanneer u verwijst naar de toepassing. Geef de versie van de toepassing standaard in de Azure-portal op de blade toepassingen, zoals wordt weergegeven in [uploaden en beheren van toepassingen](#upload-and-manage-applications).

Als u '2.7' instellen als de standaardversie voor toepassing bijvoorbeeld *blender*, en uw taken verwijzen naar de volgende omgevingsvariabele, en vervolgens uw Windows-knooppunten versie 2.7 wordt uitgevoerd:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Het volgende codefragment toont een voorbeeld van de taak vanaf de opdrachtregel die wordt gestart van de standaardversie van de *blender* toepassing:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Zie [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) in de [overzicht Batch-functies](batch-api-basics.md) voor meer informatie over omgevingsinstellingen compute-knooppunt.
> 
> 

## <a name="update-a-pools-application-packages"></a>De toepassingspakketten van een groep bijwerken
Als een bestaande pool al is geconfigureerd met een pakket, kunt u een nieuw pakket voor de pool opgeven. Als u een nieuw pakket verwijzing voor een groep, de volgende van toepassing opgeven:

* De Batch-service installeert het zojuist opgegeven pakket op alle nieuwe knooppunten die aan de pool worden toegevoegd en op een bestaande knooppunt dat is opnieuw opgestart of hersteld met een installatiekopie.
* COMPUTE knooppunten die zich al in de groep tijdens het bijwerken van het pakket verwijst naar het nieuwe toepassingspakket niet automatisch installeert. Deze berekenen knooppunten moeten worden opgestart of hersteld met een installatiekopie voor het ontvangen van het nieuwe pakket.
* Wanneer een nieuw pakket wordt geïmplementeerd, weerspiegelen de gemaakte omgevingsvariabelen in de nieuwe toepassing pakket verwijzingen.

In dit voorbeeld is de bestaande toepassingen versie 2.7 van de *blender* toepassing geconfigureerd als een van de [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. Voor het bijwerken van de pool knooppunten met versie 2.76b, Geef een nieuwe [ApplicationPackageReference] [ net_pkgref] met de nieuwe versie en de wijziging doorvoeren.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nu dat de nieuwe versie is geconfigureerd, de Batch-service in een versie 2.76b geïnstalleerd *nieuwe* knooppunt dat aan de pool wordt toegevoegd. 2.76b installeren op de knooppunten die *al* starten in de groep of deze installatiekopie. Houd er rekening mee dat opnieuw opgestart knooppunten de bestanden van eerdere implementaties van het pakket behoudt.

## <a name="list-the-applications-in-a-batch-account"></a>Lijst van de toepassingen in een Batch-account
U kunt de toepassingen en hun pakketten in een Batch-account weergeven met behulp van de [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] methode.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Inpakken
Met toepassingspakketten kunt u uw klanten, selecteer de toepassingen voor hun taken en geef de exacte versie moet worden gebruikt bij het verwerken van taken met de service Batch is ingeschakeld. U kunt ook de mogelijkheid voor uw klanten om te uploaden en bijhouden van hun eigen toepassingen in uw service opgeven.

## <a name="next-steps"></a>Volgende stappen
* De [Batch REST-API] [ api_rest] biedt ook ondersteuning voor gebruik met toepassingspakketten. Zie bijvoorbeeld de [applicationPackageReferences] [ rest_add_pool_with_packages] -element in [een groep toevoegen aan een account] [ rest_add_pool] voor informatie over het opgeven pakketten installeren met behulp van de REST-API. Zie [toepassingen] [ rest_applications] voor meer informatie over het verkrijgen van informatie over toepassingen met behulp van de Batch REST-API.
* Meer informatie over hoe u programmatisch [beheren van Azure Batch-accounts en quota's met Batch Management .NET](batch-management-dotnet.md). De [Batch Management .NET][api_net_mgmt] bibliotheek account maken en verwijderen-functies voor uw Batch-toepassing of service kunt inschakelen.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Toepassingspakketten op hoog niveau diagram"
[2]: ./media/batch-application-packages/app_pkg_02.png "De tegel toepassingen in Azure-portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Blade toepassingen in Azure-portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Toepassing details blade in Azure-portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Blade voor een nieuwe toepassing in Azure-portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Bijwerken of verwijderen van pakketten vervolgkeuzelijst in Azure-portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nieuwe toepassing pakket blade in Azure-portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Er is geen gekoppelde Opslagwaarschuwing voor account"
[10]: ./media/batch-application-packages/app_pkg_10.png "Kies de blade opslagaccount in Azure-portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Blade voor update-pakket in Azure-portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Dialoogvenster voor bevestiging pakket maken in Azure-portal verwijderen"

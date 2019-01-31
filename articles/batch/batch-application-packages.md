---
title: Toepassingspakketten installeren op rekenknooppunten - Azure Batch | Microsoft Docs
description: Gebruik de functie voor toepassingspakketten van Azure Batch om gemakkelijk te beheren meerdere toepassingen en versies voor installatie op Batch-rekenknooppunten.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/15/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6fd3eccf3de5d46520dc5a50cab66667c875799e
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55454604"
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Toepassingen implementeren op rekenknooppunten met Batch-toepassingspakketten

De functie voor toepassingspakketten van Azure Batch biedt eenvoudig beheer van taaktoepassingen en de implementatie ervan in de rekenknooppunten in uw pool. U kunt met toepassingspakketten kunt uploaden en beheren van meerdere versies van de toepassingen die de taken worden uitgevoerd, met inbegrip van hun ondersteunende bestanden. Vervolgens kunt u automatisch implementeren een of meer van deze toepassingen aan de rekenknooppunten in uw pool.

In dit artikel leert u hoe u om te uploaden en beheren van toepassingspakketten in Azure portal. Vervolgens leert u hoe u ze moet installeren op de rekenknooppunten van een pool met de [Batch .NET] [ api_net] bibliotheek.

> [!NOTE]
> 
> Toepassingspakketten worden ondersteund in alle Batch-pools die na 5 juli 2017 zijn gemaakt. De pakketten worden ondersteund in Batch-pools die zijn gemaakt tussen 10 maart 2016 en 5 juli 2017, maar alleen als de pool is gemaakt met behulp van een cloudservice-configuratie. Batch-pools die zijn gemaakt vóór 10 maart 2016 bieden geen ondersteuning voor toepassingspakketten.
>
> De API's voor het maken en beheren van toepassingspakketten maken deel uit van de [Batch Management .NET] [ api_net_mgmt] bibliotheek. De API's voor het installeren van toepassingspakketten op een rekenknooppunt maken deel uit van de [Batch .NET] [ api_net] bibliotheek. Vergelijkbare functies zijn in de beschikbare Batch-API's voor andere talen. 
>
> De functie voor toepassingspakketten die hier worden beschreven, vervangt de Batch-Apps-functie die beschikbaar zijn in eerdere versies van de service.
> 
> 

## <a name="application-package-requirements"></a>Pakket toepassingsvereisten
Voor het gebruik van toepassingspakketten, moet u [koppelt een Azure Storage-account](#link-a-storage-account) aan uw Batch-account.

## <a name="about-applications-and-application-packages"></a>Over toepassingen en toepassingspakketten
In Azure Batch, een *toepassing* verwijst naar een set versioned binaire bestanden die automatisch kan worden gedownload op de rekenknooppunten in uw pool. Een *toepassingspakket* verwijst naar een *specifieke set* van de binaire bestanden en staat voor een bepaalde *versie* van de toepassing.

![Diagram op hoog niveau van toepassingen en toepassingspakketten][1]

### <a name="applications"></a>Applicaties
Een toepassing in Batch bevat een of meer toepassingen, pakketten en Hiermee geeft u configuratie-opties voor de toepassing. Een toepassing kunt bijvoorbeeld opgeven dat de standaardversie voor het pakket van toepassing te installeren op de rekenknooppunten en of de pakketten kunnen worden bijgewerkt of verwijderd.

### <a name="application-packages"></a>Toepassingspakketten
Een toepassingspakket is een ZIP-bestand dat de binaire bestanden voor de toepassing bevat en ondersteunende bestanden die vereist zijn voor uw taken de toepassing uit te voeren. Elke toepassingspakket vertegenwoordigt een specifieke versie van de toepassing.

U kunt toepassingspakketten op het niveau van de groep en de taak opgeven. U kunt een of meer van deze pakketten en (optioneel) een versie opgeven wanneer u een groep of -taak maken.

* **Toepassingspakketten pool** zijn geïmplementeerd op *elke* knooppunt in de pool. Toepassingen worden geïmplementeerd als een knooppunt aan een pool wordt toegevoegd, en wanneer deze wordt gestart of teruggezet.
  
    Pool-toepassingspakketten zijn geschikt wanneer alle knooppunten in een pool van een taak taken uitvoeren. U kunt een of meer toepassingspakketten opgeven wanneer u een pool maakt, en u kunt toevoegen of bijwerken van een bestaande pool-pakketten. Als u een bestaande pool toepassingspakketten bijwerkt, moet u de knooppunten ervan voor het installeren van het nieuwe pakket opnieuw opstarten.
* **Taak toepassingspakketten** alleen naar een rekenknooppunt uitgevoerd van een taak, net vóór het uitvoeren van de opdrachtregel van de taak worden geïmplementeerd. Als het opgegeven pakket en de versie is al op het knooppunt, wordt deze niet is geïmplementeerd en het bestaande pakket wordt gebruikt.
  
    Toepassingspakketten zijn nuttig in omgevingen met gedeelde groepen, waarbij verschillende jobs worden uitgevoerd op één groep en de groep is niet verwijderd wanneer een taak is voltooid. Als de job minder taken dan knooppunten in de groep heeft, kunnen toepassingspakketten van taken gegevensoverdracht minimaliseren omdat uw toepassing alleen wordt geïmplementeerd op de knooppunten die taken uitvoeren.
  
    Andere scenario's die van toepassingspakketten profiteren kunnen zijn taken die een grote toepassing worden uitgevoerd, maar voor slechts een paar taken. Bijvoorbeeld, kan een vooraf verwerken fase of een merge-taak, waarbij de toepassing vooraf verwerken of het samenvoegen van zware is, profiteren van het gebruik van toepassingspakketten.

> [!IMPORTANT]
> Er zijn beperkingen op het aantal toepassingen en toepassingspakketten in een Batch-account en de maximale grootte van pakket. Zie [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor meer informatie over deze limieten.
> 
> 

### <a name="benefits-of-application-packages"></a>Voordelen van toepassingspakketten
Toepassingspakketten kunnen de code in uw Batch-oplossing vereenvoudigen en verlaag de overhead vereist voor het beheren van de toepassingen die de taken worden uitgevoerd.

Met toepassingspakketten kunt geen van de pool-begintaak om op te geven van een lange lijst met afzonderlijke bestanden te installeren op de knooppunten. U hebt geen handmatig beheer van meerdere versies van de toepassingsbestanden van uw in Azure Storage of op de knooppunten. En u hoeft niet te hoeven maken over het genereren van [SAS-URL's](../storage/common/storage-dotnet-shared-access-signature-part-1.md) voor toegang tot de bestanden in uw Storage-account. Batch is bijzonder op de achtergrond met Azure Storage voor het opslaan van toepassingspakketten en rekenknooppunten te implementeren.

> [!NOTE] 
> De totale grootte van een begintaak moet kleiner zijn dan of gelijk zijn aan 32.768 tekens, inclusief bronbestanden en omgevingsvariabelen. Als uw begintaak deze limiet overschrijdt, is toepassingspakketten te gebruiken met een andere optie. U kunt ook een ZIP-archief met de resourcebestanden maken, upload het als blob naar Azure Storage en pak het uit vanaf de opdrachtregel van de begintaak. 
>
>

## <a name="upload-and-manage-applications"></a>Uploaden en beheren van toepassingen
U kunt de [Azure-portal] [ portal] of de Batch Management-API's voor het beheren van de toepassingspakketten in uw Batch-account. In de volgende gedeelten we eerst laten zien hoe een Storage-account koppelen en vervolgens bespreken toe te voegen toepassingen en pakketten en het beheer met de portal.

### <a name="link-a-storage-account"></a>Een Storage-account koppelen
Voor het gebruik van toepassingspakketten, moet u eerst een koppeling een [Azure Storage-account](batch-api-basics.md#azure-storage-account) aan uw Batch-account. Als u een opslagaccount nog niet hebt geconfigureerd, de Azure-portal geeft een waarschuwing dat de eerste keer dat u op **toepassingen** in uw Batch-account.



!['Er is geen opslagaccount geconfigureerd' waarschuwing in Azure portal][9]

De Batch-service maakt gebruik van de gekoppelde Storage-account voor het opslaan van uw toepassingspakketten. Nadat u de twee accounts hebt gekoppeld, kan de pakketten die zijn opgeslagen in het gekoppelde opslagaccount aan uw compute-knooppunten automatisch implementeren door Batch. Als u wilt een Storage-account koppelen aan uw Batch-account, klikt u op **opslagaccount** op de **waarschuwing** venster en klik vervolgens op **Opslagaccount** opnieuw.

![Kies blade opslagaccount in Azure portal][10]

Het is raadzaam dat u een opslagaccount maken *specifiek* voor gebruik met uw Batch-account en selecteert u dit hier. Nadat u een opslagaccount hebt gemaakt, kunt u vervolgens deze koppelen aan uw Batch-account met behulp van de **Opslagaccount** venster.

> [!NOTE] 
> Momenteel u toepassingspakketten niet gebruiken met een Azure Storage-account dat is geconfigureerd met [firewall-regels](../storage/common/storage-network-security.md).
> 

De Batch-service maakt gebruik van Azure Storage voor het opslaan van uw toepassingspakketten als blok-blobs. U bent [in rekening gebracht als normale] [ storage_pricing] voor de blok-blob gegevens en de grootte van elk pakket kan niet groter zijn dan de [blob van maximale blokgrootte](../storage/common/storage-scalability-targets.md#azure-blob-storage-scale-targets). Moet u rekening houden met de grootte en het nummer van uw toepassingspakketten en verwijderen periodiek verouderde pakketten u kosten kunt minimaliseren.
> 
> 

### <a name="view-current-applications"></a>De huidige toepassingen weergeven
Als u wilt de toepassingen in uw Batch-account bekijken, klikt u op de **toepassingen** menu-item in het menu links bij weergave uw **Batch-account**.

![Toepassingen-tegel][2]

Hiermee opent u deze optie selecteren de **toepassingen** venster:

![Toepassingen weergeven][3]

In dit venster geeft de ID van elke toepassing in uw account en de volgende eigenschappen:

* **Pakketten**: Het nummer van de versies die zijn gekoppeld aan deze toepassing.
* **Standaardversie**: De versie van de toepassing geïnstalleerd indien u niet een versie aan wanneer u de toepassing voor een groep opgeeft. Deze instelling is optioneel.
* **Updates toestaan**: De waarde die aangeeft of pakket met updates, verwijderingen en toevoegingen zijn toegestaan. Als deze optie is ingesteld op **Nee**, pakketupdates en verwijderingen, zijn uitgeschakeld voor de toepassing. Alleen nieuwe toepassingspakketversies kunnen worden toegevoegd. De standaardinstelling is **Ja**.

### <a name="view-application-details"></a>Details van de toepassing weergeven
Als u wilt zien van de details voor een toepassing, selecteert u de toepassing in de **toepassingen** venster.

![Toepassingsgegevens][4]

In de toepassing, kunt u de volgende instellingen configureren voor uw toepassing.

* **Updates toestaan**: Geef op of de toepassingspakketten kunnen worden bijgewerkt of verwijderd. Zie 'Bijwerken of verwijderen van een toepassingspakket' verderop in dit artikel.
* **Standaardversie**: Geef een standaard-toepassingspakket te implementeren naar rekenknooppunten die zich op.
* **Weergavenaam**: Geef een beschrijvende naam die uw Batch-oplossing gebruiken kunt wanneer deze geeft informatie weer over de toepassing, bijvoorbeeld in de gebruikersinterface van een service die u voor uw klanten via de Batch opgeeft.

### <a name="add-a-new-application"></a>Een nieuwe toepassing toevoegen
Een toepassingspakket toevoegen voor het maken van een nieuwe toepassing en geeft u een nieuwe, unieke toepassing-ID. Het eerste toepassingspakket dat u met de nieuwe toepassings-ID toevoegen maakt ook de nieuwe toepassing.

Klik op **Toepassingen** > **Toevoegen**.

![Nieuwe toepassingsblade in Azure portal][5]

De **nieuwe toepassing** venster biedt de volgende velden om op te geven van de instellingen van uw nieuwe toepassing en het toepassingspakket.

**Toepassings-id**

Dit veld geeft de ID van uw nieuwe toepassing, onderworpen aan de standaardregels voor validatie van Azure Batch-ID is. De regels voor het ontwikkelen van een toepassings-ID zijn als volgt:

* Op Windows-knooppunten, kan de ID een combinatie van alfanumerieke tekens, afbreekstreepjes en onderstrepingstekens bevatten. Op Linux-knooppunten mogen alleen alfanumerieke tekens en onderstrepingstekens bevatten.
* Mag niet langer zijn dan 64 tekens bevatten.
* Moet uniek zijn binnen het Batch-account.
* Aanvraag te behouden en niet-hoofdlettergevoelig is.

**Versie**

Dit veld geeft de versie van het toepassingspakket dat u uploadt. Versietekenreeksen zijn afhankelijk van de volgende validatieregels:

* Op Windows-knooppunten, kan de tekenreeks voor een combinatie van alfanumerieke tekens, afbreekstreepjes, onderstrepingstekens en punten bevatten. Op Linux-knooppunten mag de tekenreeks alleen alfanumerieke tekens en onderstrepingstekens bevatten.
* Mag niet langer zijn dan 64 tekens bevatten.
* Moet uniek zijn binnen de toepassing.
* Aanvraag te behouden en niet-hoofdlettergevoelig zijn.

**Toepassingspakket**

Dit veld geeft de ZIP-bestand dat de binaire bestanden voor de toepassing bevat en de ondersteunende bestanden die nodig zijn voor het uitvoeren van de toepassing. Klik op de **selecteert u een bestand** box of het mappictogram om te bladeren naar en selecteer een ZIP-bestand dat bestanden van uw toepassing bevat.

Nadat u een bestand hebt geselecteerd, klikt u op **OK** om te beginnen met het uploaden naar Azure Storage. Wanneer de uploadbewerking voltooid is, wordt in de portal een melding weergegeven. Met deze bewerking kan enige tijd duren, afhankelijk van de grootte van het bestand dat u uploadt en de snelheid van uw netwerkverbinding.

> [!WARNING]
> Sluit niet de **nieuwe toepassing** venster voordat de uploadbewerking voltooid is. In dat geval stopt het uploadproces.
> 
> 

### <a name="add-a-new-application-package"></a>Een nieuw toepassingspakket toevoegen
Als u wilt een pakketversie van toepassing voor een bestaande toepassing toevoegen, selecteert u een toepassing in de **toepassingen** windows en klik op **pakketten** > **toevoegen**.

![Pakket toepassingsblade toevoegen in Azure portal][8]

Zoals u ziet de velden overeenkomen met die van de **nieuwe toepassing** venster, maar de **toepassings-id** selectievakje is uitgeschakeld. Als u dit hebt gedaan voor de nieuwe toepassing, geef de **versie** voor het nieuwe pakket, gaat u naar uw **toepassingspakket** ZIP-bestand en klik vervolgens op **OK** voor het uploaden van het pakket.

### <a name="update-or-delete-an-application-package"></a>Bijwerken of verwijderen van een toepassingspakket
Als u wilt bijwerken of verwijderen van een bestaand toepassingspakket, de details voor de toepassing openen, klikt u op **pakketten**, klikt u op de **weglatingsteken** in de rij van het toepassingspakket dat u wilt wijzigen, en selecteer de actie die u wilt uitvoeren.

![Bijwerken of verwijderen van pakket in Azure portal][7]

**Update**

Wanneer u klikt op **Update**, wordt de **updatepakket** windows wordt weergegeven. In dit venster is vergelijkbaar met de **nieuw toepassingspakket** venster, maar alleen het veld pakket is ingeschakeld, zodat u kunt om op te geven van een nieuw zipbestand te uploaden.

![Update-pakket-blade in Azure portal][11]

**Verwijderen**

Wanneer u klikt op **verwijderen**, u wordt gevraagd de verwijdering van de versie van het toepassingspakket te bevestigen en Batch wordt het pakket verwijderd uit Azure Storage. Als u de standaard-versie van een toepassing, verwijdert de **standaardversie** instelling voor de toepassing is verwijderd.

![Toepassing verwijderen ][12]

## <a name="install-applications-on-compute-nodes"></a>Toepassingen installeren op de rekenknooppunten
Nu dat u hebt geleerd over het beheren van pakketten van de toepassing met de Azure-portal, kunnen we bespreken hoe ze om de rekenknooppunten en voer ze uit met Batch-taken moeten worden geïmplementeerd.

### <a name="install-pool-application-packages"></a>Toepassingspakketten voor toepassingen installeren
Als u wilt installeren een toepassingspakket op alle rekenknooppunten in een pool, Geef een of meer toepassingspakket *verwijzingen* voor de pool. De toepassingspakketten die u voor een groep van toepassingen opgeeft zijn geïnstalleerd op elk knooppunt wanneer dat knooppunt aan de pool wordt toegevoegd, en wanneer het knooppunt wordt gestart of teruggezet.

In Batch .NET, geeft u een of meer [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] wanneer u een nieuwe groep maakt, of voor een bestaande pool. De [ApplicationPackageReference] [ net_pkgref] klasse Hiermee geeft u een toepassings-ID en versie te installeren op een pool van rekenknooppunten.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

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
> Als de implementatie van een toepassing-pakket om welke reden dan ook mislukt, de Batch-service wordt het knooppunt gemarkeerd [onbruikbaar][net_nodestate], en er zijn geen taken zijn gepland voor uitvoering op dat knooppunt. In dit geval moet u **opnieuw** het knooppunt opnieuw starten van de pakketimplementatie. Opnieuw opstarten van het knooppunt kan ook de taakplanning opnieuw op het knooppunt.
> 
> 

### <a name="install-task-application-packages"></a>Toepassingspakketten installeren
Net als bij een groep van toepassingen, u toepassingspakket *verwijzingen* voor een taak. Wanneer een taak is gepland om uit te voeren op een knooppunt, wordt het pakket gedownload en uitgepakt net voordat de opdrachtregel van de taak wordt uitgevoerd. Als een specifiek pakket en de versie is al geïnstalleerd op het knooppunt, wordt het pakket niet is gedownload en wordt het bestaande pakket wordt gebruikt.

Als u wilt een pakket van de toepassing taak installeren, configureren van de taak [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] eigenschap:

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
De pakketten die u hebt opgegeven voor een groep of -taak worden gedownload en uitgepakt naar een map met de naam in de `AZ_BATCH_ROOT_DIR` van het knooppunt. Batch maakt ook een omgevingsvariabele die het pad naar map met de naam bevat. De opdrachtregels gebruikt deze omgevingsvariabele wanneer u verwijst naar de toepassing op het knooppunt. 

De variabele is op Windows-knooppunten in de volgende indeling:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Op Linux-knooppunten is de indeling enigszins anders. Punten (.), afbreekstreepjes (-) en hekjes (#) worden samengevoegd tot onderstrepingstekens in de omgevingsvariabele. Houd er ook rekening mee dat het geval van de toepassings-ID behouden blijft. Bijvoorbeeld:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

`APPLICATIONID` en `version` zijn waarden die overeenkomen met de toepassingen en pakketten versie die u hebt opgegeven voor de implementatie. Bijvoorbeeld, als u die versie 2.7 van de toepassing opgegeven *blender* moet worden geïnstalleerd op Windows-knooppunten, zou de opdrachtregels deze omgevingsvariabele gebruiken voor toegang tot de bestanden:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Geef de omgevingsvariabele in deze indeling op Linux-knooppunten. Samenvoegen van de punten (.), afbreekstreepjes (-) en hekjes (#) in onderstrepingstekens en behouden het geval van de toepassings-ID:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
``` 

Wanneer u een toepassingspakket uploaden, kunt u een standaardversie implementeren naar uw rekenknooppunten. Als u een standaardversie voor een toepassing hebt opgegeven, kunt u het versieachtervoegsel weglaten wanneer u verwijst naar de toepassing. De versie van de standaard-toepassing in Azure portal, kunt u opgeven in de **toepassingen** venster, zoals wordt weergegeven in [uploaden en beheren van toepassingen](#upload-and-manage-applications).

Als u "2.7" instellen als de standaardversie voor de toepassing bijvoorbeeld *blender*, en uw taken verwijzen naar de volgende omgevingsvariabele, wordt uw Windows-knooppunten versie 2.7 wordt uitgevoerd:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Het volgende codefragment toont een voorbeeld van de taak vanaf de opdrachtregel die wordt gestart van de standaard-versie van de *blender* toepassing:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Zie [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) in de [overzicht van Batch-functies](batch-api-basics.md) voor meer informatie over de omgevingsinstellingen voor compute-knooppunt.
> 
> 

## <a name="update-a-pools-application-packages"></a>De toepassingspakketten van een groep bijwerken
Als een bestaande pool is al geconfigureerd met een toepassingspakket, kunt u een nieuw pakket voor de pool opgeven. Als u een nieuwe pakketverwijzing voor een groep, de volgende van toepassing:

* De Batch-service installeert de zojuist opgegeven pakket op alle nieuwe knooppunten die aan de pool worden toegevoegd en op de bestaande knooppunten die wordt gestart of teruggezet.
* COMPUTE-knooppunten die zich al in de pool wanneer u verwijst het pakket naar bijwerkt het nieuwe toepassingspakket niet automatisch installeert. Deze compute-knooppunten moeten worden gestart of teruggezet voor het ontvangen van het nieuwe pakket.
* Wanneer een nieuw pakket wordt geïmplementeerd, weerspiegelen de gemaakte omgevingsvariabelen in de nieuwe verwijzingen voor toepassingspakketten.

In dit voorbeeld is de bestaande groep versie 2.7 van de *blender* toepassing die is geconfigureerd als een van de [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. Voor het bijwerken van de pool-knooppunten met versie 2.76b, Geef een nieuwe [ApplicationPackageReference] [ net_pkgref] met de nieuwe versie, en dat de wijziging.

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

Nu dat de nieuwe versie is geconfigureerd, de Batch-service in een versie 2.76b geïnstalleerd *nieuwe* knooppunt dat aan de pool wordt toegevoegd. 2.76b installeren op de knooppunten die zijn *al* in de pool opnieuw opstarten of deze installatiekopie terugzetten. Houd er rekening mee dat opnieuw opgestart knooppunten de bestanden van eerdere implementaties van het pakket behoudt.

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

## <a name="wrap-up"></a>Samenvatten
Met toepassingspakketten kunt kunt u uw klanten de toepassingen voor hun werk te selecteren en geeft u de exacte versie moet worden gebruikt bij het verwerken van taken aan uw service voor Batch geschikte helpen. U kunt ook de mogelijkheid voor uw klanten om te uploaden en bijhouden van hun eigen toepassingen in uw service opgeven.

## <a name="next-steps"></a>Volgende stappen
* De [Batch REST-API] [ api_rest] biedt ook ondersteuning voor het werken met toepassingspakketten. Zie bijvoorbeeld de [applicationPackageReferences] [ rest_add_pool_with_packages] -element in [een groep toevoegen aan een account] [ rest_add_pool] voor informatie over het opgeven pakketten te installeren met behulp van de REST-API. Zie [toepassingen] [ rest_applications] voor meer informatie over het verkrijgen van informatie over toepassingen met behulp van de Batch REST-API.
* Leer hoe u programmatisch [beheren Azure Batch-accounts en -quota met Batch Management .NET](batch-management-dotnet.md). De [Batch Management .NET] [ api_net_mgmt] bibliotheek account maken en verwijderen functies voor uw Batch-toepassing of service kunt inschakelen.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
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

[1]: ./media/batch-application-packages/app_pkg_01.png "Toepassingspakketten diagram op hoog niveau"
[2]: ./media/batch-application-packages/app_pkg_02.png "De tegel toepassingen in Azure portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Toepassingen-blade in Azure portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Details van toepassingsblade in Azure portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nieuwe toepassingsblade in Azure portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Bijwerken of verwijderen van pakketten vervolgkeuzelijst in Azure portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nieuwe toepassing pakket-blade in Azure portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Er is geen gekoppelde Storage-account-waarschuwing"
[10]: ./media/batch-application-packages/app_pkg_10.png "Kies blade opslagaccount in Azure portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Update-pakket-blade in Azure portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Dialoogvenster voor bevestiging pakket maken in Azure portal verwijderen"

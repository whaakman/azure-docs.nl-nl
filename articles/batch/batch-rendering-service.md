---
title: De Azure Batch Rendering-service gebruiken voor rendering in de cloud | Microsoft Docs
description: Taken rechtstreeks vanuit Maya renderen op virtuele Azure-machines en betalen op basis van per gebruik.
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 4d22f92cafdbceee5213361d6d2b2f38904d12c6
ms.contentlocale: nl-nl
ms.lasthandoff: 08/01/2017

---

# <a name="get-started-with-the-batch-rendering-service"></a>Aan de slag met de Batch Rendering-service

De Azure Batch Rendering-service biedt mogelijkheden voor rendering in de cloud op basis van betalen per gebruik. De Batch Rendering-service verwerkt taakplanning en wachtrijen, beheert fouten en nieuwe pogingen, en automatische schaling voor uw renderingtaak. De Batch Rendering-service ondersteunt Autodesk Maya, 3ds Max en Arnold. Binnenkort komt ook ondersteuning voor andere toepassingen beschikbaar. Met de Batch-invoegtoepassing voor Maya 2017 kunt u gemakkelijk vanaf uw bureaublad een renderingtaak starten in Azure. 

## <a name="supported-applications"></a>Ondersteunde toepassingen

De Batch Rendering-service ondersteunt momenteel de volgende toepassingen:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de Batch Rendering-service hebt u het volgende nodig:

- Een [Azure-account](https://azure.microsoft.com/free/). 
- **Een Azure Batch-account.** Zie [Een Batch-account maken in Azure Portal](batch-account-create-portal.md) voor instructies over het maken van een Batch-account in Azure Portal.
- Een **Azure Storage-account.** De assets die worden gebruikt voor uw renderingtaak worden opgeslagen in Azure Storage. U kunt automatisch een opslagaccount maken bij het instellen van uw Batch-account. U kunt ook een bestaand opslagaccount gebruiken. Zie [Een opslagaccount in Azure Portal maken, beheren of verwijderen](https://docs.microsoft.com/azure/storage/storage-create-storage-account) voor meer informatie over opslagaccounts.

Voor het gebruik van de Batch-invoegtoepassing voor Maya, hebt u het volgende nodig:

- **Maya 2017**
- **Arnold voor Maya**

U kunt ook [Azure Portal](https://portal.azure.com) gebruiken voor het maken van pools van virtuele machines die vooraf zijn geconfigureerd met Maya, 3ds Max en Arnold. U kunt de portal gebruiken om mislukte taken te bewaken en te onderzoeken door toepassingslogboeken te downloaden en op afstand verbinding te maken met afzonderlijke virtuele machines die RDP of SSH gebruiken.

## <a name="basic-batch-concepts"></a>Basisconcepten voor Batch

Voordat u aan de slag gaat met de Batch Rendering-service, is het handig om vertrouwd te raken met een paar concepten van Batch, met inbegrip van rekenknooppunten, pools en taken. Zie [Intrinsiek parallelle workloads uitvoeren met Batch](batch-technical-overview.md) voor meer informatie over Azure Batch in het algemeen.

### <a name="pools"></a>Pools

Batch is een platformservice voor het uitvoeren van rekenintensief werk, zoals rendering, in een **pool** van **rekenknooppunten**. Elk rekenknooppunt in een pool is een virtuele Azure-machine (VM) met Linux of Windows. 

Zie de secties [Pool](batch-api-basics.md#pool) en [Rekenknooppunt](batch-api-basics.md#compute-node) in [Grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md) voor meer informatie over Batch-pools en -rekenknooppunten.

### <a name="jobs"></a>Taken

Een Batch-**taak** is een verzameling subtaken die worden uitgevoerd op de rekenknooppunten in een pool. Wanneer u een renderingtaak verzendt, wordt de taak door Batch in subtaken verdeeld en worden de subtaken gedistribueerd naar de rekenknooppunten in de pool voor uitvoering.

Zie de sectie [Taak](batch-api-basics.md#job) in [Grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md) voor meer informatie over batchtaken.

## <a name="use-the-batch-plug-in-for-maya-to-submit-a-render-job"></a>De Batch-invoegtoepassing voor Maya gebruiken om een renderingtaak te verzenden

Met de Batch-invoegtoepassing voor Maya kunt u rechtstreeks vanuit Maya een taak verzenden naar de Batch Rendering-service. In de volgende secties wordt beschreven hoe u de taak configureert vanuit de invoegtoepassing en vervolgens verzendt. 

### <a name="load-the-batch-plug-in-in-maya"></a>De Batch-invoegtoepassing laden in Maya

De Batch-invoegtoepassing is beschikbaar op [GitHub](https://github.com/Azure/azure-batch-maya/releases). Pak het archief uit in de gewenste map. U kunt de invoegtoepassing rechtstreeks laden vanuit de map *azure_batch_maya*.

U laadt de invoegtoepassing als volgt in Maya:

1. Voer Maya uit.
2. Open **Voorkeuren voor**  > **computervensterinstellingen** > **Invoegtoepassingenbeheer**.
3. Klik op **Bladeren**.
4. Navigeer naar en selecteer *azure_batch_maya/plug-in/AzureBatch.py*.

### <a name="authenticate-access-to-your-batch-and-storage-accounts"></a>Toegang tot uw Batch- en Storage-account verifiëren

Voor het gebruik van de invoegtoepassing moet u verifiëren met behulp van uw Azure Batch- en Azure Storage-accountsleutels. Ga als volgt te werk om uw accountsleutels op te halen:

1. Geef de invoegtoepassing weer in Maya en selecteer het tabblad **Config**.
2. Navigeer naar [Azure Portal](https://portal.azure.com).
3. Selecteer **Batch-accounts** in het menu links. Klik indien nodig op **Meer services** en filter op _Batch_.
4. Zoek de gewenste Batch-account op in de lijst.
5. Selecteer de menuopdracht **Sleutels** om uw accountnaam, account-URL en toegangssleutels weer te geven:
    - Plak de URL van het Batch-account in het veld **Service** in de Batch-invoegtoepassing.
    - Plak de accountnaam in het veld **Batch-account**.
    - Plak de primaire accountsleutel in het veld **Batchsleutel**.
7. Selecteer Opslagaccounts in het menu links. Klik indien nodig op **Meer services** en filter op _Opslag_.
8. Zoek het gewenste opslagaccount op in de lijst.
9. Selecteer de menuopdracht **Toegangssleutels** om de naam van het opslagaccount en de sleutels weer te geven.
    - Plak de naam van het opslagaccount in het veld **Opslagaccount** in de Batch-invoegtoepassing.
    - Plak de primaire accountsleutel in het veld **Opslagsleutel**.
10. Klik op **Verifiëren** om ervoor te zorgen dat de invoegtoepassing toegang heeft tot beide accounts.

Zodra u hebt geverifieerd, wordt het statusveld door de invoegtoepassing ingesteld op **Geverifieerd**: 

![Uw Batch- en Storage-account verifiëren](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Een pool voor een renderingtaak configureren

Nadat u uw Batch- en Storage-account hebt geverifieerd, stelt u een pool in voor uw renderingtaak. De invoegtoepassing slaat uw selecties op tussen sessies. Zodra u de configuratie van uw voorkeur hebt ingesteld, hoeft u deze niet wijzigen tenzij deze verandert.

In de volgende secties nemen we stapsgewijs de beschikbare opties door, die beschikbaar zijn op het tabblad **Verzenden**:

#### <a name="specify-a-new-or-existing-pool"></a>Een nieuwe of bestaande pool opgeven

Selecteer het tabblad **Verzenden** om een pool op te geven waarin u de renderingtaak wilt uitvoeren. Dit tabblad biedt opties voor het maken van een pool of het selecteren van een bestaande pool:

- U kunt **automatisch een pool inrichten voor deze taak** (standaardoptie). Als u deze optie kiest, maakt Batch de pool uitsluitend bedoeld is voor de huidige taak en wordt de pool automatisch verwijderd wanneer de renderingtaak is voltooid. Deze optie wordt aanbevolen wanneer u een één renderingtaak moet uitvoeren.
- U kunt **een bestaande permanente pool opnieuw gebruiken**. Als u een bestaande pool hebt die niet actief is, kunt u die pool opgeven voor het uitvoeren van de renderingtaak door deze te selecteren in de vervolgkeuzelijst. Hergebruik van een bestaande permanente pool bespaart de tijd die nodig is voor het inrichten van de pool.  
- U kunt **een nieuwe permanente pool maken**. Als u deze optie kiest, wordt een nieuwe pool gemaakt voor het uitvoeren van de taak. De pool wordt niet verwijderd wanneer de taak is voltooid, zodat u deze opnieuw kunt gebruiken voor toekomstige taken. Selecteer deze optie wanneer u continu renderingtaken moet uitvoeren. Bij volgende taken kunt u **een bestaande permanente pool opnieuw gebruiken** selecteren om de permanente pool te gebruiken die u hebt gemaakt voor de eerste taak.

![Een pool, installatiekopie van het besturingssysteem, VM-grootte en licentie opgeven](./media/batch-rendering-service/submit.png)

Zie de [Veelgestelde vragen over tarieven voor Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) en [Veelgestelde vragen over tarieven voor Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq) voor meer informatie over hoe de kosten voor virtuele Azure-machines zijn opgebouwd.

#### <a name="specify-the-os-image-to-provision"></a>De in te richten installatiekopie van het besturingssysteem opgeven

U kunt het type installatiekopie van het besturingssysteem opgeven dat moet worden gebruikt voor het inrichten van rekenknooppunten in de pool op het tabblad **Env** (omgeving). Batch ondersteunt momenteel de volgende installatiekopieopties voor renderingtaken:

|Besturingssysteem  |Installatiekopie  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Een VM-grootte kiezen

U kunt de VM-grootte opgeven op het tabblad **Env**. Zie [Linux VM-grootten in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) en [Windows VM-grootten in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) voor meer informatie over beschikbare VM-grootten. 

![De installatiekopie van het besturingssysteem en grootte opgeven voor de VM op het tabblad Env](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Licentieopties opgeven

U kunt de licenties die u wilt gebruiken, opgeven op het tabblad **Env**. Een aantal opties:

- **Maya**, dat standaard is ingeschakeld.
- **Arnold**, dat is ingeschakeld als Arnold is gedetecteerd als de actieve render-engine in Maya.

 Als u wilt renderen met behulp van uw eigen licentie, kunt u uw licentie-eindpunt configureren door de juiste omgevingsvariabelen toe te voegen aan de tabel. Zorg ervoor dat u de selectie van de standaardvolumelicentie-opties opheft als u dit doet.

> [!IMPORTANT]
> U wordt gefactureerd voor gebruik van de licenties zolang virtuele machines worden uitgevoerd in de pool, zelfs als de virtuele machines op dit moment niet worden gebruikt voor de rendering. Om extra kosten te voorkomen, gaat u naar het tabblad **Pools** en stelt u de grootte van de pool in op 0 knooppunten totdat u klaar bent voor het uitvoeren van een andere renderingtaak. 
>
>

#### <a name="manage-persistent-pools"></a>Permanente pools beheren

U kunt een bestaande permanente pool beheren op het tabblad **Pools**. Als u een pool selecteert in de lijst, wordt de huidige status van de pool weergegeven.

Op het tabblad **Pools** kunt u de pool ook verwijderen en het aantal virtuele machines in de pool vergroten of verkleinen. U kunt de grootte van een groep instellen op 0 knooppunten om kosten tussen workloads te vermijden.

![Pools weergeven, vergroten of verkleinen en verwijderen](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Een renderingtaak configureren voor verzending

Wanneer u de parameters voor de pool die de taak gaat renderen hebt opgegeven, moet u de taak zelf configureren. 

#### <a name="specify-scene-parameters"></a>Scène-parameters opgeven

De Batch-invoegtoepassing detecteert welke rendering-engine die u momenteel gebruikt in Maya en geeft de juiste renderinginstellingen weer op het tabblad **Verzenden**. Deze instellingen omvatten het beginframe, eindframe, uitvoervoorvoegsel en de framestap. U kunt de renderinginstellingen van het scène-bestand overschrijven door andere instellingen in de invoegtoepassing op te geven. Wijzigingen die u aanbrengt in de invoegtoepassinginstellingen worden niet permanent terug naar de renderinstellingen van het scène-bestand opgeslagen, zodat u wijzigingen per taak kunt doorvoeren zonder het scène-bestand opnieuw te hoeven uploaden.

De invoegtoepassing waarschuwt u als de render-engine die u in Maya hebt geselecteerd niet wordt ondersteund.

Als u een nieuwe scène wilt laden terwijl de invoegtoepassing geopend is, klikt u op de knop **Vernieuwen** om te controleren of de instellingen worden bijgewerkt.

#### <a name="resolve-asset-paths"></a>Assetpaden omzetten

Wanneer u de invoegtoepassing laadt, scant deze het scène-bestand op eventuele verwijzingen naar externe bestanden. Deze verwijzingen worden weergegeven op het tabblad **Assets**. Als een pad waarnaar wordt verwezen niet kan worden omgezet, probeert de invoegtoepassing het bestand te vinden op een paar standaardlocaties, met inbegrip van:

- De locatie van het scène-bestand 
- De map _sourceimages_ van het huidige project
- De huidige werkmap. 

Als de asset nog steeds niet kan worden gevonden, wordt het met een waarschuwingspictogram weergegeven:

![Ontbrekende assets worden weergegeven met een waarschuwingspictogram](./media/batch-rendering-service/missing_assets.png)

Als u de locatie weet van een verwijzing naar een niet-omgezet bestand weet, kunt u op het waarschuwingspictogram klikken. U wordt dan gevraagd een zoekpad toe te voegen. De invoegtoepassing gebruikt vervolgens dit zoekpad om eventuele ontbrekende assets om te zetten. U kunt een willekeurig aantal aanvullende zoekpaden toevoegen.

Wanneer een verwijzing is omgezet, wordt deze weergegeven met een pictogram met een groen lampje:

![Omgezette assets weergeven met een pictogram met een groen lampje](./media/batch-rendering-service/found_assets.png)

Als uw scène andere bestanden vereist die niet door de invoegtoepassing zijn gedetecteerd, kunt u extra bestanden of mappen toevoegen. Gebruik de knoppen **Bestanden toevoegen** en **Map toevoegen**. Als u een nieuwe scène laadt terwijl de invoegtoepassing geopend is, moet u op **Vernieuwen** klikken om de verwijzingen van de scène bij te werken.

#### <a name="upload-assets-to-an-asset-project"></a>Assets naar een asssetproject uploaden

Wanneer u een renderingtaak verzendt, worden de bestanden die worden weergegeven op het tabblad **Assets** tabblad automatisch geüpload naar Azure Storage als een assetproject. U kunt de assetbestanden ook onafhankelijk van een rendering uploaden met behulp van de knop **Uploaden** op het tabblad **Assets**. De naam van het assetproject is opgegeven in het veld **Project** en wordt standaard na het huidige Maya-genoemd. Wanneer assetbestanden worden geüpload, wordt de lokale bestandsstructuur bewaard. 

Na het uploaden kan door een willekeurig aantal renderingtaken naar assets worden verwezen. Alle geüploade assets zijn beschikbaar voor elke taak die verwijst naar het assetproject asset of ze nu zijn opgenomen in de scène of niet. Als u de naam wilt wijzigen van het assetproject waarnaar door uw volgende taak wordt verwezen, wijzigt u de naam in het veld **Project** op het tabblad **Assets**. Als er wordt verwezen naar bestanden die u wilt uitsluiten van het uploaden, heft u de selectie hiervan op met behulp van de groene knop naast de lijst.

#### <a name="submit-and-monitor-the-render-job"></a>De renderingtaak verzenden en volgen

Nadat u de renderingtaak in de invoegtoepassing hebt geconfigureerd, klikt u op de **Taak verzenden** op het tabblad **Verzenden** om de taak te verzenden naar Batch:

![De renderingtaak verzenden](./media/batch-rendering-service/submit_job.png)

U kunt een taak die wordt uitgevoerd volgen via het tabblad **Taken** van de invoegtoepassing. Selecteer een taak in de lijst om de huidige status van de taak weer te geven. U kunt dit tabblad ook gebruiken om taken te annuleren en te verwijderen, en ook om de uitvoering en renderinglogboeken weer te geven. 

Als u uitvoer wilt downloaden, wijzigt u het veld **Uitvoer** en stelt u de gewenste doelmap in. Klik op het tandwielpictogram om een achtergrondproces te starten dat de taak controleert en uitvoer downloadt naarmate het proces vordert: 

![Taakstatus bekijken en uitvoer downloaden](./media/batch-rendering-service/jobs.png)

U kunt Maya sluiten zonder het downloadproces te verstoren.

## <a name="next-steps"></a>Volgende stappen

Zie [Intrinsiek parallelle workloads uitvoeren met Batch](batch-technical-overview.md) voor meer informatie over Batch.

---
title: Functies - rendering Azure Batch
description: Mogelijkheden voor specifieke rendering in Azure Batch
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: be6c0f9a8874507433606903bcbd58c7723d6a8a
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791797"
---
# <a name="azure-batch-rendering-capabilities"></a>Mogelijkheden van Azure Batch rendering

Standaard Azure Batch-functies worden gebruikt voor rendering van workloads en toepassingen kunt uitvoeren. Batch bevat ook specifieke functies voor het ondersteunen van rendering-workloads.

Zie voor een overzicht van concepten van Batch, pools, jobs en taken, waaronder [in dit artikel](https://docs.microsoft.com/azure/batch/batch-api-basics).

## <a name="batch-pools"></a>Batch-Pools

### <a name="rendering-application-installation"></a>Rendering van de installatie van toepassing

Een Azure Marketplace rendering VM-installatiekopie kan worden opgegeven in de configuratie van de groep van toepassingen als alleen de vooraf geïnstalleerde toepassingen hoeft te worden gebruikt.

Er is een Windows 2016-installatiekopie en een CentOS-installatiekopie.  In de [Azure Marketplace](https://azuremarketplace.microsoft.com), kunnen u de VM-installatiekopieën vinden door te zoeken naar 'batch rendering'.

Zie voor een voorbeeldconfiguratie van toepassingen, de [zelfstudie voor Azure CLI-rendering](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli).  De Azure-portal en Batch Explorer bieden GUI-hulpprogramma's om te selecteren van een rendering-VM-installatiekopie wanneer u een pool maakt.  Als u een Batch-API gebruikt, geeft u de volgende eigenschapswaarden voor [ImageReference](https://docs.microsoft.com/rest/api/batchservice/pool/add#imagereference) bij het maken van een groep:

| Uitgever | Aanbieding | Sku | Versie |
|---------|---------|---------|--------|
| batch | rendering-centos73 | rendering | nieuwste |
| batch | rendering-windows2016 | rendering | nieuwste |

Andere opties zijn beschikbaar als aanvullende toepassingen zijn vereist voor de pool-VM's:

* Een aangepaste installatiekopie op basis van een standard Marketplace-installatiekopie:
  * Met deze optie kunt u uw virtuele machine configureren met de exacte toepassingen en specifieke versies die u nodig hebt. Zie voor meer informatie, [een aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken](https://docs.microsoft.com/azure/batch/batch-custom-images). Autodesk en Chaos Group hebt gewijzigd Arnold en V-Ray respectievelijk, om te valideren op basis van een Azure Batch service-licentieverlening. Zorg ervoor dat u beschikt over de versies van deze toepassingen met deze ondersteuning, anders werkt de licentieverlening voor betalen per gebruik niet. Huidige versies van Maya of 3ds Max licentieserver niet vereisen bij het uitvoeren van headless (in batch/opdrachtregelmodus). Neem contact op met ondersteuning van Azure als u niet zeker weet hoe u om door te gaan met deze optie.
* [Toepassingspakketten](https://docs.microsoft.com/azure/batch/batch-application-packages):
  * Verpakt met behulp van ZIP-bestanden die een of meer bestanden voor de toepassing, uploaden via de Azure-portal en geef het pakket in de configuratie van de groep. Wanneer de pool-VM's zijn gemaakt, de ZIP-bestanden worden gedownload en de bestanden hebt uitgepakt.
* Resourcebestanden:
  * Toepassingsbestanden zijn geüpload naar Azure blob-opslag en u verwijzingen naar de bestanden in de [pool-begintaak](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask). Wanneer een groep VM's zijn gemaakt, worden de resourcebestanden worden gedownload op elke virtuele machine.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Betalen voor gebruik licentieverlening voor vooraf geïnstalleerde toepassingen

De toepassingen die worden gebruikt en een vergoeding voor licentieverlening hebben moeten worden opgegeven in de configuratie van de pool.

* Geef de `applicationLicenses` eigenschap wanneer [het maken van een groep](https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body).  De volgende waarden kunnen worden opgegeven in de matrix van tekenreeksen - "vray", "arnold", "3dsmax", 'maya'.
* Wanneer u een of meer toepassingen opgeeft, wordt de kosten van deze toepassingen toegevoegd aan de kosten van de virtuele machines.  Prijzen van de toepassing worden weergegeven op de [Azure Batch-pagina met prijzen](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

> [!NOTE]
> Als u in plaats daarvan u verbinding maakt met een licentieserver de rendering-toepassingen te gebruiken, geeft u de `applicationLicenses` eigenschap.

U kunt de Azure portal of de Batch Explorer toepassingen selecteren en weergeven van de prijzen van toepassing.

Als een poging is gedaan om een toepassing te gebruiken, maar de toepassing nog niet is opgegeven de `applicationLicenses` eigenschap van de configuratie van de groep of kiest, wordt er geen bereik een licentieserver en vervolgens het uitvoeren van toepassingen is mislukt met een licentiefout en de afsluitcode dan nul.

### <a name="environment-variables-for-pre-installed-applications"></a>Omgevingsvariabelen voor vooraf geïnstalleerde toepassingen

Als u de opdrachtregel voor rendering-taken maken, moet de locatie van de installatie van de rendering toepassing uitvoerbare bestanden worden opgegeven.  Systeemomgevingsvariabelen zijn gemaakt in de Azure Marketplace VM-installatiekopieën die kunnen worden gebruikt in plaats van dat om op te geven van de daadwerkelijke paden.  Deze omgevingsvariabelen worden naast de [standaard Batch-omgevingsvariabelen](https://docs.microsoft.com/azure/batch/batch-compute-node-environment-variables) voor elke taak gemaakt.

|Toepassing|Uitvoerbare bestand van toepassing|Omgevingsvariabele|
|---------|---------|---------|
|Autodesk 3ds Max 2018.|3dsmaxcmdio.exe|3DSMAX_2018_EXEC|
|Autodesk 3ds Max 2019|3dsmaxcmdio.exe|3DSMAX_2019_EXEC|
|Autodesk Maya 2017|render.exe|MAYA_2017_EXEC|
|Autodesk Maya 2018|render.exe|MAYA_2018_EXEC|
|Chaos Group V-Ray zelfstandige|vray.exe|VRAY_3.60.4_EXEC|
Arnold 2017 vanaf de opdrachtregel|kick.exe|ARNOLD_2017_EXEC|
|2018 Arnold-opdrachtregel|kick.exe|ARNOLD_2018_EXEC|
|Blender|blender.exe|BLENDER_2018_EXEC|

### <a name="azure-vm-families"></a>Azure VM-families

Net als bij andere werkbelastingen uitvoert, rendering toepassing systeemvereisten variëren en prestatie-eisen voor taken en projecten variëren.  Een grote verscheidenheid aan VM-families zijn beschikbaar in Azure, afhankelijk van uw vereisten – de laagste kosten, beste prijs-prestatieverhouding, beste prestaties, enzovoort.
Sommige toepassingen rendering, zoals Arnold, worden op basis van CPU; anderen, zoals V-Ray en Blender cycli kunnen CPU's en/of GPU's gebruiken.
Voor een beschrijving van de beschikbare VM-reeksen en VM-grootten, [Zie VM-typen en -grootten](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="low-priority-vms"></a>Virtuele machines met lage prioriteit

Net als bij andere werkbelastingen uitvoert, kunnen de virtuele machines met lage prioriteit in Batch-pools voor rendering worden gebruikt.  VM's met lage prioriteit hetzelfde zijn als normale toegewezen virtuele machines uitvoeren, maar overtollige capaciteit voor Azure gebruiken en zijn beschikbaar voor een hoge kortingen.  De verhouding voor het gebruik van virtuele machines met lage prioriteit is dat deze virtuele machines mogelijk niet beschikbaar moet worden toegewezen of kunnen worden verschoven op elk gewenst moment, afhankelijk van de beschikbare capaciteit. Daarom gaan VM's met lage prioriteit zijn niet geschikt voor alle renderingtaken. Bijvoorbeeld, als installatiekopieën worden vele uren om weer te geven en vervolgens is het waarschijnlijk die de rendering van deze installatiekopieën wordt onderbroken en opnieuw worden opgestart vanwege VM's wordt verschoven zou niet acceptabel.

Voor meer informatie over de eigenschappen van virtuele machines met lage prioriteit en de verschillende manieren om deze te configureren met behulp van Batch, Zie [VM's met lage prioriteit gebruiken met Batch](https://docs.microsoft.com/azure/batch/batch-low-pri-vms).

## <a name="jobs-and-tasks"></a>Jobs en taken

Er is geen ondersteuning voor rendering-specifieke is vereist voor jobs en taken.  Het belangrijkste configuratie-item is de taak vanaf de opdrachtregel, die moet verwijzen naar de vereiste toepassing.
Wanneer de Azure Marketplace VM-installatiekopieën worden gebruikt, klikt u vervolgens is de aanbevolen procedure het gebruik van de omgevingsvariabelen om op te geven van het pad en de uitvoerbare toepassing.

## <a name="next-steps"></a>Volgende stappen

Voor voorbeelden van Batch rendering proberen de twee zelfstudies:

* [Rendering met de Azure CLI](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Rendering met behulp van Batch Explorer](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)

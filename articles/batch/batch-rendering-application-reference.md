---
title: Gebruik van toepassingen voor rendering - Azure Batch
description: Het gebruik van toepassingen voor rendering met Azure Batch
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 4c7fdbfcc34cdb6e10177d50bea707f1fa5f223e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53534750"
---
# <a name="rendering-applications"></a>Toepassingen weergeven

Rendering-toepassingen worden gebruikt door het maken van Batch-taken en taken. De taak vanaf de opdrachtregel-eigenschap geeft de juiste vanaf de opdrachtregel en parameters.  De eenvoudigste manier om de taken van de taak te maken is met de Batch Explorer-sjablonen die zijn opgegeven in [in dit artikel](https://docs.microsoft.com/azure/batch/batch-rendering-using#using-batch-explorer).  De sjablonen kunnen worden bekeken en gewijzigd van versies die zijn gemaakt, indien nodig.

Dit artikel bevat een korte beschrijving van hoe u elke renderingtoepassing uit te voeren.

## <a name="rendering-with-autodesk-3ds-max"></a>Rendering met Autodesk 3ds Max.

### <a name="renderer-support"></a>Ondersteuning voor renderer

Naast de renderers die is ingebouwd in 3ds Max, de volgende renderers zijn beschikbaar op de VM-installatiekopieën voor rendering en kan worden verwezen door de 3ds Max-scène-bestand:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Taak vanaf de opdrachtregel

Aanroepen van de `3dsmaxcmdio.exe` toepassing uit te voeren vanaf de opdrachtregel rendering op een knooppunt in de pool.  Deze toepassing is op het pad wanneer de taak wordt uitgevoerd. De `3dsmaxcmdio.exe` toepassing heeft de dezelfde beschikbare parameters als de `3dsmaxcmd.exe` toepassing, die wordt beschreven in de [3ds Max help-documentatie](https://help.autodesk.com/view/3DSMAX/2018/ENU/) (Rendering | Opdrachtregel Rendering sectie).

Bijvoorbeeld:

```
3dsmaxcmdio.exe -v:5 -rfw:0 -start:{0} -end:{0} -bitmapPath:"%AZ_BATCH_JOB_PREP_WORKING_DIR%\sceneassets\images" -outputName:dragon.jpg -w:1280 -h:720 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scenes\dragon.max"
```

Opmerkingen:

* Uiterst voorzichtig moet worden genomen om te controleren of dat de assetbestanden worden gevonden.  Zorg ervoor dat de paden zijn juist en relatieve met behulp van de **activa bijhouden** -venster, of gebruik de `-bitmapPath` parameter op de opdrachtregel.
* Zien of er problemen met de render, zoals de modeldatabase groter te zoeken naar activa, door het controleren van de `stdout.txt` bestand geschreven door 3ds Max. wanneer de taak wordt uitgevoerd.

### <a name="batch-explorer-templates"></a>Batch Explorer-sjablonen

Groep van toepassingen en -taak sjablonen kunnen worden benaderd vanaf de **galerie** in Batch Explorer.  De bronbestanden van de sjabloon zijn beschikbaar in de [Batch Explorer-gegevensopslagplaats op GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/3dsmax).

## <a name="rendering-with-autodesk-maya"></a>Rendering met Autodesk Maya

### <a name="renderer-support"></a>Ondersteuning voor renderer

Naast de renderers die is ingebouwd in Maya, worden de volgende renderers zijn beschikbaar op de VM-installatiekopieën voor rendering en kunnen worden verwezen door de 3ds Max-scène-bestand:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Taak vanaf de opdrachtregel

De `renderer.exe` opdrachtregel renderer wordt gebruikt in de opdrachtregel van de taak. De renderer voor de opdrachtregel wordt gedocumenteerd in [Maya help](http://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=GUID-EB558BC0-5C2B-439C-9B00-F97BCB9688E4).

In het volgende voorbeeld wordt een jobvoorbereidingstaak wordt gebruikt om te kopiëren van de scènebestanden en assets naar de werkmap van de taak-voorbereiding, een map voor uitvoer wordt gebruikt voor het opslaan van de installatiekopie van het rendering en frame 10 wordt weergegeven.

```
render -renderer sw -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

Voor de rendering van V-Ray, zou het scène-bestand voor Maya normaal V-Ray opgeven als de renderer.  Het kan ook worden opgegeven op de opdrachtregel:

```
render -renderer vray -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

Voor Arnold rendering, het scène-bestand voor Maya normaal gesproken geeft Arnold als de renderer.  Het kan ook worden opgegeven op de opdrachtregel:

```
render -renderer arnold -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

### <a name="batch-explorer-templates"></a>Batch Explorer-sjablonen

Groep van toepassingen en -taak sjablonen kunnen worden benaderd vanaf de **galerie** in Batch Explorer.  De bronbestanden van de sjabloon zijn beschikbaar in de [Batch Explorer-gegevensopslagplaats op GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/maya).

## <a name="next-steps"></a>Volgende stappen

Gebruik de groep van toepassingen en -taak sjablonen van de [gegevensopslagplaats in GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) met behulp van Batch Explorer.  Indien nodig, nieuwe sjablonen maken of wijzigen van een van de opgegeven sjablonen.
---
title: Hoe u een Microsoft Azure FXT Edge Filer eenheid afsluiten
description: Procedures voor het opstarten en veilig afsluiten van een Azure FXT Edge Filer-knooppunt
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: how-to
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 8d779547040da9855409d3408827af2e4acd17a7
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542864"
---
# <a name="how-to-safely-power-off-azure-fxt-edge-filer-hardware"></a>Hoe u veilig uitschakelen Azure FXT Edge Filer hardware

Hoewel u fysieke knop gebruiken kunt om over te schakelen op een afzonderlijke knooppunt, moet u deze niet gebruiken voor het afsluiten van de eenheid onder normale omstandigheden.

Nadat een Filer van Azure FXT Edge-knooppunt gebruikt als onderdeel van een cluster wordt, moet u de clustersoftware besturingselement in het Configuratiescherm om de hardware af te sluiten. 

> [!NOTE] 
> Om te voorkomen dat gegevens verloren gaan of beschadigd raken, moet u altijd de Configuratiescherm-software gebruiken om een Azure FXT Edge Filer af te sluiten. Gebruik geen fysieke knop voor het afsluiten, tenzij u wordt geadviseerd om dit te doen door de klantenservice van Microsoft.
> 
> In geval van nood elektrische, stroomkabels ontkoppelt of uw gegevens gebruiken van center elektriciteit mechanisme verbreken.

## <a name="shut-down-a-node-from-the-control-panel"></a>Een knooppunt van het Configuratiescherm afsluiten

Volg deze instructies voor het veilig uit te schakelen een Filer van Azure FXT Edge-knooppunt:

1. Meld u aan het cluster het Configuratiescherm. (Instructies in [Open de instellingen voor pagina's](fxt-cluster-create.md#open-the-settings-pages))
1. Klik op de **instellingen** tabblad en laad vervolgens de **Cluster** > **FXT knooppunten** pagina.
1. Zoeken in de lijst met clusterknooppunten, u wilt afsluiten. Klik op de **Schakel** knop in de **acties** kolom. 
1. Een ogenblik geduld. Het knooppunt wordt afgesloten en wordt uitgeschakeld.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de status-LED's en andere indicatoren in [Monitor Azure FXT Edge Filer hardwarestatus](fxt-monitor.md).
* Meer informatie over Azure FXT Edge Filer power voorziet [power kabels verbinden](fxt-network-power.md#connect-power-cables).

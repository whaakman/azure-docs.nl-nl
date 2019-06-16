---
title: Handmatig stoppen en starten van containers in Azure Container Instances
description: Informatie over het handmatig stoppen en starten van de containergroep van een in Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 8e62d106a42dfbec897e5e14cf68fd3d7fd823c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65070818"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Handmatig stoppen en starten van containers in Azure Container Instances

De [beleid voor opnieuw opstarten](container-instances-restart-policy.md) instelling van een containergroep bepaalt hoe de containerinstanties starten of stoppen van standaard. U kunt de standaardinstelling door handmatig stoppen of starten van een containergroep overschrijven.

## <a name="stop"></a>Stoppen

Stop de toepassing een actieve containergroep - bijvoorbeeld met behulp van de [az container stoppen] [ az-container-stop] opdracht of Azure-portal. Voor bepaalde container-workloads, kunt u een containergroep langlopende stoppen na een opgegeven periode op te slaan op kosten. 

*Wanneer u een containergroep in de status gestopt, wordt beëindigd en de containers in de groep wordt gerecycled. Deze behoudt niet de containerstatus.*

Wanneer de containers gerecycled zijn, de [resources](container-instances-container-groups.md#resource-allocation) ongedaan wordt gemaakt en facturering wordt gestopt voor de containergroep.

De actie bij stoppen heeft geen effect als de containergroep is al beëindigd (is een geslaagd of mislukt). Bijvoorbeeld, beëindigt een containergroep met eenmalig worden uitgevoerd-container-taken die zijn uitgevoerd in de status geslaagd. Probeert te stoppen van de groep in deze status kan de status niet wijzigen. 

## <a name="start"></a>Start

Wanneer een containergroep is gestopt - kun ofwel omdat de containers op hun eigen beëindigd of als u de groep - handmatig gestopt u de containers. Gebruik bijvoorbeeld de [az container start] [ az-container-start] opdracht of Azure-portal handmatig starten van de containers in de groep. Als de installatiekopie van de container voor elke container wordt bijgewerkt, wordt een nieuwe afbeelding opgehaald. 

Starten van een containergroep begint met een nieuwe implementatie met de dezelfde containerconfiguratie. Deze actie kunt u snel opnieuw kunt gebruiken een bekende container groep-configuratie die naar verwachting werkt. U hoeft te maken van een nieuwe containergroep voor dezelfde werkbelasting uitvoeren.

Alle containers in een containergroep worden gestart door deze actie. U kunt een specifieke container niet starten in de groep.

Nadat u handmatig starten of opnieuw starten van een containergroep, de container groep wordt uitgevoerd op basis van de geconfigureerde beleid voor opnieuw opstarten.
  
## <a name="restart"></a>Opnieuw starten

U kunt een containergroep opnieuw starten terwijl deze wordt uitgevoerd - bijvoorbeeld met behulp van de [az container opnieuw] [ az-container-restart] opdracht. Deze actie start alle containers in de containergroep. Als de installatiekopie van de container voor elke container wordt bijgewerkt, wordt een nieuwe afbeelding opgehaald. 

Opnieuw starten van een containergroep is handig als u wilt een implementatie-probleem op te lossen. Bijvoorbeeld, als een tijdelijke beperking wordt voorkomen uw containers uitgevoerd dat, opnieuw opstarten van de groep kan het probleem opgelost.

Alle containers in een containergroep opnieuw opgestart door deze actie. U kunt een specifieke container niet opnieuw in de groep.

Nadat u een containergroep handmatig opnieuw opstarten, de container groep wordt uitgevoerd op basis van de geconfigureerde beleid voor opnieuw opstarten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beleidsinstellingen opnieuw](container-instances-restart-policy.md) in Azure Container Instances.

Naast het handmatig een containergroep met de bestaande configuratie te starten en stoppen, kunt u [bijwerken van de instellingen](container-instances-update.md) van een actieve containergroep.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop

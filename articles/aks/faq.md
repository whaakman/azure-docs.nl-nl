---
title: Veelgestelde vragen over Azure Container Service
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Azure Container Service.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 73c49510512c9148f4fee98423b14770fa8602b9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Veelgestelde vragen over Azure Container Service (AKS)

Dit artikel adressen regelmatige vragen over Azure Container Service (AKS).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Welke Azure-regio's Azure Container Service (AKS) hebben? 

- Canada - midden 
- Canada - oost 
- VS - midden 
- VS - oost 
- Zuidoost-Azië 
- West-Europa 
- VS - west 2 

## <a name="when-will-additional-regions-be-added"></a>Wanneer u meer regio's worden toegevoegd? 

Als u meer regio's worden toegevoegd wanneer de vraag toeneemt.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Gelden er beveiligingsupdates voor AKS knooppunten? 

OS-beveiligingspatches worden toegepast op de knooppunten in het cluster volgens een schema 's nachts, maar een herstart wordt niet uitgevoerd. Indien nodig, kunnen knooppunten opnieuw worden gestart via de portal of Azure CLI. Bij het bijwerken van een cluster, de installatiekopie van het meest recente Ubuntu wordt gebruikt en alle beveiligingspatches zijn toegepast (met een opnieuw opstarten).

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>U kunt beter klanten gebruik ACS of AKSs? 

Gezien het feit dat Azure Container Service (AKS) GA op een later tijdstip wordt, wordt aangeraden dat u het implementatiemodel bouwt, ontwikkeling en tests-clusters in AKS maar productie-clusters in de ACS-Kubernetes.  

## <a name="when-will-acs-be-deprecated"></a>Als ACS afgeschaft? 

ACS wordt afgeschaft rond de tijd die AKS algemene beschikbaarheid wordt. Hebt u 12 maanden na die datum clusters migreren naar AKS. U kunt alle ACS-bewerkingen uitvoeren tijdens de periode van 12 maanden.

## <a name="does-aks-support-node-autoscaling"></a>Ondersteunt AKS knooppunt automatisch schalen? 

Knooppunt automatisch schalen wordt niet ondersteund, maar is in het overzicht. U kunt Kijk eens naar dit open-source [automatisch schalen implementatie][auto-scaler].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Waarom worden twee resourcegroepen met AKS gemaakt? 

Elk cluster Azure Container Service (AKS) bevindt zich in twee resourcegroepen. De eerste door u is gemaakt en alleen de bron AKS bevat. De tweede resourcegroep wordt automatisch gemaakt tijdens de implementatie en bevat alle infrastructurele clusterbronnen zoals virtuele machines, netwerken en opslagbronnen. Deze resourcegroep wordt gemaakt voor het opruimen van de eenvoudige resource. 

De automatisch gemaakte resourcegroep heeft een naam die lijkt op:

```
MC_myResourceGRoup_myAKSCluster_eastus
```

Wanneer u Azure-resources moet worden gebruikt met het cluster Kubernetes zoals storage-accounts of gereserveerde openbare IP-adres toevoegt, moeten deze resources worden gemaakt in de automatische resourcegroep gemaakt.   

## <a name="is-azure-key-vault-integrated-with-aks"></a>Is Azure Sleutelkluis geïntegreerd met AKS? 

Nee, is het niet, maar deze integratie is gepland. In de tussentijd kunt u proberen om de volgende oplossing van [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  
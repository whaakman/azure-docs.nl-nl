---
title: Operator aanbevolen procedures - beheer van installatiekopieën voor Container in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor informatie over het beheren en beveiligen van containerinstallatiekopieën in Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mlearned
ms.openlocfilehash: 3feadaca361950df2a09f8da33fe380fc3763763
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614816"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor het beheer van container-installatiekopie en beveiliging in Azure Kubernetes Service (AKS)

Als u toepassingen ontwikkelen en in Azure Kubernetes Service (AKS uitvoeren), is de beveiliging van uw containers en containerinstallatiekopieën belangrijkste overweging. Containers die verouderd zijn basis afbeeldingen of niet-gepatchte toepassing runtimes introduceren een beveiligingsrisico en een mogelijke aanvalsvector. Om te beperken deze risico's, moet u de hulpprogramma's die zoeken en oplossen van problemen in uw containers op build-tijd, evenals runtime integreren. De eerder in het proces dat de kwetsbaarheid of verouderd basisinstallatiekopie wordt geblokkeerd, is het veiliger het cluster. In dit artikel *containers* betekent dat zowel de containerinstallatiekopieën worden opgeslagen in een container registry, en de actieve containers.

In dit artikel richt zich op het beveiligen van uw containers in AKS. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Zoeken en afbeelding beveiligingsproblemen herstellen
> * Automatisch activeren en opnieuw implementeren van containerinstallatiekopieën wanneer een basisinstallatiekopie wordt bijgewerkt

U kunt ook de aanbevolen procedures voor lezen [cluster security][best-practices-cluster-security] and for [pod security][best-practices-pod-security].

## <a name="secure-the-images-and-run-time"></a>Beveiligen van de installatiekopieën en uitvoeringstijd

**Aanbevolen procedurerichtlijn** - Scan van uw containerinstallatiekopieën op beveiligingsproblemen en alleen installatiekopieën die zijn geslaagd voor de validatie te implementeren. Regelmatig de basisinstallatiekopieën en runtime voor de toepassing bijwerken en opnieuw implementeren van workloads in de AKS-cluster.

Een probleem met de acceptatie van werkbelastingen op basis van een container met het controleren van de beveiliging van installatiekopieën en runtime die wordt gebruikt om uw eigen toepassingen te bouwen. Hoe zorgt u ervoor dat u niet leiden beveiligingsproblemen in uw implementaties tot? De implementatiewerkstroom van uw moet bevatten een proces voor het scannen van installatiekopieën van containers met behulp van hulpprogramma's zoals [Twistlock][twistlock] or [Aqua][aqua], en vervolgens dat alleen geverifieerd afbeeldingen worden geïmplementeerd.

![Scannen en herstellen van containerinstallatiekopieën, valideren en implementeren](media/operator-best-practices-container-security/scan-container-images-simplified.png)

In het voorbeeld van een echte, kunt u een continue integratie en continue implementatie (CI/CD)-pijplijn kunt gebruiken voor het automatiseren van de installatiekopie-scans, verificatie en -implementaties. Azure Container Registry bevat deze beveiligingslekken scanmogelijkheden.

## <a name="automatically-build-new-images-on-base-image-update"></a>Automatisch maken van nieuwe installatiekopieën op updates van basisinstallatiekopieën

**Aanbevolen procedurerichtlijn** - als voor het gebruik van de basisinstallatiekopieën voor toepassingsinstallatiekopieën van, automation gebruiken om te maken van nieuwe afbeeldingen wanneer de basisinstallatiekopie wordt bijgewerkt. Als de basisinstallatiekopieën doorgaans beveiligingsverbeteringen bevatten, de containerinstallatiekopieën voor alle downstream-toepassing bijwerken

Telkens wanneer die een basisinstallatiekopie wordt bijgewerkt, moeten eventuele downstream containerinstallatiekopieën ook worden bijgewerkt. Deze bouwproces moet worden geïntegreerd in de validatie en implementatie-pijplijnen zoals [Azure pijplijnen][azure-pipelines] of Jenkins. Deze pipelines zorgt ervoor dat uw toepassingen worden uitgevoerd op de bijgewerkte op basis van installatiekopieën. Zodra de containerinstallatiekopieën van uw toepassing worden gevalideerd, kunnen de AKS-implementaties voor het uitvoeren van de meest recente, beveiligde installatiekopieën worden bijgewerkt.

Azure Container Registry taken kunt containerinstallatiekopieën ook automatisch laten bijwerken wanneer de basisinstallatiekopie wordt bijgewerkt. Deze functie kunt u voor het bouwen van een klein aantal basisinstallatiekopieën en regelmatig bewaring bijgewerkt met oplossingen voor bugs en beveiliging.

Zie voor meer informatie over updates van de basisinstallatiekopie [automatiseren installatiekopie is gebaseerd op updates van basisinstallatiekopieën met Azure Container Registry taken][acr-base-image-update].

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op over het beveiligen van uw containers. Voor het implementeren van sommige van deze gebieden, Zie de volgende artikelen:

* [Compileren van installatiekopieën op updates van basisinstallatiekopieën met Azure Container Registry taken automatiseren][acr-base-image-update]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md

---
title: Operator aanbevolen procedures - beheer van installatiekopieën voor Container in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor informatie over het beheren en beveiligen van containerinstallatiekopieën in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: 20d3d3d755a35927550064ddfdf3b983348f93d3
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109766"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor het beheer van container-installatiekopie en beveiliging in Azure Kubernetes Service (AKS)

Als u toepassingen ontwikkelen en in Azure Kubernetes Service (AKS uitvoeren), is de beveiliging van uw containers belangrijkste overweging. Containers die verouderd basisinstallatiekopieën of runtime voor de toepassing zijn introduceren een beveiligingsrisico en een mogelijke aanvalsvector. Om te beperken deze risico's, moet u de hulpprogramma's die zoeken en oplossen van problemen in uw containers integreren.

Als u toepassingen ontwikkelen en in Azure Kubernetes Service (AKS uitvoeren), is de beveiliging van uw containers en containerinstallatiekopieën belangrijkste overweging. Containers die verouderd zijn basis afbeeldingen of niet-gepatchte toepassing runtimes introduceren een beveiligingsrisico en een mogelijke aanvalsvector. Om te beperken deze risico's, moet u de hulpprogramma's die zoeken en oplossen van problemen in uw containers op build-tijd, evenals runtime integreren. De eerder in het proces dat de kwetsbaarheid of verouderd basisinstallatiekopie wordt geblokkeerd, is het veiliger het cluster. In dit artikel *containers* betekent dat zowel de containerinstallatiekopieën worden opgeslagen in een container registry, en de actieve containers.

In dit artikel richt zich op het beveiligen van uw containers in AKS. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Zoeken en afbeelding beveiligingsproblemen herstellen
> * Gebruik van een vertrouwde register met digitaal ondertekende containerinstallatiekopieën
> * Automatisch activeren en opnieuw implementeren van containerinstallatiekopieën wanneer een basisinstallatiekopie wordt bijgewerkt

U kunt ook de aanbevolen procedures voor lezen [cluster security] [ best-practices-cluster-security] en voor [pod security][best-practices-pod-security].

## <a name="secure-the-images-and-run-time"></a>Beveiligen van de installatiekopieën en uitvoeringstijd

**Aanbevolen procedurerichtlijn** - Scan van uw containerinstallatiekopieën op beveiligingsproblemen en alleen installatiekopieën die zijn geslaagd voor de validatie te implementeren. Regelmatig de basisinstallatiekopieën en runtime voor de toepassing bijwerken en opnieuw implementeren van workloads in de AKS-cluster.

Een probleem met de acceptatie van werkbelastingen op basis van een container met het controleren van de beveiliging van installatiekopieën en runtime die wordt gebruikt om uw eigen toepassingen te bouwen. Hoe zorgt u ervoor dat u niet leiden beveiligingsproblemen in uw implementaties tot? De implementatiewerkstroom van uw moet bevatten een proces voor het scannen van installatiekopieën van containers met behulp van hulpprogramma's zoals [Twistlock] [ twistlock] of [Aqua][aqua], en vervolgens alleen kunt u geverifieerde installatiekopieën worden geïmplementeerd.

![Scannen en herstellen van containerinstallatiekopieën, valideren en implementeren](media/operator-best-practices-container-security/scan-container-images-simplified.png)

In het voorbeeld van een echte, kunt u een continue integratie en continue implementatie (CI/CD)-pijplijn kunt gebruiken voor het automatiseren van de installatiekopie-scans, verificatie en -implementaties. Azure Container Registry bevat deze beveiligingslekken scanmogelijkheden.

## <a name="use-a-trusted-registry"></a>Gebruik van een vertrouwde register

**Aanbevolen procedurerichtlijn** : beperken van de installatiekopieregisters die schillen en implementaties kunnen gebruiken. Dat alleen vertrouwde registers waar u valideren en beheren van de installatiekopieën die beschikbaar zijn.

Voor extra beveiliging kunt u uw containerinstallatiekopieën ook digitaal ondertekenen net zoals u kunt uw toepassingscode digitaal ondertekenen. U vervolgens toestaan alleen AKS ondertekende installatiekopieën implementeren. Deze procedure biedt een extra beveiligingslaag in dat u AKS te kunnen halen alleen afbeeldingen digitaal ondertekende en vertrouwde door u, niet alleen afbeeldingen die een controle door een beveiligingslek doorgeven beperken. U ook voor zorgen dat de container-installatiekopie niet is geknoeid en door een installatiekopie met exact dezelfde naam vervangen.

Vertrouwde registers die digitaal zijn ondertekend containerinstallatiekopieën bieden complexiteit toevoegen aan uw omgeving, maar mogelijk zijn vereist voor bepaalde beleid of naleving van regelgeving. Azure Container Registry ondersteunt het gebruik van vertrouwde registers en installatiekopieën ondertekend.

Zie voor meer informatie over installatiekopieën van digitaal ondertekende [inhoud vertrouwen in Azure Container Registry][acr-content-trust].

## <a name="automatically-build-new-images-on-base-image-update"></a>Automatisch maken van nieuwe installatiekopieën op updates van basisinstallatiekopieën

**Aanbevolen procedurerichtlijn** - als voor het gebruik van de basisinstallatiekopieën voor toepassingsinstallatiekopieën van, automation gebruiken om te maken van nieuwe afbeeldingen wanneer de basisinstallatiekopie wordt bijgewerkt. Als de basisinstallatiekopieën doorgaans beveiligingsverbeteringen bevatten, de containerinstallatiekopieën voor alle downstream-toepassing bijwerken

Telkens wanneer die een basisinstallatiekopie wordt bijgewerkt, moeten eventuele downstream containerinstallatiekopieën ook worden bijgewerkt. Deze bouwproces moet worden geïntegreerd in de validatie en implementatie-pijplijnen zoals [Azure pijplijnen] [ azure-pipelines] of Jenkins. Deze pipelines zorgt ervoor dat uw toepassingen worden uitgevoerd op de bijgewerkte op basis van installatiekopieën. Zodra de containerinstallatiekopieën van uw toepassing worden gevalideerd, kunnen de AKS-implementaties voor het uitvoeren van de meest recente, beveiligde installatiekopieën worden bijgewerkt.

Azure Container Registry taken kunt containerinstallatiekopieën ook automatisch laten bijwerken wanneer de basisinstallatiekopie wordt bijgewerkt. Deze functie kunt u voor het bouwen van een klein aantal basisinstallatiekopieën en regelmatig bewaring bijgewerkt met oplossingen voor bugs en beveiliging.

Zie voor meer informatie over updates van de basisinstallatiekopie [automatiseren installatiekopie is gebaseerd op updates van basisinstallatiekopieën met Azure Container Registry taken][acr-base-image-update].

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op over het beveiligen van uw containers. Voor het implementeren van sommige van deze gebieden, Zie de volgende artikelen:

* [Compileren van installatiekopieën op updates van basisinstallatiekopieën met Azure Container Registry taken automatiseren][acr-base-image-update]
* [Inhoud vertrouwen in Azure Container Registry][acr-content-trust]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-content-trust]: ../container-registry/container-registry-content-trust.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md

---
title: Overzicht van Jenkins en Azure | Microsoft Docs
description: De build Jenkins hosten en automatiseringsserver in Azure implementeren en gebruiken van Azure berekenings- en -resources uit te breiden uw continue integratie en implementatie pijplijnen (CI/CD).
services: jenkins
author: rloutlaw
manager: justhe
ms.service: jenkins
ms.devlang: NA
ms.topic: article
ms.workload: na
ms.date: 08/22/2017
ms.author: routlaw
ms.custom: mvc
ms.openlocfilehash: daa202ddf0dc934c491ead3951ddc4fdc3dd819c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-and-jenkins"></a>Azure en Jenkins

[Jenkins](https://jenkins.io/) een populaire open-source automation-server gebruikt voor het instellen van continue integratie en levering (CI/CD) voor uw software-projecten. U kunt uw implementatie Jenkins in Azure hosten of breid uw bestaande Jenkins-configuratie met behulp van Azure-resources. Jenkins invoegtoepassingen zijn ook beschikbaar voor het vereenvoudigen van CI/CD van uw toepassingen naar Azure.

In dit artikel bevat een inleiding tot het gebruik van Azure met Jenkins, met gedetailleerde informatie over de kern van het Azure functies beschikbaar zijn voor Jenkins gebruikers. Als u wilt beginnen met een eigen server Jenkins in Azure, Zie onze [Quick Start](install-jenkins-solution-template.md).

## <a name="host-your-jenkins-servers-in-azure"></a>Uw Jenkins hostservers in Azure

Host Jenkins in Azure uw automation build centraliseren en schalen van uw implementatie naarmate de behoeften van uw software-projecten groeien. U kunt Jenkins implementeren in Azure worden verkregen met:
 
- [De sjabloon van de oplossing Jenkins](install-jenkins-solution-template.md) in Azure Marketplace.
- [Virtuele machines in Azure](/azure/virtual-machines/linux/overview). Zie onze [zelfstudie](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) een Jenkins-exemplaar maken op een virtuele machine.
- Op een Kubernetes cluster dat wordt uitgevoerd [Azure Container Service](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough), Zie onze [how-to](/azure/container-service/kubernetes/container-service-kubernetes-jenkin).

Bewaken en beheren van uw Azure-Jenkins met [logboekanalyse](/azure/log-analytics/log-analytics-overview), [Operations Management Suite](/azure/operations-management-suite/operations-management-suite-overview), en [Azure CLI] (/ azure-cli-overzicht).

## <a name="scale-your-build-automation-on-demand"></a>Schalen van uw build-automatisering op aanvraag

Build agents toevoegen aan uw bestaande Jenkins implementatie de capaciteit van de build Jenkins als het aantal builds en complexiteit van uw taken schalen en pijplijnen verhogen. U kunt deze uitvoeren agents voor virtuele machines in Azure bouwen met behulp van de [Azure VM Agents invoegtoepassing](jenkins-azure-vm-agents.md). Zie onze [zelfstudie](/azure/jenkins/jenkins-azure-vm-agents) voor meer informatie.

Eenmaal is geconfigureerd met een [Azure service-principal](/azure/azure-resource-manager/resource-group-overview), Jenkins taken en pijplijnen kunnen deze referentie om te gebruiken:

- Veilig opslaan en archiveren bouwen artefacten [Azure Storage](/azure/storage/common/storage-introduction) met behulp van de [Azure Storage-invoegtoepassing](https://plugins.jenkins.io/windows-azure-storage). Controleer de [Jenkins opslag how-to](/azure/storage/common/storage-java-jenkins-continuous-integration-solution) voor meer informatie.
- Beheren en configureren van Azure-resources met de [Azure CLI](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Uw code implementeren in Azure-services

Gebruik Jenkins invoegtoepassingen gebruikt voor het implementeren van uw toepassingen naar Azure als onderdeel van uw pijplijnen Jenkins CI/CD. Implementeren in [Azure App Service](/azure/app-service/) en [Azure Container Service](/azure/container-service/kubernetes/) kunt u fase, test en LDR-updates voor uw toepassingen zonder het beheren van de onderliggende infrastructuur.

 Invoegtoepassingen zijn beschikbaar voor implementatie op de volgende services en omgevingen:

- [Azure-Web-App op Linux](/azure/app-service/containers/app-service-linux-intro). Zie de [zelfstudie](java-deploy-webapp-tutorial.md) aan de slag.
- [Azure-Web-App](/azure/app-service/app-service-web-overview). Zie de [how-to](deploy-Jenkins-app-service-plugin.md) aan de slag.


---
title: 'DevOps voor kunstmatige intelligentie (AI)-toepassingen: continue integratie pijplijn in Azure met behulp van Docker, Kubernetes & Python Flask-toepassing maken'
description: 'DevOps voor kunstmatige intelligentie (AI)-toepassingen: continue integratie pijplijn in Azure met behulp van Docker en Kubernetes maken'
services: machine-learning, team-data-science-process
documentationcenter: ''
author: jainr
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jainr
ms.openlocfilehash: 233da393bb9e030d885ce588f4841dc1c707c1cb
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836263"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps voor kunstmatige intelligentie (AI)-toepassingen: continue integratie pijplijn in Azure met behulp van Docker en Kubernetes maken
Er zijn voor een toepassing AI vaak twee streams werk, Gegevenswetenschappers opbouwen van de machine learning-modellen en App-ontwikkelaars opbouwen van de toepassing en het beschikbaar te maken voor eindgebruikers te gebruiken. In dit artikel ziet u hoe u een continue integratie (CI) implementeren / pipeline-continue-levering (CD) voor een AI-toepassing. AI-toepassing is een combinatie van toepassingscode embedded met een pretrained machine learning (ML)-model. Voor dit artikel wordt bij het ophalen van een pretrained model uit een persoonlijke Azure blob storage-account is, kan een S3 AWS-account. We gebruiken een eenvoudige python flask-toepassing voor het artikel.

> [!NOTE]
> Dit is een van de verschillende manieren DIE CI/CD kan worden uitgevoerd. Er zijn alternatieven voor de tooling en andere vereisten die hieronder worden vermeld. Als er aanvullende inhoud ontwikkelt, publiceren die.
>
>

## <a name="github-repository-with-document-and-code"></a>GitHub-opslagplaats met document en code
U kunt de broncode van de downloaden [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). Een [gedetailleerde zelfstudie](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) is ook beschikbaar.

## <a name="pre-requisites"></a>Vereisten
Hier volgen de vereisten voor het volgen van de CI/CD-pijplijn die hieronder worden beschreven:
* [Visual Studio Team Services-Account](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student)
* [Azure-CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure Container Service (AKS)-cluster Kubernetes uitgevoerd](https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Azure Container register (ACR)-account](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal)
* [Kubectl om opdrachten uitvoeren op Kubernetes cluster installeren.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) We zullen deze ophalen van de configuratie van de ACS-cluster nodig. 
* De opslagplaats met uw GitHub-account vertakken.

## <a name="description-of-the-cicd-pipeline"></a>Beschrijving van de CI/CD-pipeline
De pijplijn gang gezet voor elke nieuwe doorvoer, de suite test uitvoeren als de stadia van de test duurt de laatste build-deze pakketten in een Docker-container. De container vervolgens is geïmplementeerd met behulp van Azure containerservice (ACS) en installatiekopieën worden veilig opgeslagen in Azure container register (ACR). ACS Kubernetes voor het beheren van container cluster wordt uitgevoerd, maar u kunt Docker Swarm of Mesos.

De toepassing haalt veilig de laatste model van een Azure Storage-account en pakketten die als onderdeel van de toepassing. De geïmplementeerde toepassing heeft de app-code en geleverd als een enkele container ML-model. Hiermee worden de app-ontwikkelaars en gegevens die zijn verzameld, om ervoor te zorgen dat hun productie-app altijd de meest recente code wordt uitgevoerd met de meest recente ML-model losgekoppeld.

Hieronder volgt de pipeline-architectuur. 

![Architectuur](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Stappen van de CI/CD-pipeline
1. Ontwikkelaar werken op de IDE van hun keuze op de toepassingscode.
2. Ze doorvoert de code naar broncodebeheer van hun keuze (VSTS heeft goede ondersteuning voor verschillende bron-besturingselementen)
3. Afzonderlijk, werkt het wetenschappelijk gegevens op hun model te ontwikkelen.
4. Zodra tevreden, dat zij het model publiceren naar een model-opslagplaats, in dit geval gebruiken we een blob storage-account. Dit kan eenvoudig worden vervangen door de Azure ML Workbench Model management-service via de REST-API's.
5. Een build wordt gestart in VSTS op basis van het doorvoeren in GitHub.
6. VSTS bouwen pijplijn haalt de laatste model van de Blob-container en een container maken.
7. VSTS duwt de installatiekopie naar de opslagplaats voor persoonlijke installatiekopieën in Azure Container register
8. Volgens een vooropgezet schema (elke nacht), wordt versie pijplijn gestart.
9. De installatiekopie van de meest recente van ACR is opgehaald en geïmplementeerd op de cluster Kubernetes op ACS.
10. Gebruikers-aanvraag voor de app doorloopt NTDS DNS-server.
11. DNS-server de aanvraag voor de load balancer is geslaagd en verzendt het antwoord terug naar de gebruiker.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg de [zelfstudie]((https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md)) volgt u de details en uw eigen CI/CD-pijplijn voor uw toepassing implementeren.

## <a name="references"></a>Verwijzingen
* [Team gegevens wetenschappelijke processen (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/en-us/azure/machine-learning/service/)
* [Visual Studio teamservices (VSTS)](https://www.visualstudio.com/vso/)
* [Azure Kubernetes Services (AKS)](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)
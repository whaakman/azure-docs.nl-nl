---
title: Maak continue integratie Azure pijplijn - Team Data Science Process
description: 'DevOps voor kunstmatige intelligentie (AI)-toepassingen: Het maken van de pijplijn voor continue integratie in Azure met behulp van Docker en Kubernetes'
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 05/22/2018
ms.author: tdsp
ms.custom: seodec18, previous-author=jainr, previous-ms.author=jainr
ms.openlocfilehash: d99149f8112c19a07208523a1ee26ba1c36e5362
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474221"
---
# <a name="creating-continuous-integration-pipeline-on-azure-using-docker-kubernetes-and-python-flask-application"></a>Het maken van de pijplijn voor continue integratie in Azure met behulp van Docker, Kubernetes en Python Flask-toepassing
Er zijn vaak twee stromen werkzaamheden, Data Scientists, het bouwen van machine learning-modellen en App-ontwikkelaars het bouwen van de toepassing en beschikbaar te maken voor eindgebruikers te gebruiken voor een toepassing AI. In dit artikel laten we zien hoe u voor het implementeren van een continue integratie (CI) / pijplijn voor continue levering (CD) voor een AI-toepassing. AI-toepassing is een combinatie van een model dat machine learning (ML) zijn ingebed toepassingscode. In dit artikel we bij het ophalen van een pretrained model vanuit een persoonlijke Azure-blob storage-account, wordt een AWS S3-account. Voor het artikel gebruiken we een eenvoudige python flask-toepassing.

> [!NOTE]
> Dit is een van de verschillende manieren DIE CI/CD kunnen worden uitgevoerd. Er zijn alternatieven voor de hulpprogramma's en andere vereisten die hieronder worden vermeld. Als we aanvullende inhoud ontwikkelen, publiceren we die.
>
>

## <a name="github-repository-with-document-and-code"></a>GitHub-opslagplaats met document- en -code
U kunt de broncode van downloaden [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). Een [gedetailleerde zelfstudie](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) is ook beschikbaar.

## <a name="pre-requisites"></a>Vereisten
Hier volgen de vereisten voor het volgen van de CI/CD-pijplijn die hieronder worden beschreven:
* [Azure DevOps-organisatie](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
* [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure Container Service (AKS)-cluster met Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Azure Container Registry (ACR)-account](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)
* [Kubectl voor het uitvoeren van opdrachten op basis van Kubernetes-cluster installeren.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) We moeten deze configuratie ophalen uit ACS-cluster. 
* Fork van de opslagplaats naar uw GitHub-account.

## <a name="description-of-the-cicd-pipeline"></a>Beschrijving van de CI/CD-pijplijn
De pijplijn gang gezet voor elke nieuwe doorvoer, de suite test uitvoeren als de test-passen aan de nieuwste build-deze pakketten in een Docker-container. De container wordt vervolgens geïmplementeerd met behulp van Azure Container Service (ACS) en installatiekopieën worden veilig opgeslagen in Azure Container Registry (ACR). ACS Kubernetes voor het beheren van container-cluster wordt uitgevoerd, maar u kunt Docker Swarm of Mesos.

De toepassing haalt veilig het laatste model van een Azure Storage-account en pakketten die als onderdeel van de toepassing. De geïmplementeerde toepassing heeft de app-code en het ML-model geleverd als één container. Hiermee worden de app-ontwikkelaars en data scientists, om ervoor te zorgen dat de productie-app altijd de meest recente code wordt uitgevoerd met de meest recente ML-model losgekoppeld.

De pijplijn-architectuur wordt hieronder. 

![Architectuur](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Stappen voor de CI/CD-pijplijn
1. Ontwikkelaar werken op de IDE van hun keuze in de toepassingscode.
2. Ze doorvoeren de code in broncodebeheer van hun keuze (Azure DevOps biedt goede ondersteuning voor verschillende besturingselementen voor gegevensbronnen)
3. Afzonderlijk, werken de gegevenswetenschapper over het ontwikkelen van hun model.
4. Zodra tevreden, dat zij het model publiceren naar een model-opslagplaats, in dit geval gebruiken we een blob storage-account. 
5. Een bewerking wordt gestart in Azure DevOps op basis van het doorvoeren in GitHub.
6. Azure DevOps-Build-pijplijn haalt de nieuwste model uit Blob-container en wordt een container gemaakt.
7. Azure DevOps wordt de image gepusht naar persoonlijke installatiekopie-opslagplaats in Azure Container Registry
8. Release-pijplijn wordt volgens een vast schema ('s nachts) gestart.
9. Meest recente installatiekopie vanuit ACR is opgehaald en worden geïmplementeerd op de Kubernetes-cluster in ACS.
10. Gebruikers-aanvraag voor de app gaat via DNS-server.
11. DNS-server de aanvraag tot de load balancer is geslaagd en stuurt het antwoord terug naar de gebruiker.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg de [zelfstudie](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) volgt u de details en implementeren van uw eigen CI/CD-pijplijn voor uw toepassing.

## <a name="references"></a>Verwijzingen
* [Team Data Science Process (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/azure/machine-learning/service/)
* [Azure DevOps](https://www.visualstudio.com/vso/)
* [Azure Kubernetes-Services (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)

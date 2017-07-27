---
title: Azure VM-agents gebruiken voor continue integratie met Jenkins
description: Azure VM-agents als Jenkins-slaves.
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 5f2df414b4d0e8798b7ed6d90d0ea0fb79d42fc2
ms.contentlocale: nl-nl
ms.lasthandoff: 06/21/2017

---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Azure VM-agents gebruiken voor continue integratie met Jenkins

In deze Quickstart ziet u hoe u met de invoegtoepassing Jenkins Azure VM Agents een on-demand Linux-agent (Ubuntu) maakt in Azure.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart:

* Als u nog geen Jenkins-master hebt, kunt u beginnen met de [oplossingssjabloon](install-jenkins-solution-template.md). 
* Raadpleeg [Een Azure-service-principal maken met de Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) als u nog geen Azure-service-principal hebt.

## <a name="install-azure-vm-agents-plugin"></a>Azure VM Agents-invoegtoepassing installeren

Als u begint vanuit de [oplossingssjabloon](install-jenkins-solution-template.md), wordt de invoegtoepassing Azure VM Agents geïnstalleerd in de Jenkins-master.

Anders installeert u de**invoegtoepassing** vanuit het dashboard van Jenkins.

## <a name="configure-the-plugin"></a>De invoegtoepassing configureren

* Klik in het dashboard van Jenkins op **Manage Jenkins -> Configure System ->**. Ga naar de onderkant van de pagina en zoek de sectie met de vervolgkeuzelijst **Add new cloud**. Selecteer **Microsoft Azure VM Agents** in de lijst.
* Selecteer een bestaand account in de vervolgkeuzelijst Azure Credentials.  Als u een nieuwe **Microsoft Azure-service-principal** wilt toevoegen, geeft u waarden op voor: Subscription ID, Client ID, Client Secret en OAuth 2.0 Token Endpoint.

![Azure-referenties](./media/jenkins-azure-vm-agents/service-principal.png)

* Klik op **Verify configuration** om er zeker van te zijn dat de configuratie juist is.
* Sla de configuratie op en ga verder met de volgende stap.

## <a name="template-configuration"></a>Sjabloonconfiguratie

### <a name="general-configuration"></a>Algemene configuratie
Configureer vervolgens een sjabloon die u wilt gebruiken voor het definiëren van een Azure VM-agent. 

* Klik op **Add** om een sjabloon toe te voegen. 
* Geef een naam op voor de nieuwe sjabloon. 
* Voer voor het label 'ubuntu' in. Dit label wordt gebruikt tijdens de taakconfiguratie.
* Selecteer de gewenste regio in de keuzelijst met invoervak.
* Selecteer de gewenste grootte van de virtuele machine.
* Geef de naam van het Azure Storage-account op of laat het veld leeg als u de standaardnaam 'jenkinsarmst' wilt gebruiken.
* Geef de bewaartijd in minuten op. Deze instelling bepaalt het aantal minuten dat Jenkins moet wachten voordat een niet-actieve agent automatisch wordt verwijderd. Geef 0 op om niet-actieve agents nooit te verwijderen.

![Algemene configuratie](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Configuratie van installatiekopie

Als u een Linux-agent (Ubuntu) wilt maken, selecteert u **Image reference** en gebruikt u de volgende configuratie als voorbeeld. Raadpleeg [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) voor de nieuwste ondersteunde Azure-installatiekopieën.

* Image Publisher: Canonical
* Image Offer: UbuntuServer
* Image Sku: 14.04.5-LTS
* Image Version: latest
* OS Type: Linux
* Launch Method: SSH
* Voer referenties van een beheerder in
* Voer het volgende in voor het script voor de initialisatie van virtuele machines:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Configuratie van installatiekopie](./media/jenkins-azure-vm-agents/image-config.png)

* Klik op **Verify Template** om de configuratie te controleren.
* Klik op **Opslaan**.

## <a name="create-a-job-in-jenkins"></a>Een taak maken in Jenkins

* Klik in het dashboard van Jenkins op **New Item**. 
* Voer een naam in, selecteer **Freestyle project** en klik op **OK**.
* Schakel op het tabblad **General** het selectievakje Restrict where this project can be run in en typ 'ubuntu' in het vak Label Expression. U ziet nu 'ubuntu' in de vervolgkeuzelijst.
* Klik op **Opslaan**.

![Een taak instellen](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Het nieuwe project bouwen

* Ga terug naar het dashboard van Jenkins.
* Klik met de rechtermuisknop op de nieuwe taak die u hebt gemaakt en klik vervolgens op **Build now**. De bewerking wordt gestart. 
* Zodra de bewerking is voltooid, gaat u naar **Console Output**. U ziet dat de bewerking op afstand is uitgevoerd in Azure.

![Console-uitvoer](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Naslaginformatie

* Video van Azure Friday: [Continuous Integration with Jenkins Using Azure VM Agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents) (Continue integratie met Jenkins met behulp van Azure VM-agents)
* Ondersteuningsinformatie en configuratie-opties: de Engelstalige wiki van Jenkins [Azure VM Agents plugin](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 



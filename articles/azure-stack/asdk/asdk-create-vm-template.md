---
title: In deze zelfstudie maakt u een Azure-Stack-VM met een sjabloon | Microsoft Docs
description: Beschrijft hoe de ASDK gebruiken voor het maken van een virtuele machine met behulp van een sjabloon predfined en een aangepaste sjabloon GitHub.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: ec44fc879abfe2a457e80f27db972ac4d7570dbd
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Zelfstudie: een virtuele machine met een community-sjabloon maken
Als een Azure-Stack operator of een gebruiker, kunt u een virtuele machine met [aangepaste sjablonen van GitHub Quick Start](https://github.com/Azure/AzureStack-QuickStart-Templates) in plaats van het implementeren van een handmatig van de Stack van Azure marketplace.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Meer informatie over Azure-Stack Quick Start-sjablonen 
> * Een virtuele machine maken met een aangepaste sjabloon voor GitHub
> * Start minikube en installeren van een toepassing

## <a name="learn-about-azure-stack-quickstart-templates"></a>Meer informatie over Azure-Stack Quick Start-sjablonen
Azure Stack Quick Start-sjablonen worden opgeslagen in de [openbare AzureStack Snelstartsjablonen opslagplaats](https://github.com/Azure/AzureStack-QuickStart-Templates) op GitHub. Deze repository bevat implementatiesjablonen van de Azure Resource Manager-die zijn getest met de Microsoft Azure Stack Development Kit (ASDK). U kunt deze gebruiken om eenvoudiger voor u Azure-Stack evalueren en de omgeving ASDK gebruiken. 

Na verloop van tijd hebben veel GitHub-gebruikers bijgedragen tot de opslagplaats, wat resulteert in een groot aantal meer dan 400 implementatiesjablonen. Deze opslagplaats is een goed startpunt voor een beter inzicht te krijgen van hoe u verschillende soorten omgevingen op Azure-Stack implementeren kunt. 

>[!IMPORTANT]
> Sommige van deze sjablonen zijn gemaakt door leden van de community en niet door Microsoft. Elke sjabloon is een licentie verleend onder een gebruiksrechtovereenkomst van de eigenaar, niet door Microsoft. Microsoft is niet verantwoordelijk voor deze sjablonen en voert geen controle voor beveiliging, compatibiliteit of prestatie. Community-sjablonen worden niet ondersteund onder Microsoft ondersteuningsprogramma of -service, en worden beschikbaar gesteld 'AS IS' zonder garantie van welke aard dan ook.

Als u bijdragen van uw Azure Resource Manager-sjablonen met GitHub wilt, moet u uw bijdrage aan de [azure-snelstartsjablonen opslagplaats](https://github.com/Azure/AzureStack-QuickStart-Templates).

Zie voor meer informatie over de GitHub-opslagplaats en hoe u kunt bijdragen aan de [Leesmij-bestand van de opslagplaats](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Een virtuele machine maken met een aangepaste sjabloon voor GitHub
In deze zelfstudie voorbeeld de [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) snelstartsjabloon met de Azure-Stack wordt gebruikt voor het implementeren van een virtuele Ubuntu 16.04 virtuele machine op AzureStack Minikube voor het beheren van een cluster kubenetes uitgevoerd.

Minikube is een hulpprogramma waarmee u gemakkelijk Kubernetes lokaal uitvoeren. Minikube voert een cluster met één knooppunt Kubernetes binnen een VM voor gebruikers wilt uitproberen Kubernetes of ontwikkelen met het dagelijkse. Biedt ondersteuning voor een eenvoudige, een cluster met één knooppunt Kubernetes uitgevoerd op een Linux-VM. Het is de snelste en meest meteen verder manier voor het ophalen van een volledig functionele Kubernetes-cluster met. Hiermee kunnen ontwikkelaars ontwikkelen en testen van toepassing op basis van Kubernetes implementaties op hun lokale computer. Qua architectuur is Minikube VM hoofd- en Agentonderdelen van het knooppunt wordt lokaal uitgevoerd:
- Master knooppunt onderdelen zoals API-Server en Scheduler etcd Server worden uitgevoerd in een enkel Linux-proces LocalKube genoemd.
- Knooppunt onderdelen van de agent worden uitgevoerd binnen de docker-containers precies zoals ze zouden worden uitgevoerd op een normale Agent-knooppunt. Van toepassing implementatie betreft is er geen verschil wanneer de toepassing wordt geïmplementeerd op een Minikube of reguliere Kubernetes cluster.

Deze sjabloon wordt geïnstalleerd voor de volgende onderdelen:

- Ubuntu 16.04 TNS VM
- [Docker-CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> De Ubuntu VM-installatiekopie (Ubuntu Server 16.04 TNS in dit voorbeeld) moet al zijn [toegevoegd aan de Stack van Azure marketplace](asdk-marketplace-item.md) voordat u deze stappen begint.

1.  Klik op **+ nieuw** > **aangepaste** > **sjabloonimplementatie**.

    ![](media/asdk-create-vm-template/1.PNG) 

2. Klik op **template bewerken**.

   ![](media/asdk-create-vm-template/2.PNG) 

3.  Klik op **snelstartsjabloon**.

       ![](media/asdk-create-vm-template/3.PNG)

4. Selecteer **101-vm-linux-minikube** van de beschikbare sjablonen met behulp van de **Selecteer een sjabloon** dropdown lijst en klik vervolgens op **OK**.  

   ![](media/asdk-create-vm-template/4.PNG)

5. Als u wijzigingen wilt aanbrengen in de sjabloon JSON die u kunt doen, als dit niet het of wanneer voltooid, klikt u op **opslaan** om de sjabloon bewerken dialoogvenster te sluiten.

   ![](media/asdk-create-vm-template/5.PNG) 

6.  Klik op **Parameters**, vult u of de beschikbare velden desgewenst wijzigen en klik op **OK**. Kies het abonnement gebruiken, maken of kies een bestaande resourcenaam van de groep en klik vervolgens op **maken** de sjabloonimplementatie van de te starten.

       ![](media/asdk-create-vm-template/6.PNG)

7. Kies het abonnement gebruiken, maken of kies een bestaande resourcenaam van de groep en klik vervolgens op **maken** de sjabloonimplementatie van de te starten.

   ![](media/asdk-create-vm-template/7.PNG)

8. De implementatie van de groep resource duurt enkele minuten voor het maken van de aangepaste sjabloon gebaseerde VM. U kunt de installatiestatus door meldingen en van de eigenschappen van de bronnengroep controleren. 

   ![](media/asdk-create-vm-template/8.PNG)

   >[!NOTE]
   > De virtuele machine wordt worden uitgevoerd wanneer de implementatie is voltooid. 

## <a name="start-minikube-and-install-an-application"></a>Start minikube en installeren van een toepassing
Nu dat de Linux-VM heeft gemaakt, kunt u zich kunt aanmelden minikube starten en installeren van een toepassing. 

1. Nadat de implementatie is voltooid, klikt u op **Connect** om het openbare IP-adres dat wordt gebruikt voor het verbinding maken met de Linux-VM weer te geven. 

   ![](media/asdk-create-vm-template/9.PNG)

2. Voer vanuit een opdrachtprompt met verhoogde bevoegdheid **mstsc.exe** verbinding met extern bureaublad openen en verbinding maken met de Linux-VM openbaar IP-adres gedetecteerd in de vorige stap. Wanneer u wordt gevraagd zich aanmelden bij xRDP, gebruikt u de referenties die u hebt opgegeven bij het maken van de virtuele machine.

   ![](media/asdk-create-vm-template/10.PNG)

3. Open Terminal-Emulator en voert u na opdrachten starten minikube:

    >    `sudo minikube start --vm-driver=none`
    >   
    >    `sudo minikube addons enable dashboard`
    >    
    >    `sudo minikube dashboard --url`

   ![](media/asdk-create-vm-template/11.PNG)

4. Open de webbrowser en Ga naar het dashboard kubernetes adres. Gefeliciteerd, u hebt nu een volledig werkend kubernetes installatie met minikube!

   ![](media/asdk-create-vm-template/12.PNG)

5. Als u wilt een voorbeeld van een toepassing implementeren, gaat u naar de pagina officiële documentatie van kubernetes, de sectie 'Minikube Cluster maken' overslaan als u al hebt gemaakt een hierboven. Gewoon gaan naar de sectie 'Uw Node.js-toepassing maken' op https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Meer informatie over Azure-Stack Quick Start-sjablonen 
> * Een virtuele machine maken met een aangepaste sjabloon voor GitHub
> * Start minikube en installeren van een toepassing


> [!div class="nextstepaction"]
> [Meer informatie over geavanceerde evaluatietaken](asdk-advanced-eval.md)
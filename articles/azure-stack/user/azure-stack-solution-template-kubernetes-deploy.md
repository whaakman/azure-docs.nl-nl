---
title: Een Cluster Kubernetes implementeren naar Azure Stack | Microsoft Docs
description: Informatie over het implementeren van een Kubernetes Cluster op Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 7cf865f0ce75d8308d6d42306e8e05852f763cae
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Een cluster Kubernetes implementeren naar Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

> [!Note]  
> De Azure Container Services (ACS) Kubernetes op Azure-Stack is private preview. Uw Azure-Stack-operator moet de toegang tot het Kubernetes Marketplace-item die nodig zijn voor het uitvoeren van de instructies in dit artikel.

Het volgende artikel kijkt met behulp van een Azure Resource Manager-oplossingssjabloon te implementeren en inrichten van de resources voor Kubernetes in een enkele, gecoördineerde bewerking. U moet de vereiste informatie verzamelen over uw Azure-Stack-installatie genereren van de sjabloon, en vervolgens implementeren op uw cloud.

## <a name="kubernetes-and-containers"></a>Kubernetes en containers

U kunt Azure Container Services (ACS) Kubernetes installeren op Azure-Stack. [Kubernetes](https://kubernetes.io) is een open source-systeem voor het automatiseren van de implementatie, schaalbaarheid en beheren van toepassingen in containers. Een [container](https://www.docker.com/what-container) deel uitmaakt van een afbeelding, vergelijkbaar met een virtuele machine. Omvat in tegenstelling tot een virtuele machine de installatiekopie van de container alleen is de resources die moet worden uitgevoerd van een toepassing, zoals de code, runtime de code, specifieke bibliotheken en -instellingen uit te voeren.

U kunt Kubernetes te gebruiken:

- Zeer schaalbaar, worden bijgewerkt, toepassingen ontwikkelen die kunnen worden geïmplementeerd in seconden. 
- Vereenvoudig het ontwerp van uw toepassing en de betrouwbaarheid te verbeteren door verschillende Helm toepassingen. [Helm](https://github.com/kubernetes/helm) is een open source verpakking hulpprogramma waarmee u kunt installeren en beheren van de levenscyclus van Kubernetes toepassingen.
- Bewaken en onderzoeken van de status van uw toepassingen met schaal en eenvoudig upgraden functionaliteit.

## <a name="prerequisites"></a>Vereisten 

Zorg ervoor dat u de juiste machtigingen hebt en dat uw Azure-Stack gereed is om te beginnen.

1. Controleer of u kunt toepassingen maken in uw tenant van Azure Active Directory (Azure AD). In dat geval moet u deze machtigingen in voor de implementatie Kubernetes.

    Zie voor instructies over het controleren van uw machtigingen [machtigingen voor Azure Active Directory controleren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

2. Een SSH openbare en persoonlijke sleutelpaar genereren aan te melden bij de Linux-VM op Azure-Stack. U moet de openbare sleutel bij het maken van het cluster.

    Zie voor instructies voor het genereren van een sleutel, [SSH-sleutel genereren](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

3. Controleer of u een geldig abonnement in uw Azure-Stack tenantportal hebt en dat er voldoende openbare IP-beschikbaar zijn adressen voor het toevoegen van nieuwe toepassingen.

## <a name="create-a-service-principal-in-azure-ad"></a>Een service-principal maken in Azure AD

1. Aanmelden bij de globale [Azure-portal](http://www.poartal.azure.com).
2. Controleer dat u bent aangemeld bij het gebruik van de Azure AD-tenant gekoppeld aan Azure-Stack.
3. Maak een Azure AD-toepassing.

    a. Selecteer **Azure Active Directory** > **+ App registraties** > **registratie van de nieuwe toepassing**.

    b. Voer een **naam** van de toepassing.

    c. Selecteer **Web-app / API**

    d. Voer `http://localhost` voor de **aanmeldings-URL**.

    c. Klik op **Maken**.

4. Noteer de **toepassings-ID**. U moet de ID bij het maken van het cluster. De ID wordt verwezen als **Client-ID voor Service-Principal**.

5. Selecteer **instellingen** > **sleutels**.

    a. Voer de **beschrijving**.

    b. Selecteer **verloopt nooit** voor **verloopt**.

    c. Selecteer **Opslaan**. Noteer de sleutel tekenreeks maken U moet de sleutel tekenreeks bij het maken van het cluster. De sleutel wordt verwezen als de **Service-Principal Clientgeheim**.



## <a name="give-the-service-principal-access"></a>De service principal toegang geven

De service principal toegang geven aan uw abonnement, zodat de principal resources kunt maken.

1.  Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **gebruikersabonnementen** > **+ toevoegen**.

3. Selecteer het abonnement dat u hebt gemaakt.

4. Selecteer **toegangsbeheer (IAM)** > Selecteer **+ toevoegen**.

5. Selecteer de **eigenaar** rol.

6. Selecteer de naam van de toepassing gemaakt voor uw service principal. U moet de naam in het zoekvak typt.

7. Klik op **Opslaan**.

## <a name="deploy-a-kubernetes-cluster"></a>Een Kubernetes-Cluster implementeren

1. Open de [Stack Azure portal](https://portal.local.azurestack.external).

2. Selecteer **+ nieuw** > **Compute** > **Kubernetes Cluster**.

    ![Oplossingssjabloon implementeren](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template.png)

3. Selecteer **Parameters** in de oplossingssjabloon implementeert.

    ![Oplossingssjabloon implementeren](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template-parameters.png)

2. Voer de **Linux de naam van gebruiker met beheerdersrechten**. Gebruikersnaam voor de virtuele Linux-Machines die deel van het cluster Kubernetes uitmaken en DVM.

3. Voer de **openbare SSH-sleutel** gebruikt voor autorisatie voor alle Linux-machines gemaakt als onderdeel van de cluster Kubernetes en DVM.

4. Voer de **tenant-eindpunt**. Dit is het Azure Resource Manager-eindpunt te maken van de resourcegroep voor het cluster Kubernetes verbinding te maken. U moet het eindpunt van uw Azure-Stack-provider voor een geïntegreerd systeem ophalen. Voor de Azure Stack Development Kit (ASDK), kunt u `https://management.local.azurestack.external`.

5. Voer de **tenant-ID** voor de tenant. Als u deze waarde te zoeken hulp nodig, Zie [tenant-ID ophalen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

6. Voer de **master profiel DNS-voorvoegsel** die uniek is voor de regio. Dit moet een regio-unieke naam, zoals `k8s-12345`. Probeer te hebt gekozen deze gelijk zijn aan de resourcegroep een naam als best practice.

    > [!Note]  
    > Gebruik een nieuwe en unieke master profiel DNS-voorvoegsel voor elk cluster.

7. Voer het aantal agents in het cluster. Deze waarde wordt aangeduid als de **aantal agents Pool-profiel**. Er kan tussen 1 en 32 liggen

8. Voer de **service-principal toepassings-ID** wordt gebruikt door de cloudprovider Kubernetes Azure.

9. Voer de **geheim voor service-principal client** die u hebt gemaakt bij het maken van de principal-servicetoepassing.

10. Voer de **Kubernetes Azure Cloud providerversie**. Dit is de versie voor de Kubernetes Azure provider. Azure Stack versies een aangepaste Kubernetes build voor elke versie van de Azure-Stack.

12. Selecteer **OK**.

### <a name="specify-the-solution-values"></a>Geef de waarden van de oplossing

1. Selecteer de **abonnement**.

2. Voer de naam van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep. De resourcenaam moet alfanumerieke en kleine letters.

3. Voer de locatie van de resourcegroep, zoals **lokale**.

4. Selecteer **maken.**

## <a name="connect-to-your-cluster"></a>Verbinding maken met uw cluster

U bent nu klaar om te verbinden met uw cluster. U moet de **kubectl**, de opdrachtregel Kubernetes-client. Hier vindt u instructies voor het verbinden met en beheren van het cluster in Azure Container Services-documentatie.   

Zie voor instructies [verbinding maken met het cluster](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough#connect-to-the-cluster).

## <a name="next-steps"></a>Volgende stappen

[Een Kubernetes Cluster toevoegen aan de Marketplace (voor de Azure-Stack-operator)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes op Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
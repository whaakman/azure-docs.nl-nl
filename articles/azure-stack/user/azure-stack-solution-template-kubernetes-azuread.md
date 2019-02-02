---
title: Implementeren van Kubernetes op Azure Stack met Azure Active Directory (Azure AD) | Microsoft Docs
description: Informatie over het implementeren van Kubernetes op Azure Stack met Azure Active Directory (Azure AD).
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
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 8d2d8d0595099081498202a3103caa10df8cf67d
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657838"
---
# <a name="deploy-kubernetes-to-azure-stack-using-azure-active-directory"></a>Kubernetes op Azure Stack met Azure Active Directory implementeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack is in preview. Azure Stack-niet-verbonden scenario wordt momenteel niet ondersteund door de Preview-versie.

U kunt de stappen in dit artikel om te implementeren en instellen van de resources voor Kubernetes, bij het gebruik van Azure Active Directory (Azure AD) als uw identity management-service in een enkele, bewerking gecoördineerde.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen, zorg ervoor dat u de juiste machtigingen hebt en dat uw Azure Stack gereed is.

1. Controleer of dat u toepassingen in uw tenant Azure Active Directory (Azure AD maken kunt). In dat geval moet u deze machtigingen in voor de Kubernetes-implementatie.

    Zie voor instructies over het controleren van uw machtigingen [machtigingen voor Azure Active Directory controleren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

1. Een SSH openbare en persoonlijke sleutelpaar genereren om aan te melden bij de Linux-VM in Azure Stack. U moet de openbare sleutel bij het maken van het cluster.

    Zie voor instructies over het genereren van een sleutel [SSH-sleutel genereren](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Controleer of u een geldig abonnement in de portal van uw Azure Stack-tenant hebben en dat u onvoldoende openbare IP hebt-adressen beschikbaar om toe te voegen nieuwe toepassingen.

    Het cluster kan niet worden geïmplementeerd naar een Azure Stack **beheerder** abonnement. U moet gebruiken een **gebruiker** abonnement. 

1. Als u geen Kubernetes-Cluster in de marketplace, neemt u contact op met uw Azure Stack-beheerder.

## <a name="create-a-service-principal"></a>Een service-principal maken

Instellen van een service-principal in Azure. De service-principal geeft uw toepassing toegang heeft tot de Azure Stack-resources.

1. Meld u aan de globale [Azure-portal](http://portal.azure.com).

1. Controleer of u aangemeld met behulp van de Azure AD-tenant die is gekoppeld aan de Azure Stack-exemplaar. U kunt de aanmelding schakelen door te klikken op het filterpictogram in de werkbalk van Azure.

    ![Selecteer de AD-tenant](media/azure-stack-solution-template-kubernetes-deploy/tenantselector.png)

1. Maak een Azure AD-toepassing.

    a. Selecteer **Azure Active Directory** > **+ App-registraties** > **nieuwe toepassing registreren**.

    b. Voer een **naam** van de toepassing.

    c. Selecteer **Web-app / API**.

    d. Voer `http://localhost` voor de **aanmeldings-URL**.

    c. Klik op **Create**.

1. Noteer de **toepassings-id**. U moet de ID bij het maken van het cluster. De ID wordt verwezen als **Client-ID Service-Principal**.

1. Selecteer **instellingen** > **sleutels**.

    a. Voer de **beschrijving**.

    b. Selecteer **verloopt nooit** voor **verloopt**.

    c. Selecteer **Opslaan**. Houd er rekening mee de belangrijkste tekenreeks maken U moet de sleutel tekenreeks bij het maken van het cluster. De sleutel waarnaar wordt verwezen als de **Clientgeheim Service-Principal**.

## <a name="give-the-service-principal-access"></a>De service-principal toegang verlenen

De service-principal toegang geven aan uw abonnement, zodat de principal-resources kunt maken.

1.  Aanmelden bij de [Azure Stack-portal](https://portal.local.azurestack.external/).

1. Selecteer **alle services** > **abonnementen**.

1. Selecteer het abonnement dat is gemaakt door de operator voor het gebruik van het Kubernetes-Cluster.

1. Selecteer **toegangsbeheer (IAM)** > Selecteer **roltoewijzing toevoegen**.

1. Selecteer de **Inzender** rol.

1. Selecteer de naam van de toepassing gemaakt voor uw service principal. U moet de naam in het zoekvak typt.

1. Klik op **Opslaan**.

## <a name="deploy-kubernetes"></a>Implementeren van Kubernetes

1. Open de [Azure Stack-portal](https://portal.local.azurestack.external).

1. Selecteer **+ een resource maken** > **Compute** > **Kubernetes-Cluster**. Klik op **Create**.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Basisbeginselen

1. Selecteer **basisbeginselen** in Kubernetes-Cluster maken.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Selecteer uw **abonnement** -id.

1. Voer de naam van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep. De resourcenaam moet alleen alfanumerieke tekens en kleine letters.

1. Selecteer de **locatie** van de resourcegroep. Dit is de regio die u voor uw Azure Stack-installatie kiest.

### <a name="2-kubernetes-cluster-settings"></a>2. Instellingen voor Kubernetes-Cluster

1. Selecteer **Kubernetes-Cluster instellingen** in Kubernetes-Cluster maken.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-aad.png)

1. Voer de **Linux VM-Beheerdersgebruikersnaam**. De naam van de gebruiker voor de virtuele Linux-Machines die deel van het Kubernetes-cluster uitmaken en DVM.

1. Voer de **openbare SSH-sleutel** gebruikt voor verificatie op alle Linux-machines gemaakt als onderdeel van de Kubernetes-cluster en DVM.

1. Voer de **Master profiel DNS-voorvoegsel** die uniek is voor de regio. Dit moet een regio-unieke naam, zoals `k8s-12345`. Probeer te hebt gekozen deze gelijk zijn aan de resourcegroep een naam als best practice.

    > [!Note]  
    > Gebruik een master profiel van nieuwe en unieke DNS-voorvoegsel voor elk cluster.

1. Selecteer de **Kubernetes Master Pool profiel aantal**. Het aantal bevat het aantal knooppunten in de master-pool. Er mag uit 1 tot en met 7. Deze waarde moet een oneven getal.

1. Selecteer **de VMSize van de master-VM's van Kubernetes**.

1. Selecteer de **groep Kubernetes-profiel knooppunten**. Het aantal bevat het aantal agents in het cluster. 

1. Selecteer de **Opslagprofiel**. U kunt ervoor kiezen **Blob schijf** of **beheerde schijf**. Hiermee geeft u het VM-grootte van de Kubernetes-knooppunt VM's. 

1. Selecteer **Azure AD** voor de **Azure Stack-identiteitssysteem** voor uw Azure Stack-installatie. 

1. Voer de **Service-Principal ClientId** dit wordt gebruikt door de Kubernetes Azure-cloud-provider. De Client-ID die wordt geïdentificeerd als de toepassings-ID bij het maken van uw service-principal.

1. Voer de **Clientgeheim Service-Principal** die u hebt gemaakt bij het maken van uw service-principal.

1. Voer de **Kubernetes Azure Cloud Provider-versie**. Dit is de versie van de Kubernetes Azure-provider. Azure Stack brengt een aangepaste Kubernetes-build voor elke Azure Stack-versie.

### <a name="3-summary"></a>3. Samenvatting

1. Selecteer Samenvatting. De blade wordt een validatiebericht voor de configuratie-instellingen van uw Kubernetes-Cluster.

    ![Oplossingssjabloon implementeren](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Controleer uw instellingen.

3. Selecteer **OK** om uw cluster te implementeren.

> [!TIP]  
>  Als u vragen over uw implementatie hebt, kunt u uw vragen of zien als iemand de vraag in al is beantwoord de [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack).


## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met uw cluster](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)
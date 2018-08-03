---
title: Een Kubernetes-Cluster toevoegen aan de Azure Stack Marketplace | Microsoft Docs
description: Informatie over het toevoegen van een Kubernetes-Cluster in de Azure Stack Marketplace.
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
ms.date: 05/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: ed2d55b8346acb79563a882bbaf2f46110dcf1bb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442714"
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Een Kubernetes-Cluster toevoegen aan de Azure Stack Marketplace

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!note]  
> De Azure Container Services (ACS) voor Kubernetes op Azure Stack is in de beperkte Preview-versie. Voor het aanvragen van toegang tot het Kubernetes-Marketplace-item die nodig zijn om uit te voeren van de instructies in dit artikel [een indienen om toegang te krijgen](https://aka.ms/azsk8).

U kunt een Kubernetes-Cluster als een Marketplace-item aan uw gebruikers bieden. Uw gebruikers kunnen Kubernetes implementeren in een enkele, gecoördineerde bewerking.

Het volgende artikel bekijken met behulp van een Azure Resource Manager-sjabloon te implementeren en de resources voor een zelfstandige Kubernetes-cluster inrichten. Voordat u begint, controleert u uw Azure Stack en de instellingen van de globale Azure-tenant. De vereiste gegevens verzamelen over uw Azure Stack. Benodigde resources toevoegen aan uw tenant en de Azure Stack Marketplace. Het cluster is afhankelijk van een Ubuntu-server, aangepaste scripts en de items van de Kubernetes-Cluster zich in de marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Een plan, een aanbieding en een abonnement maken

Maak een plan, een aanbieding en een abonnement voor het Kubernetes-Cluster Marketplace-item. U kunt ook gebruik van een bestaand plan en aanbieding.

1. Aanmelden bij de [-beheerportal.](https://adminportal.local.azurestack.external)

1. Maak een plan als het basisplan. Zie voor instructies [een plan maken in Azure Stack](azure-stack-create-plan.md).

1. Maak een aanbieding. Zie voor instructies [een aanbieding maken in Azure Stack](azure-stack-create-offer.md).

1. Selecteer **biedt**, en zoek de aanbieding die u hebt gemaakt.

1. Selecteer **overzicht** in de blade van de aanbieding.

1. Selecteer **in een statuswijziging**. Selecteer **openbare**.

1. Selecteer **+ nieuw** > **aanbiedingen en abonnementen** > **abonnement** naar een nieuw abonnement maken.

    a. Voer een **weergavenaam**.

    b. Voer een **gebruiker**. Gebruik Azure AD-account dat is gekoppeld aan uw tenant.

    c. **Beschrijving van de provider**

    d. Stel de **Directory-tenant** bij Azure AD-tenant voor uw Azure Stack. 

    e. Selecteer **bieden**. Selecteer de naam van de aanbieding die u hebt gemaakt. Noteer de abonnement-ID.

## <a name="add-an-ubuntu-server-image"></a>De installatiekopie van een Ubuntu-server toevoegen

De volgende Ubuntu-Server-installatiekopie toevoegen aan de Marketplace:

1. Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **meer services** > **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `UbuntuServer`.

1. Selecteer de server met het volgende profiel:
    - **Uitgever**: Canonical
    - **Bieden**: UbuntuServer
    - **SKU**: 16.04-LTS
    - **Versie**: 16.04.201802220

    > [!Note]  
    > Meer dan één versie van Ubuntu Server 16.04 LTS wordt vermeld. U moet de versie die overeenkomt met toevoegen. Het Kubernetes-Cluster is vereist voor de exacte versie van het item.

1. Selecteer **downloaden.**

## <a name="add-a-custom-script-for-linux"></a>Toevoegen van een aangepast script voor Linux

Het Kubernetes-Cluster uit de Marketplace toevoegen:

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **meer services** > **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Custom Script for Linux`.

1. Het script met het volgende profiel te selecteren:
    - **Bieden**: aangepast Script voor Linux 2.0
    - **Versie**: 2.0.3
    - **Uitgever**: Microsoft Corp

    > [!Note]  
    > Meer dan één versie van het aangepaste Script voor Linux kan worden vermeld. U moet de versie die overeenkomt met toevoegen. Het Kubernetes-Cluster is vereist voor de exacte versie van het item.

1. Selecteer **downloaden.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Het Kubernetes-Cluster toevoegen aan de marketplace

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **meer services** > **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Kubernetes Cluster`.

1. Selecteer `Kubernetes Cluster`.

1. Selecteer **downloaden.**

    > [!note]  
    > Het duurt vijf minuten voor de marketplace-item wordt weergegeven in de Marketplace.

    ![Kubernetes-Cluster](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Bijwerken of verwijderen van het Kubernetes-Cluster 

Tijdens het bijwerken van het item Kubernetes-Cluster, moet u het item dat in de Marketplace te verwijderen. Vervolgens kunt u de instructies in dit artikel voor het Kubernetes-Cluster toevoegen aan de marketplace volgen.

Het item Kubernetes-Cluster verwijderen:

1. Noteer de naam van het huidige item, zoals `Microsoft.AzureStackKubernetesCluster.0.1.0`

1. Verbinding maken met Azure Stack met PowerShell.

1. Gebruik de volgende PowerShell-cmdlet om het item te verwijderen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Volgende stappen

[Een Kubernetes-Cluster implementeren met Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Overzicht van services in Azure Stack-aanbieding](azure-stack-offer-services-overview.md)
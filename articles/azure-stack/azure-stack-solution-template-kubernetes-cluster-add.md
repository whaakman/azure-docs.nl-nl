---
title: Een Cluster Kubernetes toevoegen aan de Stack van Azure Marketplace | Microsoft Docs
description: Informatie over het toevoegen van een Kubernetes Cluster aan de Stack Azure Marketplace.
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
ms.openlocfilehash: c66b0d7ea5ade90c6bb8f88006f2a09bd407deaa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Een Cluster Kubernetes toevoegen aan de Stack van Azure Marketplace

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

> [!note]  
> De Azure Container Services (ACS) Kubernetes op Azure-Stack is private preview. Om aan te vragen toegang tot de Kubernetes Marketplace-item die nodig zijn voor het uitvoeren van de instructies in dit artikel [een verzoek om toegang te krijgen indienen](https://aka.ms/azsk8).

Als een Marketplace-item kunt u een Kubernetes Cluster aanbieden aan uw gebruikers. Uw gebruikers kunnen Kubernetes implementeren in een enkele, gecoördineerde bewerking.

Het volgende artikel bekijken met een Azure Resource Manager-sjabloon te implementeren en de resources voor een zelfstandige Kubernetes cluster inrichten. Voordat u begint, controleert u uw Azure-Stack en de instellingen van de globale Azure-tenant. De vereiste gegevens verzamelen over uw Azure-Stack. Benodigde resources toevoegen aan uw tenant en de Stack Azure Marketplace. Het cluster, is afhankelijk van een virtuele Ubuntu server aangepast script en de items Kubernetes Cluster zich in de marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Een plan, een aanbieding en een abonnement maken

Een plan, een aanbieding en een abonnement voor het Kubernetes Cluster Marketplace-item maken. U kunt ook een bestaand abonnement gebruiken en bieden.

1. Aanmelden bij de [-beheerportal.](https://adminportal.local.azurestack.external)

2. Een schema als het basisplan maken. Zie voor instructies [een plan maken in Azure-Stack](azure-stack-create-plan.md).

3. Maak een aanbieding. Zie voor instructies [een aanbieding maakt in Azure Stack](azure-stack-create-offer.md).

4. Selecteer **biedt**, en zoek de aanbieding die u hebt gemaakt.

5. Selecteer **overzicht** in de blade aanbieding.

6. Selecteer **statuswijziging**. Selecteer **openbare**.

7. Selecteer **+ nieuw** > **biedt en plant** > **abonnement** voor het maken van een nieuw abonnement.

    a. Voer een **weergavenaam**.

    b. Voer een **gebruiker**. Gebruik de Azure AD-account dat is gekoppeld aan uw tenant.

    c. **Beschrijving van opslagprovider**

    d. Stel de **Directory-tenant** met de Azure AD-tenant voor uw Azure-Stack. 

    e. Selecteer **bieden**. Selecteer de naam van de aanbieding die u hebt gemaakt. Noteer de abonnement-ID.

## <a name="add-an-ubuntu-server-image"></a>De installatiekopie van een virtuele Ubuntu server toevoegen

De volgende afbeelding van Ubuntu Server toevoegen aan de Marketplace:

1. Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management**.

3. Selecteer **+ toevoegen vanuit Azure**.

4. Voer `UbuntuServer`.

5. Selecteer de server met het volgende profiel:
    - **Publisher**: canonieke
    - **Bieden**: UbuntuServer
    - **SKU**: 16.04 TNS
    - **Versie**: 16.04.201802220

6. Selecteer **downloaden.**

## <a name="add-a-custom-script-for-linux"></a>Een aangepast script toevoegen voor Linux

Voeg het Cluster Kubernetes vanuit de Marketplace:

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management**.

3. Selecteer **+ toevoegen vanuit Azure**.

4. Voer `Custom Script for Linux`.

5. Selecteer het script dat met het volgende profiel:
    - **Bieden**: aangepast Script voor Linux 2.0
    - **Versie**: 2.0.3
    - **Publisher**: Microsoft Corp

6. Selecteer **downloaden.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Het Kubernetes Cluster toevoegen aan de marketplace

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management**.

3. Selecteer **+ toevoegen vanuit Azure**.

4. Voer `Kubernetes Cluster`.

5. Selecteer `Kubernetes Cluster`.

6. Selecteer **downloaden.**

    > [!note]  
    > Het duurt vijf minuten voor de marketplace-item moet worden weergegeven in de Marketplace.

    ![Kubernetes Cluster](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Bijwerken of verwijderen van het Kubernetes Cluster 

Bij het bijwerken van het item Kubernetes Cluster, moet u het item dat in de Marketplace verwijderen. Vervolgens kunt u de aanwijzingen in dit artikel aan het Kubernetes Cluster toevoegen aan de marketplace.

Het item Kubernetes Cluster verwijderen:

1. Noteer de naam van het huidige item, zoals `Microsoft.AzureStackKubernetesCluster.0.1.0`

2. Verbinding maken met Azure Stack met PowerShell.

3. Gebruik de volgende PowerShell-cmdlet om het item te verwijderen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Volgende stappen

[Een Cluster Kubernetes implementeren naar Azure Stack](/user/azure-stack-solution-template-kubernetes-deploy.md)

[Overzicht van services in Azure-Stack van aanbieding](azure-stack-offer-services-overview.md)
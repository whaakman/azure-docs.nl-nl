---
title: Een plan maken in Azure Stack | Microsoft Docs
description: Maak een plan waarmee abonnees virtuele machines voor inrichten als de beheerder van een cloud.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 5e532e02dbf4c0c9c938b1e7e3c591c65203e158
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251738"
---
# <a name="create-a-plan-in-azure-stack"></a>Een plan maken in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Plannen](azure-stack-key-features.md) zijn pakketten van één of meer services. Als een provider, kunt u plannen om aan te bieden aan uw gebruikers maken. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen te gebruiken van de abonnementen en services die ze bevatten. In dit voorbeeld ziet u het maken van een abonnement met de compute, network en storage resourceproviders. Dit abonnement biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

1. Aanmelden bij de [Azure Stack-beheerdersportal](https://adminportal.local.azurestack.external).

2. Voor het maken van een plan en aanbieding die gebruikers kunnen zich abonneren op, selecteer **+ een resource maken**, klikt u vervolgens **aanbiedingen + plannen**, klikt u vervolgens **Plan**.
  
   ![Een abonnement selecteren](media/azure-stack-create-plan/select-plan.png)

3. Onder **nieuw plan**, voer een **weergavenaam** en een **resourcenaam**. De weergavenaam is de beschrijvende naam van het plan die gebruikers kunnen zien. Alleen de beheerder kan zien de naam van de resource, die beheerders gebruiken om te werken met het abonnement als een Azure Resource Manager-resource.

   ![Geef details op](media/azure-stack-create-plan/plan-name.png)

4. Maak een nieuwe **resourcegroep**, of Selecteer een bestaand account, als een container voor het abonnement.

   ![Geef de resourcegroep](media/azure-stack-create-plan/resource-group.png)

5. Selecteer **Services** en selecteer vervolgens het selectievakje voor **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**. Kies vervolgens **Selecteer** aan de configuratie op te slaan. Selectievakjes worden weergegeven wanneer de muis boven elke optie.
  
   ![Services selecteren](media/azure-stack-create-plan/services.png)

6. Selecteer **quota**, **Microsoft.Storage (lokaal)**, en kies vervolgens de standaardquotum of selecteer **nieuw quotum maken** een aangepaste quota maken.
  
   ![Quota](media/azure-stack-create-plan/quotas.png)

7. Als u een nieuw quotum maakt, voert u een **naam** voor het quotum > Geef de quotawaarden > Selecteer **OK**. De **maken quotum** dialoogvenster wordt gesloten.

   ![Nieuw quotum](media/azure-stack-create-plan/new-quota.png)

   U selecteert vervolgens de nieuwe quota die u hebt gemaakt. Het quotum selecteren wijst deze toe en sluit het dialoogvenster.
  
   ![Het quotum toewijzen](media/azure-stack-create-plan/assign-quota.png)

8. Herhaal stap 6 en 7 maken en toewijzen van quota's voor **Microsoft.Network (lokaal)** en **Microsoft.Compute (lokaal)**. Wanneer alle drie de services toegewezen quota hebt, eruitzien ze als in het volgende voorbeeld.

   ![Volledige quotum toewijzingen](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Onder **quota**, kiest u **OK**, en klik vervolgens onder **nieuw plan**, kiest u **maken** om het plan te maken.

    ![Het plan maken](media/azure-stack-create-plan/create.png)

10. Uw nieuwe abonnement Selecteer **alle resources**, zoek vervolgens naar het abonnement en selecteert u de naam. Als uw lijst met resources lang is, gebruikt u **zoeken** uw plan met de naam vinden.

   ![Het plan bekijken](media/azure-stack-create-plan/plan-overview.png)

## <a name="next-steps"></a>Volgende stappen

* [Een aanbieding maken](azure-stack-create-offer.md)

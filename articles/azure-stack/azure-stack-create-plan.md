---
title: Een plan maken in Azure-Stack | Microsoft Docs
description: Als een cloudbeheerder een plan waarmee abonnees inrichten van virtuele machines te maken.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 1fa01d23108ce92fbd7c854442c0474b19395d25
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248700"
---
# <a name="create-a-plan-in-azure-stack"></a>Een plan maken in Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Plannen](azure-stack-key-features.md) zijn pakketten van één of meer services. Als een provider, kunt u plannen om aan te bieden aan uw gebruikers. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen plannen en services die ze gebruiken. Dit voorbeeld ziet u het maken van een abonnement met de compute, network en storage resourceproviders. Dit abonnement biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

1. Aanmelden bij de Azure-Stack-beheerdersportal (https://adminportal.local.azurestack.external).

2. Voor het maken van een plan en een aanbieding die gebruikers kunnen zich abonneren op, selecteer **nieuw** > **biedt + plannen** > **Plan**.
  
   ![Selecteer een plan](media/azure-stack-create-plan/select-plan.png)

3. Onder **nieuw plan**, voer een **weergavenaam** en een **resourcenaam**. De weergegeven naam is van het plan beschrijvende naam die gebruikers kunnen zien. Alleen de beheerder kan de naam van de Resource die beheerders gebruiken om te werken met het plan als een Azure Resource Manager-resource zien.

   ![Geef details](media/azure-stack-create-plan/plan-name.png)

4. Maak een nieuwe **resourcegroep**, of Selecteer een bestaande, als een container voor het plan.

   ![Geef de resourcegroep](media/azure-stack-create-plan/resource-group.png)

5. Selecteer **Services** en selecteer vervolgens het selectievakje voor **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**. Kies vervolgens **Selecteer** aan de configuratie op te slaan. Selectievakjes worden weergegeven wanneer de muis boven elke optie.
  
   ![Services selecteren](media/azure-stack-create-plan/services.png)

6. Selecteer **quota**, **Microsoft.Storage (lokaal)**, en kies vervolgens het standaardquotum- of selecteer **maken nieuwe quota** een aangepaste quota maken.
  
   ![Quota](media/azure-stack-create-plan/quotas.png)

7. Als u een nieuwe quota maakt, voert u een **naam** voor het quotum > Geef de quotawaarden > Selecteer **OK**. De **maken quotum** dialoogvenster wordt gesloten.

   ![Nieuw quotum](media/azure-stack-create-plan/new-quota.png)

   U wordt vervolgens de nieuwe quota die u hebt gemaakt. Selecteren van de quota toegewezen en wordt het dialoogvenster wordt gesloten.
  
   ![Het quotum toewijzen](media/azure-stack-create-plan/assign-quota.png)

8. Herhaal stap 6 en 7 maken en toewijzen van de quota voor **Microsoft.Network (lokaal)** en **Microsoft.Compute (lokaal)**. Wanneer alle drie de services toegewezen quota hebt, eruitzien ze zoals in het volgende voorbeeld.

   ![Volledige quotum toewijzingen](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Onder **quota**, kies **OK**, en klik vervolgens onder **nieuw plan**, kies **maken** om het plan te maken.

    ![Het plan maken](media/azure-stack-create-plan/create.png)

10. Overzicht van uw nieuwe schema, selecteer **alle resources**, zoekt u naar het plan en selecteert u de naam. Als uw lijst met resources te lang is, gebruikt u **Search** uw plan op naam vinden.

   ![De planning bekijken](media/azure-stack-create-plan/plan-overview.png)

## <a name="next-steps"></a>Volgende stappen

[Een aanbieding maken](azure-stack-create-offer.md)

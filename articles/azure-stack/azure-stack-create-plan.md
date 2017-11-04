---
title: Een plan maken in Azure-Stack | Microsoft Docs
description: Als een cloudbeheerder een plan waarmee abonnees inrichten van virtuele machines te maken.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: erikje
ms.openlocfilehash: 30759dca746fd7fd02653556cb105f419f5bf854
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-plan-in-azure-stack"></a>Een plan maken in Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Plannen](azure-stack-key-features.md) zijn pakketten van één of meer services. Als een provider, kunt u plannen om aan te bieden aan uw gebruikers. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen plannen en services die ze gebruiken. Dit voorbeeld ziet u het maken van een abonnement met de compute, network en storage resourceproviders. Dit abonnement biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

1. Aanmelden bij de Azure-Stack-beheerdersportal (https://adminportal.local.azurestack.external). Voer de referenties voor het account dat u hebt gemaakt tijdens stap 5 van de [de PowerShell-script uitvoeren](azure-stack-run-powershell-script.md) sectie.

2. Klik op om een plan en een aanbieding die gebruikers kunnen zich abonneren op **nieuw** > **Tenant biedt + plannen** > **Plan**.

   ![](media/azure-stack-create-plan/image01.png)
3. In de **nieuwe plannen** blade invullen **weergavenaam** en **resourcenaam**. De weergegeven naam is van het plan beschrijvende naam die gebruikers te zien. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om te werken met het abonnement als een Azure Resource Manager-resource.

   ![](media/azure-stack-create-plan/image02.png)
4. Maak een nieuwe **resourcegroep**, of Selecteer een bestaande, als een container voor het plan.

   ![](media/azure-stack-create-plan/image02a.png)
5. Klik op **Services**, selecteer **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**, en klik vervolgens op **Selecteer**.

   ![](media/azure-stack-create-plan/image03.png)
6. Klik op **quota**, klikt u op **Microsoft.Storage (lokaal)**, en vervolgens selecteert u het standaardquotum of klik op **maken nieuwe quota** voor het aanpassen van het quotum.

   ![](media/azure-stack-create-plan/image04.png)
7. Als u een nieuwe quota maakt, voer een naam voor het quotum > Stel de quotawaarden > klikt u op **OK** > Klik op de naam van de nieuwe quota.

   ![](media/azure-stack-create-plan/image06.png)
8. Klik op **Microsoft.Network (lokaal)**, en vervolgens selecteert u het standaardquotum of klik op **maken nieuwe quota** voor het aanpassen van het quotum.

    ![](media/azure-stack-create-plan/image07.png)
9. Als u een nieuwe quota maakt, typt u een naam voor het quotum > Stel de quotawaarden > klikt u op **OK** > Klik op de naam van de nieuwe quota.

    ![](media/azure-stack-create-plan/image08.png)
10. Klik op **Microsoft.Compute (lokaal)**, en vervolgens selecteert u het standaardquotum of klik op **maken nieuwe quota** voor het aanpassen van het quotum.

    ![](media/azure-stack-create-plan/image09.png)
11. Als u een nieuwe quota maakt, typt u een naam voor het quotum > Stel de quotawaarden > klikt u op **OK** > Klik op de naam van de nieuwe quota.

    ![](media/azure-stack-create-plan/image10.png)
12. In de **quota** blade, klikt u op **OK**, en klik vervolgens in de **nieuw Plan** blade, klikt u op **maken** om het plan te maken.

    ![](media/azure-stack-create-plan/image11.png)
13. Klik op een overzicht van uw nieuwe schema **alle resources**, zoekt u naar het plan en klik op de naam.

    ![](media/azure-stack-create-plan/image12.png)

### <a name="next-steps"></a>Volgende stappen
[Een aanbieding maken](azure-stack-create-offer.md)

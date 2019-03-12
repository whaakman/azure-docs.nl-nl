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
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: a2ac138228b7b54f486acb0f42975748136a8da7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759466"
---
# <a name="create-a-plan-in-azure-stack"></a>Een plan maken in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Abonnementen](azure-stack-key-features.md) groeperingen van een of meer services en hun quota. Als een provider, kunt u plannen om aan te bieden aan uw gebruikers maken. Uw gebruikers abonneren op zijn beurt op uw aanbiedingen te gebruiken voor het plannen, services en quota's die ze bevatten. In dit voorbeeld ziet u het maken van een abonnement met de compute, network en storage resourceproviders. Dit abonnement biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

## <a name="create-a-plan-1902-and-later"></a>Maak een plan (1902 en hoger)

1. Aanmelden bij de [Azure Stack-beheerdersportal](https://adminportal.local.azurestack.external).

2. Voor het maken van een plan en aanbieding die gebruikers kunnen zich abonneren op, selecteer **+ een resource maken**, klikt u vervolgens **aanbiedingen + plannen**, klikt u vervolgens **Plan**.
  
   ![Een abonnement selecteren](media/azure-stack-create-plan/select-plan.png)

3. Een gebruikersinterface met tabbladen weergegeven waarmee u de naam van het opgeven, services toe te voegen en quota's voor elk van de geselecteerde services definiëren. Het belangrijkste is dat kunt u de details van de aanbieding die u hebt gemaakt, voordat u besluit om deze te maken bekijken.

   Onder de **basisbeginselen** tabblad van de **nieuw plan** venster, Geef een **weergavenaam** en een **resourcenaam**. De weergavenaam is de beschrijvende naam van het plan die operators kunnen zien. Houd er rekening mee dat in de beheerdersportal Abonnementsdetails alleen zichtbaar voor operators zijn.

   ![Geef details op](media/azure-stack-create-plan/plan-name.png)

4. Maak een nieuwe **resourcegroep**, of Selecteer een bestaand account, als een container voor het abonnement.

   ![Geef de resourcegroep](media/azure-stack-create-plan/resource-group.png)

5. Selecteer de **Services** tabblad, en selecteer vervolgens het selectievakje voor **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage** .
  
   ![Services selecteren](media/azure-stack-create-plan/services.png)

6. Selecteer de **quota** tabblad. Naast **Microsoft.Storage**, kiest u het standaardquotum in de vervolgkeuzelijst of selecteer **nieuw** een aangepaste quota maken.
  
   ![Quota](media/azure-stack-create-plan/quotas.png)

7. Als u een nieuw quotum maakt, voert u een **naam** voor het quotum en geef vervolgens de quotawaarden. Selecteer **OK** te maken van het quotum.

   ![Nieuw quotum](media/azure-stack-create-plan/new-quota.png)

8. Herhaal stap 6 en 7 maken en toewijzen van quota's voor **Microsoft.Network** en **Microsoft.Compute**. Wanneer alle drie de services toegewezen quota hebt, eruitzien ze als in het volgende voorbeeld.

   ![Volledige quotum toewijzingen](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Selecteer **bekijken + maken** om te controleren van het abonnement. Bekijk alle waarden en quota's om te controleren of dat ze juist zijn. Houd er rekening mee de uitbreiding pijlen aan de linkerkant van elk paar servicequotum /. Een nieuwe functie kunt u de quota uitbreiden in de geselecteerde abonnementen, één voor één, de details van elke quota weergeven in een plan en gaat u terug naar de benodigde wijzigingen aanbrengen.

   ![Het plan maken](media/azure-stack-create-plan/create.png)

10. Wanneer u klaar bent, selecteert u **maken** om het plan te maken.

11. Het nieuwe abonnement Selecteer **plannen**, zoek vervolgens naar het abonnement en selecteert u de naam. Als uw lijst met resources lang is, gebruikt u **zoeken** uw plan met de naam vinden.

## <a name="create-a-plan-1901-and-earlier"></a>Maak een plan (1901 en eerder)

1. Aanmelden bij de [Azure Stack-beheerdersportal](https://adminportal.local.azurestack.external).

2. Voor het maken van een plan en aanbieding die gebruikers kunnen zich abonneren op, selecteer **+ een resource maken**, klikt u vervolgens **aanbiedingen + plannen**, klikt u vervolgens **Plan**.
  
   ![Een abonnement selecteren](media/azure-stack-create-plan/select-plan1901.png)

3. Onder **nieuw plan**, voer een **weergavenaam** en een **resourcenaam**. De weergavenaam is de beschrijvende naam van het plan die gebruikers kunnen zien. Alleen de beheerder kan zien de naam van de resource, die beheerders gebruiken om te werken met het abonnement als een Azure Resource Manager-resource.

   ![Geef details op](media/azure-stack-create-plan/plan-name1901.png)

4. Maak een nieuwe **resourcegroep**, of Selecteer een bestaand account, als een container voor het abonnement.

   ![Geef de resourcegroep](media/azure-stack-create-plan/resource-group1901.png)

5. Selecteer **Services** en selecteer vervolgens het selectievakje voor **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**. Kies vervolgens **Selecteer** aan de configuratie op te slaan. Selectievakjes worden weergegeven wanneer de muis boven elke optie.
  
   ![Services selecteren](media/azure-stack-create-plan/services1901.png)

6. Selecteer **quota**, **Microsoft.Storage (lokaal)**, en kies vervolgens de standaardquotum of selecteer **nieuw quotum maken** een aangepaste quota maken.
  
   ![Quota](media/azure-stack-create-plan/quotas1901.png)

7. Als u een nieuw quotum maakt, voert u een **naam** voor het quotum > Geef de quotawaarden > Selecteer **OK**. De **maken quotum** dialoogvenster wordt gesloten.

   ![Nieuw quotum](media/azure-stack-create-plan/new-quota1901.png)

   U selecteert vervolgens de nieuwe quota die u hebt gemaakt. Het quotum selecteren wijst deze toe en sluit het dialoogvenster.
  
   ![Het quotum toewijzen](media/azure-stack-create-plan/assign-quota1901.png)

8. Herhaal stap 6 en 7 maken en toewijzen van quota's voor **Microsoft.Network (lokaal)** en **Microsoft.Compute (lokaal)**. Wanneer alle drie de services toegewezen quota hebt, eruitzien ze als in het volgende voorbeeld.

   ![Volledige quotum toewijzingen](media/azure-stack-create-plan/all-quotas-assigned1901.png)

9. Onder **quota**, kiest u **OK**, en klik vervolgens onder **nieuw plan**, kiest u **maken** om het plan te maken.

    ![Het plan maken](media/azure-stack-create-plan/create1901.png)

10. Uw nieuwe abonnement Selecteer **alle resources**, zoek vervolgens naar het abonnement en selecteert u de naam. Als uw lijst met resources lang is, gebruikt u **zoeken** uw plan met de naam vinden.

    ![Het plan bekijken](media/azure-stack-create-plan/plan-overview1901.png)

## <a name="next-steps"></a>Volgende stappen

* [Een aanbieding maken](azure-stack-create-offer.md)

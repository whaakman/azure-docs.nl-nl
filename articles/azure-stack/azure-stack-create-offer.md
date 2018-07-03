---
title: Een aanbieding maken in Azure Stack | Microsoft Docs
description: Als de cloudbeheerder van een, informatie over het maken van een aanbieding voor uw gebruikers in Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/2/2018
ms.author: brenduns
ms.openlocfilehash: eed715a7c2cb967f6c9ea0b7d4442a4f9976bd17
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345886"
---
# <a name="create-an-offer-in-azure-stack"></a>Een aanbieding maken in Azure Stack

[Biedt](azure-stack-key-features.md) zijn groepen van een of meer plannen die providers die aanwezig voor gebruikers om te kopen of zich abonneert zijn op. Dit document wordt beschreven hoe u een aanbieding met maakt de [plan dat u hebt gemaakt](azure-stack-create-plan.md). Deze aanbieding biedt abonnees de mogelijkheid voor het instellen van virtuele machines.

1. Aanmelden bij de Azure Stack-beheerdersportal (https://adminportal.local.azurestack.external) en selecteer **nieuw** > **Tenant aanbiedingen + plannen** > **bieden**.

   ![Een aanbieding maken](media/azure-stack-create-offer/image01.png)
  
2. Onder **nieuwe aanbieding**, voer een **weergavenaam** en een **resourcenaam**, en klik vervolgens onder **resourcegroep**, selecteer **maken nieuwe** of **gebruik bestaande**. De weergavenaam is de beschrijvende naam voor de aanbieding. Deze beschrijvende naam is de enige informatie over de aanbieding die de gebruikers zien wanneer ze zich op een aanbieding abonneert. Gebruik de naam van een intuïtieve, die helpt gebruikers begrijpen wat wordt geleverd met de aanbieding. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![Nieuwe aanbieding](media/azure-stack-create-offer/image01a.png)
  
3. Selecteer **Basisplannen** openen de **plannen**. Selecteer de abonnementen die u wilt opnemen in de aanbieding in en kies vervolgens **Selecteer**. Het maken van de aanbieding selecteren **maken**.

   ![Abonnement selecteren](media/azure-stack-create-offer/image02.png)
  
4. Na het maken van de aanbieding, kunt u de status. Aanbiedingen moeten worden aangebracht *openbare* voor gebruikers om op te halen van de volledige weergave wanneer ze zich abonneert. Aanbiedingen kunnen zijn:

   - **Openbare**: zichtbaar voor gebruikers.
   - **Persoonlijke**: alleen zichtbaar voor beheerders in de cloud. Deze instelling is nuttig tijdens het opstellen van het plan of de aanbieding, of als beheerder van de cloud wil [maakt elk abonnement voor gebruikers](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Uit bedrijf genomen**: gesloten voor nieuwe abonnees. Beheerder van de cloud kunt gebruiken uit bedrijf genomen om te voorkomen dat toekomstige abonnementen, maar laat u huidige abonnees niet beïnvloed.

   > [!TIP]  
   > Er zijn wijzigingen in de aanbieding niet meteen zichtbaar voor gebruikers. Als u wilt zien welke wijzigingen, mogelijk gebruikers zich afmelden en opnieuw aanmelden bij de gebruikersportal aanmeldt om te zien van de nieuwe aanbieding.

   De status van de aanbieding wijzigen:

   - **Versie 1803 of hoger**:  
     Selecteer in het overzicht voor de aanbieding **toegankelijkheidsstatus**. Kies de status die u wilt gebruiken (bijvoorbeeld *openbare*) en selecteer vervolgens **opslaan**.
 
     ![Toegankelijkheidsstatus selecteren](media/azure-stack-create-offer/change-state.png)

     Als alternatief, nadat u toegang een aanbieding tot gaat u naar **Aanbiedingsinstellingen**. Selecteer **toegankelijkheidsstatus** voor een statuswijziging.

   - **Voorafgaand aan versie 1803**:  
     Selecteer **alle Resources**, zoeken naar de nieuwe aanbieding, en selecteer vervolgens de nieuwe aanbieding. Selecteer **status wijzigen**, en selecteer vervolgens **openbare**.

   > [!NOTE]
   > U kunt ook PowerShell gebruiken om te maken van de standaardaanbiedingen, plannen en quota's. Zie voor meer informatie, [PowerShell-Module voor Azure Stack 1.3.0](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0).

## <a name="next-steps"></a>Volgende stappen

- [Maken van abonnementen](azure-stack-subscribe-plan-provision-vm.md)
- [Een virtuele machine inrichten](azure-stack-provision-vm.md)

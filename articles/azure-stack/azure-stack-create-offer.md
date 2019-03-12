---
title: Een aanbieding maken in Azure Stack | Microsoft Docs
description: Als de cloudbeheerder van een, informatie over het maken van een aanbieding voor uw gebruikers in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 42336205726823dd04e0412f29c3e7a23d134d39
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57763990"
---
# <a name="create-an-offer-in-azure-stack"></a>Een aanbieding maken in Azure Stack

[Biedt](azure-stack-key-features.md) zijn groepen van een of meer plannen die providers die aanwezig voor gebruikers, die gebruikers kunnen kopen of zich abonneert zijn op. In dit artikel wordt beschreven hoe u een aanbieding met de [plan dat u hebt gemaakt](azure-stack-create-plan.md). Deze aanbieding biedt abonnees de mogelijkheid voor het instellen van virtuele machines (VM's).

## <a name="create-an-offer-1902-and-later"></a>Maak een aanbieding (1902 en hoger)

1. Aanmelden bij de [Azure Stack-beheerdersportal](https://adminportal.local.azurestack.external) en selecteer **+ een resource maken**, klikt u vervolgens **aanbiedingen + plannen**, en vervolgens **bieden**.

   ![Een aanbieding maken](media/azure-stack-create-offer/offers.png)

2. Een gebruikersinterface met tabbladen weergegeven waarmee u definieert de aanbiedingsnaam van de en bestaande toevoegen of maken van nieuwe basisplannen en aanvullende plannen. Het belangrijkste is dat kunt u de details van de aanbieding die u hebt gemaakt, voordat u besluit om deze te maken bekijken.

   In de **basisbeginselen** tabblad onder **nieuwe aanbieding**, voer een **weergavenaam** en een **resourcenaam**, en klik vervolgens onder **Resource Groep**, selecteer **nieuw** of **gebruik bestaande**. De weergavenaam is de beschrijvende naam voor de aanbieding. Deze beschrijvende naam is de enige informatie over de aanbieding die gebruikers zien wanneer ze zich op een aanbieding in de gebruikersportal abonneert. Gebruik de naam van een intuïtieve, die helpt gebruikers begrijpen wat wordt geleverd met de aanbieding. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource. Op dit tabblad kunt u ook openbaar maken van deze aanbieding of bewaar deze privé zijn, dit is de standaardinstelling. U kunt [wijzigen van de openbare of particuliere status van de aanbieding](#change-the-state-of-an-offer) later, maar ook.

   ![Nieuwe aanbieding](media/azure-stack-create-offer/new-offer.png)
  
3. Selecteer de **Basisplannen** tabblad. Selecteer de plan(s) die u wilt opnemen in de aanbieding.

   ![Abonnement selecteren](media/azure-stack-create-offer/select-plan.png)

4. Op dit moment kunt u een invoegtoepassing wilt wijzigen van het basisplan maken, maar dit is optioneel. We gaan een aanvullende plan maken in het volgende artikel [Azure Stack aanvullende plannen](create-add-on-plan.md).

5. Selecteer de **revisie + maken** tabblad. Bekijk de samenvatting van de aanbieding om ervoor te zorgen dat alle waarden correct zijn. De interface kunt u de quota uitbreiden in de gekozen plannen, één voor één, de details van elke quota weergeven in een plan en gaat u terug naar de benodigde wijzigingen aanbrengen.

6. Selecteer **maken** te maken van de aanbieding.

   ![Controleren en maken](media/azure-stack-create-offer/review-offer.png)

### <a name="change-the-state-of-an-offer"></a>De status van een aanbieding wijzigen

Na het maken van de aanbieding, kunt u de status. Aanbiedingen moeten worden aangebracht **openbare** voor gebruikers om op te halen van de volledige weergave wanneer ze zich abonneert. Aanbiedingen kunnen zijn:

   - **Openbare**: Zichtbaar voor gebruikers.
   - **Persoonlijke**: Alleen zichtbaar voor beheerders in de cloud. Deze instelling is nuttig tijdens het opstellen van het plan of de aanbieding, of als beheerder van de cloud wil [maakt elk abonnement voor gebruikers](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Buiten gebruik gesteld**: Gesloten voor nieuwe abonnees. Beheerder van de cloud kunt uit bedrijf nemen aanbiedingen om te voorkomen dat toekomstige abonnementen, maar laat u huidige abonnees niet beïnvloed.

   > [!TIP]  
   > Wijzigingen in de aanbieding zijn niet meteen zichtbaar voor de gebruiker. Als u wilt zien welke wijzigingen, mogelijk gebruikers zich afmelden en opnieuw aanmelden bij de gebruikersportal aanmeldt om te zien van de nieuwe aanbieding.

Er zijn twee manieren om te wijzigen van de status van een aanbieding:

1. In **alle resources**, selecteert u de naam van de aanbieding. Op de **overzicht** scherm voor de aanbieding, selecteer **in een statuswijziging**. Kies de status die u wilt gebruiken (bijvoorbeeld **openbare**).

   ![Status van de gekozen](media/azure-stack-create-offer/change-state.png)

2. In **alle resources**, selecteert u de naam van de aanbieding. Selecteer vervolgens **Aanbiedingsinstellingen**. Kies de status die u wilt gebruiken (bijvoorbeeld **openbare**) en selecteer vervolgens **opslaan**.

   ![Toegankelijkheidsstatus selecteren](media/azure-stack-create-offer/offer-settings.png)

## <a name="create-an-offer-1901-and-earlier"></a>Maak een aanbieding (1901 en eerder)

1. Aanmelden bij de [Azure Stack-beheerdersportal](https://adminportal.local.azurestack.external) en selecteer **+ een resource maken**, klikt u vervolgens **Tenant aanbiedingen + plannen**, en vervolgens **bieden**.

   ![Een aanbieding maken](media/azure-stack-create-offer/image01.png)
  
2. Onder **nieuwe aanbieding**, voer een **weergavenaam** en een **resourcenaam**, en klik vervolgens onder **resourcegroep**, selecteer **maken nieuwe** of **gebruik bestaande**. De weergavenaam is de beschrijvende naam voor de aanbieding. Deze beschrijvende naam is de enige informatie over de aanbieding die gebruikers zien wanneer ze zich op een aanbieding abonneert. Gebruik de naam van een intuïtieve, die helpt gebruikers begrijpen wat wordt geleverd met de aanbieding. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![Nieuwe aanbieding](media/azure-stack-create-offer/image01a.png)
  
3. Selecteer **Basisplannen** openen de **plannen**. Selecteer de abonnementen die u wilt opnemen in de aanbieding in en kies vervolgens **Selecteer**. Het maken van de aanbieding selecteren **maken**.

   ![Abonnement selecteren](media/azure-stack-create-offer/image02.png)
  
4. Na het maken van de aanbieding, kunt u de status. Aanbiedingen moeten worden aangebracht **openbare** voor gebruikers om op te halen van de volledige weergave wanneer ze zich abonneert. Aanbiedingen kunnen zijn:

   - **Openbare**: Zichtbaar voor gebruikers.
   - **Persoonlijke**: Alleen zichtbaar voor beheerders in de cloud. Deze instelling is nuttig tijdens het opstellen van het plan of de aanbieding, of als beheerder van de cloud wil [maakt elk abonnement voor gebruikers](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Buiten gebruik gesteld**: Gesloten voor nieuwe abonnees. Beheerder van de cloud kunt uit bedrijf nemen aanbiedingen om te voorkomen dat toekomstige abonnementen, maar laat u huidige abonnees niet beïnvloed.

   > [!TIP]  
   > Wijzigingen in de aanbieding zijn niet meteen zichtbaar voor de gebruiker. Als u wilt zien welke wijzigingen, mogelijk gebruikers zich afmelden en opnieuw aanmelden bij de gebruikersportal aanmeldt om te zien van de nieuwe aanbieding.

   Selecteer op het overzichtsscherm voor de aanbieding **toegankelijkheidsstatus**. Kies de status die u wilt gebruiken (bijvoorbeeld **openbare**), en selecteer vervolgens **opslaan**.

     ![Status van de gekozen](media/azure-stack-create-offer/change-stage-1807.png)

     Als alternatief, selecteer **in een statuswijziging** en kies vervolgens een staat.

    ![Toegankelijkheidsstatus selecteren](media/azure-stack-create-offer/change-stage-select-1807.png)

> [!NOTE]
> U kunt ook PowerShell gebruiken om te maken van de standaardaanbiedingen, plannen en quota's. Zie voor meer informatie, [PowerShell-Module voor Azure Stack 1.4.0](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Volgende stappen

- [Maken van abonnementen](azure-stack-subscribe-plan-provision-vm.md)
- [Een virtuele machine inrichten](azure-stack-provision-vm.md)

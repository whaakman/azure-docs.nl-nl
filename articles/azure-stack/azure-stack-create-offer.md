---
title: Maken van een aanbieding in Azure-Stack | Microsoft Docs
description: Informatie over het maken van een aanbieding voor uw gebruikers in Azure-Stack als een beheerder van de cloud.
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
ms.date: 06/07/2018
ms.author: brenduns
ms.openlocfilehash: e5b96a9464bf4d0e3b69d2f635da32c6648ce793
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247514"
---
# <a name="create-an-offer-in-azure-stack"></a>Een aanbieding maken in Azure Stack

[Biedt](azure-stack-key-features.md) zijn groepen van een of meer abonnementen die providers die aanwezig zijn voor gebruikers te kopen of te abonneren op. Dit document ziet u het maken van een aanbieding met de [plan dat u hebt gemaakt](azure-stack-create-plan.md). Deze aanbieding biedt abonnees de mogelijkheid voor het instellen van virtuele machines.

1. Aanmelden bij de Azure-Stack-beheerdersportal (https://adminportal.local.azurestack.external) en selecteer **nieuw** > **Tenant biedt + plannen** > **bieden**.

   ![Een aanbieding maken](media/azure-stack-create-offer/image01.png)
  
2. Onder **nieuwe bieden**, voer een **weergavenaam** en een **resourcenaam**, en klik vervolgens onder **resourcegroep**, selecteer **maken nieuwe** of **gebruik bestaande**. De weergavenaam is de beschrijvende naam voor de aanbieding. Deze beschrijvende naam is de enige informatie over de aanbieding die de gebruikers zien wanneer ze zich op een aanbieding abonneert. Gebruik de naam van een intuïtieve die helpt gebruikers begrijpen wat wordt geleverd met de aanbieding. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![Nieuwe aanbieding](media/azure-stack-create-offer/image01a.png)
  
3. Selecteer **baseren plannen** openen de **plannen**. Selecteer de abonnementen die u wilt opnemen in de aanbieding en kies vervolgens **Selecteer**. Maken van de aanbieding selecteren **maken**.

   ![abonnement selecteren](media/azure-stack-create-offer/image02.png)
  
4. U kunt de status wijzigen na het maken van de aanbieding. Aanbiedingen moeten worden aangebracht *openbare* voor gebruikers om op te halen van de volledige weergave wanneer ze zich abonneert. Aanbiedingen kunnen zijn:

   - **Openbare**: zichtbaar voor gebruikers.
   - **Persoonlijke**: alleen zichtbaar voor beheerders cloud. Deze instelling is nuttig bij het opstellen van het abonnement of de aanbieding, of als beheerder van de cloud wil [elk abonnement voor gebruikers maken](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Uit bedrijf genomen**: gesloten voor nieuwe abonnees. Kan de cloudbeheerder gebruiken buiten gebruik gestelde om te voorkomen dat toekomstige abonnementen, maar laat huidige abonnees blijft de situatie ongewijzigd.

   > [!TIP]  
   > Wijzigingen in de aanbieding niet onmiddellijk zichtbaar voor gebruiker. Wilt zien welke wijzigingen, kunnen gebruikers zich afmelden en opnieuw aan te melden bij de gebruikersportal om te zien van de nieuwe aanbieding hebben.

   De status van de aanbieding wijzigen:

   - **Versie 1803 en hoger**:  
     Selecteer op het overzicht voor de aanbieding **toegankelijkheid status**. Kies de gewenste status (bijvoorbeeld *openbare*) en selecteer vervolgens **opslaan**.
 
     ![Toegankelijkheid provincie selecteren](media/azure-stack-create-offer/change-state.png)

     Als alternatief kunt nadat u een aanbieding toegang gaat u naar **instellingen bieden**. Selecteer **toegankelijkheid status** voor een statuswijziging.

   - **Voorafgaand aan versie 1803**:  
     Selecteer **alle Resources**, zoeken naar uw nieuwe aanbieding en selecteer vervolgens de nieuwe aanbieding. Selecteer **wijziging van de status**, en selecteer vervolgens **openbare**.

   > [!NOTE]
   > U kunt ook PowerShell gebruiken om standaardaanbiedingen, plannen en quota's te maken. Zie voor meer informatie [Azure Stack-servicebeheerder Leesmij](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).

## <a name="next-steps"></a>Volgende stappen

- [Abonnementen maken](azure-stack-subscribe-plan-provision-vm.md)
- [Een virtuele machine inrichten](azure-stack-provision-vm.md)

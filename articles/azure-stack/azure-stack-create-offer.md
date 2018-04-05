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
ms.date: 03/27/2018
ms.author: brenduns
ms.openlocfilehash: 6a59d0c8144492ef907e5c395f05e4e8a16678a5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Een aanbieding maken in Azure Stack

[Biedt](azure-stack-key-features.md) zijn een of meer plannen providers die aanwezig zijn voor gebruikers aan te schaffen of zich abonneert op groepen. Dit document ziet u het maken van een aanbieding met de [plan dat u hebt gemaakt](azure-stack-create-plan.md) in de vorige stap. Deze aanbieding biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

1. Aanmelden bij de Azure-Stack-beheerdersportal (https://adminportal.local.azurestack.external) > klikt u op **nieuw** > **Tenant biedt + plannen** > **bieden**.

   ![](media/azure-stack-create-offer/image01.png)
2. In de **nieuwe bieden** vullen in deelvenster **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de beschrijvende naam voor de aanbieding. Deze beschrijvende naam is de enige informatie over de aanbieding die de gebruikers zien wanneer u zich abonneert. Daarom moet u een intuïtieve naam waarmee de gebruiker begrijpen wat wordt geleverd met de aanbieding te gebruiken. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![](media/azure-stack-create-offer/image01a.png)
3. Klik op **baseren plannen** openen de **plannen** deelvenster selecteert u de abonnementen die u wilt opnemen in de aanbieding en klik vervolgens op **Selecteer**. Klik op **Maken** om de aanbieding te maken.

   ![](media/azure-stack-create-offer/image02.png)
4. U kunt de status wijzigen na het maken van de aanbieding. Aanbiedingen moeten worden aangebracht *openbare* voor gebruikers om op te halen van de volledige weergave wanneer ze zich abonneert. Aanbiedingen kunnen zijn:
   - **Openbare**: zichtbaar voor gebruikers.
   - **Persoonlijke**: alleen zichtbaar voor beheerders cloud. Handig is bij het opstellen van het abonnement of de aanbieding, of als beheerder van de cloud wil [elk abonnement voor gebruikers maken](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Uit bedrijf genomen**: gesloten voor nieuwe abonnees. De beheerder van de cloud kunt gebruiken om te voorkomen dat toekomstige abonnementen, maar laat huidige abonnees intact gelaten buiten gebruik wordt gesteld.

   > [!TIP]  
   > Wijzigingen in de aanbieding zijn niet onmiddellijk zichtbaar is voor de gebruiker. Als u wilt zien welke wijzigingen, wellicht gebruikers Meld u af en meld u opnieuw aan de gebruikersportal om te zien van de nieuwe aanbieding. 

   De status van de aanbieding wijzigen: 

   - **Versie 1803 en hoger**:  
     Klik op de blade overzicht voor de aanbieding **toegankelijkheid status**, selecteert u de status die u gebruiken wilt, zoals *openbare*, en klik vervolgens op **opslaan**. 
 
     ![Toegankelijkheid provincie selecteren](media/azure-stack-create-offer/change-state.png) 

     U kunt ook wanneer u een aanbieding kunt u goto **instellingen bieden**, en selecteer vervolgens **toegankelijkheid status** voor een statuswijziging. 

   - **Voorafgaand aan versie 1803**:  
     Klik op **alle Resources**, zoeken naar uw nieuwe aanbieding, klikt u op de nieuwe aanbieding, klikt u op **wijziging van de status**, en klik vervolgens op **openbare**.

  
   > [!NOTE] 
   > U kunt ook PowerShell gebruiken om standaardaanbiedingen, plannen en quota's te maken. Zie voor meer informatie [Azure Stack-servicebeheerder Leesmij](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
   >


### <a name="next-steps"></a>Volgende stappen
[Abonnementen maken](azure-stack-subscribe-plan-provision-vm.md)      
[Een virtuele machine inrichten](azure-stack-provision-vm.md)

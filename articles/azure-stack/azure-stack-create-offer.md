---
title: Maken van een aanbieding in Azure-Stack | Microsoft Docs
description: Informatie over het maken van een aanbieding voor uw gebruikers in Azure-Stack als een beheerder van de cloud.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: erikje
ms.openlocfilehash: 269a6106f657536ba74be366f842b2f9cd86c5dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-offer-in-azure-stack"></a>Een aanbieding maken in Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Biedt](azure-stack-key-features.md) zijn een of meer plannen providers die aanwezig zijn voor gebruikers aan te schaffen of zich abonneert op groepen. Dit document ziet u het maken van een aanbieding met de [plan dat u hebt gemaakt](azure-stack-create-plan.md) in de vorige stap. Deze aanbieding biedt abonnees de mogelijkheid voor het inrichten van virtuele machines.

1. Aanmelden bij de Azure-Stack-beheerdersportal (https://adminportal.local.azurestack.external) > klikt u op **nieuw** > **Tenant biedt + plannen**  >   **Bieden**.

   ![](media/azure-stack-create-offer/image01.png)
2. In de **nieuwe bieden** blade invullen **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de beschrijvende naam van de aanbieding en is de enige informatie over de aanbieding die de gebruikers zien wanneer u zich abonneert. Daarom moet u een intuïtieve naam waarmee de gebruiker begrijpen wat wordt geleverd met de aanbieding te gebruiken. Alleen de beheerder kan de resourcenaam zien. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![](media/azure-stack-create-offer/image01a.png)
3. Klik op **Basisplannen** en selecteer op de blade **Plan** de plannen die u wilt opnemen in de aanbieding. Klik vervolgens op **Selecteren**. Klik op **Maken** om de aanbieding te maken.

   ![](media/azure-stack-create-offer/image02.png)
4. Klik op **alle Resources**, zoeken naar uw nieuwe aanbieding, klikt u op de nieuwe aanbieding, klikt u op **wijziging van de status**, en klik vervolgens op **openbare**.

   ![](media/azure-stack-create-offer/image03.png)

Aanbiedingen moeten openbaar zijn voor gebruikers om op te halen van de volledige weergave wanneer er een abonnement worden gemaakt. Aanbiedingen kunnen zijn:

* **Openbare**: zichtbaar voor gebruikers.
* **Persoonlijke**: alleen zichtbaar voor de cloudbeheerders. Handig is bij het opstellen van het abonnement of de aanbieding, of als beheerder van de cloud wil goedkeuren elk abonnement.
* **Uit bedrijf genomen**: gesloten voor nieuwe abonnees. De beheerder van de cloud kunt gebruiken om te voorkomen dat toekomstige abonnementen, maar laat huidige abonnees intact gelaten buiten gebruik wordt gesteld.

Wijzigingen in de aanbieding zijn niet onmiddellijk zichtbaar is voor de gebruiker. Overzicht van de wijzigingen mogelijk moet u afmelden/aanmelden voor een overzicht van het nieuwe abonnement in de objectkiezer' abonnement' bij het maken van resources/resourcegroepen.

> [!NOTE]
>U kunt ook standaardaanbiedingen, plannen en quota's maken met behulp van PowerShell, zoals wordt beschreven in de [Azure Stack-servicebeheerder Leesmij](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Volgende stappen
[Abonneren op een aanbieding en vervolgens inrichten van een virtuele machine](azure-stack-subscribe-plan-provision-vm.md)

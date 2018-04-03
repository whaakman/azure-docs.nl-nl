---
title: In deze zelfstudie maakt u een Azure-Stack aanbieding | Microsoft Docs
description: Informatie over het maken van een Azure-Stack-aanbieding inclusief plannen en quota's.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/27/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 8bcc2f3077e79ff83ac2e90db0bb0fa53ae83adc
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="tutorial-offer-azure-stack-iaas-services"></a>Zelfstudie: Azure Stack IaaS-services bieden
U kunt aanbiedingen die uw gebruikers (ook wel aangeduid als tenants) kunnen zich abonneren op maken als de beheerder van een Azure-Stack-cloud. Met behulp van hun abonnement, kunnen gebruikers vervolgens gebruiken Stack Azure-services.

Deze zelfstudie leert u hoe u een aanbieding zodat gebruikers kunnen maken van virtuele machines op basis van geüploade in Windows Server 2016 Datacenter installatiekopie van het Azure-Stack marketplace maakt de [vorige zelfstudie](asdk-marketplace-item.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een plan maken
> * Quota instellen
> * Openbare set aanbieding

In Azure-Stack worden services geleverd aan gebruikers met behulp van de abonnementen, aanbiedingen en plannen. Gebruikers kunnen zich abonneren op meerdere voorstellen. Aanbiedingen kunnen een of meer abonnementen hebben en plannen kunnen een of meer services hebben, zoals wordt weergegeven in het volgende diagram:

![Abonnementen en aanbiedingen plannen](media/asdk-offer-services/sop.png)

Als Azure-Stack operator services bieden u eerst gevraagd om de aanbieding en vervolgens een plan en ten slotte quota's te maken. Nadat een aanbieding is gemaakt, Azure Stack-gebruikers kunnen zich vervolgens aanmelden bij aanbiedingen via de gebruikersportal.

## <a name="create-an-offer"></a>Een aanbieding maken
**Biedt** zijn een of meer plannen dat Azure Stack operators aan gebruikers aan te schaffen of zich abonneert op groepen.

1. Aanmelden bij de [Stack Azure portal](https://adminportal.local.azurestack.external) als een beheerder van de cloud.

2. Klik op **nieuwe** > **biedt + plannen** > **bieden**.

   ![Nieuwe aanbieding](media/asdk-offer-services/new-offer.png)

2. In de **nieuwe bieden** sectie, vul **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de aanbieding openbare beschrijvende naam die gebruikers te zien. Alleen de operator cloud ziet de resourcenaam, die wordt gebruikt door beheerders voor het werken met de aanbieding als een Azure Resource Manager-resource.

   ![Weergavenaam](media/asdk-offer-services/offer-display-name.png)


## <a name="create-a-plan"></a>Een plan maken
Na het invoeren van de informatie basic aanbieding, moet u ten minste één basisplan toevoegen aan de aanbieding. 

**Plan** dat Azure Stack operators groep services en hun bijbehorende quota die aan gebruikers worden aangeboden.

1. Klik op **baseren plannen**, en in de **Plan** sectie, klikt u op **toevoegen** een nieuw plan toevoegen aan de aanbieding.

   ![Een plan toevoegen](media/asdk-offer-services/new-plan.png)

2. In de **nieuwe plannen** sectie, vul **weergavenaam** en **resourcenaam**. De weergegeven naam is van het plan beschrijvende naam die gebruikers te zien. Alleen de operator cloud ziet de resourcenaam door cloudoperators gebruikt om te werken met het abonnement als een Azure Resource Manager-resource.

   ![Weergavenaam plannen](media/asdk-offer-services/plan-display-name.png)

3. Selecteer vervolgens de services worden opgenomen in het plan. Om IaaS-services te bieden, klikt u op **Services**, selecteer **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**, en vervolgens Klik op **Selecteer**.

   ![Services plannen](media/asdk-offer-services/select-services.png)


## <a name="set-quotas"></a>Quota instellen
Nu dat de aanbieding een plan dat services bevat heeft, moet u quota's voor elk van deze instellen. 

**Quota's** bepaalt de hoeveelheid resources die een gebruiker kan gebruiken voor elke service die is opgenomen in het plan wordt aangeboden.

1. Klik op **quota**, en selecteer vervolgens de service die u wilt maken van een quotum. 

   Om te beginnen, moet u eerst een quotum voor de compute-service maken. Selecteer in de lijst naamruimte de **Microsoft.Compute** naamruimte en klik vervolgens op **maken nieuwe quota**.
   
   ![Nieuw quotum maken](media/asdk-offer-services/create-quota.png)

2. Op de **maken quotum** sectie, typ een naam voor het quotum, stel de gewenste parameters voor de quota en klikt u op **OK**.

   ![De naam van de quota](media/asdk-offer-services/quota-properties.png)

3. Selecteer de quota die u hebt gemaakt voor de **Microsoft.Compute** service.

   ![Selecteer quotum](media/asdk-offer-services/set-quota.png)

4. Herhaal stap 1-3 quota instellen voor de netwerk- en -services en klik vervolgens op OK in het gedeelte van de quota's. Klik vervolgens op **OK** op de **nieuw plan** sectie de plan-installatie te voltooien. 

   ![Alle quota instellen](media/asdk-offer-services/all-quotas-set.png)

5. Voltooi de aanbieding door te klikken op **maken** in de sectie Plan. U ziet een melding wanneer de aanbieding is gemaakt en wordt het weergegeven als een beschikbare aanbieding.

   ![Aanbieding gemaakt](media/asdk-offer-services/offer-complete.png)

## <a name="set-offer-to-public"></a>Openbare set aanbieding
Aanbiedingen moeten openbaar worden gemaakt voor gebruikers te zien bij het selecteren van een aanbieding voor een abonnement op. 

Aanbiedingen kunnen zijn:
- **Openbare**: zichtbaar voor alle gebruikers.
- **Persoonlijke**: alleen zichtbaar voor de cloudbeheerders. Handig is bij het opstellen van het abonnement of de aanbieding, of als de beheerder van de cloud wil maken van elk abonnement voor gebruikers.
- **Uit bedrijf genomen**: gesloten voor nieuwe abonnees. De beheerder van de cloud kunt gebruiken om te voorkomen dat toekomstige abonnementen, maar laat huidige abonnees intact gelaten buiten gebruik wordt gesteld.

> [!TIP]
> Wijzigingen in de aanbieding zijn niet onmiddellijk zichtbaar voor gebruikers. Wilt zien welke wijzigingen, gebruikers wellicht Meld u af en meld u opnieuw aan bij de [gebruikersportal](https://portal.local.azurestack.external) om te zien van de nieuwe aanbieding.

De nieuwe aanbieding instellen op openbaar: 
   - Versie 1803 en hoger: 
     1. Klik op het dashboard in het menu **biedt** en klik vervolgens op de aanbieding die u hebt gemaakt.

     2. Klik op **toegankelijkheid status**, en klik vervolgens op **openbare**.

        ![Status wijzigen](media/asdk-offer-services/change-state.png)

     3. De aanbieding is nu beschikbaar in de gebruikersportal van Azure-Stack.


   - Voorafgaand aan versie 1803:  
     1. Klik op het dashboard in het menu **biedt** en klik vervolgens op de aanbieding die u hebt gemaakt.

     2. Klik op **Status wijzigen** en vervolgens op **Openbaar**.

        ![Openbare status](media/asdk-offer-services/set-public.png)

     3. De aanbieding is nu beschikbaar in de gebruikersportal van Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een plan maken
> * Quota instellen
> * Openbare set aanbieding

Ga naar de volgende zelfstudie voor informatie over het abonneren op de aanbieding die u zojuist hebt gemaakt als de gebruiker van een Azure-Stack.

> [!div class="nextstepaction"]
> [Abonneren op een aanbieding](asdk-subscribe-services.md)
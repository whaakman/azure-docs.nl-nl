---
title: Virtuele machines beschikbaar maken voor de gebruikers van uw Azure-Stack | Microsoft Docs
description: Zelfstudie voor virtuele machines beschikbaar maken op Azure-Stack
services: azure-stack
documentationcenter: 
author: vhorne
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f6fce4a3230c98295afb19e633bf2801c115831f
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="make-virtual-machines-available-to-your-azure-stack-users"></a>Virtuele machines beschikbaar maken voor uw Azure-Stack-gebruikers

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt aanbiedingen die uw gebruikers (ook wel aangeduid als tenants) kunnen zich abonneren op maken als de beheerder van een Azure-Stack-cloud. Met behulp van hun abonnement, kunnen gebruikers vervolgens gebruiken Stack Azure-services.

In dit artikel leest u hoe een aanbieding maakt en test deze. Voor de test u aanmelden bij de portal als een gebruiker, abonneren op de aanbieding, en maak vervolgens een virtuele machine met behulp van het abonnement.

Wat u leert:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een installatiekopie toe te voegen
> * Testen van de aanbieding


In Azure-Stack worden services geleverd aan gebruikers met behulp van de abonnementen, aanbiedingen en plannen. Gebruikers kunnen zich abonneren op meerdere voorstellen. Aanbiedingen kunnen een of meer abonnementen hebben kunnen, en abonnementen een of meer services.

![Abonnementen en aanbiedingen plannen](media/azure-stack-key-features/image4.png)

Zie voor meer informatie, [belangrijke functies en -concepten in Azure-Stack](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Een aanbieding maken

U kunt nu dingen gereed is voor uw gebruikers. Wanneer u het proces starten, wordt u eerst gevraagd om de aanbieding en vervolgens een plan en ten slotte quota's te maken.

3. **Een aanbieding maken**

   Aanbiedingen zijn een of meer plannen providers die aanwezig zijn voor gebruikers aan te schaffen of zich abonneert op groepen.

   a. [Meld u aan](azure-stack-connect-azure-stack.md) naar de portal als een cloudbeheerder en klik vervolgens op **nieuw** > **biedt + plannen** > **bieden**.
   ![Nieuwe aanbieding](media/azure-stack-tutorial-tenant-vm/image01.png)

   b. In de **nieuwe bieden** sectie, vul **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de beschrijvende naam van de aanbieding. Alleen de operator cloud kunt zien dat de naam van de Resource. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![Weergavenaam](media/azure-stack-tutorial-tenant-vm/image02.png)

   c. Klik op **baseren plannen**, en in de **Plan** sectie, klikt u op **toevoegen** een nieuw plan toevoegen aan de aanbieding.

   ![Een plan toevoegen](media/azure-stack-tutorial-tenant-vm/image03.png)

   d. In de **nieuwe plannen** sectie, vul **weergavenaam** en **resourcenaam**. De weergegeven naam is van het plan beschrijvende naam die gebruikers te zien. Alleen de operator cloud kunt zien dat de naam van de Resource. Dit is de naam die cloudoperators gebruiken om te werken met het plan als een Azure Resource Manager-resource.

   ![Weergavenaam plannen](media/azure-stack-tutorial-tenant-vm/image04.png)

   e. Klik op **Services**, selecteer **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**, en klik vervolgens op **Selecteer**.

   ![Services plannen](media/azure-stack-tutorial-tenant-vm/image05.png)

   f. Klik op **quota**, en selecteer vervolgens de eerste service die u wilt maken van een quotum. Volg deze stappen voor de Compute, Network en Storage-services voor een IaaS-quotum.

   In dit voorbeeld maken we eerst een quotum voor de Compute-service. Selecteer in de lijst naamruimte de **Microsoft.Compute** naamruimte en klik vervolgens op **maken nieuwe quota**.
   
   ![Nieuwe quota maken](media/azure-stack-tutorial-tenant-vm/image06.png)

   g. Op de **maken quotum** sectie, typ een naam voor het quotum en stelt u de gewenste parameters voor de quota en klik op **OK**.

   ![De naam van de quota](media/azure-stack-tutorial-tenant-vm/image07.png)

   h. Nu voor **Microsoft.Compute**, selecteert u het quotum dat u hebt gemaakt.

   ![Selecteer quotum](media/azure-stack-tutorial-tenant-vm/image08.png)

   Herhaal deze stappen voor de netwerk- en -services en klik vervolgens op **OK** op de **quota** sectie.

   ik. Klik op **OK** op de **nieuw plan** sectie.

   j. Op de **Plan** sectie, selecteert u het nieuwe plan en klik op **Selecteer**.

   k. Op de **nieuwe aanbieding** sectie, klikt u op **maken**. U ziet een melding wanneer de aanbieding is gemaakt.

   l. Klik op het dashboard in het menu **biedt** en klik vervolgens op de aanbieding die u hebt gemaakt.

   m. Klik op **Status wijzigen** en vervolgens op **Openbaar**.

   ![Openbare status](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Een installatiekopie toe te voegen

Voordat u virtuele machines inrichten kan, moet u een installatiekopie toevoegen aan de Stack van Azure marketplace. U kunt de installatiekopie van uw keuze, met inbegrip van Linux-installatiekopieën, vanuit Azure Marketplace toevoegen.

Als u in een scenario voor verbonden werkt en als u uw Azure-Stack-exemplaar bij Azure hebt geregistreerd, klikt u vervolgens u de installatiekopie van het Windows Server 2016 VM vanuit Azure Marketplace downloaden kunt met behulp van de stappen in de [downloaden marketplace items van Azure naar Azure Stack](azure-stack-download-azure-marketplace-item.md) onderwerp.

Zie voor meer informatie over het toevoegen van andere items die aan de marketplace [Stack met de Azure Marketplace](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u een aanbieding hebt gemaakt, kunt u het kunt testen. Meld u aan als een gebruiker en zich abonneren op de aanbieding en voeg vervolgens een virtuele machine.

1. **Abonneren op een aanbieding**

   Nu kunt u aanmelden bij de portal als een gebruiker zich abonneren op een aanbieding.

   a. Meld u aan bij de gebruikersportal aanmeldt als een gebruiker en klik op **Neem een abonnement op**.
   - De URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal voor een geïntegreerd systeem. &lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure-Stack Development Kit gebruikt, is het adres van de portal https://portal.local.azurestack.external.

   ![Een abonnement nemen](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. In de **weergavenaam** veld, typ een naam voor uw abonnement, klik op **bieden**, klikt u op een van de aanbiedingen in de **kiest u een aanbieding** sectie en klik vervolgens op  **Maak**.

   ![Een aanbieding maken](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Als u wilt weergeven van het abonnement dat u hebt gemaakt, klikt u op **meer services**, klikt u op **abonnementen**, klik vervolgens op het nieuwe abonnement.  

   Nadat u zich op een aanbieding abonneert, vernieuw de portal om te zien welke services deel uitmaken van het nieuwe abonnement.

2. **Een virtuele machine inrichten**

   Nu kunt u aanmelden bij de portal als een gebruiker voor het inrichten van een virtuele machine met behulp van het abonnement. 

   a. Meld u aan bij de gebruikersportal aanmeldt als een gebruiker.
      - De URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal voor een geïntegreerd systeem. &lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure-Stack Development Kit gebruikt, is het adres van de portal https://portal.local.azurestack.external.

   b.  Klik op het dashboard **nieuw** > **Compute** > **Windows Server 2016 Datacenter Eval**, en klik vervolgens op **maken**.

   c. In de **basisbeginselen** sectie, typt u een **naam**, **gebruikersnaam**, en **wachtwoord**, kies een **abonnement**, Maak een **resourcegroep** (of een bestaande set selecteren) en klik vervolgens op **OK**.

   d. In de **een grootte kiezen** sectie, klikt u op **A1 standaard**, en klik vervolgens op **Selecteer**.  

   e. In de **instellingen** sectie, klikt u op **virtueel netwerk**. In de **virtueel netwerk kiezen** sectie, klikt u op **nieuw**. In de **virtueel netwerk maken** sectie, accepteer alle standaardwaarden en klik op **OK**. In de **instellingen** sectie, klikt u op **OK**.

   ![Virtueel netwerk maken](media/azure-stack-provision-vm/image04.png)

   f. In de **samenvatting** sectie, klikt u op **OK** voor het maken van de virtuele machine.  

   g. Klik op een overzicht van de nieuwe virtuele machine **alle resources**, zoekt u naar de virtuele machine en klik op de naam.

    ![Alle resources](media/azure-stack-provision-vm/image06.png)

Wat u hebt geleerd in deze zelfstudie:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een installatiekopie toe te voegen
> * Testen van de aanbieding

> [!div class="nextstepaction"]
> [Web-, mobiele en API-apps beschikbaar te maken voor uw Azure-Stack-gebruikers](azure-stack-tutorial-app-service.md)
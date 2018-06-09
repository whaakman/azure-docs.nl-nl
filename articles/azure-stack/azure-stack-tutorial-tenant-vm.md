---
title: Virtuele machines beschikbaar maken voor de gebruikers van uw Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u virtuele machines beschikbaar zijn op Azure-Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 9329cb0dbfa24cf239b820573ef7f642cdca9103
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248156"
---
# <a name="tutorial-make-virtual-machines-available-to-your-azure-stack-users"></a>Zelfstudie: virtuele machines beschikbaar te maken voor uw Azure-Stack-gebruikers

U kunt aanbiedingen die uw gebruikers (ook wel aangeduid als tenants) kunnen zich abonneren op maken als de beheerder van een Azure-Stack-cloud. Abonneer u op een aanbieding, kunnen gebruikers de Stack van Azure-services die voorziet in een aanbieding in beslag nemen.

Deze zelfstudie laat zien hoe een aanbieding voor een virtuele machine maken en vervolgens weer aanmelden als een gebruiker voor het testen van de aanbieding.

Wat u leert:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een installatiekopie toe te voegen
> * Testen van de aanbieding

In Azure-Stack worden services geleverd aan gebruikers met behulp van de abonnementen, aanbiedingen en plannen. Gebruikers kunnen zich abonneren op meerdere voorstellen. Een aanbieding kan een of meer abonnementen hebben kan, en een plan een of meer services.

![Abonnementen en aanbiedingen plannen](media/azure-stack-key-features/image4.png)

Zie voor meer informatie, [belangrijke functies en -concepten in Azure-Stack](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Een aanbieding maken

Aanbiedingen zijn een of meer plannen providers die aanwezig zijn voor gebruikers aan te schaffen of zich abonneert op groepen. Het proces van het maken van een aanbieding heeft verschillende stappen. U wordt gevraagd eerst de aanbieding en vervolgens een plannings- en ten slotte quota's maken.

1. [Meld u aan](azure-stack-connect-azure-stack.md) naar de portal als een cloudbeheerder en selecteer vervolgens **nieuw** > **biedt + plannen** > **bieden**.

   ![Nieuwe aanbieding](media/azure-stack-tutorial-tenant-vm/image01.png)

2. In **nieuwe bieden**, voer een **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de beschrijvende naam van de aanbieding. Alleen de operator cloud kunt zien dat de naam van de Resource. Dit is de naam die beheerders gebruiken om met de aanbieding te werken als Azure Resource Manager-resource.

   ![Weergavenaam](media/azure-stack-tutorial-tenant-vm/image02.png)

3. Selecteer **baseren plannen**, en in de **Plan** sectie **toevoegen** een nieuw plan toevoegen aan de aanbieding.

   ![Een plan toevoegen](media/azure-stack-tutorial-tenant-vm/image03.png)

4. In de **nieuwe plannen** sectie, vul **weergavenaam** en **resourcenaam**. De weergegeven naam is van het plan beschrijvende naam die gebruikers te zien. Alleen de operator cloud kunt zien dat de naam van de Resource. Dit is de naam die cloudoperators gebruiken om te werken met het plan als een Azure Resource Manager-resource.

   ![Weergavenaam plannen](media/azure-stack-tutorial-tenant-vm/image04.png)

5. Selecteer **Services**. Kies uit de lijst met Services **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**. Kies **Selecteer** deze services toevoegen aan het plan.

   ![Services plannen](media/azure-stack-tutorial-tenant-vm/image05.png)

6. Selecteer **quota**, en selecteer vervolgens de eerste service die u wilt maken van een quotum voor. Gebruik het volgende voorbeeld als richtlijn voor het configureren van de quota voor de Compute, Network en Storage-services voor een IaaS-quotum.

   - Maak eerst een quotum voor de Compute-service. Selecteer in de lijst naamruimte **Microsoft.Compute** en selecteer vervolgens **maken nieuwe quota**.

     ![Nieuw quotum maken](media/azure-stack-tutorial-tenant-vm/image06.png)

   - In **maken quotum**, voer een naam voor het quotum. U kunt wijzigen of accepteren op een van de quotawaarden die worden weergegeven voor de quota die u maakt. In dit voorbeeld wordt de standaardinstellingen accepteren en selecteer **OK**.

     ![De naam van de quota](media/azure-stack-tutorial-tenant-vm/image07.png)

   - Kies **Microsoft.Compute** in de lijst van de naamruimte en selecteer vervolgens de quota die u hebt gemaakt. Het quotum gekoppeld aan de Compute-service.

     ![Selecteer quotum](media/azure-stack-tutorial-tenant-vm/image08.png)

      Herhaal deze stappen voor de netwerk- en -services. Wanneer u klaar bent, selecteert u **OK** in **quota** alle quota's opslaan.

7. In **nieuw plan**, selecteer **OK**.

8. Onder **Plan**, selecteert u het nieuwe plan en vervolgens **Selecteer**.

9. In **nieuwe aanbieding**, selecteer **maken**. U ziet een melding wanneer de aanbieding wordt gemaakt.

10. Selecteer in het menu dashboard **biedt** en selecteer vervolgens de aanbieding die u hebt gemaakt.

11. Selecteer **wijziging van de status**, en vervolgens kiest **openbare**.

    ![Openbare status](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Een installatiekopie toe te voegen

Voordat u virtuele machines inrichten kan, moet u een installatiekopie toevoegen aan de Stack van Azure marketplace. U kunt de installatiekopie van uw keuze, met inbegrip van Linux-installatiekopieën, vanuit Azure Marketplace toevoegen.

Als u in een scenario voor verbonden werkt en als u uw Azure-Stack-exemplaar bij Azure hebt geregistreerd, klikt u vervolgens u de installatiekopie van het Windows Server 2016 VM vanuit Azure Marketplace downloaden kunt met behulp van de stappen in de [downloaden marketplace items van Azure naar Azure Stack](azure-stack-download-azure-marketplace-item.md) onderwerp.

Zie voor meer informatie over het toevoegen van andere items die aan de marketplace [Stack met de Azure Marketplace](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u een aanbieding hebt gemaakt, kunt u het kunt testen. U moet zich aanmelden als een gebruiker zich abonneren op de aanbieding en voeg vervolgens een virtuele machine.

1. **Abonneren op een aanbieding**

   a. Meld u aan bij de gebruikersportal aanmeldt met een gebruikersaccount en selecteer de **ophalen van een abonnement** tegel.
   - Voor een geïntegreerde, de URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure-Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

   ![Een abonnement nemen](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. In **Neem een abonnement op**, voer een naam voor uw abonnement in de **weergavenaam** veld. Selecteer **bieden**, en kies vervolgens een van de aanbiedingen in de **kiest u een aanbieding** lijst. Selecteer **Maken**.

   ![Een aanbieding maken](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Als u wilt weergeven van het abonnement, selecteer **meer services**, en selecteer vervolgens **abonnementen**. Selecteer het nieuwe abonnement om te zien welke services deel uitmaken van het abonnement.

   >[!NOTE]
   >Nadat u zich op een aanbieding abonneert, moet u wellicht de portal om te zien welke services deel uitmaken van het nieuwe abonnement te vernieuwen.

2. **Een virtuele machine inrichten**

   U kunt een virtuele machine via het nieuwe abonnement inrichten van de gebruikersportal.

   a. Meld u aan bij de gebruikersportal aanmeldt met een gebruikersaccount.
      - Voor een geïntegreerde, de URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure-Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

   b.  Selecteer op het dashboard **nieuw** > **Compute** > **Windows Server 2016 Datacenter Eval**, en selecteer vervolgens **maken**.

   c. In **basisbeginselen**, geef de volgende informatie:
      - Voer een **naam**
      - Voer een **gebruikersnaam**
      - Voer een **wachtwoord**
      - Kies een **abonnement**
      - Maak een **resourcegroep** (of een bestaande set selecteren.) 
      - Selecteer **OK** deze gegevens wilt opslaan.

   d. In **een grootte kiezen**, selecteer **A1 standaard**, en vervolgens **Selecteer**.  

   e. In **instellingen**, selecteer **virtueel netwerk**.

   f. In **virtueel netwerk kiezen**, selecteer **nieuw**.

   g. In **virtueel netwerk maken**, accepteer alle standaardwaarden en selecteert u **OK**.

   h. Selecteer **OK** in **instellingen** om op te slaan van de netwerkconfiguratie.

   ![Virtueel netwerk maken](media/azure-stack-provision-vm/image04.png)

   i. In **samenvatting**, selecteer **OK** voor het maken van de virtuele machine.  

   j. Overzicht van de nieuwe virtuele machine selecteren **alle resources**. De virtuele machine zoekt en selecteert u de naam van de zoekresultaten.

   ![Alle resources](media/azure-stack-provision-vm/image06.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een installatiekopie toe te voegen
> * Testen van de aanbieding

Voorafgaande aan de volgende zelfstudie voor meer informatie over hoe:
> [!div class="nextstepaction"]
> [SQL-databases beschikbaar maken voor uw Azure-Stack-gebruikers](azure-stack-tutorial-sql-server.md)
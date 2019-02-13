---
title: Virtuele machines beschikbaar maken voor uw Azure Stack-gebruikers | Microsoft Docs
description: Leer hoe u virtuele machines beschikbaar in Azure Stack
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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.lastreviewed: 09/11/2018
ms.custom: mvc
ms.openlocfilehash: a24b7314775ea7667ab992742586e5b5c8bbb369
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205153"
---
# <a name="tutorial-make-virtual-machines-available-to-your-azure-stack-users"></a>Zelfstudie: virtuele machines beschikbaar te maken voor uw Azure Stack-gebruikers

Als de beheerder van een Azure Stack-cloud, kunt u de aanbiedingen die uw gebruikers (soms aangeduid als tenants) kunnen zich abonneren op maken. Als u zich abonneert op een aanbieding, kunnen gebruikers de Azure Stack-services die een aanbieding biedt gebruiken.

Deze zelfstudie laat zien hoe u een aanbieding voor een virtuele machine maken en meld u vervolgens als een gebruiker aan voor het testen van de aanbieding.

Wat u leert:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een afbeelding toevoegen
> * Testen van de aanbieding

Services worden geleverd in Azure Stack, aan gebruikers via abonnementen, aanbiedingen en plannen. Gebruikers kunnen zich abonneren op meerdere aanbiedingen. Een aanbieding kan een of meer abonnementen hebben kan, en een abonnement een of meer services.

![Abonnementen, aanbiedingen en plannen](media/azure-stack-key-features/image4.png)

Zie voor meer informatie, [belangrijke functies en concepten in Azure Stack](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Een aanbieding maken

Aanbiedingen zijn groepen van een of meer plannen die providers die aanwezig voor gebruikers aan te schaffen of zich abonneert zijn op. Het proces voor het maken van een aanbieding heeft een aantal stappen uitvoeren. Eerst gevraagd u te maken van de aanbieding en vervolgens een plan en ten slotte, quota's.

1. [Meld u aan](azure-stack-connect-azure-stack.md) naar de portal als een cloudbeheerder en selecteer vervolgens **+ een resource maken** > **aanbiedingen + plannen** > **bieden**.

   ![Nieuwe aanbieding](media/azure-stack-tutorial-tenant-vm/image01.png)

1. In **nieuwe aanbieding**, voer een **weergavenaam** en **resourcenaam**, en vervolgens selecteert u een nieuwe of bestaande **resourcegroep**. De weergavenaam is de beschrijvende naam van de aanbieding. Alleen de operator cloud ziet de naam van de Resource dit is de naam die beheerders gebruiken om te werken met de aanbieding als een Azure Resource Manager-resource.

   ![Weergavenaam](media/azure-stack-tutorial-tenant-vm/image02.png)

1. Selecteer **Basisplannen**, en klik in de **Plan** sectie, selecteer **toevoegen** een nieuw abonnement toevoegen aan de aanbieding.

   ![Een plan toevoegen](media/azure-stack-tutorial-tenant-vm/image03.png)

1. In de **nieuw plan** sectie, vul **weergavenaam** en **resourcenaam**. De weergavenaam is de beschrijvende naam van het plan die gebruikers te zien. Alleen de operator cloud ziet de naam van de Resource dit is de naam die cloudoperators gebruiken om te werken met het abonnement als een Azure Resource Manager-resource.

   ![Weergavenaam van het plan](media/azure-stack-tutorial-tenant-vm/image04.png)

1. Selecteer **Services**. Kies uit de lijst met Services, **Microsoft.Compute**, **Microsoft.Network**, en **Microsoft.Storage**. Kies **Selecteer** deze services toevoegen aan het abonnement.

   ![Services plannen](media/azure-stack-tutorial-tenant-vm/image05.png)

1. Selecteer **quota**, en selecteer vervolgens de eerste service die u wilt maken van een quotum voor. Gebruik het volgende voorbeeld als richtlijn voor het configureren van quota's voor de Reken-, netwerk- en -services voor een IaaS-quotum.

   - Maak eerst een quotum voor de Compute-service. Selecteer in de lijst naamruimte **Microsoft.Compute** en selecteer vervolgens **nieuw quotum maken**.

     ![Nieuw quotum maken](media/azure-stack-tutorial-tenant-vm/image06.png)

   - In **maken quotum**, voer een naam voor het quotum. U kunt wijzigen of alle van de quotawaarden die worden weergegeven voor de quota die u maakt accepteren. In dit voorbeeld wordt de standaardinstellingen accepteren en selecteer **OK**.

     ![Quotumnaam](media/azure-stack-tutorial-tenant-vm/image07.png)

   - Kies **Microsoft.Compute** in de lijst van de naamruimte en selecteer vervolgens de quota die u hebt gemaakt. Hierdoor worden gekoppeld. het quotum voor de Compute-service.

     ![Selecteer quota](media/azure-stack-tutorial-tenant-vm/image08.png)

      Herhaal deze stappen voor de netwerk- en -services. Wanneer u klaar bent, selecteert u **OK** in **quota** om op te slaan van de quota's.

1. In **nieuw plan**, selecteer **OK**.

1. Onder **Plan**, selecteert u het nieuwe abonnement en vervolgens **Selecteer**.

1. In **nieuwe aanbieding**, selecteer **maken**. U ziet een melding wanneer de aanbieding wordt gemaakt.

1. Selecteer in het menu dashboard **biedt** en kies vervolgens de aanbieding die u hebt gemaakt.

1. Selecteer **status wijzigen**, en daarna **openbare**.

    ![Openbare status](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Een afbeelding toevoegen

Voordat u virtuele machines inrichten kunt, moet u een afbeelding toevoegen aan de Azure Stack marketplace. U kunt de installatiekopie van uw keuze, met inbegrip van Linux-installatiekopieën op Azure Marketplace toevoegen.

Als u in een verbonden scenario werkt en als u uw Azure Stack-exemplaar bij Azure hebt geregistreerd, klikt u vervolgens u de installatiekopie van het Windows Server 2016-VM vanuit de Azure Marketplace downloaden kunt met behulp van de stappen in de [marketplace downloaden items van Azure naar Azure Stack](azure-stack-download-azure-marketplace-item.md) onderwerp.

Zie voor meer informatie over het toevoegen van andere items in de marketplace [de Azure Stack Marketplace](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u een aanbieding hebt gemaakt, kunt u deze testen. U kunt zich moet aanmelden als een gebruiker Abonneer u op de aanbieding, en voeg een virtuele machine.

1. **Abonneren op een aanbieding**

   a. Meld u aan bij de gebruikersportal aanmeldt met een gebruikersaccount en selecteer de **Neem een abonnement** tegel.
   - Voor een geïntegreerd systeem, de URL is afhankelijk van op de regio en het externe domeinnaam van de operator en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

   ![Een abonnement nemen](media/azure-stack-tutorial-tenant-vm/image10.png)

   b. In **Neem een abonnement**, voer een naam in voor uw abonnement in de **weergavenaam** veld. Selecteer **bieden**, en kies vervolgens een van de aanbiedingen in de **Kies een aanbieding** lijst. Selecteer **Maken**.

   ![Een aanbieding maken](media/azure-stack-tutorial-tenant-vm/image11.png)

   c. Als u het abonnement, selecteer **alle services**, en klik vervolgens onder de **algemene** categorie selecteren **abonnementen**. Selecteer uw nieuwe abonnement om te zien welke services deel uitmaken van het abonnement.

   >[!NOTE]
   >Nadat u zich op een aanbieding abonneert, moet u wellicht Vernieuw de portal om te zien welke services deel uitmaken van het nieuwe abonnement.

1. **Een virtuele machine inrichten**

   U kunt een virtuele machine met behulp van het nieuwe abonnement inrichten van de gebruikersportal.

   a. Meld u aan bij de gebruikersportal aanmeldt met een gebruikersaccount.
      - Voor een geïntegreerd systeem, de URL is afhankelijk van op de regio en het externe domeinnaam van de operator en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
   - Als u de Azure Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

   b.  Selecteer op het dashboard, **+ een resource maken** > **Compute** > **Windows Server 2016 Datacenter Eval**, en selecteer vervolgens **Maken**.

   c. In **basisbeginselen**, geef de volgende informatie:
      - Voer een **naam**
      - Voer een **gebruikersnaam**
      - Voer een **wachtwoord**
      - Kies een **abonnement**
      - Maak een **resourcegroep** (of Selecteer een bestaande resourcegroep.) 
      - Selecteer **OK** deze gegevens op te slaan.

   d. In **Kies een grootte**, selecteer **Standard A1**, en vervolgens **Selecteer**.  

   e. In **instellingen**, selecteer **virtueel netwerk**.

   f. In **virtueel netwerk kiezen**, selecteer **nieuw**.

   g. In **virtueel netwerk maken**, accepteert u alle standaardwaarden en selecteer **OK**.

   h. Selecteer **OK** in **instellingen** om op te slaan van de netwerkconfiguratie.

      i. In **samenvatting**, selecteer **OK** te maken van de virtuele machine.  

   j. Als u wilt zien van de nieuwe virtuele machine, selecteer **alle resources**. Zoeken naar de virtuele machine en selecteert u de naam van de lijst met zoekresultaten.

   
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een aanbieding maken
> * Een afbeelding toevoegen
> * Testen van de aanbieding

Ga verder met de volgende zelfstudie voor meer informatie over het:
> [!div class="nextstepaction"]
> [SQL-databases beschikbaar maken voor uw Azure Stack-gebruikers](azure-stack-tutorial-sql-server.md)
---
title: Azure Security Center Quick Start - vrijgeven uw Windows-computers met Security Center | Microsoft Docs
description: Deze snelstartgids wordt beschreven hoe u voor het inrichten van Microsoft Monitoring Agent op een Windows-computer.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: 50cbbca9181d67bc41632a4650c76b9636a72356
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Snelstartgids: Ingebouwde Windows-computers met Azure Security Center
Nadat u vrijgeven uw Azure-abonnementen, kunt u inschakelen Security Center voor resources buiten Azure, actief is voor voorbeeld on-premises of andere clouds met het inrichten van Microsoft Monitoring Agent.

Deze snelstartgids wordt beschreven hoe u Microsoft Monitoring Agent installeren op een Windows-computer.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

U moet op de Standard-prijscategorie van Security Center voordat deze snelstartgids wordt gestart. Zie [Onboard uw Azure-abonnement Security Center Standard](security-center-get-started.md) voor upgrade-instructies. U kunt Security Center van standaard gratis proberen voor de eerste 60 dagen.

## <a name="add-new-windows-computer"></a>Toevoegen van nieuwe Windows-computer

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - overzicht** wordt geopend.

 ![Security Center-overzicht][2]

3. Selecteer onder het hoofdmenu Security Center **voorbereiding voor geavanceerde beveiliging**.
4. Selecteer **wilt u niet-Azure-computers toevoegen**.

   ![Onboarding van geavanceerde beveiliging][3]

5. Op **nieuwe niet-Azure-computers toevoegen**, een lijst met uw Log Analytics-werkruimten wordt weergegeven. De lijst bevat, indien van toepassing, de standaard-werkruimte gemaakt door Security Center wanneer automatische inrichting is ingeschakeld. Selecteer deze werkruimte of een andere werkruimte die u wilt gebruiken.

    ![Niet-Azure-computer toevoegen][4]

  De **Direct Agent** blade wordt geopend met een koppeling voor het downloaden van een Windows-agent en -sleutels voor uw werkruimte-ID moet worden gebruikt in de configuratie van de agent.

6.  Selecteer de **Windows-Agent downloaden** koppeling van toepassing op uw computer processortype voor het downloaden van het setup-bestand.

7.  Aan de rechterkant van **werkruimte-ID**, selecteer het pictogram kopiëren en plakken van de ID in Kladblok.

8.  Aan de rechterkant van **primaire sleutel**, selecteer het pictogram kopiëren en plakken van de sleutel in Kladblok.

## <a name="install-the-agent"></a>De agent installeren
Nu moet u het gedownloade bestand op de doelcomputer installeren.

1. Kopieer het bestand naar de doelcomputer en voer Setup uit.
2. Op de **Welkom** pagina **volgende**.
3. Op de **licentievoorwaarden** pagina, lees de licentievoorwaarden en selecteer vervolgens **ik ga akkoord**.
4. Op de **doelmap** pagina, wijzigen of de standaardinstallatiemap houden en selecteer vervolgens **volgende**.
5. Op de **installatieopties voor Agent** pagina, kiest u moet de agent verbinden met naar Azure logboekanalyse (OMS) en selecteer vervolgens **volgende**.
6. Op de **Azure Log Analytics** pagina, plak de **werkruimte-ID** en **Werkruimtesleutel (primaire sleutel)** die u in de vorige procedure hebt gekopieerd in Kladblok.
7. Als de computer een werkruimte voor logboekanalyse in Azure Government cloud rapporteren moet, selecteert u **Azure US Government** formulier de **Azure-Cloud** vervolgkeuzelijst.  Als de computer communiceren met de Log Analytics-service via een proxyserver moet, selecteert u **Geavanceerd** en geef de URL en het poortnummer van de proxyserver.
8. Selecteer **volgende** als u klaar bent met het leveren van de vereiste configuratie-instellingen.

  ![De agent installeren][5]

9. Op de **gereed voor installatie** pagina, Controleer uw selecties en selecteer vervolgens **installeren**.
10. Op de **configuratie voltooid** pagina **voltooien**

Als u klaar bent wordt de **MMA** in het **Configuratiescherm** weergegeven. U kunt de configuratie er bekijken en controleren of de agent is verbonden.

Zie voor meer informatie over het installeren en configureren van de agent [verbinding maken met Windows-computers](../log-analytics/log-analytics-agent-windows.md#install-the-agent-using-setup).

U kunt nu uw Azure VM's en niet-Azure-computers op één plek bewaken. Onder **Compute**, hebt u een overzicht van alle virtuele machines en computers plus de aanbevelingen. Elke kolom vertegenwoordigt een reeks aanbevelingen. De kleur vertegenwoordigt van de virtuele machine of de computer huidige beveiligingsstatus voor die aanbeveling. Security Center geeft alle detecties voor deze computers in beveiligingswaarschuwingen ook weer.

  ![Blade berekenen][6]

Er zijn twee soorten pictogrammen worden weergegeven op de **Compute** blade:

![pictogram1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Niet-Azure-computer

![pictogram2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer het niet meer nodig hebt, kunt u de agent verwijderen uit de Windows-computer.

De agent verwijderen:

1. Open het **Configuratiescherm**.
2. Open **Programma's en onderdelen**.
3. Selecteer in **Programma's en onderdelen** de optie **MMA** en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids ingericht u Microsoft Monitoring Agent op een Windows-computer. Voor meer informatie over het gebruik van Security Center, blijven de zelfstudie voor het configureren van een beveiligingsbeleid en het bepalen van de beveiliging van uw resources.

> [!div class="nextstepaction"]
> [Zelfstudie: Definiëren en beoordelen van beveiligingsbeleid](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png

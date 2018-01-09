---
title: Azure Security Center Quick Start - vrijgeven uw Linux-computers met Security Center | Microsoft Docs
description: Deze snelstartgids ziet u hoe voorbereiden uw Linux-computers met Security Center.
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
ms.openlocfilehash: 365afd2199b1b8b2e70d882f6a729384edbdffbc
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Snelstartgids: Ingebouwde Linux-computers met Azure Security Center
Nadat u vrijgeven uw Azure-abonnementen, kunt u inschakelen Security Center voor Linux-resources buiten Azure, voor voorbeeld on-premises of in andere clouds uitgevoerd door de Linux-Agent wordt ingericht.

Deze snelstartgids ziet u hoe de Linux-Agent installeren op een Linux-computer.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

U moet op de Standard-prijscategorie van Security Center voordat deze snelstartgids wordt gestart. Zie [Onboard uw Azure-abonnement Security Center Standard](security-center-get-started.md) voor upgrade-instructies. U kunt Security Center van standaard gratis proberen voor de eerste 60 dagen.

## <a name="add-new-linux-computer"></a>Toevoegen van nieuwe Linux-computer

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - overzicht** wordt geopend.

 ![Security Center-overzicht][2]

3. Selecteer onder het hoofdmenu Security Center **voorbereiding voor geavanceerde beveiliging**.
4. Selecteer **wilt u niet-Azure-computers toevoegen**.
   ![Onboarding van geavanceerde beveiliging][3]

5. Op **nieuwe niet-Azure-computers toevoegen**, een lijst met uw Log Analytics-werkruimten wordt weergegeven. De lijst bevat, indien van toepassing, de standaard-werkruimte gemaakt door Security Center wanneer automatische inrichting is ingeschakeld. Selecteer deze werkruimte of een andere werkruimte die u wilt gebruiken.

    ![Niet-Azure-computer toevoegen][4]

6.  Op de **Direct Agent** pagina onder **downloaden en VRIJGEVEN AGENT voor LINUX**, selecteer de **kopie** knop om te kopiëren de *wget* opdracht.

7.  Open Kladblok en plak deze opdracht. Sla dit bestand naar een locatie die toegankelijk is vanaf uw Linux-computer kan worden.

## <a name="install-the-agent"></a>De agent installeren

1.  Open het bestand dat is opgeslagen op uw Linux-computer. Selecteert u de volledige inhoud kopiëren en plakken van de opdracht een terminal-console openen.
2.  Zodra de installatie is voltooid, kunt u controleren dat de *omsagent* is geïnstalleerd door het uitvoeren van de *pgrep* opdracht. De opdracht retourneert de *omsagent* PID (proces-ID) zoals hieronder wordt weergegeven:

  ![De agent installeren][5]

De logboeken voor de Security Center-Agent voor Linux kunnen u vinden op: */var/opt/microsoft/omsagent/<workspace id>/log/*

  ![Logboeken voor agent][6]

Na enige tijd duurt maximaal 30 minuten, de nieuwe Linux-computer wordt weergegeven in Security Center.

U kunt nu uw Azure VM's en niet-Azure-computers op één plek bewaken. Onder **Compute**, hebt u een overzicht van alle virtuele machines en computers plus de aanbevelingen. Elke kolom vertegenwoordigt een reeks aanbevelingen. De kleur vertegenwoordigt van de virtuele machine of de computer huidige beveiligingsstatus voor die aanbeveling. Security Center geeft alle detecties voor deze computers in beveiligingswaarschuwingen ook weer.

  ![Blade COMPUTE][7] er zijn twee soorten pictogrammen worden weergegeven op de **Compute** blade:

  ![pictogram1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Niet-Azure-computer

  ![pictogram2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, kunt u de agent verwijderen van de Linux-computer.

De agent verwijderen:

1. De Linux-agent downloaden [universal script](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) op de computer.
2. Voer het .sh bundle-bestand met de *--opschonen* argument op de computer waarop de agent en de configuratie volledig verwijderd.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Volgende stappen
In deze snel starten, moet u de agent op een Linux-computer ingericht. Voor meer informatie over het gebruik van Security Center, blijven de zelfstudie voor het configureren van een beveiligingsbeleid en het bepalen van de beveiliging van uw resources.

> [!div class="nextstepaction"]
> [Zelfstudie: Definiëren en beoordelen van beveiligingsbeleid](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png

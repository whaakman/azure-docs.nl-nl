---
title: Beveiligen van uw computers en toepassingen in Azure Security Center | Microsoft Docs
description: Dit document bevat aanbevelingen in Security Center die u helpen beveiligen van uw virtuele machines en computers en uw web-apps en App Service-omgevingen.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2018
ms.author: rkarlin
ms.openlocfilehash: 7cf5f86d9a2d121ff54c40e27c6bc50847a4dfdf
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43133031"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Beveiligen van uw computers en toepassingen in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen. Aanbevelingen zijn van toepassing op Azure-resource-typen: virtuele machines (VM's) en computers, toepassingen, netwerken, SQL, en de identiteit en toegang.

In dit artikel biedt aanbevelingen die betrekking hebben op de machines en toepassingen.

## <a name="monitoring-security-health"></a>Beveiligingsstatus bewaken
U kunt de beveiligingsstatus van uw resources bewaken op de **Security Center-overzicht** dashboard. De **Resources** sectie bevat het aantal problemen geïdentificeerd en de beveiligingsstatus voor elk resourcetype.

U kunt een lijst van alle problemen weergeven door te selecteren **aanbevelingen**. Zie voor meer informatie over het toepassen van aanbevelingen [beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md).

Zie voor een volledige lijst van reken- en App-services aanbevelingen, [aanbevelingen](security-center-virtual-machine-recommendations.md).

Als u wilt doorgaan, selecteer **Compute en apps** onder **Resources** of het hoofdmenu van Security Center.
![Security Center-dashboard][1]

## <a name="monitor-compute-and-app-services"></a>Monitor voor reken- en App-services
Onder **Compute**, er zijn vier tabbladen:

- **Overzicht**: bewaking en aanbevelingen geïdentificeerd door Security Center.
- **Virtuele machines en computers**: lijst met uw virtuele machines, computers en huidige beveiligingsstatus van elk.
- **Cloud Services**: lijst met uw web- en werkrollen rollen bewaakt door Security Center.
- **App-services (Preview)**: lijst met uw App service-omgevingen en de huidige beveiligingsstatus van elk.
Als u wilt doorgaan, selecteer **Compute en apps** onder **Resources** of het hoofdmenu van Security Center.

![Compute][2]

Op elk tabblad kunt u meerdere sectie hebben en in elke sectie kunt u een afzonderlijke optie selecteren voor meer informatie over de aanbevolen stappen voor het oplossen van het specifieke probleem.

### <a name="monitoring-recommendations"></a>Aanbevelingen ten aanzien van controle
In deze sectie toont het totale aantal virtuele machines en computers dat is geïnitialiseerd voor automatische inrichting en de huidige status. In dit voorbeeld is er één aanbeveling **Problemen met de agentstatus controleren**. Selecteer deze aanbeveling.

![Problemen met de agentstatus controleren][3]

**Problemen met de agentstatus controleren** wordt geopend. Virtuele machines en computers die Security Center niet kan controleren, worden vermeld. Selecteer een virtuele machine of computer voor gedetailleerde informatie. **BEWAKINGSSTATUS** toont de reden waarom Security Center niet kan bewaken. Raadpleeg de [Security Center probleemoplossingsgids](security-center-troubleshooting-guide.md) voor een lijst met waarden, beschrijvingen en probleemoplossingen voor de waarden van **BEWAKINGSSTATUS**.

### Niet-bewaakte VM's en computers <a name="unmonitored-vms-and-computers"></a>
Een virtuele machine of computer wordt niet bewaakt door Security Center als de machine wordt niet uitgevoerd voor de Microsoft Monitoring Agent-extensie. Een computer heeft misschien een lokale agent is al geïnstalleerd, bijvoorbeeld de directe OMS-agent of de SCOM-agent. Machines met deze agents worden geïdentificeerd als niet-bewaakt omdat deze agents niet volledig worden ondersteund in Security Center. De Microsoft Monitoring Agent-extensie is vereist om volledig te profiteren van alle mogelijkheden van Security Center.

U kunt de extensie installeren op de niet-bewaakte virtuele machine of computer naast de reeds geïnstalleerde lokale agent. Configureer beide agents op dezelfde manier en verbind ze met dezelfde werkruimte. Hierdoor kan Security Center communiceren met de Microsoft Monitoring Agent-extensie en gegevens verzamelen. Zie [Enable the VM extension](../log-analytics/log-analytics-quick-collect-azurevm.md) (De VM-extensie inschakelen) voor installatie-instructies voor de Microsoft Monitoring Agent-extensie.

Zie [Problemen met de agentstatus controleren](security-center-troubleshooting-guide.md#mon-agent) voor meer informatie over de redenen waarom Security Center VM's en computers die zijn geïnitialiseerd voor automatische inrichting niet afdoende kan bewaken.

### <a name="recommendations"></a>Aanbevelingen
Deze sectie bevat een reeks aanbevelingen voor elke virtuele machine en computer, web-en werkrollen, Azure App Service Web Apps en Azure App Service Environment die wordt bewaakt door Security Center. De eerste kolom bevat de aanbeveling. De tweede kolom geeft het totale aantal resources dat is beïnvloed door deze aanbeveling. De derde kolom geeft de ernst van het probleem weer, zoals wordt geïllustreerd op de volgende schermafbeelding:

![Aanbevelingen][4]

Elke aanbeveling heeft een set acties die u kunt uitvoeren nadat u deze hebt geselecteerd. Als u bijvoorbeeld **ontbrekende systeemupdates**, het aantal virtuele machines en computers waarvoor patches ontbreken, en de ernst van de ontbrekende update wordt weergegeven, zoals wordt weergegeven in de volgende schermafbeelding:

![Systeemupdates toepassen][5]

**Systeemupdates toepassen** geeft een samenvatting van essentiële updates in een grafiek, één voor Windows en één voor Linux. Het tweede gedeelte bevat een tabel met de volgende informatie:

- **NAAM**: naam van de ontbrekende update.
- **AANTAL VM’S EN COMPUTERS**: totaal aantal virtuele machines en computers die deze update niet hebben.
- **ERNST van de UPDATE**: de ernst van deze bepaalde aanbeveling beschreven:

    - **Kritieke**: een beveiligingsprobleem bestaat een belangrijke resource (toepassing, virtuele machine of netwerkbeveiligingsgroep) en dit probleem vereist uw aandacht.
    - **Belangrijke**: niet-kritieke of extra stappen zijn vereist om een proces te voltooien of een beveiligingsprobleem.
    - **Gemiddeld**: een beveiligingsprobleem moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Aanbevelingen met de ernstaanduiding Laag worden niet standaard weergegeven, maar u kunt hierop filteren als u deze aanbevelingen wilt bekijken.)


- **STATUS**: de huidige status van de aanbeveling:

    - **Open**: de aanbeveling is nog niet opgelost.
    - **Wordt uitgevoerd**: de aanbeveling wordt momenteel toegepast op deze resources en u hoeft geen actie te ondernemen.
    - **Opgelost**: de aanbeveling is al voltooid. (Als het probleem is opgelost, wordt de vermelding grijs).

Klik op de naam van de ontbrekende update om meer informatie voor de aanbeveling weer te geven.

![Details van de aanbeveling][6]

> [!NOTE]
> Hier de aanbevelingen voor beveiliging zijn hetzelfde als die onder de **aanbevelingen** tegel. Zie [beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md) voor meer informatie over het toepassen van aanbevelingen.
>
>

### <a name="vms-and-computers"></a>VM's en computers
De sectie virtuele machines en computers geeft een overzicht van alle aanbevelingen voor de virtuele machine en computer. Elke kolom vertegenwoordigt een reeks aanbevelingen, zoals te zien is op de volgende schermafbeelding:

![Aanbevelingen voor virtuele machine en computer][7]

Er zijn vier typen pictogrammen weergegeven in deze lijst:

![Niet-Azure-computer][8] Computer zonder Azure.

![Virtuele machines van Azure Resource Manager][9] Virtuele machines van Azure Resource Manager.

![Klassieke Azure-VM][10] Klassieke Azure-VM.

![Virtuele machines geïdentificeerd vanuit de werkruimte][11] Virtuele machines die alleen zijn geïdentificeerd vanuit de werkruimte die deel uitmaakt van het weergegeven abonnement. Dit omvat VM's van andere abonnementen die rapporteren aan de werkruimte in dit abonnement, en VM’s die zijn geïnstalleerd met de directe SCOM-agent en geen resource-ID hebben.

U kunt met het pictogram dat wordt weergegeven onder elke aanbeveling snel identificeren van de virtuele machine en de computer die aandacht vereisen en wat het type aanbeveling. U kunt ook de filteroptie gebruiken om te selecteren welke opties u in dit scherm ziet.

![Filteren][12]

In het vorige voorbeeld heeft één VM een cruciale aanbeveling met betrekking tot eindpuntbeveiliging. Selecteer de virtuele machine voor meer informatie over het:

![Kritieke aanbeveling][13]

Hier ziet u de beveiligingsdetails voor de virtuele machine of computer. Onderin ziet u de aanbevolen actie en de ernst van elk probleem.

### <a name="cloud-services"></a>Cloud services
Voor cloudservices wordt een aanbeveling gemaakt wanneer de versie van het besturingssysteem verouderd is, zoals u ziet in de volgende schermafbeelding:

![Cloud services][14]

In een scenario waarin u een aanbeveling krijgt (wat is niet het geval is bij het vorige voorbeeld) hebt, moet u de stappen in de aanbeveling voor de versie van het besturingssysteem bij te werken. Wanneer een update beschikbaar is, ontvangt u een waarschuwing (rood of oranje, afhankelijk van de ernst van het probleem). Wanneer u deze waarschuwing selecteert in de WebRole1 (Windows Server wordt uitgevoerd met uw web-app automatisch geïmplementeerd in IIS) of WorkerRole1 (Windows Server wordt uitgevoerd met uw web-app automatisch geïmplementeerd in IIS) rijen, ziet u meer informatie over deze aanbeveling, zoals wordt weergegeven in de Schermafbeelding van het volgende:

![WorkerRole1][15]

Als u een meer beschrijvende uitleg wilt zien over deze aanbeveling, klikt u op **Besturingssysteemversie bijwerken** onder de kolom **BESCHRIJVING**.

![Besturingssysteemversie bijwerken][16]

### <a name="app-services-preview"></a>App Services (preview)

> [!NOTE]
> Bewaking van App Service is in preview en alleen beschikbaar op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
>
>

Onder **App services**, vindt u een lijst van uw App service-omgevingen en samenvatting van de status op basis van de evaluatie van Security Center wordt uitgevoerd.

![App-services][17]

Er zijn drie soorten pictogrammen weergegeven in deze lijst:

![Omgeving voor App-services][18] Omgeving van App-services.

![Webtoepassing][19] Web-App.

![Functions-toepassing][24] Functions-toepassing.

1. Selecteer een web-App. Een overzicht wordt geopend met drie tabbladen:

  - **Aanbevelingen**: op basis van beoordelingen die door Security Center die niet zijn uitgevoerd.
  - **Evaluaties doorgegeven**: lijst met beoordelingen uitgevoerd door Security Center die doorgegeven.
  - **Niet-beschikbare evaluaties**: lijst met beoordelingen die kan niet worden uitgevoerd vanwege een fout of de aanbeveling is niet relevant zijn voor de specifieke appservice

  Onder **aanbevelingen** is een lijst met aanbevelingen voor de geselecteerde web-App en de ernst van elke aanbeveling.

  ![Overzicht weergeven][20]

2. Selecteer een aanbeveling voor een beschrijving van de aanbeveling en een lijst van resources niet in orde, gezonde resources en niet-gescande resources.

  ![Beschrijving van de aanbeveling][21]

  Onder **evaluaties doorgegeven** is een lijst van geslaagde evaluaties.  Ernst van deze beoordeling is altijd groen.

  ![Geslaagde evaluaties][22]

3. Selecteer een evaluatie van de doorgegeven in de lijst voor een beschrijving van de evaluatie, een lijst met resources niet in orde en in orde is en een lijst met niet-gescande resources. Er is een tabblad voor de resources niet in orde, maar deze lijst is altijd leeg omdat de evaluatie van de doorgegeven.

    ![Resources die in orde zijn][23]



## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:


* [Inzicht in Azure Security Center aanbevelingen voor virtuele machines](security-center-virtual-machine-recommendations.md)
* [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png

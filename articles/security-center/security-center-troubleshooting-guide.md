---
title: Handleiding voor het oplossen van problemen met Azure Security Center | Microsoft Docs
description: Dit document helpt bij het oplossen van problemen in Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 0e0a0ce5c0795cec0e47cd5f729099f4762381a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-troubleshooting-guide"></a>Handleiding voor het oplossen van problemen met Azure Security Center
Deze handleiding is bedoeld voor IT-specialisten, informatiebeveiligingsanalisten en cloudbeheerders van organisaties die Azure Security Center gebruiken en biedt procedures voor het oplossen van problemen met Azure Security Center.

>[!NOTE] 
>Vanaf begin juni 2017 maakt Security Center gebruik van Microsoft Monitoring Agent voor het verzamelen en opslaan van gegevens. Zie [Migratie van Azure Security Center-platform](security-center-platform-migration.md) voor meer informatie. De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>

## <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
In deze handleiding wordt uitgelegd hoe u problemen oplost die betrekking hebben op Security Center. In de meeste gevallen vindt probleemoplossing in Security Center plaats door eerst de records in het [Controlelogboek](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) van het onderdeel met de fout te raadplegen. Met controlelogboeken kunt u het volgende bepalen:

* Welke bewerkingen er hebben plaatsgevonden
* Wie de bewerking heeft gestart
* Wanneer de bewerking is uitgevoerd
* De status van de bewerking
* De waarden van andere eigenschappen die u kunnen helpen bij het onderzoeken van het probleem

In het controlelogboek staan alle schrijfbewerkingen (PUT, POST, DELETE) die op uw resources zijn uitgevoerd, maar er staan geen leesbewerkingen (GET) in.

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Security Center maakt gebruik van de Microsoft Monitoring Agent. Dit is dezelfde agent die ook wordt gebruikt door de Operations Management Suite en de Log Analytics-service om beveiligingsgegevens van uw virtuele machines in Azure te verzamelen. Nadat het verzamelen van gegevens is ingeschakeld en de agent juist is geïnstalleerd op de doelcomputer, wordt als het goed is dit proces uitgevoerd:

* HealthService.exe

Als u de console voor servicebeheer (services.msc) opent, ziet u ook dat de service Microsoft Monitoring Agent wordt uitgevoerd, zoals hieronder is weergegeven:

![Services](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Als u wilt zien welke versie van de agent u hebt, opent u **Taakbeheer**, zoekt u op het tabblad **Processen** **Microsoft Monitoring Agent-Service**, klikt u er met de rechtermuisknop op en klikt u ten slotte op **Eigenschappen**. Op het tabblad **Details** kunt u de bestandsversie vinden, zoals hieronder wordt weergegeven:

![File](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a>Installatiescenario's voor Microsoft Monitoring Agent
Er zijn twee installatiescenario's die verschillende resultaten kunnen opleveren bij het installeren van Microsoft Monitoring Agent op uw computer. Dit zijn de ondersteunde scenario's:

* **Agent automatisch geïnstalleerd door Security Center**: in dit scenario kunt u de waarschuwingen op beide locaties bekijken, in Security Center en door te zoeken in logboeken. U ontvangt e-mailmeldingen op het e-mailadres dat is geconfigureerd in het beveiligingsbeleid voor het abonnement waarvan de resource deel uitmaakt.
.
* **Agent handmatig geïnstalleerd op een virtuele machine in Azure**: als u in dit scenario agents gebruikt die u hebt gedownload en handmatig hebt geïnstalleerd vóór februari 2017, kunt u de waarschuwingen alleen bekijken in de portal van Security Center als u filtert op het abonnement waartoe de werkruimte behoort. Als u filtert op het abonnement waartoe de resource behoort, krijgt u geen waarschuwingen te zien. U ontvangt e-mailmeldingen op het e-mailadres dat is geconfigureerd in het beveiligingsbeleid voor het abonnement waarvan de werkruimte deel uitmaakt.

>[!NOTE]
> U kunt het gedrag uit het tweede scenario voorkomen door de nieuwste versie van de agent te downloaden.
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>Problemen oplossen met de netwerkvereisten voor de Monitoring Agent
Agents kunnen alleen verbinding maken met Security Center en zich daarbij registreren als ze toegang hebben tot netwerkbronnen, inclusief de poortnummers en domein-URL's.

- Voor proxyservers moet u ervoor zorgen dat de juiste resources voor de proxyserver zijn geconfigureerd in de instellingen voor de agent. Lees dit artikel voor meer informatie over [het wijzigen van de proxy-instellingen](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- Als u een firewall gebruikt om de toegang tot internet te beperken, moet u uw firewall zodanig configureren dat toegang tot OMS wordt toegestaan. De agent-instellingen hoeven niet te worden aangepast.

De volgende tabel bevat de resources die nodig zijn voor communicatie.

| Agentresource | Poorten | HTTPS-controle overslaan |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |

Als er problemen zijn tijdens het onboarden van de agent, leest u het artikel [Onboarding-problemen van Operations Management Suite oplossen](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Oplossen van problemen met niet goed werkende eindpuntbeveiliging

De gastagent is het bovenliggende proces van alles wat de [Microsoft Antimalware](../security/azure-security-antimalware.md)-extensie doet. Wanneer het proces van de gastagent mislukt, kan de Microsoft Antimalware die wordt uitgevoerd als onderliggend proces van de gastagent ook mislukken.  In dergelijke scenario's wordt aanbevolen de volgende opties te controleren:

- Als de doel-VM een aangepaste installatiekopie is en de maker van de VM nooit een gastagent heeft geïnstalleerd.
- Als het doel een Linux-VM in plaats van een Windows-VM is, mislukt het installeren van de Windows-versie van de anti-malware-extensie op een Linux-VM. De Linux-gastagent heeft specifieke vereisten voor het besturingssysteem en de vereiste pakketten. Als niet aan deze vereisten wordt voldaan, werkt de VM-agent ook niet. 
- Als de VM is gemaakt met een oudere versie van gastagent. Als dat het geval is, houd er dan rekening mee dat een aantal oude agents niet automatisch kunnen bijwerken naar de nieuwe versie en dat dit tot dit probleem kan leiden. Gebruik altijd de nieuwste versie van gastagent als u uw eigen installatiekopieën maakt.
- Sommige beheersoftware van derden kan de gastagent uitschakelen of toegang tot bepaalde bestandslocaties blokkeren. Als u beheersoftware van derden op de VM hebt geïnstalleerd, zorgt u ervoor dat de agent op de uitsluitingslijst staat.
- Bepaalde firewallinstellingen of netwerkbeveiligingsgroepen (NSG) blokkeren mogelijk het netwerkverkeer naar en van de gastagent.
- Een toegangsbeheerlijst (ACL) blokkeert mogelijk de toegang tot de schijf.
- Onvoldoende schijfruimte kan er voor zorgen dat de gastagent niet goed functioneert. 

De gebruikersinterface van Microsoft Antimalware is standaard uitgeschakeld. Zie [Enabling Microsoft Antimalware User Interface on Azure Resource Manager VMs Post Deployment](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) (De Microsoft Antimalware-gebruikersinterface inschakelen op Azure Resource Manager-VM's na implementatie) voor meer informatie over hoe u deze kunt inschakelen.

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Problemen oplossen met het laden van het dashboard

Als u problemen ondervindt met het laden van het dashboard van Security Center, moet u ervoor zorgen dat de gebruiker die het Security Center-abonnement registreert (de eerste gebruiker die Security Center voor het abonnement opent) en de gebruiker die het verzamelen van gegevens inschakelt, in het abonnement de rol van *Eigenaar* of *Inzender* hebben. Vanaf dat moment kunnen ook gebruikers die in het abonnement de rol van *Lezer* hebben, het dashboard weergeven, en waarschuwingen, aanbevelingen en het beleid bekijken.

## <a name="contacting-microsoft-support"></a>Contact opnemen met Microsoft-ondersteuning
Bepaalde problemen kunnen worden geïdentificeerd aan de hand van de richtlijnen in dit artikel. Andere problemen vindt u ook beschreven in het openbare [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) van Security Center. Als u aanvullende hulp nodig hebt om bepaalde problemen op te lossen, kunt u via **Azure Portal** een nieuwe ondersteuningsaanvraag openen. Dit doet u als volgt: 

![Microsoft-ondersteuning](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid configureert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) (Gids voor de planning en werking van Azure Security Center): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor de overstap naar Azure Security Center.
* [Security health monitoring in Azure Security Center](security-center-monitoring.md) (Beveiligingsstatus controleren in Azure Security Center): meer informatie over het controleren van de status van uw Azure-resources
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure


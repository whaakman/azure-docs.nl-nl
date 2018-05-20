---
title: Doel-Updates met behulp van de SCCM-verzamelingen in Azure Automation - updatebeheer
description: Dit artikel is bedoeld om u te helpen bij het configureren van System Center Configuration Manager met deze oplossing voor het beheren van updates van met SCCM beheerde computers.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3ea95899d48b68c78af5fdc45167b08b5e0fc1ee
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="integrate-system-center-configuration-manager-with-update-management"></a>System Center Configuration Manager integreren met updatebeheer

Klanten die hebben geïnvesteerd in System Center Configuration Manager voor het beheren van pc's, servers en mobiele apparaten, rekenen ook op zijn kracht en volwassenheid bij het beheren van software-updates als onderdeel van hun software-updatebeheercyclus.

Rapporteren en beheerde Windows-servers bijwerken door het maken en vooraf gefaseerde installatie van software-update-implementaties in Configuration Manager en ophalen van gedetailleerde status weergegeven van de voltooide update-implementaties met behulp van de [bijwerken beheeroplossing](automation-update-management.md). Als u Configuration Manager gebruikt voor rapportage voor updatecompatibiliteit maar niet voor het beheren van update-implementaties met uw Windows-servers, kunt u doorgaan naar Configuration Manager melden terwijl beveiligingsupdates worden beheerd met het beheersysteem voor bijwerken.

## <a name="prerequisites"></a>Vereisten

* U moet hebben de [Update beheeroplossing](automation-update-management.md) toegevoegd aan uw Automation-account.
* Windows-servers die worden beheerd door uw System Center Configuration Manager-omgeving moeten ook rapporteren aan de Log Analytics-werkruimte waar ook de Updatebeheer-oplossing is ingeschakeld.
* Deze functie is ingeschakeld in System Center Configuration Manager huidige vertakking versie 1606 of hoger. Als u uw centrale beheersite van Configuration Manager of een zelfstandige primaire site met Log Analytics wilt integreren en verzamelingen importeren, leest u [Connect Configuration Manager to Log Analytics](../log-analytics/log-analytics-sccm.md) (Configuration Manager koppelen aan Log Analytics).  
* Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update als ze geen beveiligingsupdates krijgen van Configuration Manager.   

Hoe u clients die worden gehost in Azure IaaS met uw bestaande Configuration Manager-omgeving beheert, hangt hoofdzakelijk af van de verbinding die u hebt tussen Azure-datacenters en uw infrastructuur. Deze verbinding is van invloed op de ontwerpwijzigingen die u mogelijk moet aanbrengen in uw Configuration Manager-infrastructuur en de daarmee samenhangende kosten voor de ondersteuning van de benodigde wijzigingen. Om te begrijpen welke overwegingen u wilt evalueren voordat u doorgaat, leest u [Configuration Manager on Azure - Frequently Asked Questions](/sccm/core/understand/configuration-manager-on-azure#networking) (veelgestelde vragen).

## <a name="configuration"></a>Configuratie

### <a name="manage-software-updates-from-configuration-manager"></a>Software-updates beheren vanuit Configuration Manager 

Voer de volgende stappen uit als u update-implementaties wilt blijven beheren vanuit Configuration Manager. Azure Automation maakt verbinding naar Configuration Manager updates toepassen op de clientcomputers die zijn verbonden met uw werkruimte voor logboekanalyse. Update-inhoud is beschikbaar via de cache van de clientcomputer alsof de implementatie werd beheerd door Configuration Manager.

1. Maak een software-update-implementatie van de site op het hoogste niveau in uw Configuration Manager-hiërarchie met behulp van het proces dat wordt beschreven in het proces voor het [implementeren van software-updates](/sccm/sum/deploy-use/deploy-software-updates) (Engelstalig artikel). De enige instelling die anders moet worden geconfigureerd dan bij een standaardimplementatie, is de optie **Software-updates niet installeren** om het downloadgedrag van het implementatiepakket te bepalen. Dit gedrag wordt beheerd door het beheersysteem voor bijwerken door het maken van een geplande update-implementatie in de volgende stap.

1. Selecteer in Azure Automation **updatebeheer**. Maak een nieuwe implementatie de stappen in [maken van een Update-implementatie](automation-tutorial-update-management.md#schedule-an-update-deployment) en selecteer **groepen geïmporteerd** op de **Type** vervolgkeuzelijst voor de juiste selecteren Configuration Manager-verzameling. Houd er rekening mee houden van de volgende belangrijke punten: een. Als een onderhoudsvenster is gedefinieerd op de geselecteerde apparaatverzameling van Configuration Manager, leden van de verzameling voldoen aan deze in plaats van de **duur** instelling die is gedefinieerd in de geplande implementatie.
    b. Leden van de doelverzameling moeten verbinding hebben met internet (rechtstreeks, via een proxyserver of via de OMS-gateway).

Na het voltooien van de update-implementatie via Azure Automation wordt de doel-pc die lid van de groep op de geselecteerde computer zijn updates op het geplande tijdstip installeren vanaf de lokale clientcache. U kunt de [status van de update-implementatie bekijken](automation-tutorial-update-management.md#view-results-of-an-update-deployment) om de resultaten van uw implementatie te bewaken.

### <a name="manage-software-updates-from-azure-automation"></a>Software-updates van Azure Automation beheren

Als u updates wilt beheren voor Windows Server-VM's die Configuration Manager-clients zijn, moet u clientbeleid configureren om de Software Update Management-functie uit te schakelen voor alle clients die worden beheerd door deze oplossing. Standaard gelden clientinstellingen voor alle apparaten in de hiërarchie. Raadpleeg voor meer informatie over deze beleidsinstelling en hoe u deze kunt configureren [Het configureren van clientinstellingen in System Center Configuration Manager](/sccm/core/clients/deploy/configure-client-settings).

Na het uitvoeren van deze wijziging in de configuratie die u maakt een nieuwe implementatie de stappen in [maken van een Update-implementatie](automation-tutorial-update-management.md#schedule-an-update-deployment) en selecteer **groepen geïmporteerd** op de **Type** vervolgkeuzelijst om de juiste Configuration Manager-verzameling te selecteren.

## <a name="next-steps"></a>Volgende stappen

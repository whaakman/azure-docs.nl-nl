---
title: Gerichte updates met behulp van SCCM-verzamelingen in OMS-updatebeheer | Microsoft Docs
description: Dit artikel is bedoeld om u te helpen bij het configureren van System Center Configuration Manager met deze oplossing voor het beheren van updates van met SCCM beheerde computers.
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: c3ae8da65e03fe9e11b5657a6a40d02de0567da6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="integrate-system-center-configuration-manager-with-oms-update-management-preview"></a>Integratie van System Center Configuration Manager met OMS-updatebeheer [Preview]

Klanten die hebben geïnvesteerd in System Center Configuration Manager voor het beheren van pc's, servers en mobiele apparaten, rekenen ook op zijn kracht en volwassenheid bij het beheren van software-updates als onderdeel van hun software-updatebeheercyclus.  

Voortbouwend op de bestaande integratie van OMS en Configuration Manager, kunt u rapporten maken over beheerde Windows-servers en deze bijwerken door vooraf software-update-implementaties te maken en klaar te zetten in Configuration Manager, en gedetailleerde statusrapporten van de voltooide update-implementaties krijgen met de [Updatebeheer-oplossing](../operations-management-suite/oms-solution-update-management.md). Als u Configuration Manager gebruikt voor rapportage over updatenaleving, maar niet voor het beheren van update-implementaties met uw Windows-servers, kunt u blijven doorgaan met rapporteren aan Configuration Manager terwijl beveiligingsupdates worden beheerd met de Updatebeheer-oplossing van OMS.

## <a name="prerequisites"></a>Vereisten

* U moet de [Updatebeheer-oplossing](../operations-management-suite/oms-solution-update-management.md) hebben toegevoegd aan uw Log Analytics-werkruimte en hebben gekoppeld met uw Automation-account in dezelfde resourcegroep en regio.   
* Windows-servers die worden beheerd door uw System Center Configuration Manager-omgeving moeten ook rapporteren aan de Log Analytics-werkruimte waar ook de Updatebeheer-oplossing is ingeschakeld.  
* Deze functie ondersteunt System Center Configuration Manager Current Branch versie 1606 en hoger.  Als u uw centrale beheersite van Configuration Manager of een zelfstandige primaire site met Log Analytics wilt integreren en verzamelingen importeren, leest u [Connect Configuration Manager to Log Analytics](../log-analytics/log-analytics-sccm.md) (Configuration Manager koppelen aan Log Analytics).  
* Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update als ze geen beveiligingsupdates krijgen van Configuration Manager.   

Hoe u clients die worden gehost in Azure IaaS met uw bestaande Configuration Manager-omgeving beheert, hangt hoofdzakelijk af van de verbinding die u hebt tussen Azure-datacenters en uw infrastructuur. Deze verbinding is van invloed op de ontwerpwijzigingen die u mogelijk moet aanbrengen in uw Configuration Manager-infrastructuur en de daarmee samenhangende kosten voor de ondersteuning van de benodigde wijzigingen.  Om te begrijpen welke overwegingen u wilt evalueren voordat u doorgaat, leest u [Configuration Manager on Azure - Frequently Asked Questions](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure#networking) (veelgestelde vragen).    

## <a name="configuration"></a>Configuratie

### <a name="manage-software-updates-from-configuration-manager"></a>Software-updates beheren vanuit Configuration Manager 

Voer de volgende stappen uit als u update-implementaties wilt blijven beheren vanuit Configuration Manager.  OMS maakt verbinding met Configuration Manager om updates toe te passen op de clientcomputers die zijn verbonden met uw Log Analytics-werkruimte. Update-inhoud is beschikbaar via de cache van de clientcomputer alsof de implementatie werd beheerd door Configuration Manager.  

1. Maak een software-update-implementatie van de site op het hoogste niveau in uw Configuration Manager-hiërarchie met behulp van het proces dat wordt beschreven in het proces voor het [implementeren van software-updates](https://docs.microsoft.com/en-us/sccm/sum/deploy-use/deploy-software-updates) (Engelstalig artikel).  De enige instelling die anders moet worden geconfigureerd dan bij een standaardimplementatie, is de optie **Software-updates niet installeren** om het downloadgedrag van het implementatiepakket te bepalen. Dit gedrag wordt beheerd door de OMS Updatebeheer-oplossing door in de volgende stap een geplande update-implementatie te maken.  
2. Selecteer in Azure Portal uw Automation-account in het scherm **Automation-account**, en maak een booleaanse variabele met de naam **UseOMSForSCCMUpdates** en de waarde **true** door de aanwijzingen bij [To create a new variable with the Azure portal](../automation/automation-variables.md#to-create-a-new-variable-with-the-azure-portal) (Een nieuwe variabele maken met de Azure-portal) te volgen.
3. Open het dashboard Updatebeheer in de OMS-portal.  Maak een nieuwe implementatie door de stappen te volgen die worden beschreven in [Een update-implementatie maken](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment), en selecteer in de vervolgkeuzelijst de juiste Configuration Manager-verzameling die wordt weergegeven als een OMS-computergroep.  Houd rekening met de volgende belangrijke punten:
    1. Als er een onderhoudsvenster is gedefinieerd op de geselecteerde apparaatverzameling van Configuration Manager, houden leden van de verzameling zich daaraan in plaats van aan de **Duur**-instelling die is gedefinieerd in de geplande implementatie in OMS.
    2. Leden van de doelverzameling moeten verbinding hebben met internet (rechtstreeks, via een proxyserver of via de OMS-gateway).  

Nadat de update-implementatie met de OMS-oplossing is voltooid, worden op de doelcomputers die lid zijn van de geselecteerde computergroep op de geplande tijd updates geïnstalleerd vanuit hun lokale clientcache.  U kunt de [status van de update-implementatie bekijken](../operations-management-suite/oms-solution-update-management.md#viewing-update-deployments) om de resultaten van uw implementatie te bewaken.  

### <a name="manage-software-updates-from-oms"></a>Software-updates beheren vanuit OMS

Als u updates wilt beheren voor Windows Server-VM's die Configuration Manager-clients zijn, moet u clientbeleid configureren om de Software Update Management-functie uit te schakelen voor alle clients die worden beheerd door deze oplossing.  Standaard gelden clientinstellingen voor alle apparaten in de hiërarchie.  Raadpleeg voor meer informatie over deze beleidsinstelling en hoe u deze kunt configureren [Het configureren van clientinstellingen in System Center Configuration Manager](https://docs.microsoft.com/sccm/core/clients/deploy/configure-client-settings).  

Nadat u deze configuratiewijziging hebt uitgevoerd, maakt u een nieuwe implementatie door de stappen te volgen die worden beschreven in [Een update-implementatie maken](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment), en selecteert u in de vervolgkeuzelijst de juiste Configuration Manager-verzameling die wordt weergegeven als een OMS-computergroep. 


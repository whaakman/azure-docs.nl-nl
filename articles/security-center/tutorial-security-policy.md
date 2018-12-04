---
title: Bewerken van beveiligingsbeleid in Azure Policy | Microsoft Docs
description: Beveiligingsbeleid in Azure-beleid bewerken.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: rkarlin
ms.openlocfilehash: d6cc216f71efcd3b3973cd37349dd5145237f02f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839327"
---
# <a name="edit-security-policies-in-azure-policy"></a>Beveiligingsbeleid in Azure-beleid bewerken
Security Center helpt u de status van het beveiligingsbeleid en hoe ze worden toegepast op uw workloads. Azure Security Center wordt automatisch toegewezen de [ingebouwde beveiligingsbeleid](security-center-policy-definitions.md) voor elk abonnement dat is geïmplementeerd. U kunt configureren dat ze in [Azure Policy](../azure-policy/azure-policy-introduction.md), of met behulp van de REST-API, die ook u kunt voor het instellen van beleidsregels binnen beheergroepen en voor meerdere abonnementen. Zie [Integratie van Security Center-beveiligingsbeleid met Azure Policy](security-center-azure-policy.md) voor meer informatie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beveiligingsbeleid op basis van de REST-API configureren
> * De beveiliging van uw resources beoordelen

Als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Om de functies in deze zelfstudie te doorlopen, moet u zich in de Standard-prijscategorie van Security Center bevinden. U kunt Security Center Standard zonder kosten. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie. In de snelstartgids [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Uw Azure-abonnement registreren voor Security Center Standard) wordt u begeleid bij het upgraden naar Standard.

## <a name="configure-a-security-policy-using-the-rest-api"></a>Een beveiligingsbeleid op basis van de REST-API configureren

Als onderdeel van de ingebouwde integratie met Azure Policy kunt Azure Security Center u voordeel Azure Policy REST API voor het maken van de toewijzingen van beleid. De volgende instructies helpen u bij het maken van de toewijzingen van beleid, evenals de aanpassing van de bestaande toewijzingen. 

Belangrijke concepten in Azure Policy: 

- Een **beleidsdefinitie** is een regel 

- Een **initiatief** is een verzameling beleidsdefinities (regels) 

- Een **toewijzing** is een toepassing van een initiatief of een beleid voor een bepaald bereik (beheergroep, abonnement, enz.) 

Security Center heeft een ingebouwde initiatief dat alle van de beleidsregels voor veiligheid bevat. Als u wilt beoordelen van Security Center-beleid op uw Azure-resources, moet u een toewijzing maken in de beheergroep of het abonnement dat u wilt evalueren.  

De ingebouwde initiatief heeft alle beleidsregels van Security Center standaard ingeschakeld. U kunt kiezen om uit te schakelen van bepaald beleid van de ingebouwde initiatief, zo kunt u alle beleidsregels, met uitzondering van Security Center **web application firewall**, door de waarde van parameter van het effect van het beleid te wijzigen **Uitgeschakelde**. 

### <a name="api-examples"></a>API-voorbeelden

Vervang deze variabelen in de volgende voorbeelden:

- **{bereik}**  Voer de naam van de beheergroep of abonnement u het beleid toepast.
- **{poicyAssignmentName}**  Voer de [naam van de relevante beleidstoewijzing](#policy-names).
- **{name}**  Voer uw naam of de naam van de beheerder die de beleidswijziging goedgekeurd.

In dit voorbeeld laat zien hoe de ingebouwde Security Center-initiatief op een abonnement of beheergroep toewijzen
 
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 

In dit voorbeeld laat zien hoe de ingebouwde Security Center-initiatief op een abonnement met de volgende beleidsregels uitgeschakeld toewijzen: 

- Systeemupdates ("systemUpdatesMonitoringEffect") 

- Beveiligingsconfiguraties ("systemConfigurationsMonitoringEffect") 

- Endpoint protection ("endpointProtectionMonitoringEffect") 

 
      PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

      Request Body (JSON) 

      { 

        "properties":{ 

      "displayName":"Enable Monitoring in Azure Security Center", 

      "metadata":{ 

      "assignedBy":"{Name}" 

      }, 

      "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

      "parameters":{ 

      "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 

      "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 

      "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 

      }, 

       } 

      } 

Dit voorbeeld ziet u hoe u een toewijzing verwijderen:

    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 


## Naslaginformatie over de namen van beleid <a name="policy-names"></a>

|Naam van beleid in Security Center|Naam van het beleid weergegeven in Azure Policy |Beleidsnaam effect parameter|
|----|----|----|
|SQL-versleuteling |Niet-versleutelde SQL-database in Azure Security Center bewaken |sqlEncryptionMonitoringEffect| 
|Controleren voor SQL |Niet-gecontroleerde SQL-database in Azure Security Center bewaken |sqlAuditingMonitoringEffect|
|Systeemupdates |Ontbrekende systeemupdates in Azure Security Center bewaken |systemUpdatesMonitoringEffect|
|Storage-versleuteling |Controle uitvoeren op ontbrekende blobversleuteling voor opslagaccounts |storageEncryptionMonitoringEffect|
|Toegang tot het JIT-netwerk |Mogelijk alleen In tijd (JIT)-toegang in Azure Security Center bewaken |jitNetworkAccessMonitoringEffect |
|Adaptieve toepassingsbesturingselementen |Mogelijke opname Apps op Whitelist in Azure Security Center bewaken |adaptiveApplicationControlsMonitoringEffect|
|Netwerkbeveiligingsgroepen |Ruime netwerktoegang in Azure Security Center bewaken |networkSecurityGroupsMonitoringEffect| 
|Beveiligingsconfiguraties |Beveiligingsproblemen van besturingssystemen bewaken in Azure Security Center |systemConfigurationsMonitoringEffect| 
|Eindpuntbeveiliging |Ontbrekende Endpoint Protection bewaken in Azure Security Center |endpointProtectionMonitoringEffect |
|Schijfversleuteling |Niet-versleutelde VM-schijven in Azure Security Center bewaken |diskEncryptionMonitoringEffect|
|Evaluatie van beveiligingsproblemen |Beveiligingsproblemen van de monitor voor virtuele machines in Azure Security Center |vulnerabilityAssesmentMonitoringEffect|
|Web Application Firewall |Niet-beveiligde web-App in Azure Security Center bewaken |webApplicationFirewallMonitoringEffect |
|Next Generation Firewall |Netwerkeindpunten in Azure Security Center bewaken| |





## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u beveiligingsbeleid in Azure Policy bewerkt. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Tenantbrede zichtbaarheid verkrijgen voor Azure Security Center](security-center-management-groups.md): informatie over het instellen van beheergroepen voor Azure Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

Zie [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md) voor meer informatie over Azure Policy.

---
title: Werken met beleidsregels voor veiligheid | Microsoft Docs
description: Dit artikel wordt beschreven hoe u aan het beveiligingsbeleid in Azure Security Center werken.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: monhaber
ms.openlocfilehash: 1931026869e930caef2ff2f92fb85dade15a9c8c
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578438"
---
# <a name="working-with-security-policies"></a>Werken met beveiligingsbeleid

In dit artikel wordt uitgelegd hoe beleidsregels voor veiligheid zijn geconfigureerd en hoe ze weergeven in Security Center. Azure Security Center wordt automatisch toegewezen de [ingebouwde beveiligingsbeleid](security-center-policy-definitions.md) voor elk abonnement dat is geïmplementeerd. U kunt configureren dat ze in [Azure Policy](../governance/policy/overview.md), die u kunt ook beleidsregels binnen beheergroepen en voor meerdere abonnementen.

Zie voor instructies over het instellen van beleidsregels met behulp van PowerShell [Quick Start: Een beleidstoewijzing maken om te identificeren van niet-compatibele resources met behulp van de Azure PowerShell-module](../governance/policy/assign-policy-powershell.md).

>[!NOTE]
> Security Center aan de slag van de integratie met Azure Policy. Bestaande klanten wordt automatisch worden gemigreerd naar het nieuwe ingebouwde initiatief in Azure Policy, in plaats van het vorige beveiligingsbeleid in Security Center. Deze wijziging wordt geen invloed op uw resources of de omgeving, met uitzondering van de aanwezigheid van de nieuwe initiatief in Azure Policy.

## <a name="what-are-security-policies"></a>Wat is beveiligingsbeleid?
Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan. In Azure Policy, kunt u beleidsregels definiëren voor uw Azure-abonnementen en past u dit aan uw type workload of de vertrouwelijkheid van uw gegevens. Toepassingen met gereglementeerde gegevens, zoals persoonsgegevens, kunnen bijvoorbeeld vereisen dat een hoger beveiligingsniveau dan andere werkbelastingen. Als u wilt een beleid instellen voor abonnementen of beheergroepen, stelt u deze [Azure Policy](../governance/policy/overview.md).

Uw beveiligingsbeleid de aanbevelingen voor beveiliging die in Azure Security Center u krijgt het station. Naleving van deze kunt u mogelijke beveiligingsproblemen te identificeren en bedreigingen te verhelpen, kunt u controleren. Zie voor meer informatie over hoe om te bepalen welke optie geschikt is voor u is, de lijst met [ingebouwde beveiligingsbeleid](security-center-policy-definitions.md).

Wanneer u Security Center inschakelt, wordt het beveiligingsbeleid ingebouwd in Security Center in Azure Policy weergegeven als een ingebouwde initiatief onder de categorie Security Center. Het ingebouwde initiatief wordt automatisch toegewezen aan alle abonnementen die bij Security Center zijn geregistreerd (gratis of Standard-categorie). Het ingebouwde initiatief bevat alleen controlebeleid.


### <a name="management-groups"></a>Beheergroepen
Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Azure Beheergroepen biedt een scopeniveau boven abonnementen. U organiseert abonnementen in containers, zogenaamde 'beheergroepen', en past uw governance-beleid op de beheergroepen toe. Alle abonnementen in een beheergroep nemen automatisch het beleid over dat op de beheergroep is toegepast. Elke directory krijgt één beheergroep op het hoogste niveau, de 'hoofdbeheergroep'. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. Met deze hoofdbeheergroep kunt u algemene beleidsregels en RBAC-toewijzingen op directoryniveau toepassen. Als u beheergroepen voor gebruik met Azure Security Center instelt, volg de instructies in [tenant-brede inzicht voor Azure Security Center](security-center-management-groups.md).

> [!NOTE]
> Het is belangrijk dat u de hiërarchie van beheergroepen en abonnementen begrijpt. Zie [Organize your resources with Azure Management Groups](../governance/management-groups/index.md#root-management-group-for-each-directory) (Resources organiseren met Azure Beheergroepen) voor meer informatie over beheergroepen, hoofdbeheer en toegang tot beheergroepen.
>

## <a name="how-security-policies-work"></a>Hoe beveiligingsbeleid werkt
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid in Azure-beleid voor de volgende handelingen kunt bewerken:
- Nieuwe beleidsdefinities maken.
- Beleid toepassen binnen beheergroepen en abonnementen; deze kunnen de hele organisatie of een zakelijke eenheid binnen de organisatie vertegenwoordigen.
- Beleidsnaleving bewaken.

Lees [Beleid maken en beheren om naleving af te dwingen](../governance/policy/tutorials/create-and-manage.md) voor meer informatie over Azure Policy.

De Azure-beleid bestaat uit de volgende onderdelen:

- Een **beleid** is een regel.
- Een **initiatief** is een verzameling van het beleid.
- Een **toewijzing** is de toepassing van een initiatief of een beleid voor een bepaald bereik (beheergroep, abonnement of resourcegroep).

## <a name="view-security-policies"></a>Beveiligingsbeleid bekijken

Ga als volgt te werk als u uw beveiligingsbeleidsregels wilt weergeven in Security Center:

1. In de **Security Center** dashboard, selecteer **beveiligingsbeleid**.

    ![Het deelvenster Beleidsbeheer](./media/security-center-policies/security-center-policy-mgt.png)

   In de **beleidsbeheer** scherm ziet u het aantal beheergroepen, abonnementen en werkruimten, evenals de structuur van uw management groep.

   > [!NOTE]
   > - Het dashboard van Security Center kan een hoger aantal abonnementen onder tonen **abonnementsdekking** dan het aantal abonnementen die wordt weergegeven onder **beleidsbeheer**. Abonnementsdekking toont het aantal Standaard-, gratis en 'Niet gedekt' abonnementen. De abonnementen 'wordt niet gedekt' geen Security Center is ingeschakeld en worden niet weergegeven onder **beleidsbeheer**.
   >

   De kolommen in de tabel bevat de volgende gegevens:

   - **Toewijzing beleidsinitiatieven** – Security Center [ingebouwde beleidsregels](security-center-policy-definitions.md) en initiatieven die zijn toegewezen aan een groep abonnement of de beheergroep.
   - **Dekking** – identificeert de prijscategorie gratis of Standard, dat de beheergroep, het abonnement of de werkruimte op wordt uitgevoerd.  Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
   - **Instellingen voor** – abonnementen hebben de koppeling **instellingen bewerken**. Selecteren **instellingen bewerken** kunt u werken uw [Security Center-instellingen](security-center-policies-overview.md) voor elke groep abonnement of de beheergroep.
   - **Beveiligde score** : de [beveiligde score](security-center-secure-score.md) biedt een zekere mate van hoe de beveiliging van uw werkbelasting beveiligingsstrategie en helpt u bij het prioriteren van aanbevelingen voor verbetering.

2. Selecteer de groep abonnement of de beheergroep waarvan beleidsregels die u wilt weergeven.

   - De **beveiligingsbeleid** scherm weerspiegelt de actie op die door de beleidsregels die zijn toegewezen op de geselecteerde groep van abonnement of de beheergroep.
   - Gebruik de onderstaande koppelingen voor het openen van elk beleid aan de bovenkant **toewijzing** die van toepassing is op de groep abonnement of de beheergroep. Gebruik de koppelingen kunt u toegang tot de toewijzing en bewerken of uitschakelen van het beleid. Als u ziet dat de toewijzing van een bepaald beleid effectief eindpuntbeveiliging weigert, kunt u de koppeling bijvoorbeeld gebruiken voor toegang tot het beleid en bewerken of uitschakelen.
   - In de lijst met beleidsregels ziet u de effectieve toepassing van het beleid op uw abonnement of beheergroep. Dit betekent dat de instellingen die betrekking hebben op het bereik van elk beleid in aanmerking worden genomen en u u met de cumulatieve resultaten vindt van welke actie wordt ondernomen door het beleid. Bijvoorbeeld, als in één toewijzing het beleid is uitgeschakeld, maar in een andere dat is ingesteld op AuditIfNotExist, klikt u vervolgens het cumulatieve effect is van toepassing AuditIfNotExist. Het meer actieve effect heeft altijd voorrang.
   - Het beleid voor effect kan zijn: Toevoegen, controleren, AuditIfNotExists, weigeren, DeployIfNotExists, uitgeschakeld. Zie voor meer informatie over hoe effecten worden toegepast, [beleid begrijpen effecten](../governance/policy/concepts/effects.md).

   ![beleid scherm](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Wanneer u weergeven aan beleidsregels toegewezen, kunt u meerdere toewijzingen bekijken en kunt u zien hoe elke toewijzing is geconfigureerd op een eigen.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen en beheergroepen in bewerken [Azure Policy](../governance/policy/tutorials/create-and-manage.md). U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep.

Zie voor instructies over het bewerken van een beveiligingsbeleid in Azure Policy en [maken en beheren van beleidsregels voor het afdwingen van naleving](../governance/policy/tutorials/create-and-manage.md).

U kunt beveiligingsbeleid via de portal van Azure Policy, via de REST-API of met behulp van Windows PowerShell bewerken. Het volgende voorbeeld vindt u instructies voor het bewerken met behulp van REST-API.


## <a name="disable-security-policies"></a>Beveiligingsbeleidsregels voor uitschakelen
Als het standaardbeveiligingsbeleid een aanbeveling die niet relevant is voor uw omgeving genereert, kunt u deze kunt stoppen door het uitschakelen van de beleidsdefinitie waarmee de aanbeveling wordt verzonden.
Zie voor meer informatie over aanbevelingen [aanbevelingen voor beveiliging beheren](security-center-recommendations.md).

1. In het Beveiligingscentrum uit de **beleid en naleving** sectie, klikt u op **beveiligingsbeleid**.

   ![beleidsbeheer](./media/tutorial-security-policy/policy-management.png)

2. Klik op het abonnement of beheergroep groep waarvoor u wilt uitschakelen van de aanbeveling.

1. Klik op het toegewezen beleid.

   ![Beleid uitschakelen](./media/tutorial-security-policy/security-policy.png)

1. In de **PARAMETERS** sectie zoeken voor het beleid dat roept de bevelen aan dat u wilt uitschakelen, en in de vervolgkeuzelijst, selecteer **uitgeschakeld**

   ![Beleid uitschakelen](./media/tutorial-security-policy/disable-policy.png)
1. Klik op **Opslaan**.
   > [!Note]
   > Wijzigingen in het beleid uitschakelen kunnen pas van kracht tot 12 uur duren.


### <a name="configure-a-security-policy-using-the-rest-api"></a>Een beveiligingsbeleid op basis van de REST-API configureren

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
- **{policyAssignmentName}**  Voer de [naam van de relevante beleidstoewijzing](#policy-names).
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
    
    Hoofdtekst van de aanvraag (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName": "Controleren inschakelen in Azure Security Center", 
    
    "metagegevens": { 
    
    "assignedBy": "{Name} 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect": {"waarde": "Uitgeschakeld"}, 
    
    "endpointProtectionMonitoringEffect": {"waarde": "Uitgeschakeld"}, 
    
    }, 
    
     } 
    
    } 

Dit voorbeeld ziet u hoe u een toewijzing verwijderen:

    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 


### Naslaginformatie over de namen van beleid <a name="policy-names"></a>

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
|Evaluatie van beveiligingsproblemen |Beveiligingsproblemen van virtuele machines bewaken in Azure Security Center |vulnerabilityAssessmentMonitoringEffect|
|Web Application Firewall |Niet-beveiligde web-App in Azure Security Center bewaken |webApplicationFirewallMonitoringEffect |
|Next Generation Firewall |Netwerkeindpunten in Azure Security Center bewaken| |


### <a name="who-can-edit-security-policies"></a>Wie kan beveiligingsbeleid bewerken?
Security Center maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC), waarmee u ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot ze toegang tot hebben resources. Wat betekent dat gebruikers de rol van eigenaar, bijdrager of lezer zijn toegewezen aan de groep abonnement of resourcegroep waartoe een resource behoort. Naast deze rollen zijn er twee specifieke Security Center-rollen:

- Beveiligingslezer: Heeft weergaverechten voor Security Center, waaronder aanbevelingen, waarschuwingen, beleid en de gezondheid van, maar ze kunnen geen wijzigingen aanbrengen.
- Beveiligingsbeheerder: De dezelfde rechten weergeven als beveiligingslezer hebben, en ze kunnen ook bijwerken van het beveiligingsbeleid en aanbevelingen en waarschuwingen sluiten.



## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u beveiligingsbeleid in Azure Policy bewerkt. Zie de volgende artikelen voor meer informatie over Security Center:

* [Azure Security Center planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md): Leer hoe u Azure Security Center de ontwerpoverwegingen kennen en plan.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): Meer informatie over het controleren van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md): Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Tenant-brede inzicht voor Azure Security Center](security-center-management-groups.md): Meer informatie over het instellen van beheergroepen voor Azure Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): Krijg antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): Raadpleeg de blogberichten over beveiliging en naleving in Azure.

Zie [Wat is Azure Policy?](../governance/policy/overview.md) voor meer informatie over Azure Policy.

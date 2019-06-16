---
title: Platform-as-a-Service (PaaS)-services gebruiken in Azure DevTest Labs | Microsoft Docs
description: Informatie over het gebruik van services voor Platform-as-a-Service (Pass) in Azure DevTest Labs.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: 865ae0b3f7a7965698a67183a4c820ba71f49cd8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833916"
---
# <a name="use-platform-as-a-service-paas-services-in-azure-devtest-labs"></a>Platform-as-a-Service (PaaS)-services gebruiken in Azure DevTest Labs
PaaS wordt ondersteund in DevTest Labs via de functie omgevingen. Omgevingen in DevTest Labs worden ondersteund door de vooraf geconfigureerde Azure Resource Manager-sjablonen in een Git-opslagplaats. Omgevingen kunnen zowel PaaS en IaaS-resources bevatten. Hiermee kunt u maken van complexe systemen die Azure-resources zoals virtuele machines, databases, virtuele netwerken kunnen opnemen en Web-apps die zijn aangepast om samen te werken. Deze sjablonen kunnen consistente implementatie en verbeterd beheer van omgevingen met broncodebeheer. 

Een goed stelt een systeem kan de volgende scenario's: 

- Ontwikkelaars hebben onafhankelijke en meerdere omgevingen
- Testen op verschillende configuraties asynchroon
- Integratie in zonder de Sjabloonwijzigingen pijplijnen voor fasering en productie
- Met zowel ontwikkeling machines en omgevingen binnen de dezelfde lab verbetert beheergemak en rapportage van kosten.  

De resourceprovider DevTest Labs maakt resources uit de naam van de gebruiker van het lab, dus er zijn geen extra machtigingen moeten worden opgegeven voor de gebruiker lab gebruik van de PaaS-resources inschakelen. De volgende afbeelding ziet u een Service Fabric-cluster als een omgeving in het lab.

![Service Fabric-cluster als een omgeving](./media/create-environment-service-fabric-cluster/cluster-created.png)

Voor Zie voor meer informatie over het instellen van omgevingen, [multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md). DevTest Labs is een openbare opslagplaats met Azure Resource Manager-sjablonen die u gebruiken kunt om omgevingen te maken zonder verbinding maken met een externe bron van GitHub zelf. Zie voor meer informatie over openbare omgevingen [configureren en het gebruik openbare omgevingen in Azure DevTest Labs](devtest-lab-configure-use-public-environments.md).

In grote organisaties ontwikkelteams meestal aangeboden aan omgevingen zoals testomgevingen aangepast/geïsoleerd. Mogelijk zijn er omgevingen die moeten worden gebruikt door alle teams binnen een bedrijfseenheid of een divisie. De IT-afdeling kan wilt bieden hun omgevingen die kunnen worden gebruikt door alle teams in de organisatie.  

## <a name="customizations"></a>Aanpassingen

#### <a name="sandbox"></a>Sandbox 
De eigenaar van het testlab kunt aanpassen testomgevingen als u wilt wijzigen van de gebruikersrol van **lezer** naar **Inzender** binnen de resourcegroep. Deze mogelijkheid is in de **Lab instellingen** pagina onder de **configuratie en het beleid** van het testlab. Deze wijziging in de rol kan de gebruiker toevoegen of verwijderen van resources in die omgeving. Als u wilt de toegang verder te beperken, gebruikt u Azure-beleid. Deze functie kunt u de resources of configuratie zonder de toegang op abonnementsniveau aanpassen.

#### <a name="custom-tokens"></a>Aangepaste tokens
Er is een aangepast lab-informatie die buiten de resourcegroep en is specifiek voor omgevingen die toegang hebben tot de sjabloon. Hier volgen enkele van deze: 

- Lab netwerk-id
- Locatie
- Storage-account waarin de Resource Manager-sjablonen-bestanden worden opgeslagen. 
 
#### <a name="lab-virtual-network"></a>Virtueel netwerk van de testomgeving
De [omgevingen verbinden met virtueel netwerk van het lab](connect-environment-lab-virtual-network.md) artikel wordt beschreven hoe u uw Resource Manager-sjabloon wijzigt de `$(LabSubnetId)` token. Als u een omgeving maakt, de `$(LabSubnetId)` token wordt vervangen door het eerste subnet is ingeschakeld wanneer de **gebruiken in de virtuele machine maken** optie is ingesteld op **waar**. Hiermee kunnen onze omgeving voor netwerken eerder hebt gemaakt. Als u de dezelfde Resource Manager-sjablonen gebruiken in omgevingen in de test als fasering en productie wilt, gebruikt u `$(LabSubnetId)` als een standaardwaarde is opgegeven in de parameter voor een Resource Manager-sjabloon. 

#### <a name="environment-storage-account"></a>Omgeving Storage-Account
DevTest Labs ondersteunt het gebruik van [geneste Resource Manager-sjablonen](../azure-resource-manager/resource-group-linked-templates.md). De [[geneste Azure Resource Manager-sjablonen voor testomgevingen implementeren](deploy-nested-template-environments.md) artikel wordt uitgelegd hoe u `_artifactsLocation` en `_artifactsLocationSasToken` tokens voor het maken van een URI naar een Resource Manager-sjabloon in dezelfde map als of in een geneste map van de belangrijkste sjabloon. Zie voor meer informatie over deze twee tokens, de **implementatie artefacten** sectie van [Azure Resource Manager-handleiding met aanbevolen procedures](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md).

## <a name="user-experience"></a>Gebruikerservaring

## <a name="developer"></a>Developer
Ontwikkelaars voor het gebruik van dezelfde werkstroom voor het maken van een virtuele machine te maken van een specifieke omgeving. Ze selecteert u de omgeving en de installatiekopie van de machine en voer de vereiste informatie in de sjabloon. Iedere ontwikkelaar met een omgeving kunt voor de implementatie van wijzigingen en verbeterde binnenste lus foutopsporing. De omgeving kan op elk gewenst moment met behulp van de meest recente sjabloon worden gemaakt.  Deze functie kunt de omgevingen worden vernietigd en opnieuw worden gemaakt om te reduceren de downtime opheft handmatig maken van het systeem of het herstellen van het testen van fouten.  

### <a name="testing"></a>Testen
DevTest Labs-omgevingen kunnen onafhankelijke testen van specifieke code en configuraties asynchroon. De algemene procedure is het instellen van de omgeving met de code van de afzonderlijke pull-aanvraag en start een geautomatiseerd testen. Nadat de geautomatiseerde tests voltooid is uitgevoerd, kan handmatig testen worden uitgevoerd voor de specifieke omgeving. Dit proces wordt doorgaans uitgevoerd als onderdeel van de CI/CD-pijplijn. 

## <a name="management-experience"></a>Beheerervaring

### <a name="cost-tracking"></a>Kosten bijhouden
De kosten-functie voor documenttracking bevat Azure-resources binnen de verschillende omgevingen als onderdeel van de algemene kostentrend. De kosten door resources niet verbreken van de verschillende bronnen binnen de omgeving, maar wordt de omgeving weergegeven als een eenmalige kosten.

### <a name="security"></a>Beveiliging
Een correct geconfigureerde Azure-abonnement met DevTest Labs kunt [beperkt de toegang tot Azure-resources alleen via het lab](devtest-lab-add-devtest-user.md). Met omgevingen kunt de eigenaar van een lab gebruikers toegang krijgen tot PaaS-resources met goedgekeurde configuraties zonder te verlenen van toegang tot alle andere Azure-resources. In het scenario waar labgebruikers omgevingen aanpassen, kunt de eigenaar van het lab Inzender-toegang. De Inzender-toegang kan de lab-gebruiker toevoegen of verwijderen van Azure-resource binnen de beheerde resourcegroep. Hierdoor kan gemakkelijker bijhouden en versus kan de gebruiker Inzender-toegang tot het abonnement.

### <a name="automation"></a>Automation
Automation is een belangrijk onderdeel voor een grote schaal, effectieve-ecosysteem. Automation is nodig om te beheren of bijhouden van meerdere omgevingen voor abonnementen en labs handelen.

### <a name="cicd-pipeline"></a>CI/CD-pijplijn
PaaS-services in DevTest Labs kunt verbeteren van de CI/CD-pijplijn door implementaties waarbij toegang wordt bepaald door het lab hebben gericht.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over omgevingen: 

- 
- [Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md)
- [Configuratie en het gebruik van openbare omgevingen in Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
- [Maken van een omgeving met zelfstandige Service Fabric-cluster in Azure DevTest Labs](create-environment-service-fabric-cluster.md)
- [Verbinding maken met een omgeving met virtuele netwerk van uw lab in Azure DevTest Labs](connect-environment-lab-virtual-network.md)
- [Omgevingen integreren in uw Azure DevOps-CI/CD-pijplijnen](integrate-environments-devops-pipeline.md)
 






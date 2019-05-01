---
title: Beveiligingsoverwegingen voor Azure Container Instances
description: Aanbevelingen voor het beveiligen van installatiekopieën en geheimen voor Azure Container Instances en algemene beveiligingsoverwegingen voor elk containerplatform
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/29/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: dc516277d79e37500e73e1aee6b88c908acb9b1c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943994"
---
# <a name="security-considerations-for-azure-container-instances"></a>Beveiligingsoverwegingen voor Azure Container Instances

Dit artikel bevat de beveiligingsoverwegingen voor het gebruik van Azure Container Instances om uit te voeren van container-apps. Dit zijn een aantal van de onderwerpen:

> [!div class="checklist"]
> * **Aanbevelingen voor beveiliging** voor het beheren van installatiekopieën en geheimen voor Azure Container Instances
> * **Overwegingen voor de container-ecosysteem** gedurende de levenscyclus van de container, voor elk containerplatform

## <a name="security-recommendations-for-azure-container-instances"></a>Aanbevelingen voor beveiliging voor Azure Container Instances

### <a name="use-a-private-registry"></a>Een persoonlijk register gebruikt

Containers worden gemaakt van installatiekopieën die zijn opgeslagen in een of meer opslagplaatsen. Deze opslagplaatsen kunnen deel uitmaken van een openbaar register, zoals [Docker Hub](https://hub.docker.com), of naar een persoonlijk register. Een voorbeeld van een persoonlijk register is de [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.0/), dat lokaal kan worden geïnstalleerd of in een virtuele privécloud. U kunt ook cloudgebaseerde persoonlijke containerregisterservices, waaronder [Azure Container Registry](../container-registry/container-registry-intro.md). 

Een openbaar beschikbare containerinstallatiekopie geen veiligheid worden gegarandeerd. Containerinstallatiekopieën bestaan uit meerdere softwarelagen en elke softwarelaag kan beveiligingsproblemen bevatten. Om te helpen de bedreigingen van aanvallen verkleinen, moet u opslaan en ophalen van afbeeldingen uit een privéregister zoals Azure Container Registry of Docker Trusted Registry. Naast het bieden van een beheerd persoonlijk register, Azure Container Registry ondersteunt [service-principal-gebaseerde authenticatie](../container-registry/container-registry-authentication.md) via Azure Active Directory voor basisverificatiestromen. Deze verificatie is inclusief toegang op basis van rollen voor alleen-lezen (pull), schrijven (push) en eigenaarsmachtigingen.

### <a name="monitor-and-scan-container-images"></a>Bewaken en scannen van installatiekopieën van containers

Beveiligingsbewaking en scannen oplossingen zoals [Twistlock](https://azuremarketplace.microsoft.com/marketplace/apps/twistlock.twistlock?tab=Overview) en [Aqua Security](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) zijn beschikbaar via de Azure Marketplace. U kunt deze gebruiken om te scannen van installatiekopieën van containers in een persoonlijk register en mogelijke beveiligingsproblemen te identificeren. Het is belangrijk om te weten welk scanniveau dat de verschillende oplossingen bieden. 

### <a name="protect-credentials"></a>Referenties beveiligen

Containers kunnen verdeeld over verschillende clusters en Azure-regio's. Daarom moet u referenties die nodig zijn voor aanmeldingen of API-toegang, zoals wachtwoorden of tokens beveiligen. Zorg ervoor dat alleen gebruikers met bevoegdheden toegang deze containers in-transit en in rust tot. Inventarisatie van alle geheimen van de referentie en vervolgens vereist ontwikkelaars gebruikmaken van opkomende geheimen-management-hulpprogramma's die zijn ontworpen voor containerplatforms.  Zorg ervoor dat uw oplossing versleutelde databases, TLS-versleuteling van data-geheimen in de doorvoer en minimale bevoegdheden bevat [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md). [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md) is een cloudservice die beveiligt uw versleutelingssleutels en geheimen (zoals certificaten, verbindingsreeksen en wachtwoorden) voor toepassingen in containers. Omdat deze gegevens vertrouwelijk en bedrijfskritiek, kluizen veilige toegang tot uw sleutel zodat alleen gemachtigde toepassingen en gebruikers toegang hebben tot deze.

## <a name="considerations-for-the-container-ecosystem"></a>Overwegingen voor de container-ecosysteem

De volgende beveiligingsmaatregelen ook geïmplementeerd en beheerd effectief, kunt u beveiligen en beschermen van uw container-ecosysteem. Deze maatregelen toepassing gedurende de levenscyclus van de container, van ontwikkeling tot productie-implementatie en een scala aan containerorchestrators, hosts en -platforms. 

### <a name="use-vulnerability-management-as-part-of-your-container-development-lifecycle"></a>Beveiligingsproblemen gebruiken als onderdeel van de levenscyclus van de ontwikkeling van container 

Met behulp van effectieve beveiligingsproblemen gedurende de ontwikkelfase container, moet u de kans dat u identificeren en oplossen van beveiligingsproblemen voordat ze een ernstig probleem verbeteren. 

### <a name="scan-for-vulnerabilities"></a>Scannen op beveiligingsproblemen 

Nieuwe beveiligingsproblemen, voortdurend worden ontdekt, zodat het scannen en het identificeren van beveiligingsproblemen een continu proces is. Gedurende de levenscyclus van de container scannen van beveiligingsproblemen opnemen:

* Als een laatste controle in de ontwikkelingspijplijn, moet u een scan door een beveiligingslek in containers uitvoeren voordat u de installatiekopieën naar een openbare of persoonlijke register pusht. 
* Doorgaan met het scannen van containerinstallatiekopieën in het register, zowel voor het identificeren van eventuele fouten op overgeslagen tijdens de ontwikkeling en om alle nieuwe gedetecteerde zwakke plekken die in de code die wordt gebruikt in de containerinstallatiekopieën voorkomen kunnen.  

### <a name="map-image-vulnerabilities-to-running-containers"></a>Kaart installatiekopie beveiligingslekken aan containers uitvoeren 

U moet beschikken over een middelen van zwakke plekken van de toewijzing die we in containerinstallatiekopieën tot het uitvoeren van containers, zodat u beveiligingsproblemen met zich mee kunnen worden verholpen of opgelost.  

### <a name="ensure-that-only-approved-images-are-used-in-your-environment"></a>Zorg ervoor dat alleen goedgekeurde installatiekopieën worden gebruikt in uw omgeving 

Er is onvoldoende wijzigings- en volatiliteit in een container ecosysteem zonder toe te staan ook onbekende containers. Toestaan dat alleen goedgekeurde containerinstallatiekopieën. Hulpprogramma's en processen om te controleren en te voorkomen dat het gebruik van niet-goedgekeurde containerinstallatiekopieën hebben. 

Er is een effectieve manier om de kwetsbaarheid voor aanvallen verminderen en zo wordt voorkomen dat ontwikkelaars waardoor essentiële fouten voor het beheren van de stroom van containerinstallatiekopieën in uw ontwikkelomgeving. Bijvoorbeeld, u hebt een enkel Linux-distributie als basisinstallatiekopie, bij voorkeur een lean mogelijk goedgekeurd (Alpine of CoreOS in plaats van Ubuntu), om te beperken het oppervlak voor potentiële aanvallen. 

Installatiekopie-ondertekening of fingerprinting krijgt u een bewakingsketen waarmee u om te controleren of de integriteit van de containers. Azure Container Registry ondersteunt bijvoorbeeld van Docker [inhoud vertrouwensrelatie](https://docs.docker.com/engine/security/trust/content_trust) model, waardoor uitgevers van installatiekopieën aan te melden installatiekopieën die naar een register worden gepusht en consumenten van de afbeelding voor het ophalen van alleen afbeeldingen aangemeld.

### <a name="permit-only-approved-registries"></a>Toestaan dat alleen goedgekeurde registers 

Er is een extensie om ervoor te zorgen dat alleen goedgekeurde installatiekopieën maakt gebruik van uw omgeving om toe te staan alleen het gebruik van goedgekeurde containerregisters. Het gebruik van goedgekeurde containerregisters vereisen, vermindert de blootstelling aan risico's door het beperken van de mogelijkheden voor de introductie van onbekende beveiligingsproblemen of beveiligingsproblemen met zich mee. 

### <a name="ensure-the-integrity-of-images-throughout-the-lifecycle"></a>Zorg ervoor dat de integriteit van installatiekopieën gedurende de levenscyclus 

Onderdeel van het beheer van beveiliging gedurende de levenscyclus van de container is om te controleren of de integriteit van de containerinstallatiekopieën in het register en wanneer ze worden gewijzigd of geïmplementeerd in productie. 

* Installatiekopieën met beveiligingsproblemen, zelfs kleine, moeten niet worden toegestaan om uit te voeren in een productieomgeving. In het ideale geval moeten alle installatiekopieën die zijn geïmplementeerd in een productieomgeving worden opgeslagen in een persoonlijk register toegankelijk is voor een paar. Houd het aantal productie-installatiekopieën kleine om ervoor te zorgen dat ze effectief kunnen worden beheerd.

* Omdat het is moeilijk te achterhalen van de oorsprong van software van een openbaar beschikbare containerinstallatiekopie, maken van installatiekopieën van de bron om te controleren of de kennis van de oorsprong van de laag. Wanneer er een beveiligingsprobleem aan het licht komt in een zelf gebouwde containerinstallatiekopie, kunnen klanten sneller een oplossing vinden. Met een openbare installatiekopie moeten klanten zouden vinden van de hoofdmap van een openbare installatiekopie te corrigeren of een andere veilige installatiekopie verkrijgen via de uitgever. 

* Een grondig gescande installatiekopie geïmplementeerd in een productieomgeving is niet noodzakelijkerwijs worden bijgewerkt voor de levensduur van de toepassing. Er kunnen beveiligingsproblemen worden gerapporteerd voor lagen van de installatiekopie die niet bekend waren of die na de implementatie naar productie zijn geïntroduceerd. 

  Controleer regelmatig de installatiekopieën die zijn geïmplementeerd in een productieomgeving om installatiekopieën die verouderd zijn of die niet zijn bijgewerkt sinds langere tijd te identificeren. U kunt blue-green implementatie methoden en de rolling upgrade-mechanismen gebruiken om bij te werken van containerinstallatiekopieën zonder uitvaltijd. U kunt installatiekopieën scannen met behulp van hulpprogramma's beschreven in de vorige sectie. 

* Een pijplijn voor continue integratie (CI) met geïntegreerde beveiligingsscans voor het bouwen van beveiligde installatiekopieën en push ze naar uw privéregister gebruiken. De ingebouwde scanfunctie voor beveiligingsproblemen van de CI-oplossing zorgt ervoor dat installatiekopieën die voor alle tests slagen, naar het persoonlijke register worden gepusht van waaruit productieworkloads worden geïmplementeerd. 

  Een mislukte CI-pijplijn zorgt ervoor dat kwetsbare installatiekopieën niet worden gepusht naar het persoonlijke register dat wordt gebruikt voor workloadimplementaties voor productie. Het automatiseert de beveiliging van installatiekopieën scannen als er een groot aantal installatiekopieën. Het handmatig controleren van installatiekopieën op beveiligingsproblemen kan erg langdurig en foutgevoelig zijn. 

### <a name="enforce-least-privileges-in-runtime"></a>Minimaal benodigde bevoegdheden in runtime afdwingen 

Het concept van minimale bevoegdheden is een aanbevolen procedure van basisvereisten voor beveiliging die ook van toepassing op containers. Wanneer een beveiligingsprobleem wordt misbruikt, in het algemeen geeft de aanvaller toegang en bevoegdheden gelijk is aan die van de toepassing waarmee is geknoeid of het proces. Ervoor zorgen dat containers worden uitgevoerd met de minimale bevoegdheden en toegang is vereist voor het ophalen van het werk gedaan te krijgen de blootstelling aan risico's vermindert. 

### <a name="reduce-the-container-attack-surface-by-removing-unneeded-privileges"></a>De container kwetsbaarheid voor aanvallen verminderen door het verwijderen van onnodige bevoegdheden 

U kunt ook de mogelijkheid op aanvallen minimaliseren door het verwijderen van ongebruikte processen of bevoegdheden van de container-runtime. Bevoegde containers worden uitgevoerd als de hoofdmap. Als een kwaadwillende gebruiker of een werkbelasting verlaat u in een container met bevoegdheden, wordt de container vervolgens uitgevoerd als root op dat systeem.

### <a name="whitelist-files-and-executables-that-the-container-is-allowed-to-access-or-run"></a>Goedgekeurde bestanden en uitvoerbare bestanden die de container is toegestaan voor toegang tot of uitvoeren 

Het aantal variabelen of onbekenden te verminderen, kunt u een stabiele, betrouwbare omgeving onderhouden. Containers, zodat ze toegang hebben tot of uitvoeren alleen goedgekeurd of in de whitelist opgenomen bestanden en uitvoerbare bestanden te beperken, is een bewezen methode blootstelling aan het risico te beperken.  

Het is veel eenvoudiger is voor het beheren van een lijst met toegestane wanneer deze geïmplementeerd vanaf het begin. Een lijst met toegestane biedt een zekere mate van controle en beheer als u meer te welke bestanden en uitvoerbare bestanden vereist voor de toepassing weten zijn te laten functioneren. 

Een lijst met toegestane niet alleen vermindert de kwetsbaarheid voor aanvallen, maar ook bieden een basislijn voor afwijkingen en te voorkomen dat de use cases van het 'noisy neighbor' en container-groepen-scenario's. 

### <a name="enforce-network-segmentation-on-running-containers"></a>Afdwingen van netwerksegmentering over het uitvoeren van containers  

Ter bescherming van containers in een subnet van beveiligingsrisico's in een ander subnet, onderhouden netwerksegmentering (of nano-segmentatie) of de scheiding tussen het uitvoeren van containers. Beheren van netwerksegmentatie mogelijk ook die nodig zijn voor het gebruik van containers in branches die nodig zijn om te voldoen aan nalevingsmandaten.  

Bijvoorbeeld, het hulpprogramma partner [Aqua](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) biedt een geautomatiseerde benadering voor nano-Segmentatie. Zeeblauw bewaakt netwerkactiviteiten container tijdens runtime. Hiermee geeft u alle inkomende en uitgaande verbindingen naar/van andere containers, services, IP-adressen en het openbare internet. Nano-segmentatie wordt automatisch gemaakt op basis van de bewaakte verkeer. 

### <a name="monitor-container-activity-and-user-access"></a>Toegang tot de activiteit en de gebruiker van de container bewaken 

Net als bij elke IT-omgeving, moet u consistent activiteit en de gebruikerstoegang bewaken voor uw container-ecosysteem snel identificeren van verdachte of schadelijke activiteiten. Azure biedt container monitoring-oplossingen, waaronder:

* [Azure Monitor voor containers](../azure-monitor/insights/container-insights-overview.md) voor het bewaken van de prestaties van uw workloads in Kubernetes-omgevingen die worden gehost in Azure Kubernetes Service (AKS) geïmplementeerd. Azure Monitor voor containers biedt u de zichtbaarheid van de prestaties door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn. 

* De [Azure Container Monitoring solution](../azure-monitor/insights/containers.md) kunt u weergeven en beheren van andere Docker en Windows containerhosts op één locatie. Bijvoorbeeld:

  * Bekijk gedetailleerde controle-informatie die opdrachten die worden gebruikt met containers bevat. 
  * Problemen met containers weer te geven en gecentraliseerde logboeken zoeken zonder dat om Docker of Windows-hosts op afstand weer te geven.  
  * Containers zoeken die mogelijk veel ruis veroorzaken en in beslag nemen veel bronnen op een host.
  * Gecentraliseerde CPU, geheugen, opslag en gebruiks- en netwerkgegevens voor containers weergeven.  

  De oplossing biedt ondersteuning voor Docker Swarm, DC/OS, niet-beheerde Kubernetes, Service Fabric en Red Hat OpenShift container-orchestrators. 

### <a name="monitor-container-resource-activity"></a>Resourceactiviteit voor de container bewaken 

Uw resource-activiteiten, zoals bestanden, netwerk en andere resources die uw containers toegang hebben tot bewaken. Resourceactiviteit voor en het verbruik-controle is handig voor het bewaken van prestaties, en ook als veiligheidsmaatregel. 

[Azure Monitor](../azure-monitor/overview.md) kunt kernbewaking voor Azure-services doordat het verzamelen van metrische gegevens, activiteitenlogboeken en diagnostische logboeken. Het activiteitenlogboek geeft bijvoorbeeld aan wanneer nieuwe resources worden gemaakt of gewijzigd. 

Er zijn metrische gegevens beschikbaar met prestatiestatistieken voor verschillende resources en zelfs het besturingssysteem binnen een virtuele machine. U kunt deze gegevens bekijken met een van de verkenners in de Azure-portal en waarschuwingen maken op basis van deze metrische gegevens. Azure Monitor biedt dat de snelste metrische gegevens pipeline (5 minuten tot 1 minuut), zodat u deze voor tijdgebonden waarschuwingen en meldingen gebruiken moet. 

### <a name="log-all-container-administrative-user-access-for-auditing"></a>Meld u alle gebruiker met beheerdersrechten toegang tot de container voor controle 

Onderhouden een nauwkeurige audittrail van beheerderstoegang tot uw container-ecosysteem, container registry en containerinstallatiekopieën. Deze logboeken kunnen het nodig zijn voor controledoeleinden zijn en zijn handig als forensische bewijs na een beveiligingsincident. U kunt de [Azure Container Monitoring solution](../azure-monitor/insights/containers.md) voor dit doel. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het beheren van container zwakke plekken met oplossingen van [Twistlock](https://www.twistlock.com/solutions/microsoft-azure-container-security/) en [Aqua Security](https://www.aquasec.com/solutions/azure-container-security/).

* Meer informatie over [beveiliging van de container in Azure](https://azure.microsoft.com/resources/container-security-in-microsoft-azure/).
---
title: Beveiligen van uw computers en toepassingen in Azure Security Center | Microsoft Docs
description: Dit document bevat aanbevelingen in Security Center die u helpen beveiligen van uw virtuele machines en computers en uw web-apps en App Service-omgevingen.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2019
ms.author: monhaber
ms.openlocfilehash: 8dcaa9b98292e66d81daf3d115159b0c0c1124af
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56106717"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Beveiligen van uw computers en toepassingen in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen. Aanbevelingen zijn van toepassing op Azure-resource-typen: virtuele machines (VM's) en computers, toepassingen, netwerken, SQL, en de identiteit en toegang.

In dit artikel biedt aanbevelingen die betrekking hebben op de machines en toepassingen.

## <a name="monitoring-security-health"></a>Beveiligingsstatus bewaken
U kunt de beveiligingsstatus van uw resources bewaken op de **Security Center-overzicht** dashboard. De **Resources** sectie bevat het aantal problemen geïdentificeerd en de beveiligingsstatus voor elk resourcetype.

U kunt een lijst van alle problemen weergeven door te selecteren **aanbevelingen**. Zie voor meer informatie over het toepassen van aanbevelingen [beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md).

Zie voor een volledige lijst van reken- en App-services aanbevelingen, [aanbevelingen](security-center-virtual-machine-recommendations.md).

Als u wilt doorgaan, selecteer **Compute en apps** onder **Resources** of het hoofdmenu van Security Center.
![Security Center-dashboard](./media/security-center-virtual-machine-recommendations/overview.png)

## <a name="monitor-compute-and-app-services"></a>Reken- en app-services bewaken
Onder **Compute en apps**, er zijn de volgende tabbladen:

- **Overzicht**: bewaking en aanbevelingen door Security Center.
- **VM’s en computers**: lijst met alle virtuele machines, computers en de bijbehorende actuele beveiligingsstatussen.
- **Cloud Services**: lijst van al uw web- en werkrollen die worden bewaakt door Security Center.
- **App-services**: lijst met uw App service-omgevingen en de huidige beveiligingsstatus van elk.
- **Containers (Preview)**: lijst met de containers die worden gehost op IaaS Linux-machines en beoordelen van de beveiliging van de Docker-configuraties.
- **Rekenresources (Preview)**: lijst met aanbevelingen voor uw compute-resources, zoals Service Fabric-clusters en eventhubs.

Als u wilt doorgaan, selecteer **Compute en apps** onder **Resource beveiliging hygiëne**.

![Compute](./media/security-center-virtual-machine-recommendations/compute.png)

Op elk tabblad kunt u meerdere sectie hebben en in elke sectie kunt u een afzonderlijke optie selecteren voor meer informatie over de aanbevolen stappen voor het oplossen van het specifieke probleem.

### Niet-bewaakte VM's en computers <a name="unmonitored-vms-and-computers"></a>
Een virtuele machine of computer wordt niet bewaakt door Security Center als de machine wordt niet uitgevoerd voor de Microsoft Monitoring Agent-extensie. Een computer heeft misschien een lokale agent is al geïnstalleerd, bijvoorbeeld de directe OMS-agent of de SCOM-agent. Machines met deze agents worden geïdentificeerd als niet-bewaakt omdat deze agents niet volledig worden ondersteund in Security Center. De Microsoft Monitoring Agent-extensie is vereist om volledig te profiteren van alle mogelijkheden van Security Center.

U kunt de extensie installeren op de niet-bewaakte virtuele machine of computer naast de reeds geïnstalleerde lokale agent. Configureer beide agents op dezelfde manier en verbind ze met dezelfde werkruimte. Hierdoor kan Security Center communiceren met de Microsoft Monitoring Agent-extensie en gegevens verzamelen. Zie [Enable the VM extension](../azure-monitor/learn/quick-collect-azurevm.md) (De VM-extensie inschakelen) voor installatie-instructies voor de Microsoft Monitoring Agent-extensie.

Zie [Problemen met de agentstatus controleren](security-center-troubleshooting-guide.md#mon-agent) voor meer informatie over de redenen waarom Security Center VM's en computers die zijn geïnitialiseerd voor automatische inrichting niet afdoende kan bewaken.

### <a name="recommendations"></a>Aanbevelingen
Deze sectie bevat een reeks aanbevelingen voor elke virtuele machine en computer, web-en werkrollen, Azure App Service Web Apps en Azure App Service Environment die wordt bewaakt door Security Center. De eerste kolom bevat de aanbeveling. De tweede kolom geeft het totale aantal resources dat is beïnvloed door deze aanbeveling. De derde kolom geeft de ernst van het probleem.

Elke aanbeveling heeft een set acties die u kunt uitvoeren nadat u deze hebt geselecteerd. Als u bijvoorbeeld **ontbrekende systeemupdates**, het aantal virtuele machines en computers waarvoor patches ontbreken, en de ernst van de ontbrekende update weergegeven.

**Systeemupdates toepassen** geeft een samenvatting van essentiële updates in een grafiek, één voor Windows en één voor Linux. Het tweede gedeelte bevat een tabel met de volgende informatie:

- **NAAM**: De naam van de ontbrekende update.
- **AANTAL VM's en COMPUTERS**: Totale aantal virtuele machines en computers die niet over deze update beschikken.
- **ERNST VAN DE UPDATE**: Hierin wordt de ernst van deze bepaalde aanbeveling beschreven:

    - **Kritieke**: Een beveiligingslek in de bestaat een belangrijke resource (toepassing, virtuele machine of netwerkbeveiligingsgroep) en aandacht vereist.
    - **Belangrijke**: Niet-kritieke of extra stappen zijn vereist om een proces te voltooien of een beveiligingsprobleem.
    - **Gemiddeld**: Een beveiligingsprobleem moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Aanbevelingen met de ernstaanduiding Laag worden niet standaard weergegeven, maar u kunt hierop filteren als u deze aanbevelingen wilt bekijken.)


- **STATUS**: De huidige status van de aanbeveling:

    - **Open**: De aanbeveling is nog niet opgelost.
    - **Bezig**: De aanbeveling wordt momenteel toegepast op deze resources, en er is geen actie te ondernemen.
    - **Opgelost**: De aanbeveling is al voltooid. (Als het probleem is opgelost, wordt de vermelding grijs).

Klik op de naam van de ontbrekende update om meer informatie voor de aanbeveling weer te geven.


> [!NOTE]
> Hier de aanbevelingen voor beveiliging zijn hetzelfde als die onder de **aanbevelingen** tegel. Zie [beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md) voor meer informatie over het toepassen van aanbevelingen.
>
>

### <a name="vms-and-computers"></a>VM's en computers
De sectie virtuele machines en computers geeft een overzicht van alle aanbevelingen voor de virtuele machine en computer. Elke kolom vertegenwoordigt een reeks aanbevelingen.

![Aanbevelingen voor de computer en virtuele machine](./media/security-center-virtual-machine-recommendations/vm-computers.png)

Er zijn vier typen pictogrammen weergegeven in deze lijst:

![Niet-Azure-computer](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Computer zonder Azure.

![Virtuele machines van Azure Resource Manager](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Virtuele machines van Azure Resource Manager.

![Klassieke Azure-VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Klassieke Azure-VM.


![Virtuele machines geïdentificeerd vanuit de werkruimte](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) Virtuele machines die alleen zijn geïdentificeerd vanuit de werkruimte die deel uitmaakt van het weergegeven abonnement. Dit omvat VM's van andere abonnementen die rapporteren aan de werkruimte in dit abonnement, en VM’s die zijn geïnstalleerd met de directe SCOM-agent en geen resource-ID hebben.

U kunt met het pictogram dat wordt weergegeven onder elke aanbeveling snel identificeren van de virtuele machine en de computer die aandacht vereisen en wat het type aanbeveling. U kunt ook de filters gebruiken om te zoeken naar de lijst door **resourcetype** en door **ernst**.

Als u wilt inzoomen in de aanbevelingen voor beveiliging voor elke virtuele machine, klik op de virtuele machine.
Hier ziet u de beveiligingsdetails voor de virtuele machine of computer. Onderin ziet u de aanbevolen actie en de ernst van elk probleem.
![Cloudservices](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Cloud services
Voor cloudservices, wordt een aanbeveling gemaakt wanneer de versie van het besturingssysteem verouderd is.

![Cloud services](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

In een scenario waarin u een aanbeveling krijgt (wat is niet het geval is bij het vorige voorbeeld) hebt, moet u de stappen in de aanbeveling voor de versie van het besturingssysteem bij te werken. Wanneer een update beschikbaar is, ontvangt u een waarschuwing (rood of oranje, afhankelijk van de ernst van het probleem). Wanneer u deze waarschuwing selecteert in de WebRole1 (Windows Server wordt uitgevoerd met uw web-app automatisch geïmplementeerd in IIS) of WorkerRole1 (Windows Server wordt uitgevoerd met uw web-app automatisch geïmplementeerd in IIS) rijen, ziet u meer informatie over deze aanbeveling.

Als u een meer beschrijvende uitleg wilt zien over deze aanbeveling, klikt u op **Besturingssysteemversie bijwerken** onder de kolom **BESCHRIJVING**.



![Besturingssysteemversie bijwerken](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png)

### <a name="app-services"></a>App-services
U moet voor het inschakelen van App Service in uw abonnement om de App Service-informatie weergeven. Zie voor instructies over het inschakelen van deze functie [App Service beveiligen met Azure Security Center](security-center-app-services.md).
[!NOTE]
> Bewaking van App Service is in preview en alleen beschikbaar op de prijscategorie Standard van Security Center.


Onder **App services**, vindt u een lijst van uw App service-omgevingen en samenvatting van de status op basis van de evaluatie van Security Center wordt uitgevoerd.

![App-services](./media/security-center-virtual-machine-recommendations/app-services.png)

Er zijn drie soorten pictogrammen weergegeven in deze lijst:

![Omgeving voor App-services](./media/security-center-virtual-machine-recommendations/ase.png) Omgeving van App-services.

![Webtoepassing](./media/security-center-virtual-machine-recommendations/web-app.png) Web-App.

![Functions-toepassing](./media/security-center-virtual-machine-recommendations/function-app.png) Functions-toepassing.

1. Selecteer een web-App. Een overzicht wordt geopend met drie tabbladen:

  - **Aanbevelingen**: op basis van beoordelingen die door Security Center die niet zijn uitgevoerd.
  - **Evaluaties doorgegeven**: lijst met beoordelingen uitgevoerd door Security Center die doorgegeven.
  - **Niet-beschikbare evaluaties**: lijst met beoordelingen die kan niet worden uitgevoerd vanwege een fout of de aanbeveling is niet relevant zijn voor de specifieke appservice

  Onder **aanbevelingen** is een lijst met aanbevelingen voor de geselecteerde web-App en de ernst van elke aanbeveling.

  ![Aanbevelingen van App Services](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

2. Selecteer een aanbeveling om een beschrijving van de aanbeveling en een lijst met resources niet in orde, gezonde resources en niet-gescande resources te bekijken.

 - Onder de **doorgegeven evaluaties** kolom is een lijst van geslaagde evaluaties.  Ernst van deze beoordeling is altijd groen.

 -  Selecteer een evaluatie van de doorgegeven in de lijst voor een beschrijving van de evaluatie, een lijst met resources niet in orde en in orde is en een lijst met niet-gescande resources. Er is een tabblad voor de resources niet in orde, maar deze lijst is altijd leeg omdat de evaluatie van de doorgegeven.

    ![App Service-herstel](./media/security-center-virtual-machine-recommendations/app-service-remediation.png)


## <a name="compute-and-app-recommendations"></a>Reken- en app-aanbevelingen
|Resourcetype|Beveiligingsscore|Aanbeveling|Description|
|----|----|----|----|
|App Service|20|Web-App moet alleen toegankelijk zijn via HTTPS|Alleen beperkt de toegang van webtoepassingen via HTTPS.|
|App Service|20|Functie-App moet alleen toegankelijk zijn via HTTPS|Beperk de toegang van de functie-Apps alleen via HTTPS.|
|App Service|5|Diagnostische logboeken in appservice inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|App Service|10|Foutopsporing op afstand moet worden uitgeschakeld voor Web-App|Als u niet meer nodig hebt om deze te gebruiken voor het Web-foutopsporing uitschakelen. Externe foutopsporing moeten inkomende poorten op een functie-App moeten worden geopend.|
|App Service|10|Foutopsporing op afstand moet worden uitgeschakeld voor Functions-toepassing|Als u niet meer nodig hebt om deze te gebruiken voor functie-App-foutopsporing uitschakelen. Externe foutopsporing moeten inkomende poorten op een functie-App moeten worden geopend.|
|App Service|10|IP-beperkingen voor Web-App configureren|Een lijst met IP-adressen die zijn toegestaan voor toegang tot uw toepassing definiëren. Gebruik van IP-beperkingen beschermt u webtoepassingen tegen algemene aanvallen.|
|App Service|10|Niet alle toestaan ('* ') resources toegang tot uw toepassing| Instellen van de parameter WEBSITE_LOAD_CERTIFICATES niet toestaan ' ". Als u de parameter naar ' betekent dat alle certificaten in de persoonlijke certificaatarchief van uw web-toepassingen zijn geladen. Dit kan leiden tot misbruik van het principe van minimale bevoegdheden als is het onwaarschijnlijk dat de site toegang hebben tot alle certificaten die tijdens runtime moet.|
|App Service|20|CORS kunt u voorkomen dat elke resource toegang tot uw Web-Apps|Toestaan dat alleen vereiste domeinen om te communiceren met uw webtoepassing. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw webtoepassing.|
|App Service|20|CORS kunt u voorkomen dat elke resource toegang tot uw functie-App| Toestaan dat alleen vereiste domeinen om te communiceren met uw functie-toepassing. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw functie-toepassing.|
|Rekenresources (batch)|1|Waarschuwingsregels voor metrische gegevens op Batch-account configureren|Waarschuwingsregels voor metrische gegevens op Batch-account configureren en inschakelen van de metrische gegevens Pool verwijderen voltooid gebeurtenissen en Pool verwijderen starten|
|Rekenresources (service fabric)|10|Azure Active Directory gebruiken voor clientverificatie in Service Fabric|Verificatie van de Client alleen via Azure Active Directory uitvoeren in Service Fabric.|
|Rekenresources (automation-account)|5| Versleuteling van Automation-account inschakelen|Versleuteling van Automation-account-variabele assets inschakelen bij het opslaan van gevoelige gegevens.|
|Rekenresources (Load balancer)|5|Schakel diagnostische logboeken in Load Balancer|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (zoeken)|5|Diagnostische logboeken in Search-service inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (servicebus)|5|Diagnostische logboeken in Service Bus inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (stream analytics)|5|Inschakelen van diagnostische logboeken in Azure Stream Analytics|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (service fabric)|5|Schakel diagnostische logboeken in Service Fabric|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (batch)|5|Schakel diagnostische logboeken in Batch-accounts|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (event hub)|5|Diagnostische logboeken in Event Hub inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (logische apps)|5|Diagnostische logboeken in Logic Apps inschakelen|Logboeken inschakelen en ze maximaal een jaar bewaren. Hiermee kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd. |
|Rekenresources (service fabric)|15|Stel de eigenschap ClusterProtectionLevel op EncryptAndSign in Service Fabric|Service Fabric biedt drie niveaus van beveiliging (geen, aanmelding en EncryptAndSign) voor communicatie van knooppunt-naar-knooppunt met behulp van een primaire clustercertificaat.  Stel het beveiligingsniveau om ervoor te zorgen dat alle berichten van knooppunt-naar-knooppunt zijn versleuteld en digitaal zijn ondertekend. |
|Rekenresources (servicebus)|1|Alle autorisatieregels behalve RootManageSharedAccessKey van Service Bus-naamruimte verwijderen |Service Bus-clients moeten een beleid voor het toegangsniveau van naamruimte die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt niet gebruiken. Om uit te lijnen met minimale bevoegdheden beveiligingsmodel, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Rekenresources (event hub)|1|Alle autorisatieregels behalve RootManageSharedAccessKey uit de Event Hub-naamruimte verwijderen |Event Hub-clients moeten een beleid voor het toegangsniveau van naamruimte die toegang tot alle wachtrijen en onderwerpen in een naamruimte biedt niet gebruiken. Om uit te lijnen met minimale bevoegdheden beveiligingsmodel, moet u toegangsbeleid maken op het entiteitsniveau van de voor wachtrijen en onderwerpen voor toegang tot alleen de specifieke entiteit.|
|Rekenresources (event hub)|5|Autorisatieregels definiëren voor de Event Hub-entiteit|Controle van autorisatieregels voor de Event Hub-entiteit minste bevoegdheden toegang te verlenen.|
|Machine|50|Bewakingsagent op uw machines installeren|Installeer de agent bewaking voor het verzamelen van gegevens, updates scannen, basislijnen te scannen en endpoint protection op elke machine inschakelen.|
|Machine|50|Automatische inrichting en verzamelen van gegevens voor uw abonnementen inschakelen |Automatische inrichting en verzamelen van gegevens voor computers in uw abonnementen om gegevens te verzamelen, updates scannen, basislijnen te scannen en endpoint protection op elke machine die is toegevoegd aan uw abonnementen inschakelen.|
|Machine|40|Statusproblemen van bewakingsagenten oplossen op uw machines|Voor volledige beveiliging van Security Center, oplossen van problemen met de bewaking agent op uw virtuele machines door de instructies in de gids voor probleemoplossing| 
|Machine|40|Statusproblemen met eindpuntbescherming oplossen voor uw machines|Voor volledige Security Center beveiliging oplossen monitoring agent op uw virtuele machines door de instructies in de gids voor probleemoplossing.|
|Machine|40|Problemen oplossen met ontbrekende scangegevens op uw machines|Problemen met ontbrekende Scangegevens op virtuele machines en computers. Ontbrekende Scangegevens op de resultaten van uw machines beveiligingscontroles zoals ontbrekende update scannen, basislijn scannen en ontbrekende endpoint protection solution scannen.|
|Machine|40|Systeemupdates installeren op uw machines|Ontbrekende systeembeveiliging en essentiële updates voor het beveiligen van uw Windows- en Linux-machines en computers installeren
|Machine|15|Een firewall voor webtoepassingen toevoegen| Een web application firewall (WAF)-oplossing voor het beveiligen van uw webtoepassingen implementeren. |
|Machine|40|Besturingssysteemversie voor rollen van uw cloudservice bijwerken|De versie van besturingssysteem (OS) voor rollen van uw cloudservice worden bijgewerkt naar de meest recente versie die beschikbaar is voor uw type besturingssysteem.|
|Machine|35|Problemen met de beveiligingsconfiguratie van virtuele machines verhelpen|Beveiligingsproblemen in de configuratie van de beveiliging op uw virtuele machines te beschermen tegen aanvallen herstellen. |
|Machine|35|Beveiligingsproblemen in de configuratie van de beveiliging van uw containers herstellen|Beveiligingsproblemen verhelpen in de beveiligingsconfiguratie van computers waarop Docker is geïnstalleerd, om beveiliging tegen aanvallen te bieden.|
|Machine|25|Besturingselementen voor adaptieve toepassingen inschakelen|Toepassing voor welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure inschakelen. Hiermee kunt u beveiliging van uw virtuele machines tegen schadelijke software. Security Center maakt gebruik van machine learning voor het analyseren van de toepassingen die worden uitgevoerd op elke virtuele machine en helpt die u toepassen met behulp van deze informatie regels toestaan. Deze mogelijkheid vereenvoudigt het proces van het configureren en onderhouden van de toepassing regels voor toestaan.|
|Machine|20|Oplossing voor eindpuntbeveiliging installeren op uw machines|Installeer een oplossing voor eindpuntbeveiliging op uw virtuele machines, Beveilig ze tegen bedreigingen en zwakke plekken.|
|Machine|20|Start uw machines opnieuw op om systeemupdates toe te passen|Start uw machines opnieuw op om de systeemupdates toe te passen en de machine vanuit beveiligingsproblemen te beveiligen.|
|Machine|15|Schijfversleuteling toepassen op uw virtuele machines|Versleutel de schijven van uw virtuele machines met Azure Disk Encryption zowel voor Windows en Linux-machines. Azure Disk Encryption (ADE) maakt gebruik van de branche standaard BitLocker-functie van Windows en de DM-Crypt-functie van Linux voor besturingssysteem en schijfversleuteling om u te helpen te beschermen en uw gegevens veilig houden en te voldoen aan uw organisatie beveiliging en naleving verplichtingen klant Azure sleutelkluis. Wanneer uw behoeften nalevings- en moet u de end-to-end met behulp van uw versleutelingssleutels, met inbegrip van versleuteling van de kortstondige schijf in de (lokaal gekoppelde tijdelijke), gebruikt Azure disk encryption-gegevens versleutelen. U kunt ook standaard zijn beheerde schijven at-rest standaard versleuteld met behulp van Azure Storage-Serviceversleuteling waar de versleutelingssleutels beheerd door Microsoft-sleutels in Azure worden. Als deze voldoet aan uw nalevings- en beveiligingsvereisten, kunt u gebruikmaken van de standaard beheerde schijf-versleuteling om te voldoen aan uw vereisten.|
|Machine|30|Een evaluatie van beveiligingsproblemen op uw virtuele machines installeren|Een evaluatie van beveiligingsproblemen op uw virtuele machines installeren|
|Machine|15|Een firewall voor webtoepassingen toevoegen| Een web application firewall (WAF)-oplossing voor het beveiligen van uw webtoepassingen implementeren. |
|Machine|30|Beveiligingsproblemen met behulp van een oplossing voor evaluatie van beveiligingsproblemen herstellen|Virtuele machines waarvoor een 3e partij oplossing voor beveiligingslekken die is geïmplementeerd worden continu wordt geëvalueerd op basis van de toepassing en beveiligingsproblemen van besturingssystemen. Wanneer u dergelijke problemen worden gevonden, is deze zijn beschikbaar voor meer informatie als onderdeel van de aanbeveling.|
|Machine|30|Een evaluatie van beveiligingsproblemen op uw virtuele machines installeren|Een evaluatie van beveiligingsproblemen op uw virtuele machines installeren|
|Machine|1|Virtuele machines migreren naar nieuwe Azure Resource Manager-resources|Gebruik Azure Resource Manager voor uw virtuele machines voor verbeterde beveiliging, zoals: sterkere toegangsbeheer (RBAC), betere controle, op basis van Resource Manager-implementatie en beheer, de toegang tot beheerde identiteiten, toegang tot key vault voor geheimen, Azure AD gebaseerde verificatie en ondersteuning voor labels en resourcegroepen voor eenvoudiger beveiligingsbeheer. |
|Machine|30|Beveiligingsproblemen met behulp van een oplossing voor evaluatie van beveiligingsproblemen herstellen|Virtuele machines waarvoor een 3e partij oplossing voor beveiligingslekken die is geïmplementeerd worden continu wordt geëvalueerd op basis van de toepassing en beveiligingsproblemen van besturingssystemen. Wanneer u dergelijke problemen worden gevonden, is deze zijn beschikbaar voor meer informatie als onderdeel van de aanbeveling.|

 





## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:


* [Inzicht in Azure Security Center aanbevelingen voor virtuele machines](security-center-virtual-machine-recommendations.md)
* [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.


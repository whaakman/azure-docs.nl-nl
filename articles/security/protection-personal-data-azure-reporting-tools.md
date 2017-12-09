---
title: Beveiliging van persoonlijke gegevens met Azure-hulpprogramma's voor rapportage-document | Microsoft Docs
description: "het gebruik van Azure reporting services en -technologieën om de privacy van persoonlijke gegevens te beschermen."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 7ce6297d7d6b61ac95df58db3fa1a2a0a123a64e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Beveiliging voor documenten van persoonlijke gegevens met Azure-hulpprogramma's voor rapportage

In dit artikel wordt besproken hoe u Azure reporting services en -technologieën om de privacy van persoonlijke gegevens te beschermen.

## <a name="scenario"></a>Scenario

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse, Adriatische, Baltische veiligheid ook Florida. Om te helpen deze inspanningen, heeft die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk

Het bedrijf gebruikmaakt van Microsoft Azure voor verwerking en opslag van bedrijfsgegevens. Dit omvat persoonlijk gegevens zoals namen, adressen, telefoonnummers en creditcardgegevens van globale klantendatabase. Dit omvat ook traditionele Human Resources informatie zoals adressen, telefoonnummers, BTW-id's en andere informatie over de werknemers van het bedrijf op alle locaties. De regel cruise onderhoudt ook een grote database van derden en loyaliteit programma leden met persoonlijke gegevens voor de relaties met de huidige en eerdere klanten bijhouden.

Zakelijke werknemers toegang tot het netwerk van het bedrijf externe kantoren en reizen agents over de hele wereld hebben toegang tot een aantal bedrijfsresources.

## <a name="problem-statement"></a>Probleemformulering

Het bedrijf moet de privacy van klanten en werknemers beveiligen en functies voor het opleggen strenge controles op de verwerking van persoonlijke gegevens en toegang tot persoonlijke gegevens via een meerlaagse beveiligingsstrategie die gebruikmaakt van Azure-beheer en beveiliging en moet kunnen de beschermende maatregelen auditors van interne en externe aantonen.

## <a name="company-goal"></a>Bedrijf-doel

Als onderdeel van de beveiligingsstrategie van defense-in-depth is een doel van het bedrijf alle toegang tot en verwerking van persoonlijke gegevens bijhouden en ervoor zorgen dat de documentatie van de juiste privacy-bescherming voor persoonlijke gegevens zijn ingesteld en functioneert.

## <a name="solutions"></a>Oplossingen

Microsoft Azure biedt uitgebreide bewaking, logboekregistratie en diagnostische hulpprogramma's voor het bijhouden en registreren activiteiten en gebeurtenissen die zijn gekoppeld aan de toegang tot en het verwerken van persoonlijke gegevens, geografische stroom van gegevens en derden toegang tot persoonlijke gegevens. Omdat de beveiliging van persoonlijke gegevens in de cloud is een gedeelde verantwoordelijkheid, biedt Microsoft ook klanten:

- Gedetailleerde informatie over een eigen verwerken van gegevens van klanten

- Veiligheidsmaatregelen beheerd door Microsoft

- Waar en hoe het versturen van gegevens van klanten

- Details van de Microsoft privacy beoordelingen proces

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) van Microsoft cloud-gebaseerde, multitenant directory en identity management-service is. De service aanmelden en audit rapportagemogelijkheden bieden u gedetailleerde aanmelden en gebruik activiteit toepassingsinformatie om te controleren en zorg ervoor dat de juiste toegang tot persoonlijke gegevens van de werknemers en klanten.

Er zijn twee soorten activiteitsrapporten:

- De [audit activiteitenlogboeken rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) bieden gedetailleerde gegevens over systeemtaken activiteiten

- De [aanmeldingen rapport/activiteitenlogboek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) toont die elke activiteit die wordt vermeld in het auditrapport heeft uitgevoerd

U kunt de geschiedenis van elke taak uit te voeren en door wie elk uitgevoerd met behulp van de twee samen, bijhouden. Beide typen rapporten kunnen worden aangepast en kunnen worden gefilterd.

#### <a name="how-do-i-access-the-audit-and-security-logs"></a>Hoe krijg ik toegang tot de logboeken voor controle en beveiliging?

De logboeken voor controle en beveiliging toegankelijk zijn vanaf de Active Directory-portal op drie verschillende manieren: via de **activiteit** sectie (Selecteer **controlelogboeken** of **aanmeldingen**), of van **gebruikers en groepen** of **bedrijfstoepassingen** onder **beheren** in Active Directory. Rapporten zijn ook toegankelijk via de Azure Active Directory rapportage-API. 

1. Selecteer in de Azure-portal **Azure Active Directory.**

2. In de **activiteit** sectie **controlelogboeken.**

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. De lijstweergave aanpassen door te klikken op **kolommen** op de werkbalk.

4.  Selecteer een item in de lijstweergave voor een overzicht van alle beschikbare details over deze.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Rapportage van Azure Active Directory bevat ook twee soorten beveiligingsrapporten, **gebruikers die zijn gemarkeerd voor risico** en **riskant aanmeldingen**, waarmee u mogelijke risico's in uw Azure-omgeving bewaken.

Zie voor meer informatie over de rapportageservice [rapportage van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)

Ga naar [Azure Active Directory-activiteitenrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) voor meer details over de rapporten die beschikbaar zijn in Azure Active Directory. Deze site bevat meer informatie over het openen en gebruiken [auditlogboeken activiteitsrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) en [aanmeldingsactiviteiten rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) in de portal. Dit omvat ook informatie over [gebruikers die zijn gemarkeerd voor risico](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) en [riskant aanmelden](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) beveiligingsrapporten.

Ga naar de [Azure Active Directory-audit API-referentiemateriaal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) site voor meer informatie over verbinding maken met Azure Directory-rapportage voor via een programma.

### <a name="log-analytics"></a>Log Analytics

[Meld u Analytics](https://azure.microsoft.com/services/log-analytics/) kunt [verzamelen van gegevens van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) om het correleren met andere gegevens en bieden aanvullende analyse. Azure Monitor worden verzameld en analyseert bewakingsgegevens voor uw Azure-omgeving. 

Analyseprogramma's in logboekanalyse zoals logboek zoekopdrachten, weergaven en oplossingen werken tegen alle verzamelde gegevens, zonder dat u gecentraliseerde analyse van uw hele omgeving. Log Analytics kunt aggregeren en analyseren van Windows-gebeurtenislogboeken, IIS-logboeken en Syslogs waarmee detecteren van mogelijke overtredingen van persoonlijke gegevens die persoonlijke gegevens voor onbevoegde gebruikers worden namelijk blootgesteld.

#### <a name="how-do-i-use-log-analytics"></a>Hoe gebruik ik Log Analytics

Log Analytics is toegankelijk via de OMS-portal of de Azure-portal, via elke webbrowser. Log Analytics ondersteunt een querytaal voor het snel ophalen en samenvoegen van gegevens in de opslagplaats. U kunt maken en opslaan van logboekbestanden zoekopdrachten rechtstreeks om gegevens te analyseren in de portal.

Voor het maken van een werkruimte voor logboekanalyse in de Azure portal, het volgende doen:

1. Selecteer **logboekanalyse** uit de lijst met services in de Marketplace.

2. Selecteer **maken,** Geef de naam van de OMS-werkruimte, selecteert u uw abonnement, resourcegroep, locatie en prijscategorie.

3. Klik op **OK** om een lijst met uw werkruimten weer te geven.

4. Selecteer een werkruimte om de details te bekijken.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

Ga naar de [Log Analytics-documentatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) voor meer informatie over de service.

Ga naar de [aan de slag met een werkruimte voor logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) zelfstudie voor het maken van een werkruimte evaluatie en leer de basisbeginselen van het gebruik van de service.

Ga naar de volgende webpagina's voor meer informatie over het verbinding maken met Log Analytics gebruiken met de logboeken die hierboven worden beschreven:

[Gegevensbronnen van de Windows-gebeurtenislogboeken in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[IIS-logboeken in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[Syslog-gegevensbronnen in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Azure Monitor/Azure Activity Log 

[Monitor voor Azure](https://azure.microsoft.com/services/monitor/) basisniveau infrastructuur metrische gegevens en logboeken voor de meeste services in Microsoft Azure biedt.
Controle, kunt u krijgen grondige inzicht in uw Azure-toepassingen. Azure Monitor afhankelijk van de Azure extensie voor diagnostische gegevens (Windows of Linux) om de meeste niveau metrische gegevens voor een toepassing en logboeken te verzamelen. [De Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) is een van de resources die u met Azure-bewaking kunt weergeven. Het houdt elke API-aanroep, en biedt een schat aan informatie over activiteiten die voorkomen in [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
U kunt het activiteitenlogboek (eerder operationele of controlelogboeken genoemd) voor meer informatie zoeken over uw resource, zoals weergegeven door de Azure-infrastructuur. 

Hoewel veel van de informatie die is vastgelegd in het logboek voor de activiteit heeft betrekking op prestaties en servicestatus, is er ook informatie met betrekking tot beveiliging van gegevens. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op de resources in uw Azure-abonnement.

Het biedt bijvoorbeeld een record wanneer een beheerder verwijdert een netwerkbeveiligingsgroep dit kan invloed hebben op de beveiliging van persoonlijke gegevens. Logboekvermeldingen activiteit worden opgeslagen in Azure Monitor gedurende 90 dagen.

#### <a name="how-do-i-use-the-data-collected-by-azure-monitor"></a>Hoe gebruik ik de gegevens die worden verzameld door Azure-Monitor

Er zijn een aantal manieren voor gebruik van de gegevens in het activiteitenlogboek en andere bronnen van de Azure-Monitor.

- U kunt de gegevens naar andere locaties in de echte regel streamen.

- U kunt de gegevens opslaan voor langere tijd dan de standaardinstellingen, met behulp van een [Azure storage-account](https://docs.microsoft.com/azure/storage/common/storage-introduction) en een bewaarbeleid instellen.

- U kunt de gegevens in de afbeeldingen en grafieken, visual met behulp van de [Azure-portal](https://azure.microsoft.com/features/azure-portal/), [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft PowerBI](https://powerbi.microsoft.com/), of hulpprogramma's van derden visualisatie.

- U kunt de gegevens met behulp van de Azure Monitor REST API, CLI-opdrachten query [PowerShell](https://docs.microsoft.com/powershell/) cmdlets, of de .NET SDK.

Selecteer aan de slag met Azure Monitor **meer Services** in de Azure portal.

1. Schuif omlaag naar **Monitor** in de **controleren en beheren van** sectie.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  Monitor wordt geopend in de **activiteitenlogboek** weergeven.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

U kunt query's voor algemene filters maken en opslaan en de belangrijkste query's vervolgens vastmaken aan een portal dashboard, zodat u altijd op de hoogte bent als er zich gebeurtenissen voordoen die voldoen aan uw criteria.

1. U kunt de weergave door de resourcegroep, timespan en gebeurteniscategorie filteren.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Vervolgens kunt u query's naar een portal dashboard vastmaken door te klikken op de **pincode** knop. Zo kunt u één bron van informatie voor de operationele gegevens op uw services maken. De querynaam en het aantal resultaten wordt weergegeven op het dashboard.

U kunt ook de Monitor metrische gegevens voor alle Azure-resources weergeven, het configureren van instellingen voor diagnostische gegevens en waarschuwingen en het zoeken van het logboek. Zie voor meer informatie over het gebruik van de Monitor van Azure en het activiteitenlogboek [aan de slag met Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Azure Diagnostics 

De mogelijkheden voor diagnostische gegevens in Azure kunt verzamelen van gegevens van diverse bronnen. De Windows-gebeurtenislogboeken, waaronder het beveiligingslogboek is vooral handig bij het bijhouden en documenteren van de beveiliging van persoonlijke gegevens. Het beveiligingslogboek houdt aanmelding geslaagde en mislukte gebeurtenissen, evenals wijzigingen in machtigingen, detectie van patronen die wijzen op bepaalde typen aanvallen, wijzigingen in de beleidsregels die betrekking hebben op beveiliging, beveiligingswijzigingen voor een groepslidmaatschap en nog veel meer.

Bijvoorbeeld, gebeurtenis-ID 4695 wordt u gewezen op de poging tot unprotection van controleerbaar beveiligde gegevens. Dit heeft betrekking op de Data Protection API (DPAPI), waarmee u helpt bij de bescherming van gegevens, zoals persoonlijke sleutels, opgeslagen referenties en andere vertrouwelijke informatie.

#### <a name="how-do-i-enable-the-diagnostics-extension-for-windows-vms"></a>Hoe kan ik de extensie voor diagnostische gegevens inschakelen voor Windows-VM's?

U kunt PowerShell gebruiken voor het inschakelen van de extensie voor diagnostische gegevens voor een Windows-VM om logboekgegevens verzamelen. De stappen voor het in dat geval afhankelijk zijn van op welk implementatiemodel u (Resource Manager of klassiek) gebruiken. Als u wilt dat de extensie voor diagnostische gegevens op een bestaande virtuele machine die is gemaakt via het implementatiemodel van Resource Manager, kunt u de [Set AzureRMVMDiagnosticsExtension PowerShell-cmdlet](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1).

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$diagnosticsconfig_path* is het pad naar het bestand met de configuratie van de diagnostische gegevens in XML. Zie voor meer informatie over het inschakelen van Azure Diagnostics op een virtuele machine, [Gebruik PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows.](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics)

De extensie Azure diagnostics kan de verzamelde gegevens overdragen naar Azure storage-account of verzenden deze services, zoals Application Insights. U kunt de gegevens vervolgens gebruiken voor controle.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Hoe ik opslaan en weergeven van diagnostische gegevens?

Het is belangrijk om te onthouden diagnostische gegevens is niet permanent opgeslagen tenzij u deze naar de Microsoft Azure-opslagemulator of naar Azure storage overdragen. Om op te slaan en diagnostische gegevens weergeven in Azure Storage als volgt te werk:

1. Geef een opslagaccount in het bestand ServiceConfiguration.cscfg. Azure Diagnostics kan de Blob-service of de tabel-service, afhankelijk van het type gegevens gebruiken. Windows-gebeurtenislogboeken worden opgeslagen in tabelindeling.

2. De gegevens overbrengen. U kunt aanvragen om over te dragen van de diagnostische gegevens via het configuratiebestand. Voor SDK-2.4 en vorige, kunt u de aanvraag via een programma.

3. Gegevens van de weergeven met behulp van [Azure Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), [Server Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) in Visual Studio of [Azure Diagnostics Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) in Azure Management Studio.

Zie voor meer informatie over het uitvoeren van deze stappen [diagnostische gegevens opslaan en weergeven in Azure Storage.](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)

### <a name="azure-storage-analytics"></a>Azure Storage Analytics 

Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen die helpen bij het vastleggen van toegang tot persoonlijke gegevens die zijn opgeslagen in de service. Echter is Storage Analytics logboekregistratie niet standaard ingeschakeld voor uw opslagaccount. U kunt ze inschakelen in de Azure-portal.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>Hoe configureer bewaking voor een opslagaccount?

Als u wilt bewaken configureren voor een opslagaccount, het volgende doen:

1. Selecteer **opslagaccounts** in de Azure portal, selecteer vervolgens de naam van het account dat u wilt bewaken.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. In de **bewaking** sectie **diagnostische gegevens.**

3.  Selecteer de **type** van metrische gegevens die u wilt controleren voor elke service (Blob, tabel-bestand). Selecteer om te instrueren Azure Storage opgeslagen logboeken met diagnostische gegevens voor lees-, schrijf- en delete-aanvragen voor de blob, table en queue-services, **Blob-Logboeken, tabel logboeken** en **Logboeken in de wachtrij.**

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. Met behulp van de schuifregelaar onder instellen de **bewaren** beleid in dagen (de waarde van 1 – 365). Zeven dagen is de standaardinstelling.

5. Selecteer **opslaan** de configuratieinstellingen toe te passen.

Opslag logboekregistratie logboekvermeldingen bevatten de volgende informatie over afzonderlijke aanvragen:

- Timing gegevens zoals begintijd, end-to-end-latentie en latentie van de server.

- Details van de opslagbewerking zoals de bewerking typt, de sleutel van het opslagobject de client is toegang tot, slagen of mislukken en de HTTP-statuscode geretourneerd naar de client.

- Verificatie-informatie zoals het type verificatie gebruikt voor de client.

- Gelijktijdigheid informatie zoals de ETag-waarde en een tijdstempel voor laatste gewijzigd.

- De grootte van de aanvraag en antwoord-berichten.

Zie voor meer instructies over het inschakelen van logboekregistratie van Storage Analytics gedetailleerde, [bewaken van een opslagaccount in de Azure-portal.](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

### <a name="azure-security-center"></a>Azure Security Center 

[Azure Security Center](https://azure.microsoft.com/services/security-center/) bewaakt de beveiligingsstatus van uw Azure-resources om te voorkomen en bedreigingen te detecteren en aanbevelingen voor reageert. Het biedt help-document op verschillende manieren uw beveiligingsfuncties die de privacy van persoonlijke gegevens te beveiligen.

Beveiligingsstatus bewaken, kunt u ervoor zorgen dat aan uw beveiligingsbeleid. Beveiligingsbewaking is een proactieve strategie waarbij uw resources voor het identificeren van systemen die niet voldoen aan de organisatie-standaarden of aanbevolen procedures worden gecontroleerd. U kunt de beveiligingsstatus van de volgende resources bewaken:

- COMPUTE (virtuele machines en cloudservices)

- Netwerken (virtuele netwerken)

- Opslag en gegevens (server en database controle en detectie van bedreigingen, TDE, versleuteling van opslag)

- Toepassingen (mogelijke beveiligingsproblemen)

Beveiligingsproblemen in een van deze categorieën kunnen een bedreiging vormt voor de privacy van persoonlijke gegevens.

#### <a name="how-do-i-view-the-security-state-of-my-azure-resources"></a>Hoe kan ik de beveiligingsstatus van mijn Azure-resources bekijken?

De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. U vindt alle mogelijke beveiligingsproblemen die het identificeert in de **preventie** sectie van het dashboard.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. In de **preventie** sectie, selecteer de **Compute** tegel. Ziet u hier een **overzicht** samen met de **virtuele machines** lijst van alle virtuele machines en hun status beveiliging en de **Cloudservices** lijst met web-en werkrollen bewaakt door Security Center.

2. Op de **overzicht** tabblad, tweede een aanbeveling voor meer informatie.

3. Op de **virtuele machines** tabblad, selecteert u een virtuele machine om meer details te bekijken.

Wanneer gegevensverzameling is ingeschakeld in Azure Security Center, wordt Microsoft Monitoring Agent automatisch geconfigureerd zijn op alle bestaande en nieuw ondersteunde virtuele machines die zijn geïmplementeerd. Gegevens die worden verzameld van deze agent wordt opgeslagen in een bestaand [logboekanalyse](https://azure.microsoft.com/services/log-analytics/) werkruimte die zijn gekoppeld aan uw abonnement of een nieuwe werkruimte.

[Dreiging Intelligence-rapporten](https://docs.microsoft.com/azure/security-center/security-center-threat-report) worden geleverd door Security Center. Deze bieden u nuttige informatie om te achterhalen van de aanvaller identiteit, doelstellingen, huidige en historische aanval campagnes, en -tactieken, hulpprogramma's en procedures die worden gebruikt. Risicobeperking en het doorvoeren informatie is ook opgenomen.

Het voornaamste doel van deze rapporten bedreiging is helpt u doeltreffend reageren op de onmiddellijke bedreiging en nemen maatregelen achteraf om het probleem te verhelpen. De informatie in de rapporten kan ook van nuttig zijn wanneer u documenteren van uw beveiligingsplan voor rapportage- en controledoeleinden.

De bedreiging Intelligence-rapporten worden vermeld op. PDF-indeling, toegankelijk via een koppeling in de **rapporten** veld van de **verdachte proces dat wordt uitgevoerd** blade voor elke beveiligingswaarschuwing in Azure Security Center.

Zie voor meer informatie over het weergeven en gebruiken van de Threat Intelligence-rapport [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met de Azure Active Directory rapportage-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Wat is Log Analytics?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[Overzicht van de bewaking in Microsoft Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Inleiding tot Azure Activity Log (video)](https://azure.microsoft.com/resources/videos/intro-activity-log/)

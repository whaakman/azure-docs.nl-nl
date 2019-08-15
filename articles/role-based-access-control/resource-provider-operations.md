---
title: Bewerkingen voor de resource provider Azure Resource Manager | Microsoft Docs
description: Een lijst met de bewerkingen die beschikbaar zijn voor de resource providers van de Microsoft Azure Resource Manager
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 2d3a303cb00bab6f67abbc6354ca785880759e4d
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945536"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Bewerkingen voor de resource provider Azure Resource Manager

Dit artikel bevat een overzicht van de bewerkingen die beschikbaar zijn voor elke Azure Resource Manager resource provider. Deze bewerkingen kunnen worden gebruikt in [aangepaste rollen](custom-roles.md) om nauw keurig op [rollen gebaseerd toegangs beheer (RBAC)](overview.md) te bieden voor bronnen in Azure. Bewerkings reeksen hebben de volgende indeling:`{Company}.{ProviderName}/{resourceType}/{action}`

De bewerkingen van de resource provider zijn altijd in ontwikkeling. Als u de meest recente bewerkingen wilt downloaden, gebruikt u de bewerkings lijst van de [AzProviderOperation-](/powershell/module/az.resources/get-azprovideroperation) of [AZ-provider](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.AAD/domainServices/delete | Domein service verwijderen |
> | Action | Microsoft.AAD/domainServices/oucontainer/delete | OE-container verwijderen |
> | Action | Microsoft.AAD/domainServices/oucontainer/read | OE-containers lezen |
> | Action | Microsoft.AAD/domainServices/oucontainer/write | OE-container schrijven |
> | Action | Microsoft.AAD/domainServices/read | Domein services lezen |
> | Action | Microsoft.AAD/domainServices/replicaSets/delete | Cluster site verwijderen |
> | Action | Microsoft.AAD/domainServices/replicaSets/read | Cluster site lezen |
> | Action | Microsoft.AAD/domainServices/replicaSets/write | Cluster site schrijven |
> | Action | Microsoft.AAD/domainServices/write | Domein service schrijven |
> | Action | Microsoft.AAD/locations/operationresults/read |  |
> | Action | Microsoft.AAD/Operations/read |  |
> | Action | Microsoft.AAD/register/action | Domein service registreren |
> | Action | Microsoft.AAD/unregister/action | Registratie van domein service ongedaan maken |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | microsoft.aadiam/diagnosticsettings/delete | Een diagnostische instelling verwijderen |
> | Action | microsoft.aadiam/diagnosticsettings/read | Een diagnostische instelling lezen |
> | Action | microsoft.aadiam/diagnosticsettings/write | Een diagnostische instelling schrijven |
> | Action | microsoft.aadiam/diagnosticsettingscategories/read | Categorieën voor Diagnostische instellingen lezen |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Addons/operations/read | Hiermee worden ondersteunde RP-bewerkingen opgehaald. |
> | Action | Micro soft. Addons/REGI ster/actie | Het opgegeven abonnement registreren bij micro soft. Addons |
> | Action | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Een lijst met actuele informatie over het ondersteunings plan voor het opgegeven abonnement. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Hiermee verwijdert u het opgegeven canonieke ondersteunings plan |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/read | De opgegeven canonieke ondersteunings plan status ophalen. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/write | Hiermee wordt het type canonieke ondersteunings plan toegevoegd dat is opgegeven. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ADHybridHealthService/addsservices/action | Maak een nieuw forest voor de Tenant. |
> | Action | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Hiermee worden alle servers opgehaald voor de opgegeven service naam. |
> | Action | Microsoft.ADHybridHealthService/addsservices/alerts/read | Hiermee worden waarschuwings gegevens opgehaald voor het forest zoals alertid, datum van waarschuwing, laatste gedetecteerde waarschuwing, waarschuwing beschrijving, laatst bijgewerkt, waarschuwings niveau, waarschuwings status, koppelingen voor probleem oplossing, enzovoort. |
> | Action | Microsoft.ADHybridHealthService/addsservices/configuration/read | Hiermee wordt de service configuratie voor het forest opgehaald. Voor beeld: Forestnaam, functionaliteits niveau, domein naamgevings Master FSMO Role, FSMO-functie van schema master, enzovoort. |
> | Action | Microsoft.ADHybridHealthService/addsservices/delete | Hiermee verwijdert u een service en de bijbehorende servers samen met status gegevens. |
> | Action | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Hiermee worden de domeinen en site gegevens voor het forest opgehaald. Voor beeld: status, actieve waarschuwingen, omgezette waarschuwingen, eigenschappen zoals het functionele niveau van het domein, forest, infrastructuur master, PDC, RID-master, enzovoort.  |
> | Action | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Hiermee wordt de voorkeurs instelling van de gebruiker voor het forest opgehaald.<br>Voor beeld: MetricCounterName zoals ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches.<br>Instellingen voor de gebruikers interface grafieken, enzovoort. |
> | Action | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Hiermee wordt een forest-samen vatting opgehaald voor het opgegeven forest, zoals de Forestnaam, het aantal domeinen onder dit forest, het aantal site-en site gegevens, enzovoort. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Hiermee wordt de lijst met ondersteunde metrische gegevens voor een bepaalde service opgehaald.<br>Bijvoorbeeld extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFS-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor ADDomainService.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor ADSync-service. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Op basis van een service krijgt deze API de metrische gegevens.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFederation-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor de ADDomain-service.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor synchronisatie service. |
> | Action | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Met deze API wordt de lijst met alle onboarded ADDomainServices voor een Premium-Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/addsservices/read | Hiermee worden service Details opgehaald voor de opgegeven service naam. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Hiermee worden de replicatie Details opgehaald voor alle servers voor de opgegeven service naam. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Hiermee wordt het aantal domein controllers en de bijbehorende replicatie fouten opgehaald, indien aanwezig. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Hiermee wordt de volledige lijst met domein controllers en de replicatie Details voor het opgegeven forest opgehaald. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Voeg een Server exemplaar toe aan de service. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Tijdens de registratie van de server van ADDomainService wordt deze API aangeroepen om de referenties op te halen voor de onboarding van nieuwe servers. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Hiermee verwijdert u een server voor een bepaalde service en Tenant. |
> | Action | Microsoft.ADHybridHealthService/addsservices/write | Hiermee wordt het ADDomainService-exemplaar voor de Tenant gemaakt of bijgewerkt. |
> | Action | Microsoft.ADHybridHealthService/configuration/action | Hiermee wordt de Tenant configuratie bijgewerkt. |
> | Action | Microsoft.ADHybridHealthService/configuration/read | Hiermee wordt de Tenant configuratie gelezen. |
> | Action | Microsoft.ADHybridHealthService/configuration/write | Hiermee maakt u een Tenant configuratie. |
> | Action | Microsoft.ADHybridHealthService/logs/contents/read | Hiermee wordt de inhoud opgehaald van agent installatie-en registratie logboeken die zijn opgeslagen in BLOB. |
> | Action | Microsoft.ADHybridHealthService/logs/read | Hiermee worden de agent installatie-en registratie logboeken voor de Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/operations/read | Hiermee wordt een lijst opgehaald met bewerkingen die worden ondersteund door het systeem. |
> | Action | Microsoft.ADHybridHealthService/register/action | Hiermee wordt de ADHybrid Health Service Resource provider geregistreerd en wordt het maken van ADHybrid Health Service Resource ingeschakeld. |
> | Action | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Hiermee wordt een lijst met beschik bare regio's opgehaald die door DevOps worden gebruikt om klant incidenten te ondersteunen. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassword/read | Hiermee haalt u de lijst met ongeldige wachtwoord pogingen op voor alle gebruikers in Active Directory Federation Service. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Hiermee wordt de BLOB SAS-URI met de status en het uiteindelijke resultaat van een nieuwe in wachtrij geplaatste rapport taak opgehaald voor frequentie van beschadigde gebruikers namen en wacht woorden per IP-adres per dag voor een bepaalde Tenant. |
> | Action | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Hiermee wordt de lijst met DevOps-toestemmings tenants opgehaald. Normaal gesp roken gebruikt voor klant ondersteuning. |
> | Action | Microsoft.ADHybridHealthService/reports/isdevops/read | Haalt een waarde op die aangeeft of de Tenant is DevOps of niet. |
> | Action | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Update GebruikersID (objectid) voor de geselecteerde dev OPS-Tenant. |
> | Action | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Hiermee wordt de geselecteerde implementatie voor de opgegeven Tenant opgehaald. |
> | Action | Micro soft. ADHybridHealthService/Reports/tenantassigneddeployment/lezen | Als er een Tenant-id wordt opgegeven, wordt de opslag locatie van de Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Hiermee haalt u de geografische locatie op waarvan de gegevens worden geopend. |
> | Action | Microsoft.ADHybridHealthService/services/action | Hiermee wordt een service-exemplaar in de Tenant bijgewerkt. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Hiermee worden de waarschuwingen voor een service gelezen. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Hiermee worden de waarschuwingen voor een service gelezen. |
> | Action | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Als er een functie naam wordt gebruikt, wordt gecontroleerd of een service alles heeft wat u nodig hebt om deze functie te gebruiken. |
> | Action | Microsoft.ADHybridHealthService/services/delete | Hiermee verwijdert u een service-exemplaar in de Tenant. |
> | Action | Microsoft.ADHybridHealthService/services/exporterrors/read | Hiermee worden de export fouten voor een bepaalde synchronisatie service opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/exportstatus/read | Hiermee wordt de export status voor een bepaalde service opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Hiermee ontvangt u feedback over waarschuwingen voor een bepaalde service en server. |
> | Action | Microsoft.ADHybridHealthService/services/metricmetadata/read | Hiermee wordt de lijst met ondersteunde metrische gegevens voor een bepaalde service opgehaald.<br>Bijvoorbeeld extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFS-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor ADDomainService.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor ADSync-service. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Op basis van een service krijgt deze API het gemiddelde voor metrische gegevens voor een bepaalde service.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFederation-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor de ADDomain-service.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor synchronisatie service. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/read | Op basis van een service krijgt deze API de metrische gegevens.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFederation-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor de ADDomain-service.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor synchronisatie service. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Op basis van een service krijgt deze API de geaggregeerde weer gave voor metrische gegevens voor een bepaalde service.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFederation-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor de ADDomain-service.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor synchronisatie service. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Hiermee wordt de bewakings configuratie voor een service toegevoegd of bijgewerkt. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Hiermee worden de bewakings configuraties voor een bepaalde service opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Het toevoegen of bijwerken van bewakings configuraties voor een service. |
> | Action | Microsoft.ADHybridHealthService/services/premiumcheck/read | Met deze API wordt de lijst met alle onboarded Services voor een Premium-Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/read | Hiermee worden de service-exemplaren in de Tenant gelezen. |
> | Action | Microsoft.ADHybridHealthService/services/reports/blobUris/read | Hiermee worden alle Uri's voor Risk ante IP-rapporten opgehaald voor de afgelopen zeven dagen. |
> | Action | Microsoft.ADHybridHealthService/services/reports/details/read | Hiermee wordt een rapport van de meeste 50 gebruikers met een onjuist wacht woord van de afgelopen 7 dagen opgehaald |
> | Action | Microsoft.ADHybridHealthService/services/reports/generateBlobUri/action | Genereert een Risk ante IP-rapport en retourneert een URI die ernaar verwijst. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/action | Hiermee maakt u een Server exemplaar in de service. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Hiermee worden de waarschuwingen voor een server gelezen. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Tijdens de registratie van de server wordt deze API aangeroepen om de referenties op te halen voor de onboarding van nieuwe servers. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Voor een bepaalde server krijgt deze API een lijst met gegevens typen die worden geüpload door de servers en de meest recente tijd voor elke upload. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/delete | Hiermee verwijdert u een Server exemplaar in de service. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Hiermee worden de fout gegevens voor het exporteren van de synchronisatie voor een bepaalde synchronisatie service opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Op basis van een service krijgt deze API de metrische gegevens.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet-account vergrendelingen, totaal aantal mislukte aanvragen, openstaande token aanvragen (proxy), token aanvragen/sec., enzovoort voor de ADFederation-service.<br>NTLM-authenticaties/sec. geslaagde LDAP-bindingen per seconde, LDAP-bindings tijd, LDAP actieve threads, Kerberos-authenticaties/sec, totaal aantal ATQ-threads, enzovoort voor de ADDomain-service.<br>Profiel latentie uitvoeren, TCP-verbindingen die tot stand zijn gebracht, persoonlijke bytes van Insights-agent, Statistieken exporteren naar Azure AD voor synchronisatie service. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/metrics/read | Hiermee wordt de lijst met connectors opgehaald en worden profiel namen uitgevoerd voor de opgegeven service en het betreffende service-lid. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/read | Hiermee leest u het Server exemplaar in de service. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Hiermee wordt de service configuratie voor een bepaalde Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Hiermee wordt de status van de functie white list voor een bepaalde Tenant opgehaald. |
> | Action | Microsoft.ADHybridHealthService/services/write | Hiermee maakt u een service-exemplaar in de Tenant. |
> | Action | Microsoft.ADHybridHealthService/unregister/action | Hiermee wordt de registratie van het abonnement voor ADHybrid Health Service Resource provider ongedaan gemaakt. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. Advisor/configuraties/lezen | Configuraties ophalen |
> | Action | Microsoft.Advisor/configurations/write | Hiermee wordt een configuratie gemaakt/bijgewerkt |
> | Action | Microsoft.Advisor/generateRecommendations/action | Genereert aanbevelingen |
> | Action | Micro soft. Advisor/generateRecommendations/lezen | Hiermee wordt de status van de aanbevelingen genereren opgehaald |
> | Action | Microsoft.Advisor/metadata/read | Meta gegevens ophalen |
> | Action | Micro soft. Advisor/bewerkingen/lezen | Hiermee worden de bewerkingen voor de micro soft Advisor opgehaald |
> | Action | Micro soft. Advisor/aanbevelingen/beschikbaar/actie | Er is nieuwe aanbeveling beschikbaar in micro soft Advisor |
> | Action | Micro soft. Advisor/aanbevelingen/lezen | Aanbevelingen voor lezen |
> | Action | Micro soft. Advisor/aanbevelingen/onderdrukkingen/verwijderen | Onderdrukkingen verwijderen |
> | Action | Micro soft. Advisor/aanbevelingen/onderdrukkingen/lezen | Haalt onderdrukkingen op |
> | Action | Micro soft. Advisor/aanbevelingen/onderdrukkingen/schrijven | Hiermee worden onderdrukkingen gemaakt/bijgewerkt |
> | Action | Micro soft. Advisor/registreren/actie | Hiermee wordt het abonnement voor de micro soft Advisor geregistreerd |
> | Action | Micro soft. Advisor/onderdrukkingen/verwijderen | Onderdrukkingen verwijderen |
> | Action | Micro soft. Advisor/onderdrukkingen/lezen | Haalt onderdrukkingen op |
> | Action | Micro soft. Advisor/onderdrukkingen/schrijven | Hiermee worden onderdrukkingen gemaakt/bijgewerkt |
> | Action | Micro soft. Advisor/registratie/actie | Hiermee wordt de registratie van het abonnement voor micro soft Advisor ongedaan gemaakt |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.AlertsManagement/actionRules/delete | De actie regel verwijderen in een bepaald abonnement. |
> | Action | Microsoft.AlertsManagement/actionRules/read | Alle actie regels voor de invoer filters ophalen. |
> | Action | Micro soft. AlertsManagement/actionRules/schrijven | Een actie regel in een bepaald abonnement maken of bijwerken |
> | Action | Micro soft. AlertsManagement/Alerts/changestate/Action | Wijzig de status van de waarschuwing. |
> | Action | Microsoft.AlertsManagement/alerts/diagnostics/read | Alle diagnostische gegevens voor de waarschuwing ophalen |
> | Action | Microsoft.AlertsManagement/alerts/history/read | Geschiedenis van de waarschuwing ophalen |
> | Action | Microsoft.AlertsManagement/alerts/read | Alle waarschuwingen voor de invoer filters ophalen. |
> | Action | Microsoft.AlertsManagement/alertsList/read | Alle waarschuwingen voor de invoer filters voor alle abonnementen ophalen |
> | Action | Micro soft. AlertsManagement/alertsMetaData/lezen | Meta gegevens van waarschuwingen ophalen voor de invoer parameter. |
> | Action | Microsoft.AlertsManagement/alertsSummary/read | De samen vatting van waarschuwingen ophalen |
> | Action | Microsoft.AlertsManagement/alertsSummaryList/read | De samen vatting van waarschuwingen in abonnementen ophalen |
> | Action | Micro soft. AlertsManagement/Operations/lezen | Hiermee worden de beschik bare bewerkingen gelezen |
> | Action | Microsoft.AlertsManagement/register/action | Hiermee wordt het abonnement voor het micro soft-waarschuwings beheer geregistreerd |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Smart detector-waarschuwings regel in een bepaald abonnement verwijderen |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Alle Smart detector-waarschuwings regels voor de invoer filters ophalen |
> | Action | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Smart detector-waarschuwings regel in een bepaald abonnement maken of bijwerken |
> | Action | Micro soft. AlertsManagement/smartGroups/changestate/Action | Wijzig de status van de Smart Group |
> | Action | Microsoft.AlertsManagement/smartGroups/history/read | Geschiedenis van de slimme groep ophalen |
> | Action | Micro soft. AlertsManagement/smartGroups/lezen | Alle slimme groepen voor de invoer filters ophalen |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of de opgegeven Analyseserver naam geldig is en niet wordt gebruikt. |
> | Action | Microsoft.AnalysisServices/locations/operationresults/read | Hiermee wordt de informatie opgehaald van het opgegeven bewerkings resultaat. |
> | Action | Microsoft.AnalysisServices/locations/operationstatuses/read | Hiermee wordt de informatie opgehaald van de opgegeven bewerkings status. |
> | Action | Microsoft.AnalysisServices/operations/read | Hiermee wordt de informatie over bewerkingen opgehaald |
> | Action | Microsoft.AnalysisServices/register/action | Registreert Analysis Services resource provider. |
> | Action | Microsoft.AnalysisServices/servers/delete | Hiermee verwijdert u de Analyseserver. |
> | Action | Microsoft.AnalysisServices/servers/listGatewayStatus/action | De status van de gateway die is gekoppeld met de server weer geven. |
> | Action | Microsoft.AnalysisServices/servers/read | Hiermee wordt de informatie opgehaald van de opgegeven Analyseserver. |
> | Action | Microsoft.AnalysisServices/servers/resume/action | Hiermee wordt de Analyseserver hervat. |
> | Action | Microsoft.AnalysisServices/servers/skus/read | Beschik bare SKU-gegevens voor de server ophalen |
> | Action | Microsoft.AnalysisServices/servers/suspend/action | Hiermee wordt de Analyseserver onderbroken. |
> | Action | Microsoft.AnalysisServices/servers/write | Hiermee wordt het opgegeven Analyseserver gemaakt of bijgewerkt. |
> | Action | Microsoft.AnalysisServices/skus/read | Hiermee wordt de informatie van Sku's opgehaald |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ApiManagement/checkNameAvailability/read | Controleert of de gegeven service naam beschikbaar is |
> | Action | Microsoft.ApiManagement/operations/read | Alle API-bewerkingen lezen die beschikbaar zijn voor micro soft. ApiManagement-resource |
> | Action | Microsoft.ApiManagement/register/action | Het abonnement voor de resource provider micro soft. ApiManagement registreren |
> | Action | Micro soft. ApiManagement/rapporten/lezen | Haal rapporten op die zijn geaggregeerd met tijds perioden, een geografische regio, ontwikkel aars, producten, Api's, bewerkingen, abonnementen en byRequest. |
> | Action | Microsoft.ApiManagement/service/apis/delete | Hiermee verwijdert u de opgegeven API van het API Management service-exemplaar. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/delete | Hiermee verwijdert u de opgegeven diagnose vanuit een API. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/read | Een lijst met alle diagnostische gegevens van een API. of worden de details van de diagnose opgehaald voor een API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/write | Hiermee maakt u een nieuwe diagnose voor een API of wordt een bestaande versie bijgewerkt. of werkt de details bij van de diagnose voor een API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Hiermee verwijdert u de opgegeven opmerking van een probleem. |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/read | Een lijst met alle bijlagen voor het probleem dat is gekoppeld aan de opgegeven API. of worden de details van de kwestie bijlage opgehaald voor een API die is opgegeven met de id. |
> | Action | Micro soft. ApiManagement/service/api's/problemen/bijlagen/schrijven | Hiermee wordt een nieuwe bijlage gemaakt voor het probleem in een API of wordt een bestaand item bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/delete | Hiermee verwijdert u de opgegeven opmerking van een probleem. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/read | Een lijst met alle opmerkingen voor het probleem dat is gekoppeld aan de opgegeven API. of worden de details van de probleem Opmerking opgehaald voor een API die is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/write | Hiermee maakt u een nieuwe opmerking voor het probleem in een API of wordt een bestaand item bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/apis/issues/delete | Hiermee verwijdert u het opgegeven probleem uit een API. |
> | Action | Microsoft.ApiManagement/service/apis/issues/read | Een lijst met alle problemen die zijn gekoppeld aan de opgegeven API. of worden de details opgehaald van het probleem voor een API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/issues/write | Hiermee maakt u een nieuw probleem voor een API of wordt een bestaande versie bijgewerkt. of werkt een bestaand probleem bij voor een API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/delete | Hiermee verwijdert u de opgegeven bewerking in de API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/delete | Hiermee verwijdert u de beleids configuratie bij de API-bewerking. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/read | De lijst met beleids configuratie op het API-bewerkings niveau ophalen. of u kunt de beleids configuratie ophalen op het niveau van de API-bewerking. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/write | Hiermee wordt een beleids configuratie voor het API-bewerkings niveau gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/delete | De beleids configuratie verwijderen op het niveau van de bewerking |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/read | De beleids configuratie ophalen op het niveau van de bewerking |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/write | Beleids configuratie maken op bewerkings niveau |
> | Action | Microsoft.ApiManagement/service/apis/operations/read | Hiermee wordt een verzameling van de bewerkingen voor de opgegeven API weer gegeven. of worden de details opgehaald van de API-bewerking die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/delete | Ontkoppel de code van de bewerking. |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/read | Een lijst met alle labels die aan de bewerking zijn gekoppeld. of Get-label dat is gekoppeld aan de bewerking. |
> | Action | Micro soft. ApiManagement/service/api's/Operations/Tags/schrijven | Wijs een tag toe aan de bewerking. |
> | Action | Microsoft.ApiManagement/service/apis/operations/write | Hiermee maakt u een nieuwe bewerking in de API of werkt u een bestaande. of werkt de details van de bewerking bij in de API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/operationsByTags/read | Hiermee wordt een verzameling bewerkingen weer gegeven die zijn gekoppeld aan Tags. |
> | Action | Microsoft.ApiManagement/service/apis/policies/delete | Hiermee verwijdert u de beleids configuratie op de API. |
> | Action | Microsoft.ApiManagement/service/apis/policies/read | De beleids configuratie ophalen op API-niveau. of de beleids configuratie op het API-niveau ophalen. |
> | Action | Micro soft. ApiManagement/service/api's/beleid/schrijven | Hiermee wordt een beleids configuratie voor de API gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/apis/policy/delete | De beleids configuratie op API-niveau verwijderen |
> | Action | Microsoft.ApiManagement/service/apis/policy/read | De beleids configuratie ophalen op API-niveau |
> | Action | Micro soft. ApiManagement/service/api's/beleid/schrijven | Beleids configuratie maken op API-niveau |
> | Action | Microsoft.ApiManagement/service/apis/products/read | Een lijst met alle producten waarvan de API deel uitmaakt. |
> | Action | Microsoft.ApiManagement/service/apis/read | Een lijst met alle Api's van de API Management service-instantie. of worden de details opgehaald van de API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/releases/delete | Hiermee verwijdert u alle versies van de API of verwijdert u de opgegeven release in de API. |
> | Action | Microsoft.ApiManagement/service/apis/releases/read | Een lijst met alle releases van een API.<br>Er wordt een API-versie gemaakt wanneer u een API-revisie actueel maakt.<br>Releases worden ook gebruikt om terug te draaien naar eerdere revisies.<br>De resultaten worden gewisseld en kunnen worden beperkt door de $top en de $skip-para meters.<br>of retourneert de details van een API-release. |
> | Action | Micro soft. ApiManagement/service/api's/releases/schrijven | Hiermee maakt u een nieuwe release voor de API. of werkt de details bij van de release van de API die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apis/revisions/delete | Hiermee verwijdert u alle revisies van een API |
> | Action | Microsoft.ApiManagement/service/apis/revisions/read | Een lijst met alle revisies van een API. |
> | Action | Microsoft.ApiManagement/service/apis/schemas/delete | Hiermee verwijdert u de schema configuratie bij de API. |
> | Action | Microsoft.ApiManagement/service/apis/schemas/read | De schema configuratie ophalen op API-niveau. u kunt ook de schema configuratie ophalen op het API-niveau. |
> | Action | Micro soft. ApiManagement/service/api's/schema's/schrijven | Hiermee wordt een schema configuratie voor de API gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Verwijder de beschrijving van de tag voor de API. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Een lijst met alle beschrijvingen van tags binnen het bereik van API. Model dat vergelijkbaar is met Swagger-tagDescription is gedefinieerd op API-niveau maar tag kan worden toegewezen aan de bewerkingen of een Get-label beschrijving in het bereik van API |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Beschrijving van een tag maken/bijwerken binnen het bereik van de API. |
> | Action | Microsoft.ApiManagement/service/apis/tags/delete | Ontkoppel de tag uit de API. |
> | Action | Microsoft.ApiManagement/service/apis/tags/read | Een lijst met alle labels die zijn gekoppeld aan de API. of Get-label dat is gekoppeld aan de API. |
> | Action | Micro soft. ApiManagement/service/api's/Tags/schrijven | Wijs tag toe aan de API. |
> | Action | Microsoft.ApiManagement/service/apis/write | Hiermee maakt u een nieuwe of bijgewerkte bestaande API van het API Management service-exemplaar. of werkt de opgegeven API van het API Management service-exemplaar bij. |
> | Action | Microsoft.ApiManagement/service/apisByTags/read | Hiermee wordt een verzameling api's weer gegeven die zijn gekoppeld aan Tags. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/delete | Hiermee verwijdert u een specifieke API-versieset. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/read | Geeft een verzameling van API-versie sets in het opgegeven service-exemplaar. of worden de details opgehaald van de API-versieset die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Lijst met versie-entiteiten ophalen |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/write | Hiermee wordt een API-versieset gemaakt of bijgewerkt. of werkt de details bij van de API-versieset die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Hiermee worden de micro soft. ApiManagement-resources die worden uitgevoerd in Virtual Network bijgewerkt om bijgewerkte netwerk instellingen te kiezen. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/delete | Hiermee verwijdert u een specifiek exemplaar van de autorisatie server. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/read | Hiermee wordt een verzameling autorisatie servers weer gegeven die zijn gedefinieerd binnen een service-exemplaar. of worden de details opgehaald van de autorisatie server die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/write | Hiermee maakt u een nieuwe autorisatie server of werkt u een bestaande autorisatie server bij. of worden de details van de autorisatie server die is opgegeven door de id, bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/backends/delete | Hiermee verwijdert u de opgegeven back-end. |
> | Action | Microsoft.ApiManagement/service/backends/read | Hiermee wordt een verzameling back-endservers in het opgegeven service-exemplaar weer gegeven. of worden de details opgehaald van de back-end die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/backends/reconnect/action | Hiermee wordt de APIM-proxy op de hoogte gebracht van een nieuwe verbinding met de back-end na de opgegeven time-out. Als er geen time-out is opgegeven, wordt er een time-out van 2 minuten gebruikt. |
> | Action | Microsoft.ApiManagement/service/backends/write | Hiermee wordt een back-end gemaakt of bijgewerkt. of een bestaande back-end bijwerken. |
> | Action | Microsoft.ApiManagement/service/backup/action | Backup API Management-service naar de opgegeven container in een door de gebruiker opgegeven opslag account |
> | Action | Microsoft.ApiManagement/service/caches/delete | Hiermee verwijdert u een specifieke cache. |
> | Action | Microsoft.ApiManagement/service/caches/read | Hiermee wordt een verzameling van alle externe caches in het opgegeven service-exemplaar weer gegeven. of worden de details opgehaald van de cache die is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/caches/write | Hiermee wordt een externe cache gemaakt of bijgewerkt die moet worden gebruikt in het API Management-exemplaar. of werkt de details bij van de cache die is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/certificates/delete | Hiermee verwijdert u een specifiek certificaat. |
> | Action | Microsoft.ApiManagement/service/certificates/read | Hiermee wordt een verzameling van alle certificaten in het opgegeven service-exemplaar weer gegeven. of worden de details opgehaald van het certificaat dat is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/certificates/write | Hiermee wordt het certificaat dat wordt gebruikt voor verificatie met de back-end, gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Hiermee verwijdert u het opgegeven inhouds item. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Hiermee wordt een lijst met inhouds items geretourneerd of Details van het inhouds item geretourneerd |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Hiermee wordt een nieuw inhouds item gemaakt of een opgegeven inhouds item bijgewerkt |
> | Action | Microsoft.ApiManagement/service/contentTypes/read | Hiermee wordt een lijst met inhouds typen geretourneerd |
> | Action | Microsoft.ApiManagement/service/delete | API Management service-exemplaar verwijderen |
> | Action | Microsoft.ApiManagement/service/diagnostics/delete | Hiermee verwijdert u de opgegeven diagnose. |
> | Action | Microsoft.ApiManagement/service/diagnostics/read | Een lijst met alle diagnostische gegevens van de API Management service-instantie. of worden de details opgehaald van de diagnose die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/diagnostics/write | Hiermee wordt een nieuwe diagnose gemaakt of een bestaande bijgewerkt. of de details van de diagnostische gegevens die door de id zijn opgegeven, worden bijgewerkt. |
> | Action | Micro soft. ApiManagement/service/gateways/actie | Hiermee wordt de gateway configuratie opgehaald. of werkt de heartbeat van de gateway bij. |
> | Action | Micro soft. ApiManagement/service/gateways/verwijderen | Hiermee verwijdert u de specifieke gateway. |
> | Action | Micro soft. ApiManagement/service/gateways/sleutels/actie | Hiermee worden gateway sleutels opgehaald. |
> | Action | Micro soft. ApiManagement/service/gateways/lezen | Een lijst met gateways die zijn geregistreerd bij service-exemplaar. of worden de details opgehaald van de gateway die is opgegeven door de id. |
> | Action | Micro soft. ApiManagement/service/gateways/regeneratePrimaryKey/actie | Hiermee genereert u de primaire-gateway sleutel invalidationg alle tokens die ermee zijn gemaakt. |
> | Action | Micro soft. ApiManagement/service/gateways/regenerateSecondaryKey/actie | Hiermee genereert u de secundaire gateway sleutel invalidationg alle tokens die ermee zijn gemaakt. |
> | Action | Micro soft. ApiManagement/service/gateways/token/actie | Hiermee wordt het token voor gedeelde toegangs autorisatie opgehaald voor de gateway. |
> | Action | Micro soft. ApiManagement/service/gateways/schrijven | Hiermee wordt een gateway gemaakt of bijgewerkt die moet worden gebruikt in het API Management-exemplaar. of werkt de details bij van de gateway die is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/getssotoken/action | Hiermee wordt een SSO-token opgehaald dat kan worden gebruikt om zich aan te melden bij API Management service verouderde portal als beheerder |
> | Action | Microsoft.ApiManagement/service/groups/delete | Hiermee verwijdert u een specifieke groep van het API Management service-exemplaar. |
> | Action | Microsoft.ApiManagement/service/groups/read | Een lijst met groepen die zijn gedefinieerd binnen een service-exemplaar. of worden de details opgehaald van de groep die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/groups/users/delete | Bestaande gebruiker uit bestaande groep verwijderen. |
> | Action | Microsoft.ApiManagement/service/groups/users/read | Hiermee wordt een verzameling gebruikers entiteiten weer gegeven die aan de groep zijn gekoppeld. |
> | Action | Micro soft. ApiManagement/service/groepen/gebruikers/schrijven | Een bestaande gebruiker toevoegen aan een bestaande groep |
> | Action | Microsoft.ApiManagement/service/groups/write | Hiermee wordt een groep gemaakt of bijgewerkt. of worden de details van de groep die is opgegeven door de id, bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/identityProviders/delete | Hiermee verwijdert u de opgegeven id-provider configuratie. |
> | Action | Microsoft.ApiManagement/service/identityProviders/read | Geeft een lijst van een id-provider die is geconfigureerd in het opgegeven service-exemplaar. of worden de configuratie details opgehaald van de ID-provider die is geconfigureerd in het opgegeven service-exemplaar. |
> | Action | Microsoft.ApiManagement/service/identityProviders/write | Hiermee wordt de Identity provider-configuratie gemaakt of bijgewerkt. of werkt een bestaande Identity provider-configuratie bij. |
> | Action | Microsoft.ApiManagement/service/issues/read | Hiermee wordt een verzameling van problemen in het opgegeven service-exemplaar weer gegeven. of krijgt API Management probleem Details |
> | Action | Microsoft.ApiManagement/service/locations/networkstatus/read | Hiermee wordt de netwerk toegangs status opgehaald van de resources waarvan de service afhankelijk is in de locatie. |
> | Action | Microsoft.ApiManagement/service/loggers/delete | Hiermee verwijdert u de opgegeven logboek registratie. |
> | Action | Microsoft.ApiManagement/service/loggers/read | Hiermee wordt een verzameling Logboeken in het opgegeven service-exemplaar weer gegeven. of worden de details opgehaald van het logboek dat is opgegeven door de id. |
> | Action | Micro soft. ApiManagement/service/logger/schrijven | Hiermee wordt een logboek registratie gemaakt of bijgewerkt. of werkt een bestaande logger bij. |
> | Action | Microsoft.ApiManagement/service/managedeployments/action | SKU/eenheden wijzigen, regionale implementaties van API Management service toevoegen/verwijderen |
> | Action | Microsoft.ApiManagement/service/networkstatus/read | Hiermee wordt de netwerk toegangs status opgehaald van de resources waarvan de service afhankelijk is. |
> | Action | Microsoft.ApiManagement/service/notifications/action | Hiermee wordt een melding verzonden naar een opgegeven gebruiker |
> | Action | Microsoft.ApiManagement/service/notifications/read | Hiermee wordt een verzameling eigenschappen weer gegeven die zijn gedefinieerd binnen een service-exemplaar. of worden de details opgehaald van de melding die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Hiermee verwijdert u het e-mail bericht uit de lijst met meldingen. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Hiermee wordt de lijst met e-mail berichten voor meldingen ontvangen die zijn geabonneerd op een melding. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Hiermee wordt het e-mail adres toegevoegd aan de lijst met ontvangers voor de melding. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Hiermee verwijdert u de API Management gebruiker uit de lijst met meldingen. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Hiermee wordt de lijst opgehaald met de ontvanger van de meldings gebruiker die is geabonneerd op de melding. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Voegt de API Management gebruiker toe aan de lijst met ontvangers voor de melding. |
> | Action | Microsoft.ApiManagement/service/notifications/write | Melding van API Management Uitgever maken of bijwerken. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/delete | Hiermee verwijdert u een specifieke OpenID Connect Connect-provider van het exemplaar van de API Management service. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/read | Een lijst met alle OpenID Connect Connect-providers. of haalt een specifieke OpenID Connect Connect-provider op. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/write | Hiermee wordt de OpenID Connect Connect-provider gemaakt of bijgewerkt. of werkt de specifieke OpenID Connect Connect-provider bij. |
> | Action | Micro soft. ApiManagement/service/operationresults/lezen | Hiermee wordt de huidige status van een langlopende bewerking opgehaald |
> | Action | Microsoft.ApiManagement/service/policies/delete | Hiermee verwijdert u de globale beleids configuratie van de API Management-service. |
> | Action | Micro soft. ApiManagement/service/beleid/lezen | Een lijst met alle globale beleids definities van de API Management-service. of de globale beleids definitie van de API Management-service ophalen. |
> | Action | Micro soft. ApiManagement/service/beleid/schrijven | Hiermee wordt de globale beleids configuratie van de API Management-service gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/policy/delete | De beleids configuratie op Tenant niveau verwijderen |
> | Action | Micro soft. ApiManagement/service/beleid/lezen | De beleids configuratie ophalen op Tenant niveau |
> | Action | Micro soft. ApiManagement/service/beleid/schrijven | Beleids configuratie maken op Tenant niveau |
> | Action | Microsoft.ApiManagement/service/policySnippets/read | Een lijst met alle beleids fragmenten. |
> | Action | Microsoft.ApiManagement/service/portalsettings/read | Instellingen voor aanmelden ophalen voor de portal of instellingen voor het aanmelden ophalen voor de portal of voor het ophalen van delegerings instellingen voor de portal. |
> | Action | Micro soft. ApiManagement/service/portalsettings/schrijven | Aanmeldings instellingen bijwerken. of om aanmeldings instellingen te maken of bij te werken. of update de instellingen voor aanmelden of het bijwerken van instellingen voor het aanmelden of het bijwerken van de instellingen voor delegering. of u instellingen voor delegering wilt maken of bijwerken. |
> | Action | Microsoft.ApiManagement/service/products/apis/delete | Hiermee verwijdert u de opgegeven API uit het opgegeven product. |
> | Action | Micro soft. ApiManagement/service/producten/api's/lezen | Hiermee wordt een verzameling weer gegeven van de Api's die zijn gekoppeld aan een product. |
> | Action | Micro soft. ApiManagement/service/producten/api's/schrijven | Hiermee wordt een API toegevoegd aan het opgegeven product. |
> | Action | Microsoft.ApiManagement/service/products/delete | Product verwijderen. |
> | Action | Microsoft.ApiManagement/service/products/groups/delete | Hiermee verwijdert u de koppeling tussen de opgegeven groep en het product. |
> | Action | Microsoft.ApiManagement/service/products/groups/read | Hier wordt de verzameling ontwikkelaars groepen weer gegeven die zijn gekoppeld aan het opgegeven product. |
> | Action | Microsoft.ApiManagement/service/products/groups/write | Hiermee wordt de koppeling tussen de opgegeven ontwikkel groep en het opgegeven product toegevoegd. |
> | Action | Microsoft.ApiManagement/service/products/policies/delete | Hiermee verwijdert u de beleids configuratie van het product. |
> | Action | Microsoft.ApiManagement/service/products/policies/read | De configuratie van het beleid op product niveau ophalen. of de beleids configuratie op product niveau ophalen. |
> | Action | Micro soft. ApiManagement/service/producten/beleid/schrijven | Hiermee wordt een beleids configuratie voor het product gemaakt of bijgewerkt. |
> | Action | Microsoft.ApiManagement/service/products/policy/delete | De beleids configuratie op product niveau verwijderen |
> | Action | Micro soft. ApiManagement/service/producten/beleid/lezen | De beleids configuratie ophalen op product niveau |
> | Action | Micro soft. ApiManagement/service/producten/beleid/schrijven | Beleids configuratie maken op product niveau |
> | Action | Microsoft.ApiManagement/service/products/read | Een lijst met producten in het opgegeven service-exemplaar. of worden de details opgehaald van het product dat is opgegeven door de id. |
> | Action | Micro soft. ApiManagement/service/producten/abonnementen/lezen | Hiermee wordt de verzameling abonnementen op het opgegeven product weer gegeven. |
> | Action | Microsoft.ApiManagement/service/products/tags/delete | Ontkoppel de tag uit het product. |
> | Action | Micro soft. ApiManagement/service/Producten/Tags/lezen | Een lijst met alle labels die aan het product zijn gekoppeld. of Get-label dat is gekoppeld aan het product. |
> | Action | Micro soft. ApiManagement/service/Producten/Tags/schrijven | Wijs tag toe aan het product. |
> | Action | Micro soft. ApiManagement/service/producten/schrijven | Hiermee wordt een product gemaakt of bijgewerkt. of bestaande product gegevens bijwerken. |
> | Action | Micro soft. ApiManagement/service/productsByTags/lezen | Een lijst met producten die aan Tags zijn gekoppeld. |
> | Action | Microsoft.ApiManagement/service/properties/delete | Hiermee verwijdert u de specifieke eigenschap van het API Management service-exemplaar. |
> | Action | Microsoft.ApiManagement/service/properties/read | Hiermee wordt een verzameling eigenschappen weer gegeven die zijn gedefinieerd binnen een service-exemplaar. of worden de details opgehaald van de eigenschap die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/properties/write | Hiermee wordt een eigenschap gemaakt of bijgewerkt. of werkt de specifieke eigenschap bij. |
> | Action | Microsoft.ApiManagement/service/quotas/periods/read | Waarde van quotum teller voor periode ophalen |
> | Action | Microsoft.ApiManagement/service/quotas/periods/write | Huidige waarde van quotum teller instellen |
> | Action | Microsoft.ApiManagement/service/quotas/read | Waarden voor quotum ophalen |
> | Action | Microsoft.ApiManagement/service/quotas/write | Huidige waarde van quotum teller instellen |
> | Action | Microsoft.ApiManagement/service/read | Meta gegevens voor een API Management service-exemplaar lezen |
> | Action | Microsoft.ApiManagement/service/regions/read | Een lijst met alle Azure-regio's waarin de service bestaat. |
> | Action | Microsoft.ApiManagement/service/reports/read | Rapport ophalen dat is geaggregeerd met tijds perioden of het rapport ophalen dat is geaggregeerd door de geografische regio of het rapport ophalen dat is geaggregeerd door ontwikkel aars.<br>of haal het rapport op dat is geaggregeerd door producten.<br>of haal het rapport op dat door Api's is geaggregeerd of maak het rapport dat is geaggregeerd door bewerkingen of ontvang het rapport dat is geaggregeerd met een abonnement.<br>of aanvragen rapport gegevens ophalen |
> | Action | Microsoft.ApiManagement/service/restore/action | De API Management-service herstellen vanuit de opgegeven container in een opslag account dat door de gebruiker is opgegeven |
> | Action | Microsoft.ApiManagement/service/subscriptions/delete | Hiermee verwijdert u het opgegeven abonnement. |
> | Action | Microsoft.ApiManagement/service/subscriptions/read | Een lijst met alle abonnementen van het API Management service-exemplaar. of Hiermee wordt de opgegeven abonnements entiteit opgehaald. |
> | Action | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Hiermee wordt de primaire sleutel van het bestaande abonnement van het API Management service-exemplaar opnieuw gegenereerd. |
> | Action | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Hiermee wordt de secundaire sleutel van het bestaande abonnement van het exemplaar van de API Management-service opnieuw gegenereerd. |
> | Action | Microsoft.ApiManagement/service/subscriptions/write | Hiermee wordt het abonnement van de opgegeven gebruiker voor het opgegeven product gemaakt of bijgewerkt. of werkt de details bij van een abonnement dat is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/tagResources/read | Hiermee wordt een verzameling resources die zijn gekoppeld aan Tags weer gegeven. |
> | Action | Microsoft.ApiManagement/service/tags/delete | Hiermee verwijdert u een specifiek label van het API Management service-exemplaar. |
> | Action | Microsoft.ApiManagement/service/tags/read | Hiermee wordt een verzameling tags weer gegeven die zijn gedefinieerd binnen een service-exemplaar. of worden de details opgehaald van het label dat door de id is opgegeven. |
> | Action | Micro soft. ApiManagement/service/Tags/schrijven | Hiermee maakt u een tag. of werkt de details bij van het label dat is opgegeven met de id. |
> | Action | Microsoft.ApiManagement/service/templates/delete | Standaard API Management e-mail sjabloon opnieuw instellen |
> | Action | Microsoft.ApiManagement/service/templates/read | Hiermee worden alle e-mail sjablonen opgehaald of API Management e-mail sjabloon gegevens opgehaald |
> | Action | Micro soft. ApiManagement/service/templates/schrijven | API Management e-mail sjabloon of updates API Management e-mail sjabloon maken of bijwerken |
> | Action | Microsoft.ApiManagement/service/tenant/delete | Beleids configuratie voor de Tenant verwijderen |
> | Action | Microsoft.ApiManagement/service/tenant/deploy/action | Voert een implementatie taak uit om wijzigingen van de opgegeven Git-vertakking toe te passen op de configuratie in de data base. |
> | Action | Microsoft.ApiManagement/service/tenant/operationResults/read | Lijst met bewerkings resultaten ophalen of resultaat van een specifieke bewerking ophalen |
> | Action | Microsoft.ApiManagement/service/tenant/read | De globale beleids definitie van de API Management-service ophalen. of Details van Tenant toegangs gegevens ophalen |
> | Action | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Primaire toegangs sleutel opnieuw genereren |
> | Action | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Secundaire toegangs sleutel opnieuw genereren |
> | Action | Microsoft.ApiManagement/service/tenant/save/action | Maakt door voeren met configuratie momentopname aan de opgegeven vertakking in de opslag plaats |
> | Action | Microsoft.ApiManagement/service/tenant/syncState/read | Status van de laatste Git-synchronisatie ophalen |
> | Action | Microsoft.ApiManagement/service/tenant/validate/action | Hiermee worden wijzigingen van de opgegeven Git-vertakking gecontroleerd |
> | Action | Microsoft.ApiManagement/service/tenant/write | Beleids configuratie instellen voor de Tenant of Details van Tenant toegangs gegevens bijwerken |
> | Action | Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat voor een API Management-service uploaden |
> | Action | Microsoft.ApiManagement/service/updatehostname/action | Aangepaste domein namen instellen, bijwerken of verwijderen voor een API Management-service |
> | Action | Microsoft.ApiManagement/service/users/action | Een nieuwe gebruiker registreren |
> | Action | Microsoft.ApiManagement/service/users/confirmations/send/action | Bevestiging verzenden |
> | Action | Microsoft.ApiManagement/service/users/delete | Hiermee verwijdert u een specifieke gebruiker. |
> | Action | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Hiermee wordt een omleidings-URL met een verificatie token opgehaald voor het ondertekenen van een bepaalde gebruiker in de ontwikkelaars Portal. |
> | Action | Microsoft.ApiManagement/service/users/groups/read | Een lijst met alle gebruikers groepen. |
> | Action | Microsoft.ApiManagement/service/users/identities/read | Lijst met alle gebruikers-id's. |
> | Action | Microsoft.ApiManagement/service/users/keys/read | Sleutels ophalen die aan de gebruiker zijn gekoppeld |
> | Action | Microsoft.ApiManagement/service/users/read | Hiermee wordt een verzameling geregistreerde gebruikers weer gegeven in het opgegeven service-exemplaar. of worden de details opgehaald van de gebruiker die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/users/subscriptions/read | Hiermee wordt de verzameling abonnementen van de opgegeven gebruiker weer gegeven. |
> | Action | Microsoft.ApiManagement/service/users/token/action | Hiermee haalt u het Shared Access Authorization-token voor de gebruiker. |
> | Action | Microsoft.ApiManagement/service/users/write | Hiermee wordt een gebruiker gemaakt of bijgewerkt. of werkt de details bij van de gebruiker die is opgegeven door de id. |
> | Action | Microsoft.ApiManagement/service/write | Een nieuw exemplaar van API Management service maken |
> | Action | Micro soft. ApiManagement/registratie/actie | De registratie van het abonnement voor de resource provider micro soft. ApiManagement is ongedaan gemaakt |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. Authorization/classicAdministrators/verwijderen | Hiermee wordt de beheerder verwijderd uit het abonnement. |
> | Action | Micro soft. Authorization/classicAdministrators/operationstatuses/Read | Hiermee haalt u de bewerkingsstatussen van het abonnement op. |
> | Action | Microsoft.Authorization/classicAdministrators/read | Hiermee worden de beheerders voor het abonnement ingelezen. |
> | Action | Microsoft.Authorization/classicAdministrators/write | Hiermee wordt de beheerder gewijzigd of aan een abonnement toegevoegd. |
> | Action | Microsoft.Authorization/denyAssignments/delete | Een opdracht tot weigeren van een toewijzing verwijderen uit het opgegeven bereik. |
> | Action | Microsoft.Authorization/denyAssignments/read | Informatie ophalen over het weigeren van een toewijzing. |
> | Action | Microsoft.Authorization/denyAssignments/write | Een opdracht tot weigeren van een toewijzing maken in het opgegeven bereik. |
> | Action | Microsoft.Authorization/elevateAccess/action | Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik |
> | Action | Micro soft. autorisatie/vergren delen/verwijderen | De vergrendelingen bij het opgegeven bereik worden verwijderd. |
> | Action | Micro soft. autorisatie/vergren delen/lezen | De vergrendelingen bij het opgegeven bereik worden opgehaald. |
> | Action | Microsoft.Authorization/locks/write | Hiermee worden vergrendelingen toegevoegd bij het opgegeven bereik. |
> | Action | Micro soft. Authorization/Operations/Read | Hiermee haalt u de lijst met bewerkingen op |
> | Action | Micro soft. Authorization/permissions/Read | Hiermee worden alle machtigingen van de beller voor een bepaald bereik weergegeven in een lijst. |
> | Action | Microsoft.Authorization/policyAssignments/delete | Een beleidstoewijzing verwijderen bij het opgegeven bereik. |
> | Action | Microsoft.Authorization/policyAssignments/read | Informatie ophalen over een beleidstoewijzing. |
> | Action | Microsoft.Authorization/policyAssignments/write | Een beleidstoewijzing maken bij het opgegeven bereik. |
> | Action | Micro soft. Authorization/policyDefinitions/verwijderen | Een beleidsdefinitie verwijderen. |
> | Action | Micro soft. Authorization/policyDefinitions/lezen | Informatie ophalen over een beleidsdefinitie. |
> | Action | Microsoft.Authorization/policyDefinitions/write | Een aangepaste beleidsdefinitie maken. |
> | Action | Microsoft.Authorization/policySetDefinitions/delete | Een beleidssetdefinitie verwijderen. |
> | Action | Micro soft. Authorization/policySetDefinitions/lezen | Informatie ophalen over een beleidssetdefinitie. |
> | Action | Microsoft.Authorization/policySetDefinitions/write | Een aangepaste beleidssetdefinitie maken. |
> | Action | Microsoft.Authorization/providerOperations/read | Bewerkingen ophalen voor alle resourceproviders die kunnen worden gebruikt in roldefinities. |
> | Action | Microsoft.Authorization/roleAssignments/delete | Een roltoewijzing bij het opgegeven bereik verwijderen. |
> | Action | Micro soft. Authorization/roleAssignments/lezen | Informatie ophalen over een roltoewijzing. |
> | Action | Microsoft.Authorization/roleAssignments/write | Een roltoewijzing bij het opgegeven bereik maken. |
> | Action | Micro soft. Authorization/roleDefinitions/verwijderen | De opgegeven aangepaste roldefinitie verwijderen. |
> | Action | Micro soft. Authorization/roleDefinitions/lezen | Informatie ophalen over een roldefinitie. |
> | Action | Micro soft. Authorization/roleDefinitions/write | Een aangepaste roldefinitie maken of bijwerken met opgegeven machtigingen of toewijsbare bereiken. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | De registratie gegevens van een Azure Automation DSC lezen |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Schrijft een aanvraag om Azure Automation DSC-sleutels opnieuw te genereren |
> | Action | Microsoft.Automation/automationAccounts/certificates/delete | Hiermee wordt een Azure Automation-certificaat element verwijderd |
> | Action | Microsoft.Automation/automationAccounts/certificates/getCount/action | Hiermee wordt het aantal certificaten gelezen |
> | Action | Microsoft.Automation/automationAccounts/certificates/read | Hiermee wordt een Azure Automation-certificaat element opgehaald |
> | Action | Microsoft.Automation/automationAccounts/certificates/write | Hiermee wordt een Azure Automation certificaat-Asset gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Hiermee wordt de compilatie van een Azure Automation DSC gelezen |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Hiermee wordt de compilatie van een Azure Automation DSC gelezen |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Hiermee wordt de compilatie van een Azure Automation DSC geschreven |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Hiermee wordt de compilatie van een Azure Automation DSC geschreven |
> | Action | Microsoft.Automation/automationAccounts/configurations/content/read | Hiermee wordt de inhoud van de configuratie Media gelezen |
> | Action | Microsoft.Automation/automationAccounts/configurations/delete | Hiermee wordt de inhoud van een Azure Automation DSC verwijderd |
> | Action | Microsoft.Automation/automationAccounts/configurations/getCount/action | Hiermee wordt het aantal gelezen van een Azure Automation DSC-inhoud |
> | Action | Microsoft.Automation/automationAccounts/configurations/read | Hiermee wordt de inhoud van een Azure Automation DSC opgehaald |
> | Action | Microsoft.Automation/automationAccounts/configurations/write | Schrijft de inhoud van een Azure Automation DSC |
> | Action | Microsoft.Automation/automationAccounts/connections/delete | Hiermee wordt een Azure Automation verbindings element verwijderd |
> | Action | Microsoft.Automation/automationAccounts/connections/getCount/action | Hiermee wordt het aantal verbindingen gelezen |
> | Action | Microsoft.Automation/automationAccounts/connections/read | Hiermee wordt een Azure Automation verbindings element opgehaald |
> | Action | Microsoft.Automation/automationAccounts/connections/write | Hiermee wordt een Azure Automation verbindings-Asset gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/delete | Hiermee verwijdert u een Azure Automation verbindings type-Asset |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/read | Hiermee wordt een Azure Automation verbindings type-Asset opgehaald |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/write | Hiermee maakt u een Azure Automation verbindings type-Asset |
> | Action | Microsoft.Automation/automationAccounts/credentials/delete | Hiermee wordt een Azure Automation-referentie-element verwijderd |
> | Action | Microsoft.Automation/automationAccounts/credentials/getCount/action | Hiermee wordt het aantal referenties gelezen |
> | Action | Microsoft.Automation/automationAccounts/credentials/read | Hiermee wordt een Azure Automation referentie-element opgehaald |
> | Action | Microsoft.Automation/automationAccounts/credentials/write | Hiermee wordt een Azure Automation referentie-Asset gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/delete | Hiermee verwijdert u een Azure Automation account |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Hiermee verwijdert u Hybrid Runbook Worker resources |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Hybrid Runbook Worker resources lezen |
> | Action | Microsoft.Automation/automationAccounts/jobs/output/read | Hiermee wordt de uitvoer van een taak opgehaald |
> | Action | Microsoft.Automation/automationAccounts/jobs/read | Hiermee wordt een Azure Automation-taak opgehaald |
> | Action | Microsoft.Automation/automationAccounts/jobs/resume/action | Hiermee wordt een Azure Automation taak hervat |
> | Action | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Hiermee wordt de inhoud van het Azure Automation runbook opgehaald op het moment dat de taak wordt uitgevoerd |
> | Action | Microsoft.Automation/automationAccounts/jobs/stop/action | Hiermee wordt een Azure Automation taak gestopt |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Hiermee wordt een Azure Automation taak stroom opgehaald |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Hiermee wordt een Azure Automation taak stroom opgehaald |
> | Action | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation taak onderbroken |
> | Action | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation taak |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/delete | Hiermee wordt een Azure Automation taak schema verwijderd |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/read | Hiermee wordt een Azure Automation taak planning opgehaald |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation taak schema |
> | Action | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Hiermee wordt de werk ruimte opgehaald die is gekoppeld aan het Automation-account |
> | Action | Microsoft.Automation/automationAccounts/listKeys/action | Hiermee worden de sleutels voor het Automation-account gelezen |
> | Action | Microsoft.Automation/automationAccounts/modules/activities/read | Hiermee worden Azure Automation activiteiten opgehaald |
> | Action | Microsoft.Automation/automationAccounts/modules/delete | Hiermee wordt een Azure Automation Power shell-module verwijderd |
> | Action | Microsoft.Automation/automationAccounts/modules/getCount/action | Hiermee wordt het aantal Power shell-modules in het Automation-account opgehaald |
> | Action | Microsoft.Automation/automationAccounts/modules/read | Hiermee wordt een Azure Automation Power shell-module opgehaald |
> | Action | Microsoft.Automation/automationAccounts/modules/write | Hiermee wordt een Azure Automation Power shell-module gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Hiermee wordt de knooppunt configuratie van een Azure Automation DSC verwijderd |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Hiermee wordt de inhoud van een Azure Automation DSC-knooppunt configuratie gelezen |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Hiermee wordt de knooppunt configuratie van een Azure Automation DSC gelezen |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Hiermee wordt de knooppunt configuratie van een Azure Automation DSC geschreven |
> | Action | Microsoft.Automation/automationAccounts/nodecounts/read | Hiermee leest u de samen vatting van het aantal knoop punten voor het opgegeven type |
> | Action | Microsoft.Automation/automationAccounts/nodes/delete | Verwijdert Azure Automation DSC-knoop punten |
> | Action | Microsoft.Automation/automationAccounts/nodes/read | Azure Automation DSC-knoop punten lezen |
> | Action | Micro soft. Automation/automationAccounts/knoop punten/rapporten/inhoud/lezen | Leest Azure Automation DSC-rapport inhoud |
> | Action | Micro soft. Automation/automationAccounts/knoop punten/rapporten/lezen | Azure Automation DSC-rapporten lezen |
> | Action | Microsoft.Automation/automationAccounts/nodes/write | Hiermee worden Azure Automation DSC-knoop punten gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Hiermee wordt Azure Automation TypeFields |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/delete | Hiermee verwijdert u een Azure Automation python 2-pakket |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/read | Hiermee wordt een Azure Automation python 2-pakket opgehaald |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/write | Hiermee wordt een Azure Automation python 2-pakket gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/delete | Hiermee verwijdert u een Azure Automation python 3-pakket |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/read | Hiermee wordt een Azure Automation python 3-pakket opgehaald |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/write | Hiermee wordt een Azure Automation python 3-pakket gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/read | Hiermee wordt een Azure Automation-account opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/content/read | Hiermee wordt de inhoud van een Azure Automation runbook opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/delete | Hiermee wordt een Azure Automation runbook verwijderd |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Hiermee wordt de inhoud van een Azure Automation-runbookconcept gemaakt |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Hiermee worden de resultaten van de concept bewerking Azure Automation runbook opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/read | Hiermee wordt een Azure Automation runbook-concept opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Hiermee wordt een test taak voor een concept van een Azure Automation runbook opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Hiermee wordt een test taak voor een Azure Automation-runbookconcept hervat |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Hiermee wordt een test taak voor een Azure Automation-runbookconcept gestopt |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Hiermee wordt een test taak voor een Azure Automation-runbookconcept onderbroken |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Hiermee wordt een test taak voor een concept van een Azure Automation runbook gemaakt |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Wijzigingen aan een Azure Automation-runbookconcept ongedaan maken |
> | Action | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Hiermee wordt het aantal Azure Automation runbooks opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/publish/action | Hiermee wordt een Azure Automation-runbookconcept gepubliceerd |
> | Action | Microsoft.Automation/automationAccounts/runbooks/read | Hiermee wordt een Azure Automation runbook opgehaald |
> | Action | Microsoft.Automation/automationAccounts/runbooks/write | Hiermee wordt een Azure Automation runbook gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/schedules/delete | Hiermee verwijdert u een Azure Automation schema-Asset |
> | Action | Microsoft.Automation/automationAccounts/schedules/getCount/action | Hiermee wordt het aantal Azure Automation planningen opgehaald |
> | Action | Microsoft.Automation/automationAccounts/schedules/read | Hiermee wordt een Azure Automation schema-element opgehaald |
> | Action | Microsoft.Automation/automationAccounts/schedules/write | Hiermee wordt een Azure Automation Schedule-Asset gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Hiermee wordt een Azure Automation Software-update configuratie verwijderd |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Hiermee wordt een Azure Automation configuratie van software-updates opgehaald |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Hiermee wordt Azure Automation Software-update configuratie gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/statistics/read | Hiermee worden Azure Automation statistieken opgehaald |
> | Action | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Een Azure Automation update-implementatie machine ophalen |
> | Action | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Hiermee wordt een Azure Automation patch taak voor update beheer opgehaald |
> | Action | Microsoft.Automation/automationAccounts/usages/read | Hiermee wordt het Azure Automation gebruik opgehaald |
> | Action | Microsoft.Automation/automationAccounts/variables/delete | Hiermee wordt een Azure Automation variabel activum verwijderd |
> | Action | Microsoft.Automation/automationAccounts/variables/read | Hiermee wordt een Azure Automation variabel activum gelezen |
> | Action | Microsoft.Automation/automationAccounts/variables/write | Hiermee wordt een Azure Automation variabele-Asset gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/watchers/delete | Een Azure Automation Watcher-taak verwijderen |
> | Action | Microsoft.Automation/automationAccounts/watchers/read | Hiermee wordt een Azure Automation Watcher-taak opgehaald |
> | Action | Microsoft.Automation/automationAccounts/watchers/start/action | Een Azure Automation Watcher-taak starten |
> | Action | Microsoft.Automation/automationAccounts/watchers/stop/action | Een Azure Automation Watcher-taak stoppen |
> | Action | Micro soft. Automation/automationAccounts/mijnen/stromen/lezen | Hiermee wordt een Azure Automation Watcher-taak stroom opgehaald |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Een Azure Automation Watcher-taak acties verwijderen |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Hiermee wordt een Azure Automation Watcher-taak acties opgehaald |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Een Azure Automation Watcher-taak acties maken |
> | Action | Microsoft.Automation/automationAccounts/watchers/write | Hiermee wordt een Azure Automation Watcher-taak gemaakt |
> | Action | Microsoft.Automation/automationAccounts/webhooks/action | Hiermee wordt een URI voor een Azure Automation-webhook gegenereerd |
> | Action | Microsoft.Automation/automationAccounts/webhooks/delete | Hiermee wordt een Azure Automation-webhook verwijderd  |
> | Action | Microsoft.Automation/automationAccounts/webhooks/read | Hiermee wordt een Azure Automation-webhook gelezen |
> | Action | Microsoft.Automation/automationAccounts/webhooks/write | Hiermee wordt een Azure Automation-webhook gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/automationAccounts/write | Hiermee wordt een Azure Automation-account gemaakt of bijgewerkt |
> | Action | Microsoft.Automation/operations/read | Hiermee worden beschik bare bewerkingen voor Azure Automation resources opgehaald |
> | Action | Microsoft.Automation/register/action | Hiermee wordt het abonnement geregistreerd bij Azure Automation |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/delete | B2C-mapresource verwijderen |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/read | B2C-mapresource weergeven |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/write | B2C-Directory resource maken of bijwerken |
> | Action | Microsoft.AzureActiveDirectory/b2ctenants/read | Hiermee worden alle B2C-tenants vermeld waarvan de gebruiker lid is |
> | Action | Microsoft.AzureActiveDirectory/operations/read | Alle API-bewerkingen lezen die beschikbaar zijn voor de resourceprovider Microsoft.AzureActiveDirectory |
> | Action | Microsoft.AzureActiveDirectory/register/action | Abonnement voor de resourceprovider Microsoft.AzureActiveDirectory registreren |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.AzureStack/Operations/read | Hiermee worden de eigenschappen van een resource provider bewerking opgehaald |
> | Action | Microsoft.AzureStack/register/action | Hiermee wordt een abonnement geregistreerd bij de resource provider micro soft. AzureStack |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Hiermee verwijdert u een Azure Stack klant abonnement |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/read | Hiermee worden de eigenschappen van een Azure Stack klant abonnement opgehaald |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/write | Hiermee wordt een Azure Stack klant abonnement gemaakt of bijgewerkt |
> | Action | Microsoft.AzureStack/registrations/delete | Hiermee wordt een Azure Stack registratie verwijderd |
> | Action | Microsoft.AzureStack/registrations/getActivationKey/action | Hiermee wordt de nieuwste Azure Stack activerings sleutel opgehaald |
> | Action | Microsoft.AzureStack/registrations/products/listDetails/action | Hiermee worden uitgebreide details opgehaald voor een Azure Stack Marketplace-product |
> | Action | Microsoft.AzureStack/registrations/products/read | Hiermee worden de eigenschappen van een Azure Stack Marketplace-product opgehaald |
> | Action | Micro soft. AzureStack/registraties/producten/uploadProductLog/actie | Status en tijds tempel van de product bewerkings Azure Stack van de Marketplace vastleggen |
> | Action | Microsoft.AzureStack/registrations/read | Hiermee worden de eigenschappen van een Azure Stack registratie opgehaald |
> | Action | Microsoft.AzureStack/registrations/write | Hiermee wordt een Azure Stack registratie gemaakt of bijgewerkt |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Batch/batchAccounts/applications/delete | Hiermee wordt een toepassing verwijderd |
> | Action | Microsoft.Batch/batchAccounts/applications/read | Een lijst met toepassingen of de eigenschappen van een toepassing ophalen |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Hiermee wordt een toepassings pakket geactiveerd |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/delete | Hiermee wordt een toepassings pakket verwijderd |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/read | Hiermee worden de eigenschappen van een toepassings pakket opgehaald |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/write | Hiermee wordt een nieuw toepassings pakket gemaakt of een bestaand toepassings pakket bijgewerkt |
> | Action | Microsoft.Batch/batchAccounts/applications/write | Hiermee wordt een nieuwe toepassing gemaakt of een bestaande toepassing bijgewerkt |
> | Action | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Hiermee worden de resultaten opgehaald van een langlopende certificaat bewerking voor een batch-account |
> | Action | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Hiermee wordt het verwijderen van een certificaat voor een batch-account geannuleerd |
> | Action | Microsoft.Batch/batchAccounts/certificates/delete | Hiermee wordt een certificaat uit een batch-account verwijderd |
> | Action | Microsoft.Batch/batchAccounts/certificates/read | Een lijst met certificaten voor een batch-account of de eigenschappen van een certificaat ophalen |
> | Action | Microsoft.Batch/batchAccounts/certificates/write | Hiermee maakt u een nieuw certificaat op een batch-account of werkt u een bestaand certificaat bij |
> | Action | Microsoft.Batch/batchAccounts/delete | Hiermee verwijdert u een batch-account |
> | DataAction | Micro soft. batch/batchAccounts/Jobs/verwijderen | Hiermee wordt een taak verwijderd uit een batch-account |
> | DataAction | Micro soft. batch/batchAccounts/Jobs/lezen | Een lijst met taken voor een batch-account of de eigenschappen van een taak ophalen |
> | DataAction | Micro soft. batch/batchAccounts/Jobs/schrijven | Hiermee maakt u een nieuwe taak voor een batch-account of werkt u een bestaande taak bij |
> | DataAction | Micro soft. batch/batchAccounts/jobSchedules/verwijderen | Hiermee verwijdert u een taak schema van een batch-account |
> | DataAction | Micro soft. batch/batchAccounts/jobSchedules/lezen | Een lijst met taak planningen voor een batch-account of de eigenschappen van een taak schema ophalen |
> | DataAction | Micro soft. batch/batchAccounts/jobSchedules/schrijven | Hiermee wordt een nieuwe taak planning gemaakt voor een batch-account of wordt een bestaande taak planning bijgewerkt |
> | Action | Microsoft.Batch/batchAccounts/listkeys/action | Een lijst met toegangs sleutels voor een batch-account |
> | Action | Microsoft.Batch/batchAccounts/operationResults/read | Hiermee worden de resultaten van een langlopende batch-account bewerking opgehaald |
> | Action | Microsoft.Batch/batchAccounts/poolOperationResults/read | Hiermee worden de resultaten van een langlopende pool bewerking voor een batch-account opgehaald |
> | Action | Microsoft.Batch/batchAccounts/pools/delete | Hiermee verwijdert u een groep van een batch-account |
> | Action | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Hiermee schakelt u automatisch schalen uit voor een batch-account groep |
> | Action | Microsoft.Batch/batchAccounts/pools/read | Een lijst met groepen in een batch-account of de eigenschappen van een groep ophalen |
> | Action | Microsoft.Batch/batchAccounts/pools/stopResize/action | Hiermee wordt een actieve bewerking voor het wijzigen van de grootte van een batch-account groep gestopt |
> | Action | Microsoft.Batch/batchAccounts/pools/write | Hiermee wordt een nieuwe pool gemaakt op een batch-account of een bestaande groep bijgewerkt |
> | Action | Microsoft.Batch/batchAccounts/read | Een lijst met batch-accounts of de eigenschappen van een batch-account |
> | Action | Microsoft.Batch/batchAccounts/regeneratekeys/action | Hiermee worden de toegangs sleutels voor een batch-account opnieuw gegenereerd |
> | Action | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Synchroniseert toegangs sleutels voor het automatische opslag account dat is geconfigureerd voor een batch-account |
> | Action | Microsoft.Batch/batchAccounts/write | Hiermee maakt u een nieuw batch-account of werkt u een bestaand batch-account bij |
> | Action | Microsoft.Batch/locations/accountOperationResults/read | Hiermee worden de resultaten van een langlopende batch-account bewerking opgehaald |
> | Action | Microsoft.Batch/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of de account naam geldig is en niet wordt gebruikt. |
> | Action | Microsoft.Batch/locations/quotas/read | Hiermee worden de batch quota's van het opgegeven abonnement in de opgegeven Azure-regio opgehaald |
> | Action | Microsoft.Batch/operations/read | Hiermee worden bewerkingen weer gegeven die beschikbaar zijn voor de resource provider van micro soft. batch |
> | Action | Microsoft.Batch/register/action | Hiermee wordt het abonnement voor de batch-resource provider geregistreerd en wordt het maken van batch-accounts ingeschakeld |
> | Action | Microsoft.Batch/unregister/action | Hiermee maakt u de registratie van het abonnement voor de batch-resource provider ongedaan voor het maken van batch-accounts. |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. facturering/billingAccounts/overeenkomsten/lezen |  |
> | Action | Micro soft. facturering/billingAccounts/billingPermissions/lezen |  |
> | Action | Micro soft. facturering/billingAccounts/billingProfiles/facturen/prijzen overzicht/downloaden/actie |  |
> | Action | Micro soft. facturering/billingAccounts/billingProfiles/invoiceSections/lezen |  |
> | Action | Micro soft. facturering/billingAccounts/billingProfiles/prijzen overzicht/downloaden/actie |  |
> | Action | Micro soft. facturering/billingAccounts/billingProfiles/lezen |  |
> | Action | Micro soft. facturering/billingAccounts/klanten/lezen |  |
> | Action | Microsoft.Billing/billingAccounts/departments/read |  |
> | Action | Microsoft.Billing/billingAccounts/enrollmentAccounts/read |  |
> | Action | Micro soft. facturering/billingAccounts/producten/lezen |  |
> | Action | Microsoft.Billing/billingAccounts/read |  |
> | Action | Microsoft.Billing/departments/read |  |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.BingMaps/mapApis/Delete | Verwijderbewerking |
> | Action | Microsoft.BingMaps/mapApis/listSecrets/action | Lijst met geheimen weergeven |
> | Action | Micro soft. BingMaps/mapApis/listSingleSignOnToken/Action | Verificatietoken voor eenmalige aanmelding voor de resource lezen |
> | Action | Microsoft.BingMaps/mapApis/Read | Leesbewerking |
> | Action | Microsoft.BingMaps/mapApis/regenerateKey/action | De sleutel opnieuw genereren |
> | Action | Microsoft.BingMaps/mapApis/Write | Schrijfbewerking |
> | Action | Microsoft.BingMaps/Operations/read | Beschrijving van de bewerking. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Blockchain/blockchainMembers/delete | Hiermee verwijdert u een bestaand Block Chain-lid. |
> | Action | Microsoft.Blockchain/blockchainMembers/listApiKeys/action | Hiermee worden bestaande Block Chain member API-sleutels opgehaald of weer gegeven. |
> | Action | Microsoft.Blockchain/blockchainMembers/read | Hiermee wordt een lijst met bestaande Block Chain-leden opgehaald of weer gegeven. |
> | DataAction | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Maakt verbinding met een trans actie-knoop punt van het block Chain-lid. |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/delete | Hiermee verwijdert u een bestaand Block Chain member Trans Action-knoop punt. |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/listApiKeys/action | Hiermee worden de bestaande API-sleutels van het block Chain member-trans actie knooppunt opgehaald of weer gegeven. |
> | Action | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Hiermee wordt een lijst met bestaande Block Chain-leden transactie knooppunten opgehaald of weer gegeven. |
> | Action | Micro soft. Block Chain/blockchainMembers/transactionNodes/write | Hiermee wordt een Block Chain-lid-transactie knooppunt gemaakt of bijgewerkt. |
> | Action | Microsoft.Blockchain/blockchainMembers/write | Hiermee wordt een Block Chain-lid gemaakt of bijgewerkt. |
> | Action | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Hiermee haalt u de bewerkings resultaten van Block Chain-leden op. |
> | Action | Microsoft.Blockchain/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of de resource naam geldig is en niet wordt gebruikt. |
> | Action | Microsoft.Blockchain/operations/read | Alle bewerkingen in de resource provider van micro soft Block Chain weer geven. |
> | Action | Microsoft.Blockchain/register/action | Hiermee wordt het abonnement voor de Block Chain-resource provider geregistreerd. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Alle blauwdrukartefacten lezen |
> | Action | Microsoft.Blueprint/blueprintAssignments/delete | Alle blauwdrukartefacten verwijderen |
> | Action | Microsoft.Blueprint/blueprintAssignments/read | Alle blauwdrukartefacten lezen |
> | Action | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Azure Blueprints-id van service-principal-object ophalen. |
> | Action | Microsoft.Blueprint/blueprintAssignments/write | Alle blauwdrukartefacten maken of bijwerken |
> | Action | Microsoft.Blueprint/blueprints/artifacts/delete | Alle blauwdrukartefacten verwijderen |
> | Action | Microsoft.Blueprint/blueprints/artifacts/read | Alle blauwdrukartefacten lezen |
> | Action | Microsoft.Blueprint/blueprints/artifacts/write | Alle blauwdrukartefacten maken of bijwerken |
> | Action | Microsoft.Blueprint/blueprints/delete | Alle blauwdrukken verwijderen |
> | Action | Microsoft.Blueprint/blueprints/read | Alle blauwdrukken lezen |
> | Action | Microsoft.Blueprint/blueprints/versions/artifacts/read | Alle blauwdrukartefacten lezen |
> | Action | Microsoft.Blueprint/blueprints/versions/delete | Alle blauwdrukken verwijderen |
> | Action | Microsoft.Blueprint/blueprints/versions/read | Alle blauwdrukken lezen |
> | Action | Microsoft.Blueprint/blueprints/versions/write | Blauwdrukken maken of bijwerken |
> | Action | Microsoft.Blueprint/blueprints/write | Blauwdrukken maken of bijwerken |
> | Action | Microsoft.Blueprint/register/action | Hiermee wordt de Azure Blueprints-resourceprovider geregistreerd |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.BotService/botServices/channels/delete | Een bot-service verwijderen |
> | Action | Microsoft.BotService/botServices/channels/read | Een bot-service lezen |
> | Action | Microsoft.BotService/botServices/channels/write | Een bot-service schrijven |
> | Action | Microsoft.BotService/botServices/connections/delete | Een bot-service verwijderen |
> | Action | Microsoft.BotService/botServices/connections/read | Een bot-service lezen |
> | Action | Microsoft.BotService/botServices/connections/write | Een bot-service schrijven |
> | Action | Microsoft.BotService/botServices/delete | Een bot-service verwijderen |
> | Action | Microsoft.BotService/botServices/read | Een bot-service lezen |
> | Action | Microsoft.BotService/botServices/write | Een bot-service schrijven |
> | Action | Microsoft.BotService/locations/operationresults/read | De status van een asynchrone bewerking lezen |
> | Action | Microsoft.BotService/Operations/read | De bewerkingen voor alle resource typen lezen |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Cache/checknameavailability/action | Hiermee wordt gecontroleerd of een naam beschikbaar is voor gebruik met een nieuwe Redis Cache |
> | Action | Microsoft.Cache/locations/operationresults/read | Hiermee wordt het resultaat van een langdurige bewerking opgehaald waarvoor de header 'Location' eerder is geretourneerd naar de client |
> | Action | Microsoft.Cache/operations/read | Hiermee worden de bewerkingen weergegeven die door de provider Microsoft.Cache worden ondersteund. |
> | Action | Microsoft.Cache/redis/delete | Volledige Redis-cache verwijderen |
> | Action | Microsoft.Cache/redis/export/action | Redis-gegevens exporteren naar vooraf ingestelde opslag-blobs in een opgegeven indeling |
> | Action | Microsoft.Cache/redis/firewallRules/delete | IP-firewallregels voor een Redis Cache verwijderen |
> | Action | Microsoft.Cache/redis/firewallRules/read | De IP-firewallregels voor een Redis Cache ophalen |
> | Action | Microsoft.Cache/redis/firewallRules/write | De IP-firewallregels voor een Redis Cache bewerken |
> | Action | Microsoft.Cache/redis/forceReboot/action | Een cache-instantie geforceerd opnieuw opstarten met mogelijk gegevensverlies. |
> | Action | Microsoft.Cache/redis/import/action | Gegevens met een opgegeven indeling in Redis importeren vanuit meerdere blobs |
> | Action | Microsoft.Cache/redis/linkedservers/delete | Gekoppelde server uit een Redis-cache verwijderen |
> | Action | Microsoft.Cache/redis/linkedservers/read | Gekoppelde servers die zijn gekoppeld aan een Redis-cache ophalen. |
> | Action | Microsoft.Cache/redis/linkedservers/write | Gekoppelde server aan een Redis-cache toevoegen |
> | Action | Microsoft.Cache/redis/listKeys/action | De waarde van de toegangssleutels voor Redis-cache weergeven in de beheerportal |
> | Action | Microsoft.Cache/redis/listUpgradeNotifications/read | De nieuwste upgrademeldingen weergeven voor de cachetenant. |
> | Action | Microsoft.Cache/redis/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens voor een Redis-cache opgehaald |
> | Action | Microsoft.Cache/redis/patchSchedules/delete | Het schema voor het toepassen van patches voor een Redis Cache verwijderen |
> | Action | Microsoft.Cache/redis/patchSchedules/read | Hiermee wordt het schema voor het toepassen van patches voor een Redis Cache opgehaald |
> | Action | Microsoft.Cache/redis/patchSchedules/write | Het schema voor het toepassen van patches voor een Redis Cache aanpassen |
> | Action | Microsoft.Cache/redis/read | De instellingen en configuratie van Redis-cache weergeven in de beheerportal |
> | Action | Microsoft.Cache/redis/regenerateKey/action | De waarde van de toegangssleutels voor Redis-cache wijzigen in de beheerportal |
> | Action | Microsoft.Cache/redis/start/action | Start een cache-instantie. |
> | Action | Microsoft.Cache/redis/stop/action | Stop een cache-instantie. |
> | Action | Microsoft.Cache/redis/write | De instellingen en configuratie van Redis-cache wijzigen in de beheerportal |
> | Action | Microsoft.Cache/register/action | Hiermee wordt de resourceprovider Microsoft.Cache geregistreerd bij een abonnement |
> | Action | Microsoft.Cache/unregister/action | Hiermee wordt de registratie van de resourceprovider Microsoft.Cache bij een abonnement ongedaan gemaakt |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Capacity/appliedreservations/read | Alle reserve ringen lezen |
> | Action | Microsoft.Capacity/calculateexchange/action | Hiermee worden de Exchange-hoeveelheid en de prijs van nieuwe aankoop berekend en worden er beleids fouten geretourneerd. |
> | Action | Microsoft.Capacity/calculateprice/action | De reserverings prijs berekenen |
> | Action | Micro soft. capacity/catalogi/lezen | Catalogus van reserve ring lezen |
> | Action | Microsoft.Capacity/checkoffers/action | Alle abonnements aanbiedingen controleren |
> | Action | Microsoft.Capacity/checkscopes/action | Een abonnement controleren |
> | Action | Microsoft.Capacity/commercialreservationorders/read | Reserverings orders ophalen die zijn gemaakt in een Tenant |
> | Action | Microsoft.Capacity/exchange/action | Alle reserve ringen uitwisselen |
> | Action | Microsoft.Capacity/operations/read | Een wille keurige bewerking lezen |
> | Action | Micro soft. capacity/REGI ster/actie | Hiermee wordt de resource provider voor capaciteit geregistreerd en wordt het maken van capaciteits resources ingeschakeld. |
> | Action | Microsoft.Capacity/reservationorders/action | Een reserve ring bijwerken |
> | Action | Microsoft.Capacity/reservationorders/availablescopes/action | Beschik bare bereik zoeken |
> | Action | Microsoft.Capacity/reservationorders/calculaterefund/action | Hiermee worden de terugbetalings hoeveelheid en de prijs van nieuwe aankoop berekend en worden er beleids fouten geretourneerd. |
> | Action | Microsoft.Capacity/reservationorders/delete | Een reserve ring verwijderen |
> | Action | Microsoft.Capacity/reservationorders/merge/action | Een reserve ring samen voegen |
> | Action | Microsoft.Capacity/reservationorders/read | Alle reserve ringen lezen |
> | Action | Microsoft.Capacity/reservationorders/reservations/action | Een reserve ring bijwerken |
> | Action | Microsoft.Capacity/reservationorders/reservations/delete | Een reserve ring verwijderen |
> | Action | Microsoft.Capacity/reservationorders/reservations/read | Alle reserve ringen lezen |
> | Action | Microsoft.Capacity/reservationorders/reservations/revisions/read | Alle reserve ringen lezen |
> | Action | Microsoft.Capacity/reservationorders/reservations/write | Een reserve ring maken |
> | Action | Microsoft.Capacity/reservationorders/return/action | Reserve ring retour neren |
> | Action | Microsoft.Capacity/reservationorders/split/action | Alle reserve ringen splitsen |
> | Action | Microsoft.Capacity/reservationorders/swap/action | Alle reserve ringen omwisselen |
> | Action | Microsoft.Capacity/reservationorders/write | Een reserve ring maken |
> | Action | Microsoft.Capacity/tenants/register/action | Een Tenant registreren |
> | Action | Micro soft. capacity/registratie/actie | De registratie van een Tenant ongedaan maken |
> | Action | Microsoft.Capacity/validatereservationorder/action | Een reserve ring valideren |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Action | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Action | Micro soft. CDN/edgenodes/verwijderen |  |
> | Action | Microsoft.Cdn/edgenodes/read |  |
> | Action | Micro soft. CDN/edgenodes/schrijven |  |
> | Action | Microsoft.Cdn/operationresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Action | Microsoft.Cdn/operationresults/read |  |
> | Action | Microsoft.Cdn/operationresults/write |  |
> | Action | Microsoft.Cdn/operations/read |  |
> | Action | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/write |  |
> | Action | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/profiles/read |  |
> | Action | Microsoft.Cdn/profiles/write |  |
> | Action | Microsoft.Cdn/register/action | Hiermee wordt het abonnement voor de CDN-resourceprovider geregistreerd en wordt het maken van CDN-profielen mogelijk. |
> | Action | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Een bestaand certificaat verwijderen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | De lijst met certificaten ophalen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Een nieuw certificaat toevoegen of een bestaande bijwerken |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Delete | Een bestaande AppServiceCertificate verwijderen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Read | De lijst met CertificateOrders ophalen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Een bestaande certificaat bestelling opnieuw uitgeven |
> | Action | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Een bestaande certificaat bestelling vernieuwen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | E-mail van certificaat opnieuw verzenden |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | E-mail met aanvraag opnieuw verzenden naar een ander e-mail adres |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Site zegel ophalen voor een uitgegeven App Service Certificate |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | De lijst met certificaat acties ophalen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | E-mail geschiedenis van certificaat ophalen |
> | Action | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Domeineigendom controleren |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Write | Een nieuwe certificaat bestelling toevoegen of een bestaand item bijwerken |
> | Action | Microsoft.CertificateRegistration/operations/Read | Alle bewerkingen van de registratie van het app service-certificaat weer geven |
> | Action | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Service-Principal inrichten voor service-app-Principal |
> | Action | Microsoft.CertificateRegistration/register/action | De resource provider van micro soft-certificaten registreren voor het abonnement |
> | Action | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Het certificaat aankoop object valideren zonder dit te verzenden |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ClassicCompute/capabilities/read | Hiermee worden de mogelijkheden weergegeven |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Hiermee wordt gecontroleerd of een bepaald domeinnaam beschikbaar is. |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Hiermee wordt de beschikbaarheid van een gegeven domeinnaam opgehaald. |
> | Action | Microsoft.ClassicCompute/domainNames/active/write | Hiermee wordt de naam ingesteld van het actieve domein. |
> | Action | Microsoft.ClassicCompute/domainNames/availabilitySets/read | De beschikbaarheidsset voor de resource wordt weergegeven. |
> | Action | Microsoft.ClassicCompute/domainNames/capabilities/read | Geeft de mogelijkheden voor domeinnaamgeving weer |
> | Action | Microsoft.ClassicCompute/domainNames/delete | De domeinnamen voor resources verwijderen. |
> | Action | Micro soft. ClassicCompute/domainName/deploymentslots/lezen | Hiermee worden de implementatiesleuven weergegeven. |
> | Action | Micro soft. ClassicCompute/domainNamees/deploymentslots/roles/lezen | Rol ophalen op implementatiesite van domeinnaam |
> | Action | Micro soft. ClassicCompute/domainNamees/deploymentslots/roles/roleinstances/lezen | Rolexemplaar ophalen voor rol op implementatiesite van domeinnaam |
> | Action | Micro soft. ClassicCompute/domein naam/deploymentslots/status/lezen | De status van de implementatiesite ophalen. |
> | Action | Micro soft. ClassicCompute/domein naam/deploymentslots/status/schrijven | De status van de implementatiesite toevoegen. |
> | Action | Micro soft. ClassicCompute/domainNamees/deploymentslots/upgrade Domain/lezen | Upgradedomein ophalen voor implementatiesite van domeinnaam |
> | Action | Micro soft. ClassicCompute/domein-en deploymentslots/upgrade Domain/schrijven | Het upgradedomein voor implementatiesite op domeinnaam |
> | Action | Micro soft. ClassicCompute/domainName/deploymentslots/schrijven | Hiermee wordt de implementatie gemaakt of bijgewerkt. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/delete | De domeinnaamextensies verwijderen. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen. |
> | Action | Micro soft. ClassicCompute/domein naam/extensies/uitbrei dingen/lezen | Hiermee kunt u de domeinnaamextensies retourneren. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/write | De domeinnaamextensies toevoegen. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Een nieuwe interne taakverdeling verwijderen. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de interne load balancers van domeinnamen gelezen. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | De interne load balancers worden opgehaald. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Hiermee wordt een nieuwe interne taakverdeling gemaakt. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de eindpuntsets voor de taakverdeling van de domeinnamen gelezen. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | De eindpuntsets met gelijke taakverdeling ophalen. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | De eindpuntset met gelijke taakverdeling toevoegen. |
> | Action | Micro soft. ClassicCompute/domainName/operationstatuses/lezen | De bewerkingsstatus van de domeinnaam ophalen. |
> | Action | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen. |
> | Action | Microsoft.ClassicCompute/domainNames/read | De domeinnamen voor resources retourneren. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | De gebruikte servicecertificaten verwijderen. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de servicecertificaten van domeinnamen gelezen. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Hiermee worden de gebruikte servicecertificaten geretourneerd. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Hiermee worden de gebruikte servicecertificaten toegevoegd of gewijzigd. |
> | Action | Micro soft. ClassicCompute/Domeins/sleuven/abortMigration/actie | Hiermee wordt de migratie van een implementatiesite afgebroken. |
> | Action | Micro soft. ClassicCompute/Domeins/sleuven/commitMigration/actie | Hiermee wordt de migratie van een implementatiesite doorgevoerd. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/delete | Hiermee wordt een bepaald implementatiesleuf verwijderd. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamsleuven gelezen. |
> | Action | Micro soft. ClassicCompute/Domeins/sleuven/prepareMigration/actie | Hiermee wordt de migratie van een implementatiesite voorbereid. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/lezen | Hiermee worden de implementatiesleuven weergegeven. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | De extensieverwijzing voor de implementatiesite verwijderen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de extensieverwijzing van de rollen voor domeinnaamsleuven gelezen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Hiermee kunt u de extensieverwijzing voor de implementatiesiterol retourneren. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Hiermee kunt u de extensieverwijzing voor de implementatiesiterol toevoegen of aanpassen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | De definitie van de metrische gegevens voor de rol voor de domeinnaam ophalen. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | De metrische gegevens van de rol voor de domeinnaam ophalen. |
> | Action | Micro soft. ClassicCompute/domainNamees/sleuven/rollen/operationstatuses/lezen | De bewerkingsstatus voor de rol van de domeinnaamsleuf ophalen. |
> | Action | Micro soft. ClassicCompute/domainNamees/sleuven/rollen/providers/micro soft. Insights/diagnosticSettings/lezen | Hiermee worden de diagnostische instellingen opgehaald. |
> | Action | Micro soft. ClassicCompute/Domeins/sleuven/rollen/providers/micro soft. Insights/diagnosticSettings/schrijven | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/rollen/lezen | Hiermee wordt de rol van de implementatiesleuf opgehaald. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Hiermee worden bestanden gedownload via een verbinding met een extern bureaublad in de rolinstantie voor de rol van de domeinnaamsleuf. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de rolinstantie voor de rol van de domeinnaamsleuf opgehaald. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Hiermee wordt het rolexemplaar opgehaald. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Hiermee wordt de rolinstantie herbouwd. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Hiermee wordt de installatiekopie van het rolexemplaar opnieuw toegepast. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Hiermee worden rolinstanties opnieuw gestart. |
> | Action | Micro soft. ClassicCompute/domainNamees/sleuven/rollen/sku's/lezen | De SKU voor de rol van de implementatiesleuf ophalen. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/rollen/schrijven | De rol voor de implementatiesite toevoegen. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/starten/actie | Hiermee wordt een implementatiesleuf gestart. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/status/starten/schrijven | Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestopt. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/status/stoppen/schrijven | Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestart. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/stoppen/actie | Hiermee wordt de implementatiesleuf tijdelijk stopgezet. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/upgrade Domain/schrijven | Hiermee wordt stap voor stap een domeinupgrade uitgevoerd. |
> | Action | Micro soft. ClassicCompute/Domeins/sleuven/validateMigration/actie | Hiermee wordt de migratie van een implementatiesite gevalideerd. |
> | Action | Micro soft. ClassicCompute/domein naam/sleuven/schrijven | Hiermee wordt de implementatie gemaakt of bijgewerkt. |
> | Action | Microsoft.ClassicCompute/domainNames/swap/action | Hiermee wordt de staging-site gewisseld naar de productiesite. |
> | Action | Microsoft.ClassicCompute/domainNames/write | Hiermee worden de domeinnamen voor resources toegevoegd of gewijzigd. |
> | Action | Microsoft.ClassicCompute/moveSubscriptionResources/action | Verplaats alle klassieke resources naar een ander abonnement. |
> | Action | Microsoft.ClassicCompute/operatingSystemFamilies/read | Hiermee wordt een lijst met de gastbesturingssysteemfamilies gemaakt die beschikbaar zijn in Microsoft Azure. Ook worden de besturingssysteemversies weergegeven die beschikbaar zijn voor elke serie. |
> | Action | Microsoft.ClassicCompute/operatingSystems/read | Hiermee wordt een lijst van de versies van gastbesturingssystemen gemaakt die momenteel beschikbaar zijn in Microsoft Azure. |
> | Action | Microsoft.ClassicCompute/operations/read | Hiermee haalt u de lijst met bewerkingen op. |
> | Action | Microsoft.ClassicCompute/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Action | Microsoft.ClassicCompute/quotas/read | Hiermee worden de quota voor het abonnement opgehaald. |
> | Action | Microsoft.ClassicCompute/register/action | Registreren bij klassieke berekening |
> | Action | Microsoft.ClassicCompute/resourceTypes/skus/read | Haalt de SKU-lijst op voor ondersteunde resourcetypen. |
> | Action | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | De beschikbaarheid van het abonnement voor de klassieke verplaatsingsbewerking valideren. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de virtuele machine is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de virtuele machine is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de virtuele machine is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Hiermee worden de mogelijk asynchrone bewerkingen opgehaald |
> | Action | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Hiermee wordt een gegevensschijf gekoppeld aan een virtuele machine. |
> | Action | Microsoft.ClassicCompute/virtualMachines/capture/action | Een virtuele machine vastleggen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/delete | Hiermee worden virtuele machines verwijderd. |
> | Action | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Hiermee wordt een gegevensschijf losgekoppeld van een virtuele machine. |
> | Action | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Diagnostische instellingen van virtuele machine ophalen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/disks/read | Hiermee wordt een lijst met gegevens schijven opgehaald |
> | Action | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Hiermee wordt het RDP-bestand voor de virtuele machine gedownload. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van extensies van virtuele machines gelezen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/read | Hiermee wordt de extensie van de virtuele machine opgehaald. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/write | Hiermee wordt de extensie voor de virtuele machine ingesteld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | De definitie van de metrische gegevens voor de virtuele machine ophalen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metrics/read | Hiermee worden de metrische gegevens opgehaald. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de netwerkinterface is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de netwerkinterface is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de netwerkinterface is gekoppeld. |
> | Action | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines gelezen. |
> | Action | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Hiermee wordt onderhoud van de virtuele machine uitgevoerd. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen opgehaald. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Action | Microsoft.ClassicCompute/virtualMachines/read | Hiermee wordt een lijst met virtuele machines opgehaald. |
> | Action | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Hiermee wordt de virtuele machine opnieuw geïmplementeerd. |
> | Action | Microsoft.ClassicCompute/virtualMachines/restart/action | Hiermee worden virtuele machines opnieuw gestart. |
> | Action | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Hiermee wordt de virtuele machine afgesloten. |
> | Action | Microsoft.ClassicCompute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart. |
> | Action | Microsoft.ClassicCompute/virtualMachines/stop/action | Hiermee wordt de virtuele machine gestopt. |
> | Action | Microsoft.ClassicCompute/virtualMachines/write | Hiermee worden virtuele machines toegevoegd of gewijzigd. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | De bewerkingsstatus van een ExpressRoute-kruisverbinding ophalen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Peering van ExpressRoute-kruisverbinding verwijderen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/operationstatuses/read | De bewerkingsstatus van een ExpressRoute-kruisverbindingspeering ophalen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Peering van ExpressRoute-kruisverbinding ophalen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Peering van ExpressRoute-kruisverbinding toevoegen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/read | ExpressRoute-kruisverbindingen ophalen. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/write | ExpressRoute-kruisverbindingen toevoegen. |
> | Action | Micro soft. ClassicNetwork/gatewaySupportedDevices/lezen | De lijst met ondersteunde apparaten wordt opgehaald. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep gelezen. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen voor netwerkbeveiligingsgroepen opgehaald |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden de diagnostische instellingen voor netwerk beveiligings groepen gemaakt of bijgewerkt, deze bewerking wordt aangevuld door de Insights-resource provider. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de gebeurtenissen voor de netwerkbeveiligingsgroep opgehaald |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Hiermee wordt de beveiligingsregel verwijderd. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de beveiligingsregels voor de netwerkbeveiligingsgroep gelezen. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Hiermee wordt de beveiligingsregel opgehaald. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Hiermee wordt een beveiligingsregel toegevoegd of bijgewerkt. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/write | Hiermee wordt een nieuwe netwerkbeveiligingsgroep toegevoegd. |
> | Action | Microsoft.ClassicNetwork/operations/read | Klassieke netwerkbewerkingen ophalen. |
> | Action | Microsoft.ClassicNetwork/quotas/read | Hiermee worden de quota voor het abonnement opgehaald. |
> | Action | Microsoft.ClassicNetwork/register/action | Registreren bij klassiek netwerk |
> | Action | Microsoft.ClassicNetwork/reservedIps/delete | Hiermee wordt een gereserveerd IP verwijderd. |
> | Action | Microsoft.ClassicNetwork/reservedIps/join/action | Deelnemen aan een gereserveerde IP |
> | Action | Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde IP koppelen |
> | Action | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de gereserveerde IP's gelezen. |
> | Action | Microsoft.ClassicNetwork/reservedIps/read | Hiermee worden de gereserveerde IP's opgehaald. |
> | Action | Microsoft.ClassicNetwork/reservedIps/write | Een nieuw gereserveerd IP toevoegen |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Hiermee wordt de migratie van een virtueel netwerk afgebroken |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/capabilities/lezen | Hiermee worden de mogelijkheden weergegeven |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Hiermee wordt de beschikbaarheid van een bepaald IP-adres in een virtueel netwerk gecontroleerd. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Hiermee wordt de migratie van een virtueel netwerk doorgevoerd |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/verwijderen | Hiermee wordt het virtuele netwerk verwijderd. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/verwijderen | Hiermee wordt de intrekking van een clientcertificaat ongedaan gemaakt. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/lezen | De ingetrokken clientcertificaten lezen. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/schrijven | Hiermee wordt een clientcertificaat ingetrokken. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Hiermee wordt het clientcertificaat voor de gateway van het virtuele netwerk verwijderd. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/downloaden/actie | Hiermee wordt een certificaat gedownload op basis van miniatuur. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Het certificaatpakket van de virtuele netwerkgateway wordt weergegeven. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Clienthoofdcertificaten zoeken. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Hiermee wordt een nieuw clienthoofdcertificaat geüpload. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/Connections/Connect/Action | Hiermee wordt een gatewayverbinding tussen sites ingesteld. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Hiermee wordt een gatewayverbinding tussen sites verbroken. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/verbindingen/lezen | Hiermee wordt de lijst met verbindingen opgehaald. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/verbindingen/testen/actie | Hiermee wordt een gatewayverbinding tussen sites getest. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/verwijderen | De gateway van het virtuele netwerk wordt verwijderd. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/actie | Het configuratiescript voor het apparaat wordt gedownload. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/actie | Hiermee worden de diagnotische gegevens van de gateway gedownload. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/actie | Hiermee wordt de circuitservicesleutel opgehaald. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/listPackage/actie | Het gatewaypakket voor het virtuele netwerk wordt weergegeven. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerkgateways gelezen. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/pakketten/lezen | Het gatewaypakket voor het virtuele netwerk wordt opgehaald. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/lezen | De gateways voor het virtuele netwerk worden opgehaald. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/startDiagnostics/actie | Diagnostische gegevens voor de gateway van het virtuele netwerk starten. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/actie | Hiermee stopt u de diagnostische gegevens voor de virtuele netwerk gateway. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/gateways/schrijven | Een gateway voor het virtuele netwerk wordt toegevoegd. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/deelname/actie | Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerken gelezen. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/peer/Action | Hiermee wordt een virtueel netwerk via peercommunicatie gebruikt met een ander virtueel netwerk. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/prepareMigration/Action | Hiermee wordt de migratie van een virtueel netwerk voorbereid |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/lezen | Het virtuele netwerk wordt opgehaald. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | De proxy voor de peering van het externe virtuele netwerk. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Hiermee wordt de proxy voor de peering van het externe virtuele netwerk opgehaald. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Hiermee wordt de proxy voor de peering van het externe virtuele netwerk toegevoegd. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan het subnet is gekoppeld. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkings status gelezen van het subnet van het virtuele netwerk dat aan de netwerk beveiligings groep is gekoppeld. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan het subnet is gekoppeld. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/subnetten/associatedNetworkSecurityGroups/schrijven | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan het subnet is gekoppeld. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/validateMigration/Action | Hiermee wordt de migratie van een virtueel netwerk gevalideerd |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Hiermee wordt de peering voor het virtuele netwerk opgehaald. |
> | Action | Micro soft. ClassicNetwork/virtualNetworks/schrijven | Een nieuw virtueel netwerk wordt toegevoegd. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ClassicStorage/capabilities/lezen | Hiermee worden de mogelijkheden weergegeven |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Hiermee wordt gecontroleerd of een opslagaccount beschikbaar is. |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | De beschikbaarheid van een opslagaccount ophalen. |
> | Action | Microsoft.ClassicStorage/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Action | Micro soft. ClassicStorage/images/operationstatuses/lezen | Hiermee wordt de bewerkingsstatus van een installatiekopie opgehaald. |
> | Action | Microsoft.ClassicStorage/images/read | Hiermee wordt de afbeelding geretourneerd. |
> | Action | Microsoft.ClassicStorage/operations/read | Hiermee worden klassieke opslagbewerkingen opgehaald |
> | Action | Microsoft.ClassicStorage/osImages/read | De installatiekopie van het besturingssysteem herstellen. |
> | Action | Microsoft.ClassicStorage/osPlatformImages/read | Hiermee wordt de platforminstallatiekopie van het besturingssysteem opgehaald. |
> | Action | Microsoft.ClassicStorage/publicImages/read | Hiermee wordt de openbare installatiekopie voor de virtuele machine opgehaald. |
> | Action | Micro soft. ClassicStorage/quota's/lezen | Hiermee worden de quota voor het abonnement opgehaald. |
> | Action | Micro soft. ClassicStorage/REGI ster/actie | Registreren bij klassieke opslag |
> | Action | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Hiermee wordt de migratie van een opslagaccount afgebroken. |
> | Action | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Hiermee wordt de migratie van een opslagaccount doorgevoerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/delete | Hiermee wordt het opslagaccount verwijderd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/delete | Hiermee wordt een opgegeven opslagaccountschijf verwijderd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/write | Hiermee wordt een opslagaccountschijf toegevoegd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/delete | Hiermee verwijdert u een bepaalde installatie kopie van het opslag account. Keur. Micro soft. ClassicStorage/Storage accounts/vmImages gebruiken |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Hiermee wordt de bewerkingsstatus van de opslagaccountinstallatiekopie geretourneerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/read | Hiermee wordt de installatie kopie van het opslag account geretourneerd. Keur. Micro soft. ClassicStorage/Storage accounts/vmImages gebruiken |
> | Action | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Action | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Hiermee wordt de betreffende installatiekopie van het besturingssysteem van een opslagaccount verwijderd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/read | Hiermee wordt de installatiekopie van het besturingssysteem van een opslagaccount hersteld. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/write | Hiermee wordt de betreffende installatiekopie van het besturingssysteem van een opslagaccount toegevoegd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Hiermee wordt de migratie van een opslagaccount voorbereid. |
> | Action | Micro soft. ClassicStorage/Storage accounts/providers/micro soft. Insights/diagnosticSettings/lezen | Hiermee worden de diagnostische instellingen opgehaald. |
> | Action | Micro soft. ClassicStorage/Storage accounts/providers/micro soft. Insights/diagnosticSettings/schrijven | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Action | Micro soft. ClassicStorage/Storage accounts/providers/micro soft. Insights/metricDefinitions/lezen | Hiermee worden de metrische definities opgehaald. |
> | Action | Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | De bestaande toegangssleutels voor het opslagaccount worden geregenereerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Hiermee worden de diagnostische instellingen opgehaald. |
> | Action | Micro soft. ClassicStorage/Storage accounts/Services/diagnosticSettings/schrijven | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Hiermee worden de metrische gegevens opgehaald. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/read | De beschikbare services ophalen. |
> | Action | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Hiermee wordt de migratie van een opslagaccount gevalideerd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Hiermee wordt de betreffende installatiekopie van virtuele machine verwijderd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Hiermee wordt de bewerkingsstatus van de installatiekopie van een gegeven virtuele machine opgehaald. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Hiermee wordt de installatiekopie van de virtuele machine hersteld. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Hiermee wordt de betreffende installatiekopie van virtuele machine toegevoegd. |
> | Action | Microsoft.ClassicStorage/storageAccounts/write | Hiermee wordt een nieuw opslagaccount toegevoegd. |
> | Action | Microsoft.ClassicStorage/vmImages/read | Hiermee wordt een lijst met installatiekopieën van virtuele machines weergegeven. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/analyze/action | Met deze bewerking wordt een uitgebreide set visuele functies geëxtraheerd op basis van de inhoud van de installatie kopie.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/areaofinterest/action | Met deze bewerking wordt een omsluitend kader geretourneerd rond het belangrijkste gedeelte van de afbeelding. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/describe/action | Met deze bewerking wordt een beschrijving van een afbeelding in een door de mens lees bare taal gegenereerd met volledige zinnen.<br> De beschrijving is gebaseerd op een verzameling inhouds Tags, die ook door de bewerking worden geretourneerd.<br>Voor elke afbeelding kan meer dan één beschrijving worden gegenereerd.<br> Beschrijvingen worden geordend op basis van hun betrouwbaarheids Score.<br>Alle beschrijvingen zijn in het Engels. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/detect/action | Met deze bewerking wordt object detectie uitgevoerd voor de opgegeven installatie kopie.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/generatethumbnail/action | Met deze bewerking wordt een miniatuur afbeelding gegenereerd met de opgegeven breedte en hoogte van de gebruiker.<br> Standaard analyseert de service de installatie kopie, identificeert het rente gebied (ROI) en genereert slimme bijsnijd coördinaten op basis van het ROI.<br> Slim bijsnijden helpt bij het opgeven van een hoogte-breedte verhouding die afwijkt van die van de invoer installatie kopie |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/analyze/action | Met deze bewerking wordt inhoud in een installatie kopie herkend door een domein-specifiek model toe te passen.<br> De lijst met domein-specifieke modellen die worden ondersteund door de Computer Vision-API kan worden opgehaald met behulp van de/models GET-aanvraag.<br> Op dit moment biedt de API de volgende gebruikersspecifieke modellen: beroemdheden, bezienswaardigheden. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/models/read | Met deze bewerking wordt de lijst met domein-specifieke modellen geretourneerd die worden ondersteund door de Computer Vision-API.  De API ondersteunt momenteel de volgende gebruikersspecifieke modellen: beroemdheden Recognizer, oriëntatie punt Recognizer. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/ocr/action | Optische teken herkenning (OCR) detecteert tekst in een afbeelding en extraheert de herkende tekens in een door een machine bruikbare teken stroom.    |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/recognizetext/action | Gebruik deze interface om het resultaat van een Tekst herkennen bewerking op te halen. Wanneer u de Tekst herkennen-interface gebruikt, bevat het antwoord een veld met de naam bewerking locatie. Het veld bewerkings locatie bevat de URL die u moet gebruiken voor de bewerking resultaat van het ophalen van Tekst herkennen. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/tag/action | Met deze bewerking wordt een lijst met woorden, of tags, gegenereerd die relevant zijn voor de inhoud van de geleverde afbeelding.<br>De Computer Vision-API kan labels retour neren op basis van objecten, in de praktijk, in achtergronden of in afbeeldingen gevonden.<br>In tegens telling tot categorieën worden tags niet ingedeeld op basis van een hiërarchisch classificatie systeem, maar komen ze overeen met de inhoud van de afbeelding.<br>Tags kunnen hints bevatten om Verwar ring te voor komen of om context te bieden, bijvoorbeeld de tag "cello", kan worden gecombineerd met de hint "musical instrument".<br>Alle tags zijn in het Engels. |
> | DataAction | Microsoft.CognitiveServices/accounts/ComputerVision/textoperations/read | Deze interface wordt gebruikt om het resultaat van de tekst bewerking te herkennen. De URL naar deze interface moet worden opgehaald uit het veld bewerkings <b>locatie</b> dat is geretourneerd door de tekst herkennen-interface. |
> | Action | Microsoft.CognitiveServices/accounts/delete | API-accounts verwijderen |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/detect/action | Detecteren van menselijke gezichten in een afbeelding, gezichts rechthoeken retour neren en optioneel met faceIds, bezienswaardigheden en kenmerken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/delete | Een opgegeven gezichts lijst verwijderen. De gerelateerde installatie kopieën in de lijst met het gezichts materiaal worden ook verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/facelists/persistedfaces/verwijderen | Een gezicht verwijderen uit een gezichts lijst door opgegeven faceListId en persisitedFaceId. De afbeelding voor het gerelateerde gezicht wordt ook verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/write | Voeg een gezicht toe aan een opgegeven gezichts lijst van Maxi maal 1.000 gezichten. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/read | Haal de faceListId, naam, User Data en gezichten van een gezichts lijst op in de lijst met gezicht. Lijst met gezichts lijsten ' faceListId, name en User Data. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/write | Maak een lege face-lijst met door de gebruiker opgegeven faceListId, een naam en een optioneel User Data. Maxi maal 64 gezichts lijsten zijn toegestane Update gegevens van een gezichts lijst, inclusief naam en User Data. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/findsimilars/action | Aan de hand van het voor beeld van een query op faceId kunt u zoeken naar de vergelijk bare gezichten vanuit een faceId-matrix, een gezichts lijst of een grote gezichts lijst. faceId |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/group/action | Verdeel kandidaten in groepen op basis van vergelijk bare gezicht. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/identify/action | 1-op-veel-identificatie voor het vinden van de dichtstbijzijnde treffers van een persoons groep of een grote persoons groep. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/verwijderen | Een opgegeven grote gezichts lijst verwijderen. De gerelateerde gezichts afbeeldingen in de lijst grote gezicht worden ook verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/persistedfaces/verwijderen | Een gezicht verwijderen uit een grote gezichts lijst door opgegeven largeFaceListId en persisitedFaceId. De afbeelding voor het gerelateerde gezicht wordt ook verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/persistedfaces/lezen | Haal persistente gezicht op in grote gezichts lijst door largeFaceListId en persistedFaceId. Een lijst met gezichten ' persistedFaceId en User Data ' in een opgegeven lijst met grote vlakken. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/persistedfaces/schrijven | Voeg een gezicht toe aan een opgegeven lijst met grote vlakken, tot 1.000.000 gezichten. Het veld User Data van een opgegeven face bijwerken in een lijst met grote vlakken door de persistedFaceId ervan. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/lezen | De largeFaceListId, naam en User Data van een grote lijst ophalen. Lijst met grote gezichts lijsten: informatie over largeFaceListId, name en User Data. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/Train/actie | Een trainings taak voor een grote gezichts lijst verzenden. Training is een cruciale stap die alleen kan worden gebruikt door een getrainde grote gezichts lijst. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/training/lezen | Om te controleren of de trainings status van de grote face List is voltooid of nog steeds actief is. LargeFaceList training is een asynchrone bewerking |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largefacelists/schrijven | Maak een lege lijst met grote gezichts gebruikers met een door de gebruiker opgegeven largeFaceListId, een naam en een optioneel User Data. Update gegevens van een grote gezichts lijst, met inbegrip van naam en User Data. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/delete | Een bestaande groep grote personen met de opgegeven personGroupId verwijderen. Permanente gegevens in deze grote groep worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/action | Een nieuwe persoon maken in een opgegeven grote persoons groep. Als u een gezicht wilt toevoegen aan deze persoon, neemt u contact op met |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/delete | Een bestaande persoon verwijderen uit een grote groep personen. Alle opgeslagen persoons gegevens en gezichts afbeeldingen in de persoons vermelding worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/delete | Een gezicht verwijderen van een persoon in een grote groep personen. Gezichts gegevens en afbeelding die betrekking hebben op deze gezichts vermelding worden ook verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/read | Informatie over persoons gezicht ophalen. De permanente persoons zijde wordt opgegeven door de largePersonGroupId, personId en persistedFaceId. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/largepersongroups/personen/persistedfaces/schrijven | Voeg een gezichts afbeelding aan een persoon toe aan een grote persoons groep voor gezichts identificatie of-verificatie. Om te omgaan met de afbeelding van het bijwerken van een persoon die het veld User Data van het gezicht heeft behouden. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/read | Haal de naam en het User Data van een persoon op en de persistente faceIds die de afbeelding van het Inge schreven persoons gezicht vertegenwoordigt. De gegevens van alle personen in de opgegeven groep grote personen weer geven, waaronder personId, name, User Data and persistedFaceIds. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/write | Naam of User Data van een persoon bijwerken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/read | De gegevens van een grote persoons groep ophalen, inclusief de naam en het User Data. Deze API retourneert een lijst met gegevens van een grote persoon met groepen alle bestaande largePesonGroupId, naam en User Data van de groep. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/train/action | Een trainings taak voor een grote persoons groep verzenden. Training is een belang rijke stap dat alleen een getrainde grote persoons groep kan gebruiken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/training/read | De trainings status van een grote persoons groep controleren is voltooid of nog steeds actief. LargePersonGroup training is een asynchrone bewerking |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/write | Maak een nieuwe groep grote personen met een door de gebruiker opgegeven largePersonGroupId, een naam en een optionele User Data. De naam en het User Data van een bestaande groep van grote personen bijwerken. De eigenschappen blijven ongewijzigd als ze niet in de hoofd tekst van de aanvraag staan. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/verwijderen | Een bestaande persoons groep met opgegeven personGroupId verwijderen. De persistente gegevens in deze persoons groep worden verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/actie | Een nieuwe persoon maken in een opgegeven persoons groep. Als u een gezicht wilt toevoegen aan deze persoon, neemt u contact op met |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/verwijderen | Een bestaande persoon uit een persoons groep verwijderen. Alle opgeslagen persoons gegevens en gezichts afbeeldingen in de persoons vermelding worden verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/persistedfaces/verwijderen | Een gezicht verwijderen van een persoon in een persoons groep. Gezichts gegevens en afbeelding die betrekking hebben op deze gezichts vermelding worden ook verwijderd. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/persistedfaces/lezen | Informatie over persoons gezicht ophalen. De permanente persoons zijde wordt opgegeven door de personGroupId, personId en persistedFaceId. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/persistedfaces/schrijven | Voeg een gezichts afbeelding aan een persoon toe aan een persoons groep voor gezichts identificatie of-verificatie. Om te omgaan met de afbeelding van meerdere updates van een persoon die het veld User Data van het gezicht heeft behouden. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/lezen | Haal de naam en het User Data van een persoon op en de persistente faceIds die de afbeelding van het Inge schreven persoons gezicht vertegenwoordigt. De gegevens van alle personen in de opgegeven persoons groep weer geven, met inbegrip van personId, name, User Data en persistedFaceIds van Registered. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/personen/schrijven | Naam of User Data van een persoon bijwerken. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/lezen | Naam van persoons groep ophalen en User Data. Als u persoons gegevens wilt ophalen onder deze personGroup, gebruikt u de pesonGroupId, naam en User Data van de lijst met personen. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/Train/actie | Een trainings taak voor een persoons groep verzenden. Training is een cruciale stap die alleen kan worden gebruikt door een getrainde persoons groep. |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/training/lezen | Om te controleren of de trainings status van de persoons groep is voltooid of nog steeds actief is. PersonGroup-training is een asynchrone bewerking die wordt geactiveerd |
> | DataAction | Micro soft. CognitiveServices/accounts/face/persongroups/schrijven | Maak een nieuwe persoons groep met opgegeven personGroupId, naam en door de gebruiker opgegeven Data User. De naam en het User Data van een bestaande persoons groep bijwerken. De eigenschappen blijven ongewijzigd als ze niet in de hoofd tekst van de aanvraag staan. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/verify/action | Controleer of twee gezichten deel uitmaken van dezelfde persoon of of het ene gezicht deel uitmaakt van een persoon. |
> | DataAction | Micro soft. CognitiveServices/accounts/ImmersiveReader/getcontentmodelforreader/actie | Hiermee maakt u een insluitende lezer-sessie |
> | Action | Microsoft.CognitiveServices/accounts/listKeys/action | Een lijst met sleutels maken |
> | DataAction | Microsoft.CognitiveServices/accounts/LUIS/predict/action | Hiermee wordt de gepubliceerde eindpunt voorspelling opgehaald voor de opgegeven query. |
> | Action | Microsoft.CognitiveServices/accounts/read | Hiermee worden API-accounts gelezen. |
> | Action | Microsoft.CognitiveServices/accounts/regenerateKey/action | Sleutel opnieuw genereren |
> | Action | Microsoft.CognitiveServices/accounts/skus/read | Hiermee worden beschikbare SKU's voor een bestaande resource gelezen. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/entities/action | De API retourneert een lijst met bekende entiteiten en algemene benoemde entiteiten (\"persoon\", \"locatie\", \"organisatie\" enz.) in een bepaald document. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/keyphrases/action | De API retourneert een lijst met tekenreeksen die de belangrijkste gespreksonderwerpen in de invoertekst aanduiden. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/languages/action | De API retourneert de gedetecteerde taal en een numerieke score tussen 0 en 1. Scores dichtbij 1 geven aan dat het 100% zeker is dat de geïdentificeerde taal waar is. Er worden in totaal 120 talen ondersteund. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/sentiment/action | De API retourneert een numerieke score tussen 0 en 1.<br>Scores dicht bij 1 geven een positieve sentiment aan, terwijl scores dicht bij 0 duiden op een negatieve sentiment.<br>Een Score van 0,5 geeft het gebrek aan sentiment aan (bijvoorbeeld<br>een factoid-instructie). |
> | Action | Microsoft.CognitiveServices/accounts/usages/read | Het quotumgebruik voor een bestaande resource ophalen. |
> | Action | Microsoft.CognitiveServices/accounts/write | Hiermee worden API-accounts geschreven. |
> | Action | Microsoft.CognitiveServices/checkDomainAvailability/action | Beschik bare Sku's voor een abonnement lezen. |
> | Action | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Beschik bare Sku's voor een abonnement lezen. |
> | Action | Microsoft.CognitiveServices/locations/deleteVirtualNetworkOrSubnets/action | Melding van micro soft. Network van het verwijderen van VirtualNetworks of subnetten. |
> | Action | Microsoft.CognitiveServices/Operations/read | Alle beschikbare bewerkingen vermelden |
> | Action | Microsoft.CognitiveServices/register/action | Registreert het abonnement voor Cognitive Services |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. commerce/RateCard/lezen | Retour neren gegevens van aanbieding, meta gegevens van resource/meter en tarieven voor het gegeven abonnement. |
> | Action | Microsoft.Commerce/UsageAggregates/read | Hiermee wordt het verbruik van Microsoft Azure door een abonnement opgehaald. Het resultaat bevat statistische gegevens over het gebruik van gegevens, abonnementen en resources die zijn gerelateerd aan een bepaald tijds bereik. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Compute/availabilitySets/delete | Hiermee wordt de beschikbaarheidsset verwijderd |
> | Action | Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald |
> | Action | Microsoft.Compute/availabilitySets/vmSizes/read | Lijst met beschikbare grootten voor het maken of bijwerken van een virtuele machine in de beschikbaarheidsset weergeven |
> | Action | Microsoft.Compute/availabilitySets/write | Hiermee wordt een nieuwe beschikbaarheidsset gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/disks/beginGetAccess/action | Hiermee wordt de SAS-URI van de schijf opgehaald voor blob-toegang |
> | Action | Microsoft.Compute/disks/delete | Hiermee wordt de schijf verwijderd |
> | Action | Microsoft.Compute/disks/endGetAccess/action | Hiermee wordt de SAS-URI van de schijf ingetrokken |
> | Action | Microsoft.Compute/disks/read | Hiermee worden de eigenschappen van een schijf opgehaald |
> | Action | Microsoft.Compute/disks/write | Hiermee wordt een nieuwe schijf gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/galleries/delete | Hiermee wordt de galerie verwijderd |
> | Action | Microsoft.Compute/galleries/images/delete | Hiermee wordt de galerieafbeelding verwijderd |
> | Action | Microsoft.Compute/galleries/images/read | Hiermee worden de eigenschappen van de galerieafbeelding opgehaald |
> | Action | Microsoft.Compute/galleries/images/versions/delete | Hiermee verwijdert u de versie van de afbeeldingsgalerie |
> | Action | Microsoft.Compute/galleries/images/versions/read | Hiermee worden de eigenschappen van de versie van de galerieafbeelding opgehaald |
> | Action | Microsoft.Compute/galleries/images/versions/write | Hiermee wordt een nieuwe versie van de galerieafbeelding gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/galleries/images/write | Hiermee wordt een nieuwe galerieafbeelding gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/galleries/read | Hiermee worden de eigenschappen van de galerie opgehaald |
> | Action | Microsoft.Compute/galleries/write | Hiermee wordt een nieuwe galerie gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/hostGroups/delete | Hiermee wordt de hostgroep verwijderd |
> | Action | Micro soft. Compute/hostGroups/lezen | De eigenschappen van een hostgroep ophalen |
> | Action | Micro soft. Compute/hostGroups/schrijven | Hiermee wordt een nieuwe hostgroep gemaakt of een bestaande hostgroep bijgewerkt |
> | Action | Microsoft.Compute/hosts/delete | Hiermee wordt de host verwijderd |
> | Action | Microsoft.Compute/hosts/read | De eigenschappen van een host ophalen |
> | Action | Micro soft. Compute/hosts/write | Hiermee wordt een nieuwe host gemaakt of een bestaande host bijgewerkt |
> | Action | Microsoft.Compute/images/delete | Hiermee wordt de installatiekopie verwijderd |
> | Action | Microsoft.Compute/images/read | Ga naar de eigenschappen van de installatiekopie |
> | Action | Microsoft.Compute/images/write | Hiermee wordt een nieuwe installatiekopie gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/locations/capsOperations/read | Hiermee wordt de status van een asynchrone Caps-bewerking opgehaald |
> | Action | Microsoft.Compute/locations/diskOperations/read | Hiermee wordt de status van een asynchrone schijf bewerking opgehaald |
> | Action | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Logboeken maken om het totale aantal aanvragen op tijdsinterval weer te geven voor hulp bij diagnose van beperkingen. |
> | Action | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Logboeken maken om geaggregeerde gegevens weer te geen van beperkte aanvragen gegroepeerd op ResourceName, OperationName of het toegepaste beleid voor beperken. |
> | Action | Microsoft.Compute/locations/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | De eigenschappen van een platforminstallatiekopieaanbieding ophalen |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | De eigenschappen van een platforminstallatiekopie-SKU ophalen |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | De eigenschappen van een platforminstallatiekopieversie ophalen |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Hiermee worden de eigenschappen van een VMExtension-type opgehaald |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Hiermee worden de eigenschappen van een VMExtension-versie opgehaald |
> | Action | Microsoft.Compute/locations/publishers/read | De eigenschappen van een uitgever ophalen |
> | Action | Microsoft.Compute/locations/runCommands/read | Hiermee wordt een lijst gemaakt van de beschikbare uitvoeropdrachten op de locatie |
> | Action | Microsoft.Compute/locations/usages/read | Hiermee worden servicelimieten en huidige gebruikshoeveelheden opgehaald voor de rekenresources van het abonnement op een locatie |
> | Action | Microsoft.Compute/locations/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten van virtuele machines op een locatie |
> | Action | Microsoft.Compute/operations/read | Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Compute-resourceprovider |
> | Action | Microsoft.Compute/proximityPlacementGroups/delete | Hiermee wordt de nabijheidsplaatsingsgroep verwijderd |
> | Action | Microsoft.Compute/proximityPlacementGroups/read | De eigenschappen van een nabijheidsplaatsingsgroep ophalen |
> | Action | Microsoft.Compute/proximityPlacementGroups/write | Hiermee wordt een nieuwe nabijheidsplaatsingsgroep gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/register/action | Hiermee wordt het abonnement bij de Microsoft.Compute-resourceprovider geregistreerd |
> | Action | Microsoft.Compute/restorePointCollections/delete | Hiermee worden de herstelpuntverzameling en de hierin opgenomen herstelpunten verwijderd |
> | Action | Microsoft.Compute/restorePointCollections/read | Hiermee worden de eigenschappen van een herstelpuntverzameling opgehaald |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/delete | Hiermee wordt het herstelpunt verwijderd |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/read | Hiermee worden de eigenschappen van een herstelpunt opgehaald |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Hiermee worden de eigenschappen van een herstelpunt en de URI's van de SAS-blob opgehaald |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/write | Hiermee wordt een nieuw herstelpunt gemaakt |
> | Action | Microsoft.Compute/restorePointCollections/write | Hiermee wordt een nieuwe herstelpuntverzameling gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/sharedVMImages/delete | Hiermee wordt de SharedVMImage verwijderd |
> | Action | Microsoft.Compute/sharedVMImages/read | De eigenschappen van een SharedVMImage ophalen |
> | Action | Microsoft.Compute/sharedVMImages/versions/delete | Een SharedVMImageVersion verwijderen |
> | Action | Microsoft.Compute/sharedVMImages/versions/read | Hiermee worden de eigenschappen van een SharedVMImageVersion opgehaald |
> | Action | Microsoft.Compute/sharedVMImages/versions/replicate/action | Een SharedVMImageVersion repliceren naar de doelregio 's |
> | Action | Microsoft.Compute/sharedVMImages/versions/write | Hiermee wordt een nieuwe SharedVMImageVersion gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/sharedVMImages/write | Hiermee wordt een nieuwe SharedVMImage gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/skus/read | Hiermee wordt de lijst met de voor uw abonnement beschikbare Microsoft.Compute-SKU's opgehaald |
> | Action | Microsoft.Compute/snapshots/beginGetAccess/action | De SAS-URI van de moment opname voor BLOB-toegang ophalen |
> | Action | Microsoft.Compute/snapshots/delete | Hiermee wordt een momentopname verwijderd |
> | Action | Microsoft.Compute/snapshots/endGetAccess/action | De SAS-URI van de moment opname intrekken |
> | Action | Microsoft.Compute/snapshots/read | Hiermee worden de eigenschappen van een momentopname opgehaald |
> | Action | Microsoft.Compute/snapshots/write | Hiermee wordt een nieuwe momentopname gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/unregister/action | Hiermee wordt de registratie van het abonnement bij de Microsoft.Compute-resourceprovider ongedaan gemaakt |
> | Action | Microsoft.Compute/virtualMachines/capture/action | Hiermee wordt de virtuele machine vastgelegd via het kopiëren van virtuele harde schijven en wordt een sjabloon gegenereerd waarmee soortgelijke virtuele machines kunnen worden gemaakt |
> | Action | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Hiermee worden de op blobs gebaseerde schijven van de virtuele machine geconverteerd naar beheerde schijven |
> | Action | Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven |
> | Action | Microsoft.Compute/virtualMachines/delete | Hiermee wordt de virtuele machine verwijderd |
> | Action | Microsoft.Compute/virtualMachines/extensions/delete | Hiermee wordt de extensie van de virtuele machine verwijderd |
> | Action | Microsoft.Compute/virtualMachines/extensions/read | Hiermee worden de eigenschappen van de extensie van een virtuele machine opgehaald |
> | Action | Microsoft.Compute/virtualMachines/extensions/write | Hiermee wordt een nieuwe extensie van een virtuele machine gemaakt of een bestaande extensie bijgewerkt |
> | Action | Microsoft.Compute/virtualMachines/generalize/action | Hiermee wordt de status van de virtuele machine ingesteld op Gegeneraliseerd en wordt de virtuele machine voorbereid voor vastleggen |
> | Action | Microsoft.Compute/virtualMachines/instanceView/read | Hiermee wordt de gedetailleerde runtimestatus van de virtuele machine en bijbehorende resources opgehaald |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Aanmelden bij een virtuele machine met Windows-beheerders- of Linux-rootgebruikersbevoegdheden |
> | Action | Microsoft.Compute/virtualMachines/performMaintenance/action | Hiermee wordt de onderhoudsbewerking uitgevoerd op de VM. |
> | Action | Microsoft.Compute/virtualMachines/powerOff/action | Hiermee wordt de virtuele machine uitgeschakeld. Houd er rekening mee dat de virtuele machine nog steeds wordt gefactureerd. |
> | Action | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine opgehaald |
> | Action | Microsoft.Compute/virtualMachines/redeploy/action | Hiermee wordt de virtuele machine opnieuw geïmplementeerd |
> | Action | Microsoft.Compute/virtualMachines/reimage/action | Hiermee wordt een installatiekopie teruggezet van de virtuele machine die een differentiërende schijf gebruikt. |
> | Action | Microsoft.Compute/virtualMachines/restart/action | Hiermee wordt de virtuele machine opnieuw gestart |
> | Action | Microsoft.Compute/virtualMachines/runCommand/action | Hiermee wordt een vooraf gedefinieerd script uitgevoerd op de virtuele machine |
> | Action | Microsoft.Compute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart |
> | Action | Microsoft.Compute/virtualMachines/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt |
> | Action | Microsoft.Compute/virtualMachines/write | Hiermee wordt een nieuwe virtuele machine gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Hiermee worden de rekenresources voor de instanties van de virtuele-machineschaalset uitgeschakeld en vrijgegeven  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete | Hiermee wordt de virtuele-machineschaalset verwijderd |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete/action | Hiermee worden de instanties van de virtuele-machineschaalset verwijderd |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Hiermee wordt de extensie van de virtuele-machineschaalset verwijderd |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Hiermee worden de eigenschappen voor de extensie van een virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Hiermee wordt een nieuwe extensie van de virtuele-machineschaalset gemaakt of een bestaande extensie bijgewerkt |
> | Action | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Doorloop handmatig de platformupdatedomeinen van een virtuele-machineschaalset voor Service Fabric om een vastgelopen update die in behandeling is te voltooien. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Hiermee wordt de instantieweergave van de virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Hiermee worden instanties handmatig bijgewerkt naar het laatste model van de virtuele-machineschaalset |
> | Action | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Hiermee worden de eigenschappen van alle netwerkinterfaces van een virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | Hiermee start u een rolling upgrade om alle instanties van de virtuele-machineschaalset te verplaatsen naar de nieuwste beschikbare besturingssysteemversie van de platforminstallatiekopie. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Hiermee wordt de geschiedenis van de upgrades van het besturingssysteem voor een virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Hiermee wordt gepland onderhoud uitgevoerd op de instanties van de virtuele-machineschaalset |
> | Action | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Hiermee worden de instanties van de virtuele-machineschaalset uitgeschakeld |
> | Action | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Hiermee worden de eigenschappen van alle openbare IP-adressen van een virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/read | Hiermee worden de eigenschappen van de virtuele-machineschaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | De instanties van de virtuele-machineschaalset opnieuw implementeren |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Hiermee worden installatiekopieën voor de instanties van de virtuele-machineschaalset teruggezet |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Hiermee wordt de installatiekopie van alle schijven (besturingssysteemschijf en gegevensschijven) teruggezet voor de instanties van een virtuele-machineschaalset  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/restart/action | Hiermee worden de instanties van de virtuele-machineschaalset opnieuw gestart |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Hiermee wordt de roulerende upgrade van een virtuele-machineschaalset geannuleerd |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | De meest recente status van de roulerende upgrade voor een virtuele-machineschaalset ophalen |
> | Action | Microsoft.Compute/virtualMachineScaleSets/scale/action | Controleer of een bestaande virtuele-machineschaalset kan in-/uitschalen naar een opgegeven aantal instanties |
> | Action | Microsoft.Compute/virtualMachineScaleSets/skus/read | Hiermee wordt een lijst weergegeven van de geldige SKU's voor een bestaande virtuele-machineschaalset |
> | Action | Microsoft.Compute/virtualMachineScaleSets/start/action | Hiermee worden de instanties van de virtuele-machineschaalset gestart |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Hiermee worden de rekenresources voor een virtuele machine in een VM-schaalset uitgeschakeld en vrijgegeven |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Een specifieke virtuele machine uit een VM-schaalset verwijderen. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Hiermee wordt de instantieweergave van een virtuele machine in een VM-schaalset opgehaald. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Haal de eigenschappen van een openbaar IP-adres op dat is gemaakt met de virtuele-machine Schaalset. Virtuele-machine schaal sets kunnen Maxi maal één openbaar IP-adres per ipconfiguration maken (privé IP) |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Eigenschappen ophalen van een of alle IP-configuraties van een netwerk interface die is gemaakt met de virtuele-machine Schaalset. IP-configuraties vertegenwoordigen privé Ip's |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Hiermee worden de eigenschappen opgehaald van een of van alle netwerkinterfaces van een virtuele machine die met behulp van een virtuele-machineschaalset is gemaakt |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Hiermee wordt gepland onderhoud uitgevoerd op een instantie van een virtuele machine in een virtuele-machineschaalset |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset uitgeschakeld. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset opgehaald |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Hiermee wordt een instantie van een virtuele machine in een virtuele-machineschaalset opnieuw geïmplementeerd |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Hiermee wordt de installatiekopie van een instantie van een virtuele machine in een virtuele-machineschaalset teruggezet. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Hiermee wordt de installatiekopie van alle schijven (besturingssysteemschijf en gegevensschijven) voor de instantie van een virtuele machine teruggezet in een virtuele-machineschaalset. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset opnieuw gestart. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Hiermee wordt een vooraf gedefinieerd script uitgevoerd op een instantie van een virtuele machine in een virtuele-machineschaalset. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset gestart. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset bijgewerkt |
> | Action | Microsoft.Compute/virtualMachineScaleSets/vmSizes/read | Beschik bare grootten voor het maken of bijwerken van een virtuele machine in de Schaalset voor virtuele machines weer geven |
> | Action | Microsoft.Compute/virtualMachineScaleSets/write | Hiermee wordt een nieuwe virtuele-machineschaalset gemaakt of een bestaande bijgewerkt |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Consumption/balances/read | Het overzicht van het gebruik van een facturerings periode voor een beheer groep weer geven. |
> | Action | Microsoft.Consumption/budgets/delete | De budgetten van een abonnement of een beheer groep verwijderen. |
> | Action | Microsoft.Consumption/budgets/read | De budgetten op basis van een abonnement of een beheer groep weer geven. |
> | Action | Microsoft.Consumption/budgets/write | Hiermee maakt en bijwerkt u de budgetten op basis van een abonnement of een beheer groep. |
> | Action | Micro soft. verbruik/toeslagen/lezen | Lijst met kosten |
> | Action | Micro soft. verbruik/tegoed/lezen | Lijst tegoeden |
> | Action | Micro soft. verbruik/gebeurtenissen/lezen | Gebeurtenissen weer geven |
> | Action | Micro soft. verbruik/prognoses/lezen | Prognoses weer geven |
> | Action | Micro soft. verbruik/loten/lezen | Loten weer geven |
> | Action | Microsoft.Consumption/marketplaces/read | Vermeld de details van het resource gebruik van Marketplace voor een bereik voor EA en webdirect-abonnementen. |
> | Action | Micro soft. verbruik/operationresults/lezen | Operationresults weer geven |
> | Action | Micro soft. verbruik/bewerkingen/lezen | Een lijst met alle ondersteunde bewerkingen door de resource provider van micro soft. verbruik. |
> | Action | Micro soft. verbruik/operationstatus/lezen | Operationstatus weer geven |
> | Action | Microsoft.Consumption/pricesheets/read | De Pricesheets-gegevens voor een abonnement of een beheer groep weer geven. |
> | Action | Micro soft. verbruik/registreren/actie | Registreren voor gebruik van RP |
> | Action | Micro soft. verbruik/reservationDetails/lezen | Vermeld de gebruiks gegevens voor gereserveerde instanties per reserverings order of beheer groepen. De details van de gegevens zijn per exemplaar per niveau. |
> | Action | Micro soft. verbruik/reservationRecommendations/lezen | Een lijst met enkele of gedeelde aanbevelingen voor gereserveerde instanties voor een abonnement. |
> | Action | Microsoft.Consumption/reservationSummaries/read | Vermeld het gebruiks overzicht voor gereserveerde instanties per reserverings order of beheer groepen. De overzichts gegevens zijn maandelijks of dagelijks. |
> | Action | Microsoft.Consumption/reservationTransactions/read | Vermeld de transactie geschiedenis voor gereserveerde instanties per beheer groep. |
> | Action | Micro soft. verbruik/tags/lezen | Tags weer geven voor EA en abonnementen. |
> | Action | Micro soft. verbruik/tenants/lezen | Tenants weer geven |
> | Action | Micro soft. verbruik/tenants/registreren/actie | Registreer de actie voor het bereik van micro soft. verbruik door een Tenant. |
> | Action | Micro soft. verbruik/voor waarden/lezen | De voor waarden voor een abonnement of een beheer groep weer geven. |
> | Action | Microsoft.Consumption/usageDetails/read | Vermeld de gebruiks gegevens voor een bereik voor EA en webdirect-abonnementen. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Uitvoeren in een specifieke container. |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Logbestanden voor een specifieke container ophalen. |
> | Action | Microsoft.ContainerInstance/containerGroups/delete | De specifieke containergroep verwijderen. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de containergroep opgehaald. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de containergroep gemaakt of bijgewerkt. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens opgehaald voor de containergroep. |
> | Action | Microsoft.ContainerInstance/containerGroups/read | Alle container groepen ophalen. |
> | Action | Microsoft.ContainerInstance/containerGroups/restart/action | Hiermee wordt een specifieke containergroep opnieuw opgestart. |
> | Action | Microsoft.ContainerInstance/containerGroups/start/action | Hiermee wordt een specifieke containergroep gestart. |
> | Action | Microsoft.ContainerInstance/containerGroups/stop/action | Hiermee stopt u een specifieke container groep. De toewijzing van reken resources wordt ongedaan gemaakt en de facturering wordt gestopt. |
> | Action | Microsoft.ContainerInstance/containerGroups/write | Een specifieke containergroep maken of bijwerken. |
> | Action | Microsoft.ContainerInstance/locations/cachedImages/read | Hiermee worden installatiekopieën opgehaald uit het cachegeheugen voor het abonnement in een regio. |
> | Action | Microsoft.ContainerInstance/locations/capabilities/read | Haal de mogelijkheden voor een regio op. |
> | Action | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.ContainerInstance doorgegeven dat het virtuele netwerk of subnet wordt verwijderd. |
> | Action | Microsoft.ContainerInstance/locations/usages/read | Hiermee wordt het gebruik voor een bepaalde regio opgehaald. |
> | Action | Microsoft.ContainerInstance/register/action | Hiermee wordt het abonnement voor de resourceprovider van het containerexemplaar geregistreerd en wordt het maken van containergroepen mogelijk. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ContainerRegistry/checkNameAvailability/read | Controleert of de container register naam beschikbaar is voor gebruik. |
> | Action | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan micro soft. ContainerRegistry door gemeld dat het virtuele netwerk of subnet wordt verwijderd |
> | Action | Microsoft.ContainerRegistry/locations/operationResults/read | Hiermee wordt een asynchroon bewerking resultaat opgehaald |
> | Action | Microsoft.ContainerRegistry/operations/read | Een lijst met alle beschik bare Azure Container Registry REST API bewerkingen |
> | Action | Microsoft.ContainerRegistry/register/action | Hiermee wordt het abonnement voor de resource provider van de container register geregistreerd en wordt het maken van container registers ingeschakeld. |
> | Action | Microsoft.ContainerRegistry/registries/artifacts/delete | Artefact verwijderen in container register. |
> | Action | Microsoft.ContainerRegistry/registries/builds/cancel/action | Hiermee wordt een bestaande build geannuleerd. |
> | Action | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Hiermee wordt een koppeling opgehaald om de build-logboeken te downloaden. |
> | Action | Microsoft.ContainerRegistry/registries/builds/read | Hiermee worden de eigenschappen van de opgegeven build opgehaald of worden alle builds voor het opgegeven container register weer gegeven. |
> | Action | Micro soft. ContainerRegistry/registers/builds/schrijven | Hiermee wordt een build voor een container register bijgewerkt met de opgegeven para meters. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/delete | Hiermee verwijdert u een bouw taak uit een container register. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Hiermee worden de eigenschappen van broncode beheer voor een build-taak weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/read | Hiermee worden de eigenschappen van de opgegeven build-taak opgehaald of worden alle build-taken voor het opgegeven container register weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Hiermee verwijdert u een build-stap van een build-taak. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Hier worden de argumenten voor het maken van een build-stap weer gegeven, inclusief de geheime argumenten. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | Hiermee worden de eigenschappen van de opgegeven build-stap opgehaald of worden alle stappen voor het maken van de opgegeven build-taak weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Hiermee wordt een build-stap voor een build-taak gemaakt of bijgewerkt met de opgegeven para meters. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/write | Hiermee wordt een build-taak voor een container register gemaakt of bijgewerkt met de opgegeven para meters. |
> | Action | Microsoft.ContainerRegistry/registries/delete | Hiermee verwijdert u een container register. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Hiermee verwijdert u een gebeurtenis raster filter uit een container register. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Hiermee worden de eigenschappen van het opgegeven gebeurtenis raster filter opgehaald of worden alle Event grid-filters voor het opgegeven container register weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Hiermee wordt een gebeurtenis raster filter voor een container register met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Hiermee wordt de upload locatie opgehaald zodat de gebruiker de bron kan uploaden. |
> | Action | Microsoft.ContainerRegistry/registries/importImage/action | Importeer de installatie kopie naar het container register met de opgegeven para meters. |
> | Action | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | URL-locatie voor uploaden van bron voor een container register ophalen. |
> | Action | Microsoft.ContainerRegistry/registries/listCredentials/action | Geeft een lijst van de aanmeldings referenties voor het opgegeven container register. |
> | Action | Microsoft.ContainerRegistry/registries/listPolicies/read | Geeft een lijst van de beleids regels voor het opgegeven container register |
> | Action | Microsoft.ContainerRegistry/registries/listUsages/read | Hiermee worden de quota gebruikt voor het opgegeven container register. |
> | Action | Microsoft.ContainerRegistry/registries/metadata/read | Hiermee worden de meta gegevens van een specifieke opslag plaats voor een container register opgehaald |
> | Action | Microsoft.ContainerRegistry/registries/metadata/write | Hiermee worden de meta gegevens van een opslag plaats bijgewerkt voor een container register |
> | Action | Microsoft.ContainerRegistry/registries/operationStatuses/read | Hiermee wordt een asynchrone bewerkings status van het REGI ster opgehaald |
> | Action | Microsoft.ContainerRegistry/registries/pull/read | Pull of ophalen van installatie kopieën uit een container register. |
> | Action | Microsoft.ContainerRegistry/registries/push/write | Push of schrijf installatie kopieën naar een container register. |
> | Action | Microsoft.ContainerRegistry/registries/quarantineRead/read | In quarantaine geplaatste installatie kopieën verzamelen of ophalen uit het container register |
> | Action | Micro soft. ContainerRegistry/registers/quarantineWrite/schrijven | De quarantaine status van in quarantaine geplaatste installatie kopieën schrijven/wijzigen |
> | Action | Microsoft.ContainerRegistry/registries/queueBuild/action | Hiermee maakt u een nieuwe build op basis van de aanvraag parameters en voegt u deze toe aan de build-wachtrij. |
> | Action | Microsoft.ContainerRegistry/registries/read | Hiermee worden de eigenschappen van het opgegeven container register opgehaald of worden alle container registers weer gegeven onder de opgegeven resource groep of dit abonnement. |
> | Action | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Hiermee wordt een van de aanmeldings referenties voor het opgegeven container register opnieuw gegenereerd. |
> | Action | Microsoft.ContainerRegistry/registries/replications/delete | Hiermee verwijdert u een replicatie uit een container register. |
> | Action | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Hiermee wordt de status van een asynchrone replicatie bewerking opgehaald |
> | Action | Microsoft.ContainerRegistry/registries/replications/read | Hiermee worden de eigenschappen van de opgegeven replicatie opgehaald of worden alle replicaties voor het opgegeven container register weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/replications/write | Hiermee wordt een replicatie voor een container register met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.ContainerRegistry/registries/runs/cancel/action | Een bestaande uitvoering annuleren. |
> | Action | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Hiermee wordt de SAS-URL voor het logboek opgehaald voor een run. |
> | Action | Microsoft.ContainerRegistry/registries/runs/read | Hiermee worden de eigenschappen opgehaald van een uitvoering op basis van een container register of lijst runs. |
> | Action | Microsoft.ContainerRegistry/registries/runs/write | Hiermee wordt een run bijgewerkt. |
> | Action | Microsoft.ContainerRegistry/registries/scheduleRun/action | Een uitvoering plannen op basis van een container register. |
> | Action | Micro soft. ContainerRegistry/registers/ondertekenen/schrijven | Meta gegevens van de vertrouwens relatie push/pull voor een container register. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/delete | Hiermee verwijdert u een taak voor een container register. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Alle details van een taak voor een container register weer geven. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/read | Hiermee wordt een taak opgehaald voor een container register of worden alle taken weer gegeven. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/write | Hiermee wordt een taak voor een container register gemaakt of bijgewerkt. |
> | Action | Microsoft.ContainerRegistry/registries/updatePolicies/write | Hiermee wordt het beleid voor het opgegeven container register bijgewerkt |
> | Action | Micro soft. ContainerRegistry/registers/webhooks/verwijderen | Hiermee verwijdert u een webhook uit een container register. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Hiermee wordt de configuratie van de service-URI en aangepaste headers voor de webhook opgehaald. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Een lijst met recente gebeurtenissen voor de opgegeven webhook. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Hiermee wordt een async-bewerkings status van webhook opgehaald |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Hiermee wordt een ping-gebeurtenis geactiveerd die naar de webhook moet worden verzonden. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/read | Hiermee worden de eigenschappen van de opgegeven webhook opgehaald of worden alle webhooks voor het opgegeven container register weer gegeven. |
> | Action | Micro soft. ContainerRegistry/registers/webhooks/schrijven | Hiermee wordt een webhook voor een container register met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Micro soft. ContainerRegistry/registers/schrijven | Hiermee wordt een container register met de opgegeven para meters gemaakt of bijgewerkt. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ContainerService/containerServices/delete | Hiermee wordt een containerservice verwijderd |
> | Action | Microsoft.ContainerService/containerServices/read | Een containerservice ophalen |
> | Action | Microsoft.ContainerService/containerServices/write | Hiermee wordt een nieuwe containerservice gemaakt of wordt een bestaande bijgewerkt |
> | Action | Microsoft.ContainerService/locations/operationresults/read | Hiermee wordt de status van het resultaat van een asynchrone bewerking opgehaald |
> | Action | Microsoft.ContainerService/locations/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Action | Microsoft.ContainerService/locations/orchestrators/read | Hiermee wordt een lijst met ondersteunde orchestrators weergegeven |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Een toegangsprofiel van een beheerd cluster ophalen op rolnaam met lijstreferenties |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/read | Een toegangsprofiel van een beheerd cluster ophalen op rolnaam |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/delete | Hiermee verwijdert u een agent groep |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/read | Hiermee wordt een agent groep opgehaald |
> | Action | Micro soft. container service/managedClusters/agentPools/upgradeProfiles/lezen | Hiermee wordt het upgrade Profiel van de agent groep opgehaald |
> | Action | Microsoft.ContainerService/managedClusters/agentPools/write | Hiermee wordt een nieuwe agent groep gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.ContainerService/managedClusters/delete | Hiermee wordt een beheerd cluster verwijderd |
> | Action | Micro soft. container service/managedClusters/detectors/lezen | Beheerde cluster detectie ophalen |
> | Action | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Hiermee wordt de clusterAdmin-referentie van een beheerd cluster weergegeven |
> | Action | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Hiermee wordt de clusterUser-referentie van een beheerd cluster weergegeven |
> | Action | Micro soft. container service/managedClusters/privateEndpointConnectionsApproval/Action | Hiermee wordt bepaald of de gebruiker een verbinding met een privé-eind punt mag goed keuren |
> | Action | Microsoft.ContainerService/managedClusters/read | Een beheerd cluster ophalen |
> | Action | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Het AAD-Profiel van een beheerd cluster opnieuw instellen |
> | Action | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Het Service-Principal-Profiel van een beheerd cluster opnieuw instellen |
> | Action | Micro soft. container service/managedClusters/upgradeProfiles/lezen | Hiermee wordt het upgradeprofiel van het cluster opgehaald |
> | Action | Microsoft.ContainerService/managedClusters/write | Hiermee wordt een nieuw cluster gemaakt of wordt een bestaande bijgewerkt |
> | Action | Microsoft.ContainerService/openShiftClusters/delete | Een open-Shift-cluster verwijderen |
> | Action | Microsoft.ContainerService/openShiftClusters/read | Een open-Shift-cluster ophalen |
> | Action | Microsoft.ContainerService/openShiftClusters/write | Hiermee maakt u een nieuw open-ploeg cluster of werkt u een bestaand item bij |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/delete | Een door open ploeg beheerd cluster verwijderen |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/read | Een open Shift-beheerd cluster ophalen |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/write | Hiermee maakt u een nieuw open-Shift-beheerd cluster of wordt een bestaand item bijgewerkt |
> | Action | Microsoft.ContainerService/operations/read | Hiermee wordt een lijst met beschikbare bewerkingen op de Microsoft.ContainerService-resourceprovider weergegeven |
> | Action | Microsoft.ContainerService/register/action | Hiermee wordt het abonnement bij de Microsoft.ContainerService-resourceprovider geregistreerd |
> | Action | Microsoft.ContainerService/unregister/action | Hiermee heft u de registratie van het abonnement bij de Microsoft.ContainerService-resourceprovider op |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ContentModerator/applications/delete | Verwijderbewerking |
> | Action | Microsoft.ContentModerator/applications/listSecrets/action | Geheimen vermelden |
> | Action | Micro soft. ContentModerator/Applications/listSingleSignOnToken/actie | Tokens voor eenmalige aanmelding lezen |
> | Action | Microsoft.ContentModerator/applications/read | Leesbewerking |
> | Action | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Action | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Action | Microsoft.ContentModerator/listCommunicationPreference/action | Communicatie voorkeur weer geven |
> | Action | Microsoft.ContentModerator/operations/read | Lees bewerkingen |
> | Action | Microsoft.ContentModerator/updateCommunicationPreference/action | Communicatie voorkeur bijwerken |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.CostManagement/cloudConnectors/delete | De opgegeven Cloud connector verwijderen. |
> | Action | Microsoft.CostManagement/cloudConnectors/read | De cloudConnectors voor de geverifieerde gebruiker weer geven. |
> | Action | Microsoft.CostManagement/cloudConnectors/write | De opgegeven Cloud connector maken of bijwerken. |
> | Action | Microsoft.CostManagement/dimensions/read | Een lijst met alle ondersteunde dimensies op basis van een bereik. |
> | Action | Microsoft.CostManagement/exports/action | Voer de opgegeven export uit. |
> | Action | Microsoft.CostManagement/exports/delete | De opgegeven export verwijderen. |
> | Action | Microsoft.CostManagement/exports/read | De exports op basis van het bereik weer geven. |
> | Action | Microsoft.CostManagement/exports/run/action | Uitvoer uitvoeren. |
> | Action | Micro soft. CostManagement/export/write | De opgegeven export maken of bijwerken. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/externalSubscriptions/read | De externalSubscriptions in een externalBillingAccount voor de geverifieerde gebruiker weer geven. |
> | Action | Microsoft.CostManagement/externalBillingAccounts/read | De externalBillingAccounts voor de geverifieerde gebruiker weer geven. |
> | Action | Microsoft.CostManagement/externalSubscriptions/read | De externalSubscriptions voor de geverifieerde gebruiker weer geven. |
> | Action | Microsoft.CostManagement/externalSubscriptions/write | Gekoppelde beheer groep van externalSubscription bijwerken |
> | Action | Microsoft.CostManagement/query/action | Query's uitvoeren op gebruiks gegevens met een bereik. |
> | Action | Microsoft.CostManagement/query/read | Query's uitvoeren op gebruiks gegevens met een bereik. |
> | Action | Microsoft.CostManagement/register/action | Registreer de actie voor het bereik van micro soft. CostManagement door een abonnement. |
> | Action | Microsoft.CostManagement/reports/action | Plan rapporten over gebruiks gegevens met een bereik. |
> | Action | Micro soft. CostManagement/rapporten/lezen | Plan rapporten over gebruiks gegevens met een bereik. |
> | Action | Microsoft.CostManagement/tenants/register/action | Registreer de actie voor het bereik van micro soft. CostManagement door een Tenant. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataBox/jobs/bookShipmentPickUp/action | Hiermee kan het ophalen van retourzendingen worden geboekt. |
> | Action | Microsoft.DataBox/jobs/cancel/action | Hiermee wordt een order in uitvoering geannuleerd. |
> | Action | Microsoft.DataBox/jobs/delete | De orders verwijderen |
> | Action | Microsoft.DataBox/jobs/listCredentials/action | Hiermee wordt een lijst gemaakt van niet-versleutelde referenties die betrekking hebben op de bestelling. |
> | Action | Microsoft.DataBox/jobs/read | De orders weer geven of ophalen |
> | Action | Microsoft.DataBox/jobs/write | De orders maken of bijwerken |
> | Action | Microsoft.DataBox/locations/availableSkus/action | Met deze methode wordt de lijst met beschikbare SKU's geretourneerd. |
> | Action | Micro soft. DataBox/locaties/availableSkus/lezen | De beschik bare Sku's weer geven of ophalen |
> | Action | Microsoft.DataBox/locations/operationResults/read | De resultaten van de bewerking weer geven of ophalen |
> | Action | Micro soft. DataBox/locaties/regionConfiguration/actie | Deze methode retourneert de configuraties voor de regio. |
> | Action | Microsoft.DataBox/locations/validateAddress/action | Hiermee wordt het verzendadres gevalideerd en eventuele alternatieve adressen opgegeven. |
> | Action | Micro soft. DataBox/locaties/validateInputs/actie | Met deze methode worden alle typen validaties ondersteund. |
> | Action | Micro soft. DataBox/providers/bewerkingen/lezen | De bewerkingen weer geven of ophalen |
> | Action | Microsoft.DataBox/register/action | Provider micro soft. Databox registreren |
> | Action | Micro soft. DataBox/abonnementen/resourceGroups/moveResources/actie |  |
> | Action | Micro soft. DataBox/abonnementen/resourceGroups/validateMoveResources/actie |  |
> | Action | Micro soft. DataBox/registratie/actie | De registratie van de provider micro soft. Databox ongedaan maken |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | De waarschuwingen weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | De waarschuwingen weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Hiermee worden de bandbreedte schema's verwijderd |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | De bandbreedte schema's weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | De bandbreedte schema's weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Hiermee worden de bandbreedte schema's gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Hiermee worden de Data Box Edge-apparaten verwijderd |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Updates naar apparaat downloaden |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/getExtendedInformation/action | Hiermee haalt u uitgebreide informatie over de resource op |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Updates installeren op het apparaat |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | De taken weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | De netwerk instellingen van het apparaat weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationsStatus/read | De bewerkings status weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/delete | De orders worden verwijderd |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | De orders weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | De orders weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/write | Hiermee worden de orders gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | De Data Box Edge-apparaten weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | De Data Box Edge-apparaten weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | De Data Box Edge-apparaten weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Hiermee worden de rollen verwijderd |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Hiermee worden de rollen weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Hiermee worden de rollen weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Hiermee worden de rollen gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Scannen naar updates |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Beveiligingsinstellingen bijwerken |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Hiermee worden de shares verwijderd |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | De shares weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | De shares weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | De metagegevens van de share vernieuwen met de gegevens uit de cloud |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Hiermee worden de shares gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Hiermee verwijdert u de referenties van het opslag account |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | De referenties van het opslag account worden weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | De referenties van het opslag account worden weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Hiermee worden de referenties van het opslag account gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/delete | Hiermee verwijdert u de triggers |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | De triggers worden weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | De triggers worden weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/write | Hiermee worden de triggers gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | De samen vatting van de update weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Certificaat uploaden voor registratie van het apparaat |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Hiermee verwijdert u de gebruikers delen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | Het resultaat van de bewerking weer geven of ophalen |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Hiermee worden de gebruikers van de share weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Hiermee worden de gebruikers van de share weer gegeven of opgehaald |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Hiermee worden de share gebruikers gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Hiermee worden de Data Box Edge-apparaten gemaakt of bijgewerkt |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Hiermee worden de Data Box Edge-apparaten gemaakt of bijgewerkt |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Databricks/locations/getNetworkPolicies/action | Netwerk intentie beleid voor een subnet ophalen op basis van de locatie die wordt gebruikt door NRP |
> | Action | Microsoft.Databricks/register/action | Registreren bij Databricks. |
> | Action | Microsoft.Databricks/workspaces/delete | Hiermee wordt een Databricks-werkruimte verwijderd. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de beschikbare diagnostische instellingen voor de Databricks-werkruimte ingesteld |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de logboekdefinities voor de Databricks-werkruimte opgehaald |
> | Action | Microsoft.Databricks/workspaces/read | Hiermee wordt een lijst met Databricks-werkruimten opgehaald. |
> | Action | Microsoft.Databricks/workspaces/write | Hiermee wordt een Databricks-werkruimte gemaakt. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataCatalog/catalogs/delete | Catalogus bron voor Data Catalog resource provider verwijderen. |
> | Action | Microsoft.DataCatalog/catalogs/read | Lees de resource van de catalogus voor de resource provider van Data Catalog. |
> | Action | Microsoft.DataCatalog/catalogs/write | Bron voor het schrijven van catalogi voor Data Catalog resource provider. |
> | Action | Microsoft.DataCatalog/checkNameAvailability/read | Controleer de beschik baarheid van de catalogus naam voor de resource provider van Data Catalog. |
> | Action | Microsoft.DataCatalog/datacatalogs/delete | Verwijder de DataCatalog-resource voor de resource provider van Data Catalog. |
> | Action | Microsoft.DataCatalog/datacatalogs/read | Lees de DataCatalog-resource voor de resource provider van Data Catalog. |
> | Action | Microsoft.DataCatalog/datacatalogs/write | Schrijf de DataCatalog-resource voor de resource provider van Data Catalog. |
> | Action | Microsoft.DataCatalog/operations/read | Hiermee worden alle beschik bare bewerkingen in Data Catalog resource provider gelezen. |
> | Action | Microsoft.DataCatalog/register/action | Het abonnement voor de resource provider van Data Catalog registreren |
> | Action | Micro soft. DataCatalog/registratie/actie | De registratie van het abonnement voor Data Catalog resource provider ongedaan maken |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Hiermee wordt gecontroleerd of de naam van de Data Factory beschikbaar is voor gebruik. |
> | Action | Microsoft.DataFactory/datafactories/activitywindows/read | Hiermee worden activiteiten Vensters in de Data Factory met opgegeven para meters gelezen. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Hiermee worden activiteit Vensters voor de pijplijn activiteit gelezen met de opgegeven para meters. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Hiermee worden activiteiten Vensters voor de pijp lijn met de opgegeven para meters gelezen. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/delete | Hiermee verwijdert u een pijp lijn. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Hiermee wordt een pijp lijn onderbroken. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/read | Hiermee wordt een pijp lijn gelezen. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Hiermee wordt een pijp lijn hervat. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/update/action | Hiermee werkt u een pijp lijn bij. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/write | Hiermee wordt een pijp lijn gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Hiermee worden activiteiten Vensters voor de gegevensset met opgegeven para meters gelezen. |
> | Action | Microsoft.DataFactory/datafactories/datasets/delete | Hiermee verwijdert u alle gegevensset. |
> | Action | Microsoft.DataFactory/datafactories/datasets/read | Hiermee wordt elke gegevensset gelezen. |
> | Action | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Hiermee wordt de uitvoering van het gegevens segment voor de opgegeven gegevensset gelezen met de opgegeven begin tijd. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/read | Hiermee worden de gegevens segmenten in de opgegeven periode opgehaald. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/write | De status van het gegevens segment bijwerken. |
> | Action | Microsoft.DataFactory/datafactories/datasets/write | Hiermee wordt een gegevensset gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/datafactories/delete | Hiermee verwijdert u de Data Factory. |
> | Action | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Hiermee worden de verbindings gegevens voor elke gateway gelezen. |
> | Action | Microsoft.DataFactory/datafactories/gateways/delete | Hiermee verwijdert u alle gateways. |
> | Action | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Geeft een lijst van de verificatie sleutels voor elke gateway. |
> | Action | Microsoft.DataFactory/datafactories/gateways/read | Alle gateways worden gelezen. |
> | Action | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Hiermee worden de verificatie sleutels voor alle gateways opnieuw gegenereerd. |
> | Action | Microsoft.DataFactory/datafactories/gateways/write | Hiermee wordt een gateway gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/delete | Hiermee verwijdert u alle gekoppelde services. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/read | Hiermee wordt alle gekoppelde services gelezen. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/write | Hiermee wordt een gekoppelde service gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/datafactories/read | Hiermee wordt de Data Factory gelezen. |
> | Action | Microsoft.DataFactory/datafactories/runs/loginfo/read | Hiermee leest u een SAS-URI naar een BLOB-container die de logboeken bevat. |
> | Action | Microsoft.DataFactory/datafactories/tables/delete | Hiermee verwijdert u alle gegevensset. |
> | Action | Microsoft.DataFactory/datafactories/tables/read | Hiermee wordt elke gegevensset gelezen. |
> | Action | Microsoft.DataFactory/datafactories/tables/write | Hiermee wordt een gegevensset gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/datafactories/write | Hiermee wordt de Data Factory gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/factories/cancelpipelinerun/action | Annuleert de uitvoering van de pijp lijn die is opgegeven door de run-ID. |
> | Action | Micro soft. DataFactory/factories/cancelSandboxPipelineRun/actie | Hiermee wordt een debug-uitvoering voor de pijp lijn geannuleerd. |
> | Action | Microsoft.DataFactory/factories/createdataflowdebugsession/action | Hiermee maakt u een foutopsporingssessie voor gegevens stromen. |
> | Action | Microsoft.DataFactory/factories/dataflows/delete | Hiermee verwijdert u de gegevens stroom. |
> | Action | Microsoft.DataFactory/factories/dataflows/read | Hiermee wordt de gegevens stroom gelezen. |
> | Action | Microsoft.DataFactory/factories/dataflows/write | Gegevens stroom maken of bijwerken |
> | Action | Microsoft.DataFactory/factories/datasets/delete | Hiermee verwijdert u alle gegevensset. |
> | Action | Microsoft.DataFactory/factories/datasets/read | Hiermee wordt elke gegevensset gelezen. |
> | Action | Microsoft.DataFactory/factories/datasets/write | Hiermee wordt een gegevensset gemaakt of bijgewerkt. |
> | Action | Micro soft. DataFactory/factories/debugpipelineruns/annuleren/actie | Hiermee wordt een debug-uitvoering voor de pijp lijn geannuleerd. |
> | Action | Microsoft.DataFactory/factories/delete | Hiermee verwijdert u Data Factory. |
> | Action | Microsoft.DataFactory/factories/deletedataflowdebugsession/action | Hiermee verwijdert u een foutopsporingssessie voor gegevens stromen. |
> | Action | Microsoft.DataFactory/factories/getDataPlaneAccess/action | Hiermee krijgt u toegang tot de ADF DataPlane-service. |
> | Action | Microsoft.DataFactory/factories/getDataPlaneAccess/read | Hiermee leest u de toegang tot de ADF DataPlane-service. |
> | Action | Microsoft.DataFactory/factories/getFeatureValue/action | De waarde voor de functie voor belichtings controle ophalen voor de specifieke locatie. |
> | Action | Microsoft.DataFactory/factories/getFeatureValue/read | Hiermee wordt de waarde voor de belichtings controle functie voor de specifieke locatie gelezen. |
> | Action | Microsoft.DataFactory/factories/getGitHubAccessToken/action | Hiermee wordt het toegangs token voor GitHub opgehaald. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/delete | Hiermee worden alle Integration Runtime verwijderd. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Integration Runtime verbindings gegevens lezen. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | Ontvang SSIS Integration Runtime meta gegevens voor het opgegeven Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Hiermee wordt Integration Runtime status gelezen. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Een gekoppelde Integration Runtime verwijzing maken voor de opgegeven gedeelde Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Geeft een lijst van de verificatie sleutels voor een Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Hiermee haalt u de bewakings gegevens voor alle Integration Runtime op. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Hiermee wordt het knoop punt voor de opgegeven Integration Runtime verwijderd. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Retourneert het IP-adres voor het opgegeven knoop punt van de Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | Hiermee leest u het knoop punt voor de opgegeven Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Hiermee wordt een zelf-hostend Integration Runtime knoop punt bijgewerkt. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/read | Hiermee worden alle Integration Runtime gelezen. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | Integration Runtime-meta gegevens voor SSIS vernieuwen voor het opgegeven Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Hiermee worden de verificatie sleutels voor de opgegeven Integration Runtime opnieuw gegenereerd. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Hiermee verwijdert u gekoppelde Integration Runtime verwijzingen van de opgegeven Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/start/action | Start alle Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Hiermee worden alle Integration Runtime gestopt. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Synchroniseert de referenties voor de opgegeven Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Hiermee wordt de opgegeven Integration Runtime bijgewerkt. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/write | Hiermee wordt een Integration Runtime gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/factories/linkedServices/delete | Hiermee verwijdert u de gekoppelde service. |
> | Action | Microsoft.DataFactory/factories/linkedServices/read | Hiermee wordt de gekoppelde service gelezen. |
> | Action | Microsoft.DataFactory/factories/linkedServices/write | Een gekoppelde service maken of bijwerken |
> | Action | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Hiermee leest u de uitvoering van de activiteit voor de opgegeven ID van de pijplijn uitvoering. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/cancel/action | Annuleert de uitvoering van de pijp lijn die is opgegeven door de run-ID. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Query's uitvoeren op de uitvoering van de activiteit voor de opgegeven pijp lijn run ID. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Hiermee wordt het resultaat van de uitvoering van de query activiteit voor de opgegeven pijplijn run-ID gelezen. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/read | De pijplijn uitvoeringen worden gelezen. |
> | Action | Microsoft.DataFactory/factories/pipelines/createrun/action | Hiermee maakt u een uitvoering voor de pijp lijn. |
> | Action | Microsoft.DataFactory/factories/pipelines/delete | Hiermee verwijdert u de pijp lijn. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Hiermee wordt de voortgang van de uitvoering van de activiteit opgehaald. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | Hiermee leest u de pijplijn uitvoering. |
> | Action | Microsoft.DataFactory/factories/pipelines/read | Hiermee leest u de pijp lijn. |
> | Action | Micro soft. DataFactory/fabrieken/pijp lijnen/sandbox/actie | Hiermee maakt u een uitvoerings omgeving voor fout opsporing voor de pijp lijn. |
> | Action | Micro soft. DataFactory/fabrieken/pijp lijnen/sandbox/maken/actie | Hiermee maakt u een uitvoerings omgeving voor fout opsporing voor de pijp lijn. |
> | Action | Micro soft. DataFactory/fabrieken/pijp lijnen/sandbox/uitvoeren/actie | Hiermee maakt u een debug-uitvoering voor de pijp lijn. |
> | Action | Microsoft.DataFactory/factories/pipelines/write | Pijp lijn maken of bijwerken |
> | Action | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Voert een query uit op de uitvoeringen van de pijp lijn. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/action | Query's uitvoeren op de pijplijn uitvoeringen. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/read | Hiermee wordt het resultaat van de query pijplijn uitvoeringen gelezen. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/action | Query's uitvoeren op de trigger. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/read | Hiermee wordt het resultaat van trigger uitvoeringen gelezen. |
> | Action | Microsoft.DataFactory/factories/read | Data Factory lezen. |
> | Action | Micro soft. DataFactory/factories/sandboxpipelineruns/sandboxActivityRuns/lezen | Hiermee worden de uitvoerings gegevens van de fout opsporing voor de activiteit opgehaald. |
> | Action | Microsoft.DataFactory/factories/startdataflowdebugsession/action | Start een sessie voor fout opsporing van gegevens stromen. |
> | Action | Microsoft.DataFactory/factories/triggerruns/read | Hiermee wordt de uitvoering van de trigger gelezen. |
> | Action | Microsoft.DataFactory/factories/triggers/delete | Hiermee verwijdert u een trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/read | Hiermee wordt elke trigger gelezen. |
> | Action | Microsoft.DataFactory/factories/triggers/start/action | Hiermee wordt elke trigger gestart. |
> | Action | Microsoft.DataFactory/factories/triggers/stop/action | Hiermee stopt u een trigger. |
> | Action | Microsoft.DataFactory/factories/triggers/triggerruns/read | Hiermee wordt de uitvoering van de trigger gelezen. |
> | Action | Microsoft.DataFactory/factories/triggers/write | Hiermee wordt een trigger gemaakt of bijgewerkt. |
> | Action | Microsoft.DataFactory/factories/write | Data Factory maken of bijwerken |
> | Action | Microsoft.DataFactory/locations/configureFactoryRepo/action | Hiermee configureert u de opslag plaats voor de Factory. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/action | De waarde voor de functie voor belichtings controle ophalen voor de specifieke locatie. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/read | Hiermee wordt de waarde voor de belichtings controle functie voor de specifieke locatie gelezen. |
> | Action | Microsoft.DataFactory/operations/read | Hiermee worden alle bewerkingen in micro soft Data Factory provider gelezen. |
> | Action | Microsoft.DataFactory/register/action | Hiermee wordt het abonnement voor de Data Factory resource provider geregistreerd. |
> | Action | Microsoft.DataFactory/unregister/action | Hiermee wordt de registratie van het abonnement voor de resource provider Data Factory verwijderd. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Een reken beleid verwijderen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Informatie over een reken beleid ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Een reken beleid maken of bijwerken. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Een Data Lake Store-account ontkoppelen van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Informatie over een gekoppeld data Lake Store-account van een DataLakeAnalytics-account ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Een gekoppeld data Lake Store-account maken of bijwerken van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/delete | Een DataLakeAnalytics-account verwijderen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Een firewall regel verwijderen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Informatie over een firewall regel ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Een firewall regel maken of bijwerken. |
> | Action | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Resultaat van een DataLakeAnalytics-account bewerking ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/read | Informatie over een bestaand DataLakeAnalytics-account ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Vermeld SAS-tokens voor opslag containers van een gekoppeld opslag account van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Containers ophalen van een gekoppeld opslag account van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Een opslag account ontkoppelen van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Informatie ophalen over een gekoppeld opslag account van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Een gekoppeld opslag account maken of bijwerken van een DataLakeAnalytics-account. |
> | Action | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen om taken te annuleren die door andere gebruikers zijn verzonden. |
> | Action | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | SystemMaxAnalyticsUnits over DataLakeAnalytics-accounts overdragen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/delete | Verwijder een regel voor het virtuele netwerk. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/read | Informatie over een virtueel netwerk regel ophalen. |
> | Action | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/write | Een regel voor een virtueel netwerk maken of bijwerken. |
> | Action | Microsoft.DataLakeAnalytics/accounts/write | Een DataLakeAnalytics-account maken of bijwerken. |
> | Action | Microsoft.DataLakeAnalytics/locations/capability/read | Informatie over de mogelijkheden van een abonnement over het gebruik van DataLakeAnalytics. |
> | Action | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Controleer de beschik baarheid van een DataLakeAnalytics-account naam. |
> | Action | Microsoft.DataLakeAnalytics/locations/operationResults/read | Resultaat van een DataLakeAnalytics-account bewerking ophalen. |
> | Action | Microsoft.DataLakeAnalytics/locations/usages/read | Gegevens over quotum gebruik ophalen van een abonnement met behulp van DataLakeAnalytics. |
> | Action | Microsoft.DataLakeAnalytics/operations/read | Beschik bare bewerkingen voor DataLakeAnalytics ophalen. |
> | Action | Microsoft.DataLakeAnalytics/register/action | Registreer het abonnement op DataLakeAnalytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeStore/accounts/delete | Een Data Lake Store-account verwijderen. |
> | Action | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Schakel de sleutel kluis in voor een Data Lake Store-account. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Een EventGrid-filter verwijderen. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Een EventGrid-filter ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Een EventGrid-filter maken of bijwerken. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/delete | Een firewall regel verwijderen. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/read | Informatie over een firewall regel ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/write | Een firewall regel maken of bijwerken. |
> | Action | Microsoft.DataLakeStore/accounts/operationResults/read | Resultaat van een Data Lake Store-account bewerking ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/read | Informatie over een bestaand data Lake Store-account ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/Superuser/action | Verleen de beheerder aan Data Lake Store wanneer deze is verleend met micro soft. Authorization/roleAssignments/write. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Verwijder een vertrouwde id-provider. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Informatie over een vertrouwde id-provider ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Een vertrouwde id-provider maken of bijwerken. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/delete | Verwijder een regel voor het virtuele netwerk. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/read | Informatie over een virtueel netwerk regel ophalen. |
> | Action | Microsoft.DataLakeStore/accounts/virtualNetworkRules/write | Een regel voor een virtueel netwerk maken of bijwerken. |
> | Action | Microsoft.DataLakeStore/accounts/write | Een Data Lake Store-account maken of bijwerken. |
> | Action | Microsoft.DataLakeStore/locations/capability/read | Informatie over de mogelijkheden van een abonnement over het gebruik van data Lake Store. |
> | Action | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Controleer de beschik baarheid van een Data Lake Store-account naam. |
> | Action | Microsoft.DataLakeStore/locations/operationResults/read | Resultaat van een Data Lake Store-account bewerking ophalen. |
> | Action | Microsoft.DataLakeStore/locations/usages/read | Gegevens over quotum gebruik ophalen van een abonnement met behulp van data Lake Store. |
> | Action | Microsoft.DataLakeStore/operations/read | Beschik bare bewerkingen voor data Lake Store ophalen. |
> | Action | Microsoft.DataLakeStore/register/action | Registreer het abonnement op Data Lake Store. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DataMigration/locations/operationResults/read | De status ophalen van een langdurige bewerking die betrekking heeft op een 202-antwoord (Geaccepteerd) |
> | Action | Microsoft.DataMigration/locations/operationStatuses/read | De status ophalen van een langdurige bewerking die betrekking heeft op een 202-antwoord (Geaccepteerd) |
> | Action | Microsoft.DataMigration/register/action | Hiermee wordt het abonnement geregistreerd bij de Azure Database Migration Service-provider |
> | Action | Microsoft.DataMigration/services/addWorker/action | Hiermee wordt een DMS-werknemer toegevoegd aan de beschikbaar werknemers van de service |
> | Action | Microsoft.DataMigration/services/checkStatus/action | Controleer of de service is geïmplementeerd en wordt uitgevoerd |
> | Action | Microsoft.DataMigration/services/configureWorker/action | Hiermee wordt een DMS-werknemer geconfigureerd naar de beschikbare werknemers van de service |
> | Action | Microsoft.DataMigration/services/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/projects/accessArtifacts/action | Een URL genereren die kan worden gebruikt om GET of PUT voor projectartefacten uit te voeren |
> | Action | Microsoft.DataMigration/services/projects/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/projects/files/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/projects/files/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/projects/files/read/action | Een URL verkrijgen waarmee de inhoud van het bestand kan worden gelezen |
> | Action | Microsoft.DataMigration/services/projects/files/readWrite/action | Een URL verkrijgen waarmee de inhoud van het bestand kan worden gelezen of geschreven |
> | Action | Microsoft.DataMigration/services/projects/files/write | Bronnen en hun eigenschappen maken of bijwerken |
> | Action | Microsoft.DataMigration/services/projects/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/projects/tasks/cancel/action | Annuleer de taak als deze momenteel wordt uitgevoerd |
> | Action | Microsoft.DataMigration/services/projects/tasks/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/projects/tasks/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/projects/tasks/write | Azure Database Migration Service-taken uitvoeren |
> | Action | Microsoft.DataMigration/services/projects/write | Azure Database Migration Service-taken uitvoeren |
> | Action | Microsoft.DataMigration/services/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/removeWorker/action | Hiermee wordt een DMS-werknemer verwijderd uit de beschikbare werknemers van de service |
> | Action | Microsoft.DataMigration/services/serviceTasks/cancel/action | Annuleer de taak als deze momenteel wordt uitgevoerd |
> | Action | Microsoft.DataMigration/services/serviceTasks/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/serviceTasks/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/serviceTasks/write | Azure Database Migration Service-taken uitvoeren |
> | Action | Microsoft.DataMigration/services/slots/delete | Hiermee worden een bron en alle onderliggende bronnen verwijderd |
> | Action | Microsoft.DataMigration/services/slots/read | Lees informatie over bronnen |
> | Action | Microsoft.DataMigration/services/slots/write | Bronnen en hun eigenschappen maken of bijwerken |
> | Action | Microsoft.DataMigration/services/start/action | Start de DMS-service zodat deze opnieuw migraties kan verwerken |
> | Action | Microsoft.DataMigration/services/stop/action | Stop de DMS-service om de kosten te minimaliseren |
> | Action | Microsoft.DataMigration/services/updateAgentConfig/action | Hiermee wordt de configuratie van de DMS-agent bijgewerkt met de gegeven waarden. |
> | Action | Microsoft.DataMigration/services/write | Bronnen en hun eigenschappen maken of bijwerken |
> | Action | Microsoft.DataMigration/skus/read | Haal een overzicht op van SKU's die door DMS-bronnen worden ondersteund. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DBforMariaDB/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het wereld wijd inrichten van een bepaald abonnement. |
> | Action | Microsoft.DBforMariaDB/locations/azureAsyncOperation/read | Resultaten van MariaDB-server bewerking retour neren |
> | Action | Microsoft.DBforMariaDB/locations/operationResults/read | Resultaten van MariaDB server-bewerking retour neren op basis van ResourceGroup |
> | Action | Microsoft.DBforMariaDB/locations/operationResults/read | Resultaten van MariaDB-server bewerking retour neren |
> | Action | Microsoft.DBforMariaDB/locations/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Microsoft.DBforMariaDB/locations/securityAlertPoliciesAzureAsyncOperation/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforMariaDB/operations/read | Retour neer de lijst met MariaDB-bewerkingen. |
> | Action | Microsoft.DBforMariaDB/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Microsoft.DBforMariaDB/register/action | MariaDB-resource provider registreren |
> | Action | Microsoft.DBforMariaDB/servers/administrators/delete | Hiermee verwijdert u een bestaande beheerder van de MariaDB-server. |
> | Action | Microsoft.DBforMariaDB/servers/administrators/read | Hiermee haalt u een lijst met MariaDB-Server beheerders op. |
> | Action | Micro soft. DBforMariaDB/servers/beheerders/schrijven | Hiermee wordt de MariaDB-Server beheerder met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.DBforMariaDB/servers/advisors/createRecommendedActionSession/action | Een nieuwe actie sessie voor een aanbeveling maken |
> | Action | Microsoft.DBforMariaDB/servers/advisors/read | De lijst met Advisors retour neren |
> | Action | Microsoft.DBforMariaDB/servers/advisors/read | Een adviseur retour neren |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | De lijst met aanbevolen acties retour neren |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | De lijst met aanbevolen acties retour neren |
> | Action | Microsoft.DBforMariaDB/servers/advisors/recommendedActions/read | Een aanbevolen actie retour neren |
> | Action | Microsoft.DBforMariaDB/servers/configurations/read | De lijst met configuraties voor een server retour neren of de eigenschappen voor de opgegeven configuratie ophalen. |
> | Action | Microsoft.DBforMariaDB/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Action | Microsoft.DBforMariaDB/servers/databases/delete | Hiermee verwijdert u een bestaande MariaDB-data base. |
> | Action | Microsoft.DBforMariaDB/servers/databases/read | De lijst met MariaDB-data bases retour neren of de eigenschappen voor de opgegeven Data Base ophalen. |
> | Action | Microsoft.DBforMariaDB/servers/databases/write | Hiermee maakt u een MariaDB-data base met de opgegeven para meters of werkt u de eigenschappen voor de opgegeven Data Base bij. |
> | Action | Microsoft.DBforMariaDB/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewall regel. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/read | De lijst met firewall regels voor een server retour neren of de eigenschappen voor de opgegeven firewall regel ophalen. |
> | Action | Micro soft. DBforMariaDB/servers/firewallRules/schrijven | Hiermee maakt u een firewall regel met de opgegeven para meters of werkt u een bestaande regel bij. |
> | Action | Microsoft.DBforMariaDB/servers/logFiles/read | De lijst met MariaDB-logboeken weer geven. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic-instelling voor de resource opgehaald |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor MariaDB-servers opgehaald |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Microsoft.DBforMariaDB/servers/queryTexts/action | De tekst voor een lijst met query's retour neren |
> | Action | Microsoft.DBforMariaDB/servers/queryTexts/action | De tekst van een query retour neren |
> | Action | Microsoft.DBforMariaDB/servers/read | De lijst met servers retour neren of de eigenschappen voor de opgegeven server ophalen. |
> | Action | Microsoft.DBforMariaDB/servers/recoverableServers/read | De herstel bare MariaDB-Server gegevens retour neren |
> | Action | Microsoft.DBforMariaDB/servers/replicas/read | Replica's van een MariaDB-server ophalen |
> | Action | Microsoft.DBforMariaDB/servers/restart/action | Hiermee wordt een specifieke server opnieuw gestart. |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Details ophalen van het server Threat Detection-beleid dat op een bepaalde server is geconfigureerd |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Het server Threat Detection-beleid voor een bepaalde server wijzigen |
> | Action | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | De lijst met query statistieken voor de meest voorkomende query's retour neren. |
> | Action | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Een query statistieken retour neren |
> | Action | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Update configuraties voor de opgegeven server |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande Virtual Network regel |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | De lijst met regels voor het virtuele netwerk retour neren of de eigenschappen voor de opgegeven virtuele netwerk regel ophalen. |
> | Action | Micro soft. DBforMariaDB/servers/virtualNetworkRules/schrijven | Hiermee maakt u een regel voor het virtuele netwerk met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven virtuele-netwerk regel bij. |
> | Action | Microsoft.DBforMariaDB/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retour neren |
> | Action | Microsoft.DBforMariaDB/servers/waitStatistics/read | Een wachtende statistiek retour neren |
> | Action | Microsoft.DBforMariaDB/servers/write | Hiermee maakt u een server met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server bij. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DBforMySQL/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het wereld wijd inrichten van een bepaald abonnement. |
> | Action | Microsoft.DBforMySQL/locations/azureAsyncOperation/read | Resultaten van MySQL-server bewerking retour neren |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Resultaten van de MySQL-server bewerking retour neren op basis van ResourceGroup |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Resultaten van MySQL-server bewerking retour neren |
> | Action | Microsoft.DBforMySQL/locations/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Microsoft.DBforMySQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforMySQL/operations/read | De lijst met MySQL-bewerkingen retour neren. |
> | Action | Microsoft.DBforMySQL/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Microsoft.DBforMySQL/register/action | MySQL-resource provider registreren |
> | Action | Microsoft.DBforMySQL/servers/administrators/delete | Hiermee verwijdert u een bestaande beheerder van de MySQL-server. |
> | Action | Microsoft.DBforMySQL/servers/administrators/read | Hiermee haalt u een lijst met MySQL-Server beheerders op. |
> | Action | Microsoft.DBforMySQL/servers/administrators/write | Hiermee wordt de MySQL-Server beheerder met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.DBforMySQL/servers/advisors/createRecommendedActionSession/action | Een nieuwe actie sessie voor een aanbeveling maken |
> | Action | Microsoft.DBforMySQL/servers/advisors/read | De lijst met Advisors retour neren |
> | Action | Microsoft.DBforMySQL/servers/advisors/read | Een adviseur retour neren |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | De lijst met aanbevolen acties retour neren |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | De lijst met aanbevolen acties retour neren |
> | Action | Microsoft.DBforMySQL/servers/advisors/recommendedActions/read | Een aanbevolen actie retour neren |
> | Action | Microsoft.DBforMySQL/servers/configurations/read | De lijst met configuraties voor een server retour neren of de eigenschappen voor de opgegeven configuratie ophalen. |
> | Action | Microsoft.DBforMySQL/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Action | Microsoft.DBforMySQL/servers/databases/delete | Hiermee verwijdert u een bestaande MySQL-data base. |
> | Action | Microsoft.DBforMySQL/servers/databases/read | De lijst met MySQL-data bases retour neren of de eigenschappen voor de opgegeven Data Base ophalen. |
> | Action | Microsoft.DBforMySQL/servers/databases/write | Hiermee maakt u een MySQL-data base met de opgegeven para meters of werkt u de eigenschappen voor de opgegeven Data Base bij. |
> | Action | Microsoft.DBforMySQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewall regel. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/read | De lijst met firewall regels voor een server retour neren of de eigenschappen voor de opgegeven firewall regel ophalen. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/write | Hiermee maakt u een firewall regel met de opgegeven para meters of werkt u een bestaande regel bij. |
> | Action | Microsoft.DBforMySQL/servers/logFiles/read | De lijst met PostgreSQL-logboeken weer geven. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic-instelling voor de resource opgehaald |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor MySQL-servers opgehaald |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Microsoft.DBforMySQL/servers/queryTexts/action | De tekst voor een lijst met query's retour neren |
> | Action | Microsoft.DBforMySQL/servers/queryTexts/action | De tekst van een query retour neren |
> | Action | Microsoft.DBforMySQL/servers/read | De lijst met servers retour neren of de eigenschappen voor de opgegeven server ophalen. |
> | Action | Microsoft.DBforMySQL/servers/recoverableServers/read | De gegevens van de herstel bare MySQL-server retour neren |
> | Action | Microsoft.DBforMySQL/servers/replicas/read | Replica's van een MySQL-server ophalen |
> | Action | Microsoft.DBforMySQL/servers/restart/action | Hiermee wordt een specifieke server opnieuw gestart. |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Details ophalen van het server Threat Detection-beleid dat op een bepaalde server is geconfigureerd |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Het server Threat Detection-beleid voor een bepaalde server wijzigen |
> | Action | Microsoft.DBforMySQL/servers/topQueryStatistics/read | De lijst met query statistieken voor de meest voorkomende query's retour neren. |
> | Action | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Een query statistieken retour neren |
> | Action | Microsoft.DBforMySQL/servers/updateConfigurations/action | Update configuraties voor de opgegeven server |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande Virtual Network regel |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | De lijst met regels voor het virtuele netwerk retour neren of de eigenschappen voor de opgegeven virtuele netwerk regel ophalen. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor het virtuele netwerk met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven virtuele-netwerk regel bij. |
> | Action | Microsoft.DBforMySQL/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retour neren |
> | Action | Microsoft.DBforMySQL/servers/waitStatistics/read | Een wachtende statistiek retour neren |
> | Action | Microsoft.DBforMySQL/servers/write | Hiermee maakt u een server met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server bij. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DBforPostgreSQL/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het wereld wijd inrichten van een bepaald abonnement. |
> | Action | Microsoft.DBforPostgreSQL/locations/azureAsyncOperation/read | Resultaten van PostgreSQL-server bewerking retour neren |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Resultaten van PostgreSQL server-bewerking retour neren op basis van ResourceGroup |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Resultaten van PostgreSQL-server bewerking retour neren |
> | Action | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Micro soft. DBforPostgreSQL/locaties/privateEndpointConnectionAzureAsyncOperation/lezen | Hiermee wordt het resultaat voor een verbindings bewerking voor een privé-eind punt opgehaald |
> | Action | Micro soft. DBforPostgreSQL/locaties/privateEndpointConnectionOperationResults/lezen | Hiermee wordt het resultaat voor een verbindings bewerking voor een privé-eind punt opgehaald |
> | Action | Micro soft. DBforPostgreSQL/locaties/privateEndpointConnectionProxyAzureAsyncOperation/lezen | Hiermee wordt het resultaat opgehaald voor een proxy bewerking voor een privé-eindpunt verbinding |
> | Action | Micro soft. DBforPostgreSQL/locaties/privateEndpointConnectionProxyOperationResults/lezen | Hiermee wordt het resultaat opgehaald voor een proxy bewerking voor een privé-eindpunt verbinding |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | Retour neer de lijst met resultaten van de server bedreigings detectie. |
> | Action | Microsoft.DBforPostgreSQL/operations/read | Retour neer de lijst met PostgreSQL-bewerkingen. |
> | Action | Microsoft.DBforPostgreSQL/performanceTiers/read | Hiermee wordt de lijst met prestatie lagen weer gegeven die beschikbaar zijn. |
> | Action | Microsoft.DBforPostgreSQL/register/action | PostgreSQL-resource provider registreren |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/delete | Hiermee verwijdert u een bestaande beheerder van de PostgreSQL-server. |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/read | Hiermee haalt u een lijst met PostgreSQL-Server beheerders op. |
> | Action | Microsoft.DBforPostgreSQL/servers/administrators/write | Hiermee wordt de PostgreSQL-Server beheerder met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/read | De lijst met Advisors retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | De lijst met aanbevolen acties retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Aanbevelingen doen |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/read | De lijst met configuraties voor een server retour neren of de eigenschappen voor de opgegeven configuratie ophalen. |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/delete | Hiermee verwijdert u een bestaande PostgreSQL-data base. |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/read | De lijst met PostgreSQL-data bases retour neren of de eigenschappen voor de opgegeven Data Base ophalen. |
> | Action | Microsoft.DBforPostgreSQL/servers/databases/write | Hiermee maakt u een PostgreSQL-data base met de opgegeven para meters of werkt u de eigenschappen voor de opgegeven Data Base bij. |
> | Action | Microsoft.DBforPostgreSQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewall regel. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/read | De lijst met firewall regels voor een server retour neren of de eigenschappen voor de opgegeven firewall regel ophalen. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Hiermee maakt u een firewall regel met de opgegeven para meters of werkt u een bestaande regel bij. |
> | Action | Microsoft.DBforPostgreSQL/servers/logFiles/read | De lijst met PostgreSQL-logboeken weer geven. |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnectionProxies/verwijderen | Hiermee verwijdert u een bestaande verbindings proxy voor een persoonlijk eind punt |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnectionProxies/lezen | Hiermee wordt de lijst met particuliere endpoint-verbindings proxy's geretourneerd of worden de eigenschappen opgehaald voor de opgegeven verbinding proxy voor het particuliere eind punt. |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnectionProxies/validate/Action | Hiermee wordt een aanroep voor het maken van een privé-eind punt gevalideerd aan de kant van de NRP |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnectionProxies/schrijven | Hiermee maakt u een particuliere endpoint-verbindings proxy met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven verbinding proxy van het particuliere eind punt bij. |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnections/verwijderen | Hiermee verwijdert u een bestaande persoonlijke eindpunt verbinding |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnections/lezen | Hiermee wordt de lijst met privé-eindpunt verbindingen geretourneerd of worden de eigenschappen opgehaald voor de opgegeven verbinding met een privé-eind punt. |
> | Action | Micro soft. DBforPostgreSQL/servers/privateEndpointConnections/schrijven | Hiermee wordt een bestaande verbinding met een privé-eind punt goedgekeurd of geweigerd |
> | Action | Micro soft. DBforPostgreSQL/servers/privateLinkResources/lezen | De persoonlijke koppelings resources voor de bijbehorende PostgreSQL-server ophalen |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic-instelling voor de resource opgehaald |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor post gres-servers opgehaald |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/queryTexts/action | De tekst van een query retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/queryTexts/read | De tekst voor een lijst met query's retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/read | De lijst met servers retour neren of de eigenschappen voor de opgegeven server ophalen. |
> | Action | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | De herstel bare PostgreSQL-Server gegevens retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/replicas/read | Replica's van een PostgreSQL-server ophalen |
> | Action | Microsoft.DBforPostgreSQL/servers/restart/action | Hiermee wordt een specifieke server opnieuw gestart. |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Details ophalen van het server Threat Detection-beleid dat op een bepaalde server is geconfigureerd |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Het server Threat Detection-beleid voor een bepaalde server wijzigen |
> | Action | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | De lijst met query statistieken voor de meest voorkomende query's retour neren. |
> | Action | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Update configuraties voor de opgegeven server |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande Virtual Network regel |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | De lijst met regels voor het virtuele netwerk retour neren of de eigenschappen voor de opgegeven virtuele netwerk regel ophalen. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor het virtuele netwerk met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven virtuele-netwerk regel bij. |
> | Action | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retour neren |
> | Action | Microsoft.DBforPostgreSQL/servers/write | Hiermee maakt u een server met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server bij. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/configurations/read | De lijst met configuraties voor een server retour neren of de eigenschappen voor de opgegeven configuratie ophalen. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Action | Microsoft.DBforPostgreSQL/serversv2/delete | Hiermee verwijdert u een bestaande server. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/delete | Hiermee verwijdert u een bestaande firewall regel. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/read | De lijst met firewall regels voor een server retour neren of de eigenschappen voor de opgegeven firewall regel ophalen. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/firewallRules/write | Hiermee maakt u een firewall regel met de opgegeven para meters of werkt u een bestaande regel bij. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic-instelling voor de resource opgehaald |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor post gres-servers opgehaald |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Microsoft.DBforPostgreSQL/serversv2/read | De lijst met servers retour neren of de eigenschappen voor de opgegeven server ophalen. |
> | Action | Microsoft.DBforPostgreSQL/serversv2/updateConfigurations/action | Update configuraties voor de opgegeven server |
> | Action | Microsoft.DBforPostgreSQL/serversv2/write | Hiermee maakt u een server met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server bij. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Devices/checkNameAvailability/Action | Controleren of de naam van de IotHub beschikbaar is |
> | Action | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Controleer of de naam van de inrichtings service beschikbaar is |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Certificaat verwijderen |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Verificatie code genereren |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Hiermee wordt het certificaat opgehaald |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Certificaat bron verifiëren |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Certificaat maken of bijwerken |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Delete | De IotHub-Tenant resource verwijderen |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | EventHub Consumer Group verwijderen |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | EventHub Consumer-groep (en) ophalen |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | EventHub-Consumer groep maken |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Apparaten exporteren |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Hiermee wordt de resource voor IotHub Tenant statistieken opgehaald |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Apparaten importeren |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Hiermee wordt de IotHub-Tenant sleutel opgehaald |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Verzonden details van de taak (s) die op de opgegeven IotHub zijn ingediend |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Hiermee worden IotHub-Tenant sleutels opgehaald |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Hiermee worden de beschik bare logboek definities voor de IotHub-service opgehaald |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor de IotHub-service |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Quotum metrieken ophalen |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Read | Hiermee wordt de IotHub-Tenant resource opgehaald |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Een bericht voor alle bestaande routes testen |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Een bericht testen op basis van een gegeven test route |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Hiermee wordt de status van alle routerings eindpunten voor een IotHub opgehaald |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Write | De IotHub-Tenant resource maken of bijwerken |
> | Action | Microsoft.Devices/ElasticPools/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor de IotHub-service |
> | Action | Microsoft.Devices/iotHubs/certificates/Delete | Certificaat verwijderen |
> | Action | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Verificatie code genereren |
> | Action | Microsoft.Devices/iotHubs/certificates/Read | Hiermee wordt het certificaat opgehaald |
> | Action | Microsoft.Devices/iotHubs/certificates/verify/Action | Certificaat bron verifiëren |
> | Action | Microsoft.Devices/iotHubs/certificates/Write | Certificaat maken of bijwerken |
> | Action | Microsoft.Devices/iotHubs/Delete | IotHub-resource verwijderen |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Hiermee wordt het Event Grid filter verwijderd |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Read | Hiermee wordt het Event Grid filter opgehaald |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Write | Een nieuw Event Grid filter maken of een bestaand item bijwerken |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | EventHub Consumer Group verwijderen |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | EventHub Consumer-groep (en) ophalen |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | EventHub-Consumer groep maken |
> | Action | Microsoft.Devices/iotHubs/exportDevices/Action | Apparaten exporteren |
> | Action | Microsoft.Devices/iotHubs/importDevices/Action | Apparaten importeren |
> | Action | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | De IotHub-sleutel voor de opgegeven naam ophalen |
> | Action | Microsoft.Devices/iotHubs/iotHubStats/Read | IotHub-statistieken ophalen |
> | Action | Microsoft.Devices/iotHubs/jobs/Read | Verzonden details van de taak (s) die op de opgegeven IotHub zijn ingediend |
> | Action | Microsoft.Devices/iotHubs/listkeys/Action | Alle IotHub-sleutels ophalen |
> | Action | Microsoft.Devices/IotHubs/logDefinitions/read | Hiermee worden de beschik bare logboek definities voor de IotHub-service opgehaald |
> | Action | Microsoft.Devices/IotHubs/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor de IotHub-service |
> | Action | Microsoft.Devices/iotHubs/operationresults/Read | Resultaat van bewerking ophalen (verouderde API) |
> | Action | Microsoft.Devices/iotHubs/quotaMetrics/Read | Quotum metrieken ophalen |
> | Action | Microsoft.Devices/iotHubs/Read | Hiermee worden de IotHub-resource (s) opgehaald |
> | Action | Microsoft.Devices/iotHubs/routing/$testall/Action | Een bericht voor alle bestaande routes testen |
> | Action | Microsoft.Devices/iotHubs/routing/$testnew/Action | Een bericht testen op basis van een gegeven test route |
> | Action | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Hiermee wordt de status van alle routerings eindpunten voor een IotHub opgehaald |
> | Action | Microsoft.Devices/iotHubs/skus/Read | Geldige IotHub-Sku's ophalen |
> | Action | Microsoft.Devices/iotHubs/Write | IotHub-resource maken of bijwerken |
> | Action | Micro soft. apparaten/locaties/operationresults/lezen | Resultaat van op locatie gebaseerde bewerking ophalen |
> | Action | Micro soft. devices/operationresults/lezen | Resultaat van de bewerking ophalen |
> | Action | Microsoft.Devices/operations/Read | Alle resource provider-bewerkingen ophalen |
> | Action | Microsoft.Devices/provisioningServices/certificates/Delete | Certificaat verwijderen |
> | Action | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Verificatie code genereren |
> | Action | Microsoft.Devices/provisioningServices/certificates/Read | Hiermee wordt het certificaat opgehaald |
> | Action | Microsoft.Devices/provisioningServices/certificates/verify/Action | Certificaat bron verifiëren |
> | Action | Microsoft.Devices/provisioningServices/certificates/Write | Certificaat maken of bijwerken |
> | Action | Microsoft.Devices/provisioningServices/Delete | IotDps-resource verwijderen |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Devices/provisioningServices/keys/listkeys/Action | IotDps-sleutels voor sleutel naam ophalen |
> | Action | Microsoft.Devices/provisioningServices/listkeys/Action | Alle IotDps-sleutels ophalen |
> | Action | Microsoft.Devices/provisioningServices/logDefinitions/read | Hiermee worden de beschik bare logboek definities voor de inrichtings service opgehaald |
> | Action | Microsoft.Devices/provisioningServices/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor de inrichtings service |
> | Action | Microsoft.Devices/provisioningServices/operationresults/Read | Resultaat van DPS-bewerking ophalen |
> | Action | Microsoft.Devices/provisioningServices/Read | IotDps-resource ophalen |
> | Action | Microsoft.Devices/provisioningServices/skus/Read | Geldige IotDps-Sku's ophalen |
> | Action | Microsoft.Devices/provisioningServices/Write | IotDps-resource maken |
> | Action | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resource provider registreren en het maken van IotHub-resources inschakelen |
> | Action | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resource provider registreren en het maken van IotHub-resources inschakelen |
> | Action | Microsoft.Devices/usages/Read | Details van het abonnements gebruik voor deze provider ophalen. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DevSpaces/controllers/delete | Azure dev Spaces-controller en dataplane-Services verwijderen |
> | Action | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Verbindings details weer geven voor de infra structuur van de Azure dev Space-controller |
> | Action | Microsoft.DevSpaces/controllers/read | Eigenschappen van de controller van Azure dev Spaces lezen |
> | Action | Microsoft.DevSpaces/controllers/write | Eigenschappen van een Azure dev Space-controller maken of bijwerken |
> | Action | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Bestaande controller toewijzing voor een container host controleren |
> | Action | Microsoft.DevSpaces/locations/operationresults/read | Status lezen voor een asynchrone bewerking |
> | Action | Microsoft.DevSpaces/register/action | Micro soft dev Spaces resource provider registreren bij een abonnement |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DevTestLab/labCenters/delete | Lab Centers verwijderen. |
> | Action | Microsoft.DevTestLab/labCenters/read | Lab Centers lezen. |
> | Action | Microsoft.DevTestLab/labCenters/write | Lab Centers toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Lees Azure Resource Manager-sjablonen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Hiermee wordt een ARM-sjabloon gegenereerd voor het opgegeven artefact, worden de vereiste bestanden geüpload naar een opslag account en wordt het gegenereerde artefact gevalideerd. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Artefacten lezen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/delete | Artefact bronnen verwijderen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/read | Artefact bronnen lezen. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/write | Artefact bronnen toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Claim een wille keurig claim bare virtuele machine in het lab. |
> | Action | Microsoft.DevTestLab/labs/costs/read | Lees kosten. |
> | Action | Microsoft.DevTestLab/labs/costs/write | Kosten toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/CreateEnvironment/action | Maak virtuele machines in een lab. |
> | Action | Microsoft.DevTestLab/labs/customImages/delete | Aangepaste installatie kopieën verwijderen. |
> | Action | Microsoft.DevTestLab/labs/customImages/read | Aangepaste installatie kopieën lezen. |
> | Action | Microsoft.DevTestLab/labs/customImages/write | Aangepaste installatie kopieën toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/delete | Labs verwijderen. |
> | Action | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Zorg ervoor dat de huidige gebruiker een geldig profiel in het lab heeft. |
> | Action | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Hiermee exporteert u het resource gebruik van de test omgeving naar een opslag account |
> | Action | Microsoft.DevTestLab/labs/formulas/delete | Formules verwijderen. |
> | Action | Microsoft.DevTestLab/labs/formulas/read | Formules lezen. |
> | Action | Microsoft.DevTestLab/labs/formulas/write | Formules toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/galleryImages/read | Galerie afbeeldingen lezen. |
> | Action | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Genereer een URI voor het uploaden van aangepaste schijf installatie kopieën naar een lab. |
> | Action | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Een virtuele machine importeren in een ander lab. |
> | Action | Microsoft.DevTestLab/labs/ListVhds/action | Schijf installatie kopieën weer geven die beschikbaar zijn voor het maken van aangepaste installatie kopieën. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/delete | Meldings kanalen verwijderen. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Een melding naar het meegeleverde kanaal verzenden. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/read | Lees meldings kanalen. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/write | Meldings kanalen toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Evalueert het lab-beleid. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/delete | Beleids regels verwijderen. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/read | Beleid lezen. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/write | Beleids regels toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/policySets/read | Beleids sets lezen. |
> | Action | Microsoft.DevTestLab/labs/read | Lees Labs. |
> | Action | Microsoft.DevTestLab/labs/schedules/delete | Schema's verwijderen. |
> | Action | Microsoft.DevTestLab/labs/schedules/Execute/action | Een schema uitvoeren. |
> | Action | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Een lijst met alle toepasselijke schema's |
> | Action | Microsoft.DevTestLab/labs/schedules/read | Planningen lezen. |
> | Action | Microsoft.DevTestLab/labs/schedules/write | Schema's toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/delete | Verwijder uitlopers van de service. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/read | Lees service-uitlopers. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/write | Het toevoegen of wijzigen van service lopers. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/delete | Gedeelde galerieën verwijderen. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/read | Gedeelde galerieën lezen. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Gedeelde installatie kopieën verwijderen. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Gedeelde installatie kopieën lezen. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Gedeelde installatie kopieën toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/sharedGalleries/write | Gedeelde galerieën toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/delete | Gebruikers profielen verwijderen. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Attach/action | Koppel en maak de lease van de schijf aan de virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/users/disks/delete | Schijven verwijderen. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Detach/action | Ontkoppel en Verbreek de lease van de schijf die aan de virtuele machine is gekoppeld. |
> | Action | Microsoft.DevTestLab/labs/users/disks/read | Lees schijven. |
> | Action | Microsoft.DevTestLab/labs/users/disks/write | Schijven toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/environments/delete | Omgevingen verwijderen. |
> | Action | Microsoft.DevTestLab/labs/users/environments/read | Omgevingen lezen. |
> | Action | Microsoft.DevTestLab/labs/users/environments/write | Omgevingen toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/read | Gebruikers profielen lezen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/delete | Verwijder geheimen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/read | Lees geheimen. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/write | Geheimen toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Service fabrics verwijderen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Hier worden de toepasselijke start-en stop schema's vermeld, indien van toepassing. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Lees service fabrics. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Schema's verwijderen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Een schema uitvoeren. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Planningen lezen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Schema's toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Start een service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Een service Fabric stoppen |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Service fabrics toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/users/write | Gebruikers profielen toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Een nieuwe of bestaande gegevens schijf koppelen aan een virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Artefacten Toep assen op de virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Eigenaar worden van een bestaande virtuele machine |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/delete | Virtuele machines verwijderen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Ontkoppel de opgegeven schijf van de virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Hiermee wordt een teken reeks opgehaald waarmee de inhoud van het RDP-bestand voor de virtuele machine wordt aangeduid |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Hier worden de toepasselijke start-en stop schema's vermeld, indien van toepassing. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/read | Virtuele machines lezen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Een virtuele machine opnieuw implementeren |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Wijzig de grootte van de virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Start een virtuele machine opnieuw op. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Schema's verwijderen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Een schema uitvoeren. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Planningen lezen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Schema's toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Start een virtuele machine. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Een virtuele machine stoppen |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Hiermee worden alle gegevens schijven die zijn gekoppeld aan de virtuele machine, overgedragen aan de huidige gebruiker. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Eigendom van een bestaande virtuele machine vrijgeven |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/write | Hiermee worden virtuele machines toegevoegd of gewijzigd. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/delete | Virtuele netwerken verwijderen. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/read | Virtuele netwerken lezen. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/write | Virtuele netwerken toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/vmPools/delete | Verwijder groepen van virtuele machines. |
> | Action | Microsoft.DevTestLab/labs/vmPools/read | Virtuele-machine Pools lezen. |
> | Action | Microsoft.DevTestLab/labs/vmPools/write | Virtuele-machine Pools toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/labs/write | Labs toevoegen of wijzigen. |
> | Action | Microsoft.DevTestLab/locations/operations/read | Lees bewerkingen. |
> | Action | Microsoft.DevTestLab/register/action | Hiermee wordt het abonnement geregistreerd |
> | Action | Microsoft.DevTestLab/schedules/delete | Schema's verwijderen. |
> | Action | Microsoft.DevTestLab/schedules/Execute/action | Een schema uitvoeren. |
> | Action | Microsoft.DevTestLab/schedules/read | Planningen lezen. |
> | Action | Microsoft.DevTestLab/schedules/Retarget/action | Hiermee wordt de doel resource-id van een planning bijgewerkt. |
> | Action | Microsoft.DevTestLab/schedules/write | Schema's toevoegen of wijzigen. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DocumentDB/databaseAccountNames/read | Hiermee wordt gecontroleerd op Beschik baarheid van namen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Een verzameling verwijderen. Alleen van toepassing op API-typen: MongoDb. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: MongoDb. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Een verzameling lezen of alle verzamelingen weer geven. Alleen van toepassing op API-typen: MongoDb. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: MongoDb. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/read | Lees de door Voer van een verzameling. Alleen van toepassing op API-typen: MongoDb. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/write | De door Voer van een verzameling bijwerken. Alleen van toepassing op API-typen: MongoDb. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Een verzameling maken of bijwerken. Alleen van toepassing op API-typen: MongoDb. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Een container verwijderen. Alleen van toepassing op API-typen: ' SQL '. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: ' SQL '. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Een container lezen of alle containers weer geven. Alleen van toepassing op API-typen: ' SQL '. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: ' SQL '. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/read | Lees een container doorvoer. Alleen van toepassing op API-typen: ' SQL '. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/write | Een container doorvoer bijwerken. Alleen van toepassing op API-typen: ' SQL '. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Een container maken of bijwerken. Alleen van toepassing op API-typen: ' SQL '. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Een Data Base verwijderen. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Een grafiek verwijderen. Alleen van toepassing op API-typen: gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Een grafiek lezen of alle grafieken weer geven. Alleen van toepassing op API-typen: gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/read | Lees een grafiek doorvoer. Alleen van toepassing op API-typen: gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/write | Een grafiek doorvoer bijwerken. Alleen van toepassing op API-typen: gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Een grafiek maken of bijwerken. Alleen van toepassing op API-typen: gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Een Data Base lezen of alle data bases weer geven. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/read | Lees de door Voer van een Data Base. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/write | Een Data Base-door Voer bijwerken. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | Een database maken. Alleen van toepassing op API-typen: ' SQL ', ' MongoDb ', ' gremlin'. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Een spatie verwijderen. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Lees een spatie of geef alle spatie ruimten weer. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/read | Lees de door Voer van een spatie. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/write | Een door Voer van een spatie bijwerken. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Een tabel verwijderen. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Een tabel lezen of alle tabellen in een lijst weer geven. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/read | Een tabel doorvoer lezen. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/write | Een tabel doorvoer bijwerken. Alleen van toepassing op API-typen: Cassandra. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Een tabel maken of bijwerken. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Maak een spatie. Alleen van toepassing op API-typen: Cassandra. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Een tabel verwijderen. Alleen van toepassing op API-typen: Table. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Table. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Een tabel lezen of alle tabellen in een lijst weer geven. Alleen van toepassing op API-typen: Table. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/operationResults/read | Lees de status van de asynchrone bewerking. Alleen van toepassing op API-typen: Table. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/read | Een tabel doorvoer lezen. Alleen van toepassing op API-typen: Table. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/write | Een tabel doorvoer bijwerken. Alleen van toepassing op API-typen: Table. Alleen van toepassing op de instellings typen: door voer. |
> | Action | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Een tabel maken of bijwerken. Alleen van toepassing op API-typen: Table. |
> | Action | Microsoft.DocumentDB/databaseAccounts/backup/action | Een aanvraag indienen voor het configureren van de back-up |
> | Action | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Resource groep van een database account wijzigen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Hiermee worden de metrische definities van de verzameling gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Hiermee worden de metrische gegevens van de verzameling gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Data Base-account partitie sleutel niveau meet waarden lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Metrische gegevens van het partitie niveau voor het database account lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Database account partities in een verzameling lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Het gebruik van het database account op partitie niveau lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Hiermee worden de verzamelings gebruik gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Hiermee worden de metrische definities van de data base gelezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Hiermee worden de metrische gegevens van de data base gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Hiermee worden de database gebruik gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/delete | Hiermee verwijdert u de database accounts. |
> | Action | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Wijzig de prioriteiten van de failover van regio's van een database account. Dit wordt gebruikt voor het uitvoeren van een hand matige failover-bewerking |
> | Action | Micro soft. DocumentDB/databaseAccounts/getBackupPolicy/Action | Het back-upbeleid van het database account ophalen |
> | Action | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | De verbindings reeksen ophalen voor een database account |
> | Action | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Sleutels van een database account weer geven |
> | Action | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Hiermee worden de definities van metrische gegevens van het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/metrics/read | Hiermee worden de metrische gegevens van het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Offline een regio van een database account.  |
> | Action | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Online een regio van een database account. |
> | Action | Microsoft.DocumentDB/databaseAccounts/operationResults/read | De status van de asynchrone bewerking lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Metrische gegevens over latentie lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/read | Percentielen van replicatie latentie lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Latentie-metrische gegevens lezen voor een specifieke bron-en doel regio |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Metrische gegevens over latentie voor een specifieke doel regio lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Een particuliere endpoint-verbindings proxy van het database account verwijderen |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Een particuliere endpoint-verbindings proxy van het database account lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Een particuliere endpoint-verbindings proxy van het database account valideren |
> | Action | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Een particuliere endpoint-verbindings proxy van het database account maken of bijwerken |
> | Action | Microsoft.DocumentDB/databaseAccounts/read | Hiermee leest u een database account. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Hiermee wordt de alleen-lezen sleutels van het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Hiermee wordt de alleen-lezen sleutels van het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Sleutels van een database account draaien |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Leest de metrische gegevens over de verzameling van regio's. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Regionale data base-account partitie sleutel niveau metrieken lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Regionale data base-account gegevens voor partitie niveau lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Regionale database account partities in een verzameling lezen |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Hiermee worden de metrische gegevens van de regio en het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/restore/action | Een herstel aanvraag verzenden |
> | Action | Microsoft.DocumentDB/databaseAccounts/usages/read | Hiermee worden de gebruiks gegevens van het database account gelezen. |
> | Action | Microsoft.DocumentDB/databaseAccounts/write | Een database account bijwerken. |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan micro soft. DocumentDB door gemeld dat VirtualNetwork of subnet wordt verwijderd |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Lees status van asynchrone bewerking deleteVirtualNetworkOrSubnets |
> | Action | Microsoft.DocumentDB/locations/operationsStatus/read | Hiermee wordt de status van asynchrone bewerkingen gelezen |
> | Action | Microsoft.DocumentDB/operationResults/read | De status van de asynchrone bewerking lezen |
> | Action | Microsoft.DocumentDB/operations/read | Lees bewerkingen die beschikbaar zijn voor de micro soft-DocumentDB  |
> | Action | Microsoft.DocumentDB/register/action |  De resource provider van micro soft DocumentDB voor het abonnement registreren |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.DomainRegistration/checkDomainAvailability/Action | Controleren of een domein beschikbaar is voor aankoop |
> | Action | Microsoft.DomainRegistration/domains/Delete | Een bestaand domein verwijderen. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Eigendoms-id verwijderen |
> | Action | Micro soft. DomainRegistration/domains/domainownershipidentifiers/lezen | Eigendoms-id's weer geven |
> | Action | Micro soft. DomainRegistration/domains/domainownershipidentifiers/lezen | Eigendoms-id ophalen |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Id maken of bijwerken |
> | Action | Microsoft.DomainRegistration/domains/operationresults/Read | Een domein bewerking ophalen |
> | Action | Microsoft.DomainRegistration/domains/Read | De lijst met domeinen ophalen |
> | Action | Microsoft.DomainRegistration/domains/Read | Domein ophalen |
> | Action | Microsoft.DomainRegistration/domains/renew/Action | Een bestaand domein vernieuwen. |
> | Action | Microsoft.DomainRegistration/domains/Write | Een nieuw domein toevoegen of een bestaande bijwerken |
> | Action | Microsoft.DomainRegistration/generateSsoRequest/Action | Genereer een aanvraag voor het aanmelden bij het Domain Control Center. |
> | Action | Microsoft.DomainRegistration/listDomainRecommendations/Action | De aanbevelingen van het lijst domein ophalen op basis van tref woorden |
> | Action | Microsoft.DomainRegistration/operations/Read | Alle bewerkingen van de app service-domein registratie weer geven |
> | Action | Microsoft.DomainRegistration/register/action | De resource provider van micro soft-domeinen voor het abonnement registreren |
> | Action | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Actie voor lijst overeenkomst |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Toplevel-domeinen ophalen |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Toplevel-domein ophalen |
> | Action | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Het domein aankoop object valideren zonder dit te verzenden |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.EventGrid/domains/delete | Een domein verwijderen |
> | Action | Microsoft.EventGrid/domains/listKeys/action | Sleutels voor een domein weer geven |
> | Action | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens voor domeinen opgehaald |
> | Action | Microsoft.EventGrid/domains/read | Een domein lezen |
> | Action | Microsoft.EventGrid/domains/regenerateKey/action | Sleutel voor een domein opnieuw genereren |
> | Action | Micro soft. EventGrid/domeinen/onderwerpen/verwijderen | Een domein onderwerp verwijderen |
> | Action | Microsoft.EventGrid/domains/topics/read | Een onderwerp van een domein lezen |
> | Action | Micro soft. EventGrid/domeinen/onderwerpen/schrijven | Een domein onderwerp maken of bijwerken |
> | Action | Microsoft.EventGrid/domains/write | Een domein maken of bijwerken |
> | Action | Microsoft.EventGrid/eventSubscriptions/delete | Een eventSubscription verwijderen |
> | Action | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Volledige URL voor het gebeurtenis abonnement ophalen |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor gebeurtenis abonnementen opgehaald |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor gebeurtenis abonnementen gemaakt of bijgewerkt |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor eventSubscriptions |
> | Action | Microsoft.EventGrid/eventSubscriptions/read | Een eventSubscription lezen |
> | Action | Microsoft.EventGrid/eventSubscriptions/write | Een eventSubscription maken of bijwerken |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor onderwerpen opgehaald |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor onderwerpen gemaakt of bijgewerkt |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor onderwerpen |
> | Action | Microsoft.EventGrid/extensionTopics/read | Lees een extensionTopic. |
> | Action | Microsoft.EventGrid/locations/eventSubscriptions/read | Regionale gebeurtenis abonnementen weer geven |
> | Action | Microsoft.EventGrid/locations/operationResults/read | Het resultaat van een regionale bewerking lezen |
> | Action | Microsoft.EventGrid/locations/operationsStatus/read | De status van een regionale bewerking lezen |
> | Action | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Regionale gebeurtenis abonnementen weer geven op topictype |
> | Action | Microsoft.EventGrid/operationResults/read | Het resultaat van een bewerking lezen |
> | Action | Microsoft.EventGrid/operations/read | Lijst met EventGrid-bewerkingen. |
> | Action | Microsoft.EventGrid/operationsStatus/read | De status van een bewerking lezen |
> | Action | Microsoft.EventGrid/register/action | Hiermee wordt het abonnement voor de EventGrid-resource provider geregistreerd. |
> | Action | Microsoft.EventGrid/topics/delete | Een onderwerp verwijderen |
> | Action | Microsoft.EventGrid/topics/listKeys/action | Sleutels voor een onderwerp weer geven |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor onderwerpen opgehaald |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor onderwerpen gemaakt of bijgewerkt |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor onderwerpen |
> | Action | Microsoft.EventGrid/topics/read | Een onderwerp lezen |
> | Action | Microsoft.EventGrid/topics/regenerateKey/action | Sleutel voor een onderwerp opnieuw genereren |
> | Action | Microsoft.EventGrid/topics/write | Een onderwerp maken of bijwerken |
> | Action | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Globale gebeurtenis abonnementen weer geven op onderwerps type |
> | Action | Microsoft.EventGrid/topictypes/eventtypes/read | Lees eventtypes ondersteund door een topictype |
> | Action | Microsoft.EventGrid/topictypes/read | Een topictype lezen |
> | Action | Micro soft. EventGrid/registratie/actie | Hiermee wordt de registratie van het abonnement voor de EventGrid-resource provider ongedaan gemaakt. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. EventHub/availableClusterRegions/lezen | Lees bewerking om een lijst te maken met beschik bare vooraf ingerichte clusters per Azure-regio. |
> | Action | Microsoft.EventHub/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.EventHub/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. gebruik in plaats daarvan CheckNameAvailability. |
> | Action | Microsoft.EventHub/clusters/delete | Hiermee verwijdert u een bestaande cluster bron. |
> | Action | Microsoft.EventHub/clusters/namespaces/read | Naam ruimte ARM-Id's weer geven voor naam ruimten in een cluster. |
> | Action | Microsoft.EventHub/clusters/operationresults/read | De status van een asynchrone cluster bewerking ophalen. |
> | Action | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Lijst met resource beschrijvingen voor metrische gegevens van cluster ophalen |
> | Action | Microsoft.EventHub/clusters/read | De beschrijving van de clusterresource ophalen |
> | Action | Microsoft.EventHub/clusters/write | Hiermee maakt of wijzigt u een bestaande cluster bron. |
> | Action | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Hiermee verwijdert u de VNet-regels in de EventHub-resourceprovider voor het opgegeven VNet |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/action | De autorisatie regel voor de naam ruimte wordt bijgewerkt. Deze API is afgeschaft. Gebruik in plaats daarvan een PUT-aanroep om de naam ruimte autorisatie regel bij te werken. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/delete | Naam ruimte autorisatie regel verwijderen. De standaard regel voor autorisatie van de naam ruimte kan niet worden verwijderd.  |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/write | Maak autorisatie regels voor het naam ruimte niveau en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Action | Microsoft.EventHub/namespaces/Delete | De resource van een naamruimte verwijderen |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Action | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de nood herstel configuratie die is gekoppeld aan de naam ruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naam ruimte. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Bewerking voor het bijwerken van EventHub. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. Autorisatie regels. Gebruik een aanroep voor het bijwerken van de autorisatie regel. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Bewerking voor het verwijderen van EventHub-autorisatie regels |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | De verbindings reeks ophalen voor EventHub |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  De lijst met EventHub-autorisatie regels ophalen |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Maak EventHub-autorisatie regels en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels kunnen worden bijgewerkt. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Bewerking voor het verwijderen van de ConsumerGroup-resource |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Lijst met ConsumerGroup-resource beschrijvingen ophalen |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Eigenschappen van ConsumerGroup maken of bijwerken. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/Delete | Bewerking voor het verwijderen van de EventHub-resource |
> | Action | Microsoft.EventHub/namespaces/eventhubs/read | Lijst met EventHub-resource beschrijvingen ophalen |
> | Action | Microsoft.EventHub/namespaces/eventhubs/write | Eigenschappen van EventHub maken of bijwerken. |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/delete | Resource voor IP-filter verwijderen |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/read | Resource voor IP-filter ophalen |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/write | Resource voor IP-filter maken |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | Berichten ontvangen |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | Berichten verzenden |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/read | Hiermee wordt het berichten plan voor een naam ruimte opgehaald.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/write | Hiermee wordt het bericht abonnement voor een naam ruimte bijgewerkt.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Micro soft. EventHub/naam ruimten/networkruleset/verwijderen | Resource voor VNET-regel verwijderen |
> | Action | Micro soft. EventHub/naam ruimten/networkruleset/lezen | Hiermee wordt de NetworkRuleSet-resource opgehaald |
> | Action | Micro soft. EventHub/naam ruimten/networkruleset/schrijven | Resource voor VNET-regel maken |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/delete | Resource voor VNET-regel verwijderen |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/read | Hiermee wordt de NetworkRuleSet-resource opgehaald |
> | Action | Microsoft.EventHub/namespaces/networkrulesets/write | Resource voor VNET-regel maken |
> | Action | Microsoft.EventHub/namespaces/operationresults/read | De status van de naamruimtebewerking ophalen |
> | Action | Micro soft. EventHub/naam ruimten/providers/micro soft. Insights/diagnosticSettings/lezen | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met naam ruimte logboek bron beschrijvingen ophalen |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met resource beschrijvingen voor metrische gegevens van de naam ruimte ophalen |
> | Action | Microsoft.EventHub/namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Action | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Resource voor VNET-regel verwijderen |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Resource voor VNET-regel ophalen |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Resource voor VNET-regel maken |
> | Action | Microsoft.EventHub/namespaces/write | Maak een naam ruimte resource en werk de eigenschappen bij. Tags en capaciteit van de naam ruimte zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Action | Microsoft.EventHub/operations/read | Bewerkingen ophalen |
> | Action | Microsoft.EventHub/register/action | Hiermee wordt het abonnement voor de EventHub-resourceprovider geregistreerd en wordt het maken van EventHub-resources mogelijk |
> | Action | Microsoft.EventHub/sku/read | Lijst met resource beschrijvingen van SKU ophalen |
> | Action | Microsoft.EventHub/sku/regions/read | Lijst met SkuRegions-resource beschrijvingen ophalen |
> | Action | Microsoft.EventHub/unregister/action | Hiermee wordt de EventHub-resourceprovider geregistreerd |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. features/onderdelen/lezen | Hiermee kunt u de functies van een abonnement ophalen. |
> | Action | Micro soft. features/Operations/lezen | Hiermee haalt u de lijst met bewerkingen op. |
> | Action | Microsoft.Features/providers/features/read | Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider ophalen. |
> | Action | Microsoft.Features/providers/features/register/action | Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider registreren. |
> | Action | Microsoft.Features/providers/features/unregister/action | Hiermee kunt u de registratie van de functie van een abonnement voor een opgegeven resourceprovider ongedaan maken. |
> | Action | Microsoft.Features/register/action | Hiermee registreert u de functie van een abonnement. |

## <a name="microsoftguestconfiguration"></a>Micro soft. GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/delete | Toewijzing van gast configuratie verwijderen. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Toewijzing van gast configuratie ophalen. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Toewijzings rapport voor gast configuratie ophalen. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Nieuwe gast configuratie toewijzing maken. |
> | Action | Microsoft.GuestConfiguration/register/action | Hiermee wordt het abonnement voor de resource provider micro soft. GuestConfiguration geregistreerd. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.HDInsight/clusters/applications/delete | Toepassing voor HDInsight-cluster verwijderen |
> | Action | Microsoft.HDInsight/clusters/applications/read | Toepassing voor HDInsight-cluster ophalen |
> | Action | Microsoft.HDInsight/clusters/applications/write | Toepassing voor HDInsight-cluster maken of bijwerken |
> | Action | Micro soft. HDInsight/clusters/changerdpsetting/actie | RDP-instelling voor HDInsight-cluster wijzigen |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Configuratie van HDInsight-cluster bijwerken |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Configuraties van HDInsight-clusters ophalen |
> | Action | Microsoft.HDInsight/clusters/configurations/read | Configuraties van HDInsight-clusters ophalen |
> | Action | Microsoft.HDInsight/clusters/delete | Een HDInsight-cluster verwijderen |
> | Action | Micro soft. HDInsight/clusters/uitbrei dingen/verwijderen | Cluster uitbreiding voor HDInsight-cluster verwijderen |
> | Action | Micro soft. HDInsight/clusters/uitbrei dingen/lezen | Cluster uitbreiding voor HDInsight-cluster ophalen |
> | Action | Micro soft. HDInsight/clusters/uitbrei dingen/schrijven | Cluster uitbreiding voor HDInsight-cluster maken |
> | Action | Microsoft.HDInsight/clusters/getGatewaySettings/action | Gateway-instellingen voor HDInsight-cluster ophalen |
> | Action | Micro soft. HDInsight/clusters/providers/micro soft. Insights/diagnosticSettings/lezen | Hiermee wordt de diagnostische instelling voor het bron HDInsight-cluster opgehaald |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor het bron HDInsight-cluster gemaakt of bijgewerkt |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor het HDInsight-cluster |
> | Action | Microsoft.HDInsight/clusters/read | Details over HDInsight-cluster ophalen |
> | Action | Microsoft.HDInsight/clusters/roles/resize/action | Het formaat van een HDInsight-cluster wijzigen |
> | Action | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Gateway-instellingen voor HDInsight-cluster bijwerken |
> | Action | Microsoft.HDInsight/clusters/write | Een HDInsight-cluster maken of bijwerken |
> | Action | Microsoft.HDInsight/locations/capabilities/read | Abonnements mogelijkheden ophalen |
> | Action | Microsoft.HDInsight/locations/checkNameAvailability/read | Beschikbaarheid van de naam controleren |
> | Action | Micro soft. HDInsight/registreren/actie | HDInsight-resource provider registreren voor het abonnement |
> | Action | Micro soft. HDInsight/niet registreren/actie | Registratie van HDInsight-resource provider opheffen voor het abonnement |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ImportExport/Jobs/verwijderen | Hiermee verwijdert u een bestaande taak. |
> | Action | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Hiermee worden de BitLocker-sleutels voor de opgegeven taak opgehaald. |
> | Action | Microsoft.ImportExport/jobs/read | Hiermee worden de eigenschappen van de opgegeven taak opgehaald of wordt de lijst met taken geretourneerd. |
> | Action | Microsoft.ImportExport/jobs/write | Hiermee maakt u een taak met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven taak bij. |
> | Action | Micro soft. ImportExport/locaties/lezen | Hiermee worden de eigenschappen opgehaald voor de opgegeven locatie of wordt de lijst met locaties geretourneerd. |
> | Action | Micro soft. ImportExport/REGI ster/actie | Hiermee wordt het abonnement voor de resource provider import/export geregistreerd en wordt het maken van import/export-taken ingeschakeld. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Insights/ActionGroups/Delete | Een actiegroep verwijderen |
> | Action | Microsoft.Insights/ActionGroups/Read | Een actiegroep lezen |
> | Action | Microsoft.Insights/ActionGroups/Write | Een actiegroep maken of bijwerken |
> | Action | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Waarschuwing voor een activiteitenlogboek ingeschakeld |
> | Action | Microsoft.Insights/ActivityLogAlerts/Delete | Een waarschuwing voor een activiteitenlogboek verwijderen |
> | Action | Microsoft.Insights/ActivityLogAlerts/Read | Een waarschuwing voor een activiteitenlogboek lezen |
> | Action | Microsoft.Insights/ActivityLogAlerts/Write | Een waarschuwing voor een activiteitenlogboek maken of bijwerken |
> | Action | Microsoft.Insights/AlertRules/Activated/Action | Klassieke waarschuwing voor metrische gegevens geactiveerd |
> | Action | Microsoft.Insights/AlertRules/Delete | Een klassieke waarschuwing voor metrische gegevens verwijderen |
> | Action | Microsoft.Insights/AlertRules/Incidents/Read | Een incident van een klassieke waarschuwing voor metrische gegevens lezen |
> | Action | Microsoft.Insights/AlertRules/Read | Een klassieke waarschuwing voor metrische gegevens lezen |
> | Action | Microsoft.Insights/AlertRules/Resolved/Action | Klassieke waarschuwing voor metrische gegevens opgelost |
> | Action | Microsoft.Insights/AlertRules/Throttled/Action | Regel voor klassieke waarschuwing voor metrische gegevens beperkt |
> | Action | Microsoft.Insights/AlertRules/Write | Een klassieke waarschuwing voor metrische gegevens maken of bijwerken |
> | Action | Microsoft.Insights/AutoscaleSettings/Delete | Een instelling voor automatische schaalaanpassing verwijderen |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Een diagnostische instelling voor de resource lezen |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Een diagnostische instelling voor de resource maken of bijwerken |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Logboekdefinities lezen |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Action | Microsoft.Insights/AutoscaleSettings/Read | Een instelling voor automatische schaalaanpassing lezen |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Automatische schaalaanpassing omlaag gestart |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Automatische schaalaanpassing omlaag voltooid |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Automatische schaalaanpassing omhoog gestart |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Automatische schaalaanpassing omhoog voltooid |
> | Action | Microsoft.Insights/AutoscaleSettings/Write | Een instelling voor automatische schaalaanpassing maken of bijwerken |
> | Action | Microsoft.Insights/Baseline/Read | Een metrische basislijn lezen (preview) |
> | Action | Microsoft.Insights/CalculateBaseline/Read | Basislijn voor metrische waarden berekenen (preview) |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Delete | Een analyse-item van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Read | Een analyse-item van Application Insights lezen |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Write | Een analyse-item van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Action | Analysetabelactie voor Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Delete | Een analysetabelschema van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Read | Een analysetabelschema van Application Insights lezen |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Write | Een analysetabelschema van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/Annotations/Delete | Een Application Insights-aantekening verwijderen |
> | Action | Microsoft.Insights/Components/Annotations/Read | Een Application Insights-aantekening lezen |
> | Action | Microsoft.Insights/Components/Annotations/Write | Een Application Insights-aantekening schrijven |
> | Action | Microsoft.Insights/Components/Api/Read | De gegevens-API van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/ApiKeys/Action | Een API-sleutel van Application Insights genereren |
> | Action | Microsoft.Insights/Components/ApiKeys/Delete | Een API-sleutel van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/ApiKeys/Read | Een API-sleutel van Application Insights lezen |
> | Action | Microsoft.Insights/Components/BillingPlanForComponent/Read | Een abonnement van een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/CurrentBillingFeatures/lezen | De huidige factureringsfuncties voor een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/CurrentBillingFeatures/schrijven | De huidige factureringsfuncties voor een Application Insights-onderdeel schrijven |
> | Action | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | De standaardconfiguratie voor ALM-integratie van Application Insights lezen |
> | Action | Microsoft.Insights/Components/Delete | De configuratie van een Application Insights-onderdeel verwijderen |
> | Action | Microsoft.Insights/Components/Events/Read | Logboeken uit Application Insights ophalen met de OData-queryindeling |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Action | Exportinstellingenactie voor Application Insights |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Delete | De exportinstellingen van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Read | De exportinstellingen van Application Insights lezen |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Write | De exportinstellingen van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/ExtendQueries/Read | De uitgebreide queryresultaten van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/Favorites/Delete | Een Application Insights-favoriet verwijderen |
> | Action | Micro soft. Insights/onderdelen/Favorieten/lezen | Een Application Insights-favoriet lezen |
> | Action | Microsoft.Insights/Components/Favorites/Write | Een Application Insights-favoriet schrijven |
> | Action | Micro soft. Insights/onderdelen/FeatureCapabilities/lezen | De functiemogelijkheden van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | De beschikbare factureringsfuncties van een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/GetToken/lezen | Het token van een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/MetricDefinitions/lezen | De metrische definities van een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/metrische gegevens/lezen | De metrische gegevens van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/Move/Action | Een Application Insights-onderdeel verplaatsen naar een andere resourcegroep of een ander abonnement |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Een persoonlijk analyse-item van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Read | Een persoonlijk analyse-item van Application Insights lezen |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Write | Een persoonlijk analyse-item van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/MyFavorites/Read | Een persoonlijke favoriet van Application Insights lezen |
> | Action | Microsoft.Insights/Components/Operations/Read | De status van langlopende bewerkingen in Application Insights ophalen |
> | Action | Micro soft. Insights/onderdelen/PricingPlans/lezen | Het prijsplan van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/PricingPlans/Write | Het prijsplan van een Application Insights-onderdeel schrijven |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | De Proactive Detection-configuratie van Application Insights lezen |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | De Proactive Detection-configuratie van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Action | Microsoft.Insights/Components/Purge/Action | Gegevens uit Application Insights leegmaken |
> | Action | Micro soft. Insights/onderdelen/query/lezen | Query's uitvoeren op Application Insights-Logboeken |
> | Action | Micro soft. Insights/onderdelen/QuotaStatus/lezen | De quotumstatus van een Application Insights-onderdeel lezen |
> | Action | Micro soft. Insights/onderdelen/lezen | De configuratie van een Application Insights-onderdeel lezen |
> | Action | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | De webtestlocaties van Application Insights lezen |
> | Action | Micro soft. Insights/onderdelen/webtesten/lezen | Een configuratie voor een webtest lezen |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Delete | De configuratie voor ALM-integratie van Application Insights verwijderen |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Read | De configuratie voor ALM-integratie van Application Insights lezen |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Write | De configuratie voor ALM-integratie van Application Insights schrijven |
> | Action | Microsoft.Insights/Components/Write | Schrijven naar de configuratie van een Application Insights-onderdeel |
> | Action | Microsoft.Insights/DiagnosticSettings/Delete | Een diagnostische instelling voor de resource verwijderen |
> | Action | Microsoft.Insights/DiagnosticSettings/Read | Een diagnostische instelling voor de resource lezen |
> | Action | Microsoft.Insights/DiagnosticSettings/Write | Een diagnostische instelling voor de resource maken of bijwerken |
> | Action | Micro soft. Insights/EventCategories/lezen | Beschikbare gebeurteniscategorieën van het activiteitenlogboek lezen |
> | Action | Microsoft.Insights/eventtypes/digestevents/Read | Samenvatting beheergebeurtenistype lezen |
> | Action | Microsoft.Insights/eventtypes/values/Read | Gebeurtenissen voor het activiteitenlogboek lezen |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Een diagnostische instelling voor het netwerkstroomlogboek verwijderen |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Een diagnostische instelling voor het netwerkstroomlogboek lezen |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Een diagnostische instelling voor het netwerkstroomlogboek maken of bijwerken |
> | Action | Microsoft.Insights/ListMigrationDate/Action | Migratiedatum van abonnement terughalen |
> | Action | Microsoft.Insights/ListMigrationDate/Read | Migratiedatum van abonnement terughalen |
> | Action | Micro soft. Insights/LogDefinitions/lezen | Logboekdefinities lezen |
> | Action | Microsoft.Insights/LogProfiles/Delete | Een logboekprofiel voor het activiteitenlogboek verwijderen |
> | Action | Microsoft.Insights/LogProfiles/Read | Een logboekprofiel voor het activiteitenlogboek lezen |
> | Action | Microsoft.Insights/LogProfiles/Write | Een logboekprofiel voor het activiteitenlogboek maken of bijwerken |
> | Action | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Gegevens lezen uit de tabel ADAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/ADReplicationResult/Read | Gegevens lezen uit de tabel ADReplicationResult |
> | Action | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Gegevens lezen uit de tabel ADSecurityAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/Alert/Read | Gegevens lezen uit de tabel Alert |
> | Action | Microsoft.Insights/Logs/AlertHistory/Read | Gegevens lezen uit de tabel AlertHistory |
> | Action | Microsoft.Insights/Logs/ApplicationInsights/Read | Gegevens lezen uit de tabel ApplicationInsights |
> | Action | Microsoft.Insights/Logs/AzureActivity/Read | Gegevens lezen uit de tabel AzureActivity |
> | Action | Microsoft.Insights/Logs/AzureMetrics/Read | Gegevens lezen uit de tabel AzureMetrics |
> | Action | Microsoft.Insights/Logs/BoundPort/Read | Gegevens lezen uit de tabel BoundPort |
> | Action | Microsoft.Insights/Logs/CommonSecurityLog/Read | Gegevens lezen uit de tabel CommonSecurityLog |
> | Action | Micro soft. Insights/logboeken/ComputerGroup/lezen | Gegevens lezen uit de tabel ComputerGroup |
> | Action | Micro soft. Insights/logboeken/ConfigurationChange/lezen | Gegevens lezen uit de tabel ConfigurationChange |
> | Action | Microsoft.Insights/Logs/ConfigurationData/Read | Gegevens lezen uit de tabel ConfigurationData |
> | Action | Microsoft.Insights/Logs/ContainerImageInventory/Read | Gegevens lezen uit de tabel ContainerImageInventory |
> | Action | Microsoft.Insights/Logs/ContainerInventory/Read | Gegevens lezen uit de tabel ContainerInventory |
> | Action | Microsoft.Insights/Logs/ContainerLog/Read | Gegevens lezen uit de tabel ContainerLog |
> | Action | Microsoft.Insights/Logs/ContainerServiceLog/Read | Gegevens lezen uit de tabel ContainerServiceLog |
> | Action | Microsoft.Insights/Logs/DeviceAppCrash/Read | Gegevens lezen uit de tabel DeviceAppCrash |
> | Action | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Gegevens lezen uit de tabel DeviceAppLaunch |
> | Action | Micro soft. Insights/logboeken/DeviceCalendar/lezen | Gegevens lezen uit de tabel DeviceCalendar |
> | Action | Microsoft.Insights/Logs/DeviceCleanup/Read | Gegevens lezen uit de tabel DeviceCleanup |
> | Action | Microsoft.Insights/Logs/DeviceConnectSession/Read | Gegevens lezen uit de tabel DeviceConnectSession |
> | Action | Microsoft.Insights/Logs/DeviceEtw/Read | Gegevens lezen uit de tabel DeviceEtw |
> | Action | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Gegevens lezen uit de tabel DeviceHardwareHealth |
> | Action | Microsoft.Insights/Logs/DeviceHealth/Read | Gegevens lezen uit de tabel DeviceHealth |
> | Action | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Gegevens lezen uit de tabel DeviceHeartbeat |
> | Action | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Gegevens lezen uit de tabel DeviceSkypeHeartbeat |
> | Action | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Gegevens lezen uit de tabel DeviceSkypeSignIn |
> | Action | Microsoft.Insights/Logs/DeviceSleepState/Read | Gegevens lezen uit de tabel DeviceSleepState |
> | Action | Microsoft.Insights/Logs/DHAppFailure/Read | Gegevens lezen uit de tabel DHAppFailure |
> | Action | Micro soft. Insights/logboeken/DHAppReliability/lezen | Gegevens lezen uit de tabel DHAppReliability |
> | Action | Micro soft. Insights/logboeken/DHDriverReliability/lezen | Gegevens lezen uit de tabel DHDriverReliability |
> | Action | Microsoft.Insights/Logs/DHLogonFailures/Read | Gegevens lezen uit de tabel DHLogonFailures |
> | Action | Microsoft.Insights/Logs/DHLogonMetrics/Read | Gegevens lezen uit de tabel DHLogonMetrics |
> | Action | Microsoft.Insights/Logs/DHOSCrashData/Read | Gegevens lezen uit de tabel DHOSCrashData |
> | Action | Microsoft.Insights/Logs/DHOSReliability/Read | Gegevens lezen uit de tabel DHOSReliability |
> | Action | Micro soft. Insights/logboeken/DHWipAppLearning/lezen | Gegevens lezen uit de tabel DHWipAppLearning |
> | Action | Micro soft. Insights/logboeken/DnsEvents/lezen | Gegevens lezen uit de tabel DnsEvents |
> | Action | Microsoft.Insights/Logs/DnsInventory/Read | Gegevens lezen uit de tabel DnsInventory |
> | Action | Micro soft. Insights/logboeken/ETWEvent/lezen | Gegevens lezen uit de tabel ETWEvent |
> | Action | Micro soft. Insights/logboeken/gebeurtenis/lezen | Gegevens lezen uit de tabel Event |
> | Action | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Gegevens lezen uit de tabel ExchangeAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel ExchangeOnlineAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/Heartbeat/Read | Gegevens lezen uit de tabel Heartbeat |
> | Action | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Gegevens lezen uit de tabel IISAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/InboundConnection/Read | Gegevens lezen uit de tabel InboundConnection |
> | Action | Microsoft.Insights/Logs/KubeNodeInventory/Read | Gegevens lezen uit de tabel KubeNodeInventory |
> | Action | Microsoft.Insights/Logs/KubePodInventory/Read | Gegevens lezen uit de tabel KubePodInventory |
> | Action | Microsoft.Insights/Logs/LinuxAuditLog/Read | Gegevens lezen uit de tabel LinuxAuditLog |
> | Action | Micro soft. Insights/logboeken/MAApplication/lezen | Gegevens lezen uit de tabel MAApplication |
> | Action | Micro soft. Insights/logboeken/MAApplicationHealth/lezen | Gegevens lezen uit de tabel MAApplicationHealth |
> | Action | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Gegevens lezen uit de tabel MAApplicationHealthAlternativeVersions |
> | Action | Micro soft. Insights/logboeken/MAApplicationHealthIssues/lezen | Gegevens lezen uit de tabel MAApplicationHealthIssues |
> | Action | Microsoft.Insights/Logs/MAApplicationInstance/Read | Gegevens lezen uit de tabel MAApplicationInstance |
> | Action | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Gegevens lezen uit de tabel MAApplicationInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Gegevens lezen uit de tabel MAApplicationReadiness |
> | Action | Micro soft. Insights/logboeken/MADeploymentPlan/lezen | Gegevens lezen uit de tabel MADeploymentPlan |
> | Action | Micro soft. Insights/logboeken/MADevice/lezen | Gegevens lezen uit de tabel MADevice |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Gegevens lezen uit de tabel MADevicePnPHealth |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Gegevens lezen uit de tabel MADevicePnPHealthAlternativeVersions |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Gegevens lezen uit de tabel MADevicePnPHealthIssues |
> | Action | Microsoft.Insights/Logs/MADeviceReadiness/Read | Gegevens lezen uit de tabel MADeviceReadiness |
> | Action | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Gegevens lezen uit de tabel MADriverInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MADriverReadiness/Read | Gegevens lezen uit de tabel MADriverReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeAddin/Read | Gegevens lezen uit de tabel MAOfficeAddin |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Gegevens lezen uit de tabel MAOfficeAddinHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Gegevens lezen uit de tabel MAOfficeAddinHealthIssues |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Gegevens lezen uit de tabel MAOfficeAddinInstance |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Gegevens lezen uit de tabel MAOfficeAddinInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Gegevens lezen uit de tabel MAOfficeAddinReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeApp/Read | Gegevens lezen uit de tabel MAOfficeApp |
> | Action | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Gegevens lezen uit de tabel MAOfficeAppHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Gegevens lezen uit de tabel MAOfficeAppInstance |
> | Action | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Gegevens lezen uit de tabel MAOfficeAppReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Gegevens lezen uit de tabel MAOfficeBuildInfo |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessment |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessmentDailyCounts |
> | Action | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Gegevens lezen uit de tabel MAOfficeDeploymentStatus |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Gegevens lezen uit de tabel MAOfficeMacroHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Gegevens lezen uit de tabel MAOfficeMacroHealthIssues |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Gegevens lezen uit de tabel MAOfficeMacroIssueInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Gegevens lezen uit de tabel MAOfficeMacroIssueReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Gegevens lezen uit de tabel MAOfficeMacroSummary |
> | Action | Microsoft.Insights/Logs/MAOfficeSuite/Read | Gegevens lezen uit de tabel MAOfficeSuite |
> | Action | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Gegevens lezen uit de tabel MAOfficeSuiteInstance |
> | Action | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Gegevens lezen uit de tabel MAProposedPilotDevices |
> | Action | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Gegevens lezen uit de tabel MAWindowsBuildInfo |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessment |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessmentDailyCounts |
> | Action | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Gegevens lezen uit de tabel MAWindowsDeploymentStatus |
> | Action | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Gegevens lezen uit de tabel MAWindowsSysReqInstanceReadiness |
> | Action | Microsoft.Insights/Logs/NetworkMonitoring/Read | Gegevens lezen uit de tabel NetworkMonitoring |
> | Action | Microsoft.Insights/Logs/OfficeActivity/Read | Gegevens lezen uit de tabel OfficeActivity |
> | Action | Microsoft.Insights/Logs/Operation/Read | Gegevens lezen uit de tabel Operation |
> | Action | Microsoft.Insights/Logs/OutboundConnection/Read | Gegevens lezen uit de tabel OutboundConnection |
> | Action | Micro soft. Insights/logboeken/perf/lezen | Gegevens lezen uit de prestatietabel |
> | Action | Microsoft.Insights/Logs/ProtectionStatus/Read | Gegevens lezen uit de tabel ProtectionStatus |
> | Action | Microsoft.Insights/Logs/Read | Gegevens lezen uit al uw logboeken |
> | Action | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Gegevens lezen uit de tabel ReservedAzureCommonFields |
> | Action | Microsoft.Insights/Logs/ReservedCommonFields/Read | Gegevens lezen uit de tabel ReservedCommonFields |
> | Action | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Gegevens lezen uit de tabel SCCMAssessmentRecommendation |
> | Action | Micro soft. Insights/logboeken/SCOMAssessmentRecommendation/lezen | Gegevens lezen uit de tabel SCOMAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SecurityAlert/Read | Gegevens lezen uit de tabel SecurityAlert |
> | Action | Microsoft.Insights/Logs/SecurityBaseline/Read | Gegevens lezen uit de tabel SecurityBaseline |
> | Action | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Gegevens lezen uit de tabel SecurityBaselineSummary |
> | Action | Micro soft. Insights/logboeken/SecurityDetection/lezen | Gegevens lezen uit de tabel SecurityDetection |
> | Action | Microsoft.Insights/Logs/SecurityEvent/Read | Gegevens lezen uit de tabel SecurityEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Gegevens lezen uit de tabel ServiceFabricOperationalEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Gegevens lezen uit de tabel ServiceFabricReliableActorEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Gegevens lezen uit de tabel ServiceFabricReliableServiceEvent |
> | Action | Micro soft. Insights/logboeken/SfBAssessmentRecommendation/lezen | Gegevens lezen uit de tabel SfBAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel SfBOnlineAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel SharePointOnlineAssessmentRecommendation |
> | Action | Micro soft. Insights/logboeken/SPAssessmentRecommendation/lezen | Gegevens lezen uit de tabel SPAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Gegevens lezen uit de tabel SQLAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Gegevens lezen uit de tabel SQLQueryPerformance |
> | Action | Microsoft.Insights/Logs/Syslog/Read | Gegevens lezen uit de tabel Syslog |
> | Action | Micro soft. Insights/logboeken/SysmonEvent/lezen | Gegevens lezen uit de tabel SysmonEvent |
> | Action | Micro soft. Insights/logboeken/UAApp/lezen | Gegevens lezen uit de tabel UAApp |
> | Action | Microsoft.Insights/Logs/UAComputer/Read | Gegevens lezen uit de tabel UAComputer |
> | Action | Microsoft.Insights/Logs/UAComputerRank/Read | Gegevens lezen uit de tabel UAComputerRank |
> | Action | Microsoft.Insights/Logs/UADriver/Read | Gegevens lezen uit de tabel UADriver |
> | Action | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Gegevens lezen uit de tabel UADriverProblemCodes |
> | Action | Micro soft. Insights/logboeken/UAFeedback/lezen | Gegevens lezen uit de tabel UAFeedback |
> | Action | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Gegevens lezen uit de tabel UAHardwareSecurity |
> | Action | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Gegevens lezen uit de tabel UAIESiteDiscovery |
> | Action | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Gegevens lezen uit de tabel UAOfficeAddIn |
> | Action | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Gegevens lezen uit de tabel UAProposedActionPlan |
> | Action | Microsoft.Insights/Logs/UASysReqIssue/Read | Gegevens lezen uit de tabel UASysReqIssue |
> | Action | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Gegevens lezen uit de tabel UAUpgradedComputer |
> | Action | Micro soft. Insights/logboeken/bijwerken/lezen | Gegevens lezen uit de tabel Update |
> | Action | Microsoft.Insights/Logs/UpdateRunProgress/Read | Gegevens lezen uit de tabel UpdateRunProgress |
> | Action | Microsoft.Insights/Logs/UpdateSummary/Read | Gegevens lezen uit de tabel UpdateSummary |
> | Action | Microsoft.Insights/Logs/Usage/Read | Gegevens lezen uit de tabel Usage |
> | Action | Microsoft.Insights/Logs/W3CIISLog/Read | Gegevens lezen uit de tabel W3CIISLog |
> | Action | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Gegevens lezen uit de tabel WaaSDeploymentStatus |
> | Action | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Gegevens lezen uit de tabel WaaSInsiderStatus |
> | Action | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Gegevens lezen uit de tabel WaaSUpdateStatus |
> | Action | Microsoft.Insights/Logs/WDAVStatus/Read | Gegevens lezen uit de tabel WDAVStatus |
> | Action | Microsoft.Insights/Logs/WDAVThreat/Read | Gegevens lezen uit de tabel WDAVThreat |
> | Action | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Gegevens lezen uit de tabel WindowsClientAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/WindowsFirewall/Read | Gegevens lezen uit de tabel WindowsFirewall |
> | Action | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Gegevens lezen uit de tabel WindowsServerAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/WireData/Read | Gegevens lezen uit de tabel WireData |
> | Action | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Gegevens lezen uit de tabel WUDOAggregatedStatus |
> | Action | Microsoft.Insights/Logs/WUDOStatus/Read | Gegevens lezen uit de tabel WUDOStatus |
> | Action | Microsoft.Insights/MetricAlerts/Delete | Een waarschuwing voor metrische gegevens verwijderen |
> | Action | Microsoft.Insights/MetricAlerts/Read | Een waarschuwing voor metrische gegevens lezen |
> | Action | Micro soft. Insights/MetricAlerts/status/lezen | Status van waarschuwing voor metrische gegevens lezen |
> | Action | Micro soft. Insights/MetricAlerts/schrijven | Een waarschuwing voor metrische gegevens maken of bijwerken |
> | Action | Microsoft.Insights/MetricBaselines/Read | Metrische basis lijnen lezen |
> | Action | Micro soft. Insights/MetricDefinitions/micro soft. Insights/lezen | Metrische definities lezen |
> | Action | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Metrische definities lezen |
> | Action | Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Action | Micro soft. Insights/Metricnamespaces/lezen | Metrische naam ruimten lezen |
> | Action | Microsoft.Insights/Metrics/Action | Metrische actie |
> | Action | Micro soft. Insights/metrische gegevens/micro soft. Insights/lezen | De metrische gegevens lezen |
> | Action | Microsoft.Insights/Metrics/providers/Metrics/Read | De metrische gegevens lezen |
> | Action | Microsoft.Insights/Metrics/Read | De metrische gegevens lezen |
> | DataAction | Microsoft.Insights/Metrics/Write | Metrische gegevens schrijven |
> | Action | Microsoft.Insights/MigrateToNewpricingModel/Action | Abonnement migreren naar nieuw prijsmodel |
> | Action | Microsoft.Insights/Operations/Read | Leesbewerkingen |
> | Action | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registeren |
> | Action | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Abonnement terugdraaien naar oude prijsmodel |
> | Action | Microsoft.Insights/ScheduledQueryRules/Delete | Een geplande queryregel verwijderen |
> | Action | Microsoft.Insights/ScheduledQueryRules/Read | Een geplande queryregel lezen |
> | Action | Microsoft.Insights/ScheduledQueryRules/Write | Een geplande queryregel schrijven |
> | Action | Microsoft.Insights/Tenants/Register/Action | Hiermee wordt de Microsoft Insights-provider geïnitialiseerd |
> | Action | Microsoft.Insights/Unregister/Action | De Microsoft Insights-provider registeren |
> | Action | Microsoft.Insights/Webtests/Delete | Een configuratie voor een webtest verwijderen |
> | Action | Microsoft.Insights/Webtests/GetToken/Read | Een webtesttoken lezen |
> | Action | Micro soft. Insights/webtests/MetricDefinitions/lezen | De metrische definities van een webtest lezen |
> | Action | Micro soft. Insights/webtests/metrische gegevens/lezen | De metrische gegevens van een webtest lezen |
> | Action | Microsoft.Insights/Webtests/Read | Een configuratie voor een webtest lezen |
> | Action | Microsoft.Insights/Webtests/Write | Schrijven naar een configuratie voor een webtest |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. intune/diagnosticsettings/verwijderen | Een diagnostische instelling verwijderen |
> | Action | Micro soft. intune/diagnosticsettings/lezen | Een diagnostische instelling lezen |
> | Action | Micro soft. intune/diagnosticsettings/schrijven | Een diagnostische instelling schrijven |
> | Action | Micro soft. intune/diagnosticsettingscategories/lezen | Categorieën voor Diagnostische instellingen lezen |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.IoTCentral/appTemplates/action | Hiermee worden alle beschik bare toepassings sjablonen op Azure IoT Central opgehaald |
> | Action | Microsoft.IoTCentral/checkNameAvailability/action | Hiermee wordt gecontroleerd of een IoT Central toepassings naam beschikbaar is |
> | Action | Microsoft.IoTCentral/checkSubdomainAvailability/action | Hiermee wordt gecontroleerd of een IoT Central subdomein van de toepassing beschikbaar is |
> | Action | Microsoft.IoTCentral/IoTApps/delete | Hiermee verwijdert u een IoT Central-toepassing |
> | Action | Microsoft.IoTCentral/IoTApps/read | Hiermee wordt een enkele IoT Central-toepassing opgehaald |
> | Action | Microsoft.IoTCentral/IoTApps/write | Hiermee wordt een IoT Central-toepassing gemaakt of bijgewerkt |
> | Action | Microsoft.IoTCentral/operations/read | Hiermee worden alle beschik bare bewerkingen voor IoT Central toepassingen opgehaald |
> | Action | Microsoft.IoTCentral/register/action | Het abonnement voor de resource provider van Azure IoT Central registreren |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.IoTSpaces/Graph/delete | Hiermee wordt de resource van micro soft. IoTSpaces-grafiek verwijderd |
> | Action | Microsoft.IoTSpaces/Graph/read | Hiermee worden de resource (s) van de micro soft. IoTSpaces-grafiek opgehaald |
> | Action | Microsoft.IoTSpaces/Graph/write | Resource van micro soft. IoTSpaces Graph maken |
> | Action | Microsoft.IoTSpaces/register/action | Registreer het abonnement voor de resource provider micro soft. IoTSpaces Graph om het maken van resources in te scha kelen |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.KeyVault/checkNameAvailability/read | Hiermee wordt gecontroleerd of de opgegeven sleutelkluisnaam geldig is en niet wordt gebruikt |
> | Action | Microsoft.KeyVault/deletedVaults/read | De eigenschappen van voorlopig verwijderde sleutelkluizen weergeven |
> | Action | Microsoft.KeyVault/hsmPools/delete | Een HSM-groep verwijderen |
> | Action | Microsoft.KeyVault/hsmPools/joinVault/action | Deelnemen aan een sleutelkluis voor een HSM-groep |
> | Action | Microsoft.KeyVault/hsmPools/read | De eigenschappen van een HSM-groep weergeven |
> | Action | Microsoft.KeyVault/hsmPools/write | Een nieuwe HSM-groep maken of de eigenschappen van een bestaande HSM-groep bijwerken |
> | Action | Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sluitelkluis leegmaken |
> | Action | Microsoft.KeyVault/locations/deletedVaults/read | De eigenschappen van voorlopig verwijderde sleutelkluis weergeven |
> | Action | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.KeyVault doorgegeven dat een virtueel netwerk of subnet wordt verwijderd |
> | Action | Microsoft.KeyVault/locations/operationResults/read | Het resultaat van een langdurige uitvoeringsbewerking controleren |
> | Action | Microsoft.KeyVault/operations/read | Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.KeyVault-resourceprovider |
> | Action | Microsoft.KeyVault/register/action | Hiermee wordt een abonnement geregistreerd |
> | Action | Microsoft.KeyVault/unregister/action | Hiermee wordt de registratie van een abonnement opgeheven |
> | Action | Microsoft.KeyVault/vaults/accessPolicies/write | Een bestaand toegangsbeleid bijwerken door samenvoegen of vervangen, of een nieuw toegangsbeleid toevoegen aan een kluis. |
> | Action | Microsoft.KeyVault/vaults/delete | Een sleutelkluis verwijderen |
> | Action | Microsoft.KeyVault/vaults/deploy/action | Hiermee wordt toegang tot de geheimen in een sleutelkluis ingeschakeld tijdens het implementeren van Azure-resources |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/delete | Hiermee wordt aan Microsoft.KeyVault gemeld dat er een EventGrid-abonnement voor Key Vault wordt verwijderd |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/read | Hiermee wordt aan Microsoft.KeyVault gemeld dat er een EventGrid-abonnement voor Key Vault wordt weergegeven |
> | Action | Microsoft.KeyVault/vaults/eventGridFilters/write | Hiermee wordt aan Microsoft.KeyVault gemeld dat er een nieuw EventGrid-abonnement voor Key Vault wordt gemaakt |
> | Action | Microsoft.KeyVault/vaults/read | De eigenschappen van een sleutelkluis weergeven |
> | Action | Microsoft.KeyVault/vaults/secrets/read | De eigenschappen van een geheim weer geven, maar niet de waarde ervan. |
> | Action | Microsoft.KeyVault/vaults/secrets/write | Maak een nieuw geheim of werk de waarde van een bestaand geheim bij. |
> | Action | Microsoft.KeyVault/vaults/write | Een nieuwe sleutelkluis maken of de eigenschappen van een bestaande sleutelkluis bijwerken |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Kusto/Clusters/Activate/action | Start het cluster. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/delete | Hiermee verwijdert u een gekoppelde database configuratie resource. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/read | Hiermee wordt een gekoppelde database configuratie bron gelezen. |
> | Action | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/write | Hiermee wordt een gekoppelde database configuratie bron geschreven. |
> | Action | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Controleert de beschik baarheid van de cluster naam. |
> | Action | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Voegt databaseprincipal toe. |
> | Action | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Controleert de beschik baarheid van namen voor een bepaald type. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Hiermee verwijdert u een bron voor gegevens verbindingen. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Hiermee wordt een gegevens verbindings bron gelezen. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Schrijft een gegevens verbindings bron. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Valideert de database gegevens verbinding. |
> | Action | Microsoft.Kusto/Clusters/Databases/delete | Hiermee verwijdert u een database resource. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Hiermee verwijdert u een event hub-verbindings bron. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Hiermee wordt een event hub-verbindings bron gelezen. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Schrijft een event hub-verbindings bron. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Hiermee valideert u de data base Event hub-verbinding. |
> | Action | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Een lijst met data base-principals. |
> | Action | Microsoft.Kusto/Clusters/Databases/read | Hiermee wordt een database Resource gelezen. |
> | Action | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Hiermee verwijdert u databaseprincipal. |
> | Action | Microsoft.Kusto/Clusters/Databases/write | Schrijft een database resource. |
> | Action | Microsoft.Kusto/Clusters/Deactivate/action | Hiermee wordt het cluster gestopt. |
> | Action | Microsoft.Kusto/Clusters/delete | Hiermee verwijdert u een cluster bron. |
> | Action | Microsoft.Kusto/Clusters/read | Hiermee wordt een cluster bron gelezen. |
> | Action | Microsoft.Kusto/Clusters/SKUs/read | Hiermee wordt een cluster-SKU-resource gelezen. |
> | Action | Microsoft.Kusto/Clusters/Start/action | Start het cluster. |
> | Action | Microsoft.Kusto/Clusters/Stop/action | Hiermee wordt het cluster gestopt. |
> | Action | Microsoft.Kusto/Clusters/write | Schrijft een cluster bron. |
> | Action | Microsoft.Kusto/Locations/CheckNameAvailability/action | Hiermee wordt de beschik baarheid van resource namen gecontroleerd. |
> | Action | Microsoft.Kusto/locations/operationresults/read | Hiermee worden bronnen voor bedrijfs activiteiten gelezen |
> | Action | Micro soft. Kusto/Operations/lezen | Hiermee worden bronnen voor bedrijfs activiteiten gelezen |
> | Action | Micro soft. Kusto/REGI ster/actie | Registratie actie abonnement |
> | Action | Microsoft.Kusto/Register/action | Hiermee wordt het abonnement geregistreerd bij de resource provider Kusto. |
> | Action | Micro soft. Kusto/Sku's/lezen | Hiermee leest u een SKU-resource. |
> | Action | Micro soft. Kusto/registratie/actie | Hiermee wordt de registratie van het abonnement bij de Kusto-resource provider ongedaan gemaakt. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.LabServices/labAccounts/CreateLab/action | Een lab maken in een Lab-account. |
> | Action | Microsoft.LabServices/labAccounts/delete | Lab-accounts verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/delete | Galerie afbeeldingen verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/read | Galerie afbeeldingen lezen. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/write | Galerie afbeeldingen toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Informatie over regionale Beschik baarheid ophalen voor elke grootte categorie die is geconfigureerd met een Lab-account |
> | Action | Micro soft. LabServices/labAccounts/idleShutdowns/verwijderen | Verwijder idleshutdowns. |
> | Action | Micro soft. LabServices/labAccounts/idleShutdowns/lezen | Lees idleshutdowns. |
> | Action | Micro soft. LabServices/labAccounts/idleShutdowns/write | Idleshutdowns toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Gebruikers toevoegen aan een Lab |
> | Action | Microsoft.LabServices/labAccounts/labs/delete | Labs verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Omgevings instelling verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Omgevingen verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Omgevingen lezen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Hiermee wordt het gebruikers wachtwoord opnieuw ingesteld op een omgeving |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Start een omgeving door alle resources in de omgeving te starten. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Stopt een omgeving door alle resources in de omgeving te stoppen |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Omgevingen toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Voorziet in het inrichten van vereiste resources voor een omgevings instelling op basis van de huidige status van de instelling lab/environment. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Omgevings instelling lezen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/ResetPassword/action | Hiermee stelt u het wacht woord voor de virtuele machine van de sjabloon opnieuw in. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Hiermee wordt de huidige sjabloon afbeelding opgeslagen in de gedeelde galerie in het lab-account |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Schema's verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Planningen lezen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Schema's toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Start een sjabloon door alle resources in de sjabloon te starten. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Hiermee stopt u een sjabloon door alle resources in de sjabloon te stoppen. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Omgevings instelling toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/labs/read | Lees Labs. |
> | Action | Microsoft.LabServices/labAccounts/labs/SendEmail/action | E-mail met registratie koppeling naar het lab verzenden |
> | Action | Microsoft.LabServices/labAccounts/labs/users/delete | Gebruikers verwijderen. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/read | Gebruikers lezen. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/write | Gebruikers toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/labs/write | Labs toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/read | Lab-accounts lezen. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Verwijder sharedgalleries. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/read | Lees sharedgalleries. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/write | Sharedgalleries toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/delete | Verwijder sharedimages. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/read | Lees sharedimages. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/write | Sharedimages toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/labAccounts/write | Lab-accounts toevoegen of wijzigen. |
> | Action | Microsoft.LabServices/locations/operations/read | Lees bewerkingen. |
> | Action | Microsoft.LabServices/register/action | Hiermee wordt het abonnement geregistreerd |
> | Action | Microsoft.LabServices/users/GetOperationBatchStatus/action | Batch bewerkings status ophalen |
> | Action | Microsoft.LabServices/users/GetOperationStatus/action | Hiermee wordt de status van een langlopende bewerking opgehaald |
> | Action | Microsoft.LabServices/users/GetPersonalPreferences/action | Persoonlijke voor keuren voor een gebruiker ophalen |
> | Action | Microsoft.LabServices/users/ListAllEnvironments/action | Alle omgevingen voor de gebruiker weer geven |
> | Action | Microsoft.LabServices/users/Register/action | Een gebruiker registreren bij een beheerd Lab |
> | Action | Microsoft.LabServices/users/ResetPassword/action | Hiermee wordt het gebruikers wachtwoord opnieuw ingesteld op een omgeving |
> | Action | Microsoft.LabServices/users/StartEnvironment/action | Start een omgeving door alle resources in de omgeving te starten. |
> | Action | Microsoft.LabServices/users/StopEnvironment/action | Stopt een omgeving door alle resources in de omgeving te stoppen |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Logic/integrationAccounts/agreements/delete | Hiermee verwijdert u de overeenkomst in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor overeenkomstinhoud in het integratieaccount op. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/read | Hiermee leest u de overeenkomst in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/write | Hiermee maakt u de overeenkomst in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/delete | Hiermee verwijdert u de assembly in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor assemblyinhoud in het integratieaccount op. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/read | Hiermee leest u de assembly in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/write | Hiermee maakt u de assembly in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Hiermee verwijdert u de batchconfiguratie in het integratieaccount. |
> | Action | Micro soft. Logic/integrationAccounts/batchConfigurations/lezen | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Hiermee maakt u de batchconfiguratie in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/delete | Hiermee verwijdert u het certificaat in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/read | Hiermee leest u het certificaat in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/write | Hiermee maakt u het certificaat in het integratieaccount of werkt u het bij. |
> | Action | Microsoft.Logic/integrationAccounts/delete | Hiermee verwijdert u het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/join/action | Hiermee wordt het integratieaccount gekoppeld. |
> | Action | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor het integratieaccount op. |
> | Action | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Hiermee haalt u de sleutels in de sleutelkluis op. |
> | Action | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Hiermee registreert u traceringsgebeurtenissen in het integratieaccount in het logboek. |
> | Action | Microsoft.Logic/integrationAccounts/maps/delete | Hiermee verwijdert u de kaart in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor kaartinhoud in het integratieaccount op. |
> | Action | Micro soft. Logic/integrationAccounts/Maps/lezen | Hiermee leest u de kaart in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/maps/write | Hiermee maakt u de kaart in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/partners/delete | Hiermee verwijdert u de partner in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Hiermee geeft u de URL voor terugbellen voor partnerinhoud in het integratieaccount weer. |
> | Action | Microsoft.Logic/integrationAccounts/partners/read | Hiermee leest u de partner in het integratie account. |
> | Action | Microsoft.Logic/integrationAccounts/partners/write | Hiermee maakt u de partner in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de logboekdefinities van het integratieaccount gelezen. |
> | Action | Microsoft.Logic/integrationAccounts/read | Hiermee leest u het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/delete | Hiermee verwijdert u het schema in het integratie-account. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor schema-inhoud in het integratieaccount op. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/read | Hiermee leest u het schema in het integratieaccount. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/write | Hiermee maakt u het schema in het integratieaccount of werkt u het bij. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/delete | Hiermee verwijdert u de sessie in het integratieaccount. |
> | Action | Micro soft. Logic/integrationAccounts/sessies/lezen | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Action | Micro soft. Logic/integrationAccounts/sessies/schrijven | Hiermee maakt u de sessie in het integratieaccount of werkt u deze bij. |
> | Action | Microsoft.Logic/integrationAccounts/write | Hiermee maakt u het integratieaccount of werkt u het bij. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/delete | Hiermee kunt u de integratieserviceomgeving verwijderen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/join/action | Hiermee kunt u de integratieserviceomgeving koppelen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Hiermee wordt de bewerking van de door de integratieserviceomgeving beheerde API gelezen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Hiermee wordt de door de integratieserviceomgeving beheerde API gelezen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Hiermee worden de bewerkingsstatussen van de integratieserviceomgeving gelezen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities van de integratieserviceomgeving gelezen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/read | Hiermee kunt u de integratieserviceomgeving lezen. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/write | Hiermee kunt u de integratieserviceomgeving maken of bijwerken. |
> | Action | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Hiermee worden de aanbevolen bewerkingsgroepen van de werkstroom opgehaald. |
> | Action | Microsoft.Logic/locations/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
> | Action | Microsoft.Logic/operations/read | Hiermee haalt u de bewerking op. |
> | Action | Microsoft.Logic/register/action | Hiermee registreert u de Microsoft.Logic-resourceprovider voor een bepaald abonnement. |
> | Action | Microsoft.Logic/workflows/accessKeys/delete | Hiermee wordt de toegangssleutel verwijderd. |
> | Action | Microsoft.Logic/workflows/accessKeys/list/action | Hiermee wordt een lijst met toegangssleutelgeheimen gemaakt. |
> | Action | Microsoft.Logic/workflows/accessKeys/read | Hiermee wordt de toegangssleutel gelezen. |
> | Action | Microsoft.Logic/workflows/accessKeys/regenerate/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Action | Microsoft.Logic/workflows/accessKeys/write | Hiermee wordt de toegangssleutel opnieuw gemaakt of bijgewerkt. |
> | Action | Micro soft. Logic/werk stromen/verwijderen | Hiermee wordt de werkstroom verwijderd. |
> | Action | Microsoft.Logic/workflows/disable/action | Hiermee wordt de werkstroom uitgeschakeld. |
> | Action | Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
> | Action | Microsoft.Logic/workflows/listCallbackUrl/action | Hiermee wordt de URL voor terugbellen van de werkstroom opgehaald. |
> | Action | Microsoft.Logic/workflows/listSwagger/action | Hiermee worden de swaggerdefinities van de werkstroom opgehaald. |
> | Action | Microsoft.Logic/workflows/move/action | Hiermee wordt de werkstroom verplaatst van de bestaande abonnements-id, resourcegroep en/of naam naar een andere abonnements-id, resourcegroep en/of naam. |
> | Action | Micro soft. Logic/werk stromen/providers/micro soft. Insights/diagnosticSettings/lezen | Hiermee worden de diagnostische instellingen van de werkstroom gelezen. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden de diagnostische instelling van de werkstroom gemaakt of bijgewerkt. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de logboekdefinities van de werkstroom gelezen. |
> | Action | Micro soft. Logic/werk stromen/providers/micro soft. Insights/metricDefinitions/lezen | Hiermee worden de metrische definities van de werkstroom gelezen. |
> | Action | Microsoft.Logic/workflows/read | Hiermee wordt de werkstroom gelezen. |
> | Action | Microsoft.Logic/workflows/regenerateAccessKey/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Action | Microsoft.Logic/workflows/run/action | Hiermee wordt de werkstroomuitvoering gestart. |
> | Action | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Hiermee worden de expressietraceringen voor de uitvoeringsactie van de werkstroom opgehaald. |
> | Action | Microsoft.Logic/workflows/runs/actions/read | Hiermee wordt de uitvoeringsactie van de werkstroom gelezen. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Hiermee haalt u de expressietraceringen voor de herhaling van de uitvoeringsactie van de werkstroom op. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/read | Hiermee leest u de herhaling van de uitvoeringsactie van de werkstroom. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Hiermee wordt de aanvraaggeschiedenis van de herhalingsactie Werkstroom uitvoeren gelezen. |
> | Action | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Hiermee wordt de aanvraaggeschiedenis van de actie Werkstroom uitvoeren gelezen. |
> | Action | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Hiermee leest u de herhaling van het bereik van de uitvoeringsactie van de werkstroom. |
> | Action | Microsoft.Logic/workflows/runs/cancel/action | Hiermee wordt de werkstroomuitvoering geannuleerd. |
> | Action | Micro soft. Logic/werk stromen/uitvoeringen/verwijderen | Hiermee wordt een werkstroomuitvoering verwijderd. |
> | Action | Micro soft. Logic/werk stromen/uitvoeringen/bewerkingen/lezen | Hiermee wordt de bewerkingsstatus van de werkstroomuitvoering gelezen. |
> | Action | Micro soft. Logic/werk stromen/uitvoeringen/lezen | Hiermee wordt de werkstroomuitvoering gelezen. |
> | Action | Microsoft.Logic/workflows/suspend/action | Hiermee onderbreekt u de werkstroom. |
> | Action | Microsoft.Logic/workflows/triggers/histories/read | Hiermee wordt de geschiedenis van de triggers gelezen. |
> | Action | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Hiermee wordt de werkstroomtrigger opnieuw verzonden. |
> | Action | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Hiermee wordt de URL voor terugbellen van de trigger opgehaald. |
> | Action | Microsoft.Logic/workflows/triggers/read | Hiermee wordt de trigger gelezen. |
> | Action | Microsoft.Logic/workflows/triggers/reset/action | Hiermee stelt u de trigger opnieuw in. |
> | Action | Microsoft.Logic/workflows/triggers/run/action | Hiermee wordt de trigger uitgevoerd. |
> | Action | Microsoft.Logic/workflows/triggers/setState/action | Hiermee stelt u de status van de trigger in. |
> | Action | Microsoft.Logic/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
> | Action | Micro soft. Logic/werk stromen/versies/lezen | Hiermee wordt de werkstroomversie gelezen. |
> | Action | Micro soft. Logic/werk stromen/versies/triggers/listCallbackUrl/actie | Hiermee wordt de URL voor terugbellen van de trigger opgehaald. |
> | Action | Micro soft. Logic/werk stromen/schrijven | Hiermee wordt de werkstroom gemaakt of bijgewerkt. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Een Machine Learning Toezeggings plan koppeling verplaatsen |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Een Machine Learning Toezeggings plan koppeling lezen |
> | Action | Microsoft.MachineLearning/commitmentPlans/delete | Een Machine Learning Toezeggings plan verwijderen |
> | Action | Microsoft.MachineLearning/commitmentPlans/join/action | Een Machine Learning Toezeggings plan samen voegen |
> | Action | Microsoft.MachineLearning/commitmentPlans/read | Een Machine Learning Toezeggings plan lezen |
> | Action | Microsoft.MachineLearning/commitmentPlans/write | Een Machine Learning Toezeggings plan maken of bijwerken |
> | Action | Microsoft.MachineLearning/locations/operationresults/read | Resultaat van een Machine Learning bewerking ophalen |
> | Action | Microsoft.MachineLearning/locations/operationsstatus/read | De status van een doorlopende Machine Learning bewerking ophalen |
> | Action | Microsoft.MachineLearning/operations/read | Machine Learning-bewerkingen ophalen |
> | Action | Microsoft.MachineLearning/register/action | Hiermee wordt het abonnement voor de resource provider van de machine learning-webservice geregistreerd en wordt het maken van webservices ingeschakeld. |
> | Action | Microsoft.MachineLearning/skus/read | Sku's van Machine Learning Toezeggings plan ophalen |
> | Action | Microsoft.MachineLearning/webServices/action | Eigenschappen van regionale webservice maken voor ondersteunde regio's |
> | Action | Microsoft.MachineLearning/webServices/delete | Alle Machine Learning-webservice verwijderen |
> | Action | Microsoft.MachineLearning/webServices/listkeys/read | Sleutels naar een Machine Learning-webservice ophalen |
> | Action | Microsoft.MachineLearning/webServices/read | Alle Machine Learning-webservice lezen |
> | Action | Microsoft.MachineLearning/webServices/write | Een Machine Learning-webservice maken of bijwerken |
> | Action | Microsoft.MachineLearning/Workspaces/delete | Machine Learning-werkruimte verwijderen |
> | Action | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Lijst met sleutels voor een Machine Learning-werkruimte |
> | Action | Microsoft.MachineLearning/Workspaces/read | Alle Machine Learning-werkruimte lezen |
> | Action | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Sleutels van het opslag account die zijn geconfigureerd voor een Machine Learning-werkruimte, worden opnieuw gesynchroniseerd |
> | Action | Microsoft.MachineLearning/Workspaces/write | Machine Learning-werkruimte maken of bijwerken |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Hiermee wordt de status van een bepaalde reken bewerking opgehaald |
> | Action | Microsoft.MachineLearningServices/locations/usages/read | Gebruiks rapport voor AML-reken resources in een abonnement |
> | Action | Microsoft.MachineLearningServices/locations/vmsizes/read | Ondersteunde VM-grootten ophalen |
> | Action | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Hiermee wordt de status van een bepaalde werkruimte bewerking opgehaald |
> | Action | Microsoft.MachineLearningServices/register/action | Hiermee wordt het abonnement voor de Machine Learning Services resource provider geregistreerd |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/delete | Hiermee worden de reken resources in Machine Learning Services werk ruimte (n) verwijderd |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Geheimen vermelden voor reken resources in Machine Learning Services werk ruimte |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | Knoop punten voor Compute-resource in Machine Learning Services werk ruimte weer geven |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/read | Hiermee worden de reken resources in Machine Learning Services werk ruimte (n) opgehaald |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/write | Hiermee worden de reken resources in Machine Learning Services werk ruimte (n) gemaakt of bijgewerkt |
> | Action | Microsoft.MachineLearningServices/workspaces/delete | Hiermee verwijdert u de Machine Learning Services werk ruimte (n) |
> | DataAction | Micro soft. MachineLearningServices/werk ruimtes/experimenten/lezen | Hiermee worden experimenten in Machine Learning Services werk ruimte (n) opgehaald |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/write | Hiermee worden experimenten in Machine Learning Services werk ruimte (n) gemaakt of bijgewerkt |
> | Action | Microsoft.MachineLearningServices/workspaces/listKeys/action | Geheimen voor een Machine Learning Services-werk ruimte vermelden |
> | Action | Microsoft.MachineLearningServices/workspaces/read | Hiermee worden de Machine Learning Services werk ruimte (n) opgehaald |
> | Action | Microsoft.MachineLearningServices/workspaces/write | Hiermee wordt een Machine Learning Services werk ruimte (n) gemaakt of bijgewerkt |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ManagedIdentity/Identities/lezen | Hiermee wordt een bestaande systeem-id opgehaald |
> | Action | Microsoft.ManagedIdentity/register/action | Hiermee wordt het abonnement voor de resource provider van de beheerde identiteit geregistreerd |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC-actie voor het toewijzen van een bestaande door de gebruiker toegewezen identiteit aan een resource |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Hiermee wordt een bestaande door de gebruiker toegewezen identiteit verwijderd |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/read | Hiermee wordt een bestaande door de gebruiker toegewezen identiteit opgehaald |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/write | Hiermee wordt een nieuwe door de gebruiker toegewezen identiteit gemaakt of worden de labels bijgewerkt die zijn gekoppeld aan een bestaande door de gebruiker toegewezen identiteit |

## <a name="microsoftmanagedservices"></a>Micro soft. ManagedServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ManagedServices/marketplaceRegistrationDefinitions/lezen | Hiermee wordt een lijst met registratiedefinities voor beheerde services opgehaald. |
> | Action | Micro soft. ManagedServices/operationStatuses/lezen | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Action | Micro soft. ManagedServices/REGI ster/actie | Meld u aan bij beheerde services. |
> | Action | Micro soft. ManagedServices/registrationAssignments/verwijderen | Hiermee wordt de registratietoewijzing voor beheerde services verwijderd. |
> | Action | Micro soft. ManagedServices/registrationAssignments/lezen | Hiermee wordt een lijst met registratietoewijzingen voor beheerde services opgehaald. |
> | Action | Micro soft. ManagedServices/registrationAssignments/schrijven | Voeg de registratietoewijzing voor beheerde services toe of wijzig deze. |
> | Action | Micro soft. ManagedServices/registrationDefinitions/verwijderen | Hiermee wordt de registratiedefinitie voor beheerde services verwijderd. |
> | Action | Micro soft. ManagedServices/registrationDefinitions/lezen | Hiermee wordt een lijst met registratiedefinities voor beheerde services opgehaald. |
> | Action | Micro soft. ManagedServices/registrationDefinitions/schrijven | Voeg de registratiedefinitie voor beheerde services toe of wijzig deze. |
> | Action | Micro soft. ManagedServices/registratie/actie | Registratie bij beheerde services opheffen. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Management/checkNameAvailability/action | Hiermee wordt gecontroleerd of de opgegeven naam van de beheer groep geldig en uniek is. |
> | Action | Microsoft.Management/getEntities/action | Alle entiteiten (Beheergroepen, abonnementen enz.) weer geven voor de geverifieerde gebruiker. |
> | Action | Microsoft.Management/managementGroups/delete | Beheer groep verwijderen. |
> | Action | Microsoft.Management/managementGroups/read | Beheer groepen voor de geverifieerde gebruiker weer geven. |
> | Action | Microsoft.Management/managementGroups/subscriptions/delete | Het abonnement van de beheer groep wordt ontkoppeld. |
> | Action | Microsoft.Management/managementGroups/subscriptions/write | Het bestaande abonnement wordt gekoppeld aan de beheer groep. |
> | Action | Microsoft.Management/managementGroups/write | Een beheer groep maken of bijwerken. |
> | Action | Microsoft.Management/register/action | Het opgegeven abonnement registreren bij micro soft. Management |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Hiermee wordt het lezen van gegevens toegang verleend aan een Maps-account. |
> | Action | Microsoft.Maps/accounts/delete | Een Maps-account verwijderen. |
> | Action | Microsoft.Maps/accounts/eventGridFilters/delete | Een Event Grid filter verwijderen |
> | Action | Microsoft.Maps/accounts/eventGridFilters/read | Een Event Grid filter ophalen |
> | Action | Microsoft.Maps/accounts/eventGridFilters/write | Een Event Grid filter maken of bijwerken |
> | Action | Microsoft.Maps/accounts/listKeys/action | Lijst met account sleutels voor overzichten |
> | Action | Microsoft.Maps/accounts/read | Een Maps-account ophalen. |
> | Action | Microsoft.Maps/accounts/regenerateKey/action | Nieuwe Maps-account primaire of secundaire sleutel genereren |
> | Action | Microsoft.Maps/accounts/write | Een Maps-account maken of bijwerken. |
> | Action | Microsoft.Maps/operations/read | De provider bewerkingen lezen |
> | Action | Microsoft.Maps/register/action | De provider registreren |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Hiermee wordt een overeenkomst geretourneerd. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Hiermee wordt een ondertekende overeenkomst geaccepteerd. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Hiermee wordt een afbeelding geïmporteerd in het ACR van de eindgebruiker. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Hiermee wordt een configuratie geretourneerd. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Hiermee wordt een configuratie opgeslagen. |
> | Action | Microsoft.Marketplace/register/action | Hiermee wordt de Microsoft.Marketplace-resourceprovider in het abonnement geregistreerd. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/delete | Er wordt een Verwijder bewerking uitgevoerd op een klassieke dev service-resource. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Hiermee haalt u een service voor het beheer van bron-en klassieke resources. |
> | Action | Micro soft. MarketplaceApps/ClassicDevServices/listSingleSignOnToken/Action | Hiermee wordt de URL voor eenmalige aanmelding voor een klassieke dev service opgehaald. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/read | Voert een GET-bewerking uit op een klassieke dev service. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Hiermee genereert u een klassieke service voor het beheren van resource beheer sleutels. |
> | Action | Microsoft.MarketplaceApps/Operations/read | Lees de bewerkingen voor alle resource typen. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Een overeenkomst voor een bepaald Marketplace-item annuleren |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Een overeenkomst voor een gegeven Marketplace-item retour neren |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Een overeenkomst voor een bepaald Marketplace-item ondertekenen |
> | Action | Microsoft.MarketplaceOrdering/agreements/read | Alle overeenkomsten voor het opgegeven abonnement retour neren |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Een overeenkomst voor een bepaald item van een virtuele Marketplace-machine verkrijgen |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Een overeenkomst voor een bepaald item van een virtuele Marketplace-machine ondertekenen of annuleren |
> | Action | Microsoft.MarketplaceOrdering/operations/read | Alle mogelijke bewerkingen in de API weer geven |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Media/checknameavailability/action | Hiermee wordt gecontroleerd of een Media Services account naam beschikbaar is |
> | Action | Microsoft.Media/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of een Media Services account naam beschikbaar is |
> | Action | Microsoft.Media/mediaservices/accountfilters/delete | Een account filter verwijderen |
> | Action | Microsoft.Media/mediaservices/accountfilters/read | Een account filter lezen |
> | Action | Microsoft.Media/mediaservices/accountfilters/write | Een account filter maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/delete | Alle activa filters verwijderen |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/read | Alle activa filters lezen |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/write | Een Asset-filter maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/assets/delete | Alle activa verwijderen |
> | Action | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Versleutelings sleutel voor activa ophalen |
> | Action | Microsoft.Media/mediaservices/assets/listContainerSas/action | SAS-Url's voor de Asset-container vermelden |
> | Action | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Streaming-Locators vermelden voor Asset |
> | Action | Microsoft.Media/mediaservices/assets/read | Alle activa lezen |
> | Action | Microsoft.Media/mediaservices/assets/write | Activa maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Alle beleids regels voor inhouds sleutels verwijderen |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Beleids eigenschappen ophalen met geheimen |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/read | Alle beleids regels voor inhouds sleutels lezen |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/write | Beleid voor inhouds sleutels maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/delete | Een Media Services account verwijderen |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/delete | Event Grid filter verwijderen |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/read | Een Event Grid filter lezen |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/write | Event Grid filter maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/liveEventOperations/read | Een live gebeurtenis bewerking lezen |
> | Action | Microsoft.Media/mediaservices/liveEvents/delete | Een live gebeurtenis verwijderen |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Een live uitvoer verwijderen |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Een wille keurige live-uitvoer lezen |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Een live-uitvoer maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/liveEvents/read | Een live gebeurtenis lezen |
> | Action | Microsoft.Media/mediaservices/liveEvents/reset/action | Een live gebeurtenis bewerking opnieuw instellen |
> | Action | Microsoft.Media/mediaservices/liveEvents/start/action | Een live gebeurtenis bewerking starten |
> | Action | Microsoft.Media/mediaservices/liveEvents/stop/action | Een live gebeurtenis bewerking stoppen |
> | Action | Microsoft.Media/mediaservices/liveEvents/write | Een live gebeurtenis maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/liveOutputOperations/read | Een live uitvoer bewerking lezen |
> | Action | Microsoft.Media/mediaservices/read | Een Media Services account lezen |
> | Action | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Een streaming-eindpunt bewerking lezen |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/delete | Een streaming-eind punt verwijderen |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/read | Een streaming-eind punt lezen |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Een streaming-eindpunt bewerking schalen |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Een streaming-eindpunt bewerking starten |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Een streaming-eindpunt bewerking stoppen |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/write | Een streaming-eind punt maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/streamingLocators/delete | Een streaming-Locator verwijderen |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Inhouds sleutels weer geven |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Paden weer geven |
> | Action | Microsoft.Media/mediaservices/streamingLocators/read | Een streaming-Locator lezen |
> | Action | Microsoft.Media/mediaservices/streamingLocators/write | Een streaming-Locator maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/delete | Een streaming-beleid verwijderen |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/read | Elk streaming-beleid lezen |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/write | Een streaming-beleid maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/syncStorageKeys/action | De opslag sleutels voor een gekoppeld Azure Storage account synchroniseren |
> | Action | Microsoft.Media/mediaservices/transforms/delete | Trans formatie verwijderen |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Taak annuleren |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/delete | Taak verwijderen |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/read | Een wille keurige taak lezen |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/write | Een taak maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/transforms/read | Een trans formatie lezen |
> | Action | Microsoft.Media/mediaservices/transforms/write | Een trans formatie maken of bijwerken |
> | Action | Microsoft.Media/mediaservices/write | Een Media Services-account maken of bijwerken |
> | Action | Microsoft.Media/operations/read | Beschik bare bewerkingen ophalen |
> | Action | Microsoft.Media/register/action | Hiermee wordt het abonnement voor de Media Services resource provider geregistreerd en wordt het maken van Media Services accounts mogelijk |
> | Action | Microsoft.Media/unregister/action | Hiermee wordt de registratie van het abonnement voor de Media Services resource provider ongedaan gemaakt |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. migrate/assessmentprojects/assessmentOptions/Read | Hiermee worden de evaluatieopties opgehaald die op de opgegeven locatie beschikbaar zijn |
> | Action | Microsoft.Migrate/assessmentprojects/assessments/read | Hiermee geeft u een lijst met evaluaties binnen een project weer |
> | Action | Microsoft.Migrate/assessmentprojects/delete | Hiermee wordt het evaluatie project verwijderd |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/assessedmachines/read | De eigenschappen van een geëvalueerde machine ophalen |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/delete | Hiermee wordt een evaluatie verwijderd |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/downloadurl/action | Hiermee downloadt u de URL van een evaluatierapport |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/read | Hiermee haalt u de eigenschappen van een evaluatie op |
> | Action | Microsoft.Migrate/assessmentprojects/groups/assessments/write | Hiermee wordt een nieuwe evaluatie gemaakt of een bestaande evaluatie bijgewerkt |
> | Action | Microsoft.Migrate/assessmentprojects/groups/delete | Verwijdert een groep |
> | Action | Microsoft.Migrate/assessmentprojects/groups/read | De eigenschappen van een groep ophalen |
> | Action | Microsoft.Migrate/assessmentprojects/groups/updateMachines/action | Groep bijwerken door computers toe te voegen of te verwijderen |
> | Action | Microsoft.Migrate/assessmentprojects/groups/write | Hiermee wordt een nieuwe groep gemaakt of een bestaande groep bijgewerkt |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/delete | Hiermee verwijdert u de Hyper-Collector |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/read | Hiermee worden de eigenschappen van Hyper-Collector opgehaald |
> | Action | Microsoft.Migrate/assessmentprojects/hypervcollectors/write | Hiermee wordt een nieuwe Hyper-Collector gemaakt of een bestaande Hyper-Collector bijgewerkt |
> | Action | Micro soft. migrate/assessmentprojects/machines/lezen | Hiermee worden de eigenschappen van een machine opgehaald |
> | Action | Microsoft.Migrate/assessmentprojects/read | Hiermee worden de eigenschappen van het evaluatie project opgehaald |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/delete | VMware Collector verwijderen |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/read | Hiermee worden de eigenschappen van VMware Collector opgehaald |
> | Action | Microsoft.Migrate/assessmentprojects/vmwarecollectors/write | Hiermee wordt een nieuwe VMware Collector gemaakt of een bestaande VMware Collector bijgewerkt |
> | Action | Microsoft.Migrate/assessmentprojects/write | Hiermee maakt u een nieuw evaluatie project of werkt u een bestaand evaluatie project bij |
> | Action | Microsoft.Migrate/locations/assessmentOptions/read | Hiermee worden de evaluatieopties opgehaald die op de opgegeven locatie beschikbaar zijn |
> | Action | Microsoft.Migrate/locations/checknameavailability/action | Controleert de beschik baarheid van de resource naam voor het gegeven abonnement op de opgegeven locatie |
> | Action | Microsoft.Migrate/migrateprojects/DatabaseInstances/read | Hiermee worden de eigenschappen van een Data Base-exemplaar opgehaald |
> | Action | Microsoft.Migrate/migrateprojects/Databases/read | Hiermee worden de eigenschappen van een Data Base opgehaald |
> | Action | Microsoft.Migrate/migrateprojects/delete | Hiermee wordt een migratie project verwijderd |
> | Action | Microsoft.Migrate/migrateprojects/machines/read | Hiermee worden de eigenschappen van een machine opgehaald |
> | Action | Microsoft.Migrate/migrateprojects/MigrateEvents/Delete | Hiermee wordt een migratie gebeurtenis verwijderd |
> | Action | Microsoft.Migrate/migrateprojects/MigrateEvents/read | Hiermee worden de eigenschappen van een migratie gebeurtenis opgehaald. |
> | Action | Microsoft.Migrate/migrateprojects/read | Hiermee worden de eigenschappen van het migratie project opgehaald |
> | Action | Micro soft. migrate/migrateprojects/RefreshSummary/Action | Hiermee wordt het samen vatting van het migratie project vernieuwd |
> | Action | Microsoft.Migrate/migrateprojects/registerTool/action | Hulp programma registreren bij een migratie project |
> | Action | Microsoft.Migrate/migrateprojects/solutions/cleanupData/action | De oplossings gegevens van het migratie project opschonen |
> | Action | Micro soft. migrate/migrateprojects/Solutions/Delete | Hiermee verwijdert u een migratie project oplossing |
> | Action | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Hiermee wordt de configuratie van een migratieprojectoplossing opgehaald |
> | Action | Micro soft. migrate/migrateprojects/Solutions/Read | Hiermee worden de eigenschappen van de project-oplossing migreren opgehaald |
> | Action | Microsoft.Migrate/migrateprojects/solutions/write | Hiermee maakt u een nieuwe oplossing voor het migreren van een project of werkt u een bestaande migratie project oplossing bij |
> | Action | Microsoft.Migrate/migrateprojects/write | Hiermee maakt u een nieuw migratie project of werkt u een bestaand migratie project bij |
> | Action | Micro soft. migrate/Operations/Read | Hiermee worden bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Migrate-resourceprovider |
> | Action | Microsoft.Migrate/projects/assessments/read | Hiermee geeft u een lijst met evaluaties binnen een project weer |
> | Action | Microsoft.Migrate/projects/delete | Hiermee wordt het project verwijderd |
> | Action | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | De eigenschappen van een geëvalueerde machine ophalen |
> | Action | Microsoft.Migrate/projects/groups/assessments/delete | Hiermee wordt een evaluatie verwijderd |
> | Action | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Hiermee downloadt u de URL van een evaluatierapport |
> | Action | Microsoft.Migrate/projects/groups/assessments/read | Hiermee haalt u de eigenschappen van een evaluatie op |
> | Action | Microsoft.Migrate/projects/groups/assessments/write | Hiermee wordt een nieuwe evaluatie gemaakt of een bestaande evaluatie bijgewerkt |
> | Action | Microsoft.Migrate/projects/groups/delete | Verwijdert een groep |
> | Action | Microsoft.Migrate/projects/groups/read | De eigenschappen van een groep ophalen |
> | Action | Microsoft.Migrate/projects/groups/write | Hiermee wordt een nieuwe groep gemaakt of een bestaande groep bijgewerkt |
> | Action | Microsoft.Migrate/projects/keys/action | Hiermee worden de gedeelde sleutels voor het project opgehaald |
> | Action | Micro soft. migrate/projectes/machines/lezen | Hiermee worden de eigenschappen van een machine opgehaald |
> | Action | Microsoft.Migrate/projects/read | Hiermee worden de eigenschappen van een project opgehaald |
> | Action | Microsoft.Migrate/projects/write | Hiermee wordt een nieuw project gemaakt of een bestaand project bijgewerkt |
> | Action | Microsoft.Migrate/register/action | Hiermee wordt het abonnement bij de Microsoft.Migrate-resourceprovider geregistreerd |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.MixedReality/register/action | Registreert een abonnement voor de resource provider Mixed Reality. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Ruimtelijke ankers maken |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Ruimtelijke ankers verwijderen |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Ruimtelijke beankeringen in de buurt detecteren |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Eigenschappen van ruimtelijke ankers ophalen |
> | Action | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor micro soft. MixedReality/SpatialAnchorsAccounts opgehaald |
> | Action | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor micro soft. MixedReality/SpatialAnchorsAccounts gemaakt of bijgewerkt |
> | Action | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor micro soft. MixedReality/SpatialAnchorsAccounts |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ruimtelijke ankers zoeken |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Diagnostische gegevens verzenden om de kwaliteit van de Azure spatiale ankers-service te verbeteren |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Eigenschappen van ruimtelijke ankers bijwerken |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. NetApp/locaties/checkfilepathavailability/actie | Controleren of het bestandspad beschikbaar is |
> | Action | Micro soft. NetApp/locaties/checknameavailability/actie | Controleren of de resource naam beschikbaar is |
> | Action | Microsoft.NetApp/locations/operationresults/read | Hiermee wordt een resource van een bewerkings resultaat gelezen. |
> | Action | Microsoft.NetApp/locations/read | Hiermee wordt een resource voor beschikbaarheids controle gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Hiermee verwijdert u een groeps resource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/read | Hiermee wordt een pool resource gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Hiermee verwijdert u een volume resource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Hiermee wordt een bron van het doel van een koppeling gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Hiermee wordt een volume bron gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Hiermee verwijdert u een momentopname resource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Hiermee wordt een momentopname resource gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Schrijft een momentopname resource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Schrijft een volume resource. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/write | Schrijft een pool resource. |
> | Action | Microsoft.NetApp/netAppAccounts/delete | Hiermee verwijdert u een account bron. |
> | Action | Microsoft.NetApp/netAppAccounts/read | Hiermee wordt een account resource gelezen. |
> | Action | Microsoft.NetApp/netAppAccounts/write | Schrijft een account bron. |
> | Action | Microsoft.NetApp/Operations/read | Hiermee worden een bewerkings resources gelezen. |
> | Action | Micro soft. NetApp/REGI ster/actie | Hiermee wordt een abonnement geregistreerd bij de resource provider micro soft. NetApp |
> | Action | Micro soft. NetApp/registratie/actie | Registratie van het abonnement met de resource provider micro soft. NetApp is ongedaan gemaakt |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Application Gateway beschik bare aanvraag headers ophalen |
> | Action | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Application Gateway beschik bare reactie header ophalen |
> | Action | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Application Gateway beschik bare Server variabelen ophalen |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Application Gateway vooraf gedefinieerd SSL-beleid |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Beschik bare SSL-opties Application Gateway |
> | Action | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Hiermee worden Application Gateway beschik bare WAF-regel sets opgehaald |
> | Action | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Voegt een back-end-adres groep van een toepassings gateway samen. Niet Alertable. |
> | Action | Microsoft.Network/applicationGateways/backendhealth/action | Hiermee wordt een back-upstatus van de toepassings gateway opgehaald |
> | Action | Microsoft.Network/applicationGateways/delete | Hiermee verwijdert u een toepassings gateway |
> | Action | Micro soft. Network/applicationGateways/getBackendHealthOnDemand/Action | Hiermee wordt een back-end van de toepassings gateway opgehaald voor de opgegeven http-instelling en back-end-pool |
> | Action | Microsoft.Network/applicationGateways/read | Hiermee wordt een toepassings gateway opgehaald |
> | Action | Microsoft.Network/applicationGateways/start/action | Start een toepassings gateway |
> | Action | Microsoft.Network/applicationGateways/stop/action | Stopt een toepassings gateway |
> | Action | Microsoft.Network/applicationGateways/write | Hiermee wordt een toepassings gateway gemaakt of een toepassings gateway bijgewerkt |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Hiermee wordt een Application Gateway WAF-beleid verwijderd |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Hiermee wordt een Application Gateway WAF-beleid opgehaald |
> | Action | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Hiermee maakt u een Application Gateway WAF-beleid of werkt u een Application Gateway WAF-beleid bij |
> | Action | Microsoft.Network/applicationSecurityGroups/delete | Hiermee verwijdert u een toepassings beveiligings groep |
> | Action | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Voegt een IP-configuratie toe aan toepassings beveiligings groepen. Niet alertable. |
> | Action | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Er wordt een beveiligings regel toegevoegd aan toepassings beveiligings groepen. Niet alertable. |
> | Action | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Een lijst met IP-configuraties in de ApplicationSecurityGroup |
> | Action | Microsoft.Network/applicationSecurityGroups/read | Hiermee wordt een toepassings beveiligings groep-ID opgehaald. |
> | Action | Microsoft.Network/applicationSecurityGroups/write | Hiermee wordt een toepassings beveiligings groep gemaakt of een bestaande toepassings beveiligings groep bijgewerkt. |
> | Action | Microsoft.Network/azureFirewallFqdnTags/read | Hiermee worden Azure Firewall FQDN-Tags opgehaald |
> | Action | Microsoft.Network/azurefirewalls/delete | Azure Firewall verwijderen |
> | Action | Microsoft.Network/azurefirewalls/read | Azure Firewall ophalen |
> | Action | Microsoft.Network/azurefirewalls/write | Hiermee wordt een Azure Firewall gemaakt of bijgewerkt |
> | Action | Microsoft.Network/bastionHosts/delete | Hiermee wordt een bastion-host verwijderd |
> | Action | Microsoft.Network/bastionHosts/read | Hiermee wordt een bastion-host opgehaald |
> | Action | Microsoft.Network/bastionHosts/write | Een bastion-host maken of bijwerken |
> | Action | Microsoft.Network/bgpServiceCommunities/read | BGP-service Community's ophalen |
> | Action | Microsoft.Network/checkFrontDoorNameAvailability/action | Hiermee wordt gecontroleerd of de naam van de voor deur beschikbaar is |
> | Action | Microsoft.Network/checkTrafficManagerNameAvailability/action | Controleert de beschik baarheid van een Traffic Manager relatieve DNS-naam. |
> | Action | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Action | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Action | Microsoft.Network/connections/revoke/action | Hiermee wordt de verbindings status van een Express-route gemarkeerd als ingetrokken |
> | Action | Microsoft.Network/connections/sharedkey/action | VirtualNetworkGatewayConnection SharedKey ophalen |
> | Action | Microsoft.Network/connections/sharedKey/read | Hiermee wordt VirtualNetworkGatewayConnection SharedKey opgehaald |
> | Action | Microsoft.Network/connections/sharedKey/write | Hiermee wordt een bestaande VirtualNetworkGatewayConnection-SharedKey gemaakt of bijgewerkt |
> | Action | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Hiermee wordt de configuratie van het VPN-apparaat van VirtualNetworkGatewayConnection |
> | Action | Microsoft.Network/connections/write | Hiermee wordt een bestaande VirtualNetworkGatewayConnection gemaakt of bijgewerkt |
> | Action | Microsoft.Network/ddosCustomPolicies/delete | Hiermee wordt een aangepast DDoS-beleid verwijderd |
> | Action | Microsoft.Network/ddosCustomPolicies/read | Hiermee wordt een DDoS-aangepaste definitie van de beleids definitie opgehaald |
> | Action | Microsoft.Network/ddosCustomPolicies/write | Hiermee maakt u een aangepast DDoS-beleid of werkt u een bestaand aangepast DDoS-beleid bij |
> | Action | Microsoft.Network/ddosProtectionPlans/delete | Hiermee verwijdert u een DDoS Protection Plan |
> | Action | Microsoft.Network/ddosProtectionPlans/join/action | Voegt een DDoS Protection Plan samen. Niet alertable. |
> | Action | Microsoft.Network/ddosProtectionPlans/read | Hiermee wordt een DDoS Protection Plan opgehaald |
> | Action | Microsoft.Network/ddosProtectionPlans/write | Hiermee maakt u een DDoS Protection Plan of werkt u een DDoS Protection-abonnement bij  |
> | Action | Microsoft.Network/dnsoperationresults/read | Hiermee worden de resultaten van een DNS-bewerking opgehaald |
> | Action | Microsoft.Network/dnsoperationstatuses/read | Hiermee wordt de status van een DNS-bewerking opgehaald  |
> | Action | Microsoft.Network/dnszones/A/delete | Verwijder de recordset van een bepaalde naam en typ ' A ' van een DNS-zone. |
> | Action | Microsoft.Network/dnszones/A/read | De recordset van het type ' A ' ophalen in de JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/A/write | Een recordset van het type ' A ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/AAAA/delete | Verwijder de recordset van een bepaalde naam en typ ' AAAA ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/AAAA/read | Haal de recordset van het type ' AAAA ' op in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/AAAA/write | Een recordset van het type ' AAAA ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/all/read | Hiermee worden DNS-record sets in verschillende typen opgehaald |
> | Action | Microsoft.Network/dnszones/CAA/delete | Verwijder de recordset van een bepaalde naam en typ ' CAA ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/CAA/read | De recordset ophalen van het type ' CAA ', in JSON-indeling. De recordset bevat de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/CAA/write | Een recordset van het type ' CAA ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/CNAME/delete | Verwijder de recordset van een bepaalde naam en typ ' CNAME ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/CNAME/read | De recordset van het type CNAME ophalen in de JSON-indeling. De recordset bevat de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/CNAME/write | Een recordset van het type CNAME maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/delete | Verwijder de DNS-zone in JSON-indeling. De zone-eigenschappen zijn onder andere tags, ETAG, numberOfRecordSets en maxNumberOfRecordSets. |
> | Action | Microsoft.Network/dnszones/MX/delete | Verwijder de recordset van een bepaalde naam en typ ' MX ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/MX/read | Haal de recordset van het type ' MX ' op in de JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/MX/write | Een recordset van het type ' MX ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/NS/delete | Hiermee wordt de DNS-recordset van het type NS verwijderd |
> | Action | Microsoft.Network/dnszones/NS/read | Hiermee wordt een DNS-recordset van het type NS opgehaald |
> | Action | Microsoft.Network/dnszones/NS/write | Hiermee wordt een DNS-recordset van het type NS gemaakt of bijgewerkt |
> | Action | Microsoft.Network/dnszones/PTR/delete | Verwijder de recordset van een bepaalde naam en typ ' PTR ' van een DNS-zone. |
> | Action | Microsoft.Network/dnszones/PTR/read | Haal de recordset van het type ' PTR ' op in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/PTR/write | Een recordset van het type ' PTR ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/read | Haal de DNS-zone op in JSON-indeling. De zone-eigenschappen zijn onder andere tags, ETAG, numberOfRecordSets en maxNumberOfRecordSets. Houd er rekening mee dat met deze opdracht de record sets in de zone niet worden opgehaald. |
> | Action | Microsoft.Network/dnszones/recordsets/read | Hiermee worden DNS-record sets in verschillende typen opgehaald |
> | Action | Microsoft.Network/dnszones/SOA/read | Hiermee wordt een DNS-recordset van het type SOA opgehaald |
> | Action | Microsoft.Network/dnszones/SOA/write | Hiermee wordt een DNS-recordset van het type SOA gemaakt of bijgewerkt |
> | Action | Microsoft.Network/dnszones/SRV/delete | Verwijder de recordset van een bepaalde naam en typ ' SRV ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/SRV/read | De recordset van het type SRV ophalen in de JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/SRV/write | Een recordset van het type SRV maken of bijwerken |
> | Action | Microsoft.Network/dnszones/TXT/delete | Verwijder de recordset van een bepaalde naam en typ ' TXT ' uit een DNS-zone. |
> | Action | Microsoft.Network/dnszones/TXT/read | Haal de recordset van het type ' TXT ' op in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/dnszones/TXT/write | Een recordset van het type ' TXT ' maken of bijwerken binnen een DNS-zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/dnszones/write | Een DNS-zone binnen een resource groep maken of bijwerken.  Wordt gebruikt om de tags op een DNS-zone resource bij te werken. Houd er rekening mee dat deze opdracht niet kan worden gebruikt om record sets in de zone te maken of bij te werken. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/delete | Hiermee verwijdert u een Expressroute-autorisatie |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/read | Hiermee wordt een Expressroute-autorisatie opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/write | Hiermee wordt een bestaande Expressroute-autorisatie gemaakt of bijgewerkt |
> | Action | Microsoft.Network/expressRouteCircuits/delete | Hiermee wordt een Expressroute verwijderd |
> | Action | Microsoft.Network/expressRouteCircuits/join/action | Voegt een Express route-Circuit samen. Niet alertable. |
> | Action | Micro soft. Network/expressRouteCircuits/peering/arpTables/lezen | Hiermee wordt een Expressroute-peering-ArpTable opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Hiermee verwijdert u een Expressroute-verbinding |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Hiermee wordt een Expressroute-verbinding opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Hiermee wordt een bestaande Expressroute-verbindings resource gemaakt of bijgewerkt |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/delete | Hiermee wordt een Expressroute-peering verwijderd |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Hiermee wordt een verbinding met een peer Express route-Circuit opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/read | Hiermee wordt een Expressroute-peering opgehaald |
> | Action | Micro soft. Network/expressRouteCircuits/peering/routeTables/lezen | Hiermee wordt een Expressroute-peering-RouteTable opgehaald |
> | Action | Micro soft. Network/expressRouteCircuits/peering/routeTablesSummary/lezen | Hiermee wordt een Expressroute-peering RouteTable-samen vatting opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Hiermee wordt een Expressroute-peering-stat opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/write | Hiermee wordt een bestaande Expressroute-peering gemaakt of bijgewerkt |
> | Action | Microsoft.Network/expressRouteCircuits/read | Een Expressroute ophalen |
> | Action | Microsoft.Network/expressRouteCircuits/stats/read | Hiermee wordt een Expressroute-stat opgehaald |
> | Action | Microsoft.Network/expressRouteCircuits/write | Hiermee wordt een bestaande Expressroute gemaakt of bijgewerkt |
> | Action | Microsoft.Network/expressRouteCrossConnections/join/action | Voegt een snelle route-cross-verbinding samen. Niet alertable. |
> | Action | Micro soft. Network/expressRouteCrossConnections/peering/arpTables/lezen | Hiermee wordt een ARP-tabel van een snelle route Kruis verbinding gemaakt |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Hiermee wordt een snelle route Kruis verbindings peering verwijderd |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/read | Hiermee wordt een snelle route Kruis verbindings peering opgehaald |
> | Action | Micro soft. Network/expressRouteCrossConnections/peering/routeTables/lezen | Hiermee wordt een route tabel voor de peering van een snelle route Kruis verbinding opgehaald |
> | Action | Micro soft. Network/expressRouteCrossConnections/peering/routeTableSummary/lezen | Hiermee wordt een samen vatting van een routerings tabel met snelle route Kruis verbinding opgehaald |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/write | Hiermee maakt u een snelle route Kruis verbinding-peering of werkt u een bestaande Express route Kruis verbindings peering bij |
> | Action | Microsoft.Network/expressRouteCrossConnections/read | Snelle route-Kruis verbinding verkrijgen |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Hiermee verwijdert u een snelle route verbinding |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Hiermee wordt een snelle route verbinding opgehaald |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Hiermee maakt u een snelle route verbinding of werkt u een bestaande Express route-verbinding bij |
> | Action | Microsoft.Network/expressRouteGateways/join/action | Voegt een Express route-gateway samen. Niet alertable. |
> | Action | Microsoft.Network/expressRouteGateways/read | Express route-gateway ophalen |
> | Action | Microsoft.Network/expressRoutePorts/delete | Hiermee worden ExpressRoutePorts verwijderd |
> | Action | Microsoft.Network/expressRoutePorts/join/action | Snelle route poorten worden toegevoegd. Niet alertable. |
> | Action | Microsoft.Network/expressRoutePorts/links/read | Gets ExpressRouteLink |
> | Action | Microsoft.Network/expressRoutePorts/read | Hiermee wordt ExpressRoutePorts opgehaald |
> | Action | Microsoft.Network/expressRoutePorts/write | ExpressRoutePorts maken of bijwerken |
> | Action | Microsoft.Network/expressRoutePortsLocations/read | Locaties voor Express route poorten ophalen |
> | Action | Microsoft.Network/expressRouteServiceProviders/read | Hiermee worden snelle Route Service providers opgehaald |
> | Action | Micro soft. Network/firewallPolicies/verwijderen | Hiermee wordt een firewall beleid verwijderd |
> | Action | Micro soft. Network/firewallPolicies/samen voegen/actie | Er wordt lid van een firewall-beleid. Niet alertable. |
> | Action | Micro soft. Network/firewallPolicies/lezen | Hiermee wordt een firewall beleid opgehaald |
> | Action | Micro soft. Network/firewallPolicies/ruleGroups/verwijderen | Hiermee wordt een firewall beleidsregel groep verwijderd |
> | Action | Micro soft. Network/firewallPolicies/ruleGroups/lezen | Hiermee wordt een firewall beleidsregel groep opgehaald |
> | Action | Micro soft. Network/firewallPolicies/ruleGroups/schrijven | Hiermee wordt een firewall beleidsregel groep gemaakt of een bestaande firewall beleids regel groep bijgewerkt |
> | Action | Micro soft. Network/firewallPolicies/schrijven | Hiermee wordt een firewall beleid gemaakt of een bestaand firewall beleid bijgewerkt |
> | Action | Microsoft.Network/frontDoors/backendPools/delete | Hiermee verwijdert u een back-end-groep |
> | Action | Microsoft.Network/frontDoors/backendPools/read | Hiermee wordt een back-end-pool opgehaald |
> | Action | Microsoft.Network/frontDoors/backendPools/write | Hiermee wordt een back-end-groep gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/delete | Hiermee verwijdert u een voor deur |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/delete | Hiermee wordt een frontend-eind punt verwijderd |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Hiermee wordt HTTPS uitgeschakeld op een frontend-eind punt |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | HTTPS inschakelen op een frontend-eind punt |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/read | Hiermee wordt een frontend-eind punt opgehaald |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/write | Hiermee wordt een frontend-eind punt gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/delete | Hiermee worden de instellingen voor de status test verwijderd |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/read | Hiermee worden de status test instellingen opgehaald |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/write | Hiermee worden instellingen voor de status test gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Hiermee worden instellingen voor taak verdeling gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/read | Hiermee worden de instellingen voor taak verdeling opgehaald |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/write | Hiermee worden instellingen voor taak verdeling gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/purge/action | Inhoud in cache verwijderen van een front deur |
> | Action | Microsoft.Network/frontDoors/read | Hiermee wordt een voor deur opgehaald |
> | Action | Microsoft.Network/frontDoors/routingRules/delete | Hiermee verwijdert u een routerings regel |
> | Action | Microsoft.Network/frontDoors/routingRules/read | Hiermee wordt een routerings regel opgehaald |
> | Action | Microsoft.Network/frontDoors/routingRules/write | Hiermee wordt een routerings regel gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoors/validateCustomDomain/action | Hiermee wordt een frontend-eind punt gevalideerd voor een front-deur |
> | Action | Microsoft.Network/frontDoors/write | Hiermee wordt een voor deur gemaakt of bijgewerkt |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallManagedRuleSets/read | Hiermee worden beheerde regel sets voor Web Application firewall opgehaald |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Hiermee wordt een firewall beleid voor webtoepassingen verwijderd |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Hiermee wordt een Web Application firewall-beleid opgehaald |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Hiermee wordt een firewall beleid voor webtoepassingen gemaakt of bijgewerkt |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Voegt een load balancer back-end-adres groep samen. Niet Alertable. |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/read | Hiermee wordt een load balancer back-end-adres groep definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/delete | Hiermee verwijdert u een load balancer |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Voegt een Load Balancer frontend-IP-configuratie. Niet alertable. |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Hiermee wordt een load balancer frontend-IP-configuratie definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Voegt een load balancer binnenkomende NAT-pool samen. Niet alertable. |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/read | Hiermee wordt een load balancer binnenkomende NAT-groeps definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/delete | Hiermee wordt een load balancer binnenkomende NAT-regel verwijderd |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Voegt een load balancer binnenkomende NAT-regel. Niet Alertable. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/read | Hiermee wordt een load balancer binnenkomende NAT-regel definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/write | Hiermee maakt u een load balancer binnenkomende NAT-regel of werkt u een bestaande load balancer binnenkomende NAT-regel bij |
> | Action | Microsoft.Network/loadBalancers/loadBalancingRules/read | Hiermee wordt een load balancer de definitie van de taakverdelings regel opgehaald |
> | Action | Microsoft.Network/loadBalancers/networkInterfaces/read | Hiermee wordt verwezen naar alle netwerk interfaces onder een load balancer |
> | Action | Microsoft.Network/loadBalancers/outboundRules/read | Hiermee wordt een load balancer uitgaande regel definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/probes/join/action | Staat het gebruik van tests van een load balancer toe. Met deze machtiging healthProbe eigenschap van de VM-schaalset kan bijvoorbeeld naar de test worden verwezen. Niet alertable. |
> | Action | Microsoft.Network/loadBalancers/probes/read | Hiermee wordt een load balancer-test opgehaald |
> | Action | Microsoft.Network/loadBalancers/read | Hiermee wordt een load balancer definitie opgehaald |
> | Action | Microsoft.Network/loadBalancers/virtualMachines/read | Hiermee worden verwijzingen naar alle virtuele machines onder een load balancer opgehaald |
> | Action | Microsoft.Network/loadBalancers/write | Hiermee maakt u een load balancer of werkt u een bestaande load balancer bij |
> | Action | Microsoft.Network/localnetworkgateways/delete | Deletes LocalNetworkGateway |
> | Action | Microsoft.Network/localnetworkgateways/read | Gets LocalNetworkGateway |
> | Action | Microsoft.Network/localnetworkgateways/write | Hiermee wordt een bestaande LocalNetworkGateway gemaakt of bijgewerkt |
> | Action | Microsoft.Network/locations/autoApprovedPrivateLinkServices/read | Hiermee worden automatisch goedgekeurde persoonlijke koppelings services opgehaald |
> | Action | Microsoft.Network/locations/availableDelegations/read | Beschik bare delegaties ophalen |
> | Action | Microsoft.Network/locations/availablePrivateEndpointTypes/read | Beschik bare persoonlijke eindpunt resources ophalen |
> | Action | Microsoft.Network/locations/bareMetalTenants/action | Hiermee wordt een bare metal-Tenant toegewezen of gevalideerd |
> | Action | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Hiermee wordt de ondersteuning voor versnelde netwerken gecontroleerd |
> | Action | Microsoft.Network/locations/checkDnsNameAvailability/read | Controleert of het DNS-label beschikbaar is op de opgegeven locatie |
> | Action | Microsoft.Network/locations/checkPrivateLinkServiceVisibility/action | Controleert de zicht baarheid van de persoonlijke koppelings service |
> | Action | Microsoft.Network/locations/operationResults/read | Hiermee wordt het bewerkings resultaat van een asynchrone bericht-of verwijder bewerking opgehaald |
> | Action | Microsoft.Network/locations/operations/read | Hiermee wordt de bewerkings resource opgehaald die de status van een asynchrone bewerking vertegenwoordigt |
> | Action | Microsoft.Network/locations/serviceTags/read | Service Tags ophalen |
> | Action | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Hiermee worden de ondersteunde grootten van virtuele machines opgehaald |
> | Action | Microsoft.Network/locations/usages/read | Hiermee worden de metrische gegevens over het bronnen gebruik opgehaald |
> | Action | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Hiermee wordt een lijst met beschik bare Virtual Network endpoint services opgehaald |
> | Action | Microsoft.Network/networkIntentPolicies/delete | Hiermee wordt een beleid voor netwerk intentie verwijderd |
> | Action | Microsoft.Network/networkIntentPolicies/read | Hiermee wordt een beschrijving van het beleid voor netwerk intentie opgehaald |
> | Action | Microsoft.Network/networkIntentPolicies/write | Hiermee wordt een beleid voor netwerk intentie gemaakt of een bestaand netwerk intentie beleid bijgewerkt |
> | Action | Microsoft.Network/networkInterfaces/delete | Hiermee wordt een netwerk interface verwijderd |
> | Action | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Netwerk beveiligings groepen die zijn geconfigureerd op de netwerk interface van de virtuele machine ophalen |
> | Action | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Route tabel ophalen die is geconfigureerd op de netwerk interface van de virtuele machine |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Voegt een IP-configuratie van een netwerk interface toe. Niet alertable. |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/read | Hiermee wordt een IP-configuratie definitie van de netwerk interface opgehaald.  |
> | Action | Microsoft.Network/networkInterfaces/join/action | Voegt een virtuele machine toe aan een netwerk interface. Niet Alertable. |
> | Action | Microsoft.Network/networkInterfaces/loadBalancers/read | Hiermee worden alle load balancers opgehaald waarvan de netwerk interface deel uitmaakt |
> | Action | Microsoft.Network/networkInterfaces/read | Hiermee haalt u een definitie van een netwerk interface.  |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Hiermee verwijdert u een netwerk interface tikt u op configuratie. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/read | Hiermee wordt een netwerk interface op configuratie opgehaald. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/write | Hiermee maakt u een netwerk interface tikt u op configuratie of werkt u een bestaande netwerk interface op configuratie tikken. |
> | Action | Microsoft.Network/networkInterfaces/write | Hiermee maakt u een netwerk interface of werkt u een bestaande netwerk interface bij.  |
> | Action | Microsoft.Network/networkProfiles/delete | Hiermee wordt een netwerk profiel verwijderd |
> | Action | Microsoft.Network/networkProfiles/read | Hiermee wordt een netwerk profiel opgehaald |
> | Action | Microsoft.Network/networkProfiles/removeContainers/action | Containers verwijderen |
> | Action | Microsoft.Network/networkProfiles/setContainers/action | Containers instellen |
> | Action | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Netwerk interfaces voor containers instellen |
> | Action | Microsoft.Network/networkProfiles/write | Hiermee wordt een netwerk profiel gemaakt of bijgewerkt |
> | Action | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Hiermee wordt een standaard beveiligings regel definitie opgehaald |
> | Action | Microsoft.Network/networkSecurityGroups/delete | Hiermee wordt een netwerk beveiligings groep verwijderd |
> | Action | Microsoft.Network/networkSecurityGroups/join/action | Er wordt lid van een netwerk beveiligings groep. Niet Alertable. |
> | Action | Microsoft.Network/networkSecurityGroups/read | Hiermee wordt een definitie van een netwerk beveiligings groep opgehaald |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/delete | Hiermee verwijdert u een beveiligings regel |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/read | Hiermee wordt een definitie van een beveiligings regel opgehaald |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/write | Hiermee maakt u een beveiligings regel of werkt u een bestaande beveiligings regel bij |
> | Action | Microsoft.Network/networkSecurityGroups/write | Hiermee wordt een netwerk beveiligings groep gemaakt of een bestaande netwerk beveiligings groep bijgewerkt |
> | Action | Microsoft.Network/networkWatchers/availableProvidersList/action | Retourneert alle beschik bare Internet serviceproviders voor een opgegeven Azure-regio. |
> | Action | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Retourneert de relatieve latentie score voor Internet serviceproviders van een opgegeven locatie naar Azure-regio's. |
> | Action | Microsoft.Network/networkWatchers/configureFlowLog/action | Hiermee configureert u de logboek registratie van de stroom voor een doel bron. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/delete | Hiermee wordt een verbindings monitor verwijderd |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Query bewakings connectiviteit tussen opgegeven eind punten |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/read | Details van de verbindings monitor ophalen |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Bewakings verbinding tussen opgegeven eind punten starten |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Controle connectiviteit tussen opgegeven eind punten stoppen/onderbreken |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/write | Hiermee maakt u een verbindings monitor |
> | Action | Microsoft.Network/networkWatchers/connectivityCheck/action | Hiermee wordt de mogelijkheid voor het tot stand brengen van een directe TCP-verbinding vanaf een virtuele machine naar een bepaald eind punt, inclusief een andere VM of een wille keurige externe server, gecontroleerd. |
> | Action | Microsoft.Network/networkWatchers/delete | Hiermee wordt een netwerk-Watcher verwijderd |
> | Action | Microsoft.Network/networkWatchers/ipFlowVerify/action | Retourneert of het pakket wordt toegestaan of geweigerd van een bepaalde bestemming. |
> | Action | Microsoft.Network/networkWatchers/lenses/delete | Hiermee wordt een lens verwijderd |
> | Action | Microsoft.Network/networkWatchers/lenses/query/action | Query's bewaken van netwerk verkeer op een opgegeven eind punt |
> | Action | Microsoft.Network/networkWatchers/lenses/read | Lens Details ophalen |
> | Action | Microsoft.Network/networkWatchers/lenses/start/action | Bewaken van netwerk verkeer op een opgegeven eind punt starten |
> | Action | Microsoft.Network/networkWatchers/lenses/stop/action | Bewaken van netwerk verkeer op een opgegeven eind punt stoppen/onderbreken |
> | Action | Microsoft.Network/networkWatchers/lenses/write | Hiermee maakt u een lens |
> | Action | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Diagnose van de netwerk configuratie. |
> | Action | Microsoft.Network/networkWatchers/nextHop/action | Voor een opgegeven doel-en doel-IP-adres retourneert het type van de volgende hop en de volgende hoop IP-adres. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/delete | Hiermee wordt een pakket opname verwijderd |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Haalt informatie op over de eigenschappen en status van een pakket opname bron. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/read | De definitie voor pakket opname ophalen |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Stop de actieve sessie voor het vastleggen van pakketten. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/write | Hiermee maakt u een pakket opname |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/delete | Hiermee wordt een PingMesh verwijderd |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/read | Details van PingMesh ophalen |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/start/action | PingMesh tussen opgegeven Vm's starten |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/stop/action | PingMesh tussen opgegeven Vm's stoppen |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/write | Hiermee maakt u een PingMesh |
> | Action | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Controle van de connectiviteit voor batch query's tussen opgegeven eind punten |
> | Action | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Hiermee wordt de status van de logboek registratie van de stroom op een resource opgehaald. |
> | Action | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Hiermee wordt het probleem voor het oplossen van problemen met de eerder uitgevoerde of actieve probleemoplossings bewerking opgehaald. |
> | Action | Microsoft.Network/networkWatchers/read | De Network Watcher-definitie ophalen |
> | Action | Microsoft.Network/networkWatchers/securityGroupView/action | De geconfigureerde en efficiënte regels voor netwerk beveiligings groepen weer geven die zijn toegepast op een virtuele machine. |
> | Action | Microsoft.Network/networkWatchers/topology/action | Hiermee wordt een weer gave op netwerk niveau van resources en de bijbehorende relaties in een resource groep opgehaald. |
> | Action | Microsoft.Network/networkWatchers/troubleshoot/action | Hiermee wordt het oplossen van problemen met een netwerk bron in azure gestart. |
> | Action | Microsoft.Network/networkWatchers/write | Hiermee wordt een netwerk-Watcher gemaakt of een bestaande netwerk-Watcher bijgewerkt |
> | Action | Microsoft.Network/operations/read | Beschik bare bewerkingen ophalen |
> | Action | Microsoft.Network/p2sVpnGateways/delete | Hiermee verwijdert u een P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | VPN-profiel voor P2SVpnGateway genereren |
> | Action | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Hiermee wordt een P2S VPN-verbindings status opgehaald voor P2SVpnGateway |
> | Action | Microsoft.Network/p2sVpnGateways/read | Hiermee wordt een P2SVpnGateway opgehaald. |
> | Action | Microsoft.Network/p2sVpnGateways/write | Hiermee wordt een P2SVpnGateway geplaatst. |
> | Action | Microsoft.Network/privateDnsOperationResults/read | Hiermee worden de resultaten van een Privé-DNS bewerking opgehaald |
> | Action | Microsoft.Network/privateDnsOperationStatuses/read | Hiermee wordt de status van een Privé-DNS bewerking opgehaald |
> | Action | Microsoft.Network/privateDnsZones/A/delete | Verwijder de recordset van een bepaalde naam en typ ' A ' in een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/A/read | De recordset van het type ' A ' ophalen binnen een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/A/write | Een recordset van het type ' A ' in een Privé-DNS zone maken of bijwerken. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/AAAA/delete | Verwijder de recordset van een bepaalde naam en typ ' AAAA ' uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/AAAA/read | Haal de recordset van het type ' AAAA ' op in een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/AAAA/write | Een recordset van het type ' AAAA ' maken of bijwerken binnen een Privé-DNS zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/ALL/read | Hiermee worden Privé-DNS record sets in verschillende typen opgehaald |
> | Action | Microsoft.Network/privateDnsZones/CNAME/delete | Verwijder de recordset van een bepaalde naam en typ ' CNAME ' uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/CNAME/read | De recordset van het type CNAME ophalen binnen een Privé-DNS zone, in JSON-indeling. |
> | Action | Microsoft.Network/privateDnsZones/CNAME/write | Een recordset van het type CNAME maken of bijwerken binnen een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/delete | Een Privé-DNS zone verwijderen. |
> | Action | Microsoft.Network/privateDnsZones/MX/delete | Verwijder de recordset van een bepaalde naam en typ ' MX ' uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/MX/read | Haal de recordset van het type ' MX ' op in een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/MX/write | Een recordset van het type ' MX ' maken of bijwerken binnen een Privé-DNS zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/PTR/delete | Verwijder de recordset van een bepaalde naam en typ ' PTR ' uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/PTR/read | Haal de recordset van het type ' PTR ' op in een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/PTR/write | Een recordset van het type ' PTR ' maken of bijwerken binnen een Privé-DNS zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/read | Haal de eigenschappen van de Privé-DNS-zone op in JSON-indeling. Houd er rekening mee dat met deze opdracht de virtuele netwerken waaraan de Privé-DNS zone is gekoppeld of de record sets in de zone, niet worden opgehaald. |
> | Action | Microsoft.Network/privateDnsZones/recordsets/read | Hiermee worden Privé-DNS record sets in verschillende typen opgehaald |
> | Action | Microsoft.Network/privateDnsZones/SOA/read | Haal de recordset van het type ' SOA ' op in een Privé-DNS zone, in JSON-indeling. |
> | Action | Microsoft.Network/privateDnsZones/SOA/write | Een recordset van het type ' SOA ' in een Privé-DNS zone bijwerken. |
> | Action | Microsoft.Network/privateDnsZones/SRV/delete | De recordset van een bepaalde naam en type SRV verwijderen uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/SRV/read | De recordset van het type SRV ophalen binnen een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/SRV/write | Een recordset van het type SRV maken of bijwerken binnen een Privé-DNS zone. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/TXT/delete | Verwijder de recordset van een bepaalde naam en typ ' TXT ' uit een Privé-DNS zone. |
> | Action | Microsoft.Network/privateDnsZones/TXT/read | Haal de recordset van het type ' TXT ' op in een Privé-DNS zone, in JSON-indeling. De recordset bevat een lijst met records en de TTL, tags en ETag. |
> | Action | Microsoft.Network/privateDnsZones/TXT/write | Een recordset van het type ' TXT ' in een Privé-DNS zone maken of bijwerken. Met de opgegeven records worden de huidige records in de recordset vervangen. |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/delete | Verwijder een Privé-DNS zone koppeling naar het virtuele netwerk. |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/read | Haal de koppeling naar de Privé-DNS zone op in JSON-indeling naar eigenschappen van virtueel netwerk. |
> | Action | Microsoft.Network/privateDnsZones/virtualNetworkLinks/write | Een Privé-DNS zone koppeling naar een virtueel netwerk maken of bijwerken. |
> | Action | Microsoft.Network/privateDnsZones/write | Een Privé-DNS zone in een resource groep maken of bijwerken. Houd er rekening mee dat deze opdracht niet kan worden gebruikt voor het maken of bijwerken van koppelingen met virtuele netwerken of record sets in de zone. |
> | Action | Microsoft.Network/privateEndpoints/delete | Hiermee verwijdert u een persoonlijke eindpunt resource. |
> | Action | Microsoft.Network/privateEndpoints/read | Hiermee wordt een persoonlijke eindpunt resource opgehaald. |
> | Action | Microsoft.Network/privateEndpoints/write | Hiermee maakt u een nieuw persoonlijk eind punt of werkt u een bestaand persoonlijk eind punt bij. |
> | Action | Microsoft.Network/privateLinkServices/delete | Hiermee wordt een resource van een persoonlijke koppelings service verwijderd. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Hiermee verwijdert u een verbinding met een privé-eind punt. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Hiermee wordt een definitie van een particuliere endpoint-verbinding opgehaald. |
> | Action | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Hiermee maakt u een nieuwe verbinding voor een persoonlijk eind punt of werkt u een bestaande verbinding met een privé-eind punt bij. |
> | Action | Microsoft.Network/privateLinkServices/read | Hiermee wordt een bron van een persoonlijke koppelings service opgehaald. |
> | Action | Microsoft.Network/privateLinkServices/write | Hiermee maakt u een nieuwe privé koppelings service of werkt u een bestaande persoonlijke koppelings service bij. |
> | Action | Microsoft.Network/publicIPAddresses/delete | Hiermee verwijdert u een openbaar IP-adres. |
> | Action | Microsoft.Network/publicIPAddresses/join/action | Er wordt verbinding gemaakt met een openbaar IP-adres. Niet Alertable. |
> | Action | Microsoft.Network/publicIPAddresses/read | Hiermee wordt een definitie van een openbaar IP-adres opgehaald. |
> | Action | Microsoft.Network/publicIPAddresses/write | Hiermee maakt u een openbaar IP-adres of werkt u een bestaand openbaar IP-adres bij.  |
> | Action | Microsoft.Network/publicIPPrefixes/delete | Hiermee wordt een openbaar IP-voor voegsel verwijderd |
> | Action | Microsoft.Network/publicIPPrefixes/join/action | Er wordt een PublicIPPrefix toegevoegd. Niet alertable. |
> | Action | Microsoft.Network/publicIPPrefixes/read | Hiermee wordt een definitie van een openbaar IP-voor voegsel opgehaald |
> | Action | Microsoft.Network/publicIPPrefixes/write | Hiermee wordt een openbaar IP-voor voegsel gemaakt of een bestaand openbaar IP-voor voegsel bijgewerkt |
> | Action | Microsoft.Network/register/action | Hiermee wordt het abonnement geregistreerd |
> | Action | Microsoft.Network/routeFilters/delete | Hiermee wordt een definitie van een route filter verwijderd |
> | Action | Microsoft.Network/routeFilters/join/action | Er wordt verbinding gemaakt met een route filter. Niet Alertable. |
> | Action | Microsoft.Network/routeFilters/read | Hiermee wordt een route filter definitie opgehaald |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/delete | Hiermee wordt een regel definitie voor een route filter verwijderd |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/read | Hiermee wordt een regel definitie voor een route filter opgehaald |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/write | Hiermee maakt u een regel voor route filtering of werkt u een bestaande route filter regel bij |
> | Action | Microsoft.Network/routeFilters/write | Hiermee maakt u een route filter of werkt u een bestaand route filter bij |
> | Action | Microsoft.Network/routeTables/delete | Hiermee wordt een definitie van een route tabel verwijderd |
> | Action | Microsoft.Network/routeTables/join/action | Voegt een route tabel samen. Niet Alertable. |
> | Action | Microsoft.Network/routeTables/read | Hiermee wordt een definitie van een route tabel opgehaald |
> | Action | Microsoft.Network/routeTables/routes/delete | Hiermee verwijdert u een route definitie |
> | Action | Microsoft.Network/routeTables/routes/read | Hiermee wordt een route definitie opgehaald |
> | Action | Microsoft.Network/routeTables/routes/write | Hiermee maakt u een route of werkt u een bestaande route bij |
> | Action | Microsoft.Network/routeTables/write | Hiermee maakt u een route tabel of werkt u een bestaande route tabel bij |
> | Action | Microsoft.Network/securegateways/delete | Beveiligde gateway verwijderen |
> | Action | Microsoft.Network/securegateways/read | Beveiligde gateway ophalen |
> | Action | Microsoft.Network/securegateways/write | Hiermee wordt een beveiligde gateway gemaakt of bijgewerkt |
> | Action | Microsoft.Network/serviceEndpointPolicies/delete | Hiermee wordt een service-eindpunt beleid verwijderd |
> | Action | Microsoft.Network/serviceEndpointPolicies/join/action | Voegt een service-eindpunt beleid samen. Niet alertable. |
> | Action | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Voegt een subnet toe aan service Endpoint-beleids regels. Niet alertable. |
> | Action | Microsoft.Network/serviceEndpointPolicies/read | Hiermee wordt een beschrijving van het service-eindpunt beleid opgehaald |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Hiermee wordt een beleids definitie voor service-eind punten verwijderd |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Hiermee wordt een beschrijving van de definitie van het service-eindpunt beleid opgehaald |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Hiermee wordt een beleids definitie voor service-eind punten gemaakt of een bestaande definitie van een service-eindpunt beleid bijgewerkt |
> | Action | Microsoft.Network/serviceEndpointPolicies/write | Hiermee wordt een service-eindpunt beleid gemaakt of een bestaand service-eindpunt beleid bijgewerkt |
> | Action | Microsoft.Network/trafficManagerGeographicHierarchies/read | Hiermee wordt de Traffic Manager geografische hiërarchie opgehaald die regio's bevat die kunnen worden gebruikt met de geografische routerings methode voor verkeer |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Hiermee verwijdert u een Azure-eindpunt uit een bestaand Traffic Manager-profiel. Traffic Manager wordt het routeren van verkeer naar de verwijderde Azure-eindpunt gestopt. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Hiermee haalt u een Azure-eindpunt die tot een Traffic Manager profiel behoort, inclusief alle eigenschappen van die Azure-eindpunt. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Voeg een nieuwe Azure-eindpunt toe aan een bestaand Traffic Manager profiel of werk de eigenschappen van een bestaande Azure-eindpunt in dat Traffic Manager profiel bij. |
> | Action | Microsoft.Network/trafficManagerProfiles/delete | Verwijder het Traffic Manager profiel. Alle instellingen die aan het Traffic Manager profiel zijn gekoppeld, gaan verloren en het profiel kan niet meer worden gebruikt voor het routeren van verkeer. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Hiermee verwijdert u een extern eind punt uit een bestaand Traffic Manager profiel. Traffic Manager wordt het routeren van verkeer naar het verwijderde externe eind punt gestopt. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Hiermee haalt u een extern eind punt dat hoort bij een Traffic Manager profiel, inclusief alle eigenschappen van het externe eind punt. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Voeg een nieuw extern eind punt toe aan een bestaand Traffic Manager profiel of werk de eigenschappen van een bestaand extern eind punt in dat Traffic Manager profiel bij. |
> | Action | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Hiermee haalt u de Traffic Manager heatmap voor het gegeven Traffic Manager profiel dat query aantallen en latentie gegevens bevat op locatie en bron-IP. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Hiermee verwijdert u een genest eind punt uit een bestaand Traffic Manager profiel. Traffic Manager wordt het routerings verkeer naar het verwijderde geneste eind punt gestopt. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Hiermee wordt een genest eind punt opgehaald dat tot een Traffic Manager profiel behoort, inclusief alle eigenschappen van dat geneste eind punt. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Een nieuw genest eind punt toevoegen aan een bestaand Traffic Manager profiel of de eigenschappen van een bestaand genest eind punt in dat Traffic Manager profiel bijwerken. |
> | Action | Microsoft.Network/trafficManagerProfiles/read | De configuratie van het Traffic Manager-profiel ophalen. Dit zijn onder andere DNS-instellingen, instellingen voor verkeers routering, instellingen voor eindpunt bewaking en de lijst met eind punten die door dit Traffic Manager profiel worden gerouteerd. |
> | Action | Microsoft.Network/trafficManagerProfiles/write | Maak een Traffic Manager profiel of wijzig de configuratie van een bestaand Traffic Manager-profiel.<br>Dit omvat het in-of uitschakelen van een profiel en het wijzigen van DNS-instellingen, instellingen voor verkeers routering of instellingen voor het controleren van eind punten.<br>Eind punten die door het Traffic Manager profiel worden doorgestuurd, kunnen worden toegevoegd, verwijderd, ingeschakeld of uitgeschakeld. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Hiermee verwijdert u de sleutel op abonnements niveau die wordt gebruikt voor de verzameling van metrische gegevens voor de realtime gebruiker. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/read | Hiermee wordt de sleutel op abonnements niveau opgehaald die wordt gebruikt voor de verzameling van metrische gegevens voor de realtime gebruiker. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/write | Hiermee wordt een nieuwe sleutel op abonnements niveau gemaakt die moet worden gebruikt voor de verzameling van metrische gegevens voor de realtime gebruiker. |
> | Action | Microsoft.Network/unregister/action | Hiermee wordt de registratie van het abonnement ongedaan gemaakt |
> | Action | Microsoft.Network/virtualHubs/delete | Hiermee wordt een virtuele hub verwijderd |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Hiermee wordt een HubVirtualNetworkConnection verwijderd |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Een HubVirtualNetworkConnection ophalen |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Een HubVirtualNetworkConnection maken of bijwerken |
> | Action | Microsoft.Network/virtualHubs/read | Een virtuele hub ophalen |
> | Action | Microsoft.Network/virtualHubs/write | Een virtuele hub maken of bijwerken |
> | Action | microsoft.network/virtualnetworkgateways/connections/read | VirtualNetworkGatewayConnection ophalen |
> | Action | Microsoft.Network/virtualNetworkGateways/delete | Hiermee wordt een virtualNetworkGateway verwijderd |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | VpnClient-pakket genereren voor virtualNetworkGateway |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | VpnProfile-pakket genereren voor VirtualNetworkGateway |
> | Action | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Hiermee worden virtualNetworkGateway geadverteerde routes opgehaald |
> | Action | micro soft. Network/virtualnetworkgateways/getbgppeerstatus/Action | Hiermee wordt de BGP-peer status virtualNetworkGateway opgehaald |
> | Action | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Hiermee worden virtualnetworkgateway geleerde routes opgehaald |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Per VPN-client verbindings status voor VirtualNetworkGateway ophalen |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Vpnclient IPSec-para meters voor VirtualNetworkGateway P2S-client ophalen. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Hiermee wordt de URL van een vooraf gegenereerd VPN-client profiel pakket opgehaald |
> | Action | Microsoft.Network/virtualNetworkGateways/read | Hiermee wordt een VirtualNetworkGateway opgehaald |
> | Action | microsoft.network/virtualnetworkgateways/reset/action | Hiermee wordt een virtualNetworkGateway opnieuw ingesteld |
> | Action | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Stel de gedeelde sleutel vpnclient voor de VirtualNetworkGateway P2S-client opnieuw in. |
> | Action | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Stel vpnclient IPSec-para meters in voor VirtualNetworkGateway P2S-client. |
> | Action | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Een lijst met ondersteunde VPN-apparaten |
> | Action | Microsoft.Network/virtualNetworkGateways/write | Hiermee wordt een VirtualNetworkGateway gemaakt of bijgewerkt |
> | Action | Microsoft.Network/virtualNetworks/BastionHosts/action | Hiermee worden Bastion opgehaald in een Virtual Network. |
> | Action | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Hiermee worden Bastion opgehaald in een Virtual Network. |
> | Action | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Controleer of het IP-adres beschikbaar is op het opgegeven virtuele netwerk |
> | Action | Microsoft.Network/virtualNetworks/delete | Hiermee verwijdert u een virtueel netwerk |
> | Action | Micro soft. Network/virtualNetworks/samen voegen/actie | Voegt een virtueel netwerk samen. Niet Alertable. |
> | Action | Microsoft.Network/virtualNetworks/peer/action | Peering van een virtueel netwerk met een ander virtueel netwerk |
> | Action | Microsoft.Network/virtualNetworks/read | De virtuele-netwerk definitie ophalen |
> | Action | Microsoft.Network/virtualNetworks/subnets/delete | Hiermee verwijdert u een subnet van een virtueel netwerk |
> | Action | Microsoft.Network/virtualNetworks/subnets/join/action | Voegt een virtueel netwerk samen. Niet Alertable. |
> | Action | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Voegt een resource, zoals een opslag account of SQL database, toe aan een subnet. Niet alertable. |
> | Action | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Bereidt een subnet voor door het benodigde netwerk beleid toe te passen |
> | Action | Microsoft.Network/virtualNetworks/subnets/read | Hiermee wordt een subnet definitie van een virtueel netwerk opgehaald |
> | Action | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Hiermee worden verwijzingen naar alle virtuele machines in een subnet van een virtueel netwerk opgehaald |
> | Action | Microsoft.Network/virtualNetworks/subnets/write | Hiermee wordt een subnet van een virtueel netwerk gemaakt of een bestaand subnet van een virtueel netwerk bijgewerkt |
> | Action | Microsoft.Network/virtualNetworks/usages/read | De IP-gebruik voor elk subnet van het virtuele netwerk ophalen |
> | Action | Microsoft.Network/virtualNetworks/virtualMachines/read | Hiermee worden verwijzingen naar alle virtuele machines in een virtueel netwerk opgehaald |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Hiermee wordt een virtueel netwerk peering verwijderd |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Hiermee wordt een definitie van een virtuele netwerk-peering opgehaald |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Hiermee maakt u een virtueel netwerk of werkt u een bestaande virtuele netwerk peering bij |
> | Action | Microsoft.Network/virtualNetworks/write | Hiermee wordt een virtueel netwerk gemaakt of een bestaand virtueel netwerk bijgewerkt |
> | Action | Microsoft.Network/virtualNetworkTaps/delete | Virtual Network tikken verwijderen |
> | Action | Microsoft.Network/virtualNetworkTaps/join/action | Tik op een virtueel netwerk tikken. Niet Alertable. |
> | Action | Microsoft.Network/virtualNetworkTaps/read | Virtual Network tikken |
> | Action | Microsoft.Network/virtualNetworkTaps/write | Virtual Network tikken maken of bijwerken |
> | Action | Microsoft.Network/virtualWans/delete | Hiermee verwijdert u een virtueel WAN |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Hiermee verwijdert u een virtueel WAN-P2SVpnServerConfiguration |
> | Action | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Hiermee wordt een virtuele WAN-P2SVpnServerConfiguration opgehaald |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Hiermee wordt een virtuele WAN-P2SVpnServerConfiguration gemaakt of een bestaande virtuele WAN-P2SVpnServerConfiguration bijgewerkt |
> | Action | Microsoft.Network/virtualWans/read | Een virtueel WAN ophalen |
> | Action | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Hiermee worden ondersteunde VirtualWan-beveiligings providers opgehaald. |
> | Action | Microsoft.Network/virtualWans/virtualHubs/read | Hiermee worden alle virtuele hubs opgehaald die naar een virtueel WAN verwijzen. |
> | Action | Microsoft.Network/virtualwans/vpnconfiguration/action | Hiermee wordt een VPN-configuratie opgehaald |
> | Action | Microsoft.Network/virtualWans/vpnSites/read | Hiermee worden alle VPN-sites opgehaald die naar een virtueel WAN verwijzen. |
> | Action | Microsoft.Network/virtualWans/write | Een virtueel WAN maken of bijwerken |
> | Action | Microsoft.Network/vpnGateways/delete | Hiermee verwijdert u een VpnGateway. |
> | Action | microsoft.network/vpngateways/listvpnconnectionshealth/action | Hiermee wordt de status van de verbinding voor alle of een subset van verbindingen op een VpnGateway |
> | Action | Microsoft.Network/vpnGateways/read | Hiermee wordt een VpnGateway opgehaald. |
> | Action | microsoft.network/vpngateways/reset/action | Hiermee wordt een VpnGateway opnieuw ingesteld |
> | Action | microsoft.network/vpnGateways/vpnConnections/delete | Hiermee verwijdert u een VpnConnection. |
> | Action | microsoft.network/vpnGateways/vpnConnections/read | Hiermee wordt een VpnConnection opgehaald. |
> | Action | micro soft. Network/vpnGateways/vpnConnections/vpnLinkConnections/lezen | Hiermee wordt een VPN-koppelings verbinding opgehaald |
> | Action | microsoft.network/vpnGateways/vpnConnections/write | Hiermee wordt een VpnConnection geplaatst. |
> | Action | Microsoft.Network/vpnGateways/write | Hiermee wordt een VpnGateway geplaatst. |
> | Action | Microsoft.Network/vpnsites/delete | Hiermee verwijdert u een bron van een VPN-site. |
> | Action | Microsoft.Network/vpnsites/read | Hiermee wordt een bron van een VPN-site opgehaald. |
> | Action | micro soft. Network/vpnSites/vpnSiteLinks/lezen | Hiermee wordt een VPN-site koppeling opgehaald |
> | Action | Microsoft.Network/vpnsites/write | Hiermee wordt een bron van een VPN-site gemaakt of bijgewerkt. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Hiermee wordt gecontroleerd of de resourcenaam voor de naamruimte beschikbaar is binnen de NotificationHub-service. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Naam ruimte autorisatie regel verwijderen. De standaard regel voor autorisatie van de naam ruimte kan niet worden verwijderd.  |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | De verificatieregel voor een naamruimte De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Maak autorisatie regels voor het naam ruimte niveau en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Action | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Hiermee wordt gecontroleerd of de NotificationHub-naam beschikbaar is binnen een naamruimte. |
> | Action | Microsoft.NotificationHubs/Namespaces/Delete | De resource van een naamruimte verwijderen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | De lijst met verificatieregels voor Notification Hub ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Verificatieregels voor Notification Hub verwijderen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | De verbindingsreeks naar de Notification Hub ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | De lijst met verificatieregels voor Notification Hub ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | De verificatieregel voor Notification Hub De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Maak autorisatie regels voor de notification hub en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Een testpushmelding verzenden. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | De resource voor Notification Hub verwijderen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Lijst met resource beschrijvingen voor metrische gegevens van de naam ruimte ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Alle PNS-referenties voor notification hub ophalen. Dit omvat, WNS, MPNS, APNS-, GCM-en Baidu-referenties |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | De lijst met beschrijvingen van resources voor Notification Hub ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Een notification hub maken en de eigenschappen ervan bijwerken. De eigenschappen omvatten voornamelijk PNS-referenties. Autorisatie regels en TTL |
> | Action | Microsoft.NotificationHubs/Namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Action | Microsoft.NotificationHubs/Namespaces/write | Maak een naam ruimte resource en werk de eigenschappen bij. Tags en capaciteit van de naam ruimte zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Action | Microsoft.NotificationHubs/operationResults/read | Hiermee worden bewerkingsresultaten voor de Notification Hubs-provider geretourneerd |
> | Action | Microsoft.NotificationHubs/operations/read | Hiermee wordt een lijst met ondersteunde bewerkingen voor de Notification Hubs-provider geretourneerd |
> | Action | Microsoft.NotificationHubs/register/action | Hiermee wordt het abonnement voor de notification hubs-resource provider geregistreerd en wordt het maken van naam ruimten en notification hubs |
> | Action | Microsoft.NotificationHubs/unregister/action | Hiermee wordt de registratie van het abonnement voor de notification hubs-resource provider ongedaan gemaakt en wordt het maken van naam ruimten en notification hubs |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/read | Hiermee worden de eigenschappen van een Hyper-V-cluster opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/write | Hiermee wordt het Hyper-V-cluster gemaakt of bijgewerkt |
> | Action | Microsoft.OffAzure/HyperVSites/delete | Hiermee verwijdert u de Hyper-V-site |
> | Action | Micro soft. OffAzure/HyperVSites/healthsummary/lezen | Hiermee wordt het status overzicht voor Hyper-V-resource opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/read | Hiermee worden de eigenschappen van een Hyper-V-host opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/write | Hiermee wordt de Hyper-V-host gemaakt of bijgewerkt |
> | Action | Microsoft.OffAzure/HyperVSites/jobs/read | Hiermee worden de eigenschappen van een Hyper-V-taak opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/machines/read | Hiermee worden de eigenschappen van een Hyper-V-computer opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Hiermee worden de eigenschappen van de status van een Hyper-V-bewerking opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/read | Hiermee worden de eigenschappen van een Hyper-V-site opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/refresh/action | Hiermee worden de objecten op een Hyper-V-site vernieuwd |
> | Action | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Hiermee worden de eigenschappen van een Hyper-V run as-accounts opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/usage/read | Hiermee worden de gebruiks handelingen van een Hyper-V-site opgehaald |
> | Action | Microsoft.OffAzure/HyperVSites/write | Hiermee wordt de Hyper-V-site gemaakt of bijgewerkt |
> | Action | Microsoft.OffAzure/Operations/read | Hiermee worden de beschik bare bewerkingen gelezen |
> | Action | Microsoft.OffAzure/register/action | Hiermee wordt een abonnement geregistreerd bij de resource provider micro soft. OffAzure |
> | Action | Microsoft.OffAzure/VMwareSites/delete | Hiermee verwijdert u de VMware-site |
> | Action | Micro soft. OffAzure/VMwareSites/healthsummary/lezen | Hiermee wordt het status overzicht voor VMware-resource opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/jobs/read | Hiermee worden de eigenschappen van een VMware-taak opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/machines/read | Hiermee worden de eigenschappen van een VMware-machine opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/machines/start/action | VMware-machines starten |
> | Action | Microsoft.OffAzure/VMwareSites/machines/stop/action | Stopt de VMware-machines |
> | Action | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Hiermee worden de eigenschappen van een VMware-bewerkings status opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/read | Hiermee worden de eigenschappen van een VMware-site opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/refresh/action | Hiermee worden de objecten in een VMware-site vernieuwd |
> | Action | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Hiermee worden de eigenschappen van een VMware run as-accounts opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/usage/read | Hiermee worden de gebruiks handelingen van een VMware-site opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/read | Hiermee worden de eigenschappen van een VMware vCenter opgehaald |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/write | Hiermee wordt de VMware vCenter gemaakt of bijgewerkt |
> | Action | Microsoft.OffAzure/VMwareSites/write | Hiermee wordt de VMware-site gemaakt of bijgewerkt |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.OperationalInsights/linkTargets/read | Een lijst met bestaande accounts die niet zijn gekoppeld aan een Azure-abonnement. Als u dit Azure-abonnement aan een werk ruimte wilt koppelen, gebruikt u een klant-id die door deze bewerking is geretourneerd in de eigenschap Customer ID van de bewerking werk ruimte maken. |
> | Action | microsoft.operationalinsights/operations/read | Een lijst met alle beschik bare OperationalInsights rest API-bewerkingen. |
> | Action | microsoft.operationalinsights/register/action | Rergisters het abonnement. |
> | Action | Microsoft.OperationalInsights/register/action | Een abonnement bij een resource provider registreren. |
> | Action | microsoft.operationalinsights/unregister/action | Hiermee maakt u de registratie van het abonnement ongedaan. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoek met een nieuwe engine. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Zoek schema v2 ophalen. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/action | Zoek met een nieuwe engine. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Zoek schema v2 ophalen. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Configuratie bereik verwijderen |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Configuratie bereik ophalen |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Configuratie bereik instellen |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/delete | Gegevens bronnen onder een werk ruimte verwijderen. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/read | Gegevens bronnen onder een werk ruimte ophalen. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/write | Gegevens bronnen onder een werk ruimte maken/bijwerken. |
> | Action | Microsoft.OperationalInsights/workspaces/delete | Hiermee verwijdert u een werk ruimte. Als de werk ruimte is gekoppeld aan een bestaande werk ruimte tijdens de aanmaak tijd, wordt de werk ruimte waaraan deze is gekoppeld, niet verwijderd. |
> | Action | Microsoft.OperationalInsights/workspaces/gateways/delete | Hiermee verwijdert u een gateway die is geconfigureerd voor de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Hiermee wordt een registratie certificaat voor de werk ruimte gegenereerd. Dit certificaat wordt gebruikt om micro soft System Center Operations Manager te verbinden met de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Hiermee wordt een Intelligence Pack voor een opgegeven werk ruimte uitgeschakeld. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Hiermee wordt een Intelligence Pack voor een opgegeven werk ruimte ingeschakeld. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Hiermee worden alle Intelligence packs weer gegeven die zichtbaar zijn voor een bepaalde werk ruimte en wordt ook vermeld of het pakket is ingeschakeld of uitgeschakeld voor die werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Gekoppelde services onder de opgegeven werk ruimte verwijderen. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/read | Gekoppelde services onder de opgegeven werk ruimte ophalen. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/write | Gekoppelde services onder de opgegeven werk ruimte maken/bijwerken. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/action | Hiermee haalt u de lijst sleutels voor de werk ruimte op. Deze sleutels worden gebruikt om micro soft Operational Insights-agents te verbinden met de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/read | Hiermee haalt u de lijst sleutels voor de werk ruimte op. Deze sleutels worden gebruikt om micro soft Operational Insights-agents te verbinden met de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/managementGroups/read | Hiermee worden de namen en meta gegevens opgehaald voor System Center Operations Manager-beheer groepen die met deze werk ruimte zijn verbonden. |
> | Action | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Metrische definities ophalen onder werk ruimte |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | De instellingen voor meldingen voor de gebruiker verwijderen voor de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/read | De instellingen voor meldingen voor de gebruiker ophalen voor de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/write | De instellingen voor meldingen voor de gebruiker instellen voor de werk ruimte. |
> | Action | microsoft.operationalinsights/workspaces/operations/read | Hiermee wordt de status van een OperationalInsights-werkruimte bewerking opgehaald. |
> | Action | Microsoft.OperationalInsights/workspaces/purge/action | Opgegeven gegevens uit de werk ruimte verwijderen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | Gegevens uit de AADDomainServicesAccountLogon-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountManagement/read | Gegevens uit de AADDomainServicesAccountManagement-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesDirectoryServiceAccess/read | Gegevens uit de AADDomainServicesDirectoryServiceAccess-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | Gegevens uit de AADDomainServicesLogonLogoff-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPolicyChange/read | Gegevens uit de AADDomainServicesPolicyChange-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPrivilegeUse/read | Gegevens uit de AADDomainServicesPrivilegeUse-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesSystemSecurity/read | Gegevens uit de AADDomainServicesSystemSecurity-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Gegevens lezen uit de tabel ADAssessmentRecommendation |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/AddonAzureBackupAlerts/lezen | Gegevens uit de AddonAzureBackupAlerts-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/AddonAzureBackupJobs/lezen | Gegevens uit de AddonAzureBackupJobs-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/AddonAzureBackupPolicy/lezen | Gegevens uit de AddonAzureBackupPolicy-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/AddonAzureBackupProtectedInstance/lezen | Gegevens uit de AddonAzureBackupProtectedInstance-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/AddonAzureBackupStorage/lezen | Gegevens uit de AddonAzureBackupStorage-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFActivityRun/read | Gegevens uit de ADFActivityRun-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFPipelineRun/read | Gegevens uit de ADFPipelineRun-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADFTriggerRun/read | Gegevens uit de ADFTriggerRun-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Gegevens lezen uit de tabel ADReplicationResult |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Gegevens lezen uit de tabel ADSecurityAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/Alert/read | Gegevens lezen uit de tabel Alert |
> | Action | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Gegevens lezen uit de tabel AlertHistory |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/ApiManagementGatewayLogs/lezen | Gegevens uit de ApiManagementGatewayLogs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Gegevens uit de AppCenterError-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Gegevens lezen uit de tabel ApplicationInsights |
> | Action | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Gegevens uit de audit logs bevat-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | Gegevens uit de AutoscaleEvaluationsLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | Gegevens uit de AutoscaleScaleActionsLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | Gegevens uit de AWSCloudTrail-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Gegevens lezen uit de tabel AzureActivity |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureAssessmentRecommendation/read | Gegevens uit de AzureAssessmentRecommendation-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Gegevens lezen uit de tabel AzureMetrics |
> | Action | Microsoft.OperationalInsights/workspaces/query/BlockchainApplicationLog/read | Gegevens uit de BlockchainApplicationLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/BlockchainProxyLog/read | Gegevens uit de BlockchainProxyLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Gegevens lezen uit de tabel BoundPort |
> | Action | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Gegevens lezen uit de tabel CommonSecurityLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Gegevens lezen uit de tabel ComputerGroup |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Gegevens lezen uit de tabel ConfigurationChange |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Gegevens lezen uit de tabel ConfigurationData |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Gegevens lezen uit de tabel ContainerImageInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Gegevens lezen uit de tabel ContainerInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Gegevens lezen uit de tabel ContainerLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | Gegevens uit de ContainerNodeInventory-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/ContainerRegistryLoginEvents/lezen | Gegevens uit de ContainerRegistryLoginEvents-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/ContainerRegistryRepositoryEvents/lezen | Gegevens uit de ContainerRegistryRepositoryEvents-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Gegevens lezen uit de tabel ContainerServiceLog |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/CoreAzureBackup/lezen | Gegevens uit de CoreAzureBackup-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | Gegevens uit de DatabricksAccounts-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | Gegevens uit de DatabricksClusters-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | Gegevens uit de DatabricksDBFS-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | Gegevens uit de DatabricksJobs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | Gegevens uit de DatabricksNotebook-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | Gegevens uit de DatabricksSecrets-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | Gegevens uit de DatabricksSQLPermissions-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | Gegevens uit de DatabricksSSH-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | Gegevens uit de DatabricksTables-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | Gegevens uit de DatabricksWorkspace-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Gegevens lezen uit de tabel DeviceAppCrash |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Gegevens lezen uit de tabel DeviceAppLaunch |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Gegevens lezen uit de tabel DeviceCalendar |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Gegevens lezen uit de tabel DeviceCleanup |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Gegevens lezen uit de tabel DeviceConnectSession |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Gegevens lezen uit de tabel DeviceEtw |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Gegevens lezen uit de tabel DeviceHardwareHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Gegevens lezen uit de tabel DeviceHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Gegevens lezen uit de tabel DeviceHeartbeat |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Gegevens lezen uit de tabel DeviceSkypeHeartbeat |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Gegevens lezen uit de tabel DeviceSkypeSignIn |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Gegevens lezen uit de tabel DeviceSleepState |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Gegevens lezen uit de tabel DHAppFailure |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Gegevens lezen uit de tabel DHAppReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Gegevens lezen uit de tabel DHDriverReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Gegevens lezen uit de tabel DHLogonFailures |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Gegevens lezen uit de tabel DHLogonMetrics |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Gegevens lezen uit de tabel DHOSCrashData |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Gegevens lezen uit de tabel DHOSReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Gegevens lezen uit de tabel DHWipAppLearning |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Gegevens lezen uit de tabel DnsEvents |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Gegevens lezen uit de tabel DnsInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Gegevens lezen uit de tabel ETWEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/Event/read | Gegevens lezen uit de tabel Event |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Gegevens lezen uit de tabel Heartbeat |
> | Action | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | Gegevens uit de HuntingBookmark-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Gegevens lezen uit de tabel IISAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Gegevens lezen uit de tabel InboundConnection |
> | Action | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | Gegevens uit de InsightsMetrics-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Gegevens uit de IntuneAuditLogs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Gegevens uit de IntuneOperationalLogs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Gegevens uit de KubeEvents-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Gegevens lezen uit de tabel KubeNodeInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Gegevens lezen uit de tabel KubePodInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Gegevens uit de KubeServices-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Gegevens lezen uit de tabel LinuxAuditLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Gegevens lezen uit de tabel MAApplication |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Gegevens lezen uit de tabel MAApplicationHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Gegevens lezen uit de tabel MAApplicationHealthAlternativeVersions |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Gegevens lezen uit de tabel MAApplicationHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Gegevens lezen uit de tabel MAApplicationInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Gegevens lezen uit de tabel MAApplicationInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Gegevens lezen uit de tabel MAApplicationReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Gegevens lezen uit de tabel MADeploymentPlan |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Gegevens lezen uit de tabel MADevice |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Gegevens uit de MADeviceNotEnrolled-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | Gegevens uit de MADeviceNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Gegevens lezen uit de tabel MADevicePnPHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Gegevens lezen uit de tabel MADevicePnPHealthAlternativeVersions |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Gegevens lezen uit de tabel MADevicePnPHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Gegevens lezen uit de tabel MADeviceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Gegevens lezen uit de tabel MADriverInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Gegevens lezen uit de tabel MADriverReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Gegevens lezen uit de tabel MAOfficeAddin |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Gegevens uit de MAOfficeAddinEntityHealth-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Gegevens lezen uit de tabel MAOfficeAddinHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Gegevens uit de MAOfficeAddinHealthEventNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Gegevens lezen uit de tabel MAOfficeAddinHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Gegevens lezen uit de tabel MAOfficeAddinInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Gegevens lezen uit de tabel MAOfficeApp |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | Gegevens uit de MAOfficeAppCrashesNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Gegevens lezen uit de tabel MAOfficeAppHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Gegevens lezen uit de tabel MAOfficeAppInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | Gegevens uit de MAOfficeAppInstanceHealth-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Gegevens lezen uit de tabel MAOfficeAppReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | Gegevens uit de MAOfficeAppSessionsNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Gegevens lezen uit de tabel MAOfficeBuildInfo |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessment |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessmentDailyCounts |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Gegevens lezen uit de tabel MAOfficeDeploymentStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | Gegevens uit de MAOfficeDeploymentStatusNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | Gegevens uit de MAOfficeMacroErrorNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | Gegevens uit de MAOfficeMacroGlobalHealth-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Gegevens lezen uit de tabel MAOfficeMacroHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Gegevens lezen uit de tabel MAOfficeMacroHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Gegevens lezen uit de tabel MAOfficeMacroSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Gegevens lezen uit de tabel MAOfficeSuite |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Gegevens lezen uit de tabel MAOfficeSuiteInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Gegevens lezen uit de tabel MAProposedPilotDevices |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Gegevens lezen uit de tabel MAWindowsBuildInfo |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessment |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessmentDailyCounts |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Gegevens lezen uit de tabel MAWindowsDeploymentStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | Gegevens uit de MAWindowsDeploymentStatusNRT-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Gegevens lezen uit de tabel MAWindowsSysReqInstanceReadiness |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/McasShadowItReporting/lezen | Gegevens uit de McasShadowItReporting-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/Microservices4SpringApplicationLogs/lezen | Gegevens uit de Microservices4SpringApplicationLogs-tabel lezen |
> | Action | Micro soft. OperationalInsights/werk ruimten/query/MicrosoftInsightsAzureActivityLog/lezen | Gegevens uit de MicrosoftInsightsAzureActivityLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebApplicationLog/read | Gegevens uit de MicrosoftWebApplicationLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebFunctionExecutionLogs/read | Gegevens uit de MicrosoftWebFunctionExecutionLogs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebStdOutStdErrLog/read | Gegevens uit de MicrosoftWebStdOutStdErrLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebW3CLog/read | Gegevens uit de MicrosoftWebW3CLog-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Gegevens lezen uit de tabel NetworkMonitoring |
> | Action | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Gegevens lezen uit de tabel OfficeActivity |
> | Action | Microsoft.OperationalInsights/workspaces/query/Operation/read | Gegevens lezen uit de tabel Operation |
> | Action | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Gegevens lezen uit de tabel OutboundConnection |
> | Action | Microsoft.OperationalInsights/workspaces/query/Perf/read | Gegevens lezen uit de prestatietabel |
> | Action | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Gegevens lezen uit de tabel ProtectionStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/read | Query's uitvoeren voor de gegevens in de werk ruimte |
> | Action | Microsoft.OperationalInsights/workspaces/query/ReservedAzureCommonFields/read | Gegevens lezen uit de tabel ReservedAzureCommonFields |
> | Action | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Gegevens lezen uit de tabel ReservedCommonFields |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCCMAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCOMAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Gegevens lezen uit de tabel SecurityAlert |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Gegevens lezen uit de tabel SecurityBaseline |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Gegevens lezen uit de tabel SecurityBaselineSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Gegevens lezen uit de tabel SecurityDetection |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Gegevens lezen uit de tabel SecurityEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | Gegevens uit de SecurityIoTRawEvent-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | Gegevens uit de SecurityRecommendation-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Gegevens lezen uit de tabel ServiceFabricOperationalEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableActorEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableServiceEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SharePointOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Gegevens uit de SigninLogs-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Gegevens lezen uit de tabel SPAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Gegevens lezen uit de tabel SQLAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Gegevens lezen uit de tabel SQLQueryPerformance |
> | Action | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | Gegevens uit de SqlThreatProtectionLoginAudits-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Gegevens uit de SqlVulnerabilityAssessmentResult-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Gegevens lezen uit de tabel Syslog |
> | Action | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Gegevens lezen uit de tabel SysmonEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Gegevens lezen van een aangepast logboek |
> | Action | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | Gegevens uit de ThreatIntelligenceIndicator-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Gegevens lezen uit de tabel UAApp |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Gegevens lezen uit de tabel UAComputer |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Gegevens lezen uit de tabel UAComputerRank |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Gegevens lezen uit de tabel UADriver |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Gegevens lezen uit de tabel UADriverProblemCodes |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Gegevens lezen uit de tabel UAFeedback |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Gegevens lezen uit de tabel UAHardwareSecurity |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Gegevens lezen uit de tabel UAIESiteDiscovery |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Gegevens lezen uit de tabel UAOfficeAddIn |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Gegevens lezen uit de tabel UAProposedActionPlan |
> | Action | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Gegevens lezen uit de tabel UASysReqIssue |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Gegevens lezen uit de tabel UAUpgradedComputer |
> | Action | Microsoft.OperationalInsights/workspaces/query/Update/read | Gegevens lezen uit de tabel Update |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Gegevens lezen uit de tabel UpdateRunProgress |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Gegevens lezen uit de tabel UpdateSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/Usage/read | Gegevens lezen uit de tabel Usage |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Gegevens uit de VMBoundPort-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Gegevens uit de VMConnection-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Gegevens lezen uit de tabel W3CIISLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Gegevens lezen uit de tabel WaaSDeploymentStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Gegevens lezen uit de tabel WaaSInsiderStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Gegevens lezen uit de tabel WaaSUpdateStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Gegevens lezen uit de tabel WDAVStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Gegevens lezen uit de tabel WDAVThreat |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsClientAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Gegevens uit de WindowsEvent-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Gegevens lezen uit de tabel WindowsFirewall |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsServerAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/WireData/read | Gegevens lezen uit de tabel WireData |
> | Action | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Gegevens uit de WorkloadMonitoringPerf-tabel lezen |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Gegevens lezen uit de tabel WUDOAggregatedStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Gegevens lezen uit de tabel WUDOStatus |
> | Action | Microsoft.OperationalInsights/workspaces/read | Hiermee wordt een bestaande werk ruimte opgehaald |
> | Action | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Hiermee wordt de opgegeven gedeelde sleutel voor de werk ruimte opnieuw gegenereerd |
> | Action | microsoft.operationalinsights/workspaces/rules/read | Alle waarschuwings regels ophalen. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Hiermee wordt een opgeslagen Zoek query verwijderd |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/read | Hiermee wordt een opgeslagen Zoek query opgehaald |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/results/read | Resultaten voor opgeslagen Zoek opdrachten ophalen. Afgeschaft |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Geplande zoek acties verwijderen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Geplande zoek acties ophalen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Geplande zoek acties maken of bijwerken. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Geplande Zoek opdrachten verwijderen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Geplande Zoek opdrachten ophalen. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Geplande Zoek opdrachten maken of bijwerken. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/write | Hiermee wordt een opgeslagen Zoek query gemaakt |
> | Action | Microsoft.OperationalInsights/workspaces/schema/read | Hiermee wordt het zoek schema voor de werk ruimte opgehaald.  Zoek schema bevat de weer gegeven velden en de bijbehorende typen. |
> | Action | Microsoft.OperationalInsights/workspaces/search/action | Hiermee wordt een zoek query uitgevoerd |
> | Action | microsoft.operationalinsights/workspaces/search/read | Zoek resultaten ophalen. Keur. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee worden de gedeelde sleutels voor de werk ruimte opgehaald. Deze sleutels worden gebruikt om micro soft Operational Insights-agents te verbinden met de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee worden de gedeelde sleutels voor de werk ruimte opgehaald. Deze sleutels worden gebruikt om micro soft Operational Insights-agents te verbinden met de werk ruimte. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Hiermee verwijdert u een opslag configuratie. Hiermee wordt voor komen dat micro soft Operational Insights het lezen van gegevens uit het opslag account. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Hiermee wordt een opslag configuratie opgehaald. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Hiermee maakt u een nieuwe opslag configuratie. Deze configuraties worden gebruikt voor het ophalen van gegevens van een locatie in een bestaand opslag account. |
> | Action | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Fout logboek voor de upgrade van de zoek actie ophalen voor de werk ruimte |
> | Action | Microsoft.OperationalInsights/workspaces/usages/read | Hiermee worden gebruiks gegevens voor een werk ruimte opgehaald, inclusief de hoeveelheid gegevens die door de werk ruimte wordt gelezen. |
> | Action | Microsoft.OperationalInsights/workspaces/write | Hiermee maakt u een nieuwe werk ruimte of koppelingen naar een bestaande werk ruimte door de klant-id van de bestaande werk ruimte op te geven. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.OperationsManagement/managementAssociations/delete | Bestaande beheer koppeling verwijderen |
> | Action | Microsoft.OperationsManagement/managementAssociations/read | Bestaande beheer koppeling ophalen |
> | Action | Microsoft.OperationsManagement/managementAssociations/write | Een nieuwe beheer koppeling maken |
> | Action | Microsoft.OperationsManagement/managementConfigurations/delete | Bestaande beheer configuratie verwijderen |
> | Action | Microsoft.OperationsManagement/managementConfigurations/read | Bestaande beheer configuratie ophalen |
> | Action | Microsoft.OperationsManagement/managementConfigurations/write | Een nieuwe beheer configuratie maken |
> | Action | Microsoft.OperationsManagement/register/action | Een abonnement bij een resource provider registreren. |
> | Action | Microsoft.OperationsManagement/solutions/delete | Bestaande OMS-oplossing verwijderen |
> | Action | Microsoft.OperationsManagement/solutions/read | De OMS-oplossing ophalen |
> | Action | Microsoft.OperationsManagement/solutions/write | Nieuwe OMS-oplossing maken |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.PolicyInsights/asyncOperationResults/read | Hiermee wordt het resultaat van de asynchrone bewerking opgehaald. |
> | Action | Microsoft.PolicyInsights/operations/read | Hiermee worden ondersteunde bewerkingen voor de micro soft. PolicyInsights-naam ruimte opgehaald |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/action | Informatie over beleidsgebeurtenissen opvragen. |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/read | Informatie over beleidsgebeurtenissen opvragen. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/action | Informatie over beleidsstatussen opvragen. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/read | Informatie over beleidsstatussen opvragen. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/action | Samenvattingsinformatie over de meest recente beleidsstatussen opvragen. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/read | Samenvattingsinformatie over de meest recente beleidsstatussen opvragen. |
> | Action | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Hiermee wordt een nieuwe evaluatie van de compatibiliteit voor het geselecteerde bereik geactiveerd. |
> | Action | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Informatie opvragen over resources die zijn vereist door het beleid DeployIfNotExists beleid. |
> | Action | Microsoft.PolicyInsights/register/action | Hiermee wordt de micro soft Policy Insights-resource provider geregistreerd en worden er acties op uitgevoerd. |
> | Action | Microsoft.PolicyInsights/remediations/cancel/action | Hiermee wordt de uitvoering van micro soft-beleids herstel geannuleerd. |
> | Action | Microsoft.PolicyInsights/remediations/delete | Beleidsherstelbewerkingen verwijderen. |
> | Action | Microsoft.PolicyInsights/remediations/listDeployments/read | Hiermee wordt een lijst met implementaties gemaakt die vereist zijn voor een beleidsherstelbewerking. |
> | Action | Microsoft.PolicyInsights/remediations/read | Beleidsherstelbewerkingen ophalen. |
> | Action | Microsoft.PolicyInsights/remediations/write | Herstel bewerkingen van micro soft-beleid maken of bijwerken. |
> | Action | Microsoft.PolicyInsights/unregister/action | Hiermee wordt de registratie van de micro soft Policy Insights-resource provider ongedaan gemaakt. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Portal/consoles/delete | Hiermee wordt het Cloud Shell-exemplaar verwijderd. |
> | Action | Microsoft.Portal/consoles/read | Hiermee wordt het Cloud Shell-exemplaar gelezen. |
> | Action | Microsoft.Portal/consoles/write | Een Cloud Shell-exemplaar maken of bijwerken. |
> | Action | Microsoft.Portal/dashboards/delete | Het dashboard verwijderen uit het abonnement. |
> | Action | Microsoft.Portal/dashboards/read | De dashboards voor het abonnement lezen. |
> | Action | Microsoft.Portal/dashboards/write | Dashboard toevoegen aan een abonnement of het dashboard wijzigen. |
> | Action | Microsoft.Portal/register/action | Registreren bij de portal |
> | Action | Microsoft.Portal/usersettings/delete | Hiermee wordt de Cloud Shell-gebruikersinstellingen verwijderd. |
> | Action | Microsoft.Portal/usersettings/read | Hiermee wordt de Cloud Shell-gebruikersinstellingen gelezen. |
> | Action | Microsoft.Portal/usersettings/write | De Cloud Shell-gebruikersinstelling maken of bijwerken. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. PowerBIDedicated/capaciteiten/verwijderen | Hiermee wordt de toegewezen capaciteit van Power BI verwijderd. |
> | Action | Micro soft. PowerBIDedicated/capaciteiten/lezen | Hiermee wordt de informatie opgehaald van de opgegeven Power BI toegewezen capaciteit. |
> | Action | Microsoft.PowerBIDedicated/capacities/resume/action | Hiermee wordt de capaciteit hervat. |
> | Action | Micro soft. PowerBIDedicated/capaciteiten/sku's/lezen | Beschik bare SKU-gegevens voor de capaciteit ophalen |
> | Action | Micro soft. PowerBIDedicated/capaciteiten/onderbreken/actie | Hiermee wordt de capaciteit onderbroken. |
> | Action | Micro soft. PowerBIDedicated/capaciteiten/schrijven | Hiermee wordt de opgegeven Power BI toegewezen capaciteit gemaakt of bijgewerkt. |
> | Action | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Controleert of de naam van Power BI toegewezen capaciteit geldig is en niet wordt gebruikt. |
> | Action | Microsoft.PowerBIDedicated/locations/operationresults/read | Hiermee wordt de informatie opgehaald van het opgegeven bewerkings resultaat. |
> | Action | Micro soft. PowerBIDedicated/locaties/operationstatuses/lezen | Hiermee wordt de informatie opgehaald van de opgegeven bewerkings status. |
> | Action | Microsoft.PowerBIDedicated/operations/read | Hiermee wordt de informatie over bewerkingen opgehaald |
> | Action | Microsoft.PowerBIDedicated/register/action | Registreert Power BI toegewezen resource provider. |
> | Action | Microsoft.PowerBIDedicated/skus/read | Hiermee wordt de informatie van Sku's opgehaald |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Action | Microsoft.RecoveryServices/locations/allocateStamp/action | Allo Cate Stamp is een interne bewerking die wordt gebruikt door de service |
> | Action | microsoft.recoveryservices/Locations/backupPreValidateProtection/action |  |
> | Action | microsoft.recoveryservices/Locations/backupProtectedItem/write | Een beveiligd back-upitem maken |
> | Action | microsoft.recoveryservices/Locations/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Action | microsoft.recoveryservices/Locations/backupStatus/action | De back-upstatus voor Recovery Services kluizen controleren |
> | Action | microsoft.recoveryservices/Locations/backupValidateFeatures/action | Functies valideren |
> | Action | Microsoft.RecoveryServices/locations/checkNameAvailability/action | De beschik baarheid van resource namen controleren is een API om te controleren of de resource naam beschikbaar is |
> | Action | Microsoft.RecoveryServices/locations/operationStatus/read | Hiermee wordt de bewerkings status voor een bepaalde bewerking opgehaald |
> | Action | Microsoft.RecoveryServices/operations/read | Met deze bewerking wordt de lijst met bewerkingen voor een resource provider geretourneerd |
> | Action | Microsoft.RecoveryServices/register/action | Hiermee wordt een abonnement geregistreerd voor een bepaalde resource provider |
> | Action | microsoft.recoveryservices/Vaults/backupconfig/read | Hiermee wordt de configuratie voor Recovery Services kluis geretourneerd. |
> | Action | microsoft.recoveryservices/Vaults/backupconfig/write | Hiermee wordt de configuratie van Recovery Services-kluis bijgewerkt. |
> | Action | microsoft.recoveryservices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/delete | Een opzet voor een back-upbeveiliging verwijderen |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/read | Een doel voor back-upbeveiliging verkrijgen |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/backupProtectionIntent/write | Een doel voor back-upbeveiliging maken |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/operationResults/read | Hiermee wordt de status van de bewerking geretourneerd |
> | Action | micro soft. Recovery Services/kluizen/backupFabrics/operationsStatus/lezen | Hiermee wordt de status van de bewerking geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectableContainers/read | Alle Beveilig bare containers ophalen |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/delete | Hiermee wordt de geregistreerde container verwijderd |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/inquire/action | Een query uitvoeren voor werk belastingen binnen een container |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/items/read | Alle items in een container ophalen |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Action | micro soft. Recovery Services/kluizen/backupFabrics/protectionContainers/operationsStatus/lezen | Hiermee wordt de status opgehaald van de bewerking die is uitgevoerd op de beveiligings container. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hiermee wordt een back-up van het beveiligde item gemaakt. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Hiermee wordt het beveiligde item verwijderd |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Hiermee worden object gegevens van het beveiligde item geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Direct-item herstel inrichten voor beveiligd item |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Herstelpunten voor beveiligde items herstellen. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Herstel van onmiddellijke items intrekken voor beveiligd item |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/read | Hiermee worden alle geregistreerde containers geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/write | Hiermee maakt u een geregistreerde container |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de container lijst |
> | Action | microsoft.recoveryservices/Vaults/backupJobs/cancel/action | De taak annuleren |
> | Action | microsoft.recoveryservices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Action | micro soft. Recovery Services/kluizen/backupJobs/operationsStatus/lezen | Hiermee wordt de status van de taak bewerking geretourneerd. |
> | Action | microsoft.recoveryservices/Vaults/backupJobs/read | Hiermee worden alle taak objecten geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupJobsExport/action | Taken exporteren |
> | Action | microsoft.recoveryservices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
> | Action | micro soft. Recovery Services/kluizen/backupOperations/lezen | Hiermee wordt de status van de back-upbewerking voor Recovery Services kluis geretourneerd. |
> | Action | microsoft.recoveryservices/Vaults/backupPolicies/delete | Een beveiligings beleid verwijderen |
> | Action | microsoft.recoveryservices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Action | microsoft.recoveryservices/Vaults/backupPolicies/operations/read | Status van beleids bewerking ophalen. |
> | Action | microsoft.recoveryservices/Vaults/backupPolicies/read | Hiermee worden alle beveiligings beleidsregels geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupPolicies/write | Hiermee wordt een beveiligings beleid gemaakt |
> | Action | microsoft.recoveryservices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
> | Action | microsoft.recoveryservices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Action | microsoft.recoveryservices/Vaults/backupProtectionContainers/read | Hiermee worden alle containers van het abonnement geretourneerd |
> | Action | microsoft.recoveryservices/Vaults/backupProtectionIntents/read | Alle intenties van de back-upbeveiliging weer geven |
> | Action | microsoft.recoveryservices/Vaults/backupSecurityPIN/action | Retourneert informatie over de beveiligings pincode voor Recovery Services kluis. |
> | Action | microsoft.recoveryservices/Vaults/backupstorageconfig/read | Hiermee wordt de opslag configuratie voor Recovery Services kluis geretourneerd. |
> | Action | microsoft.recoveryservices/Vaults/backupstorageconfig/write | Hiermee wordt de opslag configuratie voor Recovery Services kluis bijgewerkt. |
> | Action | microsoft.recoveryservices/Vaults/backupUsageSummaries/read | Retourneert samen vattingen voor beveiligde items en beveiligde servers voor een Recovery Services. |
> | Action | microsoft.recoveryservices/Vaults/backupValidateOperation/action | Bewerking voor beveiligd item valideren |
> | Action | Microsoft.RecoveryServices/Vaults/certificates/write | Met de bewerking resource certificaat bijwerken wordt het resource/kluis-referentie certificaat bijgewerkt. |
> | Action | Microsoft.RecoveryServices/Vaults/delete | Met de bewerking kluis verwijderen wordt de opgegeven Azure-resource van het type kluis verwijderd |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee worden de waarschuwingen voor de Recovery Services-kluis opgehaald. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Hiermee wordt de waarschuwing opgelost. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Hiermee wordt de configuratie van de Recovery Services-kluis melding opgehaald. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Hiermee configureert u e-mail meldingen voor de Recovery Services-kluis. |
> | Action | Microsoft.RecoveryServices/Vaults/read | Met de bewerking kluis ophalen wordt een object opgehaald dat de Azure-resource van het type ' kluis ' vertegenwoordigt. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | De registratie van de container unregister kan worden gebruikt om de registratie van een container op te heffen. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking resultaten van de bewerking ophalen kan worden gebruikt om de bewerkings status en het resultaat van de asynchroon ingediende bewerking op te halen |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De bewerking containers ophalen kan worden gebruikt om de containers op te halen die voor een resource zijn geregistreerd. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking service container registreren kan worden gebruikt om een container te registreren bij de Recovery-service. |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Waarschuwings instellingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Waarschuwings instellingen maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationEvents/read | Gebeurtenissen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Infrastructuur resources verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Installatie kopie van de proces server implementeren |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Fabric migreren naar AAD |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/operationresults/lezen | De resultaten van een asynchrone bewerking op de bron-fabrics volgen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/lezen | Alle infra structuren lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Infra structuur verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat voor Fabric vernieuwen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Alle logische netwerken lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Netwerk toewijzingen verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerk toewijzingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Netwerk toewijzingen maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Beveiligbaar item detecteren |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/operationresults/lezen | De resultaten van een asynchrone bewerking op de bron beveiligings containers volgen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Beveiligings containers lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Beveiligings container verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/verwijderen | Migratie-items verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Item migreren |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Alle migratie herstel punten lezen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/operationresults/lezen | De resultaten van een asynchrone bewerking op de bron migratie-items bijhouden |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/lezen | Alle migratie-items lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Migratie testen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Migratie opschonen testen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Migratie-items maken of bijwerken |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/lezen | Beveilig bare items lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Schijven toevoegen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstel punt Toep assen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Alle beveiligde items verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover door voeren |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/operationresults/lezen | De resultaten van een asynchrone bewerking volgen op de bron beveiligde items |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Alle beveiligde items lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Alle replicatie herstel punten lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Beveiligd item verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Schijven verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Replicatie herstellen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligd item opnieuw beveiligen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Feedback verzenden |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Alle doel berekenings grootten lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Failovertest opschonen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/Action | Failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility-service bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Beveiligde items maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Alle beveiligings container toewijzingen verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource Protection-container toewijzingen bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle beveiligings container toewijzingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Toewijzing van beveiligings container verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Beveiligings container toewijzingen maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Beveiligings container overschakelen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Beveiligings containers maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Alle Recovery Services Providers verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationRecoveryServicesProviders/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource Recovery Services Providers bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Recovery Services Providers lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Provider vernieuwen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Recovery Services provider verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Recovery Services Providers maken of bijwerken |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationStorageClassifications/lezen | Alle opslag classificaties lezen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/verwijderen | Alle opslag classificatie toewijzingen verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource Storage-classificatie toewijzingen bijhouden |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/lezen | Alle opslag classificatie toewijzingen lezen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Opslag classificatie toewijzingen maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Alle vCenter verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationFabrics/replicationvCenters/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource-vCenter bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenter lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | VCenter maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Infrastructuur resources maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Taak annuleren |
> | Action | Micro soft. Recovery Services/kluizen/replicationJobs/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource taken bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/read | Alle taken lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Taak opnieuw starten |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Taak hervatten |
> | Action | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Alle migratie-items lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Netwerk toewijzingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Alle netwerken lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Beleids regels verwijderen |
> | Action | Micro soft. Recovery Services/kluizen/replicationPolicies/operationresults/lezen | De resultaten van een asynchrone bewerking op de bron beleidsregels bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Alle beleids regels lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Beleid maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Alle beveiligde items lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Alle beveiligings container toewijzingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Beveiligings containers lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Herstel plannen verwijderen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstel plan voor failover door voeren |
> | Action | Micro soft. Recovery Services/kluizen/replicationRecoveryPlans/operationresults/lezen | De resultaten van een asynchrone bewerking op de resource recovery-plannen bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Herstel plan voor geplande failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Herstel plannen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Herstel plan opnieuw beveiligen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Herstel plan voor failover testen |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Herstel plan voor het opschonen van de Failovertest |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Herstel plan voor failover |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Herstel plannen maken of bijwerken |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Recovery Services Providers lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Alle opslag classificatie toewijzingen lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Alle opslag classificaties lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Alle items lezen  |
> | Action | Microsoft.RecoveryServices/vaults/replicationUsages/read | Een kluis replicatie gebruik lezen |
> | Action | Micro soft. Recovery Services/kluizen/replicationVaultHealth/operationresults/lezen | De resultaten van een asynchrone bewerking op de status van de resource-kluis replicatie bijhouden |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Status van de kluis replicatie lezen |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Status van de kluis vernieuwen |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Alle items lezen  |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Maken of bijwerken  |
> | Action | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Alle vCenter lezen |
> | Action | microsoft.recoveryservices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Action | Microsoft.RecoveryServices/vaults/usages/read | Een kluis gebruik lezen |
> | Action | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking kluis token kan worden gebruikt om het kluis token voor back-upbewerkingen op kluis niveau op te halen. |
> | Action | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Relay/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.Relay/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. gebruik in plaats daarvan CheckNameAvailability. |
> | Action | Micro soft. relay/naam ruimten/authorizationRules/actie | De autorisatie regel voor de naam ruimte wordt bijgewerkt. Deze API is afgeschaft. Gebruik in plaats daarvan een PUT-aanroep om de naam ruimte autorisatie regel bij te werken. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Micro soft. relay/naam ruimten/authorizationRules/verwijderen | Naam ruimte autorisatie regel verwijderen. De standaard regel voor autorisatie van de naam ruimte kan niet worden verwijderd.  |
> | Action | Micro soft. relay/naam ruimten/authorizationRules/listkeys ophalen/actie | De verbindingstekenreeks naar de naamruimte ophalen |
> | Action | Micro soft. relay/naam ruimten/authorizationRules/lezen | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Micro soft. relay/naam ruimten/authorizationRules/schrijven | Maak autorisatie regels voor het naam ruimte niveau en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Action | Micro soft. relay/naam ruimten/verwijderen | De resource van een naamruimte verwijderen |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Action | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de nood herstel configuratie die is gekoppeld aan de naam ruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naam ruimte. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Bewerking voor het bijwerken van HybridConnection. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. Autorisatie regels. Gebruik een aanroep voor het bijwerken van de autorisatie regel. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Bewerking voor het verwijderen van HybridConnection-autorisatie regels |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | De verbindings reeks ophalen voor HybridConnection |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  De lijst met HybridConnection-autorisatie regels ophalen |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Micro soft. relay/naam ruimten/HybridConnections/authorizationRules/schrijven | Maak HybridConnection-autorisatie regels en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels kunnen worden bijgewerkt. |
> | Action | Micro soft. relay/naam ruimten/HybridConnections/verwijderen | Bewerking voor het verwijderen van de HybridConnection-resource |
> | Action | Micro soft. relay/naam ruimten/HybridConnections/lezen | Lijst met HybridConnection-resource beschrijvingen ophalen |
> | Action | Micro soft. relay/naam ruimten/HybridConnections/schrijven | Eigenschappen van HybridConnection maken of bijwerken. |
> | Action | Microsoft.Relay/namespaces/messagingPlan/read | Hiermee wordt het berichten plan voor een naam ruimte opgehaald.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Micro soft. relay/naam ruimten/messagingPlan/schrijven | Hiermee wordt het bericht abonnement voor een naam ruimte bijgewerkt.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Micro soft. relay/naam ruimten/operationresults/lezen | De status van de naamruimtebewerking ophalen |
> | Action | Micro soft. relay/naam ruimten/providers/micro soft. Insights/diagnosticSettings/lezen | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Micro soft. relay/naam ruimten/providers/micro soft. Insights/diagnosticSettings/schrijven | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Micro soft. relay/naam ruimten/providers/micro soft. Insights/logDefinitions/lezen | Lijst met naam ruimte logboek bron beschrijvingen ophalen |
> | Action | Micro soft. relay/naam ruimten/providers/micro soft. Insights/metricDefinitions/lezen | Lijst met resource beschrijvingen voor metrische gegevens van de naam ruimte ophalen |
> | Action | Micro soft. relay/naam ruimten/lezen | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Action | Microsoft.Relay/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/authorizationRules/actie | Bewerking voor het bijwerken van WcfRelay. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. Autorisatie regels. Gebruik een aanroep voor het bijwerken van de autorisatie regel. |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/authorizationRules/verwijderen | Bewerking voor het verwijderen van WcfRelay-autorisatie regels |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | De verbindings reeks ophalen voor WcfRelay |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/authorizationRules/lezen |  De lijst met WcfRelay-autorisatie regels ophalen |
> | Action | Micro soft. relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/Action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/authorizationRules/schrijven | Maak WcfRelay-autorisatie regels en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels kunnen worden bijgewerkt. |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/verwijderen | Bewerking voor het verwijderen van de WcfRelay-resource |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/lezen | Lijst met WcfRelay-resource beschrijvingen ophalen |
> | Action | Micro soft. relay/naam ruimten/WcfRelays/schrijven | Eigenschappen van WcfRelay maken of bijwerken. |
> | Action | Micro soft. relay/naam ruimten/schrijven | Maak een naam ruimte resource en werk de eigenschappen bij. Tags en capaciteit van de naam ruimte zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Action | Microsoft.Relay/operations/read | Bewerkingen ophalen |
> | Action | Microsoft.Relay/register/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk |
> | Action | Microsoft.Relay/unregister/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ResourceHealth/AvailabilityStatuses/actueel/lezen | De beschikbaarheidsstatus ophalen voor de opgegeven resource |
> | Action | Microsoft.ResourceHealth/AvailabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Action | Microsoft.ResourceHealth/events/read | Service Health-gebeurtenissen ophalen voor ingesteld abonnement |
> | Action | Microsoft.Resourcehealth/healthevent/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.Resourcehealth/healthevent/Activated/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.Resourcehealth/healthevent/InProgress/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.Resourcehealth/healthevent/Pending/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.Resourcehealth/healthevent/Resolved/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.Resourcehealth/healthevent/Updated/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Action | Microsoft.ResourceHealth/impactedResources/read | Betreffende resources ophalen voor ingesteld abonnement |
> | Action | Microsoft.ResourceHealth/metadata/read | Haalt meta gegevens op |
> | Action | Microsoft.ResourceHealth/Operations/read | Bewerkingen ophalen die beschikbaar zijn voor Microsoft ResourceHealth |
> | Action | Microsoft.ResourceHealth/register/action | Hiermee wordt het abonnement geregistreerd voor Microsoft ResourceHealth |
> | Action | Microsoft.ResourceHealth/unregister/action | Hiermee wordt de registratie voor het abonnement voor Microsoft ResourceHealth ongedaan gemaakt |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Resources/checkPolicyCompliance/action | Controleer de nalevingsstatus van een bepaalde resource aan de hand van het beleid voor resources. |
> | Action | Microsoft.Resources/checkResourceName/action | Controleer of de resourcenaam geldig is. |
> | Action | Microsoft.Resources/deployments/cancel/action | Hiermee kunt u een implementatie annuleren. |
> | Action | Microsoft.Resources/deployments/delete | Hiermee kunt u een implementatie verwijderen. |
> | Action | Micro soft. resources/implementaties/bewerkingen/lezen | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/deployments/validate/action | Hiermee kunt u een implementatie valideren. |
> | Action | Microsoft.Resources/deployments/whatIf/action | Hiermee worden sjabloonimplementatiewijzigingen voorspeld. |
> | Action | Microsoft.Resources/deployments/write | Hiermee kunt u een implementatie maken of bijwerken. |
> | Action | Microsoft.Resources/links/delete | Hiermee kunt u een resourcekoppeling verwijderen. |
> | Action | Microsoft.Resources/links/read | Hiermee kunt u resourcekoppelingen ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/links/write | Hiermee kunt u een resourcekoppeling maken of bijwerken. |
> | Action | Microsoft.Resources/marketplace/purchase/action | Hiermee worden een resource gekocht via de marketplace. |
> | Action | Microsoft.Resources/providers/read | De lijst met providers ophalen. |
> | Action | Microsoft.Resources/resources/read | Hiermee wordt de lijst met resources op basis van filters opgehaald. |
> | Action | Microsoft.Resources/subscriptions/locations/read | Hiermee kunt u de lijst met ondersteunde locaties ophalen. |
> | Action | Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
> | Action | Microsoft.Resources/subscriptions/providers/read | Hiermee kunt u de resourceproviders ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/read | Hiermee kunt u de lijst met abonnementen ophalen. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/delete | Hiermee kunt u een resourcegroep en de bijhorende resources verwijderen. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Hiermee worden lijsten met statussen van implementatiebewerkingen opgehaald. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Hiermee kunt u een implementatie maken of bijwerken. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Hiermee kunt u resources van een resourcegroep naar een andere resourcegroep verplaatsen. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Hiermee worden de resources voor de resourcegroep opgehaald. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Hiermee wordt de verplaatsing van resources van de ene naar de andere resourcegroep gevalideerd. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/write | Hiermee kunt u een resourcegroep maken of bijwerken. |
> | Action | Microsoft.Resources/subscriptions/resources/read | Hiermee kunt u de resources van een abonnement ophalen. |
> | Action | Microsoft.Resources/subscriptions/tagNames/delete | Hiermee kunt u een abonnementslabel verwijderen. |
> | Action | Microsoft.Resources/subscriptions/tagNames/read | Hiermee kunt u abonnementslabels ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Hiermee kunt een abonnementslabelwaarde verwijderen. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Hiermee kunt u abonnementslabelwaarden ophalen of opnemen in een lijst. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Hiermee kunt u een abonnementslabelwaarde toevoegen. |
> | Action | Microsoft.Resources/subscriptions/tagNames/write | Hiermee kunt u een abonnementslabel toevoegen. |
> | Action | Microsoft.Resources/tags/delete | Hiermee verwijdert u alle tags van een resource. |
> | Action | Microsoft.Resources/tags/read | Hiermee worden alle labels op een resource opgehaald. |
> | Action | Microsoft.Resources/tags/write | Hiermee worden de tags op een resource bijgewerkt door bestaande tags te vervangen of samen te voegen met een nieuwe set tags of door bestaande tags te verwijderen. |
> | Action | Microsoft.Resources/tenants/read | Hiermee kunt u de lijst met tenants ophalen. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Scheduler/jobcollections/delete | Hiermee verwijdert u de taakverzameling. |
> | Action | Microsoft.Scheduler/jobcollections/disable/action | Hiermee schakelt u de taakverzameling uit. |
> | Action | Microsoft.Scheduler/jobcollections/enable/action | Hiermee schakelt u de taakverzameling in. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/delete | Hiermee verwijdert u een taak. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Hiermee wordt een definitie voor een logische app gegenereerd op basis van een Scheduler-taak. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Hiermee haalt u de taakgeschiedenis op. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/read | Hiermee haalt u een taak op. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/run/action | De taak wordt uitgevoerd. |
> | Action | Micro soft. scheduler/jobcollections/Jobs/schrijven | Hiermee maakt u een taak of werkt u er een bij. |
> | Action | Micro soft. scheduler/jobcollections/lezen | Taakverzameling ophalen |
> | Action | Microsoft.Scheduler/jobcollections/write | Hiermee maakt u een taakverzameling of werkt u er een bij. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Search/checkNameAvailability/action | Controleert de beschik baarheid van de service naam. |
> | Action | Micro soft. Search/Operations/lezen | Een lijst met alle beschik bare bewerkingen van de provider micro soft. Search. |
> | Action | Microsoft.Search/register/action | Hiermee wordt het abonnement voor de zoek resource provider geregistreerd en wordt het maken van zoek Services ingeschakeld. |
> | Action | Microsoft.Search/searchServices/createQueryKey/action | Hiermee maakt u de query sleutel. |
> | Action | Microsoft.Search/searchServices/delete | Hiermee verwijdert u de zoek service. |
> | Action | Microsoft.Search/searchServices/deleteQueryKey/delete | Hiermee verwijdert u de query sleutel. |
> | Action | Microsoft.Search/searchServices/listAdminKeys/action | Hiermee worden de beheer sleutels gelezen. |
> | Action | Micro soft. Search/searchServices/listQueryKeys/Action | Retourneert de lijst met query-API-sleutels voor de gegeven Azure Search service. |
> | Action | Microsoft.Search/searchServices/listQueryKeys/read | Retourneert de lijst met query-API-sleutels voor de gegeven Azure Search service. |
> | Action | Microsoft.Search/searchServices/read | Hiermee wordt de zoek service gelezen. |
> | Action | Microsoft.Search/searchServices/regenerateAdminKey/action | Hiermee wordt de beheerders sleutel opnieuw gegenereerd. |
> | Action | Microsoft.Search/searchServices/start/action | Hiermee wordt de zoek service gestart. |
> | Action | Microsoft.Search/searchServices/stop/action | Hiermee stopt u de zoek service. |
> | Action | Microsoft.Search/searchServices/write | Hiermee wordt de zoek service gemaakt of bijgewerkt. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Hiermee worden de opgegeven regels voor het beveiligen van verkeer afgedwongen door het maken van overeenkomende beveiligings regels voor de opgegeven netwerk beveiligings groep (en) |
> | Action | Microsoft.Security/adaptiveNetworkHardenings/read | Hiermee worden aanbevelingen voor adaptieve netwerk beveiliging van een met Azure beveiligde resource opgehaald |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/read | Hiermee worden de geavanceerde instellingen voor bedreigings beveiliging voor de resource opgehaald |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/write | Hiermee worden de geavanceerde instellingen voor bedreigings beveiliging voor de resource bijgewerkt |
> | Action | Microsoft.Security/alerts/read | Hiermee worden alle beschik bare beveiligings waarschuwingen opgehaald |
> | Action | Microsoft.Security/applicationWhitelistings/read | Hiermee wordt de toepassing whitelistings |
> | Action | Microsoft.Security/applicationWhitelistings/write | Hiermee maakt u een nieuwe white list of werkt u een bestaande toepassing bij. |
> | Action | Micro soft. Security/complianceResults/lezen | Hiermee worden de compliantie resultaten voor de resource opgehaald |
> | Action | Microsoft.Security/informationProtectionPolicies/read | Hiermee wordt het Information Protection-beleid voor de resource opgehaald |
> | Action | Microsoft.Security/informationProtectionPolicies/write | Hiermee wordt het Information Protection-beleid voor de resource bijgewerkt |
> | Action | Microsoft.Security/locations/alerts/activate/action | Een beveiligings waarschuwing activeren |
> | Action | Microsoft.Security/locations/alerts/dismiss/action | Een beveiligings waarschuwing negeren |
> | Action | Microsoft.Security/locations/alerts/read | Hiermee worden alle beschik bare beveiligings waarschuwingen opgehaald |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Hiermee wordt het just-in-time-netwerk toegangs beleid verwijderd |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Initieert een just-in-time-aanvraag voor netwerk toegangs beleid |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Hiermee wordt het just-in-time-netwerk toegangs beleid opgehaald |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Hiermee maakt u een nieuw just-in-time-netwerk toegangsbeleid of wordt een bestaand beleid bijgewerkt |
> | Action | Microsoft.Security/locations/read | Hiermee wordt de locatie van de beveiligings gegevens opgehaald |
> | Action | Microsoft.Security/locations/tasks/activate/action | Een beveiligings aanbeveling activeren |
> | Action | Microsoft.Security/locations/tasks/dismiss/action | Een beveiligings aanbeveling negeren |
> | Action | Microsoft.Security/locations/tasks/read | Hiermee worden alle beschik bare beveiligings aanbevelingen opgehaald |
> | Action | Microsoft.Security/locations/tasks/resolve/action | Een beveiligings aanbeveling oplossen |
> | Action | Microsoft.Security/locations/tasks/start/action | Een beveiligings aanbeveling starten |
> | Action | Microsoft.Security/policies/read | Hiermee wordt het beveiligings beleid opgehaald |
> | Action | Microsoft.Security/policies/write | Hiermee wordt het beveiligings beleid bijgewerkt |
> | Action | Micro soft. Security/prijzen/verwijderen | Hiermee worden de prijs instellingen voor de scope verwijderd |
> | Action | Microsoft.Security/pricings/read | Hiermee worden de prijs instellingen voor de scope opgehaald |
> | Action | Micro soft. Security/prijzen/schrijven | Hiermee worden de prijs instellingen voor de scope bijgewerkt |
> | Action | Microsoft.Security/register/action | Hiermee wordt het abonnement voor Azure Security Center geregistreerd |
> | Action | Micro soft. Security/securityContacts/verwijderen | Hiermee wordt de contact persoon beveiliging verwijderd |
> | Action | Microsoft.Security/securityContacts/read | Hiermee wordt de Security-contact persoon opgehaald |
> | Action | Microsoft.Security/securityContacts/write | Hiermee wordt de beveiligings contactpersoon bijgewerkt |
> | Action | Microsoft.Security/securitySolutions/delete | Hiermee verwijdert u een beveiligings oplossing |
> | Action | Microsoft.Security/securitySolutions/read | Hiermee worden de beveiligings oplossingen opgehaald |
> | Action | Microsoft.Security/securitySolutions/write | Hiermee wordt een nieuwe beveiligings oplossing gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Security/securitySolutionsReferenceData/read | Hiermee worden de referentie gegevens van beveiligings oplossingen opgehaald |
> | Action | Micro soft. Security/securityStatuses/lezen | Hiermee worden de beveiligings statussen voor Azure-resources opgehaald |
> | Action | Micro soft. Security/securityStatusesSummaries/lezen | Hiermee worden de samen vattingen van de beveiligings status van het bereik opgehaald |
> | Action | Microsoft.Security/settings/read | Hiermee worden de instellingen voor het bereik opgehaald |
> | Action | Microsoft.Security/settings/write | Hiermee worden de instellingen voor de scope bijgewerkt |
> | Action | Microsoft.Security/tasks/read | Hiermee worden alle beschik bare beveiligings aanbevelingen opgehaald |
> | Action | Micro soft. beveiliging/opheffen/actie | Hiermee wordt de registratie van het abonnement bij Azure Security Center ongedaan gemaakt |
> | Action | Micro soft. Security/webApplicationFirewalls/verwijderen | Hiermee verwijdert u een Web Application Firewall |
> | Action | Micro soft. Security/webApplicationFirewalls/lezen | Hiermee worden de firewalls voor webtoepassingen opgehaald |
> | Action | Micro soft. Security/webApplicationFirewalls/schrijven | Hiermee wordt een nieuwe Web Application Firewall gemaakt of een bestaande bijgewerkt |
> | Action | Microsoft.Security/workspaceSettings/connect/action | Instellingen voor opnieuw verbinden van werk ruimte-instellingen wijzigen |
> | Action | Microsoft.Security/workspaceSettings/delete | Hiermee verwijdert u de werk ruimte-instellingen |
> | Action | Microsoft.Security/workspaceSettings/read | Hiermee worden de werkruimte instellingen opgehaald |
> | Action | Microsoft.Security/workspaceSettings/write | Hiermee werkt u de werk ruimte-instellingen bij |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/delete | Een diagnostische instelling verwijderen |
> | Action | Micro soft. SecurityGraph/diagnosticsettings/lezen | Een diagnostische instelling lezen |
> | Action | Micro soft. SecurityGraph/diagnosticsettings/schrijven | Een diagnostische instelling schrijven |
> | Action | Micro soft. SecurityGraph/diagnosticsettingscategories/lezen | Categorieën voor Diagnostische instellingen lezen |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.ServiceBus/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.ServiceBus/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. gebruik in plaats daarvan CheckNameAvailability. |
> | Action | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Hiermee verwijdert u de VNet-regels in de Service Bus-resourceprovider voor het opgegeven VNet |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/action | De autorisatie regel voor de naam ruimte wordt bijgewerkt. Deze API is afgeschaft. Gebruik in plaats daarvan een PUT-aanroep om de naam ruimte autorisatie regel bij te werken. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Naam ruimte autorisatie regel verwijderen. De standaard regel voor autorisatie van de naam ruimte kan niet worden verwijderd.  |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/write | Maak autorisatie regels voor het naam ruimte niveau en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Action | Microsoft.ServiceBus/namespaces/Delete | De resource van een naamruimte verwijderen |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Action | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de nood herstel configuratie die is gekoppeld aan de naam ruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naam ruimte. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Hiermee wordt het gebeurtenisrasterfilter verwijderd dat is gekoppeld aan de naamruimte. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Hiermee wordt het gebeurtenisrasterfilter opgehaald dat is gekoppeld aan de naamruimte. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Het gebeurtenisrasterfilter gekoppeld aan de naamruimte wprdt gemaakt of bijgewerkt. |
> | Action | Microsoft.ServiceBus/namespaces/eventhubs/read | Lijst met EventHub-resource beschrijvingen ophalen |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | Resource voor IP-filter verwijderen |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Resource voor IP-filter ophalen |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/write | Resource voor IP-filter maken |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | Berichten ontvangen |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | Berichten verzenden |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/read | Hiermee wordt het berichten plan voor een naam ruimte opgehaald.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/write | Hiermee wordt het bericht abonnement voor een naam ruimte bijgewerkt.<br>Deze API is afgeschaft.<br>Eigenschappen die via de MessagingPlan-resource beschikbaar worden gemaakt, worden in latere API-versies verplaatst naar de bron naam ruimte (Parent).<br>Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/migrate/action | Bewerking voor migreren van de naamruimte |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Hiermee wordt de migratieconfiguratie verwijderd. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Hiermee wordt de migratieconfiguratie opgehaald waarmee de status van de migratie en de replicatiebewerkingen in behandeling worden aangegeven |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Hiermee wordt de migratie van de Standard- naar de Premium-naamruimte teruggedraaid |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Hiermee wordt de DNS toegewezen die is gekoppeld aan de migratie van de Standard-naamruimte naar de Premium-naamruimte. Hierna is de migratie voltooid en wordt het synchroniseren van de Standard- naar de Premium-naamruimte stopgezet |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Hiermee wordt de migratie configuratie gemaakt of bijgewerkt. Hiermee wordt begonnen met het synchroniseren van resources van de standaard naar de Premium-naam ruimte |
> | Action | Micro soft. ServiceBus/naam ruimten/networkruleset/verwijderen | Resource voor VNET-regel verwijderen |
> | Action | Micro soft. ServiceBus/naam ruimten/networkruleset/lezen | Hiermee wordt de NetworkRuleSet-resource opgehaald |
> | Action | Micro soft. ServiceBus/naam ruimten/networkruleset/schrijven | Resource voor VNET-regel maken |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/delete | Resource voor VNET-regel verwijderen |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/read | Hiermee wordt de NetworkRuleSet-resource opgehaald |
> | Action | Microsoft.ServiceBus/namespaces/networkrulesets/write | Resource voor VNET-regel maken |
> | Action | Microsoft.ServiceBus/namespaces/operationresults/read | De status van de naamruimtebewerking ophalen |
> | Action | Micro soft. ServiceBus/naam ruimten/providers/micro soft. Insights/diagnosticSettings/lezen | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met resource beschrijvingen voor Diagnostische instellingen van naam ruimte ophalen |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met naam ruimte logboek bron beschrijvingen ophalen |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met resource beschrijvingen voor metrische gegevens van de naam ruimte ophalen |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Bewerking voor het bijwerken van de wachtrij. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. Autorisatie regels. Gebruik een aanroep voor het bijwerken van de autorisatie regel. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Bewerking voor het verwijderen van autorisatie regels voor de wachtrij |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | De verbindings reeks naar de wachtrij ophalen |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  De lijst met autorisatie regels voor de wachtrij ophalen |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Maak autorisatie regels voor de wachtrij en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels kunnen worden bijgewerkt. |
> | Action | Microsoft.ServiceBus/namespaces/queues/Delete | Bewerking voor het verwijderen van de wachtrij resource |
> | Action | Microsoft.ServiceBus/namespaces/queues/read | Lijst met bron beschrijvingen van de wachtrij ophalen |
> | Action | Microsoft.ServiceBus/namespaces/queues/write | Wachtrij-eigenschappen maken of bijwerken. |
> | Action | Microsoft.ServiceBus/namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Action | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Bewerking voor het bijwerken van het onderwerp. Deze bewerking wordt niet ondersteund op API-versie 2017-04-01. Autorisatie regels. Gebruik een aanroep voor het bijwerken van de autorisatie regel. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Bewerking voor het verwijderen van de autorisatie regels voor het onderwerp |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | De verbindings reeks naar het onderwerp ophalen |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  De lijst met autorisatie regels voor het onderwerp ophalen |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Maak autorisatie regels voor het onderwerp en werk de eigenschappen bij. De toegangs rechten voor de autorisatie regels kunnen worden bijgewerkt. |
> | Action | Microsoft.ServiceBus/namespaces/topics/Delete | Bewerking voor het verwijderen van de Onderwerps resource |
> | Action | Microsoft.ServiceBus/namespaces/topics/read | Lijst met beschrijvingen van Onderwerps bronnen ophalen |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Bewerking voor het verwijderen van de TopicSubscription-resource |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Lijst met TopicSubscription-resource beschrijvingen ophalen |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Bewerking voor het verwijderen van de regel Resource |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Lijst met beschrijvingen van regel resources ophalen |
> | Action | Micro soft. ServiceBus/naam ruimten/onderwerpen/abonnementen/regels/schrijven | Regel eigenschappen maken of bijwerken. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Eigenschappen van TopicSubscription maken of bijwerken. |
> | Action | Microsoft.ServiceBus/namespaces/topics/write | Eigenschappen van een onderwerp maken of bijwerken. |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Resource voor VNET-regel verwijderen |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Resource voor VNET-regel ophalen |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Resource voor VNET-regel maken |
> | Action | Microsoft.ServiceBus/namespaces/write | Maak een naam ruimte resource en werk de eigenschappen bij. Tags en capaciteit van de naam ruimte zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Action | Microsoft.ServiceBus/operations/read | Bewerkingen ophalen |
> | Action | Microsoft.ServiceBus/register/action | Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk |
> | Action | Microsoft.ServiceBus/sku/read | Lijst met resource beschrijvingen van SKU ophalen |
> | Action | Microsoft.ServiceBus/sku/regions/read | Lijst met SkuRegions-resource beschrijvingen ophalen |
> | Action | Microsoft.ServiceBus/unregister/action | Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/verwijderen | Een toepassing verwijderen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/lezen | Een toepassing lezen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/verwijderen | Een service verwijderen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/partities/lezen | Een partitie lezen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/partities/replica's/lezen | Een replica lezen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/lezen | Een service lezen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/statussen/lezen | Een servicestatus lezen |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/services/schrijven | Een service maken of bijwerken |
> | Action | Micro soft. ServiceFabric/clusters/toepassingen/schrijven | Een toepassing maken of bijwerken |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/verwijderen | Een toepassingstype verwijderen |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/lezen | Een toepassingstype lezen |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/versies/verwijderen | Een versie van een toepassingstype verwijderen |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/versies/lezen | Een versie van een toepassingstype lezen |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/versies/schrijven | Een versie van een toepassingstype maken of bijwerken |
> | Action | Micro soft. ServiceFabric/clusters/applicationTypes/schrijven | Een toepassingstype maken of bijwerken |
> | Action | Micro soft. ServiceFabric/clusters/verwijderen | Een cluster verwijderen |
> | Action | Micro soft. ServiceFabric/clusters/knoop punten/lezen | Een knooppunt lezen |
> | Action | Micro soft. ServiceFabric/clusters/lezen | Een cluster lezen |
> | Action | Micro soft. ServiceFabric/clusters/statussen/lezen | Een clusterstatus lezen |
> | Action | Micro soft. ServiceFabric/clusters/schrijven | Een cluster maken of bijwerken |
> | Action | Microsoft.ServiceFabric/locations/clusterVersions/read | Een clusterversie lezen |
> | Action | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Een clusterversie voor een specifieke omgeving lezen |
> | Action | Micro soft. ServiceFabric/locaties/operationresults/lezen | Bewerkingsresultaten lezen |
> | Action | Micro soft. ServiceFabric/locaties/bewerkingen/lezen | Bewerkingen op locatie lezen |
> | Action | Micro soft. ServiceFabric/Operations/lezen | Beschikbare bewerkingen lezen |
> | Action | Micro soft. ServiceFabric/REGI ster/actie | Een actie registreren |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.SignalRService/locations/checknameavailability/action | Hiermee wordt gecontroleerd of een naam beschikbaar is voor gebruik met een nieuwe signalerings service |
> | Action | Micro soft. SignalRService/locations/operationresults/Signaler/Read | De status van een asynchrone bewerking opvragen |
> | Action | Microsoft.SignalRService/locations/operationStatuses/operationId/read | De status van een asynchrone bewerking opvragen |
> | Action | Microsoft.SignalRService/locations/usages/read | Het quotum gebruik voor de Azure signalerings service ophalen |
> | Action | Micro soft. SignalRService/operationresults/lezen | De status van een asynchrone bewerking opvragen |
> | Action | Micro soft. SignalRService/operationstatus/lezen | De status van een asynchrone bewerking opvragen |
> | Action | Microsoft.SignalRService/register/action | Hiermee wordt de resource provider micro soft. SignalRService geregistreerd bij een abonnement |
> | Action | Micro soft. SignalRService/signalering/verwijderen | De volledige signalerings service verwijderen |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/delete | Een gebeurtenis raster filter verwijderen van een Signaaler. |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/read | Hiermee worden de eigenschappen van het opgegeven gebeurtenis raster filter opgehaald of worden alle gebeurtenis raster filters voor de opgegeven Signa lering weer gegeven. |
> | Action | Microsoft.SignalRService/SignalR/eventGridFilters/write | Een gebeurtenis raster filter voor een signaal sterkte met de opgegeven para meters maken of bijwerken. |
> | Action | Microsoft.SignalRService/SignalR/listkeys/action | De waarde van de toegangs sleutels voor de Signa lering in de beheer portal of via API weer geven |
> | Action | Microsoft.SignalRService/SignalR/read | De instellingen en configuraties van de signaal sterkte weer geven in de beheer portal of via API |
> | Action | Microsoft.SignalRService/SignalR/regeneratekey/action | Wijzig de waarde van de toegangs sleutels voor de Signa lering in de beheer portal of via API |
> | Action | Microsoft.SignalRService/SignalR/restart/action | Een Azure signalerings service opnieuw starten in de beheer portal of via de API. Er is een bepaalde uitval tijd. |
> | Action | Micro soft. SignalRService/seingevings/schrijven | Wijzig de instellingen en configuraties van de signaal sterkte in de beheer portal of via API |
> | Action | Microsoft.SignalRService/unregister/action | Hiermee wordt de registratie van de resource provider micro soft. SignalRService met een abonnement ongedaan gemaakt |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. Solutions/applicationDefinitions/applicationArtifacts/lezen | Hiermee worden de artefacten voor toepassingen van de toepassingsdefinitie weergegeven. |
> | Action | Microsoft.Solutions/applicationDefinitions/delete | Hiermee wordt een toepassingsdefinitie verwijderd. |
> | Action | Microsoft.Solutions/applicationDefinitions/read | Hiermee wordt een lijst met toepassingsdefinities opgehaald. |
> | Action | Microsoft.Solutions/applicationDefinitions/write | Hiermee wordt een toepassingsdefinitie toegevoegd of gewijzigd. |
> | Action | Microsoft.Solutions/applications/applicationArtifacts/read | Hiermee worden artefacten voor toepassingen weergegeven. |
> | Action | Microsoft.Solutions/applications/delete | Hiermee verwijdert u een toepassing. |
> | Action | Microsoft.Solutions/applications/read | Hiermee wordt een lijst met toepassingen opgehaald. |
> | Action | Microsoft.Solutions/applications/refreshPermissions/action | Hiermee worden machtigingen van de toepassing vernieuwd. |
> | Action | Microsoft.Solutions/applications/updateAccess/action | Hiermee wordt de toegang tot toepassingen bijgewerkt. |
> | Action | Microsoft.Solutions/applications/write | Hiermee wordt een toepassing gemaakt. |
> | Action | Microsoft.Solutions/jitRequests/delete | Een JIT-aanvraag verwijderen |
> | Action | Microsoft.Solutions/jitRequests/read | Hiermee wordt een lijst met JIT-aanvragen opgehaald |
> | Action | Micro soft. Solutions/jitRequests/write | Een JIT-aanvraag maken |
> | Action | Microsoft.Solutions/locations/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Action | Micro soft. Solutions/REGI ster/actie | Registreren bij Solutions. |
> | Action | Micro soft. Solutions/registratie/actie | Hiermee wordt de registratie bij oplossingen ongedaan gemaakt. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Sql/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het wereld wijd inrichten van een bepaald abonnement. |
> | Action | Microsoft.Sql/instancePools/delete | Hiermee wordt een exemplaar groep verwijderd |
> | Action | Microsoft.Sql/instancePools/read | Hiermee wordt een exemplaar groep opgehaald |
> | Action | Microsoft.Sql/instancePools/usages/read | Hiermee worden de gebruiks gegevens van een exemplaar groep opgehaald |
> | Action | Microsoft.Sql/instancePools/write | Hiermee wordt een exemplaar groep gemaakt of bijgewerkt |
> | Action | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Resultaat van de set-bewerking voor het controle beleid voor de uitgebreide server-BLOB ophalen |
> | Action | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Resultaat ophalen van de set-bewerking voor het controle beleid voor Server-blobs |
> | Action | Micro soft. SQL/locaties/mogelijkheden/lezen | Hiermee worden de mogelijkheden voor dit abonnement op een bepaalde locatie opgehaald |
> | Action | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Hiermee wordt de status van een database bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/databaseOperationResults/read | Hiermee wordt de status van een database bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd op de verwijderde server |
> | Action | Microsoft.Sql/locations/deletedServerOperationResults/read | Hiermee worden bewerkingen uitgevoerd op de verwijderde server |
> | Action | Microsoft.Sql/locations/deletedServers/read | De lijst met verwijderde servers retour neren of de eigenschappen voor de opgegeven verwijderde server ophalen. |
> | Action | Microsoft.Sql/locations/deletedServers/recover/action | Een verwijderde server herstellen |
> | Action | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Hiermee wordt de Azure async-bewerking voor een async-bewerking van een elastische groep opgehaald |
> | Action | Microsoft.Sql/locations/elasticPoolOperationResults/read | Hiermee wordt het resultaat van een elastische groeps bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd op de transparante versleutelings beveiliging voor gegevens versleuteling |
> | Action | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Hiermee worden bewerkingen uitgevoerd op de transparante versleutelings beveiliging voor gegevens versleuteling |
> | Action | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Resultaat van de set-bewerking voor het controle beleid voor de uitgebreide server-BLOB ophalen |
> | Action | Micro soft. SQL/locaties/extendedAuditingSettingsOperationResults/lezen | Resultaat van de set-bewerking voor het controle beleid voor de uitgebreide server-BLOB ophalen |
> | Action | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Hiermee wordt de status van een firewall regel bewerking opgehaald. |
> | Action | Micro soft. SQL/locaties/firewallRulesOperationResults/lezen | Hiermee wordt de status van een firewall regel bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/delete | Hiermee verwijdert u een bestaande failovergroep voor een exemplaar. |
> | Action | Micro soft. SQL/locaties/instanceFailoverGroups/failover/actie | Voert een geplande failover uit in een bestaande failovergroep voor een exemplaar. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Hiermee wordt geforceerde failover uitgevoerd in een bestaande failovergroep voor een exemplaar. |
> | Action | Micro soft. SQL/locaties/instanceFailoverGroups/lezen | Hiermee wordt de lijst met failover-groepen voor instanties geretourneerd of worden de eigenschappen opgehaald voor de opgegeven failovergroep voor het exemplaar. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/write | Hiermee maakt u een failovergroep voor een exemplaar met de opgegeven para meters of werkt u de eigenschappen of labels bij voor de opgegeven failovergroep voor het exemplaar. |
> | Action | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Hiermee wordt de status van een exemplaar groep-bewerking opgehaald |
> | Action | Microsoft.Sql/locations/instancePoolOperationResults/read | Hiermee wordt het resultaat voor een bewerking van een exemplaar groep opgehaald |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Hiermee worden de details van een specifiek interface-eind punt Azure async-bewerking geretourneerd |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Retourneert de details van de opgegeven bewerking voor het eindpunt Profiel van de interface |
> | Action | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Hiermee wordt de status van een taak agent bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/jobAgentOperationResults/read | Hiermee wordt het resultaat van een taak agent bewerking opgehaald. |
> | Action | Microsoft.Sql/locations/longTermRetentionBackups/read | Een lijst met de back-ups voor lange termijn retentie voor elke Data Base op elke server op een locatie |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Een lijst met de back-ups voor lange termijn retentie voor elke Data Base op een server |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Hiermee wordt een back-up voor de lange termijn verwijderd |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Een lijst met de back-ups voor lange termijn retentie voor een Data Base |
> | Action | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | De herstel bewerking voor de beheerde data base volt ooien |
> | Action | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd op transparante gegevens versleuteling beheerde instantie versleuteling beveiliging |
> | Action | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Hiermee worden bewerkingen uitgevoerd op transparante gegevens versleuteling beheerde instantie versleuteling beveiliging |
> | Action | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd op de transparante instantie sleutels van beheerde exemplaren van gegevens versleuteling |
> | Action | Micro soft. SQL/locaties/managedInstanceKeyOperationResults/lezen | Hiermee worden bewerkingen uitgevoerd op de transparante instantie sleutels van beheerde exemplaren van gegevens versleuteling |
> | Action | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd voor de transparante gegevens versleuteling van de beheerde data base |
> | Action | Micro soft. SQL/locaties/managedTransparentDataEncryptionOperationResults/lezen | Hiermee worden bewerkingen uitgevoerd voor de transparante gegevens versleuteling van de beheerde data base |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionAzureAsyncOperation/read | Hiermee wordt het resultaat voor een verbindings bewerking voor een privé-eind punt opgehaald |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionOperationResults/read | Hiermee wordt het resultaat voor een verbindings bewerking voor een privé-eind punt opgehaald |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Hiermee wordt het resultaat opgehaald voor een proxy bewerking voor een privé-eindpunt verbinding |
> | Action | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Hiermee wordt het resultaat opgehaald voor een proxy bewerking voor een privé-eindpunt verbinding |
> | Action | Micro soft. SQL/locaties/lezen | Hiermee worden de beschik bare locaties voor een gegeven abonnement opgehaald |
> | Action | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Hiermee worden bewerkingen uitgevoerd op de transparante server sleutels voor gegevens versleuteling |
> | Action | Micro soft. SQL/locaties/serverKeyOperationResults/lezen | Hiermee worden bewerkingen uitgevoerd op de transparante server sleutels voor gegevens versleuteling |
> | Action | Microsoft.Sql/locations/syncAgentOperationResults/read | Resultaat van de resource bewerking van de synchronisatie agent ophalen |
> | Action | Microsoft.Sql/locations/syncDatabaseIds/read | De data base-id's van de synchronisatie voor een bepaalde regio en een abonnement ophalen |
> | Action | Microsoft.Sql/locations/syncGroupOperationResults/read | Resultaat van de resource bewerking synchronisatie groep ophalen |
> | Action | Microsoft.Sql/locations/syncMemberOperationResults/read | Resultaat van de bewerking voor het synchroniseren van leden bronnen ophalen |
> | Action | Micro soft. SQL/locaties/gebruik/lezen | Hiermee wordt een verzameling metrische gegevens over gebruik voor dit abonnement opgehaald op een locatie |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Retourneert de details van de opgegeven virtuele netwerk regels Azure async-bewerking  |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Hiermee worden de details van de opgegeven bewerking voor virtuele netwerk regels geretourneerd  |
> | Action | Microsoft.Sql/managedInstances/administrators/delete | Hiermee verwijdert u een bestaande beheerder van het beheerde exemplaar. |
> | Action | Microsoft.Sql/managedInstances/administrators/read | Hiermee wordt een lijst met beheerders van beheerde instanties opgehaald. |
> | Action | Micro soft. SQL/managedInstances/beheerders/schrijven | Hiermee wordt een beheerde exemplaar beheerder met de opgegeven para meters gemaakt of bijgewerkt. |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Hiermee wordt een Bewaar beleid voor de korte termijn voor een beheerde data base opgehaald |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Hiermee wordt een Bewaar beleid voor de korte termijn voor een beheerde data base bijgewerkt |
> | Action | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/write | Gevoeligheids labels batch-update |
> | Action | Microsoft.Sql/managedInstances/databases/delete | Hiermee verwijdert u een bestaande beheerde data base |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor beheerde exemplaar databases opgehaald |
> | Action | Microsoft.Sql/managedInstances/databases/read | Hiermee wordt een bestaande beheerde data base opgehaald |
> | Action | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/read | Een beheerd database schema ophalen. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Een kolom met beheerde data base ophalen |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Het gevoeligheids label van een bepaalde kolom verwijderen |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Gevoeligheids aanbevelingen voor een bepaalde kolom uitschakelen |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Gevoeligheids aanbevelingen voor een bepaalde kolom inschakelen |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Het gevoeligheids label van een bepaalde kolom ophalen |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Het gevoeligheids label van een bepaalde kolom maken of bijwerken |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Een beheerde database tabel ophalen |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Een lijst met beleids regels voor detectie van het beleid voor beheerde data bases die zijn geconfigureerd voor een bepaalde server ophalen |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Het beleid voor het detecteren van de Data Base voor een bepaalde beheerde data base wijzigen |
> | Action | Microsoft.Sql/managedInstances/databases/securityEvents/read | Hiermee worden de beveiligings gebeurtenissen van de beheerde data base opgehaald |
> | Action | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Details van de data base ophalen Transparent Data Encryption op een bepaalde beheerde data base |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | De data base-Transparent Data Encryption voor een bepaalde beheerde data base wijzigen |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | De evaluatie van beveiligings problemen voor een bepaalde data base verwijderen |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Het beleid voor evaluatie van beveiligings problemen ophalen voor een givendatabase |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | De regel basislijn voor evaluatie van beveiligings problemen verwijderen voor een bepaalde data base |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | De regel basislijn voor evaluatie van beveiligings problemen ophalen voor een bepaalde data base |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | De regel voor de evaluatie van beveiligings problemen wijzigen voor een bepaalde data base |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Een bestaand scan resultaat converteren naar een lees bare indeling. Als er al bestaat, gebeurt er niets |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Voer de database scan voor evaluatie van beveiligings problemen uit. |
> | Action | Micro soft. SQL/managedInstances/data bases/vulnerabilityAssessments/scans/lezen | Retour neer de lijst met evaluatie records van het beveiligings probleem voor de data base of haal de scan record op voor de opgegeven scan-ID. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | De evaluatie van beveiligings problemen voor een bepaalde data base wijzigen |
> | Action | Microsoft.Sql/managedInstances/databases/write | Hiermee maakt u een nieuwe data base of werkt u een bestaande data base bij. |
> | Action | Micro soft. SQL/managedInstances/verwijderen | Hiermee verwijdert u een bestaand beheerd exemplaar. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/read | Hiermee wordt een lijst met server versleutelings beveiligingen geretourneerd of worden de eigenschappen opgehaald voor de opgegeven server versleutelings beveiliging. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/write | Werk de eigenschappen voor de opgegeven server versleutelings beveiliging bij. |
> | Action | Micro soft. SQL/managedInstances/sleutels/verwijderen | Hiermee verwijdert u een bestaande Azure SQL Managed instance-sleutel. |
> | Action | Micro soft. SQL/managedInstances/sleutels/lezen | Hiermee wordt de lijst met beheerde exemplaar sleutels geretourneerd of worden de eigenschappen opgehaald voor de opgegeven sleutel van het beheerde exemplaar. |
> | Action | Microsoft.Sql/managedInstances/keys/write | Hiermee maakt u een sleutel met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven sleutel van het beheerde exemplaar bij. |
> | Action | Micro soft. SQL/managedInstances/metricDefinitions/lezen | Metrische definities voor beheerde instanties ophalen |
> | Action | Micro soft. SQL/managedInstances/metrieken/lezen | Metrische gegevens van beheerde instanties ophalen |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor beheerde instanties opgehaald |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens retour neren die beschikbaar zijn voor beheerde instanties |
> | Action | Microsoft.Sql/managedInstances/read | De lijst met beheerde exemplaren retour neren of de eigenschappen van het opgegeven beheerde exemplaar ophalen. |
> | Action | Micro soft. SQL/managedInstances/recoverableDatabases/lezen | Hiermee wordt een lijst met herstel bare beheerde data bases geretourneerd |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Hiermee wordt een Bewaar beleid voor de korte termijn opgehaald voor een verwijderde beheerde data base |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Hiermee wordt een Bewaar beleid voor de korte termijn bijgewerkt voor een verwijderde beheerde data base |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Retourneert een lijst met restorable verwijderde beheerde data bases. |
> | Action | Micro soft. SQL/managedInstances/securityAlertPolicies/lezen | Een lijst met beleids regels voor detectie van begeleide server dreigingen ophalen die zijn geconfigureerd voor een bepaalde server |
> | Action | Micro soft. SQL/managedInstances/securityAlertPolicies/schrijven | Het beleid voor detectie van begeleide server dreigingen voor een beheerde server wijzigen |
> | Action | Microsoft.Sql/managedInstances/tdeCertificates/action | TDE-certificaat maken/bijwerken |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | De evaluatie van beveiligings problemen voor een bepaald beheerd exemplaar verwijderen |
> | Action | Micro soft. SQL/managedInstances/vulnerabilityAssessments/lezen | Het beleid voor evaluatie van beveiligings problemen voor een gegeven beheerd exemplaar ophalen |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | De evaluatie van beveiligings problemen voor een bepaald beheerd exemplaar wijzigen |
> | Action | Micro soft. SQL/managedInstances/schrijven | Hiermee maakt u een beheerd exemplaar met de opgegeven para meters of werkt u de eigenschappen of labels voor het opgegeven beheerde exemplaar bij. |
> | Action | Micro soft. SQL/Operations/lezen | Beschik bare REST bewerkingen ophalen |
> | Action | Micro soft. SQL/privateEndpointConnectionsApproval/actie | Hiermee wordt bepaald of de gebruiker een verbinding met een privé-eind punt mag goed keuren |
> | Action | Micro soft. SQL/REGI ster/actie | Hiermee wordt het abonnement voor de resource provider van micro soft SQL Database geregistreerd en wordt het maken van micro soft SQL-data bases mogelijk. |
> | Action | Micro soft. SQL/servers/administratorOperationResults/lezen | Hiermee worden bewerkingen uitgevoerd op de server beheerders |
> | Action | Micro soft. SQL/servers/beheerders/verwijderen | Server beheerder verwijderen |
> | Action | Micro soft. SQL/servers/beheerders/lezen | Details van de server beheerder ophalen |
> | Action | Micro soft. SQL/servers/beheerders/schrijven | Server beheerder maken of bijwerken |
> | Action | Micro soft. SQL/servers/Advisor/lezen | Hiermee wordt een lijst met beschik bare Advisor voor de server geretourneerd |
> | Action | Micro soft. SQL/servers/Advisors/recommendedActions/lezen | Hiermee wordt een lijst met aanbevolen acties van opgegeven Advisor voor de server geretourneerd |
> | Action | Micro soft. SQL/servers/Advisors/recommendedActions/schrijven | De aanbevolen actie Toep assen op de server |
> | Action | Micro soft. SQL/servers/Advisors/schrijven | Hiermee wordt de status voor automatisch uitvoeren van een Advisor op server niveau bijgewerkt. |
> | Action | Micro soft. SQL/servers/auditingPolicies/lezen | Details ophalen van het standaard controle beleid voor Server tabellen dat op een bepaalde server is geconfigureerd |
> | Action | Micro soft. SQL/servers/auditingPolicies/schrijven | De standaard controle van de Server tabel voor een bepaalde server wijzigen |
> | Action | Micro soft. SQL/servers/auditingSettings/operationResults/lezen | Resultaat ophalen van de set-bewerking voor het controle beleid voor Server-blobs |
> | Action | Micro soft. SQL/servers/auditingSettings/lezen | Details ophalen van het controle beleid voor de server-BLOB dat op een bepaalde server is geconfigureerd |
> | Action | Micro soft. SQL/servers/auditingSettings/schrijven | De controle van de server-BLOB voor een bepaalde server wijzigen |
> | Action | Microsoft.Sql/servers/automaticTuning/read | Retourneert instellingen voor automatisch afstemmen voor de server |
> | Action | Micro soft. SQL/servers/automaticTuning/schrijven | Hiermee worden de instellingen voor automatisch afstemmen van de server bijgewerkt en worden bijgewerkte instellingen geretourneerd |
> | Action | Micro soft. SQL/servers/backupLongTermRetentionVaults/verwijderen | Hiermee verwijdert u een bestaande back-uparchief kluis. |
> | Action | Micro soft. SQL/servers/backupLongTermRetentionVaults/lezen | Deze bewerking wordt gebruikt om een back-upkluis voor lange termijn retentie te verkrijgen. Hiermee wordt informatie geretourneerd over de kluis die op deze server is geregistreerd |
> | Action | Micro soft. SQL/servers/backupLongTermRetentionVaults/schrijven | Met deze bewerking wordt een back-upkluis voor lange termijn registratie op een server geregistreerd |
> | Action | Microsoft.Sql/servers/communicationLinks/delete | Hiermee verwijdert u een bestaande server communicatie koppeling. |
> | Action | Microsoft.Sql/servers/communicationLinks/read | Hiermee wordt de lijst met communicatie koppelingen van een opgegeven server geretourneerd. |
> | Action | Microsoft.Sql/servers/communicationLinks/write | Een server communicatie koppeling maken of bijwerken. |
> | Action | Micro soft. SQL/servers/connectionPolicies/lezen | De lijst met server verbindings beleid van een opgegeven server retour neren. |
> | Action | Micro soft. SQL/servers/connectionPolicies/schrijven | Een server verbindings beleid maken of bijwerken. |
> | Action | Micro soft. SQL/servers/data bases/advisores/lezen | Hiermee wordt een lijst geretourneerd met de Advisor die beschikbaar zijn voor de data base |
> | Action | Micro soft. SQL/servers/data bases/advisores/recommendedActions/lezen | Hiermee wordt een lijst met aanbevolen acties van de opgegeven Advisor voor de data base geretourneerd |
> | Action | Micro soft. SQL/servers/data bases/adviseurs/recommendedActions/schrijven | De aanbevolen actie Toep assen op de data base |
> | Action | Micro soft. SQL/servers/data bases/advisores/schrijven | Update status automatisch uitvoeren van een Advisor op database niveau. |
> | Action | Microsoft.Sql/servers/databases/auditingPolicies/read | Details ophalen van het controle beleid voor tabellen dat is geconfigureerd voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/auditingPolicies/schrijven | Het controle beleid voor tabellen voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/auditingSettings/lezen | Details ophalen van het BLOB-controle beleid dat is geconfigureerd voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/auditingSettings/schrijven | Het BLOB-controle beleid voor een bepaalde data base wijzigen |
> | Action | Microsoft.Sql/servers/databases/auditRecords/read | De data base-BLOB-controle records ophalen |
> | Action | Micro soft. SQL/servers/data bases/automaticTuning/lezen | Retourneert instellingen voor automatisch afstemmen voor een Data Base |
> | Action | Micro soft. SQL/servers/data bases/automaticTuning/schrijven | Hiermee worden de instellingen voor automatisch afstemmen voor een Data Base bijgewerkt en worden bijgewerkte instellingen geretourneerd |
> | Action | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Hiermee wordt de status van een database bewerking opgehaald. |
> | Action | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | De lijst met back-uparchiverbeleid van een opgegeven Data Base retour neren. |
> | Action | Micro soft. SQL/servers/data bases/backupLongTermRetentionPolicies/schrijven | Een archief beleid voor back-ups van de data base maken of bijwerken. |
> | Action | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/read | Hiermee wordt een Bewaar beleid voor de korte termijn voor een Data Base opgehaald |
> | Action | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/write | Hiermee wordt een Bewaar beleid voor de korte termijn voor een Data Base bijgewerkt |
> | Action | Microsoft.Sql/servers/databases/connectionPolicies/read | Details ophalen van het verbindings beleid dat is geconfigureerd voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/connectionPolicies/schrijven | Het verbindings beleid voor een bepaalde data base wijzigen |
> | Action | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/servers/databases/currentSensitivityLabels/write | Gevoeligheids labels batch-update |
> | Action | Micro soft. SQL/servers/data bases/dataMaskingPolicies/lezen | Retour neer de lijst met beleids regels voor database gegevens maskering. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Beleids regel voor gegevens maskering voor een bepaalde data base verwijderen |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Details ophalen van de beleids regel voor gegevens maskering die is geconfigureerd voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/dataMaskingPolicies/Rules/write | Beleids regel voor gegevens maskering wijzigen voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/dataMaskingPolicies/schrijven | Gegevens maskerings beleid voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/dataWarehouseQueries/dataWarehouseQuerySteps/lezen | Retourneert de stap gegevens van de gedistribueerde query van de Data Warehouse-query voor de geselecteerde stap-ID |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Retourneert de gegevens van de distributie query van het Data Warehouse voor de geselecteerde query-ID |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Hiermee worden de gebruikers activiteiten opgehaald van een SQL Data Warehouse-exemplaar dat actieve en onderbroken query's bevat |
> | Action | Micro soft. SQL/servers/data bases/verwijderen | Hiermee verwijdert u een bestaande data base. |
> | Action | Microsoft.Sql/servers/databases/export/action | Azure SQL Database exporteren |
> | Action | Micro soft. SQL/servers/data bases/extendedAuditingSettings/lezen | Details ophalen van het uitgebreide BLOB-controle beleid dat is geconfigureerd voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/extendedAuditingSettings/schrijven | Het uitgebreide BLOB-controle beleid voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/uitbrei dingen/lezen | Hiermee wordt een verzameling extensies opgehaald voor de data base. |
> | Action | Micro soft. SQL/servers/data bases/extensies/schrijven | De extensie voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/failover/actie | Door klant geïnitieerde data base-failover. |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Geografisch back-upbeleid voor een bepaalde data base ophalen |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Een beleid voor geoback-ups van de data base maken of bijwerken |
> | Action | Microsoft.Sql/servers/databases/importExportOperationResults/read | Hiermee worden bewerkingen voor importeren/exporteren uitgevoerd |
> | Action | Micro soft. SQL/servers/data bases/maintenanceWindowOptions/lezen | Hiermee wordt een lijst met beschik bare onderhouds Vensters voor een geselecteerde data base opgehaald. |
> | Action | Micro soft. SQL/servers/data bases/maintenanceWindows/lezen | Hiermee worden de instellingen voor onderhouds Vensters voor een geselecteerde data base opgehaald. |
> | Action | Micro soft. SQL/servers/data bases/maintenanceWindows/schrijven | Hiermee stelt u de Windows-instellingen voor onderhoud in voor een geselecteerde data base. |
> | Action | Micro soft. SQL/servers/data bases/metricDefinitions/lezen | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Micro soft. SQL/servers/data bases/metrische gegevens/lezen | Metrische gegevens voor data bases retour neren |
> | Action | Microsoft.Sql/servers/databases/move/action | De naam van een bestaande data base wijzigen. |
> | Action | Micro soft. SQL/servers/data bases/operationResults/lezen | Hiermee wordt de status van een database bewerking opgehaald. |
> | Action | Microsoft.Sql/servers/databases/operations/cancel/action | Annuleert Azure SQL Database asynchrone bewerking in behandeling die nog niet is voltooid. |
> | Action | Micro soft. SQL/servers/data bases/Operations/lezen | De lijst met bewerkingen die worden uitgevoerd op de data base retour neren |
> | Action | Microsoft.Sql/servers/databases/pause/action | Azure SQL Data Warehouse database onderbreken |
> | Action | Micro soft. SQL/servers/data bases/providers/micro soft. Insights/diagnosticSettings/lezen | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Micro soft. SQL/servers/data bases/providers/micro soft. Insights/diagnosticSettings/schrijven | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor data bases opgehaald |
> | Action | Micro soft. SQL/servers/data bases/providers/micro soft. Insights/metricDefinitions/lezen | Typen metrische gegevens die beschikbaar zijn voor data bases retour neren |
> | Action | Micro soft. SQL/servers/data bases/queryStore/queryTexts/lezen | Retourneert de verzameling query teksten die overeenkomen met de opgegeven para meters. |
> | Action | Micro soft. SQL/servers/data bases/queryStore/lezen | Retourneert de huidige waarden van de query Store-instellingen voor de data base. |
> | Action | Micro soft. SQL/servers/data bases/queryStore/schrijven | Query Store-instelling voor de data base bijwerken |
> | Action | Micro soft. SQL/servers/data bases/lezen | De lijst met data bases retour neren of de eigenschappen voor de opgegeven Data Base ophalen. |
> | Action | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/delete | De replicatie relatie geforceerd en met mogelijk gegevens verlies beëindigen |
> | Action | Micro soft. SQL/servers/data bases/replicationLinks/failover/actie | Failover na het synchroniseren van alle wijzigingen van de primaire, waardoor deze data base in de replicatie relationship\u0027s primair en de externe primaire in een secundaire |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Failover direct met mogelijk gegevens verlies, waardoor deze data base in de replicatie relationship\u0027s primair en de externe primaire in een secundaire |
> | Action | Micro soft. SQL/servers/data bases/replicationLinks/lezen | De lijst met replicatie koppelingen retour neren of de eigenschappen ophalen voor de opgegeven replicatie koppelingen. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | De replicatie relatie geforceerd of na synchronisatie met de partner beëindigen |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Replicatie modus bijwerken voor koppeling naar synchrone of asynchrone modus |
> | Action | Microsoft.Sql/servers/databases/restorePoints/action | Hiermee wordt een nieuw herstelpunt gemaakt |
> | Action | Microsoft.Sql/servers/databases/restorePoints/delete | Hiermee verwijdert u een herstel punt voor de data base. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/read | Retourneert herstel punten voor de data base. |
> | Action | Microsoft.Sql/servers/databases/resume/action | Azure SQL Data Warehouse database hervatten |
> | Action | Micro soft. SQL/servers/data bases/schema's/lezen | Een database schema ophalen. |
> | Action | Micro soft. SQL/servers/data bases/schema's/tabellen/kolommen/lezen | Een database kolom ophalen. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Het gevoeligheids label van een bepaalde kolom verwijderen |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Gevoeligheids aanbevelingen voor een bepaalde kolom uitschakelen |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Gevoeligheids aanbevelingen voor een bepaalde kolom inschakelen |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Het gevoeligheids label van een bepaalde kolom ophalen |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Het gevoeligheids label van een bepaalde kolom maken of bijwerken |
> | Action | Micro soft. SQL/servers/data bases/schema's/tabellen/lezen | Een database tabel ophalen. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Lijst met index aanbevelingen voor een Data Base ophalen |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Aanbeveling index Toep assen |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Een lijst ophalen met beleids regels voor het detecteren van de database bedreigingen die zijn geconfigureerd voor een bepaalde server |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Het beleid voor het detecteren van de beleids regels voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/securityMetrics/lezen | Hiermee wordt een verzameling van metrische gegevens voor database beveiliging opgehaald |
> | Action | Microsoft.Sql/servers/databases/sensitivityLabels/read | Gevoeligheids labels van een bepaalde Data Base weer geven |
> | Action | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Retour suggestie over het omhoog of omlaag schalen van de Data Base op basis van statistieken voor query-uitvoering om prestaties te verbeteren of kosten te verlagen |
> | Action | Microsoft.Sql/servers/databases/skus/read | Hiermee wordt een verzameling van sku's opgehaald die beschikbaar zijn voor een Data Base |
> | Action | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Synchronisatie groep annuleren |
> | Action | Microsoft.Sql/servers/databases/syncGroups/delete | Hiermee verwijdert u een bestaande synchronisatie groep. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | De lijst met data base-schema's voor de synchronisatie hub retour neren |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/logboeken/lezen | De lijst met Logboeken voor synchronisatie groepen retour neren |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/lezen | De lijst met synchronisatie groepen retour neren of de eigenschappen voor de opgegeven synchronisatie groep ophalen. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Data base-schema voor synchronisatie-hub vernieuwen |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Resultaat van de bewerking synchronisatie hub-schema vernieuwen ophalen |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/syncMembers/verwijderen | Hiermee verwijdert u een bestaand synchronisatie-lid. |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/syncMembers/lezen | De lijst met synchronisatie leden retour neren of de eigenschappen van het opgegeven Sync-lid ophalen. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Synchronisatie leden schema vernieuwen |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Resultaat ophalen van de vernieuwings bewerking voor het synchronisatie leden schema |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/syncMembers/schema's/lezen | De lijst met database schema's van het synchronisatie-lid retour neren |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/syncMembers/schrijven | Hiermee maakt u een Sync-lid met de opgegeven para meters of werkt u de eigenschappen voor het opgegeven Sync-lid bij. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Synchronisatie van synchronisatie groep activeren |
> | Action | Micro soft. SQL/servers/data bases/syncGroups/schrijven | Hiermee maakt u een synchronisatie groep met de opgegeven para meters of werkt u de eigenschappen voor de opgegeven synchronisatie groep bij. |
> | Action | Microsoft.Sql/servers/databases/topQueries/queryText/action | Hiermee wordt de Transact-SQL-tekst geretourneerd voor de geselecteerde query-ID |
> | Action | Micro soft. SQL/servers/data bases/topQueries/lezen | Hiermee worden geaggregeerde runtime statistieken geretourneerd voor de geselecteerde query in de geselecteerde tijds periode |
> | Action | Micro soft. SQL/servers/data bases/topQueries/statistieken/lezen | Hiermee worden geaggregeerde runtime statistieken geretourneerd voor de geselecteerde query in de geselecteerde tijds periode |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Hiermee worden bewerkingen uitgevoerd op de transparante gegevens versleuteling |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/read | De status en Details van de functie voor transparante gegevens versleuteling ophalen voor een bepaalde data base |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Transparante gegevens versleutelings status wijzigen |
> | Action | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Een upgrade uitvoeren voor Azure SQL Data Warehouse database |
> | Action | Microsoft.Sql/servers/databases/usages/read | Hiermee worden de gegevens van het Azure SQL Database gebruik opgehaald |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | De evaluatie van beveiligings problemen voor een bepaalde data base verwijderen |
> | Action | Micro soft. SQL/servers/data bases/vulnerabilityAssessments/lezen | Het beleid voor evaluatie van beveiligings problemen ophalen voor een givendatabase |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | De regel basislijn voor evaluatie van beveiligings problemen verwijderen voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/vulnerabilityAssessments/regels/basis lijnen/lezen | De regel basislijn voor evaluatie van beveiligings problemen ophalen voor een bepaalde data base |
> | Action | Micro soft. SQL/servers/data bases/vulnerabilityAssessments/regels/basis lijnen/schrijven | De regel voor de evaluatie van beveiligings problemen wijzigen voor een bepaalde data base |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Een bestaand scan resultaat converteren naar een lees bare indeling. Als er al bestaat, gebeurt er niets |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Voer de database scan voor evaluatie van beveiligings problemen uit. |
> | Action | Micro soft. SQL/servers/data bases/vulnerabilityAssessments/scans/lezen | Retour neer de lijst met evaluatie records van het beveiligings probleem voor de data base of haal de scan record op voor de opgegeven scan-ID. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | De evaluatie van beveiligings problemen voor een bepaalde data base wijzigen |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Voer de database scan voor evaluatie van beveiligings problemen uit. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Ophalen van het resultaat van de data base-evaluatie van het beveiligings probleem beoordelings bewerking |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Details ophalen van de evaluatie van beveiligings problemen die is geconfigureerd voor een bepaalde data base |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | De evaluatie van beveiligings problemen voor een bepaalde data base wijzigen |
> | Action | Micro soft. SQL/servers/data bases/schrijven | Hiermee maakt u een Data Base met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven Data Base bij. |
> | Action | Micro soft. SQL/servers/verwijderen | Hiermee verwijdert u een bestaande server. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Hiermee verwijdert u een bestaande configuratie voor herstel na nood gevallen voor een bepaalde server |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Een DisasterRecoveryConfiguration failover |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Failover van een DisasterRecoveryConfiguration forceren |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Hiermee wordt een verzameling configuraties voor herstel na nood gevallen opgehaald die deze server bevatten |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Configuratie voor nood herstel van de server wijzigen |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/read | Hiermee wordt een lijst geretourneerd met schattingen van elastische groepen die al zijn gemaakt voor deze server |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/write | Hiermee maakt u een nieuwe schatting voor een elastische pool voor een lijst met data bases |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/read | Hiermee wordt een lijst met beschik bare Advisor voor de elastische pool geretourneerd |
> | Action | Micro soft. SQL/servers/elasticPools/Advisors/recommendedActions/lezen | Hiermee wordt een lijst met aanbevolen acties van opgegeven Advisor voor de elastische pool geretourneerd |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | De aanbevolen actie Toep assen op de elastische pool |
> | Action | Micro soft. SQL/servers/elasticPools/advisores/schrijven | Update status automatisch uitvoeren van een adviseur op het niveau van de elastische groep. |
> | Action | Microsoft.Sql/servers/elasticPools/databases/read | Hiermee wordt een lijst met data bases voor een elastische pool opgehaald |
> | Action | Microsoft.Sql/servers/elasticPools/delete | Bestaande elastische pool verwijderen |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Activiteiten en Details ophalen voor een bepaalde pool voor Elastic data base |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Activiteiten en Details ophalen voor een bepaalde data base die deel uitmaakt van een pool voor elastische data bases |
> | Action | Micro soft. SQL/servers/elasticPools/failover/actie | Door klant geïnitieerde failover van elastische pool. |
> | Action | Micro soft. SQL/servers/elasticPools/metricDefinitions/lezen | Typen metrische gegevens retour neren die beschikbaar zijn voor Pools voor elastische data bases |
> | Action | Micro soft. SQL/servers/elasticPools/metrieken/lezen | Metrische gegevens retour neren voor Pools voor elastische data bases |
> | Action | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Hiermee wordt een asynchrone bewerking die nog niet is voltooid, geannuleerd in Azure SQL elastische pool. |
> | Action | Microsoft.Sql/servers/elasticPools/operations/read | De lijst met uitgevoerde bewerkingen voor de elastische pool retour neren |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Typen metrische gegevens retour neren die beschikbaar zijn voor Pools voor elastische data bases |
> | Action | Microsoft.Sql/servers/elasticPools/read | Details van elastische pool op een bepaalde server ophalen |
> | Action | Microsoft.Sql/servers/elasticPools/skus/read | Hiermee wordt een verzameling sku's opgehaald die beschikbaar zijn voor een elastische pool |
> | Action | Micro soft. SQL/servers/elasticPools/schrijven | Een nieuwe of gewijzigde eigenschappen van een bestaande elastische pool maken |
> | Action | Microsoft.Sql/servers/encryptionProtector/read | Hiermee wordt een lijst met server versleutelings beveiligingen geretourneerd of worden de eigenschappen opgehaald voor de opgegeven server versleutelings beveiliging. |
> | Action | Micro soft. SQL/servers/encryptionProtector/schrijven | Werk de eigenschappen voor de opgegeven server versleutelings beveiliging bij. |
> | Action | Micro soft. SQL/servers/extendedAuditingSettings/lezen | Details ophalen van het uitgebreide-server-BLOB-controle beleid dat op een bepaalde server is geconfigureerd |
> | Action | Micro soft. SQL/servers/extendedAuditingSettings/schrijven | De controle van de uitgebreide server-BLOB voor een bepaalde server wijzigen |
> | Action | Micro soft. SQL/servers/failoverGroups/verwijderen | Hiermee verwijdert u een bestaande failovergroep. |
> | Action | Micro soft. SQL/servers/failoverGroups/failover/actie | Voert een geplande failover uit in een bestaande failovergroep. |
> | Action | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Hiermee wordt geforceerde failover uitgevoerd in een bestaande failovergroep. |
> | Action | Micro soft. SQL/servers/failoverGroups/lezen | Retourneert de lijst met failover-groepen of haalt de eigenschappen voor de opgegeven failovergroep op. |
> | Action | Micro soft. SQL/servers/failoverGroups/schrijven | Hiermee maakt u een failovergroep met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven failovergroep bij. |
> | Action | Micro soft. SQL/servers/firewallRules/verwijderen | Hiermee verwijdert u een bestaande server firewall regel. |
> | Action | Micro soft. SQL/servers/firewallRules/lezen | De lijst met Server firewall regels retour neren of de eigenschappen voor de opgegeven server firewall regel ophalen. |
> | Action | Micro soft. SQL/servers/firewallRules/schrijven | Hiermee maakt u een server firewall regel met de opgegeven para meters, werkt u de eigenschappen voor de opgegeven regel bij of overschrijft u alle bestaande regels met een nieuwe server firewall regel (s). |
> | Action | Micro soft. SQL/servers/importeren/actie | Een nieuwe Data Base op de server maken en schema's en gegevens uit een DacPac-pakket implementeren |
> | Action | Microsoft.Sql/servers/importExportOperationResults/read | Hiermee worden bewerkingen voor importeren/exporteren uitgevoerd |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Hiermee wordt het opgegeven interface-eindpunt profiel verwijderd |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Hiermee worden de eigenschappen van het opgegeven interface-eindpunt profiel geretourneerd |
> | Action | Micro soft. SQL/servers/interfaceEndpointProfiles/schrijven | Hiermee maakt u een interface-eindpunt profiel met de opgegeven para meters of werkt u de eigenschappen of labels voor het opgegeven interface-eind punt bij |
> | Action | Micro soft. SQL/servers/jobAgents/verwijderen | Hiermee verwijdert u een Azure SQL DB-taak agent |
> | Action | Micro soft. SQL/servers/jobAgents/lezen | Hiermee wordt een Azure SQL DB-taak agent opgehaald |
> | Action | Micro soft. SQL/servers/jobAgents/schrijven | Hiermee wordt een Azure SQL DB-taak agent gemaakt of bijgewerkt |
> | Action | Micro soft. SQL/servers/sleutels/verwijderen | Hiermee verwijdert u een bestaande server sleutel. |
> | Action | Micro soft. SQL/servers/sleutels/lezen | De lijst met Server sleutels retour neren of de eigenschappen voor de opgegeven server sleutel ophalen. |
> | Action | Micro soft. SQL/servers/sleutels/schrijven | Hiermee maakt u een sleutel met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server sleutel bij. |
> | Action | Micro soft. SQL/servers/operationResults/lezen | Hiermee worden Server bewerkingen uitgevoerd |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Hiermee verwijdert u een bestaande verbindings proxy voor een persoonlijk eind punt |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Hiermee wordt de lijst met particuliere endpoint-verbindings proxy's geretourneerd of worden de eigenschappen opgehaald voor de opgegeven verbinding proxy voor het particuliere eind punt. |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Hiermee wordt een aanroep voor het maken van een privé-eind punt gevalideerd aan de kant van de NRP |
> | Action | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | Hiermee maakt u een particuliere endpoint-verbindings proxy met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven verbinding proxy van het particuliere eind punt bij. |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/delete | Hiermee verwijdert u een bestaande persoonlijke eindpunt verbinding |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/read | Hiermee wordt de lijst met privé-eindpunt verbindingen geretourneerd of worden de eigenschappen opgehaald voor de opgegeven verbinding met een privé-eind punt. |
> | Action | Microsoft.Sql/servers/privateEndpointConnections/write | Hiermee wordt een bestaande verbinding met een privé-eind punt goedgekeurd of geweigerd |
> | Action | Micro soft. SQL/servers/privateEndpointConnectionsApproval/actie | Hiermee wordt bepaald of de gebruiker een verbinding met een privé-eind punt mag goed keuren |
> | Action | Microsoft.Sql/servers/privateLinkResources/read | De persoonlijke koppelings resources voor de bijbehorende SQL Server ophalen |
> | Action | Micro soft. SQL/servers/providers/micro soft. Insights/metricDefinitions/lezen | Typen metrische gegevens retour neren die beschikbaar zijn voor servers |
> | Action | Microsoft.Sql/servers/read | De lijst met servers retour neren of de eigenschappen voor de opgegeven server ophalen. |
> | Action | Micro soft. SQL/servers/recommendedElasticPools/data bases/lezen | Metrische gegevens ophalen voor Aanbevolen Pools voor elastische data bases voor een bepaalde server |
> | Action | Micro soft. SQL/servers/recommendedElasticPools/lezen | Haal aanbevelingen op voor Pools voor elastische data bases om de kosten te verlagen of de prestaties te verbeteren op basis van historisch resource gebruik |
> | Action | Microsoft.Sql/servers/recoverableDatabases/read | Deze bewerking wordt gebruikt voor nood herstel van live data base om de data base terug te zetten naar het laatst bekende goede back-uppunt. Het retourneert informatie over de laatste goede back-up, maar doesn\u0027t de data base daad werkelijk te herstellen. |
> | Action | Micro soft. SQL/servers/replicationLinks/lezen | De lijst met replicatie koppelingen retour neren of de eigenschappen ophalen voor de opgegeven replicatie koppelingen. |
> | Action | Microsoft.Sql/servers/restorableDroppedDatabases/read | Een lijst ophalen met data bases die zijn verwijderd op een bepaalde server en nog steeds binnen het Bewaar beleid vallen. |
> | Action | Micro soft. SQL/servers/securityAlertPolicies/operationResults/lezen | De resultaten van de Write-bewerking van het server bedreigings detectie beleid ophalen |
> | Action | Micro soft. SQL/servers/securityAlertPolicies/lezen | Een lijst met beleids regels voor Server bedreigings detectie ophalen die zijn geconfigureerd voor een bepaalde server |
> | Action | Micro soft. SQL/servers/securityAlertPolicies/schrijven | Het server Threat Detection-beleid voor een bepaalde server wijzigen |
> | Action | Micro soft. SQL/servers/serviceObjectives/lezen | Lijst met serviceniveau doelstellingen ophalen (ook wel prestatie lagen genoemd) die beschikbaar zijn op een bepaalde server |
> | Action | Micro soft. SQL/servers/syncAgents/verwijderen | Hiermee verwijdert u een bestaande synchronisatie agent. |
> | Action | Microsoft.Sql/servers/syncAgents/generateKey/action | Registratie sleutel synchronisatie agent genereren |
> | Action | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | De lijst met gekoppelde data bases van de gesynchroniseerde agent retour neren |
> | Action | Micro soft. SQL/servers/syncAgents/lezen | De lijst met synchronisatie agenten retour neren of de eigenschappen voor de opgegeven synchronisatie agent ophalen. |
> | Action | Micro soft. SQL/servers/syncAgents/schrijven | Hiermee maakt u een synchronisatie agent met de opgegeven para meters of werkt u de eigenschappen voor de opgegeven synchronisatie agent bij. |
> | Action | Micro soft. SQL/servers/tdeCertificates/actie | TDE-certificaat maken/bijwerken |
> | Action | Micro soft. SQL/servers/gebruik/lezen | DTU-quotum van de server en het huidige DTU-consuption retour neren door alle data bases op de server |
> | Action | Micro soft. SQL/servers/virtualNetworkRules/verwijderen | Hiermee verwijdert u een bestaande Virtual Network regel |
> | Action | Micro soft. SQL/servers/virtualNetworkRules/lezen | De lijst met regels voor het virtuele netwerk retour neren of de eigenschappen voor de opgegeven virtuele netwerk regel ophalen. |
> | Action | Micro soft. SQL/servers/virtualNetworkRules/schrijven | Hiermee maakt u een regel voor het virtuele netwerk met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven virtuele-netwerk regel bij. |
> | Action | Micro soft. SQL/servers/vulnerabilityAssessments/verwijderen | De evaluatie van beveiligings problemen voor een bepaalde server verwijderen |
> | Action | Micro soft. SQL/servers/vulnerabilityAssessments/lezen | Het beleid voor evaluatie van beveiligings problemen op een bepaalde server ophalen |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/write | De evaluatie van beveiligings problemen voor een bepaalde server wijzigen |
> | Action | Micro soft. SQL/servers/schrijven | Hiermee maakt u een server met de opgegeven para meters of werkt u de eigenschappen of labels voor de opgegeven server bij. |
> | Action | Micro soft. SQL/niet registreren/actie | Hiermee wordt de registratie van het abonnement voor de resource provider van micro soft SQL Database ongedaan gemaakt en wordt het maken van micro soft SQL-data bases ingeschakeld. |
> | Action | Micro soft. SQL/virtualClusters/verwijderen | Hiermee verwijdert u een bestaand virtueel cluster. |
> | Action | Micro soft. SQL/virtualClusters/lezen | De lijst met virtuele clusters retour neren of de eigenschappen van het opgegeven virtuele cluster ophalen. |
> | Action | Micro soft. SQL/virtualClusters/schrijven | Hiermee werkt u virtuele-cluster Tags bij. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. Storage/checknameavailability/lezen | Hiermee controleert u of een accountnaam geldig is en niet wordt gebruikt. |
> | Action | Micro soft. Storage/locaties/checknameavailability/lezen | Hiermee controleert u of een accountnaam geldig is en niet wordt gebruikt. |
> | Action | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.Storage doorgegeven dat het virtuele netwerk of subnet wordt verwijderd |
> | Action | Microsoft.Storage/locations/usages/read | Retourneert de limiet en het huidige gebruik voor resources in het opgegeven abonnement |
> | Action | Microsoft.Storage/operations/read | Hiermee wordt de status van een asynchrone bewerking gepeild. |
> | Action | Micro soft. Storage/REGI ster/actie | Hiermee wordt het abonnement voor de opslagresourceprovider geregistreerd en wordt het maken van opslagaccounts mogelijk. |
> | Action | Microsoft.Storage/skus/read | Hiermee wordt een lijst weergegeven van de SKU's die worden ondersteund door Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Hiermee wordt het resultaat van het toevoegen van de blob-inhoud geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Hiermee wordt het resultaat van het verwijderen van een blob geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteAutomaticSnapshot/action | Hiermee wordt het resultaat geretourneerd van het verwijderen van een automatische momentopname |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Hiermee wordt een op overeenkomende tags gefilterde lijst met blobs in een account geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Hiermee wordt een blob of een lijst met blobs geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/runAsSuperUser/action | Hiermee wordt het resultaat van de blobopdracht geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Hiermee wordt het resultaat van het lezen van blobtags geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Hiermee wordt het resultaat van het schrijven van blobtags geretourneerd. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Hiermee wordt het resultaat van het schrijven van een blob geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Juridische bewaring van blobcontainer wissen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Hiermee wordt het resultaat van het verwijderen van een container geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Beleid voor onveranderbaarheid van blobcontainer verwijderen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Beleid voor onveranderbaarheid van blobcontainer uitbreiden |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Beleid voor onveranderbaarheid van blobcontainer vergrendelen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Beleid voor onveranderbaarheid van blobcontainer ophalen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Beleid voor onveranderbaarheid van blobcontainer plaatsen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Hiermee wordt het resultaat van het in lease geven van de blobcontainer geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermee een container geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermee wordt een lijst met containers geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Juridische bewaring voor blobcontainer instellen |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Hiermee wordt het resultaat van het patchen van een blobcontainer geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Retourneert het resultaat van de put-BLOB-container |
> | Action | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Hiermee wordt een sleutel voor gebruikersdelegatie gegenereerd voor Blob service |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read |  |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read | Hiermee worden de eigenschappen van de Blob service of statistieken geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/blobServices/write | Hiermee wordt het resultaat van de eigenschappen voor het plaatsen van de blobcontainer geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/delete | Hiermee verwijdert u een bestaand opslagaccount. |
> | Action | Microsoft.Storage/storageAccounts/failover/action | De klant kan de failover beheren in het geval er problemen met de beschikbaarheid optreden |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Retourneert het resultaat van het verwijderen van een bestand/map |
> | DataAction | Micro soft. Storage/Storage accounts/fileServices/bestands shares/files/modifypermissions/Action | Hiermee wordt het resultaat van het wijzigen van de machtiging voor een bestand/map geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Retourneert een bestand/map of een lijst met bestanden/mappen. |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Retourneert het resultaat van het schrijven van een bestand of het maken van een map. |
> | Action | Microsoft.Storage/storageAccounts/fileServices/read |  |
> | Action | Microsoft.Storage/storageAccounts/fileServices/read | Eigenschappen voor bestandsservice ophalen |
> | Action | Micro soft. Storage/Storage accounts/fileServices/shares/verwijderen |  |
> | Action | Micro soft. Storage/Storage accounts/fileServices/shares/lezen |  |
> | Action | Micro soft. Storage/Storage accounts/fileServices/shares/lezen |  |
> | Action | Micro soft. Storage/Storage accounts/fileServices/shares/schrijven |  |
> | Action | Micro soft. Storage/Storage accounts/fileServices/schrijven |  |
> | Action | Microsoft.Storage/storageAccounts/listAccountSas/action | Hiermee wordt het SAS-token van het account geretourneerd voor het opgegeven opslagaccount. |
> | Action | Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Action | Microsoft.Storage/storageAccounts/listServiceSas/action | Hiermee wordt het SAS-token van de service geretourneerd voor het opgegeven opslagaccount. |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/delete | Beleid voor opslagaccountbeheer verwijderen |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/read | Beleid voor opslagaccountbeheer ophalen |
> | Action | Microsoft.Storage/storageAccounts/managementPolicies/write | Beleid voor opslagaccountbeheer plaatsen |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/delete | Particuliere endpoint-verbindings Proxy's verwijderen |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/write | Connect-Proxy's voor privé-eind punt plaatsen |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/delete | Verbinding voor privé-eind punt verwijderen |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/read | Verbinding voor privé-eind punt ophalen |
> | Action | Microsoft.Storage/storageAccounts/privateEndpointConnections/write | Verbinding voor privé-eind punt plaatsen |
> | Action | Micro soft. Storage/Storage accounts/PrivateEndpointConnectionsApproval/Action | Privé-eindpunt verbindingen goed keuren |
> | Action | Microsoft.Storage/storageAccounts/privateLinkResources/read | Storage account groupids ophalen |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Hiermee wordt het resultaat van een wachtrij verwijderen geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Hiermee wordt het resultaat van het toevoegen van een bericht geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Hiermee wordt het resultaat van een bericht verwijderen geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Hiermee wordt het resultaat van het verwerken van een bericht geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Hiermee wordt een bericht geretourneerd |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Hiermee wordt het resultaat van het schrijven van een bericht geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/read | Hiermee wordt een wachtrij of een lijst met wachtrijen geretourneerd. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/write | Hiermee wordt het resultaat van het schrijven van een wachtrij geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Eigenschappen voor Queue-service ophalen |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Hiermee worden de eigenschappen of statistieken van de Queue-service geretourneerd. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/write | Hiermee wordt het resultaat van het instellen van de eigenschappen van de Queue-service geretourneerd |
> | Action | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Action | Microsoft.Storage/storageAccounts/regeneratekey/action | Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw. |
> | Action | Microsoft.Storage/storageAccounts/restoreBlobRanges/action | BLOB-bereiken herstellen naar de status van de opgegeven tijd |
> | Action | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Hiermee worden alle sleutels voor gebruikersdelegatie voor het opgegeven opslagaccount ingetrokken. |
> | Action | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Diagnostische instellingen van opslagaccount maken/bijwerken. |
> | Action | Microsoft.Storage/storageAccounts/tableServices/read | Eigenschappen voor Table service ophalen |
> | Action | Microsoft.Storage/storageAccounts/write | Hiermee maakt u een opslagaccount met de opgegeven parameters, werkt u de eigenschappen of labels bij of voegt u een aangepast domein toe aan het opgegeven opslagaccount. |
> | Action | Microsoft.Storage/usages/read | Retourneert de limiet en het huidige gebruik voor resources in het opgegeven abonnement |

## <a name="microsoftstoragesync"></a>micro soft. storagesync

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | microsoft.storagesync/locations/checkNameAvailability/action | Controleert of de naam van de opslag synchronisatie service geldig is en niet wordt gebruikt. |
> | Action | micro soft. storagesync/locaties/werk stromen/bewerkingen/lezen | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Action | micro soft. storagesync/Operations/lezen | Hiermee wordt een lijst met ondersteunde bewerkingen opgehaald |
> | Action | micro soft. storagesync/REGI ster/actie | Hiermee wordt het abonnement voor de opslag synchronisatie provider geregistreerd |
> | Action | microsoft.storagesync/storageSyncServices/delete | Opslag synchronisatie Services verwijderen |
> | Action | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor opslag synchronisatie Services |
> | Action | microsoft.storagesync/storageSyncServices/read | Alle opslag synchronisatie services lezen |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/delete | Alle geregistreerde servers verwijderen |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor de geregistreerde server |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/read | Een geregistreerde server lezen |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/write | Een geregistreerde server maken of bijwerken |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Alle Cloud eindpunten verwijderen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Hiermee wordt de status van een asynchrone back-up-of herstel bewerking opgehaald |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Deze actie aanroepen na een back-up |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Deze actie aanroepen na het herstellen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Deze actie aanroepen voordat u een back-up maakt |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Deze actie aanroepen voordat deze wordt hersteld |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Alle Cloud eindpunten lezen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Heartbeat herstellen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Alle Cloud eindpunten maken of bijwerken |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/delete | Alle synchronisatie groepen verwijderen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor synchronisatie groepen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/read | Alle synchronisatie groepen lezen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Server eindpunten verwijderen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor Server-eind punten |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Server eindpunten lezen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Deze actie aanroepen om bestanden op een server op te halen |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Server eindpunten maken of bijwerken |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/write | Synchronisatie groepen maken of bijwerken |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Action | microsoft.storagesync/storageSyncServices/workflows/read | Werk stromen lezen |
> | Action | microsoft.storagesync/storageSyncServices/write | Opslag synchronisatie Services maken of bijwerken |
> | Action | micro soft. storagesync/registratie/actie | Hiermee wordt de registratie van het abonnement voor de opslag synchronisatie provider ongedaan gemaakt |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/delete | Hiermee verwijdert u de Access Control records |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/read | De Access Control records weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/accessControlRecords/schrijven | De Access Control records maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/waarschuwingen/lezen | De waarschuwingen weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/back-ups/lezen | De back-upset weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/bandwidthSettings/verwijderen | Hiermee verwijdert u een bestaande band breedte-instelling (alleen 8000 Series) |
> | Action | Micro soft. StorSimple/managers/bandwidthSettings/operationResults/lezen | De bewerkings resultaten weer geven |
> | Action | Micro soft. StorSimple/managers/bandwidthSettings/lezen | De band breedte-instellingen weer geven (alleen 8000 Series) |
> | Action | Micro soft. StorSimple/managers/bandwidthSettings/schrijven | Hiermee maakt u een nieuwe of bijgewerkte band breedte-instellingen (alleen 8000 Series) |
> | Action | Micro soft. StorSimple/managers/certificaten/schrijven | De certificaten maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/certificaten/schrijven | Met de bewerking resource certificaat bijwerken wordt het resource/kluis-referentie certificaat bijgewerkt. |
> | Action | Micro soft. StorSimple/managers/clearAlerts/actie | Wis alle waarschuwingen die zijn gekoppeld aan Apparaatbeheer. |
> | Action | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | De ondersteunde configuraties van het Cloud apparaat weer geven |
> | Action | Micro soft. StorSimple/managers/configureDevice/actie | Hiermee configureert u een apparaat |
> | Action | Micro soft. StorSimple/managers/verwijderen | Hiermee worden de Apparaatbeheer verwijderd |
> | Action | Micro soft. StorSimple/managers/verwijderen | Met de bewerking kluis verwijderen wordt de opgegeven Azure-resource van het type kluis verwijderd |
> | Action | Micro soft. StorSimple/managers/apparaten/alertSettings/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/alertSettings/lezen | De instellingen van de waarschuwing weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/alertSettings/schrijven | De instellingen voor waarschuwingen maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Machtigen voor overschakeling van service-versleutelings sleutel van apparaten |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Maak een hand matige back-up om een back-up op aanvraag te maken van alle volumes die worden beveiligd door het beleid. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Hiermee verwijdert u een bestaande back-upbeleid (alleen 8000 Series) |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/read | Het back-upbeleid (alleen 8000 Series) weer geven |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Hiermee worden bestaande Schema's verwijderd |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | De planningen weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/backupPolicies/planningen/schrijven | Hiermee maakt u een nieuwe of bijgewerkte planningen |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/write | Hiermee maakt u een nieuw of bijgewerkt back-upbeleid (alleen 8000 Series) |
> | Action | Micro soft. StorSimple/managers/apparaten/back-ups/verwijderen | Hiermee verwijdert u de back-upset |
> | Action | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Een share of volume klonen met behulp van een back-upelement. |
> | Action | Micro soft. StorSimple/managers/apparaten/back-ups/elementen/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/back-ups/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/back-ups/lezen | De back-upset weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/back-ups/herstellen/actie | Herstel alle volumes uit een back-upset. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Hiermee verwijdert u de back-upschema groepen |
> | Action | Micro soft. StorSimple/managers/apparaten/backupScheduleGroups/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/backupScheduleGroups/lezen | De back-upschema groepen worden weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/backupScheduleGroups/schrijven | De back-upschema groepen maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/delete | Hiermee worden de CHAP-instellingen verwijderd |
> | Action | Micro soft. StorSimple/managers/apparaten/chapSettings/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/chapSettings/lezen | De CHAP-instellingen weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/chapSettings/schrijven | De CHAP-instellingen maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/devices/deactivate/action | Hiermee wordt een apparaat deactiveren. |
> | Action | Microsoft.StorSimple/managers/devices/delete | Hiermee worden de apparaten verwijderd |
> | Action | Micro soft. StorSimple/managers/apparaten/schijven/lezen | De schijven weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/downloaden/actie | Down load updates voor een apparaat. |
> | Action | Micro soft. StorSimple/managers/apparaten/failover/actie | Failover van het apparaat. |
> | Action | Micro soft. StorSimple/managers/apparaten/failover/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/failoverTargets/lezen | De failover-doelen van de apparaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/back-up/actie | Maak een back-up van een bestands server. |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/verwijderen | Hiermee verwijdert u de bestands servers |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/metrieken/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/metricsDefinitions/lezen | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/lezen | De bestands servers weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/verwijderen | Hiermee worden de shares verwijderd |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/metrieken/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/metricsDefinitions/lezen | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/lezen | De shares weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/shares/schrijven | De shares maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/apparaten/fileservers/schrijven | Bestands servers maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | De energie status van de controller wijzigen van hardwarecomponenten |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | De hardware-onderdeel groepen weer geven |
> | Action | Microsoft.StorSimple/managers/devices/install/action | Installeer updates op een apparaat. |
> | Action | Microsoft.StorSimple/managers/devices/installUpdates/action | Installeert updates op de apparaten (alleen 8000 Series). |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/back-up/actie | Maak een back-up van een iSCSI-server. |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/verwijderen | De iSCSI-servers worden verwijderd |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | De schijven worden verwijderd |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/schijven/metrieken/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/schijven/lezen | De schijven weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/schijven/schrijven | De schijven maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/metrieken/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/metricsDefinitions/lezen | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/lezen | De iSCSI-servers weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/iscsiservers/schrijven | De iSCSI-servers maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Een actieve taak annuleren |
> | Action | Micro soft. StorSimple/managers/apparaten/Jobs/operationResults/lezen | De bewerkings resultaten weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/taken/lezen | De taken weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverSets/action | De failover-sets voor een bestaand apparaat weer geven (alleen 8000 Series). |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Failover-doelen van de apparaten weer geven (alleen 8000 Series). |
> | Action | Micro soft. StorSimple/managers/apparaten/metrieken/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/apparaten/metricsDefinitions/lezen | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Hiermee bevestigt u een geslaagde migratie en voert u deze door. |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/confirmMigrationStatus/lezen | De status van de migratie bevestigen weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/fetchConfirmMigrationStatus/actie | De Bevestig status van de migratie ophalen. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Haal de status van de taak schatting voor migratie op. |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/fetchMigrationStatus/actie | De status voor de migratie ophalen. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Bron configuraties voor migratie importeren |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/migrationEstimate/lezen | De schatting van de migratie weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/migrationStatus/lezen | De migratie status weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/migrationSourceConfigurations/operationResults/lezen | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Migratie starten met behulp van bron configuraties |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Start een taak om de duur van het migratie proces te schatten. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/read | Hiermee worden de netwerk instellingen weer gegeven of opgehaald |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/write | Hiermee maakt u een nieuw of bijgewerkt netwerk instellingen |
> | Action | Micro soft. StorSimple/managers/apparaten/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Open bare versleutelings sleutel van Apparaatbeheer weer geven |
> | Action | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Publiceer het ondersteunings pakket voor een bestaand apparaat. Een StorSimple-ondersteunings pakket is een eenvoudig te gebruiken mechanisme dat alle relevante logboeken verzamelt om Microsoft Ondersteuning te helpen bij het oplossen van problemen met StorSimple-apparaten. |
> | Action | Micro soft. StorSimple/managers/apparaten/lezen | De apparaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/scanForUpdates/actie | Zoeken naar updates op een apparaat. |
> | Action | Micro soft. StorSimple/managers/apparaten/securitySettings/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/securitySettings/lezen | De beveiligings instellingen weer geven |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Het certificaat voor extern beheer voor een apparaat synchroniseren. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/update/action | De beveiligings instellingen bijwerken. |
> | Action | Micro soft. StorSimple/managers/apparaten/securitySettings/schrijven | Hiermee maakt u een nieuwe of bijgewerkte beveiligings instellingen |
> | Action | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Verzend een e-mail met een test waarschuwing naar geconfigureerde e-mail ontvangers. |
> | Action | Micro soft. StorSimple/managers/apparaten/shares/lezen | De shares weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/timeSettings/lezen | De tijd instellingen weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/apparaten/timeSettings/schrijven | Hiermee maakt u een nieuwe of bijgewerkte tijd instellingen |
> | Action | Micro soft. StorSimple/managers/apparaten/updates/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/devices/updateSummary/read | De samen vatting van de update weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Hiermee verwijdert u een bestaande volume container (alleen 8000 Series) |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | De metrische gegevens weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | De definities voor metrische gegevens weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/read | De volume containers weer geven (alleen 8000 Series) |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Hiermee verwijdert u een bestaand volume |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | De metrische gegevens weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | De definities voor metrische gegevens weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | De bewerkings resultaten weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | De volumes weer geven |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Hiermee maakt u een nieuw of bijgewerkt volume |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/write | Hiermee maakt u een nieuwe of bijgewerkte volume containers (alleen 8000 Series) |
> | Action | Microsoft.StorSimple/managers/devices/volumes/read | De volumes weer geven |
> | Action | Micro soft. StorSimple/managers/apparaten/schrijven | De apparaten maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/encryptionSettings/lezen | De versleutelings instellingen weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/extendedInformation/verwijderen | Hiermee verwijdert u de uitgebreide kluis gegevens |
> | Action | Micro soft. StorSimple/managers/extendedInformation/verwijderen | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Micro soft. StorSimple/managers/extendedInformation/lezen | De uitgebreide kluis gegevens weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/extendedInformation/lezen | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Micro soft. StorSimple/managers/extendedInformation/schrijven | De uitgebreide kluis gegevens maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/extendedInformation/schrijven | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Action | Micro soft. StorSimple/managers/onderdelen/lezen | De functies weer geven |
> | Action | Micro soft. StorSimple/managers/fileservers/lezen | De bestands servers weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/getEncryptionKey/action | De versleutelings sleutel voor Apparaatbeheer ophalen. |
> | Action | Micro soft. StorSimple/managers/iscsiservers/lezen | De iSCSI-servers weer geven of ophalen |
> | Action | Micro soft. StorSimple/managers/Jobs/lezen | De taken weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/listActivationKey/action | Hiermee wordt de activerings sleutel van de StorSimple-Apparaatbeheer opgehaald. |
> | Action | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Open bare versleutelings sleutels van een StorSimple-Apparaatbeheer weer geven. |
> | Action | Micro soft. StorSimple/managers/metrische gegevens/lezen | Hiermee worden de metrische gegevens weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/metricsDefinitions/lezen | Hiermee worden de metrische definities weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/migrateClassicToResourceManager/actie | Klassiek migreren naar Resource Manager van managers |
> | Action | Micro soft. StorSimple/managers/migrationSourceConfigurations/lezen | De migratie bron configuraties weer geven (alleen 8000 Series) |
> | Action | Micro soft. StorSimple/managers/operationResults/lezen | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Maak een nieuw Cloud apparaat. |
> | Action | Micro soft. StorSimple/managers/lezen | Hiermee worden de Apparaatbeheer weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/lezen | Met de bewerking kluis ophalen wordt een object opgehaald dat de Azure-resource van het type ' kluis ' vertegenwoordigt. |
> | Action | Microsoft.StorSimple/managers/regenerateActivationKey/action | Genereer de activerings sleutel opnieuw voor een bestaande StorSimple-Apparaatbeheer. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Hiermee verwijdert u de referenties van het opslag account |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/read | De referenties van het opslag account worden weer gegeven of opgehaald |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/write | De referenties van het opslag account maken of bijwerken |
> | Action | Microsoft.StorSimple/managers/storageDomains/delete | Hiermee worden de opslag domeinen verwijderd |
> | Action | Microsoft.StorSimple/managers/storageDomains/operationResults/read | De bewerkings resultaten weer geven of ophalen |
> | Action | Microsoft.StorSimple/managers/storageDomains/read | De opslag domeinen worden weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/managers/storageDomains/schrijven | De opslag domeinen maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/schrijven | Apparaatbeheer maken of bijwerken |
> | Action | Micro soft. StorSimple/managers/schrijven | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
> | Action | Micro soft. StorSimple/Operations/lezen | Hiermee worden de bewerkingen weer gegeven of opgehaald |
> | Action | Micro soft. StorSimple/REGI ster/actie | Provider micro soft. StorSimple registreren |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.StreamAnalytics/locations/quotas/Read | Stream Analytics abonnements quotum lezen |
> | Action | Microsoft.StreamAnalytics/operations/Read | Stream Analytics bewerkingen lezen |
> | Action | Microsoft.StreamAnalytics/Register/action | Een abonnement bij Stream Analytics resource provider registreren |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Delete | Stream Analytics taak verwijderen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Stream Analytics taak functie verwijderen |
> | Action | Micro soft. StreamAnalytics/streamingjobs/functions/operationresults/lezen | Resultaten van de bewerking lezen voor Stream Analytics taak functie |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Stream Analytics taak functie lezen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | De standaard definitie van een Stream Analytics taak functie ophalen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Stream Analytics taak functie testen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Functie Write Stream Analytics-taak |
> | Action | Micro soft. StreamAnalytics/streamingjobs/invoer/verwijderen | Stream Analytics taak invoer verwijderen |
> | Action | Micro soft. StreamAnalytics/streamingjobs/invoer/operationresults/lezen | Resultaten van de bewerking lezen voor Stream Analytics taak invoer |
> | Action | Micro soft. StreamAnalytics/streamingjobs/invoer/lezen | Stream Analytics taak invoer lezen |
> | Action | Micro soft. StreamAnalytics/streamingjobs/invoer/voor beeld/actie | Voor beeld van Stream Analytics taak invoer |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Stream Analytics taak invoer testen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Invoer van Stream Analytics-taak schrijven |
> | Action | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Metrische definities lezen |
> | Action | Micro soft. StreamAnalytics/streamingjobs/operationresults/lezen | Resultaten van de bewerking lezen voor Stream Analytics taak |
> | Action | Micro soft. StreamAnalytics/streamingjobs/outputs/verwijderen | Stream Analytics taak uitvoer verwijderen |
> | Action | Micro soft. StreamAnalytics/streamingjobs/outputs/operationresults/lezen | Resultaten van de bewerking lezen voor Stream Analytics taak uitvoer |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Stream Analytics taak uitvoer lezen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Stream Analytics taak uitvoer testen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Stream Analytics taak uitvoer schrijven |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instelling lezen. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Diagnostische instelling schrijven. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschik bare logboeken voor streamingjobs opgehaald |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschik bare metrische gegevens opgehaald voor streamingjobs |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Read | Stream Analytics taak lezen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Start/action | Stream Analytics taak starten |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stream Analytics taak stoppen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Stream Analytics taak transformatie verwijderen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Stream Analytics taak transformatie lezen |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Trans formatie van Stream Analytics-taak schrijven |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Write | Stream Analytics-taak schrijven |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Micro soft. abonnement/annuleren/actie | Hiermee wordt het abonnement geannuleerd |
> | Action | Microsoft.Subscription/CreateSubscription/action | Een Azure-abonnement maken |
> | Action | Microsoft.Subscription/register/action | Hiermee wordt een abonnement geregistreerd bij de resource provider micro soft. Subscription |
> | Action | Micro soft. Subscription/naam wijzigen/actie | De naam van het abonnement wijzigen |
> | Action | Micro soft. Subscription/SubscriptionDefinitions/lezen | Een definitie van een Azure-abonnement in een beheer groep ophalen. |
> | Action | Microsoft.Subscription/SubscriptionDefinitions/write | Een definitie van een Azure-abonnement maken |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.Support/register/action | Hiermee wordt het item geregistreerd bij de ondersteuningsresourceprovider |
> | Action | Microsoft.Support/supportTickets/read | Hiermee worden gegevens over het ondersteuningsticket opgehaald (inclusief status, ernst, contactgegevens en communicatie) of wordt de lijst met ondersteuningstickets voor de verschillende abonnementen opgehaald. |
> | Action | Microsoft.Support/supportTickets/write | Hiermee wordt een ondersteuningsticket gemaakt of bijgewerkt. U kunt een ondersteuningsticket maken voor problemen van technische aard of op het gebied van facturering, quota's of beheer van abonnementen. U kunt voor bestaande ondersteuningstickets de ernst, de contactgegevens en de communicatiemethoden aanpassen. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Hiermee verwijdert u het toegangs beleid. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | De eigenschappen van een toegangs beleid ophalen. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Hiermee maakt u een nieuw toegangs beleid voor een omgeving of werkt u een bestaand toegangs beleid bij. |
> | Action | Microsoft.TimeSeriesInsights/environments/delete | Hiermee verwijdert u de omgeving. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Hiermee verwijdert u de bron van de gebeurtenis. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/read | De eigenschappen van een gebeurtenis bron ophalen. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/write | Hiermee maakt u een nieuwe gebeurtenis bron voor een omgeving of werkt u een bestaande gebeurtenis bron bij. |
> | Action | Microsoft.TimeSeriesInsights/environments/read | De eigenschappen van een omgeving ophalen. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Hiermee verwijdert u de set met referentie gegevens. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | De eigenschappen van een referentie gegevensset ophalen. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Hiermee maakt u een nieuwe referentie gegevensverzameling voor een omgeving of werkt u een bestaande referentie gegevensset bij. |
> | Action | Microsoft.TimeSeriesInsights/environments/status/read | Haal de status van de omgeving op, evenals de status van de bijbehorende bewerkingen als ingangs activiteiten. |
> | Action | Microsoft.TimeSeriesInsights/environments/write | Hiermee maakt u een nieuwe omgeving of werkt u een bestaande omgeving bij. |
> | Action | Microsoft.TimeSeriesInsights/register/action | Hiermee wordt het abonnement voor de Time Series Insights resource provider geregistreerd en wordt het maken van Time Series Insights-omgevingen mogelijk gemaakt. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.VisualStudio/Account/Delete | Account verwijderen |
> | Action | Microsoft.VisualStudio/Account/Extension/Read | Account/extensie lezen |
> | Action | Microsoft.VisualStudio/Account/Project/Read | Account/project lezen |
> | Action | Microsoft.VisualStudio/Account/Project/Write | Account/project instellen |
> | Action | Microsoft.VisualStudio/Account/Read | Account lezen |
> | Action | Microsoft.VisualStudio/Account/Write | Account instellen |
> | Action | Microsoft.VisualStudio/Extension/Delete | Extensie verwijderen |
> | Action | Microsoft.VisualStudio/Extension/Read | Uitbrei ding lezen |
> | Action | Microsoft.VisualStudio/Extension/Write | Extensie instellen |
> | Action | Microsoft.VisualStudio/Project/Delete | Project verwijderen |
> | Action | Microsoft.VisualStudio/Project/Read | Project lezen |
> | Action | Microsoft.VisualStudio/Project/Write | Project instellen |
> | Action | Microsoft.VisualStudio/Register/Action | Azure-abonnement registreren bij micro soft. Visual Studio-provider |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | microsoft.web/apimanagementaccounts/apiacls/read | Apiacls van API management-accounts ophalen. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Api's voor API management-accounts verwijderen Apiacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/read | Api's voor API management-accounts ophalen Apiacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/write | Api's voor API management-accounts bijwerken Apiacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Api's voor API management-accounts ophalen Connectionacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Bevestig de toestemming code API management accounts Api's Connections. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | API-beheer accounts verwijderen Api's-verbindingen Connectionacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Get API management accounts Api's Connections Connectionacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Update API management accounts Api's Connections Connectionacls. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/delete | Api's-verbindingen van API management-accounts verwijderen. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Vraag koppelingen naar toestemming voor API management-accounts Api's-verbindingen op. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Lijst met verbindings sleutels API-beheer accounts Api's-verbindingen. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Lijst met geheimen API management accounts Api's-verbindingen. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/read | Api's-verbindingen van API management-accounts ophalen. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/write | Api's-verbindingen van API management-accounts bijwerken. |
> | Action | microsoft.web/apimanagementaccounts/apis/delete | Api's voor API management-accounts verwijderen. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | API management-accounts verwijderen Api's gelokaliseerde definities. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | API management accounts Api's gelokaliseerde definities ophalen. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Update API management accounts Api's gelokaliseerde definities. |
> | Action | microsoft.web/apimanagementaccounts/apis/read | Api's voor API management-accounts ophalen. |
> | Action | microsoft.web/apimanagementaccounts/apis/write | Api's voor API management-accounts bijwerken. |
> | Action | microsoft.web/apimanagementaccounts/connectionacls/read | Connectionacls van API management-accounts ophalen. |
> | Action | microsoft.web/availablestacks/read | Beschik bare stacks ophalen. |
> | Action | Micro soft. web/certificaten/verwijderen | Een bestaand certificaat verwijderen. |
> | Action | Microsoft.Web/certificates/Read | De lijst met certificaten ophalen. |
> | Action | Microsoft.Web/certificates/Write | Een nieuw certificaat toevoegen of een bestaande bijwerken. |
> | Action | micro soft. Web/checknameavailability/lezen | Controleer of de resource naam beschikbaar is. |
> | Action | microsoft.web/classicmobileservices/read | Klassieke Mobile Services ophalen. |
> | Action | Microsoft.Web/connectionGateways/Delete | Hiermee verwijdert u een verbindings gateway. |
> | Action | Microsoft.Web/connectionGateways/Join/Action | Er wordt lid van een verbindings gateway. |
> | Action | Microsoft.Web/connectionGateways/ListStatus/Action | Hiermee wordt de status van een verbindings gateway weer gegeven. |
> | Action | Microsoft.Web/connectionGateways/Move/Action | Hiermee verplaatst u een verbindings gateway. |
> | Action | Microsoft.Web/connectionGateways/Read | De lijst met verbindings gateways ophalen. |
> | Action | Microsoft.Web/connectionGateways/Write | Hiermee wordt een verbindings gateway gemaakt of bijgewerkt. |
> | Action | microsoft.web/connections/confirmconsentcode/action | Bevestig de code voor de toestemming van de verbinding. |
> | Action | Microsoft.Web/connections/Delete | Hiermee verwijdert u een verbinding. |
> | Action | Microsoft.Web/connections/Join/Action | Er wordt een verbinding toegevoegd. |
> | Action | microsoft.web/connections/listconsentlinks/action | Lijst met toestemmings koppelingen voor verbindingen. |
> | Action | Microsoft.Web/connections/Move/Action | Hiermee verplaatst u een verbinding. |
> | Action | Microsoft.Web/connections/Read | De lijst met verbindingen ophalen. |
> | Action | Microsoft.Web/connections/Write | Hiermee wordt een verbinding gemaakt of bijgewerkt. |
> | Action | Microsoft.Web/customApis/Delete | Hiermee verwijdert u een aangepaste API. |
> | Action | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Hiermee wordt de API-definitie geëxtraheerd uit een WSDL. |
> | Action | Microsoft.Web/customApis/Join/Action | Er wordt verbinding gemaakt met een aangepaste API. |
> | Action | Microsoft.Web/customApis/listWsdlInterfaces/Action | Geeft een lijst van de WSDL-interfaces voor een aangepaste API. |
> | Action | Microsoft.Web/customApis/Move/Action | Hiermee verplaatst u een aangepaste API. |
> | Action | Microsoft.Web/customApis/Read | De lijst met aangepaste API ophalen. |
> | Action | Microsoft.Web/customApis/Write | Hiermee wordt een aangepaste API gemaakt of bijgewerkt. |
> | Action | Microsoft.Web/deletedSites/Read | De eigenschappen van een verwijderde web-app ophalen |
> | Action | microsoft.web/deploymentlocations/read | Implementatie locaties ophalen. |
> | Action | Microsoft.Web/geoRegions/Read | De lijst met geo-regio's ophalen. |
> | Action | micro soft. Web/hostingenvironments/capaciteiten/lezen | De capaciteit van hosting omgevingen ophalen. |
> | Action | Micro soft. Web/hostingEnvironments/verwijderen | Een App Service Environment verwijderen |
> | Action | microsoft.web/hostingenvironments/detectors/read | Detecties voor hosting omgevingen ophalen. |
> | Action | micro soft. Web/hostingenvironments/diagnostiek/lezen | Diagnostische gegevens over hosting omgevingen ophalen. |
> | Action | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | De netwerk eindpunten van alle inkomende afhankelijkheden ophalen. |
> | Action | Micro soft. Web/hostingEnvironments/samen voegen/actie | Voegt een App Service Environment |
> | Action | microsoft.web/hostingenvironments/metricdefinitions/read | Metrische definities voor hosting omgevingen ophalen. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Host-omgevingen met Multirole-groepen ophalen metrische definities. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metrics/read | De metrische gegevens van de Pools voor hosting omgevingen ophalen. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Read | De eigenschappen van een front-end groep ophalen in een App Service Environment |
> | Action | microsoft.web/hostingenvironments/multirolepools/skus/read | Haal de Sku's van de Multirole-Pools voor hosting omgevingen op. |
> | Action | microsoft.web/hostingenvironments/multirolepools/usages/read | Haal hosting omgevingen op met het gebruik van Multirole-Pools. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Een nieuwe front-endwebserver maken in een App Service Environment of een bestaande groep bijwerken |
> | Action | microsoft.web/hostingenvironments/operations/read | Bewerkingen voor hosting omgevingen ophalen. |
> | Action | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | De netwerk eindpunten van alle uitgaande afhankelijkheden ophalen. |
> | Action | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Privé-eindpunt verbindingen goed keuren |
> | Action | Micro soft. Web/hostingEnvironments/lezen | De eigenschappen van een App Service Environment ophalen |
> | Action | Microsoft.Web/hostingEnvironments/reboot/Action | Alle machines in een App Service Environment opnieuw opstarten |
> | Action | microsoft.web/hostingenvironments/resume/action | Hosting omgevingen hervatten. |
> | Action | micro soft. Web/hostingenvironments/server farms/lezen | Haal hosting omgevingen op App Service plannen. |
> | Action | microsoft.web/hostingenvironments/sites/read | Haal Web Apps hosting omgevingen op. |
> | Action | microsoft.web/hostingenvironments/suspend/action | Hosting omgevingen blok keren. |
> | Action | microsoft.web/hostingenvironments/usages/read | Het gebruik van hosting omgevingen ophalen. |
> | Action | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Workerpools metrische definities voor hosting omgevingen ophalen. |
> | Action | microsoft.web/hostingenvironments/workerpools/metrics/read | Haal Workerpools-metrische gegevens voor hosting omgevingen op. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Read | De eigenschappen van een werk groep in een App Service Environment ophalen |
> | Action | microsoft.web/hostingenvironments/workerpools/skus/read | Haal hosting omgevingen Workerpools Sku's op. |
> | Action | microsoft.web/hostingenvironments/workerpools/usages/read | Haal Workerpools-gebruik op in hosting omgevingen. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Write | Een nieuwe werk groep maken in een App Service Environment of een bestaande pool bijwerken |
> | Action | Micro soft. Web/hostingEnvironments/schrijven | Een nieuwe App Service Environment maken of bestaande bijwerken |
> | Action | micro soft. Web/ishostingenvironmentnameavailable/lezen | Ophalen als de hosting omgevings naam beschikbaar is. |
> | Action | micro soft. Web/ishostnameavailable/lezen | Controleer of de hostnaam beschikbaar is. |
> | Action | micro soft. Web/isusernameavailable/lezen | Controleer of de gebruikers naam beschikbaar is. |
> | Action | Microsoft.Web/listSitesAssignedToHostName/Read | Namen ophalen van sites die zijn toegewezen aan de hostnaam. |
> | Action | microsoft.web/locations/apioperations/read | Get locations API Operations. |
> | Action | microsoft.web/locations/connectiongatewayinstallations/read | Get locations Connection gateway Installations. |
> | Action | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Melding over het verwijderen van Vnet of subnet voor locaties. |
> | Action | microsoft.web/locations/extractapidefinitionfromwsdl/action | De API-definitie van WSDL voor locaties ophalen. |
> | Action | microsoft.web/locations/listwsdlinterfaces/action | Lijst van WSDL-interfaces voor locaties. |
> | Action | microsoft.web/locations/managedapis/apioperations/read | Ophalen van locaties Managed API-bewerkingen. |
> | Action | Micro soft. Web/locaties/beheerdeapi's/samen voegen/actie | Voegt een beheerde API samen. |
> | Action | microsoft.web/locations/managedapis/read | Beheerde Api's voor locaties ophalen. |
> | Action | micro soft. Web/locaties/operationResults/lezen | Get-bewerkingen. |
> | Action | micro soft. Web/locaties/bewerkingen/lezen | Get-bewerkingen. |
> | Action | microsoft.web/operations/read | Get-bewerkingen. |
> | Action | microsoft.web/publishingusers/read | Publiceer gebruikers. |
> | Action | microsoft.web/publishingusers/write | Publicatie gebruikers bijwerken. |
> | Action | Micro soft. Web/aanbevelingen/lezen | De lijst met aanbevelingen voor abonnementen ophalen. |
> | Action | microsoft.web/register/action | Registreer de resource provider van micro soft. web voor het abonnement. |
> | Action | microsoft.web/resourcehealthmetadata/read | Resource Health meta gegevens ophalen. |
> | Action | micro soft. web/server farms/capabilities/lezen | Profiteer van de mogelijkheden van App Service-abonnementen. |
> | Action | Microsoft.Web/serverfarms/Delete | Een bestaand App Service plan verwijderen |
> | Action | micro soft. web/server farms/firstpartyapps/instellingen/verwijderen | De instellingen van de app voor de eerste partij verwijderen App Service plannen. |
> | Action | micro soft. web/server farms/firstpartyapps/Settings/lezen | Ontvang App Service-abonnementen voor de eerste partij-apps. |
> | Action | micro soft. web/server farms/firstpartyapps/Settings/write | De instellingen van de app voor de eerste partij bijwerken App Service plannen. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | App Service-abonnementen van hybride verbindings ruimten voor het verwijderen worden door gegeven. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Get App Service-abonnementen hybride verbindings naam ruimten relays. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Get App Service-abonnementen hybride verbindings naam ruimten relays Web Apps. |
> | Action | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Ontvang de limieten voor hybride verbindings plannen voor App Service abonnementen. |
> | Action | microsoft.web/serverfarms/hybridconnectionrelays/read | Ontvang App Service abonnementen hybride verbindings relays. |
> | Action | microsoft.web/serverfarms/metricdefinitions/read | Definities van metrische gegevens voor App Service plannen ophalen. |
> | Action | microsoft.web/serverfarms/metrics/read | Statistieken over het App Service plan ophalen. |
> | Action | micro soft. web/server farms/operationresults/lezen | Resultaten van de bewerking App Service plannen ophalen. |
> | Action | Microsoft.Web/serverfarms/Read | De eigenschappen van een App Service plan ophalen |
> | Action | Microsoft.Web/serverfarms/restartSites/Action | Alle Web Apps opnieuw opstarten in een App Service plan |
> | Action | microsoft.web/serverfarms/sites/read | Ontvang App Service plannen Web Apps. |
> | Action | microsoft.web/serverfarms/skus/read | Down load de Sku's van App Service-abonnementen. |
> | Action | microsoft.web/serverfarms/usages/read | Maak gebruik van App Service plannen. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Werk App Service plannen Virtual Network verbindingen gateways. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/read | App Service plannen Virtual Network verbindingen ophalen. |
> | Action | micro soft. web/server farms/virtualnetworkconnections/routes/verwijderen | App Service plannen Virtual Network verbindings routes verwijderen. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | App Service plannen Virtual Network verbindings routes ophalen. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | App Service plannen Virtual Network verbindings routes bijwerken. |
> | Action | microsoft.web/serverfarms/workers/reboot/action | Start App Service plannen werk nemers. |
> | Action | Microsoft.Web/serverfarms/Write | Een nieuw App Service plan maken of een bestaand abonnement bijwerken |
> | Action | microsoft.web/sites/analyzecustomhostname/read | Aangepaste hostnaam analyseren. |
> | Action | Microsoft.Web/sites/applySlotConfig/Action | De sleuf configuratie van de web-app Toep assen vanaf de doel site tot de huidige web-app |
> | Action | Microsoft.Web/sites/backup/Action | Een nieuwe web-app-back-up maken |
> | Action | microsoft.web/sites/backup/read | Web Apps back-up ophalen. |
> | Action | microsoft.web/sites/backup/write | Web Apps back-up bijwerken. |
> | Action | microsoft.web/sites/backups/action | Hiermee wordt een bestaande app-back-up gedetecteerd die kan worden hersteld vanuit een BLOB in azure Storage. |
> | Action | microsoft.web/sites/backups/delete | Verwijder Web Apps back-ups. |
> | Action | microsoft.web/sites/backups/list/action | Lijst Web Apps back-ups. |
> | Action | Microsoft.Web/sites/backups/Read | De eigenschappen van de back-up van een web-app ophalen |
> | Action | microsoft.web/sites/backups/restore/action | Herstel Web Apps back-ups. |
> | Action | microsoft.web/sites/backups/write | Web Apps back-ups bijwerken. |
> | Action | microsoft.web/sites/config/delete | Web Apps configuratie verwijderen. |
> | Action | Microsoft.Web/sites/config/list/Action | De gevoelige beveiligings instellingen van de web-app weer geven, zoals publicatie referenties, app-instellingen en verbindings reeksen |
> | Action | Microsoft.Web/sites/config/Read | Configuratie-instellingen voor web-apps ophalen |
> | Action | microsoft.web/sites/config/snapshots/read | Web Apps-configuratie momentopnamen ophalen. |
> | Action | Microsoft.Web/sites/config/Write | De configuratie-instellingen van de web-app bijwerken |
> | Action | microsoft.web/sites/containerlogs/action | Container-logboeken ophalen voor web-app. |
> | Action | micro soft. web/sites/containerlogs/downloaden/actie | Down load Web Apps container Logboeken. |
> | Action | microsoft.web/sites/continuouswebjobs/delete | Verwijder Web Apps doorlopende webtaken. |
> | Action | microsoft.web/sites/continuouswebjobs/read | Ontvang Web Apps doorlopende webtaken. |
> | Action | microsoft.web/sites/continuouswebjobs/start/action | Start Web Apps doorlopende webtaken. |
> | Action | microsoft.web/sites/continuouswebjobs/stop/action | Stop Web Apps doorlopende webtaken. |
> | Action | Microsoft.Web/sites/Delete | Een bestaande web-app verwijderen |
> | Action | microsoft.web/sites/deployments/delete | Web Apps-implementaties verwijderen. |
> | Action | microsoft.web/sites/deployments/log/read | Web Apps-implementaties logboek ophalen. |
> | Action | microsoft.web/sites/deployments/read | Web Apps-implementaties ophalen. |
> | Action | microsoft.web/sites/deployments/write | Web Apps-implementaties bijwerken. |
> | Action | microsoft.web/sites/detectors/read | Web Apps detectors ophalen. |
> | Action | microsoft.web/sites/diagnostics/analyses/execute/Action | Voer Web Apps diagnostische gegevens over de analyse uit. |
> | Action | microsoft.web/sites/diagnostics/analyses/read | Down load de analyse van Web Apps diagnose. |
> | Action | microsoft.web/sites/diagnostics/aspnetcore/read | Web Apps diagnostische gegevens voor ASP.NET Core-app ophalen. |
> | Action | microsoft.web/sites/diagnostics/autoheal/read | Ontvang Web Apps diagnose automatisch herstellen. |
> | Action | micro soft. web/sites/diagnostische gegevens/implementatie/lezen | Down load de implementatie van Web Apps diagnose. |
> | Action | microsoft.web/sites/diagnostics/deployments/read | Web Apps diagnostische implementaties ophalen. |
> | Action | microsoft.web/sites/diagnostics/detectors/execute/Action | Voer Web Apps diagnostische gegevens detectie uit. |
> | Action | microsoft.web/sites/diagnostics/detectors/read | Down load de detectie van Web Apps diagnose. |
> | Action | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Ophalen van mislukte aanvragen voor Web Apps diagnostische gegevens per URI. |
> | Action | micro soft. web/sites/diagnostiek/frebanalysis/lezen | Web Apps diagnostische FREB-analyse ophalen. |
> | Action | microsoft.web/sites/diagnostics/loganalyzer/read | Lees de logboek analyse van Web Apps Diagnostics. |
> | Action | micro soft. web/sites/diagnostiek/lezen | Web Apps diagnostische categorieën ophalen. |
> | Action | microsoft.web/sites/diagnostics/runtimeavailability/read | Down load de beschik baarheid van Web Apps diagnostische gegevens. |
> | Action | microsoft.web/sites/diagnostics/servicehealth/read | Web Apps diagnostische Service Health ophalen. |
> | Action | micro soft. web/sites/diagnostiek/sitecpuanalysis/lezen | Web Apps diagnostische gegevens van de site CPU ophalen. |
> | Action | microsoft.web/sites/diagnostics/sitecrashes/read | Ontvang Web Apps diagnostische gegevens van de site. |
> | Action | microsoft.web/sites/diagnostics/sitelatency/read | Site latentie van Web Apps diagnostische gegevens ophalen. |
> | Action | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Down load de analyse van de site geheugen van Web Apps diagnose. |
> | Action | micro soft. web/sites/diagnostiek/siterestartsettingupdate/lezen | Web Apps diagnostische gegevens van de site ophalen update opnieuw instellen. |
> | Action | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Web Apps diagnostische gegevens van de site voor het opnieuw starten van de gebruiker ophalen. |
> | Action | micro soft. web/sites/diagnostiek/siteswap/lezen | Ontvang Web Apps diagnostische gegevens van site swap. |
> | Action | micro soft. web/sites/diagnostiek/threadcount/lezen | Web Apps diagnostische-thread aantal ophalen. |
> | Action | microsoft.web/sites/diagnostics/workeravailability/read | Web Apps diagnostische Workeravailability-gegevens ophalen. |
> | Action | micro soft. web/sites/diagnostiek/workerprocessrecycle/lezen | Web Apps diagnostische gegevens van werk processen ophalen. |
> | Action | microsoft.web/sites/domainownershipidentifiers/read | Web Apps eigendoms-Id's van het domein ophalen. |
> | Action | microsoft.web/sites/domainownershipidentifiers/write | De eigendoms-Id's van Web Apps domein bijwerken. |
> | Action | microsoft.web/sites/extensions/delete | Verwijder Web Apps site-extensies. |
> | Action | microsoft.web/sites/extensions/read | Web Apps site-extensies ophalen. |
> | Action | microsoft.web/sites/extensions/write | Web Apps site-extensies bijwerken. |
> | Action | microsoft.web/sites/functions/action | Functions Web Apps. |
> | Action | micro soft. web/sites/functies/verwijderen | Web Apps functies verwijderen. |
> | Action | microsoft.web/sites/functions/listsecrets/action | Geef geheimen Web Apps functies weer. |
> | Action | microsoft.web/sites/functions/masterkey/read | Web Apps functies hoofd sleutel ophalen. |
> | Action | micro soft. web/sites/functies/lezen | Web Apps-functies ophalen. |
> | Action | micro soft. web/sites/functies/-token/lezen | Web Apps functions-Token ophalen. |
> | Action | microsoft.web/sites/functions/write | Web Apps-functies bijwerken. |
> | Action | micro soft. web/sites/hostnamebindings/verwijderen | Web Apps hostnaam-bindingen verwijderen. |
> | Action | micro soft. web/sites/hostnamebindings/lezen | Web Apps hostnaam-bindingen ophalen. |
> | Action | micro soft. web/sites/hostnamebindings/schrijven | Web Apps hostnaam-bindingen bijwerken. |
> | Action | microsoft.web/sites/hostruntime/functions/keys/read | Web Apps sleutels voor Hostruntime-functies ophalen. |
> | Action | Microsoft.Web/sites/hostruntime/host/_master/read | De hoofd sleutel van functie-app voor beheer bewerkingen ophalen |
> | Action | Microsoft.Web/sites/hostruntime/host/action | Voer functie-app runtime-actie uit zoals synchronisatie triggers, functies toevoegen, functies aanroepen, functies verwijderen, enzovoort. |
> | Action | microsoft.web/sites/hostruntime/host/read | Web Apps Hostruntime-host ophalen. |
> | Action | microsoft.web/sites/hybridconnection/delete | Web Apps hybride verbinding verwijderen. |
> | Action | microsoft.web/sites/hybridconnection/read | Web Apps hybride verbinding ophalen. |
> | Action | microsoft.web/sites/hybridconnection/write | Web Apps hybride verbinding bijwerken. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Web Apps relays-naam ruimten voor hybride verbindingen verwijderen. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Lijst met sleutels Web Apps relays-naam ruimten voor hybride verbindingen. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Down loads van Web Apps Hybrid Connection-naam ruimten. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Web Apps relays-naam ruimten voor hybride verbindingen bijwerken. |
> | Action | microsoft.web/sites/hybridconnectionrelays/read | Ontvang Web Apps hybride verbindings relays. |
> | Action | micro soft. web/sites/exemplaren/implementaties/verwijderen | Implementaties van Web Apps instanties verwijderen. |
> | Action | microsoft.web/sites/instances/deployments/read | Implementaties van Web Apps-instanties ophalen. |
> | Action | microsoft.web/sites/instances/extensions/log/read | Het logboek voor extensies van Web Apps-exemplaren ophalen. |
> | Action | microsoft.web/sites/instances/extensions/processes/read | Web Apps-exemplaren van extensies-processen ophalen. |
> | Action | microsoft.web/sites/instances/extensions/read | Uitbrei dingen van Web Apps-instanties ophalen. |
> | Action | microsoft.web/sites/instances/processes/delete | Verwijder de processen van Web Apps exemplaren. |
> | Action | microsoft.web/sites/instances/processes/modules/read | Get Web Apps instances-modules. |
> | Action | microsoft.web/sites/instances/processes/read | Get Web Apps instances-processen. |
> | Action | microsoft.web/sites/instances/processes/threads/read | Web Apps-exemplaren ophalen threads verwerken. |
> | Action | microsoft.web/sites/instances/read | Web Apps-instanties ophalen. |
> | Action | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Status van trigger voor synchronisatie functie voor lijst Web Apps. |
> | Action | microsoft.web/sites/metricdefinitions/read | Web Apps metrische definities ophalen. |
> | Action | microsoft.web/sites/metrics/read | Web Apps metrische gegevens ophalen. |
> | Action | microsoft.web/sites/metricsdefinitions/read | Web Apps metrische definities ophalen. |
> | Action | microsoft.web/sites/migratemysql/action | MySql-Web Apps migreren. |
> | Action | microsoft.web/sites/migratemysql/read | Ga Web Apps om MySql te migreren. |
> | Action | microsoft.web/sites/networktrace/action | Web Apps netwerk tracering. |
> | Action | microsoft.web/sites/networktraces/operationresults/read | Resultaten van Web Apps netwerk tracerings bewerking ophalen. |
> | Action | microsoft.web/sites/newpassword/action | NieuwWachtwoord Web Apps. |
> | Action | microsoft.web/sites/operationresults/read | Resultaten van Web Apps-bewerking ophalen. |
> | Action | microsoft.web/sites/operations/read | Ontvang Web Apps bewerkingen. |
> | Action | microsoft.web/sites/perfcounters/read | Prestatie meter items voor Web Apps ophalen. |
> | Action | microsoft.web/sites/premieraddons/delete | Verwijder Web Apps Premier-invoeg toepassingen. |
> | Action | microsoft.web/sites/premieraddons/read | Ontvang Web Apps Premier-invoeg toepassingen. |
> | Action | microsoft.web/sites/premieraddons/write | Werk Web Apps Premier-invoeg toepassingen bij. |
> | Action | microsoft.web/sites/privateaccess/read | Ontvang gegevens over toegang tot de persoonlijke locatie en geautoriseerde virtuele netwerken die toegang hebben tot de site. |
> | Action | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Privé-eindpunt verbindingen goed keuren |
> | Action | microsoft.web/sites/processes/modules/read | Web Apps-proces modules ophalen. |
> | Action | microsoft.web/sites/processes/read | Web Apps-processen ophalen. |
> | Action | microsoft.web/sites/processes/threads/read | Ontvang Web Apps process-threads. |
> | Action | microsoft.web/sites/publiccertificates/delete | Web Apps open bare certificaten verwijderen. |
> | Action | microsoft.web/sites/publiccertificates/read | Ontvang Web Apps open bare certificaten. |
> | Action | microsoft.web/sites/publiccertificates/write | Web Apps open bare certificaten bijwerken. |
> | Action | Microsoft.Web/sites/publish/Action | Een web-app publiceren |
> | Action | Microsoft.Web/sites/publishxml/Action | XML voor publicatie profielen ophalen voor een web-app |
> | Action | microsoft.web/sites/publishxml/read | Web Apps Publishing XML ophalen. |
> | Action | Microsoft.Web/sites/Read | De eigenschappen van een web-app ophalen |
> | Action | microsoft.web/sites/recommendationhistory/read | Geschiedenis van Web Apps aanbeveling ophalen. |
> | Action | microsoft.web/sites/recommendations/disable/action | Schakel Web Apps aanbevelingen uit. |
> | Action | Micro soft. web/sites/aanbevelingen/lezen | De lijst met aanbevelingen voor web-app ophalen. |
> | Action | microsoft.web/sites/recover/action | Web Apps herstellen. |
> | Action | Microsoft.Web/sites/resetSlotConfig/Action | De configuratie van de web-app opnieuw instellen |
> | Action | microsoft.web/sites/resourcehealthmetadata/read | Meta gegevens van Web Apps Resource Health ophalen. |
> | Action | Microsoft.Web/sites/restart/Action | Een web-app opnieuw starten |
> | Action | microsoft.web/sites/restore/read | Web Apps herstel ophalen. |
> | Action | microsoft.web/sites/restore/write | Web Apps herstellen. |
> | Action | microsoft.web/sites/restorefrombackupblob/action | De web-app terugzetten vanuit een back-upblob. |
> | Action | micro soft. web/sites/restorefromdeletedapp/actie | Web Apps van verwijderde app herstellen. |
> | Action | microsoft.web/sites/restoresnapshot/action | Herstel Web Apps moment opnamen. |
> | Action | microsoft.web/sites/siteextensions/delete | Verwijder Web Apps site-extensies. |
> | Action | microsoft.web/sites/siteextensions/read | Web Apps site-extensies ophalen. |
> | Action | microsoft.web/sites/siteextensions/write | Web Apps site-extensies bijwerken. |
> | Action | microsoft.web/sites/slots/analyzecustomhostname/read | Web Apps sleuven ophalen aangepaste hostnaam analyseren. |
> | Action | Microsoft.Web/sites/slots/applySlotConfig/Action | De sleuf configuratie van de web-app Toep assen op de huidige sleuf van de doel sleuf. |
> | Action | Microsoft.Web/sites/slots/backup/Action | Nieuwe web app-sleuf back-up maken. |
> | Action | microsoft.web/sites/slots/backup/read | Back-ups van Web Apps sleuven ophalen. |
> | Action | microsoft.web/sites/slots/backup/write | Web Apps-sleuven voor back-ups bijwerken. |
> | Action | microsoft.web/sites/slots/backups/action | Detecteer Web Apps sleuven back-ups. |
> | Action | microsoft.web/sites/slots/backups/delete | Verwijder Web Apps-sleuven back-ups. |
> | Action | microsoft.web/sites/slots/backups/list/action | Lijst met Web Apps sleuven back-ups. |
> | Action | Microsoft.Web/sites/slots/backups/Read | De eigenschappen van de back-up van een web-app-sleuf ophalen |
> | Action | microsoft.web/sites/slots/backups/restore/action | Back-ups van Web Apps sleuven herstellen. |
> | Action | microsoft.web/sites/slots/config/delete | Web Apps sleuf configuratie verwijderen. |
> | Action | Microsoft.Web/sites/slots/config/list/Action | Lijst met gevoelige instellingen voor beveiliging van de web-app-sleuf, zoals publicatie referenties, app-instellingen en verbindings reeksen |
> | Action | Microsoft.Web/sites/slots/config/Read | Configuratie-instellingen van de web-app-sleuf ophalen |
> | Action | Microsoft.Web/sites/slots/config/Write | Configuratie-instellingen van de web-app-sleuf bijwerken |
> | Action | microsoft.web/sites/slots/containerlogs/action | Bewaar container logboeken voor de web-app-sleuf. |
> | Action | microsoft.web/sites/slots/containerlogs/download/action | Down load Web Apps-sleuf container Logboeken. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/delete | Verwijder Web Apps-sleuven doorlopende webtaken. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/read | Ontvang Web Apps sleuven doorlopende webtaken. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/start/action | Start Web Apps-sleuven doorlopende webtaken. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/stop/action | Stop Web Apps-sleuven van doorlopende webtaken. |
> | Action | Microsoft.Web/sites/slots/Delete | Een bestaande sleuf voor een web-app verwijderen |
> | Action | microsoft.web/sites/slots/deployments/delete | Web Apps sleuf-implementaties verwijderen. |
> | Action | microsoft.web/sites/slots/deployments/log/read | Haal het logboek voor de implementatie van Web Apps sleuven op. |
> | Action | microsoft.web/sites/slots/deployments/read | Haal implementaties van Web Apps sleuven op. |
> | Action | microsoft.web/sites/slots/deployments/write | Implementaties van Web Apps sleuven bijwerken. |
> | Action | microsoft.web/sites/slots/detectors/read | Ontvang Web Apps sleuf detecties. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Voer de diagnostische analyse van Web Apps sleuven uit. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/read | Haal analyse van de diagnostische gegevens van Web Apps-sleuven op. |
> | Action | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Down load de diagnostische gegevens van Web Apps sleuven voor ASP.NET Core app. |
> | Action | microsoft.web/sites/slots/diagnostics/autoheal/read | Automatisch herstellen van Web Apps-sleuven automatisch herstellen. |
> | Action | micro soft. web/sites/sleuven/diagnostische gegevens/implementatie/lezen | Down load de diagnostische implementatie van Web Apps sleuven. |
> | Action | micro soft. web/sites/sleuven/diagnostische gegevens/implementaties/lezen | Down loads van Web Apps sleuven diagnostische implementaties. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Voer de diagnostische detector van Web Apps sleuven uit. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/read | Down load de detectie van Web Apps sleuven Diagnostics. |
> | Action | micro soft. web/sites/sleuven/diagnostiek/frebanalysis/lezen | Ontvang Web Apps sleuven diagnostische FREB-analyse. |
> | Action | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Ontvang Web Apps sleuven Diagnostische logboeken Log Analyzer. |
> | Action | micro soft. web/sites/sleuven/diagnostische gegevens/lezen | Ontvang Web Apps sleuf diagnostiek. |
> | Action | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Down load de beschik baarheid van de diagnostische runtime van Web Apps-sleuven. |
> | Action | microsoft.web/sites/slots/diagnostics/servicehealth/read | Web Apps-sleuven diagnostische gegevens ophalen Service Health. |
> | Action | micro soft. web/sites/sleuven/diagnostiek/sitecpuanalysis/lezen | Ontvang Web Apps sleuven diagnostische gegevens van de site CPU. |
> | Action | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Ontvang Web Apps sleuven diagnostische gegevens van de site. |
> | Action | microsoft.web/sites/slots/diagnostics/sitelatency/read | Down load Web Apps-sleuven site latentie diagnose. |
> | Action | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Ontvang Web Apps sleuven diagnostische gegevens van het site geheugen. |
> | Action | micro soft. web/sites/sleuven/diagnostiek/siterestartsettingupdate/lezen | Ophalen van de instelling update voor de diagnostische gegevens van Web Apps sites |
> | Action | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Ophalen van Web Apps-sleuven site Diagnostics-gebruiker gestart. |
> | Action | micro soft. web/sites/sleuven/diagnostiek/siteswap/lezen | Down load Web Apps-sleuven site swap. |
> | Action | microsoft.web/sites/slots/diagnostics/threadcount/read | Web Apps-sleuven van het aantal diagnostische gegevens ophalen. |
> | Action | microsoft.web/sites/slots/diagnostics/workeravailability/read | Ontvang Web Apps sleuven diagnostische gegevens Workeravailability. |
> | Action | micro soft. web/sites/sleuven/diagnostiek/workerprocessrecycle/lezen | Web Apps-sleuven ophalen werk proces wordt herhaald. |
> | Action | microsoft.web/sites/slots/domainownershipidentifiers/read | Domein eigendoms-Id's van Web Apps-sleuven ophalen. |
> | Action | microsoft.web/sites/slots/functions/listsecrets/action | Lijst met geheimen Web Apps sleuven-functies. |
> | Action | micro soft. web/sites/sleuven/functies/lezen | Web Apps-sleuven-functies ophalen. |
> | Action | micro soft. web/sites/sleuven/hostnamebindings/verwijderen | Verwijder Web Apps sleuven hostnamen bindings. |
> | Action | microsoft.web/sites/slots/hostnamebindings/read | Haal Web Apps sleuven hostname-bindingen op. |
> | Action | micro soft. web/sites/sleuven/hostnamebindings/schrijven | Web Apps sleuven hostnamen bindingen bijwerken. |
> | Action | microsoft.web/sites/slots/hybridconnection/delete | Web Apps-sleuven hybride verbinding verwijderen. |
> | Action | microsoft.web/sites/slots/hybridconnection/read | Web Apps-sleuven hybride verbinding ophalen. |
> | Action | microsoft.web/sites/slots/hybridconnection/write | Web Apps-sleuven hybride verbinding bijwerken. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Web Apps-sleuven verwijderen sites voor naam ruimten hybride verbinding. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Web Apps-sleuven voor het bijwerken van de sites met hybride verbindings ruimten door geven. |
> | Action | microsoft.web/sites/slots/hybridconnectionrelays/read | Haal Web Apps-sleuven hybride verbindings relays door. |
> | Action | microsoft.web/sites/slots/instances/deployments/read | Implementaties van Web Apps sleuf-instanties ophalen. |
> | Action | microsoft.web/sites/slots/instances/processes/delete | Verwijder de processen van Web Apps-sleuf. |
> | Action | microsoft.web/sites/slots/instances/processes/read | Web Apps-exemplaren van sleuf-processen ophalen. |
> | Action | microsoft.web/sites/slots/instances/read | Web Apps sleuf-instanties ophalen. |
> | Action | microsoft.web/sites/slots/metricdefinitions/read | Metrische definities van Web Apps-sleuven ophalen. |
> | Action | microsoft.web/sites/slots/metrics/read | Gegevens over Web Apps sleuven ophalen. |
> | Action | microsoft.web/sites/slots/migratemysql/read | Web Apps sleuven ophalen MySql migreren. |
> | Action | microsoft.web/sites/slots/networktrace/action | Web Apps sleuven voor netwerk tracering. |
> | Action | microsoft.web/sites/slots/networktraces/operationresults/read | Resultaten van de netwerk tracerings bewerking van Web Apps sleuven ophalen. |
> | Action | microsoft.web/sites/slots/newpassword/action | NieuwWachtwoord Web Apps-sleuven. |
> | Action | microsoft.web/sites/slots/operationresults/read | Resultaten van de bewerking Web Apps sleuven ophalen. |
> | Action | microsoft.web/sites/slots/operations/read | Web Apps sleuf bewerkingen ophalen. |
> | Action | microsoft.web/sites/slots/perfcounters/read | Prestatie meter items voor Web Apps sleuven ophalen. |
> | Action | microsoft.web/sites/slots/phplogging/read | Web Apps Phplogging-sleuven ophalen. |
> | Action | microsoft.web/sites/slots/premieraddons/delete | Web Apps-sleuven verwijderen premier-invoeg toepassingen. |
> | Action | microsoft.web/sites/slots/premieraddons/read | Ontvang Web Apps-sleuven premier-invoeg toepassingen. |
> | Action | microsoft.web/sites/slots/premieraddons/write | Web Apps-sleuven voor premier-invoeg toepassingen bijwerken. |
> | Action | microsoft.web/sites/slots/processes/read | Web Apps sleuf processen ophalen. |
> | Action | microsoft.web/sites/slots/publiccertificates/delete | Open bare certificaten Web Apps-sleuven verwijderen. |
> | Action | microsoft.web/sites/slots/publiccertificates/read | Ontvang Web Apps-sleuven open bare certificaten. |
> | Action | microsoft.web/sites/slots/publiccertificates/write | Open bare certificaten van Web Apps sleuven maken of bijwerken. |
> | Action | Microsoft.Web/sites/slots/publish/Action | Een web app-sleuf publiceren |
> | Action | Microsoft.Web/sites/slots/publishxml/Action | Publicatie profiel-XML ophalen voor web app-sleuf |
> | Action | Microsoft.Web/sites/slots/Read | De eigenschappen van een web-app-implementatie sleuf ophalen |
> | Action | microsoft.web/sites/slots/recover/action | Web Apps sleuven herstellen. |
> | Action | Microsoft.Web/sites/slots/resetSlotConfig/Action | Configuratie van sleuf voor web-app opnieuw instellen |
> | Action | microsoft.web/sites/slots/resourcehealthmetadata/read | Web Apps-sleuven ophalen Resource Health meta gegevens. |
> | Action | Microsoft.Web/sites/slots/restart/Action | Een web-app-sleuf opnieuw opstarten |
> | Action | microsoft.web/sites/slots/restore/read | Herstel van Web Apps sleuven ophalen. |
> | Action | micro soft. web/sites/sleuven/herstellen/schrijven | Web Apps-sleuven herstellen. |
> | Action | microsoft.web/sites/slots/restorefrombackupblob/action | Web Apps sleuf herstellen vanaf een back-upblob. |
> | Action | micro soft. web/sites/sleuven/restorefromdeletedapp/actie | Web-app-sleuven van verwijderde app herstellen. |
> | Action | microsoft.web/sites/slots/restoresnapshot/action | Herstel Web Apps sleuf momentopnamen. |
> | Action | microsoft.web/sites/slots/siteextensions/delete | Site-extensies Web Apps sleuven verwijderen. |
> | Action | microsoft.web/sites/slots/siteextensions/read | Site-extensies van Web Apps-sleuven ophalen. |
> | Action | microsoft.web/sites/slots/siteextensions/write | Site-extensies Web Apps sleuven bijwerken. |
> | Action | Microsoft.Web/sites/slots/slotsdiffs/Action | Verschillen in de configuratie van de web-app en-sleuven |
> | Action | Microsoft.Web/sites/slots/slotsswap/Action | Implementatie sleuven voor web-apps wisselen |
> | Action | microsoft.web/sites/slots/snapshots/read | Moment opnamen van Web Apps sleuven ophalen. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Delete | Configuratie-instellingen voor het bron beheer van de web-app-sleuf verwijderen |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Read | Configuratie-instellingen voor het bron beheer van de web-app-sleuf ophalen |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Write | Configuratie-instellingen voor bron beheer van de web-app-sleuf bijwerken |
> | Action | Microsoft.Web/sites/slots/start/Action | Een web-app-sleuf starten |
> | Action | Microsoft.Web/sites/slots/stop/Action | Een web-app-sleuf stoppen |
> | Action | microsoft.web/sites/slots/sync/action | Web Apps-sleuven synchroniseren. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/delete | Web Apps-sleuven die webjobs hebben geactiveerd, verwijderen. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/read | Haal Web Apps-sleuven op die webjobs hebben geactiveerd. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/run/action | Voer Web Apps-sleuven uit die webjobs hebben geactiveerd. |
> | Action | microsoft.web/sites/slots/usages/read | Web Apps sleuven gebruiken. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/delete | Web Apps-sleuven verwijderen Virtual Network verbindingen. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Web Apps-sleuven bijwerken Virtual Network verbindingen gateways. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/read | Web Apps sleuven Virtual Network verbindingen ophalen. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/write | Web Apps-sleuven Virtual Network verbindingen bijwerken. |
> | Action | microsoft.web/sites/slots/webjobs/read | Web Apps-sleuven voor webjobs ophalen. |
> | Action | Micro soft. web/sites/sleuven/schrijven | Een nieuwe web-app-sleuf maken of een bestaande site bijwerken |
> | Action | Microsoft.Web/sites/slotsdiffs/Action | Verschillen in de configuratie van de web-app en-sleuven |
> | Action | Microsoft.Web/sites/slotsswap/Action | Implementatie sleuven voor web-apps wisselen |
> | Action | microsoft.web/sites/snapshots/read | Web Apps moment opnamen ophalen. |
> | Action | Microsoft.Web/sites/sourcecontrols/Delete | Configuratie-instellingen voor broncode beheer van web-app verwijderen |
> | Action | Microsoft.Web/sites/sourcecontrols/Read | Configuratie-instellingen voor broncode beheer van web-app ophalen |
> | Action | Microsoft.Web/sites/sourcecontrols/Write | Configuratie-instellingen voor broncode beheer van web-app bijwerken |
> | Action | Microsoft.Web/sites/start/Action | Een web-app starten |
> | Action | Microsoft.Web/sites/stop/Action | Een web-app stoppen |
> | Action | microsoft.web/sites/sync/action | Web Apps synchroniseren. |
> | Action | microsoft.web/sites/syncfunctiontriggers/action | Synchronisatie functie Triggers voor Web Apps. |
> | Action | microsoft.web/sites/triggeredwebjobs/delete | Web Apps geactiveerde webjobs verwijderen. |
> | Action | microsoft.web/sites/triggeredwebjobs/history/read | Web Apps geactiveerde webjobs-geschiedenis ophalen. |
> | Action | microsoft.web/sites/triggeredwebjobs/read | Web Apps geactiveerde webjobs ophalen. |
> | Action | microsoft.web/sites/triggeredwebjobs/run/action | Web Apps geactiveerde webjobs worden uitgevoerd. |
> | Action | microsoft.web/sites/usages/read | Ontvang Web Apps gebruik. |
> | Action | microsoft.web/sites/virtualnetworkconnections/delete | Web Apps Virtual Network verbindingen verwijderen. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/read | Web Apps gateways voor Virtual Network verbindingen ophalen. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/write | Web Apps Virtual Network gateways voor verbindingen bijwerken. |
> | Action | microsoft.web/sites/virtualnetworkconnections/read | Web Apps Virtual Network verbindingen ophalen. |
> | Action | microsoft.web/sites/virtualnetworkconnections/write | Web Apps Virtual Network verbindingen bijwerken. |
> | Action | microsoft.web/sites/webjobs/read | Web Apps webjobs ophalen. |
> | Action | Microsoft.Web/sites/Write | Een nieuwe web-app maken of een bestaande bijwerken |
> | Action | microsoft.web/skus/read | Down load Sku's. |
> | Action | microsoft.web/sourcecontrols/read | Bron besturings elementen ophalen. |
> | Action | microsoft.web/sourcecontrols/write | Bron besturings elementen bijwerken. |
> | Action | microsoft.web/unregister/action | De registratie van de resource provider micro soft. web voor het abonnement ongedaan maken. |
> | Action | microsoft.web/validate/action | Subelementid. |
> | Action | microsoft.web/verifyhostingenvironmentvnet/action | Controleer het Vnet van de hosting omgeving. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Action | Microsoft.WorkloadMonitor/components/read | Hiermee worden onderdelen voor de resource opgehaald |
> | Action | Micro soft. WorkloadMonitor/componentsSummary/lezen | Hiermee wordt een overzicht van onderdelen opgehaald |
> | Action | Microsoft.WorkloadMonitor/monitorInstances/read | Hiermee worden exemplaren van monitors voor de resource opgehaald |
> | Action | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Hiermee wordt een overzicht van monitor exemplaren opgehaald |
> | Action | Micro soft. WorkloadMonitor/monitors/lezen | Hiermee worden monitors opgehaald voor de resource |
> | Action | Micro soft. WorkloadMonitor/monitors/schrijven | Monitor configureren voor de resource |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/read | Hiermee worden meldings instellingen voor de resource opgehaald |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/write | Instellingen voor meldingen voor de resource configureren |
> | Action | Microsoft.WorkloadMonitor/operations/read | Hiermee worden de ondersteunde bewerkingen opgehaald |

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Ingebouwde rollen voor Azure-resources](built-in-roles.md)

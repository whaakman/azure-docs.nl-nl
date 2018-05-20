---
title: Azure Resource Manager-resource provider bewerkingen | Microsoft Docs
description: Hier worden de bewerkingen die beschikbaar zijn voor de Microsoft Azure Resource Manager-resourceproviders
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.openlocfilehash: 08059da95baa9e70b8dba2dd847d0b28669778b7
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager resource provider-bewerkingen

Dit artikel worden de bewerkingen die beschikbaar zijn voor elke resourceprovider van Azure Resource Manager. Deze bewerkingen kunnen worden gebruikt in [aangepaste rollen](custom-roles.md) om te bieden gedetailleerde [op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) tot bronnen in Azure. Bewerking tekenreeksen, hebben de volgende indeling: `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

De resource provider-bewerkingen zijn altijd in ontwikkeling. Als u de meest recente bewerkingen, gebruikt [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) of [az provider Bewerkingslijst](/cli/azure/provider/operation#az-provider-operation-list).

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.AAD/domainServices/delete | Hiermee verwijdert u Domain Services. |
> | Bewerking | Microsoft.AAD/domainServices/read | Leest Domain Services. |
> | Bewerking | Microsoft.AAD/domainServices/write | Schrijven van domeinservices |
> | Bewerking | Microsoft.AAD/locations/operationresults/read | Lees de status van een asynchrone bewerking. |
> | Bewerking | Microsoft.AAD/Operations/read | De gelokaliseerde beschrijving voor de bewerking, die moet worden weergegeven voor de gebruiker. |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.aadiam/diagnosticsettings/DELETE | Verwijderen van een diagnostische instelling |
> | Bewerking | Microsoft.aadiam/diagnosticsettings/Read | Lezen van een diagnostische instelling |
> | Bewerking | Microsoft.aadiam/diagnosticsettings/Write | Schrijven van een diagnostische instelling |
> | Bewerking | Microsoft.aadiam/diagnosticsettingscategories/Read | Lezen van een diagnostische instelling categorieën |
> | Bewerking | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | microsoft.aadiam/tenants/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor tenants |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Addons/operations/read | Opgehaald ondersteund RP-bewerkingen. |
> | Bewerking | Microsoft.Addons/register/action | Het opgegeven abonnement registreren bij Microsoft.Addons |
> | Bewerking | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Een lijst met huidige plan informatie over ondersteuning voor het opgegeven abonnement. |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Hiermee verwijdert u de opgegeven canonieke ondersteuningsplan |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/read | De status van de opgegeven canonieke ondersteuning abonnement ophalen. |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/write | Het opgegeven type canonieke ondersteuning wordt toegevoegd. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/action | Een nieuw forest maakt voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Hiermee haalt alle servers voor de opgegeven service-naam. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/alerts/read | Details voor waarschuwingen voor de forest zoals alertid waarschuwing verhoogd datum, laatste gedetecteerd, waarschuwing beschrijving van de waarschuwing, laatste bijgewerkte, waarschuwing niveau, waarschuwing staat, waarschuwing enzovoort koppelingen probleemoplossing opgehaald... |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/configuration/read | Haalt de configuratie van de Service voor het forest. Voorbeeld - naam van het Forest, Functionla niveau, master FSMO-functie domeinnamen, FSMO-functie van schemamaster enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/delete | Hiermee verwijdert u een Service en de servers samen met de Health-gegevens. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Hiermee haalt de details van de domeinen en sites voor het forest. Voorbeeld-gezondheidsstatus, actieve waarschuwingen, waarschuwingen, eigenschappen zoals het functionele domeinniveau, Forest, infrastructuur-Master, PDC, opgelost RID-master enzovoort.  |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Hiermee haalt u de voorkeur gebruikersinstelling voor het forest.<br>Voorbeeld - MetricCounterName zoals ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes ldapsearches.<br>Instellingen voor de UI-grafieken enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Hiermee haalt forest samenvatting voor het opgegeven forest als de naam van het forest, domeinen onder dit forest aantal, aantal sites en sites gegevens, enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Hiermee wordt de lijst met ondersteunde metrische gegevens voor een bepaalde service.<br>Voor Extranet accountvergrendelingen voorbeeld, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor AD FS-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomainService.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken exporteren naar Azure AD voor ADSync-service worden uitgevoerd. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Deze API is een service krijgt, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet accountvergrendelingen, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain Service.<br>Profiel latentie uitvoert, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken voor exporteren naar Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Deze API haalt de lijst met alle vrijgegeven ADDomainServices voor een premium-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/read | Servicedetails opgehaald voor de opgegeven service-naam. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Hiermee haalt replicatiedetails voor alle servers voor de opgegeven service-naam. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Hiermee haalt u het aantal domeincontrollers en hun replicatiefouten indien van toepassing. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Opgehaald voltooien domain controller lijst samen met replicatiedetails voor de opgegeven forest. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Een exemplaar van server toevoegen aan de service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Tijdens de serverregistratie van ADDomainService, wordt deze api aangeroepen voor de referenties voor de voorbereiding op nieuwe servers. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Hiermee verwijdert u een server voor een bepaalde service en de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/write | Maken of bijwerken van het exemplaar ADDomainService voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/action | Configuratie van de Tenant updates. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/read | Leest de Tenantconfiguratie van de. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/write | De Tenantconfiguratie van een maakt. |
> | Bewerking | Microsoft.ADHybridHealthService/logs/contents/read | Hiermee wordt de inhoud van agentinstallatie en registratielogboeken die zijn opgeslagen in blob. |
> | Bewerking | Microsoft.ADHybridHealthService/logs/read | Agent-installatie en registratielogboeken opgehaald voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/operations/read | Lijst met bewerkingen die worden ondersteund door het systeem ophalen. |
> | Bewerking | Microsoft.ADHybridHealthService/register/action | Registreert de Resourceprovider ADHybrid Health-Service en wordt het maken van de resource ADHybrid Health-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Lijst met beschikbare regio's, gebruikt door DevOps ter ondersteuning van de klant incidenten ophalen. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/badpassword/read | Hiermee wordt de lijst met een onjuist wachtwoord ingevoerd voor alle gebruikers in Active Directory Federation Service. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Opgehaald Blob SAS-URI die de status en uiteindelijke resultaat van de nieuwe rapporttaak in de wachtrij voor de frequentie van onjuiste gebruikersnaam en wachtwoord probeert per UserId per IP-adres per dag voor een bepaalde Tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Hiermee haalt u de lijst van DevOps ingestemd tenants. Doorgaans gebruikt voor de klantondersteuning. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/isdevops/read | Hiermee wordt een waarde die aangeeft wheather de teannt DevOps ingestemd of niet. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Userid(objectid)-updates voor de geselecteerde dev ops-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | De geselecteerde implementatie opgehaald voor de opgegeven tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Een tenant-id opgehaald gegeven de opslaglocatie voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Hiermee haalt u de geografische locatie van waaruit de gegevens worden geopend. |
> | Bewerking | Microsoft.ADHybridHealthService/services/action | Een service-exemplaar in de tenant wordt bijgewerkt. |
> | Bewerking | Microsoft.ADHybridHealthService/services/alerts/read | Leest de waarschuwingen voor een service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/alerts/read | Leest de waarschuwingen voor een service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Gegeven een functie naam controleert of als een service alles nodig om deze functie te gebruiken. |
> | Bewerking | Microsoft.ADHybridHealthService/services/delete | Hiermee verwijdert u een service-exemplaar in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/exporterrors/read | Hiermee haalt de exportfouten voor een bepaalde sync-service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/exportstatus/read | Hiermee wordt de status van de uitvoer voor een bepaalde service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Hiermee haalt waarschuwingen feedback voor een bepaalde service en de server. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metricmetadata/read | Hiermee wordt de lijst met ondersteunde metrische gegevens voor een bepaalde service.<br>Voor Extranet accountvergrendelingen voorbeeld, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor AD FS-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomainService.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken exporteren naar Azure AD voor ADSync-service worden uitgevoerd. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Deze API is gezien een service, haalt het gemiddelde voor de metrische gegevens voor een bepaalde service.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet accountvergrendelingen, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain Service.<br>Profiel latentie uitvoert, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken voor exporteren naar Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/read | Deze API is een service krijgt, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet accountvergrendelingen, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain Service.<br>Profiel latentie uitvoert, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken voor exporteren naar Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Deze API is gezien een service, haalt de samengevoegde weergave voor de metrische gegevens voor een bepaalde service.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet accountvergrendelingen, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain Service.<br>Profiel latentie uitvoert, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken voor exporteren naar Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Toevoegen of bijwerken van de configuratie van de bewaking voor een service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Hiermee haalt u het bewaken van configuraties voor een bepaalde service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Toevoegen of bewaken van configuraties voor een service-updates. |
> | Bewerking | Microsoft.ADHybridHealthService/services/premiumcheck/read | Deze API opgehaald uit de lijst met alle vrijgegeven services voor een premium-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/read | Leest de service-exemplaren in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/reports/details/read | Rapport opgehaald van de top 50 van gebruikers met een onjuist wachtwoord fouten in de afgelopen 7 dagen |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/action | Maakt een server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Leest de waarschuwingen voor een server. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Deze api wordt aangeroepen tijdens de serverregistratie, voor de referenties voor de voorbereiding op nieuwe servers. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Deze API opgehaald voor een bepaalde server een lijst met gegevenstypen die door de servers en de tijd van laatste voor het uploaden van elke's worden geladen. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/delete | Hiermee verwijdert u een server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Hiermee haalt u de details exporteren synchronisatiefout voor een bepaalde Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Deze API is een service krijgt, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt voor het ophalen van informatie met betrekking tot: Extranet accountvergrendelingen, totaal aantal mislukte aanvragen, openstaande aanvragen voor beveiligingstokens (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, geslaagde LDAP-bindingen per seconde, LDAP Bind tijd, actieve LDAP-Threads, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain Service.<br>Profiel latentie uitvoert, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken voor exporteren naar Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/read | Leest het server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Hiermee haalt u de configuratie van de service voor een bepaalde tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Whitelisting Functiestatus opgehaald voor een bepaalde tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/write | Maakt een service-exemplaar in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/unregister/action | Heft de registratie van het abonnement voor ADHybrid Health Service Resource Provider. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Advisor/configurations/read | Configuraties ophalen |
> | Bewerking | Microsoft.Advisor/configurations/write | Bijgewerkte configuratie |
> | Bewerking | Microsoft.Advisor/generateRecommendations/action | Geeft aanbevelingen |
> | Bewerking | Microsoft.Advisor/generateRecommendations/read | Haalt status aanbevelingen genereren |
> | Bewerking | Microsoft.Advisor/operations/read | De bewerkingen voor de Microsoft Advisor opgehaald |
> | Bewerking | Microsoft.Advisor/recommendations/read | Leest aanbevelingen |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/delete | Hiermee verwijdert u de onderdrukking |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/read | Suppressions opgehaald |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/write | Bijgewerkte suppressions |
> | Bewerking | Microsoft.Advisor/register/action | Registreert het abonnement voor Microsoft Advisor |
> | Bewerking | Microsoft.Advisor/suppressions/delete | Hiermee verwijdert u de onderdrukking |
> | Bewerking | Microsoft.Advisor/suppressions/read | Suppressions opgehaald |
> | Bewerking | Microsoft.Advisor/suppressions/write | Bijgewerkte suppressions |
> | Bewerking | Microsoft.Advisor/unregister/action | Heft de registratie van het abonnement voor Microsoft Advisor |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.AlertsManagement/alerts/changestate/action | Wijzig de status van de waarschuwing. |
> | Bewerking | Microsoft.AlertsManagement/alerts/read | Alle waarschuwingen voor de invoer-filters niet ophalen. |
> | Bewerking | Microsoft.AlertsManagement/alertsSummary/read | Ophalen van de samenvatting van waarschuwingen |
> | Bewerking | Microsoft.AlertsManagement/Operations/read | Leest de bewerkingen die zijn opgegeven |
> | Bewerking | Microsoft.AlertsManagement/smartGroups/changestate/action | De status van de smartcard groep wijzigen |
> | Bewerking | Microsoft.AlertsManagement/smartGroups/read | De smartcard groepen ophalen voor de invoer-filters |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Controleert dat de opgegeven Analysis Server naam geldig is en niet in gebruik. |
> | Bewerking | Microsoft.AnalysisServices/locations/operationresults/read | Haalt de informatie van het resultaat van de opgegeven bewerking. |
> | Bewerking | Microsoft.AnalysisServices/locations/operationstatuses/read | Haalt de informatie van de status van de opgegeven bewerking. |
> | Bewerking | Microsoft.AnalysisServices/operations/read | De informatie van bewerkingen opgehaald |
> | Bewerking | Microsoft.AnalysisServices/register/action | Resourceprovider van Analysis Services wordt geregistreerd. |
> | Bewerking | Microsoft.AnalysisServices/servers/delete | Hiermee verwijdert u de Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Lijst van de status van de gateway die is gekoppeld aan de server. |
> | Bewerking | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor Analysis-Server opgehaald |
> | Bewerking | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor Analysis-Server |
> | Bewerking | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor servers |
> | Bewerking | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor Analysis-Server |
> | Bewerking | Microsoft.AnalysisServices/servers/read | Haalt de informatie van de opgegeven Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/servers/resume/action | De Analysis-Server hervat. |
> | Bewerking | Microsoft.AnalysisServices/servers/skus/read | Beschikbare SKU-informatie voor de server ophalen |
> | Bewerking | Microsoft.AnalysisServices/servers/suspend/action | Onderbreekt de Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/servers/write | Maken of bijwerken van de opgegeven Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/skus/read | Haalt de informatie van SKU 's |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ApiManagement/checkNameAvailability/read | Controleert of de opgegeven servicenaam is beschikbaar |
> | Bewerking | Microsoft.ApiManagement/operations/read | Leesbewerkingen voor alle API beschikbaar voor Microsoft.ApiManagement resource |
> | Bewerking | Microsoft.ApiManagement/register/action | Abonnement voor Microsoft.ApiManagement resourceprovider registreren |
> | Bewerking | Microsoft.ApiManagement/reports/read | Rapporten kunnen samenvoegen met perioden, geografische regio, ontwikkelaars, producten, API's, bewerkingen, abonnement en byRequest ophalen. |
> | Bewerking | Microsoft.ApiManagement/service/apis/delete | Verwijder de bestaande API |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/delete | Bestaande diagnostische verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/loggers/delete | Toewijzing van een logboek met een diagnostische instelling verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/loggers/read | Lijst met bestaande diagnostische voorkomen |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/loggers/write | Berichtenlogboek toewijzen aan een diagnostische instelling |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/read | Lijst met diagnostische gegevens of Get-details van diagnose |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/write | Nieuwe diagnostische toevoegen of bijwerken van bestaande diagnostische gegevens |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Hiermee verwijdert u het bestaande bijlage |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/read | API Management opgehaald probleem of probleem bijlagen bijlage details ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/write | Api probleem bijlage toevoegen |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/delete | Hiermee verwijdert u het bestaande opmerking |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/read | Opgehaald Opmerkingdetails van het probleem opmerkingen of opgehaald API Management probleem |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/write | Api probleem opmerking toevoegen |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/delete | Hiermee verwijdert u het bestaande probleem |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/read | Ophalen van de problemen die zijn gekoppeld aan de details van het probleem API of opgehaald API Management |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/write | Api-probleem toevoegen of bijwerken van api-probleem |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/delete | Verwijder de bestaande API-bewerking |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/delete | Verwijder voor beleidsconfiguratie van beleid voor API-bewerking |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/read | Beleid voor API-bewerking of configuratiedetails Get-beleid voor API-bewerking ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/write | Configuratiedetails van beleid voor API-bewerking instellen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/delete | Voor beleidsconfiguratie van de bewerking verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/read | Configuratiedetails van beleid voor bewerking ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/write | Configuratiedetails van beleid voor opnieuw instellen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/read | Ophalen van lijst met bewerkingen voor bestaande API of details van API-bewerking ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/delete | Koppeling van de bestaande code met bestaande bewerking verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/read | Ophalen van de labels die zijn gekoppeld aan de details van de bewerking of Tag ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/write | Bestaande code te koppelen aan bestaande bewerking |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/write | Nieuwe API-bewerkingen voor het maken of bijwerken van bestaande API-bewerking |
> | Bewerking | Microsoft.ApiManagement/service/apis/operationsByTags/read | Lijst met koppelingen bewerking/label |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/delete | Configuratie van beleid uit de API-beleid verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/read | Beleidsregels voor configuratiedetails API of Get-beleid voor API niet ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/write | Configuratiedetails van beleid voor API instellen |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/delete | Verwijder voor beleidsconfiguratie van de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/read | Configuratiedetails van beleid voor API ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/write | Configuratiedetails van beleid voor API instellen |
> | Bewerking | Microsoft.ApiManagement/service/apis/products/read | Ophalen van alle producten die de API deel uitmaakt van |
> | Bewerking | Microsoft.ApiManagement/service/apis/read | Lijst met alle geregistreerde API's of Get details van de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/delete | Hiermee verwijdert u alle versies van de API of verwijder API-versie |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/read | Get-versies voor een API of Get-details van de API reelase |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/write | Nieuwe API-versie maken of bijwerken van bestaande API-versie |
> | Bewerking | Microsoft.ApiManagement/service/apis/revisions/delete | Hiermee verwijdert u alle versies van een API |
> | Bewerking | Microsoft.ApiManagement/service/apis/revisions/read | Ophalen van revisies die horen bij een API |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/delete | Hiermee verwijdert u het bestaande Schema |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/document/read | Het document met een beschrijving van het Schema ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/document/write | Het document met een beschrijving van het Schema bijwerken |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/read | De schema's opgehaald voor een bepaalde API of de schema's die worden gebruikt door de API opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/write | Hiermee stelt u de schema's die worden gebruikt door de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Beschrijving van de Tag verwijderen van de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Beschrijvingen van de labels in het bereik van de API of Tag ophalen beschrijving opvragen in het bereik van de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/write | De beschrijving van de Tag maken/wijzigen in het bereik van de API |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/delete | Verwijder bestaande API/Tag-koppeling |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/read | Alle API/Tag-koppeling voor de API of Get-details van de API/Tag koppeling ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/write | Nieuwe API/Tag koppeling toevoegen |
> | Bewerking | Microsoft.ApiManagement/service/apis/write | Nieuwe API maken of bijwerken van bestaande API-details |
> | Bewerking | Microsoft.ApiManagement/service/apisByTags/read | Lijst met API/Tag koppelingen |
> | Bewerking | Microsoft.ApiManagement/service/api-version-sets/delete | Verwijder de bestaande VersionSet |
> | Bewerking | Microsoft.ApiManagement/service/api-version-sets/read | Lijst met versie groepsentiteiten of details van een VersionSet opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/api-version-sets/versions/read | Lijst met versie entiteiten ophalen |
> | Bewerking | Microsoft.ApiManagement/service/api-version-sets/write | Nieuwe VersionSet maken of bijwerken van bestaande VersionSet details |
> | Bewerking | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | De Microsoft.ApiManagement resources voor uitvoering in het virtuele netwerk kiest bijgewerkte netwerkinstellingen-updates. |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/delete | Verwijderen van bestaande autorisatie-server |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/read | Ophalen van lijst met autorisatieservers of details van de autorisatie-server ophalen |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/write | Een nieuwe autorisatie-server of de details van de Update van een bestaande autorisatie-server maken |
> | Bewerking | Microsoft.ApiManagement/service/backends/delete | Verwijder de bestaande back-end |
> | Bewerking | Microsoft.ApiManagement/service/backends/read | Ophalen van lijst met back-ends of details van back-end ophalen |
> | Bewerking | Microsoft.ApiManagement/service/backends/reconnect/action | Een aanvraag voor opnieuw verbinden maken |
> | Bewerking | Microsoft.ApiManagement/service/backends/write | Een nieuwe back-end toevoegen of bijwerken van bestaande back-end-details |
> | Bewerking | Microsoft.ApiManagement/service/backup/action | Back-API Management-Service aan de opgegeven container in een gebruiker opgegeven storage-account |
> | Bewerking | Microsoft.ApiManagement/service/certificates/delete | Verwijder bestaande certificaat |
> | Bewerking | Microsoft.ApiManagement/service/certificates/read | Ophalen van lijst met certificaten of details van een certificaat ophalen |
> | Bewerking | Microsoft.ApiManagement/service/certificates/write | Nieuw certificaat toevoegen |
> | Bewerking | Microsoft.ApiManagement/service/delete | Verwijderen van exemplaar van API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/delete | Bestaande diagnostische verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/loggers/delete | Toewijzing van een logboek met een diagnostische instelling verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/loggers/read | Lijst met bestaande diagnostische voorkomen |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/loggers/write | Berichtenlogboek toewijzen aan een diagnostische instelling |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/read | Lijst met diagnostische gegevens of Get-details van diagnose |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/write | Nieuwe diagnostische toevoegen of bijwerken van bestaande diagnostische gegevens |
> | Bewerking | Microsoft.ApiManagement/service/getssotoken/action | Opgehaald SSO-token dat kan worden gebruikt om aan te melden bij API Management-Service Legacy-portal als beheerder |
> | Bewerking | Microsoft.ApiManagement/service/groups/delete | Verwijder bestaande groep |
> | Bewerking | Microsoft.ApiManagement/service/groups/read | Lijst met groepen of informatie opgehaald van een groep |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/delete | Bestaande gebruiker verwijderen uit bestaande groep |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/read | Lijst van de groepsgebruikers ophalen |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/write | Bestaande gebruiker toevoegen aan bestaande groep |
> | Bewerking | Microsoft.ApiManagement/service/groups/write | Nieuwe groep maken of bijwerken van bestaande groepsnaam details |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/delete | Bestaande id-Provider verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/read | Ophalen van lijst met identiteitsproviders of Get-details van de identiteitsprovider |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/write | Een nieuwe id-Provider of Update details van een bestaande id-Provider maken |
> | Bewerking | Microsoft.ApiManagement/service/locations/networkstatus/read | Hiermee haalt de netwerk-toegangsstatus van bronnen waarop de service is afhankelijk in de locatie. |
> | Bewerking | Microsoft.ApiManagement/service/loggers/delete | Bestaande Berichtenlogboek verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/loggers/read | Ophalen van lijst met voorkomen of details van berichtenlogboek ophalen |
> | Bewerking | Microsoft.ApiManagement/service/loggers/write | Nieuwe berichtenlogboek toevoegen of bestaande berichtenlogboek details bijwerken |
> | Bewerking | Microsoft.ApiManagement/service/managedeployments/action | SKU/eenheden toevoegen of verwijderen van regionale implementaties van API Management-Service wijzigen |
> | Bewerking | Microsoft.ApiManagement/service/networkstatus/read | Hiermee haalt u de netwerk-toegangsstatus van bronnen waarop de service is afhankelijk. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/action | Wordt een melding verzonden naar een opgegeven gebruiker |
> | Bewerking | Microsoft.ApiManagement/service/notifications/read | Alle meldingen van API Management-uitgever of ophalen van API Management publisher opgehaald details van melding |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Hiermee verwijdert u het die bestaande e-mailadres gekoppeld aan een melding |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/read | E-mailontvangers die zijn gekoppeld aan API Management Publisher melding ophalen |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Maken van nieuwe e-mailontvanger van de kennisgeving |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Hiermee verwijdert u de gebruiker die is gekoppeld aan de ontvangers van meldingen |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Ontvanger gebruikers die zijn gekoppeld aan de melding ophalen |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Gebruiker toevoegen aan de ontvangers van meldingen |
> | Bewerking | Microsoft.ApiManagement/service/notifications/write | Maken of Update API Management publisher meldingen |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/delete | Bestaande OpenID Connect Provider verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/read | Lijst met providers OpenID Connect of details van OpenID Connect-Provider opvragen |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/write | Een nieuwe OpenID Connect-Provider of Update details van een bestaande OpenID Connect-Provider maken |
> | Bewerking | Microsoft.ApiManagement/service/operationresults/read | Huidige status van een langdurige bewerking opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/policies/delete | Configuratie van beleid uit Tenant beleid verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/policies/read | Beleidsregels voor Tenant of Get policy-configuratiegegevens voor de Tenant niet ophalen |
> | Bewerking | Microsoft.ApiManagement/service/policies/write | Configuratiedetails van beleid instellen voor Tenant |
> | Bewerking | Microsoft.ApiManagement/service/policySnippets/read | Alle beleid codefragmenten ophalen |
> | Bewerking | Microsoft.ApiManagement/service/portalsettings/read | Aanmelden bij instellingen voor de Portal of Get-instellingen voor de Portal aanmelden of ophalen van delegatie voor de Portal-instellingen niet ophalen |
> | Bewerking | Microsoft.ApiManagement/service/portalsettings/write | Instellingen voor aanmelden of Update aanmelden instellingen of Update aanmelden instellingen of Update aanmelden instellingen of Update Overdrachtinstellingen of Update Overdrachtinstellingen bijwerken |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/delete | Bestaande API verwijderen uit bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/read | Lijst met API's toegevoegd aan bestaande product ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/write | Bestaande API toevoegen aan bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/delete | Verwijder bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/delete | Koppeling van bestaande developer-groep met bestaande product verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/read | Lijst met groepen van ontwikkelaars die zijn gekoppeld aan het product verkrijgen |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/write | Bestaande developer-groep koppelen aan bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/delete | Configuratie van beleid vanaf de Product-beleid verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/read | Beleidsregels voor het Product of Get policy-configuratiegegevens voor Product niet ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/write | Configuratiedetails van beleid instellen voor het Product |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/delete | Configuratie van beleid verwijderen uit bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/read | Voor beleidsconfiguratie van bestaande product ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/write | Stel de beleidsconfiguratie van voor bestaande product |
> | Bewerking | Microsoft.ApiManagement/service/products/read | Ophalen van lijst met producten of details van product ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/subscriptions/read | Lijst met abonnementen product ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/delete | Koppeling van de bestaande code met bestaande Product verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/read | Ophalen van de labels die zijn gekoppeld aan de details van de Product- of Tag ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/write | Bestaand label koppelen aan bestaande Product |
> | Bewerking | Microsoft.ApiManagement/service/products/write | Nieuw product maken of bijwerken van bestaande Productgegevens |
> | Bewerking | Microsoft.ApiManagement/service/properties/delete | Hiermee verwijdert u het bestaande eigenschap |
> | Bewerking | Microsoft.ApiManagement/service/properties/read | Lijst van alle eigenschappen opgehaald of details van de opgegeven eigenschap opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/properties/write | Een nieuwe eigenschap maken of bijwerken van waarde voor de opgegeven eigenschap |
> | Bewerking | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor API Management-service opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor API Management-service |
> | Bewerking | Microsoft.ApiManagement/service/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor API Management-service |
> | Bewerking | Microsoft.ApiManagement/service/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor API Management-service |
> | Bewerking | Microsoft.ApiManagement/service/quotas/periods/read | De itemwaarde quotum voor periode ophalen |
> | Bewerking | Microsoft.ApiManagement/service/quotas/periods/write | Huidige waarde van het prestatiemeteritem quotum instellen |
> | Bewerking | Microsoft.ApiManagement/service/quotas/read | Ophalen van waarden voor quotum |
> | Bewerking | Microsoft.ApiManagement/service/quotas/write | Huidige waarde van het prestatiemeteritem quotum instellen |
> | Bewerking | Microsoft.ApiManagement/service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/reports/read | Rapport samenvoegen met perioden of Get-rapport samenvoegen met geografische regio of Get-rapport geaggregeerd door ontwikkelaars ophalen.<br>of rapport samenvoegen met producten ophalen.<br>of rapport samenvoegen met API's of Get rapport samenvoegen met de bewerkingen of Get-rapport samengevoegd per abonnement ophalen.<br>of rapportagegegevens van aanvragen |
> | Bewerking | Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/delete | Het abonnement verwijderd. Deze bewerking kan worden gebruikt voor het abonnement verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/read | Een lijst met abonnementen product of details van het product abonnement ophalen |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Abonnement primaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Abonnement secundaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/write | Een bestaande gebruiker aan een bestaand product abonneren of bijwerken van bestaande abonnementsgegevens. Deze bewerking kan worden gebruikt voor het abonnement te verlengen |
> | Bewerking | Microsoft.ApiManagement/service/tagResources/read | Lijst van labels met gekoppelde Resources ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tags/delete | Bestaande code verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/tags/read | Lijst met Tags of details van label ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tags/write | Nieuw label toevoegen of bijwerken van bestaande label details |
> | Bewerking | Microsoft.ApiManagement/service/templates/delete | Standaard API Management e-mailsjabloon opnieuw instellen |
> | Bewerking | Microsoft.ApiManagement/service/templates/read | Alle e-mailsjablonen of opgehaald API Management-e-mailbericht Sjabloondetails opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/templates/write | Maken of bijwerken van API Management-e-mailsjabloon of API Management Updates-e-mailsjabloon |
> | Bewerking | Microsoft.ApiManagement/service/tenant/delete | Configuratie van beleid voor de tenant verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/deploy/action | Voert een taak voor implementatie om de wijzigingen van de vertakking opgegeven git toepassen op de configuratie in de database. |
> | Bewerking | Microsoft.ApiManagement/service/tenant/operationResults/read | Ophalen van lijst met Bewerkingsresultaten of resultaat van een specifieke bewerking ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/read | Configuratie van beleid voor de of Get-tenant toegang informatie details ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Primaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Secundaire toegangssleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/tenant/save/action | Hiermee maakt u doorvoeren met configuratie-momentopname voor de opgegeven vertakking in de opslagplaats |
> | Bewerking | Microsoft.ApiManagement/service/tenant/syncState/read | Status van laatste git-synchronisatie ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/validate/action | Wijzigingen van de vertakking opgegeven git valideert |
> | Bewerking | Microsoft.ApiManagement/service/tenant/write | Stel de beleidsconfiguratie van voor de tenant of Update tenant toegangsgegevens informatie |
> | Bewerking | Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/updatehostname/action | Setup, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/users/action | Een nieuwe gebruiker registreren |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/attachments/delete | Hiermee verwijdert u een bijlage |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/attachments/read | Toepassing bijlagen of opgehaald bijlage opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/attachments/write | Koppelen aan een toepassing toevoegen |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/delete | Hiermee verwijdert u de bestaande toepassing |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/read | Lijst van alle gebruikerstoepassingen of toepassingsgegevens opgehaald API Management |
> | Bewerking | Microsoft.ApiManagement/service/users/applications/write | Registreert een toepassing op toepassingsgegevens van API Management of Updates |
> | Bewerking | Microsoft.ApiManagement/service/users/delete | Gebruikersaccount verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/users/generateSsoUrl/action | URL SSO genereren. De URL kan worden gebruikt voor toegang tot de beheerportal |
> | Bewerking | Microsoft.ApiManagement/service/users/groups/read | Ophalen van lijst met gebruikersgroepen |
> | Bewerking | Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels |
> | Bewerking | Microsoft.ApiManagement/service/users/read | Een lijst met geregistreerde gebruikers of accountdetails van de van een gebruiker ophalen |
> | Bewerking | Microsoft.ApiManagement/service/users/subscriptions/read | Lijst met gebruikersabonnementen ophalen |
> | Bewerking | Microsoft.ApiManagement/service/users/token/action | Token toegangstoken ophalen voor een gebruiker |
> | Bewerking | Microsoft.ApiManagement/service/users/write | Een nieuwe gebruiker of de details van de Update-account van een bestaande gebruiker registreren |
> | Bewerking | Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
> | Bewerking | Microsoft.ApiManagement/unregister/action | Abonnement voor Microsoft.ApiManagement resourceprovider van de registratie ongedaan maken |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Authorization/checkAccess/action | Hiermee wordt gecontroleerd of de aanroeper is gemachtigd een bepaalde actie uit te voeren |
> | Bewerking | Microsoft.Authorization/classicAdministrators/delete | Hiermee wordt de beheerder verwijderd uit het abonnement. |
> | Bewerking | Microsoft.Authorization/classicAdministrators/read | Hiermee worden de beheerders voor het abonnement ingelezen. |
> | Bewerking | Microsoft.Authorization/classicAdministrators/write | Hiermee wordt de beheerder gewijzigd of aan een abonnement toegevoegd. |
> | Bewerking | Microsoft.Authorization/elevateAccess/action | Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik |
> | Bewerking | Microsoft.Authorization/locks/delete | De vergrendelingen bij het opgegeven bereik worden verwijderd. |
> | Bewerking | Microsoft.Authorization/locks/read | De vergrendelingen bij het opgegeven bereik worden opgehaald. |
> | Bewerking | Microsoft.Authorization/locks/write | Hiermee worden vergrendelingen toegevoegd bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/permissions/read | Hiermee worden alle machtigingen van de beller voor een bepaald bereik weergegeven in een lijst. |
> | Bewerking | Microsoft.Authorization/policyAssignments/delete | Een beleidstoewijzing verwijderen bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/policyAssignments/read | Informatie ophalen over een beleidstoewijzing. |
> | Bewerking | Microsoft.Authorization/policyAssignments/write | Een beleidstoewijzing maken bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/delete | Een beleidsdefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/read | Informatie ophalen over een beleidsdefinitie. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/write | Een aangepaste beleidsdefinitie maken. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/delete | Een beleidssetdefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/read | Informatie ophalen over een beleidssetdefinitie. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/write | Een aangepaste beleidssetdefinitie maken. |
> | Bewerking | Microsoft.Authorization/providerOperations/read | Bewerkingen ophalen voor alle resourceproviders die kunnen worden gebruikt in roldefinities. |
> | Bewerking | Microsoft.Authorization/roleAssignments/delete | Een roltoewijzing bij het opgegeven bereik verwijderen. |
> | Bewerking | Microsoft.Authorization/roleAssignments/read | Informatie ophalen over een roltoewijzing. |
> | Bewerking | Microsoft.Authorization/roleAssignments/write | Een roltoewijzing bij het opgegeven bereik maken. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/delete | De opgegeven aangepaste roldefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/read | Informatie ophalen over een roldefinitie. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/write | Een aangepaste roldefinitie maken of bijwerken met opgegeven machtigingen of toewijsbare bereiken. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Registratie-informatie voor een Azure Automation DSC lezen |
> | Bewerking | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Schrijft een aanvraag voor het genereren van sleutels voor Azure Automation DSC |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/delete | Een Azure Automation-certificaatactivum verwijderen |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/getCount/action | Leest de telling van certificaten |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/read | Een Azure Automation-certificaatasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/write | Maken of bijwerken van een Azure Automation-certificaatactivum |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/read | Een Azure Automation DSC van compilatie leest |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/read | Een Azure Automation DSC van compilatie leest |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/write | Een Azure Automation DSC van compilatie schrijft |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/write | Een Azure Automation DSC van compilatie schrijft |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/content/read | Leest de configuratie van media-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/delete | Hiermee verwijdert u een Azure Automation DSC-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/getCount/action | Leest de telling van de inhoud van een Azure Automation DSC |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/read | Hiermee wordt de inhoud van een Azure Automation DSC opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/write | Schrijft een Azure Automation DSC-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/delete | Hiermee wordt een Azure Automation verbindingstype-asset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/getCount/action | Leest het aantal verbindingen |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/read | Een Azure Automation-verbindingsasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/write | Maken of bijwerken van een Azure Automation-verbindingsasset |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/delete | Hiermee wordt een Azure Automation verbindingstype-asset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/read | Een Azure Automation verbindingstype-asset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/write | Hiermee maakt u een Azure Automation verbindingstype-asset |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/delete | Hiermee wordt een Azure Automation-referentieasset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/getCount/action | Leest de telling van referenties |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/read | Een Azure Automation-referentieasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/write | Maken of bijwerken van een Azure Automation-referentieasset |
> | Bewerking | Microsoft.Automation/automationAccounts/delete | Hiermee verwijdert u een Azure Automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/diagnosticSettings/write | Hiermee stelt u de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Resources voor de Hybrid Runbook Worker worden verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak hervatten |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | De inhoud van het Azure Automation-runbook opgehaald op het moment van de taak wordt uitgevoerd |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/stop/action | Hiermee wordt een Azure Automation-taak gestopt |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/suspend/action | Een Azure Automation-taak wordt onderbroken |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/delete | Hiermee wordt een Azure Automation-taakschema verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/read | Een Azure Automation-taakschema opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation-taakschema |
> | Bewerking | Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/listKeys/action | Leest de sleutels voor het automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/logDefinitions/read | De beschikbare logboeken opgehaald voor het automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/activities/read | Azure Automatiseringsactiviteiten opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/delete | Hiermee verwijdert u een Azure Automation-module |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/getCount/action | Het aantal modules binnen het Automation-Account opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/read | Een Azure Automation-module opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/write | Maken of bijwerken van een Azure Automation-module |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Hiermee verwijdert u een Azure Automation DSC-knooppuntconfiguratie |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Leest configuratie-inhoud voor een Azure Automation DSC-knooppunt |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Hiermee wordt een Azure Automation DSC-knooppuntconfiguratie gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Een Azure Automation DSC-knooppuntconfiguratie schrijft |
> | Bewerking | Microsoft.Automation/automationAccounts/nodecounts/read | Aantal knooppunten samenvatting gelezen voor het opgegeven type |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/delete | Hiermee verwijdert u Azure Automation DSC-knooppunten |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/read | Azure Automation DSC-knooppunten leest |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Azure Automation DSC rapportinhoud lezen |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/reports/read | Leest Azure Automation DSC-rapporten |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/write | Maken of bijwerken van Azure Automation DSC-knooppunten |
> | Bewerking | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Azure Automation TypeFields opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read | Automation metrische definities opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/read | Hiermee ontvangt u een Azure Automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/content/read | De inhoud van een Azure Automation-runbook opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/delete | Hiermee verwijdert u een Azure Automation-runbook |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | De inhoud van een Azure Automation-runbookconcept gemaakt |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Azure Automation-runbook-concept Bewerkingsresultaten opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/publish/action | Een Azure Automation-runbookconcept gepubliceerd |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/read | Een Azure Automation-runbookconcept opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Een Azure Automation-runbookconcept opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Een Azure Automation-runbookconcept hervatten |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Hiermee wordt een Azure Automation-runbookconcept gestopt |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Wordt een Azure Automation-runbookconcept onderbroken |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Hiermee maakt u een Azure Automation-runbookconcept |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Bewerkingen aan een Azure Automation-runbookconcept ongedaan maken |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/getCount/action | De telling van Azure Automation-runbooks opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/write | Maken of bijwerken van een Azure Automation-runbook |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/delete | Hiermee wordt een Azure Automation-planningsasset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/getCount/action | De telling van Azure Automation-planningen opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/read | Een Azure Automation-planningsasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/write | Maken of bijwerken van een Azure Automation-planningsasset |
> | Bewerking | Microsoft.Automation/automationAccounts/statistics/read | Azure Automation-statistieken opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Een Azure Automation-update-implementatie machine ophalen |
> | Bewerking | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Een Azure Automation update management patch-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/usages/read | Gebruik Azure Automation opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/delete | Hiermee wordt een variabel Azure Automation-asset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/read | Hiermee wordt een variabel Azure Automation-asset gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/write | Maken of bijwerken van een Azure Automation-variabelenactivum |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/delete | Een Azure Automation-taak watcher verwijderen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/read | Een Azure Automation-watcher-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/start/action | Start een Azure Automation-taak watcher |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/stop/action | Een Azure Automation-watcher-taak stoppen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/streams/read | Een Azure Automation-taakstroom voor watcher opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Een Azure Automation watcher Taakacties verwijderen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Een Azure Automation-watcher jobacties opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Een Azure Automation-watcher jobacties maken |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/write | Hiermee maakt u een Azure Automation-taak watcher |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/action | Een URI voor een Azure Automation-webhook gegenereerd |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/delete | Hiermee wordt een Azure Automation-webhook verwijderd  |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/read | Hiermee wordt een Azure Automation-webhook gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/write | Maken of bijwerken van een Azure Automation-webhook |
> | Bewerking | Microsoft.Automation/automationAccounts/write | Maken of bijwerken van een Azure Automation-account |
> | Bewerking | Microsoft.Automation/operations/read | Beschikbare bewerkingen voor Azure Automation-resources opgehaald |
> | Bewerking | Microsoft.Automation/register/action | Het abonnement op Azure Automation registreren |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/delete | B2C-mapresource verwijderen |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/read | B2C-mapresource weergeven |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/write | B2C-mapresource maken of bijwerken |
> | Bewerking | Microsoft.AzureActiveDirectory/operations/read | Alle API-bewerkingen lezen die beschikbaar zijn voor de resourceprovider Microsoft.AzureActiveDirectory |
> | Bewerking | Microsoft.AzureActiveDirectory/register/action | Abonnement voor de resourceprovider Microsoft.AzureActiveDirectory registreren |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.AzureStack/Operations/read | De eigenschappen van een resource provider-bewerking opgehaald |
> | Bewerking | Microsoft.AzureStack/register/action | Abonnement registreert bij de resourceprovider Microsoft.AzureStack |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Hiermee verwijdert u een abonnement van de klant Azure Stack |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/read | De eigenschappen van een Azure-abonnement Stack klant opgehaald |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/write | Maken of bijwerken van een Azure-Stack klant-abonnement |
> | Bewerking | Microsoft.AzureStack/registrations/delete | Hiermee verwijdert u een Azure-Stack-registratie |
> | Bewerking | Microsoft.AzureStack/registrations/getActivationKey/action | De nieuwste Azure-Stack activeringssleutel opgehaald |
> | Bewerking | Microsoft.AzureStack/registrations/products/listDetails/action | Details voor een Azure-Stack Marketplace-product uitgebreid opgehaald |
> | Bewerking | Microsoft.AzureStack/registrations/products/read | Haalt u de eigenschappen van een Azure-Stack Marketplace-product |
> | Bewerking | Microsoft.AzureStack/registrations/read | De eigenschappen van een Azure-Stack-registratie opgehaald |
> | Bewerking | Microsoft.AzureStack/registrations/write | Maken of bijwerken van een Azure-Stack-registratie |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/delete | Een toepassing verwijderen |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/read | Een lijst met toepassingen of haalt u de eigenschappen van een toepassing |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Hiermee activeert u een toepassingspakket |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/delete | Hiermee verwijdert u een toepassingspakket |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/read | De eigenschappen van een toepassingspakket opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/write | Een nieuw pakket maken of bijwerken van een bestaand toepassingspakket |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/write | Een nieuwe toepassing maken of bijwerken van een bestaande toepassing |
> | Bewerking | Microsoft.Batch/batchAccounts/certificateOperationResults/read | De resultaten van een langdurige certificaatbewerking op een Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Hiermee annuleert u het verwijderen van een certificaat op een Batch-account is mislukt |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/delete | Hiermee verwijdert u een certificaat van een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/read | De eigenschappen van een certificaat opgehaald of ingesteld vindt u certificaten in een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/write | Maakt een nieuw certificaat op een Batch-account of een bestaand certificaat updates |
> | Bewerking | Microsoft.Batch/batchAccounts/delete | Hiermee verwijdert u een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/listkeys/action | Een lijst met sleutels voor een Batch-account toegang tot |
> | Bewerking | Microsoft.Batch/batchAccounts/operationResults/read | De resultaten van een langdurige bewerking van de Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/poolOperationResults/read | De resultaten van een langdurige bewerking van de groep van toepassingen in een Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/delete | Een groep verwijdert uit een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Schakelt automatisch schalen voor een Batch-pool-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/read | Een lijst met toepassingen op een Batch-account of haalt u de eigenschappen van een groep |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/stopResize/action | Stopt een lopende vergroten of verkleinen van de bewerking op een Batch-pool-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/upgradeOs/action | Upgrades van het besturingssysteem van een Batch-pool-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/write | Maakt een nieuwe pool voor een batchaccount of een bestaande toepassingen bijwerken |
> | Bewerking | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor de Batch-service |
> | Bewerking | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de Batch-service |
> | Bewerking | Microsoft.Batch/batchAccounts/read | Een lijst met Batch-accounts of haalt u de eigenschappen van een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/regeneratekeys/action | Opnieuw toegangssleutels voor een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Toegangssleutels voor het opslagaccount automatisch is geconfigureerd voor een Batch-account wordt gesynchroniseerd |
> | Bewerking | Microsoft.Batch/batchAccounts/write | Een nieuwe Batch-account maken of bijwerken van een bestaande Batch-account |
> | Bewerking | Microsoft.Batch/locations/checkNameAvailability/action | Controleert of de accountnaam geldig en niet in gebruik is. |
> | Bewerking | Microsoft.Batch/locations/quotas/read | Batch-quota van het opgegeven abonnement bij de opgegeven Azure-regio worden opgehaald |
> | Bewerking | Microsoft.Batch/register/action | Het abonnement voor de Batch-Resourceprovider geregistreerd en wordt het maken van de Batch-accounts |
> | Bewerking | Microsoft.Batch/unregister/action | Heft de registratie van het abonnement voor de Batch-Resourceprovider zo wordt voorkomen dat het maken van de Batch-accounts |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.BatchAI/clusters/delete | Hiermee verwijdert u een Batch AI-cluster |
> | Bewerking | Microsoft.BatchAI/clusters/read | Een lijst met Batch AI clusters of haalt u de eigenschappen van een Batch AI-cluster |
> | Bewerking | Microsoft.BatchAI/clusters/remoteLoginInformation/action | Een lijst met extern-aanmeldingsgegevens voor een Batch AI-cluster |
> | Bewerking | Microsoft.BatchAI/clusters/write | Een nieuw Batch AI-cluster maken of bijwerken van een bestaand Batch AI-cluster |
> | Bewerking | Microsoft.BatchAI/fileservers/delete | Hiermee verwijdert u een Batch AI-bestandsserver |
> | Bewerking | Microsoft.BatchAI/fileservers/read | Een lijst met Batch AI bestandsservers of haalt u de eigenschappen van een Batch AI-bestandsserver |
> | Bewerking | Microsoft.BatchAI/fileservers/resume/action | Een Batch AI-bestandsserver wordt hervat |
> | Bewerking | Microsoft.BatchAI/fileservers/suspend/action | Een Batch AI-bestandsserver wordt onderbroken |
> | Bewerking | Microsoft.BatchAI/fileservers/write | Een nieuwe Batch AI-bestandsserver maken of bijwerken van een bestaande Batch AI-bestandsserver |
> | Bewerking | Microsoft.BatchAI/jobs/delete | Hiermee verwijdert u een Batch AI-taak |
> | Bewerking | Microsoft.BatchAI/jobs/read | Een lijst met taken in Batch AI of haalt u de eigenschappen van een Batch AI-taak |
> | Bewerking | Microsoft.BatchAI/jobs/remoteLoginInformation/action | Een lijst met extern-aanmeldingsgegevens voor een Batch AI-taak |
> | Bewerking | Microsoft.BatchAI/jobs/terminate/action | Een taak met Batch AI beëindigd |
> | Bewerking | Microsoft.BatchAI/jobs/write | Een nieuwe Batch AI-taak maken of bijwerken van een bestaande Batch AI-taak |
> | Bewerking | Microsoft.BatchAI/register/action | Het abonnement voor de Batch AI-Resourceprovider geregistreerd en wordt het maken van Batch AI-resources |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Billing/billingPeriods/read | Een lijst met beschikbare facturering punten |
> | Bewerking | Microsoft.Billing/invoices/read | Een lijst met beschikbare facturen |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.BingMaps/mapApis/Delete | Verwijderbewerking |
> | Bewerking | Microsoft.BingMaps/mapApis/listSecrets/action | Lijst met geheimen weergeven |
> | Bewerking | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Verificatietoken voor eenmalige aanmelding voor de resource lezen |
> | Bewerking | Microsoft.BingMaps/mapApis/Read | Leesbewerking |
> | Bewerking | Microsoft.BingMaps/mapApis/regenerateKey/action | De sleutel opnieuw genereren |
> | Bewerking | Microsoft.BingMaps/mapApis/Write | Schrijfbewerking |
> | Bewerking | Microsoft.BingMaps/Operations/read | Beschrijving van de bewerking. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Cache/checknameavailability/action | Hiermee wordt gecontroleerd of een naam beschikbaar is voor gebruik met een nieuwe Redis Cache |
> | Bewerking | Microsoft.Cache/locations/operationresults/read | Hiermee wordt het resultaat van een langdurige bewerking opgehaald waarvoor de header 'Location' eerder is geretourneerd naar de client |
> | Bewerking | Microsoft.Cache/operations/read | Hiermee worden de bewerkingen weergegeven die door de provider Microsoft.Cache worden ondersteund. |
> | Bewerking | Microsoft.Cache/redis/delete | Volledige Redis-cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/export/action | Redis-gegevens exporteren naar vooraf ingestelde opslag-blobs in een opgegeven indeling |
> | Bewerking | Microsoft.Cache/redis/firewallRules/delete | IP-firewallregels voor een Redis Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/firewallRules/read | De IP-firewallregels voor een Redis Cache ophalen |
> | Bewerking | Microsoft.Cache/redis/firewallRules/write | De IP-firewallregels voor een Redis Cache bewerken |
> | Bewerking | Microsoft.Cache/redis/forceReboot/action | Een cache-instantie geforceerd opnieuw opstarten met mogelijk gegevensverlies. |
> | Bewerking | Microsoft.Cache/redis/import/action | Gegevens met een opgegeven indeling in Redis importeren vanuit meerdere blobs |
> | Bewerking | Microsoft.Cache/redis/linkedservers/delete | Gekoppelde server uit een Redis-cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/linkedservers/read | Gekoppelde servers die zijn gekoppeld aan een Redis-cache ophalen. |
> | Bewerking | Microsoft.Cache/redis/linkedservers/write | Gekoppelde server aan een Redis-cache toevoegen |
> | Bewerking | Microsoft.Cache/redis/listKeys/action | De waarde van de toegangssleutels voor Redis-cache weergeven in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/listUpgradeNotifications/read | De nieuwste upgrademeldingen weergeven voor de cachetenant. |
> | Bewerking | Microsoft.Cache/redis/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens voor een Redis-cache opgehaald |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/delete | Het schema voor het toepassen van patches voor een Redis Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/read | Hiermee wordt het schema voor het toepassen van patches voor een Redis Cache opgehaald |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/write | Het schema voor het toepassen van patches voor een Redis Cache aanpassen |
> | Bewerking | Microsoft.Cache/redis/read | De instellingen en configuratie van Redis-cache weergeven in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/regenerateKey/action | De waarde van de toegangssleutels voor Redis-cache wijzigen in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/start/action | Start een cache-instantie. |
> | Bewerking | Microsoft.Cache/redis/stop/action | Stop een cache-instantie. |
> | Bewerking | Microsoft.Cache/redis/write | De instellingen en configuratie van Redis-cache wijzigen in de beheerportal |
> | Bewerking | Microsoft.Cache/register/action | Hiermee wordt de resourceprovider Microsoft.Cache geregistreerd bij een abonnement |
> | Bewerking | Microsoft.Cache/unregister/action | Hiermee wordt de registratie van de resourceprovider Microsoft.Cache bij een abonnement ongedaan gemaakt |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Capacity/register/action | De capaciteit resourceprovider geregistreerd en het maken van capaciteit resources maakt. |
> | Bewerking | Microsoft.Capacity/reservationorders/action | Reservering bijwerken |
> | Bewerking | Microsoft.Capacity/reservationorders/delete | Reservering verwijderen |
> | Bewerking | Microsoft.Capacity/reservationorders/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/action | Reservering bijwerken |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/delete | Reservering verwijderen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/revisions/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/write | Reservering maken |
> | Bewerking | Microsoft.Capacity/reservationorders/write | Reservering maken |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Bewerking | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Bewerking | Microsoft.Cdn/edgenodes/delete |  |
> | Bewerking | Microsoft.Cdn/edgenodes/read |  |
> | Bewerking | Microsoft.Cdn/edgenodes/write |  |
> | Bewerking | Microsoft.Cdn/operationresults/delete |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Bewerking | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Bewerking | Microsoft.Cdn/operationresults/read |  |
> | Bewerking | Microsoft.Cdn/operationresults/write |  |
> | Bewerking | Microsoft.Cdn/operations/read |  |
> | Bewerking | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Bewerking | Microsoft.Cdn/profiles/delete |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen voor de resource opgehaald |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor de bron |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor Microsoft.Cdn |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/read |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Bewerking | Microsoft.Cdn/profiles/endpoints/write |  |
> | Bewerking | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Bewerking | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Bewerking | Microsoft.Cdn/profiles/read |  |
> | Bewerking | Microsoft.Cdn/profiles/write |  |
> | Bewerking | Microsoft.Cdn/register/action | Hiermee wordt het abonnement voor de CDN-resourceprovider geregistreerd en wordt het maken van CDN-profielen mogelijk. |
> | Bewerking | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Een bestaand certificaat verwijderen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | De lijst met certificaten ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Toevoegen van een nieuw certificaat of een bestaande bijgewerkt |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Delete | Een bestaande AppServiceCertificate verwijderen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Read | De lijst met CertificateOrders ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Geef een bestaande certificateorder |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Een bestaande certificateorder vernieuwen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Certificaat-e-mail opnieuw verzenden |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Aanvraag e-mailberichten naar een ander e-mailadres verzenden |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Site zegel voor een uitgegeven certificaat van de App-Service ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | De lijst met acties certificaat ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Geschiedenis van de e-certificaat ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Domeineigendom controleren |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Write | Een nieuwe certificateOrder toevoegen of een bestaande bijgewerkt |
> | Bewerking | Microsoft.CertificateRegistration/operations/Read | Lijst van alle bewerkingen van het app service-certificaat is geregistreerd |
> | Bewerking | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Service-principal voor service-principal voor app inrichten |
> | Bewerking | Microsoft.CertificateRegistration/register/action | Registreer de provider van Microsoft-Certificates resource voor het abonnement |
> | Bewerking | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Aankoop certificaatobject valideren zonder deze te verzenden |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicCompute/capabilities/read | Hiermee worden de mogelijkheden weergegeven |
> | Bewerking | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Hiermee wordt gecontroleerd of een bepaald domeinnaam beschikbaar is. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/active/write | Hiermee wordt de naam ingesteld van het actieve domein. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/availabilitySets/read | De beschikbaarheidsset voor de resource wordt weergegeven. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/capabilities/read | Geeft de mogelijkheden voor domeinnaamgeving weer |
> | Bewerking | Microsoft.ClassicCompute/domainNames/delete | De domeinnamen voor resources verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/delete | De domeinnaamextensies verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/read | Hiermee kunt u de domeinnaamextensies retourneren. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/write | De domeinnaamextensies toevoegen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Een nieuwe interne taakverdeling verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de interne load balancers van domeinnamen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | De interne load balancers worden opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Hiermee wordt een nieuwe interne taakverdeling gemaakt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de eindpuntsets voor de taakverdeling van de domeinnamen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Hiermee worden de eindpuntsets van de taakverdeling weergegeven |
> | Bewerking | Microsoft.ClassicCompute/domainNames/read | De domeinnamen voor resources retourneren. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | De gebruikte servicecertificaten verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de servicecertificaten van domeinnamen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Hiermee worden de gebruikte servicecertificaten geretourneerd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Hiermee worden de gebruikte servicecertificaten toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/delete | Hiermee wordt een bepaald implementatiesleuf verwijderd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamsleuven gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/read | Hiermee worden de implementatiesleuven weergegeven. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | De extensieverwijzing voor de implementatiesite verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de extensieverwijzing van de rollen voor domeinnaamsleuven gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Hiermee kunt u de extensieverwijzing voor de implementatiesiterol retourneren. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Hiermee kunt u de extensieverwijzing voor de implementatiesiterol toevoegen of aanpassen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/read | Hiermee wordt de rol van de implementatiesleuf opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Hiermee wordt de bewerkingsstatus van rolinstanties van rollen voor domeinnaamsleuven gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Hiermee wordt het rolexemplaar opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Hiermee wordt de rolinstantie herbouwd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Hiermee wordt de installatiekopie van het rolexemplaar opnieuw toegepast. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Hiermee worden rolinstanties opnieuw gestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/start/action | Hiermee wordt een implementatiesleuf gestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestopt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/stop/action | Hiermee wordt de implementatiesleuf tijdelijk stopgezet. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Hiermee wordt stap voor stap een domeinupgrade uitgevoerd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/write | Hiermee wordt de implementatie gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/swap/action | Hiermee wordt de staging-site gewisseld naar de productiesite. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/write | Hiermee worden de domeinnamen voor resources toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.ClassicCompute/moveSubscriptionResources/action | Verplaats alle klassieke resources naar een ander abonnement. |
> | Bewerking | Microsoft.ClassicCompute/operatingSystemFamilies/read | Hiermee wordt een lijst met de gastbesturingssysteemfamilies gemaakt die beschikbaar zijn in Microsoft Azure. Ook worden de besturingssysteemversies weergegeven die beschikbaar zijn voor elke serie. |
> | Bewerking | Microsoft.ClassicCompute/operatingSystems/read | Hiermee wordt een lijst van de versies van gastbesturingssystemen gemaakt die momenteel beschikbaar zijn in Microsoft Azure. |
> | Bewerking | Microsoft.ClassicCompute/quotas/read | Hiermee worden de quota voor het abonnement opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/register/action | Registreren bij klassieke berekening |
> | Bewerking | Microsoft.ClassicCompute/resourceTypes/skus/read | Haalt de SKU-lijst op voor ondersteunde resourcetypen. |
> | Bewerking | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | De beschikbaarheid van het abonnement voor de klassieke verplaatsingsbewerking valideren. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de virtuele machine is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de virtuele machine is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de virtuele machine is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Hiermee worden de mogelijk asynchrone bewerkingen opgehaald |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Hiermee wordt een gegevensschijf gekoppeld aan een virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/delete | Hiermee worden virtuele machines verwijderd. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Hiermee wordt een gegevensschijf losgekoppeld van een virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/disks/read | Hiermee wordt een lijst met gegevensschijven opgehaald |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Hiermee wordt het RDP-bestand voor de virtuele machine gedownload. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van extensies van virtuele machines gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/read | Hiermee wordt de extensie van de virtuele machine opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/write | Hiermee wordt de extensie voor de virtuele machine ingesteld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/metrics/read | Hiermee worden de metrische gegevens opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de netwerkinterface is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de netwerkinterface is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de netwerkinterface is gekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Hiermee wordt de bewerkingsstatus van virtuele machines gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Hiermee wordt onderhoud van de virtuele machine uitgevoerd. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/read | Hiermee wordt een lijst met virtuele machines opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Hiermee wordt de virtuele machine opnieuw geïmplementeerd. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/restart/action | Hiermee worden virtuele machines opnieuw gestart. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Hiermee wordt de virtuele machine afgesloten. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/stop/action | Hiermee wordt de virtuele machine gestopt. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/write | Hiermee worden virtuele machines toegevoegd of gewijzigd. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | De lijst met ondersteunde apparaten wordt opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Hiermee wordt de beveiligingsregel verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de beveiligingsregels voor de netwerkbeveiligingsgroep gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Hiermee wordt de beveiligingsregel opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Hiermee wordt een beveiligingsregel toegevoegd of bijgewerkt. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/write | Hiermee wordt een nieuwe netwerkbeveiligingsgroep toegevoegd. |
> | Bewerking | Microsoft.ClassicNetwork/quotas/read | Hiermee worden de quota voor het abonnement opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/register/action | Registreren bij klassiek netwerk |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/delete | Hiermee wordt een gereserveerd IP verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/join/action | Deelnemen aan een gereserveerde IP |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde IP koppelen |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de gereserveerde IP's gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/read | Hiermee worden de gereserveerde IP's opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/write | Een nieuw gereserveerd IP toevoegen |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Hiermee worden de mogelijkheden weergegeven |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Hiermee wordt de beschikbaarheid van een bepaald IP-adres in een virtueel netwerk gecontroleerd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/delete | Hiermee wordt het virtuele netwerk verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Hiermee wordt de intrekking van een clientcertificaat ongedaan gemaakt. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | De ingetrokken clientcertificaten lezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Hiermee wordt een clientcertificaat ingetrokken. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Hiermee wordt het clientcertificaat voor de gateway van het virtuele netwerk verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Hiermee wordt een certificaat gedownload op basis van miniatuur. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Het certificaatpakket van de virtuele netwerkgateway wordt weergegeven. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Clienthoofdcertificaten zoeken. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Hiermee wordt een nieuw clienthoofdcertificaat geüpload. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Hiermee wordt een gatewayverbinding tussen sites ingesteld. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Hiermee wordt een gatewayverbinding tussen sites verbroken. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Hiermee wordt de lijst met verbindingen opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Hiermee wordt een gatewayverbinding tussen sites getest. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | De gateway van het virtuele netwerk wordt verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Het configuratiescript voor het apparaat wordt gedownload. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Hiermee worden de diagnotische gegevens van de gateway gedownload. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Hiermee wordt de circuitservicesleutel opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Het gatewaypakket voor het virtuele netwerk wordt weergegeven. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerkgateways gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Het gatewaypakket voor het virtuele netwerk wordt opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | De gateways voor het virtuele netwerk worden opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestart. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestopt. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Een gateway voor het virtuele netwerk wordt toegevoegd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/join/action | Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerken gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Hiermee wordt een virtueel netwerk via peercommunicatie gebruikt met een ander virtueel netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/read | Het virtuele netwerk wordt opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan het subnet is gekoppeld. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep die aan het subnet van het virtueel netwerk is gekoppeld gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan het subnet is gekoppeld. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan het subnet is gekoppeld. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/write | Een nieuw virtueel netwerk wordt toegevoegd. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicStorage/capabilities/read | Hiermee worden de mogelijkheden weergegeven |
> | Bewerking | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Hiermee wordt gecontroleerd of een opslagaccount beschikbaar is. |
> | Bewerking | Microsoft.ClassicStorage/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/images/read | Hiermee wordt de afbeelding geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/osImages/read | De installatiekopie van het besturingssysteem herstellen. |
> | Bewerking | Microsoft.ClassicStorage/publicImages/read | Hiermee wordt de openbare installatiekopie voor de virtuele machine opgehaald. |
> | Bewerking | Microsoft.ClassicStorage/quotas/read | Hiermee worden de quota voor het abonnement opgehaald. |
> | Bewerking | Microsoft.ClassicStorage/register/action | Registreren bij klassieke opslag |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/delete | Hiermee wordt het opslagaccount verwijderd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/delete | Hiermee wordt een opgegeven opslagaccountschijf verwijderd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/write | Hiermee wordt een opslagaccountschijf toegevoegd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/images/delete | Hiermee verwijdert u een installatiekopie van het opgegeven opslagaccount. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/images/read | Retourneert de installatiekopie van het opslagaccount. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Hiermee wordt de betreffende installatiekopie van het besturingssysteem van een opslagaccount verwijderd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/osImages/read | Hiermee wordt de installatiekopie van het besturingssysteem van een opslagaccount hersteld. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | De bestaande toegangssleutels voor het opslagaccount worden geregenereerd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Hiermee worden de diagnostische instellingen opgehaald. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Hiermee worden diagnostische instellingen toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Hiermee worden de metrische definities opgehaald. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Hiermee worden de metrische gegevens opgehaald. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/read | De beschikbare services ophalen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/write | Hiermee wordt een nieuw opslagaccount toegevoegd. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.CognitiveServices/accounts/delete | API-accounts verwijderen |
> | Bewerking | Microsoft.CognitiveServices/accounts/listKeys/action | Een lijst met sleutels maken |
> | Bewerking | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens opgehaald voor Cognitive Services. |
> | Bewerking | Microsoft.CognitiveServices/accounts/read | Hiermee worden API-accounts gelezen. |
> | Bewerking | Microsoft.CognitiveServices/accounts/regenerateKey/action | Sleutel opnieuw genereren |
> | Bewerking | Microsoft.CognitiveServices/accounts/skus/read | Hiermee worden beschikbare SKU's voor een bestaande resource gelezen. |
> | Bewerking | Microsoft.CognitiveServices/accounts/usages/read | Het quotumgebruik voor een bestaande resource ophalen. |
> | Bewerking | Microsoft.CognitiveServices/accounts/write | Hiermee worden API-accounts geschreven. |
> | Bewerking | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Hiermee worden beschikbare SKU's voor een abonnement gelezen. |
> | Bewerking | Microsoft.CognitiveServices/Operations/read | Alle beschikbare bewerkingen vermelden |
> | Bewerking | Microsoft.CognitiveServices/register/action | Registreert het abonnement voor Cognitive Services |
> | Bewerking | Microsoft.CognitiveServices/skus/read | Leest beschikbare SKU's voor Cognitive Services. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Commerce/RateCard/read | Retourneert bieden gegevens, metagegevens van de resource/meter en tarieven voor het opgegeven abonnement. |
> | Bewerking | Microsoft.Commerce/UsageAggregates/read | Haalt de Microsoft Azure verbruik door een abonnement. Het resultaat bevat statistische gegevens over het gebruik, abonnement en de resource verwante gegevens, op een bepaalde periode. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Compute/availabilitySets/delete | Hiermee wordt de beschikbaarheidsset verwijderd |
> | Bewerking | Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald |
> | Bewerking | Microsoft.Compute/availabilitySets/vmSizes/read | Lijst met beschikbare grootten voor het maken of bijwerken van een virtuele machine in de beschikbaarheidsset weergeven |
> | Bewerking | Microsoft.Compute/availabilitySets/write | Hiermee wordt een nieuwe beschikbaarheidsset gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/disks/beginGetAccess/action | Hiermee wordt de SAS-URI van de schijf opgehaald voor blob-toegang |
> | Bewerking | Microsoft.Compute/disks/delete | Hiermee wordt de schijf verwijderd |
> | Bewerking | Microsoft.Compute/disks/endGetAccess/action | Hiermee wordt de SAS-URI van de schijf ingetrokken |
> | Bewerking | Microsoft.Compute/disks/read | Hiermee worden de eigenschappen van een schijf opgehaald |
> | Bewerking | Microsoft.Compute/disks/write | Hiermee wordt een nieuwe schijf gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/images/delete | Hiermee wordt de installatiekopie verwijderd |
> | Bewerking | Microsoft.Compute/images/read | Ga naar de eigenschappen van de installatiekopie |
> | Bewerking | Microsoft.Compute/images/write | Hiermee wordt een nieuwe installatiekopie gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/locations/capsOperations/read | Hiermee wordt de status van een asynchrone Caps-bewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/diskOperations/read | Hiermee wordt de status van een asynchrone schijfbewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | De eigenschappen van een platforminstallatiekopieaanbieding ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | De eigenschappen van een platforminstallatiekopie-SKU ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | De eigenschappen van een platforminstallatiekopieversie ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Hiermee worden de eigenschappen van een VMExtension-type opgehaald |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Hiermee worden de eigenschappen van een VMExtension-versie opgehaald |
> | Bewerking | Microsoft.Compute/locations/publishers/read | De eigenschappen van een uitgever ophalen |
> | Bewerking | Microsoft.Compute/locations/runCommands/read | Hiermee wordt een lijst gemaakt van de beschikbare opdrachten op de locatie |
> | Bewerking | Microsoft.Compute/locations/usages/read | Hiermee worden servicelimieten en huidige gebruikshoeveelheden opgehaald voor de rekenresources van het abonnement op een locatie |
> | Bewerking | Microsoft.Compute/locations/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten van virtuele machines op een locatie |
> | Bewerking | Microsoft.Compute/operations/read | Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Compute-resourceprovider |
> | Bewerking | Microsoft.Compute/register/action | Hiermee wordt het abonnement bij de Microsoft.Compute-resourceprovider geregistreerd |
> | Bewerking | Microsoft.Compute/restorePointCollections/delete | Hiermee worden de herstelpuntverzameling en de hierin opgenomen herstelpunten verwijderd |
> | Bewerking | Microsoft.Compute/restorePointCollections/read | Hiermee worden de eigenschappen van een herstelpuntverzameling opgehaald |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/delete | Hiermee wordt het herstelpunt verwijderd |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/read | Hiermee worden de eigenschappen van een herstelpunt opgehaald |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Hiermee worden de eigenschappen van een herstelpunt en de URI's van de SAS-blob opgehaald |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/write | Hiermee wordt een nieuw herstelpunt gemaakt |
> | Bewerking | Microsoft.Compute/restorePointCollections/write | Hiermee wordt een nieuwe herstelpuntverzameling gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/sharedVMImages/delete | Hiermee wordt de SharedVMImage verwijderd |
> | Bewerking | Microsoft.Compute/sharedVMImages/read | De eigenschappen van een SharedVMImage ophalen |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/delete | Een SharedVMImageVersion verwijderen |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/read | Hiermee worden de eigenschappen van een SharedVMImageVersion opgehaald |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/replicate/action | Een SharedVMImageVersion repliceren naar de doelregio 's |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/write | Hiermee wordt een nieuwe SharedVMImageVersion gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/sharedVMImages/write | Hiermee wordt een nieuwe SharedVMImage gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/skus/read | Hiermee wordt de lijst met de voor uw abonnement beschikbare Microsoft.Compute-SKU's opgehaald |
> | Bewerking | Microsoft.Compute/snapshots/beginGetAccess/action | De SAS-URI van de momentopname ophalen voor blob-toegang |
> | Bewerking | Microsoft.Compute/snapshots/delete | Hiermee wordt een momentopname verwijderd |
> | Bewerking | Microsoft.Compute/snapshots/endGetAccess/action | De SAS-URI van de momentopname intrekken |
> | Bewerking | Microsoft.Compute/snapshots/read | Hiermee worden de eigenschappen van een momentopname opgehaald |
> | Bewerking | Microsoft.Compute/snapshots/write | Hiermee wordt een nieuwe momentopname gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachines/capture/action | Hiermee wordt de virtuele machine vastgelegd via het kopiëren van virtuele harde schijven en wordt een sjabloon gegenereerd waarmee soortgelijke virtuele machines kunnen worden gemaakt |
> | Bewerking | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Hiermee worden de op blobs gebaseerde schijven van de virtuele machine geconverteerd naar beheerde schijven |
> | Bewerking | Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven |
> | Bewerking | Microsoft.Compute/virtualMachines/delete | Hiermee wordt de virtuele machine verwijderd |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/delete | Hiermee wordt de extensie van de virtuele machine verwijderd |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/read | Hiermee worden de eigenschappen van de extensie van een virtuele machine opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/write | Hiermee wordt een nieuwe extensie van een virtuele machine gemaakt of een bestaande extensie bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachines/generalize/action | Hiermee wordt de status van de virtuele machine ingesteld op Gegeneraliseerd en wordt de virtuele machine voorbereid voor vastleggen |
> | Bewerking | Microsoft.Compute/virtualMachines/instanceView/read | Hiermee wordt de gedetailleerde runtimestatus van de virtuele machine en bijbehorende resources opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachines/performMaintenance/action | Hiermee wordt de onderhoudsbewerking uitgevoerd op de VM. |
> | Bewerking | Microsoft.Compute/virtualMachines/powerOff/action | Hiermee wordt de virtuele machine uitgeschakeld. Houd er rekening mee dat de virtuele machine wordt nog wel gefactureerd. |
> | Bewerking | Microsoft.Compute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities van de virtuele machine gelezen |
> | Bewerking | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachines/redeploy/action | Hiermee wordt de virtuele machine opnieuw geïmplementeerd |
> | Bewerking | Microsoft.Compute/virtualMachines/reimage/action | Hiermee wordt een installatiekopie teruggezet van de virtuele machine die een differentiërende schijf gebruikt. |
> | Bewerking | Microsoft.Compute/virtualMachines/restart/action | Hiermee wordt de virtuele machine opnieuw gestart |
> | Bewerking | Microsoft.Compute/virtualMachines/runCommand/action | Hiermee wordt een vooraf gedefinieerd script uitgevoerd op de virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart |
> | Bewerking | Microsoft.Compute/virtualMachines/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachines/write | Hiermee wordt een nieuwe virtuele machine gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Hiermee worden de rekenresources voor de instanties van de virtuele-machineschaalset uitgeschakeld en vrijgegeven  |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/delete | Hiermee wordt de virtuele-machineschaalset verwijderd |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/delete/action | Hiermee worden de instanties van de virtuele-machineschaalset verwijderd |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Hiermee wordt de extensie van de virtuele-machineschaalset verwijderd |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Hiermee worden de eigenschappen voor de extensie van een virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Hiermee wordt een nieuwe extensie van de virtuele-machineschaalset gemaakt of een bestaande extensie bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Doorloop handmatig de platformupdatedomeinen van een virtuele-machineschaalset voor Service Fabric om een vastgelopen update die in behandeling is te voltooien. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Hiermee wordt de instantieweergave van de virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Hiermee worden instanties handmatig bijgewerkt naar het laatste model van de virtuele-machineschaalset |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Hiermee worden de eigenschappen van alle netwerkinterfaces van een virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Hiermee wordt de geschiedenis van de upgrades van het besturingssysteem voor een virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Hiermee wordt gepland onderhoud uitgevoerd op de instanties van de virtuele-machineschaalset |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Hiermee worden de instanties van de virtuele-machineschaalset uitgeschakeld |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities van de virtuele-machineschaalset gelezen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Hiermee worden de eigenschappen van alle openbare IP-adressen van een virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/read | Hiermee worden de eigenschappen van de virtuele-machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | De instanties van de virtuele-machineschaalset opnieuw implementeren |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Hiermee worden installatiekopieën voor de instanties van de virtuele-machineschaalset teruggezet |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/restart/action | Hiermee worden de instanties van de virtuele-machineschaalset opnieuw gestart |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Hiermee wordt de roulerende upgrade van een virtuele-machineschaalset geannuleerd |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | De meest recente status van de roulerende upgrade voor een virtuele-machineschaalset ophalen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/scale/action | Controleer of een bestaande virtuele-machineschaalset kan in-/uitschalen naar een opgegeven aantal instanties |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/skus/read | Hiermee wordt een lijst weergegeven van de geldige SKU's voor een bestaande virtuele-machineschaalset |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/start/action | Hiermee worden de instanties van de virtuele-machineschaalset gestart |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Hiermee worden de rekenresources voor een virtuele machine in een VM-schaalset uitgeschakeld en vrijgegeven |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Een specifieke virtuele machine uit een VM-schaalset verwijderen. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Hiermee wordt de instantieweergave van een virtuele machine in een VM-schaalset opgehaald. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Eigenschappen van openbare IP-adres die zijn gemaakt met behulp van virtuele-Machineschaalset opgehaald. Virtuele-Machineschaalset kunt mag maximaal één openbare IP per ipconfiguration (privé-IP) maken |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Eigenschappen van een of alle IP-configuraties van een netwerkinterface die is gemaakt met behulp van virtuele-Machineschaalset opgehaald. IP-configuraties vertegenwoordigen persoonlijke IP-adressen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Hiermee worden de eigenschappen opgehaald van een of van alle netwerkinterfaces van een virtuele machine die met behulp van een virtuele-machineschaalset is gemaakt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Hiermee wordt gepland onderhoud uitgevoerd op een instantie van een virtuele machine in een virtuele-machineschaalset |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset uitgeschakeld. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities in de virtuele-machineschaalset gelezen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Hiermee wordt een instantie van een virtuele machine in een virtuele-machineschaalset opnieuw geïmplementeerd |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Hiermee wordt de installatiekopie van een instantie van een virtuele machine in een virtuele-machineschaalset teruggezet. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset opnieuw gestart. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset gestart. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/write | Hiermee wordt een nieuwe virtuele-machineschaalset gemaakt of een bestaande bijgewerkt |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Consumption/balances/read | Lijst van het gebruik van de samenvatting voor een factureringsperiode voor een beheergroep. |
> | Bewerking | Microsoft.Consumption/budgets/read | Lijst van de budgetten voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/budgets/write | Maakt, bijwerken en verwijderen van de budgetten voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/marketplaces/read | Lijst van de gebruiksdetails van de marketplace-resource voor een scope voor abonnementen EA en WebDirect. |
> | Bewerking | Microsoft.Consumption/operations/read | Lijst van alle ondersteunde bewerkingen door Microsoft.Consumption resourceprovider. |
> | Bewerking | Microsoft.Consumption/pricesheets/read | Een lijst met Pricesheets gegevens voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/reservationDetails/read | Lijst van de details van tapegebruik voor gereserveerde exemplaren door reservering volgorde of beheer van groepen. De gegevens details zijn per exemplaar per dag niveau. |
> | Bewerking | Microsoft.Consumption/reservationRecommendations/read | Lijst met aanbevelingen één of gedeelde voor gereserveerde exemplaren voor een abonnement. |
> | Bewerking | Microsoft.Consumption/reservationSummaries/read | Lijst van het gebruik van de samenvatting voor gereserveerde exemplaren door reservering volgorde of beheer van groepen. De gegevens van de samenvatting bevindt zich op niveau van de maand of per dag. |
> | Bewerking | Microsoft.Consumption/reservationTransactions/read | Lijst van de transactiegeschiedenis voor gereserveerde exemplaren door beheergroepen. |
> | Bewerking | Microsoft.Consumption/terms/read | Lijst van de voorwaarden voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/usageDetails/read | Lijst van de gebruiksgegevens voor een scope voor abonnementen EA en WebDirect. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Logbestanden voor een specifieke container ophalen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/delete | De specifieke containergroep verwijderen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de containergroep opgehaald. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de containergroep gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens opgehaald voor de containergroep. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/read | Alle containergroepen ophalen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/write | Een specifieke containergroep maken of bijwerken. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerRegistry/checkNameAvailability/read | Controleert of de containernaam van het register beschikbaar voor gebruik is. |
> | Bewerking | Microsoft.ContainerRegistry/locations/operationResults/read | Een asynchrone bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/operations/read | Een lijst met alle beschikbare Azure Container register REST-API-bewerkingen |
> | Bewerking | Microsoft.ContainerRegistry/register/action | Het abonnement voor de container register-resourceprovider geregistreerd en het maken van de container registers maakt. |
> | Bewerking | Microsoft.ContainerRegistry/registries/delete | Hiermee verwijdert u een container-register. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Een raster gebeurtenisfilter verwijdert uit een container-register. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/read | De eigenschappen van de opgegeven gebeurtenis raster filter opgehaald of een lijst met alle gebeurtenis raster filters voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Maken of bijwerken van een raster gebeurtenisfilter voor een container-registersleutel met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/importImage/action | -Installatiekopie importeren naar het register van de container met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/listCredentials/action | Hier worden de aanmeldingsreferenties voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/listUsages/read | Bevat de quota voor gebruik voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/operationStatuses/read | De status van een register asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor Microsoft ContainerRegistry |
> | Bewerking | Microsoft.ContainerRegistry/registries/read | De eigenschappen van het register opgegeven container opgehaald of een lijst met alle container registers onder de opgegeven resourcegroep of abonnement. |
> | Bewerking | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Genereert een van de aanmeldingsreferenties voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/delete | Hiermee verwijdert u een replicatie uit een container-register. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | De status van een replicatie asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/read | De eigenschappen van de opgegeven replicatie opgehaald of een lijst met alle replicaties voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/write | Maken of bijwerken van een replicatie voor een container-registersleutel met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/delete | Hiermee wordt een webhook verwijderd uit het register van een container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Hiermee wordt de configuratie van de service-URI en aangepaste headers voor de webhook. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Een lijst met recente gebeurtenissen voor de opgegeven webhook. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | De status van een webhook asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Een ping gebeurtenis moet worden verzonden naar de webhook. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/read | De eigenschappen van de opgegeven webhook opgehaald of een lijst met alle webhooks voor het register opgegeven container. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/write | Maken of bijwerken van een webhook voor een container-registersleutel met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/write | Maken of bijwerken van het register van een container met de opgegeven parameters. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerService/containerServices/delete | Een containerservice verwijderen |
> | Bewerking | Microsoft.ContainerService/containerServices/read | Ophalen van een containerservice |
> | Bewerking | Microsoft.ContainerService/containerServices/write | Maakt een nieuwe containerservice of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Een profiel voor een beheerde cluster toegang door de naam van de rol met behulp van de referentie lijst ophalen |
> | Bewerking | Microsoft.ContainerService/managedClusters/accessProfiles/read | Een profiel voor een beheerde cluster toegang krijgen door rolnaam |
> | Bewerking | Microsoft.ContainerService/managedClusters/delete | Hiermee verwijdert u een beheerde cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor een beheerde cluster-bron ophalen |
> | Bewerking | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor een beheerde cluster-bron |
> | Bewerking | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor beheerd Cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/read | Ophalen van een beheerd cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/write | Hiermee maakt u een nieuw cluster met beheerde of een bestaande bijgewerkt |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContentModerator/applications/delete | Verwijderbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/listSecrets/action | Geheimen vermelden |
> | Bewerking | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Eenmalige aanmelding Tokens lezen |
> | Bewerking | Microsoft.ContentModerator/applications/read | Leesbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Bewerking | Microsoft.ContentModerator/listCommunicationPreference/action | Lijst communicatievoorkeur |
> | Bewerking | Microsoft.ContentModerator/operations/read | leesbewerkingen |
> | Bewerking | Microsoft.ContentModerator/updateCommunicationPreference/action | Communicatievoorkeur bijwerken |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.CustomerInsights/hubs/adobemetadata/action | Maken of bijwerken van de klant Azure Insights Adobe metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/adobemetadata/read | Azure klant Insights Adobe metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Een Azure klant Insights Shared Access Signature-beleid verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Geen inzichten Azure klant gedeeld toegangsbeleid handtekening lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Azure klant Insights handtekening beleid voor gedeelde toegang primaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Azure klant Insights handtekening beleid voor gedeelde toegang secundaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Een Azure klant Insights Shared Access Signature beleid maken of bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/activate/action | Activeren van een Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/activate/action | Activeren van een Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/delete | Elke klant Azure Insights-Connector verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | De status van elk Azure klant Insights-Connector runtime ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Activeren van een toewijzing van Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Een toewijzing van Azure klant Insights-Connector verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Het resultaat van een toewijzing van Azure klant Insights-Connector bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Lezen van een toewijzing van Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Maken of bijwerken van een toewijzing van Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/operations/read | Het resultaat van een klant Insights-Connector van Azure-bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/read | Lezen van een Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Maken of bijwerken van een Azure klant Insights-Connector verificatie informatie over de gatewayverbinding |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/update/action | Bijwerken van een Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/write | Maken of bijwerken van een Azure klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/crmmetadata/action | Maken of bijwerken van alle Azure klant Insights Crm-metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/crmmetadata/read | Alle Azure klant Insights Crm-metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/delete | Verwijderen van een Azure klant Insights Hub |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/delete | Een Azure klant Insights Gdpr verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/read | Een Azure klant Insights Gdpr lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/write | Maken of bijwerken van een Azure klant Insights Gdpr |
> | Bewerking | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Azure klant Insights Hub facturering tegoed ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Azure klant Insights Hub facturering geschiedenis ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/delete | De installatiekopie van een Azure klant Insights verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/read | Een Azure klant Insights afbeelding lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/write | Maken of bijwerken van een Azure klant Insights-afbeelding |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/delete | Azure klant Insights interacties verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/operations/read | Het resultaat van een Azure klant Insights interactie bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/read | Elke klant Azure Insights interactie lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Relatie koppelingen aanbevelen voor Azure klant Insights interacties |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/write | Maken of bijwerken van elke klant Azure Insights interactie |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/delete | Een Azure klant Insights Key Performance Indicator verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/operations/read | Het resultaat van een Azure klant Insights Key Performance Indicators bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/read | Een Azure klant Insights Key Performance Indicator lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Een Azure klant Insights prestatie-indicatoren opnieuw verwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/write | Maken of bijwerken van een Azure klant Insights Key Performance Indicator |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/delete | Een Azure klant Insights koppelingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/operations/read | Het resultaat van een Azure klant Insights koppelingen bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/read | Azure klant Insights koppelingen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/write | Maken of bijwerken van de klant Azure koppelingen |
> | Bewerking | Microsoft.CustomerInsights/hubs/msemetadata/action | Maken of bijwerken van alle metagegevens van de klant Azure Insights muis |
> | Bewerking | Microsoft.CustomerInsights/hubs/msemetadata/read | Azure klant Insights muis metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/operationresults/read | Resultaat van de klant Azure Insights Hub-bewerking ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/delete | Een Azure klant Insights voorspellingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/operations/read | Het resultaat van een Azure klant Insights voorspellingen bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/read | Een Azure klant Insights voorspellingen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/write | Maken of bijwerken van een Azure klant voorspellingen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Een Azure klant Insights voorspellende overeen beleid verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Het resultaat van een bewerking Azure klant Insights voorspellende overeen beleid lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Azure klant Insights voorspellende overeen beleid lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Een Azure klant Insights voorspellende overeen beleid maken of bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/delete | Een Azure klant Insights-profiel verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/operations/read | Het resultaat van een Azure klant Insights profiel bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/read | Een Azure klant Insights profiel lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/write | Schrijven van een Azure klant Insights-profiel |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor resource |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens voor de resource opgehaald |
> | Bewerking | Microsoft.CustomerInsights/hubs/read | Een Azure klant Insights Hub lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Een Azure klant Insights relatie koppelingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Het resultaat van een Azure klant Insights relatie koppelingen bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Alle koppelingen van Azure klant Insights relatie lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Maken of bijwerken van de klant Azure Insights relatie koppelingen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/delete | Verwijder alle Azure klant Insights relaties |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/operations/read | Het resultaat van een Azure Insights relaties bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/read | De relaties die Azure klant Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/write | Maken of bijwerken van de klant Azure Insights relaties |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Verwijderen van een Azure klant Insights Rbac-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Het resultaat van een Azure klant Insights Rbac toewijzing bewerking lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/read | Lezen van een Azure klant Insights Rbac-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/write | Maken of bijwerken van een Azure klant Insights Rbac-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/roles/read | Azure klant Insights Rbac rollen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Maken of bijwerken van de klant Azure Insights SalesForce metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Alle metagegevens van de klant Azure Insights SalesForce lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/delete | Een Azure klantsegmenten Insights verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Beheer alle Azure klant inzicht dynamische segmenten |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/read | Een Azure klantsegmenten Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/static/action | Beheer alle Azure klant inzicht statische segmenten |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/write | Maken of bijwerken van een Azure klantsegmenten Insights |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Een Azure klant Insights SqlConnectionStrings verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Een Azure klant Insights SqlConnectionStrings lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Maken of bijwerken van een Azure klant Insights SqlConnectionStrings |
> | Bewerking | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Type Schema voorstellen genereren van voorbeeldgegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Een Azure klant Insights hub-instellingen beheren |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/delete | Elke klant Azure Insights App weergave verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/read | Elke weergave Azure klant Insights-App lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/write | Maken of bijwerken van een Azure klant Insights App weergeven |
> | Bewerking | Microsoft.CustomerInsights/hubs/widgettypes/read | Geen typen Azure klant Insights App Widget lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/write | Maken of bijwerken van een Azure klant Insights Hub |
> | Bewerking | Microsoft.CustomerInsights/operations/read | Azure klant Insights Api Metadatas lezen |
> | Bewerking | Microsoft.CustomerInsights/register/action | Het abonnement voor de klant Insights-resourceprovider geregistreerd en wordt het maken van de klant Insights-resources |
> | Bewerking | Microsoft.CustomerInsights/unregister/action | Heft de registratie van het abonnement voor de klant Insights-resourceprovider |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Databricks/workspaces/delete | Hiermee verwijdert u een werkruimte. |
> | Bewerking | Microsoft.Databricks/workspaces/read | Haalt een lijst met werkruimten. |
> | Bewerking | Microsoft.Databricks/workspaces/write | Maakt een werkruimte. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataCatalog/catalogs/delete | Hiermee verwijdert u de catalogus. |
> | Bewerking | Microsoft.DataCatalog/catalogs/read | Eigenschappen voor catalogi onder het abonnement of resourcegroep worden opgehaald. |
> | Bewerking | Microsoft.DataCatalog/catalogs/write | Catalogus maken of bijwerken van de labels en eigenschappen voor de catalogus. |
> | Bewerking | Microsoft.DataCatalog/checkNameAvailability/action | Controleert de beschikbaarheid van de catalogus-naam voor de tenant. |
> | Bewerking | Microsoft.DataCatalog/operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op Microsoft.DataCatalog resourceprovider. |
> | Bewerking | Microsoft.DataCatalog/register/action | Abonnement registreert met Microsoft.DataCatalog resourceprovider. |
> | Bewerking | Microsoft.DataCatalog/unregister/action | Registratie opheffen Microsoft.DataCatalog resourceprovider-abonnement. |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataFactory/datafactories/activitywindows/read | Windows-activiteit in de Gegevensfactory met de opgegeven parameters worden gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Leesbewerkingen activiteit Windows voor de Pipeline-activiteit met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Leesbewerkingen activiteit Windows voor de pijplijn met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/delete | Hiermee verwijdert u een pijplijn. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Een pijplijn onderbreekt. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/read | Een pijplijn leest. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Een pijplijn hervatten. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/update/action | Een pijplijn-updates. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/write | Maken of bijwerken van een pijplijn. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Leesbewerkingen activiteit Windows voor de gegevensset met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/read | Hiermee wordt een gegevensset gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Leest de gegevens segment worden uitgevoerd voor de opgegeven gegevensset met de opgegeven begintijd. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/slices/read | Hiermee haalt de gegevenssegmenten in de opgegeven periode. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/slices/write | De Status van het gegevenssegment bijwerken. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/write | Maken of bijwerken van een Dataset. |
> | Bewerking | Microsoft.DataFactory/datafactories/delete | Hiermee verwijdert u de Gegevensfactory. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Leest de verbindingsgegevens voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/delete | Hiermee verwijdert u een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Geeft een lijst van de verificatiesleutels voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/read | Een Gateway leest. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | De verificatiesleutels genereert voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/write | Maken of bijwerken van een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/delete | Hiermee verwijdert u alle gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/read | Een gekoppelde Service leest. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/write | Maken of bijwerken van een gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor datafactories |
> | Bewerking | Microsoft.DataFactory/datafactories/read | Leest de Gegevensfactory. |
> | Bewerking | Microsoft.DataFactory/datafactories/runs/loginfo/read | Een SAS-URI naar een blob-container met de logboeken leest. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/read | Hiermee wordt een gegevensset gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/write | Maken of bijwerken van een Dataset. |
> | Bewerking | Microsoft.DataFactory/datafactories/write | Maken of bijwerken van de Gegevensfactory. |
> | Bewerking | Microsoft.DataFactory/factories/cancelpipelinerun/action | Hiermee annuleert u de pijplijn uitvoeren opgegeven door de uitvoeren-ID. |
> | Bewerking | Microsoft.DataFactory/factories/datasets/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/factories/datasets/read | Hiermee wordt een gegevensset gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/datasets/write | Maken of bijwerken van een Dataset. |
> | Bewerking | Microsoft.DataFactory/factories/delete | Hiermee verwijdert u Data Factory. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/delete | Hiermee verwijdert u eventuele integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Verbindingsgegevens voor integratie Runtime leest. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Runtimestatus van de integratie worden gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Vindt u de verificatiesleutels voor elke integratie tijdens Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Hiermee haalt de bewakingsgegevens voor alle integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Hiermee verwijdert u het knooppunt voor de opgegeven integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Retourneert het IP-adres voor het opgegeven knooppunt van de Runtime-integratie. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Updates op een host zichzelf integratie Runtime-knooppunt. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/read | Integratie Runtime leest. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | De verificatie-sleutels genereert voor de opgegeven integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/start/action | Start eventuele integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Stopt alle integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | De referenties voor de opgegeven integratie Runtime gesynchroniseerd. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Hiermee wordt de opgegeven integratie Runtime bijgewerkt. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/write | Maken of bijwerken van een integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/delete | Hiermee verwijdert u gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/read | Leesbewerkingen gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/write | Maken of bijwerken van de gekoppelde Service |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Leest dat de activiteit wordt uitgevoerd voor de opgegeven pijplijn ID uitvoeren |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/read | Leest de pijplijn wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/createrun/action | Maakt een uitvoering van de pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/delete | Hiermee verwijdert u een pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/read | Leest de pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/write | Maken of bijwerken van de pijplijn |
> | Bewerking | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.DataFactory/factories/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor fabrieken |
> | Bewerking | Microsoft.DataFactory/factories/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor fabrieken |
> | Bewerking | Microsoft.DataFactory/factories/read | Data Factory leest. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/delete | Hiermee verwijdert u een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/read | Een Trigger worden gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/start/action | Start een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/stop/action | Hiermee stopt een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/triggerruns/read | Leest de Trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/write | Maken of bijwerken van een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/write | Maken of bijwerken van de Data Factory |
> | Bewerking | Microsoft.DataFactory/register/action | Het abonnement voor de Data Factory Resource Provider registreert. |
> | Bewerking | Microsoft.DataFactory/unregister/action | Heft de registratie van het abonnement voor de Data Factory Resource Provider. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Een compute-beleid verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Informatie ophalen over een compute-beleid. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Maken of bijwerken van een compute-beleid. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Ontkoppelen van een account DataLakeStore van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Informatie ophalen over een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/delete | Een DataLakeAnalytics-account verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Een firewallregel verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Informatie ophalen over een firewallregel. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Maken of bijwerken van een firewallregel. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Resultaat van een bewerking van de account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen voor het account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor het account DataLakeAnalytics. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken voor het account DataLakeAnalytics niet ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/metricDefinitions/read | De beschikbare metrische gegevens voor het account DataLakeAnalytics niet ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/read | Informatie ophalen over een bestaande DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Lijst met SAS-tokens voor storage-containers van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Containers van een gekoppelde Storage-account van een account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Een opslagaccount van een account DataLakeAnalytics ontkoppelen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Informatie ophalen over een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen voor het annuleren van de taken die worden verzonden door andere gebruikers. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/write | Maken of bijwerken van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/capability/read | Ophalen van informatie over de functionaliteit van een abonnement met betrekking tot DataLakeAnalytics gebruiken. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Controleer de beschikbaarheid van een accountnaam DataLakeAnalytics. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/operationResults/read | Resultaat van een bewerking van de account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/operations/read | Beschikbare bewerkingen van DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/register/action | Abonnement op DataLakeAnalytics registreren. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataLakeStore/accounts/delete | Een DataLakeStore-account verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/enableKeyVault/action | KeyVault inschakelen voor een account DataLakeStore. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/delete | Een firewallregel verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/read | Informatie ophalen over een firewallregel. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/write | Maken of bijwerken van een firewallregel. |
> | Bewerking | Microsoft.DataLakeStore/accounts/operationResults/read | Resultaat van een bewerking van de account DataLakeStore ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen voor het account DataLakeStore ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor het account DataLakeStore. |
> | Bewerking | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken voor het account DataLakeStore niet ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/metricDefinitions/read | De beschikbare metrische gegevens voor het account DataLakeStore niet ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/read | Informatie ophalen over een bestaande DataLakeStore-account. |
> | Bewerking | Microsoft.DataLakeStore/accounts/Superuser/action | Supergebruiker op Data Lake Store als verleend met Microsoft.Authorization/roleAssignments/write verlenen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Verwijderen van een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Informatie ophalen over een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Maken of bijwerken van een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/write | Maken of bijwerken van een DataLakeStore-account. |
> | Bewerking | Microsoft.DataLakeStore/locations/capability/read | Ophalen van informatie over de functionaliteit van een abonnement met betrekking tot DataLakeStore gebruiken. |
> | Bewerking | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Controleer de beschikbaarheid van een accountnaam DataLakeStore. |
> | Bewerking | Microsoft.DataLakeStore/locations/operationResults/read | Resultaat van een bewerking van de account DataLakeStore ophalen. |
> | Bewerking | Microsoft.DataLakeStore/operations/read | Beschikbare bewerkingen van DataLakeStore ophalen. |
> | Bewerking | Microsoft.DataLakeStore/register/action | Abonnement op DataLakeStore registreren. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DBforMySQL/locations/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMySQL/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMySQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/delete | Hiermee wordt een bestaande firewallregel verwijderd. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/read | De lijst met firewall regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel retourneren. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/write | Hiermee maakt een firewallregel met de opgegeven parameters of update een bestaande regel. |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | De disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforMySQL/servers/read | Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald. |
> | Bewerking | Microsoft.DBforMySQL/servers/recoverableServers/read | Herstelbare gegevens van de MySQL-Server te retourneren |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren. |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforMySQL/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforPostgreSQL/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Hiermee wordt een bestaande firewallregel verwijderd. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/read | De lijst met firewall regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel retourneren. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Hiermee maakt een firewallregel met de opgegeven parameters of update een bestaande regel. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | De disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor Postgres servers |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/read | Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | De herstelbare PostgreSQL-serverinformatie retourneren |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Ophalen van gegevens van het beleid server threat detectie is geconfigureerd op een bepaalde server |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Het beleid van server threat detectie voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Devices/checkNameAvailability/Action | Controleer als IotHub naam beschikbaar is |
> | Bewerking | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Controleer als IotHub naam beschikbaar is |
> | Bewerking | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Verwijder de IotHub-tenant-bron |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | EventHub Consumer-groep verwijderen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | EventHub Consumer groep(en) ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | EventHub Consumergroep maken |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Exporteren van apparaten |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | De IotHub-tenant statistieken resource opgehaald |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Apparaten importeren |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | De IotHub-tenantsleutel opgehaald |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Details van ingediend op gegeven IotHub taken ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | IotHub tenantsleutels opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | De beschikbare logboekdefinities voor de IotHub-Service opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Quotum metrische gegevens ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Read | De bron van de tenant IotHub opgehaald |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Een bericht met alle bestaande Routes testen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Een bericht met een opgegeven test Route testen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Hiermee wordt de status van alle routering eindpunten voor een IotHub |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Write | Maken of bijwerken van de bron van de tenant IotHub |
> | Bewerking | Microsoft.Devices/ElasticPools/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Delete | Hiermee verwijdert u certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Verificatiecode genereren |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Read | Het certificaat opgehaald |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/verify/Action | Controleer de resource-certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Write | Maken of bijwerken van certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/Delete | IotHub-bron verwijderen |
> | Bewerking | Microsoft.Devices/IotHubs/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/IotHubs/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Hiermee verwijdert u het filter gebeurtenis raster |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Read | Het filter gebeurtenis raster opgehaald |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Write | Maken van nieuwe of bestaande gebeurtenis raster filter bijwerken |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | EventHub Consumer-groep verwijderen |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | EventHub Consumer groep(en) ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | EventHub Consumergroep maken |
> | Bewerking | Microsoft.Devices/iotHubs/exportDevices/Action | Exporteren van apparaten |
> | Bewerking | Microsoft.Devices/iotHubs/importDevices/Action | Apparaten importeren |
> | Bewerking | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Ophalen van IotHub Key voor de opgegeven naam |
> | Bewerking | Microsoft.Devices/iotHubs/iotHubStats/Read | IotHub-statistieken opvragen |
> | Bewerking | Microsoft.Devices/iotHubs/jobs/Read | Details van ingediend op gegeven IotHub taken ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/listkeys/Action | Alle IotHub-sleutels ophalen |
> | Bewerking | Microsoft.Devices/IotHubs/logDefinitions/read | De beschikbare logboekdefinities voor de IotHub-Service opgehaald |
> | Bewerking | Microsoft.Devices/IotHubs/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/iotHubs/quotaMetrics/Read | Quotum metrische gegevens ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/Read | De IotHub-resources opgehaald |
> | Bewerking | Microsoft.Devices/iotHubs/routing/$testall/Action | Een bericht met alle bestaande Routes testen |
> | Bewerking | Microsoft.Devices/iotHubs/routing/$testnew/Action | Een bericht met een opgegeven test Route testen |
> | Bewerking | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Hiermee wordt de status van alle routering eindpunten voor een IotHub |
> | Bewerking | Microsoft.Devices/iotHubs/skus/Read | Geldige IotHub Skus ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/Write | Maken of bijwerken van de IotHub-bron |
> | Bewerking | Microsoft.Devices/operations/Read | Alle bewerkingen van de ResourceProvider ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Delete | Hiermee verwijdert u certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Verificatiecode genereren |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Read | Het certificaat opgehaald |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/verify/Action | Controleer de resource-certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Write | Maken of bijwerken van certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/Delete | IotDps bron verwijderen |
> | Bewerking | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Devices/provisioningServices/listkeys/Action | Alle IotDps-sleutels ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/logDefinitions/read | De beschikbare logboekdefinities voor de inrichting Service opgehaald |
> | Bewerking | Microsoft.Devices/provisioningServices/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de inrichting-service |
> | Bewerking | Microsoft.Devices/provisioningServices/ProvisioningServiceKeys/listkeys/Action | IotDps sleutels voor de naam van de sleutel ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/Read | Ophalen van IotDps |
> | Bewerking | Microsoft.Devices/provisioningServices/skus/Read | Geldige IotDps Skus ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/Write | IotDps resource maken |
> | Bewerking | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resourceprovider registreren en wordt het maken van resources met IotHub |
> | Bewerking | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resourceprovider registreren en wordt het maken van resources met IotHub |
> | Bewerking | Microsoft.Devices/usages/Read | Neem abonnement informatie over het gebruik voor deze provider. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DevTestLab/labCenters/delete | Lab centers verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labCenters/read | Lab centers lezen. |
> | Bewerking | Microsoft.DevTestLab/labCenters/write | Toevoegen of wijzigen van de testomgeving centers. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Lezen van azure resource manager-sjablonen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Genereert een ARM-sjabloon voor het opgegeven artefact, uploadt de vereiste bestanden naar een opslagaccount en valideert het gegenereerde artefact. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Lezen van artefacten. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/delete | Verwijderen van artefacten gegevensbronnen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/read | Artefacten bronnen lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/write | Toevoegen of wijzigen van artefacten bronnen. |
> | Bewerking | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Een willekeurige claimable virtuele machine in de testomgeving claimen. |
> | Bewerking | Microsoft.DevTestLab/labs/costs/read | Kosten worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/costs/write | Toevoegen of wijzigen van de kosten. |
> | Bewerking | Microsoft.DevTestLab/labs/CreateEnvironment/action | Virtuele machines maken in een testomgeving. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/delete | Verwijderen van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/read | Lezen van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/write | Toevoegen of wijzigen van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Het brongebruik lab exporteert naar een opslagaccount |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/delete | Verwijder de formules. |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/read | Lees de formules. |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules. |
> | Bewerking | Microsoft.DevTestLab/labs/galleryImages/read | Afbeeldingen worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Een URI genereren voor het aangepaste schijfkopieën uploaden naar een testomgeving. |
> | Bewerking | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Een virtuele machine in een andere testomgeving importeren. |
> | Bewerking | Microsoft.DevTestLab/labs/ListVhds/action | Lijst van installatiekopieën van de schijven beschikbaar zijn voor het maken van aangepaste installatiekopie. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/delete | Notificationchannels verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Melding verzonden naar opgegeven kanaal. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/read | Notificationchannels lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/write | Toevoegen of wijzigen van notificationchannels. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Lab beleid evalueert. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/delete | Beleid verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/read | Lezen van beleid. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/write | Toevoegen of wijzigen van beleid. |
> | Bewerking | Microsoft.DevTestLab/labs/read | Labs lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/delete | Verwijderen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/Execute/action | Een schema worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Geeft een lijst van alle toepasselijke schema 's |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/delete | Service uitlopers verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/read | Service uitlopers lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/write | Toevoegen of wijzigen van de service uitlopers. |
> | Bewerking | Microsoft.DevTestLab/labs/users/delete | Gebruikersprofielen verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/Attach/action | Koppelen en maken van de lease van de schijf voor de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/delete | Schijven worden verwijderd. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/Detach/action | Ontkoppel en de lease van de schijf die is gekoppeld aan de virtuele machine wordt verbroken. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/read | Schijven worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/write | Toevoegen of wijzigen van de schijven. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/delete | Verwijder omgevingen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/read | Lezen omgevingen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/read | Gebruikersprofielen worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/delete | Geheimen verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/read | Lezen van geheimen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/write | Toevoegen of wijzigen van geheimen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Verwijder de service fabrics. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Geeft een lijst van alle toepasselijke schema 's |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Service-fabrics lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Verwijderen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Een schema worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Start een service fabric. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Een service fabric stoppen |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Toevoegen of wijzigen van de service-fabrics. |
> | Bewerking | Microsoft.DevTestLab/labs/users/write | Toevoegen of wijzigen van gebruikersprofielen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Een nieuwe of bestaande gegevensschijf koppelen aan de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Artefacten van toepassing op virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Eigenaar worden van een bestaande virtuele machine |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/delete | Verwijder de virtuele machines. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Loskoppelen van de opgegeven schijf van de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Geeft een lijst van alle toepasselijke schema 's |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/read | Virtuele machines gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Een virtuele machine implementeren |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | De grootte van virtuele Machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Start opnieuw op een virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Verwijderen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Een schema worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Een virtuele machine start. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Een virtuele machine stoppen |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Het eigendom overdraagt van gegevensschijven van de virtuele machine aan uzelf |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Release-eigendom van een bestaande virtuele machine |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/write | Hiermee worden virtuele machines toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/delete | Verwijder de virtuele netwerken. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/read | Virtuele netwerken gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/write | Toevoegen of wijzigen van virtuele netwerken. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/delete | Groepen van de virtuele machine verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/read | Virtuele machine toepassingen worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/write | Toevoegen of wijzigen van de virtuele machine van toepassingen. |
> | Bewerking | Microsoft.DevTestLab/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.DevTestLab/locations/operations/read | Lees-en schrijfopdrachten. |
> | Bewerking | Microsoft.DevTestLab/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.DevTestLab/schedules/delete | Verwijderen van schema's. |
> | Bewerking | Microsoft.DevTestLab/schedules/Execute/action | Een schema worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/schedules/Retarget/action | Updates van een planning doelresource-id. |
> | Bewerking | Microsoft.DevTestLab/schedules/write | Toevoegen of wijzigen van schema's. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DocumentDB/databaseAccountNames/read | Controleert of de naam van beschikbaarheid. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Resourcegroep van een databaseaccount wijzigen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | De verzameling leest metrische definities. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Hiermee wordt de verzameling metrische gegevens gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Database-account partitie belangrijkste niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Database-account partitie niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Database-account partities in een verzameling lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Databaseaccount partitie niveau gebruiksregels lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Leest het gebruik van de verzameling. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Metrische definities van de database gelezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Hiermee wordt de database metrische gegevens gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Hiermee wordt het gebruik van de database gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/delete | Hiermee verwijdert u de database-accounts. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Failover prioriteiten van regio's van een databaseaccount wijzigen. Dit wordt gebruikt om handmatige failover uit te voeren |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | De verbindingsreeksen niet ophalen voor een databaseaccount |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Lijst met sleutels van een databaseaccount |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Leest de database account metrische definities. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/metrics/read | Hiermee wordt de database-account metrische gegevens gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Status van de asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Latentie metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/read | Percentielen van replicatielatentie lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Latentie metrische gegevens voor een specifieke bron en doel regio lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Latentie metrische gegevens voor een specifieke regio lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read | Hiermee wordt de beschikbare logboek catageries voor databaseaccount |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de database-Account |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/read | Een databaseaccount leest. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Leest de database alleen-lezen sleutels. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Leest de database alleen-lezen sleutels. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Sleutels van een databaseaccount draaien |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Hiermee wordt de regionale verzameling metrische gegevens gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Lezen van regionale database account partitie belangrijkste niveau metrische gegevens |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Lezen van regionale database account partitie niveau metrische gegevens |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Regionale database account partities in een verzameling lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Hiermee wordt de regio en database account metrische gegevens gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/usages/read | Het gebruik van de account van de database gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/write | Bijwerken van een database-accounts. |
> | Bewerking | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.DocumentDB waarschuwt dat VirtualNetwork of Subnet wordt verwijderd |
> | Bewerking | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Status van deleteVirtualNetworkOrSubnets asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/operationResults/read | Status van de asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/operations/read | Leesbewerkingen die beschikbaar zijn voor de Microsoft-DocumentDB  |
> | Bewerking | Microsoft.DocumentDB/register/action |  Registreer de provider van de resource Microsoft DocumentDB voor het abonnement |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DomainRegistration/checkDomainAvailability/Action | Controleer of een domein aanschaffen is |
> | Bewerking | Microsoft.DomainRegistration/domains/Delete | Een bestaand domein verwijdert. |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Eigenaar-id verwijderen |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Lijst met eigenaar-id 's |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Eigenaar-id ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Maken of bijwerken van id |
> | Bewerking | Microsoft.DomainRegistration/domains/operationresults/Read | De bewerking van een domein ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/Read | De lijst met domeinen ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/Read | Domein ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/renew/Action | Vernieuwen van een bestaand domein. |
> | Bewerking | Microsoft.DomainRegistration/domains/Write | Een nieuw domein toevoegen of een bestaande bijgewerkt |
> | Bewerking | Microsoft.DomainRegistration/generateSsoRequest/Action | Genereren van een aanvraag voor het aanmelden bij domein control center. |
> | Bewerking | Microsoft.DomainRegistration/listDomainRecommendations/Action | Ophalen van de lijst domein aanbevelingen op basis van trefwoorden |
> | Bewerking | Microsoft.DomainRegistration/operations/Read | Lijst van alle bewerkingen van het app service-domeinregistratie |
> | Bewerking | Microsoft.DomainRegistration/register/action | Registreer de provider van Microsoft Domains resource voor het abonnement |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Actie van de overeenkomst |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/Read | Toplevel domeinen ophalen |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/Read | Toplevel domein ophalen |
> | Bewerking | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Aankoop domeinobject valideren zonder deze te verzenden |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/read | Microsoft Dynamics AX 2012 R3 Evaluation-implementaties in een Microsoft Dynamics Lifecycle Services-project die deel uitmaken van een gebruiker weergeven |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/write | Microsoft Dynamics AX 2012 R3 evaluatie-implementatie in een project Microsoft Dynamics levenscyclus van Services die deel uitmaken van een gebruiker maken. Implementaties kunnen worden beheerd vanaf de Azure-beheerportal |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/connectors/read | Lezen van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/connectors/write | Maken en bijwerken van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/delete | Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van de gebruiker verwijderen |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/read | Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van een gebruiker weergeven |
> | Bewerking | Microsoft.DynamicsLcs/lcsprojects/write | Maken en bijwerken van de levenscyclus van Services van Microsoft Dynamics-projecten die deel uitmaken van de gebruiker. Alleen de naam en beschrijving van eigenschappen kunnen worden bijgewerkt. Het abonnement en de locatie die is gekoppeld aan het project kunnen niet worden bijgewerkt nadat het maken |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/delete | Een eventSubscription verwijderen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Volledige url in voor het gebeurtenisabonnement ophalen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor abonnementen opgehaald |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor gebeurtenisabonnementen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor eventSubscriptions |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/read | Een eventSubscription lezen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/write | Maken of bijwerken van een eventSubscription |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor onderwerpen opgehaald |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/register/action | Registreert de eventSubscription voor de resourceprovider EventGrid en wordt het maken van abonnementen gebeurtenis raster. |
> | Bewerking | Microsoft.EventGrid/topics/delete | Een onderwerp verwijderen |
> | Bewerking | Microsoft.EventGrid/topics/listKeys/action | Lijst met sleutels voor het onderwerp |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor onderwerpen opgehaald |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/topics/read | Een onderwerp lezen |
> | Bewerking | Microsoft.EventGrid/topics/regenerateKey/action | Opnieuw genereren van de sleutel voor het onderwerp |
> | Bewerking | Microsoft.EventGrid/topics/write | Maken of bijwerken van een onderwerp |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.EventHub/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.EventHub/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy. |
> | Bewerking | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Lijst met metrische gegevens van Cluster Resource beschrijvingen |
> | Bewerking | Microsoft.EventHub/clusters/read | De beschrijving van de clusterresource ophalen |
> | Bewerking | Microsoft.EventHub/clusters/write | De beschrijving van de clusterresource ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/delete | Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/write | Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/namespaces/Delete | De resource van een naamruimte verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Bewerking voor het bijwerken van de EventHub. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Bewerking voor het verwijderen van EventHub-autorisatieregels |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | De verbindingsreeks naar EventHub ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Haal de lijst van EventHub-autorisatieregels |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Autorisatieregels EventHub maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Bewerking ConsumerGroup bron verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Lijst met beschrijvingen van de Resource ConsumerGroup ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Maken of bijwerken ConsumerGroup eigenschappen. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/Delete | Bewerking EventHub-bron verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/read | Lijst met beschrijvingen van EventHub-Resource |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/write | Maken of bijwerken EventHub-eigenschappen. |
> | Bewerking | Microsoft.EventHub/namespaces/messagingPlan/read | Hiermee wordt de Messaging plannen voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.EventHub/namespaces/messagingPlan/write | De Messaging plannen voor een naamruimte-updates.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.EventHub/namespaces/operationresults/read | De status van de naamruimtebewerking ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met Namespace logboeken Resource beschrijvingen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met Namespace metrische Resource beschrijvingen |
> | Bewerking | Microsoft.EventHub/namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/write | Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.EventHub/register/action | Hiermee wordt het abonnement voor de EventHub-resourceprovider geregistreerd en wordt het maken van EventHub-resources mogelijk |
> | Bewerking | Microsoft.EventHub/sku/read | Lijst met beschrijvingen van de Resource-Sku ophalen |
> | Bewerking | Microsoft.EventHub/sku/regions/read | Lijst met beschrijvingen van de Resource SkuRegions ophalen |
> | Bewerking | Microsoft.EventHub/unregister/action | Hiermee wordt de EventHub-resourceprovider geregistreerd |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Features/features/read | Hiermee kunt u de functies van een abonnement ophalen. |
> | Bewerking | Microsoft.Features/operations/read | Hiermee haalt u de lijst met bewerkingen op. |
> | Bewerking | Microsoft.Features/providers/features/read | Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider ophalen. |
> | Bewerking | Microsoft.Features/providers/features/register/action | Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider registreren. |
> | Bewerking | Microsoft.Features/providers/features/unregister/action | Hiermee kunt u de registratie van de functie van een abonnement voor een opgegeven resourceprovider ongedaan maken. |
> | Bewerking | Microsoft.Features/register/action | Hiermee registreert u de functie van een abonnement. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.HDInsight/clusters/changerdpsetting/action | RDP-instelling voor HDInsight-Cluster wijzigen |
> | Bewerking | Microsoft.HDInsight/clusters/configurations/action | Werk de configuratie van de HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/configurations/read | HDInsight-Cluster configuraties ophalen |
> | Bewerking | Microsoft.HDInsight/clusters/delete | Een HDInsight-Cluster verwijderen |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling voor de HDInsight-Cluster van de resource opgehaald |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instelling voor de resource HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/read | Meer informatie over de HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/roles/resize/action | Een HDInsight-Cluster vergroten of verkleinen |
> | Bewerking | Microsoft.HDInsight/clusters/write | Maken of bijwerken van HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/locations/capabilities/read | Ophalen van abonnement mogelijkheden |
> | Bewerking | Microsoft.HDInsight/locations/checkNameAvailability/read | Beschikbaarheid van de naam controleren |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ImportExport/jobs/delete | Hiermee verwijdert u een bestaande taak. |
> | Bewerking | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Hiermee haalt u de BitLocker-sleutels voor de gespecificeerde taak. |
> | Bewerking | Microsoft.ImportExport/jobs/read | Hiermee wordt de eigenschappen voor de opgegeven taak of de lijst met taken geretourneerd. |
> | Bewerking | Microsoft.ImportExport/jobs/write | Maakt een taak met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de gespecificeerde taak. |
> | Bewerking | Microsoft.ImportExport/locations/read | Haalt u de eigenschappen voor de opgegeven locatie of de lijst met locaties geretourneerd. |
> | Bewerking | Microsoft.ImportExport/register/action | Registreert het abonnement voor de resourceprovider voor importeren/exporteren en wordt het maken van de taken voor importeren/exporteren. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Insights/ActionGroups/Delete | Een actiegroep verwijderen |
> | Bewerking | Microsoft.Insights/ActionGroups/Read | Een actiegroep lezen |
> | Bewerking | Microsoft.Insights/ActionGroups/Write | Een actiegroep schrijven |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Waarschuwing voor een activiteitenlogboek geactiveerd |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Delete | Een waarschuwing voor een activiteitenlogboek verwijderen |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Read | Een waarschuwing voor een activiteitenlogboek lezen |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Write | Een waarschuwing voor een activiteitenlogboek lezen |
> | Bewerking | Microsoft.Insights/AlertRules/Activated/Action | Waarschuwingsregel geactiveerd |
> | Bewerking | Microsoft.Insights/AlertRules/Delete | Configuratie van een waarschuwingsregel verwijderen |
> | Bewerking | Microsoft.Insights/AlertRules/Incidents/Read | Incidentconfiguratie van een waarschuwingsregel lezen |
> | Bewerking | Microsoft.Insights/AlertRules/Read | Configuratie van een waarschuwingsregel lezen |
> | Bewerking | Microsoft.Insights/AlertRules/Resolved/Action | Waarschuwingsregel opgelost |
> | Bewerking | Microsoft.Insights/AlertRules/Throttled/Action | De waarschuwingsregel wordt beperkt |
> | Bewerking | Microsoft.Insights/AlertRules/Write | Naar de configuratie van een waarschuwingsregel schrijven |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Delete | Configuratie-instelling voor automatisch schalen verwijderen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Read | Configuratie-instelling voor automatisch schalen lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Bewerking Omlaag schalen via automatisch schalen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Bewerking Omhoog schalen via automatisch schalen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Write | Naar een configuratie-instelling voor automatisch schalen schrijven |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Delete | Een analyse-item van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Read | Een analyse-item van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Write | Een analyse-item van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Action | Analysetabelactie voor Application Insights |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Delete | Een analysetabelschema van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Read | Een analysetabelschema van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Write | Een analysetabelschema van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/Annotations/Delete | Een Application Insights-aantekening verwijderen |
> | Bewerking | Microsoft.Insights/Components/Annotations/Read | Een Application Insights-aantekening lezen |
> | Bewerking | Microsoft.Insights/Components/Annotations/Write | Een Application Insights-aantekening schrijven |
> | Bewerking | Microsoft.Insights/Components/Api/Read | De gegevens-API van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Action | Een API-sleutel van Application Insights genereren |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Delete | Een API-sleutel van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Read | Een API-sleutel van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/BillingPlanForComponent/Read | Een abonnement van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/CurrentBillingFeatures/Read | De huidige factureringsfuncties voor een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/CurrentBillingFeatures/Write | De huidige factureringsfuncties voor een Application Insights-onderdeel schrijven |
> | Bewerking | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | De standaardconfiguratie voor ALM-integratie van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/Delete | De configuratie van een Application Insights-onderdeel verwijderen |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Action | Exportinstellingenactie voor Application Insights |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Delete | De exportinstellingen van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Read | De exportinstellingen van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Write | De exportinstellingen van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/ExtendQueries/Read | De uitgebreide queryresultaten van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Favorites/Delete | Een Application Insights-favoriet verwijderen |
> | Bewerking | Microsoft.Insights/Components/Favorites/Read | Een Application Insights-favoriet lezen |
> | Bewerking | Microsoft.Insights/Components/Favorites/Write | Een Application Insights-favoriet schrijven |
> | Bewerking | Microsoft.Insights/Components/FeatureCapabilities/Read | De functiemogelijkheden van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | De beschikbare factureringsfuncties van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/GetToken/Read | Het token van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/MetricDefinitions/Read | De metrische definities van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Metrics/Read | De metrische gegevens van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Move/Action | Een Application Insights-onderdeel verplaatsen naar een andere resourcegroep of een ander abonnement |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Een persoonlijk analyse-item van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Read | Een persoonlijk analyse-item van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Write | Een persoonlijk analyse-item van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/MyFavorites/Read | Een persoonlijke favoriet van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/PricingPlans/Read | Het prijsplan van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/PricingPlans/Write | Het prijsplan van een Application Insights-onderdeel schrijven |
> | Bewerking | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | De Proactive Detection-configuratie van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | De Proactive Detection-configuratie van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/Components/QuotaStatus/Read | De quotumstatus van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Read | De configuratie van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | De webtestlocaties van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Delete | De configuratie voor ALM-integratie van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Read | De configuratie voor ALM-integratie van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Write | De configuratie voor ALM-integratie van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/Write | Schrijven naar de configuratie van een Application Insights-onderdeel |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Delete | Configuratie voor de diagnostische gegevens verwijderen |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Read | Een configuratie voor de diagnostische instellingen lezen |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Write | Schrijven naar de configuratie voor de diagnostische instellingen |
> | Bewerking | Microsoft.Insights/EventCategories/Read | Een gebeurteniscategorie lezen |
> | Bewerking | Microsoft.Insights/eventtypes/digestevents/Read | Samenvatting beheergebeurtenistype lezen |
> | Bewerking | Microsoft.Insights/eventtypes/values/Read | Waarden beheergebeurtenistype lezen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Configuratie voor de uitgebreide diagnostische instellingen verwijderen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Configuratie voor de uitgebreide diagnostische instellingen lezen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Configuratie voor de uitgebreide diagnostische instellingen schrijven |
> | Bewerking | Microsoft.Insights/ListMigrationDate/Action | Migratiedatum van abonnement terughalen |
> | Bewerking | Microsoft.Insights/ListMigrationDate/Read | Migratiedatum van abonnement terughalen |
> | Bewerking | Microsoft.Insights/LogDefinitions/Read | Logboekdefinities lezen |
> | Bewerking | Microsoft.Insights/LogProfiles/Delete | Configuratie voor logboekprofielen verwijderen |
> | Bewerking | Microsoft.Insights/LogProfiles/Read | Logboekprofielen lezen |
> | Bewerking | Microsoft.Insights/LogProfiles/Write | Schrijven naar een configuratie voor een logboekprofiel |
> | Bewerking | Microsoft.Insights/MetricAlerts/Delete | Een waarschuwing voor metrische gegevens verwijderen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Read | Een waarschuwing voor metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Status/Read | Status van waarschuwing voor metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Write | Een waarschuwing voor metrische gegevens schrijven |
> | Bewerking | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/Metrics/providers/Metrics/Read | De metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/Metrics/Read | De metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/Metrics/Write | Metrische gegevens schrijven |
> | Bewerking | Microsoft.Insights/MigrateToNewpricingModel/Action | Abonnement migreren naar nieuw prijsmodel |
> | Bewerking | Microsoft.Insights/Operations/Read | De bewerkingen lezen |
> | Bewerking | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registeren |
> | Bewerking | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Abonnement terugdraaien naar oude prijsmodel |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Delete | Een geplande queryregel verwijderen |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Read | Een geplande queryregel lezen |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Write | Een geplande queryregel schrijven |
> | Bewerking | Microsoft.Insights/Tenants/Register/Action | Hiermee wordt de Microsoft Insights-provider geïnitialiseerd |
> | Bewerking | Microsoft.Insights/Unregister/Action | De Microsoft Insights-provider registeren |
> | Bewerking | Microsoft.Insights/Webtests/Delete | Een configuratie voor een webtest verwijderen |
> | Bewerking | Microsoft.Insights/Webtests/GetToken/Read | Een webtesttoken lezen |
> | Bewerking | Microsoft.Insights/Webtests/MetricDefinitions/Read | De metrische definities van een webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Metrics/Read | De metrische gegevens van een webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Read | Een configuratie voor een webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Write | Schrijven naar een configuratie voor een webtest |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.KeyVault/checkNameAvailability/read | Hiermee wordt gecontroleerd of de opgegeven sleutelkluisnaam geldig is en niet wordt gebruikt |
> | Bewerking | Microsoft.KeyVault/deletedVaults/read | De eigenschappen van voorlopig verwijderde sleutelkluizen weergeven |
> | Bewerking | Microsoft.KeyVault/hsmPools/delete | Een HSM-groep verwijderen |
> | Bewerking | Microsoft.KeyVault/hsmPools/joinVault/action | Deelnemen aan een sleutelkluis voor een HSM-groep |
> | Bewerking | Microsoft.KeyVault/hsmPools/read | De eigenschappen van een HSM-groep weergeven |
> | Bewerking | Microsoft.KeyVault/hsmPools/write | Een nieuwe HSM-groep maken of de eigenschappen van een bestaande HSM-groep bijwerken |
> | Bewerking | Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sluitelkluis leegmaken |
> | Bewerking | Microsoft.KeyVault/locations/deletedVaults/read | De eigenschappen van voorlopig verwijderde sleutelkluis weergeven |
> | Bewerking | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.KeyVault doorgegeven dat een virtueel netwerk of subnet wordt verwijderd |
> | Bewerking | Microsoft.KeyVault/locations/operationResults/read | Het resultaat van een langdurige uitvoeringsbewerking controleren |
> | Bewerking | Microsoft.KeyVault/operations/read | Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.KeyVault-resourceprovider |
> | Bewerking | Microsoft.KeyVault/register/action | Hiermee wordt een abonnement geregistreerd |
> | Bewerking | Microsoft.KeyVault/unregister/action | Hiermee wordt de registratie van een abonnement opgeheven |
> | Bewerking | Microsoft.KeyVault/vaults/accessPolicies/write | Een bestaand toegangsbeleid bijwerken door samenvoegen of vervangen, of een nieuw toegangsbeleid toevoegen aan een kluis. |
> | Bewerking | Microsoft.KeyVault/vaults/delete | Een sleutelkluis verwijderen |
> | Bewerking | Microsoft.KeyVault/vaults/deploy/action | Hiermee wordt toegang tot de geheimen in een sleutelkluis ingeschakeld tijdens het implementeren van Azure-resources |
> | Bewerking | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschikbare logboeken voor een sleutelkluis opgehaald |
> | Bewerking | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens voor een sleutelkluis opgehaald |
> | Bewerking | Microsoft.KeyVault/vaults/read | De eigenschappen van een sleutelkluis weergeven |
> | Bewerking | Microsoft.KeyVault/vaults/secrets/read | De eigenschappen van een geheim weergeven, maar niet de waarde |
> | Bewerking | Microsoft.KeyVault/vaults/secrets/write | Een nieuw geheim maken of de waarde van een bestaand geheim bijwerken |
> | Bewerking | Microsoft.KeyVault/vaults/write | Een nieuwe sleutelkluis maken of de eigenschappen van een bestaande sleutelkluis bijwerken |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.LabServices/labAccounts/CreateLab/action | Een lab maken in een lab-account. |
> | Bewerking | Microsoft.LabServices/labAccounts/delete | Lab-accounts te verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Claims van een willekeurige omgeving voor een gebruiker in een omgevingsinstellingen |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Omgevingsinstelling verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Claims van de omgeving en toegewezen aan de gebruiker |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Verwijder omgevingen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Lezen omgevingen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Een omgeving begint met het starten van alle resources binnen de omgeving. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Een omgeving stopt met het stoppen van alle resources binnen de omgeving |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Bepalingen/deprovisions vereiste resources voor een omgeving instellen op basis van de huidige status van de testomgeving /-instelling. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Omgevingsinstelling gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Start een sjabloon op basis van alle resources in de sjabloon. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Start een sjabloon op basis van alle resources in de sjabloon. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Toevoegen of wijzigen van de omgeving. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/read | Labs lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/Register/action | Registreren bij beheerde lab. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/delete | Gebruikers verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/read | Gebruikers worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/write | Toevoegen of wijzigen van gebruikers. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.LabServices/labAccounts/read | Lab accounts lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sizes/read | Grootten worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/write | Toevoegen of wijzigen van accounts lab. |
> | Bewerking | Microsoft.LabServices/locations/operations/read | Lees-en schrijfopdrachten. |
> | Bewerking | Microsoft.LabServices/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.LabServices/users/GetEnvironment/action | De details van de virtuele machine opgehaald |
> | Bewerking | Microsoft.LabServices/users/ListEnvironments/action | Lijst omgevingen voor de gebruiker |
> | Bewerking | Microsoft.LabServices/users/ListLabs/action | Lijst labs voor de gebruiker. |
> | Bewerking | Microsoft.LabServices/users/Register/action | Registreren van een gebruiker aan een beheerde lab |
> | Bewerking | Microsoft.LabServices/users/StartEnvironment/action | Een omgeving begint met het starten van alle resources binnen de omgeving. |
> | Bewerking | Microsoft.LabServices/users/StopEnvironment/action | Een omgeving stopt met het stoppen van alle resources binnen de omgeving |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.LocationBasedServices/accounts/delete | (Afgeschaft: gebruik /providers/Microsoft.Maps) Verwijderen van een locatie op basis van Services-Account. |
> | Bewerking | Microsoft.LocationBasedServices/accounts/listKeys/action | (Afgeschaft: gebruik /providers/Microsoft.Maps) Lijst met Services-Account van locatie op basis van sleutels |
> | Bewerking | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | (Afgeschaft: gebruik /providers/Microsoft.Maps) De diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | (Afgeschaft: gebruik /providers/Microsoft.Maps) Maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | (Afgeschaft: gebruik /providers/Microsoft.Maps) Hiermee wordt de beschikbare metrische gegevens voor de locatie op basis van serviceaccounts |
> | Bewerking | Microsoft.LocationBasedServices/accounts/read | (Afgeschaft: gebruik /providers/Microsoft.Maps) Ophalen van een locatie op basis van Services-Account. |
> | Bewerking | Microsoft.LocationBasedServices/accounts/regenerateKey/action | (Afgeschaft: gebruik /providers/Microsoft.Maps) Nieuwe locatie op basis van Services-Account primaire of secundaire sleutel genereren |
> | Bewerking | Microsoft.LocationBasedServices/accounts/write | (Afgeschaft: gebruik /providers/Microsoft.Maps) Maken of bijwerken van een locatie op basis van Services-Account. |
> | Bewerking | Microsoft.LocationBasedServices/register/action | (Afgeschaft: gebruik /providers/Microsoft.Maps) Registreer de provider |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/delete | Hiermee verwijdert u de overeenkomst in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor overeenkomstinhoud in het integratieaccount op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/read | Hiermee leest u de overeenkomst in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/write | Hiermee maakt u de overeenkomst in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/delete | Hiermee verwijdert u de assembly in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor assemblyinhoud in het integratieaccount op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/read | Hiermee leest u de assembly in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/write | Hiermee maakt u de assembly in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Hiermee verwijdert u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Hiermee maakt u de batchconfiguratie in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/delete | Hiermee verwijdert u het certificaat in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/read | Hiermee leest u het certificaat in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/write | Hiermee maakt u het certificaat in het integratieaccount of werkt u het bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/delete | Hiermee verwijdert u het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/join/action | Hiermee wordt het integratieaccount gekoppeld. |
> | Bewerking | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor het integratieaccount op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Hiermee haalt u de sleutels in de sleutelkluis op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Hiermee registreert u traceringsgebeurtenissen in het integratieaccount in het logboek. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/delete | Hiermee verwijdert u de kaart in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor kaartinhoud in het integratieaccount op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/read | Hiermee leest u de kaart in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/write | Hiermee maakt u de kaart in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/delete | Hiermee verwijdert u de partner in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Hiermee geeft u de URL voor terugbellen voor partnerinhoud in het integratieaccount weer. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/read | Hiermee leest u de partner in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/write | Hiermee maakt u de partner in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de logboekdefinities van het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/read | Hiermee leest u het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/delete | Hiermee verwijdert u het schema in het integratie-account. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Hiermee haalt u de URL voor terugbellen voor schema-inhoud in het integratieaccount op. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/read | Hiermee leest u het schema in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/write | Hiermee maakt u het schema in het integratieaccount of werkt u het bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/delete | Hiermee verwijdert u de sessie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/read | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/write | Hiermee maakt u de sessie in het integratieaccount of werkt u deze bij. |
> | Bewerking | Microsoft.Logic/integrationAccounts/write | Hiermee maakt u het integratieaccount of werkt u het bij. |
> | Bewerking | Microsoft.Logic/locations/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
> | Bewerking | Microsoft.Logic/operations/read | Hiermee haalt u de bewerking op. |
> | Bewerking | Microsoft.Logic/register/action | Hiermee registreert u de Microsoft.Logic-resourceprovider voor een bepaald abonnement. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/delete | Hiermee wordt de toegangssleutel verwijderd. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/list/action | Hiermee wordt een lijst met toegangssleutelgeheimen gemaakt. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/read | Hiermee wordt de toegangssleutel gelezen. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/regenerate/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/write | Hiermee wordt de toegangssleutel opnieuw gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Logic/workflows/delete | Hiermee wordt de werkstroom verwijderd. |
> | Bewerking | Microsoft.Logic/workflows/disable/action | Hiermee wordt de werkstroom uitgeschakeld. |
> | Bewerking | Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
> | Bewerking | Microsoft.Logic/workflows/listCallbackUrl/action | Hiermee wordt de URL voor terugbellen van de werkstroom opgehaald. |
> | Bewerking | Microsoft.Logic/workflows/listSwagger/action | Hiermee worden de swaggerdefinities van de werkstroom opgehaald. |
> | Bewerking | Microsoft.Logic/workflows/move/action | Hiermee wordt de werkstroom verplaatst van de bestaande abonnements-id, resourcegroep en/of naam naar een andere abonnements-id, resourcegroep en/of naam. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen van de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee worden de diagnostische instelling van de werkstroom gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de logboekdefinities van de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de metrische definities van de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/read | Hiermee wordt de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/regenerateAccessKey/action | Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd. |
> | Bewerking | Microsoft.Logic/workflows/run/action | Hiermee wordt de werkstroomuitvoering gestart. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Hiermee worden de expressietraceringen voor de uitvoeringsactie van de werkstroom opgehaald. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/read | Hiermee wordt de uitvoeringsactie van de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Hiermee haalt u de expressietraceringen voor de herhaling van de uitvoeringsactie van de werkstroom op. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/repetitions/read | Hiermee leest u de herhaling van de uitvoeringsactie van de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Hiermee leest u de herhaling van het bereik van de uitvoeringsactie van de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/runs/cancel/action | Hiermee wordt de werkstroomuitvoering geannuleerd. |
> | Bewerking | Microsoft.Logic/workflows/runs/operations/read | Hiermee wordt de bewerkingsstatus van de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/read | Hiermee wordt de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/suspend/action | Hiermee onderbreekt u de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/triggers/histories/read | Hiermee wordt de geschiedenis van de triggers gelezen. |
> | Bewerking | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Hiermee wordt de werkstroomtrigger opnieuw verzonden. |
> | Bewerking | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Hiermee wordt de URL voor terugbellen van de trigger opgehaald. |
> | Bewerking | Microsoft.Logic/workflows/triggers/read | Hiermee wordt de trigger gelezen. |
> | Bewerking | Microsoft.Logic/workflows/triggers/reset/action | Hiermee stelt u de trigger opnieuw in. |
> | Bewerking | Microsoft.Logic/workflows/triggers/run/action | Hiermee wordt de trigger uitgevoerd. |
> | Bewerking | Microsoft.Logic/workflows/triggers/setState/action | Hiermee stelt u de status van de trigger in. |
> | Bewerking | Microsoft.Logic/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
> | Bewerking | Microsoft.Logic/workflows/versions/read | Hiermee wordt de werkstroomversie gelezen. |
> | Bewerking | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Hiermee wordt de URL voor terugbellen van de trigger opgehaald. |
> | Bewerking | Microsoft.Logic/workflows/write | Hiermee wordt de werkstroom gemaakt of bijgewerkt. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Verplaatsen van een Machine Learning-streven Plan koppeling |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Lezen van een Machine Learning-streven Plan koppeling |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/delete | Een Machine streven trainingsplan verwijderen |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/join/action | Lid worden van een Machine streven trainingsplan |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/read | Een Machine streven trainingsplan lezen |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/write | Maken of bijwerken van een Machine Learning streven plannen |
> | Bewerking | Microsoft.MachineLearning/locations/operationresults/read | Resultaat van een Machine Learning-bewerking ophalen |
> | Bewerking | Microsoft.MachineLearning/locations/operationsstatus/read | Status van een actieve Machine Learning-bewerking ophalen |
> | Bewerking | Microsoft.MachineLearning/operations/read | Machine Learning-bewerkingen ophalen |
> | Bewerking | Microsoft.MachineLearning/register/action | Het abonnement voor de machine learning web service resourceprovider registreert en wordt het maken van webservices. |
> | Bewerking | Microsoft.MachineLearning/skus/read | Machine Learning-streven Plan SKU's ophalen |
> | Bewerking | Microsoft.MachineLearning/webServices/action | Regionale Web Service-eigenschappen voor ondersteunde regio's maken |
> | Bewerking | Microsoft.MachineLearning/webServices/delete | Verwijderen van een Machine Learning-webservice |
> | Bewerking | Microsoft.MachineLearning/webServices/listkeys/read | Een Machine Learning-webservice-sleutels ophalen |
> | Bewerking | Microsoft.MachineLearning/webServices/read | Lezen van een Machine Learning-webservice |
> | Bewerking | Microsoft.MachineLearning/webServices/write | Maken of bijwerken van een Machine Learning-webservice |
> | Bewerking | Microsoft.MachineLearning/Workspaces/delete | Verwijderen van een Machine Learning-werkruimte |
> | Bewerking | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Lijst met sleutels voor een Machine Learning-werkruimte |
> | Bewerking | Microsoft.MachineLearning/Workspaces/read | Lezen van een Machine Learning-werkruimte |
> | Bewerking | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Sleutels van opslagaccount geconfigureerd voor een Machine Learning-werkruimte synchroniseren |
> | Bewerking | Microsoft.MachineLearning/Workspaces/write | Een Machine Learning-werkruimte gemaakt of bijgewerkt |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Controleer of er updates beschikbaar voor systeemservices voor het cluster uitoefening zijn |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Hosting-account verwijderen |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Lijst met sleutels die zijn gekoppeld aan uitoefening cluster |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/read | Een hosting-account lezen |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | De systeemservices in een cluster uitoefening bijwerken |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/write | Hosting-account maken of bijwerken |
> | Bewerking | Microsoft.MachineLearningCompute/register/action | Registreert de abonnements-ID voor de resourceprovider en wordt het maken van een machine learning rekenresources |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/delete | Hosting-account verwijderen |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/read | Een hosting-account lezen |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/write | Hosting-account maken of bijwerken |
> | Bewerking | Microsoft.MachineLearningModelManagement/register/action | Registreert de abonnements-ID voor de resourceprovider en wordt het maken van een hosting-account |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningServices/register/action | Registreert het abonnement voor de Resourceprovider voor Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/delete | Hiermee verwijdert u de rekenresources in Machine Learning Services Workspace(s) |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Lijst van geheimen rekenresources in de werkruimte van Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/read | Hiermee haalt u de rekenresources in Machine Learning Services Workspace(s) |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/write | Maken of bijwerken van de rekenresources in Machine Learning Services Workspace(s) |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/delete | Hiermee verwijdert u de Machine Learning-Services Workspace(s) |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/listKeys/action | Lijst met geheimen voor een Machine Learning-werkruimte Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/read | De Machine Learning-Services Workspace(s) opgehaald |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/write | Maken of bijwerken van een Machine Learning Services Workspace(s) |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC-actie voor het toewijzen van een bestaande gebruiker identiteit toegewezen aan een resource |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Hiermee verwijdert u een bestaande gebruiker toegewezen identiteit |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/read | Een bestaande gebruiker toegewezen identiteit opgehaald |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/write | Een nieuwe gebruiker toegewezen identiteit maken of bijwerken van de labels van een bestaande gebruiker toegewezen identiteit |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ManagedLab/labAccounts/CreateLab/action | Een lab maken in een lab-account. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/delete | Lab-accounts te verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Omgevingsinstelling verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Verwijder omgevingen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Lezen omgevingen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Omgevingsinstelling gelezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Toevoegen of wijzigen van de omgeving. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Verwijder de virtuele machines. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/read | De virtuele machines gelezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Toevoegen of wijzigen van de virtuele machines. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/read | Labs lezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/read | Lab accounts lezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/write | Toevoegen of wijzigen van accounts lab. |
> | Bewerking | Microsoft.ManagedLab/locations/operations/read | Lees-en schrijfopdrachten. |
> | Bewerking | Microsoft.ManagedLab/register/action | Hiermee wordt het abonnement geregistreerd |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Management/checkNameAvailability/action | Controleert of de naam van de opgegeven beheergroep geldig en uniek is. |
> | Bewerking | Microsoft.Management/getEntities/action | Lijst van alle entiteiten (beheergroepen, abonnementen, enz.) voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.Management/managementGroups/delete | Verwijder de beheergroep. |
> | Bewerking | Microsoft.Management/managementGroups/read | Lijst met beheergroepen voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.Management/managementGroups/subscriptions/delete | Abonnement van de beheergroep koppelt ongedaan. |
> | Bewerking | Microsoft.Management/managementGroups/subscriptions/write | Gekoppeld bestaande abonnement met de beheergroep. |
> | Bewerking | Microsoft.Management/managementGroups/write | Maken of bijwerken van een beheergroep. |
> | Bewerking | Microsoft.Management/register/action | Het opgegeven abonnement registreren bij Microsoft.Management |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Maps/accounts/delete | Een Maps-Account verwijderen. |
> | Bewerking | Microsoft.Maps/accounts/listKeys/action | Lijst met sleutels Maps |
> | Bewerking | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Maps/accounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor Maps-Accounts |
> | Bewerking | Microsoft.Maps/accounts/read | Maak een Maps-Account. |
> | Bewerking | Microsoft.Maps/accounts/regenerateKey/action | Nieuwe Maps-Account primaire of secundaire sleutel genereren |
> | Bewerking | Microsoft.Maps/accounts/write | Maken of bijwerken van een Maps-Account. |
> | Bewerking | Microsoft.Maps/register/action | Registreer de provider |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Retourneert een overeenkomst. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Een ondertekende overeenkomst accepteert. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Een afbeelding voor de eindgebruiker ACR geïmporteerd. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Retourneert een configuratie. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Hiermee slaat u een configuratie. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/delete | Biedt een DELETE-bewerking op de bron van een klassieke dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Hiermee haalt u een klassieke dev sleutels voor resource management service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Hiermee wordt de eenmalige aanmelding op URL voor een klassieke dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/read | Biedt een GET-bewerking voor een klassieke dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Genereert een klassieke dev service resource management-sleutels. |
> | Bewerking | Microsoft.MarketplaceApps/Operations/read | Leesbewerkingen voor alle resourcetypen. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Annuleren van een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Retourneren van een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Meld u aan een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/read | Retourneren van alle overeenkomsten onder opgegeven abonnement |
> | Bewerking | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Een overeenkomst voor een bepaalde marketplace-item voor virtuele machine ophalen |
> | Bewerking | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Meld u aan of een overeenkomst voor een item van de virtuele machine opgegeven marketplace annuleren |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Media/checknameavailability/action | Controleert of de naam van een Media Services-account beschikbaar is |
> | Bewerking | Microsoft.Media/locations/checkNameAvailability/action | Controleert of de naam van een Media Services-account beschikbaar is |
> | Bewerking | Microsoft.Media/mediaservices/assets/delete | Een willekeurige Asset verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Asset coderingssleutel ophalen |
> | Bewerking | Microsoft.Media/mediaservices/assets/listContainerSas/action | Lijst met activa Container SAS-URL 's |
> | Bewerking | Microsoft.Media/mediaservices/assets/read | Een willekeurige Asset lezen |
> | Bewerking | Microsoft.Media/mediaservices/assets/write | Maken of bijwerken van een willekeurige Asset |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Een inhoud sleutel beleid verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Eigenschappen van het beleid met geheimen ophalen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/read | Een sleutel-beleid van de inhoud lezen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/write | Alle inhoud sleutel beleid maken of bijwerken |
> | Bewerking | Microsoft.Media/mediaservices/delete | Een Media Services-Account verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/delete | Een raster gebeurtenisfilter verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/read | Een raster gebeurtenisfilter lezen |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/write | Maken of bijwerken van een raster gebeurtenisfilter |
> | Bewerking | Microsoft.Media/mediaservices/liveEventOperations/read | De leesbewerking van een Live gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/delete | Verwijderen van een Live gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Live uitvoer verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Live uitvoer lezen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Maken of bijwerken van Live uitvoer |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/read | Lezen van een Live gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/reset/action | De bewerking van een Live gebeurtenis opnieuw instellen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/start/action | Een Live gebeurtenis-bewerking starten |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/stop/action | De bewerking van een Live gebeurtenis stoppen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/write | Maken of bijwerken van een Live gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveOutputOperations/read | De leesbewerking van een Live-uitvoer |
> | Bewerking | Microsoft.Media/mediaservices/read | Een Media Services-Account lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Een Streaming Endpoint leesbewerking |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/delete | Streaming-eindpunt verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/read | Lezen van een Streaming-eindpunt |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Een Streaming Endpoint-bewerking schalen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Een Streaming Endpoint-bewerking starten |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Een Streaming Endpoint-bewerking stoppen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/write | Maken of bijwerken van een Streaming-eindpunt |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/delete | Streaming-Locator verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Lijst met inhoud sleutels |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Lijst met paden |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/read | Streaming-Locator lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/write | Maken of bijwerken van een Streaming-Locator |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/delete | Een Streaming-beleid verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/read | Een Streaming-beleid lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/write | Een Streaming-beleid maken of bijwerken |
> | Bewerking | Microsoft.Media/mediaservices/syncStorageKeys/action | Synchroniseren van de sleutels van de opslag voor een gekoppelde Azure Storage-account |
> | Bewerking | Microsoft.Media/mediaservices/transforms/delete | Een transformatie verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Taak annuleren |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/delete | Elke taak verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/read | Elke taak lezen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/write | Maken of bijwerken van een taak |
> | Bewerking | Microsoft.Media/mediaservices/transforms/read | Lezen van een transformatie |
> | Bewerking | Microsoft.Media/mediaservices/transforms/write | Maken of bijwerken van een transformatie |
> | Bewerking | Microsoft.Media/mediaservices/write | Een Media Services-Account maken of bijwerken |
> | Bewerking | Microsoft.Media/operations/read | Beschikbare bewerkingen ophalen |
> | Bewerking | Microsoft.Media/register/action | Het abonnement voor de Media Services-resourceprovider geregistreerd en wordt het maken van Media Services-accounts |
> | Bewerking | Microsoft.Media/unregister/action | Heft de registratie van het abonnement voor de Media Services-resourceprovider |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Migrate/Operations/read | Leest de beschikbare bewerkingen |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Application Gateway Ssl vooraf gedefinieerd beleid |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Application Gateway beschikbare Ssl-opties |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Toepassingsgateway beschikbaar Waf regelsets opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Een application gateway back-end-adresgroep toevoegen |
> | Bewerking | Microsoft.Network/applicationGateways/backendhealth/action | Een back-end toepassingsstatus van de gateway opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/delete | Een toepassingsgateway verwijderen |
> | Bewerking | Microsoft.Network/applicationGateways/effectiveNetworkSecurityGroups/action | Routetabel geconfigureerd op de toepassingsgateway ophalen |
> | Bewerking | Microsoft.Network/applicationGateways/effectiveRouteTable/action | Routetabel geconfigureerd op de toepassingsgateway ophalen |
> | Bewerking | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de gebeurtenissen voor Application Gateway opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden de beschikbare metrische gegevens voor Application Gateway opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/read | Een application gateway opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/setSecurityCenterConfiguration/action | Sets Application Gateway Security Center-configuratie |
> | Bewerking | Microsoft.Network/applicationGateways/start/action | Een application gateway wordt gestart |
> | Bewerking | Microsoft.Network/applicationGateways/stop/action | Een application gateway wordt gestopt |
> | Bewerking | Microsoft.Network/applicationGateways/write | Een toepassingsgateway maken of bijwerken van een application gateway |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/delete | Hiermee verwijdert u de beveiligingsgroep van een toepassing |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Een IP-configuratie koppelt aan beveiligingsgroepen toepassing. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Een beveiligingsregel koppelt aan beveiligingsgroepen van toepassing. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/read | Opgehaald van een toepassing beveiligingsgroep-ID. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/write | Maakt u de beveiligingsgroep van een toepassing of updates van een bestaande beveiligingsgroep voor de toepassing. |
> | Bewerking | Microsoft.Network/bgpServiceCommunities/read | Bgp-Service-community's ophalen |
> | Bewerking | Microsoft.Network/checkTrafficManagerNameAvailability/action | Controleert de beschikbaarheid van een Traffic Manager ten opzichte van DNS-naam. |
> | Bewerking | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/connections/sharedkey/action | VirtualNetworkGatewayConnection SharedKey ophalen |
> | Bewerking | Microsoft.Network/connections/sharedKey/read | VirtualNetworkGatewayConnection SharedKey opgehaald |
> | Bewerking | Microsoft.Network/connections/sharedKey/write | Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection SharedKey |
> | Bewerking | Microsoft.Network/connections/vpndeviceconfigurationscript/action | VPN-apparaatconfiguratie van VirtualNetworkGatewayConnection opgehaald |
> | Bewerking | Microsoft.Network/connections/write | Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/delete | Hiermee verwijdert u een Proxy DDoS-beveiliging plannen |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/read | De definitie van een DDoS-beveiliging plannen Proxy ophalen |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/write | Maakt een DDoS-Proxy voor het plannen van beveiliging of updates en bestaande DDoS-beveiliging plannen Proxy |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/delete | Hiermee wordt een beveiligingsplan DDoS verwijderd |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/join/action | Lid wordt een beveiligingsplan DDoS |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/read | Een beveiligingsplan DDoS opgehaald |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/write | Maakt een DDoS-beveiliging plannen of updates van een DDoS-beveiliging plannen  |
> | Bewerking | Microsoft.Network/dnsoperationresults/read | Resultaten van een DNS-bewerking opgehaald |
> | Bewerking | Microsoft.Network/dnsoperationstatuses/read | Hiermee wordt de status van een DNS-bewerking opgehaald  |
> | Bewerking | Microsoft.Network/dnszones/A/delete | Verwijder de recordset met een bepaalde naam en typt u "A" uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/A/read | De recordset van het type "A", in JSON-indeling verkrijgen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/A/write | Maken of bijwerken van een recordset van het type "A" binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/delete | Verwijder de recordset met een bepaalde naam en typ 'AAAA' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/read | De recordset van het type 'AAAA', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/write | Maken of bijwerken van een recordset van het type 'AAAA' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/all/read | DNS-recordsets in verschillende typen opgehaald |
> | Bewerking | Microsoft.Network/dnszones/CAA/delete | Verwijder de recordset met een bepaalde naam en typ 'CAA' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/CAA/read | De recordset van het type 'CAA', in JSON-indeling ophalen. De recordset omvat de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/CAA/write | Maken of bijwerken van een recordset van het type 'CAA' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/delete | Verwijder de recordset met een bepaalde naam en typ 'CNAME' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/read | De recordset van het type 'CNAME', in JSON-indeling ophalen. De recordset omvat de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/write | Maken of bijwerken van een recordset van het type 'CNAME' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/delete | Verwijder de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets. |
> | Bewerking | Microsoft.Network/dnszones/MX/delete | Verwijder de recordset met een bepaalde naam en typ 'MX' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/MX/read | De recordset van het type 'MX', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/MX/write | Maken of bijwerken van een recordset van het type 'MX' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/NS/delete | Hiermee verwijdert u de DNS-recordset van het type NS |
> | Bewerking | Microsoft.Network/dnszones/NS/read | DNS-recordset van het type NS opgehaald |
> | Bewerking | Microsoft.Network/dnszones/NS/write | Maken of bijwerken van DNS-recordset van het type NS |
> | Bewerking | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instellingen voor de DNS-zone opgehaald |
> | Bewerking | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor DNS-zone |
> | Bewerking | Microsoft.Network/dnszones/providers/Microsoft.Insights/metricDefinitions/read | De DNS-zone metrische definities opgehaald |
> | Bewerking | Microsoft.Network/dnszones/PTR/delete | Verwijder de recordset met een bepaalde naam en typ 'PTR' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/PTR/read | De recordset van het type 'PTR', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/PTR/write | Maken of bijwerken van een recordset van het type 'PTR' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/read | Ophalen van de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets. Houd er rekening mee dat deze opdracht niet de recordsets in de zone ophaalt. |
> | Bewerking | Microsoft.Network/dnszones/recordsets/read | DNS-recordsets in verschillende typen opgehaald |
> | Bewerking | Microsoft.Network/dnszones/SOA/read | DNS-recordset van het type SOA opgehaald |
> | Bewerking | Microsoft.Network/dnszones/SOA/write | Maken of bijwerken van DNS-recordset van het type SOA |
> | Bewerking | Microsoft.Network/dnszones/SRV/delete | Verwijder de recordset met een bepaalde naam en typ 'SRV' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/SRV/read | De recordset van het type 'SRV', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/SRV/write | Maken of bijwerken van de recordset van het type SRV |
> | Bewerking | Microsoft.Network/dnszones/TXT/delete | Verwijder de recordset met een bepaalde naam en typ 'TXT' uit een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/TXT/read | De recordset van het type 'TXT', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/TXT/write | Maken of bijwerken van een recordset van het type 'TXT' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/write | Maken of bijwerken van een DNS-zone binnen een resourcegroep.  Gebruikt voor het bijwerken van de labels van een DNS-zone-resource. Houd er rekening mee dat deze opdracht kan niet worden gebruikt om te maken of bijwerken van recordsets binnen de zone. |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/delete | Hiermee verwijdert u een ExpressRouteCircuit-autorisatie |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/read | Een vergunning ExpressRouteCircuit opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/write | Maken of bijwerken van een bestaande ExpressRouteCircuit-autorisatie |
> | Bewerking | Microsoft.Network/expressRouteCircuits/delete | Een ExpressRouteCircuit verwijderen |
> | Bewerking | Microsoft.Network/expressRouteCircuits/join/action | Lid wordt van een Express Route-Circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Een ExpressRouteCircuit Peering ArpTable opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Hiermee verwijdert u een verbinding ExpressRouteCircuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Een verbinding ExpressRouteCircuit opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Maken of bijwerken van een bestaande ExpressRouteCircuit verbinding Resource |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/delete | Hiermee verwijdert u een ExpressRouteCircuit Peering |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/read | Een ExpressRouteCircuit Peering opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Een ExpressRouteCircuit Peering migratiestatus opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Een overzicht van ExpressRouteCircuit Peering migratiestatus opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Een ExpressRouteCircuit Peering statistieken opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/write | Maken of bijwerken van een bestaande Peering ExpressRouteCircuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instellingen voor ExpressRoute-Circuits opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van diagnostische instellingen voor ExpressRoute-Circuits |
> | Bewerking | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read | Ophalen van de gebeurtenissen voor ExpressRoute-Circuits |
> | Bewerking | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read | De metrische definities voor ExpressRoute-Circuits opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/read | Een ExpressRouteCircuit ophalen |
> | Bewerking | Microsoft.Network/expressRouteCircuits/stats/read | Een ExpressRouteCircuit Stat opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/write | Maken of bijwerken van een bestaande ExpressRouteCircuit |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/delete | Verwijderen van Express Route Cross-verbinding |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/join/action | Cross join een Express Route-verbinding |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Een Express Route kruislings verbinding Peering Arp-tabel opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Hiermee verwijdert u een Express Route kruislings verbinding Peering |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/read | Een Route snelle Cross-verbinding Peering opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Een Express Route kruislings verbinding Peering routetabel opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Een Express Route kruislings samenvattingstabel verbinding Peering Route opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/write | Maakt een Cross-verbinding Peering voor Express Route of een bestaande Cross verbinding Peering voor Express Route-Updates |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/read | Ophalen van Express Route Cross-verbinding |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/write | Maken of bijwerken Cross Express Route-verbinding |
> | Bewerking | Microsoft.Network/expressRouteServiceProviders/read | Express Route-serviceproviders opgehaald |
> | Bewerking | Microsoft.Network/intendedPolicies/delete | Hiermee verwijdert u een beoogde beleid |
> | Bewerking | Microsoft.Network/intendedPolicies/read | Een beschrijving van het gewenste beleid opgehaald |
> | Bewerking | Microsoft.Network/intendedPolicies/write | Een beleid bedoeld maken of bijwerken van een bestaand bedoeld beleid |
> | Bewerking | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een back-endadresgroep voor load balancer |
> | Bewerking | Microsoft.Network/loadBalancers/backendAddressPools/read | Een definitie van het groep back-end-adres van load balancer ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/delete | Hiermee verwijdert u een load balancer |
> | Bewerking | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Een definitie van load balancer frontend IP-configuratie ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid wordt van een load balancer binnenkomende nat-pool |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatPools/read | Een load balancer ophalen inkomende nat-pooldefinitie |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/delete | Een binnenkomende nat-regel van load balancer verwijderen |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/read | Een load balancer ophalen inkomende nat-regel definiëren |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/write | Een binnenkomende nat-regel van load balancer maken of bijwerken van een bestaande load balancer binnenkomende nat-regel |
> | Bewerking | Microsoft.Network/loadBalancers/loadBalancingRules/read | Een load balancer load taakverdeling ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/networkInterfaces/read | Verwijzingen naar alle netwerkinterfaces onder een load balancer ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/outboundNatRules/read | Een definitie voor uitgaande nat-regel van load balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van de tests van een load balancer. Bijvoorbeeld: met deze machtiging healthProbe-eigenschap van VM-scale set kan verwijzen naar de test. |
> | Bewerking | Microsoft.Network/loadBalancers/probes/read | Een load balancer-test opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen van de Load Balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen van de Load Balancer |
> | Bewerking | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de gebeurtenissen voor de Load Balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read | De beschikbare metrische gegevens voor de Load Balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/read | De definitie van een load balancer ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/virtualMachines/read | Verwijzingen naar alle virtuele machines onder een load balancer ophalen |
> | Bewerking | Microsoft.Network/loadBalancers/write | Een load balancer maken of bijwerken van een bestaande load balancer |
> | Bewerking | Microsoft.Network/localnetworkgateways/delete | Deletes LocalNetworkGateway |
> | Bewerking | Microsoft.Network/localnetworkgateways/read | Gets LocalNetworkGateway |
> | Bewerking | Microsoft.Network/localnetworkgateways/write | Maken of bijwerken van een bestaande LocalNetworkGateway |
> | Bewerking | Microsoft.Network/locations/availableDelegations/read | Beschikbare delegeringen opgehaald |
> | Bewerking | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Ondersteuning voor controles versnelde netwerken |
> | Bewerking | Microsoft.Network/locations/checkDnsNameAvailability/read | Controleert of de DNS-label beschikbaar op de opgegeven locatie is |
> | Bewerking | Microsoft.Network/locations/operationResults/read | Het bewerkingsresultaat van een asynchrone post- of DELETE-bewerking opgehaald |
> | Bewerking | Microsoft.Network/locations/operations/read | Bewerkingsresource ophalen die de status van een asynchrone bewerking vertegenwoordigt |
> | Bewerking | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Opgehaald ondersteund grootten voor virtuele machines |
> | Bewerking | Microsoft.Network/locations/usages/read | Meetgegevens voor softwaregebruik voor resources ophalen |
> | Bewerking | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Een lijst met beschikbare virtuele netwerk eindpunt Services ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/delete | Hiermee verwijdert u een netwerkinterface |
> | Bewerking | Microsoft.Network/networkInterfaces/diagnosticIdentity/read | Diagnostische identiteit van de Resource opgehaald |
> | Bewerking | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Netwerkbeveiligingsgroepen geconfigureerde op Network Interface van de Vm ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | De routetabel is geconfigureerd op de netwerkinterface van de virtuele machine ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/ipconfigurations/read | Hiermee haalt u de definitie van de IP-configuratie voor een netwerk-interface.  |
> | Bewerking | Microsoft.Network/networkInterfaces/join/action | Een virtuele Machine koppelt aan een netwerkinterface |
> | Bewerking | Microsoft.Network/networkInterfaces/joinViaPrivateIp/action | Een resource toevoegt aan een netwerkinterface via de koppeling van een Service |
> | Bewerking | Microsoft.Network/networkInterfaces/loadBalancers/read | Alle load balancers die de netwerkinterface deel van uitmaakt opgehaald |
> | Bewerking | Microsoft.Network/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read | Hiermee worden beschikbare metrische gegevens voor de netwerkinterface opgehaald |
> | Bewerking | Microsoft.Network/networkInterfaces/read | Haalt de definitie van een netwerkinterface.  |
> | Bewerking | Microsoft.Network/networkInterfaces/serviceAssociations/delete | Hiermee verwijdert u de koppeling van een Service |
> | Bewerking | Microsoft.Network/networkInterfaces/serviceAssociations/read | De definitie van een Service-koppeling ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/serviceAssociations/validate/action | Valideert de koppeling van een Service |
> | Bewerking | Microsoft.Network/networkInterfaces/serviceAssociations/write | Maakt een nieuwe koppeling van de Service of een bestaande Service-koppeling wijzigen |
> | Bewerking | Microsoft.Network/networkInterfaces/write | Een netwerkinterface maken of bijwerken van een bestaande netwerkinterface.  |
> | Bewerking | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Een standaardbeveiligingsregeldefinitie ophalen opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/delete | Hiermee wordt een netwerkbeveiligingsgroep verwijderd |
> | Bewerking | Microsoft.Network/networkSecurityGroups/join/action | Lid wordt van een netwerkbeveiligingsgroep |
> | Bewerking | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee worden de diagnostische instellingen voor netwerkbeveiligingsgroepen opgehaald |
> | Bewerking | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee kunt u diagnostische instellingen voor netwerkbeveiligingsgroepen maken of bijwerken. Deze bewerking wordt aangevuld door de resourceprovider voor inzichten. |
> | Bewerking | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de gebeurtenissen voor de netwerkbeveiligingsgroep opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/read | Een definitie van een netwerk opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/delete | Hiermee verwijdert u een beveiligingsregel |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/read | Een beveiligingsregeldefinitie ophalen opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/write | Een beveiligingsregel maken of een bestaande beveiligingsregel bijwerken |
> | Bewerking | Microsoft.Network/networkSecurityGroups/write | Een netwerkbeveiligingsgroep maken of bijwerken van een bestaande netwerkbeveiligingsgroep |
> | Bewerking | Microsoft.Network/networkWatchers/availableProvidersList/action | Retourneert alle beschikbare internet serviceproviders voor een opgegeven Azure-regio. |
> | Bewerking | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Retourneert de score relatieve latentie voor internet serviceproviders van een opgegeven locatie en Azure-regio's. |
> | Bewerking | Microsoft.Network/networkWatchers/configureFlowLog/action | Hiermee configureert u stroom logboekregistratie voor een doelbron. |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/delete | Hiermee verwijdert u een Monitor verbinding |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen van de Monitor-verbinding ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van diagnostische instellingen voor de Monitor |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor verbinding Monitor |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Query-verbindingen tussen de opgegeven eindpunten controleren |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/read | Details van de Monitor-verbinding ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Starten van de controle van de verbinding tussen de opgegeven eindpunten |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Connectiviteit tussen de opgegeven eindpunten bewaking tijdelijk onderbreken en stoppen |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/write | Hiermee maakt u een Monitor verbinding |
> | Bewerking | Microsoft.Network/networkWatchers/connectivityCheck/action | Controleert of de mogelijkheid tot stand brengen van een directe TCP-verbinding van een virtuele machine naar een opgegeven eindpunt, met inbegrip van een andere virtuele machine of een willekeurige externe server. |
> | Bewerking | Microsoft.Network/networkWatchers/delete | Hiermee verwijdert u een netwerk-watcher |
> | Bewerking | Microsoft.Network/networkWatchers/ipFlowVerify/action | Retourneert of het pakket wordt toegestaan of geweigerd of naar een specifieke bestemming. |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/delete | Hiermee verwijdert u een Lens |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/query/action | Bewaking van netwerkverkeer op een opgegeven eindpunt query |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/read | Details van de Lens ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/start/action | Controle van netwerkverkeer op een opgegeven eindpunt starten |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/stop/action | Bewaking van netwerkverkeer op een opgegeven eindpunt tijdelijk onderbreken en stoppen |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/write | Maakt een Lens |
> | Bewerking | Microsoft.Network/networkWatchers/nextHop/action | Het volgende hoptype retourneren en vervolgens hopen dat IP-adres voor een opgegeven doel- en IP-doeladres. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/delete | Hiermee verwijdert u een pakketopname |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Hiermee haalt u informatie over eigenschappen en de status van een pakket vastleggen resource. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/read | De definitie voor het vastleggen van pakket ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Stop de actieve opnamesessie van het pakket. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/write | Maakt een pakketopname |
> | Bewerking | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Hiermee haalt u de status van logboekregistratie voor een bron-stroom. |
> | Bewerking | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Het probleemoplossing resultaat van de eerder uitgevoerde of momenteel haalt voor probleemoplossing bewerking uitgevoerd. |
> | Bewerking | Microsoft.Network/networkWatchers/read | Ophalen van de definitie van het netwerk-watcher |
> | Bewerking | Microsoft.Network/networkWatchers/securityGroupView/action | De geconfigureerde en effectieve netwerkbeveiligingsgroepen toegepast op een virtuele machine weergeven. |
> | Bewerking | Microsoft.Network/networkWatchers/topology/action | Hiermee haalt de weergave van resources en hun relaties in een netwerk-niveau in een resourcegroep. |
> | Bewerking | Microsoft.Network/networkWatchers/troubleshoot/action | Start het oplossen van een resource netwerken in Azure. |
> | Bewerking | Microsoft.Network/networkWatchers/write | Een netwerk-watcher maken of bijwerken van een bestaande netwerk-watcher |
> | Bewerking | Microsoft.Network/operations/read | Beschikbare bewerkingen ophalen |
> | Bewerking | Microsoft.Network/publicIPAddresses/delete | Hiermee verwijdert u een openbaar IP-adres. |
> | Bewerking | Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare IP-adres |
> | Bewerking | Microsoft.Network/publicIPAddresses/loadBalancerPools/delete | Hiermee verwijdert u een openbaar IP-adres load balancer back-endpool |
> | Bewerking | Microsoft.Network/publicIPAddresses/loadBalancerPools/join/action | Koppelt een openbare IP-adres load balancer back-endpool |
> | Bewerking | Microsoft.Network/publicIPAddresses/loadBalancerPools/read | Een definitie openbare IP-adres load balancer back-end van toepassingen ophalen |
> | Bewerking | Microsoft.Network/publicIPAddresses/loadBalancerPools/write | Een openbaar IP-adres load balancer back-endpool maken of bijwerken van een bestaand openbaar IP-adres load balancer back-endpool |
> | Bewerking | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instellingen voor het openbare IP-adres ophalen |
> | Bewerking | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write | De diagnostische instellingen van het openbare IP-adres maken of bijwerken |
> | Bewerking | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read | De logboekdefinities van openbare IP-adres ophalen |
> | Bewerking | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read | De metrische definities van openbare IP-adres ophalen |
> | Bewerking | Microsoft.Network/publicIPAddresses/read | De definitie van een openbaar IP-adres ophalen. |
> | Bewerking | Microsoft.Network/publicIPAddresses/write | Een openbaar IP-adres maken of bijwerken van een bestaand openbaar IP-adres.  |
> | Bewerking | Microsoft.Network/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.Network/routeFilters/delete | Hiermee verwijdert u een route filterdefinitie |
> | Bewerking | Microsoft.Network/routeFilters/join/action | Lid wordt van een routefilter |
> | Bewerking | Microsoft.Network/routeFilters/read | De definitie van een route filter ophalen |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/delete | Hiermee verwijdert u een route regel filterdefinitie |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/read | De definitie van een route filter regel ophalen |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/write | Een filterregel route maken of bijwerken van een bestaande route filterregel |
> | Bewerking | Microsoft.Network/routeFilters/write | Een routefilter maken of bijwerken van een bestaand rotue-filter |
> | Bewerking | Microsoft.Network/routeTables/delete | Een routetabeldefinitie verwijderen |
> | Bewerking | Microsoft.Network/routeTables/join/action | Een routetabel joins |
> | Bewerking | Microsoft.Network/routeTables/read | Een routetabeldefinitie opgehaald |
> | Bewerking | Microsoft.Network/routeTables/routes/delete | Hiermee verwijdert u de definitie van een route |
> | Bewerking | Microsoft.Network/routeTables/routes/read | De definitie van een route ophalen |
> | Bewerking | Microsoft.Network/routeTables/routes/write | Een route maken of een bestaande route bijwerken |
> | Bewerking | Microsoft.Network/routeTables/write | Een routetabel maken of een bestaande routetabel bijwerken |
> | Bewerking | Microsoft.Network/securegateways/applicationRuleCollections/delete | Hiermee verwijdert u een toepassing regelverzameling voor een veilige Gateway |
> | Bewerking | Microsoft.Network/securegateways/applicationRuleCollections/read | Een toepassing regelverzameling ophalen voor een opgegeven Secure Gateway |
> | Bewerking | Microsoft.Network/securegateways/applicationRuleCollections/write | Maken of bijwerken van een toepassing regelverzameling voor een Gateway beveiligen |
> | Bewerking | Microsoft.Network/securegateways/delete | Beveiligde Gateway verwijderen |
> | Bewerking | Microsoft.Network/securegateways/networkRuleCollections/delete | Hiermee verwijdert u de verzameling van een regel voor een veilige Gateway |
> | Bewerking | Microsoft.Network/securegateways/networkRuleCollections/read | De verzameling van een regel voor een opgegeven Secure Gateway ophalen |
> | Bewerking | Microsoft.Network/securegateways/networkRuleCollections/write | Maken of bijwerken van de verzameling van een regel voor een Gateway beveiligen |
> | Bewerking | Microsoft.Network/securegateways/read | Beveiligde Gateway verkrijgen |
> | Bewerking | Microsoft.Network/securegateways/write | Maken of bijwerken van een Gateway beveiligen |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/delete | Hiermee verwijdert u een Service-eindpunt-beleid |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/join/action | Lid wordt van een Service-eindpunt-beleid |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Een Subnet koppelt aan beleidsregels voor Service-eindpunt |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/read | Haalt een Beleidsbeschrijving voor Service-eindpunt |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Hiermee verwijdert u de beleidsdefinitie van een Service-eindpunt |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Een Service-eindpunt beleid definitie decodering opgehaald |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Een beleidsdefinitie voor Service-eindpunt maakt of een bestaande Service-eindpunt beleidsdefinitie-updates |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/write | Een Service-eindpunt-beleid maken of bijwerken van een bestaand beleid van de Service-eindpunt |
> | Bewerking | Microsoft.Network/trafficManagerGeographicHierarchies/read | Het Traffic Manager geografisch hiërarchie met regio's die kunnen worden gebruikt met de geografische verkeersrouteringsmethode opgehaald |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Hiermee verwijdert u een Azure-eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager stopt routeren van verkeer naar de verwijderde Azure-eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Hiermee haalt u een Azure-eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat Azure-eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Een nieuw Azure-eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande Azure-eindpunt in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/delete | Het Traffic Manager-profiel verwijdert. Alle instellingen die zijn gekoppeld aan het Traffic Manager-profiel niet verloren en het profiel kan niet meer worden gebruikt om gegevensverkeer te routeren. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Hiermee verwijdert u een extern eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager stopt routeren van verkeer naar de verwijderde externe eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Hiermee haalt u een extern eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat externe eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Een nieuwe Extern eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande Extern eindpunt in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Hiermee wordt de Traffic Manager-Heatmap voor bepaalde Traffic Manager-profiel waarin de aantallen en de latentie gegevens opvragen per locatie en de bron-IP. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Een eindpunt genest verwijdert uit een bestaand Traffic Manager-profiel. Traffic Manager stopt met het verwijderde genest eindpunt routeren van verkeer. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Hiermee haalt u een genest eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat eindpunt genest. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Een nieuw eindpunt voor genest in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaand eindpunt dat kan worden genest in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read | De diagnose van Traffic Manager-instellingen opgehaald |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor het Traffic Manager deze bewerking wordt geleverd door de insights-resourceprovider. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read | Hiermee haalt u de gebeurtenissen voor Traffic Manager |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor Traffic Manager. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/read | De profielconfiguratie Traffic Manager-ophalen. Dit omvat DNS-instellingen, instellingen voor verkeersroutering, instellingen voor eindpuntcontrole en de lijst met eindpunten die worden gerouteerd door dit Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/write | Een Traffic Manager-profiel maken, of wijzig de configuratie van een bestaand Traffic Manager-profiel.<br>Dit omvat inschakelen of uitschakelen van een profiel en DNS-instellingen, instellingen voor verkeersroutering of instellingen voor eindpuntcontrole wijzigen.<br>Eindpunten die worden gerouteerd door het Traffic Manager-profiel kunnen worden toegevoegd, verwijderd, ingeschakeld of uitgeschakeld. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Hiermee verwijdert u de abonnement sleutel gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/read | Hiermee haalt u de abonnement sleutel gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/write | Maakt een nieuw abonnement sleutel moet worden gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/unregister/action | Heft de registratie van het abonnement |
> | Bewerking | Microsoft.Network/virtualHubs/delete | Hiermee verwijdert u een virtuele-Hub |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Hiermee verwijdert u een HubVirtualNetworkConnection |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Een HubVirtualNetworkConnection ophalen |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Maken of bijwerken van een HubVirtualNetworkConnection |
> | Bewerking | Microsoft.Network/virtualHubs/read | Ophalen van een virtuele-Hub |
> | Bewerking | Microsoft.Network/virtualHubs/write | Maken of bijwerken van een virtuele-Hub |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/Connections/Read | VirtualNetworkGatewayConnection ophalen |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/delete | Hiermee verwijdert u een virtualNetworkGateway |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/generatevpnclientpackage/Action | Een pakket voor virtualNetworkGateway genereren |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/generatevpnprofile/Action | VpnProfile-pakket voor VirtualNetworkGateway genereren |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/getadvertisedroutes/Action | Opgehaald virtualNetworkGateway aangekondigd routes |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/getbgppeerstatus/Action | Haalt virtualNetworkGateway bgp-peer-status |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/getlearnedroutes/Action | Virtualnetworkgateway geleerde routes opgehaald |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/getvpnclientipsecparameters/Action | Het ophalen van Vpnclient Ipsec parameters voor VirtualNetworkGateway P2S-client. |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/getvpnprofilepackageurl/Action | De URL van het hulpprogramma voor het profiel van een vooraf gegenereerde vpn-clientpakket opgehaald |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen van het virtuele netwerk Gateway opgehaald |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write | Maken of bijwerken van de diagnostische instellingen voor virtuele netwerkgateway deze bewerking wordt geleverd door de insights-resourceprovider. |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read | Hiermee haalt u de gebeurtenissen voor de virtuele netwerkgateway |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de virtuele netwerkgateway |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/read | Een VirtualNetworkGateway opgehaald |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/reset/Action | Hiermee stelt u een virtualNetworkGateway |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/setvpnclientipsecparameters/Action | Stel Vpnclient Ipsec-parameters voor VirtualNetworkGateway P2S-client. |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Een lijst met ondersteunde VPN-apparaten |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/write | Maken of bijwerken van een VirtualNetworkGateway |
> | Bewerking | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Controleer of IP-adres beschikbaar op de opgegeven virtuele netwerk is |
> | Bewerking | Microsoft.Network/virtualNetworks/customViews/get/action | Een virtueel netwerk aangepaste weergave-inhoud ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/customViews/read | Definitie van een aangepaste weergave van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/delete | Hiermee verwijdert u een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/peer/action | Een virtueel netwerk met een ander virtueel netwerk samenwerkt |
> | Bewerking | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write | De diagnostische instellingen van het virtuele netwerk maken of bijwerken |
> | Bewerking | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read | De definities van de logboekbestanden van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Hiermee verwijdert u een virtueel netwerk peering proxy |
> | Bewerking | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Een virtueel netwerk peering proxydefinitie zijn opgehaald |
> | Bewerking | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Een virtueel netwerk peering proxy maken of bijwerken van een bestaande virtuele netwerk peering-proxy |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/delete | Hiermee verwijdert u een virtueel netwerksubnet |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource zoals opslagaccount of de SQL-database koppelt aan een subnet. |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/read | De subnet-definitie van een virtueel netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/delete | Hiermee verwijdert u een Resource navigatiekoppeling |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/read | De definitie van de navigatiekoppeling Resource ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/write | Een navigatie Resourcekoppeling maken of bijwerken van een bestaande Resource navigatie-koppeling |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/delete | Hiermee verwijdert u de koppeling van een Service-koppeling |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/details/read | De definitie van een Service koppeling koppeling Details ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/read | De definitie van een koppeling van de koppeling Service ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/validate/action | Valideert de koppeling van een Service-koppeling |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/write | Een koppeling koppeling maken of bijwerken van een bestaande koppeling van de Service-koppeling |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerksubnet |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/write | Een virtueel netwerksubnet maken of bijwerken van een bestaand virtueel netwerksubnet |
> | Bewerking | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/delete | Hiermee verwijdert u een label gegevensverkeer Consumer |
> | Bewerking | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/read | De definitie gelabeld verkeer Consumer ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/validate/action | Valideert een label gegevensverkeer Consumer |
> | Bewerking | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/write | Een Consumer verkeer gelabeld maken of bijwerken van een bestaande label gegevensverkeer Consumer |
> | Bewerking | Microsoft.Network/virtualNetworks/usages/read | Het gebruik van de IP voor elk subnet van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualMachines/read | Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Hiermee verwijdert u een virtueel netwerk peering |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | De definitie van een virtueel netwerk peering ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Een virtueel netwerk peering of een bestaande virtuele netwerk-peering bijwerken |
> | Bewerking | Microsoft.Network/virtualNetworks/write | Een virtueel netwerk maken of bijwerken van een bestaand virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/delete | Tik op virtueel netwerk verwijderen |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/join/action | Een virtueel netwerk-tap joins |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/read | Tik op virtueel netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/write | Maken of bijwerken van virtueel netwerk-Tap |
> | Bewerking | Microsoft.Network/virtualWans/delete | Verwijdert een virtuele Wan |
> | Bewerking | Microsoft.Network/virtualWans/read | Get a virtuele Wan |
> | Bewerking | Microsoft.Network/virtualWans/virtualHubProxies/delete | Hiermee verwijdert u een virtuele Hub-proxy |
> | Bewerking | Microsoft.Network/virtualWans/virtualHubProxies/read | De definitie van een virtuele Hub proxy ophalen |
> | Bewerking | Microsoft.Network/virtualWans/virtualHubProxies/write | Een proxy virtuele Hub maken of bijwerken van een virtuele Hub-proxy |
> | Bewerking | Microsoft.Network/virtualWans/virtualHubs/read | Hiermee haalt u alle virtuele-Hubs die verwijzen naar een virtuele Wan. |
> | Bewerking | Microsoft.Network/virtualwans/vpnconfiguration/action | Een VPN-configuratie opgehaald |
> | Bewerking | Microsoft.Network/virtualWans/vpnSiteProxies/delete | Hiermee verwijdert u een VPN-Site-proxy |
> | Bewerking | Microsoft.Network/virtualWans/vpnSiteProxies/read | De definitie van een VPN-Site-proxy ophalen |
> | Bewerking | Microsoft.Network/virtualWans/vpnSiteProxies/write | Een VPN-Site-proxy maken of bijwerken van een VPN-Site-proxy |
> | Bewerking | Microsoft.Network/virtualWans/vpnSites/read | Hiermee haalt u alle VPN-Sites die verwijzen naar een virtuele Wan. |
> | Bewerking | Microsoft.Network/virtualWans/write | Maken of bijwerken van een virtuele Wan |
> | Bewerking | Microsoft.Network/vpnGateways/read | Hiermee wordt een VpnGateway opgehaald. |
> | Bewerking | microsoft.network/vpnGateways/vpnConnections/read | Hiermee haalt u een VPN-verbinding. |
> | Bewerking | microsoft.network/vpnGateways/vpnConnections/write | Plaatst een VPN-verbinding. |
> | Bewerking | Microsoft.Network/vpnGateways/write | Een VpnGateway plaatst. |
> | Bewerking | Microsoft.Network/vpnsites/delete | Hiermee verwijdert u een VPN-Site-resource. |
> | Bewerking | Microsoft.Network/vpnsites/read | Hiermee haalt u een VPN-Site-resource. |
> | Bewerking | Microsoft.Network/vpnsites/write | Maken of bijwerken van een VPN-Site-resource. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Hiermee wordt gecontroleerd of de resourcenaam voor de naamruimte beschikbaar is binnen de NotificationHub-service. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | De verificatieregel voor een naamruimte De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Hiermee wordt gecontroleerd of de NotificationHub-naam beschikbaar is binnen een naamruimte. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/Delete | De resource van een naamruimte verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | De lijst met verificatieregels voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Verificatieregels voor Notification Hub verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | De verbindingsreeks naar de Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | De lijst met verificatieregels voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | De verificatieregel voor Notification Hub De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Notification Hub-autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Een testpushmelding verzenden. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | De resource voor Notification Hub verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Lijst met Namespace metrische Resource beschrijvingen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Alle Notification Hub PNS referenties ophalen. Dit omvat, WNS, MPNS, APNS, GCM en Baidu-referenties |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | De lijst met beschrijvingen van resources voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Een Notification Hub maken en bijwerken van de eigenschappen ervan. De eigenschappen zijn hoofdzakelijk PNS-referenties. Autorisatieregels en TTL |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/write | Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/register/action | Hiermee wordt het abonnement voor de NotificationHubs-resourceprovider geregistreerd en wordt het maken van naamruimten en NotificationHubs mogelijk |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.OperationalInsights/linkTargets/read | Een lijst met bestaande accounts die niet gekoppeld aan een Azure-abonnement zijn. Als deze Azure-abonnement koppeling naar een werkruimte, gebruikt u een klant-id die is geretourneerd door deze bewerking in de eigenschap van de klant-id van de bewerking werkruimte maken. |
> | Bewerking | Microsoft.OperationalInsights/register/action | Een abonnement op een resourceprovider registreren. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoeken met nieuwe-engine. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Search schema V2 worden opgehaald. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/api/query/action | Zoeken met nieuwe-engine. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Search schema V2 worden opgehaald. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Configuratiebereik verwijderen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Configuratiebereik ophalen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Configuratiebereik instellen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/delete | Verwijder de gegevensbronnen onder een werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/read | Gegevensbronnen ophalen onder een werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/write | Gegevensbronnen onder een werkruimte maken of bijwerken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/delete | Hiermee verwijdert u een werkruimte. Als u de werkruimte is gekoppeld aan een bestaande werkruimte tijdens het maken worden de werkruimte die deze is gekoppeld aan niet verwijderd. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Genereert Registratiecertificaat voor de werkruimte. Dit certificaat wordt gebruikt voor Microsoft System Center Operation Manager verbinden met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Hiermee schakelt u een intelligence pack voor een bepaalde werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Hiermee kunt een intelligence pack voor een bepaalde werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Geeft een lijst van alle intelligence packs die zichtbaar zijn voor een bepaalde worksapce en ook wordt aangegeven of het pakket is ingeschakeld of uitgeschakeld voor die werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Delete gekoppelde services onder deze werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/read | Ophalen van de gekoppelde services onder gegeven van de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/write | Services maken/bijwerken gekoppeld onder deze werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/listKeys/action | Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/listKeys/read | Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/managementGroups/read | Hiermee haalt de namen en de metagegevens voor System Center Operations Manager-beheergroepen die is verbonden met deze werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Metriek definities onder werkruimte ophalen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Verwijderen van de gebruiker meldingsinstellingen voor de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Ophalen van de gebruiker meldingsinstellingen voor de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Instellen van de gebruiker meldingsinstellingen voor de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/purge/action | Opgegeven gegevens verwijderen uit de werkruimte |
> | Bewerking | Microsoft.OperationalInsights/workspaces/read | Een bestaande werkruimte opgehaald |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Hiermee verwijdert u een opgeslagen zoekopdracht |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/read | Een opgeslagen zoekopdracht opgehaald |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/write | Hiermee maakt u een opgeslagen zoekopdracht |
> | Bewerking | Microsoft.OperationalInsights/workspaces/schema/read | Hiermee haalt de search-schema voor de werkruimte.  Search schema bevat de blootgestelde velden en hun typen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/search/action | Een zoekopdracht wordt uitgevoerd |
> | Bewerking | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Hiermee verwijdert u een configuratie van de opslag. Hiermee stopt u Microsoft Operational Insights van lezen van gegevens van het opslagaccount. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Hiermee haalt u een configuratie van de opslag. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Maakt een nieuwe opslagconfiguratie. Deze configuraties worden gebruikt voor het ophalen van gegevens uit een locatie in een bestaand opslagaccount. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/usages/read | Hiermee haalt gebruiksgegevens voor een werkruimte met inbegrip van de hoeveelheid gegevens gelezen door de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/write | Een nieuwe werkruimte of koppelingen naar een bestaande werkruimte wordt gemaakt door de klant-id van de bestaande werkruimte. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/delete | Verwijderen van bestaande Management-koppeling |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/read | Ophalen van bestaande Management-koppeling |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/write | Een nieuwe Management associatie maken |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/delete | Verwijderen van bestaande Management Configuratin |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/read | Configuratie van bestaande Management ophalen |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/write | Maak een nieuwe configuratie voor beheer |
> | Bewerking | Microsoft.OperationsManagement/register/action | Een abonnement op een resourceprovider registreren. |
> | Bewerking | Microsoft.OperationsManagement/solutions/delete | Verwijderen van bestaande OMS-oplossing |
> | Bewerking | Microsoft.OperationsManagement/solutions/read | Ophalen van OMS oplossing afgesloten |
> | Bewerking | Microsoft.OperationsManagement/solutions/write | Nieuwe OMS-oplossing maken |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.PolicyInsights/policyEvents/queryResults/action | Informatie over Beleidsgebeurtenissen opvragen. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/queryResults/action | Informatie over beleid statussen opvragen. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/summarize/action | Samenvattende informatie over de meest recente statussen beleid opvragen. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Portal/dashboards/delete | Het dashboard verwijderen uit het abonnement. |
> | Bewerking | Microsoft.Portal/dashboards/read | De dashboards voor het abonnement lezen. |
> | Bewerking | Microsoft.Portal/dashboards/write | Dashboard toevoegen aan een abonnement of het dashboard wijzigen. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/checkNameAvailability/action | Controleert of Power BI-specifieke-capaciteit van de opgegeven naam geldig is en niet in gebruik. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/delete | Hiermee verwijdert u de Power BI toegewezen capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor Power BI toegewezen capaciteit |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor Power BI-specifieke capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/read | Haalt de informatie van de opgegeven Power BI-specifieke capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/resume/action | Hervat de capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/skus/read | Beschikbare SKU-informatie voor de capaciteit ophalen |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/suspend/action | Onderbreekt de capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/write | Maken of bijwerken van de opgegeven Power BI-specifieke capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/locations/operationresults/read | Haalt de informatie van het resultaat van de opgegeven bewerking. |
> | Bewerking | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Haalt de informatie van de status van de opgegeven bewerking. |
> | Bewerking | Microsoft.PowerBIDedicated/operations/read | De informatie van bewerkingen opgehaald |
> | Bewerking | Microsoft.PowerBIDedicated/register/action | Power BI speciale resourceprovider geregistreerd. |
> | Bewerking | Microsoft.PowerBIDedicated/skus/read | Haalt de informatie van SKU 's |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Bewerking | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Bewerking | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Bewerking | Microsoft.RecoveryServices/locations/backupStatus/action | Controleer de Status van back-up voor de Recovery Services-kluizen |
> | Bewerking | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Functies valideren |
> | Bewerking | Microsoft.RecoveryServices/operations/read | Bewerking resulteert in de lijst met bewerkingen voor een Resource Provider |
> | Bewerking | Microsoft.RecoveryServices/register/action | Registreert het abonnement voor de opgegeven Resource Provider |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupconfig/read | Retourneert-configuratie voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupconfig/write | Configuratie van de updates voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Maak een back-beveiliging wordt intentie |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Retourneert de status van de bewerking |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Alle beveiligbare containers ophalen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Hiermee verwijdert u de geregistreerde Container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Voer de query voor werkbelastingen binnen een container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Alle items in een container ophalen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hiermee wordt een back-up van het beveiligde item gemaakt. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Hiermee verwijdert u beveiligd Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Geeft de details van het artikel beveiligd object |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Inrichten Instant Item herstel voor beveiligde Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Herstelpunten voor beveiligde items herstellen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Herstel op directe intrekken voor beveiligde Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een back-up beveiligd Item maken |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Hiermee maakt u een geregistreerde container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de lijst van de container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Annuleer de taak |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/read | Retourneert alle objecten van de taak |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporttaken |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupOperations/read | Back-upbewerking retourneert Status voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Een beveiligingsbeleid voor verwijderen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Status van de beleidsbewerking ophalen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beleidsregels voor beveiliging |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Hiermee maakt u het beveiligingsbeleid |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers die horen bij het abonnement |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Beveiliging PINCODE retourneert informatie voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Retourneert opslagconfiguratie voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | De opslagconfiguratie updates voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/delete | De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis' |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Oplossing voor de waarschuwing. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Hiermee haalt u de configuratie voor meldingen van Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Hiermee configureert u een e-mailmeldingen Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/read | Azure Backup diagnostische gegevens |
> | Bewerking | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/write | Azure Backup diagnostische gegevens |
> | Bewerking | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/logDefinitions/read | Azure back-uplogboeken |
> | Bewerking | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/metricDefinitions/read | Azure Backup metrische gegevens |
> | Bewerking | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | De Container registratie-bewerking kan worden gebruikt om de registratie van een container te. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking servicecontainer registreren kan worden gebruikt om te registreren van een container met Service voor herstel. |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Alle instellingen voor waarschuwingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Alle waarschuwingsinstellingen maken of bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Verwijderen van eventuele Fabrics |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Proces serverinstallatiekopie te implementeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Infrastructuur verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat vernieuwen voor de infrastructuur |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Geen netwerken lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Eventuele Netwerktoewijzingen verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Alle Netwerktoewijzingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Maken of bijwerken van eventuele Netwerktoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Beveiligbare Item detecteren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Beveiliging-Containers gelezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | De Beveiligingscontainer verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare objecten lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Verwijder alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Alle beveiligde Items lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Replicatie herstelpunten lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Het beveiligde Item verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparatie replicatie |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligde Item opnieuw beveiligen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Het opruimen van testfailover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Bijwerken van de Mobility-Service |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Maken of bijwerken van alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Alle toewijzingen van de Container beveiliging verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle toewijzingen van de Container beveiliging lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Beveiliging Container toewijzing verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Maken of bijwerken van alle toewijzingen van de Container beveiliging |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Switch Protection Container |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Maken of bijwerken van alle Containers beveiliging |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Verwijderen van een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Een Recovery Services-Providers lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Vernieuw de Provider |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Recovery Services-Provider verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Maken of bijwerken van een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Alle toewijzingen van de classificatie opslag verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Alle toewijzingen van de classificatie opslag lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Maken of bijwerken van alle toewijzingen van de classificatie opslag |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Taken verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lezen van taken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Alle taken maken of bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Maken of bijwerken van een Fabrics |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Taak annuleren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/read | Lezen van taken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Taak opnieuw starten |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Taak hervatten |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Beleid verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van beleid |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Beleid maken of bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Verwijderen van eventuele herstelplannen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstelplan-failover doorvoeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plan voor herstel voor geplande Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Alle herstelplannen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Plan voor herstel opnieuw beveiligen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test Failover herstelplan |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test het opruimen van herstelplan |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plan voor herstel van failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Maken of bijwerken van een herstelplannen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert token-informatie voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/vaults/usages/read | Lezen van het gebruik van een kluis |
> | Bewerking | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Relay/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.Relay/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/delete | Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/write | Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/Delete | De resource van een naamruimte verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | De bewerking HybridConnection bijwerken. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Bewerking HybridConnection autorisatieregels verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | De verbindingsreeks naar HybridConnection ophalen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  De lijst met autorisatieregels HybridConnection ophalen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Autorisatieregels HybridConnection maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/Delete | Bewerking HybridConnection bron verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/read | Lijst met beschrijvingen van de Resource HybridConnection ophalen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/write | Maken of bijwerken HybridConnection eigenschappen. |
> | Bewerking | Microsoft.Relay/namespaces/messagingPlan/read | Hiermee wordt de Messaging plannen voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.Relay/namespaces/messagingPlan/write | De Messaging plannen voor een naamruimte-updates.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.Relay/namespaces/operationresults/read | De status van de naamruimtebewerking ophalen |
> | Bewerking | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met Namespace metrische Resource beschrijvingen |
> | Bewerking | Microsoft.Relay/namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Bewerking | Microsoft.Relay/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | De bewerking WcfRelay bijwerken. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken. |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Bewerking WcfRelay autorisatieregels verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | De verbindingsreeks naar WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  De lijst met autorisatieregels WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Autorisatieregels WcfRelay maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/Delete | Bewerking WcfRelay bron verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/read | Lijst met beschrijvingen van de Resource WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/write | Maken of bijwerken WcfRelay eigenschappen. |
> | Bewerking | Microsoft.Relay/namespaces/write | Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.Relay/register/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk |
> | Bewerking | Microsoft.Relay/unregister/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | De beschikbaarheidsstatus ophalen voor de opgegeven resource |
> | Bewerking | Microsoft.ResourceHealth/AvailabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Bewerking | Microsoft.ResourceHealth/events/read | Service Health-gebeurtenissen ophalen voor ingesteld abonnement |
> | Bewerking | Microsoft.Resourcehealth/healthevent/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Activated/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.Resourcehealth/healthevent/InProgress/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Pending/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Resolved/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Updated/action | Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid |
> | Bewerking | Microsoft.ResourceHealth/impactedResources/read | Betreffende resources ophalen voor ingesteld abonnement |
> | Bewerking | Microsoft.ResourceHealth/register/action | Hiermee wordt het abonnement geregistreerd voor Microsoft ResourceHealth |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Resources/checkPolicyCompliance/action | Controleer de nalevingsstatus van een bepaalde resource aan de hand van het beleid voor resources. |
> | Bewerking | Microsoft.Resources/checkResourceName/action | Controleer of de resourcenaam geldig is. |
> | Bewerking | Microsoft.Resources/deployments/cancel/action | Hiermee kunt u een implementatie annuleren. |
> | Bewerking | Microsoft.Resources/deployments/delete | Hiermee kunt u een implementatie verwijderen. |
> | Bewerking | Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/deployments/validate/action | Hiermee kunt u een implementatie valideren. |
> | Bewerking | Microsoft.Resources/deployments/write | Hiermee kunt u een implementatie maken of bijwerken. |
> | Bewerking | Microsoft.Resources/links/delete | Hiermee kunt u een resourcekoppeling verwijderen. |
> | Bewerking | Microsoft.Resources/links/read | Hiermee kunt u resourcekoppelingen ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/links/write | Hiermee kunt u een resourcekoppeling maken of bijwerken. |
> | Bewerking | Microsoft.Resources/marketplace/purchase/action | Hiermee worden een resource gekocht via de marketplace. |
> | Bewerking | Microsoft.Resources/providers/read | De lijst met providers ophalen. |
> | Bewerking | Microsoft.Resources/resources/read | Hiermee wordt de lijst met resources op basis van filters opgehaald. |
> | Bewerking | Microsoft.Resources/subscriptions/locations/read | Hiermee kunt u de lijst met ondersteunde locaties ophalen. |
> | Bewerking | Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
> | Bewerking | Microsoft.Resources/subscriptions/providers/read | Hiermee kunt u de resourceproviders ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/read | Hiermee kunt u de lijst met abonnementen ophalen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/delete | Hiermee kunt u een resourcegroep en de bijhorende resources verwijderen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Hiermee worden lijsten met statussen van implementatiebewerkingen opgehaald. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Hiermee kunt u een implementatie maken of bijwerken. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Hiermee kunt u resources van een resourcegroep naar een andere resourcegroep verplaatsen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Hiermee worden de resources voor de resourcegroep opgehaald. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Hiermee wordt de verplaatsing van resources van de ene naar de andere resourcegroep gevalideerd. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/write | Hiermee kunt u een resourcegroep maken of bijwerken. |
> | Bewerking | Microsoft.Resources/subscriptions/resources/read | Hiermee kunt u de resources van een abonnement ophalen. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/delete | Hiermee kunt u een abonnementslabel verwijderen. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/read | Hiermee kunt u abonnementslabels ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Hiermee kunt een abonnementslabelwaarde verwijderen. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Hiermee kunt u abonnementslabelwaarden ophalen of opnemen in een lijst. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Hiermee kunt u een abonnementslabelwaarde toevoegen. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/write | Hiermee kunt u een abonnementslabel toevoegen. |
> | Bewerking | Microsoft.Resources/tenants/read | Hiermee kunt u de lijst met tenants ophalen. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Scheduler/jobcollections/delete | Hiermee verwijdert u de taakverzameling. |
> | Bewerking | Microsoft.Scheduler/jobcollections/disable/action | Hiermee schakelt u de taakverzameling uit. |
> | Bewerking | Microsoft.Scheduler/jobcollections/enable/action | Hiermee schakelt u de taakverzameling in. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/delete | Hiermee verwijdert u een taak. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Hiermee wordt een definitie voor een logische app gegenereerd op basis van een Scheduler-taak. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Hiermee haalt u de taakgeschiedenis op. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/read | Hiermee haalt u een taak op. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/run/action | De taak wordt uitgevoerd. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/write | Hiermee maakt u een taak of werkt u er een bij. |
> | Bewerking | Microsoft.Scheduler/jobcollections/read | Taakverzameling ophalen |
> | Bewerking | Microsoft.Scheduler/jobcollections/write | Hiermee maakt u een taakverzameling of werkt u er een bij. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Search/checkNameAvailability/action | Controleert de beschikbaarheid van de naam van de service. |
> | Bewerking | Microsoft.Search/operations/read | Een lijst met alle beschikbare bewerkingen van de provider Microsoft.Search. |
> | Bewerking | Microsoft.Search/register/action | Het abonnement voor de resource zoekmachine registreert en wordt het maken van de search-services. |
> | Bewerking | Microsoft.Search/searchServices/createQueryKey/action | Maakt de querysleutel. |
> | Bewerking | Microsoft.Search/searchServices/delete | Hiermee verwijdert u de search-service. |
> | Bewerking | Microsoft.Search/searchServices/deleteQueryKey/delete | Hiermee verwijdert u de querysleutel. |
> | Bewerking | Microsoft.Search/searchServices/diagnosticSettings/read | De diganostic instelling opgehaald voor de resource lezen |
> | Bewerking | Microsoft.Search/searchServices/diagnosticSettings/write | Maken of bijwerken van de instelling diganostic voor de bron |
> | Bewerking | Microsoft.Search/searchServices/listAdminKeys/action | De administratorsleutels leest. |
> | Bewerking | Microsoft.Search/searchServices/listQueryKeys/read | Retourneert de lijst met API-querysleutels voor de opgegeven Azure Search-service. |
> | Bewerking | Microsoft.Search/searchServices/logDefinitions/read | De beschikbare logboeken opgehaald voor de search-service |
> | Bewerking | Microsoft.Search/searchServices/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor de search-service |
> | Bewerking | Microsoft.Search/searchServices/read | Leest de search-service. |
> | Bewerking | Microsoft.Search/searchServices/regenerateAdminKey/action | De Administrator-code genereert. |
> | Bewerking | Microsoft.Search/searchServices/start/action | Start de search-service. |
> | Bewerking | Microsoft.Search/searchServices/stop/action | De search-service stopt. |
> | Bewerking | Microsoft.Search/searchServices/write | Maken of bijwerken van de search-service. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Security/alerts/read | Alle beschikbare beveiligingswaarschuwingen opgehaald |
> | Bewerking | Microsoft.Security/applicationWhitelistings/read | De toepassing whitelistings opgehaald |
> | Bewerking | Microsoft.Security/applicationWhitelistings/write | Hiermee maakt u een nieuwe toepassing whitelisting of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/complianceResults/read | De compliantieresultaten voor de resource opgehaald |
> | Bewerking | Microsoft.Security/locations/alerts/activate/action | Een beveiligingswaarschuwing activeren |
> | Bewerking | Microsoft.Security/locations/alerts/dismiss/action | Een beveiligingswaarschuwing te sluiten |
> | Bewerking | Microsoft.Security/locations/alerts/read | Alle beschikbare beveiligingswaarschuwingen opgehaald |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Initieert een netwerktoegangsbeleid just in time |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Het toegangsbeleid just in time netwerk opgehaald |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Hiermee maakt u een nieuw netwerk just-in-time-beleid of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/locations/read | De locatie van beveiliging opgehaald |
> | Bewerking | Microsoft.Security/locations/tasks/activate/action | Een beveiligingsaanbeveling activeren |
> | Bewerking | Microsoft.Security/locations/tasks/dismiss/action | Een beveiligingsaanbeveling negeren |
> | Bewerking | Microsoft.Security/locations/tasks/read | Alle beschikbare beveiligingsaanbevelingen opgehaald |
> | Bewerking | Microsoft.Security/locations/tasks/resolve/action | Een beveiligingsaanbeveling oplossen |
> | Bewerking | Microsoft.Security/locations/tasks/start/action | Start een beveiligingsaanbeveling |
> | Bewerking | Microsoft.Security/policies/read | Het beveiligingsbeleid opgehaald |
> | Bewerking | Microsoft.Security/policies/write | Updates van het beveiligingsbeleid |
> | Bewerking | Microsoft.Security/pricings/delete | Hiermee verwijdert u de prijscategorie instellingen voor het bereik |
> | Bewerking | Microsoft.Security/pricings/read | De prijscategorie instellingen voor de scope opgehaald |
> | Bewerking | Microsoft.Security/pricings/write | De prijscategorie instellingen voor de scope-updates |
> | Bewerking | Microsoft.Security/register/action | Registreert het abonnement voor Azure Security Center |
> | Bewerking | Microsoft.Security/securityContacts/delete | Neem contact op met de beveiliging wordt verwijderd |
> | Bewerking | Microsoft.Security/securityContacts/read | Neem contact op met de beveiliging opgehaald |
> | Bewerking | Microsoft.Security/securityContacts/write | Neem contact op met de beveiliging werkt |
> | Bewerking | Microsoft.Security/securitySolutions/delete | Hiermee verwijdert u een beveiligingsoplossing |
> | Bewerking | Microsoft.Security/securitySolutions/read | De beveiligingsoplossingen opgehaald |
> | Bewerking | Microsoft.Security/securitySolutions/write | Maakt een nieuwe beveiligingsoplossing of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/securitySolutionsReferenceData/read | Referentiegegevens voor de beveiligingsoplossingen opgehaald |
> | Bewerking | Microsoft.Security/securityStatuses/read | De beveiliging van health-statussen voor Azure-resources opgehaald |
> | Bewerking | Microsoft.Security/securityStatusesSummaries/read | De beveiliging van samenvattingen van de statussen voor de scope opgehaald |
> | Bewerking | Microsoft.Security/tasks/read | Alle beschikbare beveiligingsaanbevelingen opgehaald |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/delete | Hiermee verwijdert u een web application firewall |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/read | Het web application firewalls opgehaald |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/write | Hiermee maakt u een nieuwe web application firewall of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/workspaceSettings/connect/action | Wijzigen van de werkruimte-instellingen opnieuw verbinden |
> | Bewerking | Microsoft.Security/workspaceSettings/delete | Hiermee verwijdert u de werkruimte-instellingen |
> | Bewerking | Microsoft.Security/workspaceSettings/read | De werkruimte-instellingen opgehaald |
> | Bewerking | Microsoft.Security/workspaceSettings/write | De werkruimte-instellingen voor updates |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/delete | Verwijderen van een diagnostische instelling |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/read | Lezen van een diagnostische instelling |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/write | Schrijven van een diagnostische instelling |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Lezen van een diagnostische instelling categorieën |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ServiceBus/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.ServiceBus/checkNamespaceAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy. |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | De verbindingstekenreeks naar de naamruimte ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/read | De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/write | Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/Delete | De resource van een naamruimte verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Hiermee worden de sleutels voor de verificatieregels opgehaald van de primaire naamruimte voor herstel na noodgeval |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Verificatieregels van de primaire naamruimte voor herstel na noodgeval ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Hiermee wordt de beschikbaarheid van de naamruimtealias in een bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Hiermee wordt het gebeurtenisrasterfilter verwijderd dat is gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Hiermee wordt het gebeurtenisrasterfilter opgehaald dat is gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Het gebeurtenisrasterfilter gekoppeld aan de naamruimte wprdt gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventhubs/read | Lijst met beschrijvingen van EventHub-Resource |
> | Bewerking | Microsoft.ServiceBus/namespaces/messagingPlan/read | Hiermee wordt de Messaging plannen voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.ServiceBus/namespaces/messagingPlan/write | De Messaging plannen voor een naamruimte-updates.<br>Deze API is afgeschaft.<br>Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)...<br>Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrate/action | Bewerking voor migreren van de naamruimte |
> | Bewerking | Microsoft.ServiceBus/namespaces/operationresults/read | De status van de naamruimtebewerking ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met Namespace logboeken Resource beschrijvingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met Namespace metrische Resource beschrijvingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Bewerking voor het bijwerken van de wachtrij. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken. |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Bewerking autorisatieregels wachtrij verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | De verbindingsreeks naar wachtrij ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  De lijst met autorisatieregels wachtrij ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Autorisatieregels wachtrij maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/Delete | Bewerking bron van de wachtrij verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/read | Lijst met beschrijvingen van de wachtrij-Resource |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/write | Maken of wachtrijeigenschappen Update. |
> | Bewerking | Microsoft.ServiceBus/namespaces/read | De lijst met beschrijvingen van resources van naamruimten ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Bewerking voor het bijwerken van onderwerp. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Bewerking verificatieregels onderwerp verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | De verbindingsreeks naar onderwerp ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  De lijst met verificatieregels onderwerp ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Verificatieregels onderwerp maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/Delete | Bewerking onderwerp bron verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/read | Lijst met beschrijvingen van de Resource onderwerp |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Bewerking TopicSubscription bron verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Lijst met beschrijvingen van de Resource TopicSubscription ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Bewerking bron van de regel verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Lijst met beschrijvingen van de Resource regel |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Maken of de eigenschappen van de regel voor het bijwerken. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Maken of bijwerken TopicSubscription eigenschappen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/write | Maken of bijwerken onderwerp eigenschappen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/write | Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.ServiceBus/register/action | Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk |
> | Bewerking | Microsoft.ServiceBus/sku/read | Lijst met beschrijvingen van de Resource-Sku ophalen |
> | Bewerking | Microsoft.ServiceBus/sku/regions/read | Lijst met beschrijvingen van de Resource SkuRegions ophalen |
> | Bewerking | Microsoft.ServiceBus/unregister/action | Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/delete | Een toepassing verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/read | Een toepassing lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/delete | Een service verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Een partitie lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Een replica lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/read | Een service lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Een servicestatus lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/write | Een service maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/write | Een toepassing maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Een toepassingstype verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/read | Een toepassingstype lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Een versie van een toepassingstype verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Een versie van een toepassingstype lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Een versie van een toepassingstype maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/write | Een toepassingstype maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/delete | Een cluster verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/nodes/read | Een knooppunt lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/read | Een cluster lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/statuses/read | Een clusterstatus lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/write | Een cluster maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/locations/clusterVersions/read | Een clusterversie lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Een clusterversie voor een specifieke omgeving lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/operationresults/read | Bewerkingsresultaten lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/operations/read | Bewerkingen op locatie lezen |
> | Bewerking | Microsoft.ServiceFabric/operations/read | Beschikbare bewerkingen lezen |
> | Bewerking | Microsoft.ServiceFabric/register/action | Een actie registreren |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.SignalRService/checknameavailability/action | Controleert of een naam beschikbaar voor gebruik met een nieuwe SignalR-service |
> | Bewerking | Microsoft.SignalRService/register/action | De provider van de resource 'Microsoft.SignalRService' wordt geregistreerd met een abonnement |
> | Bewerking | Microsoft.SignalRService/SignalR/delete | De volledige SignalR verwijderen |
> | Bewerking | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor SignalR |
> | Bewerking | Microsoft.SignalRService/SignalR/read | De SignalR instellingen en configuraties die in de beheerportal of via API weergeven |
> | Bewerking | Microsoft.SignalRService/SignalR/write | De instellingen en configuraties in de beheerportal of via de API van de SignalR wijzigen |
> | Bewerking | Microsoft.SignalRService/unregister/action | Heft de registratie van de resourceprovider 'Microsoft.SignalRService' met een abonnement |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/delete | Hiermee wordt een toepassingsdefinitie verwijderd. |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/read | Hiermee wordt een lijst met toepassingsdefinities opgehaald. |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/write | Hiermee wordt een toepassingsdefinitie toegevoegd of gewijzigd. |
> | Bewerking | Microsoft.Solutions/applications/delete | Hiermee verwijdert u een toepassing. |
> | Bewerking | Microsoft.Solutions/applications/read | Hiermee wordt een lijst met toepassingen opgehaald. |
> | Bewerking | Microsoft.Solutions/applications/write | Hiermee wordt een toepassing gemaakt. |
> | Bewerking | Microsoft.Solutions/locations/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de resource ingelezen. |
> | Bewerking | Microsoft.Solutions/register/action | Registreren bij Solutions. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Sql/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het inrichten van wereldwijd voor een bepaald abonnement. |
> | Bewerking | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Resultaat van de server blob controle beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/capabilities/read | Hiermee haalt u de mogelijkheden voor dit abonnement in een bepaalde locatie |
> | Bewerking | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/locations/databaseOperationResults/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Opgehaald in uitvoering bewerkingen op verwijderde server |
> | Bewerking | Microsoft.Sql/locations/deletedServerOperationResults/read | Opgehaald in uitvoering bewerkingen op verwijderde server |
> | Bewerking | Microsoft.Sql/locations/deletedServers/read | De lijst met verwijderde servers of haalt de eigenschappen voor de opgegeven server verwijderde retourneren. |
> | Bewerking | Microsoft.Sql/locations/deletedServers/recover/action | Een verwijderde server herstellen |
> | Bewerking | Microsoft.Sql/locations/deleteVirtualNetworkOrSubnets/action | Verwijdert virtuele netwerk regels die zijn gekoppeld aan een virtueel netwerk of subnet |
> | Bewerking | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | De azure asynchrone bewerking voor een elastische pool asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.Sql/locations/elasticPoolOperationResults/read | Hiermee wordt het resultaat van een bewerking voor de elastische groep opgehaald. |
> | Bewerking | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/delete | Hiermee verwijdert u een bestaande exemplaar failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Geplande failover worden uitgevoerd in een bestaand exemplaar failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Geforceerde failover worden uitgevoerd in een bestaand exemplaar failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/read | Retourneert de lijst met exemplaar failover groepen of haalt u de eigenschappen voor het opgegeven exemplaar van failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/write | Maakt een groep van de failover-exemplaar met de opgegeven parameters of updates van de eigenschappen of labels voor het opgegeven exemplaar van failover-groep. |
> | Bewerking | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Beheerde database restore-bewerking is voltooid |
> | Bewerking | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Opgehaald in uitvoering bewerkingen op beheerde database transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Opgehaald in uitvoering bewerkingen op beheerde database transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/locations/networkInterfaceAzureAsyncOperation/read | Retourneert de gegevens van een specifieke netwerkinterface Azure asynchrone bewerking |
> | Bewerking | Microsoft.Sql/locations/networkInterfaceOperationResults/read | Retourneert de gegevens van de opgegeven network interface-bewerking |
> | Bewerking | Microsoft.Sql/locations/read | De beschikbare locaties voor een bepaald abonnement opgehaald |
> | Bewerking | Microsoft.Sql/locations/syncAgentOperationResults/read | Resultaat van de synchronisatie-agent resource bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/syncDatabaseIds/read | De synchronisatie-database-id's voor een bepaalde regio en een abonnement ophalen |
> | Bewerking | Microsoft.Sql/locations/syncGroupOperationResults/read | Resultaat van de synchronisatie groep resource-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/syncMemberOperationResults/read | Resultaat van de synchronisatie lid resource bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/usages/read | Hiermee wordt een verzameling van de meetgegevens voor softwaregebruik voor dit abonnement op een locatie |
> | Bewerking | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Retourneert de gegevens van de opgegeven virtuele netwerk regels azure asynchrone bewerking  |
> | Bewerking | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Retourneert de gegevens van de opgegeven virtuele netwerk regels bewerking  |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/delete | Hiermee verwijdert u een bestaande beheerder van beheerde exemplaar. |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/read | Hiermee wordt een lijst met beheerde exemplaar beheerders opgehaald. |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/write | Maken of bijwerken van de administrator beheerde exemplaar met de opgegeven parameters. |
> | Bewerking | Microsoft.Sql/managedInstances/databases/delete | Hiermee verwijdert u een bestaande beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/read | Beheerde database opgehaald bestaande |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Ophalen van gegevens van de afwijkingsdetectie van de database threat geconfigureerd op een bepaalde beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Het beleid database threat detectie voor een bepaalde beheerde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityEvents/read | Haalt de beheerde database beveiligingsgebeurtenissen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Ophalen van gegevens van de database met transparante gegevensversleuteling op een bepaalde beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | De database met transparante gegevensversleuteling voor een bepaalde beheerde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/write | Een nieuwe database maken of bijwerken van een bestaande database. |
> | Bewerking | Microsoft.Sql/managedInstances/delete | Hiermee verwijdert u een bestaand exemplaar van de beheerde. |
> | Bewerking | Microsoft.Sql/managedInstances/metricDefinitions/read | Beheerde exemplaar metrische definities ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/metrics/read | Ophalen van beheerde exemplaar metrische gegevens |
> | Bewerking | Microsoft.Sql/managedInstances/read | De lijst met beheerde-exemplaren of haalt de eigenschappen voor het opgegeven exemplaar van de beheerde retourneren. |
> | Bewerking | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Ophalen van gegevens van de afwijkingsdetectie van de beheerde server threat geconfigureerd op een bepaalde beheerde server |
> | Bewerking | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Het beleid voor detectie van bedreigingen beheerde server voor een bepaalde beheerde server wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/write | Hiermee maakt u een beheerde exemplaar met de opgegeven parameters of bijwerken van de eigenschappen of labels voor het opgegeven exemplaar van de beheerde. |
> | Bewerking | Microsoft.Sql/operations/read | Beschikbare REST-bewerkingen opgehaald |
> | Bewerking | Microsoft.Sql/register/action | Registreert het abonnement voor de resourceprovider voor Microsoft SQL-Database en wordt het maken van Microsoft SQL-Databases. |
> | Bewerking | Microsoft.Sql/servers/administratorOperationResults/read | Opgehaald in uitvoering bewerkingen op de server-beheerders |
> | Bewerking | Microsoft.Sql/servers/administrators/delete | Verwijderen van server-beheerder |
> | Bewerking | Microsoft.Sql/servers/administrators/read | Administrator-servergegevens ophalen |
> | Bewerking | Microsoft.Sql/servers/administrators/write | Maken of bijwerken van de serverbeheerder |
> | Bewerking | Microsoft.Sql/servers/advisors/read | Retourneert de lijst met adviseurs beschikbaar voor de server |
> | Bewerking | Microsoft.Sql/servers/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de server |
> | Bewerking | Microsoft.Sql/servers/advisors/recommendedActions/write | De aanbevolen actie toepassen op de server |
> | Bewerking | Microsoft.Sql/servers/advisors/write | Updates execute automatisch-status van een advisor op serverniveau. |
> | Bewerking | Microsoft.Sql/servers/auditingPolicies/read | Ophalen van gegevens van de tabel met standaardtijden server controlebeleid geconfigureerd op een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/auditingPolicies/write | Controle van wijzigingen in de standaard server tabel voor een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/operationResults/read | Resultaat van de server blob controle beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/read | Ophalen van gegevens van de server blob controlebeleid geconfigureerd op een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/write | De server blob-controle voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/automaticTuning/read | Retourneert automatische afstemming van instellingen voor de server |
> | Bewerking | Microsoft.Sql/servers/automaticTuning/write | Prestatieafstemming instellingen voor de server voor automatische updates en bijgewerkte instellingen retourneert |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Hiermee verwijdert u een bestaande back-upkluis archivering. |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Deze bewerking wordt gebruikt voor het ophalen van een back-up langdurig bewaren kluis. Gegevens over de kluis geregistreerd met deze server worden geretourneerd |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Deze bewerking wordt gebruikt voor het registreren van een back-up langdurig bewaren kluis naar een server |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/delete | Hiermee verwijdert u een bestaande koppeling van de server-communicatie. |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/read | De lijst met communicatiekoppelingen van de opgegeven server retourneren. |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/write | Maken of bijwerken van een server communicatiekanaal. |
> | Bewerking | Microsoft.Sql/servers/connectionPolicies/read | De lijst met beleidsregels voor server-verbinding van een opgegeven server retourneren. |
> | Bewerking | Microsoft.Sql/servers/connectionPolicies/write | Maken of bijwerken van een beleid van server-verbinding. |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/read | Retourneert de lijst met adviseurs die beschikbaar zijn voor de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van de opgegeven advisor voor de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | De aanbevolen actie toepassen op de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/write | Update automatisch-execute status van een advisor op databaseniveau. |
> | Bewerking | Microsoft.Sql/servers/databases/auditingPolicies/read | Ophalen van gegevens van de tabel controlebeleid geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/auditingPolicies/write | De tabel controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/auditingSettings/read | Ophalen van gegevens van de blob-controlebeleid geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/auditingSettings/write | De blob controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/auditRecords/read | De database blob-controlerecords ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/automaticTuning/read | Retourneert automatische afstemming van instellingen voor een database |
> | Bewerking | Microsoft.Sql/servers/databases/automaticTuning/write | Instellingen voor automatische afstemmen voor een database-updates en bijgewerkte instellingen retourneert |
> | Bewerking | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | De lijst met back-beleid voor archivering van een opgegeven database geretourneerd. |
> | Bewerking | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Maken of bijwerken van een database archiveringsbeleid voor de back-up. |
> | Bewerking | Microsoft.Sql/servers/databases/connectionPolicies/read | Ophalen van gegevens van het verbindingsbeleid is geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/connectionPolicies/write | Verbindingsbeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | De lijst van databasegegevens maskeren beleidsregels retourneren. |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Gegevensmaskering beleidsregel voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Ophalen van gegevens van de gegevens die zijn geconfigureerd op een bepaalde database beleidsregel maskeren |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Gegevensmaskering beleidsregel voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Beleid voor een bepaalde database gegevensmaskering wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Retourneert de gegevens van de gedistribueerde query-stap van de datawarehouse-query voor de ID voor de geselecteerde stap |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Retourneert het datawarehouse distributie query-gegevens voor geselecteerde query-ID |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Haalt de gebruikersactiviteiten van een exemplaar van SQL Data Warehouse waaronder query's uitgevoerd en onderbroken |
> | Bewerking | Microsoft.Sql/servers/databases/delete | Hiermee verwijdert u een bestaande database. |
> | Bewerking | Microsoft.Sql/servers/databases/export/action | Exporteren van Azure SQL Database |
> | Bewerking | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Ophalen van gegevens van de uitgebreide blob controlebeleid geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | De uitgebreide blob controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/extensions/read | Hiermee wordt een verzameling van uitbreidingen voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/extensions/write | De extensie voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Back-upbeleid voor een bepaalde database geo ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Een database geobackup beleid maken of bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/importExportOperationResults/read | Bewerkingen voor opgehaald in uitvoering voor importeren/exporteren |
> | Bewerking | Microsoft.Sql/servers/databases/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.Sql/servers/databases/metrics/read | Metrische gegevens voor de databases terug |
> | Bewerking | Microsoft.Sql/servers/databases/move/action | Wijzig de naam van Azure SQL Database |
> | Bewerking | Microsoft.Sql/servers/databases/operationResults/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/servers/databases/operations/cancel/action | Hiermee annuleert Azure SQL Database in behandeling zijnde asynchrone bewerking die nog niet is voltooid. |
> | Bewerking | Microsoft.Sql/servers/databases/operations/read | De lijst met bewerkingen die worden uitgevoerd op de database terug |
> | Bewerking | Microsoft.Sql/servers/databases/pause/action | Onderbreken Azure SQL Datawarehouse-Database |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor databases |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Retourneert de verzameling van query teksten die overeenkomen met de opgegeven parameters. |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/read | Retourneert de huidige waarden van de Query Store-instellingen voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/write | Query Store-instelling voor de database-updates |
> | Bewerking | Microsoft.Sql/servers/databases/read | De lijst met databases of haalt de eigenschappen voor de opgegeven database geretourneerd. |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/delete | Beëindigen van de replicatierelatie geforceerd en met mogelijk gegevensverlies |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Failover na het synchroniseren van alle wijzigingen van de primaire waardoor deze database in de replicatie-relationship\u0027s primaire en het aanbrengen van de externe primaire naar een secundair |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Failover onmiddellijk met mogelijk gegevensverlies, waardoor deze database in de replicatie-relationship\u0027s primaire en het aanbrengen van de externe primaire naar een secundair |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/read | Geretourneerde gegevens over koppelingen voor databasereplicatie tot stand gebracht voor een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Beëindigen van de replicatierelatie geforceerd of na het synchroniseren met de partner |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Replicatiemodus voor koppeling naar de modus voor synchroon of asynchroon bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/action | Hiermee wordt een nieuw herstelpunt gemaakt |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/delete | Hiermee verwijdert u een herstelpunt voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/read | Retourneert de herstelpunten voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/resume/action | Azure SQL Datawarehouse-Database hervatten |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/read | Ophalen van lijst met schema's van een database |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Ophalen van lijst met kolommen van een tabel |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Het gevoeligheidslabel van een bepaalde kolom verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Het gevoeligheidslabel van een bepaalde kolom ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Maken of bijwerken van het gevoeligheidslabel van een bepaalde kolom |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/read | Ophalen van lijst met tabellen van een database |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Lijst met indexaanbevelingen voor een database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Indexaanbeveling toepassen |
> | Bewerking | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Ophalen van gegevens van de threat beleid geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Het beleid van de detectie van bedreigingen voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/securityMetrics/read | Hiermee wordt een verzameling van database beveiliging metrische gegevens |
> | Bewerking | Microsoft.Sql/servers/databases/sensitivityLabels/read | Lijst gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Database schalen omhoog of omlaag wordt voorgesteld geretourneerd op basis van statistieken van de query kan worden uitgevoerd op de prestaties verbeteren of kosten |
> | Bewerking | Microsoft.Sql/servers/databases/skus/read | Hiermee wordt een verzameling van SKU's beschikbaar voor een database |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Synchronisatie groepssynchronisatie annuleren |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/delete | Hiermee verwijdert u een bestaande groep voor synchronisatie. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | De lijst met synchronisatie hub databaseschema retourneren |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/logs/read | De lijst met synchronisatielogboeken groep retourneren |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/read | De lijst met synchronisatie groepen of haalt u de eigenschappen voor de groep voor synchronisatie opgegeven retourneren. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Synchronisatie hub databaseschema vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Resultaat van de synchronisatiebewerking hub schema vernieuwen ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Hiermee verwijdert u een bestaand lid van de synchronisatie. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | De lijst met leden van de synchronisatie of haalt de eigenschappen voor een lid van de opgegeven synchronisatie retourneren. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Synchronisatie lid schema vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Resultaat van de synchronisatiebewerking lid schema vernieuwen ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | De lijst met synchronisatie lid databaseschema retourneren |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Een lid van de synchronisatie maakt met de opgegeven parameters of bijwerken van de eigenschappen voor de opgegeven synchronisatie-lid. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Groepssynchronisatie voor trigger sync |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/write | Hiermee maakt u een groep voor synchronisatie met de opgegeven parameters of bijwerken van de eigenschappen voor de groep voor synchronisatie opgegeven. |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/queryText/action | Retourneert de Transact-SQL-tekst voor de geselecteerde query-ID |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/read | Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/statistics/read | Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Opgehaald in uitvoering bewerkingen op transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Status en details van transparent data encryption beveiligingsfunctie voor een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Transparent data encryption status wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Azure SQL Datawarehouse-Database bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/usages/read | Hiermee haalt u de informatie van Azure SQL Database vormen van gebruik |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | De controle op beveiligingslekken voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Ophalen van gegevens van de controle op beveiligingslekken geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | De basislijn vulnerability assessment regel voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Ophalen van de basislijn vulnerability assessment regel voor een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | De basislijn vulnerability assessment regel voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Een bestaand scan resultaat niet converteren naar een menselijke leesbare indeling. Als al er niets gebeurt |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Database-scan voor beoordeling van beveiligingsproblemen worden uitgevoerd. |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | De lijst met database vulnerability assessment scan records geretourneerd of ophalen van de scan-record voor de opgegeven scan-ID. |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | De controle op beveiligingslekken voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Database-scan voor beoordeling van beveiligingsproblemen worden uitgevoerd. |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Het resultaat van de database vulnerability assessment scan Execute-bewerking ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Ophalen van gegevens van de controle op beveiligingslekken geconfigureerd op een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | De controle op beveiligingslekken voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/write | Maakt een database met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven database. |
> | Bewerking | Microsoft.Sql/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Hiermee verwijdert u een bestaande noodherstelconfiguraties voor een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Failover een DisasterRecoveryConfiguration |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Een DisasterRecoveryConfiguration een failover |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Hiermee wordt een verzameling disaster recovery configuraties met deze server |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Wijziging noodherstelconfiguratie op de server |
> | Bewerking | Microsoft.Sql/servers/elasticPoolEstimates/read | Retourneert de lijst van de elastische groep schattingen al is gemaakt voor deze server |
> | Bewerking | Microsoft.Sql/servers/elasticPoolEstimates/write | Hiermee maakt u nieuwe elastische pool schatting voor de lijst met databases die zijn opgegeven |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/read | Retourneert de lijst met adviseurs die beschikbaar zijn voor de elastische groep |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de elastische groep |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | De aanbevolen actie toepassen op de elastische groep |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/write | Update automatisch-execute status van een advisor op niveau van de elastische groep. |
> | Bewerking | Microsoft.Sql/servers/elasticPools/databases/read | Een lijst met databases ophalen voor een elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/delete | Verwijderen van de huidige elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Ophalen van activiteiten en meer informatie over een bepaalde elastische databasegroep |
> | Bewerking | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Ophalen van activiteiten en meer informatie over een bepaalde database die deel uitmaakt van een pool voor elastische database |
> | Bewerking | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn |
> | Bewerking | Microsoft.Sql/servers/elasticPools/metrics/read | Retourneren van metrische gegevens voor pools voor elastische databases |
> | Bewerking | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Azure SQL elastische pool in behandeling zijnde asynchrone bewerking die nog niet is voltooid, wordt geannuleerd. |
> | Bewerking | Microsoft.Sql/servers/elasticPools/operations/read | Retourneren van de lijst met bewerkingen die worden uitgevoerd op de elastische groep |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn |
> | Bewerking | Microsoft.Sql/servers/elasticPools/read | Details van de elastische pool op een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/elasticPools/skus/read | Hiermee wordt een verzameling van SKU's beschikbaar voor een elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/write | Maak een nieuwe eigenschappen of wijzigen van de huidige elastische pool |
> | Bewerking | Microsoft.Sql/servers/encryptionProtector/read | Retourneert een lijst met server versleuteling beveiligingen of haalt u de eigenschappen voor de opgegeven server versleuteling beveiliging. |
> | Bewerking | Microsoft.Sql/servers/encryptionProtector/write | De eigenschappen voor de opgegeven Server versleuteling beveiligingstoepassing bijwerken. |
> | Bewerking | Microsoft.Sql/servers/extendedAuditingSettings/read | Ophalen van gegevens van de uitgebreide server blob controlebeleid geconfigureerd op een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/extendedAuditingSettings/write | De uitgebreide server blob-controle voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/delete | Hiermee verwijdert u een bestaande groep voor failover. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/failover/action | Geplande failover worden uitgevoerd in een bestaande groep voor failover. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Geforceerde failover worden uitgevoerd in een bestaande groep voor failover. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/read | Retourneert de lijst met failover groepen of haalt u de eigenschappen voor de opgegeven failover-groep. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/write | Hiermee maakt u een failover-groep met de opgegeven parameters of updates van de eigenschappen of labels voor de opgegeven failover-groep. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewallregel voor de server. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/read | De lijst met serverfirewall regels of haalt u de eigenschappen voor de opgegeven server firewallregel retourneren. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/write | Een serverfirewallregel maakt met de opgegeven parameters bijwerken van de eigenschappen voor de opgegeven regel of alle bestaande regels worden overschreven met de nieuwe server firewall (s). |
> | Bewerking | Microsoft.Sql/servers/import/action | Een nieuwe database op de server maken en implementeren van schema en de gegevens uit een DacPac-pakket |
> | Bewerking | Microsoft.Sql/servers/importExportOperationResults/read | Bewerkingen voor opgehaald in uitvoering voor importeren/exporteren |
> | Bewerking | Microsoft.Sql/servers/keys/delete | Hiermee verwijdert u een bestaande serversleutel. |
> | Bewerking | Microsoft.Sql/servers/keys/read | De lijst van server-codes of haalt u de eigenschappen voor de sleutel van de opgegeven server retourneren. |
> | Bewerking | Microsoft.Sql/servers/keys/write | Maakt een sleutel met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de sleutel van de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/networkInterfaces/read | Retourneert de eigenschappen voor de opgegeven netwerkinterface |
> | Bewerking | Microsoft.Sql/servers/networkInterfaces/write | Een netwerkinterface maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven netwerkinterface |
> | Bewerking | Microsoft.Sql/servers/operationResults/read | Opgehaald in uitvoering serverbewerkingen |
> | Bewerking | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Retourtypen van metrische gegevens die beschikbaar voor servers zijn |
> | Bewerking | Microsoft.Sql/servers/read | Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald. |
> | Bewerking | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Metrische gegevens voor aanbevolen elastische databases voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/recommendedElasticPools/read | Aanbevelingen voor pools voor elastische databases kosten verlagen of verbeteren de prestaties op basis van Resourcegebruik historica ophalen |
> | Bewerking | Microsoft.Sql/servers/recoverableDatabases/read | Deze bewerking wordt gebruikt voor herstel na noodgevallen van live-database voor de database herstellen naar laatst bekende goede back-up. Deze retourneert informatie over de laatste goede back-up, maar doesn\u0027t daadwerkelijk de database herstellen. |
> | Bewerking | Microsoft.Sql/servers/restorableDroppedDatabases/read | Een lijst met databases die zijn verwijderd op een bepaalde server die binnen het bewaarbeleid ophalen. |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Resultaten van de server threat detectie beleid write-bewerking ophalen |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/read | Ophalen van gegevens van het beleid server threat detectie is geconfigureerd op een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/write | Het beleid van server threat detectie voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/serviceObjectives/read | Lijst met serviceniveaudoelstellingen (ook wel bekend als prestatielagen) beschikbaar is op een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/syncAgents/delete | Hiermee verwijdert u een bestaande sync-agent. |
> | Bewerking | Microsoft.Sql/servers/syncAgents/generateKey/action | Sync-agent registratie sleutel genereren |
> | Bewerking | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Retourneren van de lijst met databases synchroniseren-agent die zijn gekoppeld |
> | Bewerking | Microsoft.Sql/servers/syncAgents/read | Retourneert de lijst met synchronisatie agents of haalt de eigenschappen voor de opgegeven synchronisatie-agent. |
> | Bewerking | Microsoft.Sql/servers/syncAgents/write | Hiermee maakt u een synchronisatie-agent met de opgegeven parameters of bijwerken van de eigenschappen voor de opgegeven synchronisatie-agent. |
> | Bewerking | Microsoft.Sql/servers/usages/read | DTU-quotum voor server en de huidige DTU consuption geretourneerd door alle databases in de server |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/read | De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren. |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/write | Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.Sql/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |
> | Bewerking | Microsoft.Sql/unregister/action | Heft de registratie van het abonnement voor de resourceprovider voor Microsoft SQL-Database en wordt het maken van Microsoft SQL-Databases. |
> | Bewerking | Microsoft.Sql/virtualClusters/read | De lijst met virtuele clusters of haalt de eigenschappen voor de opgegeven virtuele cluster retourneren. |
> | Bewerking | Microsoft.Sql/virtualClusters/write | Virtueel cluster tags-updates. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Storage/checknameavailability/read | Hiermee controleert u of een accountnaam geldig is en niet wordt gebruikt. |
> | Bewerking | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.Storage doorgegeven dat het virtuele netwerk of subnet wordt verwijderd |
> | Bewerking | Microsoft.Storage/operations/read | Hiermee wordt de status van een asynchrone bewerking gepeild. |
> | Bewerking | Microsoft.Storage/register/action | Hiermee wordt het abonnement voor de opslagresourceprovider geregistreerd en wordt het maken van opslagaccounts mogelijk. |
> | Bewerking | Microsoft.Storage/skus/read | Hiermee wordt een lijst weergegeven van de SKU's die worden ondersteund door Microsoft.Storage. |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Juridische bewaring van blobcontainer wissen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Hiermee wordt het resultaat van het verwijderen van een container geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Beleid voor onveranderbaarheid van blobcontainer verwijderen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Beleid voor onveranderbaarheid van blobcontainer uitbreiden |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Beleid voor onveranderbaarheid van blobcontainer vergrendelen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Beleid voor onveranderbaarheid van blobcontainer ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Beleid voor onveranderbaarheid van blobcontainer plaatsen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermee wordt een container of een lijst containers geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Juridische bewaring voor blobcontainer instellen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/write | Hiermee wordt het resultaat van het plaatsen of in lease geven van de blobcontainer geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read | Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op. |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/read | Hiermee worden de eigenschappen van de Blob service of statistieken geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/write | Hiermee wordt het resultaat van de eigenschappen voor het plaatsen van de blobcontainer geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/delete | Hiermee verwijdert u een bestaand opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read | Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op. |
> | Bewerking | Microsoft.Storage/storageAccounts/listAccountSas/action | Hiermee wordt het SAS-token van het account geretourneerd voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/listServiceSas/action | Hiermee wordt het SAS-token van de service geretourneerd voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read | Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read | Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Hiermee wordt het resultaat van een wachtrij verwijderen geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/read | Hiermee wordt een wachtrij of een lijst met wachtrijen geretourneerd. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/write | Hiermee wordt het resultaat van het schrijven van een wachtrij geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/read | Hiermee worden de eigenschappen of statistieken van de Queue-service geretourneerd. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/write | Hiermee wordt het resultaat van het instellen van de eigenschappen van de Queue-service geretourneerd |
> | Bewerking | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/regeneratekey/action | Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw. |
> | Bewerking | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Diagnostische instellingen van opslagaccount maken/bijwerken. |
> | Bewerking | Microsoft.Storage/storageAccounts/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald. |
> | Bewerking | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt. |
> | Bewerking | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read | Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op. |
> | Bewerking | Microsoft.Storage/storageAccounts/write | Hiermee maakt u een opslagaccount met de opgegeven parameters, werkt u de eigenschappen of labels bij of voegt u een aangepast domein toe aan het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/usages/read | Retourneert de limiet en het huidige gebruik voor resources in het opgegeven abonnement |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | microsoft.storagesync/storageSyncServices/delete | Verwijderen van alle opslag-Synchronisatieservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor synchronisatie van opslagservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/read | Lezen van alle opslag-Synchronisatieservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/delete | Verwijder alle geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/read | Lees alle geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/write | Maken of bijwerken van een geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Geen eindpunten Cloud verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Api voor back-aanroepen van asynchrone locatie |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Aanroepen van deze actie na back-up |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Deze actie na het terugzetten van aanroepen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Aanroepen van deze actie voor back-up |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Aanroepen van deze actie vóór herstellen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Lezen van elke Cloud-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Heartbeat herstellen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Maken of bijwerken van elke Cloud-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/delete | Synchronisatiegroepen verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/read | Alle synchronisatiegroepen lezen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Geen eindpunten Server verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Lezen van elke Server-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Aanroepen van deze actie om in te trekken van de bestanden naar een server |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Maken of bijwerken van elke Server-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/write | Een synchronisatiegroepen maken of bijwerken |
> | Bewerking | microsoft.storagesync/storageSyncServices/write | Alle opslag Synchronisatieservices maken of bijwerken |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/delete | De Access Control Records verwijderd |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/read | Geeft een lijst of de Access Control Records opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/write | De Access Control Records maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/managers/alerts/read | Geeft een lijst of de waarschuwingen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/delete | Hiermee verwijdert u een bestaande bandbreedte-instellingen (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/read | Lijst van de bandbreedte-instellingen (alleen 8000 serie) |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/write | Een nieuwe maken of bijwerken van de bandbreedte-instellingen (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/Managers/certificates/write | De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis. |
> | Bewerking | Microsoft.StorSimple/managers/clearAlerts/action | Schakel alle waarschuwingen die zijn gekoppeld aan de Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Lijst met het toestel Cloud ondersteunde configuraties |
> | Bewerking | Microsoft.StorSimple/managers/configureDevice/action | Hiermee configureert u een apparaat |
> | Bewerking | Microsoft.StorSimple/managers/delete | Hiermee verwijdert u de Managers voor apparaatregistratie |
> | Bewerking | Microsoft.StorSimple/Managers/delete | De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis' |
> | Bewerking | Microsoft.StorSimple/managers/devices/alertSettings/read | Geeft een lijst of opgehaald van de instellingen voor waarschuwingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/alertSettings/write | Maken of bijwerken van de instellingen voor waarschuwingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Een handmatige back-up op aanvraag maken back-up van alle volumes die zijn beveiligd door het beleid te nemen. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Hiermee verwijdert u een beleid in een bestaande back-up (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/read | Lijst met de back-up beleidsregels (alleen 8000 serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Hiermee verwijdert u een bestaande planningen |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Lijst van de planning |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Maakt een nieuw of bijgewerkt schema 's |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/write | Maakt een nieuw of bijgewerkt beleid van de back-up (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/delete | Hiermee verwijdert u de back-upset |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Klonen van een share of het volume met behulp van een back-element. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/read | Geeft een lijst of back-upset opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/restore/action | Alle volumes terugzetten vanaf een back-upset. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Hiermee verwijdert u de back-upschema-groepen |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Geeft een lijst of de back-up schemagroepen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | De back-up schemagroepen maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/delete | Hiermee verwijdert u de Chap-instellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/read | Geeft een lijst of ontvangt de Chap-instellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/write | De Chap-instellingen maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/managers/devices/deactivate/action | Een apparaat wordt gedeactiveerd. |
> | Bewerking | Microsoft.StorSimple/managers/devices/delete | Hiermee verwijdert u de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/devices/download/action | Download updates voor een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/failover/action | Failover van het apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Duren back-up van een bestandsserver. |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/delete | Hiermee verwijdert u de bestandsservers |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/read | Geeft een lijst of opgehaald van de bestandsservers |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Hiermee verwijdert u de Shares |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Geeft een lijst of de Shares opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Maken of bijwerken van de Shares |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/write | Maken of bijwerken van de bestandsservers |
> | Bewerking | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Controller voedingsstatus van hardware-onderdeelgroepen wijzigen |
> | Bewerking | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Lijst van de Hardware-onderdeelgroepen |
> | Bewerking | Microsoft.StorSimple/managers/devices/install/action | Updates installeren op een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/installUpdates/action | Updates zijn geïnstalleerd op de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Duren back-up van een iSCSI-server. |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Hiermee verwijdert u de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Hiermee verwijdert u de schijven |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Geeft een lijst of de schijven opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Maken of bijwerken van de schijven |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/read | Geeft een lijst of opgehaald van de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/write | Maken of bijwerken van de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Een actieve taak annuleren |
> | Bewerking | Microsoft.StorSimple/managers/devices/jobs/read | Geeft een lijst of de taken opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Hiermee stelt u de failover voor een bestaand apparaat een lijst. |
> | Bewerking | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Doelen voor de failover van de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/devices/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Bevestigt een succesvolle migratie en het doorvoeren. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Haal de status van het bevestigen van de migratie. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | De status van de migratietaak schatting ophalen. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Haal de status van de migratie. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Importeren van de bron-configuraties voor migratie |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Begint met de migratie met behulp van de bron-configuraties |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Een taak voor een schatting van de duur van het migratieproces start. |
> | Bewerking | Microsoft.StorSimple/managers/devices/networkSettings/read | Geeft een lijst of de netwerkinstellingen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/networkSettings/write | Een nieuwe maken of bijwerken van netwerkinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Openbare versleutelingssleutel lijst van Apparaatbeheer |
> | Bewerking | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Publiceer ondersteuningspakket van een apparaat voor het oplossen van Microsoft Support. |
> | Bewerking | Microsoft.StorSimple/managers/devices/read | Geeft een lijst of de apparaten opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Zoeken naar updates in een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/read | Lijst van de beveiligingsinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Het certificaat voor extern beheer voor een apparaat worden gesynchroniseerd. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Werk de beveiligingsinstellingen. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/write | Maakt een nieuw of bijgewerkt beveiligingsinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Waarschuwing testbericht verzenden naar geconfigureerde e-mailontvangers. |
> | Bewerking | Microsoft.StorSimple/managers/devices/timeSettings/read | Geeft een lijst of de instellingen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/timeSettings/write | Maakt een nieuw of bijgewerkt tijdinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/updateSummary/read | Geeft een lijst of een overzicht van de Update wordt opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Hiermee verwijdert u een bestaande Volumecontainers (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/listEncryptionKeys/action | De versleutelingssleutels lijst van Volumecontainers |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Lijst van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | De definities van de metrische gegevens weergeven |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/read | Lijst van de Volumecontainers (alleen 8000 serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/rolloverEncryptionKey/action | De versleutelingssleutels rollover van Volumecontainers |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Hiermee verwijdert u een bestaande Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Lijst van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | De definities van de metrische gegevens weergeven |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Lijst van de Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Een nieuwe maken of bijwerken van Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/write | Maakt een nieuw of bijgewerkt Volumecontainers (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/write | Maken of bijwerken van de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/encryptionSettings/read | Geeft een lijst of de instellingen voor codering opgehaald |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/delete | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/managers/getActivationKey/action | Activeringscode voor Apparaatbeheer niet ophalen. |
> | Bewerking | Microsoft.StorSimple/managers/getEncryptionKey/action | Download coderingssleutel voor Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/listActivationKey/action | Hiermee haalt u de activeringssleutel van de StorSimple-Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/listPrivateEncryptionKey/action | Persoonlijke sleutel voor een StorSimple-Apparaatbeheer opgehaald. |
> | Bewerking | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Lijst met codering met openbare sleutels van een StorSimple-Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/metrics/read | Geeft een lijst of opgehaald van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/metricsDefinitions/read | Geeft een lijst of de definities van de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Maak een nieuw toestel in de cloud. |
> | Bewerking | Microsoft.StorSimple/managers/read | Geeft een lijst of de apparaat-Managers opgehaald |
> | Bewerking | Microsoft.StorSimple/Managers/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/regenarateRegistationCertificate/action | Registratiecertificaat opnieuw genereren voor de managers voor apparaatregistratie. |
> | Bewerking | Microsoft.StorSimple/managers/regenerateActivationKey/action | Activeringscode genereren voor Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Hiermee verwijdert u de Opslagaccountreferenties |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/listAccessKey/action | Toegangstoetsen lijst van Opslagaccountreferenties |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/read | Geeft een lijst of de Opslagaccountreferenties opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/write | Maken of bijwerken van de Opslagaccountreferenties |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/delete | Hiermee verwijdert u de opslag-domeinen |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/read | Geeft een lijst of de opslag domeinen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/write | Maken of bijwerken van de opslag-domeinen |
> | Bewerking | Microsoft.StorSimple/managers/write | Maken of bijwerken van de Managers voor apparaatregistratie |
> | Bewerking | Microsoft.StorSimple/Managers/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.StreamAnalytics/locations/quotas/Read | Quotum lezen Stream Analytics-abonnement |
> | Bewerking | Microsoft.StreamAnalytics/operations/Read | Lees Stream Analytics-bewerkingen |
> | Bewerking | Microsoft.StreamAnalytics/Register/action | Abonnement met Stream Analytics Resource Provider registreren |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Delete | Verwijderen van de Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Verwijderen van de Stream Analytics-functie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Resultaat van de bewerking voor de functie voor Stream Analytics-taak lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Functie Read Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Default-definitie van een functie van Stream Analytics-taak ophalen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Test Stream Analytics-functie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Stream Analytics-taakfunctie schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Stream Analytics-taak invoer verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Resultaat van de bewerking voor invoer van Stream Analytics-taak lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Invoer lezen Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Voorbeeldinvoer Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Test Stream Analytics-taak invoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Stream Analytics-taak invoer schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Resultaat van de bewerking voor Stream Analytics-taak lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Stream Analytics-Taakuitvoer verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Resultaat van de bewerking voor Stream Analytics-Taakuitvoer lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Lees Stream Analytics-Taakuitvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Testuitvoer Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Stream Analytics-Taakuitvoer schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Lees de diagnostische instelling. |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Diagnostische instelling schrijven. |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor streamingjobs |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor streamingjobs |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Read | Stream Analytics-taak lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Start/action | Stream Analytics-taak starten |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stream Analytics-taak stoppen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Stream Analytics-taak transformatie verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Transformatie lezen Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Stream Analytics-taak transformatie schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Write | Schrijven van Stream Analytics-taak |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Subscription/CreateSubscription/action | Maak een Azure-abonnement |
> | Bewerking | Microsoft.Subscription/SubscriptionDefinitions/read | De definitie van een Azure-abonnement in een beheergroep worden opgehaald. |
> | Bewerking | Microsoft.Subscription/SubscriptionDefinitions/write | De definitie van een Azure-abonnement maken |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Support/register/action | Hiermee wordt het item geregistreerd bij de ondersteuningsresourceprovider |
> | Bewerking | Microsoft.Support/supportTickets/read | Hiermee worden gegevens over het ondersteuningsticket opgehaald (inclusief status, ernst, contactgegevens en communicatie) of wordt de lijst met ondersteuningstickets voor de verschillende abonnementen opgehaald. |
> | Bewerking | Microsoft.Support/supportTickets/write | Maken of bijwerken van een Ondersteuningsticket. U kunt een Ondersteuningsticket maken voor technische, facturerings, quota's of beheer van abonnementen problemen met betrekking tot. U kunt de ernst, contactgegevens en communicatie voor bestaande ondersteuningstickets bijwerken. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Hiermee verwijdert u het toegangsbeleid. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | De eigenschappen van een toegangsbeleid opgehaald. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Maakt u een nieuw toegangsbeleid voor een omgeving of updates van een bestaande toegangsbeleid. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/delete | Hiermee verwijdert u de omgeving. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Hiermee verwijdert u de gegevensbron. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/eventsources/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor eventsources |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/read | De eigenschappen van een gebeurtenisbron opgehaald. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/write | Maakt een nieuwe gebeurtenisbron voor een omgeving, of een bestaande gebeurtenisbron bijgewerkt. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de beschikbare metrische gegevens voor omgevingen |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/read | De eigenschappen van een omgeving opgehaald. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Hiermee verwijdert u de referentie-gegevensset. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | De eigenschappen van een gegevensset verwijzing opgehaald. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Hiermee maakt u een nieuwe verwijzing gegevens voor een omgeving of updates van een bestaande referentie-gegevensset. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/status/read | De status van de status van de gekoppelde bewerkingen zoals ingress-omgeving niet ophalen. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/write | Maakt u een nieuwe omgeving of updates van een bestaande omgeving. |
> | Bewerking | Microsoft.TimeSeriesInsights/register/action | Het abonnement voor de Time Series Insights-resourceprovider geregistreerd en wordt het maken van tijd reeks Insights omgevingen. |

## <a name="microsoftvisualstudio"></a>Microsoft.visualstudio

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.VisualStudio/Account/Delete | Account verwijderen |
> | Bewerking | Microsoft.VisualStudio/Account/Read | Account lezen |
> | Bewerking | Microsoft.VisualStudio/Account/Write | Account instellen |
> | Bewerking | Microsoft.VisualStudio/Extension/Delete | Verwijderen van extensie |
> | Bewerking | Microsoft.VisualStudio/Extension/Read | Extensie lezen |
> | Bewerking | Microsoft.VisualStudio/Extension/Write | Extensie instellen |
> | Bewerking | Microsoft.VisualStudio/Project/Delete | Project verwijderen |
> | Bewerking | Microsoft.VisualStudio/Project/Read | Lees-Project |
> | Bewerking | Microsoft.VisualStudio/Project/Write | Project instellen |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.Web/apimanagementaccounts/apiacls/Read | Api Management Accounts Apiacls ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/apiacls/DELETE | Api Management Accounts API's Apiacls verwijderen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/apiacls/Read | Api Management Accounts API's Apiacls ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/apiacls/Write | Api Management Accounts API's Apiacls bijwerken. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/connectionacls/Read | Api Management Accounts API's Connectionacls ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/confirmconsentcode/Action | Controleer of u toestemming Code Api Management Accounts API's verbindingen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/DELETE | Api Management Accounts API's verbindingen Connectionacls verwijderen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/Read | Api Management Accounts API's verbindingen Connectionacls ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/connectionacls/Write | Api Management Accounts API's verbindingen Connectionacls bijwerken. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/DELETE | Api Management Accounts API's verbindingen verwijderen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/getconsentlinks/Action | Toestemming koppelingen voor verbindingen van Api Management Accounts API's niet ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/listconnectionkeys/Action | Lijst verbinding sleutels Api Management Accounts API's verbindingen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/listsecrets/Action | Lijst met geheimen Api Management Accounts API's verbindingen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/Read | Api Management Accounts API's verbindingen ophalen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Connections/Write | Api Management Accounts API's verbindingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/DELETE | Api Management-API's Accounts verwijderen. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/DELETE | Verwijderen van Api Management-API's Accounts gelokaliseerd definities. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/Read | Ophalen van Api Management-API's Accounts gelokaliseerd definities. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/localizeddefinitions/Write | Update Api Management Accounts-API's gelokaliseerd definities. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Read | Api Management-API's Accounts worden opgehaald. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/APIs/Write | Api Management-API's Accounts bijwerken. |
> | Bewerking | Microsoft.Web/apimanagementaccounts/connectionacls/Read | Api Management Accounts Connectionacls ophalen. |
> | Bewerking | Microsoft.Web/availablestacks/Read | Beschikbare Stacks ophalen. |
> | Bewerking | Microsoft.Web/billingmeters/Read | Lijst met meters facturering ophalen. |
> | Bewerking | Microsoft.Web/certificates/Delete | Een bestaand certificaat verwijderen. |
> | Bewerking | Microsoft.Web/certificates/Read | Haal de lijst van certificaten. |
> | Bewerking | Microsoft.Web/certificates/Write | Voeg een nieuw certificaat of een bestaande bijgewerkt. |
> | Bewerking | Microsoft.Web/checknameavailability/Read | Controleer of de resourcenaam van de beschikbaar is. |
> | Bewerking | Microsoft.Web/classicmobileservices/Read | Klassieke mobiele Services krijgen. |
> | Bewerking | Microsoft.Web/connectionGateways/Delete | Hiermee verwijdert u een Gateway verbinding. |
> | Bewerking | Microsoft.Web/connectionGateways/Join/Action | Een Gateway verbinding koppelt. |
> | Bewerking | Microsoft.Web/connectiongateways/liststatus/Action | Lijst met Status verbinding Gateways. |
> | Bewerking | Microsoft.Web/connectionGateways/ListStatus/Action | Hiermee wordt de status van een Gateway-verbinding. |
> | Bewerking | Microsoft.Web/connectionGateways/Move/Action | Hiermee verplaatst u een Gateway verbinding. |
> | Bewerking | Microsoft.Web/connectionGateways/Read | De lijst van Gateways verbinding ophalen. |
> | Bewerking | Microsoft.Web/connectionGateways/Write | Maken of bijwerken van een Gateway-verbinding. |
> | Bewerking | Microsoft.Web/Connections/confirmconsentcode/Action | Controleer de verbindingen toestemming Code. |
> | Bewerking | Microsoft.Web/connections/Delete | Hiermee verwijdert u een verbinding. |
> | Bewerking | Microsoft.Web/connections/Join/Action | Lid van een verbinding. |
> | Bewerking | Microsoft.Web/Connections/listconsentlinks/Action | Lijst met toestemming koppelingen voor verbindingen. |
> | Bewerking | Microsoft.Web/connections/Move/Action | Hiermee verplaatst u een verbinding. |
> | Bewerking | Microsoft.Web/connections/Read | De lijst met verbindingen opgehaald. |
> | Bewerking | Microsoft.Web/connections/Write | Maken of bijwerken van een verbinding. |
> | Bewerking | Microsoft.Web/customApis/Delete | Hiermee verwijdert u een aangepaste API gebruiken. |
> | Bewerking | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Haalt de API-definitie van een WSDL. |
> | Bewerking | Microsoft.Web/customApis/Join/Action | Een aangepaste API koppelt. |
> | Bewerking | Microsoft.Web/customApis/listWsdlInterfaces/Action | Geeft een lijst van WSDL-interfaces voor een aangepaste API. |
> | Bewerking | Microsoft.Web/customApis/Move/Action | Hiermee verplaatst u een aangepaste API gebruiken. |
> | Bewerking | Microsoft.Web/customApis/Read | De lijst met aangepaste API ophalen. |
> | Bewerking | Microsoft.Web/customApis/Write | Maken of bijwerken van een aangepaste API. |
> | Bewerking | Microsoft.Web/deploymentlocations/Read | Implementatie locaties ophalen. |
> | Bewerking | Microsoft.Web/geoRegions/Read | De lijst met Geo regions ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/capacities/Read | Hosting-omgevingen capaciteiten ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/Delete | Een App-serviceomgeving verwijderen |
> | Bewerking | Microsoft.Web/hostingenvironments/Diagnostics/Read | Hosting-omgevingen diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/inboundnetworkdependenciesendpoints/Read | De netwerkeindpunten van alle inkomende afhankelijkheden ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/metricdefinitions/Read | Hosting-omgevingen metrische definities ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/multirolepools/metricdefinitions/Read | Hosting-omgevingen MultiRole Pools metrische definities ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/multirolepools/metrics/Read | Hosting-omgevingen MultiRole Pools metrische gegevens ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/metricDefinitions/Read | Hiermee worden de beschikbare metrische gegevens voor App Service-omgeving MultiRole opgehaald |
> | Bewerking | Microsoft.Web/hostingEnvironments/multiRolePools/Read | De eigenschappen van een FrontEnd-toepassingen in een App-serviceomgeving opgehaald |
> | Bewerking | Microsoft.Web/hostingenvironments/multirolepools/skus/Read | Hosting-omgevingen MultiRole Pools SKU's ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/multirolepools/usages/Read | Hosting-omgevingen MultiRole Pools gebruik ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Een nieuwe FrontEnd-toepassingen in een App Service-omgeving maken of bijwerken van een bestaande |
> | Bewerking | Microsoft.Web/hostingenvironments/Operations/Read | Hosting-omgevingen bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/outboundnetworkdependenciesendpoints/Read | De netwerkeindpunten van alle uitgaande afhankelijkheden ophalen. |
> | Bewerking | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Web/hostingEnvironments/Read | De eigenschappen van een App-serviceomgeving opgehaald |
> | Bewerking | Microsoft.Web/hostingEnvironments/reboot/Action | Start opnieuw op alle machines in een App-serviceomgeving |
> | Bewerking | Microsoft.Web/hostingenvironments/Resume/Action | Hervat de hostomgevingen. |
> | Bewerking | Microsoft.Web/hostingenvironments/serverfarms/Read | Hosting-omgevingen App Service-abonnementen worden opgehaald. |
> | Bewerking | Microsoft.Web/hostingenvironments/sites/Read | Hosting-omgevingen Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/hostingenvironments/Suspend/Action | Onderbreken hostomgevingen. |
> | Bewerking | Microsoft.Web/hostingenvironments/usages/Read | Hosting-omgevingen gebruik ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/workerpools/metricdefinitions/Read | Hosting-omgevingen Workerpools metriek definities ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/workerpools/metrics/Read | Hosting-omgevingen Workerpools metrische gegevens ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read | Hiermee worden de beschikbare metrische gegevens voor App Service-omgeving WorkerPool opgehaald |
> | Bewerking | Microsoft.Web/hostingEnvironments/workerPools/Read | De eigenschappen van een werknemersgroep in een App-serviceomgeving opgehaald |
> | Bewerking | Microsoft.Web/hostingenvironments/workerpools/skus/Read | Hosting-omgevingen Workerpools SKU's ophalen. |
> | Bewerking | Microsoft.Web/hostingenvironments/workerpools/usages/Read | Hosting-omgevingen Workerpools gebruik ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/workerPools/Write | Een nieuwe Worker-groep in een App Service-omgeving maken of bijwerken van een bestaande |
> | Bewerking | Microsoft.Web/hostingEnvironments/Write | Een nieuwe App Service-omgeving maken of bijwerken van bestaande |
> | Bewerking | Microsoft.Web/ishostingenvironmentnameavailable/Read | Get als de naam van de Hosting-omgeving beschikbaar is. |
> | Bewerking | Microsoft.Web/ishostnameavailable/Read | Controleer of de hostnaam beschikbaar is. |
> | Bewerking | Microsoft.Web/isusernameavailable/Read | Controleer of de gebruikersnaam beschikbaar is. |
> | Bewerking | Microsoft.Web/listSitesAssignedToHostName/Read | Namen van sites die zijn toegewezen aan de hostnaam ophalen. |
> | Bewerking | Microsoft.Web/Locations/apioperations/Read | Locaties API-bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/Locations/connectiongatewayinstallations/Read | Locaties verbinding Gateway installaties worden opgehaald. |
> | Bewerking | Microsoft.Web/Locations/extractapidefinitionfromwsdl/Action | Pak de Api-definitie van WSDL voor locaties. |
> | Bewerking | Microsoft.Web/Locations/listwsdlinterfaces/Action | Lijst met WSDL-Interfaces voor locaties. |
> | Bewerking | Microsoft.Web/Locations/managedapis/apioperations/Read | Locaties Managed API-bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/locations/managedapis/Join/Action | Een beheerde API koppelt. |
> | Bewerking | Microsoft.Web/Locations/managedapis/Read | Locaties beheerde API's worden opgehaald. |
> | Bewerking | Microsoft.Web/Operations/Read | Bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/publishingusers/Read | Publiceren van gebruikers ophalen. |
> | Bewerking | Microsoft.Web/publishingusers/Write | Bijwerken van de gebruikers te publiceren. |
> | Bewerking | Microsoft.Web/recommendations/Read | De lijst met aanbevelingen voor abonnementen ophalen. |
> | Bewerking | Microsoft.Web/register/Action | De registerbronprovider Microsoft.Web voor het abonnement. |
> | Bewerking | Microsoft.Web/resourcehealthmetadata/Read | Bron Health-metagegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/Capabilities/Read | Mogelijkheden van App Service-abonnementen ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/Delete | Verwijder een bestaande App Service-Plan |
> | Bewerking | Microsoft.Web/serverfarms/firstpartyapps/Settings/DELETE | App Service-abonnementen eerste partijen Apps instellingen verwijderd. |
> | Bewerking | Microsoft.Web/serverfarms/firstpartyapps/Settings/Read | App Service-abonnementen eerste partijen Apps-instellingen ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/firstpartyapps/Settings/Write | App Service-abonnementen eerste partijen Apps instellingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/DELETE | App Service-abonnementen hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/Read | App Service-abonnementen hybride verbinding naamruimten Relays ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/hybridconnectionnamespaces/relays/sites/Read | App Service-plannen voor hybride verbinding naamruimten Relays WebApps ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/hybridconnectionplanlimits/Read | Ophalen van App Service-abonnementen hybride verbindingslimieten-Plan. |
> | Bewerking | Microsoft.Web/serverfarms/hybridconnectionrelays/Read | App Service-abonnementen hybride verbinding Relays ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/metricdefinitions/Read | App Service-abonnementen metrische definities worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/metrics/Read | App Service-abonnementen metrische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/operationresults/Read | App Service-abonnementen Bewerkingsresultaten krijgt. |
> | Bewerking | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Web/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read | Hiermee wordt de beschikbare metrische gegevens voor het App Service-Plan |
> | Bewerking | Microsoft.Web/serverfarms/Read | Opvragen van de eigenschappen voor een App Service-Plan |
> | Bewerking | Microsoft.Web/serverfarms/restartSites/Action | Start opnieuw op alle Web-Apps in een App Service-Plan |
> | Bewerking | Microsoft.Web/serverfarms/sites/Read | Web-Apps van App Service-abonnementen ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/skus/Read | App Service-abonnementen SKU's worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/usages/Read | Het gebruik van App Service-abonnementen ophalen. |
> | Bewerking | Microsoft.Web/serverfarms/virtualnetworkconnections/gateways/Write | App Service-abonnementen virtueel netwerk verbindingen Gateways bijwerken. |
> | Bewerking | Microsoft.Web/serverfarms/virtualnetworkconnections/Read | App Service-abonnementen virtuele netwerkverbindingen worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/DELETE | App Service-abonnementen virtueel netwerk verbindingen Routes verwijderen. |
> | Bewerking | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/Read | App Service-abonnementen virtueel netwerk verbindingen Routes worden opgehaald. |
> | Bewerking | Microsoft.Web/serverfarms/virtualnetworkconnections/routes/Write | App Service-abonnementen virtueel netwerk verbindingen Routes bijwerken. |
> | Bewerking | Microsoft.Web/serverfarms/workers/reboot/Action | Start opnieuw op App Service-abonnementen werknemers. |
> | Bewerking | Microsoft.Web/serverfarms/Write | Maak een nieuwe App Service-abonnement of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Web/sites/analyzecustomhostname/Read | Analyseer de aangepaste hostnaam. |
> | Bewerking | Microsoft.Web/sites/applySlotConfig/Action | Web-app sleuf configuratie van doel-sleuf in de huidige web-app toe te passen |
> | Bewerking | Microsoft.Web/sites/backup/Action | Een nieuwe web-app back-up maken |
> | Bewerking | Microsoft.Web/sites/Backup/Read | Back-up van Web Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/Backup/Write | Back-up van Web-Apps worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/backups/DELETE | Back-ups van Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/backups/List/Action | Lijst met Web-Apps back-ups. |
> | Bewerking | Microsoft.Web/sites/backups/Read | De eigenschappen van back-up van een web-app |
> | Bewerking | Microsoft.Web/sites/backups/Restore/Action | Terugzetten van back-ups van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/config/DELETE | Webconfiguratie Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/config/list/Action | Web-App gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven |
> | Bewerking | Microsoft.Web/sites/config/Read | Ophalen van configuratie-instellingen voor Web-App |
> | Bewerking | Microsoft.Web/sites/config/Write | Configuratie-instellingen voor Web-App bijwerken |
> | Bewerking | Microsoft.Web/sites/continuouswebjobs/DELETE | Web-Apps doorlopende webtaken verwijderen. |
> | Bewerking | Microsoft.Web/sites/continuouswebjobs/Read | Web-Apps doorlopende webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/continuouswebjobs/start/Action | Web-Apps doorlopende webtaken starten. |
> | Bewerking | Microsoft.Web/sites/continuouswebjobs/Stop/Action | Web-Apps doorlopende webtaken stoppen. |
> | Bewerking | Microsoft.Web/sites/Delete | Een bestaande Web-App verwijderen |
> | Bewerking | Microsoft.Web/sites/Deployments/DELETE | Implementaties van Web-Apps verwijderd. |
> | Bewerking | Microsoft.Web/sites/Deployments/log/Read | Web-Apps implementaties logboek ophalen. |
> | Bewerking | Microsoft.Web/sites/Deployments/Read | Implementaties van Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Deployments/Write | Update-implementaties van Web-Apps. |
> | Bewerking | microsoft.web/sites/diagnostics/analyses/execute/Action | Web-Apps Diagnostics analyse uitvoeren. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/analyses/Read | Analyse van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/aspnetcore/Read | Diagnostische gegevens van Web-Apps voor ASP.NET Core app worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/autoheal/Read | Web-Apps Diagnostics Autoheal ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/Deployment/Read | Implementatie van Web Apps diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/Deployments/Read | Implementaties van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | microsoft.web/sites/diagnostics/detectors/execute/Action | Voer Web Apps diagnostische gegevens uit. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/detectors/Read | Detectie van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/failedrequestsperuri/Read | Web-Apps Diagnostics mislukte aanvragen Per Uri ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/frebanalysis/Read | Web-Apps Diagnostics FREB analyse worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/loganalyzer/Read | Web-Apps Diagnostics logboek Analyzer ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/Read | Categorieën voor Web-Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/runtimeavailability/Read | Runtime-beschikbaarheid van Web-Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/servicehealth/Read | Servicestatus van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/sitecpuanalysis/Read | Web-Apps Diagnostics Site CPU analyse worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/sitecrashes/Read | Web-Apps Diagnostics Site Crashes worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/sitelatency/Read | Latentie van de Site Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/sitememoryanalysis/Read | Web-Apps Diagnostics Site geheugenanalyse worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/siterestartsettingupdate/Read | Web-Apps Diagnostics Site opnieuw opstarten instelling Update downloaden. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/siterestartuserinitiated/Read | Web-Apps Diagnostics Site opnieuw opstarten gebruiker gestarte ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/siteswap/Read | Site wisseling van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/ThreadCount/Read | Aantal threads van Web Apps diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/workeravailability/Read | Web-Apps Diagnostics Workeravailability ophalen. |
> | Bewerking | Microsoft.Web/sites/Diagnostics/workerprocessrecycle/Read | Recyclen van Web Apps Diagnostics Worker proces worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/domainownershipidentifiers/Read | Web-Apps domein eigenaar-id's worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/domainownershipidentifiers/Write | Web-Apps domein eigenaar-id's worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/Functions/Action | Functies van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/Functions/DELETE | Functies voor Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/Functions/listsecrets/Action | Lijst met geheimen Web Apps functies. |
> | Bewerking | Microsoft.Web/sites/Functions/masterKey/Read | Web-Apps functies hoofdsleutel ophalen. |
> | Bewerking | Microsoft.Web/sites/Functions/Read | Functies voor Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Functions/token/Read | Get-Web-Apps functies Token. |
> | Bewerking | Microsoft.Web/sites/Functions/Write | Bijwerken van Web Apps functies. |
> | Bewerking | Microsoft.Web/sites/hostnamebindings/DELETE | Hostnaambindings voor Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/hostnamebindings/Read | Web-Apps Hostnaambindings ophalen. |
> | Bewerking | Microsoft.Web/sites/hostnamebindings/Write | Web-Apps Hostnaambindings bijwerken. |
> | Bewerking | Microsoft.Web/sites/hybridconnection/DELETE | Web-Apps hybride verbinding verwijderen. |
> | Bewerking | Microsoft.Web/sites/hybridconnection/Read | Verbinding voor Web-Apps hybride ophalen. |
> | Bewerking | Microsoft.Web/sites/hybridconnection/Write | Web-Apps hybride verbinding bijwerken. |
> | Bewerking | Microsoft.Web/sites/hybridconnectionnamespaces/relays/DELETE | Web-Apps hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | Microsoft.Web/sites/hybridconnectionnamespaces/relays/listkeys/Action | Lijst met sleutels Web Apps hybride verbinding naamruimten Relays. |
> | Bewerking | Microsoft.Web/sites/hybridconnectionnamespaces/relays/Read | Web-Apps hybride verbinding naamruimten Relays ophalen. |
> | Bewerking | Microsoft.Web/sites/hybridconnectionnamespaces/relays/Write | Web-Apps hybride verbinding naamruimten Relays bijwerken. |
> | Bewerking | Microsoft.Web/sites/hybridconnectionrelays/Read | Web-Apps hybride verbinding Relays ophalen. |
> | Bewerking | Microsoft.Web/sites/Instances/Deployments/DELETE | Web-Apps exemplaren implementaties verwijderd. |
> | Bewerking | Microsoft.Web/sites/Instances/Deployments/Read | Implementaties van Web Apps instanties worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Instances/Extensions/log/Read | Web-Apps exemplaren extensies logboek ophalen. |
> | Bewerking | Microsoft.Web/sites/Instances/Extensions/Read | Web-Apps exemplaren extensies worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Instances/processes/DELETE | Web-Apps exemplaren processen verwijderen. |
> | Bewerking | Microsoft.Web/sites/Instances/processes/Read | Web-Apps exemplaren processen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Instances/Read | Web-Apps instanties worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/listsyncfunctiontriggerstatus/Action | Lijst met synchronisatie functie Trigger Status Web-Apps. |
> | Bewerking | Microsoft.Web/sites/metricdefinitions/Read | Web-Apps metriek definities worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/metrics/Read | Web-Apps metrische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/metricsdefinitions/Read | Web Apps metrische definities worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/migratemysql/Action | MySql-Web-Apps worden gemigreerd. |
> | Bewerking | Microsoft.Web/sites/migratemysql/Read | Ophalen van Web Apps MySql migreren. |
> | Bewerking | Microsoft.Web/sites/networktrace/Action | Netwerk Trace Web-Apps. |
> | Bewerking | Microsoft.Web/sites/NewPassword/Action | NieuwWachtwoord Web-Apps. |
> | Bewerking | Microsoft.Web/sites/operationresults/Read | Web-Apps Bewerkingsresultaten krijgt. |
> | Bewerking | Microsoft.Web/sites/Operations/Read | Bewerkingen voor Web-Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/perfcounters/Read | Prestatiemeteritems voor Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/premieraddons/DELETE | Web Apps Premier invoegtoepassingen verwijderen. |
> | Bewerking | Microsoft.Web/sites/premieraddons/Read | Web Apps Premier invoegtoepassingen ophalen. |
> | Bewerking | Microsoft.Web/sites/premieraddons/Write | Bijwerken van Web Apps Premier invoegtoepassingen. |
> | Bewerking | Microsoft.Web/sites/processes/Read | Web-Apps processen worden opgehaald. |
> | Bewerking | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Web/sites/providers/Microsoft.Insights/metricDefinitions/Read | Hiermee wordt de beschikbare metrische gegevens voor Web-App |
> | Bewerking | Microsoft.Web/sites/publiccertificates/DELETE | Web-Apps openbare certificaten verwijderen. |
> | Bewerking | Microsoft.Web/sites/publiccertificates/Read | Web-Apps openbare certificaten ophalen. |
> | Bewerking | Microsoft.Web/sites/publiccertificates/Write | Web-Apps openbare certificaten bijwerken. |
> | Bewerking | Microsoft.Web/sites/publish/Action | Een Web-App publiceren |
> | Bewerking | Microsoft.Web/sites/publishxml/Action | Ophalen van de xml van profiel voor een Web-App publiceren |
> | Bewerking | Microsoft.Web/sites/PublishXml/Read | Web-Apps publiceren XML ophalen. |
> | Bewerking | Microsoft.Web/sites/Read | De eigenschappen van een Web-App opgehaald |
> | Bewerking | Microsoft.Web/sites/recommendationhistory/Read | Web-Apps aanbeveling geschiedenis ophalen. |
> | Bewerking | Microsoft.Web/sites/Recommendations/Disable/Action | Web-Apps aanbevelingen uitschakelen. |
> | Bewerking | Microsoft.Web/sites/recommendations/Read | Haal de lijst met aanbevelingen voor web-app. |
> | Bewerking | Microsoft.Web/sites/recover/Action | Herstellen van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/resetSlotConfig/Action | Web-app-configuratie opnieuw instellen |
> | Bewerking | Microsoft.Web/sites/resourcehealthmetadata/Read | Web-Apps Resource Health metagegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/restart/Action | Een Web-App starten |
> | Bewerking | Microsoft.Web/sites/Restore/Read | Terugzetten van de Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Restore/Write | Herstellen van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/restoresnapshot/Action | Web-Apps momentopnamen terug. |
> | Bewerking | Microsoft.Web/sites/siteextensions/DELETE | Verwijderen van Web Apps Site-uitbreidingen. |
> | Bewerking | Microsoft.Web/sites/siteextensions/Read | Web-Apps-Site-uitbreidingen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/siteextensions/Write | Web-Apps-Site-uitbreidingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/slots/analyzecustomhostname/Read | Ophalen van Web Apps sleuven analyseren aangepaste hostnaam. |
> | Bewerking | Microsoft.Web/sites/slots/applySlotConfig/Action | Web-app siteconfiguratie van doel sleuf naar de huidige sleuf toepassen. |
> | Bewerking | Microsoft.Web/sites/slots/backup/Action | Nieuwe Web-Appsite back-up maken. |
> | Bewerking | Microsoft.Web/sites/slots/Backup/Read | Back-up-sleuven voor Web-Apps worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Backup/Write | Bijwerken van Web Apps sleuven back-up. |
> | Bewerking | Microsoft.Web/sites/slots/backups/DELETE | Back-ups sleuven voor Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/backups/List/Action | Lijst met Web-Apps sleuven back-ups. |
> | Bewerking | Microsoft.Web/sites/slots/backups/Read | De eigenschappen van een web-appsites back-up |
> | Bewerking | Microsoft.Web/sites/slots/backups/Restore/Action | Web-Apps sleuven back-ups herstellen. |
> | Bewerking | Microsoft.Web/sites/slots/config/DELETE | Webconfiguratie Apps sleuven verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/config/list/Action | Web-Appsite van gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven |
> | Bewerking | Microsoft.Web/sites/slots/config/Read | Web-Appsite van configuratie-instellingen ophalen |
> | Bewerking | Microsoft.Web/sites/slots/config/Write | Web-Appsite van configuratie-instellingen bijwerken |
> | Bewerking | Microsoft.Web/sites/slots/continuouswebjobs/DELETE | Web-Apps sleuven doorlopende webtaken verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/continuouswebjobs/Read | Web-Apps sleuven doorlopende webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/continuouswebjobs/start/Action | Web-Apps sleuven doorlopende webtaken starten. |
> | Bewerking | Microsoft.Web/sites/slots/continuouswebjobs/Stop/Action | Web-Apps sleuven doorlopende webtaken stoppen. |
> | Bewerking | Microsoft.Web/sites/slots/Delete | Een bestaande Web-Appsite verwijderen |
> | Bewerking | Microsoft.Web/sites/slots/Deployments/DELETE | Web-Apps sleuven implementaties verwijderd. |
> | Bewerking | Microsoft.Web/sites/slots/Deployments/log/Read | Web-Apps sleuven implementaties logboek ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Deployments/Read | Web-Apps sleuven implementaties ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Deployments/Write | Update-implementaties van Web Apps sleuven. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Web-Apps sleuven Diagnostics analyse uitvoeren. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/analyses/Read | Analyse van Web Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/aspnetcore/Read | Web-Apps sleuven diagnostische gegevens voor ASP.NET Core app worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/autoheal/Read | Web-Apps sleuven Diagnostics Autoheal ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/Deployment/Read | Implementatie van Web Apps sleuven diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/Deployments/Read | Implementaties van Web Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Voer Web Apps sleuven diagnostische gegevens uit. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/detectors/Read | Detectie van Web Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/frebanalysis/Read | Web-Apps sleuven Diagnostics FREB analyse worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/loganalyzer/Read | Web-Apps sleuven Diagnostics logboek Analyzer ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/Read | Web-Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/runtimeavailability/Read | Web-Apps sleuven Diagnostics Runtime-beschikbaarheid ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/servicehealth/Read | Servicestatus van Web Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/sitecpuanalysis/Read | Get Web Apps sleuven Diagnostics Site CPU-analyse. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/sitecrashes/Read | Web-Apps sleuven Diagnostics Site Crashes worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/sitelatency/Read | Latentie van Web Apps sleuven Diagnostics Site ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/sitememoryanalysis/Read | Web-Apps sleuven Diagnostics Site geheugenanalyse worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/siterestartsettingupdate/Read | Web-Apps sleuven Diagnostics Site opnieuw opstarten instelling Update downloaden. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/siterestartuserinitiated/Read | Web-Apps sleuven Diagnostics Site opnieuw opstarten gebruiker gestarte ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/siteswap/Read | Site wisseling van Web Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/ThreadCount/Read | Threads voor Web-Apps sleuven diagnostische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/workeravailability/Read | Web-Apps sleuven Diagnostics Workeravailability ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Diagnostics/workerprocessrecycle/Read | Recyclen van Web Apps sleuven Diagnostics Worker proces worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/domainownershipidentifiers/Read | Web-Apps sleuven domein eigenaar-id's worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/hostnamebindings/DELETE | Web-Apps sleuven Hostnaambindings verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/hostnamebindings/Read | Web-Apps sleuven Hostnaambindings ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/hostnamebindings/Write | Web-Apps sleuven Hostnaambindings bijwerken. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnection/DELETE | Sleuven hybride verbinding voor Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnection/Read | Hybride verbinding voor Web-Apps sleuven ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnection/Write | Hybride verbinding voor Web-Apps sleuven bijwerken. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnectionnamespaces/relays/DELETE | Web-Apps sleuven hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnectionnamespaces/relays/Write | Web-Apps sleuven hybride verbinding naamruimten Relays bijwerken. |
> | Bewerking | Microsoft.Web/sites/slots/hybridconnectionrelays/Read | Web-Apps sleuven hybride verbinding Relays ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Instances/Deployments/Read | Implementaties van Web Apps sleuven instanties worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Instances/processes/DELETE | Web-Apps sleuven exemplaren processen verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/Instances/processes/Read | Web-Apps sleuven exemplaren processen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Instances/Read | Web-Apps sleuven instanties worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/metricdefinitions/Read | Web-Apps sleuven metrische definities worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/metrics/Read | Web-Apps sleuven metrische gegevens worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/migratemysql/Read | Ophalen van Web Apps sleuven MySql migreren. |
> | Bewerking | Microsoft.Web/sites/slots/networktrace/Action | Netwerk Trace Web Apps sleuven. |
> | Bewerking | Microsoft.Web/sites/slots/NewPassword/Action | Sleuven NieuwWachtwoord Web-Apps. |
> | Bewerking | Microsoft.Web/sites/slots/operationresults/Read | Web-Apps sleuven Bewerkingsresultaten krijgt. |
> | Bewerking | Microsoft.Web/sites/slots/Operations/Read | Web-Apps sleuven bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/perfcounters/Read | Prestatiemeteritems van Web Apps sleuven ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/phplogging/Read | Web-Apps sleuven Phplogging ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/premieraddons/DELETE | Web Apps sleuven Premier invoegtoepassingen verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/premieraddons/Read | Web Apps sleuven Premier invoegtoepassingen ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/premieraddons/Write | Bijwerken van Web Apps sleuven Premier invoegtoepassingen. |
> | Bewerking | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt |
> | Bewerking | Microsoft.Web/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read | Hiermee wordt de beschikbare metrische gegevens voor Web-Appsite |
> | Bewerking | Microsoft.Web/sites/slots/publiccertificates/Read | Web-Apps sleuven openbare certificaten ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/publiccertificates/Write | Maken of bijwerken van Web Apps sleuven openbare certificaten. |
> | Bewerking | Microsoft.Web/sites/slots/publish/Action | Een Web-Appsite publiceren |
> | Bewerking | Microsoft.Web/sites/slots/publishxml/Action | Profiel xml publiceren voor Web-Appsite ophalen |
> | Bewerking | Microsoft.Web/sites/slots/Read | De eigenschappen van een Web-App implementatiesleuf opgehaald |
> | Bewerking | Microsoft.Web/sites/slots/recover/Action | Web-Apps sleuven herstellen. |
> | Bewerking | Microsoft.Web/sites/slots/resetSlotConfig/Action | Web-app sleuf configuratie opnieuw instellen |
> | Bewerking | Microsoft.Web/sites/slots/resourcehealthmetadata/Read | Web-Apps sleuven Resource Health metagegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/restart/Action | Een Web-Appsite starten |
> | Bewerking | Microsoft.Web/sites/slots/Restore/Read | Web-Apps sleuven terugzetten ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/Restore/Write | Web-Apps sleuven herstellen. |
> | Bewerking | Microsoft.Web/sites/slots/restoresnapshot/Action | Web-Apps sleuven momentopnamen terug. |
> | Bewerking | Microsoft.Web/sites/slots/siteextensions/DELETE | Verwijderen van Web Apps sleuven Site-uitbreidingen. |
> | Bewerking | Microsoft.Web/sites/slots/siteextensions/Read | Web-Apps sleuven Site-uitbreidingen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/siteextensions/Write | Web-Apps sleuven Site-uitbreidingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/slots/slotsdiffs/Action | Verschillen in de configuratie tussen web-app en sleuven ophalen |
> | Bewerking | Microsoft.Web/sites/slots/slotsswap/Action | Implementatie Web-appsites wisselen |
> | Bewerking | Microsoft.Web/sites/slots/Snapshots/Read | Momentopnamen van Web Apps sleuven ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Delete | Instellingen voor Web-Appsite van bronbeheer configuratie verwijderen |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Read | Configuratie-instellingen voor Web-Appsite van broncodebeheer ophalen |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Write | Instellingen voor Web-Appsite van bronbeheer configuratie bijwerken |
> | Bewerking | Microsoft.Web/sites/slots/start/Action | Een Web-Appsite starten |
> | Bewerking | Microsoft.Web/sites/slots/stop/Action | Een Web-Appsite stoppen |
> | Bewerking | Microsoft.Web/sites/slots/Sync/Action | Synchronisatie Web Apps sleuven. |
> | Bewerking | Microsoft.Web/sites/slots/triggeredwebjobs/DELETE | Web-Apps sleuven Triggered webtaken verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/triggeredwebjobs/Read | Web-Apps sleuven Triggered webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/triggeredwebjobs/Run/Action | Web-Apps sleuven Triggered webtaken worden uitgevoerd. |
> | Bewerking | Microsoft.Web/sites/slots/usages/Read | Het gebruik van Web Apps sleuven ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/virtualnetworkconnections/DELETE | Web-Apps sleuven virtuele netwerkverbindingen verwijderen. |
> | Bewerking | Microsoft.Web/sites/slots/virtualnetworkconnections/gateways/Write | Web-Apps sleuven virtueel netwerk verbindingen Gateways bijwerken. |
> | Bewerking | Microsoft.Web/sites/slots/virtualnetworkconnections/Read | Web-Apps sleuven virtuele netwerkverbindingen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/virtualnetworkconnections/Write | Web-Apps sleuven virtuele netwerkverbindingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/slots/webjobs/Read | Web-Apps sleuven webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/slots/Write | Een nieuwe Web-Appsite maken of bijwerken van een bestaande |
> | Bewerking | Microsoft.Web/sites/slotsdiffs/Action | Verschillen in de configuratie tussen web-app en sleuven ophalen |
> | Bewerking | Microsoft.Web/sites/slotsswap/Action | Implementatie Web-appsites wisselen |
> | Bewerking | Microsoft.Web/sites/Snapshots/Read | Momentopnamen van de Web-Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Delete | Instellingen voor Web-App bronbeheer configuratie verwijderen |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Read | Broncodebeheer voor Web-App-configuratie-instellingen ophalen |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Write | Instellingen voor Web-App bronbeheer configuratie bijwerken |
> | Bewerking | Microsoft.Web/sites/start/Action | Een Web-App starten |
> | Bewerking | Microsoft.Web/sites/stop/Action | Een Web-App stoppen |
> | Bewerking | Microsoft.Web/sites/Sync/Action | Synchronisatie van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/syncfunctiontriggers/Action | Synchronisatie functie Triggers voor Web-Apps. |
> | Bewerking | Microsoft.Web/sites/triggeredwebjobs/DELETE | Web-Apps Triggered webtaken verwijderen. |
> | Bewerking | Microsoft.Web/sites/triggeredwebjobs/history/Read | Web Apps Triggered webtaken geschiedenis ophalen. |
> | Bewerking | Microsoft.Web/sites/triggeredwebjobs/Read | Web-Apps Triggered webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/triggeredwebjobs/Run/Action | Geactiveerde WebJobs voor Web-Apps worden uitgevoerd. |
> | Bewerking | Microsoft.Web/sites/usages/Read | Het gebruik van Web Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/virtualnetworkconnections/DELETE | Verwijder virtuele netwerkverbindingen van Web Apps. |
> | Bewerking | Microsoft.Web/sites/virtualnetworkconnections/gateways/Read | Web-Apps virtuele verbindingen netwerkgateways ophalen. |
> | Bewerking | Microsoft.Web/sites/virtualnetworkconnections/gateways/Write | Web-Apps virtueel netwerk verbindingen Gateways bijwerken. |
> | Bewerking | Microsoft.Web/sites/virtualnetworkconnections/Read | Web-Apps virtuele netwerkverbindingen worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/virtualnetworkconnections/Write | Web-Apps virtuele netwerkverbindingen worden bijgewerkt. |
> | Bewerking | Microsoft.Web/sites/webjobs/Read | Web-Apps webtaken worden opgehaald. |
> | Bewerking | Microsoft.Web/sites/Write | Een nieuwe Web-App maken of bijwerken van een bestaande |
> | Bewerking | Microsoft.Web/skus/Read | SKU's worden opgehaald. |
> | Bewerking | Microsoft.Web/sourcecontrols/Read | Besturingselementen voor gegevensbronnen worden opgehaald. |
> | Bewerking | Microsoft.Web/sourcecontrols/Write | Besturingselementen voor gegevensbronnen bijwerken. |
> | Bewerking | Microsoft.Web/unregister/Action | Hef de registratie Microsoft.Web resourceprovider voor het abonnement. |
> | Bewerking | Microsoft.Web/Validate/Action | Valideren. |
> | Bewerking | Microsoft.Web/verifyhostingenvironmentvnet/Action | Controleer of de Hosting-omgeving Vnet. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Beschrijving |
> | --- | --- | --- |
> | Bewerking | Microsoft.WorkloadMonitor/components/read | Bewerkingen bronnen lezen |
> | Bewerking | Microsoft.WorkloadMonitor/healthInstances/read | Bewerkingen bronnen lezen |
> | Bewerking | Microsoft.WorkloadMonitor/Operations/read | Bewerkingen bronnen lezen |
> | Bewerking | Microsoft.WorkloadMonitor/workloads/delete | Hiermee verwijdert u een resource werkbelasting |
> | Bewerking | Microsoft.WorkloadMonitor/workloads/read | Een resource werkbelasting leest |
> | Bewerking | Microsoft.WorkloadMonitor/workloads/write | Schrijft een workload resource |

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen](custom-roles.md)
- [Ingebouwde rollen](built-in-roles.md)

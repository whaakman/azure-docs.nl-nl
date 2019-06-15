---
title: Azure Resource Manager-resourceproviderbewerkingen | Microsoft Docs
description: Hiermee worden de bewerkingen die beschikbaar zijn voor de Microsoft Azure Resource Manager-resourceproviders
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 07c1a726e33eb8287634b63ef2e309483c05c3f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65962111"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager-resourceproviderbewerkingen

Dit artikel worden de bewerkingen die beschikbaar zijn voor elke Azure Resource Manager-resourceprovider. Deze bewerkingen kunnen worden gebruikt in [aangepaste rollen](custom-roles.md) voor gedetailleerde [op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) tot resources in Azure. Bewerking tekenreeksen hebben de volgende indeling: `{Company}.{ProviderName}/{resourceType}/{action}`

De resource provider-bewerkingen zijn altijd nog in ontwikkeling. Als u de meest recente bewerkingen, gebruikt [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) of [az provider Bewerkingslijst](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.AAD/domainServices/delete | Delete Domain Service |
> | Bewerking | Microsoft.AAD/domainServices/oucontainer/delete | Organisatie-eenheid Container verwijderen |
> | Bewerking | Microsoft.AAD/domainServices/oucontainer/read | Organisatie-eenheid-Containers gelezen |
> | Bewerking | Microsoft.AAD/domainServices/oucontainer/write | Organisatie-eenheid Container schrijven |
> | Bewerking | Microsoft.AAD/domainServices/read | Read Domain Services |
> | Bewerking | Microsoft.AAD/domainServices/ReplicaSets/delete | Cluster-Site verwijderen |
> | Bewerking | Microsoft.AAD/domainServices/ReplicaSets/read | Cluster-Site lezen |
> | Bewerking | Microsoft.AAD/domainServices/ReplicaSets/write | Cluster-Site te schrijven |
> | Bewerking | Microsoft.AAD/domainServices/write | Write-domeinservice |
> | Bewerking | Microsoft.AAD/locations/operationresults/read |  |
> | Bewerking | Microsoft.AAD/Operations/read |  |
> | Bewerking | Microsoft.AAD/register/action | Register Domain Service |
> | Bewerking | Microsoft.AAD/unregister/action | Unregister Domain Service |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | microsoft.aadiam/diagnosticsettings/delete | Een diagnostische instelling verwijderen |
> | Bewerking | microsoft.aadiam/diagnosticsettings/read | Een diagnostische instelling lezen |
> | Bewerking | microsoft.aadiam/diagnosticsettings/write | Een diagnostische instelling schrijven |
> | Bewerking | microsoft.aadiam/diagnosticsettingscategories/read | De categorieën van een diagnostische instelling lezen |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Addons/operations/read | Haalt ondersteund RP-bewerkingen. |
> | Bewerking | Microsoft.Addons/register/action | Het opgegeven abonnement registreren bij Microsoft.Addons |
> | Bewerking | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Een lijst met huidige ondersteuningsplaninformatie voor het opgegeven abonnement. |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Hiermee verwijdert u de opgegeven Canonical-ondersteuningsabonnement |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/read | Ophalen van de status van de planning opgegeven canonieke ondersteuning. |
> | Bewerking | Microsoft.Addons/supportProviders/supportPlanTypes/write | Voegt het canonieke support-plan dat is opgegeven. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/action | Een nieuw forest maken voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Hiermee haalt alle servers voor de opgegeven naam. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/alerts/read | Krijgt u waarschuwingen informatie voor het forest, zoals alertid, waarschuwing verhoogd datum, waarschuwing gedetecteerd, waarschuwing beschrijving van recentste, laatst bijgewerkte, waarschuwing niveau, waarschuwing status, waarschuwing oplossen koppelingen enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/configuration/read | Hiermee haalt configuratie van de Service voor het forest. Voorbeeld - naam van het Forest, functioneel niveau, Domain Naming master FSMO-rol, FSMO-functie schemamaster enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/delete | Hiermee verwijdert u een Service en de servers samen met de status van gegevens. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Hiermee haalt gegevens van de domeinen en sites voor het forest. Voorbeeld van de health-status, actieve waarschuwingen, waarschuwingen, eigenschappen, zoals het functionaliteitsniveau van domein, Forest, infrastructuur-Master, PDC, opgelost RID-master enzovoort.  |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Hiermee haalt u de voorkeur gebruikersinstelling voor het forest.<br>Voorbeeld - MetricCounterName zoals ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches.<br>Instellingen voor de UI-grafieken enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Hiermee haalt forest samenvatting voor het opgegeven forest, zoals de naam van het forest, het aantal domeinen onder dit forest, het aantal sites en sites gegevens, enzovoort. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Hiermee haalt de lijst van ondersteunde metrische gegevens voor een bepaalde service.<br>Voor Extranet accountvergrendelingen voorbeeld, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor AD FS-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomainService.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken exporteren naar Azure AD voor ADSync-service worden uitgevoerd. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Deze API is een service worden gegeven, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt om op te halen van informatie met betrekking tot: Extranet accountvergrendeling af, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain-Service.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, voeren Insights Agent eigen Bytes, exporteren statistieken met Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Deze API wordt een lijst van alle onboarding ADDomainServices opgehaald voor een premium-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/read | Hiermee haalt u details van Service voor de naam van de opgegeven service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Hiermee haalt replicatiedetails voor alle servers voor de naam van de opgegeven service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Hiermee haalt u het aantal domeincontrollers en hun replicatiefouten indien van toepassing. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Haalt volledige lijst van de domain controller, samen met replicatiedetails voor de opgegeven forest. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Een server-exemplaar toevoegen aan de service. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Deze api wordt aangeroepen tijdens de serverregistratie van ADDomainService, als u de referenties voor de voorbereiding op nieuwe servers. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Hiermee verwijdert u een server voor een bepaalde service en de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/addsservices/write | Hiermee of het exemplaar ADDomainService bijgewerkt voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/action | Tenantconfiguratie voor updates. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/read | Hiermee leest u de configuratie van de Tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/configuration/write | Hiermee maakt u de configuratie van een Tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/logs/contents/read | Hiermee haalt u de inhoud van de agentinstallatie en registratielogboeken die zijn opgeslagen in blob. |
> | Bewerking | Microsoft.ADHybridHealthService/logs/read | Hiermee haalt de installatie van agent en registratielogboeken voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/operations/read | Lijst met bewerkingen die worden ondersteund door systeem ophalen. |
> | Bewerking | Microsoft.ADHybridHealthService/register/action | Hiermee wordt de ADHybrid Health Service-Resourceprovider geregistreerd en wordt het maken van de resource ADHybrid Health-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Lijst met beschikbare regio's, gebruikt door DevOps ter ondersteuning van de klant incidenten ophalen. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/badpassword/read | Hiermee haalt de lijst met mislukte wachtwoordpogingen voor alle gebruikers in Active Directory Federation Service. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Haalt Blob SAS-URI die de status en uiteindelijke resultaat van het zojuist in de wachtrij geplaatste rapporttaak voor de frequentie van onjuiste gebruikersnaam en wachtwoord probeert per gebruikers per IP-adres per dag voor een bepaalde Tenant-id. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Hiermee haalt u de lijst van DevOps ingestemd tenants. Doorgaans gebruikt voor klantondersteuning. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/isdevops/read | Hiermee haalt u een waarde die aangeeft of de tenant DevOps ingestemd of niet is. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Updates userid(objectid) voor de geselecteerde dev ops-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Haalt de geselecteerde implementatie voor de opgegeven tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Een tenant-id wordt opgegeven de opslaglocatie voor de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Hiermee haalt u de geografische locatie van waaruit de gegevens worden geopend. |
> | Bewerking | Microsoft.ADHybridHealthService/services/action | Een service-exemplaar in de tenant-updates. |
> | Bewerking | Microsoft.ADHybridHealthService/services/alerts/read | Hiermee leest u de waarschuwingen voor een service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/alerts/read | Hiermee leest u de waarschuwingen voor een service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | De opgegeven functie controleert naam of als een service biedt u alles wat nodig is om deze functie te gebruiken. |
> | Bewerking | Microsoft.ADHybridHealthService/services/delete | Hiermee verwijdert u een service-exemplaar in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/exporterrors/read | Hiermee haalt de exportfouten voor een bepaalde sync-service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/exportstatus/read | Hiermee haalt de uitvoerstatus van de voor een bepaalde service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Hiermee haalt waarschuwingen feedback voor een bepaalde service en de server. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metricmetadata/read | Hiermee haalt de lijst van ondersteunde metrische gegevens voor een bepaalde service.<br>Voor Extranet accountvergrendelingen voorbeeld, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor AD FS-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomainService.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, Insights Agent eigen Bytes, statistieken exporteren naar Azure AD voor ADSync-service worden uitgevoerd. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Deze API een service worden gegeven, wordt het gemiddelde voor metrische gegevens voor een bepaalde service opgehaald.<br>Deze API kan bijvoorbeeld worden gebruikt om op te halen van informatie met betrekking tot: Extranet accountvergrendeling af, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain-Service.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, voeren Insights Agent eigen Bytes, exporteren statistieken met Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/read | Deze API is een service worden gegeven, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt om op te halen van informatie met betrekking tot: Extranet accountvergrendeling af, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain-Service.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, voeren Insights Agent eigen Bytes, exporteren statistieken met Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Deze API een service worden gegeven, wordt de geaggregeerde weergave voor metrische gegevens voor een bepaalde service opgehaald.<br>Deze API kan bijvoorbeeld worden gebruikt om op te halen van informatie met betrekking tot: Extranet accountvergrendeling af, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain-Service.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, voeren Insights Agent eigen Bytes, exporteren statistieken met Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Toevoegen of configuratie van de bewaking voor een service-updates. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Hiermee haalt u het bewaken van configuraties voor een bepaalde service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Toevoegen of bewaken van configuraties voor een service-updates. |
> | Bewerking | Microsoft.ADHybridHealthService/services/premiumcheck/read | Deze API haalt de lijst met alle onboarding services voor een premium-tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/read | Hiermee leest u de service-exemplaren in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/reports/blobUris/read | Hiermee haalt alle riskant IP-adres rapport URI's voor de afgelopen 7 dagen. |
> | Bewerking | Microsoft.ADHybridHealthService/services/reports/details/read | Rapport van 50 belangrijkste gebruikers met mislukte wachtwoordpogingen in de afgelopen 7 dagen opgehaald |
> | Bewerking | Microsoft.ADHybridHealthService/services/reports/generateBlobUri/action | Rapport riskant IP-adres wordt gegenereerd en retourneert een URI die verwijst naar deze. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/action | Hiermee maakt u een server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Leest de waarschuwingen voor een server. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Deze api wordt aangeroepen tijdens de serverregistratie, als u de referenties voor de voorbereiding op nieuwe servers. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Voor een bepaalde server haalt deze API een lijst met gegevenstypen die door de servers en de laatste tijd voor het uploaden van elke worden geüpload. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/delete | Hiermee verwijdert u een server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Hiermee haalt u de foutdetails synchronisatie exporteren voor een bepaalde Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Deze API is een service worden gegeven, wordt de metrische gegevens-informatie.<br>Deze API kan bijvoorbeeld worden gebruikt om op te halen van informatie met betrekking tot: Extranet accountvergrendeling af, totaal aantal mislukte aanvragen, openstaande Tokenaanvragen (Proxy), Tokenaanvragen/sec enzovoort voor ADFederation-service.<br>NTLM-verificaties per seconde, LDAP geslaagde worden per seconde, LDAP Bind tijd, actieve Threads van LDAP, Kerberos-verificaties per seconde, ATQ Threads totale enzovoort voor ADDomain-Service.<br>Profiel latentie, TCP-verbindingen tot stand gebracht, voeren Insights Agent eigen Bytes, exporteren statistieken met Azure AD Sync-Service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/metrics/read | Hiermee haalt u de lijst met connectors en uitvoeringsprofiel namen voor de opgegeven service en het lid van de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/read | Hiermee leest u de server-exemplaar in de service. |
> | Bewerking | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Hiermee haalt u de configuratie van de service voor een bepaalde tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Status van de functie in de whitelist aan voor een bepaalde tenant opgehaald. |
> | Bewerking | Microsoft.ADHybridHealthService/services/write | Hiermee maakt u een service-exemplaar in de tenant. |
> | Bewerking | Microsoft.ADHybridHealthService/unregister/action | De registratie van het abonnement voor ADHybrid Health Service-Resourceprovider. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Advisor/configurations/read | Configuraties ophalen |
> | Bewerking | Microsoft.Advisor/configurations/write | Bijgewerkte configuratie |
> | Bewerking | Microsoft.Advisor/generateRecommendations/action | Aanbevelingen heeft gegenereerd |
> | Bewerking | Microsoft.Advisor/generateRecommendations/read | Haalt de status van de aanbevelingen genereren |
> | Bewerking | Microsoft.Advisor/operations/read | De bewerkingen voor de Microsoft-Advisor opgehaald |
> | Bewerking | Microsoft.Advisor/recommendations/available/action | Nieuwe aanbeveling is beschikbaar in Microsoft advisor |
> | Bewerking | Microsoft.Advisor/recommendations/read | Aanbevelingen voor leesbewerkingen |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/delete | Hiermee verwijdert u onderdrukking |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/read | Suppressions opgehaald |
> | Bewerking | Microsoft.Advisor/recommendations/suppressions/write | Bijgewerkte suppressions |
> | Bewerking | Microsoft.Advisor/register/action | Hiermee wordt het abonnement voor de Microsoft-Advisor |
> | Bewerking | Microsoft.Advisor/suppressions/delete | Hiermee verwijdert u onderdrukking |
> | Bewerking | Microsoft.Advisor/suppressions/read | Suppressions opgehaald |
> | Bewerking | Microsoft.Advisor/suppressions/write | Bijgewerkte suppressions |
> | Bewerking | Microsoft.Advisor/unregister/action | De registratie van het abonnement voor de Microsoft-Advisor |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.AlertsManagement/actionRules/delete | Actieregel in een bepaald abonnement verwijderen. |
> | Bewerking | Microsoft.AlertsManagement/actionRules/read | Alle actieregels voor de voor de invoer filters ophalen. |
> | Bewerking | Microsoft.AlertsManagement/actionRules/write | Maken of bijwerken van de actieregel in een bepaald abonnement |
> | Bewerking | Microsoft.AlertsManagement/alerts/changestate/action | Wijzig de status van de waarschuwing. |
> | Bewerking | Microsoft.AlertsManagement/alerts/diagnostics/read | Alle diagnostische gegevens voor de waarschuwing ophalen |
> | Bewerking | Microsoft.AlertsManagement/alerts/history/read | Ophalen van de geschiedenis van de waarschuwing |
> | Bewerking | Microsoft.AlertsManagement/alerts/read | Alle waarschuwingen voor de invoer filters ophalen. |
> | Bewerking | Microsoft.AlertsManagement/alertsList/read | Alle waarschuwingen voor de invoer-filters voor abonnementen ophalen |
> | Bewerking | Microsoft.AlertsManagement/alertsSummary/read | De samenvatting van de waarschuwingen ophalen |
> | Bewerking | Microsoft.AlertsManagement/alertsSummaryList/read | De samenvatting van waarschuwingen voor abonnementen ophalen |
> | Bewerking | Microsoft.AlertsManagement/Operations/read | Leest de bewerkingen die zijn opgegeven |
> | Bewerking | Microsoft.AlertsManagement/register/action | Hiermee wordt het abonnement voor het beheer van Microsoft waarschuwingen |
> | Bewerking | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Slimme detectie waarschuwingsregel in een bepaald abonnement verwijderen |
> | Bewerking | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Alle Slimme detectie-waarschuwingsregels ophalen voor de invoer-filters |
> | Bewerking | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Maken of bijwerken van de waarschuwingsregel in een bepaald abonnement Slimme detectie |
> | Bewerking | Microsoft.AlertsManagement/smartGroups/changestate/action | De status van de slimme groep wijzigen |
> | Bewerking | Microsoft.AlertsManagement/smartGroups/history/read | Ophalen van de geschiedenis van de slimme groep |
> | Bewerking | Microsoft.AlertsManagement/smartGroups/read | De intelligente groepen ophalen voor de invoer-filters |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of de opgegeven Analysis Server naam geldig is en niet wordt gebruikt. |
> | Bewerking | Microsoft.AnalysisServices/locations/operationresults/read | Haalt de informatie van het opgegeven bewerkingsresultaat. |
> | Bewerking | Microsoft.AnalysisServices/locations/operationstatuses/read | Haalt de informatie van de status van de opgegeven bewerking. |
> | Bewerking | Microsoft.AnalysisServices/operations/read | De informatie over bewerkingen opgehaald |
> | Bewerking | Microsoft.AnalysisServices/register/action | Registreert de resourceprovider Analysis Services. |
> | Bewerking | Microsoft.AnalysisServices/servers/delete | Hiermee verwijdert u de Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/servers/listGatewayStatus/action | De status van de gateway die is gekoppeld aan de server weergeven. |
> | Bewerking | Microsoft.AnalysisServices/servers/read | Haalt de informatie van de opgegeven Analysis-Server. |
> | Bewerking | Microsoft.AnalysisServices/servers/resume/action | De analyseserver hervat. |
> | Bewerking | Microsoft.AnalysisServices/servers/skus/read | Beschikbare SKU-gegevens voor de server ophalen |
> | Bewerking | Microsoft.AnalysisServices/servers/suspend/action | Hiermee wordt de analyseserver onderbroken. |
> | Bewerking | Microsoft.AnalysisServices/servers/write | Hiermee maakt of de opgegeven Analysis-Server worden bijgewerkt. |
> | Bewerking | Microsoft.AnalysisServices/skus/read | Hiermee wordt de informatie van SKU's opgehaald |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ApiManagement/checkNameAvailability/read | Controleert of de opgegeven servicenaam is beschikbaar |
> | Bewerking | Microsoft.ApiManagement/operations/read | Alle API-bewerkingen beschikbaar voor Microsoft.ApiManagement resource lezen |
> | Bewerking | Microsoft.ApiManagement/register/action | Abonnement voor Microsoft.ApiManagement resourceprovider registreren |
> | Bewerking | Microsoft.ApiManagement/reports/read | Rapporten die worden samengevoegd door perioden, geografische regio, ontwikkelaars, producten, API's, bewerkingen, abonnement en byRequest ophalen. |
> | Bewerking | Microsoft.ApiManagement/service/apis/delete | Hiermee verwijdert u de opgegeven API van het exemplaar van API Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/delete | Hiermee verwijdert u de opgegeven diagnose van een API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/read | Geeft een lijst van alle diagnostische gegevens van een API. of haalt de details van de diagnose voor een API die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/apis/diagnostics/write | Hiermee maakt u een nieuwe diagnose voor een API of een bestaande bijgewerkt. of de details van de diagnose voor een API die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Hiermee verwijdert de opgegeven opmerking uit een probleem. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/read | Een lijst met alle bijlagen voor het probleem dat is gekoppeld aan de opgegeven API. of haalt de details van de bijlage voor een API die is opgegeven door de id van het probleem. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/attachments/write | Hiermee maakt u een nieuwe bijlage voor het probleem in een API of een bestaande bijgewerkt. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/delete | Hiermee verwijdert de opgegeven opmerking uit een probleem. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/read | Een lijst met alle opmerkingen voor het probleem dat is gekoppeld met de opgegeven API. of haalt de details van het probleem Opmerking voor een API die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/comments/write | Hiermee maakt u een nieuwe opmerking voor het probleem in een API of een bestaande bijgewerkt. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/delete | Hiermee verwijdert u het opgegeven probleem van een API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/read | Geeft een lijst van alle problemen die zijn gekoppeld aan de opgegeven API. of haalt de details van het probleem voor een API die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/apis/issues/write | Hiermee maakt u een nieuw probleem voor een API of een bestaande bijgewerkt. of een bestaande uitgeven voor een API-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/delete | Hiermee verwijdert u de opgegeven bewerking in de API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/delete | Hiermee verwijdert u de configuratie van het beleid op de Api-bewerking. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/read | Haal de lijst beleidsconfiguratie op het niveau van de API-bewerking. of u de configuratie van het beleid op het niveau van de API-bewerking. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policies/write | Hiermee of bijwerken van de configuratie van beleid voor het niveau van de API-bewerking. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/delete | De configuratie van het beleid op het niveau van de bewerking verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/read | Configuratie van het beleid op het niveau van de bewerking ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/policy/write | Configuratie van beleid voor de bewerking niveau maken |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/read | Geeft een lijst van een verzameling van de bewerkingen voor de opgegeven API. of haalt de details van de API-bewerking die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/delete | Loskoppelen van de code van de bewerking. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/read | Geeft een lijst van alle Tags die zijn gekoppeld aan de bewerking. of tag die is gekoppeld aan de bewerking ophalen. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/tags/write | Label toewijzen aan de bewerking. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operations/write | Hiermee maakt u een nieuwe bewerking in de API of een bestaande bijgewerkt. of de details van de bewerking in de API die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/apis/operationsByTags/read | Bevat een reeks bewerkingen die zijn gekoppeld met tags. |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/delete | Hiermee verwijdert u de configuratie van het beleid op de Api. |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/read | Configuratie van het beleid op het niveau van de API ophalen. of de configuratie van een vraag op de API-niveau. |
> | Bewerking | Microsoft.ApiManagement/service/apis/policies/write | Hiermee of bijwerken van de configuratie van beleid voor de API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/delete | Verwijderen van de configuratie van het beleid op API-niveau |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/read | Ophalen van de configuratie van een op API-niveau |
> | Bewerking | Microsoft.ApiManagement/service/apis/policy/write | Configuratie van beleid op API niveau maken |
> | Bewerking | Microsoft.ApiManagement/service/apis/products/read | Geeft een lijst van alle producten die de API deel uit van maakt. |
> | Bewerking | Microsoft.ApiManagement/service/apis/read | Geeft een lijst van alle API's van het exemplaar van API Management-service. of haalt de details van de API die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/delete | Hiermee verwijdert u alle versies van de API of verwijdert u de opgegeven versie in de API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/read | Geeft een lijst van alle versies van een API.<br>Een API-versie is gemaakt bij het maken van een API-revisie huidige.<br>Releases worden ook gebruikt voor terugdraaien naar eerdere versies.<br>De resultaten zullen worden weggeschreven en kunnen worden beperkt door de parameters $top en $skip.<br>of geeft de details van een API-versie. |
> | Bewerking | Microsoft.ApiManagement/service/apis/releases/write | Hiermee maakt u een nieuwe versie voor de API. of de details van de release van de API die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/apis/revisions/delete | Hiermee verwijdert u alle wijzigingen van een API |
> | Bewerking | Microsoft.ApiManagement/service/apis/revisions/read | Geeft een lijst van alle versies van een API. |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/delete | Hiermee verwijdert u de schemaconfiguratie op de Api. |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/read | De schemaconfiguratie ophalen op de API-niveau. of de schemaconfiguratie op de API-niveau. |
> | Bewerking | Microsoft.ApiManagement/service/apis/schemas/write | Hiermee of schemaconfiguratie voor de API bijgewerkt. |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Verwijder de beschrijving van de tag voor de Api. |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Geeft een lijst van alle Tags beschrijvingen binnen het bereik van de API. Model die vergelijkbaar is met het swagger - tagDescription is gedefinieerd in API-niveau maar tag kan worden toegewezen aan de bewerkingen of beschrijving van de Tag binnen het bereik van de API ophalen |
> | Bewerking | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Beschrijving van de tag binnen het bereik van de Api maken/bijwerken. |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/delete | De code van de Api loskoppelen. |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/read | Geeft een lijst van alle Tags die zijn gekoppeld aan de API. of tag die is gekoppeld aan de API ophalen. |
> | Bewerking | Microsoft.ApiManagement/service/apis/tags/write | Label toewijzen aan de Api. |
> | Bewerking | Microsoft.ApiManagement/service/apis/write | Hiermee maakt u nieuwe of bestaande opgegeven API van het exemplaar van API Management-service-updates. of de opgegeven API-Updates van het exemplaar van API Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/apisByTags/read | Geeft een lijst van een verzameling API's die zijn gekoppeld aan tags. |
> | Bewerking | Microsoft.ApiManagement/service/apiVersionSets/delete | Hiermee verwijdert u specifieke Api-versie instellen. |
> | Bewerking | Microsoft.ApiManagement/service/apiVersionSets/read | Geeft een lijst van een verzameling van Sets van API-versie in het opgegeven service-exemplaar. of haalt de details van de Api-versie is ingesteld door de id opgegeven. |
> | Bewerking | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Lijst met entiteiten van versie |
> | Bewerking | Microsoft.ApiManagement/service/apiVersionSets/write | Hiermee of een Set Api-versie bijwerken. of de details van de Api-VersionSet opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Werkt de Microsoft.ApiManagement resources die worden uitgevoerd in een Virtueelnetwerk kiezen bijgewerkte netwerkinstellingen. |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/delete | Hiermee verwijdert u specifieke autorisatie-server-exemplaar. |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/read | Geeft een lijst van een verzameling van autorisatieservers die zijn gedefinieerd in een service-exemplaar. of haalt de details van de autorisatie-server die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/authorizationServers/write | Hiermee maakt u nieuwe autorisatie-server of een bestaande autorisatieserver worden bijgewerkt. of de details van de autorisatie-server die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/backends/delete | Hiermee verwijdert u de opgegeven back-end. |
> | Bewerking | Microsoft.ApiManagement/service/backends/read | Geeft een lijst van een verzameling van back-ends in de opgegeven service-exemplaar. of haalt de details van de back-end die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/backends/reconnect/action | Meldt de APIM-proxy voor het maken van een nieuwe verbinding met de back-end na de opgegeven time-out. Als er geen time-out is opgegeven, wordt de time-out van twee minuten gebruikt. |
> | Bewerking | Microsoft.ApiManagement/service/backends/write | Hiermee of bijwerken van een back-end. of een bestaande back-end-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/backup/action | Back-API Management-Service voor de opgegeven container in een gebruiker opgegeven opslagaccount |
> | Bewerking | Microsoft.ApiManagement/service/caches/delete | Hiermee verwijdert u specifieke Cache. |
> | Bewerking | Microsoft.ApiManagement/service/caches/read | Geeft een lijst van een verzameling van alle externe Caches in de opgegeven service-exemplaar. of haalt de details van de Cache die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/caches/write | Hiermee of een externe Cache moet worden gebruikt in Api Management-instantie bijgewerkt. of de details van de cache die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/certificates/delete | Hiermee verwijdert u een specifiek certificaat. |
> | Bewerking | Microsoft.ApiManagement/service/certificates/read | Geeft een lijst van een verzameling van alle certificaten in het opgegeven service-exemplaar. of haalt de details van het certificaat dat is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/certificates/write | Hiermee of bijwerken van het certificaat dat wordt gebruikt voor verificatie bij de back-end. |
> | Bewerking | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Hiermee verwijdert u opgegeven item inhoud. |
> | Bewerking | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Retourneert de lijst met items van webinhoud of retourneert inhoudsitem details |
> | Bewerking | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Hiermee maakt u nieuwe inhoud item gemaakt of bijgewerkt opgegeven inhoudsitem |
> | Bewerking | Microsoft.ApiManagement/service/contentTypes/read | Retourneert lijst met inhoudstypen |
> | Bewerking | Microsoft.ApiManagement/service/delete | API Management-Service-exemplaar verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/delete | Hiermee verwijdert u de opgegeven diagnose. |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/read | Geeft een lijst van alle diagnostische gegevens van het exemplaar van API Management-service. of haalt de details van de diagnose is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/diagnostics/write | Hiermee maakt u een nieuwe diagnostische of een bestaande bijgewerkt. of de details van de diagnose is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/getssotoken/action | Haalt het SSO-token die kan worden gebruikt om aan te melden in API Management-Service Legacy-portal aan als beheerder |
> | Bewerking | Microsoft.ApiManagement/service/groups/delete | Hiermee verwijdert u een specifieke groep van het exemplaar van API Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/groups/read | Geeft een lijst van een verzameling van groepen die zijn gedefinieerd in een service-exemplaar. of haalt de details van de groep die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/delete | Bestaande gebruiker verwijderen uit bestaande groep. |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/read | Geeft een lijst van een verzameling gebruikersentiteiten die zijn gekoppeld aan de groep. |
> | Bewerking | Microsoft.ApiManagement/service/groups/users/write | Bestaande gebruiker toevoegen aan bestaande groep |
> | Bewerking | Microsoft.ApiManagement/service/groups/write | Hiermee of een groep bijgewerkt. of de details van de groep die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/delete | Hiermee verwijdert u de configuratie van de opgegeven id-provider. |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/read | Geeft een lijst van een verzameling van id-Provider is geconfigureerd in het opgegeven service-exemplaar. of haalt de configuratiedetails van de identiteit van de Provider geconfigureerd in de opgegeven service-exemplaar. |
> | Bewerking | Microsoft.ApiManagement/service/identityProviders/write | Hiermee of bijwerken van de configuratie van de id-provider. of een bestaande configuratie van de id-provider-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/issues/read | Geeft een lijst van een verzameling van problemen in het opgegeven service-exemplaar. of informatie over API Management wordt het probleem |
> | Bewerking | Microsoft.ApiManagement/service/locations/networkstatus/read | Hiermee haalt de netwerk-toegangsstatus van resources waarop de service is afhankelijk in de locatie. |
> | Bewerking | Microsoft.ApiManagement/service/loggers/delete | Hiermee verwijdert u het opgegeven logboek. |
> | Bewerking | Microsoft.ApiManagement/service/loggers/read | Geeft een lijst van een verzameling die u kunt in de opgegeven service-exemplaar. of haalt de details van het logboek opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/loggers/write | Hiermee of een logger bijgewerkt. of een bestaande logger bijgewerkt. |
> | Bewerking | Microsoft.ApiManagement/service/managedeployments/action | Wijzigen van SKU /-eenheden, regionale implementaties van API Management-Service toevoegen/verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/networkstatus/read | Hiermee haalt u de netwerk-toegangsstatus van resources waarop de service is afhankelijk. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/action | Melding verzendt naar een opgegeven gebruiker |
> | Bewerking | Microsoft.ApiManagement/service/notifications/read | Geeft een lijst van een verzameling eigenschappen die zijn gedefinieerd in een service-exemplaar. of haalt de details van de melding dat is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Hiermee verwijdert u het e-mailbericht in de lijst van de melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Haalt de lijst van de ontvanger van e-mailmeldingen geabonneerd op een melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Het e-mailadres toegevoegd aan de lijst met ontvangers voor de melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Hiermee verwijdert u de API Management-gebruiker in de lijst van de melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Haalt de lijst van de gebruiker van de ontvanger melding geabonneerd op de melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/recipientUsers/write | De API Management-gebruiker toevoegen aan de lijst met geadresseerden voor de melding. |
> | Bewerking | Microsoft.ApiManagement/service/notifications/write | Maak of beheer van API-updates publisher-meldingen. |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/delete | Hiermee verwijdert u specifieke OpenID Connect-Provider van de API Management service-exemplaar. |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/read | Lijsten van alle de OpenId Connect-provider. of haalt specifieke OpenID Connect-Provider. |
> | Bewerking | Microsoft.ApiManagement/service/openidConnectProviders/write | Hiermee of bijwerken van de OpenID Connect-Provider. of de specifieke OpenID Connect-Provider-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/operationresults/read | Huidige status van een langdurige bewerking opgehaald |
> | Bewerking | Microsoft.ApiManagement/service/policies/delete | Hiermee verwijdert u de configuratie van het globale beleid van de Api Management-Service. |
> | Bewerking | Microsoft.ApiManagement/service/policies/read | Geeft een lijst van alle globaal beleid-definities van de Api Management-service. of de definitie van het globale beleid van de Api Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/policies/write | Hiermee of bijwerken van de configuratie van het globale beleid van de Api Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/policy/delete | De configuratie van het beleid op het niveau van de Tenant verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/policy/read | Configuratie van het beleid op het niveau van de Tenant ophalen |
> | Bewerking | Microsoft.ApiManagement/service/policy/write | Configuratie van beleid op Tenant niveau maken |
> | Bewerking | Microsoft.ApiManagement/service/policySnippets/read | Geeft een lijst van alle beleidsfragmenten. |
> | Bewerking | Microsoft.ApiManagement/service/portalsettings/read | Meld u In instellingen ophalen voor de Portal of de aanmelding van de Get-Up van de instellingen voor de Portal of ophalen van de delegatie-instellingen voor de Portal. |
> | Bewerking | Microsoft.ApiManagement/service/portalsettings/write | Aanmelden-instellingen bijwerken. of maken of meld u In de Update-instellingen. of Update registreren instellingen of instellingen voor Update registreren of Update de delegatie-instellingen. of maken of bijwerken delegatie-instellingen. |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/delete | Hiermee verwijdert de opgegeven API uit het opgegeven product. |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/read | Een lijst met een verzameling van de API's die zijn gekoppeld aan een product. |
> | Bewerking | Microsoft.ApiManagement/service/products/apis/write | Een API toevoegen aan het opgegeven product. |
> | Bewerking | Microsoft.ApiManagement/service/products/delete | Product verwijderen. |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/delete | Hiermee verwijdert u de koppeling tussen de opgegeven groep en het product. |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/read | Geeft een lijst van de verzameling van de developer-groepen die zijn gekoppeld aan het opgegeven product. |
> | Bewerking | Microsoft.ApiManagement/service/products/groups/write | Hiermee wordt de koppeling tussen de ontwikkelaar van de opgegeven groep met het opgegeven product toegevoegd. |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/delete | Hiermee verwijdert u de configuratie van het beleid op het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/read | Configuratie van het beleid op productniveau ophalen. of de configuratie van het beleid op het niveau van het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/policies/write | Hiermee of bijwerken van de configuratie van beleid voor het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/delete | De configuratie van het beleid op het niveau van Product verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/read | Configuratie van het beleid op productniveau ophalen |
> | Bewerking | Microsoft.ApiManagement/service/products/policy/write | Configuratie van beleid op Product niveau maken |
> | Bewerking | Microsoft.ApiManagement/service/products/read | Geeft een lijst van een verzameling van producten in de opgegeven service-exemplaar. of haalt de details van het product dat door de id is opgegeven. |
> | Bewerking | Microsoft.ApiManagement/service/products/subscriptions/read | Geeft een lijst van de verzameling van de abonnementen voor het opgegeven product. |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/delete | Loskoppelen van de tag van het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/read | Geeft een lijst van alle Tags die zijn gekoppeld aan het Product. of code die is gekoppeld aan het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/tags/write | Label toewijzen aan het Product. |
> | Bewerking | Microsoft.ApiManagement/service/products/write | Hiermee of bijwerken van een product. of bestaande productdetails bijwerken. |
> | Bewerking | Microsoft.ApiManagement/service/productsByTags/read | Geeft een lijst van een verzameling van producten die zijn gekoppeld aan tags. |
> | Bewerking | Microsoft.ApiManagement/service/properties/delete | Hiermee verwijdert specifieke eigenschap uit de service-exemplaar van API Management. |
> | Bewerking | Microsoft.ApiManagement/service/properties/read | Geeft een lijst van een verzameling eigenschappen die zijn gedefinieerd in een service-exemplaar. of haalt de details van de eigenschap die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/properties/write | Hiermee maakt of een eigenschap updates. of Updates van de specifieke eigenschap. |
> | Bewerking | Microsoft.ApiManagement/service/quotas/periods/read | Itemwaarde quotum voor de periode ophalen |
> | Bewerking | Microsoft.ApiManagement/service/quotas/periods/write | Huidige waarde van het prestatiemeteritem quotum instellen |
> | Bewerking | Microsoft.ApiManagement/service/quotas/read | Waarden ophalen voor quotum |
> | Bewerking | Microsoft.ApiManagement/service/quotas/write | Huidige waarde van het prestatiemeteritem quotum instellen |
> | Bewerking | Microsoft.ApiManagement/service/read | Metagegevens voor een exemplaar van API Management-Service lezen |
> | Bewerking | Microsoft.ApiManagement/service/regions/read | Geeft een lijst van alle azure-regio's waarin de service bestaat. |
> | Bewerking | Microsoft.ApiManagement/service/reports/read | Get-rapport samengevoegd door perioden of Get-rapport samengevoegd per geografische regio of Get-rapport samengevoegd door ontwikkelaars.<br>of rapport samengevoegd door producten ophalen.<br>of rapport samengevoegd door API's of Get rapport samengevoegd door handelingen of Get-rapport samengevoegd per abonnement ophalen.<br>of rapportagegegevens aanvragen ophalen |
> | Bewerking | Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in een door de gebruiker opgegeven storage-account herstellen |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/delete | Hiermee verwijdert u het opgegeven abonnement. |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/read | Geeft een lijst van alle abonnementen van de API Management service-exemplaar. het opgegeven abonnemententiteit wordt opgehaald of ingesteld. |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Genereert opnieuw de primaire sleutel van het bestaande abonnement van de API Management service-exemplaar. |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Genereert opnieuw de secundaire sleutel van het bestaande abonnement van de API Management service-exemplaar. |
> | Bewerking | Microsoft.ApiManagement/service/subscriptions/write | Hiermee maakt of updates van het abonnement van de opgegeven gebruiker voor het opgegeven product. of de details van een abonnement dat is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/tagResources/read | Geeft een lijst van een verzameling resources die zijn gekoppeld met tags. |
> | Bewerking | Microsoft.ApiManagement/service/tags/delete | Hiermee verwijdert u een specifieke tag van het exemplaar van API Management-service. |
> | Bewerking | Microsoft.ApiManagement/service/tags/read | Geeft een lijst van een verzameling tags die zijn gedefinieerd in een service-exemplaar. of haalt de details van de opgegeven door de id-tag. |
> | Bewerking | Microsoft.ApiManagement/service/tags/write | Hiermee maakt u een tag. of de details van het label dat is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/templates/delete | Opnieuw instellen van e-mailsjabloon voor standaard-API Management |
> | Bewerking | Microsoft.ApiManagement/service/templates/read | Krijgt u alle e-mailsjablonen of e-mailadres van API Management wordt informatie van de sjabloon |
> | Bewerking | Microsoft.ApiManagement/service/templates/write | Maken of bijwerken van API Management-e-mailsjabloon of API Management Updates-e-mailsjabloon |
> | Bewerking | Microsoft.ApiManagement/service/tenant/delete | Configuratie van beleid voor de tenant verwijderen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/deploy/action | De Implementatietaak van een om toe te passen van wijzigingen van de opgegeven git-vertakking in de configuratie in de database wordt uitgevoerd. |
> | Bewerking | Microsoft.ApiManagement/service/tenant/operationResults/read | Lijst met resultaten van de bewerking of het resultaat van een bepaalde bewerking |
> | Bewerking | Microsoft.ApiManagement/service/tenant/read | De definitie van het globale beleid van de Api Management-service ophalen. of Get tenant toegangsgegevens informatie |
> | Bewerking | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Primaire toegangssleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Secundaire toegangssleutel opnieuw genereren |
> | Bewerking | Microsoft.ApiManagement/service/tenant/save/action | Hiermee maakt u doorvoeren met momentopname van de op de opgegeven vertakking in de opslagplaats |
> | Bewerking | Microsoft.ApiManagement/service/tenant/syncState/read | Status van laatste git synchronisatie ophalen |
> | Bewerking | Microsoft.ApiManagement/service/tenant/validate/action | Wijzigingen van de opgegeven git-branch wordt gevalideerd |
> | Bewerking | Microsoft.ApiManagement/service/tenant/write | Configuratie van beleid voor de tenant of Update tenant toegangsgegevens informatie instellen |
> | Bewerking | Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/updatehostname/action | Installeren, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
> | Bewerking | Microsoft.ApiManagement/service/users/action | Een nieuwe gebruiker registreren |
> | Bewerking | Microsoft.ApiManagement/service/users/confirmations/send/action | Stuurt de bevestiging |
> | Bewerking | Microsoft.ApiManagement/service/users/delete | Hiermee verwijdert u een specifieke gebruiker. |
> | Bewerking | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Hiermee haalt een Omleidings-URL met een verificatietoken voor een bepaalde gebruiker aanmelden bij de portal voor ontwikkelaars. |
> | Bewerking | Microsoft.ApiManagement/service/users/groups/read | Geeft een lijst van alle gebruikersgroepen. |
> | Bewerking | Microsoft.ApiManagement/service/users/identities/read | Lijst met alle gebruikers-id's. |
> | Bewerking | Microsoft.ApiManagement/service/users/keys/read | Sleutels die zijn gekoppeld aan een gebruiker ophalen |
> | Bewerking | Microsoft.ApiManagement/service/users/read | Geeft een lijst van een verzameling van geregistreerde gebruikers in de opgegeven service-exemplaar. of haalt de details van de gebruiker die is opgegeven door de id. |
> | Bewerking | Microsoft.ApiManagement/service/users/subscriptions/read | Geeft een lijst van de verzameling van abonnementen van de opgegeven gebruiker. |
> | Bewerking | Microsoft.ApiManagement/service/users/token/action | Hiermee haalt de autorisatie Shared Access Token voor de gebruiker. |
> | Bewerking | Microsoft.ApiManagement/service/users/write | Hiermee of een gebruiker bijgewerkt. of de details van de gebruiker die is opgegeven door de id-Updates. |
> | Bewerking | Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
> | Bewerking | Microsoft.ApiManagement/unregister/action | Opheffen van de registratie-abonnement voor de resourceprovider Microsoft.ApiManagement |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Authorization/classicAdministrators/delete | Hiermee verwijdert u de beheerder van het abonnement. |
> | Bewerking | Microsoft.Authorization/classicAdministrators/operationstatuses/read | Hiermee haalt u de bewerkingsstatussen van het abonnement. |
> | Bewerking | Microsoft.Authorization/classicAdministrators/read | Hiermee leest u de beheerders voor het abonnement. |
> | Bewerking | Microsoft.Authorization/classicAdministrators/write | Toevoegen of wijzigen van de beheerder naar een abonnement. |
> | Bewerking | Microsoft.Authorization/denyAssignments/delete | Verwijder de toewijzing van een weigeren bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/denyAssignments/read | Informatie ophalen over een toewijzing weigeren. |
> | Bewerking | Microsoft.Authorization/denyAssignments/write | Maak de toewijzing van een weigeren bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/elevateAccess/action | De oproepende functie verleent toegang bij het tenantbereik Administrator voor gebruikerstoegang |
> | Bewerking | Microsoft.Authorization/locks/delete | Verwijder de vergrendelingen bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/locks/read | Hiermee haalt u de vergrendelingen bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/locks/write | Hiermee worden vergrendelingen toegevoegd bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/operations/read | De lijst met bewerkingen opgehaald |
> | Bewerking | Microsoft.Authorization/permissions/read | Geeft een lijst van alle machtigingen die de aanroeper op een bepaald bereik heeft. |
> | Bewerking | Microsoft.Authorization/policyAssignments/delete | Een beleidstoewijzing bij het opgegeven bereik verwijderen. |
> | Bewerking | Microsoft.Authorization/policyAssignments/read | Informatie ophalen over een beleidstoewijzing. |
> | Bewerking | Microsoft.Authorization/policyAssignments/write | Een beleidstoewijzing maken bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/delete | Een beleidsdefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/read | Informatie ophalen over een beleidsdefinitie. |
> | Bewerking | Microsoft.Authorization/policyDefinitions/write | De definitie van een aangepast beleid maken. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/delete | Een beleidssetdefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/read | Informatie ophalen over een beleidssetdefinitie. |
> | Bewerking | Microsoft.Authorization/policySetDefinitions/write | Een aangepaste beleidssetdefinitie maken. |
> | Bewerking | Microsoft.Authorization/providerOperations/read | Bewerkingen ophalen voor alle resourceproviders die kunnen worden gebruikt in roldefinities. |
> | Bewerking | Microsoft.Authorization/roleAssignments/delete | Een roltoewijzing bij het opgegeven bereik verwijderen. |
> | Bewerking | Microsoft.Authorization/roleAssignments/read | Informatie ophalen over een roltoewijzing. |
> | Bewerking | Microsoft.Authorization/roleAssignments/write | Maak een roltoewijzing bij het opgegeven bereik. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/delete | De opgegeven aangepaste roldefinitie verwijderen. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/read | Informatie ophalen over een roldefinitie. |
> | Bewerking | Microsoft.Authorization/roleDefinitions/write | Maken of bijwerken van een aangepaste roldefinitie met opgegeven machtigingen of toewijsbare bereiken. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Een Azure Automation DSC-registratiegegevens lezen |
> | Bewerking | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Schrijft een aanvraag voor Azure Automation DSC-sleutels opnieuw genereren |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/delete | Hiermee verwijdert u een Azure Automation-certificaatasset |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/getCount/action | Leest u het aantal certificaten |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/read | Een Azure Automation-certificaatasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/certificates/write | Hiermee maken of bijwerken van een Azure Automation-certificaatasset |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/read | Een Azure Automation DSC van compilatie leest |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/read | Een Azure Automation DSC van compilatie leest |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/write | Een Azure Automation DSC van compilatie schrijft |
> | Bewerking | Microsoft.Automation/automationAccounts/compilationjobs/write | Een Azure Automation DSC van compilatie schrijft |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/content/read | Leest de configuratie van media-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/delete | Hiermee verwijdert u een Azure Automation DSC-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/getCount/action | Leest de telling van de inhoud van een Azure Automation DSC |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/read | Hiermee wordt de inhoud van een Azure Automation DSC opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/configurations/write | Schrijft een Azure Automation DSC-inhoud |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/delete | Hiermee verwijdert u een Azure Automation-verbindingsasset |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/getCount/action | Leest u het aantal verbindingen |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/read | Een Azure Automation-verbindingsasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/connections/write | Hiermee maken of bijwerken van een Azure Automation-verbindingsasset |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/delete | Hiermee verwijdert u een Azure Automation verbindingstype-asset |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/read | Een Azure Automation verbindingstype-asset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/connectionTypes/write | Hiermee maakt u een Azure Automation verbindingstype-asset |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/delete | Hiermee verwijdert u een Azure Automation-referentieasset |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/getCount/action | Leest de telling van referenties |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/read | Een Azure Automation-referentieasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/credentials/write | Hiermee maken of bijwerken van een Azure Automation-referentieasset |
> | Bewerking | Microsoft.Automation/automationAccounts/delete | Hiermee verwijdert u een Azure Automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Hiermee verwijdert u Resources voor de Hybrid Runbook Worker |
> | Bewerking | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak wordt hervat |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | De inhoud van de Azure Automation-runbook opgehaald op het moment van de taakuitvoering |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/stop/action | Een Azure Automation-taak gestopt |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation-taak onderbroken |
> | Bewerking | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/delete | Hiermee verwijdert u een Azure Automation-taakschema |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/read | Een Azure Automation-taakschema opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation-taakschema |
> | Bewerking | Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/listKeys/action | Leest de sleutels voor het automation-account |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/activities/read | Azure Automation-activiteiten opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/delete | Hiermee verwijdert u een Azure Automation-Powershell-module |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/getCount/action | Met deze eigenschap wordt de telling van de Powershell-modules binnen het Automation-Account |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/read | Haalt een Azure Automation-Powershell-module |
> | Bewerking | Microsoft.Automation/automationAccounts/modules/write | Hiermee maken of bijwerken van een Azure Automation-Powershell-module |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Hiermee verwijdert u een Azure Automation DSC-knooppuntconfiguratie |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Leest configuratie-inhoud voor een Azure Automation DSC-knooppunt |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Leest een Azure Automation DSC-knooppuntconfiguratie |
> | Bewerking | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Schrijft een Azure Automation DSC-knooppuntconfiguratie |
> | Bewerking | Microsoft.Automation/automationAccounts/nodecounts/read | Aantal knooppunten samenvatting leest voor het opgegeven type |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/delete | Hiermee verwijdert u Azure Automation DSC-knooppunten |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/read | Azure Automation DSC-knooppunten leest |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Azure Automation DSC rapportinhoud wordt gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/reports/read | Azure Automation DSC-rapporten lezen |
> | Bewerking | Microsoft.Automation/automationAccounts/nodes/write | Hiermee maken of bijwerken van Azure Automation DSC-knooppunten |
> | Bewerking | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Azure Automation TypeFields opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/python2Packages/delete | Hiermee verwijdert u een Azure Automation Python 2-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/python2Packages/read | Haalt een Azure Automation Python 2-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/python2Packages/write | Hiermee maken of bijwerken van een Azure Automation Python 2-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/python3Packages/delete | Hiermee verwijdert u een Azure Automation Python 3-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/python3Packages/read | Haalt een Azure Automation Python 3-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/python3Packages/write | Hiermee maken of bijwerken van een Azure Automation Python 3-pakket |
> | Bewerking | Microsoft.Automation/automationAccounts/read | Een Azure Automation-account opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/content/read | Hiermee wordt de inhoud van een Azure Automation-runbook opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/delete | Hiermee verwijdert u een Azure Automation-runbook |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Hiermee maakt u de inhoud van een Azure Automation-runbookconcept |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Resultaten van de bewerking concept voor Azure Automation-runbook opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/read | Een Azure Automation-runbookconcept opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Een testtaak voor Azure Automation runbookconcept opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Een testtaak voor Azure Automation runbookconcept hervat |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Een testtaak voor Azure Automation runbookconcept gestopt |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Een testtaak voor Azure Automation runbookconcept onderbroken |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Hiermee maakt u een testtaak voor runbookconcept van Azure Automation |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Bewerkingen van een Azure Automation-runbookconcept ongedaan maken |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Met deze eigenschap wordt de telling van Azure Automation-runbooks |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/publish/action | Een Azure Automation-runbookconcept gepubliceerd |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/runbooks/write | Hiermee maken of bijwerken van een Azure Automation-runbook |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/delete | Hiermee verwijdert u een Azure Automation-planningsasset |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/getCount/action | Met deze eigenschap wordt de telling van Azure Automation-planningen |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/read | Een Azure Automation-planningsasset opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/schedules/write | Hiermee maakt of een Azure Automation-planningsasset werkt |
> | Bewerking | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Hiermee verwijdert u de Software-Updateconfiguratie van een Azure Automation |
> | Bewerking | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Een Azure Automation Software-Updateconfiguratie opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Hiermee maakt of Software-Updateconfiguratie voor Azure Automation werkt |
> | Bewerking | Microsoft.Automation/automationAccounts/statistics/read | Gets Azure Automation Statistics |
> | Bewerking | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Een Azure Automation-update-implementatie virtuele machine ophalen |
> | Bewerking | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Een Azure Automation update management patch-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/usages/read | Gebruik Azure Automation opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/delete | Hiermee wordt een variabel Azure Automation-asset verwijderd |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/read | Hiermee wordt een variabel Azure Automation-asset gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/variables/write | Maakt of updatet een variabel Azure Automation-asset |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/delete | Een Azure Automation-watcher-taak verwijderen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/read | Een Azure Automation-watcher-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/start/action | Een Azure Automation-watcher-taak starten |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/stop/action | Een Azure Automation-watcher-taak stoppen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/streams/read | Een stream met Azure Automation watcher-taak opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Een Azure Automation watcher-Taakacties verwijderen |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Een Azure Automation-watcher-taak acties opgehaald |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Een Azure Automation-watcher-taak acties maken |
> | Bewerking | Microsoft.Automation/automationAccounts/watchers/write | Hiermee maakt u een Azure Automation-watcher-taak |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/action | Genereert een URI voor een Azure Automation-webhook |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/delete | Hiermee verwijdert u een Azure Automation-webhook  |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/read | Hiermee wordt een Azure Automation-webhook gelezen |
> | Bewerking | Microsoft.Automation/automationAccounts/webhooks/write | Hiermee maken of bijwerken van een Azure Automation-webhook |
> | Bewerking | Microsoft.Automation/automationAccounts/write | Hiermee maken of bijwerken van een Azure Automation-account |
> | Bewerking | Microsoft.Automation/operations/read | Beschikbare bewerkingen voor Azure Automation-resources opgehaald |
> | Bewerking | Microsoft.Automation/register/action | Hiermee wordt het abonnement voor Azure Automation |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/delete | B2C-mapresource verwijderen |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/read | B2C-mapresource weergeven |
> | Bewerking | Microsoft.AzureActiveDirectory/b2cDirectories/write | Maken of bijwerken van de B2C-mapresource |
> | Bewerking | Microsoft.AzureActiveDirectory/b2ctenants/read | Geeft een lijst van alle B2C-tenants, waarvan de gebruiker lid is |
> | Bewerking | Microsoft.AzureActiveDirectory/operations/read | Alle API-bewerkingen lezen beschikbaar voor de resourceprovider Microsoft.AzureActiveDirectory |
> | Bewerking | Microsoft.AzureActiveDirectory/register/action | Abonnement voor de resourceprovider Microsoft.AzureActiveDirectory registreren |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.AzureStack/Operations/read | De eigenschappen van een resource provider-bewerking opgehaald |
> | Bewerking | Microsoft.AzureStack/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.AzureStack |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Hiermee verwijdert u een abonnement van de klant Azure Stack |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/read | Haalt u de eigenschappen van een klant-abonnement van Azure Stack |
> | Bewerking | Microsoft.AzureStack/registrations/customerSubscriptions/write | Hiermee maakt of een abonnement voor Azure Stack klant werkt |
> | Bewerking | Microsoft.AzureStack/registrations/delete | Hiermee verwijdert u een Azure Stack-registratie |
> | Bewerking | Microsoft.AzureStack/registrations/getActivationKey/action | Haalt de meest recente sleutel van de Azure Stack-activering |
> | Bewerking | Microsoft.AzureStack/registrations/products/listDetails/action | Details voor een Azure Stack Marketplace-product uitgebreid opgehaald |
> | Bewerking | Microsoft.AzureStack/registrations/products/read | Haalt u de eigenschappen van een Azure Stack Marketplace-product |
> | Bewerking | Microsoft.AzureStack/registrations/read | Haalt u de eigenschappen van een Azure Stack-registratie |
> | Bewerking | Microsoft.AzureStack/registrations/write | Hiermee maken of bijwerken van een Azure Stack-registratie |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/delete | Hiermee verwijdert u een toepassing |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/read | Een lijst met toepassingen of haalt u de eigenschappen van een toepassing |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Hiermee activeert u een toepassingspakket |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/delete | Hiermee verwijdert u een toepassingspakket |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/read | Hiermee worden de eigenschappen van een toepassingspakket opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/versions/write | Hiermee maakt u een nieuw toepassingspakket of een bestaand toepassingspakket bijgewerkt |
> | Bewerking | Microsoft.Batch/batchAccounts/applications/write | Hiermee maakt u een nieuwe toepassing of een bestaande toepassing bijwerkt |
> | Bewerking | Microsoft.Batch/batchAccounts/certificateOperationResults/read | De resultaten van een langdurige certificaatbewerking op een Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Hiermee annuleert u de mislukte verwijdering van een certificaat op een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/delete | Hiermee verwijdert u een certificaat van een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/read | Vindt u certificaten in een Batch-account of haalt u de eigenschappen van een certificaat |
> | Bewerking | Microsoft.Batch/batchAccounts/certificates/write | Hiermee maakt u een nieuw certificaat op een Batch-account of een bestaand certificaat updates |
> | Bewerking | Microsoft.Batch/batchAccounts/delete | Hiermee verwijdert u een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/listkeys/action | Een lijst met toegangssleutels voor een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/operationResults/read | De resultaten van een langdurige bewerking van de Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/poolOperationResults/read | De resultaten van een langdurige bewerking in de groep van toepassingen op een Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/delete | Hiermee verwijdert u een groep van een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Schakelt automatische schaling voor een Batch-pool-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/read | Een lijst met groepen in een Batch-account of haalt u de eigenschappen van een groep |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/stopResize/action | Stopt een lopende vergroten of verkleinen van de bewerking op een Batch-pool-account |
> | Bewerking | Microsoft.Batch/batchAccounts/pools/write | Hiermee maakt u een nieuwe groep in een Batch-account of een bestaande pool-updates |
> | Bewerking | Microsoft.Batch/batchAccounts/read | Geeft een lijst van Batch-accounts of haalt u de eigenschappen van een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/regeneratekeys/action | Opnieuw toegang krijgen tot de sleutels voor een Batch-account |
> | Bewerking | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Toegangssleutels voor het opslagaccount automatisch is geconfigureerd voor een Batch-account wordt gesynchroniseerd |
> | Bewerking | Microsoft.Batch/batchAccounts/write | Hiermee maakt u een nieuwe Batch-account of een bestaande Batch-account bijgewerkt |
> | Bewerking | Microsoft.Batch/locations/accountOperationResults/read | De resultaten van een langdurige bewerking van de Batch-account opgehaald |
> | Bewerking | Microsoft.Batch/locations/checkNameAvailability/action | Controleert of de accountnaam geldig en niet in gebruik is. |
> | Bewerking | Microsoft.Batch/locations/quotas/read | Batch quotum van het opgegeven abonnement opgehaald op de opgegeven Azure-regio |
> | Bewerking | Microsoft.Batch/operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op de provider van Microsoft.Batch resource |
> | Bewerking | Microsoft.Batch/register/action | Hiermee wordt het abonnement voor de Batch-Resourceprovider geregistreerd en wordt het maken van Batch-accounts |
> | Bewerking | Microsoft.Batch/unregister/action | De registratie van het abonnement voor de Batch-Resourceprovider te voorkomen dat het maken van Batch-accounts |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Billing/billingAccounts/departments/read | Lijst van alle afdelingen onder een facturering accountbereik |
> | Bewerking | Microsoft.Billing/billingAccounts/enrollmentAccounts/read | Lijst van alle inschrijvingsaccounts onder een facturering accountbereik |
> | Bewerking | Microsoft.Billing/billingAccounts/read | Alle facturering accounts waarmee de gebruiker toegang heeft tot weergeven |
> | Bewerking | Microsoft.Billing/billingPeriods/read | Een lijst met beschikbare factureringsperioden |
> | Bewerking | Microsoft.Billing/departments/read | Lijst van alle afdelingen welke gebruiker toegang heeft tot |
> | Bewerking | Microsoft.Billing/invoices/read | Een lijst met beschikbare facturen |
> | Bewerking | Microsoft.Billing/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.Billing |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.BingMaps/mapApis/Delete | Verwijderbewerking |
> | Bewerking | Microsoft.BingMaps/mapApis/listSecrets/action | De geheimen vermelden |
> | Bewerking | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Eenmalige aanmelding van de verificatietoken voor de Resource lezen |
> | Bewerking | Microsoft.BingMaps/mapApis/Read | Leesbewerking |
> | Bewerking | Microsoft.BingMaps/mapApis/regenerateKey/action | De sleutel opnieuw genereren |
> | Bewerking | Microsoft.BingMaps/mapApis/Write | Schrijfbewerking |
> | Bewerking | Microsoft.BingMaps/Operations/read | Beschrijving van de bewerking. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/delete | Hiermee verwijdert u een bestaand Blockchain-lid. |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/read | Opgehaald of een lijst met bestaande Blockchain-leden. |
> | DataAction | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Maakt verbinding met een lid van de Blockchain-transactie-knooppunt. |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/transactionNodes/delete | Hiermee verwijdert u een bestaand Blockchain lid transactie-knooppunt. |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Opgehaald of een lijst met bestaande Blockchain lid transactie knooppunten. |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/transactionNodes/write | Hiermee of een lid van de Blockchain-transactie-knooppunt wordt bijgewerkt. |
> | Bewerking | Microsoft.Blockchain/blockchainMembers/write | Hiermee of een lid van de Blockchain bijgewerkt. |
> | Bewerking | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Hiermee haalt u de resultaten van de bewerking van Blockchain-leden. |
> | Bewerking | Microsoft.Blockchain/locations/checkNameAvailability/action | Controleert of de resourcenaam geldig is en niet wordt gebruikt. |
> | Bewerking | Microsoft.Blockchain/operations/read | Lijst met alle bewerkingen in Microsoft Blockchain-Resourceprovider. |
> | Bewerking | Microsoft.Blockchain/register/action | Het abonnement voor de Blockchain-Resourceprovider geregistreerd. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Alle blauwdrukartefacten lezen |
> | Bewerking | Microsoft.Blueprint/blueprintAssignments/delete | Alle blauwdrukartefacten verwijderen |
> | Bewerking | Microsoft.Blueprint/blueprintAssignments/read | Alle blauwdrukartefacten lezen |
> | Bewerking | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Blauwdrukken voor Azure-service-principal-object id ophalen |
> | Bewerking | Microsoft.Blueprint/blueprintAssignments/write | Maken of bijwerken van alle blauwdrukartefacten |
> | Bewerking | Microsoft.Blueprint/blueprints/artifacts/delete | Alle blauwdrukartefacten verwijderen |
> | Bewerking | Microsoft.Blueprint/blueprints/artifacts/read | Alle blauwdrukartefacten lezen |
> | Bewerking | Microsoft.Blueprint/blueprints/artifacts/write | Maken of bijwerken van alle blauwdrukartefacten |
> | Bewerking | Microsoft.Blueprint/blueprints/delete | Alle blauwdrukken verwijderen |
> | Bewerking | Microsoft.Blueprint/blueprints/read | Alle blauwdrukken lezen |
> | Bewerking | Microsoft.Blueprint/blueprints/versions/artifacts/read | Alle blauwdrukartefacten lezen |
> | Bewerking | Microsoft.Blueprint/blueprints/versions/delete | Alle blauwdrukken verwijderen |
> | Bewerking | Microsoft.Blueprint/blueprints/versions/read | Alle blauwdrukken lezen |
> | Bewerking | Microsoft.Blueprint/blueprints/versions/write | Alle blauwdrukken maken of bijwerken |
> | Bewerking | Microsoft.Blueprint/blueprints/write | Alle blauwdrukken maken of bijwerken |
> | Bewerking | Microsoft.Blueprint/register/action | Hiermee wordt de Resourceprovider Azure blauwdrukken geregistreerd |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.BotService/botServices/channels/delete | Verwijderen van een Bot-Service |
> | Bewerking | Microsoft.BotService/botServices/channels/read | Lees een Botservice |
> | Bewerking | Microsoft.BotService/botServices/channels/write | Schrijven van een Bot-Service |
> | Bewerking | Microsoft.BotService/botServices/connections/delete | Verwijderen van een Bot-Service |
> | Bewerking | Microsoft.BotService/botServices/connections/read | Lees een Botservice |
> | Bewerking | Microsoft.BotService/botServices/connections/write | Schrijven van een Bot-Service |
> | Bewerking | Microsoft.BotService/botServices/delete | Verwijderen van een Bot-Service |
> | Bewerking | Microsoft.BotService/botServices/read | Lees een Botservice |
> | Bewerking | Microsoft.BotService/botServices/write | Schrijven van een Bot-Service |
> | Bewerking | Microsoft.BotService/locations/operationresults/read | De status van een asynchrone bewerking lezen |
> | Bewerking | Microsoft.BotService/Operations/read | De leesbewerkingen voor alle resourcetypen |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Cache/checknameavailability/action | Controleert of een naam beschikbaar voor gebruik met een nieuwe Redis-Cache is |
> | Bewerking | Microsoft.Cache/locations/operationresults/read | Met deze eigenschap wordt het resultaat van een langdurige bewerking waarvoor de header 'Location' eerder is geretourneerd naar de client uitgevoerd |
> | Bewerking | Microsoft.Cache/operations/read | Geeft een lijst van de bewerkingen die ondersteuning biedt voor de provider 'Microsoft.cache ongedaan maken'. |
> | Bewerking | Microsoft.Cache/redis/delete | Volledige Redis-Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/export/action | Redis-gegevens exporteren naar vooraf ingestelde opslag-blobs in de opgegeven indeling |
> | Bewerking | Microsoft.Cache/redis/firewallRules/delete | IP-firewall-regels van een Redis-Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/firewallRules/read | De IP-firewall-regels van een Redis-Cache ophalen |
> | Bewerking | Microsoft.Cache/redis/firewallRules/write | De IP-firewall-regels van een Redis-Cache bewerken |
> | Bewerking | Microsoft.Cache/redis/forceReboot/action | Geforceerd opnieuw opstarten van een cache-instantie met mogelijk gegevensverlies. |
> | Bewerking | Microsoft.Cache/redis/import/action | Gegevens van een opgegeven indeling in Redis importeren vanuit meerdere blobs |
> | Bewerking | Microsoft.Cache/redis/linkedservers/delete | Gekoppelde Server uit een Redis-Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/linkedservers/read | Gekoppelde Servers die zijn gekoppeld aan een redis-cache ophalen. |
> | Bewerking | Microsoft.Cache/redis/linkedservers/write | Gekoppelde Server toevoegen aan een Redis-Cache |
> | Bewerking | Microsoft.Cache/redis/listKeys/action | De waarde van de toegangssleutels voor Redis-Cache weergeven in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/listUpgradeNotifications/read | Lijst met de meest recente meldingen voor een upgrade uitvoert voor de cachetenant. |
> | Bewerking | Microsoft.Cache/redis/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor een Redis-Cache |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/delete | De patch-schema van een Redis-Cache verwijderen |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/read | Het toepassen van patches schema van een Redis-Cache opgehaald |
> | Bewerking | Microsoft.Cache/redis/patchSchedules/write | Het toepassen van patches schema van een Redis-Cache aanpassen |
> | Bewerking | Microsoft.Cache/redis/read | De instellingen en configuratie van de Redis-Cache weergeven in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/regenerateKey/action | Wijzig de waarde van de toegangssleutels voor Redis-Cache in de beheerportal |
> | Bewerking | Microsoft.Cache/redis/start/action | Start een cache-instantie. |
> | Bewerking | Microsoft.Cache/redis/stop/action | Stop een cache-exemplaar. |
> | Bewerking | Microsoft.Cache/redis/write | Wijzig de instellingen en configuratie in de beheerportal van de Redis-Cache |
> | Bewerking | Microsoft.Cache/register/action | De resourceprovider 'Microsoft.cache ongedaan maken' voor een abonnement geregistreerd |
> | Bewerking | Microsoft.Cache/unregister/action | De registratie van de resourceprovider 'Microsoft.cache ongedaan maken' met een abonnement |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Capacity/appliedreservations/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/calculateexchange/action | Berekent het bedrag van exchange en de prijs van nieuwe inkoop en beleid fouten retourneert. |
> | Bewerking | Microsoft.Capacity/calculateprice/action | De prijs van een reservering berekenen |
> | Bewerking | Microsoft.Capacity/catalogs/read | Catalogus van de reservering lezen |
> | Bewerking | Microsoft.Capacity/checkoffers/action | Geen Abonnementaanbiedingen controleren |
> | Bewerking | Microsoft.Capacity/checkscopes/action | Elk abonnement controleren |
> | Bewerking | Microsoft.Capacity/commercialreservationorders/read | Reserveringsorders gemaakt in een Tenant ophalen |
> | Bewerking | Microsoft.Capacity/exchange/action | Een reservering uitwisselen |
> | Bewerking | Microsoft.Capacity/operations/read | Elke leesbewerking |
> | Bewerking | Microsoft.Capacity/register/action | Hiermee wordt de capaciteit-resourceprovider geregistreerd en wordt het maken van resources van de capaciteit. |
> | Bewerking | Microsoft.Capacity/reservationorders/action | Een reservering bijwerken |
> | Bewerking | Microsoft.Capacity/reservationorders/availablescopes/action | Een beschikbare bereik zoeken |
> | Bewerking | Microsoft.Capacity/reservationorders/calculaterefund/action | Berekent de terug te betalen bedrag en de prijs van nieuwe inkoop en beleid fouten retourneert. |
> | Bewerking | Microsoft.Capacity/reservationorders/delete | Alle-reservering verwijderen |
> | Bewerking | Microsoft.Capacity/reservationorders/merge/action | Een reservering samenvoegen |
> | Bewerking | Microsoft.Capacity/reservationorders/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/action | Een reservering bijwerken |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/delete | Alle-reservering verwijderen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/revisions/read | Alle reserveringen lezen |
> | Bewerking | Microsoft.Capacity/reservationorders/reservations/write | Een reservering maken |
> | Bewerking | Microsoft.Capacity/reservationorders/return/action | Retourneert een reservering |
> | Bewerking | Microsoft.Capacity/reservationorders/split/action | Een reservering splitsen |
> | Bewerking | Microsoft.Capacity/reservationorders/swap/action | Een reservering wisselen |
> | Bewerking | Microsoft.Capacity/reservationorders/write | Een reservering maken |
> | Bewerking | Microsoft.Capacity/tenants/register/action | Een Tenant registreren |
> | Bewerking | Microsoft.Capacity/unregister/action | Een Tenant de registratie ongedaan maken |
> | Bewerking | Microsoft.Capacity/validatereservationorder/action | Een reservering valideren |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
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
> | Bewerking | Microsoft.Cdn/register/action | Hiermee wordt het abonnement voor de CDN-resourceprovider geregistreerd en wordt het maken van CDN-profielen. |
> | Bewerking | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Een bestaand certificaat verwijderen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | De lijst met certificaten ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Toevoegen van een nieuw certificaat of een bestaande bijgewerkt |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Delete | Een bestaande App service Certificate verwijderen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Read | De lijst met CertificateOrders ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Een bestaande certificaatbestelling opnieuw uitgeven |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Een bestaande certificaatbestelling vernieuwen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Certificaat e-mail opnieuw verzenden |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Aanvraag voor e-mailberichten naar een ander e-mailadres verzenden |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Zegel van de site voor een verleend App Service-certificaat ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | De lijst met certificaatacties ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Certificaat e-mailgeschiedenis ophalen |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Domeineigendom controleren |
> | Bewerking | Microsoft.CertificateRegistration/certificateOrders/Write | Een nieuwe Certificaatbestelling toevoegen of een bestaande bijgewerkt |
> | Bewerking | Microsoft.CertificateRegistration/operations/Read | Lijst van alle bewerkingen van app service certificate-registratie |
> | Bewerking | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Service-principal inrichten voor app-service-principal |
> | Bewerking | Microsoft.CertificateRegistration/register/action | Registreer de resourceprovider Microsoft Certificates voor het abonnement |
> | Bewerking | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Aankoop certificaatobject valideren zonder deze te verzenden |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicCompute/capabilities/read | Geeft de mogelijkheden |
> | Bewerking | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Controleert de beschikbaarheid van een bepaald domeinnaam. |
> | Bewerking | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Hiermee haalt u de beschikbaarheid van een bepaald domeinnaam. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/active/write | Hiermee stelt u de domeinnaam van het actieve. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/availabilitySets/read | De beschikbaarheidsset voor de resource weergeven. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/capabilities/read | Bevat de mogelijkheden voor domeinnaamgeving |
> | Bewerking | Microsoft.ClassicCompute/domainNames/delete | De domeinnamen voor resources verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Geeft de implementatiesites. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/read | Rol van implementatiesleuf van domeinnaam ophalen |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/roleinstances/read | Rolexemplaar ophalen voor de rol van implementatiesleuf van domeinnaam |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | De status van de implementatiesleuf ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | De status van de implementatiesleuf toevoegen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/read | Upgradedomein ophalen voor de implementatiesite op domeinnaam |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/write | Upgradedomein voor de implementatiesite op de domeinnaam van het bijwerken |
> | Bewerking | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Maken of bijwerken van de implementatie. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/delete | De domeinnaamextensies verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/read | Retourneert het domein bestandsnaamextensies. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/extensions/write | De domeinnaamextensies toevoegen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Een nieuwe interne taakverdeling verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de interne load balancers van domeinnamen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Hiermee haalt u de interne load balancers. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Hiermee maakt u een nieuwe interne taakverdeling. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de gelezen voor de domeinnamen eindpuntsets van taakverdeling. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Ophalen van de eindpuntsets van taakverdeling. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | De set met gelijke eindpunt toevoegen |
> | Bewerking | Microsoft.ClassicCompute/domainNames/operationstatuses/read | De bewerkingsstatus van de domeinnaam ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/read | De domeinnamen voor resources retourneren. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | De gebruikte servicecertificaten verwijderen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de servicecertificaten van domeinnamen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Hiermee wordt de gebruikte servicecertificaten geretourneerd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Toevoegen of wijzigen van de servicecertificaten die worden gebruikt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Hiermee wordt de migratie van een implementatiesite afgebroken. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Migratie van een implementatiesite worden doorgevoerd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/delete | Hiermee wordt een bepaald implementatiesleuf verwijderd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de domeinnaamsleuven gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Zorgt ervoor dat de migratie van een implementatiesite. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/read | Geeft de implementatiesites. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Verwijder de extensieverwijzing voor de implementatiesiterol. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de extensieverwijzing van domein namen sleuven rollen gelezen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Retourneert de extensieverwijzing voor de implementatiesiterol. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Toevoegen of wijzigen van de extensieverwijzing voor de implementatiesiterol. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | De metrische definitie van de rol voor de domeinnaam ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Rol van metrische gegevens voor de domeinnaam ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Bewerkingsstatus ophalen voor de rol implementatiesite voor domein namen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | De instellingen voor diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Toevoegen of wijzigen van instellingen voor diagnostische gegevens. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de metrische definities. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/read | Hiermee wordt de rol van de implementatiesleuf. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Verbinding met extern bureaublad-bestand voor de rolinstantie op de functie de naam van site downloadt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Hiermee haalt u de status van de bewerking voor de rolinstantie op rol implementatiesite voor domein namen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | De rolinstantie ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | De rolinstantie, wordt opnieuw gemaakt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | De rolinstantie werkrolgrootte. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Rolinstanties opnieuw is opgestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Sku van voor de implementatiesleuf ophalen. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/roles/write | Rol voor de implementatiesite toevoegt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/start/action | Een implementatiesleuf gestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Wijzigt de status van de implementatiesleuf gestopt. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Wijzigt de status van de implementatiesleuf gestart. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/stop/action | Hiermee wordt de implementatiesleuf onderbroken. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Overzicht wordt uitgevoerd. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Migratie van een implementatiesite valideert. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/slots/write | Maken of bijwerken van de implementatie. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/swap/action | Hiermee wisselt de staging-site naar de productiesite. |
> | Bewerking | Microsoft.ClassicCompute/domainNames/write | Toevoegen of wijzigen van de domeinnamen voor resources. |
> | Bewerking | Microsoft.ClassicCompute/moveSubscriptionResources/action | Alle klassieke resources wilt verplaatsen naar een ander abonnement. |
> | Bewerking | Microsoft.ClassicCompute/operatingSystemFamilies/read | Een lijst met de gastbesturingssysteemfamilies beschikbaar zijn in Microsoft Azure en bevat ook de besturingssysteemversies beschikbaar voor elke serie. |
> | Bewerking | Microsoft.ClassicCompute/operatingSystems/read | Geeft de versies van het gastbesturingssysteem die momenteel beschikbaar in Microsoft Azure zijn. |
> | Bewerking | Microsoft.ClassicCompute/operations/read | Hiermee haalt u de lijst met bewerkingen. |
> | Bewerking | Microsoft.ClassicCompute/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de resource gelezen. |
> | Bewerking | Microsoft.ClassicCompute/quotas/read | De quota voor het abonnement ophalen. |
> | Bewerking | Microsoft.ClassicCompute/register/action | Registreren bij klassieke berekening |
> | Bewerking | Microsoft.ClassicCompute/resourceTypes/skus/read | Hiermee haalt de Sku-lijst voor ondersteunde resourcetypen. |
> | Bewerking | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | De beschikbaarheid van het abonnement voor de klassieke verplaatsingsbewerking valideren. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Hiermee verwijdert u de netwerkbeveiligingsgroep die is gekoppeld aan de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee leest u de status van de bewerking voor de virtuele machines die zijn gekoppeld netwerkbeveiligingsgroepen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Hiermee haalt u de netwerkbeveiligingsgroep die is gekoppeld aan de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Voegt een netwerkbeveiligingsgroep die is gekoppeld aan de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | De mogelijk asynchrone bewerkingen opgehaald |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Hiermee wordt een gegevensschijf gekoppeld aan een virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/capture/action | Een virtuele machine vastleggen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/delete | Hiermee verwijdert u virtuele machines. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Hiermee wordt een gegevensschijf van virtuele machine losgekoppeld. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Virtuele machine diagnostische instellingen ophalen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/disks/read | Lijst met gegevensschijven opgehaald |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Downloadt het RDP-bestand voor de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de extensies van virtuele machines gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/read | Hiermee haalt u de extensie van de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/extensions/write | Hebt u de extensie van de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | De metrische definitie van de virtuele machine ophalen |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/metrics/read | Hiermee haalt u de metrische gegevens. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Hiermee verwijdert u de netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee leest u de status van de bewerking voor de virtuele machines die zijn gekoppeld netwerkbeveiligingsgroepen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Hiermee haalt u de netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Hiermee voegt u een netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface toe. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele machines gelezen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Hiermee wordt onderhoud uitgevoerd op de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | De instellingen voor diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Toevoegen of wijzigen van instellingen voor diagnostische gegevens. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de metrische definities. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/read | Lijst met virtuele machines opgehaald. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Implementeert de virtuele machine opnieuw. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/restart/action | Virtuele machines opnieuw is opgestart. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/shutdown/action | De virtuele machine wordt afgesloten. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/start/action | Start de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/stop/action | Hiermee stopt u de virtuele machine. |
> | Bewerking | Microsoft.ClassicCompute/virtualMachines/write | Toevoegen of wijzigen van virtuele machines. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Een express route cross-verbindingsstatus van de bewerking ophalen |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Express route cross-peering verbinding verwijderen. |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/operationstatuses/read | Een express route cross-verbindingsstatus peering bewerking ophalen |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Express route cross-peering verbinding ophalen. |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Express route cross-peering verbinding toevoegen. |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/read | Ophalen van express route cross-verbindingen. |
> | Bewerking | Microsoft.ClassicNetwork/expressroutecrossconnections/write | Express route toevoegen cross-verbindingen. |
> | Bewerking | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Hiermee haalt de lijst met ondersteunde apparaten. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Hiermee verwijdert u de netwerkbeveiligingsgroep. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Hiermee wordt de bewerkingsstatus voor de netwerkbeveiligingsgroep gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | De diagnostische instellingen voor Netwerkbeveiligingsgroepen opgehaald |
> | Bewerking | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee of de Netwerkbeveiligingsgroepen diagnostische instellingen voor deze bewerking wordt aangevuld door de resourceprovider voor inzichten bijgewerkt. |
> | Bewerking | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Hiermee haalt u de gebeurtenissen voor netwerkbeveiligingsgroep |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/read | Hiermee wordt de netwerkbeveiligingsgroep opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Hiermee verwijdert u de security rule. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de regels voor de beveiliging netwerkbeveiligingsgroep gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Hiermee haalt u de security rule. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Toevoegen of bijwerken van een beveiligingsregel. |
> | Bewerking | Microsoft.ClassicNetwork/networkSecurityGroups/write | Voegt een nieuwe netwerkbeveiligingsgroep. |
> | Bewerking | Microsoft.ClassicNetwork/operations/read | Klassiek netwerkbewerkingen ophalen. |
> | Bewerking | Microsoft.ClassicNetwork/quotas/read | De quota voor het abonnement ophalen. |
> | Bewerking | Microsoft.ClassicNetwork/register/action | Registreren bij klassiek netwerk |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/delete | Een gereserveerd Ip verwijderd. |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/join/action | Deelnemen aan een gereserveerde IP-adres |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde Ip koppelen |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de gereserveerde IP's gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/read | De gereserveerde IP-adressen opgehaald |
> | Bewerking | Microsoft.ClassicNetwork/reservedIps/write | Een nieuw gereserveerd Ip toevoegen |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Hiermee wordt de migratie van een Virtueelnetwerk afgebroken |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Geeft de mogelijkheden |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Controleert de beschikbaarheid van een opgegeven IP-adres in een virtueel netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Hiermee wordt de migratie van een Virtueelnetwerk doorgevoerd |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/delete | Hiermee verwijdert u het virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Een clientcertificaat unrevokes. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | De ingetrokken clientcertificaten lezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Hiermee wordt een clientcertificaat ingetrokken. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Hiermee verwijdert u het clientcertificaat voor gateway van virtueel netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Certificaat downloaden op basis van miniatuur. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Een lijst met certificaat gatewaypakket voor het virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | De client basiscertificaten vinden. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Hiermee wordt een nieuw clienthoofdcertificaat geüpload. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Verbinding maakt een site-naar-site-gateway-verbinding. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Hiermee wordt een site-naar-site-gateway-verbinding. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | De lijst met verbindingen opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Test de verbinding van een site-naar-site-gateway. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Hiermee verwijdert u de virtuele netwerkgateway. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Het apparaatconfiguratiescript downloadt. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | De diagnostische gegevens over de gateway gedownload. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Hiermee wordt de circuitservicesleutel opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Geeft een lijst van de gateway-pakket van het virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerkgateways gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Het gatewaypakket voor het virtuele netwerk opgehaald. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Hiermee haalt u de virtuele netwerkgateways. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Hiermee start u diagnostische voor de virtuele netwerkgateway. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Hiermee stopt u de diagnostische gegevens voor de virtuele netwerkgateway. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Hiermee wordt een virtuele netwerkgateway toegevoegd. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/join/action | Lid wordt van het virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de virtuele netwerken gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Een virtueel netwerk met een ander virtueel netwerk van collega's. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/prepareMigration/action | De migratie van een Virtueelnetwerk voorbereid |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/read | Ophalen van het virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Hiermee verwijdert u de proxy voor de peering van het externe virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Hiermee haalt u de proxy voor de peering van het externe virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Wordt toegevoegd of bijgewerkt van de proxy voor de peering van het externe virtuele netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Hiermee verwijdert u de netwerkbeveiligingsgroep die is gekoppeld aan het subnet. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Hiermee wordt de status van de bewerking voor het virtuele netwerk subnet gekoppelde netwerkbeveiligingsgroep gelezen. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Hiermee haalt u de netwerkbeveiligingsgroep die is gekoppeld aan het subnet. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Hiermee voegt u een netwerkbeveiligingsgroep die is gekoppeld aan het subnet toe. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/validateMigration/action | Valideert de migratie van een Virtueelnetwerk |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Hiermee haalt u de peering op virtueel netwerk. |
> | Bewerking | Microsoft.ClassicNetwork/virtualNetworks/write | Voeg een nieuw virtueel netwerk. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ClassicStorage/capabilities/read | Geeft de mogelijkheden |
> | Bewerking | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Controleert de beschikbaarheid van een storage-account. |
> | Bewerking | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | Profiteer van de beschikbaarheid van een storage-account. |
> | Bewerking | Microsoft.ClassicStorage/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/images/operationstatuses/read | Hiermee haalt u de bewerkingsstatus van de installatiekopie. |
> | Bewerking | Microsoft.ClassicStorage/images/read | Retourneert de installatiekopie. |
> | Bewerking | Microsoft.ClassicStorage/operations/read | Klassieke opslagbewerkingen opgehaald |
> | Bewerking | Microsoft.ClassicStorage/osImages/read | Retourneert de installatiekopie van het besturingssysteem. |
> | Bewerking | Microsoft.ClassicStorage/osPlatformImages/read | Hiermee haalt u de installatiekopie van besturingssysteem platform. |
> | Bewerking | Microsoft.ClassicStorage/publicImages/read | Hiermee haalt u de installatiekopie van het openbare virtuele machine. |
> | Bewerking | Microsoft.ClassicStorage/quotas/read | De quota voor het abonnement ophalen. |
> | Bewerking | Microsoft.ClassicStorage/register/action | Registreren bij klassieke opslag |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Hiermee wordt de migratie van een opslagaccount afgebroken. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Migratie van een opslagaccount doorgevoerd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/delete | Het storage-account verwijderen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/delete | Hiermee wordt een opgegeven opslagaccountschijf verwijderd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de resource gelezen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/disks/write | Een opslagaccountschijf toegevoegd. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/images/delete | Hiermee verwijdert u een installatiekopie van het opgegeven opslagaccount. (Afgeschaft. Use 'Microsoft.ClassicStorage/storageAccounts/vmImages') |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Retourneert de bewerkingsstatus van de storage-account-installatiekopie. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/images/read | Retourneert de installatiekopie van het opslagaccount. (Afgeschaft. Use 'Microsoft.ClassicStorage/storageAccounts/vmImages') |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Geeft een lijst van de toegangssleutels voor de storage-accounts. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de resource gelezen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Hiermee verwijdert u een installatiekopie van het opgegeven opslagaccount account besturingssysteem. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/osImages/read | Retourneert de installatiekopie van het besturingssysteem opslagaccount. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/osImages/write | Wordt toegevoegd een installatiekopie van het opgegeven opslagaccount account besturingssysteem. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Migratie van een opslagaccount voorbereid. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/read | Retourneert de storage-account met het opgegeven account. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Genereert opnieuw de bestaande toegangssleutels voor het opslagaccount. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | De instellingen voor diagnostische gegevens ophalen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Toevoegen of wijzigen van instellingen voor diagnostische gegevens. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Hiermee haalt u de metrische definities. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Hiermee haalt u de metrische gegevens. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/services/read | De beschikbare services ophalen. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Hiermee valideert u de migratie van een storage-account. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Hiermee verwijdert u de installatiekopie van een bepaalde virtuele machine. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Hiermee haalt u de bewerkingsstatus van een bepaalde virtuele machine-installatiekopie. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Retourneert de installatiekopie van de virtuele machine. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Wordt toegevoegd een installatiekopie van een bepaalde virtuele machine. |
> | Bewerking | Microsoft.ClassicStorage/storageAccounts/write | Voegt een nieuw opslagaccount. |
> | Bewerking | Microsoft.ClassicStorage/vmImages/read | Een lijst met installatiekopieën van virtuele machines. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.CognitiveServices/accounts/delete | API-accounts verwijderen |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/detect/action | Detecteer menselijke gezichten in een afbeelding, return gezichtsrechthoeken, en eventueel met faceIds, monumenten en kenmerken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/delete | Een lijst met opgegeven gezichten verwijderen. De face-gerelateerde afbeeldingen in de lijst met face wordt verwijderd, te. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/delete | Een gezicht uit een lijst met face verwijderen door de opgegeven faceListId en persisitedFaceId. De gerelateerde gezicht wordt verwijderd, te. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/persistedfaces/write | Een gezicht toevoegen aan een lijst opgegeven face maximaal 1000 gezichten. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/read | Ophalen van de lijst van een gezicht faceListId, naam, userData en gezichten in de lijst met face. Face-lijsten faceListId, naam en userData lijst. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/facelists/write | Een leeg vlak-lijst maken met de gebruiker opgegeven faceListId, naam en een optionele userData. Een lijst met mogen maximaal 64 gezichten updategegevens van een lijst gezicht, met inbegrip van de naam en userData. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/findsimilars/action | Face id van de zoekgezicht, om te zoeken naar de gelijkende gezichten uit een face id-matrix, een face-lijst of een grote face gegeven. faceId |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/group/action | Kandidaat gezichten in groepen op basis van gelijkenis face verdelen. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/identify/action | 1-op-veel-identificatie naar de dichtstbijzijnde overeenkomen met de specifieke persoon zoekgezicht van een Persoonsgroep of een grote Persoonsgroep vinden. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/delete | Een lijst met opgegeven grote gezichten verwijderen. De face-gerelateerde afbeeldingen in de lijst met grote face wordt verwijderd, te. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/delete | Een gezicht uit een lijst met grote face verwijderen door de opgegeven largeFaceListId en persisitedFaceId. De gerelateerde gezicht wordt verwijderd, te. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/read | Persistente gezichten in grote face lijst met largeFaceListId en persistedFaceId ophalen. Overzicht van de gezichten persistedFaceId en userData in een opgegeven grote face-lijst. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/persistedfaces/write | Een gezicht toevoegen aan een opgegeven grote face-lijst, tot 1.000.000 gezichten. Een opgegeven gezicht userData veld in een lijst met grote face bijwerken door de persistedFaceId. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/read | Ophalen van de lijst van een grote face largeFaceListId, naam, userData. Gegevens van de grote face-lijsten van largeFaceListId, namen en userData weer te geven. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/train/action | Een grote face lijst training taak indienen. Training is een belangrijke stap die alleen een lijst getrainde grote face kunt gebruiken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/training/read | Controleer de status grote face lijst training voltooid of nog steeds lopende. LargeFaceList Training is een asynchrone bewerking |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largefacelists/write | Maak een lege grote face-lijst met de gebruiker opgegeven largeFaceListId, naam en een optionele userData. Gegevens van een grote face-lijst, met inbegrip van de naam en userData bijwerken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/delete | Een bestaande Persoonsgroep grote met opgegeven personGroupId verwijderen. Persistente gegevens in deze Persoonsgroep grote worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/action | Maak een nieuwe persoon in een opgegeven grote Persoonsgroep. Face toevoegen aan deze persoon, neem contact op met |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/delete | Een bestaande persoon verwijderen uit een grote Persoonsgroep. Alle persoonsgegevens die zijn opgeslagen en face-installatiekopieën in de persoon die vermelding worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/delete | Een gezicht verwijderen van een persoon in een grote Persoonsgroep. Face-gegevens- en afbeeldingsbestanden die betrekking hebben op deze vermelding face worden, eveneens verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/read | Persoon face gegevens worden opgehaald. Het gezicht persistente persoon is opgegeven door de largePersonGroupId, personId en persistedFaceId. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/persistedfaces/write | Een afbeelding voor face toevoegen aan een persoon in een grote Persoonsgroep voor gezichts-id of verifiëren. Als u wilt omgaan met de installatiekopie van de Update van een persoon die permanent van face userData veld. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/read | Haal de naam van een persoon en userData en de persistente faceIds voor het gezicht geregistreerde persoon. Gegevens van alle personen in de Persoonsgroep opgegeven grote, met inbegrip van personId, naam, userData en persistedFaceIds weer te geven. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/persons/write | Werk de naam of userData van een persoon. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/read | De gegevens van een die grote Persoonsgroep, met inbegrip van de naam en het userData ophalen. Deze API retourneert grote persoon groepsinformatie lijst met alle bestaande largePesonGroupId, naam en userData van de die grote personengroepen. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/train/action | Een persoon die grote groep training taak indienen. Training is een belangrijke stap die alleen een persoon met grote groep kunt gebruiken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/training/read | Grote persoon groep trainingsstatus voltooid of nog steeds voortdurend controleren. LargePersonGroup Training is een asynchrone bewerking |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/largepersongroups/write | Maak een nieuwe die grote Persoonsgroep met de gebruiker opgegeven largePersonGroupId, naam en optionele userData. De naam van een bestaande grote Persoonsgroep en de userData bijwerken. De eigenschappen blijven ongewijzigd als ze zich niet in de hoofdtekst van de aanvraag. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/delete | Een bestaande groep personen met de opgegeven personGroupId verwijderen. Persistente gegevens in deze Persoonsgroep worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/action | Maak een nieuwe persoon in een bepaalde persoon-groep. Face toevoegen aan deze persoon, neem contact op met |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/delete | Een bestaande persoon verwijderen uit de Persoonsgroep van een. Alle persoonsgegevens die zijn opgeslagen en face-installatiekopieën in de persoon die vermelding worden verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/delete | Een gezicht verwijderen van een persoon in de Persoonsgroep van een. Face-gegevens- en afbeeldingsbestanden die betrekking hebben op deze vermelding face worden, eveneens verwijderd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/read | Persoon face gegevens worden opgehaald. Het gezicht persistente persoon is opgegeven door de personGroupId, personId en persistedFaceId. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/persistedfaces/write | Een afbeelding voor face toevoegen aan een persoon in de Persoonsgroep van een voor gezichts-id of verifiëren. Om u te bekommeren om de installatiekopie van meerdere Update een persoon die permanent van face userData veld. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/read | Haal de naam van een persoon en userData en de persistente faceIds voor het gezicht geregistreerde persoon. Lijst van alle personen informatie in de Persoonsgroep opgegeven, met inbegrip van personId, naam, userData en persistedFaceIds van geregistreerd. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/persons/write | Werk de naam of userData van een persoon. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/read | Naam van de groep persoon en userData ophalen. Voor informatie onder deze personGroup persoon, gebruikt u de pesonGroupId, de naam en de userData lijst personengroepen. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/train/action | Een persoon groep training taak indienen. Training is een belangrijke stap die alleen de groep van een persoon kunt gebruiken. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/training/read | Persoon groep trainingsstatus voltooid of nog steeds voortdurend controleren. PersonGroup Training is een asynchrone bewerking geactiveerd |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/persongroups/write | Maak een nieuwe Persoonsgroep met opgegeven personGroupId, de naam en de gebruiker opgegeven userData. De naam van een bestaande Persoonsgroep en de userData bijwerken. De eigenschappen blijven ongewijzigd als ze zich niet in de hoofdtekst van de aanvraag. |
> | DataAction | Microsoft.CognitiveServices/accounts/Face/verify/action | Controleer of twee gezichten horen bij een dezelfde persoon of een gezicht is of een persoon. |
> | Bewerking | Microsoft.CognitiveServices/accounts/listKeys/action | Een lijst met sleutels |
> | Bewerking | Microsoft.CognitiveServices/accounts/read | API-accounts worden gelezen. |
> | Bewerking | Microsoft.CognitiveServices/accounts/regenerateKey/action | Sleutel opnieuw genereren |
> | Bewerking | Microsoft.CognitiveServices/accounts/skus/read | Hiermee worden beschikbare SKU's voor een bestaande resource gelezen. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/entities/action | De API retourneert een lijst met bekende entiteiten en algemene entiteiten met de naam (\"persoon\", \"locatie\", \"organisatie\" enzovoort) in een bepaald document. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/keyphrases/action | De API retourneert een lijst met tekenreeksen die de belangrijkste gespreksonderwerpen in de invoertekst aanduiden. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/languages/action | De API retourneert de gedetecteerde taal en een numerieke score tussen 0 en 1. Scores dichtbij 1 geven aan dat het 100% zeker is dat de geïdentificeerde taal waar is. Er worden in totaal 120 talen ondersteund. |
> | DataAction | Microsoft.CognitiveServices/accounts/TextAnalytics/sentiment/action | De API retourneert een numerieke score tussen 0 en 1.<br>Scores dichtbij 1 geven een positieve stemming, terwijl scores dicht bij 0 geven een negatieve stemming.<br>Een score van 0,5 (bijvoorbeeld) geeft aan dat het ontbreken van sentiment<br>een instructie factoid). |
> | Bewerking | Microsoft.CognitiveServices/accounts/usages/read | Het quotumgebruik voor een bestaande resource ophalen. |
> | Bewerking | Microsoft.CognitiveServices/accounts/write | API-Accounts schrijft. |
> | Bewerking | Microsoft.CognitiveServices/checkDomainAvailability/action | Leest beschikbare SKU's voor een abonnement. |
> | Bewerking | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Leest beschikbare SKU's voor een abonnement. |
> | Bewerking | Microsoft.CognitiveServices/locations/deleteVirtualNetworkOrSubnets/action | Melding van Microsoft.Network VirtualNetworks of subnetten te verwijderen. |
> | Bewerking | Microsoft.CognitiveServices/Operations/read | Alle beschikbare bewerkingen vermelden |
> | Bewerking | Microsoft.CognitiveServices/register/action | Hiermee wordt een abonnement voor Cognitive Services |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Commerce/RateCard/read | Retourneert bieden gegevens, metagegevens van de resource/meter en tarieven voor het betreffende abonnement. |
> | Bewerking | Microsoft.Commerce/UsageAggregates/read | Hiermee haalt u van Microsoft Azure-verbruik door een abonnement. Het resultaat bevat statistische gegevens over gebruik, abonnement en de resourcegroep verwante gegevens, op een bepaalde periode. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Compute/availabilitySets/delete | Hiermee verwijdert u de beschikbaarheidsset |
> | Bewerking | Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset |
> | Bewerking | Microsoft.Compute/availabilitySets/vmSizes/read | Lijst met beschikbare grootten voor het maken of bijwerken van een virtuele machine in de beschikbaarheidsset |
> | Bewerking | Microsoft.Compute/availabilitySets/write | Hiermee maakt u een nieuwe beschikbaarheidsset of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/disks/beginGetAccess/action | De SAS-URI van de schijf voor toegang tot de blob ophalen |
> | Bewerking | Microsoft.Compute/disks/delete | Hiermee verwijdert u de schijf |
> | Bewerking | Microsoft.Compute/disks/endGetAccess/action | De SAS-URI van de schijf intrekken |
> | Bewerking | Microsoft.Compute/disks/read | Hiermee worden de eigenschappen van een schijf |
> | Bewerking | Microsoft.Compute/disks/write | Hiermee maakt u een nieuwe schijf of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/galleries/delete | Hiermee verwijdert u de galerie |
> | Bewerking | Microsoft.Compute/galleries/images/delete | Hiermee verwijdert u de Image z galerie |
> | Bewerking | Microsoft.Compute/galleries/images/read | Haalt u de eigenschappen van afbeelding |
> | Bewerking | Microsoft.Compute/galleries/images/versions/delete | Hiermee verwijdert u de versie van de galerie-installatiekopie |
> | Bewerking | Microsoft.Compute/galleries/images/versions/read | Haalt u de eigenschappen van de galerie met de versie van installatiekopie |
> | Bewerking | Microsoft.Compute/galleries/images/versions/write | Hiermee maakt u een nieuwe versie van het galerie-installatiekopie of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/galleries/images/write | Hiermee maakt u een nieuwe afbeelding of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/galleries/read | Haalt u de eigenschappen van de galerie |
> | Bewerking | Microsoft.Compute/galleries/write | Hiermee maakt u een nieuwe galerie of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/hostGroups/delete | Hiermee verwijdert u de hostgroep |
> | Bewerking | Microsoft.Compute/hostGroups/read | Hiermee worden de eigenschappen van een hostgroep |
> | Bewerking | Microsoft.Compute/hostGroups/write | Hiermee maakt u een nieuwe hostgroep of updates van een bestaande hostgroep |
> | Bewerking | Microsoft.Compute/hosts/delete | Hiermee verwijdert u de host |
> | Bewerking | Microsoft.Compute/hosts/read | Hiermee worden de eigenschappen van een host |
> | Bewerking | Microsoft.Compute/hosts/write | Hiermee maakt u een nieuwe host of een bestaande host-updates |
> | Bewerking | Microsoft.Compute/images/delete | Hiermee verwijdert u de installatiekopie |
> | Bewerking | Microsoft.Compute/images/read | Hiermee worden de eigenschappen van de installatiekopie |
> | Bewerking | Microsoft.Compute/images/write | Maakt een nieuwe installatiekopie of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/locations/capsOperations/read | Hiermee wordt de status van een asynchrone Caps-bewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/diskOperations/read | Hiermee wordt de status van een asynchrone schijfbewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Logboeken om weer te geven van totaal aantal aanvragen door een bepaalde periode om u te helpen bandbreedtebeperking diagnostische gegevens maken. |
> | Bewerking | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Logboeken om weer te geven statistische functies van beperkte verzoeken, gegroepeerd per ResourceName, OperationName of het toegepaste beleid voor het beperken van maken. |
> | Bewerking | Microsoft.Compute/locations/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | De eigenschappen van een Platforminstallatiekopieaanbieding ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | De eigenschappen van een Platforminstallatiekopie-Sku ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | De eigenschappen van een Platforminstallatiekopieversie ophalen |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Hiermee worden de eigenschappen van een VMExtension-Type |
> | Bewerking | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Hiermee worden de eigenschappen van een VMExtension-versie |
> | Bewerking | Microsoft.Compute/locations/publishers/read | De eigenschappen van een uitgever ophalen |
> | Bewerking | Microsoft.Compute/locations/runCommands/read | Geeft een lijst van beschikbare opdrachten op de locatie |
> | Bewerking | Microsoft.Compute/locations/usages/read | Servicelimieten en huidige gebruikshoeveelheden opgehaald voor de rekenresources van het abonnement op een locatie |
> | Bewerking | Microsoft.Compute/locations/vmSizes/read | Geeft een lijst van beschikbare VM-groottes op een locatie |
> | Bewerking | Microsoft.Compute/operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Compute-resourceprovider |
> | Bewerking | Microsoft.Compute/proximityPlacementGroups/delete | Hiermee verwijdert u de Plaatsingsgroep nabijheid |
> | Bewerking | Microsoft.Compute/proximityPlacementGroups/read | Hiermee worden de eigenschappen van een Plaatsingsgroep nabijheid |
> | Bewerking | Microsoft.Compute/proximityPlacementGroups/write | Maakt een nieuwe nabijheid plaatsing groep of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/register/action | Hiermee wordt een abonnement bij de Microsoft.Compute-resourceprovider |
> | Bewerking | Microsoft.Compute/restorePointCollections/delete | Verwijdert de herstelpuntverzameling en opgenomen herstelpunten |
> | Bewerking | Microsoft.Compute/restorePointCollections/read | De eigenschappen van een herstelpuntverzameling ophalen |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/delete | Hiermee verwijdert u het herstelpunt |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/read | Hiermee worden de eigenschappen van een herstelpunt |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Hiermee worden de eigenschappen van een herstelpunt en blob SAS URI 's |
> | Bewerking | Microsoft.Compute/restorePointCollections/restorePoints/write | Een nieuw herstelpunt wordt gemaakt |
> | Bewerking | Microsoft.Compute/restorePointCollections/write | Een nieuwe herstelpuntverzameling gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/sharedVMImages/delete | Hiermee verwijdert u de sharedvmimage verwijderd |
> | Bewerking | Microsoft.Compute/sharedVMImages/read | De eigenschappen van een SharedVMImage ophalen |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/delete | Een SharedVMImageVersion verwijderen |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/read | De eigenschappen van een SharedVMImageVersion ophalen |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/replicate/action | Een SharedVMImageVersion repliceren naar de doelregio 's |
> | Bewerking | Microsoft.Compute/sharedVMImages/versions/write | Een nieuwe SharedVMImageVersion maken of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/sharedVMImages/write | Hiermee maakt u een nieuwe sharedvmimage gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/skus/read | Hiermee haalt u de lijst van de Microsoft.Compute-SKU's beschikbaar voor uw abonnement |
> | Bewerking | Microsoft.Compute/snapshots/beginGetAccess/action | De SAS-URI van de momentopname voor toegang tot de blob ophalen |
> | Bewerking | Microsoft.Compute/snapshots/delete | Een momentopname verwijderen |
> | Bewerking | Microsoft.Compute/snapshots/endGetAccess/action | De SAS-URI van de momentopname intrekken |
> | Bewerking | Microsoft.Compute/snapshots/read | Hiermee worden de eigenschappen van een momentopname |
> | Bewerking | Microsoft.Compute/snapshots/write | Een nieuwe momentopname gemaakt of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/unregister/action | De registratie van abonnement bij de Microsoft.Compute-resourceprovider |
> | Bewerking | Microsoft.Compute/virtualMachines/capture/action | De virtuele machine vastgelegd door het kopiëren van virtuele harde schijven en genereert een sjabloon die kan worden gebruikt om soortgelijke virtuele machines te maken |
> | Bewerking | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | De op blobs gebaseerde schijven van de virtuele machine converteert naar beheerde schijven |
> | Bewerking | Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine en worden de rekenresources vrijgegeven uitgeschakeld |
> | Bewerking | Microsoft.Compute/virtualMachines/delete | Hiermee verwijdert u de virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/delete | Hiermee verwijdert u de extensie van de virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/read | Hiermee worden de eigenschappen van een VM-extensie |
> | Bewerking | Microsoft.Compute/virtualMachines/extensions/write | Hiermee maakt u een nieuwe virtuele machine-extensie of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachines/generalize/action | Status van de virtuele machine ingesteld op gegeneraliseerd en wordt de virtuele machine voorbereid voor vastleggen |
> | Bewerking | Microsoft.Compute/virtualMachines/instanceView/read | De gedetailleerde runtimestatus van de virtuele machine en de bijbehorende resources opgehaald |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Meld u aan bij een virtuele machine met Windows-beheerders- of Linux rootgebruikersbevoegdheden |
> | Bewerking | Microsoft.Compute/virtualMachines/performMaintenance/action | Onderhoudsbewerking op de virtuele machine wordt uitgevoerd. |
> | Bewerking | Microsoft.Compute/virtualMachines/powerOff/action | Hiermee wordt de virtuele machine uitgeschakeld. Houd er rekening mee dat de virtuele machine blijven, worden kosten in rekening gebracht. |
> | Bewerking | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/redeploy/action | Hiermee wordt opnieuw geïmplementeerd op virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/reimage/action | Reimages virtuele machine die een differentiërende schijf. |
> | Bewerking | Microsoft.Compute/virtualMachines/restart/action | De virtuele machine opnieuw wordt opgestart |
> | Bewerking | Microsoft.Compute/virtualMachines/runCommand/action | Een vooraf gedefinieerd script uitgevoerd op de virtuele machine |
> | Bewerking | Microsoft.Compute/virtualMachines/start/action | De virtuele machine gestart |
> | Bewerking | Microsoft.Compute/virtualMachines/vmSizes/read | Een lijst met beschikbare grootten die de virtuele machine kan worden bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachines/write | Hiermee maakt u een nieuwe virtuele machine of een bestaande virtuele machine-updates |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Hiermee uitgeschakeld en vrijgegeven de rekenresources voor de exemplaren van de virtuele-Machineschaalset opgehaald  |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/delete | Hiermee verwijdert u de virtuele-Machineschaalset |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/delete/action | Hiermee verwijdert u de instanties van de virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Verwijdert de virtuele-Machineschaalset extensie instellen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Haalt u de eigenschappen van een virtuele Machine-extensie voor het instellen van schaal |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Hiermee maakt u een nieuwe virtuele Machine schaal extensie of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | De platformupdatedomeinen van een virtuele-Machineschaalset opgehaald om te voltooien van een update die is vastgelopen in behandeling voor service fabric doorlopen handmatig |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Hiermee wordt de instantieweergave van de virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Instanties handmatig bijgewerkt naar het laatste model van de virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Eigenschappen van alle netwerkinterfaces van een virtuele-Machineschaalset ophalen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | Hiermee start u een rolling upgrade voor alle exemplaren van de virtuele-Machineschaalset verplaatsen naar de meest recente beschikbare Platform Image versie van het besturingssysteem. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Hiermee haalt u de geschiedenis van upgrades voor het besturingssysteem voor een virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Gepland onderhoud uitgevoerd op de exemplaren van de virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Hiermee wordt de exemplaren van de virtuele-Machineschaalset uitgeschakeld |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Eigenschappen van alle openbare IP-adressen van een virtuele-Machineschaalset ophalen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/read | Hiermee worden de eigenschappen van een virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | De exemplaren van de virtuele-Machineschaalset opnieuw implementeren |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/reimage/action | De exemplaren van de virtuele-Machineschaalset werkrolgrootte |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Alle schijven (Besturingssysteemschijf en gegevensschijven) werkrolgrootte voor de exemplaren van een virtuele-Machineschaalset opgehaald  |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/restart/action | De exemplaren van de virtuele-Machineschaalset opnieuw wordt opgestart |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Annuleert de rolling upgrade van een virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Meest recente status Rolling Upgrade voor een virtuele-Machineschaalset ophalen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/scale/action | Controleer of als een bestaande virtuele-Machineschaalset kunt schalen- / uitschalen naar een aantal instanties opgegeven |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/skus/read | Geeft een lijst van de geldige SKU's voor een bestaande virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/start/action | Start de exemplaren van de virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Hiermee worden uitgeschakeld en worden de rekenresources voor een virtuele Machine in een VM-Schaalset vrijgegeven. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Verwijderen van een specifieke virtuele Machine in een VM-Schaalset. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Hiermee wordt de instantieweergave van een virtuele Machine in een VM-Schaalset opgehaald. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Hiermee worden eigenschappen van openbare IP-adres gemaakt met behulp van virtuele-Machineschaalset opgehaald. Virtuele-Machineschaalset kunt maken van maximaal één openbare IP per IP-configuratie (privé-IP) |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Hiermee worden eigenschappen van één of alle IP-configuraties van een netwerkinterface die is gemaakt met behulp van virtuele-Machineschaalset opgehaald. IP-configuraties staan voor privé-IP 's |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Eigenschappen van een of alle netwerkinterfaces van een virtuele machine gemaakt met behulp van virtuele-Machineschaalset ophalen |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Gepland onderhoud uitgevoerd op de instantie van een virtuele Machine in een virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Het exemplaar van de bevoegdheden uit een virtuele Machine in een VM-Schaalset. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | De eigenschappen van een virtuele Machine in een VM-Schaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Hiermee wordt opnieuw geïmplementeerd een instantie van de virtuele Machine in een virtuele-Machineschaalset opgehaald |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | De instantie van een virtuele Machine in een virtuele-Machineschaalset werkrolgrootte. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Alle schijven (Besturingssysteemschijf en gegevensschijven) voor VM-instantie in een virtuele-Machineschaalset werkrolgrootte. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Start een exemplaar van de virtuele Machine in een VM-Schaalset. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Een vooraf gedefinieerde script wordt uitgevoerd op de instantie van een virtuele Machine in een virtuele-Machineschaalset opgehaald. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Start een exemplaar van de virtuele Machine in een VM-Schaalset. |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Hiermee worden de eigenschappen van een virtuele Machine in een VM-Schaalset bijgewerkt |
> | Bewerking | Microsoft.Compute/virtualMachineScaleSets/write | Hiermee maakt u een nieuwe virtuele-Machineschaalset opgehaald of een bestaande bijgewerkt |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Consumption/balances/read | Lijst met het gebruik van de samenvatting voor een factureringsperiode van een beheergroep. |
> | Bewerking | Microsoft.Consumption/budgets/delete | Verwijder de budgetten door een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/budgets/read | Lijst met de budgetten door een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/budgets/write | Maakt en bijwerken van de budgetten door een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/charges/read | Lijst met kosten in rekening gebracht |
> | Bewerking | Microsoft.Consumption/credits/read | Lijst met tegoed |
> | Bewerking | Microsoft.Consumption/events/read | Lijst met gebeurtenissen |
> | Bewerking | Microsoft.Consumption/forecasts/read | Lijst met voorspellingen |
> | Bewerking | Microsoft.Consumption/lots/read | Lijst met veel |
> | Bewerking | Microsoft.Consumption/marketplaces/read | Geef de details van marketplace resource gebruik voor een bereik voor EA en WebDirect abonnementen. |
> | Bewerking | Microsoft.Consumption/operationresults/read | Lijst met operationresults |
> | Bewerking | Microsoft.Consumption/operations/read | Lijst met alle ondersteunde bewerkingen door Microsoft.Consumption resourceprovider. |
> | Bewerking | Microsoft.Consumption/operationstatus/read | Lijst met operationstatus |
> | Bewerking | Microsoft.Consumption/pricesheets/read | Een lijst met Pricesheets gegevens voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/register/action | Registreren bij verbruik RP |
> | Bewerking | Microsoft.Consumption/reservationDetails/read | Geef de details van het gebruik van gereserveerde instanties van de reservering volgorde of beheer van groepen. De details-gegevens zijn per exemplaar per dag niveau. |
> | Bewerking | Microsoft.Consumption/reservationRecommendations/read | Lijst met aanbevelingen enkele of gedeelde voor gereserveerde instanties voor een abonnement. |
> | Bewerking | Microsoft.Consumption/reservationSummaries/read | Lijst met het gebruik van de samenvatting voor gereserveerde instanties van de reservering volgorde of beheer van groepen. De gegevens van de samenvatting is op het niveau van maandelijkse of dagelijks. |
> | Bewerking | Microsoft.Consumption/reservationTransactions/read | Lijst met de transactiegeschiedenis voor gereserveerde instanties van beheergroepen. |
> | Bewerking | Microsoft.Consumption/tags/read | Lijst met tags voor EA- en -abonnementen. |
> | Bewerking | Microsoft.Consumption/tenants/read | Lijst met tenants |
> | Bewerking | Microsoft.Consumption/tenants/register/action | Actie voor een bereik van Microsoft.Consumption door een tenant te registreren. |
> | Bewerking | Microsoft.Consumption/terms/read | Lijst met de voorwaarden voor een abonnement of een beheergroep. |
> | Bewerking | Microsoft.Consumption/usageDetails/read | Lijst met de informatie over het gebruik voor een bereik voor EA en WebDirect-abonnementen. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Exec naar een specifieke container. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Logboeken voor een specifieke container ophalen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/delete | De specifieke containergroep verwijderen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de containergroep opgehaald. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee of de diagnostische instelling voor de containergroep wordt bijgewerkt. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de containergroep. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/read | Alle containergroepen ophalen. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/restart/action | Een specifieke containergroep opnieuw is opgestart. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/start/action | Start een specifieke containergroep. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/stop/action | Hiermee stopt u een specifieke containergroep. COMPUTE-resources wordt ongedaan worden gemaakt en facturering wordt gestopt. |
> | Bewerking | Microsoft.ContainerInstance/containerGroups/write | Maken of bijwerken van een specifieke containergroep. |
> | Bewerking | Microsoft.ContainerInstance/locations/cachedImages/read | Hiermee haalt de installatiekopieën in de cache voor het abonnement in een regio. |
> | Bewerking | Microsoft.ContainerInstance/locations/capabilities/read | Haal de mogelijkheden voor een regio. |
> | Bewerking | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.ContainerInstance meldt dat virtuele netwerk of subnet wordt verwijderd. |
> | Bewerking | Microsoft.ContainerInstance/locations/usages/read | Het gebruik voor een specifieke regio ophalen. |
> | Bewerking | Microsoft.ContainerInstance/register/action | Het abonnement voor de container instance-resourceprovider geregistreerd en wordt het maken van groepen met containers. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerRegistry/checkNameAvailability/read | Controleert of de naam van het containerregister beschikbaar voor gebruik is. |
> | Bewerking | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.ContainerRegistry meldt dat virtuele netwerk of subnet wordt verwijderd |
> | Bewerking | Microsoft.ContainerRegistry/locations/operationResults/read | De resultaten van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/operations/read | Een lijst met alle van de beschikbare bewerkingen voor de REST-API van Azure Container Registry |
> | Bewerking | Microsoft.ContainerRegistry/register/action | Hiermee wordt het abonnement voor de container registry-resourceprovider geregistreerd en wordt het maken van de container Registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/artifacts/delete | Verwijder artefact in een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/builds/cancel/action | Een bestaande build, wordt geannuleerd. |
> | Bewerking | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Hiermee haalt u een koppeling om de build-logboeken te downloaden. |
> | Bewerking | Microsoft.ContainerRegistry/registries/builds/read | De eigenschappen van de opgegeven build opgehaald of een lijst met alle builds voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/builds/write | Werkt een build voor een container registry met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/delete | Hiermee verwijdert u een taak maken vanuit een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Hier worden de eigenschappen van het bron-besturingselement voor een build-taak. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/read | De eigenschappen van de opgegeven build-taak opgehaald of een lijst met de build-taken voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Hiermee verwijdert u een build-stap van een build-taak. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Geeft een lijst van de build-argumenten voor een build-stap met inbegrip van de geheime argumenten. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | De eigenschappen van de opgegeven build-stap opgehaald of een lijst met de build-stappen voor de opgegeven build-taak. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Hiermee of een build-stap voor een build-taak bijwerken met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/buildTasks/write | Hiermee of een taak maken voor een container registry bijgewerkt met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/delete | Hiermee verwijdert u een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Hiermee verwijdert een event grid-filter uit een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Haalt u de eigenschappen van het opgegeven event grid-filter of een lijst met alle filters voor event grid voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Hiermee of bijwerken van een event grid-filter voor een container registry met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Hiermee haalt u de uploadlocatie voor de gebruiker om te kunnen uploaden van de bron. |
> | Bewerking | Microsoft.ContainerRegistry/registries/importImage/action | Importeren van de installatiekopie naar container registry met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | Bronbestand url-locatie voor een container registry uploaden. |
> | Bewerking | Microsoft.ContainerRegistry/registries/listCredentials/action | Een lijst met de aanmeldingsreferenties voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/listPolicies/read | Geeft een lijst van de beleidsregels voor de opgegeven container registry |
> | Bewerking | Microsoft.ContainerRegistry/registries/listUsages/read | Geeft een lijst van het gebruik van het quotum voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/metadata/read | Hiermee haalt u de metagegevens van een specifieke opslagplaats voor een container registry |
> | Bewerking | Microsoft.ContainerRegistry/registries/metadata/write | De metagegevens van de opslagplaats voor een container registry-updates |
> | Bewerking | Microsoft.ContainerRegistry/registries/operationStatuses/read | Hiermee wordt een status register asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/pull/read | Pull- of installatiekopieën ophalen uit een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/push/write | Push- of installatiekopieën schrijven naar een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/quarantineRead/read | Pull- of in quarantaine geplaatste afbeeldingen kunt verkrijgen van containerregister |
> | Bewerking | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Status van de in quarantaine plaatsen van in quarantaine geplaatste afbeeldingen schrijven/wijzigen |
> | Bewerking | Microsoft.ContainerRegistry/registries/queueBuild/action | Hiermee maakt u een nieuwe build op basis van de aanvraagparameters en toe te voegen aan de build-wachtrij. |
> | Bewerking | Microsoft.ContainerRegistry/registries/read | De eigenschappen van de opgegeven container registry opgehaald of een lijst met alle containerregisters onder de opgegeven resourcegroep of abonnement. |
> | Bewerking | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Wordt opnieuw gegenereerd op een van de aanmeldingsreferenties voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/delete | Hiermee verwijdert u een replicatie van een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Hiermee wordt een status van replicatie asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/read | De eigenschappen van de opgegeven replicatie opgehaald of een lijst met alle replicaties voor de opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/replications/write | Hiermee of een replicatie voor een container registry bijgewerkt met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/runs/cancel/action | Een bestaande uitvoering annuleren. |
> | Bewerking | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Hiermee haalt u de SAS-URL van het logboek voor het uitvoeren van. |
> | Bewerking | Microsoft.ContainerRegistry/registries/runs/read | Hiermee haalt u de eigenschappen van een uitvoering op basis van een container registry of lijst met uitvoeringen. |
> | Bewerking | Microsoft.ContainerRegistry/registries/runs/write | Een uitvoeren-updates. |
> | Bewerking | Microsoft.ContainerRegistry/registries/scheduleRun/action | Een uitvoering op basis van een containerregister plannen. |
> | Bewerking | Microsoft.ContainerRegistry/registries/sign/write | Push of eruit te halen de metagegevens van inhoud vertrouwensrelatie voor een container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/tasks/delete | Hiermee verwijdert u een taak voor een container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Lijst met alle details van een taak voor een container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/tasks/read | Een taak voor een container registry of de lijst met alle taken opgehaald. |
> | Bewerking | Microsoft.ContainerRegistry/registries/tasks/write | Hiermee of een taak voor een container registry bijgewerkt. |
> | Bewerking | Microsoft.ContainerRegistry/registries/updatePolicies/write | Updates van de beleidsregels voor de opgegeven container registry |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/delete | Hiermee verwijdert een webhook uit een containerregister. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Hiermee haalt de configuratie van service-URI en aangepaste headers voor de webhook. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Een lijst met recente gebeurtenissen voor de opgegeven webhook. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | De status van een webhook asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Een ping-gebeurtenis die moet worden verzonden naar de webhook activeert. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/read | De eigenschappen van de opgegeven webhook opgehaald of een lijst met alle webhooks voor het opgegeven container registry. |
> | Bewerking | Microsoft.ContainerRegistry/registries/webhooks/write | Hiermee of een webhook voor een container registry bijgewerkt met de opgegeven parameters. |
> | Bewerking | Microsoft.ContainerRegistry/registries/write | Hiermee of een containerregister bijgewerkt met de opgegeven parameters. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContainerService/containerServices/delete | Hiermee verwijdert u een containerservice |
> | Bewerking | Microsoft.ContainerService/containerServices/read | Ophalen van een containerservice |
> | Bewerking | Microsoft.ContainerService/containerServices/write | Hiermee maakt u een nieuwe containerservice of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/locations/operationresults/read | Hiermee wordt de status van het resultaat van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerService/locations/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.ContainerService/locations/orchestrators/read | Geeft een lijst van de ondersteunde orchestrators |
> | Bewerking | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Een profiel van de toegang tot beheerde cluster met op rolnaam met behulp van de lijst met referenties ophalen |
> | Bewerking | Microsoft.ContainerService/managedClusters/accessProfiles/read | Ophalen van een profiel van de toegang tot beheerde cluster met op rolnaam |
> | Bewerking | Microsoft.ContainerService/managedClusters/agentPools/delete | Hiermee verwijdert u een groep van agent |
> | Bewerking | Microsoft.ContainerService/managedClusters/agentPools/read | Haalt een agentpool |
> | Bewerking | Microsoft.ContainerService/managedClusters/agentPools/write | Hiermee maakt u een nieuwe agentpool of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/managedClusters/delete | Hiermee verwijdert u een beheerd cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Lijst van de referentie clusterAdmin van een beheerde cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Lijst van de referentie clusterUser van een beheerde cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/read | Een beheerd cluster ophalen |
> | Bewerking | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Opnieuw instellen van de AAD-profiel van een beheerd cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Opnieuw instellen van de service-principal-profiel van een beheerde cluster |
> | Bewerking | Microsoft.ContainerService/managedClusters/upgradeprofiles/read | Het upgrade-profiel van het cluster opgehaald |
> | Bewerking | Microsoft.ContainerService/managedClusters/write | Hiermee maakt u een nieuwe beheerde cluster of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/openShiftClusters/delete | Een Open Shift-Cluster verwijderen |
> | Bewerking | Microsoft.ContainerService/openShiftClusters/read | Ophalen van een Open Shift-Cluster |
> | Bewerking | Microsoft.ContainerService/openShiftClusters/write | Hiermee maakt u een nieuw Open Shift-Cluster of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/openShiftManagedClusters/delete | Een beheerde Open Shift-Cluster verwijderen |
> | Bewerking | Microsoft.ContainerService/openShiftManagedClusters/read | Een beheerde Cluster met Open Shift ophalen |
> | Bewerking | Microsoft.ContainerService/openShiftManagedClusters/write | Hiermee maakt u een nieuw Open Shift beheerd Cluster of een bestaande bijgewerkt |
> | Bewerking | Microsoft.ContainerService/operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.ContainerService-resourceprovider |
> | Bewerking | Microsoft.ContainerService/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.ContainerService |
> | Bewerking | Microsoft.ContainerService/unregister/action | De registratie van abonnement bij de resourceprovider Microsoft.ContainerService |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ContentModerator/applications/delete | Verwijderbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/listSecrets/action | Geheimen vermelden |
> | Bewerking | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Eenmalige aanmelding Tokens lezen |
> | Bewerking | Microsoft.ContentModerator/applications/read | Leesbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Bewerking | Microsoft.ContentModerator/applications/write | Schrijfbewerking |
> | Bewerking | Microsoft.ContentModerator/listCommunicationPreference/action | Communicatievoorkeur vermelden |
> | Bewerking | Microsoft.ContentModerator/operations/read | leesbewerkingen |
> | Bewerking | Microsoft.ContentModerator/updateCommunicationPreference/action | Communicatievoorkeur bijwerken |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.CostManagement/cloudConnectors/delete | De opgegeven cloudConnector verwijderen. |
> | Bewerking | Microsoft.CostManagement/cloudConnectors/read | Lijst met de cloudConnectors voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.CostManagement/cloudConnectors/write | Maken of bijwerken van de opgegeven cloudConnector. |
> | Bewerking | Microsoft.CostManagement/dimensions/read | Lijst met alle ondersteunde dimensies door een bereik. |
> | Bewerking | Microsoft.CostManagement/exports/action | De opgegeven uitvoer worden uitgevoerd. |
> | Bewerking | Microsoft.CostManagement/exports/delete | De opgegeven export verwijderen. |
> | Bewerking | Microsoft.CostManagement/exports/read | Een lijst van de uitvoer door een bereik. |
> | Bewerking | Microsoft.CostManagement/exports/run/action | Uitvoer worden uitgevoerd. |
> | Bewerking | Microsoft.CostManagement/exports/write | Maken of bijwerken van de opgegeven uitvoer. |
> | Bewerking | Microsoft.CostManagement/externalBillingAccounts/externalSubscriptions/read | Lijst met de externalSubscriptions binnen een externalBillingAccount voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.CostManagement/externalBillingAccounts/read | Lijst met de externalBillingAccounts voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.CostManagement/externalSubscriptions/read | Lijst met de externalSubscriptions voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.CostManagement/externalSubscriptions/write | Verbonden beheergroep van externalSubscription bijwerken |
> | Bewerking | Microsoft.CostManagement/query/action | Gebruiksgegevens door een bereik op te vragen. |
> | Bewerking | Microsoft.CostManagement/query/read | Gebruiksgegevens door een bereik op te vragen. |
> | Bewerking | Microsoft.CostManagement/register/action | Actie registreren voor een bereik van Microsoft.CostManagement door een abonnement. |
> | Bewerking | Microsoft.CostManagement/reports/action | Planning rapporteert over de gegevens over het gebruik door een bereik. |
> | Bewerking | Microsoft.CostManagement/reports/read | Planning rapporteert over de gegevens over het gebruik door een bereik. |
> | Bewerking | Microsoft.CostManagement/tenants/register/action | Actie voor een bereik van Microsoft.CostManagement door een tenant te registreren. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.CustomerInsights/hubs/adobemetadata/action | Maken of bijwerken van alle Azure-klant Insights Adobe-metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/adobemetadata/read | Alle Azure-klant Insights Adobe metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Een Azure-klant Insights Shared Access Signature beleid verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Alle lezen Azure Customer Insights gedeeld toegangsbeleid voor handtekening |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Azure Customer Insights handtekening beleid voor gedeelde toegang primaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Azure Customer Insights handtekening beleid voor gedeelde toegang secundaire sleutel opnieuw genereren |
> | Bewerking | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Een Azure-klant Insights Shared Access Signature-beleid maken of bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/activate/action | Activeren van een Azure-klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/activate/action | Activeren van een Azure-klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/delete | Een Azure-klant Insights-Connector verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | De status van alle Azure Customer Insights-Connector-runtime ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Activeren van een Azure-klant Insights-Connector-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Een Azure-klant Insights Connector toewijzing verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Lezen van het resultaat van een toewijzing van Azure Customer Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Lezen van een Azure-klant Insights-Connector-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Maken of bijwerken van een Azure-klant Insights-Connector-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/operations/read | Lezen van het resultaat van elke Azure Customer Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/read | Lezen van een Azure-klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Maken of bijwerken van een Azure-klant Insights Connector verificatie-informatie |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/update/action | Een Azure-klant Insights-Connector bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/connectors/write | Maken of bijwerken van een Azure-klant Insights-Connector |
> | Bewerking | Microsoft.CustomerInsights/hubs/crmmetadata/action | Maken of bijwerken van alle Azure-klant Insights Crm-metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/crmmetadata/read | Alle Azure-klant Insights Crm-metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/delete | Een Azure-klant Insights Hub verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/delete | Een Azure-klant Insights AVG verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/read | Een Azure-klant Insights AVG lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/gdpr/write | Maken of bijwerken van een Azure-klant Insights Avg |
> | Bewerking | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Ontvang Azure-klant Insights Hub facturering-tegoed |
> | Bewerking | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Factureringsgeschiedenis van Azure-klant Insights Hub ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/delete | Een installatiekopie van het Azure-klant Insights verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/read | Een afbeelding van Azure-klant Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/images/write | De installatiekopie van een Azure-klant Insights maken of bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/delete | Azure Customer Insights interacties verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/operations/read | Het resultaat van elke Azure Insights klantinteractie lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/read | Elke Azure-klant Insights interactie lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Relatie koppelingen voorstellen voor alle Azure-klant Insights-interacties |
> | Bewerking | Microsoft.CustomerInsights/hubs/interactions/write | Maken of bijwerken van elke Azure-klant Insights interactie |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/delete | Een Azure-klant Insights Key Performance Indicator verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/operations/read | Het resultaat van elke Azure Customer Insights Key Performance Indicators lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/read | Een Azure-klant Insights Key Performance Indicator lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Een Azure-klant Insights Key Performance Indicators opnieuw verwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/kpi/write | Maken of bijwerken van een Azure-klant Insights Key Performance Indicator |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/delete | Een Azure-klant Insights-koppelingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/operations/read | Het resultaat van elke Azure Customer Insights koppelingen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/read | Een koppeling maakt Azure-klant Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/links/write | Maken of bijwerken van eventuele koppelingen in de Azure-klant |
> | Bewerking | Microsoft.CustomerInsights/hubs/msemetadata/action | Maken of bijwerken van alle Azure-klant Insights Mse metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/msemetadata/read | Alle Azure-klant Insights Mse metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/operationresults/read | Resultaten van de Azure-klant Insights Hub-bewerking ophalen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/delete | Een Azure-klant Insights voorspellingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/operations/read | Het resultaat van elke Azure Customer Insights voorspellingen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/read | De voorspellingen van een Azure-klant Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictions/write | Maken of bijwerken van een Azure-klant voorspellingen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Azure-klant Insights Overeenkomstvoorspellingen beleid verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Het resultaat van elke Azure Customer Insights voorspellende overeenkomst beleid lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Azure-klant Insights Overeenkomstvoorspellingen beleid lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Een Azure-klant Insights Overeenkomstvoorspellingen beleid maken of bijwerken |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/delete | Een Azure-klant Insights-profiel verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/operations/read | Het resultaat van elke Azure Customer Insights-profiel lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/read | Een Azure-klant Insights-profiel te lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/profiles/write | Schrijven van een Azure-klant Insights-profiel |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor de resource |
> | Bewerking | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor resource |
> | Bewerking | Microsoft.CustomerInsights/hubs/read | Een Azure-klant Insights Hub lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Een Azure-klant Insights relatie koppelingen verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Het resultaat van elke Azure Customer Insights relatie koppelingen lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Een koppeling maakt Azure-klant Insights relatie lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Maken of bijwerken van eventuele koppelingen in de Azure-klant Insights relatie |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/delete | Verwijder alle Azure-klant Insights-relaties |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/operations/read | Het resultaat van elke Azure Insights klantrelaties lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/read | Azure-klant Insights relaties lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/relationships/write | Maken of bijwerken van een Azure-klant Insights-relaties |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Een Azure-klant Insights Rbac toewijzing verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Het resultaat van elke Azure Customer Insights Rbac toewijzing lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/read | Lezen van een Azure-klant Insights Rbac-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/roleAssignments/write | Maken of bijwerken van een Azure-klant Insights Rbac-toewijzing |
> | Bewerking | Microsoft.CustomerInsights/hubs/roles/read | Lezen van een Azure-klant Insights Rbac-rollen |
> | Bewerking | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Maken of bijwerken van alle Azure-klant Insights SalesForce-metagegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Alle Azure-klant Insights SalesForce metagegevens lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/delete | De segmenten van een Azure-klant Insights verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Beheer van een Azure-klant inzicht dynamische segmenten |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/read | De segmenten van een Azure-klant Insights lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/static/action | Beheer van alle Azure-klant inzicht statische segmenten |
> | Bewerking | Microsoft.CustomerInsights/hubs/segments/write | Maken of bijwerken van een Azure-klant Insights segmenten |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Een Azure-klant Insights SqlConnectionStrings verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Een Azure-klant Insights SqlConnectionStrings lezen |
> | Bewerking | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Maken of bijwerken van een Azure-klant Insights SqlConnectionStrings |
> | Bewerking | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Voorstellen Type Schema genereren van voorbeeldgegevens |
> | Bewerking | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Azure Customer Insights hub instellingen beheren |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/delete | Een Azure-klant Insights App weergave verwijderen |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/read | Lezen van een Azure-klant Insights App weergeven |
> | Bewerking | Microsoft.CustomerInsights/hubs/views/write | Maken of bijwerken van een Azure-klant Insights App weergeven |
> | Bewerking | Microsoft.CustomerInsights/hubs/widgettypes/read | Lezen van alle typen Azure-klant Insights App Widget |
> | Bewerking | Microsoft.CustomerInsights/hubs/write | Maken of bijwerken van een Azure-klant Insights-Hub |
> | Bewerking | Microsoft.CustomerInsights/operations/read | Azure-klant Insights Api Metadatas lezen |
> | Bewerking | Microsoft.CustomerInsights/register/action | Hiermee wordt het abonnement voor de Customer Insights-resourceprovider geregistreerd en wordt het maken van Customer Insights-resources |
> | Bewerking | Microsoft.CustomerInsights/unregister/action | De registratie van het abonnement voor de Customer Insights-resourceprovider |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataBox/jobs/bookShipmentPickUp/action | Hiermee kunt u het ophalen van retourzendingen worden geboekt. |
> | Bewerking | Microsoft.DataBox/jobs/cancel/action | Hiermee wordt een order in uitvoering geannuleerd. |
> | Bewerking | Microsoft.DataBox/jobs/delete | De Orders verwijderen |
> | Bewerking | Microsoft.DataBox/jobs/listCredentials/action | Geeft een lijst van de niet-versleutelde referenties met betrekking tot de order. |
> | Bewerking | Microsoft.DataBox/jobs/read | Lijst maken of ophalen van de Orders |
> | Bewerking | Microsoft.DataBox/jobs/write | Maken of bijwerken van de Orders |
> | Bewerking | Microsoft.DataBox/locations/availableSkus/action | Deze methode retourneert de lijst met beschikbare SKU's. |
> | Bewerking | Microsoft.DataBox/locations/operationResults/read | Lijst maken of de resultaten van de bewerking ophalen |
> | Bewerking | Microsoft.DataBox/locations/validateAddress/action | Hiermee wordt het verzendadres gevalideerd en biedt alternatieve adressen indien van toepassing. |
> | Bewerking | Microsoft.DataBox/register/action | Provider Microsoft.Databox registreren |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Geeft een lijst of opgehaald van de waarschuwingen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Geeft een lijst of opgehaald van de waarschuwingen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Hiermee verwijdert u de bandbreedte-planningen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Geeft een lijst of opgehaald van de bandbreedte-planningen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Geeft een lijst of opgehaald van de bandbreedte-planningen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Hiermee maken of bijwerken van de bandbreedte-planningen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Hiermee verwijdert u de gegevens in het Edge-apparaten |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Updates downloaden van het apparaat |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/extendedInformation/action | Informatie over de uitgebreide resource opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/getExtendedInformation/action | Informatie over de uitgebreide resource opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Updates installeren op apparaat |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | Geeft een lijst of de taken opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | Geeft een lijst of de netwerkinstellingen van het apparaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/operationsStatus/read | Geeft een lijst of de status van de bewerking opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/delete | Hiermee verwijdert u de orders |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Geeft een lijst of de orders opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/read | Geeft een lijst of de orders opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/orders/write | Hiermee maakt of werkt de orders |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Geeft een lijst of opgehaald van de gegevens in het Edge-apparaten |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Geeft een lijst of opgehaald van de gegevens in het Edge-apparaten |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Geeft een lijst of opgehaald van de gegevens in het Edge-apparaten |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Hiermee verwijdert u de rollen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Geeft een lijst of de rollen opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Geeft een lijst of de rollen opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Hiermee maken of bijwerken van de rollen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Zoeken naar updates |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Update-beveiligingsinstellingen |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Hiermee verwijdert u de shares |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Geeft een lijst of de shares opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Geeft een lijst of de shares opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | Vernieuwen van de metagegevens van de share met de gegevens vanuit de cloud |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Hiermee maakt of werkt de shares |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Hiermee verwijdert u de referenties van het storage-account |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Geeft een lijst of de referenties van het storage-account opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Geeft een lijst of de referenties van het storage-account opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Hiermee maakt of werkt de opslagaccountreferenties |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/delete | Hiermee verwijdert u de triggers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Geeft een lijst of opgehaald van de triggers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/read | Geeft een lijst of opgehaald van de triggers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/triggers/write | Hiermee maken of bijwerken van de triggers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | Geeft een lijst of de update samenvatting opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Voor device Registration service-certificaat uploaden |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Hiermee verwijdert u de share-gebruikers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | Geeft een lijst of het bewerkingsresultaat opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Geeft een lijst of de gebruikers opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Geeft een lijst of de gebruikers opgehaald |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Hiermee maken of bijwerken van de gebruikers |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Hiermee maken of bijwerken van de gegevens in het Edge-apparaten |
> | Bewerking | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Hiermee maken of bijwerken van de gegevens in het Edge-apparaten |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Databricks/locations/getNetworkPolicies/action | Netwerk kunt u lezen wat het beleid voor een subnet op basis van de locatie die wordt gebruikt door NRP ophalen |
> | Bewerking | Microsoft.Databricks/register/action | Registreren bij Databricks. |
> | Bewerking | Microsoft.Databricks/workspaces/delete | Hiermee verwijdert u een Databricks-werkruimte. |
> | Bewerking | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee stelt u de beschikbare diagnostische instellingen voor de Databricks-werkruimte |
> | Bewerking | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Toevoegen of wijzigen van instellingen voor diagnostische gegevens. |
> | Bewerking | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | De logboekdefinities van de beschikbaar voor de Databricks-werkruimte opgehaald |
> | Bewerking | Microsoft.Databricks/workspaces/read | Hiermee haalt een lijst met Databricks-werkruimten. |
> | Bewerking | Microsoft.Databricks/workspaces/write | Hiermee maakt u een Databricks-werkruimte. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataCatalog/datacatalogs/delete | DataCatalog resource verwijderen voor Data Catalog-Resourceprovider. |
> | Bewerking | Microsoft.DataCatalog/datacatalogs/read | DataCatalog resource lezen voor Data Catalog-Resourceprovider. |
> | Bewerking | Microsoft.DataCatalog/datacatalogs/write | Schrijven DataCatalog resource voor Data Catalog Resource Provider. |
> | Bewerking | Microsoft.DataCatalog/operations/read | Leest alle beschikbare bewerkingen in Data Catalog Resource Provider. |
> | Bewerking | Microsoft.DataCatalog/register/action | Het abonnement voor Data Catalog-Resourceprovider registreren |
> | Bewerking | Microsoft.DataCatalog/unregister/action | Registratie van het abonnement voor Data Catalog-Resourceprovider |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Controleert of de naam van de Data Factory beschikbaar is voor gebruik. |
> | Bewerking | Microsoft.DataFactory/datafactories/activitywindows/read | Activiteit Windows in de Data Factory met de opgegeven parameters worden gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Activiteit Windows leest van de Pipeline-activiteit met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Activiteit Windows leest van de pijplijn met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/delete | Hiermee verwijdert u een pijplijn. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Hiermee wordt een pijplijn onderbroken. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/read | Hiermee leest u een pijplijn. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Een pijplijn wordt hervat. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/update/action | Een pijplijn-updates. |
> | Bewerking | Microsoft.DataFactory/datafactories/datapipelines/write | Hiermee of bijwerken van een pijplijn. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Activiteit Windows leest van de gegevensset met de opgegeven parameters. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/read | Hiermee leest u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Hiermee leest u de gegevens segment worden uitgevoerd voor de opgegeven gegevensset met de opgegeven begintijd uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/slices/read | Hiermee haalt de gegevenssegmenten in de opgegeven periode. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/slices/write | De Status van het gegevenssegment bijwerken. |
> | Bewerking | Microsoft.DataFactory/datafactories/datasets/write | Hiermee of bijwerken van een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/delete | Hiermee verwijdert u de Data Factory. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Leest de verbindingsgegevens voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/delete | Hiermee verwijdert u een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Een lijst met verificatiesleutels voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/read | Een Gateway worden gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Genereert opnieuw de verificatiesleutels voor een Gateway. |
> | Bewerking | Microsoft.DataFactory/datafactories/gateways/write | Hiermee maakt of een Gateway-Updates. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/delete | Hiermee verwijdert u eventuele gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/read | Leest alle gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/datafactories/linkedServices/write | Hiermee of bijwerken van een gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/datafactories/read | Hiermee leest u de Data Factory. |
> | Bewerking | Microsoft.DataFactory/datafactories/runs/loginfo/read | Een SAS-URI naar een blobcontainer met de logboeken worden gelezen. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/read | Hiermee leest u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/tables/write | Hiermee of bijwerken van een gegevensset. |
> | Bewerking | Microsoft.DataFactory/datafactories/write | Hiermee of bijwerken van de Data Factory. |
> | Bewerking | Microsoft.DataFactory/factories/cancelpipelinerun/action | Hiermee annuleert u de pijplijnuitvoering die is opgegeven door de run-ID. |
> | Bewerking | Microsoft.DataFactory/factories/createdataflowdebugsession/action | Hiermee maakt u een foutopsporingssessie voor stroom van gegevens. |
> | Bewerking | Microsoft.DataFactory/factories/dataflows/delete | Hiermee verwijdert u de gegevensstroom. |
> | Bewerking | Microsoft.DataFactory/factories/dataflows/read | Hiermee leest u de gegevensstroom. |
> | Bewerking | Microsoft.DataFactory/factories/dataflows/write | Maken of bijwerken van de gegevensstroom |
> | Bewerking | Microsoft.DataFactory/factories/datasets/delete | Hiermee verwijdert u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/factories/datasets/read | Hiermee leest u een gegevensset. |
> | Bewerking | Microsoft.DataFactory/factories/datasets/write | Hiermee of bijwerken van een gegevensset. |
> | Bewerking | Microsoft.DataFactory/factories/delete | Hiermee verwijdert u Data Factory. |
> | Bewerking | Microsoft.DataFactory/factories/deletedataflowdebugsession/action | Hiermee verwijdert u de foutopsporingssessie van een gegevens-stroom. |
> | Bewerking | Microsoft.DataFactory/factories/getDataPlaneAccess/action | Hiermee toegang tot de ADF-DataPlane-service. |
> | Bewerking | Microsoft.DataFactory/factories/getDataPlaneAccess/read | Leesbewerkingen toegang tot de ADF-DataPlane-service. |
> | Bewerking | Microsoft.DataFactory/factories/getFeatureValue/action | De waarde van de functie besturingselement blootstelling ophalen voor de specifieke locatie. |
> | Bewerking | Microsoft.DataFactory/factories/getFeatureValue/read | Leest de waarde voor de functie van blootstelling van besturingselement voor de specifieke locatie. |
> | Bewerking | Microsoft.DataFactory/factories/getGitHubAccessToken/action | Hiermee haalt u GitHub access token. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/delete | Hiermee verwijdert u eventuele Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Integration Runtime-verbindingsgegevens worden gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | SSIS-Integratieruntime metagegevens ophalen voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Status van de integratie-Runtime leest. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Gekoppelde Integration Runtime verwijzing in de opgegeven gedeelde Integratieruntime maken. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Een lijst met verificatiesleutels voor een integratie-Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Hiermee haalt de bewakingsgegevens voor alle Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Hiermee verwijdert u het knooppunt voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Retourneert het IP-adres voor het opgegeven knooppunt van Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | Leest het knooppunt voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Een zelf-hostende knooppunt voor Integration Runtime-updates. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/read | Leest een Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | SSIS-Integratieruntime metagegevens vernieuwen voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Genereert opnieuw de verificatie-sleutels voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Gekoppelde verwijzingen van Integration Runtime verwijdert uit de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/start/action | Start een Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Stopt alle Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Synchroniseert de referenties voor de opgegeven Integration Runtime. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | De opgegeven Integration Runtime wordt bijgewerkt. |
> | Bewerking | Microsoft.DataFactory/factories/integrationruntimes/write | Hiermee of bijwerken van een Integratieruntime. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/delete | Verwijderen gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/read | Leesbewerkingen gekoppelde Service. |
> | Bewerking | Microsoft.DataFactory/factories/linkedServices/write | Gekoppelde Service maken of bijwerken |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Leest dat de activiteit wordt uitgevoerd voor de opgegeven id voor de pijplijnuitvoering |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/cancel/action | Hiermee annuleert u de pijplijnuitvoering die is opgegeven door de run-ID. |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Query's die de activiteit wordt uitgevoerd voor de opgegeven pijplijn uitvoeren ID. |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Leest dat het resultaat van query-activiteiten wordt uitgevoerd voor de opgegeven id voor de pijplijnuitvoering |
> | Bewerking | Microsoft.DataFactory/factories/pipelineruns/read | Hiermee leest u de pijplijn wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/createrun/action | Hiermee maakt u een uitvoering voor de pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/delete | Hiermee verwijdert u een pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Hiermee haalt u de voortgang van de uitvoeringen van activiteit. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | Hiermee leest u de uitvoering van de pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/read | Leest de pijplijn. |
> | Bewerking | Microsoft.DataFactory/factories/pipelines/write | Maken of bijwerken van de pijplijn |
> | Bewerking | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Query uitgevoerd voor de foutopsporing pijplijn wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/querypipelineruns/action | Query uitgevoerd voor de pijplijn wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/querypipelineruns/read | Hiermee wordt het resultaat van Query-Pijplijnuitvoeringen gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/querytriggerruns/action | Query uitgevoerd voor de Trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/querytriggerruns/read | Hiermee leest u het resultaat van de Trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/read | Hiermee leest u Data Factory. |
> | Bewerking | Microsoft.DataFactory/factories/startdataflowdebugsession/action | Start een foutopsporingssessie voor stroom van gegevens. |
> | Bewerking | Microsoft.DataFactory/factories/triggerruns/read | Hiermee leest u de Trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/delete | Hiermee verwijdert u een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/read | Hiermee wordt een Trigger gelezen. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/start/action | Hiermee start u een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/stop/action | Hiermee stopt u een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/triggerruns/read | Hiermee leest u de Trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.DataFactory/factories/triggers/write | Hiermee of bijwerken van een Trigger. |
> | Bewerking | Microsoft.DataFactory/factories/write | Maken of bijwerken van de Data Factory |
> | Bewerking | Microsoft.DataFactory/locations/configureFactoryRepo/action | Hiermee configureert u de opslagplaats voor de factory. |
> | Bewerking | Microsoft.DataFactory/locations/getFeatureValue/action | De waarde van de functie besturingselement blootstelling ophalen voor de specifieke locatie. |
> | Bewerking | Microsoft.DataFactory/locations/getFeatureValue/read | Leest de waarde voor de functie van blootstelling van besturingselement voor de specifieke locatie. |
> | Bewerking | Microsoft.DataFactory/operations/read | Leest alle bewerkingen in Microsoft Data Factory-Provider. |
> | Bewerking | Microsoft.DataFactory/register/action | Hiermee wordt het abonnement voor de Data Factory-Resourceprovider geregistreerd. |
> | Bewerking | Microsoft.DataFactory/unregister/action | De registratie van het abonnement voor de Data Factory-Resourceprovider. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Een compute-beleid niet verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Informatie ophalen over een compute-beleid. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Maken of bijwerken van een compute-beleid. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Een DataLakeStore-account loskoppelen van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Informatie ophalen over een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/delete | Een account DataLakeAnalytics verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Een firewallregel verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Informatie ophalen over een firewallregel. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Maken of bijwerken van een firewallregel. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Resultaat van een bewerking van de account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/read | Informatie ophalen over een bestaand DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Lijst met SAS-tokens voor storage-containers van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Haal de containers op van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Een Storage-account loskoppelen van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Informatie ophalen over een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen voor het annuleren van de taken die worden ingediend door andere gebruikers. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | SystemMaxAnalyticsUnits overbrengen tussen DataLakeAnalytics accounts. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/delete | Een virtueel netwerk-regel verwijderen. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/read | Informatie ophalen over een regel voor virtuele netwerken. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/virtualNetworkRules/write | Maken of bijwerken van een regel voor virtuele netwerken. |
> | Bewerking | Microsoft.DataLakeAnalytics/accounts/write | Maken of bijwerken van een DataLakeAnalytics-account. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/capability/read | Informatie over de functionaliteit van een abonnement ophalen met betrekking tot DataLakeAnalytics gebruiken. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | De beschikbaarheid van de naam van een DataLakeAnalytics controleren. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/operationResults/read | Resultaat van een bewerking van de account DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/locations/usages/read | Quotum voor het gebruik van informatie van een abonnement met betrekking tot DataLakeAnalytics gebruiken. |
> | Bewerking | Microsoft.DataLakeAnalytics/operations/read | Beschikbare bewerkingen van DataLakeAnalytics ophalen. |
> | Bewerking | Microsoft.DataLakeAnalytics/register/action | Abonnement in DataLakeAnalytics registreren. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataLakeStore/accounts/delete | Een DataLakeStore-account verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Key Vault voor een DataLakeStore-account kunt inschakelen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Een Filter EventGrid verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Een Filter EventGrid ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Maken of bijwerken van een EventGrid Filter. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/delete | Een firewallregel verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/read | Informatie ophalen over een firewallregel. |
> | Bewerking | Microsoft.DataLakeStore/accounts/firewallRules/write | Maken of bijwerken van een firewallregel. |
> | Bewerking | Microsoft.DataLakeStore/accounts/operationResults/read | Resultaat van een bewerking DataLakeStore-account ophalen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/read | Informatie ophalen over een bestaand DataLakeStore-account. |
> | Bewerking | Microsoft.DataLakeStore/accounts/Superuser/action | Superuser op Data Lake Store als verleend met Microsoft.Authorization/roleAssignments/write verlenen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Verwijderen van een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Informatie ophalen over een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Maken of bijwerken van een vertrouwde id-provider. |
> | Bewerking | Microsoft.DataLakeStore/accounts/virtualNetworkRules/delete | Een virtueel netwerk-regel verwijderen. |
> | Bewerking | Microsoft.DataLakeStore/accounts/virtualNetworkRules/read | Informatie ophalen over een regel voor virtuele netwerken. |
> | Bewerking | Microsoft.DataLakeStore/accounts/virtualNetworkRules/write | Maken of bijwerken van een regel voor virtuele netwerken. |
> | Bewerking | Microsoft.DataLakeStore/accounts/write | Maken of bijwerken van een DataLakeStore-account. |
> | Bewerking | Microsoft.DataLakeStore/locations/capability/read | Informatie over de functionaliteit van een abonnement ophalen met betrekking tot DataLakeStore gebruiken. |
> | Bewerking | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Controleer de beschikbaarheid van een DataLakeStore-accountnaam. |
> | Bewerking | Microsoft.DataLakeStore/locations/operationResults/read | Resultaat van een bewerking DataLakeStore-account ophalen. |
> | Bewerking | Microsoft.DataLakeStore/locations/usages/read | Quotum voor het gebruik van informatie van een abonnement met betrekking tot DataLakeStore gebruiken. |
> | Bewerking | Microsoft.DataLakeStore/operations/read | Beschikbare bewerkingen van DataLakeStore ophalen. |
> | Bewerking | Microsoft.DataLakeStore/register/action | Abonnement voor DataLakeStore registreren. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DataMigration/locations/operationResults/read | Haal de status van een langdurige bewerking die betrekking hebben op een 202 antwoord (geaccepteerd) |
> | Bewerking | Microsoft.DataMigration/locations/operationStatuses/read | Haal de status van een langdurige bewerking die betrekking hebben op een 202 antwoord (geaccepteerd) |
> | Bewerking | Microsoft.DataMigration/register/action | Hiermee wordt het abonnement met de Azure Database Migration Service-provider |
> | Bewerking | Microsoft.DataMigration/services/addWorker/action | Een werknemer DMS toegevoegd aan van de Service beschikbaar werknemers |
> | Bewerking | Microsoft.DataMigration/services/checkStatus/action | Controleer of de service geïmplementeerd en wordt uitgevoerd is |
> | Bewerking | Microsoft.DataMigration/services/configureWorker/action | Hiermee configureert u een van de Service beschikbaar werknemers DMS-werkrol |
> | Bewerking | Microsoft.DataMigration/services/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/projects/accessArtifacts/action | Een URL genereren die kan worden gebruikt om GET of PUT voor projectartefacten |
> | Bewerking | Microsoft.DataMigration/services/projects/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/projects/files/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/projects/files/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/projects/files/read/action | Verkrijgen van een URL die kan worden gebruikt om de inhoud van het bestand te lezen |
> | Bewerking | Microsoft.DataMigration/services/projects/files/readWrite/action | Een URL die kan worden gebruikt om te lezen of schrijven van de inhoud van het bestand ophalen |
> | Bewerking | Microsoft.DataMigration/services/projects/files/write | Resources en hun eigenschappen maken of bijwerken |
> | Bewerking | Microsoft.DataMigration/services/projects/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/projects/tasks/cancel/action | Annuleer de taak als deze momenteel wordt uitgevoerd |
> | Bewerking | Microsoft.DataMigration/services/projects/tasks/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/projects/tasks/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/projects/tasks/write | Azure Database Migration Service-taken voor taken uitvoeren |
> | Bewerking | Microsoft.DataMigration/services/projects/write | Azure Database Migration Service-taken voor taken uitvoeren |
> | Bewerking | Microsoft.DataMigration/services/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/removeWorker/action | Hiermee verwijdert u een van de Service beschikbaar werknemers DMS-werkrol |
> | Bewerking | Microsoft.DataMigration/services/serviceTasks/cancel/action | Annuleer de taak als deze momenteel wordt uitgevoerd |
> | Bewerking | Microsoft.DataMigration/services/serviceTasks/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/serviceTasks/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/serviceTasks/write | Azure Database Migration Service-taken voor taken uitvoeren |
> | Bewerking | Microsoft.DataMigration/services/slots/delete | Hiermee verwijdert u een resource en alle onderliggende items |
> | Bewerking | Microsoft.DataMigration/services/slots/read | Lees informatie over bronnen |
> | Bewerking | Microsoft.DataMigration/services/slots/write | Resources en hun eigenschappen maken of bijwerken |
> | Bewerking | Microsoft.DataMigration/services/start/action | Start de DMS-service zodat deze opnieuw migraties kan verwerken |
> | Bewerking | Microsoft.DataMigration/services/stop/action | Stop de DMS-service om te minimaliseren, de kosten |
> | Bewerking | Microsoft.DataMigration/services/updateAgentConfig/action | De configuratie van de DMS-agent-updates met de opgegeven waarden. |
> | Bewerking | Microsoft.DataMigration/services/write | Resources en hun eigenschappen maken of bijwerken |
> | Bewerking | Microsoft.DataMigration/skus/read | Haal een lijst van SKU's die door DMS-bronnen worden ondersteund. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DBforMariaDB/locations/azureAsyncOperation/read | Retourneren van resultaten van de bewerking voor de MariaDB |
> | Bewerking | Microsoft.DBforMariaDB/locations/operationResults/read | Geretourneerde ResourceGroup op basis van de resultaten van de bewerking Server MariaDB |
> | Bewerking | Microsoft.DBforMariaDB/locations/operationResults/read | Retourneren van resultaten van de bewerking voor de MariaDB |
> | Bewerking | Microsoft.DBforMariaDB/locations/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMariaDB/locations/securityAlertPoliciesAzureAsyncOperation/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforMariaDB/operations/read | Retourneert de lijst met MariaDB-bewerkingen. |
> | Bewerking | Microsoft.DBforMariaDB/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMariaDB/register/action | MariaDB-Resourceprovider registreren |
> | Bewerking | Microsoft.DBforMariaDB/servers/configurations/read | Retourneert de lijst met configuraties voor een server of haalt de eigenschappen voor de opgegeven configuratie. |
> | Bewerking | Microsoft.DBforMariaDB/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Bewerking | Microsoft.DBforMariaDB/servers/databases/delete | Hiermee verwijdert u een bestaande Database voor MariaDB. |
> | Bewerking | Microsoft.DBforMariaDB/servers/databases/read | Retourneert de lijst met MariaDB Databases of haalt de eigenschappen voor de opgegeven Database. |
> | Bewerking | Microsoft.DBforMariaDB/servers/databases/write | Hiermee maakt u een Database voor MariaDB met de opgegeven parameters of de eigenschappen voor de opgegeven Database niet bijwerken. |
> | Bewerking | Microsoft.DBforMariaDB/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforMariaDB/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewallregel. |
> | Bewerking | Microsoft.DBforMariaDB/servers/firewallRules/read | Retourneert de lijst met firewall-regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel. |
> | Bewerking | Microsoft.DBforMariaDB/servers/firewallRules/write | Maakt een firewall-regel met de opgegeven parameters of de update een bestaande regel. |
> | Bewerking | Microsoft.DBforMariaDB/servers/logFiles/read | Retourneert de lijst met MariaDB LogFiles. |
> | Bewerking | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor MariaDB-servers |
> | Bewerking | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforMariaDB/servers/queryTexts/action | De tekst voor een lijst met query's retourneren |
> | Bewerking | Microsoft.DBforMariaDB/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Bewerking | Microsoft.DBforMariaDB/servers/recoverableServers/read | Retourneert de gegevens van de herstelbare MariaDB-Server |
> | Bewerking | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Gegevens van de server threat detection dat is geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Het beleid voor detectie van bedreigingen server voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.DBforMariaDB/servers/topQueryStatistics/read | Retourneert de lijst met Query-statistieken voor de populairste query's. |
> | Bewerking | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Configuraties voor de opgegeven server bijwerken |
> | Bewerking | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | Retourneert de lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforMariaDB/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor virtuele netwerken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforMariaDB/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retourneren |
> | Bewerking | Microsoft.DBforMariaDB/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DBforMySQL/locations/azureAsyncOperation/read | Retourneren van resultaten van de bewerking MySQL-Server |
> | Bewerking | Microsoft.DBforMySQL/locations/operationResults/read | Geretourneerde ResourceGroup op basis van resultaten van de bewerking MySQL-Server |
> | Bewerking | Microsoft.DBforMySQL/locations/operationResults/read | Retourneren van resultaten van de bewerking MySQL-Server |
> | Bewerking | Microsoft.DBforMySQL/locations/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMySQL/locations/securityAlertPoliciesAzureAsyncOperation/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforMySQL/operations/read | Retourneert de lijst met bewerkingen voor MySQL. |
> | Bewerking | Microsoft.DBforMySQL/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforMySQL/register/action | MySQL-Resourceprovider registreren |
> | Bewerking | Microsoft.DBforMySQL/servers/configurations/read | Retourneert de lijst met configuraties voor een server of haalt de eigenschappen voor de opgegeven configuratie. |
> | Bewerking | Microsoft.DBforMySQL/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Bewerking | Microsoft.DBforMySQL/servers/databases/delete | Hiermee verwijdert u een bestaande Database voor MySQL. |
> | Bewerking | Microsoft.DBforMySQL/servers/databases/read | Retourneert de lijst met MySQL-Databases of haalt de eigenschappen voor de opgegeven Database. |
> | Bewerking | Microsoft.DBforMySQL/servers/databases/write | Hiermee maakt u een MySQL-Database met de opgegeven parameters of de eigenschappen voor de opgegeven Database niet bijwerken. |
> | Bewerking | Microsoft.DBforMySQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewallregel. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/read | Retourneert de lijst met firewall-regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel. |
> | Bewerking | Microsoft.DBforMySQL/servers/firewallRules/write | Maakt een firewall-regel met de opgegeven parameters of de update een bestaande regel. |
> | Bewerking | Microsoft.DBforMySQL/servers/logFiles/read | Retourneert de lijst met PostgreSQL LogFiles. |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor MySQL-servers |
> | Bewerking | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforMySQL/servers/queryTexts/action | De tekst voor een lijst met query's retourneren |
> | Bewerking | Microsoft.DBforMySQL/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Bewerking | Microsoft.DBforMySQL/servers/recoverableServers/read | Retourneert de herstelbare gegevens van de MySQL-Server |
> | Bewerking | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Gegevens van de server threat detection dat is geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Het beleid voor detectie van bedreigingen server voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.DBforMySQL/servers/topQueryStatistics/read | Retourneert de lijst met Query-statistieken voor de populairste query's. |
> | Bewerking | Microsoft.DBforMySQL/servers/updateConfigurations/action | Configuraties voor de opgegeven server bijwerken |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Retourneert de lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor virtuele netwerken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforMySQL/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retourneren |
> | Bewerking | Microsoft.DBforMySQL/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/azureAsyncOperation/read | Retourneren van resultaten van de bewerking PostgreSQL-Server |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/operationResults/read | Geretourneerde ResourceGroup op basis van resultaten van de bewerking PostgreSQL-Server |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/operationResults/read | Retourneren van resultaten van de bewerking PostgreSQL-Server |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | De lijst met Server threat detection bewerkingsresultaat retourneren. |
> | Bewerking | Microsoft.DBforPostgreSQL/operations/read | Retourneert de lijst met bewerkingen voor PostgreSQL. |
> | Bewerking | Microsoft.DBforPostgreSQL/performanceTiers/read | Retourneert de lijst met beschikbare Prestatielagen. |
> | Bewerking | Microsoft.DBforPostgreSQL/register/action | PostgreSQL-Resourceprovider registreren |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/advisors/read | Retourneert de lijst met adviseurs |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Aanbevelingen |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/configurations/read | Retourneert de lijst met configuraties voor een server of haalt de eigenschappen voor de opgegeven configuratie. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/databases/delete | Hiermee verwijdert u een bestaande Database voor PostgreSQL. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/databases/read | Retourneert de lijst met PostgreSQL-Databases of haalt de eigenschappen voor de opgegeven Database. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/databases/write | Hiermee maakt u een PostgreSQL-Database met de opgegeven parameters of de eigenschappen voor de opgegeven Database niet bijwerken. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Hiermee verwijdert u een bestaande firewallregel. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Retourneert de lijst met firewall-regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Maakt een firewall-regel met de opgegeven parameters of de update een bestaande regel. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/logFiles/read | Retourneert de lijst met PostgreSQL LogFiles. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor Postgres-servers |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/queryTexts/action | De tekst van een query retourneren |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/queryTexts/read | De tekst voor een lijst met query's retourneren |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Retourneert de herstelbare gegevens van de PostgreSQL-Server |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Gegevens van de server threat detection dat is geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Het beleid voor detectie van bedreigingen server voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | Retourneert de lijst met Query-statistieken voor de populairste query's. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Configuraties voor de opgegeven server bijwerken |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Retourneert de lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor virtuele netwerken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Wacht statistieken voor een exemplaar retourneren |
> | Bewerking | Microsoft.DBforPostgreSQL/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/configurations/read | Retourneert de lijst met configuraties voor een server of haalt de eigenschappen voor de opgegeven configuratie. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/configurations/write | De waarde voor de opgegeven configuratie bijwerken |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/firewallRules/delete | Hiermee verwijdert u een bestaande firewallregel. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/firewallRules/read | Retourneert de lijst met firewall-regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/firewallRules/write | Maakt een firewall-regel met de opgegeven parameters of de update een bestaande regel. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de disagnostic instelling voor de resource opgehaald |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor Postgres-servers |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/updateConfigurations/action | Configuraties voor de opgegeven server bijwerken |
> | Bewerking | Microsoft.DBforPostgreSQL/serversv2/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Devices/checkNameAvailability/Action | Controleer als IotHub-naam beschikbaar is |
> | Bewerking | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Controleer de servicenaam als inrichting is beschikbaar |
> | Bewerking | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Certificaat verwijderen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Verificatiecode genereren |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Haalt het certificaat |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Controleer de resource-certificaat |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Maken of bijwerken van certificaat |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Delete | De resource IotHub-tenant verwijderen |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Event hub-Consumentengroep verwijderen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Event hub-klantengroep(en) ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Event hub-Consumentengroep maken |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Apparaten exporteren |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | De IotHub-tenant statistieken resource opgehaald |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Apparaten importeren |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Haalt de IotHub-tenantsleutel |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Details van ingediend op de opgegeven IotHub taak/taken ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | IotHub-tenantsleutels opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | De logboekdefinities van de beschikbaar voor de IotHub-Service opgehaald |
> | Bewerking | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Quotum metrische gegevens ophalen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Read | De resource van de tenant IotHub opgehaald |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Een bericht op basis van alle bestaande Routes testen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Een bericht op basis van een opgegeven test Route testen |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Hiermee haalt u de status van alle routering eindpunten voor een IotHub |
> | Bewerking | Microsoft.Devices/elasticPools/iotHubTenants/Write | Maken of bijwerken van de IotHub-tenant-resource |
> | Bewerking | Microsoft.Devices/ElasticPools/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Delete | Certificaat verwijderen |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Verificatiecode genereren |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Read | Haalt het certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/verify/Action | Controleer de resource-certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/certificates/Write | Maken of bijwerken van certificaat |
> | Bewerking | Microsoft.Devices/iotHubs/Delete | IotHub-Resource verwijderen |
> | Bewerking | Microsoft.Devices/IotHubs/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/IotHubs/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Hiermee wordt het gebeurtenisrasterfilter verwijderd |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Read | Het gebeurtenisrasterfilter opgehaald |
> | Bewerking | Microsoft.Devices/iotHubs/eventGridFilters/Write | Maak een nieuwe of bestaande Event Grid-filter bijwerken |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Event hub-Consumentengroep verwijderen |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Event hub-klantengroep(en) ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Event hub-Consumentengroep maken |
> | Bewerking | Microsoft.Devices/iotHubs/exportDevices/Action | Apparaten exporteren |
> | Bewerking | Microsoft.Devices/iotHubs/importDevices/Action | Apparaten importeren |
> | Bewerking | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | IotHub Key voor de opgegeven naam ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/iotHubStats/Read | IotHub-statistieken ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/jobs/Read | Details van ingediend op de opgegeven IotHub taak/taken ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/listkeys/Action | Alle IotHub-sleutels ophalen |
> | Bewerking | Microsoft.Devices/IotHubs/logDefinitions/read | De logboekdefinities van de beschikbaar voor de IotHub-Service opgehaald |
> | Bewerking | Microsoft.Devices/IotHubs/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de IotHub-service |
> | Bewerking | Microsoft.Devices/iotHubs/operationresults/Read | Bewerkingsresultaat van de (verouderde API) ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/quotaMetrics/Read | Quotum metrische gegevens ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/Read | Haalt de IotHub-resource (s) |
> | Bewerking | Microsoft.Devices/iotHubs/routing/$testall/Action | Een bericht op basis van alle bestaande Routes testen |
> | Bewerking | Microsoft.Devices/iotHubs/routing/$testnew/Action | Een bericht op basis van een opgegeven test Route testen |
> | Bewerking | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Hiermee haalt u de status van alle routering eindpunten voor een IotHub |
> | Bewerking | Microsoft.Devices/iotHubs/skus/Read | Geldige IotHub Skus ophalen |
> | Bewerking | Microsoft.Devices/iotHubs/Write | Maken of bijwerken van de IotHub-Resource |
> | Bewerking | Microsoft.Devices/locations/operationresults/Read | Ophalen bewerkingsresultaat op basis van locatie |
> | Bewerking | Microsoft.Devices/operationresults/Read | Bewerkingsresultaat van de ophalen |
> | Bewerking | Microsoft.Devices/operations/Read | Alle bewerkingen van de ResourceProvider ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Delete | Certificaat verwijderen |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Verificatiecode genereren |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Read | Haalt het certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/verify/Action | Controleer de resource-certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/certificates/Write | Maken of bijwerken van certificaat |
> | Bewerking | Microsoft.Devices/provisioningServices/Delete | IotDps resource verwijderen |
> | Bewerking | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Devices/provisioningServices/keys/listkeys/Action | IotDps sleutels voor de naam van de sleutel ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/listkeys/Action | Alle IotDps-sleutels ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/logDefinitions/read | De logboekdefinities van de beschikbaar voor de provisioning-Service opgehaald |
> | Bewerking | Microsoft.Devices/provisioningServices/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de provisioning-service |
> | Bewerking | Microsoft.Devices/provisioningServices/operationresults/Read | Resultaat van de DPS-bewerking ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/Read | Ophalen van IotDps |
> | Bewerking | Microsoft.Devices/provisioningServices/skus/Read | Geldige IotDps Skus ophalen |
> | Bewerking | Microsoft.Devices/provisioningServices/Write | IotDps resource maken |
> | Bewerking | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resourceprovider geregistreerd en wordt het maken van IotHub-resources |
> | Bewerking | Microsoft.Devices/register/action | Het abonnement voor de IotHub-resourceprovider geregistreerd en wordt het maken van IotHub-resources |
> | Bewerking | Microsoft.Devices/usages/Read | Neem abonnement informatie over het gebruik voor deze provider. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DevSpaces/controllers/delete | Azure Dev spaties Controller en dataplane-services verwijderen |
> | Bewerking | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Lijst met verbindingsgegevens voor de Azure Dev spaties Controller-infrastructuur |
> | Bewerking | Microsoft.DevSpaces/controllers/read | Eigenschappen lezen Azure Dev spaties netwerkcontroller |
> | Bewerking | Microsoft.DevSpaces/controllers/write | Maken of de eigenschappen van de Update Azure Dev spaties Controller |
> | Bewerking | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Bestaande domeincontroller-toewijzing voor een containerhost controleren |
> | Bewerking | Microsoft.DevSpaces/locations/operationresults/read | Status voor een asynchrone bewerking lezen |
> | Bewerking | Microsoft.DevSpaces/register/action | Microsoft Dev spaties resourceprovider registreren met een abonnement |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DevTestLab/labCenters/delete | Lab centers verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labCenters/read | Lab-datacenters worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labCenters/write | Toevoegen of wijzigen van lab datacenters. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Azure resource manager-sjablonen lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Een ARM-sjabloon voor de opgegeven artefact genereert, wordt de vereiste bestanden geüpload naar een opslagaccount en valideert de gegenereerde artefact. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Artefacten worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/delete | Artefactbronnen verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/read | Artefactbronnen lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/artifactSources/write | Toevoegen of wijzigen van artefactbronnen. |
> | Bewerking | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Claim een willekeurige claimbare virtuele machine in het lab. |
> | Bewerking | Microsoft.DevTestLab/labs/costs/read | Kosten worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/costs/write | Toevoegen of wijzigen van kosten. |
> | Bewerking | Microsoft.DevTestLab/labs/CreateEnvironment/action | Virtuele machines in een lab maken. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/delete | Verwijderen van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/read | Aangepaste installatiekopieën worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/customImages/write | Toevoegen of wijzigen van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Zorg ervoor dat de huidige gebruiker heeft een geldig profiel in het lab. |
> | Bewerking | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Hiermee exporteert u het Resourcegebruik lab in een storage-account |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/delete | Verwijderen van formules. |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/read | Formules worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules. |
> | Bewerking | Microsoft.DevTestLab/labs/galleryImages/read | Galerie met installatiekopieën worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Een URI genereren voor het uploaden van aangepaste installatiekopieën naar een Lab. |
> | Bewerking | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Een virtuele machine importeren naar een andere lab. |
> | Bewerking | Microsoft.DevTestLab/labs/ListVhds/action | Een lijst van schijf-installatiekopieën die beschikbaar zijn voor het maken van aangepaste installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/delete | Notificationchannels verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Melding verzenden naar opgegeven kanaal. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/read | Notificationchannels lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/notificationChannels/write | Toevoegen of wijzigen van notificationchannels. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Lab-beleid evalueert. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/delete | Beleid verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/read | Lezen van beleid. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/policies/write | Toevoegen of wijzigen van beleid. |
> | Bewerking | Microsoft.DevTestLab/labs/policySets/read | Beleid voor lezen wordt ingesteld. |
> | Bewerking | Microsoft.DevTestLab/labs/read | Lees labs. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/delete | Schema's verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/Execute/action | Een planning worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Een lijst met alle van toepassing schema 's |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/delete | Service runners-up verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/read | Service runners-up lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/serviceRunners/write | Toevoegen of wijzigen van de service runners-up. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/delete | Gedeelde galerieën verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/read | Gedeelde galerieën lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Gedeelde afbeeldingen verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Gedeelde afbeeldingen lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Toevoegen of wijzigen van de gedeelde-installatiekopieën. |
> | Bewerking | Microsoft.DevTestLab/labs/sharedGalleries/write | Toevoegen of wijzigen van de gedeelde galerieën. |
> | Bewerking | Microsoft.DevTestLab/labs/users/delete | Gebruikersprofielen verwijdert. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/Attach/action | Koppelen en maken van de lease van de schijf naar de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/delete | Schijven verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/Detach/action | Loskoppelen en verbreekt u de lease van de schijf die is gekoppeld aan de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/read | Lees de schijven. |
> | Bewerking | Microsoft.DevTestLab/labs/users/disks/write | Toevoegen of wijzigen van schijven. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/delete | Omgevingen verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/read | Omgevingen lezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/read | Gebruikersprofielen worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/delete | Verwijder de geheimen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/read | Lezen van geheimen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/secrets/write | Toevoegen of wijzigen van geheimen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Service fabrics verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Geeft een lijst van de planning van toepassing starten/stoppen, indien van toepassing. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Service fabrics worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Schema's verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Een planning worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Start een service fabric. |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Stoppen van een service fabric |
> | Bewerking | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Toevoegen of wijzigen van de service fabrics. |
> | Bewerking | Microsoft.DevTestLab/labs/users/write | Toevoegen of wijzigen van gebruikersprofielen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Een nieuwe of bestaande gegevensschijf koppelen aan virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Artefacten van toepassing op virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Eigenaar worden van een bestaande virtuele machine |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/delete | Verwijder de virtuele machines. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | De opgegeven schijf loskoppelen van de virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Een tekenreeks waarmee de inhoud van het RDP-bestand voor de virtuele machine opgehaald |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Geeft een lijst van de planning van toepassing starten/stoppen, indien van toepassing. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/read | Virtuele machines gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Een virtuele machine opnieuw implementeren |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Grootte van virtuele Machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Start opnieuw op een virtuele machine. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Schema's verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Een planning worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Een virtuele machine start. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Een virtuele machine stoppen |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Alle gegevensschijven die zijn gekoppeld aan de virtuele machine moet eigendom zijn van de huidige gebruiker brengt. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Release-eigendom van een bestaande virtuele machine |
> | Bewerking | Microsoft.DevTestLab/labs/virtualMachines/write | Toevoegen of wijzigen van virtuele machines. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/delete | Virtuele netwerken niet verwijderen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/read | Virtuele netwerken gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/virtualNetworks/write | Toevoegen of wijzigen van virtuele netwerken. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/delete | Verwijder de VM-pools. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/read | VM-pools worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/labs/vmPools/write | Toevoegen of wijzigen van VM-pools. |
> | Bewerking | Microsoft.DevTestLab/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.DevTestLab/locations/operations/read | Leesbewerkingen. |
> | Bewerking | Microsoft.DevTestLab/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.DevTestLab/schedules/delete | Schema's verwijderen. |
> | Bewerking | Microsoft.DevTestLab/schedules/Execute/action | Een planning worden uitgevoerd. |
> | Bewerking | Microsoft.DevTestLab/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.DevTestLab/schedules/Retarget/action | Updates van een schema doelresource-id. |
> | Bewerking | Microsoft.DevTestLab/schedules/write | Toevoegen of wijzigen van schema's. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DocumentDB/databaseAccountNames/read | Controleert op beschikbaarheid van de naam. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Een verzameling verwijderen. Alleen van toepassing op API-typen: 'mongodb'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Lezen van een verzameling of lijst met alle verzamelingen. Alleen van toepassing op API-typen: 'mongodb'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/throughput/read | Lees de verzamelingsdoorvoer van een. Alleen van toepassing op API-typen: 'mongodb'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/throughput/write | De verzamelingsdoorvoer van een bijwerken. Alleen van toepassing op API-typen: 'mongodb'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Maken of bijwerken van een verzameling. Alleen van toepassing op API-typen: 'mongodb'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Verwijderen van een container. Alleen van toepassing op API-typen: 'sql'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Lezen van een container of lijst met alle containers. Alleen van toepassing op API-typen: 'sql'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/throughput/read | De doorvoer van een container worden gelezen. Alleen van toepassing op API-typen: 'sql'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/throughput/write | Werk de doorvoer van een container. Alleen van toepassing op API-typen: 'sql'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Maken of bijwerken van een container. Alleen van toepassing op API-typen: 'sql'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Een database verwijderen. Alleen van toepassing op API-typen: 'sql', 'mongodb', 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Een grafiek verwijderen. Alleen van toepassing op API-typen: 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Lezen van een grafiek of lijst met alle grafieken. Alleen van toepassing op API-typen: 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/throughput/read | Lees de doorvoer van een grafiek. Alleen van toepassing op API-typen: 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/throughput/write | Werk de doorvoer van een grafiek. Alleen van toepassing op API-typen: 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Maken of bijwerken van een grafiek. Alleen van toepassing op API-typen: 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Lezen van een database of lijst met alle databases. Alleen van toepassing op API-typen: 'sql', 'mongodb', 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/throughput/read | Lees de doorvoer van een database. Alleen van toepassing op API-typen: 'sql', 'mongodb', 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/throughput/write | De doorvoer van een database bijwerken. Alleen van toepassing op API-typen: 'sql', 'mongodb', 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | Een database maken. Alleen van toepassing op API-typen: 'sql', 'mongodb', 'gremlin'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Een keyspace verwijderen. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Een keyspace lezen of lijst met alle keyspaces. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/throughput/read | Lees een keyspace-doorvoer. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/throughput/write | Een keyspace-doorvoer bijwerken. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Een tabel verwijderen. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Lezen van een tabel of lijst met alle tabellen. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/throughput/read | Lees de doorvoer van een tabel. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/throughput/write | De doorvoer van een tabel bijwerken. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Maken of bijwerken van een tabel. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Maak een keyspace. Alleen van toepassing op API-typen: 'cassandra'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Een tabel verwijderen. Alleen van toepassing op API-typen: 'tabel'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Lezen van een tabel of lijst met alle tabellen. Alleen van toepassing op API-typen: 'tabel'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/throughput/read | Lees de doorvoer van een tabel. Alleen van toepassing op API-typen: 'tabel'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/throughput/write | De doorvoer van een tabel bijwerken. Alleen van toepassing op API-typen: 'tabel'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Maken of bijwerken van een tabel. Alleen van toepassing op API-typen: 'tabel'. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/backup/action | Een aanvraag indient bij de back-up configureren |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Resourcegroep wijzigen van een databaseaccount |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Hiermee wordt de verzameling gelezen metrische definities. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | De verzameling metrische gegevens worden gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Database-account partitie belangrijke niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Database-account partitie niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Database-account partities in een verzameling lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Databaseaccount op het gebruik van partitie lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Hiermee leest u het gebruik van de verzameling. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | De database gelezen metrische definities |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Hiermee leest u de metrische databasegegevens. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Hiermee leest u het gebruik van de database. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/delete | Hiermee verwijdert u de database-accounts. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Failover-prioriteiten van de regio's van een databaseaccount wijzigen. Dit wordt gebruikt om handmatige failover uitvoeren |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | De verbindingsreeksen voor het account van een database ophalen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Een lijst met sleutels voor een databaseaccount |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Account metrische definities van de database gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/metrics/read | Hiermee leest u metrische gegevens voor de database. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Offline een regio van een databaseaccount.  |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Online een regio van een databaseaccount. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Status van de asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Latentie van metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/read | Percentielen van de replicatielatentie lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Metrieken voor latentie voor een bepaalde regio voor de bron- en lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Metrieken voor latentie voor een specifieke doelregio lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Een persoonlijke eindpunt verbinding proxy van de Database-Account verwijderen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/operationResults/read | Status van privateEndpointConnectionProxies asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Een persoonlijke eindpunt verbinding proxy van de Database-Account lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Een persoonlijke eindpunt verbinding proxy van de Database-Account valideren |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Maken of bijwerken van een persoonlijke eindpunt verbinding proxy van de Database-Account |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/read | Leest een databaseaccount. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Leest de database alleen-lezen-accountsleutels. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Leest de database alleen-lezen-accountsleutels. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Sleutels van een databaseaccount draaien |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | De regionale verzameling metrische gegevens worden gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Regionale database partitie sleutel niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Regionale database account partitie niveau metrische gegevens lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Regionale database account partities in een verzameling lezen |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Hiermee leest u de regio en database metrische gegevens. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/restore/action | Een restore-aanvraag verzenden |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/usages/read | Het gebruik van de account van de database gelezen. |
> | Bewerking | Microsoft.DocumentDB/databaseAccounts/write | Een database-accounts worden bijgewerkt. |
> | Bewerking | Microsoft.DocumentDB/locations/asyncoperations/read | Status van asynchrone bewerkingen leest |
> | Bewerking | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.DocumentDB meldt dat VirtualNetwork of Subnet wordt verwijderd |
> | Bewerking | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Status van deleteVirtualNetworkOrSubnets asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/operationResults/read | Status van de asynchrone bewerking lezen |
> | Bewerking | Microsoft.DocumentDB/operations/read | Lees-bewerkingen die beschikbaar zijn voor de Microsoft-DocumentDB  |
> | Bewerking | Microsoft.DocumentDB/register/action |  Registreer de resourceprovider Microsoft DocumentDB voor het abonnement |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.DomainRegistration/checkDomainAvailability/Action | Controleer of een domein beschikbaar voor aankoop is |
> | Bewerking | Microsoft.DomainRegistration/domains/Delete | Een bestaand domein verwijdert. |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Eigenaar-id verwijderen |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Lijst met eigenaar-id 's |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Eigenaar-id ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Maken of bijwerken van id |
> | Bewerking | Microsoft.DomainRegistration/domains/operationresults/Read | Een domein-bewerking ophalen |
> | Bewerking | Microsoft.DomainRegistration/domains/Read | De lijst met domeinen |
> | Bewerking | Microsoft.DomainRegistration/domains/Read | Haal domein |
> | Bewerking | Microsoft.DomainRegistration/domains/renew/Action | Vernieuwen van een bestaand domein. |
> | Bewerking | Microsoft.DomainRegistration/domains/Write | Een nieuw domein toevoegen of een bestaande bijgewerkt |
> | Bewerking | Microsoft.DomainRegistration/generateSsoRequest/Action | Genereert een aanvraag voor het beheercentrum domein aan te melden. |
> | Bewerking | Microsoft.DomainRegistration/listDomainRecommendations/Action | Ophalen van de lijst met domein aanbevelingen op basis van trefwoorden |
> | Bewerking | Microsoft.DomainRegistration/operations/Read | Lijst van alle bewerkingen van app service-domeinregistratie |
> | Bewerking | Microsoft.DomainRegistration/register/action | Registreer de resourceprovider Microsoft Domains voor het abonnement |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Overeenkomst-actie |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/Read | Toplevel domeinen ophalen |
> | Bewerking | Microsoft.DomainRegistration/topLevelDomains/Read | Haal toplevel domein |
> | Bewerking | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Aankoop domeinobject valideren zonder deze te verzenden |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.EventGrid/domains/delete | Een domein verwijderen |
> | Bewerking | Microsoft.EventGrid/domains/listKeys/action | Een lijst met sleutels voor een domein |
> | Bewerking | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor domeinen |
> | Bewerking | Microsoft.EventGrid/domains/read | Lezen van een domein |
> | Bewerking | Microsoft.EventGrid/domains/regenerateKey/action | Opnieuw genereren van de sleutel voor een domein |
> | Bewerking | Microsoft.EventGrid/domains/topics/read | Lees het onderwerp van een domein |
> | Bewerking | Microsoft.EventGrid/domains/write | Maken of bijwerken van een domein |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/delete | Een eventSubscription verwijderen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Volledige url voor het gebeurtenisabonnement ophalen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Met deze eigenschap wordt de diagnostische instelling voor gebeurtenisabonnementen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor gebeurtenisabonnementen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor eventSubscriptions |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/read | Een eventSubscription lezen |
> | Bewerking | Microsoft.EventGrid/eventSubscriptions/write | Maken of bijwerken van een eventSubscription |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor onderwerpen opgehaald |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/extensionTopics/read | Lees een extensionTopic. |
> | Bewerking | Microsoft.EventGrid/locations/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen |
> | Bewerking | Microsoft.EventGrid/locations/operationResults/read | Het resultaat van een regionale bewerking lezen |
> | Bewerking | Microsoft.EventGrid/locations/operationsStatus/read | De status van een regionale bewerking lezen |
> | Bewerking | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen door topictype |
> | Bewerking | Microsoft.EventGrid/operationResults/read | Het resultaat van een bewerking lezen |
> | Bewerking | Microsoft.EventGrid/operations/read | EventGrid lijstbewerkingen. |
> | Bewerking | Microsoft.EventGrid/operationsStatus/read | De status van een bewerking lezen |
> | Bewerking | Microsoft.EventGrid/register/action | Hiermee wordt het abonnement voor de resourceprovider EventGrid geregistreerd. |
> | Bewerking | Microsoft.EventGrid/topics/delete | Een onderwerp verwijderen |
> | Bewerking | Microsoft.EventGrid/topics/listKeys/action | Een lijst met sleutels voor een onderwerp |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor onderwerpen opgehaald |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor onderwerpen |
> | Bewerking | Microsoft.EventGrid/topics/read | Een onderwerp lezen |
> | Bewerking | Microsoft.EventGrid/topics/regenerateKey/action | Opnieuw genereren van de sleutel voor een onderwerp |
> | Bewerking | Microsoft.EventGrid/topics/write | Maken of bijwerken van een onderwerp |
> | Bewerking | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Lijst met algemene gebeurtenisabonnementen op Onderwerptype |
> | Bewerking | Microsoft.EventGrid/topictypes/eventtypes/read | Eigenschap eventtypes ondersteund door een topictype lezen |
> | Bewerking | Microsoft.EventGrid/topictypes/read | Een topictype lezen |
> | Bewerking | Microsoft.EventGrid/unregister/action | De registratie van het abonnement voor de resourceprovider EventGrid. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.EventHub/checkNameAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.EventHub/checkNamespaceAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. Deze API is gedeprecieerd in plaats daarvan gebruik CheckNameAvailability. |
> | Bewerking | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Lijst met metrische clustergegevens beschrijvingen van resources |
> | Bewerking | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Hiermee verwijdert u de VNet-regels voor EventHub-resourceprovider voor het opgegeven VNet |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is afgeschaft. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/delete | Namespace verificatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | De verbindingsreeks naar de Namespace ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/read | De lijst met verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.EventHub/namespaces/authorizationRules/write | Een Namespace-verificatieregels maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/namespaces/Delete | Namespace-Resource verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Haalt de autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Disaster Recovery primaire Namespace van verificatieregels ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Beschikbaarheid van de naamruimtealias in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. Met deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en het naamruimtealias om te verwijzen naar de secundaire naamruimte. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte opgehaald. |
> | Bewerking | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Hiermee of bijwerken van de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Bewerking voor het bijwerken van EventHub. Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. Autorisatieregels. Gebruik een PUT-aanroep om bij te werken autorisatieregel. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Bewerking voor het verwijderen van EventHub-autorisatieregels |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | De verbindingsreeks naar Event hub ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  De lijst van de EventHub-verificatieregels ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Regels voor Event hub maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Bewerking ConsumerGroup Resource verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Lijst met beschrijvingen van resources voor ConsumerGroup ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Maken of bijwerken ConsumerGroup eigenschappen. |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/Delete | Bewerking voor het verwijderen van EventHub-Resource |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/read | Lijst met beschrijvingen van resources voor EventHub |
> | Bewerking | Microsoft.EventHub/namespaces/eventhubs/write | Maken of bijwerken EventHub-eigenschappen. |
> | Bewerking | Microsoft.EventHub/namespaces/ipFilterRules/delete | IP-Filter Resource verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/ipFilterRules/read | Ophalen van IP-Filter |
> | Bewerking | Microsoft.EventHub/namespaces/ipFilterRules/write | IP-Filter Resource maken |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | Berichten ontvangen |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | Berichten verzenden |
> | Bewerking | Microsoft.EventHub/namespaces/messagingPlan/read | Hiermee wordt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.EventHub/namespaces/messagingPlan/write | Werkt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.EventHub/namespaces/networkrulesets/delete | VNET-Regelresource verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/networkrulesets/read | NetworkRuleSet Resource opgehaald |
> | Bewerking | Microsoft.EventHub/namespaces/networkrulesets/write | Resource voor VNET-regel maken |
> | Bewerking | Microsoft.EventHub/namespaces/operationresults/read | De status van Namespace-bewerking ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Lijst met beschrijvingen van resources voor Namespace diagnostische instellingen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met beschrijvingen van resources voor Namespace diagnostische instellingen |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met Namespace logboeken beschrijvingen van resources |
> | Bewerking | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met metrische gegevens over Namespace beschrijvingen van resources |
> | Bewerking | Microsoft.EventHub/namespaces/read | De lijst met de resourcebeschrijving Namespace ophalen |
> | Bewerking | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | VNET-Regelresource verwijderen |
> | Bewerking | Microsoft.EventHub/namespaces/virtualNetworkRules/read | VNET-Regelresource opgehaald |
> | Bewerking | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Resource voor VNET-regel maken |
> | Bewerking | Microsoft.EventHub/namespaces/write | Maak een Namespace-Resource en de eigenschappen ervan bijwerken. Labels en de capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.EventHub/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.EventHub/register/action | Hiermee wordt het abonnement voor de EventHub-resourceprovider geregistreerd en wordt het maken van EventHub-resources |
> | Bewerking | Microsoft.EventHub/sku/read | Lijst met beschrijvingen van resources voor Sku ophalen |
> | Bewerking | Microsoft.EventHub/sku/regions/read | Lijst met beschrijvingen van resources voor SkuRegions ophalen |
> | Bewerking | Microsoft.EventHub/unregister/action | Hiermee wordt de EventHub-Resourceprovider geregistreerd |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Features/features/read | Hiermee haalt u de functies van een abonnement. |
> | Bewerking | Microsoft.Features/operations/read | Hiermee haalt u de lijst met bewerkingen. |
> | Bewerking | Microsoft.Features/providers/features/read | Hiermee haalt de functie van een abonnement in een opgegeven resourceprovider. |
> | Bewerking | Microsoft.Features/providers/features/register/action | De functie voor een abonnement registreert in een opgegeven resourceprovider. |
> | Bewerking | Microsoft.Features/providers/features/unregister/action | De registratie van de functie voor een abonnement in een opgegeven resourceprovider. |
> | Bewerking | Microsoft.Features/register/action | Hiermee registreert u de functie van een abonnement. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Get Gast configuration toewijzing. |
> | Bewerking | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Get Gast configuratierapport toewijzing. |
> | Bewerking | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Nieuwe toewijzing van Gast-configuratie maken. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.HDInsight/clusters/applications/delete | Toepassing voor HDInsight-Cluster verwijderen |
> | Bewerking | Microsoft.HDInsight/clusters/applications/read | Toepassing voor HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/applications/write | Toepassing voor HDInsight-Cluster maken of bijwerken |
> | Bewerking | Microsoft.HDInsight/clusters/changerdpsetting/action | RDP-instelling voor HDInsight-Cluster wijzigen |
> | Bewerking | Microsoft.HDInsight/clusters/configurations/action | Configuratie van het HDInsight-Cluster bijwerken |
> | Bewerking | Microsoft.HDInsight/clusters/configurations/action | Configuraties van clusters op HDInsight ophalen |
> | Bewerking | Microsoft.HDInsight/clusters/configurations/read | Configuraties van clusters op HDInsight ophalen |
> | Bewerking | Microsoft.HDInsight/clusters/delete | Een HDInsight-Cluster verwijderen |
> | Bewerking | Microsoft.HDInsight/clusters/getGatewaySettings/action | Gatewayinstellingen voor HDInsight-Cluster ophalen |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Met deze eigenschap wordt de diagnostische instelling voor de resource HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/read | Meer informatie over HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/roles/resize/action | Grootte van een HDInsight-Cluster wijzigen |
> | Bewerking | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Gatewayinstellingen bijwerken voor HDInsight-Cluster |
> | Bewerking | Microsoft.HDInsight/clusters/write | HDInsight-Cluster maken of bijwerken |
> | Bewerking | Microsoft.HDInsight/locations/capabilities/read | Mogelijkheden voor het abonnement ophalen |
> | Bewerking | Microsoft.HDInsight/locations/checkNameAvailability/read | De naam van de beschikbaarheid controleren |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ImportExport/jobs/delete | Hiermee verwijdert u een bestaande taak. |
> | Bewerking | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Hiermee haalt u de BitLocker-sleutels voor de gespecificeerde taak. |
> | Bewerking | Microsoft.ImportExport/jobs/read | Hiermee haalt u de eigenschappen voor de opgegeven taak of de lijst met taken geretourneerd. |
> | Bewerking | Microsoft.ImportExport/jobs/write | Hiermee maakt u een taak met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de gespecificeerde taak. |
> | Bewerking | Microsoft.ImportExport/locations/read | Hiermee haalt u de eigenschappen voor de opgegeven locatie of de lijst met locaties geretourneerd. |
> | Bewerking | Microsoft.ImportExport/register/action | Hiermee wordt het abonnement voor de import/export-resourceprovider geregistreerd en wordt het maken van import/export-taken. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Insights/ActionGroups/Delete | Een actiegroep verwijderen |
> | Bewerking | Microsoft.Insights/ActionGroups/Read | Een actiegroep lezen |
> | Bewerking | Microsoft.Insights/ActionGroups/Write | Maken of bijwerken van een actiegroep |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Activiteit Log waarschuwing geactiveerd |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Delete | Waarschuwing voor een activiteitenlogboek verwijderen |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Read | Waarschuwing voor een activiteitenlogboek lezen |
> | Bewerking | Microsoft.Insights/ActivityLogAlerts/Write | Maken of bijwerken van een waarschuwing voor activiteitenlogboek |
> | Bewerking | Microsoft.Insights/AlertRules/Activated/Action | Klassieke metrische waarschuwing geactiveerd |
> | Bewerking | Microsoft.Insights/AlertRules/Delete | Een klassieke metrische waarschuwing verwijderen |
> | Bewerking | Microsoft.Insights/AlertRules/Incidents/Read | Een klassieke metrische waarschuwing incident lezen |
> | Bewerking | Microsoft.Insights/AlertRules/Read | Een klassieke waarschuwing voor metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/AlertRules/Resolved/Action | Klassieke metrische waarschuwing opgelost |
> | Bewerking | Microsoft.Insights/AlertRules/Throttled/Action | Klassieke metrische waarschuwingsregel wordt beperkt |
> | Bewerking | Microsoft.Insights/AlertRules/Write | Maken of bijwerken van een klassieke metrische waarschuwing |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Delete | Een instelling voor automatisch schalen verwijderen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | De diagnostische instelling van een resource lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Maken of bijwerken van de diagnostische instelling van een resource |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Logboekdefinities lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Read | Een instelling voor automatisch schalen lezen |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Automatisch schalen omlaag schalen gestart |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Automatisch schalen omlaag schalen voltooid |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Automatisch schalen opschalen gestart |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Automatisch schalen omhoog schalen is voltooid |
> | Bewerking | Microsoft.Insights/AutoscaleSettings/Write | Maken of bijwerken van een instelling voor automatisch schalen |
> | Bewerking | Microsoft.Insights/Baseline/Read | Lezen van een metrische basislijn (preview) |
> | Bewerking | Microsoft.Insights/CalculateBaseline/Read | Berekenen van de basislijn voor metrische waarden (preview) |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Delete | Een analyse-item van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Read | Een analyse-item van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/AnalyticsItems/Write | Een analyse-item van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Action | Actie voor Application Insights analytics tabel |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Delete | Verwijderen van een Application Insights analysetabelschema |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Read | Een Application Insights lezen analysetabelschema |
> | Bewerking | Microsoft.Insights/Components/AnalyticsTables/Write | Een Application Insights schrijven analysetabelschema |
> | Bewerking | Microsoft.Insights/Components/Annotations/Delete | Een Application Insights-aantekening verwijderen |
> | Bewerking | Microsoft.Insights/Components/Annotations/Read | Een Application Insights-aantekening lezen |
> | Bewerking | Microsoft.Insights/Components/Annotations/Write | Een Application Insights-aantekening schrijven |
> | Bewerking | Microsoft.Insights/Components/Api/Read | Lezen van gegevens van Application Insights-onderdeel API |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Action | Een Application Insights-API-sleutel genereren |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Delete | Een Application Insights-API-sleutel wordt verwijderd |
> | Bewerking | Microsoft.Insights/Components/ApiKeys/Read | Een Application Insights-API-sleutel lezen |
> | Bewerking | Microsoft.Insights/Components/BillingPlanForComponent/Read | Een abonnement van Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Huidige factureringsfuncties voor Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Huidige factureringsfuncties voor Application Insights-onderdeel schrijven |
> | Bewerking | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Een configuratie voor ALM-integratie van Application Insights standaard lezen |
> | Bewerking | Microsoft.Insights/Components/Delete | De configuratie van een application insights-onderdeel verwijderen |
> | Bewerking | Microsoft.Insights/Components/Events/Read | Logboeken ophalen uit de Application Insights met behulp van OData-queryindeling |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Action | Exportinstellingenactie voor Application Insights |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Delete | Exportinstellingen van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Read | Exportinstellingen van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/ExportConfiguration/Write | Exportinstellingen van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/ExtendQueries/Read | Uitgebreide queryresultaten van Application Insights lezen-onderdeel |
> | Bewerking | Microsoft.Insights/Components/Favorites/Delete | Een Application Insights-favoriet verwijderen |
> | Bewerking | Microsoft.Insights/Components/Favorites/Read | Een Application Insights-favoriet lezen |
> | Bewerking | Microsoft.Insights/Components/Favorites/Write | Een Application Insights-favoriet schrijven |
> | Bewerking | Microsoft.Insights/Components/FeatureCapabilities/Read | Functiemogelijkheden van Application Insights lezen onderdeel |
> | Bewerking | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Application Insights lezen beschikbare factureringsfuncties onderdeel |
> | Bewerking | Microsoft.Insights/Components/GetToken/Read | Een token van Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/MetricDefinitions/Read | Metrische definities van Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Metrics/Read | Metrische gegevens voor het onderdeel van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/Move/Action | Een Application Insights-onderdeel verplaatsen naar een andere resourcegroep of abonnement |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Een persoonlijk analyse-item van Application Insights verwijderen |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Read | Een persoonlijk analyse-item van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/MyAnalyticsItems/Write | Een persoonlijk analyse-item van Application Insights schrijven |
> | Bewerking | Microsoft.Insights/Components/MyFavorites/Read | Een persoonlijke favoriet van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/Operations/Read | Status van langlopende bewerkingen in Application Insights ophalen |
> | Bewerking | Microsoft.Insights/Components/PricingPlans/Read | Het prijsplan van een Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/PricingPlans/Write | Het prijsplan van een Application Insights-onderdeel schrijven |
> | Bewerking | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Proactive detection-configuratie van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Schrijven van Application Insights proactive detection-configuratie |
> | Bewerking | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/Components/Purge/Action | Opschonen van gegevens uit Application Insights |
> | Bewerking | Microsoft.Insights/Components/Query/Read | Query's uitvoeren op Application Insights-logboekbestanden |
> | Bewerking | Microsoft.Insights/Components/QuotaStatus/Read | Quotumstatus van Application Insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/Read | De configuratie van een application insights-onderdeel lezen |
> | Bewerking | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Webtestlocaties van Application Insights lezen |
> | Bewerking | Microsoft.Insights/Components/Webtests/Read | Configuratie voor een webtest lezen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Delete | De configuratie van een Application Insights ALM-integratie verwijderen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Read | De configuratie van een Application Insights ALM-integratie lezen |
> | Bewerking | Microsoft.Insights/Components/WorkItemConfigs/Write | Schrijven van een configuratie van Application Insights ALM-integratie |
> | Bewerking | Microsoft.Insights/Components/Write | Schrijven naar de configuratie van een application insights-onderdeel |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Delete | De diagnostische instelling van een resource verwijderen |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Read | De diagnostische instelling van een resource lezen |
> | Bewerking | Microsoft.Insights/DiagnosticSettings/Write | Maken of bijwerken van de diagnostische instelling van een resource |
> | Bewerking | Microsoft.Insights/EventCategories/Read | Beschikbare categorieën voor de gebeurtenis activiteitenlogboek lezen |
> | Bewerking | Microsoft.Insights/eventtypes/digestevents/Read | Beheergebeurtenistype lezen |
> | Bewerking | Microsoft.Insights/eventtypes/values/Read | Gebeurtenissen in activiteitenlogboeken lezen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Een diagnostische instelling voor netwerk stroom logboek niet verwijderen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Een netwerk stroom log diagnostische instelling lezen |
> | Bewerking | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Maken of bijwerken van een diagnostische instelling voor netwerk stroom log |
> | Bewerking | Microsoft.Insights/ListMigrationDate/Action | Migratiedatum van back-abonnement |
> | Bewerking | Microsoft.Insights/ListMigrationDate/Read | Migratiedatum van abonnement terughalen |
> | Bewerking | Microsoft.Insights/LogDefinitions/Read | Logboekdefinities lezen |
> | Bewerking | Microsoft.Insights/LogProfiles/Delete | Een activiteitenlogboek logboekprofiel verwijderen |
> | Bewerking | Microsoft.Insights/LogProfiles/Read | Een activiteitenlogboek logboekprofiel lezen |
> | Bewerking | Microsoft.Insights/LogProfiles/Write | Maken of bijwerken van een activiteitenlogboek logboekprofiel |
> | Bewerking | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Gegevens lezen uit de tabel ADAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/ADReplicationResult/Read | Gegevens lezen uit de tabel ADReplicationResult |
> | Bewerking | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Gegevens lezen uit de tabel ADSecurityAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/Alert/Read | Gegevens lezen uit de tabel met waarschuwingen |
> | Bewerking | Microsoft.Insights/Logs/AlertHistory/Read | Gegevens lezen uit de tabel AlertHistory |
> | Bewerking | Microsoft.Insights/Logs/ApplicationInsights/Read | Gegevens lezen uit de Application Insights-tabel |
> | Bewerking | Microsoft.Insights/Logs/AzureActivity/Read | Gegevens lezen uit de tabel AzureActivity |
> | Bewerking | Microsoft.Insights/Logs/AzureMetrics/Read | Gegevens lezen uit de tabel AzureMetrics |
> | Bewerking | Microsoft.Insights/Logs/BoundPort/Read | Gegevens lezen uit de tabel BoundPort |
> | Bewerking | Microsoft.Insights/Logs/CommonSecurityLog/Read | Gegevens lezen uit de tabel CommonSecurityLog |
> | Bewerking | Microsoft.Insights/Logs/ComputerGroup/Read | Gegevens lezen uit de tabel ComputerGroup |
> | Bewerking | Microsoft.Insights/Logs/ConfigurationChange/Read | Gegevens lezen uit de tabel ConfigurationChange |
> | Bewerking | Microsoft.Insights/Logs/ConfigurationData/Read | Gegevens lezen uit de tabel ConfigurationData |
> | Bewerking | Microsoft.Insights/Logs/ContainerImageInventory/Read | Gegevens lezen uit de tabel ContainerImageInventory |
> | Bewerking | Microsoft.Insights/Logs/ContainerInventory/Read | Gegevens lezen uit de tabel ContainerInventory |
> | Bewerking | Microsoft.Insights/Logs/ContainerLog/Read | Gegevens lezen uit de tabel ContainerLog |
> | Bewerking | Microsoft.Insights/Logs/ContainerServiceLog/Read | Gegevens lezen uit de tabel ContainerServiceLog |
> | Bewerking | Microsoft.Insights/Logs/DeviceAppCrash/Read | Gegevens lezen uit de tabel DeviceAppCrash |
> | Bewerking | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Gegevens lezen uit de tabel DeviceAppLaunch |
> | Bewerking | Microsoft.Insights/Logs/DeviceCalendar/Read | Gegevens lezen uit de tabel DeviceCalendar |
> | Bewerking | Microsoft.Insights/Logs/DeviceCleanup/Read | Gegevens lezen uit de tabel DeviceCleanup |
> | Bewerking | Microsoft.Insights/Logs/DeviceConnectSession/Read | Gegevens lezen uit de tabel DeviceConnectSession |
> | Bewerking | Microsoft.Insights/Logs/DeviceEtw/Read | Gegevens lezen uit de tabel DeviceEtw |
> | Bewerking | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Gegevens lezen uit de tabel DeviceHardwareHealth |
> | Bewerking | Microsoft.Insights/Logs/DeviceHealth/Read | Gegevens lezen uit de tabel de apparaatstatus |
> | Bewerking | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Gegevens lezen uit de tabel DeviceHeartbeat |
> | Bewerking | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Gegevens lezen uit de tabel DeviceSkypeHeartbeat |
> | Bewerking | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Gegevens lezen uit de tabel DeviceSkypeSignIn |
> | Bewerking | Microsoft.Insights/Logs/DeviceSleepState/Read | Gegevens lezen uit de tabel DeviceSleepState |
> | Bewerking | Microsoft.Insights/Logs/DHAppFailure/Read | Gegevens lezen uit de tabel DHAppFailure |
> | Bewerking | Microsoft.Insights/Logs/DHAppReliability/Read | Gegevens lezen uit de tabel DHAppReliability |
> | Bewerking | Microsoft.Insights/Logs/DHDriverReliability/Read | Gegevens lezen uit de tabel DHDriverReliability |
> | Bewerking | Microsoft.Insights/Logs/DHLogonFailures/Read | Gegevens lezen uit de tabel DHLogonFailures |
> | Bewerking | Microsoft.Insights/Logs/DHLogonMetrics/Read | Gegevens lezen uit de tabel DHLogonMetrics |
> | Bewerking | Microsoft.Insights/Logs/DHOSCrashData/Read | Gegevens lezen uit de tabel DHOSCrashData |
> | Bewerking | Microsoft.Insights/Logs/DHOSReliability/Read | Gegevens lezen uit de tabel DHOSReliability |
> | Bewerking | Microsoft.Insights/Logs/DHWipAppLearning/Read | Gegevens lezen uit de tabel DHWipAppLearning |
> | Bewerking | Microsoft.Insights/Logs/DnsEvents/Read | Gegevens lezen uit de tabel DnsEvents |
> | Bewerking | Microsoft.Insights/Logs/DnsInventory/Read | Gegevens lezen uit de tabel DnsInventory |
> | Bewerking | Microsoft.Insights/Logs/ETWEvent/Read | Gegevens lezen uit de tabel ETWEvent |
> | Bewerking | Microsoft.Insights/Logs/Event/Read | Gegevens lezen uit de tabel gebeurtenis |
> | Bewerking | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Gegevens lezen uit de tabel ExchangeAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel ExchangeOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/Heartbeat/Read | Lezen van gegevens van de Heartbeat-tabel |
> | Bewerking | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Gegevens lezen uit de tabel IISAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/InboundConnection/Read | Gegevens lezen uit de tabel InboundConnection |
> | Bewerking | Microsoft.Insights/Logs/KubeNodeInventory/Read | Gegevens lezen uit de tabel KubeNodeInventory |
> | Bewerking | Microsoft.Insights/Logs/KubePodInventory/Read | Gegevens lezen uit de tabel KubePodInventory |
> | Bewerking | Microsoft.Insights/Logs/LinuxAuditLog/Read | Gegevens lezen uit de tabel LinuxAuditLog |
> | Bewerking | Microsoft.Insights/Logs/MAApplication/Read | Gegevens lezen uit de tabel MAApplication |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationHealth/Read | Gegevens lezen uit de tabel MAApplicationHealth |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Gegevens lezen uit de tabel MAApplicationHealthAlternativeVersions |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | Gegevens lezen uit de tabel MAApplicationHealthIssues |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationInstance/Read | Gegevens lezen uit de tabel MAApplicationInstance |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Gegevens lezen uit de tabel MAApplicationInstanceReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Gegevens lezen uit de tabel MAApplicationReadiness |
> | Bewerking | Microsoft.Insights/Logs/MADeploymentPlan/Read | Gegevens lezen uit de tabel MADeploymentPlan |
> | Bewerking | Microsoft.Insights/Logs/MADevice/Read | Gegevens lezen uit de tabel MADevice |
> | Bewerking | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Gegevens lezen uit de tabel MADevicePnPHealth |
> | Bewerking | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Gegevens lezen uit de tabel MADevicePnPHealthAlternativeVersions |
> | Bewerking | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Gegevens lezen uit de tabel MADevicePnPHealthIssues |
> | Bewerking | Microsoft.Insights/Logs/MADeviceReadiness/Read | Gegevens lezen uit de tabel MADeviceReadiness |
> | Bewerking | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Gegevens lezen uit de tabel MADriverInstanceReadiness |
> | Bewerking | Microsoft.Insights/Logs/MADriverReadiness/Read | Gegevens lezen uit de tabel MADriverReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddin/Read | Gegevens lezen uit de tabel MAOfficeAddin |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Gegevens lezen uit de tabel MAOfficeAddinHealth |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Gegevens lezen uit de tabel MAOfficeAddinHealthIssues |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Gegevens lezen uit de tabel MAOfficeAddinInstance |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Gegevens lezen uit de tabel MAOfficeAddinInstanceReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Gegevens lezen uit de tabel MAOfficeAddinReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeApp/Read | Gegevens lezen uit de tabel MAOfficeApp |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Gegevens lezen uit de tabel MAOfficeAppHealth |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Gegevens lezen uit de tabel MAOfficeAppInstance |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Gegevens lezen uit de tabel MAOfficeAppReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Gegevens lezen uit de tabel MAOfficeBuildInfo |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessment |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessmentDailyCounts |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Gegevens lezen uit de tabel MAOfficeDeploymentStatus |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Gegevens lezen uit de tabel MAOfficeMacroHealth |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Gegevens lezen uit de tabel MAOfficeMacroHealthIssues |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Gegevens lezen uit de tabel MAOfficeMacroIssueInstanceReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Gegevens lezen uit de tabel MAOfficeMacroIssueReadiness |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Gegevens lezen uit de tabel MAOfficeMacroSummary |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeSuite/Read | Gegevens lezen uit de tabel MAOfficeSuite |
> | Bewerking | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Gegevens lezen uit de tabel MAOfficeSuiteInstance |
> | Bewerking | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Gegevens lezen uit de tabel MAProposedPilotDevices |
> | Bewerking | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Gegevens lezen uit de tabel MAWindowsBuildInfo |
> | Bewerking | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessment |
> | Bewerking | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessmentDailyCounts |
> | Bewerking | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Gegevens lezen uit de tabel MAWindowsDeploymentStatus |
> | Bewerking | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Gegevens lezen uit de tabel MAWindowsSysReqInstanceReadiness |
> | Bewerking | Microsoft.Insights/Logs/NetworkMonitoring/Read | Gegevens lezen uit de tabel NetworkMonitoring |
> | Bewerking | Microsoft.Insights/Logs/OfficeActivity/Read | Gegevens lezen uit de tabel OfficeActivity |
> | Bewerking | Microsoft.Insights/Logs/Operation/Read | Gegevens lezen uit de tabel |
> | Bewerking | Microsoft.Insights/Logs/OutboundConnection/Read | Gegevens lezen uit de tabel OutboundConnection |
> | Bewerking | Microsoft.Insights/Logs/Perf/Read | Gegevens lezen uit de tabel voor prestaties |
> | Bewerking | Microsoft.Insights/Logs/ProtectionStatus/Read | Gegevens lezen uit de tabel ProtectionStatus |
> | Bewerking | Microsoft.Insights/Logs/Read | Lezen van gegevens uit alle uw logboeken |
> | Bewerking | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Gegevens lezen uit de tabel ReservedAzureCommonFields |
> | Bewerking | Microsoft.Insights/Logs/ReservedCommonFields/Read | Gegevens lezen uit de tabel ReservedCommonFields |
> | Bewerking | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Gegevens lezen uit de tabel SCCMAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | Gegevens lezen uit de tabel SCOMAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SecurityAlert/Read | Gegevens lezen uit de tabel SecurityAlert |
> | Bewerking | Microsoft.Insights/Logs/SecurityBaseline/Read | Gegevens lezen uit de tabel SecurityBaseline |
> | Bewerking | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Gegevens lezen uit de tabel SecurityBaselineSummary |
> | Bewerking | Microsoft.Insights/Logs/SecurityDetection/Read | Gegevens lezen uit de tabel SecurityDetection |
> | Bewerking | Microsoft.Insights/Logs/SecurityEvent/Read | Gegevens lezen uit de tabel SecurityEvent |
> | Bewerking | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Gegevens lezen uit de tabel ServiceFabricOperationalEvent |
> | Bewerking | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Gegevens lezen uit de tabel ServiceFabricReliableActorEvent |
> | Bewerking | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Gegevens lezen uit de tabel ServiceFabricReliableServiceEvent |
> | Bewerking | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | Gegevens lezen uit de tabel SfBAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel SfBOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Gegevens lezen uit de tabel SharePointOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | Gegevens lezen uit de tabel SPAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Gegevens lezen uit de tabel SQLAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Gegevens lezen uit de tabel SQLQueryPerformance |
> | Bewerking | Microsoft.Insights/Logs/Syslog/Read | Gegevens lezen uit de Syslog-tabel |
> | Bewerking | Microsoft.Insights/Logs/SysmonEvent/Read | Gegevens lezen uit de tabel SysmonEvent |
> | Bewerking | Microsoft.Insights/Logs/UAApp/Read | Gegevens lezen uit de tabel UAApp |
> | Bewerking | Microsoft.Insights/Logs/UAComputer/Read | Gegevens lezen uit de tabel UAComputer |
> | Bewerking | Microsoft.Insights/Logs/UAComputerRank/Read | Gegevens lezen uit de tabel UAComputerRank |
> | Bewerking | Microsoft.Insights/Logs/UADriver/Read | Gegevens lezen uit de tabel UADriver |
> | Bewerking | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Gegevens lezen uit de tabel UADriverProblemCodes |
> | Bewerking | Microsoft.Insights/Logs/UAFeedback/Read | Gegevens lezen uit de tabel UAFeedback |
> | Bewerking | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Gegevens lezen uit de tabel UAHardwareSecurity |
> | Bewerking | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Gegevens lezen uit de tabel UAIESiteDiscovery |
> | Bewerking | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Gegevens lezen uit de tabel UAOfficeAddIn |
> | Bewerking | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Gegevens lezen uit de tabel UAProposedActionPlan |
> | Bewerking | Microsoft.Insights/Logs/UASysReqIssue/Read | Gegevens lezen uit de tabel UASysReqIssue |
> | Bewerking | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Gegevens lezen uit de tabel UAUpgradedComputer |
> | Bewerking | Microsoft.Insights/Logs/Update/Read | Gegevens lezen uit de tabel bijwerken |
> | Bewerking | Microsoft.Insights/Logs/UpdateRunProgress/Read | Gegevens lezen uit de tabel UpdateRunProgress |
> | Bewerking | Microsoft.Insights/Logs/UpdateSummary/Read | Gegevens lezen uit de tabel UpdateSummary |
> | Bewerking | Microsoft.Insights/Logs/Usage/Read | Gegevens lezen uit de tabel Gebruik |
> | Bewerking | Microsoft.Insights/Logs/W3CIISLog/Read | Gegevens lezen uit de tabel W3CIISLog |
> | Bewerking | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Gegevens lezen uit de tabel WaaSDeploymentStatus |
> | Bewerking | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Gegevens lezen uit de tabel WaaSInsiderStatus |
> | Bewerking | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Gegevens lezen uit de tabel WaaSUpdateStatus |
> | Bewerking | Microsoft.Insights/Logs/WDAVStatus/Read | Gegevens lezen uit de tabel WDAVStatus |
> | Bewerking | Microsoft.Insights/Logs/WDAVThreat/Read | Gegevens lezen uit de tabel WDAVThreat |
> | Bewerking | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Gegevens lezen uit de tabel WindowsClientAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/WindowsFirewall/Read | Gegevens lezen uit de tabel WindowsFirewall |
> | Bewerking | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Gegevens lezen uit de tabel WindowsServerAssessmentRecommendation |
> | Bewerking | Microsoft.Insights/Logs/WireData/Read | Gegevens lezen uit de tabel WireData |
> | Bewerking | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Gegevens lezen uit de tabel WUDOAggregatedStatus |
> | Bewerking | Microsoft.Insights/Logs/WUDOStatus/Read | Gegevens lezen uit de tabel WUDOStatus |
> | Bewerking | Microsoft.Insights/MetricAlerts/Delete | Een waarschuwing voor metrische gegevens verwijderen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Read | Een waarschuwing voor metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Status/Read | Status van waarschuwing voor metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/MetricAlerts/Write | Maken of bijwerken van een waarschuwing voor metrische gegevens |
> | Bewerking | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/MetricDefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.Insights/Metricnamespaces/Read | Metrische gegevens lezen-naamruimten |
> | Bewerking | Microsoft.Insights/Metrics/Action | Metrische actie |
> | Bewerking | Microsoft.Insights/Metrics/Microsoft.Insights/Read | De metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/Metrics/providers/Metrics/Read | De metrische gegevens lezen |
> | Bewerking | Microsoft.Insights/Metrics/Read | De metrische gegevens lezen |
> | DataAction | Microsoft.Insights/Metrics/Write | Metrische gegevens schrijven |
> | Bewerking | Microsoft.Insights/MigrateToNewpricingModel/Action | Abonnement migreren naar nieuw prijsmodel |
> | Bewerking | Microsoft.Insights/Operations/Read | Leesbewerkingen |
> | Bewerking | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registreren |
> | Bewerking | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Abonnement terugdraaien naar oude prijsmodel |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Delete | Een geplande queryregel verwijderen |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Read | Een geplande queryregel lezen |
> | Bewerking | Microsoft.Insights/ScheduledQueryRules/Write | Een geplande queryregel schrijven |
> | Bewerking | Microsoft.Insights/Tenants/Register/Action | De Microsoft Insights-provider geïnitialiseerd |
> | Bewerking | Microsoft.Insights/Unregister/Action | De Microsoft Insights-provider registreren |
> | Bewerking | Microsoft.Insights/Webtests/Delete | Configuratie voor een webtest verwijderen |
> | Bewerking | Microsoft.Insights/Webtests/GetToken/Read | Een webtesttoken lezen |
> | Bewerking | Microsoft.Insights/Webtests/MetricDefinitions/Read | Een metrische definities van webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Metrics/Read | De metrische gegevens van een webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Read | Configuratie voor een webtest lezen |
> | Bewerking | Microsoft.Insights/Webtests/Write | Schrijven naar een configuratie voor een webtest |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Intune/diagnosticsettings/delete | Een diagnostische instelling verwijderen |
> | Bewerking | Microsoft.Intune/diagnosticsettings/read | Een diagnostische instelling lezen |
> | Bewerking | Microsoft.Intune/diagnosticsettings/write | Een diagnostische instelling schrijven |
> | Bewerking | Microsoft.Intune/diagnosticsettingscategories/read | De categorieën van een diagnostische instelling lezen |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.IoTCentral/appTemplates/action | Haalt u alle beschikbare sjablonen op Azure IoT Central |
> | Bewerking | Microsoft.IoTCentral/checkNameAvailability/action | Controleert of de naam van een IoT Central-toepassing beschikbaar is |
> | Bewerking | Microsoft.IoTCentral/checkSubdomainAvailability/action | Controleert of een subdomein IoT Central-toepassing beschikbaar is |
> | Bewerking | Microsoft.IoTCentral/IoTApps/delete | Hiermee verwijdert u een IoT Central-toepassingen |
> | Bewerking | Microsoft.IoTCentral/IoTApps/read | Haalt een enkele IoT Central-toepassing |
> | Bewerking | Microsoft.IoTCentral/IoTApps/write | Hiermee maken of bijwerken van een IoT Central-toepassingen |
> | Bewerking | Microsoft.IoTCentral/operations/read | Haalt alle beschikbare bewerkingen op de centrale IoT-toepassingen |
> | Bewerking | Microsoft.IoTCentral/register/action | Het abonnement voor Azure IoT Central resourceprovider registreren |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.IoTSpaces/Graph/delete | Hiermee verwijdert u Microsoft.IoTSpaces Graph-resource |
> | Bewerking | Microsoft.IoTSpaces/Graph/read | Haalt de Microsoft.IoTSpaces Graph-resource (s) |
> | Bewerking | Microsoft.IoTSpaces/Graph/write | Microsoft.IoTSpaces Graph-resource maken |
> | Bewerking | Microsoft.IoTSpaces/register/action | Abonnement voor de resourceprovider Microsoft.IoTSpaces Graph om in te schakelen op het maken van resources registreren |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.KeyVault/checkNameAvailability/read | Controleert of een key vault-naam geldig is en niet wordt gebruikt |
> | Bewerking | Microsoft.KeyVault/deletedVaults/read | De eigenschappen van voorlopig verwijderde sleutelkluizen weergeven |
> | Bewerking | Microsoft.KeyVault/hsmPools/delete | Een HSM-groep verwijderen |
> | Bewerking | Microsoft.KeyVault/hsmPools/joinVault/action | Lid van een sleutelkluis te maken van een HSM-groep |
> | Bewerking | Microsoft.KeyVault/hsmPools/read | De eigenschappen van een HSM-groep weergeven |
> | Bewerking | Microsoft.KeyVault/hsmPools/write | Een nieuwe HSM-pool van update de eigenschappen van een bestaande HSM-groep maken |
> | Bewerking | Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sleutelkluis leegmaken |
> | Bewerking | Microsoft.KeyVault/locations/deletedVaults/read | De eigenschappen van een voorlopig verwijderde sleutelkluis weergeven |
> | Bewerking | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Microsoft.KeyVault waarschuwt dat een virtueel netwerk of subnet wordt verwijderd |
> | Bewerking | Microsoft.KeyVault/locations/operationResults/read | Het resultaat van een langdurige uitvoeringsbewerking controleren |
> | Bewerking | Microsoft.KeyVault/operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.KeyVault-resourceprovider |
> | Bewerking | Microsoft.KeyVault/register/action | Hiermee wordt een abonnement geregistreerd |
> | Bewerking | Microsoft.KeyVault/unregister/action | De registratie van een abonnement |
> | Bewerking | Microsoft.KeyVault/vaults/accessPolicies/write | Een bestaand toegangsbeleid bijwerken door samenvoegen of vervangen, of een nieuw toegangsbeleid toevoegen aan een kluis. |
> | Bewerking | Microsoft.KeyVault/vaults/delete | Een sleutelkluis verwijderen |
> | Bewerking | Microsoft.KeyVault/vaults/deploy/action | Hiermee wordt toegang tot de geheimen in een key vault bij het implementeren van Azure-resources |
> | Bewerking | Microsoft.KeyVault/vaults/eventGridFilters/delete | Microsoft.KeyVault waarschuwt dat een abonnement EventGrid voor Key Vault wordt verwijderd |
> | Bewerking | Microsoft.KeyVault/vaults/eventGridFilters/read | Microsoft.KeyVault waarschuwt dat een abonnement EventGrid voor Key Vault wordt weergegeven |
> | Bewerking | Microsoft.KeyVault/vaults/eventGridFilters/write | Microsoft.KeyVault waarschuwt dat een nieuw abonnement EventGrid voor Key Vault wordt gemaakt |
> | Bewerking | Microsoft.KeyVault/vaults/read | De eigenschappen van een sleutelkluis weergeven |
> | Bewerking | Microsoft.KeyVault/vaults/secrets/read | De eigenschappen van een geheim, maar niet de waarde weergeven |
> | Bewerking | Microsoft.KeyVault/vaults/secrets/write | Een nieuw geheim maken of de waarde van een bestaand geheim bijwerken |
> | Bewerking | Microsoft.KeyVault/vaults/write | Een nieuwe sleutelkluis maken of bijwerken van de eigenschappen van een bestaande sleutelkluis |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Kusto/Clusters/Activate/action | Start het cluster. |
> | Bewerking | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Controleert de beschikbaarheid van de naam van het cluster. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Database-principals toegevoegd. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Controleert de beschikbaarheid van de naam van een bepaald type. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Hiermee verwijdert u de resource van een gegevens-verbindingen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Hiermee wordt een resource van de verbindingen gegevens gelezen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Een resource van de verbindingen gegevens schrijft. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Valideert de gegevensverbinding van de database. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/delete | Hiermee verwijdert u een databaseresource. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Hiermee verwijdert u de resource van een Event Hub-verbindingen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Hiermee leest u de resource van een Event Hub-verbindingen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Schrijft de resource van een Event Hub-verbindingen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Valideert de verbinding met database Event Hub. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Een lijst met database-principals. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/read | Hiermee wordt een databaseresource gelezen. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Hiermee verwijdert u database-principals. |
> | Bewerking | Microsoft.Kusto/Clusters/Databases/write | Een databaseresource schrijft. |
> | Bewerking | Microsoft.Kusto/Clusters/Deactivate/action | Stopt het cluster. |
> | Bewerking | Microsoft.Kusto/Clusters/delete | Hiermee verwijdert u een cluster-bron. |
> | Bewerking | Microsoft.Kusto/Clusters/read | Hiermee leest u een cluster-bron. |
> | Bewerking | Microsoft.Kusto/Clusters/SKUs/read | Hiermee wordt een cluster SKU-resource gelezen. |
> | Bewerking | Microsoft.Kusto/Clusters/Start/action | Start het cluster. |
> | Bewerking | Microsoft.Kusto/Clusters/Stop/action | Stopt het cluster. |
> | Bewerking | Microsoft.Kusto/Clusters/write | Schrijft een cluster-bron. |
> | Bewerking | Microsoft.Kusto/Locations/CheckNameAvailability/action | Controleert de beschikbaarheid van de resource-naam. |
> | Bewerking | Microsoft.Kusto/locations/operationresults/read | Leesbewerkingen bewerkingen resources |
> | Bewerking | Microsoft.Kusto/Operations/read | Leesbewerkingen bewerkingen resources |
> | Bewerking | Microsoft.Kusto/Register/action | Hiermee wordt het abonnement bij de Kusto-Resourceprovider geregistreerd. |
> | Bewerking | Microsoft.Kusto/SKUs/read | Leest een SKU-resource. |
> | Bewerking | Microsoft.Kusto/Unregister/action | De registratie van het abonnement op de Kusto-Resourceprovider. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.LabServices/labAccounts/CreateLab/action | Een lab maken in een lab-account. |
> | Bewerking | Microsoft.LabServices/labAccounts/delete | Lab-accounts verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/galleryImages/delete | Verwijder de galerie met installatiekopieën. |
> | Bewerking | Microsoft.LabServices/labAccounts/galleryImages/read | Galerie met installatiekopieën worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/galleryImages/write | Toevoegen of wijzigen van de galerie met installatiekopieën. |
> | Bewerking | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Informatie over de regionale beschikbaarheid voor elke grootte categorie geconfigureerd onder een lab-account ophalen |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Gebruikers toevoegen aan een lab |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Claims van een willekeurige omgeving voor een gebruiker in een omgevingsinstellingen |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Omgevingsinstelling verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Claims van de omgeving en wijst deze toe aan de gebruiker |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Omgevingen verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Omgevingen lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Hiermee stelt u het wachtwoord van de gebruiker in een omgeving |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Start een omgeving op basis van alle resources binnen de omgeving. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Een omgeving stopt met het stoppen van alle resources binnen de omgeving |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Bepalingen/deprovisions vereiste resources voor een omgeving instellen op basis van de huidige status van de instelling van de testomgeving /. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Omgevingsinstelling lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Hiermee slaat u huidige sjablooninstallatiekopie naar de gedeelde galerie in het lab-account |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Schema's verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Schema's worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Toevoegen of wijzigen van schema's. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Start een sjabloon op basis van alle resources in de sjabloon. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Hiermee stopt een sjabloon door het stoppen van alle resources in de sjabloon. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Toevoegen of wijzigen van de omgevingsinstelling. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/read | Lees labs. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/Register/action | Registreren bij beheerde lab. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/SendEmail/action | E-mailbericht met registratiekoppeling verzenden naar het lab |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/delete | Gebruikers verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/read | Gebruikers worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/users/write | Toevoegen of wijzigen van gebruikers. |
> | Bewerking | Microsoft.LabServices/labAccounts/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.LabServices/labAccounts/read | Lab-accounts worden gelezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Sharedgalleries verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedGalleries/read | Sharedgalleries lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedGalleries/write | Toevoegen of wijzigen van sharedgalleries. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedImages/delete | Sharedimages verwijderen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedImages/read | Sharedimages lezen. |
> | Bewerking | Microsoft.LabServices/labAccounts/sharedImages/write | Toevoegen of wijzigen van sharedimages. |
> | Bewerking | Microsoft.LabServices/labAccounts/write | Toevoegen of wijzigen van lab-accounts. |
> | Bewerking | Microsoft.LabServices/locations/operations/read | Leesbewerkingen. |
> | Bewerking | Microsoft.LabServices/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.LabServices/users/GetEnvironment/action | Haalt de details van de virtuele machine |
> | Bewerking | Microsoft.LabServices/users/GetOperationBatchStatus/action | Status van de batch-bewerking ophalen |
> | Bewerking | Microsoft.LabServices/users/GetOperationStatus/action | Hiermee wordt de status van de langdurige bewerking opgehaald |
> | Bewerking | Microsoft.LabServices/users/GetPersonalPreferences/action | Persoonlijke voorkeuren voor een gebruiker ophalen |
> | Bewerking | Microsoft.LabServices/users/ListAllEnvironments/action | Lijst van alle omgevingen voor de gebruiker |
> | Bewerking | Microsoft.LabServices/users/ListEnvironments/action | Lijst met omgevingen voor de gebruiker |
> | Bewerking | Microsoft.LabServices/users/ListLabs/action | Lijst met labs voor de gebruiker. |
> | Bewerking | Microsoft.LabServices/users/Register/action | Registreren van een gebruiker aan een beheerde lab |
> | Bewerking | Microsoft.LabServices/users/ResetPassword/action | Hiermee stelt u het wachtwoord van de gebruiker in een omgeving |
> | Bewerking | Microsoft.LabServices/users/StartEnvironment/action | Start een omgeving op basis van alle resources binnen de omgeving. |
> | Bewerking | Microsoft.LabServices/users/StopEnvironment/action | Een omgeving stopt met het stoppen van alle resources binnen de omgeving |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | DataAction | Microsoft.LogAnalytics/logs/ADAssessmentRecommendation/read | Gegevens lezen uit de tabel ADAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/ADReplicationResult/read | Gegevens lezen uit de tabel ADReplicationResult |
> | DataAction | Microsoft.LogAnalytics/logs/ADSecurityAssessmentRecommendation/read | Gegevens lezen uit de tabel ADSecurityAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/Alert/read | Gegevens lezen uit de tabel met waarschuwingen |
> | DataAction | Microsoft.LogAnalytics/logs/AlertHistory/read | Gegevens lezen uit de tabel AlertHistory |
> | DataAction | Microsoft.LogAnalytics/logs/AppCenterError/read | Gegevens lezen uit de tabel AppCenterError |
> | DataAction | Microsoft.LogAnalytics/logs/ApplicationInsights/read | Gegevens lezen uit de Application Insights-tabel |
> | DataAction | Microsoft.LogAnalytics/logs/AuditLogs/read | Gegevens lezen uit de tabel AuditLogs |
> | DataAction | Microsoft.LogAnalytics/logs/AzureActivity/read | Gegevens lezen uit de tabel AzureActivity |
> | DataAction | Microsoft.LogAnalytics/logs/AzureMetrics/read | Gegevens lezen uit de tabel AzureMetrics |
> | DataAction | Microsoft.LogAnalytics/logs/BoundPort/read | Gegevens lezen uit de tabel BoundPort |
> | DataAction | Microsoft.LogAnalytics/logs/CommonSecurityLog/read | Gegevens lezen uit de tabel CommonSecurityLog |
> | DataAction | Microsoft.LogAnalytics/logs/ComputerGroup/read | Gegevens lezen uit de tabel ComputerGroup |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationChange/read | Gegevens lezen uit de tabel ConfigurationChange |
> | DataAction | Microsoft.LogAnalytics/logs/ConfigurationData/read | Gegevens lezen uit de tabel ConfigurationData |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerImageInventory/read | Gegevens lezen uit de tabel ContainerImageInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerInventory/read | Gegevens lezen uit de tabel ContainerInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerLog/read | Gegevens lezen uit de tabel ContainerLog |
> | DataAction | Microsoft.LogAnalytics/logs/ContainerServiceLog/read | Gegevens lezen uit de tabel ContainerServiceLog |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppCrash/read | Gegevens lezen uit de tabel DeviceAppCrash |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceAppLaunch/read | Gegevens lezen uit de tabel DeviceAppLaunch |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCalendar/read | Gegevens lezen uit de tabel DeviceCalendar |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceCleanup/read | Gegevens lezen uit de tabel DeviceCleanup |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceConnectSession/read | Gegevens lezen uit de tabel DeviceConnectSession |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceEtw/read | Gegevens lezen uit de tabel DeviceEtw |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHardwareHealth/read | Gegevens lezen uit de tabel DeviceHardwareHealth |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHealth/read | Gegevens lezen uit de tabel de apparaatstatus |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceHeartbeat/read | Gegevens lezen uit de tabel DeviceHeartbeat |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeHeartbeat/read | Gegevens lezen uit de tabel DeviceSkypeHeartbeat |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSkypeSignIn/read | Gegevens lezen uit de tabel DeviceSkypeSignIn |
> | DataAction | Microsoft.LogAnalytics/logs/DeviceSleepState/read | Gegevens lezen uit de tabel DeviceSleepState |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppFailure/read | Gegevens lezen uit de tabel DHAppFailure |
> | DataAction | Microsoft.LogAnalytics/logs/DHAppReliability/read | Gegevens lezen uit de tabel DHAppReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHDriverReliability/read | Gegevens lezen uit de tabel DHDriverReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonFailures/read | Gegevens lezen uit de tabel DHLogonFailures |
> | DataAction | Microsoft.LogAnalytics/logs/DHLogonMetrics/read | Gegevens lezen uit de tabel DHLogonMetrics |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSCrashData/read | Gegevens lezen uit de tabel DHOSCrashData |
> | DataAction | Microsoft.LogAnalytics/logs/DHOSReliability/read | Gegevens lezen uit de tabel DHOSReliability |
> | DataAction | Microsoft.LogAnalytics/logs/DHWipAppLearning/read | Gegevens lezen uit de tabel DHWipAppLearning |
> | DataAction | Microsoft.LogAnalytics/logs/DnsEvents/read | Gegevens lezen uit de tabel DnsEvents |
> | DataAction | Microsoft.LogAnalytics/logs/DnsInventory/read | Gegevens lezen uit de tabel DnsInventory |
> | DataAction | Microsoft.LogAnalytics/logs/ETWEvent/read | Gegevens lezen uit de tabel ETWEvent |
> | DataAction | Microsoft.LogAnalytics/logs/Event/read | Gegevens lezen uit de tabel gebeurtenis |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/ExchangeOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeOnlineAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/Heartbeat/read | Lezen van gegevens van de Heartbeat-tabel |
> | DataAction | Microsoft.LogAnalytics/logs/IISAssessmentRecommendation/read | Gegevens lezen uit de tabel IISAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/InboundConnection/read | Gegevens lezen uit de tabel InboundConnection |
> | DataAction | Microsoft.LogAnalytics/logs/KubeEvents/read | Gegevens lezen uit de tabel KubeEvents |
> | DataAction | Microsoft.LogAnalytics/logs/KubeNodeInventory/read | Gegevens lezen uit de tabel KubeNodeInventory |
> | DataAction | Microsoft.LogAnalytics/logs/KubePodInventory/read | Gegevens lezen uit de tabel KubePodInventory |
> | DataAction | Microsoft.LogAnalytics/logs/KubeServices/read | Gegevens lezen uit de tabel KubeServices |
> | DataAction | Microsoft.LogAnalytics/logs/LinuxAuditLog/read | Gegevens lezen uit de tabel LinuxAuditLog |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplication/read | Gegevens lezen uit de tabel MAApplication |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealth/read | Gegevens lezen uit de tabel MAApplicationHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealthAlternativeVersions/read | Gegevens lezen uit de tabel MAApplicationHealthAlternativeVersions |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationHealthIssues/read | Gegevens lezen uit de tabel MAApplicationHealthIssues |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstance/read | Gegevens lezen uit de tabel MAApplicationInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationInstanceReadiness/read | Gegevens lezen uit de tabel MAApplicationInstanceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAApplicationReadiness/read | Gegevens lezen uit de tabel MAApplicationReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MADeploymentPlan/read | Gegevens lezen uit de tabel MADeploymentPlan |
> | DataAction | Microsoft.LogAnalytics/logs/MADevice/read | Gegevens lezen uit de tabel MADevice |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceNotEnrolled/read | Gegevens lezen uit de tabel MADeviceNotEnrolled |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealth/read | Gegevens lezen uit de tabel MADevicePnPHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealthAlternativeVersions/read | Gegevens lezen uit de tabel MADevicePnPHealthAlternativeVersions |
> | DataAction | Microsoft.LogAnalytics/logs/MADevicePnPHealthIssues/read | Gegevens lezen uit de tabel MADevicePnPHealthIssues |
> | DataAction | Microsoft.LogAnalytics/logs/MADeviceReadiness/read | Gegevens lezen uit de tabel MADeviceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverInstanceReadiness/read | Gegevens lezen uit de tabel MADriverInstanceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MADriverReadiness/read | Gegevens lezen uit de tabel MADriverReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddin/read | Gegevens lezen uit de tabel MAOfficeAddin |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealth/read | Gegevens lezen uit de tabel MAOfficeAddinHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinHealthIssues/read | Gegevens lezen uit de tabel MAOfficeAddinHealthIssues |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstance/read | Gegevens lezen uit de tabel MAOfficeAddinInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinInstanceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAddinReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeApp/read | Gegevens lezen uit de tabel MAOfficeApp |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppHealth/read | Gegevens lezen uit de tabel MAOfficeAppHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppInstance/read | Gegevens lezen uit de tabel MAOfficeAppInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeAppReadiness/read | Gegevens lezen uit de tabel MAOfficeAppReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeBuildInfo/read | Gegevens lezen uit de tabel MAOfficeBuildInfo |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessment/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessment |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessmentDailyCounts |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeDeploymentStatus/read | Gegevens lezen uit de tabel MAOfficeDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroHealth/read | Gegevens lezen uit de tabel MAOfficeMacroHealth |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroHealthIssues/read | Gegevens lezen uit de tabel MAOfficeMacroHealthIssues |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueInstanceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroIssueReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeMacroSummary/read | Gegevens lezen uit de tabel MAOfficeMacroSummary |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuite/read | Gegevens lezen uit de tabel MAOfficeSuite |
> | DataAction | Microsoft.LogAnalytics/logs/MAOfficeSuiteInstance/read | Gegevens lezen uit de tabel MAOfficeSuiteInstance |
> | DataAction | Microsoft.LogAnalytics/logs/MAProposedPilotDevices/read | Gegevens lezen uit de tabel MAProposedPilotDevices |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsBuildInfo/read | Gegevens lezen uit de tabel MAWindowsBuildInfo |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessment/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessment |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessmentDailyCounts |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsDeploymentStatus/read | Gegevens lezen uit de tabel MAWindowsDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/MAWindowsSysReqInstanceReadiness/read | Gegevens lezen uit de tabel MAWindowsSysReqInstanceReadiness |
> | DataAction | Microsoft.LogAnalytics/logs/NetworkMonitoring/read | Gegevens lezen uit de tabel NetworkMonitoring |
> | DataAction | Microsoft.LogAnalytics/logs/OfficeActivity/read | Gegevens lezen uit de tabel OfficeActivity |
> | DataAction | Microsoft.LogAnalytics/logs/Operation/read | Gegevens lezen uit de tabel |
> | DataAction | Microsoft.LogAnalytics/logs/OutboundConnection/read | Gegevens lezen uit de tabel OutboundConnection |
> | DataAction | Microsoft.LogAnalytics/logs/Perf/read | Gegevens lezen uit de tabel voor prestaties |
> | DataAction | Microsoft.LogAnalytics/logs/ProtectionStatus/read | Gegevens lezen uit de tabel ProtectionStatus |
> | Bewerking | Microsoft.LogAnalytics/logs/read | Lezen van gegevens uit alle uw logboeken |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedAzureCommonFields/read | Gegevens lezen uit de tabel ReservedAzureCommonFields |
> | DataAction | Microsoft.LogAnalytics/logs/ReservedCommonFields/read | Gegevens lezen uit de tabel ReservedCommonFields |
> | DataAction | Microsoft.LogAnalytics/logs/SCCMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCCMAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SCOMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCOMAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityAlert/read | Gegevens lezen uit de tabel SecurityAlert |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaseline/read | Gegevens lezen uit de tabel SecurityBaseline |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityBaselineSummary/read | Gegevens lezen uit de tabel SecurityBaselineSummary |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityDetection/read | Gegevens lezen uit de tabel SecurityDetection |
> | DataAction | Microsoft.LogAnalytics/logs/SecurityEvent/read | Gegevens lezen uit de tabel SecurityEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricOperationalEvent/read | Gegevens lezen uit de tabel ServiceFabricOperationalEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableActorEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableActorEvent |
> | DataAction | Microsoft.LogAnalytics/logs/ServiceFabricReliableServiceEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableServiceEvent |
> | DataAction | Microsoft.LogAnalytics/logs/SfBAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SfBOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBOnlineAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SharePointOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SharePointOnlineAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SigninLogs/read | Gegevens lezen uit de tabel SigninLogs |
> | DataAction | Microsoft.LogAnalytics/logs/SPAssessmentRecommendation/read | Gegevens lezen uit de tabel SPAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SQLAssessmentRecommendation/read | Gegevens lezen uit de tabel SQLAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/SQLQueryPerformance/read | Gegevens lezen uit de tabel SQLQueryPerformance |
> | DataAction | Microsoft.LogAnalytics/logs/Syslog/read | Gegevens lezen uit de Syslog-tabel |
> | DataAction | Microsoft.LogAnalytics/logs/SysmonEvent/read | Gegevens lezen uit de tabel SysmonEvent |
> | DataAction | Microsoft.LogAnalytics/logs/Tables.Custom/read | Lezen van gegevens uit een aangepaste logboek |
> | DataAction | Microsoft.LogAnalytics/logs/UAApp/read | Gegevens lezen uit de tabel UAApp |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputer/read | Gegevens lezen uit de tabel UAComputer |
> | DataAction | Microsoft.LogAnalytics/logs/UAComputerRank/read | Gegevens lezen uit de tabel UAComputerRank |
> | DataAction | Microsoft.LogAnalytics/logs/UADriver/read | Gegevens lezen uit de tabel UADriver |
> | DataAction | Microsoft.LogAnalytics/logs/UADriverProblemCodes/read | Gegevens lezen uit de tabel UADriverProblemCodes |
> | DataAction | Microsoft.LogAnalytics/logs/UAFeedback/read | Gegevens lezen uit de tabel UAFeedback |
> | DataAction | Microsoft.LogAnalytics/logs/UAHardwareSecurity/read | Gegevens lezen uit de tabel UAHardwareSecurity |
> | DataAction | Microsoft.LogAnalytics/logs/UAIESiteDiscovery/read | Gegevens lezen uit de tabel UAIESiteDiscovery |
> | DataAction | Microsoft.LogAnalytics/logs/UAOfficeAddIn/read | Gegevens lezen uit de tabel UAOfficeAddIn |
> | DataAction | Microsoft.LogAnalytics/logs/UAProposedActionPlan/read | Gegevens lezen uit de tabel UAProposedActionPlan |
> | DataAction | Microsoft.LogAnalytics/logs/UASysReqIssue/read | Gegevens lezen uit de tabel UASysReqIssue |
> | DataAction | Microsoft.LogAnalytics/logs/UAUpgradedComputer/read | Gegevens lezen uit de tabel UAUpgradedComputer |
> | DataAction | Microsoft.LogAnalytics/logs/Update/read | Gegevens lezen uit de tabel bijwerken |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateRunProgress/read | Gegevens lezen uit de tabel UpdateRunProgress |
> | DataAction | Microsoft.LogAnalytics/logs/UpdateSummary/read | Gegevens lezen uit de tabel UpdateSummary |
> | DataAction | Microsoft.LogAnalytics/logs/Usage/read | Gegevens lezen uit de tabel Gebruik |
> | DataAction | Microsoft.LogAnalytics/logs/VMBoundPort/read | Gegevens lezen uit de tabel VMBoundPort |
> | DataAction | Microsoft.LogAnalytics/logs/VMConnection/read | Gegevens lezen uit de tabel VMConnection |
> | DataAction | Microsoft.LogAnalytics/logs/W3CIISLog/read | Gegevens lezen uit de tabel W3CIISLog |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSDeploymentStatus/read | Gegevens lezen uit de tabel WaaSDeploymentStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSInsiderStatus/read | Gegevens lezen uit de tabel WaaSInsiderStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WaaSUpdateStatus/read | Gegevens lezen uit de tabel WaaSUpdateStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVStatus/read | Gegevens lezen uit de tabel WDAVStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WDAVThreat/read | Gegevens lezen uit de tabel WDAVThreat |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsClientAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsClientAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsEvent/read | Gegevens lezen uit de tabel WindowsEvent |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsFirewall/read | Gegevens lezen uit de tabel WindowsFirewall |
> | DataAction | Microsoft.LogAnalytics/logs/WindowsServerAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsServerAssessmentRecommendation |
> | DataAction | Microsoft.LogAnalytics/logs/WireData/read | Gegevens lezen uit de tabel WireData |
> | DataAction | Microsoft.LogAnalytics/logs/WorkloadMonitoringPerf/read | Gegevens lezen uit de tabel WorkloadMonitoringPerf |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOAggregatedStatus/read | Gegevens lezen uit de tabel WUDOAggregatedStatus |
> | DataAction | Microsoft.LogAnalytics/logs/WUDOStatus/read | Gegevens lezen uit de tabel WUDOStatus |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/delete | Hiermee verwijdert u de overeenkomst in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor overeenkomstinhoud in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/read | Hiermee wordt de overeenkomst in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/agreements/write | Hiermee of bijwerken van de overeenkomst in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/delete | Hiermee verwijdert u de assembly in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor assemblyinhoud in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/read | Hiermee wordt de assembly in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/assemblies/write | Hiermee of bijwerken van de assembly in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Hiermee verwijdert u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Hiermee of bijwerken van de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/delete | Hiermee verwijdert u het certificaat in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/read | Hiermee wordt het certificaat in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/certificates/write | Hiermee of bijwerken van het certificaat in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/delete | Hiermee verwijdert u het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/join/action | Lid wordt van het Integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor integratie-account. |
> | Bewerking | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Hiermee haalt de sleutels in de key vault. |
> | Bewerking | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Registreert de traceringsgebeurtenissen in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/delete | Hiermee verwijdert u de kaart in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor kaartinhoud in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/read | Hiermee wordt de kaart in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/maps/write | Hiermee of bijwerken van de kaart in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/delete | Hiermee verwijdert u de partner in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor partnerinhoud in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/read | Hiermee wordt de partner in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/partners/write | Hiermee of bijwerken van de partner in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Hiermee wordt de logboekdefinities van het Integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/read | Hiermee wordt het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Genereert de toegangssleutelgeheimen opnieuw. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/delete | Hiermee verwijdert u het schema in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Hiermee haalt de URL voor terugbellen voor schema-inhoud in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/read | Hiermee wordt het schema in het integratieaccount gelezen. |
> | Bewerking | Microsoft.Logic/integrationAccounts/schemas/write | Hiermee of bijwerken van het schema in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/delete | Hiermee verwijdert u de sessie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/read | Hiermee leest u de batchconfiguratie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/sessions/write | Hiermee of bijwerken van de sessie in het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationAccounts/write | Hiermee of bijwerken van het integratieaccount. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/delete | Hiermee verwijdert u de integratie van service-omgeving. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/join/action | Lid wordt van de integratie van Service-omgeving. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Hiermee leest u dat de integratie van service-omgeving beheerde API-bewerking. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Hiermee leest u dat de integratie van service-omgeving beheerde API. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Hiermee leest u de integratie bewerkingsstatussen van service-omgeving. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de integratie gelezen metrische definities van service-omgeving. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/read | Hiermee leest u de integratie van service-omgeving. |
> | Bewerking | Microsoft.Logic/integrationServiceEnvironments/write | Hiermee of bijwerken van de integratie van service-omgeving. |
> | Bewerking | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Hiermee haalt u de werkstroom aanbevolen bewerkingsgroepen. |
> | Bewerking | Microsoft.Logic/locations/workflows/validate/action | Hiermee valideert u de werkstroom. |
> | Bewerking | Microsoft.Logic/operations/read | Hiermee haalt u de bewerking. |
> | Bewerking | Microsoft.Logic/register/action | Hiermee registreert u de Microsoft.Logic-resourceprovider voor een bepaald abonnement. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/delete | Hiermee verwijdert u de toegangssleutel. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/list/action | Een lijst met toegangssleutelgeheimen. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/read | Hiermee wordt de toegangssleutel gelezen. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/regenerate/action | Genereert de toegangssleutelgeheimen opnieuw. |
> | Bewerking | Microsoft.Logic/workflows/accessKeys/write | Hiermee of bijwerken van de toegangssleutel. |
> | Bewerking | Microsoft.Logic/workflows/delete | Hiermee verwijdert u de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/disable/action | Hiermee schakelt u de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
> | Bewerking | Microsoft.Logic/workflows/listCallbackUrl/action | Hiermee haalt u de callback-URL voor de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/listSwagger/action | Hiermee haalt u de werkstroom swaggerdefinities. |
> | Bewerking | Microsoft.Logic/workflows/move/action | Werkstroom verplaatst van de bestaande abonnements-id, resourcegroep en/of de naam naar een andere abonnements-id, resourcegroep en/of naam. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instellingen voor de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee of de diagnostische instelling van werkstroom bijgewerkt. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Hiermee wordt de logboekdefinities van de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Hiermee wordt de werkstroom gelezen metrische definities. |
> | Bewerking | Microsoft.Logic/workflows/read | Hiermee wordt de werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/regenerateAccessKey/action | Genereert de toegangssleutelgeheimen opnieuw. |
> | Bewerking | Microsoft.Logic/workflows/run/action | Een uitvoering van de werkstroom wordt gestart. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Hiermee haalt u de actie expressietraceringen werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/read | Hiermee wordt de actie van de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Hiermee haalt u de werkstroom expressietraceringen voor herhaling actie. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/repetitions/read | Hiermee wordt de uitvoeringsactie van werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Hiermee wordt de geschiedenis van aanvraag voor de herhaling van de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Hiermee wordt de geschiedenis van de aanvraag van de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Hiermee wordt het bereik van de uitvoeringsactie werkstroom gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/cancel/action | Hiermee annuleert u de uitvoering van een werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/runs/delete | Hiermee verwijdert u een uitvoering van een werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/runs/operations/read | Hiermee wordt de bewerkingsstatus van werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/runs/read | Hiermee wordt de werkstroomuitvoering gelezen. |
> | Bewerking | Microsoft.Logic/workflows/suspend/action | Hiermee wordt de werkstroom onderbroken. |
> | Bewerking | Microsoft.Logic/workflows/triggers/histories/read | Hiermee wordt de geschiedenis van de trigger gelezen. |
> | Bewerking | Microsoft.Logic/workflows/triggers/histories/resubmit/action | De werkstroomtrigger. |
> | Bewerking | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Hiermee haalt u de callback-URL voor de trigger. |
> | Bewerking | Microsoft.Logic/workflows/triggers/read | Hiermee wordt de trigger gelezen. |
> | Bewerking | Microsoft.Logic/workflows/triggers/reset/action | Hiermee stelt u de trigger. |
> | Bewerking | Microsoft.Logic/workflows/triggers/run/action | De trigger wordt uitgevoerd. |
> | Bewerking | Microsoft.Logic/workflows/triggers/setState/action | Hiermee stelt u de Triggerstatus van de. |
> | Bewerking | Microsoft.Logic/workflows/validate/action | Hiermee valideert u de werkstroom. |
> | Bewerking | Microsoft.Logic/workflows/versions/read | Hiermee wordt de Werkstroomversie gelezen. |
> | Bewerking | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Hiermee haalt u de callback-URL voor de trigger. |
> | Bewerking | Microsoft.Logic/workflows/write | Hiermee of bijwerken van de werkstroom. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Verplaatsen van een Machine Learning-toezegging Plan koppeling |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Een Machine Learning-toezegging Plan koppeling lezen |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/delete | Een Machine Learning-Toezeggingsabonnement verwijderen |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/join/action | Deelnemen aan een Machine Learning-Toezeggingsabonnement |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/read | Een Machine Learning-Toezeggingsabonnement lezen |
> | Bewerking | Microsoft.MachineLearning/commitmentPlans/write | Maken of bijwerken van een Toezeggingsabonnement voor Machine Learning |
> | Bewerking | Microsoft.MachineLearning/locations/operationresults/read | Resultaat van een Machine Learning-bewerking |
> | Bewerking | Microsoft.MachineLearning/locations/operationsstatus/read | Status van een bewerking uitgevoerd waarmee Machine Learning ophalen |
> | Bewerking | Microsoft.MachineLearning/operations/read | Machine Learning-bewerkingen ophalen |
> | Bewerking | Microsoft.MachineLearning/register/action | Hiermee wordt het abonnement voor de machine learning web service-resourceprovider geregistreerd en wordt het maken van webservices. |
> | Bewerking | Microsoft.MachineLearning/skus/read | Machine Learning-toezegging Plan SKU's ophalen |
> | Bewerking | Microsoft.MachineLearning/webServices/action | Regionale Web Service-eigenschappen voor ondersteunde regio's maken |
> | Bewerking | Microsoft.MachineLearning/webServices/delete | Een Machine Learning-webservice verwijderen |
> | Bewerking | Microsoft.MachineLearning/webServices/listkeys/read | Een Machine Learning-webservice-sleutels ophalen |
> | Bewerking | Microsoft.MachineLearning/webServices/read | Lezen van een Machine Learning-webservice |
> | Bewerking | Microsoft.MachineLearning/webServices/write | Maken of bijwerken van een Machine Learning-webservice |
> | Bewerking | Microsoft.MachineLearning/Workspaces/delete | Een Machine Learning-werkruimte verwijderen |
> | Bewerking | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Lijst met sleutels voor een Machine Learning-werkruimte |
> | Bewerking | Microsoft.MachineLearning/Workspaces/read | Lezen van een Machine Learning-werkruimte |
> | Bewerking | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Sleutels van opslagaccount dat is geconfigureerd voor een Machine Learning-werkruimte opnieuw synchroniseren |
> | Bewerking | Microsoft.MachineLearning/Workspaces/write | Een Machine Learning-werkruimte gemaakt of bijgewerkt |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Controleer of er updates beschikbaar voor systeemservices voor het cluster uitoefening zijn |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Hosting-account verwijderen |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | De sleutels die zijn gekoppeld aan uitoefening cluster weergeven |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/read | Hosting-account lezen |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Het bijwerken van de systeemservices in een cluster uitoefening |
> | Bewerking | Microsoft.MachineLearningCompute/operationalizationClusters/write | Maken of bijwerken van een hosting-account |
> | Bewerking | Microsoft.MachineLearningCompute/register/action | Abonnements-ID bij de resourceprovider geregistreerd en wordt het maken van een machine learning-compute-resources |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/delete | Hosting-account verwijderen |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/read | Hosting-account lezen |
> | Bewerking | Microsoft.MachineLearningModelManagement/accounts/write | Maken of bijwerken van een hosting-account |
> | Bewerking | Microsoft.MachineLearningModelManagement/register/action | Abonnements-ID bij de resourceprovider geregistreerd en wordt het maken van een hosting-account |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Hiermee wordt de status van een bepaalde compute-bewerking opgehaald |
> | Bewerking | Microsoft.MachineLearningServices/locations/usages/read | Gebruiksrapport voor aml compute-resources in een abonnement |
> | Bewerking | Microsoft.MachineLearningServices/locations/vmsizes/read | Ondersteunde vm-grootten opvragen |
> | Bewerking | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Hiermee wordt de status van een bepaalde werkruimte bewerking opgehaald |
> | Bewerking | Microsoft.MachineLearningServices/register/action | Hiermee wordt het abonnement voor de Resourceprovider voor Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/delete | Hiermee verwijdert u de compute-resources in werkruimten voor Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Lijst met geheimen voor compute-resources in de werkruimte van Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | Lijst met knooppunten voor compute-resource in de werkruimte van Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/read | Hiermee haalt u de compute-resources in werkruimten voor Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/computes/write | Hiermee maken of bijwerken van de compute-resources in werkruimten voor Machine Learning-Services |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/delete | Hiermee verwijdert u de Machine Learning-Services-werkruimte |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/listKeys/action | Lijst met geheimen voor een Machine Learning-Services-werkruimte |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/read | De Machine Learning-Services werkruimten opgehaald |
> | Bewerking | Microsoft.MachineLearningServices/workspaces/write | Hiermee maken of bijwerken van een Machine Learning Services werkruimten |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ManagedIdentity/register/action | Hiermee wordt het abonnement voor de beheerde identiteit-resourceprovider geregistreerd |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC-actie voor het toewijzen van een bestaande gebruiker toegewezen identiteit aan een resource |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Hiermee verwijdert u een bestaande gebruiker toegewezen identiteit |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/read | Een bestaande gebruiker toegewezen identiteit opgehaald |
> | Bewerking | Microsoft.ManagedIdentity/userAssignedIdentities/write | Hiermee maakt u een nieuwe gebruiker toegewezen identiteit of updates van de labels die zijn gekoppeld aan een bestaande gebruiker toegewezen identiteit |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ManagedLab/labAccounts/CreateLab/action | Een lab maken in een lab-account. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/delete | Lab-accounts verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/delete | Labs verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Omgevingsinstelling verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Omgevingen verwijderen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Omgevingen lezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Toevoegen of wijzigen van omgevingen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Omgevingsinstelling lezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Toevoegen of wijzigen van de omgevingsinstelling. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Verwijder de virtuele machines. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/read | De virtuele machines gelezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Toevoegen of wijzigen van de virtuele machines. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/read | Lees labs. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/labs/write | Toevoegen of wijzigen van labs. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/read | Lab-accounts worden gelezen. |
> | Bewerking | Microsoft.ManagedLab/labAccounts/write | Toevoegen of wijzigen van lab-accounts. |
> | Bewerking | Microsoft.ManagedLab/locations/operations/read | Leesbewerkingen. |
> | Bewerking | Microsoft.ManagedLab/register/action | Hiermee wordt het abonnement geregistreerd |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Management/checkNameAvailability/action | Controleert of de naam van de opgegeven beheergroep geldig en uniek is. |
> | Bewerking | Microsoft.Management/getEntities/action | Lijst met alle entiteiten (beheergroepen, abonnementen, enzovoort) voor de geverifieerde gebruiker. |
> | Bewerking | Microsoft.Management/managementGroups/delete | Beheergroep verwijderen. |
> | Bewerking | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Bewerking | Microsoft.Management/managementGroups/subscriptions/delete | Hiermee koppelt u abonnement uit de beheergroep ongedaan maken. |
> | Bewerking | Microsoft.Management/managementGroups/subscriptions/write | Collega's het bestaande abonnement met de beheergroep. |
> | Bewerking | Microsoft.Management/managementGroups/write | Maken of bijwerken van een beheergroep. |
> | Bewerking | Microsoft.Management/register/action | Het opgegeven abonnement registreren bij Microsoft.Management |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Verleent toegang tot het lezen van gegevens naar een maps-account. |
> | Bewerking | Microsoft.Maps/accounts/delete | Een Maps-Account verwijderen. |
> | Bewerking | Microsoft.Maps/accounts/eventGridFilters/delete | Een Event Grid-filter verwijderen |
> | Bewerking | Microsoft.Maps/accounts/eventGridFilters/read | Een Event Grid-filter ophalen |
> | Bewerking | Microsoft.Maps/accounts/eventGridFilters/write | Maken of bijwerken van een Event Grid-filter |
> | Bewerking | Microsoft.Maps/accounts/listKeys/action | Een lijst met kaarten-Account sleutels |
> | Bewerking | Microsoft.Maps/accounts/read | Maak een Maps-Account. |
> | Bewerking | Microsoft.Maps/accounts/regenerateKey/action | Nieuwe kaarten-Account primaire of secundaire sleutel genereren |
> | Bewerking | Microsoft.Maps/accounts/write | Maken of bijwerken van een Maps-Account. |
> | Bewerking | Microsoft.Maps/operations/read | De providerbewerkingen lezen |
> | Bewerking | Microsoft.Maps/register/action | De provider registreren |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Retourneert een overeenkomst. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Een ondertekende overeenkomst accepteert. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Een installatiekopie naar ACR van de eindgebruiker geïmporteerd. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Retourneert een configuratie. |
> | Bewerking | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Hiermee slaat u een configuratie. |
> | Bewerking | Microsoft.Marketplace/register/action | Hiermee wordt u Microsoft.Marketplace-resourceprovider geregistreerd in het abonnement. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/delete | Biedt een DELETE-bewerking op de bron van een klassieke dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Hiermee haalt u een klassieke dev servicesleutels resource management. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Hiermee haalt de eenmalige aanmelding op URL voor een classic dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/read | Biedt een GET-bewerking op een klassieke dev-service. |
> | Bewerking | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Genereert een klassieke dev service resource management-sleutels. |
> | Bewerking | Microsoft.MarketplaceApps/Operations/read | De leesbewerkingen voor alle resourcetypen. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Annuleren van een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Retourneert een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Meld u aan een overeenkomst voor een bepaalde marketplace-item |
> | Bewerking | Microsoft.MarketplaceOrdering/agreements/read | Retourneren van alle overeenkomsten onder het opgegeven abonnement |
> | Bewerking | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Een overeenkomst voor een bepaalde marketplace-item voor virtuele machine ophalen |
> | Bewerking | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Meld u aan of annuleren van een overeenkomst voor een bepaalde marketplace-item voor virtuele machine |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Media/checknameavailability/action | Controleert of de naam van een Media Services-account beschikbaar is |
> | Bewerking | Microsoft.Media/locations/checkNameAvailability/action | Controleert of de naam van een Media Services-account beschikbaar is |
> | Bewerking | Microsoft.Media/mediaservices/accountfilters/delete | Een Filter Account verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/accountfilters/read | Een Filter Account lezen |
> | Bewerking | Microsoft.Media/mediaservices/accountfilters/write | Maken of bijwerken van een Account-Filter |
> | Bewerking | Microsoft.Media/mediaservices/assets/assetfilters/delete | Een Filter Asset verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/assets/assetfilters/read | Een Filter Asset lezen |
> | Bewerking | Microsoft.Media/mediaservices/assets/assetfilters/write | Maken of bijwerken van een Asset-Filter |
> | Bewerking | Microsoft.Media/mediaservices/assets/delete | Verwijderen van Assets |
> | Bewerking | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Asset-coderingssleutel ophalen |
> | Bewerking | Microsoft.Media/mediaservices/assets/listContainerSas/action | Lijst met activa-Container SAS-URL 's |
> | Bewerking | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Lijst met Streaming-Locators voor Asset |
> | Bewerking | Microsoft.Media/mediaservices/assets/read | Een Asset lezen |
> | Bewerking | Microsoft.Media/mediaservices/assets/write | Maken of bijwerken van een Asset |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Alle inhoud sleutel beleid verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Eigenschappen van beleid met geheimen ophalen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/read | Alle inhoud sleutel beleid lezen |
> | Bewerking | Microsoft.Media/mediaservices/contentKeyPolicies/write | Alle inhoud sleutel beleid maken of bijwerken |
> | Bewerking | Microsoft.Media/mediaservices/delete | Een Media Services-Account verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/delete | Een Event Grid-Filter verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/read | Lezen van een Event Grid-Filter |
> | Bewerking | Microsoft.Media/mediaservices/eventGridFilters/write | Maken of bijwerken van een Event Grid-Filter |
> | Bewerking | Microsoft.Media/mediaservices/liveEventOperations/read | Elke leesbewerking van de Live-gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/delete | Een Live gebeurtenis verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Live uitvoer verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Live uitvoer lezen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Maken of bijwerken van een Live-uitvoer |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/read | Een Live gebeurtenis lezen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/reset/action | Opnieuw instellen van een Live-gebeurtenis-bewerking |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/start/action | Start een Live-gebeurtenis-bewerking |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/stop/action | Elke bewerking die wordt Live-gebeurtenis beëindigen |
> | Bewerking | Microsoft.Media/mediaservices/liveEvents/write | Maken of bijwerken van een Live gebeurtenis |
> | Bewerking | Microsoft.Media/mediaservices/liveOutputOperations/read | Elke leesbewerking van de Live-uitvoer |
> | Bewerking | Microsoft.Media/mediaservices/read | Een Media Services-Account lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Alle Streaming Endpoint leesbewerking |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/delete | Een Streaming-eindpunt verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/read | Lezen van een Streaming-eindpunt |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Elke Streaming Endpoint-bewerking schalen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Alle Streaming Endpoint-bewerking starten |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Elke bewerking die Streaming eindpunt stoppen |
> | Bewerking | Microsoft.Media/mediaservices/streamingEndpoints/write | Maken of bijwerken van een Streaming-eindpunt |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/delete | Een Streaming-Locator verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Een lijst met inhoud sleutels |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Lijst met paden |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/read | Een Streaming-Locator lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingLocators/write | Maken of bijwerken van een Streaming-Locator |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/delete | Een Streaming-beleid verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/read | Een Streaming-beleid lezen |
> | Bewerking | Microsoft.Media/mediaservices/streamingPolicies/write | Een Streaming-beleid maken of bijwerken |
> | Bewerking | Microsoft.Media/mediaservices/syncStorageKeys/action | Synchroniseren van de Storage-sleutels voor een gekoppelde Azure Storage-account |
> | Bewerking | Microsoft.Media/mediaservices/transforms/delete | Elke transformatie verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Taak annuleren |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/delete | Een taak verwijderen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/read | Elke taak lezen |
> | Bewerking | Microsoft.Media/mediaservices/transforms/jobs/write | Maken of bijwerken van een taak |
> | Bewerking | Microsoft.Media/mediaservices/transforms/read | Lezen van een transformatie |
> | Bewerking | Microsoft.Media/mediaservices/transforms/write | Maken of bijwerken van een transformatie |
> | Bewerking | Microsoft.Media/mediaservices/write | Maken of bijwerken van een Media Services-Account |
> | Bewerking | Microsoft.Media/operations/read | Beschikbare bewerkingen ophalen |
> | Bewerking | Microsoft.Media/register/action | Hiermee wordt het abonnement voor de Media Services-resourceprovider geregistreerd en wordt het maken van Media Services-accounts |
> | Bewerking | Microsoft.Media/unregister/action | De registratie van het abonnement voor de resourceprovider van Media Services |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Migrate/locations/assessmentOptions/read | Haalt de beoordelingsopties die beschikbaar in de opgegeven locatie zijn |
> | Bewerking | Microsoft.Migrate/locations/checknameavailability/action | Controleert de beschikbaarheid van de naam van de resource voor het betreffende abonnement op de opgegeven locatie |
> | Bewerking | Microsoft.Migrate/migrateprojects/read | Hiermee haalt u de eigenschappen van migrate-project. |
> | Bewerking | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Hiermee wordt de configuratie van migrate project oplossing opgehaald |
> | Bewerking | Microsoft.Migrate/migrateprojects/solutions/read | Hiermee haalt u de eigenschappen van migrate project-oplossing. |
> | Bewerking | Microsoft.Migrate/Operations/read | Een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Migrate-resourceprovider |
> | Bewerking | Microsoft.Migrate/projects/assessments/read | Een lijst met beoordelingen binnen een project |
> | Bewerking | Microsoft.Migrate/projects/delete | Hiermee verwijdert u het project |
> | Bewerking | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Hiermee worden de eigenschappen van een geëvalueerde computer |
> | Bewerking | Microsoft.Migrate/projects/groups/assessments/delete | Hiermee verwijdert u een evaluatie |
> | Bewerking | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Downloads van een evaluatierapport-URL |
> | Bewerking | Microsoft.Migrate/projects/groups/assessments/read | Haalt u de eigenschappen van een evaluatie |
> | Bewerking | Microsoft.Migrate/projects/groups/assessments/write | Hiermee maakt u een nieuwe evaluatie van of de evaluatie van een bestaande bijgewerkt |
> | Bewerking | Microsoft.Migrate/projects/groups/delete | Hiermee verwijdert u een groep |
> | Bewerking | Microsoft.Migrate/projects/groups/read | Hiermee worden de eigenschappen van een groep |
> | Bewerking | Microsoft.Migrate/projects/groups/write | Hiermee maakt u een nieuwe groep of updates van een bestaande groep |
> | Bewerking | Microsoft.Migrate/projects/keys/action | Haalt gedeelde sleutels voor het project |
> | Bewerking | Microsoft.Migrate/projects/machines/read | Hiermee worden de eigenschappen van een virtuele machine opgehaald |
> | Bewerking | Microsoft.Migrate/projects/read | Hiermee worden de eigenschappen van een project opgehaald |
> | Bewerking | Microsoft.Migrate/projects/write | Hiermee maakt u een nieuw project of een bestaand project bijwerken |
> | Bewerking | Microsoft.Migrate/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.Migrate |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.MixedReality/register/action | Hiermee wordt een abonnement geregistreerd voor de resourceprovider voor Mixed Reality. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Ruimtelijke ankers maken |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Ruimtelijke ankers verwijderen |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | In de buurt ruimtelijke ankers detecteren |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermee worden eigenschappen van ruimtelijke ankers |
> | Bewerking | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor Microsoft.MixedReality/SpatialAnchorsAccounts opgehaald |
> | Bewerking | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor Microsoft.MixedReality/SpatialAnchorsAccounts |
> | Bewerking | Microsoft.MixedReality/SpatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor Microsoft.MixedReality/SpatialAnchorsAccounts |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ruimtelijke ankers zoeken |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Verzenden van diagnostische gegevens over ter verbetering van de kwaliteit van de service Azure ruimtelijke ankers |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Eigenschappen van ruimtelijke ankers bijwerken |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.NetApp/locations/operationresults/read | Hiermee wordt een bewerking resultaat resource gelezen. |
> | Bewerking | Microsoft.NetApp/locations/read | Leest een beschikbaarheid van controleren resources |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Hiermee verwijdert u een pool resource. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/read | Hiermee wordt een pool resource gelezen. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Hiermee verwijdert u een volumeresource. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Hiermee leest u de doelresource van een koppelpunt. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Hiermee wordt een volumeresource gelezen. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Hiermee verwijdert u de resource van een momentopname. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Hiermee leest u de resource van een momentopname. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Schrijft een momentopname van bron. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Schrijft een volumeresource. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/capacityPools/write | Schrijft een pool resource. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/delete | Hiermee verwijdert u een account-resource. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/read | Hiermee leest u de resource van een account. |
> | Bewerking | Microsoft.NetApp/netAppAccounts/write | Hiermee worden de resource van een geschreven. |
> | Bewerking | Microsoft.NetApp/Operations/read | Hiermee wordt een bewerking resources gelezen. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Application Gateway beschikbaar aanvraagheaders ophalen |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Application Gateway beschikbaar Response-Header ophalen |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Application Gateway beschikbaar servervariabelen ophalen |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Application-Gateway Ssl vooraf gedefinieerd beleid |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Application Gateway beschikbare Ssl-opties |
> | Bewerking | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Application Gateway beschikbaar Waf regelsets opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Lid wordt van een application gateway back-end-adresgroep. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/applicationGateways/backendhealth/action | Een application gateway back-endstatus opgehaald |
> | Bewerking | Microsoft.Network/applicationGateways/delete | Hiermee verwijdert u een application gateway |
> | Bewerking | Microsoft.Network/applicationGateways/read | Haalt een application gateway |
> | Bewerking | Microsoft.Network/applicationGateways/start/action | Een application gateway wordt gestart |
> | Bewerking | Microsoft.Network/applicationGateways/stop/action | Een application gateway wordt gestopt |
> | Bewerking | Microsoft.Network/applicationGateways/write | Hiermee maakt u een application gateway of een application gateway-updates |
> | Bewerking | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Hiermee verwijdert u een Application Gateway WAF-beleid |
> | Bewerking | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Haalt een Application Gateway WAF-beleid |
> | Bewerking | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Hiermee maakt u een Application Gateway WAF-beleid of een Application Gateway WAF-beleid werkt |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/delete | Hiermee verwijdert u een Toepassingsbeveiligingsgroep |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Lid wordt van een IP-configuratie naar Toepassingsbeveiligingsgroepen. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Lid wordt van een beveiligingsregel voor beveiligingsgroepen voor toepassing. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Een lijst met IP-configuraties in met de ApplicationSecurityGroup |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/read | Haalt een Toepassingsbeveiligingsgroep-ID. |
> | Bewerking | Microsoft.Network/applicationSecurityGroups/write | Hiermee maakt u een Toepassingsbeveiligingsgroep of updates van een bestaande Toepassingsbeveiligingsgroep. |
> | Bewerking | Microsoft.Network/azureFirewallFqdnTags/read | Firewall van Azure FQDN labels opgehaald |
> | Bewerking | Microsoft.Network/azurefirewalls/delete | Verwijderen van de Firewall van Azure |
> | Bewerking | Microsoft.Network/azurefirewalls/read | Firewall van Azure ophalen |
> | Bewerking | Microsoft.Network/azurefirewalls/write | Hiermee maken of bijwerken van een Azure-Firewall |
> | Bewerking | Microsoft.Network/bastionHosts/delete | Hiermee verwijdert u een bastionhost |
> | Bewerking | Microsoft.Network/bastionHosts/read | Een bastionhost opgehaald |
> | Bewerking | Microsoft.Network/bastionHosts/write | Maken of bijwerken van een bastionhost |
> | Bewerking | Microsoft.Network/bgpServiceCommunities/read | Service voor Bgp-community's ophalen |
> | Bewerking | Microsoft.Network/checkFrontDoorNameAvailability/action | Hiermee wordt gecontroleerd of de naam van een voordeur beschikbaar |
> | Bewerking | Microsoft.Network/checkTrafficManagerNameAvailability/action | Controleert de beschikbaarheid van een Traffic Manager ten opzichte van DNS-naam. |
> | Bewerking | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/connections/revoke/action | Een Express Route-verbindingsstatus markeert als ingetrokken |
> | Bewerking | Microsoft.Network/connections/sharedkey/action | Get VirtualNetworkGatewayConnection SharedKey |
> | Bewerking | Microsoft.Network/connections/sharedKey/read | Gets VirtualNetworkGatewayConnection SharedKey |
> | Bewerking | Microsoft.Network/connections/sharedKey/write | Hiermee maakt of een bestaande VirtualNetworkGatewayConnection SharedKey werkt |
> | Bewerking | Microsoft.Network/connections/vpndeviceconfigurationscript/action | VPN-apparaatconfiguratie van VirtualNetworkGatewayConnection opgehaald |
> | Bewerking | Microsoft.Network/connections/write | Hiermee maakt of een bestaande VirtualNetworkGatewayConnection werkt |
> | Bewerking | Microsoft.Network/ddosCustomPolicies/delete | Verwijdert een DDoS aangepast beleid |
> | Bewerking | Microsoft.Network/ddosCustomPolicies/read | Haalt een DDoS aangepast beleidsdefinitie definitie |
> | Bewerking | Microsoft.Network/ddosCustomPolicies/write | Hiermee een DDoS aangepast beleid maken of bijwerken van een bestaand aangepast DDoS-beleid |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/delete | Hiermee verwijdert u een DDoS-beschermingsplan |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/join/action | Lid wordt van een DDoS-beschermingsplan. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/read | Hiermee wordt een DDoS-beschermingsplan opgehaald |
> | Bewerking | Microsoft.Network/ddosProtectionPlans/write | Hiermee maakt u een DDoS Protection-Plan gemaakt of bijgewerkt een DDoS Protection-Plan  |
> | Bewerking | Microsoft.Network/dnsoperationresults/read | Resultaten van een DNS-bewerking opgehaald |
> | Bewerking | Microsoft.Network/dnsoperationstatuses/read | Hiermee wordt de status van een DNS-bewerking opgehaald  |
> | Bewerking | Microsoft.Network/dnszones/A/delete | Verwijderen van de recordset van een opgegeven naam en type 'A' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/A/read | Haal de recordset van het type 'A', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/A/write | Maken of bijwerken van een recordset van het type 'A' in een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/delete | Verwijderen van de recordset van een opgegeven naam en typ 'AAAA' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/read | Krijg de recordset van het type 'AAAA', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/AAAA/write | Maken of bijwerken van een recordset van het type 'AAAA' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/all/read | DNS-recordsets in verschillende typen opgehaald |
> | Bewerking | Microsoft.Network/dnszones/CAA/delete | Verwijderen van de recordset van een opgegeven naam en typ 'CAA' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/CAA/read | Krijg de recordset van het type 'CAA', in JSON-indeling. De recordset bevat de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/CAA/write | Maken of bijwerken van een recordset van het type 'CAA' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/delete | Verwijderen van de recordset van een opgegeven naam en typ 'CNAME' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/read | Krijg de recordset van het type 'CNAME', in JSON-indeling. De recordset bevat de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/CNAME/write | Maken of bijwerken van een recordset van het type 'CNAME' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/delete | Verwijder de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn tags, etag numberOfRecordSets en maxNumberOfRecordSets. |
> | Bewerking | Microsoft.Network/dnszones/MX/delete | Verwijderen van de recordset van een opgegeven naam en typ 'MX' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/MX/read | Haal de recordset van het type 'MX', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/MX/write | Maken of bijwerken van een recordset van het type 'MX' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/NS/delete | Hiermee verwijdert u de DNS-recordset van type NS |
> | Bewerking | Microsoft.Network/dnszones/NS/read | DNS-recordset van type NS opgehaald |
> | Bewerking | Microsoft.Network/dnszones/NS/write | Hiermee maken of bijwerken van DNS-recordset van type NS |
> | Bewerking | Microsoft.Network/dnszones/PTR/delete | Verwijderen van de recordset van een opgegeven naam en typ 'PTR' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/PTR/read | Krijg de recordset van het type 'PTR', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/PTR/write | Maken of bijwerken van een recordset van het type 'PTR' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/read | De DNS-zone, in JSON-indeling verkrijgen. De zone-eigenschappen zijn tags, etag numberOfRecordSets en maxNumberOfRecordSets. Houd er rekening mee dat deze opdracht de recordsets opgenomen in de zone niet ophaalt. |
> | Bewerking | Microsoft.Network/dnszones/recordsets/read | DNS-recordsets in verschillende typen opgehaald |
> | Bewerking | Microsoft.Network/dnszones/SOA/read | DNS-recordset van het type SOA opgehaald |
> | Bewerking | Microsoft.Network/dnszones/SOA/write | Hiermee maakt of DNS-recordset van het type SOA werkt |
> | Bewerking | Microsoft.Network/dnszones/SRV/delete | Verwijderen van de recordset van een opgegeven naam en typ 'SRV' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/SRV/read | Krijg de recordset van het type 'SRV', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/SRV/write | Maken of bijwerken van de recordset van type SRV |
> | Bewerking | Microsoft.Network/dnszones/TXT/delete | Verwijder de recordset van een opgegeven naam en typ 'TXT' van een DNS-zone. |
> | Bewerking | Microsoft.Network/dnszones/TXT/read | Krijg de recordset van het type 'TXT', in JSON-indeling. De recordset bevat een lijst met records, evenals de TTL-waarde, labels en etag. |
> | Bewerking | Microsoft.Network/dnszones/TXT/write | Maken of bijwerken van een recordset van het type 'TXT' binnen een DNS-zone. De opgegeven records worden vervangen door de huidige records in de recordset. |
> | Bewerking | Microsoft.Network/dnszones/write | Maken of bijwerken van een DNS-zone binnen een resourcegroep.  Gebruikt voor het bijwerken van de tags op een DNS-zone-resource. Houd er rekening mee dat met deze opdracht niet kan worden gebruikt voor het maken of bijwerken van recordsets in de zone. |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/delete | Hiermee verwijdert u de autorisatie van een expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/read | Wordt een expressroute-circuit geautoriseerd |
> | Bewerking | Microsoft.Network/expressRouteCircuits/authorizations/write | Hiermee maakt of een bestaande expressroute-circuit autorisatie werkt |
> | Bewerking | Microsoft.Network/expressRouteCircuits/delete | Hiermee verwijdert u een expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/join/action | Lid wordt van een Express Route-Circuit. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Een expressroute-circuit Peering ArpTable opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Hiermee verwijdert u de verbinding van een expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Een expressroute-circuit verbinding opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Hiermee maakt of een bestaande expressroute-circuit Verbindingsresource werkt |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/delete | Hiermee verwijdert u een Peering voor expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Peer Express Route-Circuitverbinding opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/read | Haalt een Peering voor expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Een expressroute-circuit Peering RouteTable opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Een overzicht van expressroute-circuit Peering RouteTable opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Haalt een expressroute-circuit Peering statistieken |
> | Bewerking | Microsoft.Network/expressRouteCircuits/peerings/write | Hiermee maakt of werkt een bestaande expressroute-circuit Peering |
> | Bewerking | Microsoft.Network/expressRouteCircuits/read | Ophalen van een expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCircuits/stats/read | Een expressroute-circuit Stat opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCircuits/write | Hiermee maken of bijwerken van een bestaande expressroute-circuit |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/join/action | Joins een Express Route Cross-verbinding. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Een Express-Route Cross-verbinding Peering Arp-tabel opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Hiermee verwijdert u een Express-Route Cross-Peering verbinding |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/read | Een Express-Route Cross-Peering verbinding opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Een Express-Route Cross-Route-Table met Peering verbinding opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Een Express-Route Cross-tabel Samenvatting verbinding Peering-Route opgehaald |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/peerings/write | Hiermee maakt u een Cross-verbinding Peering voor Express Route of een bestaande Cross-verbinding Peering voor Express Route-Updates |
> | Bewerking | Microsoft.Network/expressRouteCrossConnections/read | Express Route ophalen Cross-verbinding |
> | Bewerking | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Hiermee verwijdert u een Expressroute-verbinding |
> | Bewerking | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Een Express Route-verbinding opgehaald |
> | Bewerking | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Een Express Route-verbinding hiermee maken of bijwerken van een bestaande Express Route-verbinding |
> | Bewerking | Microsoft.Network/expressRouteGateways/join/action | Lid wordt van een Expressroute-Gateway. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/expressRouteGateways/read | Verkrijg de Gateway van het Express Route |
> | Bewerking | Microsoft.Network/expressRoutePorts/delete | Deletes ExpressRoutePorts |
> | Bewerking | Microsoft.Network/expressRoutePorts/join/action | Express Route-poorten koppelt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/expressRoutePorts/links/read | Gets ExpressRouteLink |
> | Bewerking | Microsoft.Network/expressRoutePorts/read | Gets ExpressRoutePorts |
> | Bewerking | Microsoft.Network/expressRoutePorts/write | Hiermee maakt of ExpressRoutePorts werkt |
> | Bewerking | Microsoft.Network/expressRoutePortsLocations/read | Get Express Route poorten locaties |
> | Bewerking | Microsoft.Network/expressRouteServiceProviders/read | Express Route-serviceproviders opgehaald |
> | Bewerking | Microsoft.Network/frontDoors/backendPools/delete | Hiermee verwijdert u een back-endpool |
> | Bewerking | Microsoft.Network/frontDoors/backendPools/read | Een back-endpool opgehaald |
> | Bewerking | Microsoft.Network/frontDoors/backendPools/write | Hiermee maken of bijwerken van een back-endpool |
> | Bewerking | Microsoft.Network/frontDoors/delete | Hiermee verwijdert u een voordeur |
> | Bewerking | Microsoft.Network/frontDoors/frontendEndpoints/delete | Hiermee verwijdert u een front-end-eindpunt |
> | Bewerking | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | HTTPS op een Frontend-eindpunt wordt uitgeschakeld |
> | Bewerking | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | Hiermee kunt HTTPS op een Frontend-eindpunt |
> | Bewerking | Microsoft.Network/frontDoors/frontendEndpoints/read | Haalt een frontend-eindpunt |
> | Bewerking | Microsoft.Network/frontDoors/frontendEndpoints/write | Maakt of updatet een frontend-eindpunt |
> | Bewerking | Microsoft.Network/frontDoors/healthProbeSettings/delete | Hiermee verwijdert u de gezondheid van test-instellingen |
> | Bewerking | Microsoft.Network/frontDoors/healthProbeSettings/read | Health test-instellingen worden opgehaald |
> | Bewerking | Microsoft.Network/frontDoors/healthProbeSettings/write | Hiermee maken of bijwerken van instellingen voor health test |
> | Bewerking | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Hiermee maken of bijwerken van instellingen voor taakverdeling |
> | Bewerking | Microsoft.Network/frontDoors/loadBalancingSettings/read | Haalt de instellingen voor taakverdeling |
> | Bewerking | Microsoft.Network/frontDoors/loadBalancingSettings/write | Hiermee maken of bijwerken van instellingen voor taakverdeling |
> | Bewerking | Microsoft.Network/frontDoors/purge/action | In de cache opgeslagen inhoud verwijderen uit een voordeur |
> | Bewerking | Microsoft.Network/frontDoors/read | Haalt een voordeur |
> | Bewerking | Microsoft.Network/frontDoors/routingRules/delete | Hiermee verwijdert u een regel voor doorsturen |
> | Bewerking | Microsoft.Network/frontDoors/routingRules/read | Met deze eigenschap wordt een regel voor doorsturen |
> | Bewerking | Microsoft.Network/frontDoors/routingRules/write | Hiermee maken of bijwerken van een regel voor doorsturen |
> | Bewerking | Microsoft.Network/frontDoors/validateCustomDomain/action | Valideert een frontend-eindpunt voor een voordeur |
> | Bewerking | Microsoft.Network/frontDoors/write | Hiermee maakt of een voordeur werkt |
> | Bewerking | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Hiermee verwijdert u een Web Application Firewall-beleid |
> | Bewerking | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Haalt een Web Application Firewall-beleid |
> | Bewerking | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Hiermee maakt of een Web Application Firewall-beleid werkt |
> | Bewerking | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een load balancer-back-end-adresgroep. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/loadBalancers/backendAddressPools/read | Een load balancer-back-end-adres-groepsdefinitie opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/delete | Hiermee verwijdert u een load balancer |
> | Bewerking | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Lid wordt van een Load Balancer-Frontend-IP-configuratie. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Haalt een load balancer front-end IP-configuratie van definitie |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid wordt van een load balancer binnenkomende NAT-pool. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatPools/read | Een load balancer wordt binnenkomende nat-groepsdefinitie |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/delete | Hiermee verwijdert u een load balancer binnenkomende nat-regel |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/read | Een load balancer wordt binnenkomende nat-regel definiëren |
> | Bewerking | Microsoft.Network/loadBalancers/inboundNatRules/write | Hiermee maakt u een load balancer binnenkomende nat-regel of een bestaande load balancer binnenkomende nat-regel bijgewerkt |
> | Bewerking | Microsoft.Network/loadBalancers/loadBalancingRules/read | Een load balancer load balancing regeldefinitie opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/networkInterfaces/read | Verwijzingen naar alle netwerkinterfaces onder een load balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/outboundRules/read | De definitie van een load balancer-regel voor uitgaande opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van tests van een load balancer. Bijvoorbeeld: met deze machtiging Statustest-eigenschap van de VM-schaalset set kan verwijzen naar de test. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/loadBalancers/probes/read | Haalt een load balancer-test |
> | Bewerking | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/virtualMachines/read | Verwijzingen naar de virtuele machines onder een load balancer opgehaald |
> | Bewerking | Microsoft.Network/loadBalancers/write | Hiermee maakt u een load balancer of een bestaande load balancer-updates |
> | Bewerking | Microsoft.Network/localnetworkgateways/delete | Deletes LocalNetworkGateway |
> | Bewerking | Microsoft.Network/localnetworkgateways/read | Gets LocalNetworkGateway |
> | Bewerking | Microsoft.Network/localnetworkgateways/write | Hiermee maakt of een bestaande LocalNetworkGateway werkt |
> | Bewerking | Microsoft.Network/locations/availableDelegations/read | Beschikbare overdrachten opgehaald |
> | Bewerking | Microsoft.Network/locations/availablePrivateEndpointResources/read | Beschikbare privé-eindpunt ophaalt-resources |
> | Bewerking | Microsoft.Network/locations/bareMetalTenants/action | Kan worden toegewezen of valideert van een Bare Metal-Tenant |
> | Bewerking | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Controleert of ondersteuning voor versnelde netwerken |
> | Bewerking | Microsoft.Network/locations/checkDnsNameAvailability/read | Controleert of de dns-label, op de opgegeven locatie beschikbaar is |
> | Bewerking | Microsoft.Network/locations/operationResults/read | Het resultaat van een asynchrone POST of DELETE-bewerking opgehaald |
> | Bewerking | Microsoft.Network/locations/operations/read | Bewerkingsresource met de status van een asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.Network/locations/serviceTags/read | Servicetags ophalen |
> | Bewerking | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Haalt de grootte van virtuele machines ondersteund |
> | Bewerking | Microsoft.Network/locations/usages/read | Metrische gegevens over gebruik voor de resources opgehaald |
> | Bewerking | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Een lijst met beschikbare Services voor virtueel netwerk-eindpunt ophalen |
> | Bewerking | Microsoft.Network/networkIntentPolicies/delete | Hiermee verwijdert u een intentie netwerkbeleid |
> | Bewerking | Microsoft.Network/networkIntentPolicies/read | Een beschrijving van het netwerk intentie beleid opgehaald |
> | Bewerking | Microsoft.Network/networkIntentPolicies/write | Hiermee maakt u een beleid voor netwerken kunt u lezen wat of een bestaande netwerkbeleid voor doel-updates |
> | Bewerking | Microsoft.Network/networkInterfaces/delete | Hiermee verwijdert u een netwerkinterface |
> | Bewerking | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Netwerkbeveiligingsgroepen geconfigureerde op Network Interface van de Vm ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Routetabel geconfigureerd op de netwerkinterface van de virtuele machine ophalen |
> | Bewerking | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Lid wordt van een IP-configuratie voor de netwerkinterface. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/networkInterfaces/ipconfigurations/read | Hiermee haalt u de definitie van de IP-configuratie voor een netwerk-interface.  |
> | Bewerking | Microsoft.Network/networkInterfaces/join/action | Een virtuele Machine koppelt aan een netwerkinterface. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/networkInterfaces/loadBalancers/read | Alle load balancers die de netwerkinterface onderdeel van is opgehaald |
> | Bewerking | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Bewerking | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Hiermee verwijdert u een configuratie voor de netwerkinterface te tikken. |
> | Bewerking | Microsoft.Network/networkInterfaces/tapConfigurations/read | Hiermee haalt u een configuratie voor de netwerkinterface te tikken. |
> | Bewerking | Microsoft.Network/networkInterfaces/tapConfigurations/write | Hiermee maakt u een Netwerkinterfaceconfiguratie voor tikt u op of een bestaande Tik Netwerkinterfaceconfiguratie bijgewerkt. |
> | Bewerking | Microsoft.Network/networkInterfaces/write | Hiermee maakt u een netwerkinterface gemaakt of bijgewerkt van een bestaande netwerkinterface.  |
> | Bewerking | Microsoft.Network/networkProfiles/delete | Hiermee verwijdert u een netwerkprofiel |
> | Bewerking | Microsoft.Network/networkProfiles/read | Een netwerkprofiel opgehaald |
> | Bewerking | Microsoft.Network/networkProfiles/removeContainers/action | Hiermee verwijdert u Containers |
> | Bewerking | Microsoft.Network/networkProfiles/setContainers/action | Hiermee stelt u Containers |
> | Bewerking | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Hiermee stelt u Container-netwerkinterfaces |
> | Bewerking | Microsoft.Network/networkProfiles/write | Hiermee maakt of een netwerkprofiel werkt |
> | Bewerking | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Definitie van een standaard beveiligingsregels opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/delete | Hiermee wordt een netwerkbeveiligingsgroep verwijderd |
> | Bewerking | Microsoft.Network/networkSecurityGroups/join/action | Een netwerkbeveiligingsgroep koppelt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/networkSecurityGroups/read | De definitie van een network security opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/delete | Hiermee verwijdert u een beveiligingsregel |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/read | Een definitie van beveiligingsregels opgehaald |
> | Bewerking | Microsoft.Network/networkSecurityGroups/securityRules/write | Hiermee maakt u een regel of een beveiligingsregel voor bestaande updates |
> | Bewerking | Microsoft.Network/networkSecurityGroups/write | Hiermee een netwerkbeveiligingsgroep maken of bijwerken van een bestaande netwerkbeveiligingsgroep |
> | Bewerking | Microsoft.Network/networkWatchers/availableProvidersList/action | Retourneert alle beschikbare internet serviceproviders voor een opgegeven Azure-regio. |
> | Bewerking | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Retourneert de score van de relatieve latentie voor internet serviceproviders van een opgegeven locatie naar Azure-regio's. |
> | Bewerking | Microsoft.Network/networkWatchers/configureFlowLog/action | Hiermee configureert u een stroom logboekregistratie voor een doelbron. |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/delete | Hiermee verwijdert u een nieuwe Verbindingscontrole |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Query verbindingen tussen de opgegeven eindpunten controleren |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/read | Details van de Verbindingsmonitor ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Beginnen met controleren van de connectiviteit tussen de opgegeven eindpunten |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Stoppen/onderbroken verbindingen tussen de opgegeven eindpunten controleren |
> | Bewerking | Microsoft.Network/networkWatchers/connectionMonitors/write | Hiermee maakt u een nieuwe Verbindingscontrole |
> | Bewerking | Microsoft.Network/networkWatchers/connectivityCheck/action | Controleert of de mogelijkheid tot stand brengen van een rechtstreekse TCP-verbinding van een virtuele machine naar een bepaald eindpunt, met inbegrip van een andere virtuele machine of een willekeurige externe server. |
> | Bewerking | Microsoft.Network/networkWatchers/delete | Hiermee verwijdert u een network watcher |
> | Bewerking | Microsoft.Network/networkWatchers/ipFlowVerify/action | Retourneert of het pakket wordt toegestaan of vanaf of naar een bepaald doel verboden. |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/delete | Hiermee verwijdert u een Lens |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/query/action | Bewaking van netwerkverkeer op een opgegeven eindpunt query |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/read | Details van de Lens ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/start/action | De bewaking van netwerkverkeer op een opgegeven eindpunt starten |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/stop/action | Bewaking van netwerkverkeer op een opgegeven eindpunt stoppen/onderbroken |
> | Bewerking | Microsoft.Network/networkWatchers/lenses/write | Hiermee maakt u een Lens |
> | Bewerking | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Diagnose van de configuratie van het netwerk. |
> | Bewerking | Microsoft.Network/networkWatchers/nextHop/action | Voor een opgegeven doel en de doel-IP-adres, het volgende hoptype retourneren en vervolgens hopen dat IP-adres. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/delete | Hiermee verwijdert u een pakketopname |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Hiermee haalt u informatie over de eigenschappen en de status van de resource van een pakket vastleggen. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/read | De definitie van de packet-capture ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Stop de actieve sessie van pakket vastleggen. |
> | Bewerking | Microsoft.Network/networkWatchers/packetCaptures/write | Hiermee maakt u een pakketopname |
> | Bewerking | Microsoft.Network/networkWatchers/pingMeshes/delete | Hiermee verwijdert u een PingMesh |
> | Bewerking | Microsoft.Network/networkWatchers/pingMeshes/read | Details van PingMesh ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/pingMeshes/start/action | PingMesh tussen de opgegeven virtuele machines starten |
> | Bewerking | Microsoft.Network/networkWatchers/pingMeshes/stop/action | PingMesh tussen de opgegeven virtuele machines stoppen |
> | Bewerking | Microsoft.Network/networkWatchers/pingMeshes/write | Hiermee maakt u een PingMesh |
> | Bewerking | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Batchquery verbindingen tussen de opgegeven eindpunten controleren |
> | Bewerking | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Hiermee wordt de status van de stroom zich aanmelden op een resource opgehaald. |
> | Bewerking | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Het oplossen van problemen met resultaat van de eerder uitgevoerde of momenteel haalt probleemoplossing bewerking uitgevoerd. |
> | Bewerking | Microsoft.Network/networkWatchers/read | De definitie van de netwerk-watcher ophalen |
> | Bewerking | Microsoft.Network/networkWatchers/securityGroupView/action | Bekijk de geconfigureerde en toegepaste netwerkbeveiligingsgroepsregels regels voor netwerkbeveiligingsgroepen toegepast op een virtuele machine. |
> | Bewerking | Microsoft.Network/networkWatchers/topology/action | Hiermee haalt een weergave voor de netwerk-niveau van resources en hun relaties in een resourcegroep. |
> | Bewerking | Microsoft.Network/networkWatchers/troubleshoot/action | Start het oplossen van een resource netwerken in Azure. |
> | Bewerking | Microsoft.Network/networkWatchers/write | Hiermee maakt u een network watcher of een bestaande netwerk-watcher-updates |
> | Bewerking | Microsoft.Network/operations/read | Beschikbare bewerkingen ophalen |
> | Bewerking | Microsoft.Network/p2sVpnGateways/delete | Hiermee verwijdert u een P2SVpnGateway. |
> | Bewerking | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | Vpn-profiel voor P2SVpnGateway genereren |
> | Bewerking | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Hiermee haalt u de status van een P2S-Vpn-verbinding voor P2SVpnGateway |
> | Bewerking | Microsoft.Network/p2sVpnGateways/read | Hiermee haalt u een P2SVpnGateway. |
> | Bewerking | Microsoft.Network/p2sVpnGateways/write | Hebt u een P2SVpnGateway. |
> | Bewerking | Microsoft.Network/privateEndpoints/delete | Hiermee verwijdert u een persoonlijke endpoint-resource. |
> | Bewerking | Microsoft.Network/privateEndpoints/read | Hiermee haalt u een persoonlijke endpoint-resource. |
> | Bewerking | Microsoft.Network/privateEndpoints/write | Hiermee maakt u een nieuwe persoonlijke eindpunt of een bestaande persoonlijke eindpunt bijgewerkt. |
> | Bewerking | Microsoft.Network/privateLinkServices/delete | Hiermee verwijdert u de bron van een persoonlijke verbinding-service. |
> | Bewerking | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Hiermee verwijdert u een persoonlijke eindpunt-verbinding. |
> | Bewerking | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Hiermee haalt u een definitie voor de verbinding persoonlijke eindpunt. |
> | Bewerking | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Hiermee maakt u een nieuwe persoonlijke eindpunt verbinding of updates van een bestaande persoonlijke eindpunt verbinding. |
> | Bewerking | Microsoft.Network/privateLinkServices/read | Hiermee haalt u de bron van een persoonlijke verbinding-service. |
> | Bewerking | Microsoft.Network/privateLinkServices/write | Hiermee maakt u een nieuwe persoonlijke koppelingsservice of particuliere koppeling voor een bestaande service-updates. |
> | Bewerking | Microsoft.Network/publicIPAddresses/delete | Hiermee verwijdert u een openbaar Ip-adres. |
> | Bewerking | Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare ip-adres. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Bewerking | Microsoft.Network/publicIPAddresses/write | Hiermee maakt u een openbaar Ip-adres of een bestaande openbare Ip-adres-updates.  |
> | Bewerking | Microsoft.Network/publicIPPrefixes/delete | Hiermee verwijdert u een openbaar Ip-voorvoegsel |
> | Bewerking | Microsoft.Network/publicIPPrefixes/join/action | Een PublicIPPrefix koppelt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/publicIPPrefixes/read | De definitie van een openbaar Ip-voorvoegsel opgehaald |
> | Bewerking | Microsoft.Network/publicIPPrefixes/write | Hiermee maakt u een openbaar Ip-voorvoegsel of Updates van een bestaande openbare Ip-voorvoegsel |
> | Bewerking | Microsoft.Network/register/action | Hiermee wordt het abonnement geregistreerd |
> | Bewerking | Microsoft.Network/routeFilters/delete | Hiermee verwijdert u de definitie van een route-filter |
> | Bewerking | Microsoft.Network/routeFilters/join/action | Lid wordt van een routefilter. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/routeFilters/read | De definitie van een route-filter opgehaald |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/delete | Hiermee verwijdert u de regeldefinitie van een route-filter |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/read | Met deze eigenschap wordt de regeldefinitie van een route-filter |
> | Bewerking | Microsoft.Network/routeFilters/routeFilterRules/write | Hiermee maakt u een routefilterregel of een bestaande routefilterregel Updates |
> | Bewerking | Microsoft.Network/routeFilters/write | Hiermee maakt u een routefilter of een bestaande routefilter werkt |
> | Bewerking | Microsoft.Network/routeTables/delete | Hiermee verwijdert u de definitie van een route-table |
> | Bewerking | Microsoft.Network/routeTables/join/action | Een routetabel koppelt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/routeTables/read | De definitie van een route-tabel opgehaald |
> | Bewerking | Microsoft.Network/routeTables/routes/delete | Hiermee verwijdert u de definitie van een route |
> | Bewerking | Microsoft.Network/routeTables/routes/read | De definitie van een route opgehaald |
> | Bewerking | Microsoft.Network/routeTables/routes/write | Hiermee maakt u een route of een bestaande route-Updates |
> | Bewerking | Microsoft.Network/routeTables/write | Hiermee maakt u een routetabel gemaakt of een bestaande routetabel bijgewerkt |
> | Bewerking | Microsoft.Network/securegateways/delete | Beveiligde Gateway verwijderen |
> | Bewerking | Microsoft.Network/securegateways/read | Beveiligde Gateway ophalen |
> | Bewerking | Microsoft.Network/securegateways/write | Hiermee maken of bijwerken van een Gateway beveiligen |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/delete | Hiermee verwijdert u een Service-eindpuntbeleid |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/join/action | Een Service-eindpuntbeleid koppelt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Een Subnet koppelt aan beleidsregels voor Service-eindpunt. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/read | Een beschrijving van het Service-eindpunt beleid opgehaald |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Hiermee verwijdert u een Service-Eindpuntbeleidsdefinitie |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Een beschrijving van de definitie van het Service-eindpunt beleid opgehaald |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Hiermee maakt u een Service-Eindpuntbeleidsdefinitie of een bestaande Service-Eindpuntbeleidsdefinitie updates |
> | Bewerking | Microsoft.Network/serviceEndpointPolicies/write | Hiermee maakt u een Service-eindpuntbeleid of een bestaande Service-eindpuntbeleid updates |
> | Bewerking | Microsoft.Network/trafficManagerGeographicHierarchies/read | Haalt de Traffic Manager geografisch hiërarchie met regio's die kunnen worden gebruikt met de geografische verkeersrouteringsmethode |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Hiermee verwijdert u een Azure-eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager wordt niet meer routeren van verkeer naar de verwijderde Azure-eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Hiermee haalt u een Azure-eindpunt dat tot een Traffic Manager-profiel behoort, met inbegrip van alle eigenschappen van dit Azure-eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Een nieuw Azure-eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande Azure-eindpunt in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/delete | Traffic Manager-profiel verwijderen. Alle instellingen die zijn gekoppeld aan het Traffic Manager-profiel, gaan verloren en het profiel kan niet meer worden gebruikt om verkeer te routeren. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Hiermee verwijdert u een extern eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager wordt niet meer routeren van verkeer naar het externe eindpunt is verwijderd. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Hiermee haalt u een extern eindpunt die deel uitmaakt van een Traffic Manager-profiel, met inbegrip van alle eigenschappen van het externe eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Een nieuwe Extern eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaand extern eindpunt in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Hiermee haalt de Traffic Manager-Heatmap voor de opgegeven Traffic Manager-profiel waarin de aantallen en latentie gegevens opvragen per locatie en de bron-IP. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Hiermee verwijdert u een genest eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager wordt niet meer routeren van verkeer naar de verwijderde genest eindpunt. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Hiermee haalt u een genest eindpunt die deel uitmaakt van een Traffic Manager-profiel, met inbegrip van alle eigenschappen van dit eindpunt is genest. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Een nieuw genest eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande genest eindpunt in dat Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/read | Ophalen van de profielconfiguratie van het Traffic Manager. Dit omvat de DNS-instellingen, instellingen voor routering van verkeer, controle-instellingen van endpoint en de lijst met eindpunten doorgestuurd door dit Traffic Manager-profiel. |
> | Bewerking | Microsoft.Network/trafficManagerProfiles/write | Een Traffic Manager-profiel maken of wijzigen van de configuratie van een bestaand Traffic Manager-profiel.<br>Dit omvat het inschakelen of uitschakelen van een profiel en DNS-instellingen, instellingen voor routering van verkeer of eindpunt controle-instellingen wijzigen.<br>Eindpunten doorgestuurd door de Traffic Manager-profiel kunnen worden toegevoegd, verwijderd, ingeschakeld of uitgeschakeld. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Hiermee verwijdert u het abonnementsniveau-sleutel die wordt gebruikt voor de verzameling van Realtime-metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/read | Hiermee haalt u het abonnementsniveau-sleutel die wordt gebruikt voor de verzameling van Realtime-metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/trafficManagerUserMetricsKeys/write | Hiermee maakt u een nieuwe sleutel abonnement moet worden gebruikt voor de verzameling van Realtime-metrische gegevens over gebruikers. |
> | Bewerking | Microsoft.Network/unregister/action | De registratie van het abonnement |
> | Bewerking | Microsoft.Network/virtualHubs/delete | Hiermee verwijdert u een virtuele Hub |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Hiermee verwijdert u een HubVirtualNetworkConnection |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Een HubVirtualNetworkConnection ophalen |
> | Bewerking | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Maken of bijwerken van een HubVirtualNetworkConnection |
> | Bewerking | Microsoft.Network/virtualHubs/read | Een virtuele Hub ophalen |
> | Bewerking | Microsoft.Network/virtualHubs/write | Maken of bijwerken van een virtuele Hub |
> | Bewerking | microsoft.network/virtualnetworkgateways/connections/read | Get VirtualNetworkGatewayConnection |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/delete | Hiermee verwijdert u een virtualNetworkGateway |
> | Bewerking | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | VpnClient-pakket voor virtualNetworkGateway genereren |
> | Bewerking | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Genereren van VpnProfile-pakket voor VirtualNetworkGateway |
> | Bewerking | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Haalt virtualNetworkGateway aangekondigde routes |
> | Bewerking | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Met deze eigenschap virtualNetworkGateway bgp-peer-status |
> | Bewerking | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Virtualnetworkgateway geleerde routes opgehaald |
> | Bewerking | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Per status van Vpn-Client de verbinding voor VirtualNetworkGateway ophalen |
> | Bewerking | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Get Vpnclient Ipsec-parameters voor VirtualNetworkGateway P2S-client. |
> | Bewerking | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Hiermee haalt u de URL van het hulpprogramma voor het profiel van een vooraf gegenereerde vpn-clientpakket |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/read | Een VirtualNetworkGateway opgehaald |
> | Bewerking | microsoft.network/virtualnetworkgateways/reset/action | Hiermee stelt u een virtualNetworkGateway |
> | Bewerking | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Opnieuw instellen zonder gedeelde sleutel voor VirtualNetworkGateway P2S-client. |
> | Bewerking | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Stel Vpnclient Ipsec-parameters voor VirtualNetworkGateway P2S-client. |
> | Bewerking | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Een lijst met ondersteunde Vpn-apparaten |
> | Bewerking | Microsoft.Network/virtualNetworkGateways/write | Hiermee maakt of een VirtualNetworkGateway werkt |
> | Bewerking | Microsoft.Network/virtualNetworks/BastionHosts/action | Hiermee haalt bastionhost verwijzingen in een Virtueelnetwerk. |
> | Bewerking | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Hiermee haalt bastionhost verwijzingen in een Virtueelnetwerk. |
> | Bewerking | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Controleer of Ip-adres beschikbaar op de opgegeven virtuele netwerk is |
> | Bewerking | Microsoft.Network/virtualNetworks/delete | Hiermee verwijdert u een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/peer/action | Een virtueel netwerk met een ander virtueel netwerk van collega 's |
> | Bewerking | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/delete | Hiermee verwijdert u een subnet van een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Bereidt van een subnet door toe te passen die nodig zijn netwerkbeleid |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/read | De definitie van een virtueel netwerk subnet opgehaald |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Hiermee haalt u verwijzingen naar alle virtuele machines in een subnet van een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/subnets/write | Hiermee maakt u een virtueel netwerksubnet of updates van het subnet van een bestaande virtuele netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/usages/read | Het gebruik van de IP voor elk subnet van het virtuele netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualMachines/read | Hiermee haalt u verwijzingen naar alle virtuele machines in een virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Hiermee verwijdert u een peering op virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Haalt de definitie van een virtueel netwerk-peering |
> | Bewerking | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Hiermee maakt u een peering op virtueel netwerk of werkt een bestaande peering op virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworks/write | Hiermee maakt u een virtueel netwerk of updates van een bestaand virtueel netwerk |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/delete | Tik op virtueel netwerk verwijderen |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/join/action | Lid wordt van een virtueel netwerk te tikken. Niet Signaleerbare. |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/read | Tik op virtueel netwerk ophalen |
> | Bewerking | Microsoft.Network/virtualNetworkTaps/write | Maken of bijwerken van Virtual Network Tap |
> | Bewerking | Microsoft.Network/virtualWans/delete | Verwijdert een virtuele Wan |
> | Bewerking | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Hiermee verwijdert u een virtuele Wan P2SVpnServerConfiguration |
> | Bewerking | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Een virtuele Wan P2SVpnServerConfiguration opgehaald |
> | Bewerking | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Hiermee maakt u een virtuele Wan P2SVpnServerConfiguration of een bestaande virtuele Wan P2SVpnServerConfiguration updates |
> | Bewerking | Microsoft.Network/virtualWans/read | Get a virtuele Wan |
> | Bewerking | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Haalt ondersteund VirtualWan Security Providers. |
> | Bewerking | Microsoft.Network/virtualWans/virtualHubs/read | Hiermee haalt u alle virtuele Hubs die verwijzen naar een virtueel Wan. |
> | Bewerking | Microsoft.Network/virtualwans/vpnconfiguration/action | Hiermee wordt een Vpn-configuratie opgehaald |
> | Bewerking | Microsoft.Network/virtualWans/vpnSites/read | Hiermee haalt u alle VPN-Sites die verwijzen naar een virtueel Wan. |
> | Bewerking | Microsoft.Network/virtualWans/write | Maken of bijwerken van een virtueel Wan |
> | Bewerking | Microsoft.Network/vpnGateways/delete | Hiermee verwijdert u een VPN-gateway. |
> | Bewerking | microsoft.network/vpngateways/listvpnconnectionshealth/action | Status van de verbinding opgehaald voor alle of een subset van verbindingen op een VPN-gateway |
> | Bewerking | Microsoft.Network/vpnGateways/read | Hiermee haalt u een VPN-gateway. |
> | Bewerking | microsoft.network/vpngateways/reset/action | Hiermee stelt u een VPN-gateway |
> | Bewerking | microsoft.network/vpnGateways/vpnConnections/delete | Hiermee verwijdert u een VPN-verbinding. |
> | Bewerking | microsoft.network/vpnGateways/vpnConnections/read | Hiermee haalt u een VPN-verbinding. |
> | Bewerking | microsoft.network/vpnGateways/vpnConnections/write | Hebt u een VPN-verbinding. |
> | Bewerking | Microsoft.Network/vpnGateways/write | Hebt u een VPN-gateway. |
> | Bewerking | Microsoft.Network/vpnsites/delete | Hiermee verwijdert u een Vpn-Site-resource. |
> | Bewerking | Microsoft.Network/vpnsites/read | Hiermee haalt u een Vpn-Site-resource. |
> | Bewerking | Microsoft.Network/vpnsites/write | Hiermee maakt of een Vpn-Site-resource-updates. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Hiermee wordt gecontroleerd of de naam van een bepaalde Namespace-resource beschikbaar binnen de NotificationHub-service. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | De lijst met verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Namespace verificatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | De verbindingsreeks naar de Namespace ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | De lijst met verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Namespace opnieuw genereren/verificatieregel primaire, de sleutel opgeven die moet worden gegenereerd |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Een Namespace-verificatieregels maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Controleert of de NotificationHub-naam beschikbaar is binnen een Namespace wordt. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/Delete | Namespace-Resource verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | De lijst met verificatieregels voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Verificatieregels voor Notification Hub verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | De verbindingsreeks met de Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | De lijst met verificatieregels voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Notification Hub opnieuw genereren/verificatieregel primaire, de sleutel opgeven die moet worden gegenereerd |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Verificatieregels voor Notification Hub maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Een testpushmelding verzenden. |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Resource voor Notification Hub verwijderen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Lijst met metrische gegevens over Namespace beschrijvingen van resources |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Alle Notification Hub PNS-referenties ophalen. Dit omvat, WNS-, MPNS-, APNS-, GCM- en Baidu-referenties |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Lijst met beschrijvingen van resources voor Notification Hub ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Een Notification Hub maken en de eigenschappen ervan bijwerken. De eigenschappen bevatten voornamelijk PNS-referenties. Verificatieregels en TTL |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/read | De lijst met de resourcebeschrijving Namespace ophalen |
> | Bewerking | Microsoft.NotificationHubs/Namespaces/write | Maak een Namespace-Resource en de eigenschappen ervan bijwerken. Labels en de capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.NotificationHubs/operationResults/read | Retourneert de resultaten van de bewerking voor Notification Hubs-provider |
> | Bewerking | Microsoft.NotificationHubs/operations/read | Retourneert een lijst met ondersteunde bewerkingen voor Notification Hubs-provider |
> | Bewerking | Microsoft.NotificationHubs/register/action | Hiermee wordt het abonnement voor de NotificationHubs-resourceprovider geregistreerd en wordt het maken van naamruimten en NotificationHubs mogelijk |
> | Bewerking | Microsoft.NotificationHubs/unregister/action | De registratie van het abonnement voor de NotificationHubs-resourceprovider en wordt het maken van naamruimten en NotificationHubs mogelijk |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.OffAzure/HyperVSites/clusters/read | Haalt u de eigenschappen van een Hyper-V-cluster |
> | Bewerking | Microsoft.OffAzure/HyperVSites/clusters/write | Hiermee maken of bijwerken van de Hyper-V-cluster |
> | Bewerking | Microsoft.OffAzure/HyperVSites/delete | Hiermee verwijdert u de Hyper-V-site |
> | Bewerking | Microsoft.OffAzure/HyperVSites/hosts/read | Hiermee worden de eigenschappen van een Hyper-V-host opgehaald |
> | Bewerking | Microsoft.OffAzure/HyperVSites/hosts/write | Hiermee maken of bijwerken van de Hyper-V-host |
> | Bewerking | Microsoft.OffAzure/HyperVSites/jobs/read | Hiermee worden de eigenschappen van een Hyper-V-taken opgehaald |
> | Bewerking | Microsoft.OffAzure/HyperVSites/machines/read | Haalt u de eigenschappen van een Hyper-V-machines |
> | Bewerking | Microsoft.OffAzure/HyperVSites/machines/start/action | Hyper-V-machines starten |
> | Bewerking | Microsoft.OffAzure/HyperVSites/machines/stop/action | Stopt de Hyper-V-machines |
> | Bewerking | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Hiermee worden de eigenschappen van de status van een Hyper-V-bewerking opgehaald |
> | Bewerking | Microsoft.OffAzure/HyperVSites/read | Hiermee worden de eigenschappen van een Hyper-V-site opgehaald |
> | Bewerking | Microsoft.OffAzure/HyperVSites/refresh/action | Hiermee vernieuwt u de objecten op een Hyper-V-site |
> | Bewerking | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Hiermee worden de eigenschappen van een Hyper-V uitvoeren als-accounts opgehaald |
> | Bewerking | Microsoft.OffAzure/HyperVSites/usage/read | Met deze eigenschap wordt het gebruik van een Hyper-V-site |
> | Bewerking | Microsoft.OffAzure/HyperVSites/write | Hiermee maken of bijwerken van de Hyper-V-site |
> | Bewerking | Microsoft.OffAzure/Operations/read | Leest de beschikbare bewerkingen |
> | Bewerking | Microsoft.OffAzure/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.OffAzure |
> | Bewerking | Microsoft.OffAzure/VMwareSites/delete | Hiermee verwijdert u de VMware-site |
> | Bewerking | Microsoft.OffAzure/VMwareSites/jobs/read | Hiermee worden de eigenschappen van een VMware-taken opgehaald |
> | Bewerking | Microsoft.OffAzure/VMwareSites/machines/read | Haalt u de eigenschappen van een VMware-machines |
> | Bewerking | Microsoft.OffAzure/VMwareSites/machines/start/action | VMware-machines starten |
> | Bewerking | Microsoft.OffAzure/VMwareSites/machines/stop/action | Stopt de VMware-machines |
> | Bewerking | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Hiermee worden de eigenschappen van de status van een VMware-bewerking opgehaald |
> | Bewerking | Microsoft.OffAzure/VMwareSites/read | Hiermee worden de eigenschappen van een VMware-site opgehaald |
> | Bewerking | Microsoft.OffAzure/VMwareSites/refresh/action | Hiermee vernieuwt u de objecten op een VMware-site |
> | Bewerking | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Hiermee worden de eigenschappen van een VMware run as-accounts opgehaald |
> | Bewerking | Microsoft.OffAzure/VMwareSites/usage/read | Met deze eigenschap wordt het gebruik van een VMware-site |
> | Bewerking | Microsoft.OffAzure/VMwareSites/vcenters/read | Hiermee worden de eigenschappen van een VMware vCenter opgehaald |
> | Bewerking | Microsoft.OffAzure/VMwareSites/vcenters/write | Hiermee maakt of de VMware vCenter-updates |
> | Bewerking | Microsoft.OffAzure/VMwareSites/write | Hiermee maken of bijwerken van de VMware-site |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.OperationalInsights/linkTargets/read | Een lijst met bestaande accounts die niet gekoppeld aan een Azure-abonnement zijn. Als u wilt dit Azure-abonnement koppelen aan een werkruimte, gebruikt u een klant-id die is geretourneerd door deze bewerking in de eigenschap van de klant-id van de bewerking werkruimte maken. |
> | Bewerking | microsoft.operationalinsights/operations/read | Een lijst met alle van de beschikbare OperationalInsights Rest-API-bewerkingen. |
> | Bewerking | microsoft.operationalinsights/register/action | Rergisters het abonnement. |
> | Bewerking | Microsoft.OperationalInsights/register/action | Een abonnement bij een resourceprovider registreren. |
> | Bewerking | microsoft.operationalinsights/unregister/action | De registratie van het abonnement. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoeken met de nieuwe engine. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Zoekschema V2 ophalen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/api/query/action | Zoeken met de nieuwe engine. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Zoekschema V2 ophalen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Configuratiebereik verwijderen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Configuratiebereik ophalen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Het configuratiebereik instellen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/delete | Gegevensbronnen onder een werkruimte verwijderen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/read | Gegevensbronnen onder een werkruimte ophalen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/datasources/write | Gegevensbronnen onder een werkruimte maken/bijwerken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/delete | Hiermee verwijdert u een werkruimte. Als de werkruimte is gekoppeld aan een bestaande werkruimte tijdens de aanmaak vervolgens de werkruimte die deze is gekoppeld niet verwijderd. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/gateways/delete | Hiermee verwijdert u een gateway zijn geconfigureerd voor de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Het certificaat voor serverregistratie voor de werkruimte gegenereerd. Dit certificaat wordt gebruikt om Microsoft System Center Operation Manager verbinden met de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Hiermee wordt een intelligence pack voor een opgegeven werkruimte uitgeschakeld. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Hiermee kunt een intelligence pack voor een opgegeven werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Hiermee worden alle intelligencepacks die zichtbaar zijn voor een opgegeven werkruimte en ook wordt aangegeven of het pack is ingeschakeld of uitgeschakeld voor die werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Verwijderen gekoppelde services onder de opgegeven werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/read | Profiteer van gekoppelde services onder de opgegeven werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/linkedServices/write | Maken/bijwerken gekoppelde services onder de opgegeven werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/listKeys/action | Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/listKeys/read | Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/managementGroups/read | Hiermee haalt de namen en metagegevens voor System Center Operations Manager-beheergroepen die zijn verbonden met deze werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Metrische definities onder werkruimte ophalen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Verwijderen van de gebruiker meldingsinstellingen voor de werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Meldingsinstellingen van de gebruiker voor de werkruimte ophalen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Instellen van de gebruiker meldingsinstellingen voor de werkruimte. |
> | Bewerking | microsoft.operationalinsights/workspaces/operations/read | Hiermee haalt u de status van een bewerking OperationalInsights-werkruimte. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/purge/action | Opgegeven gegevens uit de werkruimte verwijderen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | Gegevens lezen uit de tabel AADDomainServicesAccountLogon |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | Gegevens lezen uit de tabel AADDomainServicesLogonLogoff |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Gegevens lezen uit de tabel ADAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Gegevens lezen uit de tabel ADReplicationResult |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Gegevens lezen uit de tabel ADSecurityAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Alert/read | Gegevens lezen uit de tabel met waarschuwingen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Gegevens lezen uit de tabel AlertHistory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Gegevens lezen uit de tabel AppCenterError |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Gegevens lezen uit de Application Insights-tabel |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Gegevens lezen uit de tabel AuditLogs |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | Gegevens lezen uit de tabel AutoscaleEvaluationsLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | Gegevens lezen uit de tabel AutoscaleScaleActionsLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | Gegevens lezen uit de tabel AWSCloudTrail |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Gegevens lezen uit de tabel AzureActivity |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Gegevens lezen uit de tabel AzureMetrics |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Gegevens lezen uit de tabel BoundPort |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Gegevens lezen uit de tabel CommonSecurityLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Gegevens lezen uit de tabel ComputerGroup |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Gegevens lezen uit de tabel ConfigurationChange |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Gegevens lezen uit de tabel ConfigurationData |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Gegevens lezen uit de tabel ContainerImageInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Gegevens lezen uit de tabel ContainerInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Gegevens lezen uit de tabel ContainerLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | Gegevens lezen uit de tabel ContainerNodeInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Gegevens lezen uit de tabel ContainerServiceLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | Gegevens lezen uit de tabel DatabricksAccounts |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | Gegevens lezen uit de tabel DatabricksClusters |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | Gegevens lezen uit de tabel DatabricksDBFS |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | Gegevens lezen uit de tabel DatabricksJobs |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | Gegevens lezen uit de tabel DatabricksNotebook |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | Gegevens lezen uit de tabel DatabricksSecrets |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | Gegevens lezen uit de tabel DatabricksSQLPermissions |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | Gegevens lezen uit de tabel DatabricksSSH |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | Gegevens lezen uit de tabel DatabricksTables |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | Gegevens lezen uit de tabel DatabricksWorkspace |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Gegevens lezen uit de tabel DeviceAppCrash |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Gegevens lezen uit de tabel DeviceAppLaunch |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Gegevens lezen uit de tabel DeviceCalendar |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Gegevens lezen uit de tabel DeviceCleanup |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Gegevens lezen uit de tabel DeviceConnectSession |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Gegevens lezen uit de tabel DeviceEtw |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Gegevens lezen uit de tabel DeviceHardwareHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Gegevens lezen uit de tabel de apparaatstatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Gegevens lezen uit de tabel DeviceHeartbeat |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Gegevens lezen uit de tabel DeviceSkypeHeartbeat |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Gegevens lezen uit de tabel DeviceSkypeSignIn |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Gegevens lezen uit de tabel DeviceSleepState |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Gegevens lezen uit de tabel DHAppFailure |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Gegevens lezen uit de tabel DHAppReliability |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Gegevens lezen uit de tabel DHDriverReliability |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Gegevens lezen uit de tabel DHLogonFailures |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Gegevens lezen uit de tabel DHLogonMetrics |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Gegevens lezen uit de tabel DHOSCrashData |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Gegevens lezen uit de tabel DHOSReliability |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Gegevens lezen uit de tabel DHWipAppLearning |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Gegevens lezen uit de tabel DnsEvents |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Gegevens lezen uit de tabel DnsInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Gegevens lezen uit de tabel ETWEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Event/read | Gegevens lezen uit de tabel gebeurtenis |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel ExchangeOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Lezen van gegevens van de Heartbeat-tabel |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | Gegevens lezen uit de tabel HuntingBookmark |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Gegevens lezen uit de tabel IISAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Gegevens lezen uit de tabel InboundConnection |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | Gegevens lezen uit de tabel InsightsMetrics |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Gegevens lezen uit de tabel IntuneAuditLogs |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Gegevens lezen uit de tabel IntuneOperationalLogs |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Gegevens lezen uit de tabel KubeEvents |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Gegevens lezen uit de tabel KubeNodeInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Gegevens lezen uit de tabel KubePodInventory |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Gegevens lezen uit de tabel KubeServices |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Gegevens lezen uit de tabel LinuxAuditLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Gegevens lezen uit de tabel MAApplication |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Gegevens lezen uit de tabel MAApplicationHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Gegevens lezen uit de tabel MAApplicationHealthAlternativeVersions |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Gegevens lezen uit de tabel MAApplicationHealthIssues |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Gegevens lezen uit de tabel MAApplicationInstance |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Gegevens lezen uit de tabel MAApplicationInstanceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Gegevens lezen uit de tabel MAApplicationReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Gegevens lezen uit de tabel MADeploymentPlan |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Gegevens lezen uit de tabel MADevice |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Gegevens lezen uit de tabel MADeviceNotEnrolled |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | Gegevens lezen uit de tabel MADeviceNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Gegevens lezen uit de tabel MADevicePnPHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Gegevens lezen uit de tabel MADevicePnPHealthAlternativeVersions |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Gegevens lezen uit de tabel MADevicePnPHealthIssues |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Gegevens lezen uit de tabel MADeviceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Gegevens lezen uit de tabel MADriverInstanceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Gegevens lezen uit de tabel MADriverReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Gegevens lezen uit de tabel MAOfficeAddin |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Gegevens lezen uit de tabel MAOfficeAddinEntityHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Gegevens lezen uit de tabel MAOfficeAddinHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Gegevens lezen uit de tabel MAOfficeAddinHealthEventNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Gegevens lezen uit de tabel MAOfficeAddinHealthIssues |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Gegevens lezen uit de tabel MAOfficeAddinInstance |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinInstanceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Gegevens lezen uit de tabel MAOfficeAddinReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Gegevens lezen uit de tabel MAOfficeApp |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | Gegevens lezen uit de tabel MAOfficeAppCrashesNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Gegevens lezen uit de tabel MAOfficeAppHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Gegevens lezen uit de tabel MAOfficeAppInstance |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | Gegevens lezen uit de tabel MAOfficeAppInstanceHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Gegevens lezen uit de tabel MAOfficeAppReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | Gegevens lezen uit de tabel MAOfficeAppSessionsNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Gegevens lezen uit de tabel MAOfficeBuildInfo |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessment |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAOfficeCurrencyAssessmentDailyCounts |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Gegevens lezen uit de tabel MAOfficeDeploymentStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | Gegevens lezen uit de tabel MAOfficeDeploymentStatusNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | Gegevens lezen uit de tabel MAOfficeMacroErrorNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | Gegevens lezen uit de tabel MAOfficeMacroGlobalHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Gegevens lezen uit de tabel MAOfficeMacroHealth |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Gegevens lezen uit de tabel MAOfficeMacroHealthIssues |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueInstanceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Gegevens lezen uit de tabel MAOfficeMacroIssueReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Gegevens lezen uit de tabel MAOfficeMacroSummary |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Gegevens lezen uit de tabel MAOfficeSuite |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Gegevens lezen uit de tabel MAOfficeSuiteInstance |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Gegevens lezen uit de tabel MAProposedPilotDevices |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Gegevens lezen uit de tabel MAWindowsBuildInfo |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessment |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Gegevens lezen uit de tabel MAWindowsCurrencyAssessmentDailyCounts |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Gegevens lezen uit de tabel MAWindowsDeploymentStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | Gegevens lezen uit de tabel MAWindowsDeploymentStatusNRT |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Gegevens lezen uit de tabel MAWindowsSysReqInstanceReadiness |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebW3CLog/read | Gegevens lezen uit de tabel MicrosoftWebW3CLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Gegevens lezen uit de tabel NetworkMonitoring |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Gegevens lezen uit de tabel OfficeActivity |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Operation/read | Gegevens lezen uit de tabel |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Gegevens lezen uit de tabel OutboundConnection |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Perf/read | Gegevens lezen uit de tabel voor prestaties |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Gegevens lezen uit de tabel ProtectionStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/read | Query's uitvoeren op de gegevens in de werkruimte |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ReservedAzureCommonFields/read | Gegevens lezen uit de tabel ReservedAzureCommonFields |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Gegevens lezen uit de tabel ReservedCommonFields |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCCMAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Gegevens lezen uit de tabel SCOMAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Gegevens lezen uit de tabel SecurityAlert |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Gegevens lezen uit de tabel SecurityBaseline |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Gegevens lezen uit de tabel SecurityBaselineSummary |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Gegevens lezen uit de tabel SecurityDetection |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Gegevens lezen uit de tabel SecurityEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | Gegevens lezen uit de tabel SecurityIoTRawEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | Gegevens lezen uit de tabel SecurityRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Gegevens lezen uit de tabel ServiceFabricOperationalEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableActorEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Gegevens lezen uit de tabel ServiceFabricReliableServiceEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SfBOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Gegevens lezen uit de tabel SharePointOnlineAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Gegevens lezen uit de tabel SigninLogs |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Gegevens lezen uit de tabel SPAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Gegevens lezen uit de tabel SQLAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Gegevens lezen uit de tabel SQLQueryPerformance |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | Gegevens lezen uit de tabel SqlThreatProtectionLoginAudits |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Gegevens lezen uit de tabel SqlVulnerabilityAssessmentResult |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Gegevens lezen uit de Syslog-tabel |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Gegevens lezen uit de tabel SysmonEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Lezen van gegevens uit een aangepaste logboek |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | Gegevens lezen uit de tabel ThreatIntelligenceIndicator |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Gegevens lezen uit de tabel UAApp |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Gegevens lezen uit de tabel UAComputer |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Gegevens lezen uit de tabel UAComputerRank |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Gegevens lezen uit de tabel UADriver |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Gegevens lezen uit de tabel UADriverProblemCodes |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Gegevens lezen uit de tabel UAFeedback |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Gegevens lezen uit de tabel UAHardwareSecurity |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Gegevens lezen uit de tabel UAIESiteDiscovery |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Gegevens lezen uit de tabel UAOfficeAddIn |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Gegevens lezen uit de tabel UAProposedActionPlan |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Gegevens lezen uit de tabel UASysReqIssue |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Gegevens lezen uit de tabel UAUpgradedComputer |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Update/read | Gegevens lezen uit de tabel bijwerken |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Gegevens lezen uit de tabel UpdateRunProgress |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Gegevens lezen uit de tabel UpdateSummary |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/Usage/read | Gegevens lezen uit de tabel Gebruik |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Gegevens lezen uit de tabel VMBoundPort |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Gegevens lezen uit de tabel VMConnection |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Gegevens lezen uit de tabel W3CIISLog |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Gegevens lezen uit de tabel WaaSDeploymentStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Gegevens lezen uit de tabel WaaSInsiderStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Gegevens lezen uit de tabel WaaSUpdateStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Gegevens lezen uit de tabel WDAVStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Gegevens lezen uit de tabel WDAVThreat |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsClientAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Gegevens lezen uit de tabel WindowsEvent |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Gegevens lezen uit de tabel WindowsFirewall |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Gegevens lezen uit de tabel WindowsServerAssessmentRecommendation |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WireData/read | Gegevens lezen uit de tabel WireData |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Gegevens lezen uit de tabel WorkloadMonitoringPerf |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Gegevens lezen uit de tabel WUDOAggregatedStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Gegevens lezen uit de tabel WUDOStatus |
> | Bewerking | Microsoft.OperationalInsights/workspaces/read | Een bestaande werkruimte opgehaald |
> | Bewerking | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | De opgegeven werkruimte gedeelde sleutel opnieuw genereren |
> | Bewerking | microsoft.operationalinsights/workspaces/rules/read | Alle regels voor waarschuwingen ophalen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Hiermee verwijdert u een opgeslagen zoekquery |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/read | Een opgeslagen zoekquery opgehaald |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/results/read | Opgeslagen zoekopdrachten resultaten. Afgeschaft |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Geplande zoekacties verwijderen. |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Geplande zoekacties ophalen. |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Maken of bijwerken van de geplande zoekacties. |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Geplande zoekopdrachten verwijderen. |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Geplande zoekopdrachten ophalen. |
> | Bewerking | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Maken of bijwerken van de geplande zoekopdrachten. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/savedSearches/write | Hiermee maakt u een opgeslagen zoekquery |
> | Bewerking | Microsoft.OperationalInsights/workspaces/schema/read | Hiermee haalt het zoekschema voor de werkruimte.  Het zoekschema bevat de beschikbare velden en hun type. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Bewerking | microsoft.operationalinsights/workspaces/search/read | Zoekresultaten ophalen. Afgeschaft. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Hiermee wordt een opslagconfiguratie verwijderd. Microsoft Operational Insights, deze niet meer gegevens te lezen uit het opslagaccount. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Hiermee wordt een opslagconfiguratie opgehaald. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Hiermee maakt u een nieuwe opslagconfiguratie. Deze configuraties worden gebruikt voor het ophalen van gegevens vanaf een locatie in een bestaand opslagaccount. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Log Search Upgrade vertaling fout voor de werkruimte ophalen |
> | Bewerking | Microsoft.OperationalInsights/workspaces/usages/read | Hiermee haalt gegevens over gebruik voor een werkruimte, inclusief de hoeveelheid gegevens die door de werkruimte wordt gelezen. |
> | Bewerking | Microsoft.OperationalInsights/workspaces/write | Hiermee maakt u een nieuwe werkruimte of koppelingen naar een bestaande werkruimte door op te geven van de klant-id van de bestaande werkruimte. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/delete | Bestaande beheerkoppeling verwijderen |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/read | Bestaande beheerkoppeling ophalen |
> | Bewerking | Microsoft.OperationsManagement/managementAssociations/write | Een nieuwe beheerkoppeling maken |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/delete | Bestaande beheerconfiguratie verwijderen |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/read | Bestaande beheerconfiguratie ophalen |
> | Bewerking | Microsoft.OperationsManagement/managementConfigurations/write | Een nieuwe beheerconfiguratie maken |
> | Bewerking | Microsoft.OperationsManagement/register/action | Een abonnement bij een resourceprovider registreren. |
> | Bewerking | Microsoft.OperationsManagement/solutions/delete | Bestaande OMS-oplossing verwijderen |
> | Bewerking | Microsoft.OperationsManagement/solutions/read | Bestaande OMS-oplossing ophalen |
> | Bewerking | Microsoft.OperationsManagement/solutions/write | Nieuwe OMS-oplossing maken |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.PolicyInsights/asyncOperationResults/read | Hiermee haalt u het resultaat van asynchrone bewerking. |
> | Bewerking | Microsoft.PolicyInsights/operations/read | Haalt ondersteunde bewerkingen op Microsoft.PolicyInsights naamruimte |
> | Bewerking | Microsoft.PolicyInsights/policyEvents/queryResults/action | Gegevens opvragen over Beleidsgebeurtenissen. |
> | Bewerking | Microsoft.PolicyInsights/policyEvents/queryResults/read | Gegevens opvragen over Beleidsgebeurtenissen. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/queryResults/action | Informatie over de statussen van een query. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/queryResults/read | Informatie over de statussen van een query. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/summarize/action | Samenvattingsinformatie over de statussen van het meest recente query. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/summarize/read | Samenvattingsinformatie over de statussen van het meest recente query. |
> | Bewerking | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Hiermee wordt een nieuwe evaluatie van naleving voor de geselecteerde scope geactiveerd. |
> | Bewerking | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Gegevens opvragen over resources die vereist zijn DeployIfNotExists-beleid. |
> | Bewerking | Microsoft.PolicyInsights/register/action | Hiermee wordt de Microsoft Policy Insights-resourceprovider geregistreerd en kunnen er acties erop. |
> | Bewerking | Microsoft.PolicyInsights/remediations/cancel/action | Microsoft Policy te herstellen in uitvoering annuleren. |
> | Bewerking | Microsoft.PolicyInsights/remediations/delete | Beleid voor herstel verwijderen. |
> | Bewerking | Microsoft.PolicyInsights/remediations/listDeployments/read | Geeft een lijst van de implementaties die zijn vereist voor een beleid voor herstel. |
> | Bewerking | Microsoft.PolicyInsights/remediations/read | Get-beleid te herstellen. |
> | Bewerking | Microsoft.PolicyInsights/remediations/write | Maken of bijwerken van Microsoft Policy te herstellen. |
> | Bewerking | Microsoft.PolicyInsights/unregister/action | De registratie van de resourceprovider Policy Insights van Microsoft. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Portal/consoles/delete | Hiermee verwijdert u het exemplaar van Cloud Shell. |
> | Bewerking | Microsoft.Portal/consoles/read | Hiermee leest u het exemplaar van Cloud Shell. |
> | Bewerking | Microsoft.Portal/consoles/write | Maken of bijwerken van een exemplaar van Cloud Shell. |
> | Bewerking | Microsoft.Portal/dashboards/delete | Hiermee verwijdert u het dashboard van het abonnement. |
> | Bewerking | Microsoft.Portal/dashboards/read | Hiermee leest u de dashboards voor het abonnement. |
> | Bewerking | Microsoft.Portal/dashboards/write | Toevoegen of wijzigen van dashboard naar een abonnement. |
> | Bewerking | Microsoft.Portal/register/action | Registreren bij de Portal |
> | Bewerking | Microsoft.Portal/usersettings/delete | Hiermee verwijdert u de instellingen voor Cloud Shell. |
> | Bewerking | Microsoft.Portal/usersettings/read | Hiermee leest u de instellingen voor Cloud Shell. |
> | Bewerking | Microsoft.Portal/usersettings/write | Maken of bijwerken van de gebruikersinstelling voor Cloud Shell. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/delete | Verwijdert de Power BI toegewezen capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/read | Haalt de informatie van de opgegeven toegewezen capaciteit van Power BI. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/resume/action | Hervat de capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/skus/read | Beschikbare SKU-gegevens voor de capaciteit ophalen |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/suspend/action | Hiermee onderbreekt u de capaciteit. |
> | Bewerking | Microsoft.PowerBIDedicated/capacities/write | Hiermee maakt of de opgegeven toegewezen capaciteit van Power BI-updates. |
> | Bewerking | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Hiermee wordt gecontroleerd of de opgegeven toegewezen capaciteit van Power BI naam geldig is en niet wordt gebruikt. |
> | Bewerking | Microsoft.PowerBIDedicated/locations/operationresults/read | Haalt de informatie van het opgegeven bewerkingsresultaat. |
> | Bewerking | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Haalt de informatie van de status van de opgegeven bewerking. |
> | Bewerking | Microsoft.PowerBIDedicated/operations/read | De informatie over bewerkingen opgehaald |
> | Bewerking | Microsoft.PowerBIDedicated/register/action | Power BI toegewezen resourceprovider registreert. |
> | Bewerking | Microsoft.PowerBIDedicated/skus/read | Hiermee wordt de informatie van SKU's opgehaald |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Bewerking | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Bewerking | Microsoft.RecoveryServices/Locations/backupPreValidateProtection/action |  |
> | Bewerking | Microsoft.RecoveryServices/Locations/backupProtectedItem/write | Een beveiligd back-upitem maken |
> | Bewerking | Microsoft.RecoveryServices/Locations/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Locations/backupStatus/action | Controleer back-upstatus voor Recovery Services-kluizen |
> | Bewerking | Microsoft.RecoveryServices/Locations/backupValidateFeatures/action | Functies valideren |
> | Bewerking | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Controleer dat de beschikbaarheid van de Resource-naam is een API om te controleren of de resourcenaam van de beschikbaar is |
> | Bewerking | Microsoft.RecoveryServices/locations/operationStatus/read | Status van de bewerking voor een bepaalde bewerking opgehaald |
> | Bewerking | Microsoft.RecoveryServices/operations/read | Bewerking wordt de lijst met bewerkingen voor een Resourceprovider geretourneerd |
> | Bewerking | Microsoft.RecoveryServices/register/action | Hiermee wordt een abonnement voor een bepaalde Resourceprovider |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupconfig/read | Retourneert-configuratie voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupconfig/write | Configuratie van de updates voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/delete | Een back-Upbeveiligingsintentie verwijderen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Een back-Upbeveiligingsintentie ophalen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Een back-Upbeveiligingsintentie maken |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Status van de bewerking geretourneerd |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Alle beveiligbare containers ophalen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Hiermee verwijdert u de geregistreerde Container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Workloads in een container navragen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Alle items in een container ophalen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hiermee wordt een back-up van het beveiligde item gemaakt. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Hiermee wordt een beveiligd Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Direct Itemherstel inrichten voor beveiligd Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Herstelpunten voor beveiligde items herstellen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Direct Itemherstel intrekken voor beveiligd Item |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Hiermee maakt u een geregistreerde container |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de containerlijst |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | De taak annuleren |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobs/read | Alle taakobjecten geretourneerd |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupOperations/read | Retourneert de back-upbewerking Status voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Een beveiligingsbeleid verwijderd |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Status van de beleidsbewerking ophalen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Hiermee maakt u beleid voor beveiliging |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers van het abonnement |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Lijst van alle back-up Protection Intents |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Beveiligingspincode retourneert informatie over Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Retourneert de opslagconfiguratie voor Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | De opslagconfiguratie updates voor de Recovery Services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Bewerking op de beveiligde Item valideren |
> | Bewerking | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/delete | De opgegeven Azure-resource van het type 'kluis' Hiermee verwijdert u de bewerking kluis verwijderen |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Oplossing voor de waarschuwing. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Hiermee haalt u de configuratie voor meldingen van Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Hiermee configureert u een e-mailmeldingen op Recovery services-kluis. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | De bewerking servicecontainer kan worden gebruikt om de registratie van een container te. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking servicecontainer registreren kan worden gebruikt voor het registreren van een container met de Recovery-Service. |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lezen van alle instellingen voor waarschuwingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Maken of bijwerken van eventuele instellingen voor waarschuwingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Alle configuraties verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Installatiekopie van de processerver implementeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Fabric migreren naar AAD |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Infrastructuur verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat vernieuwen voor de infrastructuur |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Lezen van logische netwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Netwerktoewijzingen verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Maken of een Netwerktoewijzingen bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Beveiligbaar Item detecteren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Beveiligingscontainer verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Verwijder alle Items voor migratie |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Migreren van Item |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Lezen van alle herstelpunten voor migratie |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Alle Items die migratie lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Test migreren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Test migreren opschonen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Een migratie-artikelen maken of bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare Items lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Schijven toevoegen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Verwijder alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lezen van alle herstelpunten voor replicatie |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Beveiligd Item verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Schijven verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Replicatie herstellen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligd Item opnieuw beveiligen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Feedback verzenden |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Lees elk doel Compute-grootten |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Failovertest opschonen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility-Service bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Maken of bijwerken van alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Alle Beveiligingscontainertoewijzingen verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Toewijzing van Beveiligingscontainer verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Maken of bijwerken van eventuele Beveiligingscontainertoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Switch-Beveiligingscontainer |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Maken of bijwerken van alle Beveiligingscontainers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Verwijderen van een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Provider vernieuwen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Recovery Services-Provider verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Maken of bijwerken van een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Verwijderen van eventuele Opslagclassificatietoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Maken of bijwerken van eventuele Opslagclassificatietoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Alle vCenters verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenters lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Maken of bijwerken van eventuele vCenters |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Maken of bijwerken van eventuele Fabrics |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Taak annuleren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/read | Alle taken lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Taak opnieuw starten |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Taak hervatten |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Alle Items die migratie lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Alle netwerken lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Beleid verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Elk beleid lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Een beleid maken of bijwerken |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Plannen voor herstel verwijderen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstelplan voor failover doorvoeren |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Herstelplan voor geplande Failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lees elk plannen voor herstel |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Herstelplan voor opnieuw beveiligen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Herstelplan voor Failover testen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plan voor herstel van test voor de failovertest |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Herstelplan voor failover |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Maken of bijwerken van eventuele plannen voor herstel |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Alle lezen  |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationUsages/read | Lezen van een kluis-gebruik voor replicatie |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | De status van de replicatie van elke kluis lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Status van de kluis vernieuwen |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Alle lezen  |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Maken of bijwerken  |
> | Bewerking | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Alle vCenters lezen |
> | Bewerking | Microsoft.RecoveryServices/vaults/usages/read | Lezen van een kluis-gebruik |
> | Bewerking | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Bewerking | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Relay/checkNameAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.Relay/checkNamespaceAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. Deze API is gedeprecieerd in plaats daarvan gebruik CheckNameAvailability. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is afgeschaft. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/delete | Namespace verificatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | De verbindingsreeks naar de Namespace ophalen |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/read | De lijst met verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/authorizationRules/write | Een Namespace-verificatieregels maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/Delete | Namespace-Resource verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Haalt de autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Disaster Recovery primaire Namespace van verificatieregels ophalen |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Beschikbaarheid van de naamruimtealias in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. Met deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en het naamruimtealias om te verwijzen naar de secundaire naamruimte. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte opgehaald. |
> | Bewerking | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Hiermee of bijwerken van de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | De bewerking HybridConnection bijwerken. Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. Autorisatieregels. Gebruik een PUT-aanroep om bij te werken autorisatieregel. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Bewerking voor het verwijderen van autorisatieregels HybridConnection |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | De verbindingsreeks naar HybridConnection ophalen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  De lijst met regels voor HybridConnection |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Autorisatieregels HybridConnection maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/Delete | Bewerking HybridConnection Resource verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/read | Lijst met beschrijvingen van resources voor HybridConnection |
> | Bewerking | Microsoft.Relay/namespaces/HybridConnections/write | Maken of bijwerken HybridConnection eigenschappen. |
> | Bewerking | Microsoft.Relay/namespaces/messagingPlan/read | Hiermee wordt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.Relay/namespaces/messagingPlan/write | Werkt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.Relay/namespaces/operationresults/read | De status van Namespace-bewerking ophalen |
> | Bewerking | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met metrische gegevens over Namespace beschrijvingen van resources |
> | Bewerking | Microsoft.Relay/namespaces/read | De lijst met de resourcebeschrijving Namespace ophalen |
> | Bewerking | Microsoft.Relay/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | De bewerking WcfRelay bijwerken. Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. Autorisatieregels. Gebruik een PUT-aanroep om bij te werken autorisatieregel. |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Bewerking om te verwijderen van WcfRelay-autorisatieregels |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | De verbindingsreeks naar WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  De lijst met regels voor WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Autorisatieregels WcfRelay maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/Delete | Bewerking WcfRelay Resource verwijderen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/read | Lijst met beschrijvingen van resources voor WcfRelay ophalen |
> | Bewerking | Microsoft.Relay/namespaces/WcfRelays/write | Maken of bijwerken WcfRelay eigenschappen. |
> | Bewerking | Microsoft.Relay/namespaces/write | Maak een Namespace-Resource en de eigenschappen ervan bijwerken. Labels en de capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.Relay/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.Relay/register/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources |
> | Bewerking | Microsoft.Relay/unregister/action | Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Hiermee haalt u de beschikbaarheidsstatus voor de opgegeven resource |
> | Bewerking | Microsoft.ResourceHealth/AvailabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Bewerking | Microsoft.ResourceHealth/events/read | Get Service Health-gebeurtenissen voor het opgegeven abonnement |
> | Bewerking | Microsoft.Resourcehealth/healthevent/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Activated/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.Resourcehealth/healthevent/InProgress/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Pending/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Resolved/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.Resourcehealth/healthevent/Updated/action | Geeft aan de wijziging in de status van de opgegeven resource |
> | Bewerking | Microsoft.ResourceHealth/impactedResources/read | Betreffende Resources ophalen voor het opgegeven abonnement |
> | Bewerking | Microsoft.ResourceHealth/Operations/read | De bewerkingen die beschikbaar zijn voor Microsoft ResourceHealth ophalen |
> | Bewerking | Microsoft.ResourceHealth/register/action | Hiermee wordt het abonnement voor Microsoft ResourceHealth |
> | Bewerking | Microsoft.ResourceHealth/unregister/action | De registratie van het abonnement voor Microsoft ResourceHealth |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Resources/checkPolicyCompliance/action | Controleer de status van naleving van een bepaalde resource op basis van resource-beleidsregels. |
> | Bewerking | Microsoft.Resources/checkResourceName/action | Controleer de naam van de resource op geldigheid. |
> | Bewerking | Microsoft.Resources/deployments/cancel/action | Een implementatie annuleert. |
> | Bewerking | Microsoft.Resources/deployments/delete | Hiermee verwijdert u een implementatie. |
> | Bewerking | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Bewerking | Microsoft.Resources/deployments/read | Opgehaald of een lijst met implementaties. |
> | Bewerking | Microsoft.Resources/deployments/validate/action | Hiermee valideert u een implementatie. |
> | Bewerking | Microsoft.Resources/deployments/whatIf/action | Voorspelt Sjabloonwijzigingen voor implementatie. |
> | Bewerking | Microsoft.Resources/deployments/write | Hiermee maakt of een implementatie-updates. |
> | Bewerking | Microsoft.Resources/links/delete | Hiermee verwijdert u een resourcekoppeling. |
> | Bewerking | Microsoft.Resources/links/read | Opgehaald of een lijst met resourcekoppelingen. |
> | Bewerking | Microsoft.Resources/links/write | Hiermee of een resourcekoppeling bijgewerkt. |
> | Bewerking | Microsoft.Resources/marketplace/purchase/action | Een resource in de marketplace-aankopen. |
> | Bewerking | Microsoft.Resources/providers/read | De lijst met providers ophalen. |
> | Bewerking | Microsoft.Resources/resources/read | De lijst met resources op basis van filters ophalen. |
> | Bewerking | Microsoft.Resources/subscriptions/locations/read | Hiermee haalt u de lijst met ondersteunde locaties. |
> | Bewerking | Microsoft.Resources/subscriptions/operationresults/read | Resultaten van de bewerking voor het abonnement ophalen. |
> | Bewerking | Microsoft.Resources/subscriptions/providers/read | Opgehaald of geeft een lijst van resourceproviders. |
> | Bewerking | Microsoft.Resources/subscriptions/read | Hiermee haalt u de lijst met abonnementen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/delete | Hiermee verwijdert u een resourcegroep en alle bijbehorende resources. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Opgehaald of een lijst met statussen van implementatiebewerkingen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Opgehaald of een lijst met implementaties. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Hiermee maakt of een implementatie-updates. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Resources verplaatst van een resourcegroep naar een andere. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Bewerking | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Hiermee haalt de resources voor de resourcegroep. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Valideer de verplaatsing van resources van een resourcegroep naar een andere. |
> | Bewerking | Microsoft.Resources/subscriptions/resourceGroups/write | Hiermee of bijwerken van een resourcegroep. |
> | Bewerking | Microsoft.Resources/subscriptions/resources/read | Hiermee haalt u resources van een abonnement. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/delete | Hiermee verwijdert u een abonnementslabel. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/read | Opgehaald of worden abonnement tags. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Hiermee verwijdert u een abonnementslabelwaarde. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Opgehaald of een lijst met tagwaarden abonnement. |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Een abonnementslabelwaarde toevoegen |
> | Bewerking | Microsoft.Resources/subscriptions/tagNames/write | Hiermee voegt u een abonnementslabel toe. |
> | Bewerking | Microsoft.Resources/tags/delete | Hiermee verwijdert u alle tags van een resource. |
> | Bewerking | Microsoft.Resources/tags/read | Hiermee haalt u alle tags van een resource. |
> | Bewerking | Microsoft.Resources/tags/write | De tags van een resource-updates te vervangen of samenvoegen van bestaande tags met een nieuwe set van labels of verwijdering van de bestaande tags. |
> | Bewerking | Microsoft.Resources/tenants/read | Hiermee haalt u de lijst met tenants. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Scheduler/jobcollections/delete | Verwijdert u de taakverzameling. |
> | Bewerking | Microsoft.Scheduler/jobcollections/disable/action | Hiermee schakelt u de taakverzameling. |
> | Bewerking | Microsoft.Scheduler/jobcollections/enable/action | Hiermee schakelt u de taakverzameling. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/delete | Hiermee verwijdert u een taak. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | De definitie van de logische App op basis van een Scheduler-taak genereert. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Haalt de taakgeschiedenis op. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/read | Hiermee haalt u taak. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/run/action | De taak wordt uitgevoerd. |
> | Bewerking | Microsoft.Scheduler/jobcollections/jobs/write | Hiermee maakt of de taak voor het bijwerken. |
> | Bewerking | Microsoft.Scheduler/jobcollections/read | Taakverzameling ophalen |
> | Bewerking | Microsoft.Scheduler/jobcollections/write | Hiermee maakt u taakverzameling of werkt. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Search/checkNameAvailability/action | Controleert de beschikbaarheid van de naam van de service. |
> | Bewerking | Microsoft.Search/operations/read | Een lijst met alle beschikbare bewerkingen van de provider Microsoft.Search. |
> | Bewerking | Microsoft.Search/register/action | Het abonnement voor de search-resourceprovider geregistreerd en wordt het maken van de search-services. |
> | Bewerking | Microsoft.Search/searchServices/createQueryKey/action | Hiermee maakt u de querysleutel. |
> | Bewerking | Microsoft.Search/searchServices/delete | Hiermee verwijdert u de search-service. |
> | Bewerking | Microsoft.Search/searchServices/deleteQueryKey/delete | Hiermee verwijdert u de querysleutel. |
> | Bewerking | Microsoft.Search/searchServices/listAdminKeys/action | Hiermee leest u de beheersleutels. |
> | Bewerking | Microsoft.Search/searchServices/listQueryKeys/read | Retourneert de lijst met API-querysleutels voor de opgegeven Azure Search-service. |
> | Bewerking | Microsoft.Search/searchServices/read | Hiermee leest u de search-service. |
> | Bewerking | Microsoft.Search/searchServices/regenerateAdminKey/action | De beheerder-sleutel opnieuw genereren. |
> | Bewerking | Microsoft.Search/searchServices/start/action | Start de search-service. |
> | Bewerking | Microsoft.Search/searchServices/stop/action | Stopt de search-service. |
> | Bewerking | Microsoft.Search/searchServices/write | Hiermee of bijwerken van de search-service. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Het opgegeven verkeer beperken van regels door overeenkomende beveiligingsregels op de opgegeven netwerkbeveiligingsgroep(en) maken wordt afgedwongen |
> | Bewerking | Microsoft.Security/adaptiveNetworkHardenings/read | Haalt adaptieve netwerk beperken aanbevelingen van een beveiligde resource |
> | Bewerking | Microsoft.Security/advancedThreatProtectionSettings/read | Hiermee haalt u de Advanced Threat Protection instellingen voor de resource |
> | Bewerking | Microsoft.Security/advancedThreatProtectionSettings/write | De instellingen Advanced Threat Protection voor de resource-updates |
> | Bewerking | Microsoft.Security/alerts/read | Alle beschikbare beveiligingswaarschuwingen opgehaald |
> | Bewerking | Microsoft.Security/applicationWhitelistings/read | De toepassing whitelistings opgehaald |
> | Bewerking | Microsoft.Security/applicationWhitelistings/write | Hiermee maakt u een nieuwe opname in de whitelist of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/complianceResults/read | Hiermee haalt u de compliantieresultaten voor de resource |
> | Bewerking | Microsoft.Security/informationProtectionPolicies/read | Beveiligingsbeleid voor apps van de informatie opgehaald voor de resource |
> | Bewerking | Microsoft.Security/informationProtectionPolicies/write | De beleidsregels voor gegevensbeveiliging voor de resource-updates |
> | Bewerking | Microsoft.Security/locations/alerts/activate/action | Een beveiligingswaarschuwing geactiveerd |
> | Bewerking | Microsoft.Security/locations/alerts/dismiss/action | Een beveiligingswaarschuwing negeren |
> | Bewerking | Microsoft.Security/locations/alerts/read | Alle beschikbare beveiligingswaarschuwingen opgehaald |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Hiermee verwijdert u de just-in-time-toegang netwerkbeleid |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Initieert een just-in-time toegang beleid netwerkaanvraag |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Haalt de just-in-time-netwerkbeleid |
> | Bewerking | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Hiermee maakt u een nieuw beleid voor de netwerken van just-in-time-toegang of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/locations/read | Met deze eigenschap wordt de locatie van de beveiliging |
> | Bewerking | Microsoft.Security/locations/tasks/activate/action | Een beveiligingsaanbeveling activeren |
> | Bewerking | Microsoft.Security/locations/tasks/dismiss/action | Een beveiligingsaanbeveling negeren |
> | Bewerking | Microsoft.Security/locations/tasks/read | Alle beschikbare beveiligingsaanbevelingen opgehaald |
> | Bewerking | Microsoft.Security/locations/tasks/resolve/action | Een beveiligingsaanbeveling oplossen |
> | Bewerking | Microsoft.Security/locations/tasks/start/action | Start een beveiligingsaanbeveling |
> | Bewerking | Microsoft.Security/policies/read | Haalt het beveiligingsbeleid |
> | Bewerking | Microsoft.Security/policies/write | Updates van het beveiligingsbeleid |
> | Bewerking | Microsoft.Security/pricings/delete | Hiermee verwijdert u de instellingen voor prijscategorie voor het bereik |
> | Bewerking | Microsoft.Security/pricings/read | De prijzen instellingen voor de scope worden opgehaald |
> | Bewerking | Microsoft.Security/pricings/write | De prijzen instellingen voor de scope-updates |
> | Bewerking | Microsoft.Security/register/action | Hiermee wordt het abonnement voor Azure Security Center |
> | Bewerking | Microsoft.Security/securityContacts/delete | Hiermee verwijdert u de contactpersoon voor beveiliging |
> | Bewerking | Microsoft.Security/securityContacts/read | Haalt de contactpersoon voor beveiliging |
> | Bewerking | Microsoft.Security/securityContacts/write | Updates van de contactpersoon voor beveiliging |
> | Bewerking | Microsoft.Security/securitySolutions/delete | Hiermee verwijdert u een beveiligingsoplossing voor |
> | Bewerking | Microsoft.Security/securitySolutions/read | Haalt de beveiligingsoplossingen |
> | Bewerking | Microsoft.Security/securitySolutions/write | Hiermee maakt u een nieuwe beveiligingsoplossing voor of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/securitySolutionsReferenceData/read | Haalt de beveiligingsoplossingen referentiegegevens |
> | Bewerking | Microsoft.Security/securityStatuses/read | Hiermee haalt u de beveiliging health-statussen voor Azure-resources |
> | Bewerking | Microsoft.Security/securityStatusesSummaries/read | Hiermee haalt u de beveiliging samenvattingen van de statussen voor het bereik |
> | Bewerking | Microsoft.Security/settings/read | De instellingen voor de scope worden opgehaald |
> | Bewerking | Microsoft.Security/settings/write | De instellingen voor de scope-updates |
> | Bewerking | Microsoft.Security/tasks/read | Alle beschikbare beveiligingsaanbevelingen opgehaald |
> | Bewerking | Microsoft.Security/unregister/action | De registratie van het abonnement van Azure Security Center |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/delete | Hiermee verwijdert u een web application firewall |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/read | De web application firewalls opgehaald |
> | Bewerking | Microsoft.Security/webApplicationFirewalls/write | Hiermee maakt u een nieuwe web application firewall of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Security/workspaceSettings/connect/action | Opnieuw verbinden met de instellingen-werkruimte instellingen wijzigen |
> | Bewerking | Microsoft.Security/workspaceSettings/delete | Hiermee verwijdert u de werkruimte-instellingen |
> | Bewerking | Microsoft.Security/workspaceSettings/read | De werkruimte instellingen worden opgehaald |
> | Bewerking | Microsoft.Security/workspaceSettings/write | Instellingen van de werkruimte updates |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/delete | Een diagnostische instelling verwijderen |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/read | Een diagnostische instelling lezen |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettings/write | Een diagnostische instelling schrijven |
> | Bewerking | Microsoft.SecurityGraph/diagnosticsettingscategories/read | De categorieën van een diagnostische instelling lezen |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ServiceBus/checkNameAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.ServiceBus/checkNamespaceAvailability/action | Beschikbaarheid van de naamruimte in bepaald abonnement gecontroleerd. Deze API is gedeprecieerd in plaats daarvan gebruik CheckNameAvailability. |
> | Bewerking | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Hiermee verwijdert u de VNet-regels in Service Bus-Resourceprovider voor het opgegeven VNet |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/action | Updates Namespace-autorisatieregel. Deze API is afgeschaft. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Namespace verificatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd.  |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | De verbindingsreeks naar de Namespace ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/read | De lijst met verificatieregels voor naamruimten ophalen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/authorizationRules/write | Een Namespace-verificatieregels maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/Delete | Namespace-Resource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Haalt de autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Disaster Recovery primaire Namespace van verificatieregels ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Schakelt Disaster Recovery en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Beschikbaarheid van de naamruimtealias in bepaald abonnement gecontroleerd. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Hiermee verwijdert u de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. Met deze bewerking kan alleen worden aangeroepen via de primaire naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Hiermee wordt een GEO DR-failover aangeroepen en het naamruimtealias om te verwijzen naar de secundaire naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Hiermee wordt de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte opgehaald. |
> | Bewerking | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Hiermee of bijwerken van de Disaster Recovery-configuratie die is gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Hiermee verwijdert u het gebeurtenisrasterfilter gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Hiermee wordt het gebeurtenisrasterfilter gekoppeld aan de naamruimte opgehaald. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Hiermee of bijwerken van het gebeurtenisrasterfilter gekoppeld aan de naamruimte. |
> | Bewerking | Microsoft.ServiceBus/namespaces/eventhubs/read | Lijst met beschrijvingen van resources voor EventHub |
> | Bewerking | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | IP-Filter Resource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Ophalen van IP-Filter |
> | Bewerking | Microsoft.ServiceBus/namespaces/ipFilterRules/write | IP-Filter Resource maken |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/browse/action | Berichten bladeren |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/defer/action | Berichten uitstellen |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | Berichten ontvangen |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/schedule/action | Shedule berichten |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | Berichten verzenden |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/setstate/action | Set-sessiestatus |
> | Bewerking | Microsoft.ServiceBus/namespaces/messagingPlan/read | Hiermee wordt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.ServiceBus/namespaces/messagingPlan/write | Werkt het Berichtenplan voor een naamruimte.<br>Deze API is afgeschaft.<br>Eigenschappen van het berichtenplan worden verplaatst naar de (bovenliggende) Namespace-resource in latere API-versies...<br>Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrate/action | Naamruimtebewerking migreren |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Hiermee verwijdert u de configuratie voor de migratie. |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Met deze eigenschap wordt de configuratie voor de migratie waarmee wordt aangegeven van de status van de migratie en replicatiebewerkingen in behandeling |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | De standaard voor de migratie van de premium-naamruimte wordt teruggezet |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | De DNS-server die is gekoppeld aan de standard-naamruimte voor de premium-naamruimte die de migratie is voltooid en stopt met het synchroniseren resources van de standard naar premium-naamruimte wordt toegewezen |
> | Bewerking | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Hiermee maakt u of de configuratie voor de migratie van Updates. Hiermee wordt gestart met het synchroniseren van resources van de standaard naar de premium-naamruimte |
> | Bewerking | Microsoft.ServiceBus/namespaces/networkrulesets/delete | VNET-Regelresource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/networkrulesets/read | NetworkRuleSet Resource opgehaald |
> | Bewerking | Microsoft.ServiceBus/namespaces/networkrulesets/write | Resource voor VNET-regel maken |
> | Bewerking | Microsoft.ServiceBus/namespaces/operationresults/read | De status van Namespace-bewerking ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Lijst met beschrijvingen van resources voor Namespace diagnostische instellingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Lijst met beschrijvingen van resources voor Namespace diagnostische instellingen |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Lijst met Namespace logboeken beschrijvingen van resources |
> | Bewerking | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Lijst met metrische gegevens over Namespace beschrijvingen van resources |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Bewerking voor het bijwerken van de wachtrij. Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. Autorisatieregels. Gebruik een PUT-aanroep om bij te werken autorisatieregel. |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Bewerking voor het verwijderen van wachtrij-autorisatieregels |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | De verbindingsreeks naar de wachtrij ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  De lijst met regels voor wachtrij |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Wachtrij-verificatieregels maken en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/Delete | Bewerking wachtrij Resource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/read | Lijst met beschrijvingen van resources voor wachtrij |
> | Bewerking | Microsoft.ServiceBus/namespaces/queues/write | Maken of Update-wachtrij-eigenschappen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/read | De lijst met de resourcebeschrijving Namespace ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | ACS-naamruimte verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Bewerking voor het bijwerken van onderwerp. Met deze bewerking wordt niet ondersteund in API-versie 2017-04-01. Autorisatieregels. Gebruik een PUT-aanroep om bij te werken autorisatieregel. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Bewerking voor het verwijderen van regels voor onderwerp |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | De verbindingsreeks naar het onderwerp ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  De lijst met regels voor onderwerp ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | De primaire of secundaire sleutel voor de Resource opnieuw genereren |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Maken van autorisatieregels onderwerp en de eigenschappen ervan bijwerken. De toegangsrechten van de autorisatie-regels kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/Delete | Bewerking voor het onderwerp Resource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/read | Lijst met beschrijvingen van resources voor onderwerp |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Bewerking TopicSubscription Resource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Lijst met beschrijvingen van resources voor TopicSubscription ophalen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Bewerking Regelresource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Lijst met beschrijvingen van resources voor regel |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Maken of de eigenschappen van de regel voor het bijwerken. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Maken of bijwerken TopicSubscription eigenschappen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/topics/write | Maken of bijwerken onderwerp eigenschappen. |
> | Bewerking | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | VNET-Regelresource verwijderen |
> | Bewerking | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | VNET-Regelresource opgehaald |
> | Bewerking | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Resource voor VNET-regel maken |
> | Bewerking | Microsoft.ServiceBus/namespaces/write | Maak een Namespace-Resource en de eigenschappen ervan bijwerken. Labels en de capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt. |
> | Bewerking | Microsoft.ServiceBus/operations/read | Bewerkingen ophalen |
> | Bewerking | Microsoft.ServiceBus/register/action | Hiermee wordt het abonnement voor de service bus-resourceprovider geregistreerd en wordt het maken van service bus-resources |
> | Bewerking | Microsoft.ServiceBus/sku/read | Lijst met beschrijvingen van resources voor Sku ophalen |
> | Bewerking | Microsoft.ServiceBus/sku/regions/read | Lijst met beschrijvingen van resources voor SkuRegions ophalen |
> | Bewerking | Microsoft.ServiceBus/unregister/action | Hiermee wordt het abonnement voor de service bus-resourceprovider geregistreerd en wordt het maken van service bus-resources |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/delete | Een toepassing verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/read | Een toepassing lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/delete | Een Service verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Een partitie lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Een Replica lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/read | Een Service lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Een servicestatus lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/services/write | Een Service maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/applications/write | Een toepassing maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Een toepassingstype verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/read | Een toepassingstype lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Versie van een toepassingstype verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Versie van een toepassingstype lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Maken of bijwerken van versie van een toepassingstype |
> | Bewerking | Microsoft.ServiceFabric/clusters/applicationTypes/write | Maken of bijwerken van een toepassingstype |
> | Bewerking | Microsoft.ServiceFabric/clusters/delete | Een Cluster verwijderen |
> | Bewerking | Microsoft.ServiceFabric/clusters/nodes/read | Een knooppunt lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/read | Een Cluster lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/statuses/read | Een clusterstatus lezen |
> | Bewerking | Microsoft.ServiceFabric/clusters/write | Een Cluster maken of bijwerken |
> | Bewerking | Microsoft.ServiceFabric/locations/clusterVersions/read | Een Clusterversie lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Een Clusterversie voor een specifieke omgeving lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/operationresults/read | Bewerkingsresultaten lezen |
> | Bewerking | Microsoft.ServiceFabric/locations/operations/read | Bewerkingen op locatie lezen |
> | Bewerking | Microsoft.ServiceFabric/operations/read | Beschikbare bewerkingen lezen |
> | Bewerking | Microsoft.ServiceFabric/register/action | Een actie registreren |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.SignalRService/locations/checknameavailability/action | Controleert of een naam beschikbaar voor gebruik met een nieuwe SignalR-service is |
> | Bewerking | Microsoft.SignalRService/locations/operationresults/signalr/read | De status van een bewerking asynchorous opvragen |
> | Bewerking | Microsoft.SignalRService/locations/operationstatus/operationId/read |  |
> | Bewerking | Microsoft.SignalRService/locations/usages/read | Het gebruik van het quotum voor Azure SignalR service ophalen |
> | Bewerking | Microsoft.SignalRService/operationresults/read | De status van een bewerking asynchorous opvragen |
> | Bewerking | Microsoft.SignalRService/operationstatus/read |  |
> | Bewerking | Microsoft.SignalRService/register/action | Hiermee wordt de 'Microsoft.SignalRService'-resourceprovider geregistreerd met een abonnement |
> | Bewerking | Microsoft.SignalRService/SignalR/delete | De hele SignalR-service verwijderen |
> | Bewerking | Microsoft.SignalRService/SignalR/listFeatures/read | Lijst met aangepaste FeatureFlags van een SignalR-service. Standaard zijn uitgesloten. |
> | Bewerking | Microsoft.SignalRService/SignalR/listkeys/action | De waarde van SignalR toegangssleutels weergeven in de beheerportal of via API |
> | Bewerking | Microsoft.SignalRService/SignalR/read | De instellingen en configuraties van het SignalR weergeven in de beheerportal of via API |
> | Bewerking | Microsoft.SignalRService/SignalR/regeneratekey/action | Wijzig de waarde van de toegangssleutels SignalR in de beheerportal of via API |
> | Bewerking | Microsoft.SignalRService/SignalR/restart/action | Een Azure SignalR-service opnieuw starten in de beheerportal of via de API. Er zijn bepaalde uitvaltijd. |
> | Bewerking | Microsoft.SignalRService/SignalR/switchFeatures/action | Schakel in- / uitschakelen SignalR FeatureFlags ondersteunde eigenschappen in de beheerportal of via de API. |
> | Bewerking | Microsoft.SignalRService/SignalR/write | Wijzig de instellingen en configuraties in de beheerportal of via de API van het SignalR |
> | Bewerking | Microsoft.SignalRService/unregister/action | De registratie van de resourceprovider 'Microsoft.SignalRService' met een abonnement |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/applicationArtifacts/read | Een lijst met App-artefacten van de definitie van toepassing. |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/delete | Hiermee verwijdert u de definitie van een toepassing. |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/read | Een lijst met toepassingsdefinities opgehaald. |
> | Bewerking | Microsoft.Solutions/applicationDefinitions/write | Toevoegen of wijzigen van de definitie van een toepassing. |
> | Bewerking | Microsoft.Solutions/applications/applicationArtifacts/read | Een lijst met App-artefacten. |
> | Bewerking | Microsoft.Solutions/applications/delete | Hiermee verwijdert u een toepassing. |
> | Bewerking | Microsoft.Solutions/applications/read | Hiermee haalt een lijst met toepassingen. |
> | Bewerking | Microsoft.Solutions/applications/refreshPermissions/action | Hiermee vernieuwt u machtiging(en) van toepassing. |
> | Bewerking | Microsoft.Solutions/applications/updateAccess/action | Toegang tot de toepassing van updates. |
> | Bewerking | Microsoft.Solutions/applications/write | Hiermee wordt een toepassing gemaakt. |
> | Bewerking | Microsoft.Solutions/jitRequests/delete | Een JitRequest verwijderen |
> | Bewerking | Microsoft.Solutions/jitRequests/read | Hiermee wordt een lijst JitRequests opgehaald |
> | Bewerking | Microsoft.Solutions/jitRequests/write | Hiermee maakt u een JitRequest |
> | Bewerking | Microsoft.Solutions/locations/operationStatuses/read | Hiermee wordt de bewerkingsstatus van de resource gelezen. |
> | Bewerking | Microsoft.Solutions/register/action | Registreren bij Solutions. |
> | Bewerking | Microsoft.Solutions/unregister/action | De registratie van oplossingen. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Sql/checkNameAvailability/action | Controleer of de opgegeven server naam beschikbaar is voor het inrichten van over de hele wereld voor een bepaald abonnement. |
> | Bewerking | Microsoft.Sql/instancePools/delete | Hiermee verwijdert u een exemplaar van toepassingen |
> | Bewerking | Microsoft.Sql/instancePools/read | Haalt een exemplaar van toepassingen |
> | Bewerking | Microsoft.Sql/instancePools/usages/read | Een exemplaar van de pool gebruiksinformatie opgehaald |
> | Bewerking | Microsoft.Sql/instancePools/write | Hiermee maken of bijwerken van een exemplaar van toepassingen |
> | Bewerking | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Resultaat van de uitgebreide server blob auditing beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Resultaat van de server blob auditing beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/capabilities/read | Hiermee haalt u de mogelijkheden voor dit abonnement in een bepaalde locatie |
> | Bewerking | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/locations/databaseOperationResults/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Haalt onderhanden bewerkingen op de server verwijderd |
> | Bewerking | Microsoft.Sql/locations/deletedServerOperationResults/read | Haalt onderhanden bewerkingen op de server verwijderd |
> | Bewerking | Microsoft.Sql/locations/deletedServers/read | Retourneert de lijst met verwijderde servers of haalt de eigenschappen voor de opgegeven server verwijderd. |
> | Bewerking | Microsoft.Sql/locations/deletedServers/recover/action | Herstellen van een server verwijderd |
> | Bewerking | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | De azure asynchrone bewerking voor een elastische pool asynchrone bewerking opgehaald |
> | Bewerking | Microsoft.Sql/locations/elasticPoolOperationResults/read | Hiermee haalt u het resultaat van een elastische pool-bewerking. |
> | Bewerking | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Haalt onderhanden bewerkingen op transparent data encryption versleutelingsbeveiliging |
> | Bewerking | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Haalt onderhanden bewerkingen op transparent data encryption versleutelingsbeveiliging |
> | Bewerking | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Resultaat van de uitgebreide server blob auditing beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Resultaat van de uitgebreide server blob auditing beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Hiermee haalt u de status van een firewall-regel-bewerking. |
> | Bewerking | Microsoft.Sql/locations/firewallRulesOperationResults/read | Hiermee haalt u de status van een firewall-regel-bewerking. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/delete | Hiermee verwijdert u een bestaande instantie failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Geplande failover uitgevoerd in een bestaand exemplaar van failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Geforceerde failover uitgevoerd in een bestaand exemplaar van failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/read | Retourneert de lijst met exemplaar failover groepen of haalt u de eigenschappen voor het opgegeven exemplaar van failover-groep. |
> | Bewerking | Microsoft.Sql/locations/instanceFailoverGroups/write | Hiermee maakt u een exemplaar van failover-groep met de opgegeven parameters of de eigenschappen of labels voor de failovergroep voor de opgegeven instantie bijgewerkt. |
> | Bewerking | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | De status van een exemplaar van toepassingen bewerking opgehaald |
> | Bewerking | Microsoft.Sql/locations/instancePoolOperationResults/read | Het resultaat opgehaald voor een bewerking van de groep van exemplaar |
> | Bewerking | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Retourneert de gegevens van een specifieke interface-eindpunt Azure asynchrone bewerking |
> | Bewerking | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Retourneert de gegevens van de opgegeven interface eindpunt profiel bewerking |
> | Bewerking | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Hiermee haalt u de status van een job-agentbewerking. |
> | Bewerking | Microsoft.Sql/locations/jobAgentOperationResults/read | Hiermee haalt u het resultaat van een job-agentbewerking. |
> | Bewerking | Microsoft.Sql/locations/longTermRetentionBackups/read | Geeft een lijst van de back-ups bewaren lange termijn voor elke database op elke server in een locatie |
> | Bewerking | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Geeft een lijst van de back-ups bewaren lange termijn voor elke database op een server |
> | Bewerking | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Hiermee verwijdert u een back-up bewaren lange termijn |
> | Bewerking | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Geeft een lijst van de back-ups bewaren lange termijn voor een database |
> | Bewerking | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Beheerde database restore-bewerking is voltooid |
> | Bewerking | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Haalt lopende bewerkingen op versleutelingsbeveiliging van transparent data encryption beheerd exemplaar |
> | Bewerking | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Haalt lopende bewerkingen op versleutelingsbeveiliging van transparent data encryption beheerd exemplaar |
> | Bewerking | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Haalt lopende bewerkingen op transparante gegevensversleuteling beheerde exemplaar sleutels |
> | Bewerking | Microsoft.Sql/locations/managedInstanceKeyOperationResults/read | Haalt lopende bewerkingen op transparante gegevensversleuteling beheerde exemplaar sleutels |
> | Bewerking | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Opgehaald in lopende bewerkingen op een beheerde database transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Opgehaald in lopende bewerkingen op een beheerde database transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Hiermee haalt u het resultaat voor een persoonlijke eindpunt-verbindingsbewerking proxy |
> | Bewerking | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Hiermee haalt u het resultaat voor een persoonlijke eindpunt-verbindingsbewerking proxy |
> | Bewerking | Microsoft.Sql/locations/read | De beschikbare locaties voor een bepaald abonnement opgehaald |
> | Bewerking | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Haalt onderhanden bewerkingen voor sleutels transparent data encryption-server |
> | Bewerking | Microsoft.Sql/locations/serverKeyOperationResults/read | Haalt onderhanden bewerkingen voor sleutels transparent data encryption-server |
> | Bewerking | Microsoft.Sql/locations/syncAgentOperationResults/read | Resultaat van de synchronisatiebewerking voor de resource van agent ophalen |
> | Bewerking | Microsoft.Sql/locations/syncDatabaseIds/read | De synchronisatie-database-id's voor een bepaalde regio en een abonnement ophalen |
> | Bewerking | Microsoft.Sql/locations/syncGroupOperationResults/read | Resultaat van de synchronisatiebewerking voor de resource van groep ophalen |
> | Bewerking | Microsoft.Sql/locations/syncMemberOperationResults/read | Resultaat van de synchronisatiebewerking voor de resource van lid ophalen |
> | Bewerking | Microsoft.Sql/locations/usages/read | Hiermee haalt u een verzameling van metrische gegevens over gebruik voor dit abonnement in een locatie |
> | Bewerking | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Retourneert de gegevens van de opgegeven virtuele-netwerkregels azure asynchrone bewerking  |
> | Bewerking | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Retourneert de gegevens van de bewerking van de regels opgegeven virtuele netwerk  |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/delete | Hiermee verwijdert u een bestaande beheerder van het beheerde exemplaar. |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/read | Hiermee haalt u een lijst met beheerd exemplaar voor beheerders. |
> | Bewerking | Microsoft.Sql/managedInstances/administrators/write | Hiermee of beheerder beheerd exemplaar bijwerken met de opgegeven parameters. |
> | Bewerking | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Hiermee haalt u een bewaarbeleid voor korte termijn voor een beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Een bewaarbeleid voor de korte termijn voor een beheerde database-updates |
> | Bewerking | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/delete | Hiermee verwijdert u een bestaande beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Hiermee worden de beschikbare logboeken opgehaald voor databases met beheerd exemplaar |
> | Bewerking | Microsoft.Sql/managedInstances/databases/read | Hiermee wordt opgehaald van bestaande beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/read | Het schema van een beheerde database ophalen. |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Een kolom van de beheerde database ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | De gevoeligheid, label van een bepaalde kolom verwijderen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Gevoeligheid aanbevelingen voor een bepaalde kolom uitschakelen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Gevoeligheid aanbevelingen op een bepaalde kolom inschakelen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | De gevoeligheid, label van een bepaalde kolom ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Maken of bijwerken van het gevoeligheidslabel van de van een bepaalde kolom |
> | Bewerking | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Een beheerde database-tabel |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Een lijst met beheerde database threat detection beleidsregels die zijn geconfigureerd voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | De database threat detectiebeleid voor een bepaalde beheerde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/securityEvents/read | Haalt de beheerde database-beveiligingsgebeurtenissen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | De gegevens van de Transparent Data Encryption-database op een bepaalde beheerde database ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | De Transparent Data Encryption-database voor een bepaalde beheerde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | De evaluatie van beveiligingsproblemen voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | De beleidsregels van de evaluatie van beveiligingsproblemen op een givendatabase ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Een bestaande scanresultaat om een mens leesbaar indeling te converteren. Als al er niets gebeurt |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Kwetsbaarheidsscan evaluatie van de database worden uitgevoerd. |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | De lijst met database-beveiligingsprobleem evaluatie scan records retourneren of de record van de scan voor de opgegeven scan-id ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | De evaluatie van beveiligingsproblemen voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/databases/write | Maakt een nieuwe database gemaakt of bijgewerkt van een bestaande database. |
> | Bewerking | Microsoft.Sql/managedInstances/delete | Hiermee verwijdert u een bestaande beheerde exemplaar. |
> | Bewerking | Microsoft.Sql/managedInstances/encryptionProtector/read | Retourneert een lijst van de server encryption Protector of haalt u de eigenschappen voor de opgegeven server versleutelingsbeveiliging. |
> | Bewerking | Microsoft.Sql/managedInstances/encryptionProtector/write | De eigenschappen voor de opgegeven Server Encryption Protector bijgewerkt. |
> | Bewerking | Microsoft.Sql/managedInstances/keys/delete | Hiermee verwijdert u een bestaande Azure SQL Managed Instance-sleutel. |
> | Bewerking | Microsoft.Sql/managedInstances/keys/read | Retourneert de lijst met beheerd exemplaar sleutels of haalt u de eigenschappen voor de sleutel opgegeven beheerde exemplaar. |
> | Bewerking | Microsoft.Sql/managedInstances/keys/write | Hiermee maakt u een sleutel met de opgegeven parameters of de eigenschappen of labels voor de sleutel opgegeven beheerde exemplaar bijwerken. |
> | Bewerking | Microsoft.Sql/managedInstances/metricDefinitions/read | Metrische definities van beheerde exemplaar ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/metrics/read | Beheerd exemplaar van metrische gegevens ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor beheerde exemplaren |
> | Bewerking | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor beheerde exemplaren zijn |
> | Bewerking | Microsoft.Sql/managedInstances/read | Retourneert de lijst met beheerde exemplaren of haalt de eigenschappen voor het opgegeven beheerde exemplaar. |
> | Bewerking | Microsoft.Sql/managedInstances/recoverableDatabases/read | Retourneert een lijst met herstelbare beheerde databases |
> | Bewerking | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Hiermee haalt u een bewaarbeleid voor korte termijn voor een verwijderde beheerde database |
> | Bewerking | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Een bewaarbeleid voor de korte termijn voor een verwijderde beheerde database-updates |
> | Bewerking | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Retourneert dat een lijst van die kunnen worden hersteld beheerde databases verwijderd. |
> | Bewerking | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Een lijst met beheerde server threat detection beleidsregels die zijn geconfigureerd voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/managedInstances/securityAlertPolicies/write | De beheerde server threat detection-beleid voor een bepaalde beheerde server wijzigen |
> | Bewerking | Microsoft.Sql/managedInstances/tdeCertificates/action | Certificaat voor TDE maken/bijwerken |
> | Bewerking | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | De evaluatie van beveiligingsproblemen voor een bepaald beheerd exemplaar verwijderen |
> | Bewerking | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Ophalen van de beleidsregels van de evaluatie beveiligingslek met betrekking tot op een bepaald beheerd exemplaar |
> | Bewerking | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Wijzigen van de evaluatie van beveiligingsproblemen voor een bepaald beheerd exemplaar |
> | Bewerking | Microsoft.Sql/managedInstances/write | Hiermee maakt u een beheerd exemplaar met de opgegeven parameters of de eigenschappen of labels voor de opgegeven beheerde exemplaar bijwerken. |
> | Bewerking | Microsoft.Sql/operations/read | Beschikbare REST-bewerkingen opgehaald |
> | Bewerking | Microsoft.Sql/register/action | Hiermee wordt het abonnement voor de Microsoft SQL Database-resourceprovider geregistreerd en wordt het maken van Microsoft SQL-Databases. |
> | Bewerking | Microsoft.Sql/servers/administratorOperationResults/read | Haalt onderhanden bewerkingen op server-beheerders |
> | Bewerking | Microsoft.Sql/servers/administrators/delete | Serverbeheerder verwijderen |
> | Bewerking | Microsoft.Sql/servers/administrators/read | Details van de server-beheerder ophalen |
> | Bewerking | Microsoft.Sql/servers/administrators/write | Maken of bijwerken van de serverbeheerder |
> | Bewerking | Microsoft.Sql/servers/advisors/read | Retourneert de lijst met adviseurs die beschikbaar zijn voor de server |
> | Bewerking | Microsoft.Sql/servers/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van het opgegeven advisor voor de server |
> | Bewerking | Microsoft.Sql/servers/advisors/recommendedActions/write | De aanbevolen actie toepassen op de server |
> | Bewerking | Microsoft.Sql/servers/advisors/write | Updates uitvoeren automatisch-status van een advisor op serverniveau. |
> | Bewerking | Microsoft.Sql/servers/auditingPolicies/read | Gegevens van de standaard-server-tabel controleren dat is geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.Sql/servers/auditingPolicies/write | Controle van wijzigingen in de standaard server tabel voor een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/operationResults/read | Resultaat van de server blob auditing beleid Set-bewerking ophalen |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/read | Gegevens van de server blob controlebeleid geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.Sql/servers/auditingSettings/write | De controle voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/automaticTuning/read | Retourneert de functie automatisch afstemmen van de instellingen voor de server |
> | Bewerking | Microsoft.Sql/servers/automaticTuning/write | Updates van de instellingen voor automatisch afstemmen van de server en retourneert de bijgewerkte instellingen |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Hiermee verwijdert u een bestaande back-upkluis archivering. |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Met deze bewerking wordt gebruikt om een back-up langdurig bewaren kluis ophalen. Deze retourneert informatie over de kluis geregistreerd met deze server |
> | Bewerking | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Deze bewerking wordt gebruikt voor het registreren van een back-up langetermijnretentie kluis op een server |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/delete | Hiermee verwijdert u een bestaande koppeling van de server-communicatie. |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/read | Retourneert de lijst met communicatiekoppelingen van de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/communicationLinks/write | Maken of bijwerken van de koppeling van een server-communicatie. |
> | Bewerking | Microsoft.Sql/servers/connectionPolicies/read | Retourneert de lijst met beleidsregels voor server-verbinding van de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/connectionPolicies/write | Maken of bijwerken van een beleid van server-verbinding. |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/read | Retourneert de lijst met adviseurs die beschikbaar zijn voor de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van het opgegeven advisor voor de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | De aanbevolen actie toepassen op de database |
> | Bewerking | Microsoft.Sql/servers/databases/advisors/write | Update automatisch-uitvoeren status van een advisor op databaseniveau. |
> | Bewerking | Microsoft.Sql/servers/databases/auditingPolicies/read | De gegevens van de tabel controleren dat is geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/auditingPolicies/write | De tabel controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/auditingSettings/read | De gegevens van de blob controleren dat is geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/auditingSettings/write | De blob-controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/auditRecords/read | De controlerecords voor database-blob ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/automaticTuning/read | Retourneert de instellingen voor automatisch afstemmen voor een database |
> | Bewerking | Microsoft.Sql/servers/databases/automaticTuning/write | De instellingen voor automatisch afstemmen voor een database-updates en bijgewerkte instellingen retourneert |
> | Bewerking | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Retourneert de lijst met back-beleid voor archivering van de opgegeven database. |
> | Bewerking | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Maken of bijwerken van een beleid database back-up voor archivering. |
> | Bewerking | Microsoft.Sql/servers/databases/connectionPolicies/read | Details van de verbindingsbeleid geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/connectionPolicies/write | Verbindingsbeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Retourneert de lijst met beleidsregels voor gegevensmaskering database. |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Gegevens maskeren beleidsregel voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Details van de gegevens maskeren beleidsregel die zijn geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Gegevens maskeren beleidsregel voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Gegevens maskeren beleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Retourneert de stap gedistribueerde query informatie van datawarehouse-query voor de geselecteerde stap-ID |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Retourneert de datawarehouse distributie query-gegevens voor de geselecteerde query-ID |
> | Bewerking | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | De activiteiten van gebruikers van een exemplaar van SQL Data Warehouse met actieve en tijdelijk ingetrokken query's worden opgehaald |
> | Bewerking | Microsoft.Sql/servers/databases/delete | Hiermee verwijdert u een bestaande database. |
> | Bewerking | Microsoft.Sql/servers/databases/export/action | Azure SQL-Database exporteren |
> | Bewerking | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | De gegevens van de uitgebreide blob controleren dat is geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | De uitgebreide blob controlebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/extensions/read | Hiermee haalt een verzameling van extensies voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/extensions/write | De extensie voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Geo back-upbeleid voor een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Een database geobackup-beleid maken of bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/importExportOperationResults/read | Haalt onderhanden import/export-bewerkingen |
> | Bewerking | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Hiermee haalt een lijst met beschikbare onderhoudsvensters voor een geselecteerde database. |
> | Bewerking | Microsoft.Sql/servers/databases/maintenanceWindows/read | Onderhoud van windows-instellingen voor een geselecteerde database opgehaald. |
> | Bewerking | Microsoft.Sql/servers/databases/maintenanceWindows/write | Hiermee stelt u onderhoud windows-instellingen voor een geselecteerde database. |
> | Bewerking | Microsoft.Sql/servers/databases/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.Sql/servers/databases/metrics/read | Metrische gegevens voor de databases terug |
> | Bewerking | Microsoft.Sql/servers/databases/move/action | Wijzig de naam van een bestaande database. |
> | Bewerking | Microsoft.Sql/servers/databases/operationResults/read | Hiermee haalt u de status van een databasebewerking. |
> | Bewerking | Microsoft.Sql/servers/databases/operations/cancel/action | Azure SQL Database, wordt geannuleerd in afwachting van asynchrone bewerking die nog niet is voltooid. |
> | Bewerking | Microsoft.Sql/servers/databases/operations/read | Retourneert de lijst met bewerkingen die worden uitgevoerd op de database |
> | Bewerking | Microsoft.Sql/servers/databases/pause/action | Onderbreken Azure SQL-Datawarehouse-Database |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor databases |
> | Bewerking | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor databases zijn |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Retourneert het verzamelen van query-teksten die overeenkomen met de opgegeven parameters. |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/read | Retourneert de huidige waarden van de Query Store-instellingen voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/queryStore/write | Query Store-instelling voor de database-updates |
> | Bewerking | Microsoft.Sql/servers/databases/read | Retourneert de lijst met databases of haalt de eigenschappen voor de opgegeven database. |
> | Bewerking | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/delete | Beëindigen van de replicatierelatie geforceerd en met mogelijk gegevensverlies |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Failover na het synchroniseren van alle wijzigingen van de primaire, deze database maken in de relationship\u0027s replicatie van primaire en het aanbrengen van de externe primaire naar een secundair |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Failover onmiddellijk met het verlies van gegevens, deze database maken in de relationship\u0027s replicatie van primaire en het aanbrengen van de externe primaire naar een secundair |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/read | Retourneert de lijst met replicatie worden gekoppeld of haalt u de eigenschappen voor de opgegeven replicatiekoppelingen. |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Beëindigen van de replicatierelatie geforceerd of na het synchroniseren met de partner |
> | Bewerking | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Replicatiemodus voor koppeling naar de modus voor synchroon of asynchroon bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/action | Een nieuw herstelpunt wordt gemaakt |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/delete | Hiermee verwijdert u een herstelpunt voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/restorePoints/read | Retourneert de herstelpunten voor de database. |
> | Bewerking | Microsoft.Sql/servers/databases/resume/action | Azure SQL Datawarehouse-Database hervatten |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/read | Het schema van een database ophalen. |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Een databasekolom ophalen. |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | De gevoeligheid, label van een bepaalde kolom verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Gevoeligheid aanbevelingen voor een bepaalde kolom uitschakelen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Gevoeligheid aanbevelingen op een bepaalde kolom inschakelen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | De gevoeligheid, label van een bepaalde kolom ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Maken of bijwerken van het gevoeligheidslabel van de van een bepaalde kolom |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/read | Een databasetabel ophalen. |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Lijst met indexaanbevelingen op een database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Indexaanbeveling toepast |
> | Bewerking | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Een lijst met database threat detection beleidsregels die zijn geconfigureerd voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/securityAlertPolicies/write | De database threat detectiebeleid voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/securityMetrics/read | Hiermee wordt een verzameling van database security metrische gegevens |
> | Bewerking | Microsoft.Sql/servers/databases/sensitivityLabels/read | Lijst met gevoeligheidslabels van een bepaalde database |
> | Bewerking | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Suggesties over het schalen van de database omhoog of omlaag retourneren op basis van statistieken van de query kan worden uitgevoerd op de prestaties verbeteren of uw kosten verlagen |
> | Bewerking | Microsoft.Sql/servers/databases/skus/read | Hiermee haalt u een verzameling van SKU's beschikbaar voor een database |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Synchronisatie met synchronisatie annuleren |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/delete | Hiermee verwijdert u een bestaande synchronisatiegroep. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Retourneert de lijst met synchronisatie hub databaseschema 's |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/logs/read | Retourneren van de lijst van de groep synchronisatielogboeken |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/read | Retourneert de lijst met synchronisatie groepen of haalt u de eigenschappen voor de groep opgegeven voor de synchronisatieshare. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Synchronisatieschema hub database vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Ophalen van resultaat van de synchronisatiebewerking hub schema vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Hiermee verwijdert u een bestaande synchronisatielid. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Retourneert de lijst met synchronisatieleden of haalt de eigenschappen voor een lid van de opgegeven voor de synchronisatieshare. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Synchronisatieschema lid vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Ophalen van resultaat van de synchronisatiebewerking lid schema vernieuwen |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Retourneert de lijst met synchronisatie lid databaseschema 's |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Hiermee maakt u een synchronisatielid met de opgegeven parameters of de eigenschappen niet bijwerken voor een lid van de opgegeven voor de synchronisatieshare. |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Synchronisatie met trigger sync |
> | Bewerking | Microsoft.Sql/servers/databases/syncGroups/write | Hiermee maakt u een groep voor synchronisatie met de opgegeven parameters of de eigenschappen voor de opgegeven synchronisatiegroep bij te werken. |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/queryText/action | Retourneert de Transact-SQL-tekst voor de geselecteerde query-ID |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/read | Retourneert de runtime-statistieken voor de geselecteerde query samengevoegd in een geselecteerde tijdsperiode |
> | Bewerking | Microsoft.Sql/servers/databases/topQueries/statistics/read | Retourneert de runtime-statistieken voor de geselecteerde query samengevoegd in een geselecteerde tijdsperiode |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Haalt onderhanden bewerkingen op transparante gegevensversleuteling |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Status en details van transparent data encryption beveiligingsfunctie voor een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Transparent data encryption status wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Azure SQL-Datawarehouse-Database bijwerken |
> | Bewerking | Microsoft.Sql/servers/databases/usages/read | Hiermee haalt u de informatie van het gebruik van Azure SQL Database |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | De evaluatie van beveiligingsproblemen voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | De beleidsregels van de evaluatie van beveiligingsproblemen op een givendatabase ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database verwijderen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | De basislijn beveiligingslek evaluatie van de regel voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Een bestaande scanresultaat om een mens leesbaar indeling te converteren. Als al er niets gebeurt |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Kwetsbaarheidsscan evaluatie van de database worden uitgevoerd. |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | De lijst met database-beveiligingsprobleem evaluatie scan records retourneren of de record van de scan voor de opgegeven scan-id ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | De evaluatie van beveiligingsproblemen voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Kwetsbaarheidsscan evaluatie van de database worden uitgevoerd. |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Haal het resultaat van de database door een beveiligingslek scan voor evaluatie van de bewerking uitvoeren |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | De gegevens van de evaluatie van beveiligingsproblemen geconfigureerd op een bepaalde database ophalen |
> | Bewerking | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | De evaluatie van beveiligingsproblemen voor een bepaalde database wijzigen |
> | Bewerking | Microsoft.Sql/servers/databases/write | Hiermee maakt u een database met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven database. |
> | Bewerking | Microsoft.Sql/servers/delete | Hiermee verwijdert u een bestaande server. |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Hiermee verwijdert u een bestaande configuratie van de herstel na noodgevallen voor een bepaalde server |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Failover een DisasterRecoveryConfiguration |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Failover een DisasterRecoveryConfiguration afgedwongen |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Hiermee wordt een verzameling disaster recovery-configuraties die deze server |
> | Bewerking | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Server disaster recovery-configuratie wijzigen |
> | Bewerking | Microsoft.Sql/servers/elasticPoolEstimates/read | Retourneert de lijst van de elastische pool schattingen al werden gemaakt voor deze server |
> | Bewerking | Microsoft.Sql/servers/elasticPoolEstimates/write | Hiermee maakt u nieuwe elastische pool schatting voor de lijst met databases die zijn opgegeven |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/read | Retourneert de lijst met adviseurs die beschikbaar zijn voor de elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Retourneert de lijst met aanbevolen acties van het opgegeven advisor voor de elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | De aanbevolen actie toepassen op de elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/advisors/write | Update automatisch-uitvoeren status van een advisor op het niveau van de elastische pool. |
> | Bewerking | Microsoft.Sql/servers/elasticPools/databases/read | Hiermee haalt u een lijst met databases voor een elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/delete | Bestaande elastische pool verwijderen |
> | Bewerking | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Activiteiten en meer informatie over een bepaalde elastische databasegroep ophalen |
> | Bewerking | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Activiteiten en meer informatie over een bepaalde database die deel uitmaakt van een pool voor elastische database ophalen |
> | Bewerking | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn |
> | Bewerking | Microsoft.Sql/servers/elasticPools/metrics/read | Retourneren van metrische gegevens voor pools voor elastische databases |
> | Bewerking | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Elastische Azure SQL-pool in afwachting van asynchrone bewerking die nog niet is voltooid, wordt geannuleerd. |
> | Bewerking | Microsoft.Sql/servers/elasticPools/operations/read | Retourneert de lijst met bewerkingen die worden uitgevoerd op de elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Hiermee wordt de diagnostische instelling voor de resource opgehaald |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Hiermee maken of bijwerken van de diagnostische instelling voor de resource |
> | Bewerking | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn |
> | Bewerking | Microsoft.Sql/servers/elasticPools/read | Gegevens van elastische pool op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.Sql/servers/elasticPools/skus/read | Hiermee haalt u een verzameling van SKU's beschikbaar voor een elastische pool |
> | Bewerking | Microsoft.Sql/servers/elasticPools/write | Een nieuwe maken of wijzigen van de eigenschappen van bestaande elastische pool |
> | Bewerking | Microsoft.Sql/servers/encryptionProtector/read | Retourneert een lijst van de server encryption Protector of haalt u de eigenschappen voor de opgegeven server versleutelingsbeveiliging. |
> | Bewerking | Microsoft.Sql/servers/encryptionProtector/write | De eigenschappen voor de opgegeven Server Encryption Protector bijgewerkt. |
> | Bewerking | Microsoft.Sql/servers/extendedAuditingSettings/read | Gegevens van de uitgebreide server blob auditing dat is geconfigureerd op een bepaalde server niet ophalen |
> | Bewerking | Microsoft.Sql/servers/extendedAuditingSettings/write | De uitgebreide controle voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/delete | Hiermee verwijdert u een bestaande failovergroep. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/failover/action | Geplande failover uitgevoerd in een bestaande failovergroep. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Geforceerde failover uitgevoerd in een bestaande failovergroep. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/read | Retourneert de lijst met failover groepen of haalt u de eigenschappen voor de opgegeven failovergroep. |
> | Bewerking | Microsoft.Sql/servers/failoverGroups/write | Hiermee maakt u een failovergroep met de opgegeven parameters of de eigenschappen of labels voor de opgegeven failovergroep bijgewerkt. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/delete | Hiermee verwijdert u een bestaande server firewall-regel. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/read | Retourneert de lijst met serverfirewall-regels of haalt u de eigenschappen voor de opgegeven server firewall-regel. |
> | Bewerking | Microsoft.Sql/servers/firewallRules/write | Hiermee maakt u een server firewall-regel met de opgegeven parameters, de eigenschappen voor de opgegeven regel bijwerken of alle bestaande regels overschrijven met de nieuwe server firewall-regels. |
> | Bewerking | Microsoft.Sql/servers/import/action | Een nieuwe database op de server maken en implementeren van schema en de gegevens uit een DacPac-pakket |
> | Bewerking | Microsoft.Sql/servers/importExportOperationResults/read | Haalt onderhanden import/export-bewerkingen |
> | Bewerking | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Hiermee verwijdert u de opgegeven interface endpoint-profiel |
> | Bewerking | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Retourneert de eigenschappen voor de opgegeven interface endpoint-profiel |
> | Bewerking | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Hiermee maakt u een profiel van de eindpunt interface met de opgegeven parameters of de eigenschappen of labels voor het eindpunt van de opgegeven interface-updates |
> | Bewerking | Microsoft.Sql/servers/jobAgents/delete | Hiermee verwijdert u de agent van een Azure SQL DB-taken |
> | Bewerking | Microsoft.Sql/servers/jobAgents/read | Een agent van Azure SQL DB-taak opgehaald |
> | Bewerking | Microsoft.Sql/servers/jobAgents/write | Hiermee maken of bijwerken van de agent van een Azure SQL DB-taken |
> | Bewerking | Microsoft.Sql/servers/keys/delete | Hiermee verwijdert u een bestaande serversleutel. |
> | Bewerking | Microsoft.Sql/servers/keys/read | Retourneert de lijst met server sleutels of haalt u de eigenschappen voor de sleutel van de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/keys/write | Hiermee maakt u een sleutel met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de sleutel van de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/operationResults/read | Haalt onderhanden serverbewerkingen |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Hiermee verwijdert u een bestaande persoonlijke eindpunt verbinding proxy |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Retourneert de lijst met persoonlijke eindpunt verbinding proxy's of haalt u de eigenschappen voor de opgegeven persoonlijke eindpunt verbinding-proxy. |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Valideert een particulier eindpunt verbinding aanroep aan NRP maken |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | De proxy van een persoonlijke eindpunt verbinding maakt met de opgegeven parameters of de eigenschappen of labels voor de opgegeven persoonlijke eindpunt verbinding proxy bijgewerkt. |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnections/delete | Hiermee verwijdert u een bestaande persoonlijke eindpunt-verbinding |
> | Bewerking | Microsoft.Sql/servers/privateEndpointConnections/read | Retourneert de lijst met verbindingen van de persoonlijke eindpunt of haalt u de eigenschappen voor de opgegeven persoonlijke eindpunt-verbinding. |
> | Bewerking | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Resultaattypen van metrische gegevens die beschikbaar voor servers zijn |
> | Bewerking | Microsoft.Sql/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Bewerking | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Metrische gegevens over aanbevolen pools voor elastische databases voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/recommendedElasticPools/read | Aanbeveling voor pools voor elastische databases naar uw kosten verlagen of verbeteren op basis van historisch Resourcegebruik ophalen |
> | Bewerking | Microsoft.Sql/servers/recoverableDatabases/read | Deze bewerking wordt gebruikt voor herstel na noodgevallen van live-database naar de database herstellen naar laatst bekende goede back-up. Deze retourneert informatie over de laatste goede back-up, maar deze doesn\u0027t daadwerkelijk de database herstellen. |
> | Bewerking | Microsoft.Sql/servers/replicationLinks/read | Retourneert de lijst met replicatie worden gekoppeld of haalt u de eigenschappen voor de opgegeven replicatiekoppelingen. |
> | Bewerking | Microsoft.Sql/servers/restorableDroppedDatabases/read | Haal een lijst van databases die zijn verwijderd van een bepaalde server die zich nog steeds binnen het bewaarbeleid. |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Resultaten van de schrijfbewerking server threat detection beleid ophalen |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/read | Een lijst met server threat detection beleidsregels die zijn geconfigureerd voor een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/securityAlertPolicies/write | Het beleid voor detectie van bedreigingen server voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/serviceObjectives/read | Lijst met serviceniveaudoelstellingen (ook wel bekend als prestatielagen) beschikbaar is op een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/syncAgents/delete | Hiermee verwijdert u een bestaande sync-agent. |
> | Bewerking | Microsoft.Sql/servers/syncAgents/generateKey/action | Synchronisatie-agent registratiesleutel genereren |
> | Bewerking | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Retourneert de lijst met databases synchroniseren-agent die is gekoppeld |
> | Bewerking | Microsoft.Sql/servers/syncAgents/read | Retourneert de lijst met synchronisatieagents of haalt de eigenschappen voor de opgegeven sync-agent. |
> | Bewerking | Microsoft.Sql/servers/syncAgents/write | Hiermee maakt u een sync-agent met de opgegeven parameters of de eigenschappen voor de opgegeven sync-agent bijwerken. |
> | Bewerking | Microsoft.Sql/servers/tdeCertificates/action | Certificaat voor TDE maken/bijwerken |
> | Bewerking | Microsoft.Sql/servers/usages/read | Server DTU-quotum en huidige DTU consuption geretourneerd door alle databases in de-server |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/delete | Hiermee verwijdert u een bestaande regel voor het virtuele netwerk |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/read | Retourneert de lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.Sql/servers/virtualNetworkRules/write | Hiermee maakt u een regel voor virtuele netwerken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel van de opgegeven virtuele netwerk. |
> | Bewerking | Microsoft.Sql/servers/vulnerabilityAssessments/delete | De evaluatie van beveiligingsproblemen voor een bepaalde server verwijderen |
> | Bewerking | Microsoft.Sql/servers/vulnerabilityAssessments/read | Het beleid van de evaluatie van beveiligingsproblemen op een bepaalde server ophalen |
> | Bewerking | Microsoft.Sql/servers/vulnerabilityAssessments/write | De evaluatie van beveiligingsproblemen voor een bepaalde server wijzigen |
> | Bewerking | Microsoft.Sql/servers/write | Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server. |
> | Bewerking | Microsoft.Sql/unregister/action | De registratie van het abonnement voor de resourceprovider van Microsoft SQL-Database en wordt het maken van Microsoft SQL-Databases. |
> | Bewerking | Microsoft.Sql/virtualClusters/delete | Hiermee verwijdert u een bestaand virtueel cluster. |
> | Bewerking | Microsoft.Sql/virtualClusters/read | Retourneert de lijst met virtuele clusters of haalt de eigenschappen voor de opgegeven virtuele-cluster. |
> | Bewerking | Microsoft.Sql/virtualClusters/write | Virtueel cluster tags-updates. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Storage/checknameavailability/read | Controleert of de accountnaam van dat geldig is en niet wordt gebruikt. |
> | Bewerking | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Hiermee wordt aan Microsoft.Storage dat virtuele netwerk of subnet is wordt verwijderd |
> | Bewerking | Microsoft.Storage/locations/usages/read | Retourneert de limiet en het huidige aantal gebruik voor resources in het opgegeven abonnement |
> | Bewerking | Microsoft.Storage/operations/read | Controleert of de status van een asynchrone bewerking. |
> | Bewerking | Microsoft.Storage/register/action | Hiermee wordt het abonnement voor de storage resourceprovider geregistreerd en wordt het maken van opslagaccounts mogelijk. |
> | Bewerking | Microsoft.Storage/skus/read | Geeft een lijst van de SKU's ondersteund door Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Retourneert het resultaat van de blobinhoud toe te voegen |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Retourneert het resultaat van het verwijderen van een blob |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteAutomaticSnapshot/action | Retourneert het resultaat van het verwijderen van een momentopname van een automatische |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Retourneert de lijst met blobs onder een account met overeenkomende tags filteren |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Retourneert een blob of een lijst met blobs |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Retourneert het resultaat van het lezen van de blob-tags |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Retourneert het resultaat van het schrijven van blob-tags |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Retourneert het resultaat van het schrijven van een blob |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Blobcontainer wissen juridische bevatten |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Retourneert het resultaat van het verwijderen van een container |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Beleid voor Onveranderbaarheid van blobcontainer verwijderen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Beleid voor Onveranderbaarheid van blobcontainer uitbreiden |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Beleid voor Onveranderbaarheid van blobcontainer vergrendelen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Beleid voor Onveranderbaarheid van blobcontainer ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Beleid voor Onveranderbaarheid van blobcontainer plaatsen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Retourneert het resultaat van de lease-blob-container |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/read | Retourneert een container |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/read | Retourneert lijst met containers |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Juridische bewaring voor blobcontainer instellen |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/write | Retourneert het resultaat van de patch-blob-container |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/containers/write | Retourneert het resultaat van put blob-container |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Retourneert een sleutel voor het delegeren van gebruiker voor de blob-service |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/read | Retourneert blob-service-eigenschappen of statistieken |
> | Bewerking | Microsoft.Storage/storageAccounts/blobServices/write | Retourneert het resultaat van put blob service-eigenschappen |
> | Bewerking | Microsoft.Storage/storageAccounts/delete | Hiermee verwijdert u een bestaand opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/failover/action | Klanten is kunnen voor het beheren van de failover in het geval van problemen met de beschikbaarheid |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read |  |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write |  |
> | Bewerking | Microsoft.Storage/storageAccounts/fileServices/read | Service bestandseigenschappen ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/listAccountSas/action | Retourneert de Account-SAS-token voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/listkeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/listServiceSas/action | Retourneert het Service-SAS-token voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/managementPolicies/delete | Opslagaccountbeheer verwijderen |
> | Bewerking | Microsoft.Storage/storageAccounts/managementPolicies/read | Accountbeleid ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/managementPolicies/write | Opslagaccountbeheer plaatsen |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Retourneert het resultaat van een wachtrij verwijderen |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Retourneert het resultaat van het toevoegen van een bericht |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Retourneert het resultaat van een bericht verwijderen |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Retourneert het resultaat van het verwerken van een bericht |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Retourneert een bericht |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Retourneert het resultaat van het schrijven van een bericht |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/read | Retourneert een wachtrij of een lijst met wachtrijen. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/queues/write | Retourneert het resultaat van het schrijven van een wachtrij |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/read | Eigenschappen van de Queue-service ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/read | Retourneert een wachtrij service-eigenschappen of statistieken. |
> | Bewerking | Microsoft.Storage/storageAccounts/queueServices/write | Retourneert het resultaat van het instellen van eigenschappen van de queue-service |
> | Bewerking | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/regeneratekey/action | Genereert opnieuw de toegangssleutels voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Trekt alle gebruiker delegering sleutels voor het opgegeven opslagaccount. |
> | Bewerking | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Diagnostische instellingen van opslagaccount maken/bijwerken. |
> | Bewerking | Microsoft.Storage/storageAccounts/tableServices/read | Tabel-service-eigenschappen ophalen |
> | Bewerking | Microsoft.Storage/storageAccounts/write | Hiermee maakt u een opslagaccount met de opgegeven parameters of update de eigenschappen of labels of het aangepaste domein voor het opgegeven opslagaccount toegevoegd. |
> | Bewerking | Microsoft.Storage/usages/read | Retourneert de limiet en het huidige aantal gebruik voor resources in het opgegeven abonnement |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | microsoft.storagesync/locations/checkNameAvailability/action | Controleert dat Opslagnaam van de synchronisatieservice is geldig en niet wordt gebruikt. |
> | Bewerking | Microsoft.storagesync/Locations/workflows/Operations/Read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | microsoft.storagesync/storageSyncServices/delete | Verwijderen van eventuele Opslagsynchronisatieservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor Opslagsynchronisatieservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/read | Lees elk Opslagsynchronisatieservices |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/delete | Verwijderen van een geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor de geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/read | Lezen van een geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/registeredServers/write | Maken of bijwerken van een geregistreerde Server |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Alle Cloudeindpunten verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Hiermee wordt de status van een asynchrone back-up-/ herstelbewerking opgehaald |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Aanroepen van deze actie na de back-up |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Deze actie na herstel aanroepen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Aanroepen van deze actie voor back-up |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Aanroepen van deze actie vóór herstellen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Elke Cloudeindpunten lezen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Heartbeat herstellen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Maken of bijwerken van een Cloud-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/delete | Alle synchronisatiegroepen verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor synchronisatiegroepen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/read | Geen synchronisatiegroepen lezen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Alle servereindpunten verwijderen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor Server-eindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Alle servereindpunten lezen |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Aanroepen van deze actie om in te trekken van bestanden naar een server |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Maken of bijwerken van alle servereindpunten |
> | Bewerking | microsoft.storagesync/storageSyncServices/syncGroups/write | Een synchronisatiegroepen maken of bijwerken |
> | Bewerking | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | microsoft.storagesync/storageSyncServices/workflows/operations/read | Hiermee wordt de status van een asynchrone bewerking opgehaald |
> | Bewerking | microsoft.storagesync/storageSyncServices/workflows/read | Werkstromen lezen |
> | Bewerking | microsoft.storagesync/storageSyncServices/write | Maken of bijwerken van eventuele Opslagsynchronisatieservices |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/delete | Hiermee verwijdert u de Access Control Records |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/read | Geeft een lijst of de Access Control Records opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/accessControlRecords/write | Maken of bijwerken van de Access Control Records |
> | Bewerking | Microsoft.StorSimple/managers/alerts/read | Geeft een lijst of opgehaald van de waarschuwingen |
> | Bewerking | Microsoft.StorSimple/managers/backups/read | Geeft een lijst of back-upset opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/delete | Hiermee verwijdert u een bestaande bandbreedte-instellingen (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/read | Lijst van de bandbreedte-instellingen (8000-serie alleen) |
> | Bewerking | Microsoft.StorSimple/managers/bandwidthSettings/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt bandbreedte-instellingen (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/certificates/write | De certificaten maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/Managers/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Bewerking | Microsoft.StorSimple/managers/clearAlerts/action | Schakel alle waarschuwingen die zijn gekoppeld aan de device manager. |
> | Bewerking | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Lijst met het Cloudapparaat ondersteunde configuraties |
> | Bewerking | Microsoft.StorSimple/managers/configureDevice/action | Hiermee configureert u een apparaat |
> | Bewerking | Microsoft.StorSimple/managers/delete | Hiermee verwijdert u de Apparaatmanagers |
> | Bewerking | Microsoft.StorSimple/Managers/delete | De opgegeven Azure-resource van het type 'kluis' Hiermee verwijdert u de bewerking kluis verwijderen |
> | Bewerking | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/alertSettings/read | Geeft een lijst of opgehaald van de instellingen voor meldingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/alertSettings/write | Maken of bijwerken van de instellingen voor meldingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Voor Service Rollover van de versleutelingssleutel van apparaten toestaan |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Neem een handmatige back-up maken van een on-demand back-up van alle volumes die zijn beveiligd door het beleid. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Hiermee verwijdert u een beleid in een bestaande back-up (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/read | Lijst met de back-up-beleidsregels (8000-serie alleen) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Hiermee verwijdert u een bestaande schema 's |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Lijst van de schema 's |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt schema 's |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupPolicies/write | Maakt u een nieuwe gemaakt of bijgewerkt met het beleid van de back-up (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/delete | Hiermee verwijdert u de back-upset |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Klonen van een share of een volume met behulp van een back-element. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/read | Geeft een lijst of back-upset opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backups/restore/action | Alle volumes herstellen vanuit een back-upset. |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Hiermee verwijdert u de back-upschema-groepen |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Geeft een lijst of de back-up plannen groepen opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | De back-up plannen groepen maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/delete | De Chap-instellingen worden verwijderd |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/read | Geeft een lijst of de Chap-instellingen worden opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/chapSettings/write | Maken of bijwerken van de Chap-instellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/deactivate/action | Een apparaat wordt gedeactiveerd. |
> | Bewerking | Microsoft.StorSimple/managers/devices/delete | Hiermee verwijdert u de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/devices/disks/read | Geeft een lijst of de schijven opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/download/action | Download updates voor een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/failover/action | Failover van het apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/failover/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/failoverTargets/read | Geeft een lijst of de Failover-doelen van de apparaten opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Back-up maken van een bestandsserver. |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/delete | Hiermee verwijdert u de bestandsservers |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/read | Geeft een lijst of de bestandsservers opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Hiermee verwijdert u de Shares |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Geeft een lijst of de Shares opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Maken of bijwerken van de Shares |
> | Bewerking | Microsoft.StorSimple/managers/devices/fileservers/write | Maken of bijwerken van de bestandsservers |
> | Bewerking | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Controller-/ uitstatus van hardware-onderdeelgroepen wijzigen |
> | Bewerking | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Lijst van de Hardware-onderdeelgroepen |
> | Bewerking | Microsoft.StorSimple/managers/devices/install/action | Updates installeren op een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/installUpdates/action | Hiermee worden updates geïnstalleerd op de apparaten (alleen 8000-serie). |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Back-up maken van een iSCSI-server. |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Hiermee verwijdert u de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Hiermee verwijdert u de schijven |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Geeft een lijst of de schijven opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Maken of bijwerken van de schijven |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/read | Geeft een lijst of opgehaald van de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/iscsiservers/write | Maken of bijwerken van de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Een actieve taak annuleren |
> | Bewerking | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/jobs/read | Geeft een lijst of de taken opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Een lijst van de sets failover voor een bestaand apparaat (alleen 8000-serie). |
> | Bewerking | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Doelen voor de failover van de apparaten (alleen 8000-serie). |
> | Bewerking | Microsoft.StorSimple/managers/devices/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Een geslaagde migratie bevestigt en voer deze door. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | Lijst met de migratiestatus controleren |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Haal de status van het bevestigen van de migratie. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Haal de status van de schatting van de migratietaak. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Haal de status van de migratie. |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Importeren van de bron-configuraties voor migratie |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | Lijst van de schatting van de migratie |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | De migratiestatus weergeven |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Migratie met behulp van de bron-configuraties starten |
> | Bewerking | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Een taak om een schatting van de duur van het migratieproces te starten. |
> | Bewerking | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/networkSettings/read | Geeft een lijst of de instellingen van het netwerk worden opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/networkSettings/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt netwerkinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Lijst met openbare versleutelingssleutel van Apparaatbeheer |
> | Bewerking | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Het ondersteuningspakket voor een bestaand apparaat publiceren. Een StorSimple-ondersteuning-pakket is een mechanisme voor eenvoudig te gebruiken die worden verzameld van alle relevante Logboeken om te helpen van Microsoft Support bij het oplossen van problemen met StorSimple-apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/read | Geeft een lijst of de apparaten opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Scannen op updates voor een apparaat. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/read | Lijst van de instellingen voor beveiliging |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Het certificaat voor extern beheer voor een apparaat synchroniseren. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Werk de instellingen voor beveiliging. |
> | Bewerking | Microsoft.StorSimple/managers/devices/securitySettings/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt beveiligingsinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | E-mailwaarschuwing test verzenden naar geconfigureerde e-mailontvangers. |
> | Bewerking | Microsoft.StorSimple/managers/devices/shares/read | Geeft een lijst of de Shares opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/timeSettings/read | Geeft een lijst of de instellingen van de tijd worden opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/timeSettings/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt tijdinstellingen |
> | Bewerking | Microsoft.StorSimple/managers/devices/updates/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/devices/updateSummary/read | Geeft een lijst of opgehaald van de samenvatting bijwerken |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Hiermee verwijdert u een bestaande Volumecontainers (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Lijst van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Lijst van de metrische definities |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/read | Lijst van de Volumecontainers (8000-serie alleen) |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Hiermee verwijdert u een bestaande Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Lijst van de metrische gegevens |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Lijst van de metrische definities |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | Lijst van de resultaten van de bewerking |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Lijst van de Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumeContainers/write | Hiermee maakt u een nieuw gemaakt of bijgewerkt Volumecontainers (alleen 8000-serie) |
> | Bewerking | Microsoft.StorSimple/managers/devices/volumes/read | Lijst van de Volumes |
> | Bewerking | Microsoft.StorSimple/managers/devices/write | Maken of bijwerken van de apparaten |
> | Bewerking | Microsoft.StorSimple/managers/encryptionSettings/read | Geeft een lijst of de instellingen voor codering worden opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/extendedInformation/delete | Hiermee verwijdert u de uitgebreide Vault-gegevens |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/delete | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/managers/extendedInformation/read | Geeft een lijst of de kluis uitgebreide informatie ophalen |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/managers/extendedInformation/write | De kluis uitgebreide informatie maken of bijwerken |
> | Bewerking | Microsoft.StorSimple/Managers/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Bewerking | Microsoft.StorSimple/managers/features/read | Overzicht van de functies |
> | Bewerking | Microsoft.StorSimple/managers/fileservers/read | Geeft een lijst of de bestandsservers opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/getEncryptionKey/action | Coderingssleutel ophalen voor Apparaatbeheer. |
> | Bewerking | Microsoft.StorSimple/managers/iscsiservers/read | Geeft een lijst of opgehaald van de iSCSI-Servers |
> | Bewerking | Microsoft.StorSimple/managers/jobs/read | Geeft een lijst of de taken opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/listActivationKey/action | Hiermee haalt u de activeringssleutel van de StorSimple Device Manager. |
> | Bewerking | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Lijst met codering met openbare sleutels van een StorSimple Device Manager. |
> | Bewerking | Microsoft.StorSimple/managers/metrics/read | Geeft een lijst of de metrische gegevens opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/metricsDefinitions/read | Geeft een lijst of de metrische definities opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Migreren van klassiek naar Resource Manager van Mangers |
> | Bewerking | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | Lijst van de bron-configuraties voor migratie (8000-serie alleen) |
> | Bewerking | Microsoft.StorSimple/managers/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Maak een nieuw cloudapparaat. |
> | Bewerking | Microsoft.StorSimple/managers/read | Geeft een lijst of de Apparaatmanagers opgehaald |
> | Bewerking | Microsoft.StorSimple/Managers/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Bewerking | Microsoft.StorSimple/managers/regenerateActivationKey/action | De activeringssleutel opnieuw genereren voor een bestaande StorSimple Device Manager. |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Hiermee verwijdert u de referenties van het Storage-Account |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/read | Geeft een lijst of de referenties van het Storage-Account opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/storageAccountCredentials/write | Maken of bijwerken van de referenties van het Storage-Account |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/delete | Hiermee verwijdert u de Opslagdomeinen |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/operationResults/read | Geeft een lijst of de resultaten van de bewerking opgehaald |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/read | Geeft een lijst of opgehaald van de opslag-domeinen |
> | Bewerking | Microsoft.StorSimple/managers/storageDomains/write | Maken of bijwerken van de opslag-domeinen |
> | Bewerking | Microsoft.StorSimple/managers/write | Maken of bijwerken van de Device Manager-services |
> | Bewerking | Microsoft.StorSimple/Managers/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
> | Bewerking | Microsoft.StorSimple/operations/read | Geeft een lijst of de bewerkingen opgehaald |
> | Bewerking | Microsoft.StorSimple/register/action | Provider Microsoft.StorSimple registreren |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.StreamAnalytics/locations/quotas/Read | Lezen Stream Analytics-Abonnementquotum |
> | Bewerking | Microsoft.StreamAnalytics/operations/Read | Stream Analytics-bewerkingen lezen |
> | Bewerking | Microsoft.StreamAnalytics/Register/action | Abonnement registreren met Stream Analytics-Resourceprovider |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Delete | Stream Analytics-taak verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Stream Analytics-taakfunctie verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Bewerkingsresultaten lezen voor Stream Analytics-taakfunctie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Lezen Stream Analytics-taakfunctie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Standaarddefinitie ophalen van een Stream Analytics-taakfunctie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Test Stream Analytics-taakfunctie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Stream Analytics-taakfunctie schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Stream Analytics-Taakinvoer verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Bewerkingsresultaten lezen voor Stream Analytics-Taakinvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Lezen Stream Analytics-Taakinvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Voorbeeld van Stream Analytics-Taakinvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Test Stream Analytics-Taakinvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Stream Analytics-Taakinvoer schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Metrische definities lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Bewerkingsresultaten lezen voor Stream Analytics-taak |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Stream Analytics-Taakuitvoer verwijderen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Bewerkingsresultaten lezen voor Stream Analytics-Taakuitvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Lezen Stream Analytics-Taakuitvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Test Stream Analytics-Taakuitvoer |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Stream Analytics-Taakuitvoer schrijven |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Diagnostische instelling lezen. |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Diagnostische instelling schrijven. |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | De beschikbare logboeken opgehaald voor streamingjobs |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Hiermee haalt u de beschikbare metrische gegevens voor streamingjobs |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Read | Stream Analytics-taak lezen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Start/action | Stream Analytics-taak starten |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stream Analytics-taak stoppen |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Verwijderen van Stream Analytics-Taaktransformatie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Lezen Stream Analytics-Taaktransformatie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Schrijven van Stream Analytics-Taaktransformatie |
> | Bewerking | Microsoft.StreamAnalytics/streamingjobs/Write | Schrijven van Stream Analytics-taak |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Subscription/CreateSubscription/action | Maak een Azure-abonnement |
> | Bewerking | Microsoft.Subscription/register/action | Hiermee wordt een abonnement bij de resourceprovider Microsoft.Subscription |
> | Bewerking | Microsoft.Subscription/SubscriptionDefinitions/read | De definitie van een Azure-abonnement in een beheergroep ophalen. |
> | Bewerking | Microsoft.Subscription/SubscriptionDefinitions/write | De definitie van een Azure-abonnement maken |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.Support/register/action | Hiermee wordt het item geregistreerd bij de resourceprovider voor ondersteuning |
> | Bewerking | Microsoft.Support/supportTickets/read | Hiermee worden gegevens over het ondersteuningsticket opgehaald (inclusief status, ernst, contactgegevens en communicatie) of wordt de lijst met ondersteuningstickets voor de verschillende abonnementen opgehaald. |
> | Bewerking | Microsoft.Support/supportTickets/write | Hiermee wordt een ondersteuningsticket gemaakt of bijgewerkt. U kunt een ondersteuningsticket maken voor problemen van technische aard of op het gebied van facturering, quota's of beheer van abonnementen. U kunt voor bestaande ondersteuningstickets de ernst, de contactgegevens en de communicatiemethoden aanpassen. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Hiermee verwijdert u het toegangsbeleid. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Hiermee worden de eigenschappen van een toegangsbeleid. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Hiermee maakt u een nieuw toegangsbeleid voor een omgeving of een bestaand toegangsbeleid bijwerken. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/delete | Hiermee verwijdert u de omgeving. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Hiermee verwijdert u de gebeurtenisbron. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/read | Hiermee worden de eigenschappen van een gebeurtenisbron. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/eventsources/write | Hiermee maakt u een nieuwe gebeurtenisbron voor een omgeving, of een bestaande gebeurtenisbron bijgewerkt. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/read | Hiermee worden de eigenschappen van een omgeving. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Hiermee verwijdert u de referentiegegevensset. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Hiermee worden de eigenschappen van een referentiegegevensset. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Hiermee maakt u een nieuwe referentiegegevensset voor een omgeving, of een bestaande referentiegegevensset bijgewerkt. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/status/read | Haal de status van de omgeving, de status van de gekoppelde bewerkingen zoals het inkomend verkeer. |
> | Bewerking | Microsoft.TimeSeriesInsights/environments/write | Hiermee maakt u een nieuwe omgeving of updates van een bestaande omgeving. |
> | Bewerking | Microsoft.TimeSeriesInsights/register/action | Hiermee wordt het abonnement voor de Time Series Insights-resourceprovider geregistreerd en wordt het maken van Time Series Insights-omgevingen. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.VisualStudio/Account/Delete | Account verwijderen |
> | Bewerking | Microsoft.VisualStudio/Account/Extension/Read | Lees-/ Accountextensie |
> | Bewerking | Microsoft.VisualStudio/Account/Project/Read | Lezen-Account of het aangevraagde Project |
> | Bewerking | Microsoft.VisualStudio/Account/Project/Write | Set-Account of het aangevraagde Project |
> | Bewerking | Microsoft.VisualStudio/Account/Read | Account lezen |
> | Bewerking | Microsoft.VisualStudio/Account/Write | Account instellen |
> | Bewerking | Microsoft.VisualStudio/Extension/Delete | Extensie verwijderen |
> | Bewerking | Microsoft.VisualStudio/Extension/Read | Extensie lezen |
> | Bewerking | Microsoft.VisualStudio/Extension/Write | Extensie instellen |
> | Bewerking | Microsoft.VisualStudio/Project/Delete | Project verwijderen |
> | Bewerking | Microsoft.VisualStudio/Project/Read | Lezen-Project |
> | Bewerking | Microsoft.VisualStudio/Project/Write | Project instellen |
> | Bewerking | Microsoft.VisualStudio/Register/Action | Azure-abonnement registreren bij Microsoft.VisualStudio-provider |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | microsoft.web/apimanagementaccounts/apiacls/read | Api Management-Accounts Apiacls ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Api Management Accounts API's Apiacls verwijderen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/apiacls/read | Api Management Accounts API's Apiacls ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/apiacls/write | Api Management Accounts API's Apiacls bijwerken. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Api Management Accounts API's Connectionacls ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Controleer of u toestemming Code Api Management-Accounts API-verbindingen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Api Management-Accounts API's verbindingen Connectionacls verwijderen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Api Management-Accounts API's verbindingen Connectionacls ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Api Management-Accounts API's verbindingen Connectionacls bijwerken. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/delete | Api Management-Accounts API's verbindingen verwijderen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Instemmingskoppelingen voor Api Management-Accounts API-verbindingen ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Lijst met verbinding sleutels Api Management-Accounts API's-verbindingen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Lijst met geheimen Api Management-Accounts API-verbindingen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/read | Api Management-Accounts API-verbindingen ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/connections/write | Api Management-Accounts API's verbindingen bijwerken. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/delete | Api Management-API's-Accounts verwijderen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Verwijderen van Api Management API's voor Accounts gelokaliseerd definities. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Ophalen van Api Management API's voor Accounts gelokaliseerd definities. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | API's van update Api Management-Accounts gelokaliseerd definities. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/read | Api Management-API's-Accounts ophalen. |
> | Bewerking | microsoft.web/apimanagementaccounts/apis/write | Api Management-API's-Accounts worden bijgewerkt. |
> | Bewerking | microsoft.web/apimanagementaccounts/connectionacls/read | Api Management-Accounts Connectionacls ophalen. |
> | Bewerking | microsoft.web/availablestacks/read | Get-Stacks beschikbaar. |
> | Bewerking | Microsoft.Web/certificates/Delete | Een bestaand certificaat verwijderen. |
> | Bewerking | Microsoft.Web/certificates/Read | De lijst met certificaten ophalen. |
> | Bewerking | Microsoft.Web/certificates/Write | Voeg een nieuw certificaat of een bestaande bijgewerkt. |
> | Bewerking | microsoft.web/checknameavailability/read | Controleer of de resourcenaam van de beschikbaar is. |
> | Bewerking | microsoft.web/classicmobileservices/read | Klassieke mobiele Services ophalen. |
> | Bewerking | Microsoft.Web/connectionGateways/Delete | Hiermee verwijdert u de Verbindingsgateway van een. |
> | Bewerking | Microsoft.Web/connectionGateways/Join/Action | Lid wordt van de Verbindingsgateway van een. |
> | Bewerking | Microsoft.Web/connectionGateways/ListStatus/Action | Hiermee wordt de status van een Gateway-verbinding. |
> | Bewerking | Microsoft.Web/connectionGateways/Move/Action | Hiermee verplaatst u de Verbindingsgateway van een. |
> | Bewerking | Microsoft.Web/connectionGateways/Read | De lijst met Gateways verbinding verkrijgen. |
> | Bewerking | Microsoft.Web/connectionGateways/Write | Hiermee of bijwerken van een Gateway-verbinding. |
> | Bewerking | microsoft.web/connections/confirmconsentcode/action | Controleer of verbindingen toestemming Code. |
> | Bewerking | Microsoft.Web/connections/Delete | Hiermee verwijdert u een verbinding. |
> | Bewerking | Microsoft.Web/connections/Join/Action | Lid wordt van een verbinding. |
> | Bewerking | microsoft.web/connections/listconsentlinks/action | Lijst met Instemmingskoppelingen voor verbindingen. |
> | Bewerking | Microsoft.Web/connections/Move/Action | Hiermee plaatst u een verbinding. |
> | Bewerking | Microsoft.Web/connections/Read | De lijst met verbindingen ophalen. |
> | Bewerking | Microsoft.Web/connections/Write | Hiermee of een verbinding bijwerken. |
> | Bewerking | Microsoft.Web/customApis/Delete | Hiermee verwijdert u een aangepaste API. |
> | Bewerking | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | API-definitie van een WSDL worden uitgepakt. |
> | Bewerking | Microsoft.Web/customApis/Join/Action | Een aangepaste API koppelt. |
> | Bewerking | Microsoft.Web/customApis/listWsdlInterfaces/Action | WSDL-interfaces worden voor een aangepaste API. |
> | Bewerking | Microsoft.Web/customApis/Move/Action | Hiermee verplaatst u een aangepaste API. |
> | Bewerking | Microsoft.Web/customApis/Read | De lijst met aangepaste API voor ophalen. |
> | Bewerking | Microsoft.Web/customApis/Write | Hiermee of een aangepaste API bijgewerkt. |
> | Bewerking | Microsoft.Web/deletedSites/Read | Hiermee worden de eigenschappen van een Web-App met verwijderd |
> | Bewerking | microsoft.web/deploymentlocations/read | Implementatie locaties ophalen. |
> | Bewerking | Microsoft.Web/geoRegions/Read | Haal de lijst geografische regio's. |
> | Bewerking | microsoft.web/hostingenvironments/capacities/read | Ophalen die als host fungeert omgevingen capaciteit. |
> | Bewerking | Microsoft.Web/hostingEnvironments/Delete | Een App Service-omgeving verwijderen |
> | Bewerking | microsoft.web/hostingenvironments/detectors/read | Ophalen die als host fungeert detectoren omgevingen. |
> | Bewerking | microsoft.web/hostingenvironments/diagnostics/read | Ophalen die als host fungeert omgevingen diagnostische gegevens. |
> | Bewerking | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | De netwerkeindpunten van alle inkomende afhankelijkheden ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/Join/Action | Lid wordt van een App Service Environment |
> | Bewerking | microsoft.web/hostingenvironments/metricdefinitions/read | Ophalen die als host fungeert de metrische definities van omgevingen. |
> | Bewerking | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Metrische definities van omgevingen met meerdere rollen hebben groepen ophalen worden gehost. |
> | Bewerking | microsoft.web/hostingenvironments/multirolepools/metrics/read | Omgevingen met meerdere rollen hebben Pools metrische gegevens ophalen worden gehost. |
> | Bewerking | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Hiermee worden de eigenschappen van een front-Endgroep in een App Service Environment |
> | Bewerking | microsoft.web/hostingenvironments/multirolepools/skus/read | Ophalen die als host fungeert omgevingen met meerdere rollen hebben Pools SKU's. |
> | Bewerking | microsoft.web/hostingenvironments/multirolepools/usages/read | Ophalen hosten van meerdere rollen hebben Pools het gebruik van omgevingen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Een nieuwe front-Endgroep in een App Service Environment maken of een bestaande bijgewerkt |
> | Bewerking | microsoft.web/hostingenvironments/operations/read | Ophalen die als host fungeert omgevingen bewerkingen. |
> | Bewerking | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | De netwerkeindpunten van alle uitgaande afhankelijkheden ophalen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Eindpunt van de particuliere verbindingen goedkeuren |
> | Bewerking | Microsoft.Web/hostingEnvironments/Read | Hiermee worden de eigenschappen van een App Service Environment |
> | Bewerking | Microsoft.Web/hostingEnvironments/reboot/Action | Alle machines in een App Service Environment opnieuw opstarten |
> | Bewerking | microsoft.web/hostingenvironments/resume/action | Hostingomgevingen worden hervat. |
> | Bewerking | microsoft.web/hostingenvironments/serverfarms/read | Get-omgevingen met App Service-plannen die als host fungeert. |
> | Bewerking | microsoft.web/hostingenvironments/sites/read | Get-omgevingen met WebApps host. |
> | Bewerking | microsoft.web/hostingenvironments/suspend/action | Onderbreken hostingomgevingen. |
> | Bewerking | microsoft.web/hostingenvironments/usages/read | Ophalen hosten van het gebruik van omgevingen. |
> | Bewerking | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Die als host fungeert omgevingen Workerpools metrische definities ophalen. |
> | Bewerking | microsoft.web/hostingenvironments/workerpools/metrics/read | Omgevingen Workerpools metrische gegevens ophalen worden gehost. |
> | Bewerking | Microsoft.Web/hostingEnvironments/workerPools/Read | Hiermee worden de eigenschappen van een groep met werkrollen in een App Service Environment |
> | Bewerking | microsoft.web/hostingenvironments/workerpools/skus/read | Ophalen die als host fungeert omgevingen Workerpools SKU's. |
> | Bewerking | microsoft.web/hostingenvironments/workerpools/usages/read | Ophalen die als host fungeert Workerpools het gebruik van omgevingen. |
> | Bewerking | Microsoft.Web/hostingEnvironments/workerPools/Write | Een nieuwe groep met werkrollen in een App Service Environment maken of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Web/hostingEnvironments/Write | Een nieuwe App Service-omgeving maken of bestaande bijgewerkt |
> | Bewerking | microsoft.web/ishostingenvironmentnameavailable/read | U ontvangt als de naam van de host-omgeving beschikbaar is. |
> | Bewerking | microsoft.web/ishostnameavailable/read | Controleer of de hostnaam beschikbaar is. |
> | Bewerking | microsoft.web/isusernameavailable/read | Controleer of de gebruikersnaam beschikbaar is. |
> | Bewerking | Microsoft.Web/listSitesAssignedToHostName/Read | Namen van sites die zijn toegewezen aan de hostnaam ophalen. |
> | Bewerking | microsoft.web/locations/apioperations/read | Locaties API-bewerkingen ophalen. |
> | Bewerking | microsoft.web/locations/connectiongatewayinstallations/read | Get-locaties verbinding Gateway installaties. |
> | Bewerking | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Vnet of subnet verwijderen melding voor locaties. |
> | Bewerking | microsoft.web/locations/extractapidefinitionfromwsdl/action | Api-definitie uit WSDL uitpakken voor locaties. |
> | Bewerking | microsoft.web/locations/listwsdlinterfaces/action | Lijst met WSDL-Interfaces voor locaties. |
> | Bewerking | microsoft.web/locations/managedapis/apioperations/read | Locaties beheerde API-bewerkingen ophalen. |
> | Bewerking | Microsoft.Web/locations/managedapis/Join/Action | Lid wordt van een beheerde API. |
> | Bewerking | microsoft.web/locations/managedapis/read | Get-locaties beheerde API's. |
> | Bewerking | microsoft.web/operations/read | Bewerkingen ophalen. |
> | Bewerking | microsoft.web/publishingusers/read | Krijg gebruikers. |
> | Bewerking | microsoft.web/publishingusers/write | Update gebruikers publiceren. |
> | Bewerking | Microsoft.Web/recommendations/Read | De lijst met aanbevelingen voor abonnementen ophalen. |
> | Bewerking | microsoft.web/register/action | Registreer de resourceprovider Microsoft.Web voor het abonnement. |
> | Bewerking | microsoft.web/resourcehealthmetadata/read | Resource Health metagegevens ophalen. |
> | Bewerking | microsoft.web/serverfarms/capabilities/read | Get-mogelijkheden van App Service-abonnementen. |
> | Bewerking | Microsoft.Web/serverfarms/Delete | Een bestaande App Service-Plan verwijderen |
> | Bewerking | microsoft.web/serverfarms/firstpartyapps/settings/delete | App Service-abonnementen eerste partij Apps instellingen verwijderen. |
> | Bewerking | microsoft.web/serverfarms/firstpartyapps/settings/read | Eerste partij Apps instellingen voor App Service-abonnementen ophalen. |
> | Bewerking | microsoft.web/serverfarms/firstpartyapps/settings/write | App Service-abonnementen eerste partij Apps instellingen bijwerken. |
> | Bewerking | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | App Service-abonnementen hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | App Service-abonnementen hybride verbinding naamruimten Relays ophalen. |
> | Bewerking | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | App Service-abonnementen hybride verbinding naamruimten Relays WebApps ophalen. |
> | Bewerking | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Get-Plan limieten van App Service-abonnementen hybride verbinding. |
> | Bewerking | microsoft.web/serverfarms/hybridconnectionrelays/read | App Service-abonnementen hybride verbinding Relays ophalen. |
> | Bewerking | microsoft.web/serverfarms/metricdefinitions/read | Metrische definities van App Service-abonnementen ophalen. |
> | Bewerking | microsoft.web/serverfarms/metrics/read | App Service-abonnementen metrische gegevens ophalen. |
> | Bewerking | microsoft.web/serverfarms/operationresults/read | App Service-abonnementen Bewerkingsresultaten krijgt. |
> | Bewerking | Microsoft.Web/serverfarms/Read | Hiermee worden de eigenschappen van een App Service-Plan |
> | Bewerking | Microsoft.Web/serverfarms/restartSites/Action | Start opnieuw op alle Web-Apps in een App Service-Plan |
> | Bewerking | microsoft.web/serverfarms/sites/read | Get-Web-Apps van App Service-plannen. |
> | Bewerking | microsoft.web/serverfarms/skus/read | App Service-abonnementen SKU's ophalen. |
> | Bewerking | microsoft.web/serverfarms/usages/read | Het gebruik van App Service-abonnementen ophalen. |
> | Bewerking | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | App Service-abonnementen Virtual Network-verbindingen Gateways bijwerken. |
> | Bewerking | microsoft.web/serverfarms/virtualnetworkconnections/read | App Service-abonnementen Virtual Network-verbindingen ophalen. |
> | Bewerking | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | App Service-abonnementen Virtual Network-verbindingen Routes verwijderen. |
> | Bewerking | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | App Service-abonnementen Virtual Network-verbindingen Routes ophalen. |
> | Bewerking | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | App Service-abonnementen Virtual Network-verbindingen Routes bijwerken. |
> | Bewerking | microsoft.web/serverfarms/workers/reboot/action | Opnieuw opstarten werkrollen voor App Service-plannen. |
> | Bewerking | Microsoft.Web/serverfarms/Write | Maak een nieuwe App Service-Plan of een bestaande bijgewerkt |
> | Bewerking | microsoft.web/sites/analyzecustomhostname/read | Analyseer aangepaste hostnaam. |
> | Bewerking | Microsoft.Web/sites/applySlotConfig/Action | Web app siteconfiguratie van doelsleuf naar de huidige webtoepassing toepassen |
> | Bewerking | Microsoft.Web/sites/backup/Action | Een nieuwe web-app back-up maken |
> | Bewerking | microsoft.web/sites/backup/read | Get-back-up van Web-Apps. |
> | Bewerking | microsoft.web/sites/backup/write | Back-up van Web-Apps bijwerken. |
> | Bewerking | microsoft.web/sites/backups/action | Detecteert een bestaand app back-up die kan worden hersteld vanuit een blob in Azure storage. |
> | Bewerking | microsoft.web/sites/backups/delete | Back-ups van Web-Apps verwijderen. |
> | Bewerking | microsoft.web/sites/backups/list/action | Lijst met Web Apps back-ups. |
> | Bewerking | Microsoft.Web/sites/backups/Read | Hiermee worden de eigenschappen van back-up van een web-app |
> | Bewerking | microsoft.web/sites/backups/restore/action | Herstellen van back-ups van Web-Apps. |
> | Bewerking | microsoft.web/sites/backups/write | Back-ups van Web-Apps bijwerken. |
> | Bewerking | microsoft.web/sites/config/delete | Configuratie van de Web-Apps verwijderen. |
> | Bewerking | Microsoft.Web/sites/config/list/Action | Overzicht van Web-App gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen |
> | Bewerking | Microsoft.Web/sites/config/Read | Instellingen voor Web-App-configuratie ophalen |
> | Bewerking | microsoft.web/sites/config/snapshots/read | Web Apps-Config momentopnamen ophalen. |
> | Bewerking | Microsoft.Web/sites/config/Write | Web-App-configuratie-instellingen bijwerken |
> | Bewerking | microsoft.web/sites/containerlogs/action | Containerlogbestanden ophalen voor Web-App worden ingepakt. |
> | Bewerking | microsoft.web/sites/containerlogs/download/action | Download de logboeken voor Web Apps in containers. |
> | Bewerking | microsoft.web/sites/continuouswebjobs/delete | Doorlopende webtaken van Web-Apps verwijderen. |
> | Bewerking | microsoft.web/sites/continuouswebjobs/read | Doorlopende webtaken van Web-Apps ophalen. |
> | Bewerking | microsoft.web/sites/continuouswebjobs/start/action | Doorlopende webtaken van Web Apps starten. |
> | Bewerking | microsoft.web/sites/continuouswebjobs/stop/action | Web Apps doorlopende webtaken stoppen. |
> | Bewerking | Microsoft.Web/sites/Delete | Een bestaande Web-App verwijderen |
> | Bewerking | microsoft.web/sites/deployments/delete | Verwijder Web Apps-implementaties. |
> | Bewerking | microsoft.web/sites/deployments/log/read | Get-logboek voor Web Apps-implementaties. |
> | Bewerking | microsoft.web/sites/deployments/read | Get-implementaties van Web-Apps. |
> | Bewerking | microsoft.web/sites/deployments/write | Update-implementaties van Web-Apps. |
> | Bewerking | microsoft.web/sites/detectors/read | Get-detectoren voor Web-Apps. |
> | Bewerking | microsoft.web/sites/diagnostics/analyses/execute/Action | Web Apps diagnostische analyses uitvoeren. |
> | Bewerking | microsoft.web/sites/diagnostics/analyses/read | Analyse van Web Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/aspnetcore/read | Diagnostische gegevens van Web-Apps ophalen voor ASP.NET Core-app. |
> | Bewerking | microsoft.web/sites/diagnostics/autoheal/read | Web Apps Diagnostics Autoheal ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/deployment/read | Implementatie van Web Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/deployments/read | Get-Web-Apps diagnostische gegevens over implementaties. |
> | Bewerking | microsoft.web/sites/diagnostics/detectors/execute/Action | Web Apps Diagnostics Detector worden uitgevoerd. |
> | Bewerking | microsoft.web/sites/diagnostics/detectors/read | Web Apps Diagnostics Detector ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Web Apps diagnostische gegevens over mislukte aanvragen Per Uri ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/frebanalysis/read | Get-Web-Apps diagnostische gegevens FREB analyse. |
> | Bewerking | microsoft.web/sites/diagnostics/loganalyzer/read | Get Web Apps Diagnostics Log Analyzer. |
> | Bewerking | microsoft.web/sites/diagnostics/read | Categorieën voor Web-Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/runtimeavailability/read | Web Apps diagnostische gegevens over Runtime-beschikbaarheid ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/servicehealth/read | Status van de Service Web Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Analyse van CPU-Web-Apps diagnostische gegevens Site ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/sitecrashes/read | Web Apps Diagnostics Site Crashes ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/sitelatency/read | Latentie van de Site Web Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Analyse van Web Apps diagnostische gegevens over de Site geheugen ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Get-Web-Apps diagnostische gegevens Site opnieuw opstarten instelling bijwerken. |
> | Bewerking | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Web Apps Diagnostics Site opnieuw starten door de gebruiker geïnitieerde ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/siteswap/read | Get-Site wisselen van Web Apps diagnostische gegevens. |
> | Bewerking | microsoft.web/sites/diagnostics/threadcount/read | Threadaantal van Web Apps diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/workeravailability/read | Web Apps Diagnostics Workeravailability ophalen. |
> | Bewerking | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Recyclen van Web Apps Diagnostics Worker proces ophalen. |
> | Bewerking | microsoft.web/sites/domainownershipidentifiers/read | Web Apps-domein eigenaar-id's ophalen |
> | Bewerking | microsoft.web/sites/domainownershipidentifiers/write | Web Apps-domein eigenaar-id's worden bijgewerkt. |
> | Bewerking | microsoft.web/sites/extensions/delete | Verwijderen van Web Apps-Site-uitbreidingen. |
> | Bewerking | microsoft.web/sites/extensions/read | Get-Web-Apps-Site-uitbreidingen. |
> | Bewerking | microsoft.web/sites/extensions/write | Web Apps-Site-extensies worden bijgewerkt. |
> | Bewerking | microsoft.web/sites/functions/action | Functies van Web-Apps. |
> | Bewerking | microsoft.web/sites/functions/delete | Verwijderen van functies voor Web-Apps. |
> | Bewerking | microsoft.web/sites/functions/listsecrets/action | Lijst met geheimen Web Apps-functies. |
> | Bewerking | microsoft.web/sites/functions/masterkey/read | Get-hoofdsleutel voor Web Apps-functies. |
> | Bewerking | microsoft.web/sites/functions/read | Get-functies voor Web-Apps. |
> | Bewerking | microsoft.web/sites/functions/token/read | Get Web Apps-functies-Token. |
> | Bewerking | microsoft.web/sites/functions/write | Web Apps-functies bijwerken. |
> | Bewerking | microsoft.web/sites/hostnamebindings/delete | Web Apps-Hostnaambindingen verwijderen. |
> | Bewerking | microsoft.web/sites/hostnamebindings/read | Web Apps-Hostnaambindingen ophalen. |
> | Bewerking | microsoft.web/sites/hostnamebindings/write | Web Apps-Hostnaambindingen bijwerken. |
> | Bewerking | microsoft.web/sites/hostruntime/functions/keys/read | Web Apps Hostruntime functies sleutels ophalen. |
> | Bewerking | Microsoft.Web/sites/hostruntime/host/_master/read | De hoofdsleutel van de functie-App voor de bewerkingen ophalen |
> | Bewerking | Microsoft.Web/sites/hostruntime/host/action | Runtime-actie, zoals triggers synchroniseren, functies toevoegen, aanroepen van functies, functies enzovoort verwijderen van functie-App uitvoeren. |
> | Bewerking | microsoft.web/sites/hostruntime/host/read | Web Apps Hostruntime Host ophalen. |
> | Bewerking | microsoft.web/sites/hybridconnection/delete | Web Apps hybride verbinding verwijderen. |
> | Bewerking | microsoft.web/sites/hybridconnection/read | Get-Web-Apps hybride verbinding. |
> | Bewerking | microsoft.web/sites/hybridconnection/write | Web Apps hybride verbinding bijwerken. |
> | Bewerking | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Web Apps hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Lijst met sleutels Web Apps hybride verbinding naamruimten Relays. |
> | Bewerking | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Get-Web-Apps hybride verbinding naamruimten Relays. |
> | Bewerking | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Web Apps hybride verbinding naamruimten Relays werk. |
> | Bewerking | microsoft.web/sites/hybridconnectionrelays/read | Get-Web-Apps hybride verbinding Relays. |
> | Bewerking | microsoft.web/sites/instances/deployments/delete | Web Apps-instanties implementaties verwijderen. |
> | Bewerking | microsoft.web/sites/instances/deployments/read | Web Apps-instanties implementaties ophalen. |
> | Bewerking | microsoft.web/sites/instances/extensions/log/read | Web Apps-instanties extensies Log ophalen. |
> | Bewerking | microsoft.web/sites/instances/extensions/processes/read | Web Apps-instanties extensies processen ophalen. |
> | Bewerking | microsoft.web/sites/instances/extensions/read | Get-extensies voor Web Apps-instanties. |
> | Bewerking | microsoft.web/sites/instances/processes/delete | Verwijderen van Web Apps-instanties processen. |
> | Bewerking | microsoft.web/sites/instances/processes/modules/read | Web Apps-instanties processen Modules ophalen. |
> | Bewerking | microsoft.web/sites/instances/processes/read | Web Apps-instanties processen ophalen. |
> | Bewerking | microsoft.web/sites/instances/processes/threads/read | Web Apps-instanties processen Threads ophalen. |
> | Bewerking | microsoft.web/sites/instances/read | Web Apps-instanties ophalen. |
> | Bewerking | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Lijst met synchronisatie functie Trigger Status Web-Apps. |
> | Bewerking | microsoft.web/sites/metricdefinitions/read | Web Apps metrische definities ophalen. |
> | Bewerking | microsoft.web/sites/metrics/read | Web Apps-metrische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/metricsdefinitions/read | Web Apps metrische definities ophalen. |
> | Bewerking | microsoft.web/sites/migratemysql/action | MySql-Web-Apps migreren. |
> | Bewerking | microsoft.web/sites/migratemysql/read | Ophalen van Web Apps MySql migreren. |
> | Bewerking | microsoft.web/sites/networktrace/action | Netwerk traceren Web-Apps. |
> | Bewerking | microsoft.web/sites/networktraces/operationresults/read | Web Apps-netwerk traceren Bewerkingsresultaten krijgt. |
> | Bewerking | microsoft.web/sites/newpassword/action | NewPassword Web-Apps. |
> | Bewerking | microsoft.web/sites/operationresults/read | Web Apps Bewerkingsresultaten krijgt. |
> | Bewerking | microsoft.web/sites/operations/read | Bewerkingen voor Web-Apps ophalen. |
> | Bewerking | microsoft.web/sites/perfcounters/read | Get-prestatiemeteritems voor Web-Apps. |
> | Bewerking | microsoft.web/sites/premieraddons/delete | Web Apps Premier-invoegtoepassingen verwijderen. |
> | Bewerking | microsoft.web/sites/premieraddons/read | Web Apps Premier-invoegtoepassingen ophalen. |
> | Bewerking | microsoft.web/sites/premieraddons/write | Web Apps Premier-invoegtoepassingen werken. |
> | Bewerking | microsoft.web/sites/privateaccess/read | Ophalen van gegevens over persoonlijke site toegang inschakelen en toegestane virtuele netwerken die toegang hebben tot de site. |
> | Bewerking | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Eindpunt van de particuliere verbindingen goedkeuren |
> | Bewerking | microsoft.web/sites/processes/modules/read | Web Apps processen Modules ophalen. |
> | Bewerking | microsoft.web/sites/processes/read | Web Apps processen ophalen. |
> | Bewerking | microsoft.web/sites/processes/threads/read | Get-Web-Apps processen Threads. |
> | Bewerking | microsoft.web/sites/publiccertificates/delete | Web Apps openbare certificaten verwijderen. |
> | Bewerking | microsoft.web/sites/publiccertificates/read | Web Apps openbare certificaten ophalen. |
> | Bewerking | microsoft.web/sites/publiccertificates/write | Web Apps openbare certificaten bijwerken. |
> | Bewerking | Microsoft.Web/sites/publish/Action | Een Web-App publiceren |
> | Bewerking | Microsoft.Web/sites/publishxml/Action | Ophalen van de XML-bestand profiel voor een Web-App publiceren |
> | Bewerking | microsoft.web/sites/publishxml/read | Get-Web-Apps publiceren XML. |
> | Bewerking | Microsoft.Web/sites/Read | Hiermee worden de eigenschappen van een Web-App |
> | Bewerking | microsoft.web/sites/recommendationhistory/read | Ophalen van de Web-Apps aanbeveling geschiedenis. |
> | Bewerking | microsoft.web/sites/recommendations/disable/action | Web Apps aanbevelingen uitschakelen. |
> | Bewerking | Microsoft.Web/sites/recommendations/Read | De lijst met aanbevelingen voor web-app ophalen. |
> | Bewerking | microsoft.web/sites/recover/action | Herstellen van Web-Apps. |
> | Bewerking | Microsoft.Web/sites/resetSlotConfig/Action | Web app-configuratie opnieuw instellen |
> | Bewerking | microsoft.web/sites/resourcehealthmetadata/read | Web Apps-Resource Health metagegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/restart/Action | Een Web-App opnieuw starten |
> | Bewerking | microsoft.web/sites/restore/read | Herstellen van Web-Apps ophalen. |
> | Bewerking | microsoft.web/sites/restore/write | Herstellen van WebApps. |
> | Bewerking | microsoft.web/sites/restorefrombackupblob/action | Web-App herstellen vanuit back-Blob. |
> | Bewerking | microsoft.web/sites/restorefromdeletedwebapp/action | Web-Apps herstellen van verwijderde App. |
> | Bewerking | microsoft.web/sites/restoresnapshot/action | Terugzetten van momentopnamen van de Web-Apps. |
> | Bewerking | microsoft.web/sites/siteextensions/delete | Verwijderen van Web Apps-Site-uitbreidingen. |
> | Bewerking | microsoft.web/sites/siteextensions/read | Get-Web-Apps-Site-uitbreidingen. |
> | Bewerking | microsoft.web/sites/siteextensions/write | Web Apps-Site-extensies worden bijgewerkt. |
> | Bewerking | microsoft.web/sites/slots/analyzecustomhostname/read | Ophalen van Web Apps-sleuven analyseren aangepaste hostnaam. |
> | Bewerking | Microsoft.Web/sites/slots/applySlotConfig/Action | Web app-site-configuratie van doelsleuf voor de huidige sleuf van toepassing. |
> | Bewerking | Microsoft.Web/sites/slots/backup/Action | Nieuwe Web-App sleuf back-up maken. |
> | Bewerking | microsoft.web/sites/slots/backup/read | Get-back-up-sleuven voor Web-Apps. |
> | Bewerking | microsoft.web/sites/slots/backup/write | Web Apps-sleuven back-up. |
> | Bewerking | microsoft.web/sites/slots/backups/action | Web Apps-sleuven back-ups detecteren. |
> | Bewerking | microsoft.web/sites/slots/backups/delete | Back-ups sleuven voor Web-Apps verwijderen. |
> | Bewerking | microsoft.web/sites/slots/backups/list/action | Lijst met Web Apps-sleuven back-ups. |
> | Bewerking | Microsoft.Web/sites/slots/backups/Read | Hiermee worden de eigenschappen van een web-appsites back-up |
> | Bewerking | microsoft.web/sites/slots/backups/restore/action | Web Apps-sleuven back-ups herstellen. |
> | Bewerking | microsoft.web/sites/slots/config/delete | Delete Web Apps Slots Config. |
> | Bewerking | Microsoft.Web/sites/slots/config/list/Action | Overzicht van Web-App sleuf gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen |
> | Bewerking | Microsoft.Web/sites/slots/config/Read | Configuratie-instellingen voor Web-App sleuf ophalen |
> | Bewerking | Microsoft.Web/sites/slots/config/Write | Web-App sleuf configuratie-instellingen bijwerken |
> | Bewerking | microsoft.web/sites/slots/containerlogs/action | Logboeken voor containers ophalen voor de Web-App-site worden ingepakt. |
> | Bewerking | microsoft.web/sites/slots/containerlogs/download/action | Web Apps-sleuven Containerlogboeken downloaden. |
> | Bewerking | microsoft.web/sites/slots/continuouswebjobs/delete | Web Apps-sleuven doorlopende webtaken verwijderen. |
> | Bewerking | microsoft.web/sites/slots/continuouswebjobs/read | Web Apps-sleuven doorlopende webtaken ophalen. |
> | Bewerking | microsoft.web/sites/slots/continuouswebjobs/start/action | Web Apps-sleuven doorlopende webtaken starten. |
> | Bewerking | microsoft.web/sites/slots/continuouswebjobs/stop/action | Web Apps-sleuven doorlopende webtaken stoppen. |
> | Bewerking | Microsoft.Web/sites/slots/Delete | Verwijderen van een bestaande sleuf van de Web-App |
> | Bewerking | microsoft.web/sites/slots/deployments/delete | Web Apps-sleuven implementaties verwijderen. |
> | Bewerking | microsoft.web/sites/slots/deployments/log/read | Web Apps-sleuven implementaties Log ophalen. |
> | Bewerking | microsoft.web/sites/slots/deployments/read | Web Apps-sleuven implementaties ophalen. |
> | Bewerking | microsoft.web/sites/slots/deployments/write | Update-implementaties van Web Apps-sleuven. |
> | Bewerking | microsoft.web/sites/slots/detectors/read | Web Apps-sleuven detectoren ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Web Apps-sleuven diagnostische analyses uitvoeren. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/analyses/read | Analyse van Web Apps-sleuven diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Web Apps-sleuven diagnostische gegevens ophalen voor ASP.NET Core-app. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/autoheal/read | Web Apps-sleuven Diagnostics Autoheal ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/deployment/read | Implementatie van Web Apps-sleuven diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/deployments/read | Web Apps-sleuven diagnostische gegevens over implementaties ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Web Apps-sleuven Diagnostics Detector worden uitgevoerd. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/detectors/read | Web Apps-sleuven Diagnostics Detector ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Get-Web-Apps-sleuven Diagnostics FREB analyse. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Get Web Apps Slots Diagnostics Log Analyzer. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/read | Web Apps-sleuven diagnostische gegevens ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Web Apps-sleuven diagnostische gegevens over Runtime-beschikbaarheid ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/servicehealth/read | Web Apps-sleuven Diagnostics-servicestatus ophalen |
> | Bewerking | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Get Web Apps-sleuven diagnostische Site analyse van CPU. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Web Apps-sleuven Diagnostics Site Crashes ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/sitelatency/read | Get-latentie van Web Apps-sleuven diagnostische gegevens van Site. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Get-analyse over het geheugen van de Site van Web Apps-sleuven diagnostische gegevens. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Get-Web-Apps-sleuven Diagnostics Site opnieuw opstarten instelling bijwerken. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Web Apps-sleuven Diagnostics Site opnieuw starten door de gebruiker geïnitieerde ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/siteswap/read | Get-Site wisselen van Web Apps-sleuven diagnostische gegevens. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/threadcount/read | Get-threads van Web Apps-sleuven diagnostische gegevens. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/workeravailability/read | Web Apps-sleuven Diagnostics Workeravailability ophalen. |
> | Bewerking | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Recyclen van Web Apps-sleuven Diagnostics Worker proces ophalen. |
> | Bewerking | microsoft.web/sites/slots/domainownershipidentifiers/read | Get Web Apps Slots Domain Ownership Identifiers. |
> | Bewerking | microsoft.web/sites/slots/functions/listsecrets/action | Lijst met geheimen Web Apps-sleuven functies. |
> | Bewerking | microsoft.web/sites/slots/functions/read | Get-functies van Web Apps-sleuven. |
> | Bewerking | microsoft.web/sites/slots/hostnamebindings/delete | Web Apps-sleuven Hostnaambindingen verwijderen. |
> | Bewerking | microsoft.web/sites/slots/hostnamebindings/read | Web Apps-sleuven Hostnaambindingen ophalen. |
> | Bewerking | microsoft.web/sites/slots/hostnamebindings/write | Web Apps-sleuven Hostnaambindingen bijwerken. |
> | Bewerking | microsoft.web/sites/slots/hybridconnection/delete | Web Apps-sleuven hybride verbinding verwijderen. |
> | Bewerking | microsoft.web/sites/slots/hybridconnection/read | Get-Web-Apps-sleuven hybride verbinding. |
> | Bewerking | microsoft.web/sites/slots/hybridconnection/write | Web Apps-sleuven hybride verbinding bijwerken. |
> | Bewerking | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Web Apps-sleuven hybride verbinding naamruimten Relays verwijderen. |
> | Bewerking | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Web Apps-sleuven hybride verbinding naamruimten Relays werk. |
> | Bewerking | microsoft.web/sites/slots/hybridconnectionrelays/read | Web Apps-sleuven hybride verbinding Relays ophalen. |
> | Bewerking | microsoft.web/sites/slots/instances/deployments/read | Web Apps-sleuven exemplaren implementaties ophalen. |
> | Bewerking | microsoft.web/sites/slots/instances/processes/delete | Verwijderen van Web Apps-sleuven exemplaren processen. |
> | Bewerking | microsoft.web/sites/slots/instances/processes/read | Web Apps-sleuven exemplaren processen ophalen. |
> | Bewerking | microsoft.web/sites/slots/instances/read | Web Apps-sleuven instanties ophalen. |
> | Bewerking | microsoft.web/sites/slots/metricdefinitions/read | Web Apps-sleuven metrische definities ophalen. |
> | Bewerking | microsoft.web/sites/slots/metrics/read | Metrische gegevens voor sleuven van Web-Apps ophalen. |
> | Bewerking | microsoft.web/sites/slots/migratemysql/read | Get Web Apps Slots Migrate MySql. |
> | Bewerking | microsoft.web/sites/slots/networktrace/action | Netwerk traceren Web Apps-sleuven. |
> | Bewerking | microsoft.web/sites/slots/networktraces/operationresults/read | Web Apps-sleuven netwerk traceren Bewerkingsresultaten ophalen. |
> | Bewerking | microsoft.web/sites/slots/newpassword/action | Sleuven NewPassword Web-Apps. |
> | Bewerking | microsoft.web/sites/slots/operationresults/read | Web Apps-sleuven Bewerkingsresultaten krijgt. |
> | Bewerking | microsoft.web/sites/slots/operations/read | Web Apps-sleuven bewerkingen ophalen. |
> | Bewerking | microsoft.web/sites/slots/perfcounters/read | Prestatiemeteritems voor Web Apps-sleuven ophalen. |
> | Bewerking | microsoft.web/sites/slots/phplogging/read | Web Apps-sleuven Phplogging ophalen. |
> | Bewerking | microsoft.web/sites/slots/premieraddons/delete | Web Apps sleuven Premier-invoegtoepassingen verwijderen. |
> | Bewerking | microsoft.web/sites/slots/premieraddons/read | Web Apps sleuven Premier-invoegtoepassingen ophalen. |
> | Bewerking | microsoft.web/sites/slots/premieraddons/write | Web Apps sleuven Premier-invoegtoepassingen werken. |
> | Bewerking | microsoft.web/sites/slots/processes/read | Web Apps-sleuven processen ophalen. |
> | Bewerking | microsoft.web/sites/slots/publiccertificates/delete | Web Apps-sleuven openbare certificaten verwijderen. |
> | Bewerking | microsoft.web/sites/slots/publiccertificates/read | Web Apps-sleuven openbare certificaten ophalen. |
> | Bewerking | microsoft.web/sites/slots/publiccertificates/write | Maken of bijwerken van Web Apps-sleuven openbare certificaten. |
> | Bewerking | Microsoft.Web/sites/slots/publish/Action | Een Web-App-site publiceren |
> | Bewerking | Microsoft.Web/sites/slots/publishxml/Action | Ophalen van de XML-bestand profiel voor de Web-App-site publiceren |
> | Bewerking | Microsoft.Web/sites/slots/Read | Hiermee worden de eigenschappen van de implementatiesite voor een Web-App |
> | Bewerking | microsoft.web/sites/slots/recover/action | Web Apps-sleuven herstellen. |
> | Bewerking | Microsoft.Web/sites/slots/resetSlotConfig/Action | Web app-site-configuratie opnieuw instellen |
> | Bewerking | microsoft.web/sites/slots/resourcehealthmetadata/read | Web Apps-sleuven Resource Health metagegevens ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/restart/Action | Opnieuw opstarten van een Web-App-sleuf |
> | Bewerking | microsoft.web/sites/slots/restore/read | Get-Web-Apps-sleuven terugzetten. |
> | Bewerking | microsoft.web/sites/slots/restore/write | Web Apps-sleuven herstellen. |
> | Bewerking | microsoft.web/sites/slots/restorefrombackupblob/action | Web Apps-sleuf herstellen vanuit back-Blob. |
> | Bewerking | microsoft.web/sites/slots/restorefromdeletedwebapp/action | Web-Appsites herstellen van verwijderde App. |
> | Bewerking | microsoft.web/sites/slots/restoresnapshot/action | Web Apps-sleuven momentopnamen terugzetten. |
> | Bewerking | microsoft.web/sites/slots/siteextensions/delete | Site-uitbreidingen voor Web Apps-sleuven verwijderen. |
> | Bewerking | microsoft.web/sites/slots/siteextensions/read | Get-Site-uitbreidingen voor Web Apps-sleuven. |
> | Bewerking | microsoft.web/sites/slots/siteextensions/write | Site-uitbreidingen voor Web Apps-sleuven bijwerken. |
> | Bewerking | Microsoft.Web/sites/slots/slotsdiffs/Action | Ophalen van de verschillen in de configuratie tussen web-app en sleuven |
> | Bewerking | Microsoft.Web/sites/slots/slotsswap/Action | Web-App implementatiesites wisselen |
> | Bewerking | microsoft.web/sites/slots/snapshots/read | Web Apps-sleuven momentopnamen ophalen. |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Delete | Instellingen voor Web-App sleuf bronbeheer configuratie verwijderen |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Read | Configuratie-instellingen voor broncodebeheer voor Web-App sleuf ophalen |
> | Bewerking | Microsoft.Web/sites/slots/sourcecontrols/Write | Web-App sleuf bron besturingselement configuratie-instellingen bijwerken |
> | Bewerking | Microsoft.Web/sites/slots/start/Action | Een Web-App-site starten |
> | Bewerking | Microsoft.Web/sites/slots/stop/Action | Een Web-App-site stoppen |
> | Bewerking | microsoft.web/sites/slots/sync/action | Synchronisatie van Web Apps-sleuven. |
> | Bewerking | microsoft.web/sites/slots/triggeredwebjobs/delete | Web Apps-sleuven geactiveerde webtaken verwijderen. |
> | Bewerking | microsoft.web/sites/slots/triggeredwebjobs/read | Get Web Apps Slots Triggered WebJobs. |
> | Bewerking | microsoft.web/sites/slots/triggeredwebjobs/run/action | Run Web Apps Slots Triggered WebJobs. |
> | Bewerking | microsoft.web/sites/slots/usages/read | Get Web Apps Slots Usages. |
> | Bewerking | microsoft.web/sites/slots/virtualnetworkconnections/delete | Verwijderen van Web Apps-sleuven virtuele netwerkverbindingen. |
> | Bewerking | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Web Apps-sleuven virtueel netwerk verbindingen Gateways bijwerken. |
> | Bewerking | microsoft.web/sites/slots/virtualnetworkconnections/read | Web Apps-sleuven Virtual Network-verbindingen ophalen. |
> | Bewerking | microsoft.web/sites/slots/virtualnetworkconnections/write | Web Apps-sleuven virtuele netwerkverbindingen worden bijgewerkt. |
> | Bewerking | microsoft.web/sites/slots/webjobs/read | Get Web Apps Slots WebJobs. |
> | Bewerking | Microsoft.Web/sites/slots/Write | Een nieuwe sleuf van de Web-App maken of een bestaande bijgewerkt |
> | Bewerking | Microsoft.Web/sites/slotsdiffs/Action | Ophalen van de verschillen in de configuratie tussen web-app en sleuven |
> | Bewerking | Microsoft.Web/sites/slotsswap/Action | Web-App implementatiesites wisselen |
> | Bewerking | microsoft.web/sites/snapshots/read | Momentopnamen van de Web-Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Delete | Instellingen voor Web-App bronbeheer-configuratie verwijderen |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Read | Broncodebeheer voor Web-App-configuratie-instellingen ophalen |
> | Bewerking | Microsoft.Web/sites/sourcecontrols/Write | Instellingen voor configuratie bronbeheer van Web-App bijwerken |
> | Bewerking | Microsoft.Web/sites/start/Action | Een Web-App starten |
> | Bewerking | Microsoft.Web/sites/stop/Action | Een Web-App stoppen |
> | Bewerking | microsoft.web/sites/sync/action | Synchronisatie van Web-Apps. |
> | Bewerking | microsoft.web/sites/syncfunctiontriggers/action | Synchronisatie-functie wordt geactiveerd voor Web-Apps. |
> | Bewerking | microsoft.web/sites/triggeredwebjobs/delete | Geactiveerde webtaken voor Web-Apps verwijderen. |
> | Bewerking | microsoft.web/sites/triggeredwebjobs/history/read | Ophalen van de Web Apps geactiveerde webtaken geschiedenis. |
> | Bewerking | microsoft.web/sites/triggeredwebjobs/read | Get-Web-Apps geactiveerde webtaken. |
> | Bewerking | microsoft.web/sites/triggeredwebjobs/run/action | Web Apps geactiveerde webtaken uitvoeren. |
> | Bewerking | microsoft.web/sites/usages/read | Het gebruik van Web-Apps ophalen. |
> | Bewerking | microsoft.web/sites/virtualnetworkconnections/delete | Verwijder virtuele netwerkverbindingen van Web-Apps. |
> | Bewerking | microsoft.web/sites/virtualnetworkconnections/gateways/read | Web Apps virtuele verbindingen netwerkgateways ophalen. |
> | Bewerking | microsoft.web/sites/virtualnetworkconnections/gateways/write | Web Apps virtueel netwerk verbindingen Gateways bijwerken. |
> | Bewerking | microsoft.web/sites/virtualnetworkconnections/read | Web Apps Virtual Network-verbindingen ophalen. |
> | Bewerking | microsoft.web/sites/virtualnetworkconnections/write | Web Apps-virtuele netwerkverbindingen worden bijgewerkt. |
> | Bewerking | microsoft.web/sites/webjobs/read | WebJobs voor Web-Apps ophalen. |
> | Bewerking | Microsoft.Web/sites/Write | Een nieuwe Web-App maken of een bestaande bijgewerkt |
> | Bewerking | microsoft.web/skus/read | SKU's ophalen. |
> | Bewerking | microsoft.web/sourcecontrols/read | Besturingselementen voor gegevensbronnen ophalen. |
> | Bewerking | microsoft.web/sourcecontrols/write | Besturingselementen voor gegevensbronnen bijwerken. |
> | Bewerking | microsoft.web/unregister/action | Registratie van de resourceprovider Microsoft.Web voor het abonnement. |
> | Bewerking | microsoft.web/validate/action | Valideren. |
> | Bewerking | microsoft.web/verifyhostingenvironmentvnet/action | Controleer of die als host fungeert omgeving Vnet. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Actietype | Bewerking | Description |
> | --- | --- | --- |
> | Bewerking | Microsoft.WorkloadMonitor/components/read | Hiermee haalt u onderdelen voor de resource |
> | Bewerking | Microsoft.WorkloadMonitor/componentsSummary/read | Overzicht van onderdelen opgehaald |
> | Bewerking | Microsoft.WorkloadMonitor/monitorInstances/read | Hiermee haalt u exemplaren van de monitors voor de resource |
> | Bewerking | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Overzicht van de monitor-instanties opgehaald |
> | Bewerking | Microsoft.WorkloadMonitor/monitors/read | Hiermee haalt u monitors voor de resource |
> | Bewerking | Microsoft.WorkloadMonitor/monitors/write | Monitor voor de resource configureren |
> | Bewerking | Microsoft.WorkloadMonitor/notificationSettings/read | Instellingen voor meldingen voor de resource opgehaald |
> | Bewerking | Microsoft.WorkloadMonitor/notificationSettings/write | Configureren van instellingen voor meldingen voor de resource |
> | Bewerking | Microsoft.WorkloadMonitor/operations/read | De ondersteunde bewerkingen opgehaald |

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Ingebouwde rollen voor Azure-resources](built-in-roles.md)

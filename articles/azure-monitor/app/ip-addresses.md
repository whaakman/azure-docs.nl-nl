---
title: IP-adressen die worden gebruikt door Application Insights en Log Analytics | Microsoft Docs
description: Firewall-uitzonderingen die zijn vereist voor Application Insights
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mbullwin
ms.openlocfilehash: 30c4449893cf1286485d256d98a86ae6fbe2688c
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537151"
---
# <a name="ip-addresses-used-by-application-insights-and-log-analytics"></a>IP-adressen die worden gebruikt door Application Insights en Log Analytics
De [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) service maakt gebruik van een aantal IP-adressen. Mogelijk moet u deze adressen weten als de app die u bewaakt wordt gehost achter een firewall.

> [!NOTE]
> Hoewel deze adressen statisch zijn, is het mogelijk dat we moeten deze van tijd tot tijd wijzigen. Application Insights al het verkeer vertegenwoordigt uitgaand verkeer met uitzondering van de beschikbaarheidsbewaking van webtoepassingen en webhooks waarvoor firewallregels voor binnenkomend verkeer.
> 
> 

> [!TIP]
> Abonneren op deze pagina als een RSS-feed door toe te voegen https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/azure-monitor/app/ip-addresses.md.atom naar uw favoriete RSS-/ ATOM-lezer om te worden geïnformeerd van de meest recente wijzigingen.
> 
> 

## <a name="outgoing-ports"></a>Uitgaande poorten
U moet het aantal uitgaande poorten openen in de firewall van uw server om toe te staan de Application Insights-SDK en/of Status Monitor om gegevens te verzenden naar de portal:

| Doel | URL | IP | Poorten |
| --- | --- | --- | --- |
| Telemetrie |dc.services.visualstudio.com<br/>dc.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244<br/>40.85.218.175<br/>104.211.92.54<br/>52.175.198.74<br/>51.140.6.23<br/>40.71.12.231<br/>13.69.65.22<br/>13.78.108.165<br/>13.70.72.233<br/>20.44.8.7<br/>13.86.218.248<br/>40.79.138.41<br/>52.231.18.241<br/>13.75.38.7<br/>102.133.162.117<br/>40.73.171.20<br/>102.133.155.50 | 443 |
| Live Metrics Stream |rt.services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |

## <a name="status-monitor"></a>Status Monitor
Status monitorconfiguratie - die nodig zijn alleen als u wijzigingen aanbrengt.

| Doel | URL | IP | Poorten |
| --- | --- | --- | --- |
| Configuratie |`management.core.windows.net` | |`443` |
| Configuratie |`management.azure.com` | |`443` |
| Configuratie |`login.windows.net` | |`443` |
| Configuratie |`login.microsoftonline.com` | |`443` |
| Configuratie |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Configuratie |`auth.gfx.ms` | |`443` |
| Configuratie |`login.live.com` | |`443` |
| Installatie |`packages.nuget.org` , `nuget.org`, `api.nuget.org`, `az320820.vo.msecnd.net` (NuGet-Downloads) | |`443` |

## <a name="availability-tests"></a>Beschikbaarheidstests
Dit is de lijst met adressen waaruit [webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md) worden uitgevoerd. Als u webtests uitvoeren op uw app wilt maken, maar uw webserver beperkt is tot specifieke clients van dienst, hebt u inkomend verkeer toestaan van onze beschikbaarheid testen van servers.

Open poorten 80 (http) en 443 (https) voor inkomend verkeer van deze adressen (IP-adressen zijn gegroepeerd op locatie):

```
Australia East
20.40.124.176/28
20.40.124.240/28
20.40.125.80/28

Brazil South
191.233.26.176/28
191.233.26.128/28
191.233.26.64/28

France Central (Formerly France South)
20.40.129.96/28
20.40.129.112/28
20.40.129.128/28
20.40.129.144/28

France Central
20.40.129.32/28
20.40.129.48/28
20.40.129.64/28
20.40.129.80/28

East Asia
52.229.216.48/28
52.229.216.64/28
52.229.216.80/28

North Europe
52.158.28.64/28
52.158.28.80/28
52.158.28.96/28
52.158.28.112/28

Japan East
52.140.232.160/28
52.140.232.176/28
52.140.232.192/28

West Europe
51.144.56.96/28
51.144.56.112/28
51.144.56.128/28
51.144.56.144/28
51.144.56.160/28
51.144.56.176/28

UK South
51.105.9.128/28
51.105.9.144/28
51.105.9.160/28

UK West
20.40.104.96/28
20.40.104.112/28
20.40.104.128/28
20.40.104.144/28

Southeast Asia
52.139.250.96/28
52.139.250.112/28
52.139.250.128/28
52.139.250.144/28

West US
40.91.82.48/28
40.91.82.64/28
40.91.82.80/28
40.91.82.96/28
40.91.82.112/28
40.91.82.128/28

Central US
13.86.97.224/28
13.86.97.240/28
13.86.98.48/28
13.86.98.0/28
13.86.98.16/28
13.86.98.64/28

North Central US
23.100.224.16/28
23.100.224.32/28
23.100.224.48/28
23.100.224.64/28
23.100.224.80/28
23.100.224.96/28
23.100.224.112/28
23.100.225.0/28

South Central US
20.45.5.160/28
20.45.5.176/28
20.45.5.192/28
20.45.5.208/28
20.45.5.224/28
20.45.5.240/28

East US
20.42.35.32/28
20.42.35.64/28
20.42.35.80/28
20.42.35.96/28
20.42.35.112/28
20.42.35.128/28

```  

## <a name="application-insights-api"></a>Application Insights-API
| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| API |api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80,443 |
| API-documenten |dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80,443 |
| Interne API |aigs.aisvc.visualstudio.com<br/>aigs1.aisvc.visualstudio.com<br/>aigs2.aisvc.visualstudio.com<br/>aigs3.aisvc.visualstudio.com<br/>aigs4.aisvc.visualstudio.com<br/>aigs5.aisvc.visualstudio.com<br/>aigs6.aisvc.visualstudio.com |Dynamische|443 |

## <a name="log-analytics-api"></a>Log Analytics-API

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| API |api.loganalytics.io<br/>*.api.loganalytics.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80,443 |
| API-documenten |dev.loganalytics.io<br/>docs.loganalytics.io<br/>www.loganalytics.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80,443 |

## <a name="application-insights-analytics"></a>Application Insights Analytics

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Analytics-Portal | analytics.applicationinsights.io | Dynamische | 80,443 |
| CDN | applicationanalytics.azureedge.net | Dynamische | 80,443 |
| Media CDN | applicationanalyticsmedia.azureedge.net | Dynamische | 80,443 |

Opmerking: *. applicationinsights.io domein is eigendom van Application Insights-team.

## <a name="log-analytics-portal"></a>Log Analytics-Portal

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Portal | portal.loganalytics.io | Dynamische | 80,443 |
| CDN | applicationanalytics.azureedge.net | Dynamische | 80,443 |

Opmerking: *. loganalytics.io domein eigendom is van het team van Log Analytics.

## <a name="application-insights-azure-portal-extension"></a>Application Insights-Azure portal extensie

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Application Insights-extensie | stamp2.app.insightsportal.visualstudio.com | Dynamische | 80,443 |
| Application Insights-extensie CDN | insightsportal-prod2-cdn.aisvc.visualstudio.com<br/>insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com<br/>insightsportal-cdn-aimon.applicationinsights.io | Dynamische | 80,443 |

## <a name="application-insights-sdks"></a>Application Insights-SDK 's

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Application Insights-JS SDK CDN | az416426.vo.msecnd.net | Dynamische | 80,443 |
| Application Insights Java SDK | aijavasdk.blob.core.windows.net | Dynamische | 80,443 |

## <a name="alert-webhooks"></a>Webhooks voor waarschuwingen

| Doel | IP | Poorten
| --- | --- | --- |
| Waarschuwingen | 23.96.11.4 | 443 |

## <a name="profiler"></a>Profiler

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Agent | agent.azureserviceprofiler.net<br/>*.agent.azureserviceprofiler.net | 40.68.32.221<br/>40.85.246.0<br/>40.85.246.57<br/>40.117.252.0<br/>40.117.253.100<br/>51.140.140.162<br/>51.140.140.184<br/>51.143.96.206<br/>51.143.98.157<br/>52.161.8.88<br/>52.161.29.225<br/>52.178.147.66<br/>52.178.149.106<br/>52.230.122.9<br/>52.230.124.46<br/>104.40.217.71<br/>104.211.89.26<br/>104.211.90.232<br/>20.190.60.38<br/>20.190.60.32<br/>52.173.196.230<br/>52.173.196.209<br/>23.102.44.211<br/>23.102.45.216<br/>13.69.51.218<br/>13.69.51.175<br/>138.91.32.98<br/>138.91.37.93<br/>40.121.61.208<br/>40.121.57.2<br/>51.140.60.235<br/>51.140.180.52<br/>52.138.31.112<br/>52.138.31.127<br/>104.211.90.234<br/>104.211.91.254<br/>13.70.124.27<br/>13.75.195.15<br/>52.185.132.101<br/>52.185.132.170<br/>20.188.36.28<br/>40.89.153.171<br/>52.141.22.239<br/>52.141.22.149<br/>102.133.162.233<br/>102.133.161.73 | 443
| Portal | gateway.azureserviceprofiler.net | Dynamische | 443
| Opslag | *.core.windows.net | Dynamische | 443

## <a name="snapshot-debugger"></a>Snapshot Debugger

> [!NOTE]
> Profiler- en Snapshot Debugger delen dezelfde set IP-adressen.

| Doel | URI | IP | Poorten |
| --- | --- | --- | --- |
| Agent | ppe.azureserviceprofiler.net<br/>*.ppe.azureserviceprofiler.net | 40.68.32.221<br/>40.85.246.0<br/>40.85.246.57<br/>40.117.252.0<br/>40.117.253.100<br/>51.140.140.162<br/>51.140.140.184<br/>51.143.96.206<br/>51.143.98.157<br/>52.161.8.88<br/>52.161.29.225<br/>52.178.147.66<br/>52.178.149.106<br/>52.230.122.9<br/>52.230.124.46<br/>104.40.217.71<br/>104.211.89.26<br/>104.211.90.232<br/>20.190.60.38<br/>20.190.60.32<br/>52.173.196.230<br/>52.173.196.209<br/>23.102.44.211<br/>23.102.45.216<br/>13.69.51.218<br/>13.69.51.175<br/>138.91.32.98<br/>138.91.37.93<br/>40.121.61.208<br/>40.121.57.2<br/>51.140.60.235<br/>51.140.180.52<br/>52.138.31.112<br/>52.138.31.127<br/>104.211.90.234<br/>104.211.91.254<br/>13.70.124.27<br/>13.75.195.15<br/>52.185.132.101<br/>52.185.132.170<br/>20.188.36.28<br/>40.89.153.171<br/>52.141.22.239<br/>52.141.22.149<br/>102.133.162.233<br/>102.133.161.73 | 443
| Portal | ppe.gateway.azureserviceprofiler.net | Dynamische | 443
| Opslag | *.core.windows.net | Dynamische | 443

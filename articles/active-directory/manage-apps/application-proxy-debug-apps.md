---
title: Fouten opsporen in toepassingsproxy-toepassingen - Azure Active Directory | Microsoft Docs
description: Fouten opsporen in problemen met toepassingsproxy van Azure Active Directory (Azure AD)-toepassingen.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: d0a12bde119e9dae3f950603fac4bce060bb5f91
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66172263"
---
# <a name="debug-application-proxy-application-issues"></a>Fouten opsporen in problemen met toepassingsproxy-toepassingen 

Dit artikel helpt bij het oplossen van problemen met toepassingsproxy van Azure Active Directory (Azure AD)-toepassingen. Als u de Application Proxy-service voor externe toegang tot een on-premises web-toepassing, maar u problemen ondervindt bij het verbinding maken met de toepassing, gebruikt u deze stroomdiagram fouten opsporen in problemen met toepassingen. 

## <a name="before-you-begin"></a>Voordat u begint

Bij het oplossen van problemen met Application Proxy, wordt u aangeraden dat u beginnen met de connectors. Voer eerst de werkstroom voor het oplossen van problemen in [problemen met fouten opsporen in Application Proxy Connector](application-proxy-debug-connectors.md) om ervoor te zorgen Application Proxy connectors correct zijn geconfigureerd. Als u steeds problemen ondervindt nog, kunt u terug naar dit artikel om op te lossen van de toepassing.  

Zie voor meer informatie over de toepassingsproxy:

- [Externe toegang tot on-premises toepassingen via de toepassingsproxy](application-proxy.md)
- [Application Proxy connectors](application-proxy-connectors.md)
- [Installeren en registreren van een connector](application-proxy-add-on-premises-application.md)
- [Oplossen van problemen met Application Proxy- en foutberichten](application-proxy-troubleshoot.md)

## <a name="flowchart-for-application-issues"></a>Stroomdiagram voor problemen met toepassingen

Deze stroomdiagram leidt u door de stappen voor het opsporen van fouten van de meest voorkomende problemen met de verbinding te maken met de toepassing. Zie voor meer informatie over elke stap in de tabel de stroomdiagram te volgen.

![Stroomdiagram dat toont de stappen voor het opsporen van fouten in een toepassing](media/application-proxy-debug-apps/application-proxy-apps-debugging-flowchart.png)

|  | Bewerking | Description | 
|---------|---------|---------|
|1 | Open een browser en voer uw referenties toegang tot de app | Probeer het met uw referenties aanmelden bij de app en Controleer op fouten met betrekking tot gebruiker, zoals [deze zakelijke app kan niet worden geopend](application-proxy-sign-in-bad-gateway-timeout-error.md). |
|2 | Controleer of de toewijzing van de gebruiker naar de app | Zorg ervoor dat uw account toegang heeft tot de app uit in het bedrijfsnetwerk bevinden, en test u aanmelden bij de app met de volgende stappen in [de toepassing testen](application-proxy-add-on-premises-application.md#test-the-application). Als de aanmelding bij problemen zich blijven voordoen, raadpleegt u [oplossen aanmelden](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-troubleshoot-sign-in-errors).  |
|3 | Open een browser en probeert te krijgen tot de app. | Als er een fout wordt onmiddellijk weergegeven, Controleer of de Application Proxy correct is geconfigureerd. Zie voor meer informatie over de specifieke foutberichten [Application Proxy oplossen van problemen en foutberichten](application-proxy-troubleshoot.md).  |
|4 | Controleer de instellingen voor uw aangepaste domein of de fout oplossen | Als de pagina helemaal niet wordt weergegeven, controleert u of uw aangepaste domein is correct geconfigureerd aan de hand [werken met aangepaste domeinen](application-proxy-configure-custom-domain.md).<br></br>Als de pagina niet worden geladen en een foutbericht wordt weergegeven, de fout oplossen door te verwijzen naar [Application Proxy oplossen van problemen en foutberichten](application-proxy-troubleshoot.md). <br></br>Als het duurt langer dan 20 seconden voor een foutbericht wordt weergegeven, is er mogelijk probleem met de netwerkverbinding. Ga naar de [fouten opsporen in Application Proxy connectors](application-proxy-debug-connectors.md) artikel voor probleemoplossing.  |
|5 | Als de problemen zich blijven voordoen, gaat u naar het connector-foutopsporing | Het is mogelijk dat er een probleem met de netwerkverbinding tussen de proxy en de connector of de connector en de back-end. Ga naar de [fouten opsporen in Application Proxy connectors](application-proxy-debug-connectors.md) artikel voor probleemoplossing. |
|6 | Publiceren van alle resources, het controleren van de browser ontwikkelhulpprogramma's en koppelingen herstellen | Zorg ervoor dat het publicatiepad bevat alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing. Zie voor meer informatie, [een on-premises app toevoegen aan Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). <br></br>Gebruik de ontwikkelhulpprogramma's (F12 extra in Internet Explorer of Microsoft Edge) en het selectievakje van de browser voor het publiceren van problemen, zoals beschreven in [toepassingspagina wordt niet correct weergegeven](application-proxy-page-appearance-broken-problem.md). <br></br>Bekijk de opties voor het oplossen van verbroken koppelingen in [koppelingen op de pagina werken niet](application-proxy-page-links-broken-problem.md). |
|7 | Controleren op de netwerklatentie | Als de pagina langzaam wordt geladen, informatie over manieren voor minimale netwerklatentie in [overwegingen voor de latentie wordt verminderd](application-proxy-network-topology.md#considerations-for-reducing-latency). | 
|8 | Zie aanvullende hulp voor het oplossen van problemen | Als de problemen zich blijven voordoen, vindt u aanvullende artikelen over probleemoplossing in de [Application Proxy-documentatie voor probleemoplossing](application-proxy-page-appearance-broken-problem.md). |

## <a name="next-steps"></a>Volgende stappen


* [Publiceren van toepassingen op afzonderlijke netwerken en locaties met behulp van connectorgroepen](application-proxy-connector-groups.md)
* [Werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md)
* [Application Proxy en de connector-fouten oplossen](application-proxy-troubleshoot.md)
* [De Azure AD Application Proxy Connector op de achtergrond installeren](application-proxy-register-connector-powershell.md)

---
title: 'Fouten opsporen in Application Proxy connectors: Azure Active Directory | Microsoft Docs'
description: Problemen met Azure Active Directory (Azure AD) Application Proxy connectors kunt verhelpen.
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
ms.openlocfilehash: c3088ae777fe1a64be218105d36fdb9e01d7b798
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66172233"
---
# <a name="debug-application-proxy-connector-issues"></a>Application Proxy connector problemen verhelpen 

Dit artikel helpt bij het oplossen van problemen met Azure Active Directory (Azure AD) Application Proxy connectors. Als u de Application Proxy-service voor externe toegang tot een on-premises web-toepassing, maar u problemen ondervindt bij het verbinding maken met de toepassing, gebruikt u deze stroomdiagram om op te sporen connector problemen. 

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u de Application Proxy-connector hebt geïnstalleerd en een probleem zijn. Bij het oplossen van problemen met Application Proxy, wordt u aangeraden dat u begint met deze stroom oplossen om te bepalen of de Application Proxy connectors correct zijn geconfigureerd. Als u er nog steeds geen verbinding met de toepassing, volgt u de werkstroom voor het oplossen van problemen in [fouten opsporen in Application Proxy toepassingsproblemen](application-proxy-debug-apps.md).  


Zie voor meer informatie over Application Proxy en met behulp van de connectors:

- [Externe toegang tot on-premises toepassingen via de toepassingsproxy](application-proxy.md)
- [Application Proxy connectors](application-proxy-connectors.md)
- [Installeren en registreren van een connector](application-proxy-add-on-premises-application.md)
- [Oplossen van problemen met Application Proxy- en foutberichten](application-proxy-troubleshoot.md)

## <a name="flowchart-for-connector-issues"></a>Stroomdiagram voor connector-problemen

Deze stroomdiagram leidt u door de stappen voor het opsporen van fouten in enkele van de meest voorkomende problemen die connector. Zie voor meer informatie over elke stap in de tabel de stroomdiagram te volgen.

![Stroomdiagram dat toont de stappen voor het opsporen van fouten in een connector](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

|  | Bewerking | Description | 
|---------|---------|---------|
|1 | De connectorgroep toegewezen aan de app zoeken | Hebt u waarschijnlijk een connector is geïnstalleerd op meerdere servers, in welk geval de connectors worden [toegewezen aan connectorgroepen](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups). Zie voor meer informatie over connectorgroepen [publiceren van toepassingen op afzonderlijke netwerken en locaties met connectorgroepen](application-proxy-connector-groups.md). |
|2 | De connector te installeren en een groep worden toegewezen | Als u een connector is geïnstalleerd hebt, raadpleegt u [Installeer en registreer een connector](application-proxy-add-on-premises-application.md#install-and-register-a-connector).<br></br>Als de connector niet is toegewezen aan een groep, Zie [de connector toewijzen aan een groep](application-proxy-connector-groups.md#create-connector-groups).<br></br>Als de toepassing is niet aan een connectorgroep toegewezen, Zie [de toepassing toewijzen aan een connectorgroep](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups).|
|3 | Een poorttest uitvoeren op de server-connector | Voer op de connectorserver, een poorttest met behulp van [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) of andere poort testen hulpprogramma om te controleren of poort 443 en 80 open zijn.|
|4 | De domeinen en -poorten configureren | [Zorg ervoor dat de poorten en domeinen correct zijn geconfigureerd](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) voor de connector goed, er zijn bepaalde poorten die geopend moeten zijn en de URL's die uw server moet toegang hebben tot. |
|5 | Controleer of er een back-end-proxy in gebruik is | Controleer of de connectors zijn met behulp van back-end-proxyservers en overslaan van deze. Zie voor meer informatie, [connector proxy problemen kunt oplossen en problemen met de netwerkverbinding service](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues). |
|6 | Bijwerken van de connector en updater voor gebruik van de back-end-proxy | Als u een back-end-proxy wordt gebruikt, moet u om te controleren of dat de connector gebruikmaakt van dezelfde proxy. Zie voor meer informatie over het oplossen van problemen en configureren van connectors werken met proxyservers [werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md). |
|7 | Laden van de interne URL van de app op de server-connector | Op de connectorserver de interne URL van de app worden geladen. |
|8 | Verbinding met het interne netwerk controleren | Er is een probleem met de netwerkverbinding in uw interne netwerk dat deze foutopsporing stroom is niet kunt vaststellen. De toepassing moet toegankelijk zijn intern voor de connectors om te werken. U kunt inschakelen en weergeven van gebeurtenislogboeken van de connector, zoals beschreven in [Application Proxy connectors](application-proxy-connectors.md#under-the-hood). |
|9 | De time-outwaarde voor de back-end verlengen | In de **extra instellingen** wijzigen voor uw toepassing, de **Toepassingstime-out back-end** instelt op **lang**. Zie [on-premises Apps toevoegen aan Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). |
|10 | Als er problemen blijven optreden, gericht op specifieke flow problemen, app controleren en stromen foutopsporing SSO | Gebruik de [fouten opsporen in Application Proxy toepassingsproblemen](application-proxy-debug-apps.md) stroom oplossen van problemen. |

## <a name="next-steps"></a>Volgende stappen


* [Publiceren van toepassingen op afzonderlijke netwerken en locaties met behulp van connectorgroepen](application-proxy-connector-groups.md)
* [Werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md)
* [Application Proxy en de connector-fouten oplossen](application-proxy-troubleshoot.md)
* [De Azure AD Application Proxy Connector op de achtergrond installeren](application-proxy-register-connector-powershell.md)

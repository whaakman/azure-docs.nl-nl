---
title: Oplossen van een Application Gateway in Azure – ILB as-omgeving | Microsoft Docs
description: Informatie over het oplossen van een toepassingsgateway met behulp van een interne Load Balancer met een App Service Environment in Azure
services: vpn-gateway
documentationCenter: na
author: genlin
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2018
ms.author: genli
ms.openlocfilehash: ad52d2b1df458d04a1ca9bd52a99bab38ddabef1
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308578"
---
# <a name="back-end-server-certificate-is-not-whitelisted-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>Back-end-servercertificaat is niet whitelisted voor een toepassingsgateway met behulp van een interne Load Balancer met een App Service Environment

In dit artikel Hiermee lost u het volgende probleem: Een certificaat is niet in de whitelist opgenomen wanneer u een toepassingsgateway maken met behulp van een interne Load Balancer (ILB) samen met een App Service Environment (ASE) aan de back-end, als u end-to-end SSL gebruikt in Azure.

## <a name="symptoms"></a>Symptomen

Wanneer u een toepassingsgateway met behulp van een ILB met een as-omgeving op de back-end maakt, is het mogelijk dat de back-end-server niet in orde. Dit probleem treedt op als het certificaat voor verificatie van de toepassingsgateway komt niet overeen met het certificaat is geconfigureerd op de back-endserver. Zie het volgende scenario als voorbeeld:

**Application Gateway-configuratie:**

- **Listener:** Meerdere locaties
- **Poort:** 443
- **Hostnaam:** test.appgwtestase.com
- **SSL-certificaat:** CN=test.appgwtestase.com
- **Back-end-groep:** IP-adres of FQDN
- **IP-adres:**: 10.1.5.11
- **HTTP-instellingen:** HTTPS
- **Poort:**: 443
- **Aangepaste test:** Hostname – test.appgwtestase.com
- **Certificaat voor clientverificatie:** .cer van test.appgwtestase.com
- **Back-Endstatus:** Niet in orde – back-end-servercertificaat is niet opgenomen in de whitelist met Application Gateway.

**ASE-configuratie:**

- **ILB IP:** 10.1.5.11
- **Domeinnaam:** appgwtestase.com
- **App Service:** test.appgwtestase.com
- **SSL-Binding:** SNI SSL-CN=test.appgwtestase.com

Wanneer u toegang de toepassingsgateway tot, ontvangt de volgende strekking weergegeven omdat de back-endserver beschadigd is:

**502-webserver heeft een ongeldig antwoord ontvangen terwijl deze fungeerde als gateway of proxy.**

## <a name="solution"></a>Oplossing

Wanneer u een hostnaam niet gebruikt voor toegang tot een HTTPS-website, wordt de back-endserver het certificaat is geconfigureerd op de standaardwebsite geretourneerd in het geval SNI is uitgeschakeld. Voor een ILB as-omgeving, is het standaard-certificaat is afkomstig van het ILB-certificaat. Als er geen certificaten zijn geconfigureerd voor de ILB, wordt het certificaat is afkomstig van het ASE-App-certificaat.

Wanneer u een volledig gekwalificeerde domeinnaam (FQDN) gebruiken voor toegang tot de ILB, retourneert de back-endserver in de HTTP-instellingen het juiste certificaat dat wordt geüpload. Als dat niet het geval is, kunt u de volgende opties:

- FQDN-naam in de back-endpool van de application gateway gebruiken om te verwijzen naar het IP-adres van de ILB. Deze optie werkt alleen als u een privé-DNS-zone hebt of een aangepaste DNS-server geconfigureerd. Anders moet u een 'A'-record maken voor een openbare DNS-server.

- Het geüploade certificaat gebruiken op de ILB of het standaard-certificaat (ILB-certificaat) in de HTTP-instellingen. De application gateway haalt het certificaat wanneer deze van de ILB IP-adres voor de test.

- Gebruik een certificaat met jokertekens op de ILB en de back-endserver, zodat voor alle websites, het certificaat gebruikelijk is. Deze oplossing is echter mogelijk alleen in het geval van subdomeinen en niet als voor elk van de websites die verschillende hostnamen vereist.

- Schakel de **gebruiken voor appservice** optie voor application gateway in het geval u met behulp van het IP-adres van de ILB.

Als u wilt overhead verminderen, kunt u het ILB-certificaat in de HTTP-instellingen waardoor het testpad werken uploaden. (Deze stap is alleen voor opname in de whitelist. Optie wordt niet gebruikt voor SSL-communicatie.) U kunt het ILB-certificaat ophalen door het openen van de ILB met het IP-adres van uw browser op HTTPS en CER-indeling en uploaden van het certificaat op de desbetreffende HTTP-instellingen exporteren van het SSL-certificaat in een met Base-64 gecodeerd.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

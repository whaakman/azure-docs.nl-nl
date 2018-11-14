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
ms.openlocfilehash: 16cfe4c1db8fe9ba4c80f6451611237e3ee12c55
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617879"
---
# <a name="back-end-server-certificate-is-not-whitelisted-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>Back-end-servercertificaat is niet whitelisted voor een toepassingsgateway met behulp van een interne Load Balancer met een App Service Environment

In dit artikel Hiermee lost u het volgende probleem: een certificaat is niet in de whitelist opgenomen wanneer u een toepassingsgateway maken met behulp van een interne Load Balancer (ILB) samen met een App Service Environment (ASE) aan de back-end, als u end-to-end SSL gebruikt in Azure.

## <a name="symptoms"></a>Symptomen

Wanneer u een toepassingsgateway met behulp van een ILB met een as-omgeving op de back-end maakt, is het mogelijk dat de back-end-server niet in orde. Dit probleem treedt op als het certificaat voor verificatie van de toepassingsgateway komt niet overeen met het certificaat is geconfigureerd op de back-endserver. Zie het volgende scenario als voorbeeld:

**Application Gateway-configuratie:**

- **Listener:** multi-site
- **Poort:** 443
- **Hostnaam:** test.appgwtestase.com
- **SSL-certificaat:** CN=test.appgwtestase.com
- **Back-Endpool:** IP-adres of FQDN-naam
- **IP-adres:**: 10.1.5.11
- **HTTP-instellingen:** HTTPS
- **Poort:**: 443
- **Aangepaste test:** hostnaam: test.appgwtestase.com
- **Certificaat voor clientverificatie:** .cer van test.appgwtestase.com
- **Back-Endstatus:** niet in orde – back-end-servercertificaat is niet opgenomen in de whitelist met Application Gateway.

**ASE-configuratie:**

- **ILB-IP:** 10.1.5.11
- **Domeinnaam:** appgwtestase.com
- **App Service:** test.appgwtestase.com
- **SSL-Binding:** SNI SSL-CN=test.appgwtestase.com

Wanneer u toegang de toepassingsgateway tot, ontvangt de volgende strekking weergegeven omdat de back-endserver beschadigd is:

**502-webserver heeft een ongeldig antwoord ontvangen terwijl deze fungeerde als gateway of proxy.**

## <a name="solution"></a>Oplossing

Wanneer u een hostnaam niet gebruikt voor toegang tot een HTTPS-website, retourneert de back-endserver het certificaat is geconfigureerd op de standaardwebsite. Voor een ILB as-omgeving, is het standaard-certificaat is afkomstig van het ILB-certificaat. Als er geen certificaten zijn geconfigureerd voor de ILB, wordt het certificaat is afkomstig van het ASE-App-certificaat.

Wanneer u een volledig gekwalificeerde domeinnaam (FQDN) gebruiken voor toegang tot de ILB, retourneert de back-endserver in de HTTP-instellingen het juiste certificaat dat wordt geüpload. In dit geval, houd rekening met de volgende opties:

- FQDN-naam in de back-endpool van de application gateway gebruiken om te verwijzen naar het IP-adres van de ILB. Deze optie werkt alleen als u een privé-DNS-zone hebt of een aangepaste DNS-server geconfigureerd. Anders moet u een 'A'-record maken voor een openbare DNS-server.

- Het geüploade certificaat gebruiken op de ILB of het standaard-certificaat in de HTTP-instellingen. De application gateway haalt het certificaat wanneer deze van de ILB IP-adres voor de test.

- Gebruik een certificaat met jokertekens op de ILB en de back-endserver.

- Schakel de **gebruiken voor appservice** optie voor de toepassingsgateway.

Als u wilt overhead verminderen, kunt u het ILB-certificaat in de HTTP-instellingen waardoor het testpad werken uploaden. (Deze stap is alleen voor opname in de whitelist. Optie wordt niet gebruikt voor SSL-communicatie.) U kunt het ILB-certificaat ophalen door het openen van de ILB met het IP-adres op HTTPS en exporteren van het SSL-certificaat in een met Base-64 gecodeerd CER-indeling en uploaden van het certificaat op de desbetreffende HTTP-instellingen.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

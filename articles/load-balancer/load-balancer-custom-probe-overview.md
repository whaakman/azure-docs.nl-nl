---
title: Aangepaste tests van Load Balancer gebruiken voor het bewaken van de integriteitsstatus van de | Microsoft Docs
description: Informatie over het gebruik van aangepaste tests voor Azure Load Balancer voor het bewaken van instanties achter de Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: kumud
ms.openlocfilehash: 69991a0b805b5502fc96fab4ce902b3d8bc77baf
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056355"
---
# <a name="understand-load-balancer-probes"></a>Load Balancer-testen

Azure Load Balancer maakt gebruik van de gezondheid van tests om te bepalen welk exemplaar van de groep back-end ontvangt nieuwe stromen.   U kunt statuscontroles gebruiken voor het detecteren van het uitvallen van een toepassing op een back-end-instantie.  U kunt ook het antwoord van de test status van uw toepassing gebruiken om aan te geven aan de Load Balancer of u wilt doorgaan met het verzenden van nieuwe stromen of stoppen met het nieuwe stromen te verzenden naar een exemplaar van de back-end voor het beheren van de belasting of geplande uitvaltijd.

Statuscontroles beheren of nieuwe stromen worden gemaakt met een goede back-end-exemplaren. Wanneer een statustest mislukt, stopt Load Balancer nieuwe stromen te verzenden naar het desbetreffende exemplaar niet in orde.  TCP-verbindingen tot stand gebrachte doorgaan na-fout health-test.  Bestaande UDP-stromen worden verplaatst de uit het niet in orde exemplaar naar een ander exemplaar in de back-endpool.

Als alle tests voor een back-endpool mislukken, wordt Basic Load Balancers alle bestaande TCP-stromen naar de back-endpool beëindigd terwijl de standaardversie van Load balancer wordt tot stand gebrachte TCP stromen om door te gaan; toestaan geen nieuwe stromen worden worden verzonden naar de back-endpool.

Cloudservicerollen (werkrollen en webrollen) gebruiken een gastagent voor bewaking van de test. TCP- of HTTP aangepaste statustests moeten worden geconfigureerd wanneer u Cloud Services met IaaS-VM's achter Load Balancer gebruiken.

## <a name="understand-probe-count-and-timeout"></a>Aantal test-en time-out

Afhankelijk van testgedrag:

* Het aantal geslaagde tests waarmee een exemplaar om te worden met het label voorheen.
* Het aantal mislukte tests die ertoe leiden dat een exemplaar om te worden aangeduid als offline.

De waarden voor time-outs en frequentie in SuccessFailCount te bepalen of een exemplaar is bevestigd dat moet worden uitgevoerd of niet wordt uitgevoerd. In de Azure-portal, is de time-out ingesteld op tweemaal de waarde van de frequentie.

De configuratie van alle exemplaren met gelijke voor een eindpunt (dat wil zeggen, een set met load balancing) van de test moet hetzelfde zijn. Een andere Testconfiguratie voor elke rolinstantie of de virtuele machine in de dezelfde gehoste service voor een bepaald eindpunt combinatie is niet mogelijk. Elk exemplaar moet bijvoorbeeld hebben dezelfde lokale poorten en time-outs.

> [!IMPORTANT]
> Een load balancer-test maakt gebruik van het IP-adres 168.63.129.16. Dit openbare IP-adres vergemakkelijkt de communicatie met interne platform bronnen voor de bring-your-own-IP-Azure Virtual Network scenario. De virtuele openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en wordt niet gewijzigd. Het is raadzaam dat u dit IP-adres in een lokale firewall-beleid toestaan. Deze moet niet worden beschouwd als een veiligheidsrisico inhouden omdat alleen het interne Azure-platform kan een bericht van dat adres van bron. Als u niet toestaan dat dit IP-adres in uw firewall-beleid, wordt onverwacht gedrag optreedt in een verscheidenheid aan scenario's. Het gedrag omvat het configureren van de dezelfde IP-adresbereik van 168.63.129.16 en dupliceren van IP-adressen.

## <a name="learn-about-the-types-of-probes"></a>Meer informatie over de typen tests

### <a name="guest-agent-probe"></a>Test van de Gast-agent

De test voor een gast-agent is alleen beschikbaar voor Azure Cloud Services. Load Balancer maakt gebruik van de gastagent binnen de virtuele machine. Vervolgens wordt geluisterd en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar in de status gereed heeft is. (Overige statussen zijn bezet, Recycling of stoppen.)

Zie voor meer informatie, [het servicedefinitiebestand (csdef) configureren voor statuscontroles](https://msdn.microsoft.com/library/azure/ee758710.aspx) of [aan de slag met het maken van een openbare load balancer voor cloudservices](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Wat is een gast-agent test markeren van een exemplaar als niet in orde?

Als de guest-agent niet reageert met een HTTP 200 OK, markeert de load balancer het exemplaar als reageert niet. Wordt geblokkeerd verkeer verzenden naar dat exemplaar. De load balancer blijft pingt u het exemplaar. Als de guest-agent met een HTTP-200 reageert, stuurt de load balancer verkeer opnieuw naar dat exemplaar.

Wanneer u een Webrol, de websitecode meestal wordt uitgevoerd in w3wp.exe, die niet wordt bewaakt door de Azure-infrastructuur of Gast-agent. Fouten in w3wp.exe (bijvoorbeeld 500 HTTP-antwoorden) worden niet gerapporteerd aan de gastagent. Als gevolg daarvan kan neemt de load balancer dat exemplaar uitgeschakeld.

### <a name="http-custom-probe"></a>Aangepaste HTTP-test

De aangepaste HTTP-test heeft voorrang op de standaard guest agent-test. U kunt uw eigen aangepaste logica om de status van de rolinstantie vast te maken. De load balancer controleert uw eindpunt elke 15 seconden standaard. Het exemplaar wordt beschouwd als in de load balancer-rotatie als met een HTTP-200 binnen de time-outperiode reageert. De time-outperiode is 31 seconden standaard.

Een aangepaste HTTP-test is handig als u wilt om uw eigen logica als u wilt verwijderen van exemplaren van de load balancer-rotatie te implementeren. U wilt bijvoorbeeld een exemplaar verwijderen als deze hoger dan 90% CPU is en een niet-200-status retourneert. Als u web-functies die gebruikmaken van w3wp.exe hebt, krijgt u ook automatische bewaking van uw website. De status van een niet-200 terug fouten in de websitecode van uw naar de load balancer-test.

> [!NOTE]
> De aangepaste HTTP-test ondersteunt relatieve paden en HTTP-protocol. HTTPS wordt niet ondersteund.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Wat is een aangepaste HTTP-test markeren van een exemplaar als niet in orde?

* De HTTP-aanvraag retourneert een HTTP-responscode dan 200 (bijvoorbeeld, 403, 404 of 500). Deze positieve bevestiging waarschuwt u meteen moet van het exemplaar van de toepassing buiten gebruik gesteld.
* De HTTP-server reageert niet op alle na de time-outperiode. Afhankelijk van de time-outwaarde die is ingesteld, meerdere aanvragen van de WebTest onbeantwoorde mogelijk gaan voordat de test wordt gemarkeerd als niet wordt uitgevoerd (dat wil zeggen, voordat u SuccessFailCount tests worden verzonden).
* De verbinding via een TCP-opnieuw instellen van de server wordt gesloten.

### <a name="tcp-custom-probe"></a>Aangepaste TCP-test

TCP-aangepaste tests opnieuw verbinding maken met het uitvoeren van een handshake drie richtingen met de gedefinieerde poort.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Wat is een TCP-test aangepaste markeren van een exemplaar als niet in orde?

* De TCP-server reageert niet op alle na de time-outperiode. Wanneer de test is gemarkeerd als niet wordt uitgevoerd, is afhankelijk van het aantal mislukte test-aanvragen die zijn geconfigureerd om te gaan onbeantwoorde voordat u markeert de test als niet wordt uitgevoerd.
* De test ontvangt een TCP opnieuw instellen van de rolinstantie.

Zie voor meer informatie over het configureren van een HTTP-status-test of een TCP-test [maken van een openbare load balancer in Resource Manager met behulp van PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>In orde exemplaren weer in de load balancer-rotatie toevoegen

TCP- en HTTP-tests worden beschouwd als in orde en markeren van de rolinstantie als in orde wanneer:

* De load balancer wordt een positieve test de eerste keer dat de virtuele machine wordt opgestart.
* Het nummer voor SuccessFailCount (zoals eerder beschreven) definieert de waarde van geslaagde tests die nodig zijn voor het markeren van de rolinstantie als in orde. Als een rolinstantie is verwijderd, kan het aantal geslaagde, opeenvolgende tests moet gelijk zijn aan of groter zijn dan de waarde van SuccessFailCount markeren van de rolinstantie als het uitvoeren.

> [!NOTE]
> Als de status van een rolinstantie fluctueert, wordt de load balancer meer wacht voordat de rolinstantie wordt geplaatst in de status in orde. Deze extra wachttijd beschermt de gebruiker en de infrastructuur en is een opzettelijke beleid.

## <a name="use-log-analytics-for-a-load-balancer"></a>Log analytics gebruiken voor een load balancer

U kunt [melden analytics](load-balancer-monitor-log.md) om te controleren op de status van load balancer-test en count-test. Logboekregistratie kan worden gebruikt met Power BI of Azure Operational Insights voor statistische gegevens over de integriteitsstatus van de load balancer.

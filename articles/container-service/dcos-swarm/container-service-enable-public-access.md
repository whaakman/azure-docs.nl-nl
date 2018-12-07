---
title: (AFGESCHAFT) Toegang tot Azure DC/OS-container-app inschakelen
description: Klik hier voor meer informatie over het inschakelen van openbare toegang tot de DC/OS-containers in Azure Container Service.
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3e4ba15fa1925ca40ad7760acbd14331fbdd1343
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52996597"
---
# <a name="deprecated-enable-public-access-to-an-azure-container-service-application"></a>(AFGESCHAFT) Openbare toegang tot een Azure Container Service-toepassing inschakelen

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Een DC/OS-container in de ACS [openbare agentpool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatisch wordt blootgesteld aan internet. Standaard-poorten **80**, **443**, **8080** worden geopend, en elke (openbare) container luistert op deze poorten toegankelijk zijn. Dit artikel leest u hoe meer om poorten te openen voor uw toepassingen in Azure Container Service.

## <a name="open-a-port-portal"></a>Open een poort (portal)
Eerst moet de gewenste poort te openen.

1. Meld u aan bij de portal.
2. Zoek de resourcegroep die u hebt de Azure Container Service geïmplementeerd.
3. Selecteer de agent load balancer (die de naam die vergelijkbaar is met **XXXX-agent-lb-XXXX**).
   
    ![Azure container service load balancer](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Klik op **tests** en vervolgens **toevoegen**.
   
    ![Azure container service load balancer-testen](./media/container-service-enable-public-access/add-probe.png)
5. Vul het formulier test in en klikt u op **OK**.
   
   | Veld | Description |
   | --- | --- |
   | Name |Een beschrijvende naam van de test. |
   | Poort |De poort van de container om te testen. |
   | Pad |(Als in de HTTP-modus) Het pad relatief website te controleren. HTTPS wordt niet ondersteund. |
   | Interval |De hoeveelheid tijd tussen test probeert, in seconden. |
   | Drempelwaarde voor onjuiste status |Dit is het aantal opeenvolgende pogingen voordat de container niet in orde. |
6. Terug in de eigenschappen van de agent load balancer, klikt u op **Taakverdelingsregels** en vervolgens **toevoegen**.
   
    ![Azure container service load balancer-regels](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Vul het formulier in de load balancer en klikt u op **OK**.
   
   | Veld | Description |
   | --- | --- |
   | Name |Een beschrijvende naam van de load balancer. |
   | Poort |De openbare binnenkomende poort. |
   | Back-endpoort |De interne-openbare poort van de container voor het routeren van verkeer. |
   | Back-endpool |De containers in deze groep zijn het doel voor deze load balancer. |
   | Test |De test gebruikt om te bepalen of een doel in de **back-endpool** in orde is. |
   | Sessiepersistentie |Bepaalt hoe verkeer vanaf een client moet worden verwerkt voor de duur van de sessie.<br><br>**Geen**: opeenvolgende aanvragen van dezelfde client kunnen worden verwerkt door elke container.<br>**Client IP**: opeenvolgende aanvragen van hetzelfde client-IP worden verwerkt door dezelfde container.<br>**Client IP en protocol**: opeenvolgende aanvragen van de dezelfde client-IP-adres en protocol combinatie worden verwerkt door dezelfde container. |
   | Time-out voor inactiviteit |(Alleen TCP) Binnen enkele minuten de tijd voor bewaren van een TCP/HTTP-client openen zonder afhankelijk te *keepalive* berichten. |

## <a name="add-a-security-rule-portal"></a>Toevoegen van een beveiligingsregel (portal)
Vervolgens moeten we een beveiligingsregel routeren van verkeer van onze poort geopend via de firewall toevoegen.

1. Meld u aan bij de portal.
2. Zoek de resourcegroep die u hebt de Azure Container Service geïmplementeerd.
3. Selecteer de **openbare** netwerkbeveiligingsgroep agent (die de naam die vergelijkbaar is met **XXXX-agent-openbare-nsg-XXXX**).
   
    ![Azure container service-netwerkbeveiligingsgroep](./media/container-service-enable-public-access/agent-nsg.png)
4. Selecteer **inkomende beveiligingsregels** en vervolgens **toevoegen**.
   
    ![Azure container service regels voor netwerkbeveiligingsgroepen](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Vul de firewallregel wilt toestaan dat uw openbare poort en klik op **OK**.
   
   | Veld | Description |
   | --- | --- |
   | Name |Een beschrijvende naam van de firewallregel. |
   | Prioriteit |De positie van de prioriteit voor de regel. Des te lager het nummer hoe hoger de prioriteit. |
   | Bron |Beperk het inkomende IP-adresbereik worden toegestaan of geweigerd door deze regel. Gebruik **eventuele** niet opgeven een beperking. |
   | Service |Selecteer een set vooraf gedefinieerde services die deze beveiligingsregel is. Gebruik anders **aangepaste** te maken van uw eigen. |
   | Protocol |Beperken van verkeer op basis van **TCP** of **UDP**. Gebruik **eventuele** niet opgeven een beperking. |
   | Poortbereik |Wanneer **Service** is **aangepaste**, Hiermee geeft u het bereik van poorten waarop deze regel betrekking heeft. U kunt één poort zijn, zoals **80**, of een bereik, zoals **1024-1500**. |
   | Bewerking |Verkeer toestaan of weigeren die voldoet aan de criteria. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het verschil tussen [openbare en persoonlijke DC/OS-agents](container-service-dcos-agents.md).

Lees meer informatie over [beheer van uw DC/OS-containers](container-service-mesos-marathon-ui.md).


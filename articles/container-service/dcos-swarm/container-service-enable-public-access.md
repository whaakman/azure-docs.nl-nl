---
title: Toegang tot Azure DC/OS container app inschakelen
description: Het inschakelen van openbare toegang tot DC/OS-containers in Azure Container Service.
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: aedc97335a0b9ad00cf653477b62bf530b556900
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="enable-public-access-to-an-azure-container-service-application"></a>Openbare toegang tot een Azure Container Service-toepassing

Elke DC/OS-container in de ACS [openbare agent groep](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatisch wordt blootgesteld aan internet. Standaard-poorten **80**, **443**, **8080** zijn geopend en luistert op deze poorten (openbare) containers toegankelijk zijn. In dit artikel leest u hoe meer poorten voor uw toepassingen in Azure Container Service openen.

## <a name="open-a-port-portal"></a>Open een poort (portal)
Eerst moet de poort die we willen openen.

1. Aanmelden bij de portal.
2. De resourcegroep die u hebt geïmplementeerd met de Azure Container Service om te zoeken.
3. Selecteer de agent load balancer (die de naam vergelijkbaar met **XXXX-agent-lb-XXXX**).
   
    ![Azure container service load balancer](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Klik op **Probes** en vervolgens **toevoegen**.
   
    ![Azure container service load balancer-tests](./media/container-service-enable-public-access/add-probe.png)
5. Vul het formulier test en klik op **OK**.
   
   | Veld | Beschrijving |
   | --- | --- |
   | Naam |Een beschrijvende naam van de test. |
   | Poort |De poort van de container om te testen. |
   | Pad |(Wanneer in de HTTP-modus) Het pad van de relatieve website naar test. HTTPS wordt niet ondersteund. |
   | Interval |De hoeveelheid tijd tussen test probeert, in seconden. |
   | Drempelwaarde voor onjuiste status |Dit is het aantal opeenvolgende test pogingen voordat de container niet in orde. |
6. Klik op de eigenschappen van de agent load balancer op **Taakverdelingsregels** en vervolgens **toevoegen**.
   
    ![Azure container service load balancer-regels](./media/container-service-enable-public-access/add-balancer-rule.png)
7. De load balancer-formulier invullen en klik op **OK**.
   
   | Veld | Beschrijving |
   | --- | --- |
   | Naam |Een beschrijvende naam van de load balancer. |
   | Poort |De openbare binnenkomende poort. |
   | Back-endpoort |De interne openbare poort van de container voor het routeren van verkeer. |
   | Back-endpool |De containers in deze groep zijn het doel voor deze load balancer. |
   | Test |De test die wordt gebruikt om te bepalen of een doel in de **back-endpool** in orde is. |
   | Sessiepersistentie |Hiermee wordt bepaald hoe het verkeer van een client moet worden verwerkt voor de duur van de sessie.<br><br>**Geen**: opeenvolgende aanvragen van dezelfde client door elke container kunnen worden verwerkt.<br>**Client IP**: opeenvolgende aanvragen van hetzelfde client-IP worden verwerkt door de container.<br>**Client IP en -protocol**: opeenvolgende aanvragen van dezelfde client IP-adres en protocol combinatie worden verwerkt door de container. |
   | Time-out voor inactiviteit |(Alleen TCP) In minuten, de tijd voor bewaren van een TCP/HTTP-client openen zonder afhankelijk *keepalive-* berichten. |

## <a name="add-a-security-rule-portal"></a>Toevoegen van een beveiligingsregel (portal)
Vervolgens moet een regel waarmee verkeer van onze poort is geopend via de firewall toevoegen.

1. Aanmelden bij de portal.
2. De resourcegroep die u hebt geïmplementeerd met de Azure Container Service om te zoeken.
3. Selecteer de **openbare** netwerkbeveiligingsgroep agent (die de naam vergelijkbaar met **XXXX-agent-openbare-nsg-XXXX**).
   
    ![Azure container service-netwerkbeveiligingsgroep](./media/container-service-enable-public-access/agent-nsg.png)
4. Selecteer **inkomende beveiligingsregels** en vervolgens **toevoegen**.
   
    ![Azure container service netwerkbeveiligingsgroepen](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Vul de firewallregel wilt toestaan dat de openbare poort en klik op **OK**.
   
   | Veld | Beschrijving |
   | --- | --- |
   | Naam |Een beschrijvende naam van de firewallregel. |
   | Prioriteit |De positie van de prioriteit voor de regel. Hoe lager het getal hoe hoger de prioriteit. |
   | Bron |Beperk het binnenkomende IP-adresbereik moet worden toegestaan of geweigerd door deze regel. Gebruik **eventuele** beperking niet opgeven. |
   | Service |Selecteer een set vooraf gedefinieerde services is bedoeld voor deze regel. Gebruik anders **aangepaste** voor het maken van uw eigen. |
   | Protocol |Op basis van verkeer te beperken **TCP** of **UDP**. Gebruik **eventuele** beperking niet opgeven. |
   | Poortbereik |Wanneer **Service** is **aangepaste**, bepaalt het bereik van de poorten die door deze regel is van invloed op. U kunt een losse poort zijn, zoals **80**, of een bereik, zoals **1024 1500**. |
   | Actie |Verkeer toestaan of weigeren die voldoet aan de criteria. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het verschil tussen [openbare en persoonlijke DC/OS-agents](container-service-dcos-agents.md).

Meer informatie lezen over [beheer van uw DC/OS-containers](container-service-mesos-marathon-ui.md).


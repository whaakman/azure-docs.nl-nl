---
title: Azure Cloud Services-naam Def. LoadBalancerProbe Schema | Microsoft Docs
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 113374a8-8072-4994-9d99-de391a91e6ea
caps.latest.revision: "14"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 31c974c5a4b9dc9cff882ff42b73ee023fc4ad9b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Azure Cloud Services-definitie LoadBalancerProbe Schema
De load balancer-test is een klant gedefinieerde health test van UDP-eindpunten en eindpunten in rolinstanties. De `LoadBalancerProbe` is geen zelfstandige element; deze wordt gecombineerd met de Webrol of functie in een servicedefinitiebestand worker. Een `LoadBalancerProbe` kunnen worden gebruikt door meer dan één rol.

De standaardextensie voor het servicedefinitiebestand is csdef.

## <a name="the-function-of-a-load-balancer-probe"></a>De functie van een load balancer-test
De Azure Load Balancer is verantwoordelijk voor het routeren van binnenkomende verkeer naar uw rolinstanties. De load balancer bepaalt welke exemplaren verkeer door elk exemplaar regelmatig scannen om te bepalen van de status van dit exemplaar kunnen ontvangen. De load balancer-tests elke instantie meerdere keren per minuut. Er zijn twee verschillende opties voor het ontwikkelen van de status van het exemplaar naar de load balancer – de standaard load balancer-test of een aangepaste load balancer-test die wordt geïmplementeerd door de LoadBalancerProbe definiëren in het csdef-bestand.

De standaard load balancer-test maakt gebruik van de Guest-Agent op de virtuele machine, waardoor wordt geluisterd en reageert met een HTTP 200 OK antwoord wanneer het exemplaar in de status gereed (zoals wanneer het exemplaar is niet in het gesprek, recyclen, stoppen, statussen enz.). Als de Guest Agent niet reageert met HTTP 200 OK, wordt de Azure Load Balancer markeert de instantie als niet-reagerende en stopt verkeer kunnen verzenden naar dit exemplaar. De Azure Load Balancer, blijft het pingen van het exemplaar en als de Guest-Agent met een HTTP 200 reageert, de Azure Load Balancer verzendt verkeer naar dat exemplaar opnieuw. Wanneer u een Webrol uw websitecode gewoonlijk wordt uitgevoerd in w3wp.exe die niet wordt bewaakt door de Azure-infrastructuur of guest-agent, wat betekent dat fouten in w3wp.exe (bv. 500 HTTP-antwoorden) is niet gerapporteerd de guest-agent en de load balancer niet weet te nemen dat exemplaar buiten de draaihoek.

De aangepaste load balancer-test heeft voorrang op de standaard guest agent test en kunt u uw eigen aangepaste regels om te bepalen van de status van de rolinstantie maken. De load balancer regelmatig uw eindpunt (elke 15 seconden, standaard) en het exemplaar is tests worden overwogen in rotatie als deze met een TCP ACK of een HTTP 200 binnen de time-outperiode (standaard 31 seconden reageert). Dit kan handig zijn voor het implementeren van uw eigen logica voor het verwijderen van instanties van de load balancer draaiing, bijvoorbeeld de status van een niet-200 wordt geretourneerd als het exemplaar hoger dan 90% CPU is zijn. Voor webrollen met w3wp.exe, betekent dit ook dat u een automatische bewaking van uw website, omdat er fouten in uw websitecode van de status van een niet-200 terug naar de load balancer-test. Als u een LoadBalancerProbe niet in het csdef-bestand definieert, is het load balancer (zoals eerder beschreven standaardgedrag) worden gebruikt.

Als u een aangepaste load balancer-test gebruikt, moet u ervoor zorgen dat uw logica rekening gehouden met de methode RoleEnvironment.OnStop. Wanneer u de standaard load balancer-test, het exemplaar wordt gemaakt buiten rotatie voordat OnStop wordt aangeroepen, maar een aangepaste load balancer-test kunt doorgaan met het retourneren van een 200 OK tijdens de gebeurtenis OnStop. Als u de gebeurtenis OnStop gebruikt voor het opschonen van de cache, stopt u de service of anders u wijzigingen aanbrengt die het runtimegedrag van uw service kunnen beïnvloeden, moet u ervoor te zorgen dat uw aangepaste load balancer-test logica het exemplaar van de draaihoek verwijdert.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Basic-service definitie schema voor een load balancer-test
 De basisindeling van een service-definitie-bestand met een load balancer-test is als volgt.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-elementen
De `LoadBalancerProbes` element van het servicedefinitiebestand bevat de volgende elementen:

- [LoadBalancerProbes Element](#LoadBalancerProbes)
- [LoadBalancerProbe Element](#LoadBalancerProbe)

##  <a name="LoadBalancerProbes"></a>LoadBalancerProbes Element
De `LoadBalancerProbes` -element de verzameling van load balancer-tests worden beschreven. Dit element is het bovenliggende element van de [LoadBalancerProbe Element](#LoadBalancerProbe). 

##  <a name="LoadBalancerProbe"></a>LoadBalancerProbe Element
De `LoadBalancerProbe` element wordt gedefinieerd voor de health-test voor een model. U kunt meerdere load balancer-tests kunt definiëren. 

De volgende tabel beschrijft de kenmerken van de `LoadBalancerProbe` element:

|Kenmerk|Type|Beschrijving|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Vereist. De naam van de load balancer-test. De naam moet uniek zijn.|
| `protocol`          | `string` | Vereist. Hiermee geeft u het protocol van het eindpunt. Mogelijke waarden zijn `http` en `tcp`. Als `tcp` is opgegeven, wordt een ontvangen ACK is vereist voor de test kan alleen slagen. Als `http` is opgegeven, wordt een 200 OK reactie van de opgegeven URI is vereist voor de test kan alleen slagen.|
| `path`              | `string` | De URI die wordt gebruikt voor het aanvragen van de status van de virtuele machine. `path`is vereist als `protocol` is ingesteld op `http`. Anders wordt is het niet toegestaan.<br /><br /> Er is geen standaardwaarde.|
| `port`              | `integer` | Optioneel. De poort voor communicatie van de test. Dit is optioneel voor een willekeurig eindpunt als dezelfde poort vervolgens voor de test gebruikt wordt. U kunt een andere poort voor hun gezocht, evenals configureren. Mogelijke waarden tussen 1 en 65535 op.<br /><br /> De standaardwaarde is ingesteld door het eindpunt.|
| `intervalInSeconds` | `integer` | Optioneel. Het interval in seconden, voor hoe vaak het eindpunt voor de status van de health-test. Het interval is meestal iets meer dan een halve de toegewezen time-outperiode (in seconden) waarmee twee volledige tests voordat u het exemplaar buiten de draaihoek.<br /><br /> De standaardwaarde is 15, de minimumwaarde is 5.|
| `timeoutInSeconds`  | `integer` | Optioneel. De time-outperiode in seconden, toegepast op de test waar geen antwoord resulteert in meer verkeer wordt geleverd met het eindpunt wordt gestopt. Deze waarde kan eindpunten moeten worden uitgevoerd buiten de draaihoek sneller of langzamer dan de typische tijden worden gebruikt in Azure (die de standaardinstellingen).<br /><br /> De standaardwaarde is 31, de minimale waarde 11 is.|

## <a name="see-also"></a>Zie ook
[Cloud-Service (klassiek) definitie Schema](schema-csdef-file.md)
---
title: Azure Cloud Services Def. LoadBalancerProbe Schema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 113374a8-8072-4994-9d99-de391a91e6ea
caps.latest.revision: 14
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: f7b0ba3b4797149798037dee0188850eff6baf1d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003285"
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Azure Cloud Services-definitie LoadBalancerProbe Schema
De load balancer-test is een klant gedefinieerde statustest van UDP-eindpunten en -eindpunten in rolinstanties. De `LoadBalancerProbe` is geen zelfstandige element; deze wordt gecombineerd met de Webrol of werkrol in een service-definitiebestand. Een `LoadBalancerProbe` kan worden gebruikt door meer dan één rol.

De standaardextensie voor het servicedefinitiebestand is .csdef.

## <a name="the-function-of-a-load-balancer-probe"></a>De functie van een load balancer-test
De Azure Load Balancer is verantwoordelijk voor de routering van inkomend verkeer naar uw rolinstanties. De load balancer bepaalt welke instanties kunnen verkeer ontvangen door elk exemplaar regelmatig scannen om te bepalen van de status van deze instantie. De load balancer controleert elk exemplaar meerdere keren per minuut. Er zijn twee verschillende opties voor het leveren van exemplaar van de status naar de load balancer: de standaard load balancer-test of een aangepaste load balancer-test, die wordt geïmplementeerd door de LoadBalancerProbe definiëren in het csdef-bestand.

De standaard load balancer-test maakt gebruik van de Guest-Agent op de virtuele machine, die luistert en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar is in de status gereed heeft (zoals wanneer het exemplaar is niet in de bezet, Bezig met recyclen, stoppen, Staten enz.). Als de Guest-Agent niet reageert met een HTTP 200 OK, wordt de Azure Load Balancer markeert het exemplaar als niet-reagerende en verkeer verzenden naar dat exemplaar stopt. De Azure Load Balancer blijft pingt u het exemplaar en als de Guest-Agent met een HTTP-200 reageert, de Azure Load Balancer stuurt verkeer naar dat exemplaar opnieuw. Bij het gebruik van een Webrol die de websitecode van uw wordt gewoonlijk uitgevoerd in w3wp.exe die niet wordt bewaakt door de Azure-infrastructuur of gastagent, waardoor fouten in w3wp.exe (bv. 500 HTTP-antwoorden) wordt niet gerapporteerd aan de gastagent en de load balancer niet weet te nemen dat exemplaar uitgeschakeld.

De aangepaste load balancer-test overschrijft de standaard guest agent-test en kunt u uw eigen aangepaste logica om de status van de rolinstantie vast te maken. De load balancer regelmatig tests uw eindpunt (elke 15 seconden, standaard) en het exemplaar wordt beschouwd in rotatie als met een ACK TCP of HTTP-200 binnen de time-outperiode (standaard 31 seconden reageert). Dit kan nuttig zijn om uw eigen logica als u wilt verwijderen van exemplaren van de load balancer-rotatie, bijvoorbeeld de status van een niet-200 retourneren als het exemplaar hoger dan 90% CPU is te implementeren zijn. Voor webrollen met behulp van w3wp.exe, betekent dit ook dat u automatische bewaking van uw website, omdat er fouten in de websitecode van uw een niet-200-status retourneren aan de load balancer-test. Als u niet een LoadBalancerProbe in het csdef-bestand definieert, wordt de load balancer standaard (zoals eerder beschreven) worden gebruikt.

Als u een aangepaste load balancer-test, moet u ervoor zorgen dat uw logica rekening gehouden met de methode RoleEnvironment.OnStop. Wanneer u de standaard load balancer test, het exemplaar uit rotatie voorafgaand aan die wordt aangeroepen OnStop is gehaald, maar een aangepaste load balancer-test om terug te keren een 200 OK tijdens de OnStop-gebeurtenis kan doorgaan. Als u de gebeurtenis OnStop gebruikt voor het opschonen van de cache, service of anders u wijzigingen aanbrengt die invloed kunnen zijn op het runtimegedrag van uw service te stoppen, moet u ervoor te zorgen dat uw aangepaste load balancer-test logica verwijdert van het exemplaar uit rotatie.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Definitieschema voor Basic-service voor een load balancer-test
 De eenvoudige indeling van een service-definitie-bestand met een load balancer-test is als volgt.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-elementen
De `LoadBalancerProbes` element van het servicedefinitiebestand bevat de volgende elementen:

- [LoadBalancerProbes-Element](#LoadBalancerProbes)
- [LoadBalancerProbe-Element](#LoadBalancerProbe)

##  <a name="LoadBalancerProbes"></a> LoadBalancerProbes-Element
De `LoadBalancerProbes` -element worden beschreven voor het verzamelen van load balancer-testen. Dit element is het bovenliggende element van de [LoadBalancerProbe Element](#LoadBalancerProbe). 

##  <a name="LoadBalancerProbe"></a> LoadBalancerProbe-Element
De `LoadBalancerProbe` element wordt gedefinieerd met de statustest voor een model. U kunt meerdere load balancer-testen definiëren. 

De volgende tabel beschrijft de kenmerken van de `LoadBalancerProbe` element:

|Kenmerk|Type|Beschrijving|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Vereist. De naam van de load balancer-test. De naam moet uniek zijn.|
| `protocol`          | `string` | Vereist. Hiermee geeft u het protocol van het eindpunt. Mogelijke waarden zijn `http` en `tcp`. Als `tcp` is opgegeven, wordt een ontvangen ACK is vereist voor de test om succesvol te zijn. Als `http` is opgegeven, wordt een 200 OK reactie van de opgegeven URI is vereist voor de test om succesvol te zijn.|
| `path`              | `string` | De URI die wordt gebruikt voor het aanvragen van health-status van de virtuele machine. `path` is vereist als `protocol` is ingesteld op `http`. Anders wordt is het niet toegestaan.<br /><br /> Er is geen standaardwaarde.|
| `port`              | `integer` | Optioneel. De poort voor communicatie van de test. Dit is optioneel voor een willekeurig eindpunt omdat dezelfde poort vervolgens voor de test gebruikt wordt. U kunt een andere poort voor hun probing ook configureren. Mogelijke waarden tussen 1 en 65535 op.<br /><br /> De standaardwaarde is ingesteld door het eindpunt.|
| `intervalInSeconds` | `integer` | Optioneel. Het interval in seconden, hoe vaak u wilt het eindpunt voor de integriteitsstatus van de test. Het interval is meestal iets meer dan een halve de toegewezen time-outperiode (in seconden) waarmee twee volledige testen voordat u het exemplaar uitgeschakeld.<br /><br /> De standaardwaarde is 15, de minimumwaarde is 5.|
| `timeoutInSeconds`  | `integer` | Optioneel. De time-outperiode, in seconden, toegepast op de test waar geen antwoord resulteert in meer verkeer naar het eindpunt bezorging stoppen. Met deze waarde kan eindpunten moeten worden uitgevoerd buiten de rotatie sneller of langzamer dan de normale keren worden gebruikt in Azure (dit zijn de standaardinstellingen).<br /><br /> De standaardwaarde is 31, de minimale waarde 11.|

## <a name="see-also"></a>Zie ook
[(Klassiek) Definitieschema voor cloud Service](schema-csdef-file.md)
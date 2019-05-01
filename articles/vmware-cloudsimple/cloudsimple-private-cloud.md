---
title: Persoonlijke clouds in VMware-oplossing door CloudSimple - Azure
description: Meer informatie over de privéclouds CloudSimple en concepten.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dc07b4eea553e6cb3d9b522826e860ddbfbc1513
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577043"
---
# <a name="cloudsimple-private-cloud-overview"></a>Overzicht van de privécloud CloudSimple

CloudSimple transformeert en breidt de VMware-workloads met openbare clouds in minuten. De service CloudSimple gebruikt, kunt u implementeren VMware systeemeigen op Azure bare metal-infrastructuur. Uw implementatie wordt toegevoegd aan Azure-locaties en volledig kan worden geïntegreerd met de rest van de Azure-cloud.

* De oplossing CloudSimple biedt volledige VMware operationele continuïteit. Deze oplossing biedt u de voordelen van de openbare cloud van:
  * Elasticiteit
  * Innovatie
  * Efficiëntie
* Met CloudSimple profiteert u van een model voor het gebruik van cloud dat uw totale eigendomskosten verlaagt. Het biedt ook op aanvraag inrichten, betaal-als-u-groeit en capaciteitsoptimalisatie van.
* CloudSimple is volledig compatibel met:
  * Bestaande hulpprogramma 's
  * Vaardigheden
  * Processen
* Deze compatibiliteit kan uw teams voor het beheren van werkbelastingen op de Azure-cloud, zonder te onderbreken van uw beleid:
  * Netwerk
  * Beveiliging  
  * Gegevensbeveiliging  
  * Controleren
* CloudSimple beheert de infrastructuur en alle vereiste netwerk- en beheer van services. De CloudSimple-service kan uw team zich richten op:
  * Waarde voor het bedrijf
  * Toepassingsinrichting
  * Bedrijfscontinuïteit
  * Ondersteuning
  * Beleidsafdwinging

## <a name="private-cloud-environment-overview"></a>Overzicht van de privécloud-omgeving

Een privécloud is een geïsoleerde VMware-stack, zoals deze omgevingen:

* ESXi-hosts
* vCenter
* vSAN
* NSX

Persoonlijke clouds worden beheerd door een vCenter-server in een eigen beheerdomein.

De stack wordt uitgevoerd op:

* Toegewezen knooppunten
* Geïsoleerde voor bare metal-hardwareknooppunten

Gebruikers gebruiken de stack met systeemeigen VMware-hulpprogramma's, waaronder:

* vCenter
* NSX Manager

U kunt specifieke knooppunten in de Azure-locaties kunt implementeren. Vervolgens kunt u ze beheren met Azure en CloudSimple. Een privécloud bestaat uit een of meer vSphere-hostclusters en elk cluster 3 tot en met 16 knooppunten bevat.

U kunt een privécloud met knooppunten die zijn aangeschaft:

* Betalen per gebruik-knooppunten
* Gereserveerde, toegewezen knooppunten

U kunt de privécloud verbinden met uw on-premises omgeving en het Azure-netwerk met behulp van de volgende verbindingen:

* Beveiligen
* Persoonlijke VPN
* ExpressRoute van Azure

De privécloud-omgeving is ontworpen om te voorkomen dat een single point of failure:

* ESXi-clusters zijn geconfigureerd met hoge beschikbaarheid van vSphere en grootte hebben van ten minste één extra knooppunt voor tolerantie.
* virtueel SAN biedt redundante primaire opslag. virtueel SAN moet ten minste drie knooppunten voor beveiliging tegen een storing. U kunt vSAN om een hogere tolerantie voor grotere clusters configureren.
* U kunt vCenter PSC-server en virtuele machines NSX Manager configureren met RAID 10-beleid voor opslag om te beveiligen tegen fout bij de opslag. Ze zijn vervolgens beveiligd door vSphere HA tegen storingen in knooppunt en het netwerk.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Scenario's voor het implementeren van een privécloud

* **Data center buiten gebruik stellen of de migratie**

  * Zorg voor extra capaciteit wanneer u de grenzen van uw bestaande datacenter bereiken of vernieuwen van hardware.
  * Benodigde capaciteit in de cloud toevoegen en verwijderen van de problemen die van het beheer van hardware wordt vernieuwd.
  * Verminder de risico's en de kosten van de cloud kunt migreren, in vergelijking met tijdrovende conversies of rearchitecture.
  * Gebruik vertrouwde VMware-hulpprogramma's en vaardigheden om te versnellen cloudmigraties. Gebruik in de cloud, Azure-services te moderniseren van uw toepassingen in uw eigen tempo.

* **Vouw op aanvraag**

  * Vouw in de cloud om te voldoen aan onvoorziene behoeften, zoals nieuwe ontwikkelomgevingen of seizoensgebonden capaciteit pieken.
  * Nieuwe capaciteit op aanvraag maken en bewaar alleen als u deze nodig hebt.
  * Uw investeringen vooraf verkorten, Versnel de snelheid van de inrichting en verminder complexiteit met de dezelfde architectuur en het beleid in zowel on-premises en de cloud.

* **Herstel na noodgevallen en virtuele bureaubladen in de Azure-cloud**

  * Externe toegang tot gegevens, apps en pc's in de Azure-cloud maken. Met hoge bandbreedte verbindingen, u uploaden / downloaden van gegevens snel te herstellen van incidenten. Netwerken met lage latentie geven u snel antwoord keer dat gebruikers van een bureaublad-app verwachten.

  * Alle beleidsregels en netwerken in de cloud met behulp van de portal CloudSimple en vertrouwde hulpprogramma's van VMware repliceren. Deze replicatie vermindert de inspanning en het risico van het maken en beheren van herstel na Noodgevallen en VDI-implementaties.

* **Krachtige toepassingen en -databases**

  * Uw meest veeleisende workloads uitvoeren met de architectuur van het hypergeconvergeerd geleverd door CloudSimple.
  * Oracle, Microsoft SQL server, middleware-systemen en krachtige geen SQL-databases worden uitgevoerd.

  * Ervaar de cloud als uw eigen datacenter met hoge snelheid 25 Gbps netwerkverbindingen. Snelle verbindingen kunnen u hybride apps met betrekking on-premises VMware op Azure tot, uitvoeren en Azure privé-workloads, zonder verlies van prestaties.

* **Echte hybride**

  * Lever een geïntegreerde ervaring DevOps in VMware en Azure-services.
  * VMware-beheer voor Azure-services en oplossingen die kunnen worden toegepast voor alle uw workloads optimaliseren.
  * Toegang tot services van de openbare cloud zonder dat uw datacenter uitbreiden of opnieuw ontwerpen van uw toepassingen.
  * Centraliseer identiteiten, beleid voor toegangsbeheer, logboekregistratie en bewaking van VMware-toepassingen op Azure.

## <a name="limits"></a>Limits

De onderstaande tabel bevat de limieten van het knooppunt op resources van een privécloud.

| Resource | Limiet |
|----------|-------|
| Minimum aantal knooppunten aan een privécloud maken | 3 |
| Maximum aantal knooppunten in een cluster in een privécloud | 16 |
| Maximum aantal knooppunten in een privécloud | 64 |
| Minimum aantal knooppunten in een nieuw cluster | 3 |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een privécloud maken](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Meer informatie over het [een privécloud-omgeving configureren](quickstart-create-private-cloud.md)
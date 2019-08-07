---
title: Persoonlijke Clouds in VMware-oplossing door CloudSimple-Azure
description: Meer informatie over persoonlijke Clouds en concepten van CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9885366d5987870fe2739083ff47abaae9ef6ed1
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816204"
---
# <a name="cloudsimple-private-cloud-overview"></a>Overzicht van CloudSimple Private Cloud

Met CloudSimple worden VMware-workloads binnen enkele minuten getransformeerd en uitgebreid naar open bare Clouds. Met de CloudSimple-service kunt u VMware systeem eigen implementeren op een Azure bare-metal infra structuur. Uw implementatie bevindt zich op Azure-locaties en integreert volledig met de rest van de Azure-Cloud.

* De CloudSimple-oplossing biedt volledige VMware-operationele continuïteit. Deze oplossing biedt u de voor delen van de open bare cloud van:
  * Elasticiteit
  * Innovatie
  * Efficiëntie
* Met CloudSimple profiteert u van een model voor Cloud verbruik dat uw total cost of ownership verlaagt. Het biedt ook mogelijkheden voor inrichting op aanvraag, betalen per gebruik en capaciteits optimalisatie.
* CloudSimple is volledig compatibel met:
  * Bestaande hulpprogram ma's
  * Vaardigheden
  * Processen
* Met deze compatibiliteit kunnen uw teams werk belastingen op de Azure-Cloud beheren zonder uw beleid te onderbreken:
  * Netwerk
  * Beveiliging  
  * Gegevensbescherming  
  * Controleren
* CloudSimple beheert de infra structuur en alle benodigde netwerk-en beheer Services. Met de CloudSimple-service kan uw team zich richten op het volgende:
  * Bedrijfs waarde
  * Toepassing inrichten
  * Bedrijfscontinuïteit
  * Ondersteuning
  * Beleidsafdwinging

## <a name="private-cloud-environment-overview"></a>Overzicht van de privécloud-omgeving

Een privécloud is een geïsoleerde VMware-stack, zoals deze omgevingen:

* ESXi-hosts
* vCenter
* vSAN
* NSX

Persoonlijke Clouds worden beheerd door een vCenter-Server in een eigen beheer domein.

De stack wordt uitgevoerd op:

* Toegewezen knooppunten
* Geïsoleerde bare metal-hardwareprofielen

Gebruikers gebruiken de stack via systeem eigen VMware-hulpprogram ma's, waaronder:

* vCenter
* NSX Manager

U kunt toegewezen knoop punten implementeren op Azure-locaties. Vervolgens kunt u ze beheren met Azure en CloudSimple. Een privécloud bestaat uit een of meer vSphere-clusters en elk cluster bevat 3 tot 16 knoop punten.

U kunt een privécloud maken met behulp van ingerichte knoop punten:

* Betalen per gebruik-knoop punten
* Gereserveerde, toegewezen knoop punten

U kunt de privécloud koppelen aan uw on-premises omgeving en het Azure-netwerk met behulp van de volgende verbindingen:

* Beveiligen
* Particuliere VPN
* Azure ExpressRoute

De privécloud is ontworpen om een Single Point of Failure te elimineren:

* ESXi-clusters zijn geconfigureerd met een hoge Beschik baarheid van vSphere en hebben een grootte van ten minste één reserve knooppunt voor tolerantie.
* vSAN biedt redundante primaire opslag. vSan vereist ten minste drie knoop punten om bescherming te bieden tegen één fout. U kunt vSAN configureren om hogere toleranties voor grotere clusters te bieden.
* U kunt vCenter-, PSC-en NSX-beheer machines configureren met RAID-10-opslag beleid om te beschermen tegen opslag storingen. Ze worden vervolgens beschermd door vSphere HA tegen knoop punten en netwerk fouten.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Scenario's voor het implementeren van een privécloud

* **Buiten gebruik stellen of migreren van data centers**

  * Krijg extra capaciteit wanneer u de limieten bereikt van uw bestaande Data Center of hardware vernieuwt.
  * U kunt de benodigde capaciteit toevoegen aan de Cloud en het beheer van hardware-vernieuwingen elimineren.
  * Verminder het risico en de kosten voor Cloud migraties, vergeleken met tijdrovende conversies of herarchitectuur.
  * Gebruik vertrouwde VMware-hulpprogram ma's en-vaardig heden om Cloud migraties te versnellen. Gebruik Azure-Services in de cloud om uw toepassingen in uw tempo te moderniseren.

* **Op aanvraag uitvouwen**

  * Breid uit naar de cloud om te voldoen aan niet-verwachte behoeften, zoals nieuwe ontwikkelings omgevingen of bursts met een seizoen capaciteit.
  * Maak op aanvraag nieuwe capaciteit en bewaar deze alleen zolang u dat nodig hebt.
  * Verminder uw investering in de voor grond, Versnel snelheid van het inrichten en verminder de complexiteit met dezelfde architectuur en hetzelfde beleid in zowel on-premises als in de Cloud.

* **Herstel na nood gevallen en virtuele Bureau bladen in de Azure-Cloud**

  * Externe toegang tot gegevens, apps en Bureau bladen in de Azure-Cloud tot stand brengen. Met verbindingen met een hoge band breedte kunt u snel gegevens uploaden/downloaden om van incidenten te herstellen. Netwerken met lage latentie bieden snelle reactie tijden die gebruikers verwachten van een desktop-app.

  * Repliceer al uw beleids regels en netwerken in de Cloud met behulp van de CloudSimple-Portal en vertrouwde VMware-hulpprogram ma's. Deze replicatie vermindert de inspanning en het risico van het maken en beheren van DR-en VDI-implementaties.

* **Toepassingen en data bases met hoge prestaties**

  * Voer uw meest veeleisende workloads uit, met de hypergeconvergeerd-architectuur van CloudSimple.
  * Voer Oracle, micro soft SQL Server, middleware Systems en no-SQL-data bases met hoge prestaties uit.

  * Ervaar de Cloud als uw eigen Data Center met hoge snelheid van 25 Gbps netwerk verbindingen. Met verbindingen met hoge snelheid kunt u hybride apps uitvoeren die on-premises, VMware op Azure en persoonlijke werk belastingen van Azure omvatten, zonder de prestaties te verbeteren.

* **Waar hybride**

  * Verdeel DevOps tussen VMware en Azure-Services.
  * Optimaliseer VMware-beheer voor Azure-Services en-oplossingen die in al uw workloads kunnen worden toegepast.
  * Toegang tot open bare Cloud Services zonder uw Data Center uit te breiden of uw toepassingen opnieuw te ontwerpen.
  * Centraliseer identiteiten, Toegangs beheer beleid, logboek registratie en controle voor VMware-toepassingen op Azure.

## <a name="limits"></a>Limieten

Onderstaande tabel bevat de limieten voor knoop punten van resources van een privécloud.

| Resource | Limiet |
|----------|-------|
| Minimum aantal knoop punten voor het maken van een privécloud | 3 |
| Maximum aantal knoop punten in een cluster in een privécloud | 16 |
| Maximum aantal knoop punten in een privécloud | 64 |
| Minimum aantal knoop punten op een nieuw cluster | 3 |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [maken van een privécloud](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Meer informatie over het [configureren van een privécloud](quickstart-create-private-cloud.md)
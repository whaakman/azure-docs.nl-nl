---
title: Azure VMware-oplossing door CloudSimple Private Cloud beheren
description: Beschrijft de mogelijkheden die beschikbaar zijn voor het beheren van uw CloudSimple Private Cloud-resources en activiteit
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 05a2fb451b3acce1011c1d5f4cf17f0a865d57d0
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333155"
---
# <a name="manage-private-cloud-resources-and-activity"></a>Private Cloud-resources en activiteiten beheren

Persoonlijke clouds worden beheerd via CloudSimple-portal.  Controleer de status, de beschikbare resources, de activiteit op de privécloud, en andere instellingen van de portal CloudSimple.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Toegang tot de CloudSimple-portal

Toegang tot de [CloudSimple portal](access-cloudsimple-portal.md).

## <a name="view-the-list-of-private-clouds"></a>De lijst met Privéclouds weergeven

De **Privéclouds** tabblad op de **Resources** pagina geeft een lijst van alle persoonlijke Clouds in uw abonnement. Gegevens omvatten de naam, het aantal vSphere-clusters, locatie, huidige status van de persoonlijke gegevens van de cloud en resource.

![Private Cloud-pagina](media/manage-private-cloud.png)

Selecteer een Private Cloud voor meer informatie en acties.

## <a name="private-cloud-summary"></a>Private Cloud-overzicht

Bekijk een compleet overzicht van de geselecteerde Private Cloud.  Overzichtspagina omvat de DNS-servers geïmplementeerd op de Privécloud.  U kunt DNS instellen van on-premises DNS-servers worden doorgestuurd naar uw persoonlijke Cloud DNS-servers.  Zie voor meer informatie over DNS-doorsturen [DNS configureren voor naamomzetting voor vCenter van on-premises Privécloud](https://docs.azure.cloudsimple.com/on-premises-dns-setup/).

![Private Cloud-overzicht](media/private-cloud-summary.png)

### <a name="available-actions"></a>Beschikbare acties

* [VSphere-client starten](https://docs.azure.cloudsimple.com/vsphere-access/). Toegang tot de vCenter voor deze Privécloud.
* [Kopen van knooppunten](create-nodes.md). Knooppunten toevoegen aan deze Privécloud.
* [Vouw](expand-private-cloud.md). Knooppunten toevoegen aan deze Privécloud.
* **Vernieuwen**. De informatie op deze pagina bijwerken.
* **Verwijder**. U kunt de Privécloud op elk gewenst moment verwijderen. **Voordat u verwijdert, ervoor zorgen dat u back-up van alle systemen en gegevens hebt gemaakt.** Een Privécloud verwijdert, worden alle virtuele machines, vCenter-configuratie en gegevens verwijderd. Klik op **verwijderen** in de sectie Samenvatting voor de geselecteerde Private Cloud. Na de verwijdering van alle gegevens van de Private Cloud in een veilige en uiterst compatibele wissing proces gewist.
* [Wijzigen van vSphere bevoegdheden](escalate-private-cloud-privileges.md).  Escaleren uw rechten op deze Privécloud.

## <a name="private-cloud-vlanssubnets"></a>Private Cloud-VLAN's / subnetten

De lijst met gedefinieerde VLAN's / subnetten weergeven voor de geselecteerde Private Cloud.  De lijst bevat het beheer van VLAN's / subnetten gemaakt wanneer de privécloud is gemaakt.

![Private Cloud - VLAN's / subnetten](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Beschikbare acties

* [VLAN's / subnetten toevoegen](https://docs.azure.cloudsimple.com/create-vlan-subnet/). Een VLAN/subset toevoegen aan deze Privécloud.

Selecteer een VLAN/Subnet voor de volgende acties
* [Firewall-tabel koppelen](https://docs.azure.cloudsimple.com/firewall/). Een firewall-tabel koppelen aan deze Privécloud.
* **Edit**
* **Verwijder** (alleen de gebruiker gedefinieerd VLAN's / subnetten)

## <a name="private-cloud-activity"></a>Private Cloud-activiteit

Bekijk de volgende informatie voor de geselecteerde Private Cloud.  De informatie over de activiteit is een gefilterde lijst van alle activiteiten voor de geselecteerde Private Cloud.  Deze pagina bevat maximaal 25 recente activiteiten.

* Recente waarschuwingen
* Recente gebeurtenissen
* Recente taken
* Recente audit

![Private Cloud - activiteit](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Cloud rekken

Cloud rekken zijn de bouwstenen van uw Privécloud. Elk rack biedt een eenheid van de capaciteit. CloudSimple configureert automatisch cloud rekken op basis van uw selecties tijdens het maken of uitbreiden van een Private Cloud.  De volledige lijst van cloud rekken weergeven, met inbegrip van de Privécloud die elk aan is toegewezen.

![Private Cloud - Cloud rekken](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere beheernetwerk

Lijst met VMware-management-resources en virtuele machines die momenteel zijn geconfigureerd op de Privécloud. Informatie bevat de softwareversie, de volledig gekwalificeerde domeinnaam (FQDN) en het IP-adres van de resources.

![Private Cloud - vSphere beheernetwerk](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [Privéclouds](cloudsimple-private-cloud.md)
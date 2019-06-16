---
title: Azure privé-DNS-Veelgestelde vragen
description: Veelgestelde vragen over Azure privé-DNS
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: c963cb1b6930b41a703b479e0213311d971e6606
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082844"
---
# <a name="azure-private-dns-faq"></a>Azure privé-DNS-Veelgestelde vragen

> [!IMPORTANT]
> Azure privé-DNS is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="does-azure-dns-support-private-domains"></a>Biedt ondersteuning voor Azure DNS private domeinen?

Ondersteuning voor persoonlijke domeinen wordt ondersteund met de functie van Azure privé-DNS-Zones. Persoonlijke DNS-zones worden beheerd met dezelfde hulpmiddelen als internetgerichte Azure DNS-zones. Ze zijn omgezet alleen uit in de opgegeven virtuele netwerken. Zie voor meer informatie de [overzicht](private-dns-overview.md).

Zie voor informatie over andere interne DNS-opties in Azure, [naamomzetting voor VM's en rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure DNS Private Zones werken in verschillende Azure-regio's?

Ja. Privézones wordt ondersteund voor DNS-omzetting tussen virtuele netwerken in Azure-regio's. Privézones werkt zelfs zonder expliciet peering van de virtuele netwerken. De virtuele netwerken moeten worden opgegeven als het probleem zou moeten virtuele netwerken voor de privézone. Klanten moet mogelijk de virtuele netwerken aan worden gekoppeld voor TCP/HTTP-verkeer van de ene regio naar een andere.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Verbinding met Internet is van virtuele netwerken die zijn vereist voor privézones?

Nee. Privézones werken samen met virtuele netwerken. Klanten gebruiken ze voor het beheren van domeinen voor virtuele machines of andere resources binnen en tussen virtuele netwerken. Verbinding met Internet is niet vereist voor naamomzetting.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Kan de dezelfde privézone worden gebruikt voor verschillende virtuele netwerken voor het omzetten van?

Ja. U kunt maximaal 1000 virtuele netwerken koppelen aan een enkele privézone.

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-linked-virtual-network-to-a-private-zone"></a>Een virtueel netwerk dat bij een ander abonnement hoort kan worden toegevoegd als een gekoppeld virtueel netwerk aan een privézone?

Ja. U moet schrijfmachtiging bewerking op de virtuele netwerken en de privé-DNS-zone. De machtiging schrijven kan worden verleend aan verschillende RBAC-rollen. De klassieke netwerk Inzender RBAC-rol heeft bijvoorbeeld schrijfmachtigingen voor de virtuele netwerken. Zie voor meer informatie over RBAC-rollen [Role-based access control van](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>De DNS-records automatisch worden geregistreerd virtuele machine in een privé-zone worden automatisch verwijderd wanneer u de virtuele machine verwijderen?

Ja. Als u een virtuele machine in een gekoppeld virtueel netwerk met automatische registratie ingeschakeld verwijdert, worden automatisch de geregistreerde records verwijderd.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Kan een record automatisch worden geregistreerd virtuele machine in een privé-zone in een gekoppeld virtueel netwerk handmatig worden verwijderd?

Ja. U kunt de automatisch geregistreerde DNS-records overschrijven met een handmatig gemaakte DNS-record in de zone. De volgende vraag en antwoord-adres in dit onderwerp.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>Wat gebeurt er wanneer ik wil een nieuwe DNS-record handmatig maken in een privé-zone met de naam van de dezelfde host als een bestaande virtuele-machine automatisch worden geregistreerd in een gekoppeld virtueel netwerk?

U probeert te maken van een nieuwe DNS-record handmatig in een privé-zone met de naam van de dezelfde host als een virtuele machine van de bestaande, automatisch worden geregistreerd in een gekoppeld virtueel netwerk. Wanneer u dit doet, overschrijft de nieuwe DNS-record de record automatisch worden geregistreerd virtuele machine. Als u dit handmatig gemaakte DNS-record verwijderen uit de zone het opnieuw probeert, slaagt de verwijdering. De automatische registratie zich weer voordoet, zolang de virtuele machine nog bestaat en of een privé IP-adres is gekoppeld aan. De DNS-record wordt automatisch opnieuw gemaakt in de zone.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Wat gebeurt er wanneer we een gekoppeld virtueel netwerk van een privézone ontkoppelen? De records automatisch worden geregistreerd virtuele machine van het virtuele netwerk verwijderd uit de zone te?

Ja. Als u wilt ontkoppelen van een gekoppeld virtueel netwerk van een privé-zone, moet u de DNS-zone als u wilt verwijderen van de koppeling van de gekoppelde virtuele netwerk bijwerken. Virtuele machine-records die automatisch zijn geregistreerd worden tijdens dit proces wordt verwijderd uit de zone.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Wat gebeurt er wanneer we een gekoppeld virtueel netwerk dat gekoppeld aan een privézone verwijderen? Hebben we de privézone als u wilt ontkoppelen van het virtuele netwerk als een gekoppelde virtuele netwerk van de zone handmatig bijwerken?

Ja. Wanneer u een gekoppeld virtueel netwerk zonder deze eerst ontkoppelen van een privé-zone verwijdert, wordt de verwijderingsbewerking slaagt. Maar het virtuele netwerk wordt niet automatisch ontkoppeld van uw persoonlijke zone, indien van toepassing. U moet het virtuele netwerk van de persoonlijke zone handmatig ontkoppelen. Om deze reden, het virtuele netwerk van uw privé-zone te ontkoppelen voordat u deze verwijdert.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>DNS-omzetting met behulp van de standaard FQDN-naam (internal.cloudapp.net) nog steeds werken, zelfs wanneer een privézone (bijvoorbeeld private.contoso.com) is gekoppeld aan een virtueel netwerk?

Ja. Privézones wordt de standaard DNS-oplossingen met behulp van de zone Azure verschafte internal.cloudapp.net niet vervangen. Het wordt aangeboden als een extra functie of een uitbreiding. Of u op de Azure verschafte internal.cloudapp.net of op uw eigen privé-zone vertrouwen, gebruikt u de FQDN-naam van de zone die u wilt oplossen op basis van.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Wordt het DNS-achtervoegsel op virtuele machines in een gekoppeld virtueel netwerk met die van de persoonlijke zone gewijzigd?

Nee. De DNS-achtervoegsel op de virtuele machines in het gekoppelde virtuele netwerk als het standaard-Azure verschafte achtervoegsel blijft ("*. internal.cloudapp.net '). U kunt dit DNS-achtervoegsel handmatig wijzigen op uw virtuele machines met die van de privézone.

## <a name="what-are-the-usage-limits-for-azure-private-dns"></a>Wat zijn de limieten voor Azure privé-DNS?

De volgende standaardlimieten van toepassing wanneer u Azure privé-DNS.

| Resource | Standaardlimiet |
| --- | --- |
|Persoonlijke DNS-zones per abonnement|1000|
|Recordsets per privé-DNS-zone|25,000|
|Records per Recordset|20|
|Virtueel netwerk-koppelingen per privé-DNS-zone|1000|
|Virtuele netwerken-koppelingen per privé-DNS-zones met automatische registratie ingeschakeld|100|
|Aantal privé-DNS-zones die kunt u een virtueel netwerk gekoppeld aan met automatische registratie ingeschakeld|1|
|Aantal privé-DNS-zones die kunt u een virtueel netwerk gekoppeld|1000|

## <a name="is-there-portal-support-for-private-zones"></a>Is er portal ondersteuning voor privézones?

Ja, en private zones die al zijn gemaakt via API's, PowerShell, CLI en SDK's zijn zichtbaar in de Azure portal.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure privé-DNS](private-dns-overview.md)
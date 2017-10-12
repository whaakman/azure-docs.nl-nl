---
title: Aan de slag met Azure DNS met behulp van Azure Portal | Microsoft Docs
description: Informatie over het maken van een DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone en -record met behulp van Azure Portal.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 93b24e3d9fbb3fbb3ea995271fd63d1e82eb9c9e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Aan de slag met Azure DNS met behulp van Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Dit artikel leidt u stapsgewijs door de procedure voor het maken van uw eerste DNS-zone en -record met behulp van Azure Portal. U kunt deze stappen ook uitvoeren met Azure PowerShell of de platformoverschrijdende Azure CLI.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Tot slot moet u de naamservers voor het domein configureren om de DNS-zone te publiceren naar internet. Deze stappen worden hieronder afzonderlijk beschreven.

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
2. Klik in het menu Hub en klik op **Nieuw > Netwerken >** en klik vervolgens op **DNS-zone** om de blade DNS-zone maken te openen.

    ![DNS-zone](./media/dns-getstarted-portal/openzone650.png)

4. Voer op de blade **DNS-zone maken** de volgende waarden in en klik op **Maken**:


   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|contoso.com|De naam van de DNS-zone|
   |**Abonnement**|[Uw abonnement]|Selecteer een abonnement waarin de DNS-zone moet worden gemaakt.|
   |**Resourcegroep**|**Nieuwe maken:** contosoDNSRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

> [!NOTE]
> De resourcegroep verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

## <a name="create-a-dns-record"></a>Een DNS-record maken

In het volgende voorbeeld ziet u hoe de nieuwe 'A'-record wordt gemaakt. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van Azure Portal). 

1. Nu de DNS-zone is gemaakt, klikt u in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op de blade Alle resources op de DNS-zone **contoso.com**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u **contoso.com** invoeren in het vak **Filteren op naam...** voor eenvoudige toegang tot de DNS-zone.

1. Selecteer boven in de blade **DNS-zone** de optie **+ Recordset** om de blade **Recordset toevoegen** te openen.

1. Voer op de blade **Recordset toevoegen** de volgende waarden in en klik op **OK**. In dit voorbeeld maakt u een A-record.

   |**Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|www|Naam van de record|
   |**Type**|A| Type DNS-record dat u wilt maken; toegestane waarden zijn A, AAAA, CNAME MX, NS, SRV, TXT en PTR.  Voor meer informatie over recordtypen gaat u naar [Overview of DNS zones and records](dns-zones-records.md) (Overzicht van DNS-zones en -records)|
   |**TTL**|1|Time-to-Live van de DNS-aanvraag.|
   |**TTL-eenheid**|Uren|Maateenheid van de TTL-waarde.|
   |**IP-adres**|ipAddressValue| Dit is het IP-adres dat de DNS-record oplost.|

## <a name="view-records"></a>Records weergeven

In het onderste gedeelte van de blade van de DNS-zone ziet u de records voor de DNS-zone. U ziet de standaardrecords voor DNS en SOA, die in elke zone worden gemaakt, plus alle nieuwe records die u hebt gemaakt.

![zone](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Naamservers bijwerken

Wanneer uw DNS-zone en -records correct zijn ingesteld, moet u uw domeinnaam configureren voor het gebruik van de Azure DNS-naamservers . Op die manier kunnen andere gebruikers op internet uw DNS-records vinden.

De naamservers voor de zone worden verstrekt in Azure Portal:

![zone](./media/dns-getstarted-portal/viewzonens500.png)

Deze naamservers moeten worden geconfigureerd met de domeinnaamregistrar (waar u de domeinnaam hebt gekocht). Uw registrar biedt u de mogelijkheid om de naamservers voor het domein in te stellen. Zie [Uw domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

1. Klik in het deelvenster **Favorieten** van Azure Portal op **Alle resources**. Klik op de blade Alle resources op de resourcegroep **MyResourceGroup**. Als het abonnement dat u hebt geselecteerd, al verschillende resources heeft, kunt u **MyResourceGroup** invoeren in het vak **Filteren op naam...** voor eenvoudige toegang tot de resourcegroep.
1. Klik op de blade **MyResourceGroup** op de knop **Verwijderen**.
1. De portal vereist dat u de naam van de resourcegroep typt om te bevestigen dat u deze wilt verwijderen. Klik op **Verwijderen**, typ *MyResourceGroup* als naam van de resourcegroep en klik op **Verwijderen**. Als u een resourcegroep verwijdert, worden alle resources binnen de resourcegroep verwijderd. Zorg er dus altijd voor dat u de inhoud van een resourcegroep bevestigt, voordat u deze verwijdert. Alle resources die zich in de resourcegroep bevinden en de resourcegroep zelf worden verwijderd. Dit proces duurt enkele minuten.


## <a name="next-steps"></a>Volgende stappen

Zie [Azure DNS Overview](dns-overview.md) (Overzicht van Azure DNS) voor meer informatie over Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van Azure Portal) voor meer informatie over het beheren van DNS-records in Azure DNS.


---
title: Aan de slag met Azure DNS met behulp van de Azure-portal | Microsoft Docs
description: Informatie over het maken van een DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone en -record met behulp van de Azure-portal.
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
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 79f0c9297c4be70f705f325274f3d9241ea4bc3f
ms.lasthandoff: 03/29/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Aan de slag met Azure DNS met behulp van de Azure-portal

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Dit artikel leidt u stapsgewijs door de procedure voor het maken van uw eerste DNS-zone en -record met behulp van de Azure-portal. U kunt deze stappen ook uitvoeren met Azure PowerShell of de platformoverschrijdende Azure CLI.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Tot slot moet u de naamservers voor het domein configureren om de DNS-zone te publiceren naar internet. Deze stappen worden hieronder allemaal beschreven.

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
2. Klik in het menu Hub en klik op **Nieuw > Netwerken >** en klik vervolgens op **DNS-zone** om de blade DNS-zone maken te openen.

    ![DNS-zone](./media/dns-getstarted-portal/openzone650.png)

4. Op de blade **DNS-zone maken** geeft u de DNS-zone een naam. Bijvoorbeeld *contoso.com*.
 
    ![Zone maken](./media/dns-getstarted-portal/newzone250.png)

5. Daarna geeft u de resourcegroep op die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep selecteren. Als u ervoor kiest om een nieuwe resourcegroep te maken, gebruikt u de vervolgkeuzelijst **Locatie** om de locatie van de resourcegroep op te geven. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

6. U kunt het selectievakje **Vastmaken aan dashboard** ingeschakeld laten als u de nieuwe zone eenvoudig op het dashboard wilt kunnen vinden. Klik vervolgens op **Maken**.

    ![Vastmaken aan dashboard](./media/dns-getstarted-portal/pindashboard150.png)

7. Nadat u op maken hebt geklikt, wordt de geconfigureerde nieuwe zone op het dashboard weergegeven.

    ![Maken](./media/dns-getstarted-portal/creating150.png)

8. Wanneer de nieuwe zone is gemaakt, wordt de blade voor de nieuwe zone in het dashboard geopend.


## <a name="create-a-dns-record"></a>Een DNS-record maken

In het volgende voorbeeld ziet u hoe de nieuwe 'A'-record wordt gemaakt. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van de Azure-portal). 


1. Selecteer boven in de blade **DNS-zone** de optie **+ Recordset** om de blade **Recordset toevoegen** te openen.

    ![Nieuwe recordset](./media/dns-getstarted-portal/newrecordset500.png)

4. In de blade **Recordset toevoegen** geeft u de recordset een naam. U kunt de recordset bijvoorbeeld '**www**' noemen.

    ![Recordset toevoegen](./media/dns-getstarted-portal/addrecordset500.png)

5. Selecteer het type record dat u wilt maken. Selecteer voor dit voorbeeld **A**.
6. Stel de **TTL** in. De standaard TTL-waarde (Time to Live) is een uur.
7. Voeg het IP-adres van de record toe.
8. Selecteer onder in de blade **OK** om de DNS-record te maken.


## <a name="view-records"></a>Records weergeven

In het onderste gedeelte van de blade van de DNS-zone ziet u de records voor de DNS-zone. U ziet de standaardrecords voor DNS en SOA, die in elke zone worden gemaakt, plus alle nieuwe records die u hebt gemaakt.

![zone](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Naamservers bijwerken

Wanneer uw DNS-zone en -records correct zijn ingesteld, moet u uw domeinnaam configureren voor het gebruik van de Azure DNS-naamservers . Op die manier kunnen andere gebruikers op internet uw DNS-records vinden.

De naamservers voor de zone worden verstrekt in de Azure-portal:

![zone](./media/dns-getstarted-portal/viewzonens500.png)

Deze naamservers moeten worden geconfigureerd met de domeinnaamregistrar (waar u de domeinnaam hebt gekocht). Uw registrar zal u de mogelijkheid bieden om de naamservers voor het domein in te stellen. Zie [Uw domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

Zie [Azure DNS Overview](dns-overview.md) (Overzicht van Azure DNS) voor meer informatie over Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van de Azure-portal) voor meer informatie over het beheren van DNS-records in Azure DNS.



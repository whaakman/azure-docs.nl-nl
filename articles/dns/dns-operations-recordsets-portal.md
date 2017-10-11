---
title: Beheren van DNS-recordsets en records met Azure DNS | Microsoft Docs
description: Azure DNS biedt de mogelijkheid voor het beheren van DNS-recordsets en records bij het hosten van uw domein.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 001b80ccba43beab44f6a598f820df65a85a345f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>Beheren van DNS-records en recordsets met behulp van de Azure-portal

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

In dit artikel laat zien hoe recordsets en records voor de DNS-zone beheren met behulp van de Azure-portal.

Het is belangrijk om het verschil tussen de DNS-recordsets en afzonderlijke DNS-records. Een recordset is een verzameling van records in een zone die dezelfde naam hebben en hetzelfde type zijn. Zie voor meer informatie [maken DNS-recordsets en records met behulp van de Azure-portal](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Een nieuwe recordset en record maken

Zie het maken van een record in de Azure-portal [maken DNS-records met behulp van de Azure-portal](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>Een recordset weergeven

1. In de Azure portal, gaat u naar de **DNS-zone** blade.
2. De recordset zoekt en selecteert u deze. Hiermee opent u de eigenschappen van de recordset.

    ![Zoeken naar een Recordset](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a>Een nieuwe record toevoegen aan een Recordset

U kunt maximaal 20 records toevoegen aan een recordset. Een recordset mag niet twee identieke records. Lege recordsets (met nul records) kunnen worden gemaakt, maar worden niet weergegeven op de Azure DNS-naamservers. Recordsets van het type CNAME kunnen maximaal één record bevatten.

1. Op de **eigenschappen van de recordset** blade voor uw DNS-zone, klikt u op de recordset die u wilt toevoegen van een record.

    ![Selecteer een Recordset](./media/dns-operations-recordsets-portal/selectset500.png)

2. Geef dat de eigenschappen van de recordset door in de velden te vullen.

    ![Een record toevoegen](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Klik op **opslaan** boven aan de blade als u uw instellingen opslaan. Sluit de blade.
4. In de hoek ziet u dat de record wordt opgeslagen.

    ![Recordset opslaan](./media/dns-operations-recordsets-portal/saving150.png)

Nadat de record is opgeslagen, de waarden op de **DNS-zone** blade geeft de nieuwe record.

## <a name="update-a-record"></a>Een record bijwerken

De velden die u kunt bijwerken wanneer u een record in een bestaande recordset bijwerkt, zijn afhankelijk van het type record waarmee u werkt.

1. Op de **eigenschappen van de recordset** blade voor uw recordset en zoekt u de record.
2. De record wijzigen. Wanneer u een record wijzigt, kunt u de beschikbare instellingen voor de record. In het volgende voorbeeld wordt de **IP-adres** hebt ingeschakeld, en het IP-adres is bezig te worden gewijzigd.

    ![Een record wijzigen](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klik op **opslaan** boven aan de blade als u uw instellingen opslaan. In de rechterbovenhoek ziet u de melding dat de record is opgeslagen.

    ![Recordset opgeslagen](./media/dns-operations-recordsets-portal/saved150.png)

Nadat de record is opgeslagen, de waarden voor de record moet worden ingesteld op de **DNS-zone** blade geeft de bijgewerkte record.

## <a name="remove-a-record-from-a-record-set"></a>Een record verwijderen uit een Recordset

U kunt de Azure-portal gebruiken om records te verwijderen van een recordset. Houd er rekening mee dat de laatste record van een recordset niet de recordset verwijderen worden.

1. Op de **eigenschappen van de recordset** blade voor uw recordset en zoekt u de record.
2. Klik op de record die u wilt verwijderen. Selecteer vervolgens **verwijderen**.

    ![Een record verwijderen](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klik op **opslaan** boven aan de blade als u uw instellingen opslaan.
4. Nadat de record is verwijderd, de waarden voor de record op de **DNS-zone** blade geeft de verwijzing wordt verwijderd.

## <a name="delete"></a>Een recordset verwijderen

1. Op de **eigenschappen van de recordset** blade voor uw recordset en klikt u op **verwijderen**.

    ![Een recordset verwijderen](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Er verschijnt een bericht waarin wordt gevraagd als u wilt verwijderen van de recordset.
3. Controleren of de naam overeenkomt met de recordset die u wilt verwijderen en klik vervolgens op **Ja**.
4. Op de **DNS-zone** blade, Controleer of de recordset is niet meer zichtbaar.

## <a name="work-with-ns-and-soa-records"></a>Werken met NS en SOA-records

NS en SOA-records die automatisch worden gemaakt, worden anders worden beheerd vanuit andere recordtypen.

### <a name="modify-soa-records"></a>SOA-records wijzigen

U kunt toevoegen of verwijderen van records uit de automatisch gemaakte SOA-record is ingesteld in het toppunt van de zone (naam = ' @ '). Echter, kunt u een van de parameters binnen de SOA-record (met uitzondering van de "Host") en de TTL-waarde van de recordset.

### <a name="modify-ns-records-at-the-zone-apex"></a>NS-records in het toppunt van de zone wijzigen

De NS-recordset in het toppunt van de zone wordt automatisch gemaakt met elke DNS-zone. Het bevat de namen van de Azure DNS-naamservers toegewezen aan de zone.

U kunt extra naam ingesteld van servers aan deze NS-record, ter ondersteuning van collega hosting domeinen met meer dan één DNS-provider toevoegen. U kunt ook de TTL-waarde en de metagegevens voor deze recordset wijzigen. U kan echter verwijderen of wijzigen van de vooraf ingestelde Azure DNS-naamservers.

Houd er rekening mee dat dit alleen voor de NS-recordset in het toppunt van de zone geldt. Andere NS-recordsets in de zone (zoals gebruikt voor het delegeren van onderliggende zones) kunnen worden gewijzigd zonder beperking.

### <a name="delete-soa-or-ns-record-sets"></a>SOA- of NS recordsets verwijderen

U kunt de SOA niet verwijderen en NS-record wordt ingesteld in het toppunt van de zone (naam = ' @ ') die automatisch worden gemaakt wanneer de zone wordt gemaakt. Ze worden automatisch verwijderd wanneer u de zone verwijdert.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure DNS de [Azure DNS-overzicht](dns-overview.md).
* Zie voor meer informatie over het automatiseren van DNS [maken van DNS-zones en -recordsets met de .NET SDK](dns-sdk.md).
* Zie voor meer informatie over de reverse DNS-records [overzicht van de reverse DNS- en biedt ondersteuning in Azure](dns-reverse-dns-overview.md).

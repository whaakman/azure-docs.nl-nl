---
title: Een Azure bastion-host maken | Microsoft Docs
description: In dit artikel leest u hoe u een Azure bastion-host maakt
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: cherylmc
ms.openlocfilehash: 881d89170c82731fca3f6977da486eac5be772f8
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466996"
---
# <a name="create-an-azure-bastion-host-preview"></a>Een Azure bastion-host maken (preview)

In dit artikel wordt beschreven hoe u een Azure bastion-host maakt. Zodra u de Azure Bastion-service in uw virtuele netwerk hebt ingericht, is de naadloze RDP/SSH-ervaring beschikbaar voor al uw virtuele machines in hetzelfde virtuele netwerk. Deze implementatie is per virtueel netwerk, niet per abonnement/account of virtuele machine.

Er zijn twee manieren waarop u een bastion-host kunt maken:

* Maak een bastion-resource met behulp van de Azure Portal.
* Maak een bastion-bron in de Azure Portal met behulp van bestaande VM-instellingen.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

De open bare preview is beperkt tot de volgende open bare Azure-regio's:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Een bastion-host maken

In deze sectie kunt u een nieuwe Azure Bastion-resource maken op basis van de Azure Portal.

1. Klik op de start pagina van het [Azure Portal-Bastion-voor beeld](https://aka.ms/BastionHost)op **+ een resource maken**. Zorg ervoor dat u de meegeleverde koppeling gebruikt om toegang te krijgen tot de portal voor deze preview, niet de gewone Azure Portal.

1. Typ **Bastion**in het veld *Marketplace doorzoeken* op de pagina **Nieuw** en klik vervolgens op **Enter** om naar de zoek resultaten te gaan.

1. Klik in de resultaten op **Bastion (preview)** . Zorg ervoor dat de uitgever van *micro soft* is en dat de categorie *netwerk*is.

1. Klik op de pagina **Bastion (voor beeld)** op **maken** om de pagina **een bastion maken** te openen.

1. Configureer op de pagina **een bastion maken** een nieuwe Bastion-resource. Geef de configuratie-instellingen voor uw Bastion-resource op.

    ![een bastion maken](./media/bastion-create-host-portal/settings.png)

    * **Abonnement**: Het Azure-abonnement dat u wilt gebruiken voor het maken van een nieuwe Bastion-resource.
    * **Resourcegroep**: De Azure-resource groep waarin de nieuwe Bastion-resource wordt gemaakt. Als u geen bestaande resource groep hebt, kunt u een nieuwe maken.
    * **Naam**: De naam van de nieuwe Bastion-resource
    * **Regio**: De open bare Azure-regio waarin de resource wordt gemaakt.
    * **Virtueel netwerk**: Het virtuele netwerk waarin de Bastion-resource wordt gemaakt. U kunt tijdens dit proces een nieuw virtueel netwerk maken in de portal, als u geen bestaand virtueel netwerk wilt gebruiken. Als u een bestaand virtueel netwerk gebruikt, zorg er dan voor dat het bestaande virtuele netwerk voldoende vrije adres ruimte heeft om de vereisten van het Bastion-subnet aan te passen.
    * **Subnet**: Het subnet in het virtuele netwerk waarop de nieuwe bastion-host wordt geïmplementeerd. U moet een subnet maken met de naam waarde **AzureBastionSubnet**. Met deze waarde kan Azure weten met welk subnet de Bastion-resources moeten worden geïmplementeerd. Dit wijkt af van een gateway-subnet. We raden u ten zeerste aan om ten minste een/27 of groter subnet (/27,/26 enzovoort) te gebruiken. Maak de **AzureBastionSubnet** zonder route tabellen of delegaties. Wanneer u netwerk beveiligings groepen op de **AzureBastionSubnet**gebruikt, raadpleegt u [werken met nsg's](bastion-nsg.md).
    * **Openbaar IP-adres**: Het open bare IP-adres van de Bastion-bron waarop RDP/SSH wordt gebruikt (via poort 443). Maak een nieuw openbaar IP-adres of gebruik een bestaande. Het open bare IP-adres moet zich in dezelfde regio bevinden als de Bastion-resource die u maakt.
    * **Openbare IP-adresnaam**: De naam van de resource voor het open bare IP-adres.
    * **SKU openbaar IP-adres**: Standaard vooraf **ingevuld.** Azure Bastion gebruikt/ondersteunt alleen de standaard open bare IP-SKU.
    * **Toewijzing**: Standaard vooraf ingevuld naar **statisch**.

1. Wanneer u klaar bent met het opgeven van de instellingen, klikt u op **controleren + maken**. Hiermee worden de waarden gevalideerd. Zodra de validatie is geslaagd, kunt u beginnen met het aanmaak proces.
1. Klik op de pagina een bastion maken op **maken**.
1. Er wordt een bericht weer gegeven met de melding dat uw implementatie zich bevindt. De status wordt op deze pagina weer gegeven wanneer de resources zijn gemaakt. Het duurt ongeveer 5 minuten voordat de Bastion-resource wordt gemaakt en geïmplementeerd.

## <a name="createvmset"></a>Een bastion-host maken met behulp van VM-instellingen

Als u een bastion-host in de portal maakt met behulp van een bestaande virtuele machine, worden er automatisch verschillende instellingen gebruikt die overeenkomen met de virtuele machines en/of het virtuele netwerk.

1. Navigeer in het [Azure Portal-Bastion-voor beeld](https://aka.ms/BastionHost)naar uw virtuele machine en klik vervolgens op **verbinding maken**.

    ![VM-verbinding](./media/bastion-create-host-portal/vmsettings.png)

1. Klik in de rechter zijbalk op **Bastion**en **gebruik vervolgens Bastion**.

    ![Bastion](./media/bastion-create-host-portal/vmbastion.png)

1. Vul op de pagina Bastion de volgende instellingen in:

    * **Naam**: De naam van de bastion-host die u wilt maken.
    * **Subnet**: Het subnet in het virtuele netwerk waarop de Bastion-resource wordt geïmplementeerd. Het subnet moet worden gemaakt met de naam **AzureBastionSubnet**. Hiermee kan Azure weten met welk subnet de Bastion-resource moet worden geïmplementeerd. Dit wijkt af van een gateway-subnet. Klik op **subnet-configuratie beheren** om het Azure Bastion-subnet te maken. We raden u ten zeerste aan om ten minste een/27 of groter subnet (/27,/26 enzovoort) te gebruiken. Maak de **AzureBastionSubnet** zonder netwerk beveiligings groepen, route tabellen of delegaties. Klik op **maken** om het subnet te maken en ga vervolgens verder met de volgende instellingen.

      ![Bastion](./media/bastion-create-host-portal/subnet.png)
      
    * **Openbaar IP-adres**: Het open bare IP-adres van de Bastion-bron waarop RDP/SSH wordt gebruikt (via poort 443). Maak een nieuw openbaar IP-adres of gebruik een bestaande. Het open bare IP-adres moet zich in dezelfde regio bevinden als de Bastion-resource die u maakt.
    * **Openbare IP-adresnaam**: De naam van de resource voor het open bare IP-adres.
1. Klik op het scherm validatie op **maken**. Wacht ongeveer 5 minuten voordat de Bastion-resource is gemaakt en geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Lees de [Veelgestelde vragen over Bastion](bastion-faq.md)

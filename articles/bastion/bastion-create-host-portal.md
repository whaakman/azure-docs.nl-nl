---
title: Maken van een Azure-bastionhost | Microsoft Docs
description: In dit artikel wordt informatie over het maken van een Azure-bastionhost
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 5fbd64d2f001a0fbe91de2d954889816a1819479
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67339439"
---
# <a name="create-an-azure-bastion-host-preview"></a>Maken van een Azure-bastionhost (Preview)

Dit artikel ziet u hoe u een Azure-bastionhost maakt. Nadat u de service Azure bastionhost in uw virtuele netwerk inrichten, is de naadloze RDP/SSH-ervaring beschikbaar voor alle virtuele machines in hetzelfde virtuele netwerk. Deze implementatie is per virtueel netwerk, niet per abonnement/account of virtuele machine.

Er zijn twee manieren die u kunt een resource van de host bastionhost maken:

* Een Bastionomgeving-resource met de Azure portal maken.
* Een Bastionomgeving-resource maken in Azure portal met behulp van de instellingen van een bestaande virtuele machine.

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voordat u begint

De openbare preview is beperkt tot de volgende Azure openbare regio's:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

Als u wilt deelnemen aan deze Preview-versie, moet u registreren. Volg deze stappen om u te registreren voor de Preview-versie:

[!INCLUDE [register](../../includes/bastion-preview-register-include.md)]

## <a name="createhost"></a>Maken van een bastionhost

In deze sectie kunt u een nieuwe bastionhost Azure-resource maken vanuit Azure portal.

1. Vanaf de startpagina in de [ Azure portal - preview](https://aka.ms/BastionHost), klikt u op **+ een resource maken**. Zorg ervoor dat u de koppeling om toegang tot de portal voor deze Preview-versie, niet de normale Azure portal.

1. Op de **nieuw** pagina, in de *Marketplace doorzoeken* veld, typt u **bastionhost**, klikt u vervolgens op **Enter** om te gaan naar de lijst met zoekresultaten.

1. Klik in de resultaten op **bastionhost (preview)** . Zorg ervoor dat de uitgever is *Microsoft* en de categorie is *netwerken*.

1. Op de **bastionhost (preview)** pagina, klikt u op **maken** openen de **maken van een bastionforest** pagina.

1. Op de **maken van een bastionforest** pagina, een nieuwe bastionhost resource configureren. Geef de configuratie-instellingen voor uw Bastionforest.

    ![een bastionomgeving maken](./media/bastion-create-host-portal/settings.png)

    * **Abonnement**: De Azure-abonnement die u wilt gebruiken om een nieuwe bastionhost-resource te maken.
    * **Resourcegroep**: De Azure-resourcegroep waarin de nieuwe bastionhost resource wordt gemaakt in. Als u een bestaande resourcegroep hebt, kunt u een nieuwe maken.
    * **Naam**: De naam van de nieuwe bastionhost-resource
    * **Regio**: De Azure-openbare regio waarin de resource wordt gemaakt.
    * **Virtueel netwerk**: Het virtuele netwerk waarin de resource van de Bastionomgeving wordt gemaakt in. U kunt een nieuw virtueel netwerk maken in de portal tijdens dit proces, in het geval u niet hebt of niet wilt gebruiken van een bestaand virtueel netwerk. Als u van een bestaand virtueel netwerk gebruikmaakt, zorg er dan voor dat het bestaande virtuele netwerk heeft onvoldoende vrije adresruimte op aan de Bastionomgeving subnet te voldoen.
    * **Subnet**: Het subnet in uw virtuele netwerk waaraan de nieuwe resource van de Bastionomgeving-host wordt geïmplementeerd. U moet maken met behulp van de waarde van de **AzureBastionSubnet**. Deze waarde kunt Azure weet welk subnet de Bastionomgeving resources wilt implementeren. Dit is anders dan een gatewaysubnet. Is het raadzaam dat u ten minste een/27 of groter subnet (/ 27, / 26 enzovoort). Maak de **AzureBastionSubnet** routeren zonder eventuele Network Security Groups, tabellen, of overdrachten.
    * **Openbaar IP-adres**: Het openbare IP-adres van de Bastionomgeving resource waarop RDP/SSH wordt gebruikt (via poort 443). Maak een nieuw openbaar IP-adres of gebruik een bestaande resourcegroep. Het openbare IP-adres moet zich in dezelfde regio bevinden als de Bastionomgeving-resource die u maakt.
    * **Openbare IP-adresnaam**: De naam van het openbare IP-adresresource.
    * **Openbare IP-adres SKU**: Vooraf ingevuld standaard **Standard**.
    * **Toewijzing**: Vooraf ingevuld standaard **statische**.

1. Wanneer u klaar bent met de instellingen op te geven, klikt u op **revisie + maken**. Hiermee valideert u de waarden. Nadat de validatie is geslaagd, u kunt beginnen met het aanmaakproces.
1. Klik op het maken van een pagina van de bastionomgeving, **maken**.
1. U ziet een bericht waarin wordt aangegeven dat uw implementatie uitgevoerd wordt. De status wordt weergegeven op deze pagina nadat de resources zijn gemaakt. Het duurt ongeveer 5 minuten voor de Bastionomgeving resource moet worden gemaakt en geïmplementeerd.

## <a name="createvmset"></a>Maken van een bastionhost met behulp van VM-instellingen

Als u een bastionhost in de portal met behulp van een bestaande virtuele machine maakt, wordt in verschillende instellingen automatisch standaard overeenkomt met uw virtuele machine en/of het virtuele netwerk.

1. In de [preview-portal](https://aka.ms/BastionHost), gaat u naar uw virtuele machine en klik vervolgens op **Connect**.

    ![Virtuele machine verbinding maken](./media/bastion-create-host-portal/vmsettings.png)

1. Klik op de juiste zijbalk **bastionhost**, klikt u vervolgens **gebruik bastionhost**.

    ![Bastion](./media/bastion-create-host-portal/vmbastion.png)

1. Vul de velden van de volgende instellingen op de pagina bastionhost:

    * **Naam**: De naam van de bastionhost die u wilt maken.
    * **Subnet**: Het subnet in uw virtuele netwerk waarin bastionhost resource wordt geïmplementeerd. Het subnet moet worden gemaakt met de naam van de **AzureBastionSubnet**. Hierdoor weten welk subnet voor het implementeren van de Bastionomgeving resource toe aan Azure. Dit is anders dan een gatewaysubnet. Klik op **beheren subnetconfiguratie** om het Subnet van de Bastionomgeving Azure te maken. Is het raadzaam dat u ten minste een/27 of groter subnet (/ 27, / 26 etc.). Maak de **AzureBastionSubnet** routeren zonder eventuele Network Security Groups, tabellen, of overdrachten. Klik op **maken** voor het maken van het subnet, gaat u verder met de volgende instellingen.

      ![Bastion](./media/bastion-create-host-portal/subnet.png)
      
    * **Openbaar IP-adres**: Het openbare IP-adres van de Bastionomgeving resource waarop RDP/SSH wordt gebruikt (via poort 443). Maak een nieuw openbaar IP-adres of gebruik een bestaande resourcegroep. Het openbare IP-adres moet zich in dezelfde regio bevinden als de Bastionomgeving-resource die u maakt.
    * **Openbare IP-adresnaam**: De naam van het openbare IP-adresresource.
1. Klik op het scherm validatie **maken**. Wacht ongeveer 5 minuten voor de Bastionomgeving resource moet worden gemaakt en geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Lees de [bastionhost Veelgestelde vragen](bastion-faq.md)
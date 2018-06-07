---
title: Azure SQL-Database beheerd exemplaar VNet-configuratie | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een virtueel netwerk (VNet) met een Azure SQL Database beheerd-exemplaar.
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 45b645fe556d1f9630ee15b1f2f605bbaf3f87f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648061"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Een VNet configureren voor beheerde Azure SQL Database-exemplaar

Azure SQL Database beheerd-instantie (preview) moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Deze implementatie kunt de volgende scenario's: 
- Rechtstreeks verbinding maken met een exemplaar beheerd vormen een on-premises netwerk 
- Verbinding maken met een exemplaar beheerd gekoppelde server of een andere lokale gegevensopslag 
- Verbinding maken met een exemplaar beheerde Azure-resources  

## <a name="plan"></a>Plannen

Plan hoe u een exemplaar beheerd in het virtuele netwerk met behulp van uw antwoorden op de volgende vragen implementeren: 
- Wilt u één of meerdere beheerde exemplaren implementeren? 

  Het aantal exemplaren beheerd bepaalt de minimale grootte van het subnet toewijzen voor uw exemplaren beheerd. Zie voor meer informatie [bepalen van de grootte van het subnet voor beheerde exemplaar](#create-a-new-virtual-network-for-managed-instances). 
- Wilt u uw exemplaar beheerd implementeren in een bestaand virtueel netwerk of maakt u een nieuw netwerk? 

   Als u van plan bent te gebruiken van een bestaand virtueel netwerk, moet u die netwerkconfiguratie voor uw beheerde exemplaar wijzigen. Zie voor meer informatie [bestaand virtueel netwerk wijzigen voor beheerde exemplaar](#modify-an-existing-virtual-network-for-managed-instances). 

   Als u van plan bent een nieuw virtueel netwerk maken, Zie [nieuw virtueel netwerk maken voor beheerde exemplaar](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Vereisten

Voor het maken van de beheerde exemplaar moet u subnet binnen het VNet dat voldoet reserveren voor de volgende vereisten:
- **Leeg**: het subnet moet een andere cloudservice die is gekoppeld aan het niet bevatten en mag geen gatewaysubnet. Het niet mogelijk te maken van exemplaar beheerd in het subnet dat bronnen dan beheerde exemplaar bevat of andere resources binnen het subnet later toevoegen.
- **Er is geen NSG**: het subnet moet een Netwerkbeveiligingsgroep gekoppeld niet hebben.
- **Hebt u specifieke routetabel**: het subnet moet een gebruiker Route-tabel (UDR) met de volgende Hop Internet 0.0.0.0/0 hebben als de enige route toegewezen. Zie voor meer informatie [de vereiste routetabel maken en koppelen](#create-the-required-route-table-and-associate-it)
3. **Optionele aangepaste DNS**: als aangepaste DNS is opgegeven in het VNet, Azure recursieve resolvers IP-adres (zoals 168.63.129.16) moet worden toegevoegd aan de lijst. Zie voor meer informatie [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md).
4. **Er is geen Service-eindpunt**: het subnet moet een Service-eindpunt gekoppeld (opslag of Sql) niet hebben. Zorg ervoor dat Service-eindpunten-optie is uitgeschakeld als u VNet.
5. **Voldoende IP-adressen**: het subnet moet minimaal 16 IP-adressen hebben. Zie voor meer informatie [bepalen van de grootte van het subnet voor exemplaren beheerd](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Het niet mogelijk voor het implementeren van nieuwe instantie worden beheerd als het doelsubnet niet compatibel met alle bovenstaande vereisten is. De bestemming Vnet en het subnet moeten zijn opgeslagen in overeenstemming met deze vereisten exemplaar beheerd (vóór en na de implementatie) inbreuk kan ertoe leiden dat exemplaar voor de defecte status heeft en niet meer beschikbaar. Herstellen van status vereist u nieuw exemplaar in een VNet aan het nalevingsbeleid netwerken maken, serviceniveau-Instantiegegevens opnieuw maken en herstellen van uw databases. Dit introduceert belangrijke uitvaltijd voor uw toepassingen.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>De grootte van subnet bepalen voor exemplaren beheerd

Wanneer u een beheerde exemplaar maakt, wijst Azure een aantal virtuele machines, afhankelijk van de laaggrootte van de die u tijdens het inrichten. Omdat deze virtuele machines gekoppeld aan uw subnet zijn, maar ze nodig hebben IP-adressen. Azure kan extra virtuele machines toewijzen zodat de hoge beschikbaarheid tijdens normale bewerkingen en serviceonderhoud. Hierdoor is het aantal vereiste IP-adressen in een subnet groter dan het aantal exemplaren beheerd in dat subnet. 

Standaard is een exemplaar beheerd moet minimaal 16 IP-adressen in een subnet, en maximaal 256 IP-adressen kunt gebruiken. Als gevolg hiervan kunt u subnetmaskers /28-/24 bij het definiëren van uw subnet IP-adresbereiken. 

Als u wilt implementeren meerdere beheerde exemplaren binnen het subnet en moeten op subnetgrootte optimaliseren, kunt u deze parameters gebruiken om een berekening: 

- Azure gebruikt vijf IP-adressen in het subnet voor de eigen behoeften 
- Elk exemplaar van het algemeen moet twee adressen 

**Voorbeeld**: U van plan bent acht exemplaren beheerd. Dat betekent dat u moet 5 + 8 * 2 = 21 IP-adressen. Als het IP-adresbereiken in macht van 2 zijn gedefinieerd, moet u het IP-adresbereik van 32 (2 ^ 5) IP-adressen. Daarom moet u het subnet met subnetmasker/27 reserveren. 

## <a name="create-a-new-virtual-network-for-managed-instances"></a>Een nieuw virtueel netwerk maken voor beheerde exemplaren 

Maken van een virtuele Azure-netwerk is een vereiste voor het maken van een exemplaar beheerd. U kunt de Azure-portal [PowerShell](../virtual-network/quick-create-powershell.md), of [Azure CLI](../virtual-network/quick-create-cli.md). De volgende sectie bevat de stappen die met de Azure portal. De informatie die hier worden besproken van toepassing op elk van deze methoden.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek en klik op **Virtueel netwerk**, controleer of **Resource Manager** is geselecteerd als implementatiemodel en klik vervolgens op **Maken**.

   ![virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Vul het formulier virtueel netwerk met de gevraagde informatie op een manier zoals de volgende schermafbeelding:

   ![formulier virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Klik op **Create**.

   De adresruimte en het subnet in CIDR-notatie opgegeven. 

   > [!IMPORTANT]
   > De standaardwaarden maken subnet waarmee alle VNet-adresruimte. Als u deze optie kiest, kunt u alle andere resources binnen het virtuele netwerk dan beheerd exemplaar niet maken. 

   De aanbevolen aanpak is het volgende: 
   - Subnetgrootte berekenen door [bepalen van de grootte van het subnet voor beheerde exemplaar](#determine-the-size-of-subnet-for-managed-instances) sectie  
   - Evalueren van de behoeften voor de rest van VNet 
   - Vul in VNet en subnet adresbereiken dienovereenkomstig 

   Zorg ervoor dat de Service-eindpunten blijft optie **uitgeschakelde**. 

   ![formulier virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

## <a name="create-the-required-route-table-and-associate-it"></a>De vereiste routetabel maken en koppelen

1. Aanmelden bij Azure Portal  
2. Zoek en klik op **Routetabel** en klik vervolgens op de pagina Routetabel op **Maken**.

   ![formulier routetabel maken](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Een route van de volgende Hop Internet 0.0.0.0/0 op een manier zoals de volgende schermafbeeldingen maken:

   ![toevoegen aan routetabel](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![route](./media/sql-database-managed-instance-tutorial/route.png)

4. Deze route aan het subnet voor het exemplaar worden beheerd, klikt u op een manier zoals de volgende schermafbeeldingen koppelen:

    ![subnet](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![routetabel instellen](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![instelling routetabel opslaan](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Als uw VNet is gemaakt, bent u klaar om uw beheerde exemplaar te maken.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Een bestaand virtueel netwerk voor beheerde exemplaren wijzigen 

Vragen en antwoorden in deze sectie beschreven hoe u een exemplaar beheerd toevoegen aan bestaande virtuele netwerk. 

**Gebruikt u klassieke of Resource Manager-implementatiemodel voor de bestaande virtuele netwerk?** 

U kunt alleen een exemplaar van beheerd in Resource Manager virtuele netwerken maken. 

**Wilt u de nieuwe subnet maken voor beheerde SQL-exemplaar of bestaande gebruiken?**

Als u maken van nieuwe wilt: 

- Subnetgrootte berekenen door de richtlijnen in de [bepalen van de grootte van het subnet voor exemplaren beheerd](#determine-the-size-of-subnet-for-managed-instances) sectie.
- Volg de stappen in [toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet](../virtual-network/virtual-network-manage-subnet.md). 
- Een routetabel met één vermelding maken **0.0.0.0/0**, zoals de volgende hop van Internet en deze koppelen aan het subnet voor het exemplaar worden beheerd.  

Als u maken van een beheerd exemplaar binnen een bestaande subnet wilt: 
- Als het subnet leeg - is een beheerde-exemplaar kan niet worden gemaakt in een subnet met andere bronnen zoals het gatewaysubnet controleren 
- Subnetgrootte berekenen door de richtlijnen in de [bepalen van de grootte van het subnet voor exemplaren beheerd](#determine-the-size-of-subnet-for-managed-instances) sectie en controleer of de juiste grootte. 
- Controleer dat de service-eindpunten die niet zijn ingeschakeld op het subnet.
- Zorg ervoor dat er geen netwerkbeveiligingsgroepen die zijn gekoppeld aan het subnet 

**Hebt u aangepaste DNS-server geconfigureerd?** 

Zo ja, Zie [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md). 

- De vereiste routetabel maken en koppelen: Zie [de vereiste routetabel maken en koppelen](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is er een exemplaar beheerd](sql-database-managed-instance.md)
- Zie voor een zelfstudie waarin wordt getoond hoe u een VNet maken en databases herstellen vanaf een databaseback-up maken van een exemplaar beheerd, [maakt u een Azure SQL Database beheerd instantie](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor DNS-problemen [een aangepaste DNS configureren](sql-database-managed-instance-custom-dns.md)

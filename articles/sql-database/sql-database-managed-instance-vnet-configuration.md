---
title: Azure SQL Database Managed Instance VNet Configuration | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een virtueel netwerk (VNet) met een Azure SQL Database Managed Instance.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: f634167f24c221e702696174ea86a212c535695b
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42054120"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Een VNet configureren voor het beheerde exemplaar van Azure SQL Database

Azure SQL Database Managed Instance (preview) moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Deze implementatie kunt de volgende scenario's: 
- Verbinding maken met een beheerd exemplaar rechtstreeks vanuit een on-premises netwerk 
- Verbinding te maken van een beheerd exemplaar met gekoppelde server of een andere on-premises gegevensarchief 
- Een beheerd exemplaar verbinden met Azure-resources  

## <a name="plan"></a>Plannen

Plan hoe het implementeren van een beheerd exemplaar in virtueel netwerk met behulp van uw antwoorden op de volgende vragen: 
- Wilt u één of meerdere beheerde exemplaren implementeren? 

  Het aantal beheerde instanties bepaalt de minimale grootte van het subnet toewijzen voor uw beheerde instanties. Zie voor meer informatie, [bepaalt de grootte van het subnet voor Managed Instance](#determine-the-size-of-subnet-for-managed-instances). 
- Heb ik nodig voor het implementeren van uw beheerde exemplaar in een bestaand virtueel netwerk of maakt u een nieuw netwerk? 

   Als u van plan bent een bestaand virtueel netwerk gebruiken, moet u die netwerkconfiguratie om te voldoen aan uw beheerde exemplaar wijzigen. Zie voor meer informatie, [wijzigen bestaand virtueel netwerk voor beheerd exemplaar](#modify-an-existing-virtual-network-for-managed-instances). 

   Als u van plan bent een nieuw virtueel netwerk te maken, Zie [nieuw virtueel netwerk maken voor de Managed Instance](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Vereisten

Voor het maken van een beheerd exemplaar moet u toe te wijzen aan een subnet binnen het VNet dat voldoet aan de volgende vereisten:
- **Niet leeg zijn**: het subnet moet een andere cloudservice die is gekoppeld aan het niet bevatten en mag geen gatewaysubnet. Kunt u zich niet beheerd exemplaar maakt in het subnet met andere resources dan beheerd exemplaar of andere bronnen binnen het subnet later toevoegen.
- **Er is geen NSG**: het subnet moet een Netwerkbeveiligingsgroep die is gekoppeld aan deze niet hebben.
- **Hebt u specifieke routetabel**: het subnet moet een gebruiker Route tabel (UDR) met 0.0.0.0/0 Next Hop Internet hebben als de enige route die is toegewezen. Zie voor meer informatie, [de vereiste routetabel maken en deze koppelen](#create-the-required-route-table-and-associate-it)
3. **Optionele aangepaste DNS**: als aangepaste DNS is opgegeven in het VNet, recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure moet worden toegevoegd aan de lijst. Zie voor meer informatie, [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md).
4. **Er is geen Service-eindpunt**: het subnet mag geen een Service-eindpunt dat is gekoppeld aan (Storage of Sql). Zorg ervoor dat de Service-eindpunten-optie is uitgeschakeld bij het maken van VNet.
5. **Voldoende IP-adressen**: het subnet moet minimaal 16 IP-adressen hebben. Zie voor meer informatie, [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Kunt u zich niet implementeren van nieuwe beheerd exemplaar als het doelsubnet niet compatibel met alle van de bovenstaande vereisten is. De doel-Vnet en het subnet moeten zijn opgeslagen in overeenstemming met deze voorwaarden Managed Instance (vóór en na de implementatie), inbreuk kan ertoe leiden dat exemplaar beschadigde status heeft en niet beschikbaar. Herstellen vanaf dat staat, moet u nieuwe sessie maken in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens opnieuw maken en herstellen van uw databases. Dit introduceert significante downtime voor uw toepassingen.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>De grootte van het subnet voor beheerde instanties bepalen

Wanneer u een beheerd exemplaar maakt, wijst Azure een aantal virtuele machines, afhankelijk van de laaggrootte van de die u tijdens het inrichten. Omdat deze virtuele machines gekoppeld aan het subnet zijn, ze hebben IP-adressen nodig. Voor hoge beschikbaarheid tijdens normale bewerkingen en onderhoud van de service, kan Azure extra virtuele machines toewijzen. Als gevolg hiervan is het aantal vereiste IP-adressen in een subnet groter is dan het aantal beheerde exemplaren in dat subnet. 

Standaard is een beheerd exemplaar moet minimaal 16 IP-adressen in een subnet, en mag maximaal 256 IP-adressen gebruiken. Als gevolg hiervan kunt u subnetmaskers /28-/24 bij het definiëren van uw subnet IP-adresbereiken. 

Als u van plan bent te implementeren van meerdere beheerde exemplaren binnen het subnet en wilt optimaliseren op de grootte van het gatewaysubnet, moet u deze parameters gebruiken om te vormen een berekening: 

- Vijf IP-adressen worden gebruikt in Azure in het subnet voor de eigen behoeften 
- Elk exemplaar algemeen moeten twee adressen 
- Elke bedrijfskritiek exemplaar moeten vier adressen

**Voorbeeld**: U van plan bent de drie algemene en twee bedrijven essentiële beheerde instanties. Dat betekent dat u moet 5 + 3 * 2 + 2 * 4 = 19 IP-adressen. Als het IP-adresbereiken zijn gedefinieerd in de macht van 2, moet u het IP-adresbereik van 32 (2 ^ 5) IP-adressen. Daarom moet u het subnet met het subnetmasker/27 reserveren. 

## <a name="create-a-new-virtual-network-for-managed-instance-using-azure-resource-manager-deployment"></a>Een nieuw virtueel netwerk maken voor beheerd exemplaar met behulp van Azure Resource Manager-implementatie

De eenvoudigste manier om te maken en configureren van virtueel netwerk is met Azure Resource Manager-implementatiesjabloon.

1. Meld u aan bij Azure Portal.

2. Gebruik **implementeren in Azure** knop om virtuele netwerk in Azure-cloud te implementeren:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Deze knop opent u een formulier dat u gebruiken kunt om netwerkomgeving te configureren waarin u Managed Instance kunt implementeren.

  > [!Note]
  > Deze Azure Resource Manager-sjabloon implementeert een virtueel netwerk met twee subnetten. Een subnet met de naam **ManagedInstances** is gereserveerd voor beheerde instanties en heeft routetabel, vooraf geconfigureerd terwijl het andere subnet met de naam **standaard** wordt gebruikt voor andere resources die toegang moeten hebben tot de beheerde Het exemplaar (bijvoorbeeld Azure virtuele Machines). U kunt verwijderen **standaard** subnet als u deze niet nodig.

3. Netwerkomgeving configureren. U kunt parameters van uw netwerkomgeving configureren op de volgende notatie:

![Azure-netwerk configureren](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

U mogelijk de namen wijzigen van het VNet en subnetten en IP-adresbereiken die zijn gekoppeld aan uw netwerkresources aanpassen. Zodra u op "Aankoop" knop drukt, wordt dit formulier maken en configureren van uw omgeving. Als u geen twee subnetten kunt u de standaardwaarde kunt verwijderen. 

## <a name="create-a-new-virtual-network-for-managed-instances-using-portal"></a>Een nieuw virtueel netwerk maken voor beheerde instanties met behulp van portal

Het maken van een Azure-netwerk is een vereiste voor het maken van een beheerd exemplaar. U kunt de Azure-portal [PowerShell](../virtual-network/quick-create-powershell.md), of [Azure CLI](../virtual-network/quick-create-cli.md). De volgende sectie bevat de stappen met behulp van de Azure portal. Details van de hier besproken gelden voor elk van deze methoden.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek en klik op **Virtueel netwerk**, controleer of **Resource Manager** is geselecteerd als implementatiemodel en klik vervolgens op **Maken**.

   ![virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Vul het formulier virtueel netwerk met de aangevraagde informatie, op een manier als in de volgende schermafbeelding:

   ![formulier virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Klik op **Create**.

   De adresruimte en het subnet zijn in CIDR-notatie opgegeven. 

   > [!IMPORTANT]
   > De standaardwaarden maken subnet waarmee alle VNet-adresruimte. Als u deze optie kiest, kunt u alle andere resources binnen het virtuele netwerk dan het beheerde exemplaar niet maken. 

   De aanbevolen aanpak is het volgende: 
   - Grootte van het gatewaysubnet berekenen door [bepaalt de grootte van het subnet voor Managed Instance](#determine-the-size-of-subnet-for-managed-instances) sectie  
   - Beoordeling van de behoeften voor de rest van VNet 
   - Vul in het VNet en subnet-adresbereiken dienovereenkomstig 

   Zorg ervoor dat de Service-eindpunten blijft optie **uitgeschakelde**. 

   ![formulier virtueel netwerk maken](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

### <a name="create-the-required-route-table-and-associate-it"></a>De vereiste routetabel maken en deze koppelen

1. Aanmelden bij Azure Portal  
2. Zoek en klik op **Routetabel** en klik vervolgens op de pagina Routetabel op **Maken**.

   ![formulier routetabel maken](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Een route 0.0.0.0/0 Next Hop Internet op een manier, zoals de volgende schermafbeeldingen maken:

   ![toevoegen aan routetabel](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![route](./media/sql-database-managed-instance-tutorial/route.png)

4. Koppel deze route met het subnet voor het beheerde exemplaar, op een manier, zoals de volgende schermafbeeldingen:

    ![subnet](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![routetabel instellen](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![instelling routetabel opslaan](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Als uw VNet is gemaakt, bent u klaar om te maken van uw beheerde exemplaar.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Een bestaand virtueel netwerk voor beheerde instanties wijzigen 

De vragen en antwoorden in deze sectie laten zien hoe u een beheerd exemplaar toevoegen aan bestaande virtuele netwerk. 

**Gebruikt u klassieke of Resource Manager-implementatiemodel voor de bestaande virtuele netwerk?** 

U kunt alleen een beheerd exemplaar maken in Resource Manager-virtuele netwerken. 

**Wilt u het nieuwe subnet maken voor SQL beheerd exemplaar of gebruik bestaande?**

Als u wilt om nieuwe te maken: 

- Grootte van het gatewaysubnet berekenen door de richtlijnen in de [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances) sectie.
- Volg de stappen in [toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet](../virtual-network/virtual-network-manage-subnet.md). 
- Maak een routetabel met één item, **0.0.0.0/0**, zoals de volgende hop Internet en deze koppelen aan het subnet voor het beheerde exemplaar.  

Als u maken van een beheerd exemplaar in een bestaand subnet wilt: 
- Als het subnet leeg is: een beheerd exemplaar kan niet worden gemaakt in een subnet met andere resources met inbegrip van het gatewaysubnet controleren 
- Grootte van het gatewaysubnet berekenen door de richtlijnen in de [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances) sectie en controleer of de juiste grootte. 
- Controleer dat de service-eindpunten niet zijn ingeschakeld op het subnet.
- Zorg ervoor dat er geen netwerkbeveiligingsgroepen die zijn gekoppeld aan het subnet zijn 

**Hebt u een aangepaste DNS-server geconfigureerd?** 

Zo ja, Zie [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md). 

- De vereiste routetabel maken en deze koppelen: Zie [de vereiste routetabel maken en deze koppelen](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie waarin wordt getoond hoe een VNet maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [het maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor DNS-problemen, [een aangepaste DNS-server configureren](sql-database-managed-instance-custom-dns.md)

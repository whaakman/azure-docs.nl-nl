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
ms.openlocfilehash: b17749999f7903746651403c5948933332dbee5d
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047929"
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
- **Toegewezen subnet**: het subnet moet een andere cloudservice die is gekoppeld aan het niet bevatten en mag geen gatewaysubnet. Kunt u zich niet beheerd exemplaar maakt in het subnet met andere resources dan beheerd exemplaar of andere bronnen binnen het subnet later toevoegen.
- **Er is geen NSG**: het subnet moet een Netwerkbeveiligingsgroep die is gekoppeld aan deze niet hebben. 
- **Hebt u specifieke routetabel**: het subnet moet een gebruiker Route tabel (UDR) met 0.0.0.0/0 Next Hop Internet hebben als de enige route die is toegewezen. Zie voor meer informatie, [de vereiste routetabel maken en deze koppelen](#create-the-required-route-table-and-associate-it)
3. **Optionele aangepaste DNS**: als aangepaste DNS is opgegeven in het VNet, recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure moet worden toegevoegd aan de lijst. Zie voor meer informatie, [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md).
4. **Er is geen Service-eindpunten**: het subnet moet een Service-eindpunt dat is gekoppeld aan deze niet hebben. Zorg ervoor dat de Service-eindpunten-optie is uitgeschakeld bij het maken van VNet.
5. **Voldoende IP-adressen**: het subnet moet de minimumwaarde van 16 IP-adressen (aanbevolen minimum is 32 IP-adressen). Zie voor meer informatie, [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Kunt u zich niet implementeren van nieuwe beheerd exemplaar als het doelsubnet niet compatibel met alle van de bovenstaande vereisten is. De doel-Vnet en het subnet moeten zijn opgeslagen in overeenstemming met deze voorwaarden Managed Instance (vóór en na de implementatie), inbreuk kan ertoe leiden dat exemplaar beschadigde status heeft en niet beschikbaar. Herstellen vanaf dat staat, moet u nieuwe sessie maken in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens opnieuw maken en herstellen van uw databases. Dit introduceert significante downtime voor uw toepassingen.

Met de introductie van _bedoeling netwerkbeleid_, u kunt een netwerkbeveiligingsgroep (NSG) op een Managed Instance-subnet toevoegen nadat het beheerde exemplaar is gemaakt.

U kunt nu een NSG gebruiken om te beperken de IP-adresbereiken waaruit toepassingen en gebruikers kunnen doorzoeken en beheren de gegevens door het filteren van netwerkverkeer dat wordt omgeleid naar poort 1433. 

> [!IMPORTANT]
> Wanneer u de NSG-regels die de toegang tot poort 1433 wordt beperkt configureert, moet u ook de hoogste prioriteit heeft regels voor binnenkomende verbindingen worden weergegeven in de onderstaande tabel invoegen. Anders kunt u lezen wat netwerkbeleid Hiermee blokkeert u de wijziging als niet compatibel zijn.

| NAAM       |PORT                        |PROTOCOL|BRON           |BESTEMMING|ACTIE|
|------------|----------------------------|--------|-----------------|-----------|------|
|beheer  |9000, 9003, 1438, 1440, 1452|Alle     |Alle              |Alle        |Toestaan |
|mi_subnet   |Alle                         |Alle     |MI-SUBNET        |Alle        |Toestaan |
|health_probe|Alle                         |Alle     |AzureLoadBalancer|Alle        |Toestaan |

De routering-ervaring is ook verbeterd zodat naast het 0.0.0.0/0 volgende hoptype Internet route, kunt u nu UDR voor het routeren van verkeer naar uw on-premises privé IP-adresbereiken via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA) toevoegen.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>De grootte van het subnet voor beheerde instanties bepalen

Wanneer u een beheerd exemplaar maakt, wijst Azure een aantal virtuele machines, afhankelijk van de laag die u hebt geselecteerd tijdens het inrichten. Omdat deze virtuele machines gekoppeld aan het subnet zijn, ze hebben IP-adressen nodig. Voor hoge beschikbaarheid tijdens normale bewerkingen en onderhoud van de service, kan Azure extra virtuele machines toewijzen. Als gevolg hiervan is het aantal vereiste IP-adressen in een subnet groter is dan het aantal beheerde exemplaren in dat subnet. 

Standaard is een beheerd exemplaar moet minimaal 16 IP-adressen in een subnet, en mag maximaal 256 IP-adressen gebruiken. Als gevolg hiervan kunt u subnetmaskers /28-/24 bij het definiëren van uw subnet IP-adresbereiken. 

> [!IMPORTANT]
> Grootte van het gatewaysubnet met 16 IP-adressen is de minimumwaarde met beperkte mogelijkheden voor het verder beheerd exemplaar voor scale-out. Kiezen subnet met het voorvoegsel /27 of lager wordt sterk aanbevolen. 

Als u van plan bent te implementeren van meerdere beheerde exemplaren binnen het subnet en wilt optimaliseren op de grootte van het gatewaysubnet, moet u deze parameters gebruiken om te vormen een berekening: 

- Vijf IP-adressen worden gebruikt in Azure in het subnet voor de eigen behoeften 
- Elk exemplaar algemeen moeten twee adressen 
- Elke bedrijfskritiek exemplaar moeten vier adressen

**Voorbeeld**: U van plan bent de drie algemene en twee bedrijven essentiële beheerde instanties. Dat betekent dat u moet 5 + 3 * 2 + 2 * 4 = 19 IP-adressen. Als het IP-adresbereiken zijn gedefinieerd in de macht van 2, moet u het IP-adresbereik van 32 (2 ^ 5) IP-adressen. Daarom moet u het subnet met het subnetmasker/27 reserveren. 

> [!IMPORTANT]
> Berekening die wordt weergegeven boven zijn verouderd met verdere verbeteringen. 

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

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Een bestaand virtueel netwerk voor beheerde instanties wijzigen 

De vragen en antwoorden in deze sectie laten zien hoe u een beheerd exemplaar toevoegen aan bestaande virtuele netwerk. 

**Gebruikt u klassieke of Resource Manager-implementatiemodel voor de bestaande virtuele netwerk?** 

U kunt alleen een beheerd exemplaar maken in Resource Manager-virtuele netwerken. 

**Wilt u het nieuwe subnet maken voor SQL beheerd exemplaar of gebruik bestaande?**

Als u wilt om nieuwe te maken: 

- Grootte van het gatewaysubnet berekenen door de richtlijnen in de [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances) sectie.
- Volg de stappen in [toevoegen, wijzigen of verwijderen van een virtueel netwerksubnet](../virtual-network/virtual-network-manage-subnet.md). 
- Maak een routetabel met één item, **0.0.0.0/0**, zoals de volgende hop Internet en deze koppelen aan het subnet voor het beheerde exemplaar.  

Als u maken van een beheerd exemplaar in een bestaand subnet wilt, raden wij de volgende PowerShell-script voor het voorbereiden van het subnet.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
De voorbereiding van het subnet is gedaan in drie eenvoudige stappen:

- Valideren: de geselecteerde virtuele netwok en het subnet zijn gevalideerd voor beheerd exemplaar netwerkvereisten
- Controleer of - gebruiker een set wijzigingen die moeten worden aangebracht in het subnet voor de Managed Instance-implementatie voorbereiden en wordt gevraagd om toestemming wordt weergegeven
- Voorbereiden: virtueel netwerk en subnet juist zijn geconfigureerd

**Hebt u een aangepaste DNS-server geconfigureerd?** 

Zo ja, Zie [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md). 

- De vereiste routetabel maken en deze koppelen: Zie [de vereiste routetabel maken en deze koppelen](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie waarin wordt getoond hoe een VNet maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [het maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor DNS-problemen, [een aangepaste DNS-server configureren](sql-database-managed-instance-custom-dns.md)

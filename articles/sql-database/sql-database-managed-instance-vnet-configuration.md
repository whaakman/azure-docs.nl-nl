---
title: Azure SQL Database Managed Instance VNet Configuration | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een virtueel netwerk (VNet) met een Azure SQL Database Managed Instance.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 559859e1f844cae37b0baaf48b0ad7c5cc2d4b0a
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542428"
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

Voor het maken van een beheerd exemplaar, maakt u een toegewezen subnet (de Managed Instance-subnet) binnen het virtuele netwerk dat aan de volgende vereisten voldoet:
- **Toegewezen subnet**: de Managed Instance-subnet mag geen andere cloud-services die zijn gekoppeld aan deze bevatten en moet niet een gatewaysubnet. Kunt u zich niet aan een beheerd exemplaar maakt in een subnet met andere resources dan Managed Instance en u de andere resources in het subnet niet op de later kunt toevoegen.
- **Compatibel Netwerkbeveiligingsgroep (NSG)**: een NSG die is gekoppeld aan een Managed Instance-subnet moet de regels die wordt weergegeven in de volgende tabellen (verplichte beveiligingsregels voor binnenkomend verkeer en verplichte uitgaande beveiligingsregels) voor alle andere regels bevatten. U kunt een NSG gebruiken voor het beheren van toegang tot het eindpunt van de gegevens Managed Instance volledig door te filteren van het verkeer op poort 1433. 
- **Tabel compatibel is door de gebruiker gedefinieerde route (UDR)**: beheerd exemplaar van het subnet moet de routetabel van een gebruiker met **0.0.0.0/0 Next Hop Internet** als de verplichte UDR zijn toegewezen. Bovendien kunt u een UDR dat verkeer van routes met on-premises privé IP-adresbereiken als een doel via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA) toevoegen. 
- **Optionele aangepaste DNS**: als een aangepaste DNS-server is opgegeven op thevirtual netword, recursieve naamomzetting IP-adres (zoals 168.63.129.16) van Azure moet worden toegevoegd aan de lijst. Zie voor meer informatie, [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md). De aangepaste DNS-server moeten kunnen omzetten van hostnamen in de volgende domeinen bevinden en hun subdomeinen: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, en *microsoftonline-p.com*. 
- **Er is geen service-eindpunten**: beheerd exemplaar van het subnet moet een service-eindpunt dat is gekoppeld aan deze niet hebben. Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- **Voldoende IP-adressen**: beheerd exemplaar van het subnet moet de minimumwaarde van 16 IP-adressen (aanbevolen minimum is 32 IP-adressen). Zie voor meer informatie, [bepaalt de grootte van het subnet voor beheerde instanties](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Kunt u zich niet aan het implementeren van een nieuwe beheerd exemplaar als het doelsubnet niet compatibel met alle van de volgende vereisten is. Als u een beheerd exemplaar maakt, een *bedoeling netwerkbeleid* wordt toegepast op het subnet om te voorkomen dat niet-compatibele wijzigingen aan de configuratie van netwerken. Nadat het laatste exemplaar is verwijderd uit het subnet, de *bedoeling netwerkbeleid* ook is verwijderd

### <a name="mandatory-inbound-security-rules"></a>Verplichte beveiligingsregels voor binnenkomend verkeer 

| NAAM       |PORT                        |PROTOCOL|BRON           |BESTEMMING|ACTIE|
|------------|----------------------------|--------|-----------------|-----------|------|
|beheer  |9000, 9003, 1438, 1440, 1452|TCP     |Alle              |Alle        |Toestaan |
|mi_subnet   |Alle                         |Alle     |MI-SUBNET        |Alle        |Toestaan |
|health_probe|Alle                         |Alle     |AzureLoadBalancer|Alle        |Toestaan |

### <a name="mandatory-outbound-security-rules"></a>Verplichte uitgaande beveiligingsregels 

| NAAM       |PORT          |PROTOCOL|BRON           |BESTEMMING|ACTIE|
|------------|--------------|--------|-----------------|-----------|------|
|beheer  |80, 443, 12000|TCP     |Alle              |Alle        |Toestaan |
|mi_subnet   |Alle           |Alle     |Alle              |MI-SUBNET  |Toestaan |

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

![Azure-netwerk configureren](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

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
- Zie voor een zelfstudie waarin wordt getoond hoe een VNet maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [het maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [een aangepaste DNS-server configureren](sql-database-managed-instance-custom-dns.md)

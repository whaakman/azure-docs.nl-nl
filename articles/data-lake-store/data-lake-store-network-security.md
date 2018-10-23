---
title: Netwerkbeveiliging in Azure Data Lake Storage Gen1 | Microsoft Docs
description: Begrijpen hoe de IP-firewall en integratie van virtueel netwerk werken in Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 0da5962bc0b48a387ee82a1db36099682e14bca3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168065"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1---preview"></a>Integratie van virtueel netwerk voor Azure Data Lake Storage Gen1 - Preview

Introductie van integratie van virtueel netwerk voor Azure Data Lake Storage Gen1 (preview-versie). Met VNet-integratie kunt u niet-gemachtigde toegang tot Azure Data Lake Storage Gen1-accounts voorkomen door deze accounts te vergrendelen voor specifieke virtuele netwerken en subnetten. U kunt nu het ADLS Gen1-account configureren om alleen verkeer te accepteren uit de toegewezen virtuele netwerken en subnetten, en om toegang vanuit alle andere locaties te blokkeren. Dit helpt om het ADLS-account te beveiligen tegen externe bedreigingen.

VNet-integratie voor ADLS Gen1 maakt gebruik van de service-eindpuntbeveiliging van het virtuele netwerk tussen de Virtual Network- en Azure Active Directory-services om extra beveiligingsclaims te genereren in het toegangstoken. Deze claims worden vervolgens gebruikt om het virtuele netwerk te verifiëren bij het ADLS Gen1-account en toegang toe te staan.

> [!NOTE]
> Dit is een preview-technologie en we raden u aan om deze niet te gebruiken in productieomgevingen.
>
> Er zijn geen extra kosten gemoeid met het gebruik van deze mogelijkheden. Het standaardtarief wordt in rekening gebracht voor ADLS Gen1 [(prijzen](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable)) en alle Azure-services die u gebruikt ([prijzen](https://azure.microsoft.com/pricing/#product-picker)).

## <a name="scenarios-for-vnet-integration-for-adls-gen1"></a>Scenario's voor VNET-integratie voor ADLS Gen1

Met VNET-integratie voor ADLS Gen1 kunt u de toegang tot het ADLS Gen1-account vanuit toegewezen virtuele netwerken en subnetten beperken.  Andere VNet’s/VM’s in Azure hebben geen toegang tot het account zodra het is vergrendeld voor het opgegeven VNet-subnet.  Functioneel biedt VNET-integratie voor ADLS Gen1 hetzelfde scenario als [service-eindpunten voor virtuele netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview).  In de sectie hieronder worden een paar belangrijke verschillen gedetailleerd besproken. 

![Scenariodiagram voor VNet-integratie voor ADLS Gen1](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> De bestaande IP-firewallregels kunnen worden gebruikt naast de VNet-regels om ook toegang vanaf on-premises netwerken toe te staan. 

## <a name="optimal-routing-with-adls-gen1-vnet-integration"></a>Optimale routering met VNet-integratie voor ADLS Gen1

Een belangrijk voordeel van service-eindpunten voor VNet is [optimale routering](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) vanuit het virtuele netwerk.  Als u dezelfde optimalisering van routes wilt toepassen op ADLS Gen1-accounts, gebruikt u de volgende [door de gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) van het VNet naar het ADLS Gen1-account:

- **Openbaar IP-adres van ADLS**: gebruik het IP-adres voor de ADLS Gen1-doelaccounts.  U kunt de IP-adressen voor het ADLS Gen1-account identificeren door [de DNS-namen van de accounts](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity) om te zetten.  Maak een afzonderlijke vermelding voor elk adres.

```azurecli
# Create a Route table for your resource group
az network route-table create --resource-group $RgName --name $RouteTableName

# Create Route Table Rules for ADLS Public IP Addresses
# There will be one rule per ADLS Public IP Addresses 
az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet

# Update the VNet and apply the newly created Route Table to it
az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
```

## <a name="data-exfiltration-from-the-customer-vnet"></a>Gegevensoverdracht vanuit het klanten-VNet

Naast het beveiligen van de toegang tot ADLS-accounts vanuit Virtual Network wilt u wellicht ook voorkomen dat gegevens worden overgedragen naar een niet-gemachtigd account.

Onze aanbeveling is om een firewall te gebruiken in het VNet om uitgaand verkeer te filteren op basis van de URL van het doelaccount, en om alleen toegang toe te staan aan gemachtigde ADLS Gen1-accounts.

Een aantal beschikbare opties zijn:
- [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview): u kunt [een Azure-firewall implementeren en configureren](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) voor het VNet, en uitgaand ADLS-verkeer beveiligen en beperken tot de bekende en gemachtigde account-URL.
- Firewall voor [virtueel-netwerkapparaat](https://azure.microsoft.com/solutions/network-appliances/): als de beheerder alleen het gebruik van bepaalde commerciële firewall-leveranciers toestaat, kunt u een van de NVA-firewalls gebruiken die beschikbaar zijn in Azure Marketplace, om dezelfde taak uit te voeren.

> [!NOTE]
> Door firewalls te gebruiken in het gegevenspad, wordt een extra hop in het gegevenspad geïntroduceerd. Dit kan de netwerkprestaties beïnvloeden voor end-to-end-gegevensuitwisseling, inclusief beschikbare doorvoer en verbindingslatentie. 

## <a name="limitations"></a>Beperkingen
1.  HDInsight-clusters moeten opnieuw worden gemaakt nadat ze zijn toegevoegd aan de preview-versie.  Clusters die zijn gemaakt vóórdat VNet-integratie voor ADLS Gen1 beschikbaar was, moeten opnieuw worden gemaakt om ondersteuning te bieden voor deze nieuwe functie. 
2.  Als u bij het maken van een nieuw HDInsight-cluster een ADLS Gen1-account selecteert waarvoor VNet-integratie is ingeschakeld, mislukt het proces. U moet de VNet-regel eerst uitschakelen of u kunt **Toegang vanuit alle netwerken en services toestaan** via de blade **Firewall en virtuele netwerken** van het ADLS-account.  Raadpleeg de sectie [Uitzonderingen](##Exceptions) voor meer informatie.
3.  De preview-versie van VNET-integratie voor ADLS Gen1 werkt niet met [beheerde identiteiten voor Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
4.  Bestands-/mapgegevens in het ADLS Gen1-account met VNet zijn niet toegankelijk vanuit de portal.  Dit omvat toegang vanuit een VM in het VNET en activiteiten zoals het gebruik van Data Explorer.  Accountbeheeractiviteiten blijven werken.  Bestands-/mapgegevens in het ADLS-account met VNet zijn toegankelijk via alle niet-portalresources: SDK-toegang, PowerShell-scripts, andere Azure-services (die niet afkomstig zijn uit de portal), enzovoort... 

## <a name="configuration"></a>Configuratie

### <a name="step1-configure-your-vnet-to-use-aad-service-endpoint"></a>Stap 1: Het VNET configureren voor het gebruik van een AAD-service-eindpunt
1.  Ga naar de Azure-portal en meld u aan bij uw account. 
2.  [Maak een nieuw virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in uw abonnement of ga naar een bestaand virtueel netwerk.  Het VNET moet zich op dit moment in dezelfde regio bevinden als het ADLS Gen 1-account. 
3.  Kies op de blade Virtueel netwerk de optie **Service-eindpunten**. 
4.  Klik op **Toevoegen** om een nieuw service-eindpunt toe te voegen.
![Een VNet-service-eindpunt toevoegen](media/data-lake-store-network-security/config-vnet-1.png)
5.  Kies **Microsoft.AzureActiveDirectory** als service voor het eindpunt.
![Het service-eindpunt Microsoft.AzureActiveDirectory selecteren](media/data-lake-store-network-security/config-vnet-2.png)
6.  Kies de subnetten waarvoor u connectiviteit wilt toestaan, en klik op **Toevoegen**.
![Het subnet selecteren](media/data-lake-store-network-security/config-vnet-3.png)
7.  Het duurt maximaal 15 minuten voordat het service-eindpunt is toegevoegd. Zodra het eindpunt is toegevoegd, wordt het weergegeven in de lijst. Controleer of het inderdaad wordt weergegeven en of alle configuratiedetails kloppen. 
![Geslaagde toevoeging van het service-eindpunt](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-vnetsubnet-for-your-adls-gen1-account"></a>Stap2: Het toegestane VNET/Subnet instellen voor het ADLS Gen1-account
1.  Nadat u het VNET hebt geconfigureerd, [maakt u een nieuw Azure Data Lake Storage Gen1-account](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) in uw abonnement of gaat u naar een bestaand ADLS Gen1-account. Het ADLS Gen1-account moet zich op dit moment in dezelfde regio bevinden als het VNET. 
2.  Kies **Firewall en virtuele netwerken**.

  > [!NOTE]
  > Als u **Firewall en virtuele netwerken** niet ziet in de instellingen, meldt u zich af bij de portal. Sluit de browser. Wis de browsercache. Start de computer opnieuw op en probeer het nog een keer.

  ![Een VNet-regel toevoegen aan het ADLS-account](media/data-lake-store-network-security/config-adls-1.png)
3.  Kies **Geselecteerde netwerken**. 
4.  Klik op **Bestaand virtueel netwerk toevoegen**.
  ![Bestaand virtueel netwerk toevoegen](media/data-lake-store-network-security/config-adls-2.png)
5.  Kies de VNet’s en subnetten om connectiviteit toe te staan. Klik vervolgens op **Toevoegen**.
  ![Het VNet en de subnetten kiezen](media/data-lake-store-network-security/config-adls-3.png)
6.  Zorg ervoor dat de VNet’s en subnetten juist worden weergegeven in de lijst en kies **Opslaan**.
  ![De nieuwe regel opslaan](media/data-lake-store-network-security/config-adls-4.png)

  > [!NOTE]
  > Na het opslaan duurt het maximaal 5 minuten voordat de instellingen zijn doorgevoerd.

7.  [Optioneel] Als u naast VNet’s en subnetten connectiviteit vanaf specifieke IP-adressen wilt toestaan, kunt u dit doen in de sectie **Firewall** op dezelfde pagina. 

## <a name="exceptions"></a>Uitzonderingen
Er zijn twee selectievakjes in het gebied Uitzonderingen op de blade **Firewall en virtuele netwerken** waarmee de connectiviteit kan worden ingeschakeld vanuit een set services en virtuele machines in Azure.
![Uitzonderingen: Firewall en virtuele netwerken](media/data-lake-store-network-security/firewall-exceptions.png)
- **Toestaan dat alle Azure-services toegang hebben tot deze Data Lake Storage Gen1-account** maakt het voor alle Azure-services (zoals Azure Data Factory, Event Hubs, alle Azure-VM’s, enzovoort)... mogelijk om te communiceren met het ADLS-account.

- Door **Azure Data Lake Analytics toegang te verlenen tot dit Data Lake Storage Gen1-account** wordt connectiviteit van de Azure Data Lake Analytics-service met dit ADLS-account mogelijk. 

We raden u aan deze uitzonderingen uitgeschakeld te houden en alleen in te schakelen als u connectiviteit nodig hebt van deze andere services buiten het VNet.

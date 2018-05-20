---
title: Hybride cloud-verbindingen configureren met Azure en Azure Stack | Microsoft Docs
description: Informatie over het configureren van hybride cloud-verbindingen met Azure en Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 048e2636aabe406728c8fe1b93ef861f13346256
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Zelfstudie: hybride cloud-verbindingen configureren met Azure en Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt toegang tot bronnen met beveiliging in globale Azure en Azure-Stack met behulp van het patroon van hybride verbindingen. 

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Bewaren van gegevens on-premises voor privacy- of regelgeving, maar hebben toegang tot globale Azure-resources.
> - Een verouderde systeem tijdens het gebruik van cloud-geschaalde app-implementatie en resources in Azure met globale onderhouden.

## <a name="prerequisites"></a>Vereisten

Enkele onderdelen zijn vereist voor het bouwen van een hybride implementatie van de connectiviteit en kunnen even duren om voor te bereiden. 

Een Azure OEM/Hardware Partner kan een productie-Azure-Stack implementeren en alle gebruikers kunnen een ASDK implementeren. Een Azure-Stack-Operator moet ook de App Service implementeren, plannen en aanbiedingen maken, een tenantabonnement maken en de Windows Server 2016-installatiekopie toe te voegen. 

Als u al beschikt over sommige van deze onderdelen, zorg er dan voor dat ze voldoen aan de vereisten voor het begin.

In dit onderwerp wordt ervan uitgegaan dat u enige kennis van Azure en Azure-Stack hebben. Als u weten voordat u doorgaat wilt, moet beginnen met de volgende onderwerpen:
 - [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Belangrijkste concepten van Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure
 - Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
 - Maak een [Web-App](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) in Azure. Noteer de nieuwe Web-App-URL, zoals het wordt later gebruikt.

### <a name="azure-stack"></a>Azure Stack
 - Gebruik uw productie-Azure-Stack of implementeren Azure Stack Development Kit gekoppeld hieronder:
    - https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 
    - De installatie duurt normaal gesproken een paar uur moet zijn voltooid, dus plan dienovereenkomstig.
 - Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services naar Azure-Stack. 
 - [Plan/aanbiedingen maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) binnen de Azure-Stack-omgeving. 
 - [Tenantabonnement maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure-Stack-omgeving. 


### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u aan de volgende criteria hebt voldaan:

 - Controleer of u een extern gericht openbaar IPv4-adres voor het VPN-apparaat hebt. Dit IP-adres kan zich niet achter een NAT bevinden.
 - Zorg ervoor dat alle resources worden geïmplementeerd in dezelfde regio/locatie.

#### <a name="example-values"></a>Voorbeeldwaarden

In de voorbeelden in dit artikel worden de volgende waarden gebruikt. U kunt deze waarden gebruiken voor het maken van een testomgeving of ernaar verwijzen beter inzicht in de voorbeelden in dit artikel. Zie [Over instellingen voor VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings) voor meer informatie over instellingen voor VPN-gateways in het algemeen.

Specificaties voor verbinding:
 - **VPN-type**: op Route gebaseerd
 - **Verbindingstype**: Site-naar-site (IPsec)
 - **Gatewaytype**: VPN
 - **Azure verbindingsnaam**: Azure-Gateway-AzureStack-S2SGateway (wordt de portal automatisch doorvoeren deze waarde)
 - **Azure Stack verbindingsnaam**: AzureStack-Gateway-Azure-S2SGateway (wordt de portal automatisch doorvoeren deze waarde)
 - **Gedeelde sleutel**: een compatibel is met VPN-hardware, met overeenkomende waarden aan beide zijden van de verbinding
 - **Abonnement**: een abonnement bij voorkeur
 - **Resourcegroep**: Test-Infra

| Azure/Azure Stack-verbinding | Naam | Subnet | IP-adres |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure vNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack-vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure virtuele netwerkgateway | Azure-Gateway |  |  |
| De virtuele netwerkgateway Azure Stack | AzureStack-Gateway |  |  |
| Azure openbare IP-adres | Azure-GatewayPublicIP |  | Bij het maken van bepaald |
| Azure Stack openbare IP-adres | AzureStack GatewayPublicIP |  | Bij het maken van bepaald |
| Azure lokale netwerkgateway | AzureStack S2SGateway<br>   10.100.100.0/23 |  | Azure Stack openbare IP-waarde |
| De lokale netwerkgateway Azure Stack | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure openbaar IP-waarde |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Een virtueel netwerk maken in globale Azure en Azure-Stack

> [!Note]  
> U moet ervoor zorgen dat er geen overlapping van IP-adressen in Azure of Stack Azure vNet-adresruimten is. 

Een vNet maken in het Resource Manager-implementatiemodel met behulp van de Azure-portal. Gebruik de [voorbeeldwaarden](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) als u deze stappen uitvoert als onderdeel van een zelfstudie. Als u deze stappen niet uitvoert als onderdeel van een zelfstudie, vervangt u de waarden door die van uzelf. 

1. Open een browser, ga naar [Azure Portal](http://portal.azure.com/) en meld u aan met uw Azure-account.
2. Klik op **Een resource maken**. In de **zoeken van de marketplace** veld `virtual network`''. Zoek **virtueel netwerk** in de geretourneerde lijst en open de **virtueel netwerk** pagina.
3. In de buurt van de onderkant van de pagina virtuele netwerk van de **een implementatiemodel selecteren** Selecteer **Resource Manager**, en selecteer vervolgens **maken**. Hiermee opent u de pagina Virtueel netwerk maken.
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn.
5. Herhaal deze stappen uit de **tenantportal** van Azure-Stack.

## <a name="add-a-gateway-subnet"></a>Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u eerst het gatewaysubnet maken voor het virtuele netwerk waarmee u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen opgegeven in het gatewaysubnet.

Navigeer in de [portal](http://portal.azure.com/) naar het virtuele netwerk van Resource Manager waarvoor u een gateway wilt maken.

1. In de **instellingen** gedeelte van uw VNet pagina **subnetten** uit te breiden de **subnetten** pagina.
2. Op de **subnetten** pagina **+ gatewaysubnet** openen de **subnet toevoegen** pagina.

    ![](media/azure-stack-solution-hybrid-connectivity/image4.png)

3. Als **naam** voor het subnet wordt automatisch de waarde GatewaySubnet ingevuld. Deze waarde is vereist voor Azure voor het herkennen van het subnet als het gatewaysubnet. Aanpassen automatisch gevuld **-adresbereik** waarden om te voldoen aan uw configuratie, selecteer vervolgens **OK** aan de onderkant van de pagina om het subnet te maken.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>De Gateway van een virtueel netwerk maken in Azure en Azure Stack

1. Selecteer aan de linkerkant van de portal-pagina + en voer 'Virtuele netwerkgateway' in de zoekopdracht. In **resultaten**, zoek en selecteer **virtuele netwerkgateway**.
2. Aan de onderkant van de **virtuele netwerkgateway** pagina **maken**. Hiermee opent u de pagina **Gateway van het virtuele netwerk maken**.
3. Op de **virtuele netwerkgateway aanmaken** pagina, geeft u de waarden voor uw virtuele netwerkgateway, zoals beschreven in de voorbeelden van waarden plus de aanvullende waarden zoals hieronder beschreven.
    - **SKU**: basic
    - **Virtueel netwerk**: Selecteer het virtuele netwerk dat u eerder hebt gemaakt. Deze selecteert automatisch het gatewaysubnet dat u eerder hebt gemaakt. 
    - **Eerste IP-configuratie**: dit is het openbare IP-adres van uw Gateway. 
        - Klik op de **configuratie van IP-gateway maken** en dit gaat u naar de **openbare IP-adres kiezen** pagina.
        - Klik op **+ nieuw** openen de **openbare IP-adres maken** pagina.
        - Voer een **naam** voor uw openbare IP-adres. Laat de SKU als **Basic**, selecteer daarna **OK** onder aan deze pagina uw wijzigingen op te slaan.

    > [!Note]  
    > VPN-Gateway ondersteunt momenteel alleen dynamische openbare IP-adrestoewijzing. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

4. Controleer de instellingen. 
5. Klik op **Maken** om de VPN-gateway aan te maken. De instellingen worden gevalideerd en op het dashboard wordt de tegel 'De gateway van het virtuele netwerk implementeren' weergegeven. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

    Nadat de gateway is gemaakt, gaat u na welk IP-adres eraan is toegewezen. Bekijk hiervoor het virtuele netwerk in de portal. De gateway wordt weergegeven als verbonden apparaat. U kunt het aangesloten apparaat (uw virtuele netwerkgateway) voor meer informatie.
6. Herhaal deze stappen op uw Azure-Stack-implementatie.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>De lokale netwerkgateway maken in Azure en Azure-Stack

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. Geeft u de site een naam waarmee Azure of Azure-Stack kan hiernaar wordt verwezen, geeft u vervolgens het IP-adres van de on-premises VPN-apparaat waarop u een verbinding maakt. U geeft ook de IP-adresvoorvoegsels op die via de VPN-gateway worden doorgestuurd naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden. Als uw on-premises netwerk verandert of als u het openbare IP-adres voor het VPN-apparaat moet wijzigen, kunt u de waarden later eenvoudig bijwerken.

1. Selecteer in de portal **+ maken van een resource**.
2. Voer in het zoekvak **lokale netwerkgateway**, drukt u vervolgens op **Enter** om te zoeken. Er wordt dan een lijst met resultaten geretourneerd. Klik op **lokale netwerkgateway**, selecteer vervolgens de **maken** te openen de **de lokale netwerkgateway maken** pagina.
3. Op de **pagina voor gateway het lokale netwerk maken**, geef de waarden voor de gateway van uw lokale netwerk, zoals beschreven in onze voorbeelden van waarden plus de aanvullende waarden zoals hieronder beschreven.
    - **IP-adres**: dit is het openbare IP-adres van het VPN-apparaat dat u wilt verbinding maken met Azure of Azure-Stack. Geef een geldig openbaar IP-adres op. Het IP-adres mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure. Als u op dit moment niet beschikt over het IP-adres, kunt u de waarden uit de schermafbeelding gebruiken. U moet dan wel later terugkeren om uw tijdelijke IP-adres te vervangen door het openbare IP-adres van uw VPN-apparaat. Anders kan er geen verbinding worden gemaakt vanuit Azure.
    - **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft, niet overlappen met bereiken van andere netwerken waarmee u verbinding wilt maken. Azure stuurt het adresbereik dat u opgeeft, door naar het IP-adres van het on-premises VPN-apparaat. Gebruik uw eigen waarden hier als u wilt verbinding maken met uw on-premises site, niet de waarden die worden weergegeven in het voorbeeld.
    - **BGP-instellingen configureren**: alleen bij het configureren van BGP gebruiken. Selecteer deze anders niet.
    - **Abonnement**: Controleer of het juiste abonnement zichtbaar is.
    - **Resourcegroep**: Selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
    - **Locatie**: Selecteer de locatie die dit object wordt gemaakt in. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.
4. Wanneer u klaar bent met de waarden opgeven, selecteert u de **maken** knop aan de onderkant van de pagina voor het maken van de lokale netwerkgateway.
5. Herhaal deze stappen op uw Azure-Stack-implementatie.

## <a name="configure-your-connection"></a>Configureer uw verbinding

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. In deze stap configureert u uw VPN-apparaat, een verbinding genoemd. Wanneer u uw verbinding configureert, moet u het volgende:
    - Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
    - Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI. Als u wilt zoeken naar het openbare IP-adres van uw VPN-gateway met de Azure portal, gaat u naar de gateways voor virtueel netwerk en selecteer vervolgens de naam van uw gateway.
Maak de site-naar-site-VPN-verbinding tussen de gateway van uw virtuele netwerk en het on-premises VPN-apparaat.
1. Selecteer in de portal **+ maken van een resource**.
2. Voer in het zoekvak **verbindingen**, drukt u vervolgens op **Enter** om te zoeken. Er wordt dan een lijst met resultaten geretourneerd. Klik op **verbindingen**, selecteer vervolgens de knop openen de **verbindingen maken** pagina.
3. Op de **verbindingen maken** pagina, configureer de waarden voor de verbinding.
     - Grondbeginselen van:
        1. **Verbindingstype**: Selecteer de Site-naar-site (IPSec).
        2. **Resourcegroep**: (Selecteer uw testgroep resource)
     - Instellingen:
        1. **Virtuele netwerkgateway**: Selecteer de virtuele netwerkgateway die u eerder hebt gemaakt.
        2. **Lokale netwerkgateway**: Selecteer de lokale netwerkgateway die u eerder hebt gemaakt. 
        3. **Verbindingsnaam**: dit automatisch te vullen met de waarden van de twee gateways. 
        4. **Gedeelde sleutel**: de hier gebruikte waarde moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. In het voorbeeld wordt 'abc123' gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
    - De resterende waarden voor **Abonnement**, **Resourcegroep**, en **Locatie** zijn vast.
4. Klik op **OK** om uw verbinding te maken. Hier ziet u verbinding maken flash op het scherm.
5. Vindt u de verbinding in de ** verbindingen pagina van de virtuele netwerkgateway. De status wordt verder **onbekende** naar **verbinding maken**, en vervolgens naar **geslaagd**.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

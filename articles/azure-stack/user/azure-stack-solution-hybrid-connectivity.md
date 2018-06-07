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
ms.date: 05/25/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 72c5c4b0f0ab752bb02e6bee7cd038afca44ee1b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605192"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Zelfstudie: hybride cloud-verbindingen configureren met Azure en Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt toegang tot bronnen met beveiliging in globale Azure en Azure-Stack met behulp van het patroon van hybride verbindingen.

In deze zelfstudie maakt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Bewaren van gegevens die lokaal om te voldoen aan de privacy- of regelgeving, maar hebben toegang tot globale Azure-resources.
> - Een verouderde systeem tijdens het gebruik van cloud-geschaalde app-implementatie en resources in Azure met globale onderhouden.

## <a name="prerequisites"></a>Vereisten

Enkele onderdelen zijn vereist voor het bouwen van een implementatie van hybride verbindingen. Sommige van deze onderdelen kan even duren om voor te bereiden, zodat u zult moeten dienovereenkomstig plannen.

**Azure Stack**

Een Azure OEM/Hardware Partner een productie-Stack van Azure kunt implementeren en alle gebruikers een Azure Stack Development Kit (ASDK) kunnen implementeren.

**Azure Stack-onderdelen**

Een Azure-Stack-Operator moet de App Service implementeren, plannen en aanbiedingen maken, een tenantabonnement maken en de Windows Server 2016-installatiekopie toe te voegen. Als u al beschikt over sommige van deze onderdelen, zorg er dan voor dat ze voldoen aan de vereisten voordat u deze zelfstudie.

Deze zelfstudie wordt ervan uitgegaan dat u enige basiskennis hebben van Azure en Azure-Stack. Voor meer informatie voordat u de zelfstudie begint, leest u de volgende artikelen:

 - [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Belangrijkste concepten van Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
 - Maak een [Web-App](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) in Azure. Noteer de Web-App-URL, omdat u hebt deze in de zelfstudie nodig.

### <a name="azure-stack"></a>Azure Stack

 - Uw productie-Azure-Stack gebruiken of implementeren van de Azure-Stack Development Kit van https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >Implementatie van de ASDK kunt 7 uur duren, dus plan dienovereenkomstig.

 - Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services naar Azure-Stack.
 - [Plannen en aanbiedingen maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in de Azure-Stack-omgeving.
 - [Tenantabonnement maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure-Stack-omgeving.

### <a name="before-you-begin"></a>Voordat u begint

Controleer of dat u de volgende criteria voldoet aan voordat u begint met het configureren van hybride cloud-verbindingen:

 - In dat geval moet u een extern gericht openbaar IPv4-adres in voor uw VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
 - Alle resources worden geïmplementeerd in dezelfde regio/locatie.

#### <a name="tutorial-example-values"></a>Zelfstudie voorbeeldwaarden

De voorbeelden in deze zelfstudie gebruikt de volgende waarden. U kunt deze waarden gebruiken voor het maken van een testomgeving of Raadpleeg voor een beter inzicht in de voorbeelden. Zie [Over instellingen voor VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings) voor meer informatie over instellingen voor VPN-gateways in het algemeen.

Specificaties voor verbinding:

 - **VPN-type**: op Route gebaseerd
 - **Verbindingstype**: Site-naar-site (IPsec)
 - **Gatewaytype**: VPN
 - **Azure verbindingsnaam**: Azure-Gateway-AzureStack-S2SGateway (wordt de portal automatisch doorvoeren deze waarde)
 - **Azure Stack verbindingsnaam**: AzureStack-Gateway-Azure-S2SGateway (wordt de portal automatisch doorvoeren deze waarde)
 - **Gedeelde sleutel**: een compatibel is met VPN-hardware, met overeenkomende waarden aan beide zijden van de verbinding
 - **Abonnement**: een abonnement bij voorkeur
 - **Resourcegroep**: Test-Infra

IP-adressen, netwerk en subnet:

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

Gebruik de volgende stappen voor het maken van een virtueel netwerk met behulp van de portal. U kunt deze gebruiken [voorbeeldwaarden](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) als u dit artikel als een zelfstudie. Maar als u dit artikel voor het configureren van een productieomgeving, vervangt u de voorbeeldinstellingen met uw eigen waarden.

> [!IMPORTANT]
> U moet ervoor zorgen dat er geen IP-adressen in Azure of Stack Azure vNet adresruimten overlappen.

Een vNet maken in Azure:

1. Uw browser gebruiken voor verbinding met de [Azure-portal](http://portal.azure.com/) en meld u aan met uw Azure-account.
2. Selecteer **maken van een resource**. In de **zoeken van de marketplace** veld `virtual network`''. Zoeken naar **virtueel netwerk** in de lijst met resultaten en selecteer vervolgens **virtueel netwerk**.
3. Van de **een implementatiemodel selecteren** Selecteer **Resource Manager**, en selecteer vervolgens **maken**.
4. Op **virtueel netwerk maken**, configureer de VNet-instellingen. De namen van de vereiste velden worden voorafgegaan door een rode asterisk.  Wanneer u een geldige waarde invoert, wordt het sterretje verandert in een groen vinkje.

Een vNet maken in Azure-Stack:

* Herhaal de vorige stappen (1-4) met behulp van de Azure-Stack **tenantportal**.

## <a name="add-a-gateway-subnet"></a>Een gatewaysubnet toevoegen

Voordat de verbinding met het maken van het virtuele netwerk met een gateway, moet u voor het maken van het gatewaysubnet voor het virtuele netwerk dat u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen die u in het gatewaysubnet opgeeft.

In de [Azure-portal](http://portal.azure.com/), gaat u naar het virtuele netwerk waar u wilt maken van een virtuele netwerkgateway van Resource Manager.

1. Selecteer het vNet openen de **virtueel netwerk** pagina.
2. In **instellingen**, selecteer **subnetten**.
3. Op de **subnetten** pagina **+ gatewaysubnet** openen de **subnet toevoegen** pagina.

    ![Gatewaysubnet toevoegen](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. De **naam** voor het subnet wordt automatisch gevuld met de waarde 'GatewaySubnet'. Deze waarde is vereist voor Azure voor het herkennen van het subnet als het gatewaysubnet.
5. Wijzig de **-adresbereik** waarden die zijn opgegeven om te voldoen aan uw configuratie en selecteer vervolgens **OK**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>De Gateway van een virtueel netwerk maken in Azure en Azure Stack

Gebruik de volgende stappen voor het maken van een virtuele netwerkgateway in Azure.

1. Selecteer aan de linkerkant van de portal-pagina **+** en voer de gateway van virtueel netwerk in het zoekveld.
2. In **resultaten**, selecteer **virtuele netwerkgateway**.
3. In **virtuele netwerkgateway**, selecteer **maken** openen de **virtuele netwerkgateway aanmaken** pagina.
4. Op **virtuele netwerkgateway aanmaken**, geef de waarden voor de gateway van uw netwerk, zoals wordt weergegeven in **zelfstudie voorbeeldwaarden**, en de volgende aanvullende waarden:

    - **SKU**: basic
    - **Virtueel netwerk**: Selecteer het virtuele netwerk dat u eerder hebt gemaakt. Het gatewaysubnet dat u hebt gemaakt, wordt automatisch geselecteerd.
    - **Eerste IP-configuratie**: dit is het openbare IP-adres van uw Gateway.
        - Selecteer **configuratie van IP-gateway maken**, wat u duurt de **openbare IP-adres kiezen** pagina.
        - Selecteer **+ nieuw** openen de **openbare IP-adres maken** pagina.
        - Voer een **naam** voor uw openbare IP-adres. Laat de SKU als **Basic**, en selecteer vervolgens **OK** uw wijzigingen op te slaan.

       > [!Note]
       > VPN-Gateway ondersteunt momenteel alleen dynamische openbare IP-adrestoewijzing. Dit betekent niet dat dat het IP-adres verandert nadat deze toegewezen aan uw VPN-gateway. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Vergroten of verkleinen, opnieuw wordt ingesteld of andere interne onderhoud/upgrades voor uw VPN-gateway niet het IP-adres te wijzigen.

4. Controleer of de gateway-instellingen.
5. Selecteer **maken** de VPN-gateway te maken. De gateway-instellingen worden gevalideerd en de tegel 'Virtuele netwerkgateway implementeren' wordt weergegeven op uw dashboard.

   >[!Note]
   >Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

    Nadat de gateway is gemaakt, kunt u het IP-adres toegewezen door te kijken naar het virtuele netwerk in de portal kunt zien. De gateway wordt weergegeven als verbonden apparaat. Als u meer informatie over de gateway, selecteer het apparaat.

6. Herhaal de vorige stappen (1-5) op de implementatie van Azure-Stack.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>De lokale netwerkgateway maken in Azure en Azure-Stack

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft een naam die Azure of Azure-Stack kan naar verwijzen en geef vervolgens de site:

- Het IP-adres van de on-premises VPN-apparaat dat u maakt een verbinding voor.
- De IP-adresvoorvoegsels die worden doorgestuurd via de VPN-gateway aan de VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden.

  >[!Note]
  >Als uw wijzigingen in het lokale netwerk of als u wijzigen van het openbare IP-adres voor de VPN-apparaat wilt, kunt u deze waarden later eenvoudig bijwerken.

1. Selecteer in de portal **+ maken van een resource**.
2. Voer in het zoekvak **lokale netwerkgateway**, selecteer daarna **Enter** om te zoeken. Er wordt dan een lijst met resultaten geretourneerd.
3. Selecteer **lokale netwerkgateway**, selecteer daarna **maken** openen de **de lokale netwerkgateway maken** pagina.
4. Op **de lokale netwerkgateway maken**, geef de waarden voor uw lokale netwerkgateway met onze **zelfstudie voorbeeldwaarden**. De volgende aanvullende waarden bevatten.

    - **IP-adres**: dit is het openbare IP-adres van het VPN-apparaat dat u wilt verbinding maken met Azure of Azure-Stack. Geef een geldig openbaar IP-adres dat niet achter een NAT bevinden, zodat het adres door Azure bereiken kan. Als u het IP-adres nu hebt, kunt u een waarde van het voorbeeld als tijdelijke aanduiding, maar hebt u teruggaan en vervang de tijdelijke aanduiding met het openbare IP-adres van uw VPN-apparaat. Azure kan geen verbinding met het apparaat, totdat u een geldig adres opgeven.
    - **Adresruimte**: dit is het adresbereik van het netwerk dat dit lokaal netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u opgeeft niet met adresbereiken van andere netwerken die u verbinding maken overlappen wilt met. Azure stuurt het adresbereik dat u opgeeft, door naar het IP-adres van het on-premises VPN-apparaat. Uw eigen waarden gebruiken als u wilt verbinding maken met uw on-premises site, niet de Voorbeeldwaarde van een.
    - **BGP-instellingen configureren**: alleen bij het configureren van BGP gebruiken. Selecteer deze anders niet.
    - **Abonnement**: Controleer of het juiste abonnement zichtbaar is.
    - **Resourcegroep**: Selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
    - **Locatie**: Selecteer de locatie die dit object wordt gemaakt in. U kunt selecteren die uw VNet bevindt zich in dezelfde locatie, maar u bent niet verplicht om dit te doen.
5. Wanneer u klaar bent met de vereiste waarden opgeven, selecteert u **maken** voor het maken van de lokale netwerkgateway.
6. Herhaal deze stappen (1-5) op de implementatie van Azure-Stack.

## <a name="configure-your-connection"></a>Configureer uw verbinding

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. Het VPN-apparaat dat u configureert, wordt een verbinding genoemd. Het configureren van uw verbinding, hebt u het volgende nodig:

- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
- Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI. Als u wilt zoeken naar het openbare IP-adres van uw VPN-gateway met de Azure portal, gaat u naar de gateways voor virtueel netwerk en selecteer vervolgens de naam van uw gateway.

Gebruik de volgende stappen voor het maken van een Site-naar-Site VPN-verbinding tussen uw virtuele netwerkgateway en uw on-premises VPN-apparaat.

1. Selecteer in de Azure-portal **+ maken van een resource**.
2. Zoeken naar **verbindingen**.
3. In **resultaten**, selecteer **verbindingen**.
4. Op **verbinding**, selecteer **maken**.
5. Op **verbinding maken**, configureer de volgende instellingen:

    - **Verbindingstype**: Selecteer de Site-naar-site (IPSec).
    - **Resourcegroep**: Selecteer uw testgroep resource.
    - **Virtuele netwerkgateway**: Selecteer de virtuele netwerkgateway die u hebt gemaakt.
    - **Lokale netwerkgateway**: Selecteer de lokale netwerkgateway die u hebt gemaakt.
    - **Verbindingsnaam**: dit wordt automatisch ingevuld met de waarden van de twee gateways.
    - **Gedeelde sleutel**: deze waarde moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. Voorbeeld van de zelfstudie 'abc123' gebruikt, maar u kunt (en moeten) gebruikmaken van iets meer complexe. Het belangrijkste is dat deze waarde dezelfde waarde die u opgeeft moet bij het configureren van uw VPN-apparaat.
    - De waarden voor **abonnement**, **resourcegroep**, en **locatie** worden opgelost.

6. Selecteer **OK** om uw verbinding te maken.

U kunt zien dat de verbinding in de **verbindingen** pagina van de virtuele netwerkgateway. De status wordt verder *onbekende* naar *verbinding maken*, en vervolgens naar *geslaagd*.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-Cloud-patronen, [ontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

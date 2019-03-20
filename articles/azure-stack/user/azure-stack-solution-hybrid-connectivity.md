---
title: Hybride cloud-connectiviteit configureren met Azure en Azure Stack | Microsoft Docs
description: Informatie over het configureren van hybride cloud-connectiviteit met Azure en Azure Stack.
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: c9be377dc74ac936aa3139d395b6a02f3b3192eb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58084358"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Zelfstudie: hybride cloud-connectiviteit configureren met Azure en Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt toegang tot bronnen met beveiliging in de globale Azure en Azure Stack met behulp van het patroon van hybride verbinding.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Gegevens on-premises houden om te voldoen aan de privacy- of wettelijke vereisten, maar hebben toegang tot globale Azure-resources.
> - Onderhouden een verouderd systeem tijdens het gebruik van cloud-geschaalde app-implementatie en resources in de globale Azure.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen van pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.


## <a name="prerequisites"></a>Vereisten

Een aantal onderdelen zijn vereist voor het bouwen van een implementatie van hybride verbinding. Sommige van deze onderdelen neemt tijd om voor te bereiden, zodat u zult moeten dienovereenkomstig te plannen.

**Azure Stack**

Een Azure-Partner voor OEM/Hardware een productie Azure Stack kunt implementeren, en alle gebruikers een Azure Stack Development Kit (ASDK) kunnen implementeren.

**Azure Stack-onderdelen**

Een Azure Stack-Operator moet de App Service implementeren, plannen en aanbiedingen maken, maakt u een tenantabonnement en de installatiekopie van Windows Server 2016 kunt toevoegen. Als u nog enkele van deze onderdelen hebt, zorg er dan voor dat ze voldoen aan de vereisten voordat u begint met deze zelfstudie.

In deze zelfstudie wordt ervan uitgegaan dat u enige basiskennis van Azure en Azure Stack hebt. Lees de volgende artikelen voor meer informatie voordat u de zelfstudie begint:

 - [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack-basisbegrippen](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Als u nog geen Azure-abonnement hebt, maakt u een  [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
 - Maak een [Web-App](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts) in Azure. Noteer de URL van de Web-App, omdat u hebt deze nodig in de zelfstudie.

### <a name="azure-stack"></a>Azure Stack

 - Gebruik van de productie van Azure Stack of implementeren van de Azure Stack Development Kit van https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >Implementatie van de ASDK kunt 7 uur duren, dus dienovereenkomstig te plannen.

 - Implementeer [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services met Azure Stack.
 - [Plannen en aanbiedingen maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) in de Azure Stack-omgeving.
 - [Maken van tenantabonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure Stack-omgeving.

### <a name="before-you-begin"></a>Voordat u begint

Controleer of dat u de volgende criteria voldoet aan voordat u begint met het configureren van hybride cloud-verbindingen:

 - U moet een extern gericht openbaar IPv4-adres voor uw VPN-apparaat. Dit IP-adres kan zich niet achter een NAT bevinden.
 - Alle resources worden geïmplementeerd in dezelfde regio/locatie.

#### <a name="tutorial-example-values"></a>Voorbeeld van de zelfstudie waarden

De voorbeelden in deze zelfstudie gebruikt de volgende waarden. U kunt deze waarden gebruiken voor het maken van een testomgeving of ze raadplegen voor een beter begrip van de voorbeelden. Voor meer informatie over VPN-Gateway-instellingen in het algemeen, Zie [informatie over VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Verbinding specificaties:

 - **VPN-type**: Op route gebaseerd
 - **Verbindingstype**: Site-naar-site (IPsec)
 - **Gatewaytype**: VPN
 - **De naam van de verbinding met het Azure**: Azure-Gateway-AzureStack-S2SGateway (wordt de portal automatisch ingevuld deze waarde)
 - **Azure Stack-verbindingsnaam**: AzureStack-Gateway-Azure-S2SGateway (wordt de portal automatisch ingevuld deze waarde)
 - **Gedeelde sleutel**: Een compatibel is met VPN-hardware, met de bijbehorende waarden aan beide zijden van de verbinding
 - **Abonnement**: Elke gewenste abonnement
 - **Resourcegroep**: Test-Infra

IP-adressen, netwerk en subnet:

| Azure/Azure Stack-verbinding | Name | Subnet | IP-adres |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure vNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure-netwerk | Azure-Gateway |  |  |
| Virtuele netwerkgateway van Azure Stack | AzureStack-Gateway |  |  |
| Azure openbaar IP-adres | Azure-GatewayPublicIP |  | Bij het maken van bepaald |
| Azure Stack openbaar IP-adres | AzureStack-GatewayPublicIP |  | Bij het maken van bepaald |
| Lokale netwerkgateway van Azure | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Azure Stack openbaar IP-waarde |
| Lokale netwerkgateway van Azure Stack | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure openbaar IP-waarde |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Een virtueel netwerk maken in de globale Azure en Azure Stack

Gebruik de volgende stappen uit om te maken van een virtueel netwerk met behulp van de portal. U kunt deze gebruiken [voorbeeldwaarden](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) als u dit artikel als onderdeel van een zelfstudie. Maar als u van dit artikel gebruikmaakt om een productieomgeving te configureren, de voorbeeldinstellingen vervangen door uw eigen waarden.

> [!IMPORTANT]
> U moet ervoor zorgen dat er geen IP-adressen in Azure of Azure Stack-vNet-adresruimten overlappen.

Een vNet maken in Azure:

1. Uw browser gebruiken voor verbinding met de [Azure-portal](https://portal.azure.com/) en meld u aan met uw Azure-account.
2. Selecteer **een resource maken**. In de **marketplace doorzoeken** veld `virtual network`''. Zoek **virtueel netwerk** in de lijst met resultaten en selecteer vervolgens **Virtueelnetwerk**.
3. Uit de **een implementatiemodel selecteren** Selecteer **Resource Manager**, en selecteer vervolgens **maken**.
4. Op **virtueel netwerk maken**, configureer de VNet-instellingen. De namen van de vereiste velden worden voorafgegaan door een rood sterretje.  Wanneer u een geldige waarde invoert, wordt het sterretje verandert in een groen vinkje.

Een vNet maken in Azure Stack:

* Herhaal de vorige stappen (1-4) met behulp van de Azure Stack **tenantportal**.

## <a name="add-a-gateway-subnet"></a>Een gatewaysubnet toevoegen

Voordat u het virtuele netwerk verbindt met een gateway, moet u het gatewaysubnet maken voor het virtuele netwerk dat u verbinding wilt maken. De gatewayservices gebruiken de IP-adressen die u in het gatewaysubnet opgeeft.

In de [Azure-portal](https://portal.azure.com/), gaat u naar het virtuele netwerk waar u om een virtuele netwerkgateway te maken van Resource Manager.

1. Selecteer het vNet te openen de **virtueel netwerk** pagina.
2. In **instellingen**, selecteer **subnetten**.
3. Op de **subnetten** weergeeft, schakelt **+ gatewaysubnet** openen de **subnet toevoegen** pagina.

    ![Gatewaysubnet toevoegen](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. De **naam** voor het subnet wordt automatisch ingevuld met de waarde 'GatewaySubnet'. Deze waarde is vereist voor Azure voor het herkennen van het subnet als het gatewaysubnet.
5. Wijzig de **adresbereik** waarden die zijn opgegeven om te voldoen aan uw configuratie en selecteer vervolgens **OK**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Een virtuele netwerkgateway maken in Azure en Azure Stack

Gebruik de volgende stappen maakt u een virtuele netwerkgateway in Azure.

1. Aan de linkerkant van de portal-pagina, selecteer **+**  en voert u 'virtuele netwerkgateway' in het zoekveld.
2. In **resultaten**, selecteer **gateway van virtueel netwerk**.
3. In **gateway van virtueel netwerk**, selecteer **maken** openen de **virtuele netwerkgateway maken** pagina.
4. Op **virtuele netwerkgateway maken**, geef de waarden voor de gateway van uw netwerk, zoals wordt weergegeven in **voorbeeld van de zelfstudie waarden**, en de volgende aanvullende waarden:

   - **SKU**: basic
   - **Virtueel netwerk**: Selecteer het Virtueelnetwerk dat u eerder hebt gemaakt. Het gatewaysubnet dat u hebt gemaakt, wordt automatisch geselecteerd.
   - **Eerste IP-configuratie**:  Dit is het openbare IP-adres van uw Gateway.
     - Selecteer **gateway IP-configuratie maken**, die gaat u naar de **openbare IP-adres kiezen** pagina.
     - Selecteer **+ nieuwe maken** openen de **openbare IP-adres maken** pagina.
     - Voer een **naam** voor uw openbare IP-adres. Laat SKU staan **Basic**, en selecteer vervolgens **OK** uw wijzigingen op te slaan.

       > [!Note]
       > VPN-Gateway ondersteunt momenteel alleen dynamische openbare IP-adrestoewijzing. Echter, betekent dit niet dat het IP-adres verandert nadat deze toegewezen aan uw VPN-gateway. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Vergroten of verkleinen, opnieuw wordt ingesteld of andere interne onderhoud of upgrades worden uitgevoerd op uw VPN-gateway niet het IP-adres te wijzigen.

4. Controleer of de gateway-instellingen.
5. Selecteer **maken** te maken van de VPN-gateway. De gateway-instellingen worden gevalideerd en de tegel 'Virtuele netwerkgateway implementeren' wordt weergegeven op uw dashboard.

   >[!Note]
   >Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

    Nadat de gateway is gemaakt, ziet u de IP-adres is toegewezen door te kijken naar het virtuele netwerk in de portal. De gateway wordt weergegeven als verbonden apparaat. Als u meer informatie over de gateway, selecteer het apparaat.

6. Herhaal de vorige stappen (1-5) op uw Azure Stack-implementatie.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>De lokale netwerkgateway maken in Azure en Azure Stack

De lokale netwerkgateway verwijst doorgaans naar uw on-premises locatie. U geeft een naam waarmee Azure of Azure Stack kunt naar verwijzen en geef vervolgens de site:

- Het IP-adres van de on-premises VPN-apparaat die u voor een verbinding maakt.
- De IP-adresvoorvoegsels die worden doorgestuurd via de VPN-gateway naar het VPN-apparaat. De adresvoorvoegsels die u opgeeft, zijn de voorvoegsels die zich in uw on-premises netwerk bevinden.

  >[!Note]
  >Als uw on-premises netwerk verandert of als u wilt wijzigen van het openbare IP-adres voor de VPN-apparaat, kunt u deze waarden later eenvoudig bijwerken.

1. Selecteer in de portal **+ een resource maken**.
2. Voer in het zoekvak **lokale netwerkgateway**en selecteer vervolgens **Enter** om te zoeken. Er wordt dan een lijst met resultaten geretourneerd.
3. Selecteer **lokale netwerkgateway**en selecteer vervolgens **maken** openen de **lokale netwerkgateway maken** pagina.
4. Op **lokale netwerkgateway maken**, geef de waarden voor uw lokale netwerkgateway met behulp van onze **voorbeeld van de zelfstudie waarden**. De volgende aanvullende waarden bevatten.

    - **IP-adres**: Dit is het openbare IP-adres van het VPN-apparaat dat u wilt verbinding maken met Azure of Azure Stack. Geef een geldig openbaar IP-adres die zich niet achter een NAT-apparaat, zodat Azure het adres kan bereiken. Als u geen nu het IP-adres hebt, kunt u een waarde uit het voorbeeld als een tijdelijke aanduiding, maar u zult moeten gaat u terug en vervang de tijdelijke aanduiding door het openbare IP-adres van uw VPN-apparaat. Azure kan geen verbinding maken met het apparaat totdat u een geldig adres opgeven.
    - **Adresruimte**: Dit is het adresbereik van het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de opgegeven bereiken niet overlappen met bereiken van andere netwerken waarmee u verbinding wilt maken. Azure stuurt het adresbereik dat u opgeeft, door naar het IP-adres van het on-premises VPN-apparaat. Gebruik uw eigen waarden als u wilt verbinding maken met uw on-premises site, niet de Voorbeeldwaarde van een.
    - **BGP-instellingen configureren**: Gebruik alleen bij het configureren van BGP. Selecteer deze anders niet.
    - **Abonnement**: Controleer of dat het juiste abonnement wordt weergegeven.
    - **Resourcegroep**: Selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of u een resourcegroep selecteren die u al hebt gemaakt.
    - **Locatie**: Selecteer de locatie waarin dit object wordt gemaakt. U kunt selecteert u de locatie die zich in uw VNet bevindt, maar u bent niet verplicht om dit te doen.
5. Wanneer u klaar bent met de vereiste waarden op te geven, selecteert u **maken** om de lokale netwerkgateway te maken.
6. Herhaal deze stappen (1-5) op uw Azure Stack-implementatie.

## <a name="configure-your-connection"></a>Uw verbinding configureren

Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. Het VPN-apparaat dat u zo configureren dat wordt aangeduid als een verbinding. Als u wilt uw verbinding configureert, hebt u het volgende nodig:

- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te genereren.
- Het openbare IP-adres van de gateway van uw virtuele netwerk. U kunt het openbare IP-adres weergeven met behulp van Azure Portal, PowerShell of de CLI. Als u wilt zoeken op het openbare IP-adres van uw VPN-gateway met behulp van de Azure portal, gaat u naar de gateways voor virtueel netwerk, en selecteer vervolgens de naam van uw gateway.

Gebruik de volgende stappen uit om te maken van een Site-naar-Site VPN-verbinding tussen uw virtuele netwerkgateway en uw on-premises VPN-apparaat.

1. Selecteer in de Azure portal, **+ een resource maken**.
2. Zoeken naar **verbindingen**.
3. In **resultaten**, selecteer **verbindingen**.
4. Op **verbinding**, selecteer **maken**.
5. Op **verbinding maken**, configureer de volgende instellingen:

    - **Verbindingstype**: Selecteer de Site-naar-site (IPSec).
    - **Resourcegroep**: Selecteer de resourcegroep van de test.
    - **Virtuele netwerkgateway**: Selecteer de virtuele netwerkgateway die u hebt gemaakt.
    - **Lokale netwerkgateway**: Selecteer de lokale netwerkgateway die u hebt gemaakt.
    - **Verbindingsnaam**: Dit wordt automatisch gevuld met behulp van de waarden van de twee gateways.
    - **Gedeelde sleutel**: Deze waarde moet overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. Het voorbeeld van de zelfstudie 'abc123' gebruikt, maar u kunt gebruiken (aanbevolen) ingewikkeldere. Het belangrijkste is dat deze waarde dezelfde waarde die u opgeeft moet bij het configureren van uw VPN-apparaat.
    - De waarden voor **abonnement**, **resourcegroep**, en **locatie** worden opgelost.

6. Selecteer **OK** om uw verbinding te maken.

U kunt zien dat de verbinding in de **verbindingen** pagina van de virtuele netwerkgateway. De status verandert van *onbekende* naar *verbinden*, en klik vervolgens op *geslaagd*.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).

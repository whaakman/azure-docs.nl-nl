---
title: Azure Functions integreren met een Azure-netwerk
description: Een stapsgewijze zelfstudie waarin wordt uitgelegd hoe u verbinding maakt u een functie met een Azure-netwerk
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: article
ms.date: 5/03/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 07c7d7fb682708bf813820440d9c790c28b1f3e5
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834606"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>Zelfstudie: functies integreert met een Azure-netwerk

Deze zelfstudie leert u hoe u Azure Functions gebruiken voor het verbinding maken met bronnen in een Azure-netwerk. u maakt een functie die toegang tot zowel het internet en een virtuele machine met WordPress in het virtuele netwerk heeft.

> [!div class="checklist"]
> * Een functie-app maken in het Premium-abonnement
> * Een WordPress-site implementeren op virtuele machine in een virtueel netwerk
> * De functie-app verbinden met het virtuele netwerk
> * Maak een functie-proxy voor toegang tot resources van WordPress
> * Een WordPress-bestand uit in het virtuele netwerk aan te vragen

> [!NOTE]  
> In deze zelfstudie maakt een functie-app in het Premium-abonnement. Deze hostingabonnement is momenteel in preview. Zie voor meer informatie, [Premium-abonnement].

## <a name="topology"></a>Topologie

Het volgende diagram toont de architectuur van de oplossing die u maakt:

 ![Gebruikersinterface voor de integratie van virtuele netwerken](./media/functions-create-vnet/topology.png)

Functies die worden uitgevoerd in het Premium-abonnement hebben dezelfde host mogelijkheden als web-apps in Azure App Service, waaronder de VNet-integratie-functie. Zie voor meer informatie over VNet-integratie, inclusief het oplossen van problemen en geavanceerde configuratie, [uw app integreren met een Azure virtual network](../app-service/web-sites-integrate-with-vnet.md).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie is het belangrijk dat u bekend bent met IP-adressering en op een subnet zetten. U kunt beginnen met [in dit artikel die betrekking heeft op de basisprincipes van adressen en subnetten](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Veel dat meer artikelen en video's zijn online beschikbaar.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-function-app-in-a-premium-plan"></a>Een functie-app maken in een Premium-abonnement

U maakt eerst een functie-app in de [Premium-abonnement]. Dit abonnement biedt serverloze schaal ondersteuning te bieden voor integratie van virtuele netwerken.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

U kunt de functie-app aan het dashboard vastmaken door het speldpictogram in de rechterbovenhoek te selecteren. Vast te maken, maakt het eenvoudiger om terug te keren naar deze functie-app nadat u uw virtuele machine hebt gemaakt.

## <a name="create-a-vm-inside-a-virtual-network"></a>Een virtuele machine binnen een virtueel netwerk maken

Maak vervolgens een vooraf geconfigureerde virtuele machine die wordt uitgevoerd WordPress binnen een virtueel netwerk ([WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) door Jetware). Een WordPress-VM wordt vanwege de lage kosten en het gemak gebruikt. In dit scenario dezelfde werkt met alle bronnen in een virtueel netwerk, zoals REST-API's, App Service-omgevingen en andere Azure-services. 

1. Kies in de portal **+ een resource maken** in het navigatiedeelvenster links in, in het veld Zoektype `WordPress LEMP7 Max Performance`, en druk op Enter.

1. Kies **Wordpress LEMP Max Performance** in de lijst met zoekresultaten. Selecteer een softwareabonnement van **Wordpress LEMP Max Performance voor CentOS** als de **softwareabonnement** en selecteer **maken**.

1. In de **basisbeginselen** tabblad, gebruikt u de virtuele machine-instellingen zoals opgegeven in de tabel onder de afbeelding:

    ![Tabblad van de basisbeginselen voor het maken van een virtuele machine](./media/functions-create-vnet/create-vm-1.png)

    | Instelling      | Voorgestelde waarde  | Description      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Uw abonnement | Het abonnement waarin uw resources worden gemaakt. | 
    | **[Resourcegroep](../azure-resource-manager/resource-group-overview.md)**  | myResourceGroup | Kies `myResourceGroup`, of de resourcegroep die u hebt gemaakt met de functie-app. Met behulp van dezelfde resourcegroep voor de functie-app, WordPress VM, en hostingplan is het eenvoudiger voor het opschonen van resources, wanneer u klaar bent met deze zelfstudie. |
    | **Naam van virtuele machine** | VNET-Wordpress | De VM-naam moet uniek zijn in de resourcegroep |
    | **[Regio](https://azure.microsoft.com/regions/)** | (Europa) West-Europa | Kies een regio bij u in de buurt of in de buurt van de functies die toegang hebben tot de virtuele machine. |
    | **Grootte** | B1s | Kies **grootte wijzigen** en selecteer vervolgens de standaardinstallatiekopie B1s, met 1 vCPU en 1 GB geheugen. |
    | **Verificatietype** | Wachtwoord | Om wachtwoordverificatie te gebruiken, moet u ook opgeven een **gebruikersnaam**, een veilige **wachtwoord**, en vervolgens **wachtwoord bevestigen**. Voor deze zelfstudie hoeft u zich aanmeldt bij de virtuele machine, tenzij u moet oplossen. |

1. Kies de **netwerken** tabblad en selecteer onder de virtuele netwerken configureren **nieuw**.

1. In **virtueel netwerk maken**, gebruikt u de instellingen in de tabel onder de afbeelding:

    ![Tabblad netwerk van virtuele machine maken](./media/functions-create-vnet/create-vm-2.png)

    | Instelling      | Voorgestelde waarde  | Description      |
    | ------------ | ---------------- | ---------------- |
    | **Naam** | myResourceGroup-vnet | U kunt de standaardnaam die is gegenereerd voor het virtuele netwerk. |
    | **Adresbereik** | 10.10.0.0/16 | Gebruik een één-adresbereik voor het virtuele netwerk. |
    | **Subnetnaam** | Tutorial-Net | Naam van het subnet. |
    | **Adresbereik** (subnet) | 10.10.1.0/24   | De grootte van het gatewaysubnet wordt gedefinieerd hoeveel interfaces kunnen worden toegevoegd aan het subnet. Dit subnet wordt gebruikt door de WordPress-site.  Een `/24` subnet biedt 254 adressen van de host. |

1. Selecteer **OK** te maken van het virtuele netwerk.

1. Terug in de **netwerken** tabblad **geen** voor **openbaar IP-adres**.

1. Kies de **Management** tabblad, klik dan in **opslagaccount voor diagnostische gegevens**, kiest u het Opslagaccount dat u hebt gemaakt met de functie-app.

1. Selecteer **Controleren + maken**. Nadat de validatie is voltooid, schakelt u **maken**. De virtuele machine maken proces duurt enkele minuten. De gemaakte virtuele machine kan alleen toegang tot het virtuele netwerk.

1. Nadat de virtuele machine is gemaakt, kiest u **naar de resource gaan** als de pagina voor uw nieuwe virtuele machine, en kies vervolgens **netwerken** onder **instellingen**.

1. Controleer of er geen **openbaar IP-adres**. Maak een notitie het **privé-IP**, waarmee u verbinding maken met de virtuele machine vanuit uw functie-app.

    ![Netwerkinstellingen voor in de virtuele machine](./media/functions-create-vnet/vm-networking.png)

U hebt nu een WordPress-site geïmplementeerd volledig binnen het virtuele netwerk. Deze site is niet toegankelijk is vanaf het openbare internet.

## <a name="connect-your-function-app-to-the-virtual-network"></a>Uw functie-app verbinden met het virtuele netwerk

Een WordPress-site die wordt uitgevoerd in een virtuele machine in een virtueel netwerk, kunt u nu uw functie-app verbinding met dit virtuele netwerk.

1. Selecteer in de nieuwe functie-app, **platformfuncties** > **netwerken**.

    ![Netwerken in de functie-app kiezen](./media/functions-create-vnet/networking-0.png)

1. Onder **VNet-integratie**, selecteer **Klik hier om te configureren**.

    ![Status voor het configureren van een netwerk-functie](./media/functions-create-vnet/Networking-1.png)

1. Selecteer op de pagina van de integratie van virtueel netwerk **VNet toevoegen (preview)**.

    ![Toevoegen van de VNet-integratie-preview](./media/functions-create-vnet/networking-2.png)

1. In **Status netwerkfunctie**, gebruikt u de instellingen in de tabel onder de afbeelding:

    ![Definieert het virtuele netwerk voor functie-app](./media/functions-create-vnet/networking-3.png)

    | Instelling      | Voorgestelde waarde  | Description      |
    | ------------ | ---------------- | ---------------- |
    | **Virtueel netwerk** | MyResourceGroup-vnet | Dit virtuele netwerk is degene die u eerder hebt gemaakt. |
    | **Subnet** | Nieuw subnet maken | Maak een subnet in het virtuele netwerk voor uw functie-app te gebruiken. VNet-integratie moet worden geconfigureerd voor het gebruik van een leeg subnet. Het maakt niet uit dat uw functies een ander subnet dan uw virtuele machine gebruiken. Het virtuele netwerk routeert automatisch verkeer tussen de twee subnetten. |
    | **Subnetnaam** | Function-Net | Naam van het nieuwe subnet. |
    | **Virtueel netwerkadresblok** | 10.10.0.0/16 | Kies de dezelfde-Adresblok die worden gebruikt door de WordPress-site. U mag slechts één-Adresblok gedefinieerd hebben. |
    | **Adresbereik** | 10.10.2.0/24   | De grootte van het gatewaysubnet beperkt het totale aantal exemplaren die uw functie-app van de Premium-plan naar uitschalen kunt. In dit voorbeeld wordt een `/24` subnet met 254 beschikbare host-adressen. Dit subnet is te weinig ingerichte, maar gemakkelijk te berekenen. |

1. Selecteer **OK** om toe te voegen van het subnet. Sluit de pagina's voor VNet-integratie en Status netwerkfunctie om terug te keren naar de pagina van uw functie-app.

De functie-app kan nu toegang tot het virtuele netwerk waar de WordPress-site wordt uitgevoerd. Vervolgens gebruikt u [Azure Functions-proxy's](functions-proxies.md) retourneren van een bestand op de WordPress-site.

## <a name="create-a-proxy-to-access-vm-resources"></a>Een proxy voor toegang tot VM-resources maken

Met VNet-integratie is ingeschakeld, kunt u een proxy maken in uw functie-app voor het doorsturen van aanvragen naar de virtuele machine die wordt uitgevoerd in het virtuele netwerk.

1. Selecteer in uw functie-app **proxy's** > **+**, gebruikt u de proxy-instellingen in de tabel onder de afbeelding:

    ![De proxy-instellingen definiëren](./media/functions-create-vnet/create-proxy.png)

    | Instelling  | Voorgestelde waarde  | Description      |
    | -------- | ---------------- | ---------------- |
    | **Naam** | Fabriek | De naam mag bestaan uit een willekeurige waarde. Het wordt gebruikt voor het identificeren van de proxy. |
    | **Routesjabloon** | /plant | De route die is toegewezen aan een VM-resource. |
    | **Backend URL** | http://<YOUR_VM_IP>/wp-content/themes/twentyseventeen/assets/images/header.jpg | Vervang `<YOUR_VM_IP>` met het IP-adres van uw WordPress-VM die u eerder hebt gemaakt. Deze toewijzing retourneert een enkel bestand van de site. |

1. Selecteer **maken** de proxy toevoegen aan uw functie-app.

## <a name="try-it-out"></a>Uitproberen

1. Probeer in uw browser voor toegang tot de URL die u als gebruikt de **back-end-URL**. Zoals verwacht, wordt de aanvraag verloopt. Er is een time-out treedt op omdat uw WordPress-site wordt alleen gekoppeld aan het virtuele netwerk en niet via internet.

1. Kopieer de **Proxy-URL** van uw nieuwe proxy-waarde en plak deze in de adresbalk van uw browser. De geretourneerde afbeelding is uit de WordPress-site die worden uitgevoerd binnen het virtuele netwerk.

    ![Fabriek afbeeldingsbestand geretourneerd door de WordPress-site](./media/functions-create-vnet/plant.png)

Uw functie-app is verbonden met internet en het virtuele netwerk. De proxy wordt ontvangen van een aanvraag via het openbare internet en vervolgens die fungeert als een eenvoudige HTTP-proxy voor het doorsturen van deze aanvraag voor het gekoppelde virtuele netwerk. De proxy stuurt vervolgens het antwoord terug naar u openbaar via internet.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie wordt de WordPress-site fungeert als een API die wordt aangeroepen met behulp van een proxy in de functie-app. In dit scenario maakt een goede zelfstudie, omdat het is gemakkelijk om te stellen en te visualiseren. U kunt een andere API die is geïmplementeerd in een virtueel netwerk gebruiken. U kunt ook hebt een functie gemaakt met code die wordt aangeroepen API's die zijn geïmplementeerd in het virtuele netwerk. Een realistischer scenario is een functie die gebruikmaakt van gegevens client-API's voor het aanroepen van een SQL Server-exemplaar geïmplementeerd in het virtuele netwerk.

Functies die worden uitgevoerd in een Premium-abonnement delen de dezelfde onderliggende App Service-infrastructuur als web-apps op PremiumV2-abonnementen. De documentatie voor [web-apps in Azure App Service](../app-service/overview.md) is van toepassing op de functies van uw Premium-abonnement.

> [!div class="nextstepaction"]
> [Meer informatie over de netwerkopties van functies](./functions-networking-options.md)

[Premium-abonnement]: functions-scale.md#premium-plan-public-preview

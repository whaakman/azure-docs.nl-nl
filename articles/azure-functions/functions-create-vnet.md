---
title: Azure Functions integreren met een Azure-netwerk
description: Een stapsgewijze zelfstudie waarin wordt uitgelegd hoe u verbinding maakt u een functie met een Azure-netwerk
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 96ab479d3373eb6e575a00898f7007a4df252e39
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125666"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Een functie-app integreren met een Azure-netwerk

Deze zelfstudie leert u hoe u Azure Functions gebruiken voor het verbinding maken met bronnen in een Azure-netwerk.

Voor deze zelfstudie implementeert een WordPress-site op een virtuele machine in een virtueel netwerk dat is niet toegankelijk vanaf internet. We vervolgens implementeert een functie met toegang tot zowel het internet en het virtuele netwerk. We die functie gebruiken voor toegang tot resources van de WordPress-site die is geïmplementeerd in het virtuele netwerk.

Zie voor meer informatie over de werking van het systeem, probleemoplossing en geavanceerde configuratie [uw app integreren met een Azure virtual network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Azure functions in de Premium-abonnement hebben dezelfde host mogelijkheden als web-apps, zodat de functionaliteit en beperkingen in dat artikel van toepassing op functions.

## <a name="topology"></a>Topologie

 ![Gebruikersinterface voor de integratie van virtuele netwerken][1]

## <a name="create-a-vm-inside-a-virtual-network"></a>Een virtuele machine binnen een virtueel netwerk maken

Als u wilt starten, maken we een vooraf geconfigureerde virtuele machine die wordt uitgevoerd WordPress binnen een virtueel netwerk. 

We hebben gekozen voor WordPress op een virtuele machine omdat deze een van de minst dure resources dat kunnen worden geïmplementeerd in een virtueel netwerk. Houd er rekening mee dat dit scenario kan ook worden gebruikt met een resource in een virtueel netwerk, zoals REST-API's, App Service-omgevingen en andere Azure-services.

1. Ga naar Azure Portal.
2. Een nieuwe resource toevoegen door het openen van de **een resource maken** blade.
3. Zoeken naar "[WordPress LEMP7 Max Performance op CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)' en open de blade voor het maken. 
4. Op de **basisbeginselen** tabblad, configureer dan de virtuele machine met de volgende informatie:
    1. Maak een nieuwe resourcegroep voor deze virtuele machine te maken met het opschonen van de resources die aan het einde van de zelfstudie gemakkelijker. Hier gebruiken we '-functie-VNET-Tutorial' als voorbeeld.
    1. Geef de virtuele machine een unieke naam. Als voorbeeld gebruiken we 'VNET-Wordpress'.
    1. Selecteer de regio het dichtst bij u.
    1. Selecteer de gewenste grootte als B1s (1 vCPU, 1 GB geheugen).
    1. Voor de administrator-account, kiest u wachtwoordverificatie en voer een unieke gebruikersnaam en wachtwoord. Voor deze zelfstudie hoeft u zich aanmeldt bij de virtuele machine, tenzij u moet oplossen.
    
        ![Tabblad van de basisbeginselen voor het maken van een virtuele machine](./media/functions-create-vnet/create-vm-1.png)

1. Verplaatsen naar de **netwerken** tabblad en voer de volgende informatie:
    1.  Maak een nieuw virtueel netwerk.
    1.  Voer een bereik met privé-adres en een subnet in dat adresbereik. De grootte van het gatewaysubnet, bepaalt hoeveel virtuele machines kunt u in de App Service-plan. Als het IP-adressering en definiëren van subnetten zijn nieuw voor u, er is een [document die betrekking heeft op de basisbeginselen](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). IP-adressen en subnetten zijn belangrijk in dit scenario raden wij aan dat u een aantal artikelen te lezen en bekijk een aantal video's online totdat het zinvol. 
    
        In dit voorbeeld zijn we kiest voor het gebruik van het netwerk 10.10.0.0/16 aan een subnet van 10.10.1.0/24. We overmatige inrichting en met behulp van een /16 subnet omdat het is gemakkelijk te berekenen welke subnetten in het netwerk 10.10.0.0/16 beschikbaar zijn.
        
        <img src="./media/functions-create-vnet/create-vm-2.png" width="700">

1. Terug op de **netwerken** tabblad, stelt u het openbare IP-adres op **geen**. Deze stap implementeert de virtuele machine met toegang met alleen het virtuele netwerk.
       
    <img src="./media/functions-create-vnet/create-vm-2-1.png" width="700">

7. De virtuele machine maken. Het proces duurt ongeveer 5 minuten.
8. Nadat de virtuele machine is gemaakt, gaat u naar de **netwerken** tabblad en merk op het privé IP-adres voor later. De virtuele machine mag geen een openbaar IP-adres hebben.

    ![14]

U hebt nu een WordPress-site geïmplementeerd volledig binnen het virtuele netwerk. Deze site is niet toegankelijk is vanaf het openbare internet.

## <a name="create-a-function-app-in-a-premium-plan"></a>Een functie-app maken in een Premium-abonnement

De volgende stap is het maken van een functie-app in een Premium-abonnement. Een Premium-abonnement biedt serverloze schaal met alle voordelen van een toegewezen App Service-plan. Functie-apps die zijn gemaakt via het verbruiksabonnement bieden geen ondersteuning voor integratie van virtuele netwerken.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

## <a name="connect-your-function-app-to-your-virtual-network"></a>Uw functie-app verbinden met uw virtuele netwerk

Een WordPress-site die als host fungeert voor bestanden in uw virtuele netwerk, kunt u nu de functie-app verbinding met het virtuele netwerk.

1.  Selecteer in de portal voor de functie-app uit de vorige stap **platformfuncties**. Selecteer vervolgens **netwerken**.

    <img src="./media/functions-create-vnet/networking-0.png" width="850">

1.  Selecteer **Klik hier om te configureren** onder **VNet-integratie**.

    ![Status voor het configureren van een netwerk-functie](./media/functions-create-vnet/Networking-1.png)

1. Selecteer op de pagina van de integratie van virtueel netwerk **VNet toevoegen (preview)**.

    <img src="./media/functions-create-vnet/networking-2.png" width="600"> 
    
1.  Maak een nieuw subnet voor uw functie en de App Service-plan te gebruiken. Houd er rekening mee dat het totale aantal virtuele machines die u aan uw App Service-plan toevoegen kunt wordt beperkt door de grootte van het gatewaysubnet. Het virtuele netwerk wordt automatisch gerouteerd verkeer tussen de subnetten van het virtuele netwerk, zodat het maakt niet uit dat de functie zich in een ander subnet van de virtuele machine. 
    
    <img src="./media/functions-create-vnet/networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-virtual-network"></a>Een functie die toegang heeft tot een resource in uw virtuele netwerk maken

De functie-app kan nu toegang tot het virtuele netwerk met onze WordPress-site. We gaan de functie wilt gebruiken voor toegang tot dit bestand en dienen deze terug naar de gebruiker. In dit voorbeeld gebruiken we een WordPress-site als de API en een proxy als de aanroepende functie omdat ze eenvoudig kunt instellen en visualiseren. 

U kunt net zo eenvoudig een andere API die is geïmplementeerd in een virtueel netwerk gebruiken. U kunt ook een andere functie gebruiken met code waarmee de API-aanroepen naar de API die is geïmplementeerd in uw virtuele netwerk. Een SQL Server-exemplaar geïmplementeerd in uw virtuele netwerk is een goed voorbeeld.

1. Open in de portal, de functie-app uit de vorige stap.
1. Maken van een proxy door te selecteren **proxy's** > **+**.

    <img src="./media/functions-create-vnet/new-proxy.png" width="250">

1. Configureer de proxynaam en de route. In dit voorbeeld maakt gebruik van ' / plant ' als een route.
1. Vul in uw WordPress-site-IP-adres uit eerdere en stel **back-end-URL** naar `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`
    
    <img src="./media/functions-create-vnet/create-proxy.png" width="900">

Als u probeert om naar de URL van uw back-end rechtstreeks door te plakken in een nieuw browsertabblad te gaan, moet de pagina nu time-out. Dit komt doordat uw WordPress-site is verbonden met alleen het virtuele netwerk en niet via internet. Als u de URL van uw proxyserver in de browser plakken, ziet u een fabriek-afbeelding (opgehaald uit uw WordPress-site) in uw virtuele netwerk. 

Uw functie-app is verbonden met internet en het virtuele netwerk. De proxy wordt ontvangen van een aanvraag via het openbare internet en vervolgens die fungeert als een eenvoudige HTTP-proxy om door te sturen die aanvraag langs in het virtuele netwerk. De proxy stuurt vervolgens het antwoord terug naar u via het openbare internet. 

<img src="./media/functions-create-vnet/plant.png" width="900">

## <a name="next-steps"></a>Volgende stappen

Functies die worden uitgevoerd in een Premium-abonnement delen de dezelfde onderliggende App Service-infrastructuur als web-apps op PremiumV2-abonnementen. De documentatie voor web-apps is van toepassing op de functies van uw Premium-abonnement.

* [Meer informatie over de netwerkopties van functies](./functions-networking-options.md)
* [Lees de veelgestelde vragen over netwerken functies](./functions-networking-faq.md)
* [Meer informatie over virtuele netwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Inschakelen van meer netwerken functies en controle met App Service-omgevingen](../app-service/environment/intro.md)
* [Verbinding maken met afzonderlijke on-premises bronnen zonder wijzigingen van de firewall via hybride verbindingen](../app-service/app-service-hybrid-connections.md)
* [Meer informatie over Functions-proxy 's](./functions-proxies.md)

<!--Image references -->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/create-function-app.png
[3]: ./media/functions-create-vnet/create-app-service-plan.png
[4]: ./media/functions-create-vnet/configure-vnet.png
[5]: ./media/functions-create-vnet/create-vm-1.png
[6]: ./media/functions-create-vnet/create-vm-2.png
[7]: ./media/functions-create-vnet/create-vm-2-1.png
[8]: ./media/functions-create-vnet/networking-1.png
[9]: ./media/functions-create-vnet/networking-2.png
[10]: ./media/functions-create-vnet/networking-3.png
[11]: ./media/functions-create-vnet/new-proxy.png
[12]: ./media/functions-create-vnet/create-proxy.png
[14]: ./media/functions-create-vnet/vm-networking.png

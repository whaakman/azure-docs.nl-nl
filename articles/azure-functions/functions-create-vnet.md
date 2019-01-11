---
title: Azure Functions integreren met een Azure-netwerk
description: Een stapsgewijze zelfstudie waarin u wordt getoond hoe een functie verbinden met een Azure-netwerk
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 12/03/2018
ms.author: alkarche
ms.openlocfilehash: 1140c23a0aa5344119c35434316ec73cc3918f90
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198368"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Een functie-App integreren met een Azure-netwerk
Deze stapsgewijze zelfstudie leert u hoe u Azure Functions gebruiken voor het verbinding maken met bronnen in een Azure VNET. 

Voor deze zelfstudie wordt er een wordpress-site op een virtuele machine in een particulier, niet-toegankelijk is via internet, VNET implementeren. We gaan een functie met een toegang vervolgens implementeren op het internet en het VNET. We die functie gebruiken voor toegang tot resources van de wordpress-site binnen het VNET geïmplementeerd.

Raadpleeg het document voor meer informatie over de werking van het systeem, probleemoplossing en geavanceerde configuratie [uw app integreren met een Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Azure Functions in de toegewezen planning heeft dezelfde host mogelijkheden als web-apps, zodat de functionaliteit en beperkingen in dat document ook van toepassing zijn functies.

## <a name="topology"></a>Topologie
 ![VNet-integratie UI][1]

## <a name="creating-a-vm-inside-of-a-vnet"></a>Het maken van een virtuele machine binnen een VNET

Als u wilt beginnen, maken we een vooraf geconfigureerde VM met het Wordpress binnen een VNET. 

WordPress op een virtuele machine is gekozen omdat het een van de minst dure resources dat kunnen worden geïmplementeerd in een VNET. Houd er rekening mee dat dit scenario kan ook worden gebruikt met een resource in een VNET met inbegrip van REST API's en andere Azure-Services, App Service-omgevingen, enzovoort.

1.  Ga naar de Azure-portal
2.  Een nieuwe resource toevoegen door het openen van de blade 'Een resource maken'
3.  Zoeken naar "[Wordpress LEMP7 Max Performance op CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)' en open de blade voor het maken 
4.  Configureer de virtuele machine met de volgende informatie in de blade voor het maken:
    1.  Maak een nieuwe resourcegroep voor deze virtuele machine te maken met het opschonen van de resources die aan het einde van de zelfstudie gemakkelijker. Kan ik mijn resourcegroep '-functie-VNET-Tutorial' met de naam
    1.  Geef de virtuele machine een unieke naam. Kan ik mijn "VNET-Wordpress" met de naam
    1.  Selecteer de regio het dichtst bij u
    1.  Selecteer de gewenste grootte als B1s (1 vcpu, 1 GB geheugen)
    1.  Voor de administrator-account, kiest u wachtwoordverificatie en voer een unieke gebruikersnaam en wachtwoord. Voor deze zelfstudie moet u niet aanmelden bij de virtuele machine, tenzij u moet oplossen.
    
        <img src="./media/functions-create-vnet/create-VM-1.png" width="700">

1. Verplaatsen naar het tabblad netwerk en voer de volgende informatie:
    1.  Nieuw virtueel netwerk maken
    1.  Voer in de gewenste privé-adresbereik en subnet binnen dat adresbereik. De grootte van het gatewaysubnet, bepaalt hoeveel virtuele machines kunt u in de App Service-plan. Als het IP-adressering en definiëren van subnetten zijn nieuw voor u, is er een [document die betrekking hebben op de basisbeginselen](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). IP-adressen en subnetten zijn belangrijk in dit scenario, zodat ik kan een paar artikelen te lezen en enkele video's online bekijken totdat het zinvol aanbevelen. 
        1. In dit voorbeeld kan ik ervoor gekozen voor het gebruik van het netwerk 10.10.0.0/16 aan een subnet van 10.10.1.0/24. Ik heb geselecteerd overprovision en gebruiken van een /16 subnet omdat het is gemakkelijk te berekenen welke subnetten in het netwerk 10.10.0.0/16 beschikbaar zijn.
        
        <img src="./media/functions-create-vnet/create-VM-2.png" width="700">

1. Terug in het tabblad netwerk stelt u het openbare IP-adres op 'None'. Hierdoor wordt de virtuele machine met toegang met alleen het VNET geïmplementeerd.
       
    <img src="./media/functions-create-vnet/create-VM-2.1.png" width="700">

7. De virtuele machine maken. Dit duurt ongeveer 5 minuten.
8. Nadat de virtuele machine is gemaakt, gaat u naar het tabblad netwerk en noteer het privé-IP-adres voor later. De virtuele machine mag geen een openbaar IP-adres hebben.

    ![14]

U hebt nu een wordpress-site geïmplementeerd volledig binnen het virtuele netwerk. Deze site is niet toegankelijk is vanaf het openbare internet.

## <a name="create-a-dedicated-function-app"></a>Een specifieke functie-App maken

De volgende stap is het maken van een functie-App binnen een standard of hoger App Service-Plan. Houd er rekening mee dat verbruik plan functie-apps bieden geen ondersteuning voor VNET-integratie.

1. Ga naar de Azure-portal
2. Een nieuwe resource toevoegen door het openen van de blade 'Een resource maken'
3. Selecteer 'Serverloze functie-App'
4. Geef al uw normale informatie in de blade voor het maken, met één uitzondering:
    1. Selecteer een niveau van standard of hoger App Service-Plan.

        <img src="./media/functions-create-vnet/Create-App-Service-Plan.PNG" width="300">
    
1. Mijn voltooide maken blade gezien als volgt uit.

    <img src="./media/functions-create-vnet/Create-Function-App.png" width="300">


## <a name="connect-your-function-app-to-your-vnet"></a>Uw functie-App verbinden met uw VNET

Nu we hebben een wordpress-site die als host optreedt veel bestanden uit binnen een VNET en moet nu de functie-app verbinden met dit VNET.

1.  Selecteer in de portal voor de functie-App uit de vorige stap **platformfuncties**en selecteer vervolgens **netwerken**
1.  Selecteer **Klik om te configureren** onder VNet-integratie

    <img src="./media/functions-create-vnet/Networking-1.png" width="450">

1. Selecteer op de pagina VNET-integratie **VNet toevoegen (preview)**

    <img src="./media/functions-create-vnet/Networking-2.png" width="600"> 
    
1.  Maak een nieuw subnet van uw functie- en App Service-plan te gebruiken. Houd er rekening mee dat het totale aantal virtuele machines die u aan uw app service-plan toevoegen kunt wordt beperkt door de grootte van het gatewaysubnet. Uw VNET wordt automatisch gerouteerd verkeer tussen de subnetten in uw VNET, zodat het maakt niet uit dat de functie zich in een ander subnet van de virtuele machine. 
    
    <img src="./media/functions-create-vnet/Networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-vnet"></a>Een functie maken die toegang heeft tot een resource in uw VNET

De functie-App hebben nu toegang tot het VNET met onze wordpress-site, we gaan de functie wilt gebruiken voor toegang tot dit bestand en dienen deze terug naar de gebruiker. In dit voorbeeld gebruiken we een wordpress-site als de API en een functie-Proxy als de oproepende functies omdat ze eenvoudig kunt instellen en visualiseren. U kunt net zo eenvoudig een andere API geïmplementeerd binnen een VNET en een andere functie met een code voor het maken van API-aanroepen naar deze API die is geïmplementeerd in uw VNET. Een SQL-server geïmplementeerd in uw VNET is een goed voorbeeld.

1. Open de functie-App uit de vorige stap in de portal
1. Maken van een Proxy door te selecteren **proxy's** > **+**

    <img src="./media/functions-create-vnet/New-Proxy.png" width="250">

1. Configureer de Proxy-naam en de route. Ik heb geselecteerd /plant als mijn route.
1. Vul uw wordpress-site-IP-adres uit eerdere en de URL van de back-end ingesteld op `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`
    
    <img src="./media/functions-create-vnet/Create-Proxy.png" width="900">

Als u probeert om naar de URL van uw back-end rechtstreeks door te plakken in een nieuw browsertabblad te gaan, moet de pagina nu time-out. Dit is te verwachten, zoals uw wordpress-site is verbonden met alleen uw VNET en niet via internet. Als u de URL van uw proxyserver in de browser plakken ziet u een afbeelding prachtige fabriek, opgehaald uit uw Wordpress-site in uw VNET. 

Uw functie-App is verbonden met Internet en uw VNET. De proxy wordt ontvangen van een aanvraag via het openbare internet en vervolgens die fungeert als een eenvoudige HTTP-proxy om door te sturen die aanvraag langs in het virtuele netwerk. De proxy stuurt vervolgens het antwoord terug naar u via het openbare internet. 

<img src="./media/functions-create-vnet/Plant.png" width="900">

## <a name="next-steps"></a>Volgende stappen

Azure Functions in App Service-plannen worden uitgevoerd op dezelfde service als web-apps, waardoor alle van de documentatie voor Web-Apps wordt toegepast op specifieke functies.

1. [Meer informatie over VNET-integratie met App Service / hier functies](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
1. [Meer informatie over vnet's in Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)
1. [Voor de functies voor netwerkbeheer inschakelen en beheren met App Service-omgevingen](https://docs.microsoft.com/azure/app-service/environment/intro)
1. [Verbinding maken met afzonderlijke on-premises bronnen zonder wijzigingen van de firewall maken via hybride verbindingen](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)
1. [Meer informatie over de functie proxy 's](https://review.docs.microsoft.com/azure/azure-functions/functions-proxies)

<!--Image references-->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/Create-Function-App.png
[3]: ./media/functions-create-vnet/Create-App-Service-Plan.PNG
[4]: ./media/functions-create-vnet/configure-VNET.png
[5]: ./media/functions-create-vnet/create-VM-1.png
[6]: ./media/functions-create-vnet/create-VM-2.png
[7]: ./media/functions-create-vnet/create-VM-2.1.png
[8]: ./media/functions-create-vnet/Networking-1.png
[9]: ./media/functions-create-vnet/Networking-2.png
[10]: ./media/functions-create-vnet/Networking-3.png
[11]: ./media/functions-create-vnet/New-Proxy.png
[12]: ./media/functions-create-vnet/Create-Proxy.png
[14]: ./media/functions-create-vnet/VM-Networking.png

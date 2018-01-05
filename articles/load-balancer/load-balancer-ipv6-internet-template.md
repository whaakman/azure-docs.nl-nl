---
title: Een internetgerichte load balancer met IPv6 - Azure-sjabloon implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van IPv6-ondersteuning voor Azure Load Balancer en taakverdeling virtuele machines.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
keywords: IPv6-, azure load balancer, dual-stack, openbare IP-adres, systeemeigen ipv6, mobiele, iot
ms.assetid: 2998e943-13fc-4ea9-a68c-875e53a08db3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 90439d792eac618671a9de9938302d8930c986d8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Een internetgerichte load balancer-oplossing met IPv6 met een sjabloon implementeren

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure-CLI](load-balancer-ipv6-internet-cli.md)
> * [Sjabloon](load-balancer-ipv6-internet-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer is een Layer-4 (TCP, UDP) load balancer. De load balancer biedt hoge beschikbaarheid bij het verdelen van inkomend verkeer over gezonde service-exemplaren in cloudservices of virtuele machines in een load balancer-set. Azure Load Balancer kan deze services ook toepassen op meerdere poorten, meerdere IP-adressen of allebei.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

Het volgende diagram illustreert de oplossing voor taakverdeling wordt geïmplementeerd met behulp van de voorbeeldsjabloon die in dit artikel wordt beschreven.

![Load balancer-scenario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende Azure-resources:

* de virtuele netwerkinterface voor elke virtuele machine met zowel IPv4 als IPv6-adressen die zijn toegewezen
* een internetgerichte Load Balancer met een IPv4 en IPv6-openbare IP-adres
* twee taakverdelingsregels het openbare VIP's worden toegewezen aan de persoonlijke eindpunten
* een Beschikbaarheidsset met de twee virtuele machines
* twee virtuele machines (VM's)

## <a name="deploying-the-template-using-the-azure-portal"></a>De sjabloon met de Azure portal implementeren

In dit artikel wordt verwezen naar een sjabloon die is gepubliceerd in de [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) galerie. U kunt de sjabloon downloaden uit de galerie of starten van de implementatie in Azure rechtstreeks vanuit de galerie. In dit artikel wordt ervan uitgegaan dat u de sjabloon hebt gedownload op uw lokale computer.

1. Open het Azure-portal en meld u aan met een account met machtigingen voor het maken van virtuele machines en netwerkresources binnen een Azure-abonnement. Tenzij u bestaande resources gebruikt, moet het account ook machtiging voor het maken van een resourcegroep en een opslagaccount.
2. Klik op '+ Nieuw' van de menu vervolgens naar het type 'sjabloon' in het zoekvak. Selecteer 'Sjabloonimplementatie' in de zoekresultaten.

    ![lb-ipv6-portal-stap 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. In de alles blade, klik op 'Sjabloonimplementatie'.

    ![lb-ipv6-portal-stap 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klik op 'Maken'.

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klik op 'Sjabloon bewerken'. Verwijder de bestaande inhoud kopiëren en plakken in de volledige inhoud van het sjabloonbestand (zoals het begin en einde {}), en klikt u op 'Opgeslagen'.

    > [!NOTE]
    > Als u Microsoft Internet Explorer, wanneer u u plakt een dialoogvenster waarin u toegang tot het Windows Klembord ontvangen. Klik op 'Toegang toestaan'.

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klik op 'Parameters bewerken'. In de blade Parameters het waarden per de richtlijnen in de sectie van de sjabloon parameters opgeven Klik op 'Opgeslagen' Sluit de blade Parameters. Selecteer uw abonnement, met een bestaande resourcegroep of maak een in de blade aangepaste implementatie. Als u een resourcegroep maken wilt, selecteert u een locatie voor de resourcegroep. Klik vervolgens op **juridische voorwaarden**, klikt u vervolgens op **aankoop** voor de juridische voorwaarden. Azure begint de resources te implementeren. Het duurt enkele minuten voor het implementeren van alle resources.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Zie voor meer informatie over deze parameters de [sjabloonparameters en variabelen](#template-parameters-and-variables) verderop in dit artikel.

7. Klik op Bladeren, schuif omlaag in de lijst totdat u overzicht 'Resourcegroepen' en klik vervolgens op het overzicht van de resources die zijn gemaakt door de sjabloon.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Klik op de naam van de resourcegroep die u in stap 6 hebt opgegeven op de resourceblade groepen. U ziet een lijst met alle bronnen die zijn geïmplementeerd. Als alles goed is gegaan, moet er 'Geslaagd' onder "Laatste implementatie". Als dit niet het geval is, zorg ervoor dat het account dat u machtigingen voor het maken van de benodigde resources heeft.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Als u uw resourcegroepen onmiddellijk na het voltooien van stap 6 bladert, wordt 'Laatste implementatie' de status van 'Implementatie' weergegeven terwijl de resources worden geïmplementeerd.

9. Klik op 'myIPv6PublicIP' in de lijst met resources. U ziet dat er een IPv6-adres onder IP-adres en dat de DNS-naam is de waarde die u hebt opgegeven voor de parameter dnsNameforIPv6LbIP in stap 6. Deze resource wordt de openbare IPv6-adres en hostnaam naam die toegankelijk is voor Internet-clients.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Connectiviteit valideren

Wanneer de sjabloon is geïmplementeerd, kunt u verbinding kunt valideren door de volgende taken voltooien:

1. Aanmelden bij de Azure-portal en verbinding maken met elk van de VM die is gemaakt door de sjabloonimplementatie. Als u een Windows Server-VM ipconfig uitvoeren geïmplementeerd/alle vanaf een opdrachtprompt. U ziet dat de virtuele machines zowel IPv4 als IPv6-adressen hebben. Als u virtuele Linux-machines hebt geïmplementeerd, moet u de Linux-besturingssysteem voor het ontvangen van de instructies voor uw Linux-distributie met dynamische IPv6-adressen configureren.
2. Start een verbinding met het openbare IPv6-adres van de load balancer van een client verbonden met een IPv6-Internet. Om te bevestigen dat de load balancer is taakverdeling tussen de twee virtuele machines, kunt u een webserver zoals Microsoft Internet Information Services (IIS) installeren op elk van de virtuele machines. De standaardwebpagina op elke server kan de tekst 'Server0' of 'Server1' als unieke identificatie bevatten. Vervolgens opent u een Internet-browser op een client verbonden met een IPv6-Internet en blader naar de hostnaam voor de parameter dnsNameforIPv6LbIP van de load balancer om te bevestigen end-to-end IPv6-verbinding met elke virtuele machine opgegeven. Als u alleen de webpagina van slechts één server, moet u wellicht uw browsercache wissen. Open meerdere persoonlijke browsersessies. U ziet een reactie van elke server.
3. Start een verbinding met het openbare IPv4-adres van de load balancer van een client verbonden met een IPv4-Internet. Om te bevestigen dat de load balancer is met de twee virtuele machines voor taakverdeling, kan u testen met behulp van IIS, zoals beschreven in stap 2.
4. Van elke virtuele machine, een uitgaande verbinding naar een IPv6- of Internet IPv4 verbonden apparaat te starten. In beide gevallen is het bron-IP gezien door het bestemmingsapparaat het openbare IPv4- of IPv6-adres van de load balancer.

> [!NOTE]
> ICMP voor zowel IPv4 als IPv6 wordt geblokkeerd in de Azure-netwerk. ICMP-hulpprogramma's zoals altijd pingen mislukt als gevolg hiervan. Gebruik een alternatieve TCP zoals TCPing of de Test-NetConnection van PowerShell-cmdlet connectiviteit testen. Houd er rekening mee dat de IP-adressen in het diagram ziet u voorbeelden van waarden die u ziet mogelijk zijn. Aangezien de IPv6-adressen dynamisch worden toegewezen, wordt de adressen die u ontvangt, verschillen en kunnen verschillen per regio. Bovendien is het gebruikelijk dat het openbare IPv6-adres op de load balancer om te beginnen met een ander voorvoegsel dan de persoonlijke IPv6-adressen in de groep back-end.

## <a name="template-parameters-and-variables"></a>Sjabloonparameters en variabelen

Een Azure Resource Manager-sjabloon bevat meerdere variabelen en parameters die u aan uw behoeften aanpassen kunt. Variabelen worden gebruikt voor vaste waarden die u niet dat een gebruiker wilt te wijzigen. Parameters worden gebruikt voor waarden dat u wilt dat een gebruiker op te geven wanneer de sjabloon implementeren. De voorbeeldsjabloon is geconfigureerd voor het scenario dat in dit artikel wordt beschreven. U kunt dit aanpassen aan de behoeften van uw omgeving.

De voorbeeldsjabloon die wordt gebruikt in dit artikel bevat de volgende parameters en variabelen:

| De parameter / Variable | Opmerkingen |
| --- | --- |
| adminUsername |Geef de naam van de admin-account aan te melden bij de virtuele machines met gebruikt. |
| adminPassword |Geef het wachtwoord voor de admin-account aan te melden bij de virtuele machines met gebruikt. |
| dnsNameforIPv4LbIP |Geef de DNS-hostnaam die u wilt aanwijzen als de openbare naam van de load balancer. Deze naam wordt omgezet naar het openbare IPv4-adres van de load balancer. De naam moet worden in kleine letters en overeenkomen met de reguliere expressie: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Geef de DNS-hostnaam die u wilt aanwijzen als de openbare naam van de load balancer. Deze naam wordt omgezet naar het openbare IPv6-adres van de load balancer. De naam moet worden in kleine letters en overeenkomen met de reguliere expressie: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Dit kan dezelfde naam als het IPv4-adres zijn. Wanneer een client verzendt een DNS-query voor deze naam Azure retourneert registreert de A- en AAAA wanneer de naam wordt gedeeld. |
| vmNamePrefix |Geef het voorvoegsel van de virtuele machine. De sjabloon wordt toegevoegd een getal (0, 1, enz.) op de naam wanneer de virtuele machines worden gemaakt. |
| nicNamePrefix |Geef het voorvoegsel van de netwerk-interface. De sjabloon wordt toegevoegd een getal (0, 1, enz.) op de naam wanneer de netwerkinterfaces worden gemaakt. |
| storageAccountName |Voer de naam van een bestaand opslagaccount of geef de naam van een nieuwe door de sjabloon worden gemaakt. |
| availabilitySetName |Voer vervolgens de naam van de beschikbaarheidsset moet worden gebruikt met de virtuele machines |
| addressPrefix |Het adresvoorvoegsel dat wordt gebruikt voor het definiëren van het adresbereik van het virtuele netwerk |
| subnetName |De naam van het subnet in gemaakt voor het VNet |
| subnetPrefix |Het adresvoorvoegsel dat wordt gebruikt voor het definiëren van het adresbereik van het subnet |
| vnetName |Geef de naam voor het VNet dat wordt gebruikt door de virtuele machines. |
| ipv4PrivateIPAddressType |De toewijzingsmethode gebruikt voor de particuliere IP-adres (statisch of dynamisch) |
| ipv6PrivateIPAddressType |De toewijzingsmethode gebruikt voor de particuliere IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| numberOfInstances |Het aantal exemplaren van taakverdeling geïmplementeerd door de sjabloon |
| ipv4PublicIPAddressName |Geef de DNS-naam die u wilt gebruiken om te communiceren met het openbare IPv4-adres van de load balancer. |
| ipv4PublicIPAddressType |De toewijzingsmethode voor het openbare IP-adres (statisch of dynamisch) |
| Ipv6PublicIPAddressName |Geef de DNS-naam die u wilt gebruiken om te communiceren met het openbare IPv6-adres van de load balancer. |
| ipv6PublicIPAddressType |De toewijzingsmethode gebruikt voor het openbare IP-adres (dynamisch). IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| lbName |Geef de naam van de load balancer. Deze naam wordt weergegeven in de portal of wanneer er naar wordt verwezen met CLI of PowerShell-opdracht gebruikt. |

De resterende variabelen in de sjabloon bevatten afgeleide waarden die worden toegewezen wanneer Azure de resources maakt. Deze variabelen niet wijzigen.

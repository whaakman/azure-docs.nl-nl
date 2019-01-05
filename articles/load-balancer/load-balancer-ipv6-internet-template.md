---
title: Een internetgerichte load balancer met IPv6 - Azure-sjabloon implementeren
titlesuffix: Azure Load Balancer
description: Klik hier voor meer informatie over het implementeren van IPv6-ondersteuning voor Azure Load Balancer en VM's met load balancing.
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6-, azure-load balancer, dual-stack, openbaar IP-adres, systeemeigen IPv6-, mobiele, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 4a8c7309a07238ef3410e42c3d631ad525f023cc
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039789"
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Een internetgerichte load balancer-oplossing met IPv6 met behulp van een sjabloon implementeren

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Sjabloon](load-balancer-ipv6-internet-template.md)



Azure Load Balancer is een Layer-4 (TCP, UDP) load balancer. De load balancer biedt hoge beschikbaarheid bij het verdelen van inkomend verkeer over gezonde service-exemplaren in cloudservices of virtuele machines in een load balancer-set. Azure Load Balancer kan deze services ook toepassen op meerdere poorten, meerdere IP-adressen of allebei.

## <a name="example-deployment-scenario"></a>Voorbeeldscenario voor implementatie

Het volgende diagram illustreert de load balancer-oplossing wordt geïmplementeerd met behulp van de voorbeeldsjabloon die in dit artikel wordt beschreven.

![Load balancer-scenario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende Azure-resources:

* Een virtuele netwerkinterface voor elke virtuele machine met zowel IPv4 als IPv6-adressen die zijn toegewezen
* een internetgerichte Load Balancer met een IPv4- en een openbare IPv6-IP-adres
* Twee laden regels voor taakverdeling voor de openbare VIP's toewijzen aan de persoonlijke eindpunten
* een Beschikbaarheidsset met de twee virtuele machines
* Twee virtuele machines (VM's)

## <a name="deploying-the-template-using-the-azure-portal"></a>Implementatie van de sjabloon met behulp van de Azure portal

In dit artikel wordt verwezen naar een sjabloon die is gepubliceerd in de [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) galerie. U kunt de sjabloon downloaden uit de galerie of starten van de implementatie in Azure rechtstreeks vanuit de galerie. In dit artikel wordt ervan uitgegaan dat u de sjabloon hebt gedownload naar uw lokale computer.

1. Open het Azure-portal en meld u aan met een account met machtigingen voor het maken van VM's en netwerken bronnen binnen een Azure-abonnement. Bovendien, tenzij u bestaande resources, het account heeft toestemming nodig om een resourcegroep en een opslagaccount te maken.
2. Klik op '+ Nieuw' van de menu vervolgens naar het type 'sjabloon' in het zoekvak in. Selecteer 'Sjabloonimplementatie' in de zoekresultaten.

    ![lb-ipv6-portal-stap 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. In de alles blade, klikt u op "Sjabloonimplementatie."

    ![lb-ipv6-portal-stap 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klik op 'Maken'.

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klik op 'Sjabloon bewerken'. Verwijder de bestaande inhoud, kopiëren en plakken in de volledige inhoud van het sjabloonbestand (aan het begin en eindigen {}) en klik op 'Opslaan'.

    > [!NOTE]
    > Als u Microsoft Internet Explorer, wanneer u u plakt er een dialoogvenster waarin wordt gevraagd u wilt toestaan dat toegang tot het Windows-Klembord. Klik op 'Toegang toestaan'.

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klik op 'Parameters bewerken'. Geef de waarden overeenkomstig de richtlijnen in het gedeelte van de sjabloon parameters in de blade Parameters en klikt u op 'Opslaan' om te sluiten van de Parameters-blade. Selecteer uw abonnement, een bestaande resourcegroep of maak een in de blade aangepaste implementatie. Als u een resourcegroep maken wilt, selecteert u een locatie voor de resourcegroep. Klik vervolgens op **juridische voorwaarden**, klikt u vervolgens op **aankoop** voor de juridische bepalingen. Azure begint met de bronnen te installeren. Het duurt enkele minuten om alle resources te implementeren.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Zie voor meer informatie over deze parameters de [sjabloonparameters en variabelen](#template-parameters-and-variables) verderop in dit artikel.

7. Als u wilt zien van de resources die door de sjabloon is gemaakt, klikt u op Bladeren, schuif omlaag in de lijst totdat u ziet 'Resourcegroepen', en klik erop.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Op het tabblad groepen, klikt u op de naam van de resourcegroep die u hebt opgegeven in stap 6. U ziet een lijst van alle resources die zijn geïmplementeerd. Als alles goed is, moet er 'Geslaagd' onder "Laatste implementatie". Als dit niet het geval is, zorg ervoor dat het account dat u machtigingen voor het maken van de benodigde resources.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Als u uw resourcegroepen onmiddellijk na voltooiing van stap 6 bladert, wordt 'Laatste implementatie' de status van de 'Implementeren' weergegeven terwijl de resources worden geïmplementeerd.

9. Klik op 'myIPv6PublicIP' in de lijst met resources. U ziet dat er een IPv6-adres onder IP-adres en dat de DNS-naam is de waarde die u hebt opgegeven voor de parameter dnsNameforIPv6LbIP in stap 6. Deze resource is de openbare IPv6-adres en de hostnaam naam die toegankelijk is voor Internet-clients.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Verbinding valideren

Wanneer de sjabloon met succes is geïmplementeerd, kunt u verbinding kunt controleren door de volgende taken voltooien:

1. Meld u aan bij Azure portal en maak verbinding met elk van de virtuele machines die zijn gemaakt door de sjabloonimplementatie. Als u een Windows Server VM, ipconfig uitvoeren geïmplementeerd/alle vanaf een opdrachtprompt. U ziet dat de virtuele machines zowel IPv4 als IPv6-adressen hebben. Als u Linux-VM's geïmplementeerd, moet u de Linux-besturingssysteem voor het ontvangen van dynamische IPv6-adressen met behulp van de instructies voor uw Linux-distributie configureren.
2. Start een verbinding met het openbare IPv6-adres van de load balancer van een client IPv6-Internet is verbonden. Om te bevestigen dat de load balancer is taakverdeling tussen de twee virtuele machines, kunt u een webserver, zoals Microsoft Internet Information Services (IIS) installeren op elk van de virtuele machines. De standaard-webpagina op elke server kan de tekst 'Server0' of 'Server1' voor het aanduiden van deze bevatten. Vervolgens opent u een internetbrowser op een client met IPv6-Internet is verbonden en blader naar de hostnaam die u hebt opgegeven voor de parameter dnsNameforIPv6LbIP van de load balancer om te bevestigen van end-to-end IPv6-verbinding met elke virtuele machine. Als u alleen de webpagina van slechts één server ziet, moet u mogelijk uw browsercache wissen. Open meerdere persoonlijke browsersessies. Hier ziet u een reactie van elke server.
3. Start een verbinding met het openbare IPv4-adres van de load balancer van een client met IPv4-Internet is verbonden. Om te bevestigen dat de load balancer is met de twee virtuele machines te verdelen, kunt u testen met behulp van IIS, zoals beschreven in stap 2.
4. Initiëren van elke virtuele machine, een uitgaande verbinding naar een IPv6- of Internet verbinding met een IPv4-apparaat. In beide gevallen is de bron-IP gezien door het bestemmingsapparaat het openbare IPv4- of IPv6-adres van de load balancer.

> [!NOTE]
> ICMP voor zowel IPv4 als IPv6 wordt geblokkeerd in het Azure-netwerk. ICMP-hulpprogramma's zoals altijd pingen mislukt als gevolg hiervan. Connectiviteit wilt testen, gebruikt u een alternatieve TCP zoals tcping uitvoeren of de cmdlet Test-NetConnection van PowerShell. Houd er rekening mee dat de IP-adressen in het diagram ziet u voorbeelden van waarden die u mogelijk te zien zijn. Aangezien de IPv6-adressen dynamisch worden toegewezen, wordt de adressen die u ontvangt kunnen variëren en per regio kunnen verschillen. Bovendien is het gebruikelijk voor het openbare IPv6-adres op de load balancer om te beginnen met een ander voorvoegsel is dan de persoonlijke IPv6-adressen in de back-endpool.

## <a name="template-parameters-and-variables"></a>Sjabloonparameters en variabelen

Een Azure Resource Manager-sjabloon bevat meerdere variabelen en parameters die u aan uw behoeften aanpassen kunt. Variabelen worden gebruikt voor vaste waarden die u niet dat een gebruiker wilt te wijzigen. Parameters worden gebruikt voor waarden die u wilt dat een gebruiker op te geven wanneer de sjabloon te implementeren. De voorbeeldsjabloon is geconfigureerd voor het scenario dat in dit artikel wordt beschreven. U kunt deze aanpassen aan de behoeften van uw omgeving.

De voorbeeldsjabloon die wordt gebruikt in dit artikel bevat de volgende variabelen en parameters:

| Parameter / Variable | Opmerkingen |
| --- | --- |
| adminUsername |Geef de naam van het beheerdersaccount dat wordt gebruikt om aan te melden bij de virtuele machines met. |
| adminPassword |Geef het wachtwoord voor het beheerdersaccount dat wordt gebruikt om aan te melden bij de virtuele machines met. |
| dnsNameforIPv4LbIP |Geef de DNS-hostnaam die u wilt aanwijzen als de openbare naam van de load balancer. Deze naam wordt omgezet naar het openbare IPv4-adres van de load balancer. De naam moet letters en overeenkomen met de reguliere expressie: ^ [a-z] [een-z0 - 9-]{1,61}[a-z0-9] $. |
| dnsNameforIPv6LbIP |Geef de DNS-hostnaam die u wilt aanwijzen als de openbare naam van de load balancer. Deze naam wordt omgezet naar het openbare IPv6-adres van de load balancer. De naam moet letters en overeenkomen met de reguliere expressie: ^ [a-z] [een-z0 - 9-]{1,61}[a-z0-9] $. Dit kan dezelfde naam als het IPv4-adres zijn. Wanneer een client verzendt een DNS-query voor deze naam Azure retourneert registreert de A- en AAAA wanneer de naam wordt gedeeld. |
| vmNamePrefix |Geef het voorvoegsel van de virtuele machine. De sjabloon wordt toegevoegd een getal (0, 1, enzovoort) op de naam als de virtuele machines worden gemaakt. |
| nicNamePrefix |Geef het voorvoegsel van de netwerk-interface. De sjabloon wordt toegevoegd een getal (0, 1, enzovoort) op de naam als de netwerkinterfaces worden gemaakt. |
| storageAccountName |Voer de naam van een bestaand opslagaccount of geef de naam van een nieuwe moet worden gemaakt door de sjabloon. |
| availabilitySetName |Voer vervolgens de naam van de beschikbaarheidsset moet worden gebruikt met de virtuele machines |
| addressPrefix |Het adresvoorvoegsel dat wordt gebruikt voor het definiëren van het adresbereik van het Virtueelnetwerk |
| subnetName |De naam van het subnet in gemaakt voor het VNet |
| subnetPrefix |Het adresvoorvoegsel dat wordt gebruikt voor het definiëren van het adresbereik van het subnet |
| vnetName |Geef de naam voor de VNet die worden gebruikt door de virtuele machines. |
| ipv4PrivateIPAddressType |De toewijzingsmethode gebruikt voor het particuliere IP-adres (statisch of dynamisch) |
| ipv6PrivateIPAddressType |De toewijzingsmethode voor het privé IP-adres (dynamisch) gebruikt. IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| numberOfInstances |Het aantal exemplaren met gelijke geïmplementeerd door de sjabloon |
| ipv4PublicIPAddressName |Geef de DNS-naam die u wilt gebruiken om te communiceren met het openbare IPv4-adres van de load balancer. |
| ipv4PublicIPAddressType |De toewijzingsmethode gebruikt voor het openbare IP-adres (statisch of dynamisch) |
| Ipv6PublicIPAddressName |Geef de DNS-naam die u wilt gebruiken om te communiceren met de openbare IPv6-adres van de load balancer. |
| ipv6PublicIPAddressType |De toewijzingsmethode voor het openbare IP-adres (dynamisch) gebruikt. IPv6 biedt alleen ondersteuning voor dynamische toewijzing. |
| lbName |Geef de naam van de load balancer. Deze naam wordt weergegeven in de portal of met de CLI of PowerShell-opdracht met betrekking tot het gebruikt. |

De resterende variabelen in de sjabloon bevat de afgeleide waarden die worden toegewezen wanneer de resources in Azure gemaakt. Deze variabelen niet wijzigen.

## <a name="next-steps"></a>Volgende stappen

Zie voor de JSON-syntaxis en de eigenschappen van een load balancer in een sjabloon, [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).

---
title: Persoonlijke gegevens beschermen met Azure-netwerkbeveiligingsfuncties | Microsoft Docs
description: Persoonlijke gegevens beschermen met beveiligingsfuncties van Azure-netwerk
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: d61b29f1327f57bc32b2c53de3fe58e53fcf3cac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Persoonlijke gegevens beschermen met netwerkbeveiligingsfuncties: Azure Application Gateway en Netwerkbeveiligingsgroepen

Dit artikel bevat informatie en procedures waarmee u Azure Application Gateway en Netwerkbeveiligingsgroepen gebruiken om persoonlijke gegevens te beveiligen.

Een belangrijk aspect vormt in een meerlaagse beveiligingsstrategie om de privacy van persoonlijke gegevens te beschermen, is een beveiliging tegen misbruik van algemene beveiligingsproblemen zoals SQL-injectie of cross-site scripting. Ongewenste netwerkverkeer buiten uw Azure virtual network beschermt tegen mogelijke inbreuk op gevoelige gegevens, en houden Microsoft Azure biedt u hulpprogramma's om u te helpen uw gegevens beschermen tegen kwaadwillende personen.

## <a name="scenario"></a>Scenario

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse, Adriatische, Baltische veiligheid ook Florida. Ter bevordering van deze inspanningen is die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk

Het bedrijf gebruikmaakt van Microsoft Azure worden bedrijfsgegevens opgeslagen in de cloud en toepassingen worden uitgevoerd op virtuele machines die worden verwerkt en toegang tot deze gegevens. Deze gegevens omvatten persoonlijk gegevens zoals namen, adressen, telefoonnummers en creditcardgegevens van globale klantendatabase. Dit omvat ook traditionele Human Resources informatie zoals adressen, telefoonnummers, BTW-id's en andere informatie over de werknemers van het bedrijf op alle locaties. De regel cruise onderhoudt ook een grote database van derden en loyaliteit programma leden met persoonlijke gegevens voor de relaties met de huidige en eerdere klanten bijhouden.

Zakelijke werknemers toegang tot het netwerk van het bedrijf externe kantoren en reizen agents over de hele wereld toegang hebben tot een aantal bedrijfsresources en webtoepassingen die worden gehost in Azure VM's gebruiken om te communiceren met het.

## <a name="problem-statement"></a>Probleemformulering

Het bedrijf dient de privacy van klanten te beschermen en persoonlijke gegevens van de werknemers van aanvallers die gebruikmaken van beveiligingsproblemen om uit te voeren schadelijke code die persoonlijke gegevens worden namelijk blootgesteld software opgeslagen of gebruikt door het bedrijf cloud-gebaseerde toepassingen.

## <a name="company-goal"></a>Bedrijf-doel

Het doel van het bedrijf om ervoor te zorgen dat onbevoegden geen toegang tot zakelijke Azure Virtual Networks en de toepassingen en gegevens die door misbruik van veelvoorkomende beveiligingslekken in staan. 

## <a name="solutions"></a>Oplossingen

Microsoft Azure biedt beveiligingsmethoden om te voorkomen dat ongewenste verkeer Azure Virtual Networks invoeren. Beheer van binnenkomende en uitgaande verkeer wordt normaal uitgevoerd door firewalls. In Azure, kunt u de toepassingsgateway met de Web Application Firewall en de Netwerkbeveiligingsgroep groepen (NSG), die als een eenvoudige gedistribueerde firewall fungeren. Deze hulpprogramma's kunnen u detecteren en blokkeren van ongewenste netwerkverkeer.

### <a name="application-gatewayweb-application-firewall"></a>Application Gateway of Web Application Firewall

De [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF)-onderdeel van de [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) webtoepassingen zijn steeds meer doelen van aanvallen die gebruikmaken van algemene bekende beveiligt beveiligingsproblemen. Een gecentraliseerde WAF wordt beschermd tegen aanvallen via Internet door en beveiligingsbeheer zonder eventuele wijzigingen in de toepassing.

Azure WAF adressen verschillende aanval categorieën inclusief SQL-injectie, cross-site scripting schendingen van HTTP-protocol en afwijkingen, bots, crawlers, scanners, veelvoorkomende onvolkomenheden in de toepassing, HTTP-Denial of Service en andere algemene aanvallen, zoals het invoegen van de opdracht HTTP-aanvraag ' smokkelen ', HTTP-antwoord splitsen en extern bestand insluiting aanvallen. 

U kunt een toepassingsgateway maken met WAF of WAF toevoegen aan een bestaande toepassingsgateway. Azure Application Gateway vereist in beide gevallen moet een eigen subnet.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Hoe maak ik een toepassingsgateway met WAF? 

Als een nieuwe toepassingsgateway maken met WAF ingeschakeld, het volgende doen:

1. Aanmelden bij de Azure portal en in de **Favorieten** deelvenster van de portal klikt u op **nieuw**

2. Klik op de blade **Nieuw** op **Netwerken**.

3. Klik op **toepassingsgateway**.

4. Navigeer naar de Azure-portal **klikt u op nieuw \> Networking \> Application Gateway.**

   ![Toepassingsgateways maken](media/protect-netsec/app-gateway-01.png)

5. In de **basisbeginselen** blade die wordt weergegeven, geef de waarden voor de volgende velden: naam, laag SKU (Standard of WAF)-grootte (klein, middelgroot of groot), aantal (2 voor hoge beschikbaarheid)-instantie abonnement, resourcegroep en locatie.

6. In de **instellingen** blade die wordt weergegeven onder **virtueel netwerk**, klikt u op **Kies een virtueel netwerk**. Deze stap wordt geopend, voer de blade van het virtueel netwerk kiezen.

7. Klik op **nieuw** openen de **virtueel netwerk maken** blade.

8. Voer de volgende waarden: naam, de adresruimte, subnetnaam, adres-subnetbereik. Klik op **OK**.

9. Op de **instellingen** blade onder **Frontend IP-configuratie**, kies het type IP-adres.

10. Klik op **Kies een openbaar IP-adres** vervolgens **nieuwe maken.**

11. De standaardwaarde accepteren en klik op **OK.**

12. Op de **instellingen** blade onder **listenerconfiguratie**, selecteer het gebruik van HTTP of HTTPS onder **Protocol**. Er is een certificaat vereist voor het gebruik van HTTPS.

13. Configureer de specifieke instellingen WAF: **Firewall-status** (**ingeschakeld**) en **firewallmodus** (**preventie**). Als u ervoor kiest **detectie** als de modus, wordt alleen verkeer vastgelegd.

14. Controleer de **samenvatting** pagina en klik op **OK**. De toepassingsgateway is nu in de wachtrij geplaatst en gemaakt.

Nadat de toepassingsgateway is gemaakt, kunt u navigeren naar het in de portal en gaan met de configuratie van de toepassingsgateway.

![gemaakte toepassingsgateway](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a>Hoe kan ik WAF naar een bestaande toepassing toevoegen

Voor het bijwerken van een bestaande toepassingsgateway ter ondersteuning van WAF in de modus voor preventie van het volgende doen:

1. Klik in het deelvenster **Favorieten** van Azure Portal op **Alle resources**.

2. Klik op de bestaande toepassingsgateway in de **alle resources** blade. 
>[!NOTE]
Opmerking: Als het abonnement dat u hebt geselecteerd al verschillende bronnen heeft, kunt u de naam in het Filter met de naam... voor eenvoudige toegang tot de DNS-zone.
3. Klik op **Web application firewall** en de instellingen voor de toepassingsgateway bijwerken: **upgraden naar de laag WAF** (ingeschakeld) **Firewall-status** (ingeschakeld)  **Firewallmodus** (preventie). U moet ook de regelinstellingen configureren en uitgeschakelde regels configureren.

Zie voor meer gedetailleerde informatie over het maken van een nieuwe toepassingsgateway met WAF en WAF toevoegen aan een bestaande toepassingsgateway [een toepassingsgateway maken met web application firewall via de portal.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Een [netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) bevat een lijst met regels voor toestaan of weigeren van netwerkverkeer naar bronnen die zijn verbonden met [Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet). Nsg's kunnen worden gekoppeld aan subnetten of afzonderlijke virtuele machines. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. Verkeer kan verder worden beperkt door ook een NSG te koppelen aan een VM of NIC.

Nsg's bevatten vier eigenschappen: naam, regio, resourcegroep en regels.
>[!Note]
Hoewel een NSG bestaat in een resourcegroep, kan deze worden gekoppeld aan resources in elke willekeurige resourcegroep, mits de resource tot dezelfde Azure-regio als de NSG behoort.

NSG-regels negen eigenschappen bevatten: naam, het Protocol (TCP, UDP of \*, waaronder ICMP als UDP en TCP), bron poortbereik, doelpoortbereik, bronadres voorvoegsel, doel-adresvoorvoegsel, richting (inkomend of uitgaand), () prioriteit tussen 100 en 4096) en toegang tot type (toestaan of weigeren). Alle nsg's bevatten een set met standaardregels die kan worden verwijderd of overschreven door de regels die u maakt.

#### <a name="how-do-i-implement-nsgs"></a>Hoe ik nsg's implementeren?

Nsg's implementeert vereist plannen en er zijn verschillende ontwerpoverwegingen, u moet rekening te houden. Deze omvatten beperkingen met betrekking tot het aantal nsg's per abonnement en -regels per NSG; 'S VNet en subnetten ontwerpen, speciale regels, ICMP-verkeer, isolatie van lagen met subnetten, load balancers en meer.

Zie voor meer richtlijnen bij het plannen en implementeren van nsg's en een voorbeeld implementatiescenario [filteren van netwerkverkeer met netwerkbeveiligingsgroepen.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Hoe maak ik regels in een NSG

U maakt regels voor binnenkomende verbindingen in een bestaande NSG door het volgende doen:

1. Klik op **Bladeren**, en vervolgens **Netwerkbeveiligingsgroepen**.

2. Klik in de lijst van nsg's op **NSG-FrontEnd**, en vervolgens **inkomende beveiligingsregels voor verbindingen.**

3. Klik in de lijst met regels voor binnenkomend verkeer op **toevoegen.**

4. Geef de waarden in de volgende velden: naam, prioriteit, bron, Protocol, bron-bereik, bestemming bestemming poortbereik en in te grijpen.

De nieuwe regel wordt weergegeven in de NSG na enkele seconden.

![netwerkbeveiligingsregels](media/protect-netsec/inbound-security.png)

Zie voor meer instructies voor het nsg's maken in subnetten, regels maken en een NSG koppelen aan een subnet front-end en back-end [netwerkbeveiligingsgroepen met de Azure portal maken.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Volgende stappen

[Azure-netwerkbeveiliging](https://azure.microsoft.com/blog/azure-network-security/)

[Aanbevolen beveiligingsprocedures voor Azure-netwerk](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Informatie over een netwerkbeveiligingsgroep ophalen](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)

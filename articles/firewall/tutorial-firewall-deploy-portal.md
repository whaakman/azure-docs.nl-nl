---
title: Azure Firewall implementeren en configureren met de Azure-portal
description: In deze zelfstudie leert u hoe u Azure Firewall kunt implementeren en configureren via de Azure-portal.
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: ff8df51011ef664950ecfeb9eef0b201306c8ad5
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221641"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Zelfstudie: Azure Firewall implementeren en configureren met de Azure-portal

Azure Firewall heeft twee soorten regels om de uitgaande toegang te beheren:

- **Toepassingsregels**

   Hiermee kunt u volledig gekwalificeerde domeinnamen (FQDN’s) configureren waartoe toegang kan worden verkregen via een subnet. U kunt bijvoorbeeld toegang tot *github.com* via uw subnet toestaan.
- **Netwerkregels**

   Hiermee kunt u regels configureren die een bronadres, protocol, doelpoort en doeladres bevatten. U kunt bijvoorbeeld een regel maken die toestaat dat verkeer naar poort 53 (DNS) naar het IP-adres van uw DNS-server gaat via uw subnet.

Netwerkverkeer is onderhevig aan de geconfigureerde firewallregels wanneer u het routeert naar de firewall als standaardgateway van het subnet.

Toepassings- en netwerkregels worden opgeslagen in *regelverzamelingen*. Een regelverzameling is een lijst met regels die dezelfde actie en prioriteit hebben.  Een netwerkregelverzameling is een lijst met netwerkregels en een toepassingsregelverzameling is een lijst met toepassingsregels.

Azure Firewall beschikt over NAT-regels, netwerkregels en toepassingsregels. Zie [Verwerkingslogica voor Azure Firewall-regels](rule-processing.md) voor meer informatie over de verwerkingslogica voor Azure Firewall-regels.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een testnetwerkomgeving instellen
> * Een firewall implementeren
> * Een standaardroute maken
> * Toepassingsregels configureren
> * Netwerkregels configureren
> * De firewall testen



Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze zelfstudie maakt u één VNet met drie subnetten:
- **FW-SN** – De firewall bevindt zich in dit subnet.
- **Workload-SN** – De workloadserver bevindt zich in dit subnet. Het netwerkverkeer van dit subnet gaat via de firewall.
- **Jump-SN** – De "jumpserver" bevindt zich in dit subnet. De jumpserver heeft een openbaar IP-adres waarmee u verbinding kunt maken via een extern bureaublad. Van daaruit kunt u vervolgens (via een ander extern bureaublad) verbinding maken met de workloadserver.

![Netwerkinfrastructuur van zelfstudie](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Deze zelfstudie maakt gebruik van een vereenvoudigde netwerkconfiguratie voor gemakkelijke implementatie. Voor productie-implementaties wordt een [hub-en-spoke-model](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) aanbevolen, waarbij de firewall zich in zijn eigen VNet bevindt en workloadservers zich in gekoppelde VNets bevinden in dezelfde regio met één of meer subnetten.



## <a name="set-up-the-network-environment"></a>De netwerkomgeving instellen
Maak eerst een resourcegroep met de resources die nodig zijn om de firewall te implementeren. Maak vervolgens een VNet, subnetten en testservers.

### <a name="create-a-resource-group"></a>Een resourcegroep maken
1. Meld u aan bij de Azure-portal op [http://portal.azure.com](http://portal.azure.com).
1. Klik op de startpagina van de Azure-portal op **Resourcegroepen** en klik vervolgens op **Toevoegen**.
2. Bij **Resourcegroepnaam** typt u **Test-FW-RG**.
3. Bij **Abonnement** selecteert u uw abonnement.
4. Bij **Resourcegroeplocatie** selecteert u een locatie. Alle volgende resources die u maakt, moeten zich op dezelfde locatie bevinden.
5. Klik op **Create**.


### <a name="create-a-vnet"></a>Een VNet maken
1. Klik op de startpagina van de Azure-portal op **Alle services**.
2. Klik onder **Netwerken** op **Virtuele netwerken**.
3. Klik op **Add**.
4. Bij **Naam** typt u **Test-FW-VN**.
5. Bij **Adresruimte** typt u **10.0.0.0/16**.
7. Bij **Abonnement** selecteert u uw abonnement.
8. Bij **Resourcegroep** selecteert u **Bestaande gebruiken** en selecteert u vervolgens **Test-FW-RG**.
9. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
10. Onder **Subnet** typt u bij **Naam** de naam **AzureFirewallSubnet**. De firewall zal zich in dit subnet bevinden, en de subnetnaam **moet** AzureFirewallSubnet zijn.
11. Bij **Adresbereik** typt u **10.0.1.0/24**.
12. Gebruik de andere standaardinstellingen en klik vervolgens op **Maken**.

> [!NOTE]
> De minimale grootte van het subnet AzureFirewallSubnet is /25.

### <a name="create-additional-subnets"></a>Extra subnetten maken

Maak vervolgens subnetten voor de jumpserver en een subnet voor de workloadservers.

1. Klik op de startpagina van de Azure-portal op **Resourcegroepen** en klik vervolgens op **Test-FW-RG**.
2. Klik op het virtuele netwerk **Test-FW-VN**.
3. Klik op **Subnetten** en klik vervolgens op **+Subnet**.
4. Bij **Naam** typt u **Workload-SN**.
5. Bij **Adresbereik** typt u **10.0.2.0/24**.
6. Klik op **OK**.

Maak nog een subnet met **Jump-SN** als naam en **10.0.3.0/24** als adresbereik.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak nu de virtuele jump- en workloadmachines en plaats ze in de toepasselijke subnetten.

1. Klik op de startpagina van de Azure-portal op **Alle services**.
2. Klik onder **Compute** op **Virtuele machines**.
3. Klik achtereenvolgens op **Toevoegen**, **Windows Server**, **Windows Server 2016 Datacenter** en **Maken**.

**Basisinstellingen**

1. Bij **Naam** typt u **Srv-Jump**.
5. Typ een gebruikersnaam en wachtwoord.
6. Bij **Abonnement** selecteert u uw abonnement.
7. Bij **Resourcegroep** klikt u op **Bestaande gebruiken** en selecteert u vervolgens **Test-FW-RG**.
8. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
9. Klik op **OK**.

**Grootte**

1. Kies een geschikte grootte voor een virtuele testmachine waarop Windows Server wordt uitgevoerd, bijvoorbeeld **B2ms** (8 GB RAM, 16 GB opslag).
2. Klik op **Selecteren**.

**Instellingen**

1. Onder **Netwerk** selecteert u bij **Virtueel netwerk** de optie **Test-FW-VN**.
2. Bij **Subnet** selecteert u **Jump-SN**.
3. Bij **Openbare poorten voor inkomend verkeer selecteren** selecteert u **RDP (3389)**. 

    Het is raadzaam de toegang tot uw openbare IP-adres te beperken, maar u moet poort 3389 openen zodat u een extern bureaublad met de jumpserver kunt verbinden. 
2. Behoud de andere standaardinstellingen en klik op **OK**.

**Samenvatting**

Controleer de samenvatting en klik vervolgens op **Maken**. Het duurt enkele minuten voordat dit is voltooid.

Herhaal dit proces om nog een virtuele machine te maken, genaamd **Srv-Work**.

Gebruik de informatie in de volgende tabel om de **Instellingen** voor de virtuele machine Srv-Work te configureren. De rest van de configuratie is hetzelfde als voor de virtuele machine Srv-Jump.


|Instelling  |Waarde  |
|---------|---------|
|Subnet|Workload-SN|
|Openbaar IP-adres|Geen|
|Openbare poorten voor inkomend verkeer selecteren|Geen openbare poorten voor inkomend verkeer|


## <a name="deploy-the-firewall"></a>De firewall implementeren

1. Klik op de startpagina van de portal op **Een resource maken**.
2. Klik op **Netwerken** en klik bij **Aanbevolen** op **Alles weergeven**.
3. Klik op **Firewall** en klik vervolgens op **Maken**. 
4. Gebruik op de pagina **Firewall maken** de volgende tabel om de firewall te configureren:
   
   |Instelling  |Waarde  |
   |---------|---------|
   |Naam     |Test-FW01|
   |Abonnement     |\<uw abonnement\>|
   |Resourcegroep     |**Bestaande gebruiken**: Test-FW-RG |
   |Locatie     |Selecteer dezelfde locatie die u eerder hebt gebruikt|
   |Een virtueel netwerk kiezen     |**Bestaande gebruiken**: Test-FW-VN|
   |Openbaar IP-adres     |**Nieuwe maken**. Het openbare IP-adres moet van het type Standaard-SKU zijn.|

2. Klik op **Controleren + maken**.
3. Controleer de samenvatting en klik vervolgens op **Maken** om de firewall te maken.

   Het duurt enkele minuten voordat deze is geïmplementeerd.
4. Zodra de implementatie is voltooid, gaat u naar de resourcegroep **Test-FW-RG** en klikt u op de firewall **Test-FW01**.
6. Noteer het privé-IP-adres. U zult het later gebruiken wanneer u de standaardroute maakt.


## <a name="create-a-default-route"></a>Een standaardroute maken

Voor het subnet **Workload-SN** configureert u de standaardroute voor uitgaand verkeer om via de firewall te gaan.

1. Klik op de startpagina van de Azure-portal op **Alle services**.
2. Klik onder **Netwerken** op **Routetabellen**.
3. Klik op **Add**.
4. Bij **Naam** typt u **Firewall-route**.
5. Bij **Abonnement** selecteert u uw abonnement.
6. Bij **Resourcegroep** selecteert u **Bestaande gebruiken** en selecteert u vervolgens **Test-FW-RG**.
7. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
8. Klik op **Create**.
9. Klik op **Vernieuwen** en klik vervolgens op de routetabel **Firewall-route**.
10. Klik op **Subnetten** en klik vervolgens op **Koppelen**.
11. Klik op **Virtueel netwerk** en selecteer vervolgens **Test-FW-VN**.
12. Klik bij **Subnet** op **Workload-SN**.
13. Klik op **OK**.
14. Klik op **Routes** en klik vervolgens op **Toevoegen**.
15. Bij **Routenaam** typt u **FW-DG**.
16. Bij **Adresvoorvoegsel** typt u **0.0.0.0/0**.
17. Bij **Volgend hoptype** selecteert u **Virtueel apparaat**.

    Azure Firewall is eigenlijk een beheerde service, maar Virtueel apparaat werkt in deze situatie.
1. Bij **Adres van de volgende hop** typt u het privé-IP-adres voor de firewall dat u eerder hebt genoteerd.
2. Klik op **OK**.


## <a name="configure-application-rules"></a>Toepassingsregels configureren


1. Open de resourcegroep **Test-FW-RG** en klik op de firewall **Test-FW01**.
2. Klik op de pagina **Test-FW01** onder **Instellingen** op **Regels**.
3. Klik op **Toepassingsregelverzameling toevoegen**.
4. Bij **Naam** typt u **App-Coll01**.
5. Bij **Prioriteit** typt u **200**.
6. Bij **Actie** selecteert u **Toestaan**.
7. Onder **Regels** typt u bij **Naam** de naam **AllowGH**.
8. Bij **Bronadressen** typt u **10.0.2.0/24**.
9. Bij **Protocol:poort** typt u **http, https**. 
10. Bij **Doel-FQDN’s** typt u **github.com**.
11. Klik op **Add**.

Azure Firewall bevat een ingebouwde regelverzameling voor infrastructuur-FQDN’s die standaard zijn toegestaan. Deze FQDN’s zijn specifiek voor het platform en kunnen niet voor andere doeleinden worden gebruikt. Zie [FQDN's voor infrastructuur](infrastructure-fqdns.md) voor meer informatie.

## <a name="configure-network-rules"></a>Netwerkregels configureren

1. Klik op **Netwerkregelverzameling toevoegen**.
2. Bij **Naam** typt u **Net-Coll01**.
3. Bij **Prioriteit** typt u **200**.
4. Bij **Actie** selecteert u **Toestaan**.

6. Onder **Regels** typt u bij **Naam** de naam **AllowDNS**.
8. Bij **Protocol** selecteert u **UDP**.
9. Bij **Bronadressen** typt u **10.0.2.0/24**.
10. Bij doeladres typt u **209.244.0.3,209.244.0.4**.
11. Bij **Doelpoorten** typt u **53**.
12. Klik op **Add**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Het primaire en secundaire DNS-adres voor de netwerkinterface **Srv-Work** wijzigen

Voor testdoeleinden in deze zelfstudie configureert u het primaire en secundaire DNS-adres. Dit is geen algemene Azure Firewall-vereiste. 

1. Open de resourcegroep **Test-FW-RG** in de Azure-portal.
2. Klik op de netwerkinterface voor de virtuele machine **Srv-Work**.
3. Klik onder **Instellingen** op **DNS-servers**.
4. Klik onder **DNS-servers** op **Aangepast**.
5. Typ **209.244.0.3** in het tekstvak **DNS-server toevoegen** en **209.244.0.4** in het volgende tekstvak.
6. Klik op **Opslaan**. 
7. Start de virtuele machine **Srv-Work** opnieuw.


## <a name="test-the-firewall"></a>De firewall testen

1. Controleer in de Azure-portal de netwerkinstellingen voor de virtuele machine **Srv-Work** en noteer het privé-IP-adres.
2. Verbind een extern bureaublad met de virtuele machine **Srv-Jump** en open van daaruit een externe bureaubladverbinding met het privé-IP-adres van **Srv-Work**.

5. Open Internet Explorer en blader naar http://github.com.
6. Klik op **OK** en **Sluiten** bij de beveiligingswaarschuwingen.

   U zou nu de startpagina van GitHub moeten zien.

7. Blader naar http://www.msn.com.

   U zou nu door de firewall moeten worden geblokkeerd.

Nu u hebt geverifieerd dat de firewallregels werken:

- Kunt u bladeren naar de enige toegestane FQDN, maar niet naar andere.
- Kunt u DNS-namen omzetten met behulp van de geconfigureerde externe DNS-server.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de firewall-resources voor de volgende zelfstudie bewaren. Als u ze niet meer nodig hebt, verwijdert u de resourcegroep **Test-FW-RG** om alle firewall-gerelateerde resources te verwijderen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een firewall maken
> * Een standaardroute maken
> * Toepassings- en netwerkfirewallregels configureren
> * De firewall testen

Als volgende kunt u de Azure Firewall-logboeken bewaken.

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)

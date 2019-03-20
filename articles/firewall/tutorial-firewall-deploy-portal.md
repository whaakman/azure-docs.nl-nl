---
title: 'Zelfstudie: Azure Firewall implementeren en configureren met de Azure-portal'
description: In deze zelfstudie leert u hoe u Azure Firewall kunt implementeren en configureren via de Azure-portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 3/14/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 288a6e1b1d88fcef6fbd5554ba811acc1dab776e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994257"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Zelfstudie: Azure Firewall implementeren en configureren met de Azure-portal

Het beheren van toegang tot uitgaande netwerken is een belangrijk onderdeel van een algemeen netwerkbeveiligingsabonnement. Zo wilt u wellicht de toegang tot websites, uitgaande IP-adressen of poorten beperken.

Een van de manieren waarop u de toegang tot uitgaande netwerken kunt beheren vanaf een Azure-subnet is met Azure Firewall. Met Azure Firewall kunt u het volgende configureren:

* Toepassingsregels die volledig gekwalificeerde domeinnamen (FQDN's) definiëren waartoe toegang kan worden verkregen via een subnet.
* Netwerkregels die een bronadres, protocol, doelpoort en doeladres definiëren.

Netwerkverkeer is onderhevig aan de geconfigureerde firewallregels wanneer u het routeert naar de firewall als standaardgateway van het subnet.

Voor deze zelfstudie maakt u één vereenvoudigd VNet met drie subnetten voor eenvoudige implementatie. Voor productie-implementaties wordt een [hub-en-spoke-model](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) aanbevolen, waarbij de firewall zich in zijn eigen VNet bevindt en workloadservers zich in gekoppelde VNets bevinden in dezelfde regio met één of meer subnetten.

- **AzureFirewallSubnet** – De firewall bevindt zich in dit subnet.
- **Workload-SN** – De workloadserver bevindt zich in dit subnet. Het netwerkverkeer van dit subnet gaat via de firewall.
- **Jump-SN** – De "jumpserver" bevindt zich in dit subnet. De jumpserver heeft een openbaar IP-adres waarmee u verbinding kunt maken via een extern bureaublad. Van daaruit kunt u vervolgens (via een ander extern bureaublad) verbinding maken met de workloadserver.

![Netwerkinfrastructuur van zelfstudie](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een testnetwerkomgeving instellen
> * Een firewall implementeren
> * Een standaardroute maken
> * Een toepassing configureren om toegang tot msn.com toe te staan
> * Een netwerkregel configureren om toegang tot externe DNS-servers toe te staan
> * De firewall testen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="set-up-the-network"></a>Het netwerk instellen

Maak eerst een resourcegroep met de resources die nodig zijn om de firewall te implementeren. Maak vervolgens een VNet, subnetten en testservers.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

De resourcegroep bevat alle resources voor de zelfstudie.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik op de startpagina van de Azure-portal op **Resourcegroepen** > **Toevoegen**.
3. Bij **Resourcegroepnaam** typt u **Test-FW-RG**.
4. Bij **Abonnement** selecteert u uw abonnement.
5. Bij **Resourcegroeplocatie** selecteert u een locatie. Alle volgende resources die u maakt, moeten zich op dezelfde locatie bevinden.
6. Klik op **Create**.

### <a name="create-a-vnet"></a>Een VNet maken

Dit VNet bevat drie subnetten.

1. Klik op de startpagina van de Azure-portal op **Alle services**.
2. Klik onder **Netwerken** op **Virtuele netwerken**.
3. Klik op **Add**.
4. Bij **Naam** typt u **Test-FW-VN**.
5. Bij **Adresruimte** typt u **10.0.0.0/16**.
6. Bij **Abonnement** selecteert u uw abonnement.
7. Voor **Resourcegroep** selecteert u **Bestaande gebruiken** > **Test-FW-RG**.
8. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
9. Onder **Subnet** typt u bij **Naam** de naam **AzureFirewallSubnet**. De firewall zal zich in dit subnet bevinden, en de subnetnaam **moet** AzureFirewallSubnet zijn.
10. Bij **Adresbereik** typt u **10.0.1.0/24**.
11. Gebruik de andere standaardinstellingen en klik vervolgens op **Maken**.

> [!NOTE]
> De minimale grootte van het subnet AzureFirewallSubnet is /26.

### <a name="create-additional-subnets"></a>Extra subnetten maken

Maak vervolgens subnetten voor de jumpserver en een subnet voor de workloadservers.

1. Klik op de startpagina van de Azure-portal op **Resourcegroepen** > **Test-FW-RG**.
2. Klik op het virtuele netwerk **Test-FW-VN**.
3. Klik op **Subnetten** > **+Subnet**.
4. Bij **Naam** typt u **Workload-SN**.
5. Bij **Adresbereik** typt u **10.0.2.0/24**.
6. Klik op **OK**.

Maak nog een subnet met **Jump-SN** als naam en **10.0.3.0/24** als adresbereik.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak nu de virtuele jump- en workloadmachines en plaats ze in de toepasselijke subnetten.

1. Klik in Azure Portal op **Een resource maken**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *Test-FW-RG* voor de resourcegroep.
    - *Srv-Jump* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.

4. Klik voor **Openbare binnenkomende poorten** onder **Regels voor binnenkomende poort** op **Geselecteerde poorten toestaan**.
5. Selecteer **RDP (3389)** voor **Binnenkomende poorten selecteren**.

6. Accepteer de overige standaardwaarden en klik op **Volgende: Schijven**.
7. Accepteer de standaardwaarden voor schijven en klik op **Volgende: Netwerken**.
8. Zorg ervoor dat **Test-FW-VN** is geselecteerd voor het virtuele netwerk en dat het subnet **Jump-SN** is.
9. Klik voor **Openbaar IP** op **Nieuwe maken**.
10. Typ **Srv-Jump-PIP** voor de naam van het openbare IP-adres en klik op **OK**.
11. Accepteer de overige standaardwaarden en klik op **Volgende: Beheer**.
12. Klik op **Uit** om diagnostische gegevens over opstarten uit te schakelen. Accepteer de overige standaardwaarden en klik op **Review + create**.
13. Controleer de instellingen op de overzichtspagina en klik op **Maken**.

Herhaal dit proces om nog een virtuele machine te maken, genaamd **Srv-Work**.

Gebruik de informatie in de volgende tabel om de virtuele machine Srv-Work te configureren. De rest van de configuratie is hetzelfde als voor de virtuele machine Srv-Jump.

|Instelling  |Value  |
|---------|---------|
|Subnet|Workload-SN|
|Openbare IP|Geen|
|Openbare poorten voor inkomend verkeer|Geen|

## <a name="deploy-the-firewall"></a>De firewall implementeren

Implementeer de firewall in het VNet.

1. Klik op de startpagina van de portal op **Een resource maken**.
2. Klik op **Netwerken** en klik bij **Aanbevolen** op **Alles weergeven**.
3. Klik op **Firewall** > **Maken**. 
4. Gebruik op de pagina **Firewall maken** de volgende tabel om de firewall te configureren:

   |Instelling  |Waarde  |
   |---------|---------|
   |Name     |Test-FW01|
   |Abonnement     |\<uw abonnement\>|
   |Resourcegroep     |**Gebruik bestaande**: Test-FW-RG |
   |Locatie     |Selecteer dezelfde locatie die u eerder hebt gebruikt|
   |Een virtueel netwerk kiezen     |**Gebruik bestaande**: Test-FW-VN|
   |Openbaar IP-adres     |**Nieuwe maken**. Het openbare IP-adres moet van het type Standaard-SKU zijn.|

5. Klik op **Controleren + maken**.
6. Controleer de samenvatting en klik vervolgens op **Maken** om de firewall te maken.

   Het duurt enkele minuten voordat deze is geïmplementeerd.
7. Zodra de implementatie is voltooid, gaat u naar de resourcegroep **Test-FW-RG** en klikt u op de firewall **Test-FW01**.
8. Noteer het privé-IP-adres. U zult het later gebruiken wanneer u de standaardroute maakt.

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
10. Klik op **Subnetten** > **Koppelen**.
11. Klik op **Virtueel netwerk** > **Test-FW-VN**.
12. Klik bij **Subnet** op **Workload-SN**. Zorg ervoor dat u alleen selecteert de **Workload-SN** subnet voor deze route, anders uw firewall werkt niet correct.

13. Klik op **OK**.
14. Klik op **Routes** > **Toevoegen**.
15. Bij **Routenaam** typt u **FW-DG**.
16. Bij **Adresvoorvoegsel** typt u **0.0.0.0/0**.
17. Bij **Volgend hoptype** selecteert u **Virtueel apparaat**.

    Azure Firewall is eigenlijk een beheerde service, maar Virtueel apparaat werkt in deze situatie.
18. Bij **Adres van de volgende hop** typt u het privé-IP-adres voor de firewall dat u eerder hebt genoteerd.
19. Klik op **OK**.

## <a name="configure-an-application-rule"></a>Een toepassingsregel configureren

Dit is de toepassingsregel waarmee uitgaande toegang wordt toegestaan tot msn.com.

1. Open de resourcegroep **Test-FW-RG** en klik op de firewall **Test-FW01**.
2. Klik op de pagina **Test-FW01** onder **Instellingen** op **Regels**.
3. Klik op het tabblad **Verzameling met toepassingsregels**.
4. Klik op **Toepassingsregelverzameling toevoegen**.
5. Bij **Naam** typt u **App-Coll01**.
6. Bij **Prioriteit** typt u **200**.
7. Bij **Actie** selecteert u **Toestaan**.
8. Onder **Regels**, **Doel-FQDN's**, typt u bij **Naam** **AllowGH**.
9. Bij **Bronadressen** typt u **10.0.2.0/24**.
10. Bij **Protocol:poort** typt u **http, https**.
11. Bij **Doel-FQDN’s** typt u **msn.com**.
12. Klik op **Add**.

Azure Firewall bevat een ingebouwde regelverzameling voor infrastructuur-FQDN’s die standaard zijn toegestaan. Deze FQDN’s zijn specifiek voor het platform en kunnen niet voor andere doeleinden worden gebruikt. Zie [FQDN's voor infrastructuur](infrastructure-fqdns.md) voor meer informatie.

## <a name="configure-a-network-rule"></a>Een netwerkregel configureren

Dit is de netwerkregel waarmee uitgaande toegang tot twee IP-adressen op poort 53 (DNS) wordt toegestaan.

1. Klik op het tabblad **Verzameling met netwerkregels**.
2. Klik op **Netwerkregelverzameling toevoegen**.
3. Bij **Naam** typt u **Net-Coll01**.
4. Bij **Prioriteit** typt u **200**.
5. Bij **Actie** selecteert u **Toestaan**.

6. Onder **Regels** typt u bij **Naam** de naam **AllowDNS**.
7. Bij **Protocol** selecteert u **UDP**.
8. Bij **Bronadressen** typt u **10.0.2.0/24**.
9. Bij doeladres typt u **209.244.0.3,209.244.0.4**.
10. Bij **Doelpoorten** typt u **53**.
11. Klik op **Add**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Het primaire en secundaire DNS-adres voor de netwerkinterface **Srv-Work** wijzigen

Voor testdoeleinden in deze zelfstudie configureert u het primaire en secundaire DNS-adres. Dit is een algemene vereiste voor de Firewall van Azure niet.

1. Open de resourcegroep **Test-FW-RG** in de Azure-portal.
2. Klik op de netwerkinterface voor de virtuele machine **Srv-Work**.
3. Klik onder **Instellingen** op **DNS-servers**.
4. Klik onder **DNS-servers** op **Aangepast**.
5. Typ **209.244.0.3** in het tekstvak **DNS-server toevoegen** en **209.244.0.4** in het volgende tekstvak.
6. Klik op **Opslaan**. 
7. Start de virtuele machine **Srv-Work** opnieuw.

## <a name="test-the-firewall"></a>De firewall testen

Test nu de firewall om te controleren of deze werkt zoals verwacht.

1. Controleer in de Azure-portal de netwerkinstellingen voor de virtuele machine **Srv-Work** en noteer het privé-IP-adres.
2. Verbind een extern bureaublad met de virtuele machine **Srv-Jump** en open van daaruit een externe bureaubladverbinding met het privé-IP-adres van **Srv-Work**.

3. Open Internet Explorer en blader naar https://msn.com.
4. Klik op **OK** > **Sluiten** bij de beveiligingswaarschuwingen.

   U zou nu de startpagina van MSN moeten zien.

5. Blader naar https://www.msn.com.

   U zou nu door de firewall moeten worden geblokkeerd.

U hebt nu gecontroleerd of de firewall-regels zijn werken:

- Kunt u bladeren naar de enige toegestane FQDN, maar niet naar andere.
- Kunt u DNS-namen omzetten met behulp van de geconfigureerde externe DNS-server.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de firewall-resources voor de volgende zelfstudie bewaren. Als u ze niet meer nodig hebt, verwijdert u de resourcegroep **Test-FW-RG** om alle firewall-gerelateerde resources te verwijderen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)

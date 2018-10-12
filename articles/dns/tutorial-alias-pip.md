---
title: 'Zelfstudie: Een Azure DNS-aliasrecord zo configureren dat die naar een openbaar Azure-IP-adres verwijst.'
description: In deze zelfstudie leert u hoe u een Azure DNS-aliasrecord zo configureert dat die naar een openbaar Azure-IP-adres verwijst.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 842015d853790e3ac78214cca6a70becb7f9fadf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991218"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Zelfstudie: Een aliasrecord zo configureren dat die naar een openbaar Azure-IP-adres verwijst 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een netwerkinfrastructuur maken
> * Een virtuele webserver maken
> * Een aliasrecord maken
> * Het aliasrecord testen


Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
U moet een beschikbare domeinnaam hebben die u in Azure DNS kunt hosten om te testen. U moet het volledige beheer over dit domein hebben, inclusief de mogelijkheid om naamserverrecords (NS) voor het domein in te stellen.

Zie voor instructies voor het hosten van uw domein in Azure DNS [Zelfstudie: Uw domein in Azure DNS hosten](dns-delegate-domain-azure-dns.md).

Het voorbeelddomein dat wordt gebruikt voor deze zelfstudie is contoso.com is, maar moet uw eigen domeinnaam gebruiken.

## <a name="create-the-network-infrastructure"></a>De netwerkinfrastructuur maken
Maak eerst een VNet en een subnet waaraan u de webservers gaat toevoegen.
1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Klik in de linkerbovenhoek in de portal op **Een resource maken**, typ *resourcegroep* in het zoekvak en maak een resourcegroep met de naam **RG-DNS-Alias-pip**.
3. Klik op **Een resource maken** klik op **Netwerken** en klik vervolgens op **Virtueel netwerk**.
4. Maak een virtueel netwerk met de naam **VNet-Server**, plaats het in de resourcegroep **RG-DNS-Alias-pip** en geef het subnet de naam **SN-Web**.

## <a name="create-a-web-server-virtual-machine"></a>Een virtuele webserver maken
1. Klik op **Een resource maken**, klik op **Virtuele machine met Windows Server 2016**.
2. Typ **Web-01** als naam en plaats de virtuele machine in de resourcegroep **RG-DNS-Alias-TM**. Typ een gebruikersnaam en wachtwoord en klik op **OK**.
3. Kies voor de **Grootte** een SKU met 8 GB RAM.
4. Selecteer voor **Instellingen** het virtuele netwerk **VNet-Servers** en het subnet **SN-Web**. Selecteer voor openbare binnenkomende poorten **HTTP**, **HTTPS**, en **RDP (3389)** en klik vervolgens op **OK**.
5. Klik op de overzichtspagina op **Maken**.

   Dit duurt enkele minuten.

### <a name="install-iis"></a>IIS installeren

Installeren IIS op **Web01**.

1. Maak verbinding met **Web-01** en meld u aan.
2. Klik op het dashboard Serverbeheer op **Functies en onderdelen toevoegen**.
3. Klik drie maal op **Volgende** en selecteer op de pagina **Serverfuncties** de optie **Webserver (IIS)**.
4. Klik op **Functies toevoegen** en vervolgens op **Volgende**.
5. Klik vier maal op **Volgende** en klik vervolgens op **Installeren**.

   Het duurt enkele minuten voordat dit is voltooid.
6. Wanneer de installatie is voltooid, klikt u op **Sluiten**.
7. Open een webbrowser en ga naar **localhost** om te controleren of de IIS-standaardwebpagina wordt weergegeven.

## <a name="create-an-alias-record"></a>Een aliasrecord maken

Maak een aliasrecord dat naar het openbare IP-adres verwijst.

1. Klik op uw Azure DNS-zone om de zone te openen.
2. Klik op **Recordset**.
3. Typ **web01** in het tekstvak **Naam**.
4. Laat het **Type** een **A**-record.
5. Klik op het selectievakje **Alias recordset**.
6. Klik op **Azure-service kiezen** en selecteer het openbare IP-adres **Web-01-ip**.

## <a name="test-the-alias-record"></a>Het aliasrecord testen

1. Klik in de resourcegroep **RG-DNS-Alias-pip** op de virtuele machine **Web-01**. Noteer het openbare IP-adres.
1. Ga in een webbrowser naar de Fully Qualified Domain Name voor de virtuele machine Web01-01. Bijvoorbeeld: **web01.contoso.com**. De IIS-standaardwebpagina moet worden weergegeven.
2. Sluit de webbrowser.
3. Stop de virtuele machine **Web-01** en start deze opnieuw.
4. Wanneer de machine opnieuw is opgestart, noteert u het nieuwe openbare IP-adres van de virtuele machine.
5. Open een webbrowser en ga nogmaals naar de Fully Qualified Domain Name voor de virtuele machine Web01-01. Bijvoorbeeld: **web01.contoso.com**.
6. Dit moet nog steeds lukken, omdat u een aliasrecord hebt gebruikt om te verwijzen naar de openbare IP-adresresource en niet naar een standaard A-record.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep **RG-DNS-Alias-pip** als u deze niet langer nodig hebt om de resources die u in deze zelfstudie hebt gemaakt te verwijderen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een aliasrecord zo configureert dat die naar een openbaar Azure-IP-adres verwijst. Als u meer wilt weten over Azure DNS en web-apps gaat u verder met de zelfstudie voor web-apps.

> [!div class="nextstepaction"]
> [DNS-records voor een web-app in een aangepast domein maken](./dns-web-sites-custom-domain.md)

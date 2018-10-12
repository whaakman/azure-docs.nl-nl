---
title: 'Zelfstudie: een Azure DNS-aliasrecord maken ter ondersteuning van hoofddomeinnamen met Traffic Manager'
description: In deze zelfstudie wordt uitgelegd hoe u een Azure DNS-aliasrecord kunt configureren om het gebruik van uw hoofddomeinnaam met Traffic Manager te ondersteunen.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 4a5d8968861f6f2938e605d2f7106529ca401df4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967435"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Zelfstudie: een Azure DNS-aliasrecord configureren om het gebruik van hoofddomeinnaam met Traffic Manager te ondersteunen 

U kunt een aliasrecord maken voor uw hoofddomeinnaam (bijvoorbeeld contoso.com) om te verwijzen naar een Traffic Manager-profiel. Dus in plaats van hiervoor een omleidingsservice te gebruiken, kunt u Azure DNS zo configureren dat deze rechtstreeks vanuit uw zone naar een Traffic Manager-profiel verwijst. 


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een host-VM en netwerkinfrastructuur maken
> * Een Traffic Manager-profiel maken
> * Een aliasrecord maken
> * De aliasrecord testen


Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
U moet een beschikbare domeinnaam hebben die u in Azure DNS kunt hosten om te testen. U moet het volledige beheer over dit domein hebben, inclusief de mogelijkheid om naamserverrecords (NS) voor het domein in te stellen.

Zie voor instructies voor het hosten van uw domein in Azure DNS [Zelfstudie: Uw domein in Azure DNS hosten](dns-delegate-domain-azure-dns.md).

Het voorbeelddomein dat wordt gebruikt voor deze zelfstudie is contoso.com, maar u moet uw eigen domeinnaam gebruiken.

## <a name="create-the-network-infrastructure"></a>De netwerkinfrastructuur maken
Maak eerst een VNet en een subnet waaraan u de webservers gaat toevoegen.
1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Klik in de linkerbovenhoek in de portal op **Een resource maken**, typ *resourcegroep* in het zoekvak en maak een resourcegroep met de naam **RG-DNS-Alias-TM**.
3. Klik op **Een resource maken**, klik op **Netwerken** en klik vervolgens op **Virtueel netwerk**.
4. Maak een virtueel netwerk met de naam **VNet-Servers**, plaats het in de resourcegroep **RG-DNS-Alias-TM** en geef het subnet de naam **SN-Web**.

## <a name="create-two-web-server-virtual-machines"></a>Maak twee virtuele machines die als webserver dienen
1. Klik op **Een resource maken** en klik op **Virtuele machine met Windows Server 2016**.
2. Typ **Web-01** als naam en plaats de virtuele machine in de resourcegroep **RG-DNS-Alias-TM**. Typ een gebruikersnaam en wachtwoord en klik op **OK**.
3. Kies voor de **Grootte** een SKU met 8 GB RAM.
4. Selecteer voor **Instellingen** het virtuele netwerk **VNet-Servers** en het subnet **SN-Web**.
5. Klik op **Openbaar IP-adres** en klik onder **Toewijzing** op **Statisch**. Klik vervolgens op **OK**.
6. Selecteer voor openbare binnenkomende poorten **HTTP**, **HTTPS**, en **RDP (3389)** en klik op **OK**.
7. Klik op de overzichtspagina op **Maken**.

   Dit duurt enkele minuten.
6. Herhaal dit proces om nog een virtuele machine te maken, genaamd **Web-02**.

### <a name="add-a-dns-label"></a>Een DNS-label toevoegen
De openbare IP-adressen hebben een DNS-label nodig om te werken met Traffic Manager.
1. Klik in de resourcegroep **RG-DNS-Alias-TM** op het openbare IP-adres **Web-01-ip**.
2. Klik onder **Instellingen** op **Configuratie**.
3. Typ in het tekstvak voor het DNS-naamlabel **web01pip**.
4. Klik op **Opslaan**.

Herhaal deze procedure voor het openbare IP-adres **Web-02-ip** en gebruik **web02pip** voor het DNS-naamlabel.

### <a name="install-iis"></a>IIS installeren

Installeer IIS op zowel **Web-01** als **Web-02**.

1. Maak verbinding met **Web-01** en meld u aan.
2. Klik op het dashboard Serverbeheer op **Functies en onderdelen toevoegen**.
3. Klik driemaal op **Volgende** en selecteer op de pagina **Serverfuncties** de optie **Webserver (IIS)**.
4. Klik op **Functies toevoegen** en vervolgens op **Volgende**.
5. Klik viermaal op **Volgende** en klik vervolgens op **Installeren**.

   Het duurt enkele minuten voordat dit is voltooid.
6. Wanneer de installatie is voltooid, klikt u op **Sluiten**.
7. Open een webbrowser en ga naar **localhost** om te controleren of de IIS-standaardwebpagina wordt weergegeven.

Herhaal dit proces om IIS te installeren op **Web-02**.


## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

Due 

1. Open de resourcegroep **RG-DNS-Alias-TM** en klik op het openbare IP-adres **Web-01-ip**. Noteer het IP-adres voor later gebruik. Herhaal dit voor het openbare IP-adres **Web-02-ip**.
1. Klik op **Een resource maken**, klik op **Netwerken** en klik vervolgens op **Traffic Manager-profiel**.
2. Typ **TM-alias-test** als naam en plaats de virtuele machine in de resourcegroep **RG-DNS-Alias-TM**.
3. Klik op **Create**.
4. Wanneer de implementatie is voltooid, klikt u op **Naar de resource gaan**.
5. Klik op de Traffic Manager-profielpagina onder **Instellingen**op **Eindpunten**.
6. Klik op **Add**.
7. Selecteer **Extern eindpunt** voor **Type**, en typ **EP-Web01** bij **Naam**.
8. Typ in het tekstvak **Fully-qualified domain name (FQDN) of IP** het IP-adres voor **Web-01-ip** dat u eerder hebt genoteerd.
9. Selecteer dezelfde **Locatie** als uw andere resources, en klik vervolgens op **OK**.

Herhaal deze procedure om het eindpunt **Web-02** toe te voegen met behulp van het IP-adres dat u eerder hebt genoteerd voor **Web-02-ip**.

## <a name="create-an-alias-record"></a>Een aliasrecord maken

Maak een aliasrecord dat naar het Traffic Manager-profiel verwijst.

1. Klik op uw Azure DNS-zone om de zone te openen.
2. Klik op **Recordset**.
3. Laat het tekstvak **Naam** leeg om de hoofddomeinnaam te vertegenwoordigen (bijvoorbeeld contoso.com).
4. Laat het **Type** een **A**-record.
5. Klik op het selectievakje **Alias recordset**.
6. Klik op **Azure-service kiezen** en selecteer het Traffic Manager-profiel **TM-alias-test**.

## <a name="test-the-alias-record"></a>De aliasrecord testen

1. Blader via een webbrowser naar uw hoofddomeinnaam (bijvoorbeeld contoso.com). De IIS-standaardwebpagina moet worden weergegeven. Sluit de webbrowser.
2. Schakel de virtuele machine **Web-01** uit en wacht een paar minuten totdat deze volledig is afgesloten.
3. Open een nieuwe webbrowser en blader weer naar uw hoofddomeinnaam.
4. De standaard IIS-pagina zou opnieuw moeten worden weergegeven, omdat Traffic Manager de situatie heeft afgehandeld en verkeer naar **Web-02** heeft geleid.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep **RG-DNS-Alias-TM** als u deze niet langer nodig hebt om de resources die u in deze zelfstudie hebt gemaakt te verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aliasrecord gemaakt om uw hoofddomeinnaam te gebruiken om naar een Traffic Manager-profiel te verwijzen. Als u meer wilt weten over Azure DNS en web-apps gaat u verder met de zelfstudie voor web-apps.

> [!div class="nextstepaction"]
> [DNS-records voor een web-app in een aangepast domein maken](./dns-web-sites-custom-domain.md)

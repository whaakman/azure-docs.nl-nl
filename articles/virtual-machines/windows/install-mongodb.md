---
title: MongoDB installeren op een Windows virtuele machine in Azure | Microsoft Docs
description: Informatie over het MongoDB installeren op een virtuele machine van Azure waarop Windows Server 2012 R2 is gemaakt met het implementatiemodel van Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: f3fe9751467a1fc34f4e9d02855c4aff307424a3
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
ms.locfileid: "26745976"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installeren en configureren van MongoDB op een Windows-VM in Azure
[MongoDB](http://www.mongodb.org) is een populaire open-source, hoogwaardige NoSQL-database. In dit artikel begeleidt u bij het installeren en configureren van MongoDB op een Windows Server 2016 virtuele machine (VM) in Azure. U kunt ook [MongoDB installeren op een Linux VM in Azure](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Vereisten
Voordat u installeren en configureren van MongoDB, moet u een virtuele machine maken en in het ideale geval een gegevensschijf aan toevoegen. Zie de volgende artikelen voor het maken van een virtuele machine en een gegevensschijf toevoegen:

* Maak een Windows Server-VM met [de Azure-portal](quick-create-portal.md) of [Azure PowerShell](quick-create-powershell.md).
* Een gegevensschijf koppelen aan een Windows Server-VM met [de Azure-portal](attach-managed-disk-portal.md) of [Azure PowerShell](attach-disk-ps.md).

Om te beginnen met MongoDB installeren en configureren, [Meld u aan bij uw virtuele machine van Windows Server](connect-logon.md) met behulp van extern bureaublad.

## <a name="install-mongodb"></a>MongoDB installeren
> [!IMPORTANT]
> Beveiligingsfuncties van MongoDB, zoals verificatie en IP-adresbinding zijn niet standaard ingeschakeld. Beveiligingsfuncties moeten worden ingeschakeld voordat u MongoDB in een productieomgeving implementeert. Zie voor meer informatie [MongoDB-beveiliging en verificatie](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Nadat u verbinding hebt gemaakt met uw virtuele machine via Extern bureaublad, opent u Internet Explorer uit vanaf de taakbalk.
2. Selecteer **aanbevolen instellingen voor beveiliging, privacy en compatibiliteit gebruiken** wanneer Internet Explorer eerst wordt geopend en klik op **OK**.
3. Verbeterde beveiliging van Internet Explorer is standaard ingeschakeld. De MongoDB-website toevoegen aan de lijst met toegestane websites:
   
   * Selecteer de **extra** pictogram in de rechterbovenhoek.
   * In **Internetopties**, selecteer de **beveiliging** tabblad en selecteer vervolgens de **vertrouwde websites** pictogram.
   * Klik op de **Sites** knop. Voeg *https://\*. mongodb.com* aan de lijst met vertrouwde sites en sluit het dialoogvenster.
     
     ![Instellingen van Internet Explorer configureren](./media/install-mongodb/configure-internet-explorer-security.png)
4. Blader naar de [MongoDB - Downloads](http://www.mongodb.com/downloads) pagina (http://www.mongodb.com/downloads).
5. Selecteer, indien nodig de **Community Server** edition en selecteer vervolgens de meest recente huidige stabiel vrij te geven voor*Windows Server 2008 R2 64-bits of hoger*. Het installatieprogramma downloaden, klikt u op **downloaden (msi)**.
   
    ![MongoDB-installatieprogramma downloaden](./media/install-mongodb/download-mongodb.png)
   
    Het installatieprogramma uitvoeren nadat het downloaden voltooid is.
6. Lees en accepteer de gebruiksrechtovereenkomst. Wanneer u wordt gevraagd, selecteert u **Complete** installeren.
7. Indien gewenst, kunt u ook installeren kompas, een grafische interface voor MongoDB.
8. Klik op het laatste scherm **installeren**.

## <a name="configure-the-vm-and-mongodb"></a>Configureer de virtuele machine en MongoDB
1. De padvariabelen zijn niet bijgewerkt door het MongoDB-installatieprogramma. Zonder de MongoDB `bin` locatie in de variabele path, moet u het volledige pad opgeven telkens wanneer u een uitvoerbaar bestand van MongoDB gebruiken. De locatie toevoegen aan de variabele path:
   
   * Met de rechtermuisknop op de **Start** menu en selecteer **System**.
   * Klik op **Geavanceerde systeeminstellingen**, en klik vervolgens op **omgevingsvariabelen**.
   * Onder **systeemvariabelen**, selecteer **pad**, en klik vervolgens op **bewerken**.
     
     ![Padvariabelen configureren](./media/install-mongodb/configure-path-variables.png)
     
     Het pad toevoegen aan uw MongoDB `bin` map. MongoDB wordt doorgaans geïnstalleerd in *C:\Program Files\MongoDB*. Controleer of het installatiepad op de virtuele machine. Het volgende voorbeeld wordt de standaard MongoDB-installatielocatie naar de `PATH` variabele:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Zorg ervoor dat het begin van de regel toevoegen (`;`) om aan te geven dat u wilt toevoegen aan een locatie voor uw `PATH` variabele.

2. MongoDB-gegevens en logboekbestanden mappen op de gegevensschijf van uw maken. Van de **Start** selecteert u **opdrachtprompt**. De volgende voorbeelden maken de mappen op station F:
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Start een MongoDB-exemplaar met de volgende opdracht, het aanpassen van het pad naar uw gegevens en meld u mappen dienovereenkomstig:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Het kan enkele minuten voor MongoDB worden de bestanden journaal toegewezen en beginnen met luisteren voor verbindingen duren. Alle berichten in het logboek worden omgeleid naar de *F:\MongoLogs\mongolog.log* opslaan als `mongod.exe` server wordt gestart en wijst journaal-bestanden.
   
   > [!NOTE]
   > De opdrachtprompt blijft gericht zijn op deze taak terwijl het MongoDB-exemplaar wordt uitgevoerd. Sluit het opdrachtpromptvenster om te blijven uitvoeren MongoDB. Of MongoDB installeren als service, zoals beschreven in de volgende stap.

4. Voor een meer robuuste MongoDB-ervaring, installeert u de `mongod.exe` als een service. Een service maakt, dat u hoeft niet te laat een opdrachtprompt uitgevoerd elke keer dat u wilt gebruiken van MongoDB. Maken van de service als volgt het pad naar uw adreslijsten gegevens en logboekbestanden dienovereenkomstig aanpassen:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    De voorgaande opdracht maakt een service met de naam MongoDB, met een beschrijving van "DB met Mongo". Er zijn ook de volgende parameters opgeven:
   
   * De `--dbpath` optie geeft u de locatie van de map data.
   * De `--logpath` optie moet worden gebruikt om op te geven van een logboekbestand, omdat de service niet beschikt over een opdrachtvenster om uitvoer weer te geven.
   * De `--logappend` optie geeft u de uitvoer toe te voegen aan het bestaande bestand zorgt ervoor dat de service opnieuw wordt opgestart.
   
   Voer de volgende opdracht voor het starten van de MongoDB-service:
   
    ```
    net start MongoDB
    ```
   
    Zie voor meer informatie over het maken van de service MongoDB [configureren van een Windows-Service voor MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testen van de MongoDB-exemplaar
Met MongoDB uitgevoerd als één exemplaar of als een service is geïnstalleerd, kunt u nu starten maken en gebruiken van uw databases. Voor het starten van de administratieve MongoDB-shell, een ander opdrachtpromptvenster openen vanuit de **Start** menu en voer de volgende opdracht:

```
mongo  
```

Kunt u de databases met de naam van de `db` opdracht. Voeg enkele gegeven als volgt:

```
db.foo.insert( { a : 1 } )
```

Zoek naar gegevens als volgt:

```
db.foo.find()
```

De uitvoer lijkt op die in het volgende voorbeeld:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Sluit de `mongo` console als volgt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Netwerkbeveiligingsgroep regels en firewall configureren
Nu dat MongoDB geïnstalleerd en wordt uitgevoerd is, opent u een poort in Windows Firewall zodat u op afstand verbinding met MongoDB maken kunt. Open een administratieve PowerShell een opdrachtprompt en voer de volgende opdracht voor het maken van een nieuwe regel binnenkomende verbindingen voor TCP-poort 27017 toestaan:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

U kunt ook de regel maken met behulp van de **Windows Firewall met geavanceerde beveiliging** grafisch beheerprogramma. Maak een nieuwe regel binnenkomende verbindingen voor TCP-poort 27017 toestaan.

Indien nodig, maakt u een regel van de Netwerkbeveiligingsgroep voor toegang tot MongoDB van buiten het bestaande virtuele netwerk van Azure-subnet. U kunt de regels van de Netwerkbeveiligingsgroep maken met behulp van de [Azure-portal](nsg-quickstart-portal.md) of [Azure PowerShell](nsg-quickstart-powershell.md). Net als bij de Windows Firewall-regels toestaan TCP-poort 27017 aan de virtuele netwerkinterface van de MongoDB-VM.

> [!NOTE]
> TCP-poort 27017 is de standaardpoort gebruikt door MongoDB. U kunt deze poort wijzigen met behulp van de `--port` parameter bij het starten van `mongod.exe` handmatig of via een service. Als u de poort wijzigt, zorg er dan voor dat de Windows Firewall- en Netwerkbeveiligingsgroep regels in de voorgaande stappen bijwerken.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe installeren en configureren van MongoDB op uw Windows-VM. U kunt nu toegang tot MongoDB op uw Windows-VM aan de hand van de geavanceerde onderwerpen in de [MongoDB-documentatie](https://docs.mongodb.com/manual/).


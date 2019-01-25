---
title: MongoDB installeren op een Windows-VM in Azure | Microsoft Docs
description: Leer hoe u MongoDB op een virtuele Azure-machine met Windows Server 2012 R2 zijn gemaakt met het implementatiemodel van Resource Manager installeert.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: a45d6a6064173cea7ed15065ab3464718cc8578e
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888818"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installeren en configureren van MongoDB op een Windows-VM in Azure
[MongoDB](http://www.mongodb.org) is een populaire open-source, hoogwaardige NoSQL-database. In dit artikel begeleidt u bij het installeren en configureren van MongoDB op een Windows Server 2016 virtuele machine (VM) in Azure. U kunt ook [MongoDB op een Linux-VM in Azure installeert](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Vereisten
Voordat u installeren en configureren van MongoDB, moet u een virtuele machine maken en in het ideale geval een gegevensschijf toevoegen aan deze. Zie de volgende artikelen voor een virtuele machine maken en een gegevensschijf toevoegen:

* Maak een Windows Server-VM met [de Azure-portal](quick-create-portal.md) of [Azure PowerShell](quick-create-powershell.md).
* Een gegevensschijf koppelen aan een Windows Server-VM met [de Azure-portal](attach-managed-disk-portal.md) of [Azure PowerShell](attach-disk-ps.md).

Om te beginnen met MongoDB installeren en configureren, [Meld u aan bij uw Windows Server-VM](connect-logon.md) met behulp van extern bureaublad.

## <a name="install-mongodb"></a>MongoDB installeren
> [!IMPORTANT]
> MongoDB-beveiligingsfuncties, zoals een verificatie- en IP-adresbinding worden niet standaard ingeschakeld. Beveiligingsfuncties die moeten worden ingeschakeld voordat u MongoDB in een productieomgeving implementeert. Zie voor meer informatie, [MongoDB beveiligings- en Authenticatieservices](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. Nadat u uw virtuele machine via Extern bureaublad verbinding hebt gemaakt, opent u Internet Explorer uit vanaf de taakbalk.
2. Selecteer **gebruik aanbevolen instellingen voor beveiliging, privacy en compatibiliteit** wanneer Internet Explorer eerst wordt geopend, en klikt u op **OK**.
3. Verbeterde beveiliging van Internet Explorer is standaard ingeschakeld. De MongoDB-website toevoegen aan de lijst met toegestane sites:
   
   * Selecteer de **extra** pictogram in de rechterbovenhoek.
   * In **Internetopties**, selecteer de **Security** tabblad, en selecteer vervolgens de **vertrouwde Sites** pictogram.
   * Klik op de **Sites** knop. Voeg *https://\*. mongodb.com* aan de lijst met vertrouwde sites hebben en sluit het dialoogvenster.
     
     ![Instellingen van Internet Explorer configureren](./media/install-mongodb/configure-internet-explorer-security.png)
4. Blader naar de [MongoDB - Downloads](http://www.mongodb.com/downloads) pagina (http://www.mongodb.com/downloads).
5. Selecteer, indien nodig de **Community Server** edition en selecteer vervolgens de meest recente huidige stabiele release voor*Windows Server 2008 R2 64-bits en hoger*. Als u wilt het installatieprogramma hebt gedownload, klikt u op **downloaden (msi)**.
   
    ![MongoDB-installatieprogramma downloaden](./media/install-mongodb/download-mongodb.png)
   
    Voer het installatieprogramma nadat het downloaden voltooid is.
6. Lees en accepteer de gebruiksrechtovereenkomst. Wanneer u wordt gevraagd, selecteert u **voltooid** installeren.
7. Indien gewenst, kunt u ook installeren kompas, een grafische interface voor MongoDB.
8. Klik op het laatste scherm **installeren**.

## <a name="configure-the-vm-and-mongodb"></a>Configureren van de virtuele machine en MongoDB
1. De padvariabelen zijn niet bijgewerkt door de MongoDB-installatieservice. Zonder de MongoDB `bin` locatie in uw padomgevingsvariabele, moet u elke keer dat u een MongoDB-uitvoerbaar bestand gebruikt voor het volledige pad opgeven. De locatie toevoegen aan de variabele path:
   
   * Met de rechtermuisknop op de **Start** menu en selecteer **System**.
   * Klik op **Geavanceerde systeeminstellingen**, en klik vervolgens op **omgevingsvariabelen**.
   * Onder **systeemvariabelen**, selecteer **pad**, en klik vervolgens op **bewerken**.
     
     ![Padvariabelen configureren](./media/install-mongodb/configure-path-variables.png)
     
     Het pad toevoegen aan uw MongoDB `bin` map. MongoDB wordt doorgaans geïnstalleerd in *C:\Program Files\MongoDB*. Controleer of het installatiepad op de virtuele machine. Het volgende voorbeeld wordt de standaard MongoDB-installatielocatie naar de `PATH` variabele:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Zorg ervoor dat u de toonaangevende puntkomma toevoegen (`;`) om aan te geven dat u wilt toevoegen aan een locatie voor uw `PATH` variabele.

2. MongoDB-gegevens en logboekbestanden mappen op de gegevensschijf van uw maken. Uit de **Start** in het menu **opdrachtprompt**. De volgende voorbeelden maken de mappen op station F:
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Een MongoDB-exemplaar starten met de volgende opdracht, aanpassen van het pad naar uw gegevens en mappen dienovereenkomstig aan te melden:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Het duurt enkele minuten voor MongoDB toegewezen van de logboekbestanden en start luistert voor verbindingen. Alle berichten in het logboek worden doorgestuurd naar de *F:\MongoLogs\mongolog.log* als het bestand `mongod.exe` server wordt gestart en logboekbestanden kan worden toegewezen.
   
   > [!NOTE]
   > De opdrachtprompt blijven onze aandacht richten op deze taak terwijl uw MongoDB-exemplaar dat wordt uitgevoerd. Sluit het opdrachtpromptvenster om door te gaan met MongoDB. Of installeer MongoDB als service, zoals beschreven in de volgende stap.

4. Voor een meer robuuste MongoDB-ervaring, installeert u de `mongod.exe` als een service. Het maken van een service betekent dat u hoeft niet te laat een opdrachtprompt uitgevoerd telkens wanneer u wilt gebruiken van MongoDB. De service maken als volgt het pad naar de mappen van uw gegevens en logboekbestanden dienovereenkomstig aanpassen:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    De voorgaande opdracht maakt u een service met de naam van MongoDB, met een beschrijving van "Mongo DB". De volgende parameters zijn ook opgegeven:
   
   * De `--dbpath` optie geeft u op de locatie van de map voor gegevens.
   * De `--logpath` optie moet worden gebruikt om op te geven van een logboekbestand, omdat de service heeft geen een opdrachtvenster om uitvoer weer te geven.
   * De `--logappend` optie geeft u de uitvoer toe te voegen aan het bestaande bestand zorgt ervoor dat de service opnieuw worden opgestart.
   
   De MongoDB-service wilt starten, moet u de volgende opdracht uitvoeren:
   
    ```
    net start MongoDB
    ```
   
    Zie voor meer informatie over het maken van de MongoDB-service [configureren van een Windows-Service voor MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testen van de MongoDB-exemplaar
MongoDB wordt uitgevoerd als één instantie of als een service is geïnstalleerd, kunt u het maken en gebruiken van uw databases nu starten. Voor het starten van de administratieve MongoDB-shell, een ander opdrachtpromptvenster openen vanuit de **Start** in het menu en voer de volgende opdracht:

```
mongo  
```

U kunt de databases met een lijst de `db` opdracht. Voer enkele gegevens als volgt in:

```
db.foo.insert( { a : 1 } )
```

Zoeken naar gegevens als volgt:

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

## <a name="configure-firewall-and-network-security-group-rules"></a>Firewall- en Network Security Group regels configureren
MongoDB is geïnstalleerd en wordt uitgevoerd, opent u een poort in Windows Firewall, zodat u op afstand verbinding met MongoDB maken kunt. Voor het maken van een nieuwe regel binnenkomende verbindingen om toe te staan van TCP-poort 27017, open een beheeropdrachtprompt PowerShell en voer de volgende opdracht:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

U kunt ook de regel maken met behulp van de **Windows Firewall met geavanceerde beveiliging** grafisch beheerprogramma. Maak een nieuwe regel binnenkomende verbindingen voor TCP-poort 27017 toestaan.

Indien nodig, maakt u een regel voor Netwerkbeveiligingsgroep voor toegang tot MongoDB van buiten het bestaande subnet met virtuele Azure-netwerk. U kunt de regels Network Security Group maken met behulp van de [Azure-portal](nsg-quickstart-portal.md) of [Azure PowerShell](nsg-quickstart-powershell.md). Net als bij de Windows Firewall-regels, kunt u TCP-poort 27017 aan de virtuele netwerkinterface van de MongoDB-VM.

> [!NOTE]
> TCP-poort 27017 is de standaardpoort gebruikt met MongoDB. U kunt deze poort wijzigen met behulp van de `--port` parameter bij het starten van `mongod.exe` handmatig of via een service. Als u de poort wijzigt, zorg ervoor dat u het bijwerken van de Windows Firewall- en Network Security Group regels in de voorgaande stappen.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u kunt installeren en configureren van MongoDB op uw Windows-VM. U kunt nu toegang tot MongoDB op de Windows-VM aan de hand van de geavanceerde onderwerpen in de [MongoDB-documentatie](https://docs.mongodb.com/manual/).


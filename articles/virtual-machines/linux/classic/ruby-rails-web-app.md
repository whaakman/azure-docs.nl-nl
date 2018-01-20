---
title: Een Ruby op Rails website op een Linux-VM hosten | Microsoft Docs
description: Instellen en een Ruby op Rails gebaseerde website op Azure met behulp van een virtuele Linux-machine host.
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: 1ee30aadc1bd07e7ac9a1894e4be832436ab5910
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby on Rails-webtoepassing op een Azure VM
Deze zelfstudie laat zien hoe voor het hosten van een Ruby op Rails website op Azure met behulp van een virtuele Linux-machine.  

Deze zelfstudie is gevalideerd met behulp van Ubuntu Server 14.04 TNS. Als u een ander Linux-distributiepunt gebruikt, moet u mogelijk de stappen voor het installeren van Rails wijzigen.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../../azure-resource-manager/resource-manager-deployment-model.md).  Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]
>

## <a name="create-an-azure-vm"></a>Een Azure virtuele machine maken
Begint met het maken van een virtuele machine in Azure met een Linux-installatiekopie.

U kunt de Azure portal of de Azure-opdrachtregelinterface (CLI) gebruiken voor het maken van de virtuele machine.

### <a name="azure-portal"></a>Azure Portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Klik op **nieuw**, typt u 'Ubuntu Server 14.04' in het zoekvak. Klik op het item dat is geretourneerd door de zoekopdracht. Selecteer het implementatiemodel **klassieke**, klik op 'Maken'.
3. Geef waarden voor de vereiste velden in de blade grondbeginselen: naam (voor de virtuele machine), gebruikersnaam, authenticatietype en de bijbehorende aanmeldingsreferenties Azure-abonnement, resourcegroep en locatie.

   ![Maak een nieuwe Ubuntu-afbeelding](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Nadat de virtuele machine is ingericht, klik op de VM-naam en klikt u op **eindpunten** in de **instellingen** categorie. De SSH-eindpunt, die worden vermeld onder vinden **zelfstandige**.

   ![Standaardeindpunt](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure-CLI
Volg de stappen in [maken van een virtuele Machine waarop Linux][vm-instructions].

Nadat de virtuele machine is ingericht, kunt u het SSH-eindpunt kunt krijgen met de volgende opdracht:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby op Rails installeren
1. SSH gebruiken voor verbinding met de virtuele machine.
2. Gebruik de volgende opdrachten Ruby installeren op de virtuele machine van de SSH-sessie:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    De installatie kan enkele minuten duren. Wanneer deze is voltooid, moet u de volgende opdracht gebruiken om te controleren of Ruby is geïnstalleerd:

        ruby -v

3. Gebruik de volgende opdracht voor het installeren van Rails:

        sudo gem install rails --no-rdoc --no-ri -V

    Gebruik de--vlaggen Nee rdoc en--Nee k om over te slaan voor het installeren van de documentatie, die sneller is.
    Met deze opdracht wordt waarschijnlijk lang duren om uit te voeren, zodat de -V toe te voegen, wordt informatie over de installatievoortgang weergegeven.

## <a name="create-and-run-an-app"></a>Maken en een app uitvoeren
Terwijl u nog steeds wordt aangemeld via SSH, voer de volgende opdrachten:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

De [nieuwe](http://guides.rubyonrails.org/command_line.html#rails-new) opdracht maakt u een nieuwe Rails-app. De [server](http://guides.rubyonrails.org/command_line.html#rails-server) opdracht start u de webserver WEBrick die wordt geleverd met Rails. (Voor gebruik in productieomgevingen, u wilt waarschijnlijk wilt gebruiken een andere server, zoals Eenhoorn of passagiers.)

De uitvoer ziet er als volgt uit.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Een eindpunt toevoegen
1. Ga naar de [Azure portal] [https://portal.azure.com] en selecteert u de virtuele machine.

2. Selecteer **EINDPUNTEN** in de **instellingen** langs de linkerkant van de rand de pagina.

3. Klik op **toevoegen** boven aan de pagina.

4. In de **eindpunt toevoegen** dialoogvenster pagina, voer de volgende informatie:

   * **Naam**: HTTP
   * **Protocol**: TCP
   * **Openbare poort**: 80
   * **Particuliere poort**: 3000
   * **Zwevende PI adres**: uitgeschakeld
   * **ACL - volgorde**: 1001 of een andere waarde die de prioriteit van deze regel wordt ingesteld.
   * **ACL - naam**: allowHTTP
   * **ACL - actie**: toestaan
   * **ACL - extern subnet**: 1.0.0.0/16

     Dit eindpunt heeft een openbare poort 80 dat verkeer wordt gerouteerd naar de particuliere poort van 3000, waarbij de Rails server luistert. De toegangsbeheerlijstregel kunt openbaar verkeer op poort 80.

     ![nieuwe endpoint](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Klik op OK om op te slaan van het eindpunt.

6. Een bericht moet worden weergegeven waarin wordt vermeld **eindpunt van de virtuele machine opslaan**. Als dit bericht verdwijnt, is het eindpunt is actief. U kunt nu uw toepassing testen door te navigeren naar de DNS-naam van uw virtuele machine. De website moet worden de volgende strekking weergegeven:

    ![standaardpagina rails][default-rails-cloud]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de meeste van de stappen handmatig. In een productieomgeving zou u uw app op een ontwikkelcomputer schrijven en deze implementeren in de Azure VM. De meeste productieomgevingen hosten ook de toepassing Rails in combinatie met een andere server-proces zoals Apache of NginX, welke ingangen aanvragen routering naar meerdere exemplaren van de toepassing Rails ten behoeve van statische resources. Zie http://rubyonrails.org/deploy/ voor meer informatie.

Voor meer informatie over Ruby op Rails, gaat u naar de [Ruby op Rails handleidingen][rails-guides].

Voor het gebruik van Azure-services van uw toepassing Ruby, Zie:

* [Niet-gestructureerde gegevens blobs opslaan][blobs]
* [Store sleutel-waardeparen met tabellen][tables]
* [Inhoud van de hoge bandbreedte met het netwerk van de levering van inhoud][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal-classic.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png

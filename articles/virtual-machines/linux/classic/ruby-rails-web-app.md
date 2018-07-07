---
title: Een Ruby on Rails-website op een Linux VM hosten | Microsoft Docs
description: Instellen en een Ruby on Rails-website op Azure met behulp van een virtuele Linux-machine host.
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: 6ea1d249b7f9aec3a45923b162a97ce7f83d0d31
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901150"
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby on Rails-webtoepassing op een Azure VM
Deze zelfstudie leert hoe u een Ruby on Rails-website op Azure host met behulp van een virtuele Linux-machine.  

In deze zelfstudie is gevalideerd met behulp van Ubuntu Server 14.04 TNS. Als u gebruikmaakt van een andere Linux-distributie, moet u mogelijk om te wijzigen van de stappen voor het installeren van Rails.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../../azure-resource-manager/resource-manager-deployment-model.md).  Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]
>

## <a name="create-an-azure-vm"></a>Een Azure-VM maken
Beginnen met het maken van een Azure-VM met een installatiekopie van Linux.

U kunt de Azure portal of de Azure-opdrachtregelinterface (CLI) gebruiken voor het maken van de virtuele machine.

### <a name="azure-portal"></a>Azure Portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
2. Klik op **een resource maken**, typ 'Ubuntu Server 14.04' in het zoekvak in. Klik op het item dat is geretourneerd door de zoekopdracht. Selecteer voor het implementatiemodel **klassieke**, klik vervolgens op 'Maken'.
3. Geef waarden voor de vereiste velden in de blade grondbeginselen: naam (voor de virtuele machine), gebruikersnaam, het verificatietype en de bijbehorende referenties, Azure-abonnement, resourcegroep en locatie.

   ![Een nieuwe Ubuntu-installatiekopie maken](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Nadat de virtuele machine is ingericht, klikt u op de naam van de virtuele machine en klikt u op **eindpunten** in de **instellingen** categorie. De SSH-eindpunt, die worden vermeld onder vinden **zelfstandige**.

   ![Eindpunt](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure-CLI
Volg de stappen in [maken van een virtuele Machine wordt uitgevoerd op Linux][vm-instructions].

Nadat de virtuele machine is ingericht, kunt u het SSH-eindpunt krijgen door de volgende opdracht uit:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installeer Ruby on Rails
1. Gebruik SSH om verbinding met de virtuele machine te maken.
2. Gebruik de volgende opdrachten voor het installeren van Ruby op de virtuele machine van de SSH-sessie:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    De installatie kan enkele minuten duren. Wanneer deze is voltooid, gebruikt u de volgende opdracht uit om te controleren of Ruby is geïnstalleerd:

        ruby -v

3. Gebruik de volgende opdracht voor het installeren van Rails:

        sudo gem install rails --no-rdoc --no-ri -V

    Gebruik de--niet-rdoc en--niet-gereserveerde instanties vlaggen om over te slaan de documentatie die sneller te installeren.
    Met deze opdracht wordt waarschijnlijk een lang duren om uit te voeren, zodat de -V toe te voegen, wordt informatie over de voortgang van de installatie weergegeven.

## <a name="create-and-run-an-app"></a>Maken en uitvoeren van een app
Terwijl u nog steeds bent aangemeld via SSH, voer de volgende opdrachten:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

De [nieuwe](http://guides.rubyonrails.org/command_line.html#rails-new) opdracht maakt u een nieuwe Rails-app. De [server](http://guides.rubyonrails.org/command_line.html#rails-server) opdracht start u de WEBrick-webserver die wordt geleverd met Rails. (Voor gebruik in productieomgevingen, u wilt waarschijnlijk wilt gebruiken een andere server, zoals Unicorn of passagiers.)

De uitvoer ziet er als volgt uit.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Een eindpunt toevoegen
1. Ga naar de [Azure portal] [https://portal.azure.com] en selecteer de virtuele machine.

2. Selecteer **EINDPUNTEN** in de **instellingen** aan de linkerkant van de rand de pagina.

3. Klik op **toevoegen** aan de bovenkant van de pagina.

4. In de **eindpunt toevoegen** dialoogvenster pagina, voer de volgende informatie:

   * **Naam**: HTTP
   * **Protocol**: TCP
   * **Openbare poort**: 80
   * **Particuliere poort**: 3000
   * **Zwevende PI adres**: uitgeschakeld
   * **Toegangsbeheerlijst - volgorde**: 1001 of een andere waarde die de prioriteit van deze toegangsregel ingesteld.
   * **Toegangsbeheerlijst - naam**: allowHTTP
   * **Toegangsbeheerlijst - actie**: toestaan
   * **Toegangsbeheerlijst - extern subnet**: 1.0.0.0/16

     Dit eindpunt heeft een openbare poort 80 die verkeer naar de particuliere poort van 3000 routeert, waar de Rails-server luistert. De toegangsbeheerlijstregel toestaat openbare verkeer op poort 80.

     ![nieuw eindpunt](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Klik op OK om op te slaan van het eindpunt.

6. Een bericht moet worden weergegeven met de mededeling **eindpunt van de virtuele machine opslaan**. Zodra dit bericht verdwijnt, is het eindpunt is actief. U kunt nu uw toepassing testen door te navigeren naar de DNS-naam van uw virtuele machine. De website moet worden de volgende strekking weergegeven:

    ![standaardpagina rails][default-rails-cloud]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de meeste van de stappen handmatig. In een productieomgeving zou u schrijft u uw app op een ontwikkelcomputer en deze implementeren in de Azure-VM. De meeste productieomgevingen host ook de Rails-toepassing in combinatie met een andere server-proces, zoals Apache of NginX, welke ingangen aanvragen doorsturen naar meerdere exemplaren van de Rails-toepassing ten behoeve van statische resources. Zie voor meer informatie, http://guides.rubyonrails.org/routing.html.

Voor meer informatie over de Ruby on Rails, gaat u naar de [Ruby on Rails-handleidingen][rails-guides].

Zie voor het gebruik van Azure-services van uw Ruby-toepassing:

* [Niet-gestructureerde gegevens blobs Store][blobs]
* [Store-sleutel/waarde-paren met tabellen][tables]
* [Leveren van inhoud met hoge bandbreedte met het Content Delivery Network][cdn-howto]

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

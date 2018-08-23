---
title: Implementatie van de virtuele machine van Azure met Chef | Microsoft Docs
description: Meer informatie over het gebruik van Chef voor de implementatie geautomatiseerde virtuele machine en configuratie op Microsoft Azure
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: jeconnoc
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: 3a6fbc8410dbc5aec4522f0972a29c67527edb23
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42058669"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Implementatie van virtuele Azure-machine automatiseren met Chef
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef is een uitstekend hulpprogramma voor het leveren van automation en desired state-configuraties.

Cloud-api met de meest recente release, Chef biedt naadloze integratie met Azure, zodat u de mogelijkheid om u te richten en te implementeren configuratiestatussen via één opdracht.

In dit artikel, moet u uw Chef-omgeving voor het inrichten van virtuele machines van Azure en helpt bij het maken van een beleid of "CookBook" en vervolgens implementeert deze handleiding op een virtuele machine van Azure instellen.

Laten we beginnen.

## <a name="chef-basics"></a>Chef-basisbeginselen
Voordat u begint, [bekijken van de basisconcepten van Chef](http://www.chef.io/chef). 

Het volgende diagram illustreert de Chef-architectuur op hoog niveau.

![][2]

Chef heeft drie belangrijkste architectuuronderdelen: Chef-Server, Chef-Client (knooppunt) en Chef-werkstation.

De Chef-Server is het beheerpunt en er zijn twee opties voor de Chef-Server: een gehoste oplossing of een on-premises-oplossing. We gebruiken een gehoste oplossing.

De Chef-Client (node) is de agent die bevindt zich op de servers die u beheert.

De Chef-werkstation is het beheerwerkstation waar we beleid maken en uitvoeren van opdrachten voor beheer. We voeren de **mes** opdracht uit vanaf het werkstation Chef om de infrastructuur te beheren.

Er is ook het concept van "Handleidingen" en 'Recepten'. Dit zijn effectief de beleidsregels die we definiëren en toepassen op de servers.

## <a name="preparing-the-workstation"></a>Het werkstation voorbereiden
Ten eerste kunt het werkstation voorbereiden. Ik gebruik een standaard Windows-werkstation. We moeten maken van een map voor het opslaan van de configuratiebestanden en handleidingen.

Maak eerst een map genaamd C:\chef.

Vervolgens maakt u een tweede directory c:\chef\cookbooks genoemd.

Nu moeten we de instellingen van het Azure-bestand downloaden, zodat Chef met het Azure-abonnement communiceren kan.

Download uw publicatie-instellingen met behulp van PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) opdracht. 

Sla het publish settings-bestand in C:\chef.

## <a name="creating-a-managed-chef-account"></a>Het maken van een beheerd account met Chef
Aanmelden voor een gehoste Chef-account [hier](https://manage.chef.io/signup).

Tijdens het aanmeldingsproces, wordt u gevraagd om te maken van een nieuwe organisatie.

![][3]

Als uw organisatie is gemaakt, downloadt u de starterskit.

![][4]

> [!NOTE]
> Als u een waarschuwing dat uw sleutels worden opnieuw ingesteld op prompt ontvangt, is het ok om door te gaan als er geen bestaande infrastructuur zo nog geconfigureerd.
> 
> 

Deze starter kit zip-bestand bevat uw organisatie configuratiebestanden en sleutels.

## <a name="configuring-the-chef-workstation"></a>Configureren van het werkstation met Chef
Pak de inhoud van de chef-starter.zip naar C:\chef.

Kopieer alle bestanden onder starter\chef de chef-opslagplaats\.chef aan uw directory c:\chef.

Uw directory ziet er ongeveer als in het volgende voorbeeld.

![][5]

U hebt nu vier bestanden, met inbegrip van het Azure publishing-bestand in de hoofdmap van c:\chef.

Het PEM-bestanden bevatten van uw organisatie en persoonlijke sleutels van de beheerder voor de communicatie, terwijl het bestand knife.rb de configuratie van de mes bevat. We moeten het bestand knife.rb bewerken.

Open het bestand in uw editor naar keuze en de 'cookbook_path' wijzigen door het verwijderen van de /... / van het pad, zodat deze wordt weergegeven zoals volgende.

    cookbook_path  ["#{current_dir}/cookbooks"]

Voeg ook de volgende regel zetten op basis van de naam van uw Azure publish settings-bestand.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Het bestand knife.rb moet er nu uitzien zoals in het volgende voorbeeld.

![][6]

Deze regels zorgt ervoor dat de mes verwijst naar de map handleidingen onder c:\chef\cookbooks en ook onze Azure Publish Settings-bestand tijdens de Azure-bewerkingen gebruikt.

## <a name="installing-the-chef-development-kit"></a>De Chef Development Kit installeren
Volgende [download en installeer](http://downloads.getchef.com/chef-dk/windows) de ChefDK (Chef Development Kit) voor het instellen van uw Chef-werkstation.

![][7]

In de standaardlocatie van c:\opscode installeren. Deze installatie duurt ongeveer 10 minuten.

Controleer of dat uw padomgevingsvariabele bevat vermeldingen voor C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Als ze niet er zijn, zorg er dan voor dat u deze paden hebt toegevoegd.

*HOUD ER REKENING MEE DAT DE VOLGORDE VAN HET PAD IS BELANGRIJK.* Als uw opscode paden niet in de juiste volgorde hebt u problemen.

Start opnieuw op uw werkstation voordat u doorgaat.

Er wordt vervolgens installeert u de mes-Azure-extensie. Dit biedt mes met de invoegtoepassing' Azure'.

Voer de volgende opdracht uit.

    chef gem install knife-azure ––pre

> [!NOTE]
> Het argument – vooraf zorgt ervoor dat u de meest recente RC-versie van de Azure-invoegtoepassing voor mes waarmee u toegang hebt tot de meest recente set API's ontvangt.
> 
> 

Is het waarschijnlijk dat een aantal afhankelijkheden ook worden geïnstalleerd op hetzelfde moment.

![][8]

Om te controleren of dat alles juist is geconfigureerd, moet u de volgende opdracht uitvoeren.

    knife azure image list

Als alles correct is geconfigureerd, ziet u een lijst met beschikbare installatiekopieën van Azure door te bladeren.

Gefeliciteerd! Het werkstation is ingesteld.

## <a name="creating-a-cookbook"></a>Het maken van een Cookbook
Een Cookbook wordt gebruikt door Chef voor het definiëren van een reeks opdrachten die u wilt uitvoeren op uw beheerde client. Het maken van een Cookbook is eenvoudig en gebruiken we de **chef genereren cookbook** opdracht voor het genereren van de sjabloon Cookbook. Ik zal worden aanroepen van mijn Cookbook webserver als ik wil graag een beleid dat door IIS automatisch implementeert.

Voer de volgende opdracht in uw map C:\Chef.

    chef generate cookbook webserver

Hiermee wordt een reeks van bestanden in de map C:\Chef\cookbooks\webserver gegenereerd. Nu moeten we de reeks opdrachten die wij willen graag de Chef-client uit te voeren op de beheerde virtuele machine definiëren.

De opdrachten worden opgeslagen in het bestand default.rb. In dit bestand moet ik een reeks opdrachten waarmee IIS wordt geïnstalleerd, start IIS en een sjabloon voor bestanden worden gekopieerd naar de wwwroot-map definiëren.

Wijzigen van het bestand C:\chef\cookbooks\webserver\recipes\default.rb en voeg de volgende regels toe.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Sla het bestand wanneer u klaar bent.

## <a name="creating-a-template"></a>Het maken van een sjabloon
Zoals we eerder vermeld, moet voor het genereren van een sjabloon voor bestanden die worden gebruikt als de pagina default.html.

Voer de volgende opdracht voor het genereren van de sjabloon.

    chef generate template webserver Default.htm

Nu gaat u naar het bestand C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bewerk het bestand door enkele eenvoudige 'Hallo wereld"HTML-code toe te voegen en sla het bestand.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Het Cookbook uploaden naar de Chef-Server
We zijn in deze stap maakt een kopie van het Cookbook die we hebben gemaakt op de lokale computer en uploaden naar de Chef-gehoste-Server. Na het uploaden, het Cookbook worden weergegeven onder de **beleid** tabblad.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Een virtuele machine met Mes Azure implementeren
We nu een Azure-machine implementeren en toepassen van de 'Webserver' Cookbook die de IIS web service en de standaard-webpagina wordt geïnstalleerd.

Gebruik hiervoor de **mes azure-server maken** opdracht.

Ben voorbeeld van de opdracht volgende verschijnt.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

De parameters behoeven geen uitleg. Vervangen door uw specifieke variabelen en uitgevoerd.

> [!NOTE]
> Via de opdrachtregel, ben ik mijn filterregels voor eindpunt netwerk ook automatiseren met behulp van de parameter – tcp-eindpunten. Ik hebt poorten 80 en 3389 voor toegang tot mijn webpagina's en RDP-sessie geopend.
> 
> 

Nadat u de opdracht uitvoert, gaat u naar de Azure-portal en ziet u de computer die begint met het inrichten.

![][13]

Er verschijnt de opdrachtprompt volgende.

![][10]

Zodra de implementatie voltooid is, moeten we geen verbinding maken met de webservice via poort 80 als we waren de poort geopend wanneer we de virtuele machine met de opdracht mes Azure ingericht zijn. Als deze virtuele machine de virtuele machine die alleen in mijn cloudservice is, moet ik het verbinding maken met de cloud service-url.

![][11]

Zoals u ziet, krijg ik creative met mijn HTML-code.

Vergeet niet dat we kunnen ook verbinding maken via een RDP-sessie vanuit Azure portal via poort 3389.

Ik hoop dat dit is handig! Ga en vandaag nog uw infrastructuur als code ontwikkelproces met Azure.

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->

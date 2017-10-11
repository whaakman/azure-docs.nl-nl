---
title: Implementatie van de virtuele machine van Azure met Chef | Microsoft Docs
description: Meer informatie over het gebruik van Chef voor implementatie van geautomatiseerde virtuele machine en de configuratie op Microsoft Azure
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: b6db0fbb4e0de896994954974ddcc39daad9c125
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Implementatie van virtuele Azure-machine automatiseren met Chef
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef is een uitstekend hulpprogramma voor het leveren van automation en gewenste status configuraties.

Met de meest recente versie van de cloud-api biedt Chef naadloze integratie met Azure, zodat u de mogelijkheid voor het inrichten en implementeren van configuratiestatussen via één opdracht.

In dit artikel ziet ik u het instellen van uw omgeving Chef inrichten van virtuele machines in Azure en helpt u bij het maken van een beleid of "CookBook" en vervolgens deze cookbook implementeert op een virtuele machine van Azure.

We begint!

## <a name="chef-basics"></a>Chef basisbeginselen
Voordat u begint, voorgesteld ik dat u de basisconcepten van Chef bekijken. Er is geweldige materiaal <a href="http://www.chef.io/chef" target="_blank">hier</a> en ik het beste hebt u een snelle Lees voordat u deze stapsgewijze kennismaking. Ik zal echter de basisbeginselen samenvatting voordat we aan de slag.

Het volgende diagram illustreert de op hoog niveau Chef-architectuur.

![][2]

Chef heeft drie architectuur hoofdonderdelen: Chef-Server, Chef-Client (knooppunt) en Chef-werkstation.

De Chef Server onze beheerpunt en er zijn twee opties voor de Chef Server: een gehoste oplossing of een on-premises-oplossing. We gebruiken een gehoste oplossing.

De Chef Client (knooppunt) is de agent die zich op de servers die u beheert.

Het werkstation Chef is onze beheerwerkstation waar we onze beleid maken en onze management-opdrachten uit te voeren. We voeren de **mes** opdracht van het werkstation Chef om onze infrastructuur te beheren.

Er is ook het concept van 'Cookbooks' en 'Recepten'. Dit zijn effectief de beleidsregels die we definiëren en toepassen op onze servers.

## <a name="preparing-the-workstation"></a>Het werkstation voorbereiden
Ten eerste kunt het werkstation voorbereiden. Ik gebruik een standaard Windows-werkstation. Er moet een map voor het opslaan van onze configuratiebestanden en cookbooks maken.

Eerst een map met de naam C:\chef maken.

Vervolgens maakt u een tweede directory c:\chef\cookbooks aangeroepen.

Nu moeten we ons Azure-instellingen-bestand downloaden zodat Chef met ons Azure-abonnement communiceren kan.

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
Download uw publicatie-instellingen met behulp van de PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) opdracht. 

Sla het bestand van de instellingen voor publiceren in C:\chef.

## <a name="creating-a-managed-chef-account"></a>Een beheerde Chef-account maken
Aanmelden voor een gehoste Chef account [hier](https://manage.chef.io/signup).

Tijdens het aanmeldingsproces, wordt u gevraagd een nieuwe organisatie maken.

![][3]

Nadat uw organisatie is gemaakt, downloadt u de starterskit.

![][4]

> [!NOTE]
> Als u gevraagd waarschuwing wordt dat uw sleutels worden opnieuw ingesteld, is het ok om door te gaan als er geen bestaande infrastructuur nog geconfigureerd.
> 
> 

Deze starter kit zip-bestand bevat de configuratiebestanden van de organisatie en de sleutels.

## <a name="configuring-the-chef-workstation"></a>Het werkstation Chef configureren
Pak de inhoud van de chef-starter.zip naar C:\chef.

Kopieer alle bestanden onder chef-starter\chef-opslagplaats\.chef aan uw directory c:\chef.

Uw directory ziet er nu ongeveer het volgende voorbeeld.

![][5]

U hebt nu vier bestanden met inbegrip van het Azure publishing bestand in de hoofdmap van c:\chef.

Het PEM-bestanden bevatten van uw organisatie en persoonlijke sleutels van de beheerder voor communicatie terwijl het bestand knife.rb de configuratie van uw mes bevat. Bewerk het bestand knife.rb moet.

Open het bestand in uw editor naar keuze en de 'cookbook_path' wijzigen door het verwijderen van de /... / van het pad, zodat deze wordt weergegeven zoals volgende.

    cookbook_path  ["#{current_dir}/cookbooks"]

Voeg ook de volgende regel als gevolg van de naam van uw Azure bestand publicatie-instellingen.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Uw bestand knife.rb nu zijn vergelijkbaar met het volgende voorbeeld.

![][6]

Deze regels zorgt ervoor dat mes verwijst naar de map cookbooks onder c:\chef\cookbooks en ook onze Azure Publish Settings-bestand tijdens de Azure-bewerkingen gebruikt.

## <a name="installing-the-chef-development-kit"></a>De Chef Development Kit installeren
Volgende [downloaden en installeren](http://downloads.getchef.com/chef-dk/windows) de ChefDK (Chef Development Kit) voor het instellen van uw Chef-werkstation.

![][7]

In de standaardlocatie van c:\opscode installeren. Deze installatie duurt ongeveer 10 minuten.

Bevestig dat uw padvariabele bevat vermeldingen voor C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Als ze niet er zijn, zorg er dan voor dat u deze paden toevoegt.

*HOUD ER REKENING MEE DAT DE VOLGORDE VAN HET PAD IS BELANGRIJK!* Als uw opscode-paden niet in de juiste volgorde zijn hebt u problemen.

Start opnieuw op uw werkstation voordat u doorgaat.

Vervolgens wordt de extensie mes Azure installeert. Dit biedt mes met de invoegtoepassing' Azure'.

Voer de volgende opdracht.

    chef gem install knife-azure ––pre

> [!NOTE]
> Het argument – pre zorgt ervoor dat u de meest recente RC-versie van de Azure-invoegtoepassing voor mes dat toegang tot de meest recente set API's biedt ontvangt.
> 
> 

Is het waarschijnlijk dat een aantal afhankelijkheden ook worden geïnstalleerd op hetzelfde moment.

![][8]

Voer de volgende opdracht om te controleren of dat alles correct is geconfigureerd.

    knife azure image list

Als alles correct is geconfigureerd, ziet u een lijst met beschikbare Azure installatiekopieën door te bladeren.

Gefeliciteerd. Het werkstation is ingesteld.

## <a name="creating-a-cookbook"></a>Maken van een Cookbook
Een Cookbook wordt gebruikt door Chef voor het definiëren van een reeks opdrachten die u wilt uitvoeren op uw beheerde client. Het maken van een Cookbook is eenvoudig en gebruiken we de **chef genereren cookbook** opdracht voor het genereren van onze Cookbook-sjabloon. Ik zal worden aanroepen van mijn webserver Cookbook als ik een beleid dat automatisch wordt geïmplementeerd IIS zou willen.

Voer de volgende opdracht in uw map C:\Chef.

    chef generate cookbook webserver

Hierdoor wordt een aantal bestanden in de map C:\Chef\cookbooks\webserver gegenereerd. Nu moeten we de reeks opdrachten die we onze Chef-client wilt uitvoeren op onze beheerde virtuele machine wilt definiëren.

De opdrachten worden opgeslagen in het bestand default.rb. In dit bestand moet ik een verzameling opdrachten die IIS is geïnstalleerd, start IIS en een sjabloonbestand kopieert naar de wwwroot-map definiëren.

Wijzigen van het bestand C:\chef\cookbooks\webserver\recipes\default.rb en voeg de volgende regels.

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

Sla het bestand als u klaar bent.

## <a name="creating-a-template"></a>Maken van een sjabloon
Zoals we eerder vermeld, moeten we een sjabloonbestand dat wordt gebruikt als onze pagina met default.html genereren.

Voer de volgende opdracht voor het genereren van de sjabloon.

    chef generate template webserver Default.htm

Nu gaat u naar het bestand C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bewerk het bestand door enkele eenvoudige 'Hallo wereld' HTML-code toe te voegen en sla het bestand.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Het Cookbook uploaden naar de Chef-Server
We zijn een kopie van het Cookbook die er op de lokale computer gemaakt en uploaden naar de Server van de gehoste Chef in deze stap. Na het uploaden, het Cookbook wordt weergegeven onder de **beleid** tabblad.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Een virtuele machine met Mes Azure implementeren
We nu implementeren van een virtuele machine van Azure en toepassen van de 'Webserver' Cookbook die onze IIS web service en de standaard webpagina wordt geïnstalleerd.

Gebruik hiervoor de **mes azure-server maken** opdracht.

Ben voorbeeld van de opdracht volgende weergegeven.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

De parameters behoeven geen uitleg. Vervangen door uw specifieke variabelen en uitgevoerd.

> [!NOTE]
> Via de de opdrachtregel, ik ben ook automatiseren mijn filterregels endpoint-netwerk met behulp van de parameter – tcp-eindpunten. Ik hebt up poorten 80 en 3389 voor toegang tot mijn webpagina's en RDP-sessie geopend.
> 
> 

Zodra u de opdracht uitvoert, gaat u naar de Azure-portal en ziet u de computer die begint met het inrichten.

![][13]

Er verschijnt de opdrachtprompt volgende.

![][10]

Zodra de implementatie voltooid is, moet er verbinding maken met de web-service via poort 80 als we had de poort geopend wanneer we de virtuele machine met de opdracht mes Azure ingericht. Als deze virtuele machine de virtuele machine die alleen in mijn cloudservice is, moet ik het verbinding maken met de cloud service-url.

![][11]

Zoals u ziet, krijg ik creative met mijn HTML-code.

Vergeet niet dat we kunnen ook verbinding maken via een RDP-sessie vanaf de Azure portal via poort 3389.

Ik hopen dat u dat dit is handig zijn. Ga en uw infrastructuur vandaag starten als code reis met Azure.

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

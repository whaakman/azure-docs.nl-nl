---
title: Implementatie van de virtuele machine van Azure met Chef | Microsoft Docs
description: Meer informatie over het gebruik van Chef voor de implementatie geautomatiseerde virtuele machine en configuratie op Microsoft Azure
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 74b92c277b1d6eaa0984e55a70459bad59c2bf84
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719273"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Implementatie van virtuele Azure-machine automatiseren met Chef

Chef is een uitstekend hulpprogramma voor het leveren van automation en desired state-configuraties.

Met de meest recente versie van de cloud-API biedt Chef naadloze integratie met Azure, zodat u de mogelijkheid om u te richten en te implementeren configuratiestatussen via één opdracht.

In dit artikel, moet u uw Chef-omgeving voor het inrichten van virtuele machines van Azure en helpt bij het maken van een beleid of "Cookbook" en vervolgens implementeert deze handleiding op een virtuele machine van Azure instellen.

## <a name="chef-basics"></a>Chef-basisbeginselen
Voordat u begint, [bekijken van de basisconcepten van Chef](https://www.chef.io/chef).

Het volgende diagram illustreert de Chef-architectuur op hoog niveau.

![][2]

Chef heeft drie belangrijkste architectuuronderdelen: Chef-Server, Chef-Client (knooppunt) en Chef-werkstation.

De Chef-Server is het beheerpunt en er zijn twee opties voor de Chef-Server: een gehoste oplossing of een on-premises-oplossing.

De Chef-Client (node) is de agent die bevindt zich op de servers die u beheert.

De Chef-werkstation, wat de naam voor zowel de beheerder is werkstation waar u beleid maken en uitvoeren van opdrachten voor het beheer en de software-updatepakket van Chef-hulpprogramma's.

U ziet in het algemeen _uw werkstation_ als de locatie waar u acties uitvoeren en _Chef werkstation_ voor het softwarepakket.
Bijvoorbeeld, downloadt u de opdracht mes als onderdeel van _Chef werkstation_, maar u mes-opdrachten uit uitvoeren _uw werkstation_ om infrastructuur te beheren.

Chef maakt ook gebruik van de concepten van "Handleidingen" en "recepten', is nagenoeg de beleidsregels die we definiëren en toepassen op de servers.

## <a name="preparing-your-workstation"></a>Uw werkstation voorbereiden

Eerst uw werkstation voorbereiden door het maken van een map voor het opslaan van Chef-configuratiebestanden en handleidingen.

Maak een map genaamd C:\Chef.

Download en installeer de meest recente [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versie u aan bij uw werkstation.

## <a name="configure-azure-service-principal"></a>Azure Service Principal configureren

In eenvoudigste met de bepalingen en Azure Service-Principal is een service-account.   We gebruiken een Service-Principal om ons Azure-resources te maken van onze Chef-werkstation te helpen.  Als u wilt de relevante Service-Principal maken met de vereiste machtigingen moeten we de volgende opdrachten vanuit PowerShell:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Neem notitie van uw abonnements-id, de tenant-id, de ClientID en het Clientgeheim (het wachtwoord dat u hierboven hebt ingesteld), moet u dit later op. 

## <a name="setup-chef-server"></a>Chef-Server instellen

Deze handleiding wordt ervan uitgegaan dat u zal zich registreren voor Chef die wordt gehost.

Als u niet al een Chef-Server, kunt u het volgende doen:

* Zich aanmelden voor [gehost Chef](https://manage.chef.io/signup), dit is de snelste manier om aan de slag met Chef.
* Een zelfstandige Chef-Server installeren op linux gebaseerde virtuele machine, na de [installatie-instructies](https://docs.chef.io/install_server.html) van [Chef Docs](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Het maken van een account die wordt gehost Chef

Aanmelden voor een account die wordt gehost Chef [hier](https://manage.chef.io/signup).

Tijdens het registratieproces wordt u gevraagd om te maken van een nieuwe organisatie.

![][3]

Als uw organisatie is gemaakt, downloadt u de starterskit.

![][4]

> [!NOTE]
> Als u een waarschuwing dat uw sleutels worden opnieuw ingesteld op prompt krijgt, is het goed om door te gaan als er geen bestaande infrastructuur zo nog geconfigureerd.
>

Deze starter kit zip-bestand bevat uw organisatie-configuratiebestanden en sleutel van de gebruiker in de `.chef` directory.

De `organization-validator.pem` moeten afzonderlijk worden gedownload omdat het een persoonlijke sleutel en persoonlijke sleutels moeten niet worden opgeslagen op de Chef-Server. Van [Chef beheren](https://manage.chef.io/), gaat u naar de sectie beheer en selecteer 'Opnieuw instellen van validatiesleutel', waarmee u een bestand afzonderlijk worden gedownload. Sla het bestand op c:\chef.

### <a name="configuring-your-chef-workstation"></a>Configureren van uw werkstation Chef

Pak de inhoud van de chef-starter.zip naar c:\chef.

Kopieer alle bestanden onder starter\chef de chef-opslagplaats\.chef aan uw directory c:\chef.

Kopieer de `organization-validator.pem` van het bestand in c:\chef, als deze wordt opgeslagen in c:\Downloads

Uw directory ziet er ongeveer als in het volgende voorbeeld.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

U hebt nu vijf bestanden en vier mappen (met inbegrip van de map leeg chef-opslagplaats) in de hoofdmap van c:\chef.

### <a name="edit-kniferb"></a>Knife.rb bewerken

Het PEM-bestanden bevatten van uw organisatie en administratieve persoonlijke sleutels voor de communicatie en het bestand knife.rb bevat de configuratie van de Mes. We moeten het bestand knife.rb bewerken.

Open het bestand knife.rb in de editor van uw keuze. De ongewijzigde bestand ziet er ongeveer als:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Voeg de volgende informatie toe aan uw knife.rb:

validation_client_name   "myorg-validator"

validation_key "#{current_dir}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


Deze regels zorgt ervoor dat de mes verwijst naar de map handleidingen onder c:\chef\cookbooks en maakt ook gebruik van de Azure-Service-Principal die u hebt gemaakt tijdens de Azure-bewerkingen.

Het bestand knife.rb moet er nu uitzien zoals in het volgende voorbeeld:

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Chef-werkstation installeren

Volgende [download en installeer](https://downloads.chef.io/chef-workstation/) Chef werkstation.
Installeer Chef werkstation de standaardlocatie. Deze installatie kan enkele minuten duren.

Op het bureaublad ziet u een 'gewicht PowerShell', dit een omgeving die is geladen met het hulpprogramma die u nodig hebt is voor interactie met de Chef-producten. Het gewicht PowerShell zijn nieuwe ad-hoc opdrachten beschikbaar, zoals `chef-run` ook als traditionele Chef CLI-opdrachten, zoals `chef`. Zie de geïnstalleerde versie van Chef werkstation en de Chef-hulpprogramma's met `chef -v`. U kunt ook uw werkstation versie controleren door 'Over Chef werkstation' in de Chef-werkstation-App te selecteren.

`chef --version` moeten er ongeveer als:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> De volgorde van het pad is belangrijk. Als uw opscode paden niet in de juiste volgorde hebt u problemen.
>

Start opnieuw op uw werkstation voordat u doorgaat.

### <a name="install-knife-azure"></a>Mes Azure installeren

In deze zelfstudie wordt ervan uitgegaan dat u de Azure Resource Manager gebruikt om te communiceren met uw virtuele machine.

De mes-Azure-extensie installeren. Dit biedt mes met de invoegtoepassing' Azure'.

Voer de volgende opdracht uit.

    chef gem install knife-azure ––pre

> [!NOTE]
> Het argument – vooraf zorgt ervoor dat u de meest recente RC-versie van de Azure-invoegtoepassing voor mes waarmee u toegang hebt tot de meest recente set API's ontvangt.
>
>

Is het waarschijnlijk dat een aantal afhankelijkheden ook worden geïnstalleerd op hetzelfde moment.

![][8]

Om te controleren of dat alles juist is geconfigureerd, moet u de volgende opdracht uitvoeren.

    knife azurerm server list

Als alles correct is geconfigureerd, ziet u een lijst met beschikbare installatiekopieën van Azure door te bladeren.

Gefeliciteerd! Uw werkstation is ingesteld.

## <a name="creating-a-cookbook"></a>Het maken van een Cookbook

Een Cookbook wordt gebruikt door Chef voor het definiëren van een reeks opdrachten die u wilt uitvoeren op uw beheerde client. Het maken van een Cookbook is vrij eenvoudig, gebruikt u de **chef genereren cookbook** opdracht voor het genereren van de sjabloon Cookbook. Deze handleiding is bedoeld voor een webserver die automatisch wordt geïmplementeerd IIS.

Voer de volgende opdracht in uw map C:\Chef.

    chef generate cookbook webserver

Met deze opdracht genereert een reeks van bestanden in de map C:\Chef\cookbooks\webserver. Vervolgens definieert u de reeks opdrachten voor de Chef-client op de beheerde virtuele machine worden uitgevoerd.

De opdrachten worden opgeslagen in het bestand default.rb. In dit bestand definieert u een reeks opdrachten waarmee IIS wordt geïnstalleerd, start IIS en een sjabloon voor bestanden worden gekopieerd naar de wwwroot-map.

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
In deze stap maakt u een sjabloonbestand gebruikt als de pagina default.html genereren.

Voer de volgende opdracht voor het genereren van de sjabloon:

    chef generate template webserver Default.htm

Navigeer naar de `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb` bestand. Bewerk het bestand door enkele eenvoudige 'Hallo wereld"HTML-code toe te voegen en sla het bestand.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Het Cookbook uploaden naar de Chef-Server
In deze stap maakt u een kopie van het Cookbook die u hebt gemaakt op de lokale computer en te uploaden naar de Chef-gehoste-Server. Na het uploaden, het Cookbook wordt weergegeven onder de **beleid** tabblad.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Een virtuele machine met Mes Azure implementeren
Implementeren van een virtuele machine van Azure en toepassen van de 'Webserver' Cookbook die de IIS web service en de standaard-webpagina wordt geïnstalleerd.

Gebruik hiervoor de **mes azurerm-server maken** opdracht.

Een voorbeeld van de opdracht verschijnt het volgende.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


Het bovenstaande voorbeeld wordt een Standard_DS2_v2 virtuele machine maken met Windows Server 2016 is geïnstalleerd in de regio VS-West. Vervangen door uw specifieke variabelen en uitgevoerd.

> [!NOTE]
> Via de opdrachtregel, ben ik mijn filterregels voor eindpunt netwerk ook automatiseren met behulp van de parameter – tcp-eindpunten. Ik hebt poorten 80 en 3389 voor toegang tot de webpagina en de RDP-sessie geopend.
>
>

Nadat u de opdracht uitvoert, gaat u naar de Azure-portal om te zien van uw computer begint met het inrichten.

![][15]

Er verschijnt de opdrachtprompt volgende.

![][16]

Zodra de implementatie is voltooid, wordt het openbare IP-adres van de nieuwe virtuele machine wordt weergegeven op de voltooiing van de implementatie, u kunt deze kopiëren en plak deze in een webbrowser en weergeven van de website die u hebt geïmplementeerd. Wanneer we de virtuele machine geïmplementeerd we poort 80 geopend, zodat deze extern beschikbaar moet zijn.   

![][11]

In dit voorbeeld maakt gebruik van creative HTML-code.

U kunt ook de status van het knooppunt weergeven [Chef beheren](https://manage.chef.io/). 

![][17]

Vergeet niet dat u kunt ook verbinding maken via een RDP-sessie vanuit Azure portal via poort 3389.

Bedankt. Ga en vandaag nog uw infrastructuur als code ontwikkelproces met Azure.

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
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->

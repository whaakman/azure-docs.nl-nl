---
title: Een ILB maken en gebruiken met een Azure App Service-omgeving
description: Meer informatie over het maken en gebruiken van een Azure App Service-omgeving met internetisolatie
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 9f7343102cf7af6d7f2ba6b4b2f08b7b855da6f8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Een ILB maken en gebruiken met een App Service-omgeving #

 Azure App Service Environment is een implementatie van Azure App Service in een subnet in een virtueel Azure-netwerk (VNet). Er zijn twee manieren om een AS-omgeving te implementeren: 

- Met een VIP-adres op een extern IP-adres, vaak aangeduid als Externe AS-omgeving.
- Met een VIP-adres op een intern IP-adres, vaak aangeduid als een ILB AS-omgeving omdat het interne eindpunt een ILB (Internal Load Balancer) is. 

In dit artikel wordt uitgelegd hoe u een ILB AS-omgeving maakt. Zie [Introduction to App Service environments][Intro] (Inleiding tot App Service-omgevingen) voor een overzicht van de AS-omgeving. Zie [Create an External ASE][MakeExternalASE] (Een Externe AS-omgeving maken) voor informatie over het maken van een Externe AS-omgeving.

## <a name="overview"></a>Overzicht ##

U kunt een AS-omgeving implementeren met een eindpunt dat toegankelijk is via internet of met een IP-adres in uw VNet. De AS-omgeving moet zijn geïmplementeerd met een ILB om het IP-adres in te stellen op een VNet-adres. Als u de AS-omgeving implementeert met een ILB, moet u het volgende opgeven:

-   Uw eigen domein dat u gebruikt bij het maken van apps.
-   Het certificaat dat wordt gebruikt voor HTTPS.
-   DNS-beheer voor uw domein.

Hierna kunt u dingen doen zoals:

-   Intranettoepassingen veilig hosten in de cloud, waartoe u toegang hebt via een site-naar-site- of Azure ExpressRoute-VPN.
-   Apps die niet worden vermeld op openbare DNS-servers, hosten in de cloud.
-   Back-end-apps met internetisolatie maken, waarmee front-end-apps veilig kunnen worden geïntegreerd.

### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit ###

Er is een aantal dingen dat u niet kunt doen wanneer u een ILB AS-omgeving gebruikt:

-   Op IP-gebaseerd SSL gebruiken.
-   IP-adressen toewijzen aan specifieke apps.
-   Een certificaat kopen en gebruiken met een app via Azure Portal. U kunt certificaten rechtstreeks bij een certificeringsinstantie verkrijgen en ze gebruiken met uw apps. U kunt ze niet verkrijgen via Azure Portal.

## <a name="create-an-ilb-ase"></a>Een ILB AS-omgeving maken ##

Ga als volgt te werk om een ILB AS-omgeving te maken:

1. Selecteer in Azure Portal achtereenvolgens **Nieuw** > **Web en mobiel** > **App Service Environment**.

2. Selecteer uw abonnement.

3. Selecteer of maak een resourcegroep.

4. Selecteer of maak een VNet.

5. Als u een bestaand VNet selecteert, moet u een subnet maken om de AS-omgeving in te plaatsen. Zorg ervoor dat u een subnetgrootte instelt die groot genoeg is voor een eventuele toekomstige groei van uw AS-omgeving. We raden een grootte aan van `/25`. Dit formaat bevat 128 adressen en kan de grootst mogelijke AS-omgeving verwerken. De minimale grootte die u kunt selecteren is een `/28`. Deze grootte kan, afhankelijk van de behoeften van uw infrastructuur, worden geschaald naar maximaal 11 exemplaren.

    * Ga verder dan het maximumaantal van 100 exemplaren in uw App Service-plannen.

    * Schaal naar bijna 100 met snellere front-endschaling.

6. Selecteer **Virtueel netwerk/locatie** > **Virtuele-netwerkconfiguratie**. Stel het **VIP-type** in op **Intern**.

7. Voer een domeinnaam in. Dit is het domein dat wordt gebruikt voor apps die zijn gemaakt in deze AS-omgeving. Er zijn enkele beperkingen. Het domein kan niet zijn:

    * net   

    * azurewebsites.net

    * p.azurewebsites.net

    * &lt;naam van AS-omgeving&gt;.p.azurewebsites.net

   De aangepaste domeinnaam die wordt gebruikt voor apps en de domeinnaam die wordt gebruikt voor uw AS-omgeving mogen niet overlappen. Voor een ILB AS-omgeving met de domeinnaam _contoso.com_ kunt u geen aangepaste domeinnamen voor de apps gebruiken, zoals:

    * www.contoso.com

    * abcd.def.contoso.com

    * abcd.contoso.com

   Als u de aangepaste domeinnamen voor de apps kent, kiest u een domein voor de ILB AS-omgeving die niet conflicteert met deze aangepaste domeinnamen. In dit voorbeeld kunt u voor het domein van uw AS-omgeving een naam gebruiken zoals *contoso-internal.com*, omdat deze naam niet conflicteert met aangepaste domeinnamen die eindigen op *.contoso.com*.

8. Selecteer **OK**, en selecteer vervolgens **Maken**.

    ![ASE maken][1]

Op de blade **Virtueel netwerk** ziet u de optie **Virtuele-netwerkconfiguratie**. U kunt deze optie gebruiken om een extern VIP-adres of een intern VIP-adres te selecteren. De standaard is **Extern**. Als u **Extern** selecteert, maakt de AS-omgeving gebruik van een IP-adres dat toegankelijk is via internet. Als u **Intern** selecteert, is de AS-omgeving geconfigureerd met een ILB op een IP-adres binnen uw VNet.

Nadat u **Intern** hebt geselecteerd, wordt de mogelijkheid om meer IP-adressen toe te voegen aan de AS-omgeving verwijderd. In plaats hiervan moet u het domein van de AS-omgeving opgeven. In een AS-omgeving met een extern VIP-adres wordt de naam van de AS-omgeving gebruikt in het domein voor apps die zijn gemaakt in deze AS-omgeving.

Als u het **VIP-type** instelt op **Intern**, wordt de naam van de AS-omgeving niet gebruikt in het domein van de AS-omgeving. U geeft het domein expliciet op. Als uw domein *contoso.corp.net* is en u een app in deze AS-omgeving maakt met de naam *timereporting*, is de URL voor deze app timereporting.contoso.corp.net.


## <a name="create-an-app-in-an-ilb-ase"></a>Een app maken in een ILB AS-omgeving ##

Het maken van een app in een ILB AS-omgeving werkt hetzelfde als het maken van een app in een AS-omgeving.

1. Selecteer in Azure Portal achtereenvolgens **Nieuw** > **Web en mobiel** > **Web** of **Mobile** of **API-app**.

2. Voer de naam van de app in.

3. Selecteer het abonnement.

4. Selecteer of maak een resourcegroep.

5. Selecteer of maak een App Service-plan. Als u een nieuw App Service-plan wilt maken, selecteert u uw AS-omgeving als locatie. Selecteer de groep met werkrollen waarin u het App Service-plan wilt maken. Wanneer u het App Service-plan maakt, selecteert u uw AS-omgeving als de locatie en de groep met werkrollen. Wanneer u de naam van de app opgeeft, wordt het domein onder de app-naam vervangen door het domein van de AS-omgeving.

6. Selecteer **Maken**. Als u de app wilt weergeven op het dashboard, vinkt u het selectievakje **Vastmaken aan dashboard** aan.

    ![Het maken van een App Service-plan][2]

    Bij **App-naam** wordt de domeinnaam bijgewerkt zodat deze verwijst naar het domein van de AS-omgeving.

## <a name="post-ilb-ase-creation-validation"></a>Het maken van een post-ILB AS-omgeving valideren ##

Een ILB AS-omgeving verschilt iets van de niet-ILB AS-omgeving. Zoals al eerder is aangegeven, moet u uw eigen DNS beheren. U moet ook uw eigen certificaat voor HTTPS-verbindingen opgeven.

Nadat u de AS-omgeving hebt gemaakt, wordt in de domeinnaam het domein weergegeven dat u hebt opgegeven. In het menu **Instelling** wordt een nieuw item weergegeven genaamd **ILB-certificaat**. De AS-omgeving wordt gemaakt met een certificaat waarin het domein van de ILB AS-omgeving niet wordt opgegeven. Als u de AS-omgeving met dit certificaat gebruikt, verschijnt er een melding in de browser dat het certificaat ongeldig is. Met dit certificaat kunt u HTTPS eenvoudiger testen, maar u moet uw eigen certificaat uploaden dat is gekoppeld aan het domein van uw ILB AS-omgeving. Deze stap is nodig, ongeacht of het certificaat zelfondertekend is of is verkregen via een certificeringsinstantie.

![De domeinnaam van de ILB AS-omgeving][3]

Voor uw ILB AS-omgeving is een geldig SSL-certificaat vereist. Gebruik interne certificeringsinstanties, koop een certificaat van een externe gebruiker of gebruik een zelfondertekend certificaat. De volgende certificaatkenmerken moeten juist zijn geconfigureerd, ongeacht wat de bron van het SSL-certificaat is:

* **Onderwerp**: dit kenmerk moet zijn ingesteld op *.hier-uw-hoofddomein.
* **Alternatieve onderwerpnaam**: dit kenmerk moet zowel **.hier-uw-hoofddomein* als **.scm.hier-uw-hoofddomein* bevatten. SSL-verbindingen met de SCM/Kudu-site die zijn gekoppeld aan elke app, gebruiken een adres met de notatie *uw-app-naam.scm.hier-uw-hoofddomein*.

Converteer het SSL-certificaat naar een .pfx-bestand of sla het certificaat in deze indeling op. Het pfx-bestand moet alle tussenliggende certificaten en basiscertificaten bevatten. Beveilig het bestand met een wachtwoord.

Als u een zelfondertekend certificaat wilt maken, kunt u hier de PowerShell-opdrachten gebruiken. Zorg ervoor dat u de domeinnaam van de ILB AS-omgeving gebruikt in plaats van *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Het certificaat dat met deze PowerShell-opdrachten is gegenereerd, wordt in browsers gemarkeerd met een vlag, omdat het certificaat niet afkomstig is van een certificeringsinstantie uit de vertrouwensketen van de browser. Als u een certificaat wilt verkrijgen dat wordt vertrouwd in de browser, kunt u het aanschaffen bij een commerciële certificeringsinstantie uit de vertrouwensketen van de browser. 

![ILB-certificaat instellen][4]

Ga als volgt te werk om uw eigen certificaten te uploaden en de toegang te testen:

1. Ga nadat de AS-omgeving is gemaakt naar de gebruikersinterface van de AS-omgeving. Selecteer **AS-omgeving** > **Instellingen** > **ILB-certificaat**.

2. Als u het ILB-certificaat wilt instellen, selecteert u het .pfx-bestand met het certificaat en voert u het wachtwoord in. Het duurt enige tijd voordat deze stap is verwerkt. Er wordt een bericht weergegeven waarin staat dat een uploadbewerking wordt uitgevoerd.

3. Haal het ILB-adres voor de AS-omgeving op. Selecteer **AS-omgeving** > **Eigenschappen** > **Virtueel IP-adres**.

4. Maak een web-app in de AS-omgeving, nadat de AS-omgeving is gemaakt.

5. Maak een VM, als u in dit VNet nog geen VM hebt.

    > [!NOTE] 
    > Maak deze VM niet in hetzelfde subnet als de AS-omgeving, want deze actie mislukt of veroorzaakt problemen.
    >
    >

6. Stel het DNS in voor het domein van de AS-omgeving. U kunt een jokerteken gebruiken bij uw domein in het DNS. Als u een aantal eenvoudige tests wilt uitvoeren, bewerkt u het hostbestand op de VM zodat de naam van de web-app is ingesteld op het virtuele IP-adres:

    a. Als de AS-omgeving de domeinnaam _.ilbase.com_ heeft en u een web-app maakt met de naam _mytestapp_, is het adres _mytestapp.ilbase.com_. Vervolgens stelt u _mytestapp.ilbase.com_ in om te worden omgezet naar het ILB-adres. (In Windows bevindt het hostbestand zich op _C:\Windows\System32\drivers\etc\_.)

    b. Als u het publiceren van webimplementaties of de toegang tot de geavanceerde console wilt testen, maakt u een record voor _mytestapp.scm.ilbase.com_.

7. Gebruik een browser op deze VM en ga naar http://mytestapp.ilbase.com. (Of ga naar de web-app-naam die bij uw domein hoort.)

8. Gebruik een browser op deze VM en ga naar https://mytestapp.ilbase.com. Als u een zelfondertekend certificaat gebruikt, aanvaardt u het gebrek aan beveiliging.

    Het IP-adres voor de ILB wordt vermeld bij **IP-adressen**. Deze lijst bevat ook de IP-adressen die worden gebruikt voor het externe VIP-adres en voor binnenkomend beheerverkeer.

    ![IP-adres voor ILB][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>WebJobs, Functions en de ILB AS-omgeving ##

Een ILB AS-omgeving biedt ondersteuning voor zowel Functions als WebJobs. Als u echter met deze wilt werken via de portal, hebt u netwerktoegang tot de SCM-site nodig.  Dit betekent dat de host van de browser zich in het virtuele netwerk moet bevinden of ermee moet zijn verbonden.  

Als u Azure Functions in een ILB AS-omgeving gebruikt, wordt er mogelijk een foutbericht weergegeven waarin staat: Uw functies kunnen momenteel niet worden opgehaald. Probeer het later opnieuw. Deze fout treedt op omdat de gebruikersinterface van Functions gebruikmaakt van de SCM-site via HTTPS en het basiscertificaat geen onderdeel uitmaakt van de vertrouwensketen van de browser. Bij WebJobs treedt een soortgelijk probleem op. U kunt het volgende doen om dit probleem te voorkomen:

- Voeg het certificaat toe aan het vertrouwde certificaatarchief. Hierdoor wordt de blokkering van Edge en Internet Explorer opgeheven.
- Gebruik Chrome en ga eerst naar de SCM-site, accepteer het niet-vertrouwde certificaat en ga vervolgens naar de portal.
- Gebruik een commercieel certificaat uit de vertrouwensketen van de browser.  Dit is de beste optie.  

## <a name="dns-configuration"></a>DNS-configuratie ##

Wanneer u een extern VIP-adres gebruikt, wordt de DNS-server beheerd met Azure. Elke app die is gemaakt in de AS-omgeving wordt automatisch toegevoegd aan Azure DNS, wat een openbaar DNS is. In een ILB AS-omgeving moet u uw eigen DNS beheren. Voor een gegeven domein, bijvoorbeeld _contoso.net_, moet u DNS A-records in het DNS maken die verwijzen naar uw ILB-adres voor:

- *.contoso.net
- *.scm.contoso.net

Als het domein van de ILB AS-omgeving voor meerdere zaken buiten deze AS-omgeving wordt gebruikt, moet u het DNS mogelijk per app-naam beheren. Deze methode is lastig omdat u elke nieuwe app-naam moet toevoegen in het DNS wanneer u deze maakt. Daarom raden we u aan om een toegewezen domein te gebruiken.

## <a name="publish-with-an-ilb-ase"></a>Publiceren met een ILB AS-omgeving ##

Elke app die wordt gemaakt, heeft twee eindpunten. In een ILB AS-omgeving hebt u *&lt;app-naam>.&lt;Domein voor ILB AS-omgeving>* en *&lt;app-naam>.scm.&lt;Domein voor ILB AS-omgeving>*. 

De SCM-sitenaam leidt naar de Kudu-console, genaamd de **Geavanceerde portal**, binnen Azure Portal. Met behulp van de Kudu-console kunt u omgevingsvariabelen bekijken, de schijf verkennen, een console gebruiken, en nog veel meer. Zie [Kudu-console voor Azure App Service][Kudu] voor meer informatie. 

In de App Service met meerdere tenants en in een Externe AS-omgeving is sprake van eenmalige aanmelding tussen Azure Portal en de Kudu-console. Voor de ILB AS-omgeving moet u, echter, uw publicatiereferenties gebruiken om u aan te melden bij de Kudu-console.

Op internet gebaseerde CI-systemen, zoals GitHub en Visual Studio Team Services, werken niet met een ILB AS-omgeving, omdat het publicatie-eindpunt niet toegankelijk is via internet. In plaats hiervan moet u een CI-systeem gebruiken dat gebruikmaakt van een pull-model, bijvoorbeeld Dropbox.

De publicatie-eindpunten voor apps in een ILB AS-omgeving maken gebruik van het domein waarmee de ILB AS-omgeving is gemaakt. Dit domein wordt weergegeven in het publicatieprofiel van de app en in de portalblade van de app (**Overzicht** > **Essentials** en ook **Eigenschappen**). Als u een ILB AS-omgeving hebt met het subdomein *contoso.net* en een app met de naam *mytest*, gebruikt u *mytest.contoso.net* voor FTP en *mytest.scm.contoso.net* voor webimplementatie.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Een ILB AS-omgeving koppelen met een WAF-apparaat ##

Azure App Service biedt veel beveiligingsmaatregelen die het systeem beveiligen. Deze maatregelen helpen ook om te bepalen of een app is gehackt. De beste beveiliging voor een webtoepassing is om een hostingplatform, zoals Azure App Service, te koppelen met een WAF (Web Application Firewall). De ILB AS-omgeving is geschikt voor dergelijk gebruik omdat deze een toepassingseindpunt met netwerkisolatie heeft.

Zie [Configure a web application firewall with your App Service environment][ASEWAF] (Een WAF (Web Application Firewall) configureren met uw App Service-omgeving) voor meer informatie over het configureren van de ILB AS-omgeving met een WAF-apparaat. In dit artikel leest u hoe u een virtueel Barracuda-apparaat gebruikt met de AS-omgeving. Een andere optie is het gebruik van Azure Application Gateway. Application Gateway maakt gebruik van de OWASP-kernregels om alle toepassingen te beveiligen die erachter zijn geplaatst. Zie [Introduction to the Azure web application firewall][AppGW] (Inleiding tot de WAF (Web Application Firewall) in Azure) voor meer informatie over Application Gateway.

## <a name="get-started"></a>Aan de slag ##

* Zie [Introduction to App Service environments][Intro] (Inleiding tot App Service-omgevingen) om aan de slag te gaan met AS-omgevingen.
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md

---
title: Maken en gebruiken van een interne load balancer met een Azure App Service-omgeving
description: "Meer informatie over het maken en gebruiken van een geïsoleerd van internet Azure App Service-omgeving"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: cc7bdd7860506c20187dc913b72111824d1737ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Maken en gebruiken van een interne load balancer met een App Service-omgeving #

 Azure App Service-omgeving is een implementatie van Azure App Service in een subnet in een Azure-netwerk (VNet). Er zijn twee manieren voor het implementeren van een App Service-omgeving (as-omgeving): 

- Een VIP-adres op een externe IP-adres, vaak aangeduid als een externe as-omgeving.
- Een VIP-adres op een interne IP-adres, vaak genoemd, een ILB-as-omgeving omdat het interne eindpunt, een interne load balancer (ILB is). 

In dit artikel leest u hoe een ILB-as-omgeving te maken. Zie voor een overzicht op de as-omgeving [Inleiding tot de App Service-omgevingen][Intro]. Zie voor meer informatie over het maken van een externe as-omgeving, [maken van een externe as-omgeving][MakeExternalASE].

## <a name="overview"></a>Overzicht ##

U kunt een as-omgeving met een internet toegankelijke eindpunt of met een IP-adres implementeren in uw VNet. Om in te stellen het IP-adres in een VNet-adres, moet de as-omgeving worden geïmplementeerd met een ILB. Wanneer u uw as-omgeving met een ILB implementeert, moet u het volgende opgeven:

-   Uw eigen domein dat u bij het maken van uw apps.
-   Het certificaat dat wordt gebruikt voor HTTPS.
-   DNS-beheer voor uw domein.

Tegenprestatie kunt u doen, zoals:

-   Intranet-hosttoepassingen veilig in de cloud, die u via een site-naar-site of Azure ExpressRoute VPN-toegang.
-   Host-apps in de cloud die niet zijn opgenomen in de openbare DNS-servers.
-   Internet geïsoleerd back-end-apps, die uw front-apps kunnen veilig worden geïntegreerd met maken.

### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit ###

Er zijn een aantal zaken die u niet mogelijk wanneer u een ILB-as-omgeving:

-   IP-gebaseerde SSL wordt gebruikt.
-   IP-adressen toewijzen aan specifieke apps.
-   Koop en een certificaat met een app via de Azure portal gebruiken. U kunt certificaten rechtstreeks vanuit een certificeringsinstantie verkrijgen en ze gebruiken met uw apps. U kunt deze kan niet verkrijgen via de Azure portal.

## <a name="create-an-ilb-ase"></a>Een as ILB-omgeving maken ##

Een as ILB-omgeving maken:

1. Selecteer in de Azure-portal **nieuw** > **Web en mobiel** > **App Service-omgeving**.

2. Selecteer uw abonnement.

3. Selecteer of maak een resourcegroep.

4. Selecteer of maak een VNet.

5. Als u een bestaande VNet selecteert, moet u een subnet voor het opslaan van de as-omgeving maken. Zorg ervoor dat een subnetgrootte die groot genoeg voor een eventuele toekomstige groei van uw as-omgeving instellen. Een grootte van het is raadzaam `/25`, waardoor 128 adressen heeft en een maximale grootte as-omgeving kan verwerken. De minimale grootte die u kunt selecteren is een `/28`. Nadat de infrastructuur nodig heeft, kan de grootte van deze tot maximaal 11 exemplaren worden uitgebreid.

    * Verder dan het maximumaantal 100 exemplaren in uw App Service-abonnementen.

    * In de buurt van 100, maar met meer snelle front-schaling schalen.

6. Selecteer **virtuele netwerklocatie** > **virtuele netwerkconfiguratie**. Stel de **VIP Type** naar **interne**.

7. Voer de domeinnaam van een. Dit domein wordt gebruikt voor apps die zijn gemaakt in deze as-omgeving. Er zijn enkele beperkingen. Kan niet worden:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   De aangepaste domeinnaam gebruikt voor apps en de domeinnaam die wordt gebruikt door uw as-omgeving kunnen elkaar niet overlappen. Voor een as ILB-omgeving met de domeinnaam _contoso.com_, u kunt aangepaste domeinnamen voor uw apps zoals niet gebruiken:

    * www.contoso.com

    * ABCD.def.contoso.com

    * ABCD.contoso.com

   Als u weet dat de aangepaste domeinnamen voor uw apps, kiest u een domein voor de as ILB-omgeving die u geen een conflict met deze aangepaste domeinnamen hebt. In dit voorbeeld kunt u ongeveer *contoso internal.com* voor het domein van uw as-omgeving omdat dat geen met aangepaste domeinnamen die eindigen conflicten op *. contoso.com*.

8. Selecteer **OK**, en selecteer vervolgens **maken**.

    ![ASE maken][1]

Op de **virtueel netwerk** blade, er is een **virtuele netwerkconfiguratie** optie. U kunt deze gebruiken om een externe VIP of een interne VIP te selecteren. De standaardwaarde is **externe**. Als u selecteert **externe**, uw as-omgeving maakt gebruik van een internet toegankelijke VIP. Als u selecteert **intern**, uw as-omgeving is geconfigureerd met een ILB op een IP-adres binnen uw VNet.

Nadat u hebt geselecteerd **intern**, de mogelijkheid meer IP-adressen toevoegen aan uw as-omgeving wordt verwijderd. In plaats daarvan moet u het domein van de as-omgeving te bieden. In een as met een externe VIP-omgeving, wordt de naam van de as-omgeving in het domein gebruikt voor apps die zijn gemaakt in die as-omgeving.

Als u instelt **VIP Type** naar **intern**, de naam van uw as-omgeving wordt niet gebruikt in het domein voor de as-omgeving. U Geef expliciet het domein. Als uw domein *contoso.corp.net* en u een app maken in de betreffende as-omgeving met de naam *timereporting*, de URL voor die app timereporting.contoso.corp.net is.


## <a name="create-an-app-in-an-ilb-ase"></a>Een app maken in een ILB-as-omgeving ##

U kunt een app maken in een ILB-as-omgeving op dezelfde manier als u een app in een as-omgeving normaal maken.

1. Selecteer in de Azure-portal **nieuw** > **Web en mobiel** > **Web** of **Mobile** of **API-App**.

2. Voer de naam van de app.

3. Selecteer het abonnement.

4. Selecteer of maak een resourcegroep.

5. Selecteer of maak een App Service-abonnement. Als u maken van een nieuw App Service-plan wilt, selecteert u uw as-omgeving als de locatie. Selecteer de worker-groep waar u uw App Service-abonnement worden gemaakt. Wanneer u de App Service-abonnement hebt gemaakt, selecteert u uw as-omgeving als de locatie en de worker-groep. Wanneer u de naam van de app opgeeft, wordt het domein onder de appnaam van uw vervangen door het domein voor uw as-omgeving.

6. Selecteer **Maken**. Als u wilt dat de app op uw dashboard wilt weergeven, selecteert u de **vastmaken aan dashboard** selectievakje.

    ![App Service-abonnement maken][2]

    Onder **appnaam**, de domeinnaam wordt bijgewerkt met het domein van uw as-omgeving.

## <a name="post-ilb-ase-creation-validation"></a>Validatie van post ILB as-omgeving maken ##

Er is een ILB-as-omgeving iets anders dan de niet - ILB as-omgeving. Als al hebt genoteerd moet u uw eigen DNS beheren. U moet ook uw eigen certificaat voor HTTPS-verbindingen opgeven.

Nadat u uw as-omgeving hebt gemaakt, ziet u de domeinnaam in het opgegeven domein. Een nieuw item wordt weergegeven in de **instelling** menu aangeroepen **ILB certificaat**. De as-omgeving wordt gemaakt met een certificaat dat niet Geef het domein ILB as-omgeving. Als u het as-omgeving met dat certificaat gebruikt, ziet uw browser u dat het is ongeldig. Dit certificaat vereenvoudigt het testen van HTTPS, maar u moet voor het uploaden van uw eigen certificaat dat gekoppeld aan uw domein ILB as-omgeving. Deze stap is nodig, ongeacht of het certificaat is zelfondertekend of verkregen via een certificeringsinstantie.

![De domeinnaam ILB as-omgeving][3]

Uw as ILB-omgeving moet een geldig SSL-certificaat. Interne CA's gebruiken, een certificaat kopen bij een externe verlener of een zelfondertekend certificaat gebruiken. Ongeacht de bron van het SSL-certificaat moeten de volgende kenmerken van de certificaten correct worden geconfigureerd:

* **Onderwerp**: dit kenmerk moet worden ingesteld op *.your, root, domein, hier.
* **Alternatieve onderwerpnaam**: dit kenmerk moet bevatten beide **.your, root, domein, hier* en **.scm.your-hoofdmap-domain-hier*. SSL-verbindingen met de SCM/Kudu-site die is gekoppeld aan elke app een adres van het formulier gebruiken *your-app-name.scm.your-root-domain-here*.

Het SSL-certificaat, converteren/opslaan als een .pfx-bestand. Het pfx-bestand moet alle tussenliggende bevatten en de hoofd-certificaten. Beveilig deze met een wachtwoord.

Als u maken van een zelfondertekend certificaat wilt, kunt u hier de PowerShell-opdrachten gebruiken. Zorg ervoor dat u uw domeinnaam ILB as-omgeving in plaats van *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Het certificaat dat u deze PowerShell-opdrachten genereren is door browsers gemarkeerd, omdat het certificaat is niet gemaakt door een certificeringsinstantie die in de vertrouwensketen van uw browser. Als u een certificaat dat uw browser vertrouwt, kunt u het aanschaffen van een commerciële certificeringsinstantie in uw browser vertrouwensketen. 

![Set ILB certificaat][4]

Uw eigen certificaten uploaden en toegang testen:

1. Nadat de as-omgeving is gemaakt, gaat u naar de gebruikersinterface van de as-omgeving. Selecteer **as-omgeving** > **instellingen** > **ILB certificaat**.

2. Het certificaat ILB instellen, selecteert u het PFX-certificaatbestand en voer het wachtwoord. Deze stap duurt enige tijd om te verwerken. Er verschijnt een bericht dat een uploadbewerking uitgevoerd wordt.

3. Haal de ILB-adres voor uw as-omgeving. Selecteer **as-omgeving** > **eigenschappen** > **virtueel IP-adres**.

4. Een web-app maken in uw as-omgeving, nadat de as-omgeving is gemaakt.

5. Een virtuele machine maken als u niet in dit VNet hebt.

    > [!NOTE] 
    > Niet probeert te maken van deze virtuele machine in hetzelfde subnet als de as-omgeving, omdat deze wordt niet uitgevoerd of problemen veroorzaken.
    >
    >

6. Stel de DNS-server voor uw domein as-omgeving. U kunt een jokerteken gebruiken met uw domein in uw DNS. Bewerk het bestand hosts op de virtuele machine de naam van de web-app instellen op het VIP-IP-adres hiervoor enkele eenvoudige tests uit:

    a. Als uw as-omgeving de domeinnaam heeft _. ilbase.com_ en maken van de web-app met de naam _mytestapp_, het gericht op _mytestapp.ilbase.com_. Vervolgens stelt u _mytestapp.ilbase.com_ omzetten naar het adres van de ILB. (Het hosts-bestand in vensters is _C:\Windows\System32\drivers\etc\_.)

    b. Als u wilt testen webpublicaties implementatie of toegang tot de geavanceerde console, maakt u een record voor _mytestapp.scm.ilbase.com_.

7. Een browser gebruiken die op deze virtuele machine en Ga naar http://mytestapp.ilbase.com. (Of Ga naar wat de naam van uw web-app met uw domein is.)

8. Een browser gebruiken die op deze virtuele machine en Ga naar https://mytestapp.ilbase.com. Als u een zelfondertekend certificaat gebruikt, accepteert u het gebrek aan beveiliging.

    Het IP-adres voor de ILB wordt vermeld onder **IP-adressen**. Deze lijst heeft ook de IP-adressen die door het externe VIP en voor binnenkomende beheer van verkeer.

    ![ILB IP-adres][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Webtaken, functies en de as ILB-omgeving ##

Zowel functies en webtaken worden ondersteund op een as ILB-omgeving, maar voor de portal om hiermee te werken, moet u toegang tot het netwerk hebben tot de SCM-site.  Dit betekent dat de browser moet op een host die is in- of verbonden met het virtuele netwerk.  

Als u Azure Functions op een as ILB-omgeving gebruikt, krijgt u mogelijk een foutbericht weergegeven waarin wordt gemeld 'We kunnen geen uw functies nu ophalen. Probeer het later opnieuw." Deze fout treedt op omdat de gebruikersinterface van de functies maakt gebruik van de site SCM via HTTPS en het basiscertificaat niet in de vertrouwensketen browser is. Webtaken heeft een soortgelijk probleem. U kunt het volgende doen om dit probleem te voorkomen:

- Het certificaat toevoegen aan uw vertrouwde certificaatarchief. Dit blokkering opgeheven rand en Internet Explorer.
- Chrome gebruiken en gaat u naar de site SCM eerst, niet-vertrouwd certificaat te accepteren en gaat u naar de portal.
- Een commerciële certificaat in uw browser vertrouwensketen gebruiken.  Dit is de beste optie.  

## <a name="dns-configuration"></a>DNS-configuratie ##

Wanneer u een externe VIP gebruikt, wordt de DNS-server wordt beheerd door Azure. Elke app gemaakt in uw as-omgeving wordt automatisch toegevoegd aan Azure DNS, die een openbare DNS-server is. In een ILB as-omgeving, moet u uw eigen DNS beheren. Voor een bepaald domein, zoals _contoso.net_, moet u DNS A-records in DNS die verwijzen naar uw adres ILB voor maken:

- *. contoso.net
- *. scm.contoso.net

Als uw domein ILB as-omgeving wordt gebruikt voor meerdere dingen buiten deze as-omgeving, moet u mogelijk DNS beheren op basis van de per-app-naam. Deze methode is lastig omdat u de appnaam van elke nieuwe toevoegen in uw DNS moet wanneer u deze maakt. Daarom is het raadzaam dat u een speciale domein.

## <a name="publish-with-an-ilb-ase"></a>Publiceren met een ILB-as-omgeving ##

Voor elke app die gemaakt, zijn er twee eindpunten. In een ILB as-omgeving, hebt u  *&lt;app-naam >.&lt; ILB as-omgeving Domain >* en  *&lt;app-naam > .scm.&lt; ILB as-omgeving Domain >*. 

De sitenaam SCM Hiermee gaat u naar de Kudu-console, genaamd de **geavanceerde portal**, binnen de Azure-portal. De Kudu-console kunt u omgevingsvariabelen weergeven, de schijf verkennen, gebruikt u een console, en nog veel meer. Zie voor meer informatie [Kudu-console voor Azure App Service][Kudu]. 

Er is eenmalige aanmelding tussen de Azure-portal en de Kudu-console in de multitenant-App Service en in een externe as-omgeving. Voor de ILB as-omgeving, echter, moet u uw publishing referenties gebruiken om aan te melden bij de Kudu-console.

Internetgebaseerde CI systemen, zoals GitHub en Visual Studio Team Services, werkt niet met een ILB-as-omgeving omdat het publishing eindpunt niet toegankelijk internet. In plaats daarvan moet u een CI-besturingssysteem dat gebruikmaakt van een pull-model, zoals Dropbox gebruiken.

Het domein dat de as ILB-omgeving is gemaakt met de publicatie eindpunten voor apps in een ILB-as-omgeving gebruiken Dit domein wordt weergegeven in het profiel voor het publiceren van de app en de portalblade van de app (**overzicht** > **Essentials** en ook **eigenschappen**). Als u een ILB-as-omgeving met het subdomein hebt *contoso.net* en een app met de naam *mytest*, gebruik *mytest.contoso.net* voor FTP en *mytest.scm.contoso.net* voor implementatie.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Combineer een ILB-as-omgeving met een WAF-apparaat ##

Azure App Service biedt veel beveiligingsfuncties die beschermen van het systeem. Ze helpen ook om te bepalen of een app is gehackte. De beste beveiliging voor een webtoepassing is het een host-platform, zoals Azure App Service, Combineer met web application firewall (WAF). Omdat de as ILB-omgeving een toepassingseindpunt netwerk geïsoleerd van heeft, is het geschikt is voor dit gebruik.

Zie voor meer informatie over het configureren van de as ILB-omgeving met een apparaat WAF, [web application firewall configureren met uw App-serviceomgeving][ASEWAF]. Dit artikel laat zien hoe u een virtueel apparaat Barracuda met uw as-omgeving. Een andere optie is het gebruik van Azure Application Gateway. Toepassingsgateway gebruikt de OWASP core-regels voor het beveiligen van alle toepassingen erachter geplaatst. Zie voor meer informatie over Application Gateway [Inleiding tot de Azure-web application firewall][AppGW].

## <a name="get-started"></a>Aan de slag ##

* Als u wilt beginnen met ASEs, Zie [Inleiding tot de App Service-omgevingen][Intro].
 
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

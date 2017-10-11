Domain Name System (DNS) wordt gebruikt om de zaken niet vinden op het internet. Bijvoorbeeld, wanneer u een adres in uw browser invoeren of klik op een koppeling op een webpagina, gebruikt deze DNS te zetten van het domein in een IP-adres. Het IP-adres is ongeveer zoals een adres, maar het is niet erg menselijke gebruiksvriendelijke. Bijvoorbeeld, is het veel eenvoudiger te onthouden een DNS-naam zoals **contoso.com** dan een IP-adres zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3 onthoudt.

De DNS-systeem is gebaseerd op *records*. Registreert een specifieke koppelen *naam*, zoals **contoso.com**, met een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, u een naam in DNS zoekt, zoekt de record en maakt gebruik van wat deze verwijst naar als het adres. Als de waarde die deze naar verwijst een IP-adres is, wordt in de browser die waarde gebruiken. Als deze naar een andere DNS-naam verwijst, klikt u vervolgens heeft de toepassing oplossing opnieuw te doen. Alle naamomzetting wordt uiteindelijk eindigen op een IP-adres.

Wanneer u een Azure-Website maakt, wordt een DNS-naam automatisch toegewezen aan de site. Deze naam heeft de vorm van  **&lt;yoursitename&gt;. azurewebsites.net**. Wanneer u uw website als een Azure Traffic Manager-eindpunt toevoegt, uw website vervolgens toegankelijk is via de  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domein.

> [!NOTE]
> Als uw website is geconfigureerd als een Traffic Manager-eindpunt, gebruikt u de **. trafficmanager.net** genomen bij het maken van DNS-records.
> 
> U kunt CNAME-records alleen gebruiken met Traffic Manager
> 
> 

Er zijn ook meerdere typen met records, elk met hun eigen functies en -beperkingen, maar voor websites als Traffic Manager-eindpunten geconfigureerd, alleen belangrijk voor ons over een bepaald; *CNAME* records.

### <a name="cname-or-alias-record"></a>CNAME- of Alias-record
Een CNAME-record verwijst een *specifieke* DNS-naam, zoals **mail.contoso.com** of **www.contoso.com**, naar een andere (canoniek) domeinnaam. In het geval van Azure Websites met Traffic Manager, de canonieke domeinnaam is de  **&lt;myapp >. trafficmanager.net** domeinnaam van uw Traffic Manager-profiel. Zodra gemaakt, CNAME maakt een alias voor de  **&lt;myapp >. trafficmanager.net** domeinnaam. De CNAME-vermelding wordt omgezet in het IP-adres van uw  **&lt;myapp >. trafficmanager.net** domeinnaam automatisch, zodat als het IP-adres van de website wordt gewijzigd, hoeft u geen actie te ondernemen.

Wanneer verkeer op Traffic Manager binnenkomt, worden vervolgens het verkeer naar uw website, met behulp van de taakverdelingsmethode die hij geconfigureerd wordt voor routeert. Dit is volledig transparant voor bezoekers van uw website. Ze zien alleen de aangepaste domeinnaam in hun browser.

> [!NOTE]
> Sommige registrars domein dat u subdomeinen toewijzen wanneer u een CNAME-record, zoals alleen **www.contoso.com**, en niet de hoofd-namen, zoals **contoso.com**. Zie voor meer informatie over de CNAME-records, de documentatie bij uw registrar <a href="http://en.wikipedia.org/wiki/CNAME_record">de vermelding Wikipedia (Engelstalig) op de CNAME-record</a>, of de <a href="http://tools.ietf.org/html/rfc1035">IETF domeinnamen - implementatie en specificatie</a> document.
> 
> 


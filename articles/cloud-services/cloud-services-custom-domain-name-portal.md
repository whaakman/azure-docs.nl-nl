---
title: Een aangepaste domeinnaam configureren in Cloudservices | Microsoft Docs
description: Informatie over het weergeven van uw Azure-toepassing of de gegevens met het internet op een aangepast domein door DNS-instellingen configureren.  Deze voorbeelden worden de Azure portal gebruiken.
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 139ec6578dc9e76039c5fb13e7a7741aa8ba4e0d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Een aangepaste domeinnaam voor een Azure-cloud-service configureren
Wanneer u een Cloudservice maakt, Azure toegewezen aan een subdomein van **cloudapp.net**. Bijvoorbeeld, als uw Cloudservice met de naam 'contoso', zich uw gebruikers toegang tot uw toepassing op een URL als http://contoso.cloudapp.net. Azure wordt ook een virtueel IP-adres toegewezen.

Echter, kan ook worden blootgesteld uw toepassing op uw eigen domeinnaam, zoals **contoso.com**. In dit artikel wordt uitgelegd hoe reserve of een aangepaste domeinnaam configureren voor Cloud Service-web-rollen.

U al begrijpen wat CNAME en A-records zijn? [Korte inleiding voorbij de uitleg](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> De procedures in deze taak van toepassing op Azure Cloud Services. Zie voor App-Services, [aangepaste DNS-naam van een bestaande toewijzen aan Azure Web Apps](../app-service/app-service-web-tutorial-custom-domain.md). Zie voor storage-accounts [een aangepaste domeinnaam configureren voor het eindpunt van de Azure Blob-opslag](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Aan de slag sneller--gebruik van de nieuwe Azure [scenario begeleide](http://support.microsoft.com/kb/2990804)!  Het maakt koppelen van een aangepaste domeinnaam en beveiligen van communicatie (SSL) met Azure Cloud Services of Azure Websites, een module.
> 
> 

## <a name="understand-cname-and-a-records"></a>Records CNAME en A begrijpen
CNAME (of aliasrecords) en een records kunnen u een domeinnaam koppelen aan een specifieke server (of service in dit geval) echter werken ze anders. Er zijn ook enkele specifieke overwegingen bij het gebruik van A-records met Azure-Cloud-services die u overwegen moet voordat u besluit die wordt gebruikt.

### <a name="cname-or-alias-record"></a>CNAME- of Alias-record
Een CNAME-record verwijst een *specifieke* domein, zoals **contoso.com** of **www.contoso.com**, naar een canonieke domeinnaam. In dit geval de canonieke domeinnaam is de **[myapp] .cloudapp .net** domeinnaam van uw Azure gehoste toepassing. Zodra gemaakt, CNAME maakt een alias voor de **[myapp] .cloudapp .net**. De CNAME-vermelding wordt omgezet in het IP-adres van uw **[myapp] .cloudapp .net** service automatisch, zodat als het IP-adres van de cloudservice wordt gewijzigd, u hoeft geen actie te ondernemen.

> [!NOTE]
> Sommige registrars domein kunnen alleen u subdomeinen toewijzen wanneer u een CNAME-record, bijvoorbeeld www.contoso.com en geen basis-namen, zoals contoso.com. Zie voor meer informatie over de CNAME-records, de documentatie bij uw registrar [de vermelding Wikipedia (Engelstalig) op de CNAME-record](http://en.wikipedia.org/wiki/CNAME_record), of de [IETF domeinnamen - implementatie en specificatie](http://tools.ietf.org/html/rfc1035) document.
> 
> 

### <a name="a-record"></a>een record
Een *A* record wordt een domein, zoals toegewezen **contoso.com** of **www.contoso.com**, *of een jokertekendomein* zoals  **\*. contoso.com**, een IP-adres. In het geval van een Azure Cloud Service, het virtuele IP-adres van de service. Zodat het belangrijkste voordeel van een A-record via een CNAME-record is dat u kunt een vermelding die gebruikmaakt van een jokerteken, zoals \* **. contoso.com**, die zou staat aanvragen verwerken voor meerdere subdomeinen zoals **mail.contoso.com**, **login.contoso.com**, of **www.contso.com**.

> [!NOTE]
> Omdat een A-record is toegewezen aan een statisch IP-adres, kan deze wijzigingen automatisch niet omzetten naar de IP-adres van uw Cloud-Service. Het IP-adres wordt gebruikt door uw Cloud-Service is toegewezen de eerste keer dat u implementeert op een lege sleuf (productie of staging.) Als u de implementatie voor de site verwijdert, wordt het IP-adres wordt vrijgegeven door Azure en alle toekomstige implementaties in de sleuf kunnen een nieuw IP-adres worden gegeven.
> 
> Het IP-adres van een opgegeven implementatiesleuf (productie of staging) is gemakkelijk persistent bij het wisselen tussen fasering en productie-implementaties of het uitvoeren van een in-place upgrade van een bestaande implementatie. Zie voor meer informatie over het uitvoeren van deze acties [cloudservices beheren](cloud-services-how-to-manage-portal.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Voeg een CNAME-record voor uw aangepaste domein
Als u wilt een CNAME-record maken, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's van uw registrar. Elke registrar heeft een methode vergelijkbaar maar enigszins verschillen van het opgeven van een CNAME-record, maar de concepten zijn hetzelfde.

1. Gebruik een van deze methoden om te zoeken naar de **. cloudapp.net** domeinnaam toegewezen aan uw cloudservice.
   
   * Meld u aan bij de [Azure-portal], selecteer uw cloudservice, kijkt u naar de **Essentials** sectie en gaat u naar de **Site-URL** vermelding.
     
       ![snelle weergave sectie URL van de site wordt weergegeven][csurl]
     
       **OR**
   * Installeer en configureer [Azure Powershell](/powershell/azure/overview), en gebruik vervolgens de volgende opdracht:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Sla de domeinnaam die is gebruikt in de URL die is geretourneerd door een van de methoden, zoals u deze hebt bij het maken van een CNAME-record.
2. Meld u aan bij uw DNS-registratieservice website en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gebieden van de site met het label **domeinnaam**, **DNS**, of **naam Serverbeheer**.
3. Nu kunt u selecteren of invoeren van CNAME zoeken. Mogelijk moet het recordtype in een vervolgkeuzelijst omlaag selecteren of Ga naar een pagina Geavanceerde instellingen. U ziet er voor de woorden **CNAME**, **Alias**, of **subdomeinen**.
4. U moet ook Geef het domein of subdomein alias voor de CNAME, zoals **www** als u wilt maken van een alias voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein wilt, kan dit worden vermeld als de '**@**' symbool in DNS-hulpprogramma's van uw registrar.
5. Vervolgens moet u een canonieke hostnaam, uw toepassing is opgeven **cloudapp.net** domein in dit geval.

Bijvoorbeeld de volgende CNAME-record al het verkeer van stuurt **www.contoso.com** naar **contoso.cloudapp.net**, de aangepaste domeinnaam van uw geïmplementeerde toepassing:

| Alias/Host-naam/subdomein | Canonieke domein |
| --- | --- |
| www |Contoso.cloudapp.NET |

> [!NOTE]
> Een bezoeker van **www.contoso.com** Zie nooit de waar host (contoso.cloudapp.net), zodat het proces doorsturen onzichtbaar voor de eindgebruiker.
> 
> Het bovenstaande voorbeeld is alleen van toepassing op het verkeer bij de **www** subdomein. Aangezien u kunt geen jokertekens CNAME-records gebruiken, moet u een CNAME voor elk domein/subdomein maken. Als u wilt dat om verkeer te regelen van subdomeinen, zoals *. contoso.com naar uw adres cloudapp.net, kunt u een **Omleidings-URL** of **URL doorsturen** vermelding in uw DNS-instellingen, of maak een A-record.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Een A-record voor uw aangepaste domein toevoegen
Voor het maken van een A-record, moet u het virtuele IP-adres van uw cloudservice te zoeken. Voeg een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein met behulp van de hulpprogramma's van uw registrar. Elke registrar heeft een methode vergelijkbaar maar enigszins verschillen van het opgeven van een A-record, maar de concepten zijn hetzelfde.

1. Gebruik een van de volgende methoden om het IP-adres van uw cloudservice ophalen.
   
   * Meld u aan bij de [Azure-portal], selecteer uw cloudservice, kijkt u naar de **Essentials** sectie en gaat u naar de **openbare IP-adressen** vermelding.
     
       ![snelle weergave sectie waarin het VIP][vip]
     
       **OR**
   * Installeer en configureer [Azure Powershell](/powershell/azure/overview), en gebruik vervolgens de volgende opdracht:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Sla het IP-adres, zoals u deze hebt bij het maken van een A-record.
2. Meld u aan bij uw DNS-registratieservice website en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gebieden van de site met het label **domeinnaam**, **DNS**, of **naam Serverbeheer**.
3. Nu kunt u selecteren of invoeren van de record zoeken. Mogelijk moet het recordtype in een vervolgkeuzelijst omlaag selecteren of Ga naar een pagina Geavanceerde instellingen.
4. Selecteer of typ het domein of subdomein die door deze A-record wordt gebruikt. Selecteer bijvoorbeeld **www** als u wilt maken van een alias voor **www.customdomain.com**. Als u maken van een vermelding jokerteken voor alle subdomeinen wilt, voert u ' ***'. Hierin vindt u alle subdomeinen zoals **mail.customdomain.com**, **login.customdomain.com**, en **www.customdomain.com**.
   
    Als u maken van een A-record voor het hoofddomein wilt, kan dit worden vermeld als de '**@**' symbool in DNS-hulpprogramma's van uw registrar.
5. Geef het IP-adres van uw cloudservice in het opgegeven veld. Hiermee wordt de domein-vermelding in de A-record met het IP-adres van uw cloud service-implementatie gebruikt.

Bijvoorbeeld, het volgende een record al het verkeer van stuurt **contoso.com** naar **137.135.70.239**, het IP-adres van uw geïmplementeerde toepassing:

| Host-naam/subdomein | IP-adres |
| --- | --- |
| @ |137.135.70.239 |

Dit voorbeeld wordt het maken van een A-record voor het hoofddomein. Als u maken van een vermelding jokertekens ten aanzien van alle subdomeinen wilt, voert u ' ***' als het subdomein.

> [!WARNING]
> IP-adressen in Azure zijn standaard dynamisch. Wilt u waarschijnlijk gebruik van een [gereserveerd IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) om ervoor te zorgen dat uw IP-adres niet wijzigt.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Cloud Services beheren](cloud-services-how-to-manage-portal.md)
* [CDN-inhoud toewijzen aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md)
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over hoe [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png

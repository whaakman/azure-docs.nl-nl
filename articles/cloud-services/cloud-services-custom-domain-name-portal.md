---
title: Een aangepaste domeinnaam configureren in Cloud Services | Microsoft Docs
description: Leer hoe u uw Azure-toepassing of de gegevens naar het internet op een aangepast domein beschikbaar maken door het configureren van DNS-instellingen.  Deze voorbeelden gebruiken de Azure-portal.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: e339d50a379015d7aebe19b25127e9804d8efc52
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237244"
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Een aangepaste domeinnaam voor een Azure-cloudservice configureren
Wanneer u een Cloudservice maakt, Azure toegewezen aan een subdomein van **cloudapp.net**. Bijvoorbeeld, als uw Cloudservice is de naam "contoso", uw gebruikers zich toegang tot uw toepassing op een URL, zoals http://contoso.cloudapp.net. Azure wijst ook een virtueel IP-adres.

Echter, u kunt ook uw toepassingen beschikbaar stellen op uw eigen domeinnaam, zoals **contoso.com**. In dit artikel wordt uitgelegd hoe u reserve of een aangepaste domeinnaam voor webrollen Cloudservice configureren.

U al begrijpen wat CNAME- en A-records zijn? [Landingspagina voorbij de uitleg](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> De procedures in deze taak gelden voor Azure Cloud Services. Zie voor App-Services, [een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service/app-service-web-tutorial-custom-domain.md). Zie voor storage-accounts, [een aangepaste domeinnaam voor uw Azure Blob storage-eindpunt configureren](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Aan de slag sneller, gebruikt u de nieuwe Azure [begeleide procedure](https://support.microsoft.com/kb/2990804)!  Het maakt koppelen van een aangepaste domeinnaam en beveiligen van communicatie (SSL) met Azure Cloud Services of Azure Websites, een module.
> 
> 

## <a name="understand-cname-and-a-records"></a>CNAME- en A-records begrijpen
CNAME (of aliasrecords) en een records, kunt u de naam van een domein koppelen aan een specifieke server (of-service in dit geval) maar ze werken anders. Er zijn ook enkele specifieke overwegingen bij het gebruik van A-records met Azure Cloud services waarmee u rekening houden moet voordat u besluit te gebruiken.

### <a name="cname-or-alias-record"></a>CNAME- of Alias-record
Een CNAME-record verwijst een *specifieke* domein, zoals **contoso.com** of **www.contoso.com**, naar een canonieke domeinnaam. In dit geval de canonieke domeinnaam is de **[Mijntoep] .cloudapp .net** domeinnaam van uw Azure-toepassing die wordt gehost. Nadat u hebt gemaakt, de CNAME maakt u een alias voor de **[Mijntoep] .cloudapp .net**. De CNAME-vermelding wordt omgezet in het IP-adres van uw **[Mijntoep] .cloudapp .net** service automatisch, zodat als het IP-adres van de cloudservice wordt gewijzigd, u hoeft geen actie te ondernemen.

> [!NOTE]
> Sommige registratieservices domein kunnen u alleen subdomeinen toewijzen bij het gebruik van een CNAME-record, bijvoorbeeld www.contoso.com en geen hoofdmap namen, zoals contoso.com. Zie voor meer informatie over CNAME-records, de documentatie van uw registrar [de Wikipedia-vermelding op CNAME-record](http://en.wikipedia.org/wiki/CNAME_record), of de [IETF-domeinnamen - implementatie en -specificatie](http://tools.ietf.org/html/rfc1035) document.
> 
> 

### <a name="a-record"></a>Een record
Een *A* record Hiermee wijst u een domein, zoals **contoso.com** of **www.contoso.com**, *of een wildcard-domein* zoals  **\*. contoso.com**, een IP-adres. In het geval van een Azure Cloud Service, het virtuele IP-adres van de service. Zodat het belangrijkste voordeel van een A-record via een CNAME-record is dat u kunt één vermelding die gebruikmaakt van een jokerteken, zoals \* **. contoso.com**, die aanvragen voor meerdere subdomeinen zoals zou verwerken  **mail.contoso.com**, **login.contoso.com**, of **www.contso.com**.

> [!NOTE]
> Omdat een A-record is toegewezen aan een statisch IP-adres, kan niet deze wijzigingen automatisch omgezet naar het IP-adres van uw Cloudservice. Het IP-adres gebruikt door uw Cloud-Service is toegewezen de eerste keer dat u implementeert op een lege sleuf (productie of staging.) Als u de implementatie voor de site verwijdert, wordt het IP-adres is uitgegeven door Azure en alle toekomstige implementaties naar de site een nieuwe IP-adres kunnen worden gegeven.
> 
> Het IP-adres van een bepaald implementatiesleuf (productie- of faseringsomgevingen) worden eenvoudig persistent gemaakt bij het wisselen tussen fasering en productie-implementaties of het uitvoeren van een in-place upgrade van een bestaande implementatie. Zie voor meer informatie over het uitvoeren van deze acties [cloudservices beheren](cloud-services-how-to-manage-portal.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Een CNAME-record voor uw aangepaste domein toevoegen
Voor het maken van een CNAME-record, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's van uw registrar. Elke registrar heeft een methode vergelijkbaar, maar enigszins afwijken van het opgeven van een CNAME-record, maar de concepten zijn hetzelfde.

1. Gebruik een van deze methoden om te zoeken de **. cloudapp.net** domeinnaam toegewezen aan uw cloudservice.
   
   * Meld u aan bij de [Azure Portal], selecteert u uw cloudservice, kijken naar de **Essentials** sectie en gaat u naar de **Site-URL** vermelding.
     
       ![snelle weergave-sectie van de site-URL][csurl]
     
       **OR**
   * Installeer en configureer [Azure Powershell](/powershell/azure/overview), en gebruik vervolgens de volgende opdracht uit:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Sla de naam van het domein in de URL die is geretourneerd door een van beide methoden wordt gebruikt als u deze nodig heeft bij het maken van een CNAME-record.
2. Meld u aan bij uw DNS-registratieservice-website en Ga naar de pagina voor het beheren van DNS. Zoek koppelingen of gebieden van de site met het label **domeinnaam**, **DNS**, of **Serverbeheernaam**.
3. Nu kunt u selecteren of invoeren van de CNAME vinden. Mogelijk moet u het recordtype in een vervolgkeuzelijst omlaag selecteren of Ga naar een pagina met geavanceerde instellingen. U ziet er voor de woorden **CNAME**, **Alias**, of **subdomeinen**.
4. U moet ook opgeven het domein of subdomein alias voor de CNAME, zoals **www** als u wilt maken van een alias voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein wilt, kan deze worden vermeld als de '**@**' symbool in hulpprogramma's van uw registrar voor DNS.
5. Vervolgens kunt u een canonieke hostnaam, van uw toepassing moet opgeven **cloudapp.net** domein in dit geval.

Bijvoorbeeld, de volgende CNAME-record verzendt al het verkeer van **www.contoso.com** naar **contoso.cloudapp.net**, de aangepaste domeinnaam van uw geïmplementeerde toepassing:

| Alias/Host-naam/subdomein | Canonieke domein |
| --- | --- |
| www |contoso.cloudapp.net |

> [!NOTE]
> Een bezoeker van **www.contoso.com** ziet nooit de waarde true host (contoso.cloudapp.net), zodat het proces doorsturen zichtbaar voor de eindgebruiker is.
> 
> Het bovenstaande voorbeeld geldt alleen voor verkeer op de **www** subdomein. Omdat u kunt geen jokertekens met de CNAME-records gebruiken, moet u een CNAME voor elk domein/subdomein maken. Als u wilt instellen dat verkeer van subdomeinen, bijvoorbeeld *. contoso.com is, naar uw adres cloudapp.net, kunt u een **URL-omleiding** of **URL doorsturen** vermelding in de DNS-instellingen, of maak een A-record.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Een A-record voor uw aangepaste domein toevoegen
Voor het maken van een A-record, moet u eerst het virtuele IP-adres van uw cloudservice vinden. Vervolgens een nieuw item in de tabel DNS voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's van uw registrar. Elke registrar heeft een methode vergelijkbaar, maar enigszins afwijken van het opgeven van een A-record, maar de concepten zijn hetzelfde.

1. Gebruik een van de volgende methoden om het IP-adres van uw cloudservice.
   
   * Meld u aan bij de [Azure Portal], selecteert u uw cloudservice, kijken naar de **Essentials** sectie en gaat u naar de **openbare IP-adressen** vermelding.
     
       ![snelle weergave-sectie van de VIP][vip]
     
       **OR**
   * Installeer en configureer [Azure Powershell](/powershell/azure/overview), en gebruik vervolgens de volgende opdracht uit:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Sla het IP-adres als u deze nodig heeft bij het maken van een A-record.
2. Meld u aan bij uw DNS-registratieservice-website en Ga naar de pagina voor het beheren van DNS. Zoek koppelingen of gebieden van de site met het label **domeinnaam**, **DNS**, of **Serverbeheernaam**.
3. Nu kunt u selecteren of invoeren van een record vinden. Mogelijk moet u het recordtype in een vervolgkeuzelijst omlaag selecteren of Ga naar een pagina met geavanceerde instellingen.
4. Selecteer of Voer het domein of subdomein dat door deze A-record wordt gebruikt. Selecteer bijvoorbeeld **www** als u wilt maken van een alias voor **www.customdomain.com**. Als u maken van een jokerteken-vermelding voor alle subdomeinen wilt, voert u ' ***'. Deze bestrijken alle subdomeinen zoals **mail.customdomain.com**, **login.customdomain.com**, en **www.customdomain.com**.
   
    Als u maken van een A-record voor het hoofddomein wilt, kan deze worden vermeld als de '**@**' symbool in hulpprogramma's van uw registrar voor DNS.
5. Voer het IP-adres van uw cloudservice in het opgegeven veld. Hiermee wordt de domein-vermelding in de A-record met het IP-adres van uw cloud service-implementatie gebruikt.

Bijvoorbeeld, de volgende een record al het verkeer van verzendt **contoso.com** naar **137.135.70.239**, het IP-adres van uw geïmplementeerde toepassing:

| Host-naam/subdomein | IP-adres |
| --- | --- |
| \@ |137.135.70.239 |

In dit voorbeeld ziet u het maken van een A-record voor het hoofddomein. Als u een jokerteken-vermelding aan zodat alle subdomeinen maken wilt, voert u dus ' ***' als het subdomein.

> [!WARNING]
> IP-adressen in Azure zijn standaard dynamisch. Wilt u waarschijnlijk gebruik van een [gereserveerd IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) om ervoor te zorgen dat uw IP-adres niet verandert.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Cloud Services beheren](cloud-services-how-to-manage-portal.md)
* [CDN-inhoud toewijzen aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md)
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Meer informatie over het [implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure Portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png

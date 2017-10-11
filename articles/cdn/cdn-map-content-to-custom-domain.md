---
title: Azure CDN-inhoud toewijzen aan een aangepast domein | Microsoft Docs
description: Informatie over het Azure CDN-inhoud toewijzen aan een aangepast domein.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: cd6db44f7776859d1e6a893543cf0666182ca41a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="map-azure-cdn-content-to-a-custom-domain"></a>Azure CDN-inhoud aan een aangepast domein toewijzen
U kunt een aangepast domein toewijzen aan een CDN-eindpunt om te gebruiken van uw eigen domeinnaam in URL's voor de inhoud in cache in plaats van met behulp van een subdomein van azureedge.net.

Er zijn twee manieren uw aangepaste domein toewijzen aan een CDN-eindpunt:

1. [Maken van een CNAME-record bij uw domeinregistrar en uw aangepaste domein en het subdomein toewijzen aan het CDN-eindpunt](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    Een CNAME-record is een DNS-functie die een brondomein, bijvoorbeeld `www.contosocdn.com` of `cdn.contoso.com`, toewijst aan een doeldomein. In dit geval wordt het brondomein is voor uw aangepaste domeinen en subdomeinen (een subdomein, bijvoorbeeld **www** of **cdn** is altijd vereist). Het doeldomein is uw CDN-eindpunt.  
   
    Het proces van uw aangepaste domein toewijzen aan uw CDN-eindpunt kan echter leiden tot een korte periode van uitvaltijd voor het domein wanneer u bij het registreren van het domein in de Azure Portal.
2. [Toevoegen van een tussenliggende registratiestap met **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Als uw aangepaste domein wordt momenteel ondersteund door een toepassing met een service level agreement (SLA) die vereist dat er geen uitvaltijd, dan kunt u de Azure **cdnverify** subdomein waarmee een tussenliggende registratiestap zodat gebruikers zich toegang tot uw domein terwijl de DNS-toewijzing plaatsvindt.  

Nadat u uw aangepaste domein met een van de bovenstaande procedures geregistreerd, wilt u [verifiëren dat het aangepaste subdomein verwijst naar uw CDN-eindpunt](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> U moet een CNAME-record maken met uw domeinregistrar uw domein toewijzen aan het CDN-eindpunt. CNAME-records wijzen specifieke subdomeinen zoals `www.contoso.com` of `cdn.contoso.com`. Het is niet mogelijk een CNAME-record om toe te wijzen een hoofddomein zoals `contoso.com`.
> 
> Een subdomein kan alleen worden gekoppeld aan een CDN-eindpunt. De CNAME-record die u maakt, wordt al het verkeer geadresseerd aan het subdomein met het opgegeven eindpunt gerouteerd.  Bijvoorbeeld, als u koppelen `www.contoso.com` met uw CDN-eindpunt vervolgens kan niet koppelt u deze met andere Azure-eindpunten, zoals een eindpunt storage-account of een cloud service-eindpunt. U kunt echter verschillende subdomeinen uit hetzelfde domein gebruiken voor andere service-eindpunten. U kunt ook verschillende subdomeinen toewijzen aan hetzelfde CDN-eindpunt.
> 
> Voor **Azure CDN van Verizon** (standaard en Premium) eindpunten, houd er rekening mee dat het duurt maximaal voordat **90 minuten** voor het aangepaste domeinwijzigingen doorgeven aan CDN edge-knooppunten.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Een aangepast domein voor een Azure CDN-eindpunt registreren
1. Meld u aan bij de [Azure-Portal](https://portal.azure.com/).
2. Klik op **Bladeren**, klikt u vervolgens **CDN-profielen**, en vervolgens de CDN-profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein.  
3. In de **CDN-profiel** blade, klikt u op het CDN-eindpunt waarmee u wilt koppelen van het subdomein.
4. Klik boven in de blade een eindpunt op de **aangepast domein toevoegen** knop.  In de **toevoegen van een aangepast domein** blade ziet u de hostnaam eindpunt afgeleid van uw CDN-eindpunt te gebruiken bij het maken van een nieuwe CNAME-record. De indeling van het adres van de host-naam wordt weergegeven als  **&lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam moet worden gebruikt bij het maken van de CNAME-record kopiëren.  
5. Navigeer naar de website van uw domeinregistrar en ga naar het gedeelte voor het maken van DNS-records. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
6. Zoek het gedeelte voor het beheren van CNAME's. Mogelijk moet u naar een pagina met geavanceerde instellingen gaan en de woorden CNAME, Alias of Subdomeinen zoeken.
7. Maak een nieuwe CNAME-record dat is toegewezen uw gekozen subdomein (bijvoorbeeld **www** of **cdn**) aan de hostnaam die is opgegeven in de **toevoegen van een aangepast domein** blade. 
8. Ga terug naar de **toevoegen van een aangepast domein** blade en voert u uw aangepaste domein, met inbegrip van het subdomein in het dialoogvenster. Voer bijvoorbeeld de domeinnaam de indeling `www.contoso.com` of `cdn.contoso.com`.   
   
   Azure controleert of de CNAME-record bestaat voor de domeinnaam die u hebt ingevoerd. Als de CNAME juist is, wordt uw aangepaste domein gevalideerd.  Voor **Azure CDN van Verizon** (standaard en Premium) eindpunten, duurt tot 90 minuten voor instellingen van aangepast domein echter doorgegeven aan alle CDN edge-knooppunten.  
   
   Houd er rekening mee dat in sommige gevallen het tijd voor de CNAME-record duren kan doorgeven aan de naamservers op het Internet. Als uw domein niet onmiddellijk worden gevalideerd en u denkt dat de CNAME-record juist is, wacht een paar minuten en probeer het opnieuw.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Een aangepast domein voor een Azure CDN-eindpunt met behulp van het subdomein tussenliggende cdnverify registreren
1. Meld u aan bij de [Azure-Portal](https://portal.azure.com/).
2. Klik op **Bladeren**, klikt u vervolgens **CDN-profielen**, en vervolgens de CDN-profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein.  
3. In de **CDN-profiel** blade, klikt u op het CDN-eindpunt waarmee u wilt koppelen van het subdomein.
4. Klik boven in de blade een eindpunt op de **aangepast domein toevoegen** knop.  In de **toevoegen van een aangepast domein** blade ziet u de hostnaam eindpunt afgeleid van uw CDN-eindpunt te gebruiken bij het maken van een nieuwe CNAME-record. De indeling van het adres van de host-naam wordt weergegeven als  **&lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam moet worden gebruikt bij het maken van de CNAME-record kopiëren.
5. Navigeer naar de website van uw domeinregistrar en ga naar het gedeelte voor het maken van DNS-records. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
6. Zoek het gedeelte voor het beheren van CNAME's. Wellicht hebt u om te gaan naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**, of **subdomeinen**.
7. Maak een nieuwe CNAME-record en geef een alias voor een subdomein waarin de **cdnverify** subdomein. Bijvoorbeeld, het subdomein dat u opgeeft in de notatie worden **cdnverify.www** of **cdnverify.cdn**. Geef vervolgens de host-naam, uw CDN-eindpunt in de notatie **cdnverify.&lt; EndpointName >. azureedge.net**. Uw DNS-toewijzing moet eruitzien als:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Ga terug naar de **toevoegen van een aangepast domein** blade en voert u uw aangepaste domein, met inbegrip van het subdomein in het dialoogvenster. Voer bijvoorbeeld de domeinnaam de indeling `www.contoso.com` of `cdn.contoso.com`. Houd er rekening mee dat in deze stap maakt u niet hoeft begint het subdomein met **cdnverify**.  
   
    Azure controleert of de CNAME-record bestaat voor de cdnverify-domeinnaam die u hebt ingevoerd.
9. Op dit moment uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet wordt gerouteerd naar uw CDN-eindpunt. Na een wachttijd lang genoeg om toe te staan de instellingen van aangepast domein te kunnen doorvoeren naar de CDN edge-knooppunten (90 minuten **Azure CDN van Verizon**, 1-2 minuten voor **Azure CDN van Akamai**), Ga terug naar uw DNS de website van Registrar en een andere CNAME-record maken die een subdomein wordt toegewezen aan uw CDN-eindpunt. Geef bijvoorbeeld het subdomein als **www** of **cdn**, en de hostnaam als  **&lt;EndpointName >. azureedge.net**. De registratie van uw aangepaste domein is met deze stap is voltooid.
10. Ten slotte kunt u het hebt gemaakt met behulp van de CNAME-record verwijderen **cdnverify**, zoals deze alleen als een tussenliggende stap nodig was.  

## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Controleer of het aangepaste subdomein verwijst naar uw CDN-eindpunt
* Nadat u de registratie van uw aangepaste domein hebt voltooid, kunt u toegang tot inhoud die in cache is opgeslagen op uw CDN-eindpunt met behulp van het aangepaste domein.
  Eerst voor zorgen dat u openbare inhoud die in cache is opgeslagen op het eindpunt hebben. Bijvoorbeeld, als uw CDN-eindpunt gekoppeld aan een opslagaccount is, de CDN in de cache opgeslagen inhoud in een openbare blob-containers. Zorg ervoor dat de container is ingesteld dat openbare toegang en dat deze ten minste één blob bevat voor het testen van het aangepaste domein.
* Ga naar het adres van de blob die gebruikmaken van het aangepaste domein in uw browser. Bijvoorbeeld, als uw aangepaste domein is `cdn.contoso.com`, de URL naar een blob in de cache is vergelijkbaar met de volgende URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Zie ook
[Het Content Delivery Network (CDN) voor Azure inschakelen](cdn-create-new-endpoint.md)  


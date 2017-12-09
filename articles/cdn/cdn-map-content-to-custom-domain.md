---
title: Een aangepast domein toevoegen aan uw CDN-eindpunt | Microsoft Docs
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
ms.date: 10/09/2017
ms.author: mazha
ms.openlocfilehash: ec53b91b8aba4e38a8f7cb4b010d6be2a62150d5
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>Een aangepast domein toevoegen aan uw CDN-eindpunt
Nadat u een profiel maakt, doorgaans maakt ook u een of meer CDN-eindpunten (een subdomein van azureedge.net) voor het leveren van uw inhoud met behulp van HTTP en HTTPS. Standaard is dit eindpunt opgenomen in alle URL's bijvoorbeeld `http(s)://contoso.azureedge.net/photo.png`). Voor uw gemak Azure CDN biedt de mogelijkheid van het koppelen van een aangepast domein (bijvoorbeeld `www.contoso.com`) met het eindpunt. Met deze optie kunt u een aangepast domein gebruiken voor het leveren van uw inhoud in plaats van uw eindpunt. Deze optie is handig als u bijvoorbeeld wilt u uw eigen domeinnaam moet zichtbaar zijn voor uw klanten voor de huisstijl van de toepassing.

Als u nog geen een aangepast domein, moet u er eerst een aanschaffen bij een domeinprovider. Wanneer u een aangepast domein hebt ontvangen, als volgt te werk:
1. [Toegang tot de DNS-records van uw domeinprovider](#step-1-access-dns-records-by-using-your-domain-provider)
2. [De DNS CNAME-record (s) maken](#step-2-create-the-cname-dns-records)
    - Optie 1: Direct toewijzing van uw aangepaste domein met het CDN-eindpunt
    - Optie 2: Toewijzing van uw aangepaste domein met het CDN-eindpunt met behulp van de **cdnverify** subdomein 
3. [De toewijzing van de CNAME-record in Azure inschakelen](#step-3-enable-the-cname-record-mapping-in-azure)
4. [Controleer of het aangepaste subdomein verwijst naar uw CDN-eindpunt](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint)
5. [(Afhankelijke stap) De permanente aangepast domein toewijzen aan de CDN-eindpunt](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint)

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>Stap 1: Toegang DNS registreert met behulp van uw domeinprovider

Als u van Azure voor host gebruikmaakt uw [DNS-domeinen](https://docs.microsoft.com/azure/dns/dns-overview), moet u DNS van de domeinprovider naar een Azure DNS overdragen. Zie [Delegate a domain to Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns) (Een domein aan Azure DNS overdragen) voor meer informatie.

Anders, als u uw domeinprovider gebruikt voor het afhandelen van uw DNS-domein, meld u aan de website van uw domeinprovider. De pagina vinden voor het beheren van DNS-records door de documentatie van de provider advies of zoeken naar gebieden van de website met het label **domeinnaam**, **DNS**, of **naam Serverbeheer**. Vaak kunt u de pagina DNS-records vinden door de gegevens over uw account weer te geven en te zoeken naar een koppeling zoals **mijn domeinen**. Sommige providers hebben verschillende koppelingen naar verschillende soorten records toevoegen.

> [!NOTE]
> Bij bepaalde providers, zoals GoDaddy, worden wijzigingen in DNS-records pas van kracht wanneer u op een afzonderlijke link **Save Changes** klikt. 


## <a name="step-2-create-the-cname-dns-records"></a>Stap 2: De bijbehorende records van DNS CNAME maken

Voordat u een aangepast domein met een Azure CDN-eindpunt gebruiken kunt, moet u eerst een canonieke naam (CNAME)-record maken met uw domeinprovider. Een CNAME-record is een type van de record in de naam System DNS (Domain) die een brondomein wordt toegewezen aan een domein bestemming door te geven van een alias-domeinnaam voor de 'canonieke' of true domeinnaam. Het brondomein is uw aangepaste domein (en het subdomein) en het doeldomein van uw CDN-eindpunt is voor Azure CDN. Azure CDN controleert of de DNS CNAME-record wanneer u het aangepaste domein aan het eindpunt van de portal of de API toevoegt. 

Een specifieke domeinen en subdomeinen, zoals een CNAME-record toegewezen `www.contoso.com` of `cdn.contoso.com`; het is niet mogelijk een CNAME-record om toe te wijzen een hoofddomein zoals `contoso.com`. Een subdomein kan worden gekoppeld aan slechts één CDN-eindpunt. Een CNAME-record van alle verkeer geadresseerd aan het subdomein met het opgegeven eindpunt routes. Bijvoorbeeld, als u koppelen `www.contoso.com` met uw CDN-eindpunt kan niet koppelt u deze aan een andere Azure-eindpunt, zoals een eindpunt storage-account of een cloud service-eindpunt. U kunt echter verschillende subdomeinen uit hetzelfde domein gebruiken voor andere service-eindpunten. U kunt ook verschillende subdomeinen toewijzen aan hetzelfde CDN-eindpunt.

Gebruik een van de volgende opties om uw aangepaste domein toewijzen aan een CDN-eindpunt:

- Optie 1: Direct toewijzing van uw aangepaste domein met het CDN-eindpunt. Als er geen verkeer productie wordt uitgevoerd op het aangepaste domein, kunt u rechtstreeks een aangepast domein toewijzen aan een CDN-eindpunt. Het proces van uw aangepaste domein toewijzen aan uw CDN-eindpunt kan leiden tot een korte periode van uitvaltijd voor het domein, terwijl u bij het registreren van het domein in de Azure-portal. De vermelding voor de CNAME-toewijzing worden in deze indeling: 
 
  | NAAM             | TYPE  | WAARDE                  |
  |------------------|-------|------------------------|
  | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |


- Optie 2: Toewijzing van uw aangepaste domein met het CDN-eindpunt met behulp van de **cdnverify** subdomein. Als de productie-verkeer kan niet worden onderbroken op het aangepaste domein wordt uitgevoerd, kunt u een tijdelijke CNAME-toewijzing maken naar uw CDN-eindpunt. Met deze optie gebruikt u de Azure **cdnverify** subdomein te geven van een tussenliggende registratiestap zodat gebruikers toegang uw domein zonder onderbreking tijdens de DNS-toewijzing tot plaatsvindt.

   1. Maak een nieuwe CNAME-record en geef een alias voor een subdomein waarin de **cdnverify** subdomein. Bijvoorbeeld: `cdnverify.www` of `cdnverify.cdn`. 
   2. Geef de hostnaam, uw CDN-eindpunt in de volgende indeling is: `cdnverify.<EndpointName>.azureedge.net`. De vermelding voor de CNAME-toewijzing worden in deze indeling: 

   | NAAM                       | TYPE  | WAARDE                            |
   |----------------------------|-------|----------------------------------|
   | `cdnverify.www.contoso.com` | `CNAME` | `cdnverify.contoso.azureedge.net` | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>Stap 3: De toewijzing van de CNAME-record in Azure inschakelen

Nadat u uw aangepaste domein met een van de voorgaande procedures geregistreerd, kunt u vervolgens de functie voor het aangepaste domein in Azure CDN inschakelen. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en blader naar de CDN-profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein.  
2. In de **CDN-profiel** blade, selecteer het CDN-eindpunt waarmee u wilt koppelen van het subdomein.
3. Klik in de linkerbovenhoek van de blade een eindpunt op **aangepaste domeinen**. 

   ![Knop voor het aangepaste domein](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. In de **aangepaste hostnaam** tekst Voer uw aangepaste domein, met inbegrip van het subdomein. Bijvoorbeeld: `www.contoso.com` of `cdn.contoso.com`.

   ![Een aangepast domein-dialoogvenster toevoegen](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. Klik op **Add**.

   Azure controleert of de CNAME-record voor de domeinnaam die u hebt ingevoerd bestaat. Als de CNAME juist is, wordt uw aangepaste domein gevalideerd. Het kan even duren voor de CNAME-record doorgeven aan de naamservers. Als uw domein is niet onmiddellijk gevalideerd, controleert u of de CNAME-record klopt, wacht een paar minuten en probeer het opnieuw. Voor **Azure CDN van Verizon** eindpunten (standaard en Premium), het kan tot 90 minuten duren voordat voor het aangepaste domeininstellingen doorgegeven aan alle CDN edge-knooppunten.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Stap 4: Controleren of de aangepaste subdomein verwijst naar uw CDN-eindpunt

Nadat u de registratie van uw aangepaste domein hebt voltooid, controleert u of dat de aangepaste subdomein verwijst naar uw CDN-eindpunt.
 
1. Zorg ervoor dat u openbare inhoud die in cache is opgeslagen op het eindpunt. Bijvoorbeeld, als uw CDN-eindpunt gekoppeld aan een opslagaccount is, de CDN in de cache opgeslagen inhoud in een openbare blob-containers. Controleer of de container is ingesteld dat openbare toegang en ten minste één blob bevat voor het testen van het aangepaste domein.

2. Ga naar het adres van de blob met behulp van het aangepaste domein in uw browser. Bijvoorbeeld, als uw aangepaste domein is `cdn.contoso.com`, moet de URL naar de blob in de cache worden vergelijkbaar met de volgende URL: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Stap 5 (afhankelijke stap): de permanente aangepast domein toewijzen aan de CDN-eindpunt

Deze stap is afhankelijk van stap 2, optie 2: toewijzing van uw aangepaste domein met het CDN-eindpunt met behulp van de **cdnverify** subdomein. Als u van het tijdelijke gebruikmaakt **cdnverify** subdomein en hebt gecontroleerd of het werkt, kunt u uw aangepaste domein voor permanente vervolgens toewijzen aan het CDN-eindpunt.

1. Op de website van de domeinprovider van uw, een DNS CNAME-record uw aangepaste domein die permanent worden toegewezen aan het CDN-eindpunt te maken. De vermelding voor de CNAME-toewijzing worden in deze indeling: 
 
   | NAAM             | TYPE  | WAARDE                  |
   |------------------|-------|------------------------|
   | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |
2. Verwijder de CNAME-record met de **cdnverify** subdomein dat u eerder hebt gemaakt.

## <a name="see-also"></a>Zie ook
[Het Content Delivery Network (CDN) voor Azure inschakelen](cdn-create-new-endpoint.md)  
[Uw domein aan Azure DNS overdragen](../dns/dns-domain-delegation.md)

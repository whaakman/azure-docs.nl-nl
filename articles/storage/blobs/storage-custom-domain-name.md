---
title: Een aangepaste domein naam configureren voor uw Azure-opslag account | Microsoft Docs
description: Gebruik de Azure Portal om uw eigen canonieke naam (CNAME) toe te wijzen aan de Blob Storage-of Web-eind punt in een Azure-opslag account.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: normesta
ms.reviewer: dineshm
ms.subservice: blobs
ms.openlocfilehash: 2359befc05bff867a8f8b17943ed67d906ff4971
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534337"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Een aangepaste domein naam configureren voor uw Azure-opslag account

U kunt een aangepast domein configureren voor toegang tot BLOB-gegevens in uw Azure-opslag account. Het standaard eindpunt voor Azure Blob Storage is  *\<Storage-account-name >. blob. core. Windows. net*. U kunt ook het web-eind punt gebruiken dat wordt gegenereerd als onderdeel van de [functie statische websites](storage-blob-static-website.md). Als u een aangepast domein en subdomein, zoals *www\.-contoso.com*, aan het BLOB-of Web-eind punt voor uw opslag account toewijst, kunnen uw gebruikers dat domein gebruiken om toegang te krijgen tot BLOB-gegevens in uw opslag account.

> [!IMPORTANT]
> Azure Storage heeft geen systeem eigen ondersteuning voor HTTPS met aangepaste domeinen. U kunt [Azure CDN momenteel gebruiken om toegang te krijgen tot blobs met aangepaste domeinen via https](storage-https-custom-domain-cdn.md).
> 
> 
> [!NOTE]
> Opslag accounts ondersteunen momenteel slechts één aangepaste domein naam per account. U kunt geen aangepaste domein naam toewijzen aan zowel de web-als blob service-eind punten.
> 
> [!NOTE]
> De toewijzing werkt alleen voor subdomeinen (bijvoorbeeld www\.-contoso.com). Als u uw web-eind punt beschikbaar wilt stellen voor het hoofd domein (bijvoorbeeld contoso.com), moet u [een aangepast domein toevoegen aan uw Azure CDN-eind punt](https://docs.microsoft.com/azure/cdn/cdn-map-content-to-custom-domain).

De volgende tabel bevat enkele voor beelden van Url's voor BLOB-gegevens die zich bevinden in een opslag account met de naam *mystorageaccount*. Het aangepaste subdomein dat is geregistreerd voor het opslag account is *www\.-contoso.com*:

| Resourcetype | Standaard-URL | URL voor aangepast domein |
| --- | --- | --- |
| Storage-account | http:\//mystorageaccount.blob.core.windows.net | http:\//www.contoso.com |
| Blob |http:\//mystorageaccount.blob.core.windows.net/mycontainer/myblob | http:\//www.contoso.com/mycontainer/myblob |
| Basis container | http:\//mystorageaccount.blob.core.Windows.net/myblob of http:\//mystorageaccount.blob.core.Windows.net/$root/myblob | http:\//www.contoso.com/myblob of http:\//www.contoso.com/$root/myblob |
| Web |  http:\//mystorageaccount. [ zone]. Web. core. Windows. net/$Web/[indexdoc] of http:\//mystorageaccount. [ zone]. Web. core. Windows. net/[indexdoc] of http:\//mystorageaccount. [ zone]. Web. core. Windows. net/$Web of http:\//mystorageaccount. [ zone]. Web. core. Windows. net/ | http:\//www.contoso.com/$Web of http:\//www.contoso.com/of http:\//www.contoso.com/$Web/[indexdoc] of http:\//www.contoso.com/[indexdoc] |

> [!NOTE]  
> Zoals in de volgende secties wordt weer gegeven, zijn alle voor beelden voor het eind punt van de BLOB service ook van toepassing op het eind punt van de webservice.

## <a name="direct-vs-intermediary-cname-mapping"></a>Directe versus tussenliggende CNAME-toewijzing

U kunt uw aangepaste domein met een subdomein (bijvoorbeeld www\.contoso.com) naar het BLOB-eind punt voor uw opslag account op twee manieren laten wijzen: 
* Gebruik direct CNAME-toewijzing.
* Gebruik het *asverify* -subdomein.

### <a name="direct-cname-mapping"></a>Directe CNAME-toewijzing

De eerste en eenvoudigste methode is het maken van een canonieke-naam record (CNAME) die uw aangepaste domein en subdomein rechtstreeks aan het BLOB-eind punt toewijst. Een CNAME-record is een DNS-functie (Domain Name System) die een bron domein toewijst aan een doel domein. In ons voor beeld is het bron domein uw eigen aangepaste domein en subdomein (*bijvoorbeeld\.www-contoso.com*). Het doel domein is uw BLOB service-eind punt (bijvoorbeeld*mystorageaccount.blob.core.Windows.net*).

De directe methode wordt in de sectie ' een aangepast domein registreren ' besproken.

### <a name="intermediary-mapping-with-asverify"></a>Tussenliggende toewijzing met *asverify*

De tweede methode gebruikt ook CNAME-records. Om uitval tijd te voor komen, maakt het echter eerst gebruik van een speciale subdomein *asverify* die wordt herkend door Azure.

Het toewijzen van uw aangepaste domein aan een BLOB-eind punt kan een korte periode van uitval tijd veroorzaken tijdens het registreren van het domein in de [Azure Portal](https://portal.azure.com). Als het domein momenteel een toepassing ondersteunt met een Service Level Agreement (SLA) die geen downtime vereist, gebruikt u het Azure *asverify* -subdomein als een tussenliggende registratie stap. Deze stap zorgt ervoor dat gebruikers toegang hebben tot uw domein terwijl de DNS-toewijzing plaatsvindt.

De intermediaire methode wordt behandeld in een aangepast domein registreren met behulp van het subdomein *asverify* .

## <a name="register-a-custom-domain"></a>Een aangepast domein registreren
Registreer het domein met behulp van de procedure in deze sectie als de volgende instructies van toepassing zijn:
* U ondervindt zich niet dat het domein kort niet beschikbaar is voor uw gebruikers.
* Uw aangepaste domein is momenteel niet bezig met het hosten van een toepassing. 

U kunt Azure DNS gebruiken om een aangepaste DNS-naam te configureren voor uw Azure Blob Store. Zie [Use Azure DNS to provide custom domain settings for an Azure service](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

Als uw aangepaste domein momenteel een toepassing ondersteunt die geen downtime kan hebben, gebruikt u de procedure in een aangepast domein registreren met behulp van het subdomein *asverify* .

Als u een aangepaste domein naam wilt configureren, maakt u een nieuwe CNAME-record in DNS. In de CNAME-record wordt een alias voor een domein naam opgegeven. In ons voor beeld wordt het adres van uw aangepaste domein toegewezen aan het eind punt van de Blob-opslag van uw opslag account.

U kunt de DNS-instellingen van uw domein doorgaans beheren op de website van uw domein registratie service. Elke registratie service heeft een vergelijk bare, maar iets andere methode om een CNAME-record op te geven, maar het concept is hetzelfde. Omdat sommige basis pakketten voor domein registratie geen DNS-configuratie bieden, moet u mogelijk uw domein registratie pakket upgraden voordat u de CNAME-record kunt maken.

1. Ga in het [Azure Portal](https://portal.azure.com)naar uw opslag account.

1. Selecteer in het menu venster onder **BLOB-service**de optie **aangepast domein**.  
   Het deel venster **aangepast domein** wordt geopend.

1. Meld u aan bij de website van uw domein registratie service en ga vervolgens naar de pagina voor het beheren van DNS.  
   Mogelijk vindt u de pagina in een sectie met de naam **domein naam**, **DNS**of **naam server beheer**.

1. Zoek het gedeelte voor het beheren van CNAME's.  
   Mogelijk moet u naar een pagina met geavanceerde instellingen gaan en zoeken naar **CNAME**, **alias**of subdomeinen.

1. Een nieuwe CNAME-record maken, een subdomein alias opgeven, zoals **www** of **Foto's** (subdomein is vereist, hoofd domeinen worden niet ondersteund) en geef vervolgens een hostnaam op.  
   De hostnaam is het eind punt van de BLOB-service. De indeling is  *\<mystorageaccount >. blob. core. Windows. net*, waarbij *mystorageaccount* de naam is van uw opslag account. De te gebruiken hostnaam wordt weer gegeven in item #1 van het deel venster **aangepast domein** in de [Azure Portal](https://portal.azure.com). 

1. Voer in het deel venster **aangepast domein** in het tekstvak de naam van uw aangepaste domein in, inclusief het subdomein.  
   Als uw domein bijvoorbeeld *contoso.com* is en uw subdomein alias is *www*, voert u **www\.-contoso.com**in. Als uw subdomein *Foto's*is, voert u **Photos.contoso.com**in.

1. Als u uw aangepaste domein wilt registreren, selecteert u **Opslaan**.  
   Als de registratie is geslaagd, ontvangt de portal een melding dat uw opslag account is bijgewerkt.

Nadat uw nieuwe CNAME-record is door gegeven via DNS, kunnen ze BLOB-gegevens weer geven met behulp van uw aangepaste domein als uw gebruikers over de juiste machtigingen beschikken.

## <a name="register-a-custom-domain-by-using-the-asverify-subdomain"></a>Een aangepast domein registreren met behulp van het subdomein *asverify*
Als uw aangepaste domein momenteel ondersteuning biedt voor een toepassing met een SLA waarvoor geen downtime nodig is, registreert u uw aangepaste domein met behulp van de procedure in deze sectie. Door een CNAME te maken die verwijst van *asverify\< . > van subdomein. customdomain\<>* naar *asverify.\< Storage account >. blob. core. Windows. net*, u kunt uw domein vooraf registreren bij Azure. U kunt vervolgens een tweede CNAME maken die verwijst van  *\<> van subdomein\< . customdomain >* naar  *\<Storage account >. blob. core. Windows. net*en vervolgens wordt het verkeer naar uw aangepaste domein omgeleid naar uw BLOB-eind punt.

Het subdomein *asverify* is een speciaal subdomein dat wordt herkend door Azure. Door *asverify* in te stellen op uw eigen subdomein, staat u toe dat Azure uw aangepaste domein herkent zonder dat de DNS-record voor het domein hoeft te worden gewijzigd. Wanneer u de DNS-record voor het domein wijzigt, wordt deze toegewezen aan het BLOB-eind punt zonder downtime.

1. Ga in het [Azure Portal](https://portal.azure.com)naar uw opslag account.

1. Selecteer in het menu venster onder **BLOB-service**de optie **aangepast domein**.  
   Het deel venster **aangepast domein** wordt geopend.

1. Meld u aan bij de website van uw DNS-provider en ga vervolgens naar de pagina voor het beheren van DNS.  
   Mogelijk vindt u de pagina in een sectie met de naam **domein naam**, **DNS**of **naam server beheer**.

1. Zoek het gedeelte voor het beheren van CNAME's.  
   Mogelijk moet u naar een pagina met geavanceerde instellingen gaan en zoeken naar **CNAME**, **alias**of subdomeinen.

1. Maak een nieuwe CNAME-record, geef een subdomein alias op die het subdomein *asverify* bevat, zoals **asverify. www** of **asverify. Foto's**, en geef vervolgens een hostnaam op.  
   De hostnaam is het eind punt van de BLOB-service. De indeling is *asverify.\< mystorageaccount >. blob. core. Windows. net*, waarbij *mystorageaccount* de naam is van uw opslag account. De te gebruiken hostnaam wordt weer gegeven in item #2 van het deel venster *aangepast domein* in de [Azure Portal](https://portal.azure.com).

1. Voer in het deel venster **aangepast domein** in het tekstvak de naam van uw aangepaste domein in, inclusief het subdomein.  
   Neem *asverify*niet op. Als uw domein bijvoorbeeld *contoso.com* is en uw subdomein alias is *www*, voert u **www\.-contoso.com**in. Als uw subdomein *Foto's*is, voert u **Photos.contoso.com**in.

1. Schakel het selectie vakje **indirecte CNAME-validatie gebruiken** in.

1. Als u uw aangepaste domein wilt registreren, selecteert u **Opslaan**.  
   Als de registratie is geslaagd, ontvangt de portal een melding dat uw opslag account is bijgewerkt. Uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein wordt nog niet doorgestuurd naar uw opslag account.

1. Ga terug naar de website van uw DNS-provider en maak een andere CNAME-record die uw subdomein toewijst aan het eind punt van de BLOB-service.  
   Geef bijvoorbeeld het subdomein op als *www* of *Foto's* (zonder *asverify*) en geef de hostnaam op als  *\<mystorageaccount >. blob. core. Windows. net*, waarbij *mystorageaccount* de naam van uw opslag account. Met deze stap is de registratie van uw aangepaste domein voltooid.

1. Ten slotte kunt u de zojuist gemaakte CNAME-record die het *asverify* -subdomein bevat, verwijderen. Dit is alleen vereist als een intermediaire stap.

Nadat uw nieuwe CNAME-record is door gegeven via DNS, kunnen ze BLOB-gegevens weer geven met behulp van uw aangepaste domein als uw gebruikers over de juiste machtigingen beschikken.

## <a name="test-your-custom-domain"></a>Uw aangepaste domein testen

Om te bevestigen dat uw aangepaste domein is toegewezen aan het eind punt van de BLOB-service, maakt u een BLOB in een open bare container in uw opslag account. Ga in een webbrowser naar de blob met behulp van een URI in de volgende indeling:`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Als u bijvoorbeeld toegang wilt krijgen tot een webformulier in de *myForms* -container in het aangepaste subdomein *Photos.contoso.com* , kunt u de volgende URI gebruiken:`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Registratie van een aangepast domein ongedaan maken

Gebruik een van de volgende procedures om de registratie van een aangepast domein voor uw Blob Storage-eind punt op te heffen.

### <a name="azure-portal"></a>Azure Portal

Ga als volgt te werk om de aangepaste domein instelling te verwijderen:

1. Ga in het [Azure Portal](https://portal.azure.com)naar uw opslag account.

1. Selecteer in het menu venster onder **BLOB-service**de optie **aangepast domein**.  
   Het deel venster **aangepast domein** wordt geopend.

1. Wis de inhoud van het tekstvak met de aangepaste domein naam.

1. Selecteer de knop **Opslaan**.

Nadat het aangepaste domein is verwijderd, ziet u een portal melding dat uw opslag account is bijgewerkt.

### <a name="azure-cli"></a>Azure-CLI

Als u een aangepaste domein registratie wilt verwijderen, gebruikt u de opdracht [AZ Storage account update](https://docs.microsoft.com/cli/azure/storage/account) CLI en geeft u een lege`""`teken reeks ( `--custom-domain` ) op voor de argument waarde.

* Opdracht indeling:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Opdracht voorbeeld:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u een aangepaste domein registratie wilt verwijderen, gebruikt u de Power shell [-cmdlet Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) en geeft u`""`een lege teken `-CustomDomainName` reeks () op voor de argument waarde.

* Opdracht indeling:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Opdracht voorbeeld:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Volgende stappen
* [Een aangepast domein toewijzen aan een Azure Content Delivery Network (CDN)-eind punt](../../cdn/cdn-map-content-to-custom-domain.md)
* [Azure CDN gebruiken om toegang te krijgen tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Statische website-hosting in Azure Blob-opslag (preview-versie)](storage-blob-static-website.md)

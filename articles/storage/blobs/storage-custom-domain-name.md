---
title: Een aangepaste domeinnaam voor uw Azure storage-account configureren | Microsoft Docs
description: De Azure portal gebruiken voor uw eigen canonieke naam (CNAME) toewijzen aan het eindpunt van Blob storage- of website in Azure storage-account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: e40b6fe115d6b6dea38ead9f0b2550d96bd04c7a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112633"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Een aangepaste domeinnaam voor uw Azure storage-account configureren

U kunt een aangepast domein voor toegang tot blobgegevens in uw Azure storage-account configureren. Het standaardeindpunt voor Azure Blob-opslag is  *\<storage-account-name >. blob.core.windows.net*. U kunt ook het eindpunt op het web die wordt gegenereerd als onderdeel van de [statische websites-functie (preview)](storage-blob-static-website.md). Als u een aangepast domein en subdomein, zoals toewijst *www\.contoso.com*, naar het blob of web-eindpunt voor uw opslagaccount, uw gebruikers dat domein toegang tot blob-gegevens in uw storage-account kunnen gebruiken.

> [!IMPORTANT]
> Azure Storage biedt niet nog systeemeigen ondersteuning voor HTTPS met aangepaste domeinen. U kunt op dit moment [Azure CDN gebruiken voor toegang tot blobs met behulp van aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md).
> 
> 
> [!NOTE]
> Storage-accounts ondersteunen momenteel slechts één aangepaste domeinnaam per account. U kunt een aangepaste domeinnaam kan niet toewijzen aan de web- en blob service-eindpunten.
> 
> [!NOTE]
> De toewijzing kan alleen worden gebruikt voor subdomeinen (bijvoorbeeld www\.contoso.com). Als u wilt uw eindpunt op het web beschikbaar hebt voor het hoofddomein (bijvoorbeeld contoso.com), wordt u [Azure CDN gebruiken met aangepaste domeinen](storage-https-custom-domain-cdn.md)

De volgende tabel ziet u enkele voorbeeld-URL's voor blob-gegevens die zich in een opslagaccount met de naam *mystorageaccount*. Het aangepaste subdomein dat geregistreerd voor het opslagaccount is *www\.contoso.com*:

| Resourcetype | Standaard-URL | Aangepaste domein-URL |
| --- | --- | --- |
| Storage-account | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Root-container | http://mystorageaccount.blob.core.windows.net/myblob of http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob of http://www.contoso.com/$root/myblob |
| Web |  http://mystorageaccount.[zone].web.core.windows.net/$web/[indexdoc] or http://mystorageaccount.[zone].web.core.windows.net/[indexdoc] or http://mystorageaccount.[zone].web.core.windows.net/$web or http://mystorageaccount.[zone].web.core.windows.net/ | http://www.contoso.com/$web or http://www.contoso.com/ or http://www.contoso.com/$web/[indexdoc] or  http://www.contoso.com/[indexdoc] |

> [!NOTE]  
> Zoals u in de volgende secties, alle voorbeelden voor het eindpunt van blob service ook van toepassing op het web service-eindpunt.

## <a name="direct-vs-intermediary-cname-mapping"></a>Directe versus tussenliggende CNAME-toewijzing

U kunt uw aangepaste domein voorafgegaan door een subdomein verwijzen (bijvoorbeeld www\.contoso.com) aan het blobeindpunt voor uw opslagaccount op twee manieren: 
* Gebruik direct CNAME-toewijzing.
* Gebruik de *asverify* tussenliggende subdomein.

### <a name="direct-cname-mapping"></a>Directe CNAME-toewijzing

De methode voor eerste en eenvoudigste, is het maken van een canonieke naam (CNAME)-record die uw aangepaste domein en subdomein dat rechtstreeks naar het blobeindpunt wordt toegewezen. Een CNAME-record is een domain name system (DNS)-functie die een brondomein wordt toegewezen aan een doeldomein. In ons voorbeeld is het brondomein uw eigen aangepaste domein en subdomein (*www\.contoso.com*, bijvoorbeeld). Het doeldomein is het eindpunt van de blob-service (*mystorageaccount.blob.core.windows.net*, bijvoorbeeld).

De directe methode die wordt beschreven in de sectie 'Een aangepast domein registreren'.

### <a name="intermediary-mapping-with-asverify"></a>Tussenliggende toewijzing met *asverify*

De tweede methode maakt ook gebruik van CNAME-records. Om te voorkomen uitvaltijd, maar eerst service maakt gebruik van een speciale subdomein *asverify* die wordt herkend door Azure.

Uw aangepaste domein toewijzen aan een blobeindpunt kan leiden tot een korte periode van uitvaltijd terwijl u bezig zijn met het registreren van het domein in de [Azure-portal](https://portal.azure.com). Als een toepassing met een service level agreement (SLA) waarvoor geen downtime op dit moment biedt ondersteuning voor het domein, gebruikt u de Azure *asverify* subdomein als een tussenliggende registratiestap. Deze stap zorgt ervoor dat gebruikers toegang uw domein tot hebben terwijl de DNS-toewijzing doorgevoerd wordt.

De tussenliggende methode valt registreren in een aangepast domein met behulp van de *asverify* subdomein.

## <a name="register-a-custom-domain"></a>Registreren van een aangepast domein
Het domein registreren met behulp van de procedure in deze sectie als de volgende instructies van toepassing zijn:
* U bent unconcerned dat het domein kort niet beschikbaar voor uw gebruikers is.
* Uw aangepaste domein wordt een toepassing niet gehost. 

Azure DNS kunt u een aangepaste DNS-naam voor uw Azure Blob-archief configureren. Zie [Use Azure DNS to provide custom domain settings for an Azure service](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

Als uw aangepaste domein momenteel een toepassing die geen uitvaltijd ondersteunt, gebruikt u de procedure in het Register een aangepast domein met behulp van de *asverify* subdomein.

Als u wilt een aangepaste domeinnaam configureren, maakt u een nieuwe CNAME-record in DNS. De CNAME-record bevat een alias voor de naam van een domein. In ons voorbeeld het het adres van uw aangepaste domein wordt toegewezen aan het eindpunt van uw storage-account Blob-opslag.

Meestal kunt u beheren van uw domein-DNS-instellingen op de website van uw domeinregistrar. Elke registrar heeft een methode vergelijkbaar, maar enigszins afwijken van het opgeven van een CNAME-record, maar het concept is hetzelfde. Omdat sommige pakketten van de registratie van basic domein geen DNS-configuratie biedt, moet u mogelijk uw domein registratiepakket bijwerken voordat u kunt de CNAME-record maken.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.

1. In het menu onder **Blob-Service**, selecteer **aangepast domein**.  
   De **aangepast domein** deelvenster wordt geopend.

1. Aanmelden bij de website van uw domeinregistrar en ga vervolgens naar de pagina voor het beheren van DNS.  
   U kunt de pagina vinden in een sectie met de naam **domeinnaam**, **DNS**, of **Serverbeheernaam**.

1. Zoek het gedeelte voor het beheren van CNAME's.  
   Mogelijk hebt u om te gaan naar een pagina met geavanceerde instellingen en zoek naar **CNAME**, **Alias**, of **subdomeinen**.

1. Maak een nieuwe CNAME-record, voer een subdomein alias zoals **www** of **foto's** (subdomein is vereist, hoofddomeinen worden niet ondersteund), en geef vervolgens de naam van een host.  
   De hostnaam is het eindpunt van de blob-service. De indeling  *\<mystorageaccount >. blob.core.windows.net*, waarbij *mystorageaccount* is de naam van uw storage-account. De naam van de host te gebruiken wordt weergegeven in item #1 van de **aangepast domein** deelvenster in de [Azure-portal](https://portal.azure.com). 

1. In de **aangepast domein** deelvenster in het tekstvak, voer de naam van uw aangepaste domein, met inbegrip van het subdomein.  
   Bijvoorbeeld, als uw domein is *contoso.com* en je alias subdomein is *www*, voer **www\.contoso.com**. Als uw subdomein *foto's*, voer **photos.contoso.com**.

1. Selecteer voor het registreren van uw aangepaste domein, **opslaan**.  
   Als de registratie geslaagd is, de portal een melding dat uw storage-account is bijgewerkt.

Nadat de nieuwe CNAME-record is doorgegeven via DNS, als uw gebruikers de juiste machtigingen hebben, kunnen ze de blob-gegevens bekijken met behulp van uw aangepaste domein.

## <a name="register-a-custom-domain-by-using-the-asverify-subdomain"></a>Een aangepast domein registreren met behulp van de *asverify* subdomein
Als uw aangepaste domein momenteel een toepassing met een SLA die vereist dat er geen service niet beschikbaar is ondersteunt, moet u uw aangepaste domein registreren met behulp van de procedure in deze sectie. Door het maken van een CNAME die van *asverify.\< subdomein >. \<customdomain >* naar *asverify.\< storageaccount >. blob.core.windows.net*, kunt u uw domein met Azure vooraf registreren. U kunt vervolgens een tweede CNAME die van maken  *\<subdomein >.\< customdomain >* naar  *\<storageaccount >. blob.core.windows.net*, en vervolgens verkeer naar uw aangepaste domein wordt omgeleid naar uw blobeindpunt.

De *asverify* subdomein is een speciale subdomein wordt herkend door Azure. Door het begin *asverify* aan uw eigen subdomein, kan u Azure voor het herkennen van uw aangepaste domein zonder te hoeven wijzigen van de DNS-record voor het domein. Wanneer u mag de DNS-record voor het domein worden gewijzigd, wordt deze toegewezen aan het blobeindpunt zonder uitvaltijd.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.

1. In het menu onder **Blob-Service**, selecteer **aangepast domein**.  
   De **aangepast domein** deelvenster wordt geopend.

1. Aanmelden bij de website van uw DNS-provider en gaat u naar de pagina voor het beheren van DNS.  
   U kunt de pagina vinden in een sectie met de naam **domeinnaam**, **DNS**, of **Serverbeheernaam**.

1. Zoek het gedeelte voor het beheren van CNAME's.  
   Mogelijk hebt u om te gaan naar een pagina met geavanceerde instellingen en zoek naar **CNAME**, **Alias**, of **subdomeinen**.

1. Maak een nieuwe CNAME-record, bieden de alias van een subdomein met de *asverify* subdomein, zoals **asverify.www** of **asverify.photos**, en geef vervolgens de naam van een host.  
   De hostnaam is het eindpunt van de blob-service. De indeling *asverify.\< mystorageaccount >. blob.core.windows.net*, waarbij *mystorageaccount* is de naam van uw storage-account. De naam van de host te gebruiken wordt weergegeven in item #2 van de *aangepast domein* deelvenster in de [Azure-portal](https://portal.azure.com).

1. In de **aangepast domein** deelvenster in het tekstvak, voer de naam van uw aangepaste domein, met inbegrip van het subdomein.  
   Maak daarbij dan geen *asverify*. Bijvoorbeeld, als uw domein is *contoso.com* en je alias subdomein is *www*, voer **www\.contoso.com**. Als uw subdomein *foto's*, voer **photos.contoso.com**.

1. Selecteer de **indirecte CNAME-validatie gebruiken** selectievakje.

1. Selecteer voor het registreren van uw aangepaste domein, **opslaan**.  
   Als de registratie geslaagd is, de portal een melding dat uw storage-account is bijgewerkt. Uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet wordt doorgestuurd naar uw storage-account.

1. Ga terug naar de website van uw DNS-provider en maak vervolgens een andere CNAME-record die uw subdomein wordt toegewezen aan het eindpunt van de blob-service.  
   Bijvoorbeeld, geef het subdomein als *www* of *foto's* (zonder de *asverify*) en geef de hostnaam van de als  *\<mystorageaccount >. blob.core.windows.net*, waarbij *mystorageaccount* is de naam van uw storage-account. De registratie van uw aangepaste domein is met deze stap is voltooid.

1. Ten slotte kunt u de zojuist gemaakte CNAME-record met verwijderen de *asverify* subdomein, die alleen als een tussenliggend stap vereist is.

Nadat de nieuwe CNAME-record is doorgegeven via DNS, als uw gebruikers de juiste machtigingen hebben, kunnen ze de blob-gegevens bekijken met behulp van uw aangepaste domein.

## <a name="test-your-custom-domain"></a>Uw aangepaste domein testen

Om te bevestigen dat uw aangepaste domein is toegewezen aan uw eindpunt van blob service, een blob in een openbare container binnen uw opslagaccount te maken. Klik vervolgens in een webbrowser toegang krijgen tot de blob met behulp van een URI in de volgende indeling: `http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Bijvoorbeeld, voor toegang tot een webformulier in het *myforms* -container in de *photos.contoso.com* aangepaste subdomein, kunt u de volgende URI: `http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Registratie ongedaan maken van een aangepast domein

Als u wilt een aangepast domein voor het eindpunt van Blob-opslag voor de registratie ongedaan maken, gebruikt u een van de volgende procedures.

### <a name="azure-portal"></a>Azure Portal

Als u wilt verwijderen van de instelling van het aangepaste domein, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.

1. In het menu onder **Blob-Service**, selecteer **aangepast domein**.  
   De **aangepast domein** deelvenster wordt geopend.

1. Hiermee schakelt u de inhoud van het tekstvak met de naam van uw aangepaste domein.

1. Selecteer de knop **Opslaan**.

Nadat u het aangepaste domein hebt verwijderd, ziet u een portalmelding dat uw storage-account is bijgewerkt.

### <a name="azure-cli"></a>Azure-CLI

U kunt de registratie van een aangepast domein verwijderen met de [az storage account update](https://docs.microsoft.com/cli/azure/storage/account) CLI opdracht in en geef vervolgens een lege tekenreeks (`""`) voor de `--custom-domain` argumentwaarde.

* De opdrachtindeling van de:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Opdrachtvoorbeeld van de:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt de registratie van een aangepast domein verwijderen met de [Set AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) PowerShell-cmdlet en geef vervolgens een lege tekenreeks (`""`) voor de `-CustomDomainName` argumentwaarde.

* De opdrachtindeling van de:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Opdrachtvoorbeeld van de:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Volgende stappen
* [Een aangepast domein toewijzen aan een eindpunt Azure Content Delivery Network (CDN)](../../cdn/cdn-map-content-to-custom-domain.md)
* [Azure CDN gebruiken voor toegang tot blobs met behulp van aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Statische website hosting in Azure Blob-opslag (preview)](storage-blob-static-website.md)

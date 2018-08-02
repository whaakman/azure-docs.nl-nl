---
title: Een aangepaste domeinnaam voor uw Azure Storage-account configureren | Microsoft Docs
description: De Azure portal gebruiken voor uw eigen canonieke naam (CNAME) toewijzen aan de Blob of web-eindpunt in een Azure Storage-account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: tamram
ms.component: blobs
ms.openlocfilehash: 5fd823e9105157f8292d5a9554850b0f4338a392
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398849"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Een aangepaste domeinnaam voor uw Azure Storage-account configureren

U kunt een aangepast domein voor toegang tot blobgegevens in uw Azure storage-account configureren. Het standaardeindpunt voor Blob-opslag is `<storage-account-name>.blob.core.windows.net`. U kunt ook het eindpunt op het web gegenereerd als onderdeel van de [statische websites-functie (preview)](storage-blob-static-website.md). Als u een aangepast domein en subdomein zoals toewijst **www.contoso.com** naar de blob of web-eindpunt voor uw opslag account, uw gebruikers kunnen vervolgens toegang tot blobgegevens in uw storage-account met behulp van dat domein.

> [!IMPORTANT]
> Azure Storage biedt niet nog systeemeigen ondersteuning voor HTTPS met aangepaste domeinen. U kunt op dit moment [het Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md).
>

> [!NOTE]  
> Storage-accounts ondersteunen momenteel slechts één aangepaste domeinnaam per account. Dit betekent dat u een aangepaste domeinnaam kan niet toewijzen aan de web- en blob service-eindpunten.

De volgende tabel ziet u enkele voorbeeld-URL's voor blob-gegevens die zich in een opslagaccount met de naam **mystorageaccount**. Het aangepaste domein voor het opslagaccount dat is geregistreerd **www.contoso.com**:

| Resourcetype | Standaard-URL | Aangepaste domein-URL |
| --- | --- | --- | --- |
| Storage-account | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Root-container | http://mystorageaccount.blob.core.windows.net/myblob of http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob of http://www.contoso.com/$root/myblob |
| Web |  http://mystorageaccount. [zone].web.core.windows.net/$web/[indexdoc] of http://mystorageaccount. [ zone].Web.Core.Windows.NET/[indexdoc] of http://mystorageaccount. [ zone].Web.Core.Windows.NET/$web of http://mystorageaccount. [ zone].Web.Core.Windows.NET/ | http://www.contoso.com/$web of http://www.contoso.com/ of http://www.contoso.com/$web / [indexdoc] of http://www.contoso.com/[indexdoc] |

> [!NOTE]  
> Alle voorbeelden voor het eindpunt van Blob service hieronder zijn ook van toepassing op het web service-eindpunt.

## <a name="direct-vs-intermediary-domain-mapping"></a>Versus tussenliggende domeintoewijzing direct

Er zijn twee manieren om uw aangepaste domein verwijst naar het blobeindpunt voor uw opslagaccount: directe CNAME-toewijzing en het gebruik van de *asverify* tussenliggende subdomein.

### <a name="direct-cname-mapping"></a>Directe CNAME-toewijzing

De methode voor eerste en eenvoudigste, is het maken van een canonieke naam (CNAME)-record die uw aangepaste domein en subdomein dat rechtstreeks naar het blobeindpunt wordt toegewezen. Een CNAME-record is een domain name system (DNS)-functie die een brondomein wordt toegewezen aan een doeldomein. In dit geval het brondomein is uw eigen aangepaste domein en subdomein, bijvoorbeeld *www.contoso.com*. Is uw Blob-service-eindpunt, zoals het doeldomein *mystorageaccount.blob.core.windows.net*.

De directe methode die wordt beschreven in [registreren van een aangepast domein](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Tussenliggende toewijzing met *asverify*

De tweede methode maakt ook gebruik van CNAME-records, maar eerst de veiligheidsmaatregelen voor een speciale subdomein wordt herkend door Azure geen downtime: **asverify**.

Het proces voor het toewijzen van uw aangepaste domein naar een blobeindpunt kan resulteren in een korte periode van uitvaltijd voor het domein terwijl u registreert in de [Azure-portal](https://portal.azure.com). Als uw aangepaste domein wordt momenteel ondersteund door een toepassing met een service level agreement (SLA) waarvoor geen downtime, dan kunt u de Azure *asverify* subdomein als een tussenliggende registratiestap. Deze tussenliggende stap zorgt ervoor dat gebruikers kunnen toegang krijgen tot uw domein terwijl de DNS-toewijzing doorgevoerd wordt.

De tussenliggende methode wordt beschreven in [registreren van een aangepast domein met de *asverify* subdomein](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Registreren van een aangepast domein
Gebruik deze procedure voor het registreren van uw aangepaste domein hebt u geen zorgen te maken over het domein dat even niet beschikbaar voor uw gebruikers, of als uw aangepaste domein is momenteel geen host voor een toepassing. Azure DNS kunt u een aangepaste DNS-naam voor uw Azure Blob-archief configureren. Zie [Use Azure DNS to provide custom domain settings for an Azure service](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

Als uw aangepaste domein wordt momenteel ondersteund door een toepassing die geen uitvaltijd, volgt u de procedure die wordt beschreven [registreren van een aangepast domein met de *asverify* subdomein](#register-a-custom-domain-using-the-asverify-subdomain).

Als u wilt een aangepaste domeinnaam configureren, moet u een nieuwe CNAME-record maken in DNS. De CNAME-record bevat een alias voor de naam van een domein. Het wordt in dit geval wordt het adres van uw aangepaste domein toegewezen aan het eindpunt van de Blob-opslag voor uw storage-account.

Normaal gesproken kunt u beheren van uw domein-DNS-instellingen op de website van uw domeinregistrar. Elke registrar heeft een methode vergelijkbaar, maar enigszins afwijken van het opgeven van een CNAME-record, maar het concept is hetzelfde. Sommige pakketten van de registratie Basisbeleid bieden DNS-configuratie, dus u uw domein registratie-pakket bijwerken moet misschien voordat u kunt de CNAME-record maken.

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepast domein** openen de *aangepast domein* blade.
1. Meld u aan bij de website van uw domeinregistrar en Ga naar de pagina voor het beheren van DNS. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
1. Zoek het gedeelte voor het beheren van CNAME's. Mogelijk moet u gaat u naar een pagina met geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**, of **subdomeinen**.
1. Een nieuwe CNAME-record maken en geef de alias van een subdomein zoals **www** of **foto's**. Geef vervolgens een hostnaam, uw Blob service-eindpunt, in de indeling is **mystorageaccount.blob.core.windows.net** (waarbij *mystorageaccount* is de naam van uw storage-account). De naam van de host te gebruiken wordt weergegeven in item #1 van de *aangepast domein* -blade in de [Azure-portal](https://portal.azure.com).
1. In het tekstvak in op de *aangepast domein* -blade in de [Azure-portal](https://portal.azure.com), voer de naam van uw aangepaste domein, met inbegrip van het subdomein. Bijvoorbeeld, als uw domein is **contoso.com** en je alias subdomein is **www**, voer **www.contoso.com**. Als uw subdomein **foto's**, voer **photos.contoso.com**. Het subdomein is *vereist*.
1. Selecteer **opslaan** op de *aangepast domein* blade voor het registreren van uw aangepaste domein. Als de registratie gelukt is, ziet u een portalmelding dat uw storage-account is bijgewerkt.

Zodra de nieuwe CNAME-record is doorgegeven via DNS, kunnen uw gebruikers blob-gegevens met behulp van uw aangepaste domein, weergeven, zolang ze de juiste machtigingen hebben.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Registreer een aangepast domein met de *asverify* subdomein
Gebruik deze procedure voor het registreren van uw aangepaste domein als uw aangepaste domein wordt momenteel ondersteund door een toepassing met een SLA die is vereist dat er geen downtime. Door het maken van een CNAME die van `asverify.<subdomain>.<customdomain>` naar `asverify.<storageaccount>.blob.core.windows.net`, kunt u uw domein met Azure vooraf registreren. U kunt vervolgens een tweede CNAME die van maken `<subdomain>.<customdomain>` naar `<storageaccount>.blob.core.windows.net`, waarna het verkeer naar uw aangepaste domein worden omgeleid naar uw blobeindpunt.

De **asverify** subdomein is een speciale subdomein wordt herkend door Azure. Door het begin `asverify` aan uw eigen subdomein, kan u Azure voor het herkennen van uw aangepaste domein zonder te wijzigen van de DNS-record voor het domein. Wanneer u mag de DNS-record voor het domein worden gewijzigd, wordt deze toegewezen aan het blobeindpunt zonder uitvaltijd.

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepast domein** openen de *aangepast domein* blade.
1. Meld u aan bij de website van uw DNS-provider en Ga naar de pagina voor het beheren van DNS. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
1. Zoek het gedeelte voor het beheren van CNAME's. Mogelijk moet u gaat u naar een pagina met geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**, of **subdomeinen**.
1. Maak een nieuwe CNAME-record, en geef de alias van een subdomein met de *asverify* subdomein. Bijvoorbeeld, **asverify.www** of **asverify.photos**. Geef vervolgens een hostnaam, uw Blob service-eindpunt, in de indeling is **asverify.mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** is de naam van uw storage-account). De naam van de host te gebruiken wordt weergegeven in item #2 van de *aangepast domein* -blade in de [Azure-portal](https://portal.azure.com).
1. In het tekstvak in op de *aangepast domein* -blade in de [Azure-portal](https://portal.azure.com), voer de naam van uw aangepaste domein, met inbegrip van het subdomein. Maak daarbij dan geen *asverify*. Bijvoorbeeld, als uw domein is **contoso.com** en je alias subdomein is **www**, voer **www.contoso.com**. Als uw subdomein **foto's**, voer **photos.contoso.com**. Het subdomein is vereist.
1. Selecteer de **indirecte CNAME-validatie gebruiken** selectievakje.
1. Selecteer **opslaan** op de *aangepast domein* blade voor het registreren van uw aangepaste domein. Als de registratie gelukt is, ziet u een portalmelding waarin staat dat uw storage-account is bijgewerkt. Op dit moment uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet wordt doorgestuurd naar uw storage-account.
1. Terug naar de website van uw DNS-provider en maken van een andere CNAME-record die uw subdomein wordt toegewezen aan het eindpunt van de Blob-service. Bijvoorbeeld, geef het subdomein als **www** of **foto's** (zonder de *asverify*), en de hostnaam als **mystorageaccount.blob.core.windows.net**  (waarbij **mystorageaccount** is de naam van uw storage-account). De registratie van uw aangepaste domein is met deze stap is voltooid.
1. Ten slotte kunt u de CNAME-record die u hebt gemaakt die verwijderen de **asverify** subdomein, omdat deze alleen als een tussenliggend stap nodig is.

Zodra de nieuwe CNAME-record is doorgegeven via DNS, kunnen uw gebruikers blob-gegevens met behulp van uw aangepaste domein, weergeven, zolang ze de juiste machtigingen hebben.

## <a name="test-your-custom-domain"></a>Uw aangepaste domein testen

Om te bevestigen dat uw aangepaste domein inderdaad is toegewezen aan uw eindpunt van Blob service, een blob in een openbare container binnen uw opslagaccount te maken. Vervolgens, in een webbrowser gebruiken een URI in de volgende indeling voor toegang tot de blob:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Bijvoorbeeld, kunt u de volgende URI voor toegang tot een webformulier in het **myforms** -container in de **photos.contoso.com** aangepaste subdomein:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Registratie ongedaan maken van een aangepast domein

Als u wilt een aangepast domein voor het eindpunt van Blob-opslag voor de registratie ongedaan maken, gebruikt u een van de volgende procedures.

### <a name="azure-portal"></a>Azure Portal

Voer de volgende stappen uit in Azure portal om te verwijderen van de instelling van het aangepaste domein:

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepast domein** openen de *aangepast domein* blade.
1. Hiermee schakelt u de inhoud van het tekstvak met de naam van uw aangepaste domein.
1. Selecteer de knop **Opslaan**.

Wanneer het aangepaste domein is verwijderd, ziet u een portalmelding waarin staat dat uw storage-account is bijgewerkt.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Gebruik de [az storage account update](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_update) CLI opdracht en geeft u een lege tekenreeks (`""`) voor de `--custom-domain` argumentwaarde te verwijderen van de registratie van een aangepast domein.

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

Gebruik de [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) PowerShell-cmdlet en geef een lege tekenreeks (`""`) voor de `-CustomDomainName` argumentwaarde te verwijderen van de registratie van een aangepast domein.

* De opdrachtindeling van de:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Opdrachtvoorbeeld van de:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Volgende stappen
* [Een aangepast domein toewijzen aan een eindpunt Azure Content Delivery Network (CDN)](../../cdn/cdn-map-content-to-custom-domain.md)
* [De Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)
* [Statische website hosting in Azure Blob Storage (Preview)](storage-blob-static-website.md)

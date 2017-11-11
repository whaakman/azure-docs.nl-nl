---
title: Een aangepaste domeinnaam configureren voor het eindpunt van de Azure Blob-opslag | Microsoft Docs
description: De Azure portal gebruiken voor uw eigen canonieke naam (CNAME) toewijzen aan het eindpunt van de Blob-opslag in een Azure Storage-account.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: tamram
ms.openlocfilehash: 4dfca37e3a369fdfb41b4064b72ccd027bd8cbd3
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Een aangepaste domeinnaam configureren voor het eindpunt voor Blob Storage

U kunt een aangepast domein voor toegang tot blobgegevens in uw Azure storage-account configureren. Het standaardeindpunt voor Blob storage is `<storage-account-name>.blob.core.windows.net`. Als u een aangepaste domeinen en subdomeinen zoals toewijst **www.contoso.com** naar het blobeindpunt voor de opslag van uw account, uw gebruikers kunnen vervolgens toegang tot blobgegevens in uw storage-account met behulp van dat domein.

> [!IMPORTANT]
> Azure-opslag ondersteunt nog systeemeigen geen HTTPS met aangepaste domeinen. U kunt momenteel [Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md).
>

De volgende tabel bevat enkele voorbeeld-URL's voor blob-gegevens die zich in een opslagaccount met de naam **mystorageaccount**. Het aangepaste domein is geregistreerd voor het opslagaccount is **www.contoso.com**:

| Resourcetype | Standaard-URL | Aangepast domein-URL |
| --- | --- | --- |
| Storage-account | http://mystorageaccount.BLOB.Core.Windows.NET | http://www.contoso.com |
| Blob |http://mystorageaccount.BLOB.Core.Windows.NET/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Hoofdcontainer | http://mystorageaccount.BLOB.Core.Windows.NET/myblob of http://mystorageaccount.blob.core.windows.net/$ root/myblob| http://www.contoso.com/myblob of http://www.contoso.com/$ root/myblob |

## <a name="direct-vs-intermediary-domain-mapping"></a>Directe versus tussenliggend domein toewijzing

Er zijn twee manieren uw aangepaste domein verwijzen naar het blobeindpunt voor voor uw opslagaccount: directe CNAME toewijzing en het gebruik van de *asverify* tussenliggende subdomein.

### <a name="direct-cname-mapping"></a>Directe CNAME-toewijzing

De methode voor eerste en eenvoudigste, is het maken van een record canonieke naam (CNAME) waarmee uw aangepaste domeinen en subdomeinen rechtstreeks aan het blobeindpunt. Een CNAME-record is een domain name system (DNS)-functie die een brondomein wordt toegewezen aan een doeldomein. In dit geval wordt het brondomein is uw eigen aangepaste domeinen en subdomeinen, bijvoorbeeld *www.contoso.com*. Het doeldomein van uw eindpunt Blob-service is bijvoorbeeld *mystorageaccount.blob.core.windows.net*.

De directe methode wordt beschreven in [registreren van een aangepast domein](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Tussenliggende toewijzing met *asverify*

De tweede methode gebruikt ook CNAME-records, maar eerst de veiligheidsmaatregelen voor een speciale subdomein geen uitvaltijd wordt herkend door Azure: **asverify**.

Het proces voor het toewijzen van uw aangepaste domein naar een blobeindpunt kan resulteren in een korte periode van uitvaltijd voor het domein, terwijl u registreert in de [Azure-portal](https://portal.azure.com). Als uw aangepaste domein wordt momenteel ondersteund door een toepassing met een service level agreement (SLA) waarvoor uitvaltijd, dan kunt u de Azure *asverify* subdomein als een tussenliggende registratiestap. Deze tussenliggende stap zorgt u ervoor kunnen gebruikers zich toegang tot uw domein terwijl de DNS-toewijzing plaatsvindt.

De tussenliggende methode wordt beschreven in [registreren van een aangepast domein met de *asverify* subdomein](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Registreren van een aangepast domein
Gebruik deze procedure voor het registreren van uw aangepaste domein als u zich geen zorgen over het domein wordt kort niet beschikbaar voor uw gebruikers, of als uw aangepaste domein is momenteel geen host voor een toepassing. Azure DNS kunt u een aangepaste DNS-naam voor uw Azure Blob-opslag configureren. Zie voor meer informatie [gebruik Azure DNS-instellingen van aangepast domein voor een Azure-service kan leveren](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage).

Als uw aangepaste domein wordt momenteel ondersteund door een toepassing die geen uitvaltijd, volgt u de procedure beschreven in [registreren van een aangepast domein met de *asverify* subdomein](#register-a-custom-domain-using-the-asverify-subdomain).

Als u wilt een aangepaste domeinnaam configureren, moet u een nieuwe CNAME-record in DNS. De CNAME-record bevat een alias voor een domeinnaam. In dit geval wordt het adres van uw aangepaste domein toegewezen aan het eindpunt Blob-opslag voor uw opslagaccount.

Doorgaans kunt u uw domein-DNS-instellingen op uw domeinregistrar website beheren. Elke registrar heeft een methode vergelijkbaar maar enigszins verschillen van het opgeven van een CNAME-record, maar het concept is hetzelfde. Sommige pakketten van de registratie basic domein bieden geen DNS-configuratie, dus u uw domein registratiepakket upgraden moet misschien voordat u kunt de CNAME-record maken.

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepaste domeinen** openen de *aangepaste domeinen* blade.
1. Meld u aan bij uw domeinregistrar website en Ga naar de pagina voor het beheren van DNS. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
1. Zoek het gedeelte voor het beheren van CNAME's. Wellicht hebt u om te gaan naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**, of **subdomeinen**.
1. Maak een nieuwe CNAME-record en geef een alias voor een subdomein zoals **www** of **foto's**. Geef een host-naam, uw eindpunt Blob-service in de notatie **mystorageaccount.blob.core.windows.net** (waarbij *mystorageaccount* is de naam van uw storage-account). De hostnaam gebruiken wordt weergegeven in item #1 van de *aangepaste domeinen* blade in de [Azure-portal](https://portal.azure.com).
1. In het tekstvak op de *aangepaste domeinen* blade in de [Azure-portal](https://portal.azure.com), voer de naam van uw aangepaste domein, met inbegrip van het subdomein. Als uw domein bijvoorbeeld **contoso.com** en je alias subdomein is **www**, voer **www.contoso.com**. Als een subdomein is **foto's**, voer **photos.contoso.com**. Het subdomein is *vereist*.
1. Selecteer **opslaan** op de *aangepaste domeinen* blade registreren van uw aangepaste domein. Als de registratie gelukt is, ziet u een portal melding uw storage-account is bijgewerkt.

Als uw nieuwe CNAME-record is doorgegeven via DNS is vastgesteld, kunnen uw gebruikers blob-gegevens met behulp van uw aangepaste domein bekijken, zolang ze de juiste machtigingen hebben.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Registreren van een aangepast domein met de *asverify* subdomein
Gebruik deze procedure voor het registreren van uw aangepaste domein als uw aangepaste domein wordt momenteel ondersteund door een toepassing met een SLA die dat vereist er geen uitvaltijd. Door het maken van een CNAME die vanaf `asverify.<subdomain>.<customdomain>` naar `asverify.<storageaccount>.blob.core.windows.net`, kunt u uw domein met Azure vooraf registreren. Vervolgens kunt u een tweede CNAME die vanaf maken `<subdomain>.<customdomain>` naar `<storageaccount>.blob.core.windows.net`, waarna de clientzijdebewaking verkeer naar uw aangepaste domein wordt omgeleid naar het eindpunt van de blob.

De **asverify** subdomein is een speciale subdomein dat wordt herkend door Azure. Door voorafgaand `asverify` voor uw eigen subdomein u toe dat Azure voor het herkennen van uw aangepaste domein zonder te wijzigen van de DNS-record voor het domein. Wanneer u de DNS-record voor het domein wijzigen, worden deze toegewezen aan het blobeindpunt zonder uitvaltijd.

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepaste domeinen** openen de *aangepaste domeinen* blade.
1. Meld u aan bij de website van uw DNS-provider en Ga naar de pagina voor het beheren van DNS. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.
1. Zoek het gedeelte voor het beheren van CNAME's. Wellicht hebt u om te gaan naar een pagina Geavanceerde instellingen en zoek naar de woorden **CNAME**, **Alias**, of **subdomeinen**.
1. Maak een nieuwe CNAME-record en geef een alias voor een subdomein waarin de *asverify* subdomein. Bijvoorbeeld: **asverify.www** of **asverify.photos**. Geef een host-naam, uw eindpunt Blob-service in de notatie **asverify.mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** is de naam van uw storage-account). De hostnaam gebruiken wordt weergegeven in item #2 van de *aangepaste domeinen* blade in de [Azure-portal](https://portal.azure.com).
1. In het tekstvak op de *aangepaste domeinen* blade in de [Azure-portal](https://portal.azure.com), voer de naam van uw aangepaste domein, met inbegrip van het subdomein. Neem geen *asverify*. Als uw domein bijvoorbeeld **contoso.com** en je alias subdomein is **www**, voer **www.contoso.com**. Als een subdomein is **foto's**, voer **photos.contoso.com**. Het subdomein is vereist.
1. Selecteer de **indirecte CNAME-validatie gebruiken** selectievakje.
1. Selecteer **opslaan** op de *aangepaste domeinen* blade registreren van uw aangepaste domein. Als de registratie gelukt is, ziet u een portal melding uw storage-account is bijgewerkt. Op dit moment uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet wordt gerouteerd naar uw opslagaccount.
1. Terug naar de website van uw DNS-provider en een andere CNAME-record maken die een subdomein wordt toegewezen aan het eindpunt van de Blob-service. Geef bijvoorbeeld het subdomein als **www** of **foto's** (zonder de *asverify*), en de hostnaam als **mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** is de naam van uw storage-account). De registratie van uw aangepaste domein is met deze stap is voltooid.
1. Ten slotte kunt u de CNAME-record dat u hebt gemaakt die verwijderen de **asverify** subdomein, zoals deze is alleen als een tussenliggende stap nodig.

Als uw nieuwe CNAME-record is doorgegeven via DNS is vastgesteld, kunnen uw gebruikers blob-gegevens met behulp van uw aangepaste domein bekijken, zolang ze de juiste machtigingen hebben.

## <a name="test-your-custom-domain"></a>Testen van uw aangepaste domein

Om te bevestigen dat uw aangepaste domein inderdaad is toegewezen aan het eindpunt van de Blob-service, maakt u een blob in een openbare container in uw opslagaccount. Klik, in een webbrowser een URI gebruikt in de volgende notatie voor toegang tot de blob:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Bijvoorbeeld, kunt u de volgende URI voor toegang tot een webformulier in de **myforms** container in de **photos.contoso.com** aangepast subdomein:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Een aangepast domein opgeheven

Als u wilt een aangepast domein voor het eindpunt van de Blob-opslag opgeheven, moet u een van de volgende procedures gebruiken.

### <a name="azure-portal"></a>Azure Portal

Voer de volgende stappen in de Azure portal om de instelling van het aangepast domein te verwijderen:

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Onder **BLOB-SERVICE** Selecteer op de blade menu **aangepaste domeinen** openen de *aangepaste domeinen* blade.
1. Hiermee schakelt u de inhoud van het tekstvak met uw aangepaste domeinnaam.
1. Selecteer de knop **Opslaan**.

Wanneer het aangepaste domein is verwijderd, ziet u een portal melding uw storage-account is bijgewerkt.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Gebruik de [az storage account bijwerken](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_update) CLI opdracht in en geef een lege tekenreeks (`""`) voor de `--custom-domain` argumentwaarde verwijderen van de registratie van een aangepast domein.

* De opdrachtindeling van de:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Voorbeeld van de opdracht:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Gebruik de [Set AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) PowerShell-cmdlet en geef een lege tekenreeks (`""`) voor de `-CustomDomainName` argumentwaarde verwijderen van de registratie van een aangepast domein.

* De opdrachtindeling van de:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Voorbeeld van de opdracht:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Volgende stappen
* [Een aangepast domein toewijzen aan een Azure Content Delivery Network (CDN)-eindpunt](../../cdn/cdn-map-content-to-custom-domain.md)
* [Azure CDN gebruiken voor toegang tot blobs met aangepaste domeinen via HTTPS](storage-https-custom-domain-cdn.md)

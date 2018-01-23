---
title: Azure DNS integreren met uw Azure-resources | Microsoft Docs
description: Informatie over het gebruik van Azure DNS langs voor DNS voor uw Azure-resources.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/19/2018
ms.author: kumud
ms.openlocfilehash: cbc769cd7356b3057fd2aae295071b04d2e40d91
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Gebruik Azure DNS de instellingen van het aangepaste domein voor een Azure-service opgeven

Azure DNS biedt DNS voor een aangepast domein voor een van uw Azure-resources dat ondersteuning van aangepaste domeinen of die een volledig gekwalificeerde domeinnaam (FQDN) hebben. Een voorbeeld is er een Azure-web-app en u wilt dat gebruikers toegang tot het door u contoso.com of www.contoso.com als een FQDN-naam. In dit artikel begeleidt u bij het configureren van uw Azure-service met Azure DNS voor het gebruik van aangepaste domeinen.

## <a name="prerequisites"></a>Vereisten

Om Azure DNS gebruiken voor uw aangepaste domein, moet u eerst uw domein aan Azure DNS overdragen. Ga naar [een domein aan Azure DNS overdragen](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van de naamservers voor overdracht. Als uw domein is toegewezen aan uw Azure DNS-zone, zich u voor het configureren van de DNS-records die nodig zijn.

U kunt een vanity of een aangepast domein voor [Azure functie Apps](#azure-function-app), [openbare IP-adressen](#public-ip-address), [App Service (Web-Apps)](#app-service-web-apps), [Blob storage](#blob-storage), en [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Azure-functie-App

Voor het configureren van een aangepast domein voor Azure-functie apps is een CNAME-record gemaakt en de configuratie op de functie app zelf.
 
Navigeer naar **andere** > **functie-App** en selecteert u de functie-App. Klik op **platformfuncties** en klikt u onder **NETWORKING** klikt u op **aangepaste domeinen**.

![de functie app-blade](./media/dns-custom-domain/functionapp.png)

Let op de huidige url op de **aangepaste domeinen** blade dit adres wordt gebruikt als de alias voor de DNS-record gemaakt.

![aangepast domein-blade](./media/dns-custom-domain/functionshostname.png)

Navigeer naar de DNS-Zone en klik op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blade en klik op **OK** om deze te maken.

|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Naam     | myfunctionapp        | Deze waarde samen met het domeinnaamlabel is de FQDN voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting         |
|Alias     | adatumfunction.azurewebsites.net        | De DNS-naam die u maakt de alias voor, in dit voorbeeld is de adatumfunction.azurewebsites.net DNS-naam opgegeven standaard naar de functie-app.        |

Ga terug naar uw app in de functie, klikt u op **platformfuncties**, en klikt u onder **NETWORKING** klikt u op **aangepaste domeinen**, klikt u vervolgens onder **hostnamen**klikt u op **+ toevoegen hostnaam**.

Op de **hostnaam toevoegen** blade de CNAME-record in de **hostnaam** tekstveld en klikt u op **valideren**. Als de record kan worden gevonden, is de **hostnaam toevoegen** knop wordt weergegeven. Klik op **hostnaam toevoegen** de alias toevoegen.

![functie apps toevoegen host naam blade](./media/dns-custom-domain/functionaddhostname.png)

## <a name="public-ip-address"></a>Openbaar IP-adres

Een aangepast domein configureren voor services die gebruikmaken van een openbare IP-adres resource zoals Application Gateway, Load Balancer, Service in de Cloud, VM's van Resource Manager en Classic VM's, een CNAME-record gebruikt.

Navigeer naar **Networking** > **openbaar IP-adres**, selecteert u de openbare IP-resource en klikt u op **configuratie**. Het IP-adres weergegeven specificeren.

![openbare IP-blade](./media/dns-custom-domain/publicip.png)

Navigeer naar de DNS-Zone en klik op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blade en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Naam     | mywebserver        | Deze waarde samen met het domeinnaamlabel is de FQDN voor de aangepaste domeinnaam.        |
|Type     | A        | Een A-record gebruiken als de bron een IP-adres is in.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting         |
|IP-adres     | <your ip address>       | Het openbare IP-adres.|

![een A-record maken](./media/dns-custom-domain/arecord.png)

Zodra de A-record is gemaakt, voer `nslookup` voor het valideren van de record wordt omgezet.

![openbare IP-adres DNS-zoekopdracht](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>App Service (Web-Apps)

De volgende stappen gaat u door het configureren van een aangepast domein voor een app service-web-app.

Navigeer naar **Web en mobiel** > **App Service** en selecteer de bron die u configureert een aangepaste domeinnaam en klik op **aangepaste domeinen**.

Let op de huidige url op de **aangepaste domeinen** blade dit adres wordt gebruikt als de alias voor de DNS-record gemaakt.

![blade voor aangepaste domeinen](./media/dns-custom-domain/url.png)

Navigeer naar de DNS-Zone en klik op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blade en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Naam     | mywebserver        | Deze waarde samen met het domeinnaamlabel is de FQDN voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias. Als de bron een IP-adres gebruikt, wordt een A-record gebruikt.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting         |
|Alias     | webserver.azurewebsites.net        | De DNS-naam die u maakt de alias voor, in dit voorbeeld is de webserver.azurewebsites.net DNS-naam opgegeven standaard naar de web-app.        |


![Een CNAME-record maken](./media/dns-custom-domain/createcnamerecord.png)

Ga terug naar de app-service die is geconfigureerd voor de aangepaste domeinnaam. Klik op **aangepaste domeinen**, klikt u vervolgens op **hostnamen**. Als u wilt toevoegen de CNAME-record dat u hebt gemaakt, klikt u op **+ toevoegen hostnaam**.

![afbeelding 1](./media/dns-custom-domain/figure1.png)

Uitvoeren zodra het proces voltooid is, **nslookup** valideren naamomzetting werkt.

![afbeelding 1](./media/dns-custom-domain/finalnslookup.png)

Voor meer informatie over het toewijzen van een aangepast domein in App Service, gaat u naar [aangepaste DNS-naam van een bestaande toewijzen aan Azure Web Apps](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Als u een aangepast domein aanschaffen moet, gaat u naar [aanschaffen van een aangepaste domeinnaam voor Azure-Web-Apps](../app-service/custom-dns-web-site-buydomains-web-app.md) voor meer informatie over App Service-domeinen.

## <a name="blob-storage"></a>Blob Storage

De volgende stappen gaat u door het configureren van een CNAME-record voor een blob storage-account via de methode asverify. Deze methode ervoor zorgt dat er is geen uitvaltijd.

Navigeer naar **opslag** > **Opslagaccounts**, selecteer uw storage-account en klikt u op **aangepaste domeinen**. De FQDN-naam in stap 2 specificeren, wordt deze waarde wordt gebruikt om de eerste CNAME-record te maken

![BLOB storage aangepast domein](./media/dns-custom-domain/blobcustomdomain.png)

Navigeer naar de DNS-Zone en klik op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blade en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Naam     | asverify.mystorageaccount        | Deze waarde samen met het domeinnaamlabel is de FQDN voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting         |
|Alias     | asverify.adatumfunctiona9ed.blob.core.windows.net        | De DNS-naam die u maakt de alias voor, in dit voorbeeld is de asverify.adatumfunctiona9ed.blob.core.windows.net DNS-naam opgegeven standaard naar de storage-account.        |

Ga terug naar uw storage-account door te klikken op **opslag** > **Opslagaccounts**, selecteer uw storage-account en klik op **aangepaste domeinen**. Typ de alias die u hebt gemaakt zonder het voorvoegsel asverify in het tekstvak, selectievakje ** indirecte CNAME-validatie gebruiken en op **opslaan**. Nadat deze stap voltooid is, terug naar de DNS-zone en maakt u een CNAME-record zonder het voorvoegsel asverify.  Daarna u veilig verwijderen van de CNAME-record met het voorvoegsel cdnverify zijn.

![BLOB storage aangepast domein](./media/dns-custom-domain/indirectvalidate.png)

DNS-omzetting valideren door te voeren`nslookup`

Voor meer informatie over een aangepast domein toewijzen aan een blobeindpunt-opslag [een aangepaste domeinnaam configureren voor het eindpunt van de Blob-opslag](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

De volgende stappen gaat u door het configureren van een CNAME-record voor een CDN-eindpunt met de methode cdnverify. Deze methode ervoor zorgt dat er is geen uitvaltijd.

Navigeer naar **Networking** > **CDN-profielen**, selecteer uw CDN-profiel en klikt u op **eindpunten** onder **algemene**.

Selecteer het eindpunt dat u werkt met en klikt u op **+ aangepaste domeinen**. Opmerking de **hostnaam van het eindpunt** als deze waarde de record die de CNAME-record verwijst is naar.

![Aangepast CDN-domein](./media/dns-custom-domain/endpointcustomdomain.png)

Navigeer naar de DNS-Zone en klik op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blade en klik op **OK** om deze te maken.

|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Naam     | cdnverify.mycdnendpoint        | Deze waarde samen met het domeinnaamlabel is de FQDN voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.net        | De DNS-naam die u maakt de alias voor, in dit voorbeeld is de cdnverify.adatumcdnendpoint.azureedge.net DNS-naam opgegeven standaard naar de storage-account.        |

Ga terug naar uw CDN-eindpunt door te klikken op **Networking** > **CDN-profielen**, en selecteer uw CDN-profiel. Klik op **+ aangepaste domeinen** uw CNAME-record alias zonder het voorvoegsel cdnverify invoeren en op **toevoegen**.

Nadat deze stap voltooid is, terug naar de DNS-zone en maakt u een CNAME-record zonder het voorvoegsel cdnverify.  Daarna u veilig verwijderen van de CNAME-record met het voorvoegsel cdnverify zijn. Ga voor meer informatie over CDN en het configureren van een aangepast domein zonder de tussenliggende registratiestap naar [kaart Azure CDN-inhoud naar een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [omgekeerde DNS voor services die worden gehost in Azure configureren](dns-reverse-dns-for-azure-services.md).
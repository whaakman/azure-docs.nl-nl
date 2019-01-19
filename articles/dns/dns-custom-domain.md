---
title: Azure DNS integreren met uw Azure-resources
description: Informatie over het gebruik van Azure DNS langs voor DNS voor uw Azure-resources.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/18/2019
ms.author: victorh
ms.openlocfilehash: b513e898e25397f54b8f7f7590a4466523a705ff
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401415"
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Azure DNS gebruiken om aangepaste domein-instellingen voor een Azure-service te geven

Azure DNS biedt DNS voor een aangepast domein voor het gebruik van uw Azure-resources die ondersteuning voor aangepaste domeinen of die een volledig gekwalificeerde domeinnaam (FQDN) hebben. Een voorbeeld is u een Azure-web-app hebt en u wilt dat uw gebruikers toegang tot het door een van beide contoso.com of www.contoso.com als een FQDN-naam gebruiken. In dit artikel begeleidt u bij het configureren van uw Azure-service met Azure DNS voor het gebruik van aangepaste domeinen.

## <a name="prerequisites"></a>Vereisten

Om Azure DNS gebruiken voor uw aangepaste domein, moet u eerst uw domein naar Azure DNS delegeren. Ga naar [een domein delegeren naar Azure DNS](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van de naamservers voor overdracht. Als uw domein wordt overgedragen naar de Azure DNS-zone, bent u kunt configureren van de DNS-records die nodig zijn.

U kunt een vanity of een aangepast domein voor [Azure functie-Apps](#azure-function-app), [openbare IP-adressen](#public-ip-address), [App Service (Web-Apps)](#app-service-web-apps), [Blob storage](#blob-storage), en [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Azure Functions-App

Een CNAME-record is gemaakt voor het configureren van een aangepast domein voor Azure-functie-apps, en de configuratie op de functie-app zelf.
 
Navigeer naar **functie-App** en selecteer uw functie-app. Klik op **platformfuncties** en klikt u onder **netwerken** klikt u op **aangepaste domeinen**.

![functie-app-blade](./media/dns-custom-domain/functionapp.png)

Houd er rekening mee de url voor de huidige op de **aangepaste domeinen** blade dit adres wordt gebruikt als de alias voor de DNS-record gemaakt.

![aangepast domein-blade](./media/dns-custom-domain/functionshostname.png)

Navigeer naar de DNS-Zone en klikt u op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blad en klik op **OK** om deze te maken.

|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Name     | myfunctionapp        | Deze waarde samen met het label van de domeinnaam is de FQDN-naam voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting van de         |
|Alias     | adatumfunction.azurewebsites.net        | De DNS-naam maakt u de alias voor, in dit voorbeeld is de DNS-naam van het adatumfunction.azurewebsites.net geboden door standaard aan de functie-app.        |

Ga terug naar uw functie-app, klikt u op **platformfuncties**, en klikt u onder **netwerken** klikt u op **aangepaste domeinen**, klikt u vervolgens onder **aangepaste hostnamen** klikt u op **+ hostnaam toevoegen**.

Op de **hostnaam toevoegen** blade, voer de CNAME-record in de **hostnaam** tekstveld en klikt u op **valideren**. Als de record wordt gevonden, de **hostnaam toevoegen** knop wordt weergegeven. Klik op **hostnaam toevoegen** om toe te voegen van de alias.

![functie-apps toevoegen host naam blade](./media/dns-custom-domain/functionaddhostname.png)

## <a name="public-ip-address"></a>Openbaar IP-adres

Een aangepast domein configureren voor services die gebruikmaken van een openbaar IP-adres bron, zoals Application Gateway, Load Balancer, Cloudservice, virtuele machines Resource Manager en klassieke virtuele machines, een CNAME-record die wordt gebruikt.

Navigeer naar **netwerken** > **openbaar IP-adres**, selecteert u de openbare IP-resource en klik op **configuratie**. Het IP-adres weergegeven specificeren.

![de blade openbaar IP-adres](./media/dns-custom-domain/publicip.png)

Navigeer naar de DNS-Zone en klikt u op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blad en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Name     | mywebserver        | Deze waarde samen met het label van de domeinnaam is de FQDN-naam voor de aangepaste domeinnaam.        |
|Type     | A        | Als de resource een IP-adres, gebruikt u een A-record.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting van de         |
|IP-adres     | <your ip address>       | Het openbare IP-adres.|

![een A-record maken](./media/dns-custom-domain/arecord.png)

Nadat de A-record is gemaakt, voert `nslookup` voor het valideren van de record wordt opgelost.

![openbare IP-dns-zoekactie](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>App Service (Web Apps)

De volgende stappen gaat u door het configureren van een aangepast domein voor een app service web-app.

Navigeer naar **App Service** en selecteer de resource die u configureert een aangepaste domeinnaam en klik op **aangepaste domeinen**.

Houd er rekening mee de url voor de huidige op de **aangepaste domeinen** blade dit adres wordt gebruikt als de alias voor de DNS-record gemaakt.

![blade voor aangepaste domeinen](./media/dns-custom-domain/url.png)

Navigeer naar de DNS-Zone en klikt u op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blad en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Name     | mywebserver        | Deze waarde samen met het label van de domeinnaam is de FQDN-naam voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias. Als de resource een IP-adres gebruikt, zou een A-record worden gebruikt.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting van de         |
|Alias     | webserver.azurewebsites.net        | De DNS-naam maakt u de alias voor, in dit voorbeeld is de DNS-naam van het webserver.azurewebsites.net geboden door standaard naar de web-app.        |


![een CNAME-record maken](./media/dns-custom-domain/createcnamerecord.png)

Ga terug naar de app-service die is geconfigureerd voor de aangepaste domeinnaam. Klik op **aangepaste domeinen**, klikt u vervolgens op **hostnamen**. Als u wilt toevoegen de CNAME-record die u hebt gemaakt, klikt u op **+ hostnaam toevoegen**.

![afbeelding 1](./media/dns-custom-domain/figure1.png)

Nadat het proces voltooid is, voert **nslookup** valideren naamomzetting werkt.

![afbeelding 1](./media/dns-custom-domain/finalnslookup.png)

Voor meer informatie over een aangepast domein toewijzen aan App Service, gaat u naar [een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Als u een aangepast domein kopen wilt, gaat u naar [een aangepaste domeinnaam voor Azure Web Apps kopen](../app-service/manage-custom-dns-buy-domain.md) voor meer informatie over App Service-domeinen.

## <a name="blob-storage"></a>Blob Storage

De volgende stappen gaat u door het configureren van een CNAME-record voor een blob storage-account met behulp van de methode asverify. Deze methode zorgt ervoor dat er is geen downtime.

Navigeer naar **opslag** > **Opslagaccounts**, selecteert u uw storage-account en klikt u op **aangepast domein**. De FQDN-naam in stap 2 specificeren, deze waarde wordt gebruikt om de eerste CNAME-record maken

![BLOB storage-aangepast domein](./media/dns-custom-domain/blobcustomdomain.png)

Navigeer naar de DNS-Zone en klikt u op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blad en klik op **OK** om deze te maken.


|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Name     | asverify.mystorageaccount        | Deze waarde samen met het label van de domeinnaam is de FQDN-naam voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting van de         |
|Alias     | asverify.adatumfunctiona9ed.blob.core.windows.net        | De DNS-naam maakt u de alias voor, in dit voorbeeld is de DNS-naam van het asverify.adatumfunctiona9ed.blob.core.windows.net standaard naar het opslagaccount dat is opgegeven.        |

Ga terug naar uw storage-account door te klikken op **opslag** > **Opslagaccounts**, selecteert u uw storage-account en klikt u op **aangepast domein**. Typ de alias die u hebt gemaakt zonder het voorvoegsel asverify in het tekstvak, selectievakje ** indirecte CNAME-validatie gebruiken en op **opslaan**. Nadat deze stap voltooid is, gaat u terug naar de DNS-zone en maak een CNAME-record zonder het voorvoegsel asverify.  Nadat verwijzen, bent u veilig verwijderen van het CNAME-record met het voorvoegsel cdnverify.

![BLOB storage-aangepast domein](./media/dns-custom-domain/indirectvalidate.png)

DNS-omzetting valideren door te voeren `nslookup`

Voor meer informatie over een aangepast domein toewijzen aan een eindpunt van blob storage gaat u naar [een aangepaste domeinnaam voor uw Blob storage-eindpunt configureren](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

De volgende stappen gaat u door het configureren van een CNAME-record voor een CDN-eindpunt met behulp van het cdnverify-methode. Deze methode zorgt ervoor dat er is geen downtime.

Navigeer naar **netwerken** > **CDN-profielen**, selecteert u uw CDN-profiel.

Selecteer het eindpunt dat u werkt met en klikt u op **+ aangepast domein**. Houd er rekening mee de **hostnaam van eindpunt** als deze waarde de record die de CNAME-record verwijst is naar.

![Aangepast CDN-domein](./media/dns-custom-domain/endpointcustomdomain.png)

Navigeer naar de DNS-Zone en klikt u op **+ Recordset**. Vul de volgende informatie op de **recordset toevoegen** blad en klik op **OK** om deze te maken.

|Eigenschap  |Waarde  |Beschrijving  |
|---------|---------|---------|
|Name     | cdnverify.mycdnendpoint        | Deze waarde samen met het label van de domeinnaam is de FQDN-naam voor de aangepaste domeinnaam.        |
|Type     | CNAME        | Gebruik een CNAME-record maakt gebruik van een alias.        |
|TTL     | 1        | 1 wordt gebruikt voor 1 uur        |
|TTL-eenheid     | Uren        | Uren worden gebruikt als de tijdmeting van de         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.net        | De DNS-naam maakt u de alias voor, in dit voorbeeld is de DNS-naam van het cdnverify.adatumcdnendpoint.azureedge.net standaard naar het opslagaccount dat is opgegeven.        |

Ga terug naar uw CDN-eindpunt door te klikken op **netwerken** > **CDN-profielen**, en selecteer uw CDN-profiel. Klik op **+ aangepast domein** je alias CNAME-record zonder het voorvoegsel cdnverify invoeren en op **toevoegen**.

Nadat deze stap voltooid is, gaat u terug naar de DNS-zone en maak een CNAME-record zonder het cdnverify-voorvoegsel.  Nadat verwijzen, bent u veilig verwijderen van het CNAME-record met het voorvoegsel cdnverify. Ga voor meer informatie over CDN en hoe u een aangepast domein zonder de tussenliggende registratiestap configureren naar [kaart Azure CDN-inhoud aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [reverse-DNS voor services die worden gehost in Azure configureren](dns-reverse-dns-for-azure-services.md).
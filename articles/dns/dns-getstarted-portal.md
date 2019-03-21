---
title: 'Snelstart: een Azure DNS-zone maken en vastleggen met behulp van de Azure portal'
description: Gebruik deze stapsgewijze quickstart om te leren hoe u een Azure DNS-zone en -record maakt met behulp van de Azure-portal.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: feb46114b3cf1b04e6a181f84bcdc41c17f1c0ba
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58119068"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-the-azure-portal"></a>Quickstart: Maak een Azure DNS-zone en -record met behulp van de Azure-portal

U kunt Azure DNS configureren voor het omzetten van hostnamen in uw openbare domein. Bijvoorbeeld, als u hebt aangeschaft de *contoso.xyz* domeinnaam van een domeinnaamregistrar, kunt u Azure DNS configureren op de host de *contoso.xyz* domein en los *www.contoso.xyz* naar het IP-adres van uw webserver of web-app.

In deze quickstart maakt u een testdomein en maakt u vervolgens een adresrecord om *www* om te zetten in het IP-adres *10.10.10.10*.

>[!IMPORTANT]
>Alle namen en IP-adressen in deze quickstart zijn voorbeelden en geen weerspiegelingen van praktijkscenario’s.

<!---
You can also perform these steps using [Azure PowerShell](dns-getstarted-powershell.md) or the cross-platform [Azure CLI](dns-getstarted-cli.md).
--->

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor alle portal-stappen meldt u zich aan bij de [Azure-portal](https://portal.azure.com).

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone bevat de DNS-vermeldingen voor een domein. Als u uw domein wilt hosten in Azure DNS, maakt u een DNS-zone voor deze domeinnaam. 

**De DNS-zone maken:**

1. Selecteer linksboven **Een resource maken**, en vervolgens **Netwerken** en **DNS-zone**.

1. Typ of selecteer op de pagina **DNS-zone maken** de volgende waarden:

   - **Naam**: Voer *contoso.xyz* in voor het voorbeeld in deze snelstartgids. De naam van de DNS-zone mag elke waarde zijn die nog niet is geconfigureerd op de Azure DNS-servers. Een werkelijke waarde zou een domein zijn dat u hebt gekocht via een domeinnaamregistrar.
   - **Resourcegroep**: Selecteer **nieuw**, voer *MyResourceGroup*, en selecteer **OK**. De naam van de resourcegroep moet uniek zijn binnen het Azure-abonnement. 

1. Selecteer **Maken**.

   ![DNS-zone](./media/dns-getstarted-portal/openzone650.png)

Het maken van de zone kan een paar minuten duren.

## <a name="create-a-dns-record"></a>Een DNS-record maken

DNS-vermeldingen of -records voor het domein maakt u binnen de DNS-zone. Maak een nieuwe adresrecord of A-record om een hostnaam om te zetten in een IPv4-adres.

**Een A-record maken:**

1. In de Azure-portal onder **alle resources**, open de **contoso.xyz** DNS-zone in de **MyResourceGroup** resourcegroep. U kunt *contoso.xyz* invoeren in het vak **Filteren op naam**  om deze eenvoudiger te vinden.

1. Selecteer boven aan de pagina **DNS-zone** de optie **+ Recordset**.

1. Typ of selecteer op de pagina **Recordset toevoegen** de volgende waarden:

   - **Naam**: Voer *www* in. De recordnaam is de hostnaam die u wilt omzetten in het opgegeven IP-adres.
   - **Type**: Selecteer **A**. A-records worden het meest gebruikt, maar er zijn andere recordtypen voor e-mailservers (MX), IPv6-adressen (AAAA), enzovoort. 
   - **TTL**: Voer *1* in. *Time-to-live* van de DNS-aanvraag geeft aan hoelang DNS-servers en -clients een antwoord kunnen opslaan in de cache.
   - **TTL-eenheid**: Selecteer **Uren**. Dit is de tijdseenheid voor de **TTL**-waarde. 
   - **IP-adres**: Voer voor het voorbeeld in deze snelstartgids *10.10.10.10* in. Deze waarde is het IP-adres waarin de recordnaam wordt omgezet. In een praktijkscenario zou u het openbare IP-adres van de webserver invoeren.

Omdat deze Quick Start alleen voor testdoeleinden snelle is, is er geen hoeft te configureren van de DNS-naamservers aan een domeinnaamregistrar. Met een echte productiedomein, moet u iedereen op Internet om op te lossen van de naam van de host verbinding maken met uw webserver of de app. U gaat naar uw domeinnaamregistrar om de naamserverrecords te vervangen door de Azure DNS-naamservers. Zie [Zelfstudie: Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md#delegate-the-domain) voor meer informatie.

## <a name="test-the-name-resolution"></a>De naamomzetting testen

Nu u een testzone hebt met daarin een DNS-record, kunt u de naamomzetting testen met het hulpprogramma *nslookup*. 

**DNS-naamomzetting testen:**

1. In de Azure-portal onder **alle resources**, open de **contoso.xyz** DNS-zone in de **MyResourceGroup** resourcegroep. U kunt *contoso.xyz* invoeren in het vak **Filteren op naam**  om deze eenvoudiger te vinden.

1. Kopieer een van de namen van de naamservers uit de lijst op de pagina **Overzicht**. 

   ![zone](./media/dns-getstarted-portal/viewzonens500.png)

1. Open een opdrachtprompt en voer de volgende opdracht uit:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Bijvoorbeeld:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Er verschijnt een scherm dat er ongeveer als volgt uitziet:

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

De hostnaam **www\.contoso.xyz** wordt omgezet naar **10.10.10.10**, net zoals u deze hebt geconfigureerd. Met dit resultaat wordt gecontroleerd of de naamomzetting juist werkt. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de resources die u in deze Quick Start hebt gemaakt niet meer nodig hebt, verwijdert u deze door het verwijderen van de **MyResourceGroup** resourcegroep. Open de **MyResourceGroup** resource-groep, en selecteer **resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [DNS-records voor een web-app in een aangepast domein maken](./dns-web-sites-custom-domain.md)
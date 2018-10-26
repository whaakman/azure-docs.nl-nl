---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d001d76bca5b9a0837349b6e05b3b0a271ea3a73
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133721"
---
> [!NOTE]
> U kunt Azure DNS gebruiken om een aangepaste DNS-naam voor uw Azure Web Apps te configureren. Zie [Use Azure DNS to provide custom domain settings for an Azure service](../articles/dns/dns-custom-domain.md#app-service-web-apps) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.
>
>

Meld u aan bij de website van uw domeinprovider.

Ga naar de pagina voor het beheren van DNS-records. Elke domeinprovider heeft zijn eigen interface voor het beheren van DNS-records. Raadpleeg daarom de documentatie van de provider. Doorgaans heeft het sitegedeelte waar u moet zijn, een naam als **Domain Name**, **DNS** of **Name Server Management**. 

Vaak kunt u de pagina met DNS-records vinden door uw accountgegevens te bekijken en te zoeken naar een link als **My domains** (of iets vergelijkbaars). Ga naar de betreffende pagina en kijk of u daar een link ziet zoals **Zone file**, **DNS Records** of **Advanced configuration**.

In de schermafbeelding hieronder wordt een voorbeeld van een pagina met DNS-records weergegeven:

![Voorbeeld van een pagina met DNS-records](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

In het getoonde voorbeeld selecteert u **Add** om een record te maken. Sommige providers hebben afzonderlijke links voor verschillende typen records. Raadpleeg ook hiervoor de documentatie van de provider.

> [!NOTE]
> Bij bepaalde providers, zoals GoDaddy, worden wijzigingen in DNS-records pas van kracht wanneer u op een afzonderlijke link **Save Changes** klikt. 

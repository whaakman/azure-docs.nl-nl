---
title: Een Azure DNS-subdomein delegeren
description: Leer hoe u een Azure DNS-subdomein delegeren.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/22/2019
ms.author: victorh
ms.openlocfilehash: 87a80703c473245660a850645ca3fef21bbd80f6
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452714"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Een Azure DNS-subdomein delegeren

De Azure-portal kunt u een DNS-subdomein delegeren. Als u eigenaar van het domein contoso.com, kunt u bijvoorbeeld een subdomein met de naam delegeren *engineering* aan een andere, afzonderlijke zone die afzonderlijk kan worden beheerd in de zone contoso.com.

## <a name="prerequisites"></a>Vereisten

Voor het overdragen van een Azure DNS-subdomein, moet u eerst uw openbare domein naar Azure DNS delegeren. Zie [een domein delegeren naar Azure DNS](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van de naamservers voor overdracht. Nadat uw domein wordt overgedragen naar de Azure DNS-zone, kunt u een subdomein delegeren.

De voorbeelden in dit artikel gebruikt het domein contoso.com. U moet uw eigen domein vervangen bij het gebruik van deze procedures.

## <a name="create-a-zone-for-your-subdomain"></a>Maak een zone voor het subdomein

Maak eerst de zone voor de **engineering** subdomein.

1. Selecteer in de Azure-portal **een resource maken**.
2. Typ in het zoekvak **DNS**, en selecteer **DNS-zone**.
3. Selecteer **Maken**.
4. In de **DNS-zone maken** deelvenster, type **engineering.contoso.com** in de **naam** in het tekstvak.
5. Selecteer de resourcegroep voor uw zone. Mogelijk wilt u dezelfde resourcegroep gebruiken als de bovenliggende zone vergelijkbare resources om samen te bewaren.
6. Klik op **Create**.
7. Nadat de implementatie is voltooid, gaat u naar de nieuwe zone.

## <a name="note-the-name-servers"></a>Houd er rekening mee de naamservers

Kopieer vervolgens de vier naamservers voor uw subdomein.

1. Op de **engineering** zone in het deelvenster Houd er rekening mee de vier naamservers voor de zone. U kunt deze naamservers wordt later gebruiken.
2. Maak een **A** record die moet worden gebruikt voor het testen. Maak bijvoorbeeld een **www** A vastleggen en deze configureren met een **10.10.10.10** IP-adres.

## <a name="create-an-ns-record"></a>Een NS-record maken

Maak vervolgens een naam (naamserver)-record voor de **engineering** zone.

1. Ga aan de zone voor het bovenliggende domein.
2. Selecteer **Recordset toevoegen**.
3. Op de **recordset toevoegen** deelvenster, type **engineering** in de **naam** in het tekstvak.
4. Voor **Type**, selecteer **NS**.
5. Onder **naamserver**, voer de vier naamservers die u eerder hebt genoteerd bij de **engineering** zone.
6. Klik op **OK**.

## <a name="test-the-delegation"></a>De overdracht testen

Nslookup gebruiken voor het testen van de overdracht.

1. Open een PowerShell-venster.
2. Typ het volgende achter de opdrachtprompt `nslookup www.engineering.<your domain name>.`
3. U moet een niet-bindend antwoord met het adres ontvangt **10.10.10.10**.



## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [reverse-DNS voor services die worden gehost in Azure configureren](dns-reverse-dns-for-azure-services.md).